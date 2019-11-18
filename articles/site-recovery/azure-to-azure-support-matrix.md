---
title: Unterstützungsmatrix für die Notfallwiederherstellung von Azure-VMs zwischen Azure-Regionen mit Azure Site Recovery | Microsoft-Dokumentation
description: Fasst die Voraussetzungen und die Unterstützung für Notfallwiederherstellung von Azure-VMs aus einer Region in einer anderen mit Azure Site Recovery zusammen.
author: rayne-wiselman
manager: carmonm
ms.service: site-recovery
ms.topic: article
ms.date: 10/26/2019
ms.author: raynew
ms.openlocfilehash: 726b7ebd21dadf0e179f2752a9783d63492cb4c3
ms.sourcegitcommit: 6c2c97445f5d44c5b5974a5beb51a8733b0c2be7
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 11/05/2019
ms.locfileid: "73622470"
---
# <a name="support-matrix-for-replicating-azure-vms-from-one-region-to-another"></a>Unterstützungsmatrix für die Replikation von Azure-VMs von einer Region in eine andere

Dieser Artikel fasst die Unterstützung und die Voraussetzungen für die Notfallwiederherstellung von Azure-VMs aus einer Azure-Region in einer anderen mit dem Dienst [Azure Site Recovery](site-recovery-overview.md) zusammen.


## <a name="deployment-method-support"></a>Unterstützung für die Bereitstellungsmethode

**Bereitstellung** |  **Unterstützung**
--- | ---
**Azure-Portal** | Unterstützt.
**PowerShell** | Unterstützt. [Weitere Informationen](azure-to-azure-powershell.md)
**REST-API** | Unterstützt.
**BEFEHLSZEILENSCHNITTSTELLE (CLI)** | Derzeit nicht unterstützt


## <a name="resource-support"></a>Ressourcenunterstützung

**Ressourcenaktion** | **Details**
--- | --- 
**Tresore über Ressourcengruppen hinweg verschieben** | Nicht unterstützt
**Verschieben von Compute-, Speicher- und Netzwerkressourcen über Ressourcengruppen hinweg** | Nicht unterstützt.<br/><br/> Wenn Sie eine VM oder zugehörige Komponenten wie Speicher bzw. Netzwerke verschieben, nachdem die VM repliziert wurde, müssen Sie die Replikation für die VM deaktivieren und dann wieder aktivieren.
**Replizieren von Azure-VMs von einem Abonnement in ein anderes zur Notfallwiederherstellung** | Innerhalb des gleichen Azure Active Directory-Mandanten unterstützt.
**Migrieren von VMs zwischen Regionen innerhalb von unterstützten geografischen Clustern (innerhalb von Abonnements und übergreifend)** | Innerhalb des gleichen Azure Active Directory-Mandanten unterstützt.
**Migrieren von VMs in derselben Region** | Nicht unterstützt.

## <a name="region-support"></a>Unterstützung für Regionen

Sie können virtuelle Computer zwischen zwei beliebigen Regionen im gleichen geografischen Cluster replizieren. Geographische Cluster werden unter Berücksichtigung von Datenlatenz und Souveränität definiert.


**Geografischer Cluster** | **Azure-Regionen**
-- | --
Amerika | Kanada, Osten; Kanada, Mitte; USA, Süden-Mitte; USA, Westen-Mitte; USA, Osten; USA, Osten 2; USA, Westen; USA, Westen 2; USA, Mitte; USA, Norden-Mitte
Europa | „Vereinigtes Königreich, Westen“, „Vereinigtes Königreich, Süden“, „Europa, Norden“, „Europa, Westen“, „Frankreich, Mitte“, „Frankreich, Süden“, „Südafrika, Westen“, „Südafrika, Norden“, „Norwegen, Osten“, „Norwegen, Westen“
Asien | „Indien, Süden“, „Indien, Mitte“, „Indien, Westen“, „Asien, Südosten“, „Asien, Osten“, „Japan, Osten“, „Japan, Westen“, „Südkorea, Mitte“, „Südkorea, Süden“, „VAE, Mitte“, „VAE, Norden“
Australien   | Australien, Osten; Australien, Südosten; Australien, Mitte; Australien, Mitte 2
Azure Government    | „US GOV Virginia“; „US GOV Iowa“; „US GOV Arizona“; „US GOV Texas“; „US DoD, Osten“; „US DoD, Mitte“ 
Deutschland | „Deutschland, Mitte“; „Deutschland, Nordosten“
China | China, Osten; China, Norden; China, Norden 2; China, Osten 2
Eingeschränkte Regionen, die für eine Notfallwiederherstellung innerhalb des Landes reserviert sind |„Deutschland, Norden“ reserviert für Kunden in „Deutschland, Westen-Mitte“, „Schweiz, Westen“ reserviert für Kunden in „Schweiz, Norden“, „Frankreich, Süden“ reserviert für Kunden in „Frankreich, Mitte“ 

>[!NOTE]
>
> - Für **Brasilien, Süden** können Sie eine Replikation und ein Failover zu diesen Regionen durchführen: USA, Süden-Mitte; USA, Westen-Mitte; USA, Osten; USA, Osten 2; USA, Westen; USA, Westen 2 und USA, Norden-Mitte.
> - „Brasilien Süd“ kann nur als Quellregion verwendet werden, von der aus virtuelle Computer mithilfe von Site Recovery replizieren können. Es kann nicht als Zielregion dienen. Dies liegt an Latenzproblemen aufgrund der geografischen Entfernungen. Beachten Sie Folgendes: Wenn Sie ein Failover von „Brasilien, Süden“ als Quellregion zu einem Ziel ausführen, wird ein Failback zu „Brasilien, Süden“ aus der Zielregion unterstützt.
> - Sie können innerhalb von Regionen arbeiten, auf die Sie entsprechenden Zugriff haben.
> - Wenn Sie die Region nicht sehen können, in der Sie einen Tresor erstellen möchten, dann stellen Sie sicher, dass Ihr Abonnement in dieser Region Zugriff auf das Erstellen von Ressourcen hat.
> - Wenn Sie beim Aktivieren der Replikation eine Region in einem geografischen Cluster nicht sehen können, stellen Sie sicher, dass Ihr Abonnement über Berechtigungen zum Erstellen von virtuellen Computern in dieser Region verfügt.



## <a name="cache-storage"></a>Cachespeicher

In dieser Tabelle ist die Unterstützung für das Cachespeicherkonto zusammengefasst, das von Site Recovery während der Replikation verwendet wird.

**Einstellung** | **Unterstützung** | **Details**
--- | --- | ---
Allgemeine V2-Speicherkonten (heiße und kalte Ebene) | Unterstützt | Die Nutzung von GPv2 wird nicht empfohlen, da die Transaktionskosten für V2- erheblich höher als für V1-Speicherkonten sind.
Storage Premium | Nicht unterstützt | Standard-Speicherkonten werden für den Cachespeicher verwendet, um die Kosten zu optimieren.
Azure Storage-Firewalls für virtuelle Netzwerke  | Unterstützt | Wenn Sie ein Cache- oder Zielspeicherkonto mit aktivierter Firewall verwenden, müssen Sie [vertrauenswürdigen Microsoft-Diensten den Zugriff erlauben](https://docs.microsoft.com/azure/storage/common/storage-network-security#exceptions), indem Sie die entsprechende Option auswählen.


## <a name="replicated-machine-operating-systems"></a>Replizierte Computerbetriebssysteme

Site Recovery unterstützt die Replikation von Azure-VMs, auf denen die in diesem Abschnitt angegebenen Betriebssysteme ausgeführt werden.

### <a name="windows"></a>Windows


**Betriebssystem** | **Details**
--- | ---
Windows Server 2019 | Unterstützt für Server Core, Server mit Desktopdarstellung.
Windows Server 2016  | Unterstützt für Server Core, Server mit Desktopdarstellung.
Windows Server 2012 R2 | Unterstützt.
Windows Server 2012 | Unterstützt.
Windows Server 2008 R2 mit SP1/SP2 | Unterstützt.<br/><br/> Ab Version 9.30.x.x (Veröffentlichung vermutlich Anfang November 2019) der Mobility Service-Erweiterung für virtuelle Azure-Computer müssen Sie eine [Windows-Wartungsstapelaktualisierung (SSU)](https://support.microsoft.com/help/4490628) und ein [SHA-2-Update](https://support.microsoft.com/help/4474419) auf Computern installieren, die unter Windows Server 2008 R2 SP1/SP2 ausgeführt werden.  SHA-1 wird ab September 2019 nicht mehr unterstützt, und wenn SHA-2-Codesignierung nicht aktiviert ist, wird die Installation bzw. das Upgrade der Agent-Erweiterung nicht ordnungsgemäß durchgeführt. Weitere Informationen zum [SHA-2-Upgrade und den Anforderungen](https://aka.ms/SHA-2KB).
Windows Server 2008 mit SP2 | Ab Version 9.30.x.x (Veröffentlichung vermutlich Anfang November 2019) der Mobility Service-Erweiterung für virtuelle Azure-Computer müssen Sie eine [Windows-Wartungsstapelaktualisierung (SSU)](https://support.microsoft.com/help/4493730) und ein [SHA-2-Update](https://support.microsoft.com/help/4474419) auf Computern installieren, die unter Windows Server 2008 SP2 mit SP2 ausgeführt werden.  SHA-1 wird ab September 2019 nicht mehr unterstützt, und wenn SHA-2-Codesignierung nicht aktiviert ist, wird die Installation bzw. das Upgrade der Agent-Erweiterung nicht ordnungsgemäß durchgeführt. Weitere Informationen zum [SHA-2-Upgrade und den Anforderungen](https://aka.ms/SHA-2KB).
Windows 10 (x64) | Unterstützt.
Windows 8.1 (x64) | Unterstützt.
Windows 8 (x64) | Unterstützt.
Windows 7 (x64) mit SP1 und höher | Ab Version 9.30.x.x (Veröffentlichung vermutlich Anfang November 2019) der Mobility Service-Erweiterung für virtuelle Azure-Computer müssen Sie eine [Windows-Wartungsstapelaktualisierung (SSU)](https://support.microsoft.com/help/4490628) und ein [SHA-2-Update](https://support.microsoft.com/help/4474419) auf Computern installieren, die unter Windows 7 mit SP1 ausgeführt werden.  SHA-1 wird ab September 2019 nicht mehr unterstützt, und wenn SHA-2-Codesignierung nicht aktiviert ist, wird die Installation bzw. das Upgrade der Agent-Erweiterung nicht ordnungsgemäß durchgeführt. Weitere Informationen zum [SHA-2-Upgrade und den Anforderungen](https://aka.ms/SHA-2KB).



#### <a name="linux"></a>Linux

**Betriebssystem** | **Details**
--- | ---
Red Hat Enterprise Linux | 6.7, 6.8, 6.9, 6.10, 7.0, 7.1, 7.2, 7.3, 7.4, 7.5, 7.6, 7.7, 8.0
CentOS | 6.5, 6.6, 6.7, 6.8, 6.9, 6.10, 7.0, 7.1, 7.2, 7.3, 7.4, 7.5, 7.6, 7.7, 8.0
Ubuntu 14.04 LTS Server | [Unterstützte Kernel-Versionen](#supported-ubuntu-kernel-versions-for-azure-virtual-machines)
Ubuntu 16.04 LTS Server | [Unterstützte Kernel-Version](#supported-ubuntu-kernel-versions-for-azure-virtual-machines)<br/><br/> Bei Ubuntu-Servern mit kennwortbasierter Authentifizierung und Anmeldung und mit dem Paket „cloud-init“ zum Konfigurieren von Cloud-VMs kann bei einem Failover die kennwortbasierte Anmeldung deaktiviert sein (je nach cloud-init-Konfiguration). Die kennwortbasierte Anmeldung kann auf der VM wieder aktiviert werden, indem (für die VM, für die das Failover ausgeführt wurde) das Kennwort im Azure-Portal unter „Support“ > „Problembehandlung“ im Menü „Einstellungen“ zurückgesetzt wird.
Ubuntu 18.04 LTS Server | [Unterstützte Kernel-Version](#supported-ubuntu-kernel-versions-for-azure-virtual-machines)
Debian 7 | [Unterstützte Kernel-Versionen](#supported-debian-kernel-versions-for-azure-virtual-machines)
Debian 8 | [Unterstützte Kernel-Versionen](#supported-debian-kernel-versions-for-azure-virtual-machines)
SUSE Linux Enterprise Server 12 | SP1, SP2, SP3, SP4. [(Unterstützte Kernel-Versionen)](#supported-suse-linux-enterprise-server-12-kernel-versions-for-azure-virtual-machines)
SUSE Linux Enterprise Server 11 | SP3<br/><br/> Ein Upgrade von replizierenden Computern von SP3 auf SP4 wird nicht unterstützt. Wenn ein replizierter Computer aktualisiert wurde, müssen Sie die Replikation deaktivieren und nach dem Upgrade dann wieder aktivieren.
SUSE Linux Enterprise Server 11 | SP4
Oracle Linux | 6.4, 6.5, 6.6, 6.7, 6.8, 6.9, 6.10, 7.0, 7.1, 7.2, 7.3, 7.4, 7.5, 7.6, 7.7<br/><br/> Mit einem Red Hat-kompatiblen Kernel oder Unbreakable Enterprise Kernel Release 3, 4 und 5 (UEK3, UEK4, UEK5) 


#### <a name="supported-ubuntu-kernel-versions-for-azure-virtual-machines"></a>Unterstützte Ubuntu-Kernelversionen für virtuelle Azure-Computer

**Release** | **Mobility Service-Version** | **Kernelversion** |
--- | --- | --- |
14.04 LTS | 9.28 | 3.13.0-24-generic bis 3.13.0-170-generic,<br/>3.16.0-25-generic bis 3.16.0-77-generic,<br/>3.19.0-18-generic bis 3.19.0-80-generic,<br/>4.2.0-18-generic bis 4.2.0-42-generic,<br/>4.4.0-21-generic bis 4.4.0-148-generic,<br/>4.15.0-1023-azure bis 4.15.0-1045-azure |
14.04 LTS | 9.27 | 3.13.0-24-generic bis 3.13.0-170-generic,<br/>3.16.0-25-generic bis 3.16.0-77-generic,<br/>3.19.0-18-generic bis 3.19.0-80-generic,<br/>4.2.0-18-generic bis 4.2.0-42-generic,<br/>4.4.0-21-generic bis 4.4.0-148-generic,<br/>4.15.0-1023-azure bis 4.15.0-1045-azure |
14.04 LTS | 9,26 | 3.13.0-24-generic bis 3.13.0-170-generic,<br/>3.16.0-25-generic bis 3.16.0-77-generic,<br/>3.19.0-18-generic bis 3.19.0-80-generic,<br/>4.2.0-18-generic bis 4.2.0-42-generic,<br/>4.4.0-21-generic bis 4.4.0-148-generic,<br/>4.15.0-1023-azure bis 4.15.0-1045-azure |
14.04 LTS | 9.25 | 3.13.0-24-generic bis 3.13.0-169-generic,<br/>3.16.0-25-generic bis 3.16.0-77-generic,<br/>3.19.0-18-generic bis 3.19.0-80-generic,<br/>4.2.0-18-generic bis 4.2.0-42-generic,<br/>4.4.0-21-generic bis 4.4.0-146-generic,<br/>4.15.0-1023-azure bis 4.15.0-1042-azure |
|||
16.04 LTS | 9.28 | 4.4.0-21-generic bis 4.4.0-159-generic,<br/>4.8.0-34-generic bis 4.8.0-58-generic,<br/>4.10.0-14-generic bis 4.10.0-42-generic,<br/>4.11.0-13-generic bis 4.11.0-14-generic,<br/>4.13.0-16-generic bis 4.13.0-45-generic,<br/>4.15.0-13-generic bis 4.15.0-58-generic<br/>4.11.0-1009-azure bis 4.11.0-1016-azure,<br/>4.13.0-1005-azure bis 4.13.0-1018-azure <br/>4.15.0-1012-azure bis 4.15.0-1055-azure|
16.04 LTS | 9.27 | 4.4.0-21-generic bis 4.4.0-154-generic,<br/>4.8.0-34-generic bis 4.8.0-58-generic,<br/>4.10.0-14-generic bis 4.10.0-42-generic<br/>4.11.0-13-generic bis 4.11.0-14-generic,<br/>4.13.0-16-generic bis 4.13.0-45-generic,<br/>4.15.0-13-generic bis 4.15.0-55-generic<br/>4.11.0-1009-azure bis 4.11.0-1016-azure,<br/>4.13.0-1005-azure bis 4.13.0-1018-azure <br/>4.15.0-1012-azure bis 4.15.0-1051-azure|
16.04 LTS | 9,26 | 4.4.0-21-generic bis 4.4.0-148-generic,<br/>4.8.0-34-generic bis 4.8.0-58-generic,<br/>4.10.0-14-generic bis 4.10.0-42-generic<br/>4.11.0-13-generic bis 4.11.0-14-generic,<br/>4.13.0-16-generic bis 4.13.0-45-generic,<br/>4.15.0-13-generic bis 4.15.0-50-generic<br/>4.11.0-1009-azure bis 4.11.0-1016-azure,<br/>4.13.0-1005-azure bis 4.13.0-1018-azure <br/>4.15.0-1012-azure bis 4.15.0-1045-azure|
16.04 LTS | 9.25 | 4.4.0-21-generic bis 4.4.0-146-generic,<br/>4.8.0-34-generic bis 4.8.0-58-generic,<br/>4.10.0-14-generic bis 4.10.0-42-generic,<br/>4.11.0-13-generic bis 4.11.0-14-generic,<br/>4.13.0-16-generic bis 4.13.0-45-generic,<br/>4.15.0-13-generic bis 4.15.0-48-generic<br/>4.11.0-1009-azure bis 4.11.0-1016-azure,<br/>4.13.0-1005-azure bis 4.13.0-1018-azure <br/>4.15.0-1012-azure bis 4.15.0-1042-azure|
16.04 LTS | 9.24 | 4.4.0-21-generic bis 4.4.0-143-generic,<br/>4.8.0-34-generic bis 4.8.0-58-generic,<br/>4.10.0-14-generic bis 4.10.0-42-generic,<br/>4.11.0-13-generic bis 4.11.0-14-generic,<br/>4.13.0-16-generic bis 4.13.0-45-generic,<br/>4.15.0-13-generic bis 4.15.0-46-generic<br/>4.11.0-1009-azure bis 4.11.0-1016-azure,<br/>4.13.0-1005-azure bis 4.13.0-1018-azure <br/>4.15.0-1012-azure bis 4.15.0-1040-azure|
|||
18.04 LTS | [9.29](https://support.microsoft.com/help/4528026/update-rollup-41-for-azure-site-recovery) | 4.15.0-20-generic bis 4.15.0-64-generic </br> 4.18.0-13-generic bis 4.18.0-25-generic </br> 5.0.0-15-generic bis 5.0.0-29-generic </br> 4.15.0-1009-azure bis 4.15.0-1037-azure </br> 4.18.0-1006-azure bis 4.18.0-1025-azure </br> 5.0.0-1012-azure bis 5.0.0-1020-azure


#### <a name="supported-debian-kernel-versions-for-azure-virtual-machines"></a>Unterstützte Debian-Kernelversionen für virtuelle Azure-Computer

**Release** | **Mobility Service-Version** | **Kernelversion** |
--- | --- | --- |
Debian 7 | 9.25, 9.26, 9.27, 9.28 | 3.2.0-4-amd64 bis 3.2.0-6-amd64, 3.16.0-0.bpo.4-amd64 |
|||
Debian 8 | 9.28 | 3.16.0-4-amd64 bis 3.16.0-10-amd64, 4.9.0-0.bpo.4-amd64 bis 4.9.0-0.bpo.9-amd64 |
Debian 8 | 9.27 | 3.16.0-4-amd64 bis 3.16.0-9-amd64, 4.9.0-0.bpo.4-amd64 bis 4.9.0-0.bpo.9-amd64 |
Debian 8 | 9.25, 9.26 | 3.16.0-4-amd64 bis 3.16.0-8-amd64, 4.9.0-0.bpo.4-amd64 bis 4.9.0-0.bpo.8-amd64 |

#### <a name="supported-suse-linux-enterprise-server-12-kernel-versions-for-azure-virtual-machines"></a>Unterstützte SUSE Linux Enterprise Server 12-Kernelversionen für Azure-VMs

**Release** | **Mobility Service-Version** | **Kernelversion** |
--- | --- | --- |
SUSE Linux Enterprise Server 12 (SP1, SP2, SP3, SP4) | 9.28 | SP1 3.12.49-11-default bis 3.12.74-60.64.40-default</br></br> SP1(LTSS) 3.12.74-60.64.45-default bis 3.12.74-60.64.118-default</br></br> SP2 4.4.21-69-default bis 4.4.120-92.70-default</br></br>SP2(LTSS) 4.4.121-92.73-default bis 4.4.121-92.117-default</br></br>SP3 4.4.73-5-default bis 4.4.180-94.100-default</br></br>SP3 4.4.138-4.7-azure bis 4.4.180-4.31-azure</br></br>SP4 4.12.14-94.41-default bis 4.12.14-95.29-default</br>SP4 4.12.14-6.3-azure bis 4.12.14-6.23-azure |
SUSE Linux Enterprise Server 12 (SP1, SP2, SP3, SP4) | 9.27 | SP1 3.12.49-11-default bis 3.12.74-60.64.40-default</br></br> SP1(LTSS) 3.12.74-60.64.45-default bis 3.12.74-60.64.115-default</br></br> SP2 4.4.21-69-default bis 4.4.120-92.70-default</br></br>SP2(LTSS) 4.4.121-92.73-default bis 4.4.121-92.117-default</br></br>SP3 4.4.73-5-default bis 4.4.180-94.97-default</br></br>SP3 4.4.138-4.7-azure bis 4.4.180-4.31-azure</br></br>SP4 4.12.14-94.41-default bis 4.12.14-95.24-default</br>SP4 4.12.14-6.3-azure bis 4.12.14-6.18-azure |
SUSE Linux Enterprise Server 12 (SP1, SP2, SP3, SP4) | 9,26 | SP1 3.12.49-11-default bis 3.12.74-60.64.40-default</br></br> SP1(LTSS) 3.12.74-60.64.45-default bis 3.12.74-60.64.110-default</br></br> SP2 4.4.21-69-default bis 4.4.120-92.70-default</br></br>SP2(LTSS) 4.4.121-92.73-default bis 4.4.121-92.109-default</br></br>SP3 4.4.73-5-default bis 4.4.178-94.91-default</br></br>SP3 4.4.138-4.7-azure bis 4.4.178-4.28-azure</br></br>SP4 4.12.14-94.41-default bis 4.12.14-95.16-default</br>SP4 4.12.14-6.3-azure bis 4.12.14-6.9-azure |
SUSE Linux Enterprise Server 12 (SP1, SP2, SP3, SP4) | 9.25 | SP1 3.12.49-11-default bis 3.12.74-60.64.40-default</br></br> SP1(LTSS) 3.12.74-60.64.45-default bis 3.12.74-60.64.107-default</br></br> SP2 4.4.21-69-default bis 4.4.120-92.70-default</br></br>SP2(LTSS) 4.4.121-92.73-default bis 4.4.121-92.104-default</br></br>SP3 4.4.73-5-default bis 4.4.176-94.88-default</br></br>SP3 4.4.138-4.7-azure bis 4.4.176-4.25-azure</br></br>SP4 4.12.14-94.41-default bis 4.12.14-95.13-default</br>SP4 4.12.14-6.3-azure bis 4.12.14-6.9-azure |

## <a name="replicated-machines---linux-file-systemguest-storage"></a>Replizierte Computer – Linux-Dateisystem/Gastspeicher

* Dateisysteme: ext3, ext4, ReiserFS (nur Suse Linux Enterprise Server), XFS, BTRFS
* Volume-Manager: LVM2
* Multipfadsoftware: Gerätezuordnung


## <a name="replicated-machines---compute-settings"></a>Replizierte Computer – Compute-Einstellungen

**Einstellung** | **Unterstützung** | **Details**
--- | --- | ---
Size | Jede Größe von virtuellen Azure-Computern mit mindestens 2 CPU-Kernen und 1 GB RAM | Überprüfen Sie die [Größen der virtuellen Azure-Computer](../virtual-machines/windows/sizes.md).
Verfügbarkeitsgruppen | Unterstützt | Wenn Sie die Replikation für eine Azure-VM mit den Standardoptionen aktivieren, wird basierend auf den Einstellungen der Quellregion automatisch eine Verfügbarkeitsgruppe erstellt. Sie können diese Einstellungen ändern.
Verfügbarkeitszonen | Unterstützt |
Hybridnutzungsvorteil (Hybrid Use Benefit, HUB) | Unterstützt | Wenn für den virtuellen Quellcomputer eine HUB-Lizenz aktiviert wurde, verwendet auch die Testfailover- oder Failover-VM die HUB-Lizenz.
VM-Skalierungsgruppen | Nicht unterstützt |
Azure-Katalogimages – von Microsoft veröffentlicht | Unterstützt | Wird unterstützt, wenn auf der VM ein unterstütztes Betriebssystem ausgeführt wird.
Azure-Katalogimages – von Drittanbietern veröffentlicht | Unterstützt | Wird unterstützt, wenn auf der VM ein unterstütztes Betriebssystem ausgeführt wird.
Benutzerdefinierte Images – von Drittanbietern veröffentlicht | Unterstützt | Wird unterstützt, wenn auf der VM ein unterstütztes Betriebssystem ausgeführt wird.
Mit Site Recovery migrierte virtuelle Computer | Unterstützt | Wenn eine VMware-VM oder ein physischer Computer mit Site Recovery zu Azure migriert wurde, müssen Sie die ältere Version des Mobility Service deinstallieren, die auf dem Computer ausgeführt wird, und den Computer neu starten, bevor er in einer anderen Azure-Region repliziert werden kann.
RBAC-Richtlinien | Nicht unterstützt | Richtlinien für die rollenbasierte Zugriffssteuerung (Role Based Access Control, RBAC) auf virtuellen Computern werden nicht in die Failover-VM in der Zielregion repliziert.
Erweiterungen | Nicht unterstützt | Erweiterungen werden nicht auf die Failover-VM in der Zielregion repliziert. Nach dem Failover müssen sie manuell installiert werden.

## <a name="replicated-machines---disk-actions"></a>Replizierte Computer – Datenträgeraktionen

**Aktion** | **Details**
-- | ---
Größe des Datenträgers auf einer replizierten VM ändern | Unterstützt
Hinzufügen eines Datenträgers zu einem replizierten virtuellen Computer | Unterstützt

## <a name="replicated-machines---storage"></a>Replizierte Computer – Speicher

In dieser Tabelle ist die Unterstützung für den Betriebssystemdatenträger, Datenträger und temporären Datenträger der Azure-VM zusammengefasst.

- Es ist wichtig, dass die VM-Datenträgergrenzwerte und -Ziele für [Linux](../virtual-machines/linux/disk-scalability-targets.md)- und [Windows](../virtual-machines/windows/disk-scalability-targets.md)-VMs eingehalten werden, um Leistungsprobleme zu vermeiden.
- Wenn Sie für die Bereitstellung die Standardeinstellungen verwenden, erstellt Site Recovery Datenträger und Speicherkonten automatisch basierend auf den Einstellungen für die Datenquelle.
- Achten Sie darauf, dass Sie die Richtlinien befolgen, wenn Sie Anpassungen vornehmen möchten.

**Komponente** | **Unterstützung** | **Details**
--- | --- | ---
Maximale Größe des Betriebssystemdatenträgers | 2\.048 GB | [Erfahren Sie mehr](../virtual-machines/windows/managed-disks-overview.md) zu VM-Datenträgern.
Temporärer Datenträger | Nicht unterstützt | Der temporäre Datenträger ist immer von der Replikation ausgeschlossen.<br/><br/> Speichern Sie auf dem temporären Datenträger keine persistenten Daten. [Weitere Informationen](../virtual-machines/windows/managed-disks-overview.md)
Maximale Größe des Datenträgers | 8\.192 GB für verwaltete Datenträger<br></br>4\.095 GB für nicht verwaltete Datenträger|
Minimale Größe des Datenträgers | Keine Einschränkung für nicht verwaltete Datenträger. 2 GB für verwaltete Datenträger | 
Maximale Anzahl von Datenträgern | Bis zu 64, gemäß der Unterstützung für eine bestimmte Azure-VM-Größe | [Erfahren Sie mehr](../virtual-machines/windows/sizes.md) zu VM-Größen.
Änderungsrate für Datenträger | Maximal 10 MBit/s pro Datenträger für Storage Premium. Maximal 2 MBit/s pro Datenträger für Standardspeicher. | Wenn die durchschnittliche Datenänderungsrate auf dem Datenträger dauerhaft über dem Maximalwert liegt, kann dies durch die Replikation nicht aufgeholt werden.<br/><br/>  Falls der Maximalwert aber nur sporadisch überschritten wird, kann die Replikation aufholen, aber es kommt ggf. zu einer leichten Verzögerung bei den Wiederherstellungspunkten.
Datenträger – Standard-Speicherkonto | Unterstützt |
Datenträger – Storage Premium-Konto | Unterstützt | Wenn ein virtueller Computer Datenträger in Premium- und Standard-Speicherkonten aufweist, können Sie für jeden Datenträger ein eigenes Zielspeicherkonto auswählen, um sicherzustellen, dass die gleiche Speicherkonfiguration in der Zielregion vorhanden ist.
Verwalteter Datenträger – Standard | Unterstützt in Azure-Regionen, in denen Azure Site Recovery unterstützt wird. |
Verwalteter Datenträger – Premium | Unterstützt in Azure-Regionen, in denen Azure Site Recovery unterstützt wird. |
SSD Standard | Unterstützt |
Redundanz | LRS und GRS werden unterstützt.<br/><br/> ZRS wird nicht unterstützt.
Kalter und heißer Speicher | Nicht unterstützt | VM-Datenträger werden für kalten und heißen Speicher nicht unterstützt
Speicherplätze | Unterstützt |
Verschlüsselung ruhender Daten (SSE) | Unterstützt | SSE ist die Standardeinstellung für Speicherkonten.   
Azure Disk Encryption (ADE) für Windows | Unterstützt für virtuelle Computer mit verwalteten Datenträgern. Virtuelle Computer, die nicht verwaltete Datenträgern nutzen, werden nicht unterstützt. |
Azure Disk Encryption (ADE) für Linux | Nicht unterstützt |
Hinzufügen von Datenträgern im laufendem Betrieb | Unterstützt | Die Aktivierung der Replikation für einen Datenträger, den Sie einer replizierten Azure-VM hinzufügen, wird für VMs unterstützt, die verwaltete Datenträger verwenden.
Entfernen von Datenträgern im laufendem Betrieb | Nicht unterstützt | Wenn Sie Datenträger auf dem virtuellen Computer entfernen, müssen Sie die Replikation deaktivieren und dann für den virtuellen Computer erneut aktivieren.
Ausschließen von Datenträgern | Unterstützung. Sie müssen [PowerShell](azure-to-azure-exclude-disks.md) zum Konfigurieren verwenden. |  Temporäre Datenträger sind standardmäßig ausgeschlossen.
Speicherplätze direkt  | Für absturzkonsistente Wiederherstellungspunkte unterstützt. Anwendungskonsistente Wiederherstellungspunkte werden nicht unterstützt. |
Dateiserver mit horizontaler Skalierung  | Für absturzkonsistente Wiederherstellungspunkte unterstützt. Anwendungskonsistente Wiederherstellungspunkte werden nicht unterstützt. |
LRS | Unterstützt |
GRS | Unterstützt |
RA-GRS | Unterstützt |
ZRS | Nicht unterstützt |
Kalter und heißer Speicher | Nicht unterstützt | Datenträger für virtuelle Computer werden auf kaltem und heißem Speicher nicht unterstützt.
Azure Storage-Firewalls für virtuelle Netzwerke  | Unterstützt | Wenn Sie den Zugriff auf virtuelle Netzwerke in Speicherkonten einschränken, lassen Sie [vertrauenswürdige Microsoft-Dienste](https://docs.microsoft.com/azure/storage/common/storage-network-security#exceptions) zu.
Allgemeine V2-Speicherkonten (heiße und kalte Ebene) | Ja | Die Transaktionskosten steigen gegenüber allgemeinen V1-Speicherkonten erheblich an.

>[!IMPORTANT]
> Um Leistungsprobleme zu vermeiden, stellen Sie sicher, dass Sie die Skalierbarkeits- und Leistungsziele für VM-Datenträger für virtuelle [Linux](../virtual-machines/linux/disk-scalability-targets.md)- oder [Windows](../virtual-machines/windows/disk-scalability-targets.md)-Computer beachten. Wenn Sie die Standardeinstellungen verwenden, erstellt Site Recovery die erforderlichen Datenträger und Speicherkonten auf Basis der Quellkonfiguration. Wenn Sie Ihre eigenen Einstellungen anpassen und verwenden möchten, halten Sie die Skalierbarkeits- und Leistungsziele für Datenträger für Ihre virtuellen Quellcomputer ein.

## <a name="limits-and-data-change-rates"></a>Grenzwerte und Datenänderungsraten

Die folgende Tabelle enthält die Site Recovery-Grenzwerte.

- Diese Grenzwerte basieren auf unseren Tests, können aber begreiflicherweise nicht alle möglichen E/A-Kombinationen für Anwendungen abdecken.
- Die tatsächlichen Ergebnisse können je nach Ihrer E/A-Mischung für die App variieren.
- Zwei Grenzwerte müssen beachtet werden, nämlich jene für Datenänderungen bei Datenträgern und jene für Datenänderungen bei virtuellen Computern.
- Beispiel: Bei Verwendung eines in der folgenden Tabelle beschriebenen Premium P20-Datenträgers kann Site Recovery Datenänderungen von 5MB/s pro Datenträger mit maximal fünf solcher Datenträger pro VM verarbeiten, weil der Grenzwert für die Gesamtänderungsrate bei 25MB/s pro VM liegt.

**Speicherziel** | **Durchschnittliche E/A-Größe des Quelldatenträgers** |**Durchschnittliche Datenänderungsrate des Quelldatenträgers** | **Gesamte Datenänderungsrate des Quelldatenträgers pro Tag**
---|---|---|---
Standardspeicher | 8 KB | 2 MB/s | 168 GB pro Datenträger
Premium-Datenträger – P10 oder P15 | 8 KB  | 2 MB/s | 168 GB pro Datenträger
Premium-Datenträger – P10 oder P15 | 16 KB | 4 MB/s |  336 GB pro Datenträger
Premium-Datenträger – P10 oder P15 | 32 KB oder höher | 8 MB/s | 672 GB pro Datenträger
Premium-Datenträger – P20, P30, P40 oder P50 | 8 KB    | 5 MB/s | 421 GB pro Datenträger
Premium-Datenträger – P20, P30, P40 oder P50 | 16 KB oder höher |20 MB/s | 1\.684 GB pro Datenträger

## <a name="replicated-machines---networking"></a>Replizierte Computer – Netzwerk
**Einstellung** | **Unterstützung** | **Details**
--- | --- | ---
NIC | Unterstützte maximale Anzahl für eine bestimmte Azure-VM-Größe | Netzwerkkarten werden erstellt, wenn die VM während des Failovers erstellt wird.<br/><br/> Die Anzahl von Netzwerkkarten auf dem virtuellen Failovercomputer ist abhängig von der Anzahl von Netzwerkkarten, die auf dem virtuellen Quellcomputer vorhanden waren, als die Replikation aktiviert wurde. Falls Sie eine Netzwerkkarte nach dem Aktivieren der Replikation hinzufügen oder entfernen, wirkt sich dies nicht auf die Anzahl von Netzwerkkarten auf der replizierten VM nach dem Failover aus. Beachten Sie außerdem, dass nach einem Failover die Reihenfolge der Netzwerkadapter unter Umständen nicht mehr der ursprünglichen Reihenfolge entspricht.
Internetlastenausgleich | Unterstützt | Ordnen Sie den vorkonfigurierten Lastenausgleich mit einem Azure-Automatisierungsskript in einem Wiederherstellungsplan zu.
Interner Lastenausgleich | Unterstützt | Ordnen Sie den vorkonfigurierten Lastenausgleich mit einem Azure-Automatisierungsskript in einem Wiederherstellungsplan zu.
Öffentliche IP-Adresse | Unterstützt | Ordnen Sie der Netzwerkkarte eine vorhandene öffentliche IP-Adresse zu. Oder erstellen Sie eine öffentliche IP-Adresse, und ordnen Sie diese der Netzwerkkarte zu, indem Sie ein Azure-Automatisierungsskript in einem Wiederherstellungsplan verwenden.
NSG auf Netzwerkkarte | Unterstützt | Ordnen Sie die NSG der Netzwerkkarte zu, indem Sie ein Azure-Automatisierungsskript in einem Wiederherstellungsplan verwenden.
NSG im Subnetz | Unterstützt | Ordnen Sie die NSG dem Subnetz zu, indem Sie ein Azure-Automatisierungsskript in einem Wiederherstellungsplan verwenden.
Reservierte (statische) IP-Adresse | Unterstützt | Wenn die Netzwerkkarte auf dem virtuellen Quellcomputer über eine statische IP-Adresse verfügt und für das Zielsubnetz die gleiche IP-Adresse verfügbar ist, wird sie der VM zugewiesen, für die das Failover ausgeführt wurde.<br/><br/> Falls das Zielsubnetz nicht über die gleiche IP-Adresse verfügt, wird eine der verfügbaren IP-Adressen im Subnetz für den virtuellen Computer reserviert.<br/><br/> Sie können eine feste IP-Adresse und ein Subnetz auch unter **Replizierte Elemente** > **Einstellungen** > **Compute und Netzwerk** > **Netzwerkschnittstellen** angeben.
Dynamische IP-Adresse | Unterstützt | Wenn die Netzwerkkarte auf der Quelle über eine dynamische IP-Adressierung verfügt, ist die Netzwerkkarte auf dem virtuellen Computer, für den das Failover ausgeführt wurde, standardmäßig ebenfalls dynamisch.<br/><br/> Sie können dies in eine feste IP-Adresse ändern, falls es erforderlich ist.
Mehrere IP-Adressen | Nicht unterstützt | Wenn Sie ein Failover für einen virtuellen Computer mit einem Netzwerkadapter mit mehreren IP-Adressen ausführen, wird nur die primäre IP-Adresse des Netzwerkadapters in der Quellregion beibehalten. Wenn Sie mehrere IP-Adressen zuweisen möchten, können Sie virtuelle Computer zu einem [Wiederherstellungsplan](recovery-plan-overview.md) hinzufügen und ein Skript anfügen, um dem Plan zusätzliche IP-Adressen zuzuweisen. Alternativ können Sie die Änderung nach dem Failover manuell oder mithilfe eines Skripts vornehmen. 
Traffic Manager     | Unterstützt | Sie können Traffic Manager so vorkonfigurieren, dass Datenverkehr in regelmäßigen Abständen an den Endpunkt in der Quellregion und bei einem Failover an den Endpunkt in der Zielregion weitergeleitet wird.
Azure DNS | Unterstützt |
Benutzerdefinierter DNS  | Unterstützt |
Nicht authentifizierter Proxy | Unterstützt | [Weitere Informationen](site-recovery-azure-to-azure-networking-guidance.md)    
Authentifizierter Proxy | Nicht unterstützt | Wenn der virtuelle Computer einen authentifizierten Proxy für ausgehende Verbindungen verwendet, kann er nicht mit Azure Site Recovery repliziert werden.    
VPN-Site-to-Site-Verbindung mit lokalem Standort<br/><br/>(mit oder ohne ExpressRoute)| Unterstützt | Stellen Sie sicher, dass die UDRs und NSGs so konfiguriert sind, dass der Datenverkehr für die Sitewiederherstellung nicht lokal weitergeleitet wird. [Weitere Informationen](site-recovery-azure-to-azure-networking-guidance.md)    
VNet-zu-VNet-Verbindung | Unterstützt | [Weitere Informationen](site-recovery-azure-to-azure-networking-guidance.md)  
VNET-Dienstendpunkte | Unterstützt | Wenn Sie den Zugriff auf virtuelle Netzwerke in Speicherkonten einschränken, stellen Sie sicher, dass den vertrauenswürdigen Microsoft-Diensten Zugriff auf das Speicherkonto gewährt wird.
Beschleunigte Netzwerke | Unterstützt | Auf dem virtuellen Quellcomputer muss der beschleunigte Netzwerkbetrieb aktiviert sein. [Weitere Informationen](azure-vm-disaster-recovery-with-accelerated-networking.md)



## <a name="next-steps"></a>Nächste Schritte
- Lesen Sie [Netzwerkkonzepte für die Replikation zwischen Azure-Standorten](site-recovery-azure-to-azure-networking-guidance.md) zum Replizieren von virtuellen Azure-Computern.
- Stellen Sie die Notfallwiederherstellung bereit, indem Sie [Azure-VMs replizieren](site-recovery-azure-to-azure.md).
