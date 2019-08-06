---
title: Azure-Support für VMs der Generation 2 (Vorschau) | Microsoft-Dokumentation
description: 'Übersicht: Azure-Support für VMs der Generation 2'
services: virtual-machines-windows
documentationcenter: ''
author: laurenhughes
manager: gwallace
editor: ''
tags: azure-resource-manager
ms.service: virtual-machines-windows
ms.workload: infrastructure-services
ms.tgt_pltfrm: vm-windows
ms.devlang: na
ms.topic: article
ms.date: 05/23/2019
ms.author: lahugh
ms.openlocfilehash: 7cd938369cdc4f4ca711a442d89cd6ef3d6d08e8
ms.sourcegitcommit: 75a56915dce1c538dc7a921beb4a5305e79d3c7a
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 07/24/2019
ms.locfileid: "68477085"
---
# <a name="support-for-generation-2-vms-preview-on-azure"></a>Unterstützung für VMs der Generation 2 (Vorschau) in Azure

> [!IMPORTANT]
> Der Azure-Support für VMs der Generation 2 (Vorschau) befindet sich derzeit in der Vorschauphase.
> Diese Vorschauversion wird ohne Vereinbarung zum Servicelevel bereitgestellt und ist nicht für Produktionsworkloads vorgesehen. Manche Features werden möglicherweise nicht unterstützt oder sind nur eingeschränkt verwendbar.
> Weitere Informationen finden Sie unter [Zusätzliche Nutzungsbestimmungen für Microsoft Azure-Vorschauversionen](https://azure.microsoft.com/support/legal/preview-supplemental-terms/).

Die Unterstützung für virtuelle Computer (VMs) der Generation 2 ist jetzt als Vorschauversion in Azure verfügbar. Sie können die Generation eines virtuellen Computers nach der Erstellung nicht mehr ändern, deshalb sollten Sie die Aspekte auf dieser Seite genauestens prüfen, bevor Sie eine Generation auswählen. 

VMs der Generation 2 unterstützen wichtige Features, die bei VMs der Generation 1 nicht unterstützt werden. Zu diesen Features gehören mehr Speicher, Intel Software Guard Extensions (Intel SGX) und virtualisierter persistenter Speicher (vPMEM). VMs der Generation 2 weisen auch einige Features auf, die in Azure noch nicht unterstützt werden. Weitere Informationen finden Sie im Abschnitt [Features und Funktionen](#features-and-capabilities).

VMs der Generation 2 verwenden die neue UEFI-basierte Startarchitektur und nicht mehr die BIOS-basierte Architektur von VMs der Generation 1. Im Vergleich zu VMs der Generation 1 können VMs der Generation 2 eine verbesserte Start- und Installationszeit aufweisen. Einen Überblick über VMs der Generation 2 und einige der Unterschiede zwischen Generation 1 und Generation 2 finden Sie unter [Sollte ich einen virtuellen Computer der Generation 1 oder 2 in Hyper-V erstellen?](https://docs.microsoft.com/windows-server/virtualization/hyper-v/plan/should-i-create-a-generation-1-or-2-virtual-machine-in-hyper-v).

## <a name="generation-2-vm-sizes"></a>VM-Größen der Generation 2

VMs der Generation 1 werden von allen VM-Größen in Azure unterstützt. Azure bietet jetzt Unterstützung für die Vorschauversion von Generation 2 für die folgenden ausgewählten VM-Serien:

* [B-Serie](https://docs.microsoft.com/azure/virtual-machines/windows/b-series-burstable)
* [Dsv2-Serie](https://docs.microsoft.com/azure/virtual-machines/windows/sizes-general#dsv2-series) und [Dsv3-Serie](https://docs.microsoft.com/azure/virtual-machines/windows/sizes-general#dsv3-series-1)
* [Esv3-Serie](https://docs.microsoft.com/azure/virtual-machines/windows/sizes-memory#esv3-series)
* [Fsv2-Serie](https://docs.microsoft.com/azure/virtual-machines/windows/sizes-compute#fsv2-series-1)
* [GS-Serie](https://docs.microsoft.com/azure/virtual-machines/windows/sizes-previous-gen#gs-series)
* [Ls-Serie](https://docs.microsoft.com/azure/virtual-machines/windows/sizes-previous-gen#ls-series) und [Lsv2-Serie](https://docs.microsoft.com/azure/virtual-machines/windows/sizes-storage#lsv2-series)
* [Mv2-Serie](https://docs.microsoft.com/azure/virtual-machines/windows/sizes-memory#mv2-series)
* [NCv2-Serie](https://docs.microsoft.com/azure/virtual-machines/windows/sizes-gpu#ncv2-series) und [NCv3-Serie](https://docs.microsoft.com/azure/virtual-machines/windows/sizes-gpu#ncv3-series)
* [ND-Serie](https://docs.microsoft.com/azure/virtual-machines/windows/sizes-gpu#nd-series)

## <a name="generation-2-vm-images-in-azure-marketplace"></a>VM-Images der Generation 2 in Azure Marketplace

VMs der Generation 2 unterstützen die folgenden Marketplace-Images:

* Windows Server 2019 Datacenter
* Windows Server 2016 Datacenter
* Windows Server 2012 R2 Datacenter
* Windows Server 2012 Datacenter

## <a name="on-premises-vs-azure-generation-2-vms"></a>Lokale VMs und Azure-VMs der Generation 2 im Vergleich

Azure unterstützt zurzeit einige der Funktionen nicht, die lokale Hyper-V-Instanzen für VMs der Generation 2 unterstützen.

| Funktionen der Generation 2                | Lokale Hyper-V-Instanz | Azure |
|-------------------------------------|---------------------|-------|
| Sicherer Start                         | :heavy_check_mark:  | :x:   |
| Abgeschirmte VM                         | :heavy_check_mark:  | :x:   |
| vTPM                                | :heavy_check_mark:  | :x:   |
| Virtualisierungsbasierte Sicherheit (VBS) | :heavy_check_mark:  | :x:   |
| VHDX-Format                         | :heavy_check_mark:  | :x:   |

## <a name="features-and-capabilities"></a>Features und Funktionen

### <a name="generation-1-vs-generation-2-features"></a>Features der Generationen 1 und 2 im Vergleich

| Feature | Generation 1 | Generation 2 |
|---------|--------------|--------------|
| Start             | PCAT                      | UEFI                               |
| Datenträgercontroller | IDE                       | SCSI                               |
| VM-Größen         | Alle VM-Größen | Nur VMs, die Storage Premium unterstützen |

### <a name="generation-1-vs-generation-2-capabilities"></a>Funktionen der Generationen 1 und 2 im Vergleich

| Funktion | Generation 1 | Generation 2 |
|------------|--------------|--------------|
| Betriebssystem-Datenträger > 2 TB                    | :x:                        | :heavy_check_mark: |
| Benutzerdefinierter Datenträger/Image/Swapbetriebssystem         | :heavy_check_mark:         | :heavy_check_mark: |
| Unterstützung für VM-Skalierungsgruppe | :heavy_check_mark:         | :heavy_check_mark: |
| ASR/Backup                        | :heavy_check_mark:         | :x:                |
| Shared Image Gallery              | :heavy_check_mark:         | :x:                |
| Azure Disk Encryption             | :heavy_check_mark:         | :x:                |

## <a name="creating-a-generation-2-vm"></a>Erstellen einer VM der Generation 2

### <a name="marketplace-image"></a>Marketplace-Image

VMs der Generation 2 können aus einem Marketplace-Image, das den UEFI-Start unterstützt, über das Azure-Portal oder die Azure-Befehlszeilenschnittstelle erstellt werden.

Das `windowsserver-gen2preview`-Angebot enthält nur Windows-Images der Generation 2. Dieses Paket vermeidet Verwechslungen zwischen Images der Generationen 1 und 2. Wählen Sie zum Erstellen einer VM der Generation 2 **Images** aus diesem Angebot aus, und führen Sie den Standardprozess für die VM-Erstellung aus.

Im Marketplace werden derzeit die folgenden Windows-Images der Generation 2 angeboten:

* 2019-datacenter-gen2
* 2016-datacenter-gen2
* 2012-r2-datacenter-gen2
* 2012-datacenter-gen2

Im Abschnitt [Features und Funktionen](#features-and-capabilities) finden Sie eine aktuelle Liste der unterstützten Marketplace-Images.

### <a name="managed-image-or-managed-disk"></a>Verwaltetes Image oder verwalteter Datenträger

Sie können VMs der Generation 2 aus einem verwalteten Image oder einem verwalteten Datenträger auf die gleiche Weise erstellen, wie Sie eine VM der Generation 1 erstellen würden.

### <a name="virtual-machine-scale-sets"></a>VM-Skalierungsgruppen

VMs der Generation 2 können auch mithilfe von VM-Skalierungsgruppen erstellt werden. Verwenden Sie an der Azure-Befehlszeilenschnittstelle Azure-Skalierungsgruppen, um VMs der Generation 2 zu erstellen.

## <a name="frequently-asked-questions"></a>Häufig gestellte Fragen

* **Sind VMs der Generation 2 in allen Azure-Regionen verfügbar?**  
    Ja. Allerdings stehen nicht alle [VM-Größen der Generation 2](#generation-2-vm-sizes) in allen Regionen zur Verfügung. Die Verfügbarkeit von VMs der Generation 2 richtet sich nach der Verfügbarkeit der VM-Größe.

* **Gibt es einen Preisunterschied zwischen VMs der 1. und 2. Generation?**  
   Nein.

* **Wie erhöhe ich die Größe des Betriebssystemdatenträgers?**  
  Betriebssystemdatenträger mit einer Größe von mehr als 2 TB sind neu bei VMs der 2. Generation. Standardmäßig sind die Betriebssystemdatenträger für VMs der Generation 2 kleiner als 2 TB. Sie können die Größe des Datenträgers auf die empfohlene maximale Größe von 4 TB erhöhen. Verwenden Sie zum Erhöhen der Größe von Betriebssystemdatenträgern die Azure-Befehlszeilenschnittstelle oder das Azure-Portal. Weitere Informationen zum programmgesteuerten Erweitern von Datenträgern finden Sie unter [Ändern der Größe eines Datenträgers](expand-os-disk.md).

  So erhöhen Sie die Größe für Betriebssystemdatenträger über das Azure-Portal

  1. Navigieren Sie im Azure-Portal zur Seite mit den VM-Eigenschaften.
  1. Verwenden Sie die Schaltfläche **Beenden**, um die VM herunterzufahren und ihre Zuordnung aufzuheben.
  1. Wählen Sie im Abschnitt **Datenträger** den Betriebssystemdatenträger aus, den Sie vergrößern möchten.
  1. Wählen Sie im Abschnitt **Datenträger** die Option **Konfiguration** aus, und ändern Sie die **Größe** auf den gewünschten Wert.
  1. Navigieren Sie zurück zur Seite mit den Eigenschaften der VM, und **starten** Sie die VM.
  
  Unter Umständen wird eine Warnmeldung für Betriebssystemdatenträger mit einer Größe von mehr als 2 TB angezeigt. Die Warnung gilt nicht für VMs der Generation 2. Allerdings werden Größen für Betriebssystemdatenträger von mehr als 4 TB *nicht empfohlen*.

* **Unterstützen VMs der Generation 2 den beschleunigten Netzwerkbetrieb?**  
    Ja. Weitere Informationen finden Sie unter [Erstellen eines virtuellen Computers mit beschleunigtem Netzwerkbetrieb](../../virtual-network/create-vm-accelerated-networking-cli.md).

* **Wird VHDX für Generation 2 unterstützt?**  
    Nein, VMs der Generation 2 unterstützen nur VHD.

* **Wird für VMs der Generation 2 Azure Disk Storage Ultra unterstützt?**  
    Ja.

* **Kann ich VMs der Generation 1 zu Generation 2 migrieren?**  
    Nein, Sie können die Generation eines virtuellen Computers nicht mehr ändern, nachdem Sie ihn erstellt haben. Wenn Sie zwischen VM-Generationen wechseln möchten, müssen Sie einen neuen virtuellen Computer einer anderen Generation erstellen.

## <a name="next-steps"></a>Nächste Schritte

* Erfahren Sie mehr über [virtuelle Computer der Generation 2 in Hyper-V](https://docs.microsoft.com/windows-server/virtualization/hyper-v/plan/should-i-create-a-generation-1-or-2-virtual-machine-in-hyper-v).

* Erfahren Sie, wie Sie [eine VHD vorbereiten](prepare-for-upload-vhd-image.md), damit diese aus den lokalen Systemen in Azure hochgeladen werden kann.
