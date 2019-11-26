---
title: Hochladen einer VHD in Azure mithilfe der Azure CLI
description: Erfahren Sie, wie Sie mit der Azure CLI eine VHD auf einen verwalteten Azure-Datenträger hochladen und den verwalteten Datenträger regionsübergreifend kopieren können.
services: virtual-machines-linux,storage
author: roygara
ms.author: rogarana
ms.date: 09/20/2019
ms.topic: article
ms.service: virtual-machines-linux
ms.tgt_pltfrm: linux
ms.subservice: disks
ms.openlocfilehash: 5215a7d899af15dc028189aee5760a6ec5b6577d
ms.sourcegitcommit: be8e2e0a3eb2ad49ed5b996461d4bff7cba8a837
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 10/23/2019
ms.locfileid: "72803992"
---
# <a name="upload-a-vhd-to-azure-using-azure-cli"></a>Hochladen einer VHD in Azure mithilfe der Azure CLI

In diesem Artikel wird beschrieben, wie Sie eine VHD von Ihrem lokalen Computer auf einen verwalteten Azure-Datenträger hochladen. Bisher mussten Sie Ihre Daten eher umständlich per Staging in einem Speicherkonto zwischenspeichern und dieses verwalten. Diese Schritte sind nun nicht mehr notwendig, um eine VHD hochzuladen. Stattdessen erstellen Sie einen leeren verwalteten Datenträger und laden die VHD direkt auf diesen hoch. Dies vereinfacht das Hochladen lokaler VMs in Azure und ermöglicht Ihnen, eine VHD bis zu 32 TiB direkt auf einen großen verwalteten Datenträger hochzuladen.

Wenn Sie eine Sicherungslösung für IaaS-VMs in Azure bereitstellen, sollten Sie einen direkten Upload verwenden, um Kundensicherungen auf verwalteten Datenträgern wiederherzustellen. Wenn Sie eine VHD von einem Computer hochladen, der sich außerhalb von Azure befindet, hängt die Geschwindigkeit von der lokalen Bandbreite ab. Wenn Sie einen virtuellen Azure-Computer verwenden, ist die Bandbreite mit der von Standardfestplattenlaufwerken (Standard-HDDs) identisch.

Der direkte Upload wird derzeit für verwaltete Datenträger des Typs HDD Standard, SSD Standard und SSD Premium unterstützt. Für SSD Ultra wird dies noch nicht unterstützt.

## <a name="prerequisites"></a>Voraussetzungen

- Laden Sie die neueste [Version von AzCopy V10](../../storage/common/storage-use-azcopy-v10.md#download-and-install-azcopy) herunter.
- [Installieren Sie die Azure-Befehlszeilenschnittstelle](/cli/azure/install-azure-cli).
- Eine lokal gespeicherte VHD-Datei
- Wenn Sie beabsichtigen, eine VHD aus der lokalen Umgebung hochzuladen: Eine VHD, die [für Azure vorbereitet wurde](../windows/prepare-for-upload-vhd-image.md) und lokal gespeichert ist.
- Oder ein verwalteter Datenträger in Azure, wenn Sie eine Kopieraktion ausführen möchten.

## <a name="create-an-empty-managed-disk"></a>Erstellen eines leeren verwalteten Datenträgers

Wenn Sie Ihre VHD in Azure hochladen möchten, müssen Sie einen leeren verwalteten Datenträger erstellen, der für diesen Hochladevorgang konfiguriert ist. Bevor Sie einen solchen Datenträger erstellen, sollten Sie einige zusätzliche Informationen über diese Datenträger wissen.

Diese Art des verwalteten Datenträgers verfügt über zwei eindeutige Zustände:

- „ReadyToUpload“: Der Datenträger ist für den Upload bereit, jedoch wurde keine [SAS (Shared Access Signature)](https://docs.microsoft.com/azure/storage/common/storage-dotnet-shared-access-signature-part-1) generiert.
- „ActiveUpload“: Der Datenträger ist für den Upload bereit, und die SAS wurde generiert.

Für den verwalteten Datenträger fallen in beiden Zuständen Kosten an, die sich aus den [Preisen für HDD Standard-Datenträger](https://azure.microsoft.com/pricing/details/managed-disks/) ergeben. Welcher Art von Datenträger verwendet wird, spielt keine Rolle. Das bedeutet, dass beispielsweise der Abrechnungsvorgang für P10 und S10 identisch ist. Dies ist solange der Fall, bis `revoke-access` für den verwalteten Datenträger aufgerufen wird. Dieser Vorgang ist erforderlich, um den Datenträger an eine VM anzufügen.

Bevor Sie eine leere HDD Standard zum Hochladen erstellen können, benötigen Sie die Dateigröße der VHD in Byte, die Sie hochladen möchten. Um diese abzurufen, können Sie `wc -c <yourFileName>.vhd` oder `ls -al <yourFileName>.vhd` verwenden. Dieser Wert wird verwendet, wenn der Parameter **--upload-size-bytes** angegeben wird.

Erstellen Sie für das Hochladen eine leere HDD Standard, indem Sie in einem [Datenträgererstellungs](/cli/azure/disk#az-disk-create)-Cmdlet sowohl den **--for-upload**- als auch den **--upload-size-bytes**-Parameter angeben:

```bash
az disk create -n mydiskname -g resourcegroupname -l westus2 --for-upload --upload-size-bytes 34359738880 --sku standard_lrs
```

Wenn Sie für den Upload einen SSD Premium- oder SSD Standard-Datenträger verwenden möchten, ersetzen Sie **standard_lrs** durch **premium_LRS** oder **standardSSD_lrs**. SSD Ultra wird noch nicht unterstützt.

Sie haben nun einen leeren verwalteten Datenträger erstellt, der für den Hochladevorgang konfiguriert ist. Um eine VHD auf den Datenträger hochzuladen, benötigen Sie eine beschreibbare SAS, mit der Sie auf den Datenträger als Hochladeziel verweisen können.

Verwenden Sie den folgenden Befehl, um eine beschreibbare SAS für Ihren leeren verwalteten Datenträger zu generieren:

```bash
az disk grant-access -n mydiskname -g resourcegroupname --access-level Write --duration-in-seconds 86400
```

Beispiel eines Rückgabewerts:

```
{
  "accessSas": "https://md-impexp-t0rdsfgsdfg4.blob.core.windows.net/w2c3mj0ksfgl/abcd?sv=2017-04-17&sr=b&si=600a9281-d39e-4cc3-91d2-923c4a696537&sig=xXaT6mFgf139ycT87CADyFxb%2BnPXBElYirYRlbnJZbs%3D"
}
```

## <a name="upload-vhd"></a>Hochladen der VHD

Sie verfügen nun über eine SAS für Ihren leeren verwalteten Datenträger und können diese verwenden, um diesen Datenträger als Ziel für den Uploadbefehl anzugeben.

Verwenden Sie AzCopy v10, um die lokale VHD-Datei auf einen verwalteten Datenträger hochzuladen. Dabei müssen Sie den generierten SAS-URI angeben.

Der Durchsatz für diesen Upload entspricht dem des gleichwertigen [HDD Standard-Datenträgers](disks-types.md#standard-hdd). Wenn Sie beispielsweise als Größe S4 verwenden, beträgt der Durchsatz bis zu 60 MiB/s. Verwenden Sie hingegen als Größe S70, beträgt der Durchsatz bis zu 500 MiB/s.

```bash
AzCopy.exe copy "c:\somewhere\mydisk.vhd" "sas-URI" --blob-type PageBlob
```

Wenn Ihre SAS während des Uploads abläuft und Sie `revoke-access` noch nicht aufgerufen haben, können Sie eine neue SAS beziehen, um den Upload mit `grant-access` fortzusetzen.

Nachdem der Upload abgeschlossen wurde und Sie keine Daten mehr auf den Datenträger schreiben müssen, widerrufen Sie die SAS. Dadurch ändert sich der Zustand des verwalteten Datenträgers, und Sie können den Datenträger an eine VM anfügen.

```bash
az disk revoke-access -n mydiskname -g resourcegroupname
```

## <a name="copy-a-managed-disk"></a>Kopieren eines verwalteten Datenträgers

Der direkte Upload vereinfacht auch das Kopieren eines verwalteten Datenträgers. Sie können entweder innerhalb der gleichen Region oder regionsübergreifend (in eine andere Region) kopieren.

Das folgende Skript führt dies für Sie aus. Der Vorgang ähnelt mit einigen Unterschieden den zuvor beschriebenen Schritten, da Sie nun mit einem vorhandenen Datenträger arbeiten.

> [!IMPORTANT]
> Sie müssen ein Offset von 512 hinzufügen, wenn Sie die Datenträgergröße eines verwalteten Datenträgers aus Azure in Byte bereitstellen. Dies liegt daran, dass Azure die Fußzeile beim Zurückgeben der Datenträgergröße auslässt. Der Kopiervorgang schlägt fehl, wenn Sie nicht so vorgehen. Das folgende Skript fügt dieses Offset bereits für Sie ein.

Ersetzen Sie `<sourceResourceGroupHere>`, `<sourceDiskNameHere>`, `<targetDiskNameHere>`, `<targetResourceGroupHere>` und `<yourTargetLocationHere>` (ein Beispiel für einen Standortwert ist „uswest2“) durch Ihre Werte, und führen Sie dann das folgende Skript aus, um einen verwalteten Datenträger zu kopieren.

```bash
sourceDiskName = <sourceDiskNameHere>
sourceRG = <sourceResourceGroupHere>
targetDiskName = <targetDiskNameHere>
targetRG = <targetResourceGroupHere>
targetLocale = <yourTargetLocationHere>

sourceDiskSizeBytes= $(az disk show -g $sourceRG -n $sourceDiskName --query '[uniqueId]' -o tsv)

az disk create -g $targetRG -n $targetDiskName -l $targetLocale --for-upload --upload-size-bytes $(($sourceDiskSizeBytes+512)) --sku standard_lrs

targetSASURI = $(az disk grant-access -n $targetDiskName -g $targetRG  --access-level Write --duration-in-seconds 86400 -o tsv)

sourceSASURI=$(az disk grant-access -n $sourceDiskName -g $sourceRG --duration-in-seconds 86400 --query [accessSas] -o tsv)

.\azcopy copy $sourceSASURI $targetSASURI --blob-type PageBlob

az disk revoke-access -n $sourceDiskName -g $sourceRG

az disk revoke-access -n $targetDiskName -g $targetRG
```

## <a name="next-steps"></a>Nächste Schritte

Nachdem Sie nun erfolgreich eine VHD auf einen verwalteten Datenträger hochgeladen haben, können Sie den Datenträger als [Datenträger für Daten an eine vorhandene VM](add-disk.md) anfügen oder [den Datenträger als Betriebssystemdatenträger](upload-vhd.md#create-the-vm) an eine VM anfügen, um eine neue VM zu erstellen. 

