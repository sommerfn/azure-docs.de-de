---
title: Azure-Support für VMs der Generation 2 (Vorschau) | Microsoft-Dokumentation
description: 'Übersicht: Azure-Support für VMs der Generation 2'
services: virtual-machines-windows
documentationcenter: ''
author: laurenhughes
manager: gwallace
editor: ''
tags: azure-resource-manager
ms.service: virtual-machines-windows
ms.workload: infrastructure-services
ms.tgt_pltfrm: vm-windows
ms.topic: article
ms.date: 10/16/2019
ms.author: lahugh
ms.openlocfilehash: 1b906a9e3ce803b7f319a920f29b5a60f62c47b1
ms.sourcegitcommit: ae461c90cada1231f496bf442ee0c4dcdb6396bc
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 10/17/2019
ms.locfileid: "72550157"
---
# <a name="support-for-generation-2-vms-preview-on-azure"></a>Unterstützung für VMs der Generation 2 (Vorschau) in Azure

> [!IMPORTANT]
> Der Azure-Support für VMs der Generation 2 (Vorschau) befindet sich derzeit in der Vorschauphase.
> Diese Vorschauversion wird ohne Vereinbarung zum Servicelevel bereitgestellt und ist nicht für Produktionsworkloads vorgesehen. Manche Features werden möglicherweise nicht unterstützt oder sind nur eingeschränkt verwendbar.
> Weitere Informationen finden Sie unter [Zusätzliche Nutzungsbestimmungen für Microsoft Azure-Vorschauversionen](https://azure.microsoft.com/support/legal/preview-supplemental-terms/).

Die Unterstützung für virtuelle Computer (VMs) der Generation 2 ist jetzt als Vorschauversion in Azure verfügbar. Sie können die Generation eines virtuellen Computers nach der Erstellung nicht mehr ändern, deshalb sollten Sie die Aspekte auf dieser Seite genauestens prüfen, bevor Sie eine Generation auswählen. 

VMs der Generation 2 unterstützen wichtige Features, die bei VMs der Generation 1 nicht unterstützt werden. Zu diesen Features gehören mehr Speicher, Intel Software Guard Extensions (Intel SGX) und virtualisierter persistenter Speicher (vPMEM). VMs der Generation 2 weisen auch einige Features auf, die in Azure noch nicht unterstützt werden. Weitere Informationen finden Sie im Abschnitt [Features und Funktionen](#features-and-capabilities).

VMs der Generation 2 verwenden die neue UEFI-basierte Startarchitektur und nicht mehr die BIOS-basierte Architektur von VMs der Generation 1. Im Vergleich zu VMs der Generation 1 können VMs der Generation 2 eine verbesserte Start- und Installationszeit aufweisen. Einen Überblick über VMs der Generation 2 und einige der Unterschiede zwischen Generation 1 und Generation 2 finden Sie unter [Sollte ich einen virtuellen Computer der Generation 1 oder 2 in Hyper-V erstellen?](https://docs.microsoft.com/windows-server/virtualization/hyper-v/plan/should-i-create-a-generation-1-or-2-virtual-machine-in-hyper-v).

## <a name="generation-2-vm-sizes"></a>VM-Größen der Generation 2

VMs der Generation 1 werden von allen VM-Größen in Azure unterstützt. Azure bietet jetzt Unterstützung für die Vorschauversion von Generation 2 für die folgenden ausgewählten VM-Serien:

* [B-Serie](https://docs.microsoft.com/azure/virtual-machines/windows/b-series-burstable)
* [DC-Serie](https://docs.microsoft.com/azure/virtual-machines/windows/sizes-general#dc-series)
* [Dsv2-Serie](https://docs.microsoft.com/azure/virtual-machines/windows/sizes-general#dsv2-series) und [Dsv3-Serie](https://docs.microsoft.com/azure/virtual-machines/windows/sizes-general#dsv3-series-1)
* [Esv3-Serie](https://docs.microsoft.com/azure/virtual-machines/windows/sizes-memory#esv3-series)
* [Fsv2-Serie](https://docs.microsoft.com/azure/virtual-machines/windows/sizes-compute#fsv2-series-1)
* [GS-Serie](https://docs.microsoft.com/azure/virtual-machines/windows/sizes-previous-gen#gs-series)
* [HB-Serie](https://docs.microsoft.com/azure/virtual-machines/windows/sizes-hpc#hb-series)
* [HC-Serie](https://docs.microsoft.com/azure/virtual-machines/windows/sizes-hpc#hc-series)
* [Ls-Serie](https://docs.microsoft.com/azure/virtual-machines/windows/sizes-previous-gen#ls-series) und [Lsv2-Serie](https://docs.microsoft.com/azure/virtual-machines/windows/sizes-storage#lsv2-series)
* [Mv2-Serie](https://docs.microsoft.com/azure/virtual-machines/windows/sizes-memory#mv2-series)
* [NCv2-Serie](https://docs.microsoft.com/azure/virtual-machines/windows/sizes-gpu#ncv2-series) und [NCv3-Serie](https://docs.microsoft.com/azure/virtual-machines/windows/sizes-gpu#ncv3-series)
* [ND-Serie](https://docs.microsoft.com/azure/virtual-machines/windows/sizes-gpu#nd-series)
* [NVv3-Serie](https://docs.microsoft.com/azure/virtual-machines/windows/sizes-gpu#nvv3-series--1)

> [!NOTE]
> Die Verwendung von VM-Images der Generation 2 für VMs der Mv2-Serie ist allgemein verfügbar, da die Mv2-Serie ausschließlich mit VM-Images der Generation 2 arbeitet. VM-Images der Generation 1 werden auf VMs der Mv2-Serie nicht unterstützt. 

## <a name="generation-2-vm-images-in-azure-marketplace"></a>VM-Images der Generation 2 in Azure Marketplace

VMs der Generation 2 unterstützen die folgenden Marketplace-Images:

* Windows Server 2019 Datacenter
* Windows Server 2016 Datacenter
* Windows Server 2012 R2 Datacenter
* Windows Server 2012 Datacenter
* SUSE Linux Enterprise Server 15 SP1
* SUSE Linux Enterprise Server 12 SP4

## <a name="on-premises-vs-azure-generation-2-vms"></a>Lokale VMs und Azure-VMs der Generation 2 im Vergleich

Azure unterstützt zurzeit einige der Funktionen nicht, die lokale Hyper-V-Instanzen für VMs der Generation 2 unterstützen.

| Funktionen der Generation 2                | Lokale Hyper-V-Instanz | Azure |
|-------------------------------------|---------------------|-------|
| Sicherer Start                         | :heavy_check_mark:  | :x:   |
| Abgeschirmte VM                         | :heavy_check_mark:  | :x:   |
| vTPM                                | :heavy_check_mark:  | :x:   |
| Virtualisierungsbasierte Sicherheit (VBS) | :heavy_check_mark:  | :x:   |
| VHDX-Format                         | :heavy_check_mark:  | :x:   |

## <a name="features-and-capabilities"></a>Features und Funktionen

### <a name="generation-1-vs-generation-2-features"></a>Features der Generationen 1 und 2 im Vergleich

| Feature | Generation 1 | Generation 2 |
|---------|--------------|--------------|
| Start             | PCAT                      | UEFI                               |
| Datenträgercontroller | IDE                       | SCSI                               |
| VM-Größen         | Alle VM-Größen | Nur VMs, die Storage Premium unterstützen |

### <a name="generation-1-vs-generation-2-capabilities"></a>Funktionen der Generationen 1 und 2 im Vergleich

| Funktion | Generation 1 | Generation 2 |
|------------|--------------|--------------|
| Betriebssystem-Datenträger > 2 TB                    | :x:                | :heavy_check_mark: |
| Benutzerdefinierter Datenträger/Image/Swapbetriebssystem         | :heavy_check_mark: | :heavy_check_mark: |
| Unterstützung für VM-Skalierungsgruppe | :heavy_check_mark: | :heavy_check_mark: |
| Azure Site Recovery               | :heavy_check_mark: | :x:                |
| Sichern/Wiederherstellen                    | :heavy_check_mark: | :heavy_check_mark: |
| Shared Image Gallery              | :heavy_check_mark: | :heavy_check_mark: |
| Azure Disk Encryption             | :heavy_check_mark: | :x:                |

## <a name="creating-a-generation-2-vm"></a>Erstellen einer VM der Generation 2

### <a name="marketplace-image"></a>Marketplace-Image

VMs der Generation 2 können aus einem Marketplace-Image, das den UEFI-Start unterstützt, über das Azure-Portal oder die Azure-Befehlszeilenschnittstelle erstellt werden.

#### <a name="azure-portal"></a>Azure-Portal

Generation 2-Images für Windows und SLES sind im selben Serverangebot wie die Gen1-Images enthalten. Aus Sicht des Ablaufs bedeutet dies, dass Sie das Angebot und die SKU im Portal für Ihren virtuellen Computer (VM) auswählen. Unterstützt die SKU sowohl Generation 1- als Generation 2-Images, können Sie im Ablauf der VM-Erstellung auf der Registerkarte *Erweitert* auswählen, dass eine VM der Generation 2 erstellt werden soll.

Derzeit unterstützen die folgenden SKUs sowohl Generation 1- als auch Generation 2-Images:

* Windows Server 2012
* Windows Server 2012 R2
* Windows Server 2016
* Windows Server 2019

Wenn Sie eine Windows Server-SKU als Angebot auswählen, gibt es auf der Registerkarte **Erweitert** die Option, entweder eine **Gen 1**- (BIOS) oder eine **Gen 2**-VM (UEFI) zu erstellen. Wenn Sie **Gen 2** auswählen, müssen Sie sicherstellen, dass die auf der Registerkarte **Grundlagen** ausgewählte VM-Größe [für VMs der Generation 2 unterstützt wird](#generation-2-vm-sizes).

![Virtuellen Computer der Gen 1 oder Gen 2 auswählen](./media/generation-2/gen1-gen2-select.png)

#### <a name="powershell"></a>PowerShell

Sie können auch PowerShell verwenden, um einen virtuellen Computer zu erstellen, indem Sie direkt auf die SKU der Generation 1 oder der Generation 2 verweisen.

Verwenden Sie beispielsweise das folgende PowerShell-Cmdlet, um eine Liste der SKUs im `WindowsServer`-Angebot abzurufen.

```powershell
Get-AzVMImageSku -Location westus2 -PublisherName MicrosoftWindowsServer -Offer WindowsServer
```

Wenn Sie einen virtuellen Computer mit Windows Server 2012 als Betriebssystem erstellen, wählen Sie entweder die VM-SKU der Generation 1 (BIOS) oder der Generation 2 (UEFI) aus, die wie folgt aussieht:

```powershell
2012-Datacenter
2012-datacenter-gensecond
```

Im Abschnitt [Features und Funktionen](#features-and-capabilities) finden Sie eine aktuelle Liste der unterstützten Marketplace-Images.

### <a name="managed-image-or-managed-disk"></a>Verwaltetes Image oder verwalteter Datenträger

Sie können VMs der Generation 2 aus einem verwalteten Image oder einem verwalteten Datenträger auf die gleiche Weise erstellen, wie Sie eine VM der Generation 1 erstellen würden.

### <a name="virtual-machine-scale-sets"></a>VM-Skalierungsgruppen

VMs der Generation 2 können auch mithilfe von VM-Skalierungsgruppen erstellt werden. Verwenden Sie an der Azure-Befehlszeilenschnittstelle Azure-Skalierungsgruppen, um VMs der Generation 2 zu erstellen.

## <a name="frequently-asked-questions"></a>Häufig gestellte Fragen

* **Sind VMs der Generation 2 in allen Azure-Regionen verfügbar?**  
    Ja. Allerdings stehen nicht alle [VM-Größen der Generation 2](#generation-2-vm-sizes) in allen Regionen zur Verfügung. Die Verfügbarkeit von VMs der Generation 2 richtet sich nach der Verfügbarkeit der VM-Größe.

* **Gibt es einen Preisunterschied zwischen VMs der 1. und 2. Generation?**  
   Nein.

* **Ich verfüge über eine VHD-Datei aus meiner lokalen VM der Generation 2. Kann ich die VHD-Datei verwenden, um einen virtuellen Computer der Generation 2 in Azure zu erstellen?**
  Ja, Sie können die VHD-Datei der Generation 2 in Azure einbringen und zum Erstellen eines virtuellen Computers der Generation 2 verwenden. Führen Sie dazu die folgenden Schritte aus:
    1. Laden Sie die VHD-Datei in ein Speicherkonto in derselben Region hoch, in der Sie Ihre VM erstellen möchten.
    1. Erstellen Sie einen verwalteten Datenträger aus der VHD-Datei. Legen Sie die Eigenschaft „Hyper-V-Generation“ auf „V2“ fest. Die folgenden PowerShell-Befehle legen die Eigenschaft „Hyper-V-Generation“ beim Erstellen eines verwalteten Datenträgers fest.

        ```powershell
        $sourceUri = 'https://xyzstorage.blob.core.windows.net/vhd/abcd.vhd'. #<Provide location to your uploaded .vhd file>
        $osDiskName = 'gen2Diskfrmgenvhd'  #<Provide a name for your disk>
        $diskconfig = New-AzDiskConfig -Location '<location>' -DiskSizeGB 127 -AccountType Standard_LRS -OsType Windows -HyperVGeneration "V2" -SourceUri $sourceUri -CreateOption 'Import'
        New-AzDisk -DiskName $osDiskName -ResourceGroupName '<Your Resource Group>' -Disk $diskconfig
        ```

    1. Nachdem der Datenträger verfügbar ist, erstellen Sie einen virtuellen Computer durch Anfügen dieses Datenträgers. Der erstellte virtuelle Computer ist eine VM der Generation 2.
    Wenn der virtuelle Computer der Generation 2 erstellt ist, können Sie optional das Image dieser VM verallgemeinern. Wenn Sie das Image verallgemeinern, können Sie es verwenden, um mehrere VMs zu erstellen.

* **Wie erhöhe ich die Größe des Betriebssystemdatenträgers?**  
  Betriebssystemdatenträger mit einer Größe von mehr als 2 TB sind neu bei VMs der 2. Generation. Standardmäßig sind die Betriebssystemdatenträger für VMs der Generation 2 kleiner als 2 TB. Sie können die Größe des Datenträgers auf die empfohlene maximale Größe von 4 TB erhöhen. Verwenden Sie zum Erhöhen der Größe von Betriebssystemdatenträgern die Azure-Befehlszeilenschnittstelle oder das Azure-Portal. Weitere Informationen zum programmgesteuerten Erweitern von Datenträgern finden Sie unter [Ändern der Größe eines Datenträgers](expand-os-disk.md).

  So erhöhen Sie die Größe für Betriebssystemdatenträger über das Azure-Portal:

  1. Navigieren Sie im Azure-Portal zur Seite mit den VM-Eigenschaften.
  1. Verwenden Sie die Schaltfläche **Beenden**, um die VM herunterzufahren und ihre Zuordnung aufzuheben.
  1. Wählen Sie im Abschnitt **Datenträger** den Betriebssystemdatenträger aus, den Sie vergrößern möchten.
  1. Wählen Sie im Abschnitt **Datenträger** die Option **Konfiguration** aus, und ändern Sie die **Größe** auf den gewünschten Wert.
  1. Navigieren Sie zurück zur Seite mit den Eigenschaften der VM, und **starten** Sie die VM.
  
  Unter Umständen wird eine Warnmeldung für Betriebssystemdatenträger mit einer Größe von mehr als 2 TB angezeigt. Die Warnung gilt nicht für VMs der Generation 2. Allerdings werden Größen für Betriebssystemdatenträger von mehr als 4 TB *nicht empfohlen*.

* **Unterstützen VMs der Generation 2 den beschleunigten Netzwerkbetrieb?**  
    Ja. Weitere Informationen finden Sie unter [Erstellen eines virtuellen Computers mit beschleunigtem Netzwerkbetrieb](../../virtual-network/create-vm-accelerated-networking-cli.md).

* **Wird VHDX für Generation 2 unterstützt?**  
    Nein, VMs der Generation 2 unterstützen nur VHD.

* **Wird für VMs der Generation 2 Azure Disk Storage Ultra unterstützt?**  
    Ja.

* **Kann ich VMs der Generation 1 zu Generation 2 migrieren?**  
    Nein, Sie können die Generation eines virtuellen Computers nicht mehr ändern, nachdem Sie ihn erstellt haben. Wenn Sie zwischen VM-Generationen wechseln möchten, müssen Sie einen neuen virtuellen Computer einer anderen Generation erstellen.

## <a name="next-steps"></a>Nächste Schritte

* Erfahren Sie mehr über [virtuelle Computer der Generation 2 in Hyper-V](https://docs.microsoft.com/windows-server/virtualization/hyper-v/plan/should-i-create-a-generation-1-or-2-virtual-machine-in-hyper-v).

* Erfahren Sie, wie Sie [eine VHD vorbereiten](prepare-for-upload-vhd-image.md), damit diese aus den lokalen Systemen in Azure hochgeladen werden kann.
