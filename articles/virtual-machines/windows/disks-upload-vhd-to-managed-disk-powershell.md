---
title: Hochladen einer VHD in Azure mithilfe von Azure PowerShell
description: Erfahren Sie, wie Sie mit Azure PowerShell eine VHD auf einen verwalteten Azure-Datenträger hochladen und den verwalteten Datenträger regionsübergreifend kopieren können.
author: roygara
ms.author: rogarana
ms.date: 05/06/2019
ms.topic: article
ms.service: virtual-machines-linux
ms.tgt_pltfrm: linux
ms.subservice: disks
ms.openlocfilehash: d193dcd0c0539c2daa7220d915fdc3e02c8ea798
ms.sourcegitcommit: 12de9c927bc63868168056c39ccaa16d44cdc646
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 10/17/2019
ms.locfileid: "72512439"
---
# <a name="upload-a-vhd-to-azure-using-azure-powershell"></a>Hochladen einer VHD in Azure mithilfe von Azure PowerShell

In diesem Artikel wird beschrieben, wie Sie eine VHD von Ihrem lokalen Computer auf einen verwalteten Azure-Datenträger hochladen. Bisher mussten Sie Ihre Daten eher umständlich per Staging in einem Speicherkonto zwischenspeichern und dieses verwalten. Diese Schritte sind nun nicht mehr notwendig, um eine VHD hochzuladen. Stattdessen erstellen Sie einen leeren verwalteten Datenträger und laden die VHD direkt auf diesen hoch. Dies vereinfacht das Hochladen lokaler VMs in Azure und ermöglicht Ihnen, eine VHD bis zu 32 TiB direkt auf einen großen verwalteten Datenträger hochzuladen.

Wenn Sie eine Sicherungslösung für IaaS-VMs in Azure bereitstellen, sollten Sie einen direkten Upload verwenden, um Kundensicherungen auf verwalteten Datenträgern wiederherzustellen. Wenn Sie eine VHD von einem Computer hochladen, der sich außerhalb von Azure befindet, hängt die Geschwindigkeit von der lokalen Bandbreite ab. Wenn Sie einen virtuellen Azure-Computer verwenden, ist die Bandbreite mit der von Standardfestplattenlaufwerken (Standard-HDDs) identisch.

Der direkte Upload wird derzeit für verwaltete Datenträger des Typs HDD Standard, SSD Standard und SSD Premium unterstützt. Für SSD Ultra wird dies noch nicht unterstützt.

## <a name="prerequisites"></a>Voraussetzungen

- Laden Sie die neueste [Version von AzCopy V10](../../storage/common/storage-use-azcopy-v10.md#download-and-install-azcopy) herunter.
- [Installieren Sie das Azure PowerShell-Modul](/powershell/azure/install-Az-ps).
- Wenn Sie beabsichtigen, eine VHD aus der lokalen Umgebung hochzuladen: Eine VHD, die [für Azure vorbereitet wurde](prepare-for-upload-vhd-image.md) und lokal gespeichert ist.
- Oder ein verwalteter Datenträger in Azure, wenn Sie eine Kopieraktion ausführen möchten.

## <a name="create-an-empty-managed-disk"></a>Erstellen eines leeren verwalteten Datenträgers

Wenn Sie Ihre VHD in Azure hochladen möchten, müssen Sie einen leeren verwalteten Datenträger erstellen, der für diesen Hochladevorgang konfiguriert ist. Bevor Sie einen solchen Datenträger erstellen, sollten Sie einige zusätzliche Informationen über diese Datenträger wissen.

Diese Art des verwalteten Datenträgers verfügt über zwei eindeutige Zustände:

- „ReadyToUpload“: Der Datenträger ist für den Upload bereit, jedoch wurde keine [SAS (Shared Access Signature)](https://docs.microsoft.com/azure/storage/common/storage-dotnet-shared-access-signature-part-1) generiert.
- „ActiveUpload“: Der Datenträger ist für den Upload bereit, und die SAS wurde generiert.

Für den verwalteten Datenträger fallen in beiden Zuständen Kosten an, die sich aus den [Preisen für HDD Standard-Datenträger](https://azure.microsoft.com/pricing/details/managed-disks/) ergeben. Welcher Art von Datenträger verwendet wird, spielt keine Rolle. Das bedeutet, dass beispielsweise der Abrechnungsvorgang für P10 und S10 identisch ist. Dies ist solange der Fall, bis `revoke-access` für den verwalteten Datenträger aufgerufen wird. Dieser Vorgang ist erforderlich, um den Datenträger an eine VM anzufügen.

Bevor Sie eine leere HDD Standard zum Hochladen erstellen, benötigen Sie die Dateigröße der VHD in Byte, die Sie hochladen möchten. Der Beispielcode ruft diese Größe für Sie ab. Wenn Sie die Größe manuell abrufen möchten, können Sie Folgendes verwenden: `$vhdSizeBytes = (Get-Item "<fullFilePathHere>").length`. Dieser Wert wird verwendet, wenn der Parameter **-UploadSizeInBytes** angegeben wird.

Erstellen Sie nun für das Hochladen (Upload) einen leeren HDD Standard-Datenträger. Führen Sie dazu in Ihrer lokalen Shell das [New-AzDiskConfig](https://docs.microsoft.com/powershell/module/az.compute/new-azdiskconfig?view=azps-1.8.0)-Cmdlet aus, für das Sie die **Upload**-Einstellung im **-CreateOption**-Parameter und den **-UploadSizeInBytes**-Parameter angeben. Rufen Sie dann [New-AzDisk](https://docs.microsoft.com/powershell/module/az.compute/new-azdisk?view=azps-1.8.0) auf, um den Datenträger zu erstellen:

```powershell
$vhdSizeBytes = (Get-Item "<fullFilePathHere>").length

$diskconfig = New-AzDiskConfig -SkuName 'Standard_LRS' -OsType 'Windows' -UploadSizeInBytes $vhdSizeBytes -Location 'West US' -CreateOption 'Upload'

New-AzDisk -ResourceGroupName 'myResourceGroup' -DiskName 'myDiskName' -Disk $diskconfig
```

Wenn Sie für den Upload einen SSD Premium- oder SSD Standard-Datenträger verwenden möchten, ersetzen Sie **Standard_LRS** durch **Premium_LRS** oder **StandardSSD_LRS**. SSD Ultra wird noch nicht unterstützt.

Sie haben nun einen leeren verwalteten Datenträger erstellt, der für den Hochladevorgang konfiguriert ist. Um eine VHD auf den Datenträger hochzuladen, benötigen Sie eine beschreibbare SAS, mit der Sie auf den Datenträger als Hochladeziel verweisen können.

Verwenden Sie den folgenden Befehl, um eine beschreibbare SAS für Ihren leeren verwalteten Datenträger zu generieren:

```powershell
$diskSas = Grant-AzDiskAccess -ResourceGroupName 'myResouceGroup' -DiskName 'myDiskName' -DurationInSecond 86400 -Access 'Write'

$disk = Get-AzDisk -ResourceGroupName 'myResourceGroup' -DiskName 'myDiskName'
```

## <a name="upload-vhd"></a>Hochladen der VHD

Sie verfügen nun über eine SAS für Ihren leeren verwalteten Datenträger und können diese verwenden, um diesen Datenträger als Ziel für den Uploadbefehl anzugeben.

Verwenden Sie AzCopy v10, um die lokale VHD-Datei auf einen verwalteten Datenträger hochzuladen. Dabei müssen Sie den generierten SAS-URI angeben.

Der Durchsatz für diesen Upload entspricht dem des gleichwertigen [HDD Standard-Datenträgers](disks-types.md#standard-hdd). Wenn Sie beispielsweise als Größe S4 verwenden, beträgt der Durchsatz bis zu 60 MiB/s. Verwenden Sie hingegen als Größe S70, beträgt der Durchsatz bis zu 500 MiB/s.

```
AzCopy.exe copy "c:\somewhere\mydisk.vhd" $diskSas.AccessSAS --blob-type PageBlob
```

Wenn Ihre SAS während des Uploads abläuft und Sie `revoke-access` noch nicht aufgerufen haben, können Sie eine neue SAS beziehen, um den Upload mit `grant-access` fortzusetzen.

Nachdem der Upload abgeschlossen wurde und Sie keine Daten mehr auf den Datenträger schreiben müssen, widerrufen Sie die SAS. Dadurch ändert sich der Zustand des verwalteten Datenträgers, und Sie können den Datenträger an eine VM anfügen.

```powershell
Revoke-AzDiskAccess -ResourceGroupName 'myResourceGroup' -DiskName 'myDiskName'
```

## <a name="copy-a-managed-disk"></a>Kopieren eines verwalteten Datenträgers

Der direkte Upload vereinfacht auch das Kopieren eines verwalteten Datenträgers. Sie können entweder innerhalb der gleichen Region oder regionsübergreifend (in eine andere Region) kopieren.

Das folgende Skript führt dies für Sie aus. Der Vorgang ähnelt mit einigen Unterschieden den zuvor beschriebenen Schritten, da Sie nun mit einem vorhandenen Datenträger arbeiten.

> [!IMPORTANT]
> Sie müssen ein Offset von 512 hinzufügen, wenn Sie die Datenträgergröße eines verwalteten Datenträgers aus Azure in Byte bereitstellen. Dies liegt daran, dass Azure die Fußzeile beim Zurückgeben der Datenträgergröße auslässt. Der Kopiervorgang schlägt fehl, wenn Sie nicht so vorgehen. Das folgende Skript fügt dieses Offset bereits für Sie ein.

Ersetzen Sie `<sourceResourceGroupHere>`, `<sourceDiskNameHere>`, `<targetDiskNameHere>`, `<targetResourceGroupHere>`, `<yourOSTypeHere>` und `<yourTargetLocationHere>` (ein Beispiel für einen Standortwert ist „uswest2“) durch Ihre Werte, und führen Sie dann das folgende Skript aus, um einen verwalteten Datenträger zu kopieren.

```powershell

$sourceRG = <sourceResourceGroupHere>
$sourceDiskName = <sourceDiskNameHere>
$targetDiskName = <targetDiskNameHere>
$targetRG = <targetResourceGroupHere>
$targetLocate = <yourTargetLocationHere>
#Expected value for OS is either "Windows" or "Linux"
$targetOS = <yourOSTypeHere>

$sourceDisk = Get-AzDisk -ResourceGroupName $sourceRG -DiskName $sourceDiskName

# Adding the sizeInBytes with the 512 offset, and the -Upload flag
$targetDiskconfig = New-AzDiskConfig -SkuName 'Standard_LRS' -osType $targetOS -UploadSizeInBytes $($sourceDisk.DiskSizeBytes+512) -Location $targetLocate -CreateOption 'Upload'

$targetDisk = New-AzDisk -ResourceGroupName $targetRG -DiskName $targetDiskName -Disk $targetDiskconfig

$sourceDiskSas = Grant-AzDiskAccess -ResourceGroupName $sourceRG -DiskName $sourceDiskName -DurationInSecond 86400 -Access 'Read'

$targetDiskSas = Grant-AzDiskAccess -ResourceGroupName $targetRG -DiskName $targetDiskName -DurationInSecond 86400 -Access 'Write'

azcopy copy $sourceDiskSas.AccessSAS $targetDiskSas.AccessSAS --blob-type PageBlob

Revoke-AzDiskAccess -ResourceGroupName $sourceRG -DiskName $sourceDiskName

Revoke-AzDiskAccess -ResourceGroupName $targetRG -DiskName $targetDiskName 
```

## <a name="next-steps"></a>Nächste Schritte

Sie haben erfolgreich eine VHD auf den verwalteten Datenträger hochgeladen und können Datenträger nun an eine VM anfügen, um ihn zu verwenden.

Informationen dazu, wie Sie einen Datenträger für Daten an eine VM anfügen, finden Sie in folgendem Artikel: [Attach a data disk to a Windows VM with PowerShell (Hinzufügen eines Datenträgers zu einer Windows-VM mit PowerShell)](attach-disk-ps.md). Informationen zum Verwenden des Datenträgers als Betriebssystemdatenträger finden Sie unter [Erstellen einer Windows-VM von einem speziellen Datenträger](create-vm-specialized.md#create-the-new-vm).
