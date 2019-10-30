---
title: Horizontale SAP HANA-Skalierung mit Standbyknoten auf Azure-VMs mit Azure NetApp Files auf SUSE Linux Enterprise Server | Microsoft-Dokumentation
description: Hochverfügbarkeitsleitfaden für SAP NetWeaver unter SUSE Linux Enterprise Server mit Azure NetApp Files für SAP-Anwendungen
services: virtual-machines-windows,virtual-network,storage
documentationcenter: saponazure
author: rdeltcheva
manager: juergent
editor: ''
tags: azure-resource-manager
keywords: ''
ms.assetid: 5e514964-c907-4324-b659-16dd825f6f87
ms.service: virtual-machines-windows
ms.topic: article
ms.tgt_pltfrm: vm-windows
ms.workload: infrastructure-services
ms.date: 04/30/2019
ms.author: radeltch
ms.openlocfilehash: 11b0746c3e9e137775b2466af776b4cd9ba1e5df
ms.sourcegitcommit: b050c7e5133badd131e46cab144dd5860ae8a98e
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 10/23/2019
ms.locfileid: "72791671"
---
# <a name="sap-hana-scale-out-with-standby-node-on-azure-vms-with-azure-netapp-files-on-suse-linux-enterprise-server"></a>Horizontale SAP HANA-Skalierung mit Standbyknoten auf Azure-VMs mit Azure NetApp Files auf SUSE Linux Enterprise Server 

[dbms-guide]:dbms-guide.md
[deployment-guide]:deployment-guide.md
[planning-guide]:planning-guide.md

[anf-azure-doc]:https://docs.microsoft.com/azure/azure-netapp-files/
[anf-avail-matrix]:https://azure.microsoft.com/global-infrastructure/services/?products=netapp&regions=all 
[anf-register]:https://docs.microsoft.com/azure/azure-netapp-files/azure-netapp-files-register
[anf-sap-applications-azure]:https://www.netapp.com/us/media/tr-4746.pdf

[2205917]: https://launchpad.support.sap.com/#/notes/2205917
[1944799]: https://launchpad.support.sap.com/#/notes/1944799
[1928533]: https://launchpad.support.sap.com/#/notes/1928533
[2015553]: https://launchpad.support.sap.com/#/notes/2015553
[2178632]: https://launchpad.support.sap.com/#/notes/2178632
[2191498]: https://launchpad.support.sap.com/#/notes/2191498
[2243692]: https://launchpad.support.sap.com/#/notes/2243692
[1984787]: https://launchpad.support.sap.com/#/notes/1984787
[1999351]: https://launchpad.support.sap.com/#/notes/1999351
[1410736]:https://launchpad.support.sap.com/#/notes/1410736
[1900823]: https://launchpad.support.sap.com/#/notes/1900823

[sap-swcenter]:https://support.sap.com/en/my-support/software-downloads.html

[suse-ha-guide]:https://www.suse.com/products/sles-for-sap/resource-library/sap-best-practices/
[suse-drbd-guide]:https://www.suse.com/documentation/sle-ha-12/singlehtml/book_sleha_techguides/book_sleha_techguides.html
[suse-ha-12sp3-relnotes]:https://www.suse.com/releasenotes/x86_64/SLE-HA/12-SP3/

[sap-hana-ha]:sap-hana-high-availability.md
[nfs-ha]:high-availability-guide-suse-nfs.md


Dieser Artikel beschreibt die Bereitstellung eines hochverfügbaren HANA-Systems in einer Konfiguration mit horizontaler Skalierung mit Standby auf virtuellen Azure-Computern mit [Azure NetApp Files](https://docs.microsoft.com/azure/azure-netapp-files/azure-netapp-files-introduction/) für die freigegebenen Speichervolumes.  
In den Beispielkonfigurationen, Installationsbefehlen usw. werden die HANA-Instanz **03** und HANA-System-ID **HN1** verwendet. Die Beispiele basieren auf HANA 2.0 SP4 und SUSE Linux Enterprise Server für SAP 12 SP4. 

Lesen Sie zuerst die folgenden SAP-Hinweise und -Dokumente:

* [Azure NetApp Files-Dokumentation][anf-azure-doc] 
* SAP-Hinweis [1928533] mit folgenden Informationen:  
  * Liste der Azure-VM-Größen, die für die Bereitstellung von SAP-Software unterstützt werden
  * Wichtige Kapazitätsinformationen für Größen von Azure-VMs
  * Unterstützte SAP-Software und Kombinationen aus Betriebssystem (OS) und Datenbank
  * Erforderliche SAP-Kernelversion für Windows und Linux in Microsoft Azure
* In SAP-Hinweis [2015553] sind die Voraussetzungen für Bereitstellungen von SAP-Software in Azure aufgeführt, die von SAP unterstützt werden.
* SAP-Hinweis [2205917] enthält empfohlene Betriebssystemeinstellungen für den SUSE Linux Enterprise Server for SAP Applications.
* SAP-Hinweis [1944799] enthält SAP-Richtlinien für SUSE Linux Enterprise Server für SAP-Anwendungen.
* SAP-Hinweis [2178632] enthält ausführliche Informationen zu allen Überwachungsmetriken, die für SAP in Azure gemeldet werden.
* SAP-Hinweis [2191498] enthält die erforderliche SAP Host Agent-Version für Linux in Azure.
* SAP-Hinweis [2243692] enthält Informationen zur SAP-Lizenzierung unter Linux in Azure.
* SAP-Hinweis [1984787] enthält allgemeine Informationen zu SUSE Linux Enterprise Server 12.
* SAP-Hinweis [1999351] enthält Informationen zur Problembehandlung für die Azure-Erweiterung zur verbesserten Überwachung für SAP.
* SAP-Hinweis [1900823] enthält Informationen zu SAP HANA-Speicheranforderungen.
* Das [WIKI der SAP-Community](https://wiki.scn.sap.com/wiki/display/HOME/SAPonLinuxNotes) enthält alle erforderlichen SAP-Hinweise für Linux.
* [Azure Virtual Machines – Planung und Implementierung für SAP unter Linux][planning-guide]
* [Bereitstellung von Azure Virtual Machines für SAP unter Linux][deployment-guide]
* [Azure Virtual Machines – DBMS-Bereitstellung für SAP unter Linux][dbms-guide]
* [Best Practices für SUSE SAP HA][suse-ha-guide] Die Leitfäden enthalten alle erforderlichen Informationen, um NetWeaver HA und SAP HANA System Replication vor Ort einzurichten. Verwenden Sie diese Leitfäden als allgemeine Basis. Sie bieten wesentlich mehr Informationen.
* [SUSE High Availability Extension 12 SP3 Release Notes][suse-ha-12sp3-relnotes] (Versionshinweise zur SUSE-Hochverfügbarkeitserweiterung 12 SP3, in englischer Sprache)
* [NetApp-SAP-Anwendungen in Microsoft Azure mithilfe von Azure NetApp Files][anf-sap-applications-azure]
* [SAP HANA auf den NetApp-Systemen mit NFS](https://www.netapp.com/us/media/tr-4435.pdf). Das Konfigurationshandbuch enthält Informationen zur Einrichtung von SAP HANA mit NFS, das von Azure NetApp Files bereitgestellt wird.


## <a name="overview"></a>Übersicht

Eine der Methoden zum Erreichen von HANA-Hochverfügbarkeit ist das automatische Hostfailover. Zum Konfigurieren des automatischen Hostfailovers wird dem HANA-System mindestens ein virtueller Computer hinzugefügt und als Standbyknoten konfiguriert. Wenn der aktive Knoten ausfällt, übernimmt automatisch ein Standbyknoten. In der dargestellten Konfiguration mit virtuellen Azure-Computern wird dies durch [NFS in Azure NetApp Files](https://docs.microsoft.com/azure/azure-netapp-files/azure-netapp-files-introduction/) erreicht.  

Der Standbyknoten muss auf alle Datenbankvolumes zugreifen können. Die HANA-Volumes müssen als NFSv4-Volumes bereitgestellt werden. Der verbesserte, auf Dateilease basierende Sperrmechanismus im NFSv4-Protokoll wird zum `I/O`-Fencing verwendet. 

> [!IMPORTANT]
> Zur Unterstützung der Konfiguration ist es zwingend erforderlich, die HANA-Daten- und Protokollvolumes als NFSv4.1-Volumes bereitzustellen und diese unter Verwendung des NFSv4.1-Protokolls einzubinden. Die Konfiguration zum automatischen Failover des HANA-Hosts mit dem Standbyknoten wird bei NFSv3 nicht unterstützt.

![Hochverfügbarkeit von SAP NetWeaver – Übersicht](./media/high-availability-guide-suse-anf/sap-hana-scale-out-standby-netapp-files-suse.png)

Entsprechend den SAP HANA-Netzwerkempfehlungen wurden innerhalb eines virtuellen Azure-Netzwerks drei Subnetze erstellt: für die Kommunikation mit dem Speichersystem, für die interne Kommunikation zwischen den HANA-Knoten und für die Clientkommunikation. Die Azure NetApp-Volumes befinden sich in einem separaten Subnetz, das an [Azure NetApp Files delegiert wurde](https://docs.microsoft.com/azure/azure-netapp-files/azure-netapp-files-delegate-subnet).  

Die Subnetze in dieser Beispielkonfiguration sind:  

  - `storage` 10.23.2.0/24  
  - `hana` 10.23.3.0/24  
  - `client` 10.23.0.0/24  
  - `anf` 10.23.1.0/26  

## <a name="setting-up-the-azure-netapp-files-infrastructure"></a>Einrichten der Infrastruktur für Azure NetApp Files 

Bevor Sie mit der Einrichtung der Azure NetApp Files-Infrastruktur beginnen, sollten Sie sich mit der [Azure NetApp Files-Dokumentation][anf-azure-doc] vertraut machen. Azure NetApp Files ist in verschiedenen [Azure-Regionen](https://azure.microsoft.com/global-infrastructure/services/?products=netapp) verfügbar. Überprüfen Sie, ob Azure NetApp Files in Ihrer ausgewählten Azure-Region angeboten wird.  

Unter dem folgenden Link sehen Sie die Verfügbarkeit von Azure NetApp Files nach Azure-Region: [Verfügbarkeit von Azure NetApp Files nach Azure-Region][anf-avail-matrix].  
Fordern Sie Onboarding für Azure NetApp Files an, bevor Sie Azure NetApp Files bereitstellen. Befolgen Sie dazu die [Registrierungsanweisungen für Azure NetApp Files][anf-register]. 

### <a name="deploy-azure-netapp-files-resources"></a>Bereitstellen von Azure NetApp Files-Ressourcen  

In den folgenden Schritten wird davon ausgegangen, dass Sie bereits [Azure Virtual Network](https://docs.microsoft.com/azure/virtual-network/virtual-networks-overview) bereitgestellt haben. Die Azure NetApp Files-Ressourcen und die virtuellen Computer, auf denen die Azure NetApp Files-Ressourcen eingebunden werden, müssen im gleichen virtuellen Azure-Netzwerk oder in mittels Peering verknüpften virtuellen Azure-Netzwerken bereitgestellt werden.  

1. Fordern Sie [Onboarding für Azure NetApp Files](https://docs.microsoft.com/azure/azure-netapp-files/azure-netapp-files-register) an, sofern noch nicht geschehen.  

2. Erstellen Sie das NetApp-Konto entsprechend den [Anweisungen zum Erstellen eines NetApp-Kontos](https://docs.microsoft.com/azure/azure-netapp-files/azure-netapp-files-create-netapp-account) in der ausgewählten Azure-Region.  

3. Richten Sie entsprechend den [Anweisungen zum Einrichten eines Azure NetApp Files-Kapazitätspools](https://docs.microsoft.com/azure/azure-netapp-files/azure-netapp-files-set-up-capacity-pool) einen Azure NetApp Files-Kapazitätspool ein.  
Die in diesem Artikel vorgestellte HANA-Architektur verwendet einen einzigen Azure NetApp Files-Kapazitätspool mit der Dienstebene „Ultra“. Für HANA-Workloads in Azure empfehlen wir die [Dienstebene](https://docs.microsoft.com/azure/azure-netapp-files/azure-netapp-files-service-levels) „Ultra“ oder „Premium“ für Azure NetApp Files.  

4. Delegieren Sie ein Subnetz für Azure NetApp Files, wie in den [Anweisungen zum Delegieren eines Subnetzes für Azure NetApp Files](https://docs.microsoft.com/azure/azure-netapp-files/azure-netapp-files-delegate-subnet) beschrieben.  

5. Stellen Sie Azure NetApp Files-Volumes entsprechend den [Anweisungen zum Erstellen eines Azure NetApp Files-Volumes](https://docs.microsoft.com/azure/azure-netapp-files/azure-netapp-files-create-volumes) bereit.  Stellen Sie sicher, dass Sie beim Bereitstellen der Volumes die Version **NFSv4.1** verwenden. Derzeit sind für den Zugriff auf NFSv4.1 zusätzliche Whitelists erforderlich. Stellen Sie die Volumes im festgelegten [Subnetz](https://docs.microsoft.com/rest/api/virtualnetwork/subnets) für Azure NetApp Files bereit. Denken Sie daran, dass sich die Azure NetApp Files-Ressourcen und die virtuellen Azure-Computer im gleichen virtuellen Azure-Netzwerk oder in mittels Peering verknüpften virtuellen Azure-Netzwerken befinden müssen. Beispielsweise sind „<b>HN1</b>-data-mnt00001“, „<b>HN1</b>-log-mnt00001“ usw. die Namen der Volumes, „nfs://10.23.1.5/<b>HN1</b>-data-mnt00001“, „nfs://10.23.1.4/<b>HN1</b>-log-mnt00001“ usw. sind die Dateipfade für die Azure NetApp Files-Volumes.  

   1. Volume „<b>HN1</b>-data-mnt00001 (nfs://10.23.1.5/<b>HN1</b>-data-mnt00001)“
   2. Volume „<b>HN1</b>-data-mnt00002 (nfs://10.23.1.6/<b>HN1</b>-data-mnt00002)“
   3. Volume „<b>HN1</b>-log-mnt00001 (nfs://10.23.1.4/<b>HN1</b>-log-mnt00001)“
   4. Volume „<b>HN1</b>-log-mnt00002 (nfs://10.23.1.6/<b>HN1</b>-log-mnt00002)“
   5. Volume „<b>HN1</b>-shared (nfs://10.23.1.4/<b>HN1</b>-shared)“
   
   In diesem Beispiel haben wir separate Azure NetApp Files für die einzelnen HANA-Daten- und -Protokollvolumes verwendet. Für eine kostenoptimierte Konfiguration in kleineren oder nicht produktiven Systemen können alle Daten- und Protokollbereitstellungen auf einem einzelnen Volume platziert werden.  

### <a name="important-considerations"></a>Wichtige Hinweise

Wenn Sie Azure NetApp Files für die Hochverfügbarkeitsarchitektur von SAP NetWeaver unter SUSE in Betracht ziehen, beziehen Sie die folgenden wichtigen Überlegungen mit ein:

- Die Mindestgröße eines Kapazitätspools beträgt 4 TiB.  
- Die Mindestvolumegröße ist 100 GiB.
- Azure NetApp Files und alle virtuellen Computer, auf denen Azure NetApp Files-Volumes eingebunden werden sollen, müssen sich im selben virtuellen Azure-Netzwerk oder in [über Peering gekoppelten virtuellen Netzwerken](https://docs.microsoft.com/azure/virtual-network/virtual-network-peering-overview) in derselben Region befinden.  
- Das ausgewählte virtuelle Netzwerk muss über ein an Azure NetApp Files delegiertes Subnetz verfügen.
- Der Durchsatz eines Azure NetApp-Volumes ist eine Funktion des Volumekontingents und der Dienstebene, wie in [Dienstebenen für Azure NetApp Files](https://docs.microsoft.com/azure/azure-netapp-files/azure-netapp-files-service-levels) beschrieben. Stellen Sie bei der Größenanpassung der HANA Azure NetApp-Volumes sicher, dass der sich ergebende Durchsatz die HANA-Systemanforderungen erfüllt.  
- Azure NetApp Files bietet [Exportrichtlinien](https://docs.microsoft.com/azure/azure-netapp-files/azure-netapp-files-configure-export-policy): Sie können die zulässigen Clients und den Zugriffstyp (Lesen und Schreiben, schreibgeschützt usw.) steuern. 
- Azure NetApp Files wertet derzeit noch keine Zonen aus. Das Azure NetApp Files-Feature wird bisher nicht in allen Verfügbarkeitszonen in einer Azure-Region bereitgestellt. Achten Sie auf mögliche Latenzauswirkungen in einigen Azure-Regionen.  
- Um eine niedrige Latenz zu erzielen, ist es wichtig, dass die virtuellen Computer in unmittelbarer Nähe des Azure NetApp-Speichers bereitgestellt werden. Bei SAP HANA-Workloads ist eine niedrige Latenz sehr wichtig. Arbeiten Sie mit Ihrem Microsoft-Vertreter zusammen, um sicherzustellen, dass die virtuellen Computer und die Azure NetApp Files-Volumes in unmittelbarer Nähe zueinander bereitgestellt werden.  
- Die Benutzer-ID für „<b>sid</b>adm“ und die Gruppen-ID für `sapsys` auf den virtuellen Computern müssen mit der Konfiguration in Azure NetApp Files übereinstimmen. 

> [!IMPORTANT]
> Bei SAP HANA-Workloads ist eine niedrige Latenz sehr wichtig. Arbeiten Sie mit Ihrem Microsoft-Vertreter zusammen, um sicherzustellen, dass die virtuellen Computer und die Azure NetApp Files-Volumes in unmittelbarer Nähe zueinander bereitgestellt werden.  

> [!IMPORTANT]
> Wenn die Benutzer-ID für „<b>sid</b>adm“ und die Gruppen-ID für `sapsys` auf den virtuellen Computern und in der Azure NetApp-Konfiguration nicht übereinstimmen, werden die Berechtigungen für Dateien auf Azure NetApp-Volumes, die auf virtuellen Computern bereitgestellt sind, als `nobody` angezeigt. Stellen Sie beim [Onboarding eines neuen Systems](https://forms.office.com/Pages/ResponsePage.aspx?id=v4j5cvGGr0GRqy180BHbRxjSlHBUxkJBjmARn57skvdUQlJaV0ZBOE1PUkhOVk40WjZZQVJXRzI2RC4u) in Azure NetApp Files sicher, dass Sie die richtige Benutzer-ID für „<b>sid</b>adm“ und die Gruppen-ID für `sapsys` angeben.

### <a name="sizing-for-hana-database-on-azure-netapp-files"></a>Größenanpassung für eine HANA-Datenbank in Azure NetApp Files

Der Durchsatz eines Azure NetApp-Volumes ist eine Funktion der Volumegröße und der Dienstebene, wie in [Dienstebenen für Azure NetApp Files](https://docs.microsoft.com/azure/azure-netapp-files/azure-netapp-files-service-levels) beschrieben. 

Beim Entwerfen der Infrastruktur für SAP in Azure müssen Sie einige Mindestanforderungen von SAP an den Speicher beachten, aus denen sich die Mindestdurchsatzeigenschaften ergeben:

- Lese-/Schreibaktivität mit 250 MB/s in „/hana/log“ bei einer E/A-Größe von 1 MB aktivieren  
- Leseaktivität mit mindestens 400 MB/s in „/hana/data“ für E/A-Größen von 16 MB und 64 MB aktivieren  
- Schreibaktivität mit mindestens 250 MB/s in „/hana/data“ für E/A-Größen von 16 MB und 64 MB aktivieren  

Die [Azure NetApp Files-Durchsatzlimits](https://docs.microsoft.com/azure/azure-netapp-files/azure-netapp-files-service-levels) pro 1 TiB an Volumekontingent lauten:
- Storage Premium-Tarif: 64 MIB/s  
- Storage Ultra-Tarif: 128 MIB/s  

Um die SAP-Mindestanforderungen für den Durchsatz für Daten und Protokolle und die Richtlinien für `/hana/shared` zu erfüllen, werden folgende Größen empfohlen:

| Volume | Size<br /> Storage Premium-Tarif | Size<br /> Storage Ultra-Tarif |
| --- | --- | --- |
| /hana/log/ | 4 TiB | 2 TiB |
| /hana/data | 6,3 TiB | 3,2 TiB |
| /hana/shared | Max. 512 GB, 1 x RAM pro 4 Workerknoten | Max. 512 GB, 1 x RAM pro 4 Workerknoten |

Die SAP HANA-Konfiguration für das in diesem Artikel vorgestellte Layout mit Azure NetApp Files-Tarif „Storage Ultra“ sieht wie folgt aus:

| Volume | Size<br /> Storage Ultra-Tarif |
| --- | --- |
| /hana/log/mnt00001 | 2 TiB |
| /hana/log/mnt00002 | 2 TiB |
| /hana/data/mnt00001 | 3,2 TiB |
| /hana/data/mnt00002 | 3,2 TiB |
| /hana/shared | 2 TiB |

> [!NOTE]
> Die hier angegebenen Empfehlungen für die Azure NetApp Files-Größenanpassung zielen darauf ab, die Mindestanforderungen zu erfüllen, die SAP an seine Infrastrukturanbieter stellt. In realen Kundenbereitstellungen und Workloadszenarien sind sie möglicherweise nicht ausreichend. Verwenden Sie diese Empfehlungen also als Ausgangspunkt, und nehmen Sie Anpassungen auf Grundlage der Anforderungen der spezifischen Workload vor.  

> [!TIP]
> Sie können die Größe der Azure NetApp Files-Volumes dynamisch anpassen, ohne die Bereitstellung der Volumes aufheben (`unmount`) oder die virtuellen Computer oder SAP HANA beenden zu müssen. Damit kann Ihre Anwendung sowohl den erwarteten als auch unvorhergesehenen Durchsatzanforderungen flexibel gerecht werden.

## <a name="deploy-linux-virtual-machines-via-azure-portal"></a>Bereitstellen von virtuellen Linux-Computern über das Azure-Portal

Zuerst müssen Sie die Azure NetApp Files-Volumes erstellen. Erstellen Sie im [Azure Virtual Network](https://docs.microsoft.com/azure/virtual-network/virtual-networks-overview) die [Azure Virtual Network-Subnetze](https://docs.microsoft.com/azure/virtual-network/virtual-network-manage-subnet). Stellen Sie die virtuellen Computer bereit. Erstellen Sie die zusätzlichen Netzwerkschnittstellen, und fügen Sie diese an die entsprechenden virtuellen Computer an. Jeder virtuelle Computer verfügt über drei Netzwerkschnittstellen, die den drei Azure Virtual Network-Subnetzen (`storage`, `hana` und `client`) entsprechen.  Weitere Informationen finden Sie in [Erstellen eines virtuellen Linux-Computers in Azure mit mehreren Netzwerkschnittstellenkarten](https://docs.microsoft.com/azure/virtual-machines/linux/multiple-nics).  

> [!IMPORTANT]
> Bei SAP HANA-Workloads ist eine niedrige Latenz sehr wichtig. Arbeiten Sie mit Ihrem Microsoft-Vertreter zusammen, um sicherzustellen, dass die virtuellen Computer und die Azure NetApp Files-Volumes in unmittelbarer Nähe zueinander bereitgestellt werden, um eine niedrige Latenz zu erzielen. Übermitteln Sie beim [Onboarding eines neuen SAP HANA-Systems](https://forms.office.com/Pages/ResponsePage.aspx?id=v4j5cvGGr0GRqy180BHbRxjSlHBUxkJBjmARn57skvdUQlJaV0ZBOE1PUkhOVk40WjZZQVJXRzI2RC4u), das SAP HANA Azure NetApp Files verwendet, die erforderlichen Informationen.  
> 
In den nächsten Schritten wird davon ausgegangen, dass Sie bereits die Ressourcengruppe, das Azure Virtual Network und die drei Azure Virtual Network-Subnetze erstellt haben: `storage`, `hana` und `client`.  Wählen Sie beim Bereitstellen der virtuellen Computer das Speichersubnetz aus, sodass die Speichernetzwerk-Schnittstelle die primäre Schnittstelle auf den virtuellen Computern ist. Ist dies nicht möglich, muss eine explizite Route zum delegierten Subnetz von Azure NetApp über das Gateway des Speichersubnetzes konfiguriert werden. 

> [!IMPORTANT]
> Stellen Sie sicher, dass das von Ihnen ausgewählte Betriebssystem SAP-zertifiziert ist für SAP HANA auf den spezifischen VM-Typen, die Sie verwenden. Die Liste der SAP HANA-zertifizierten VM-Typen und BS-Releases für diese kann unter [Zertifizierte SAP HANA-IaaS-Plattformen](https://www.sap.com/dmc/exp/2014-09-02-hana-hardware/enEN/iaas.html#categories=Microsoft%20Azure) nachgeschlagen werden. Stellen Sie sicher, dass Sie in die Details des jeweils aufgeführten VM-Typs klicken, um die vollständige Liste der von SAP HANA unterstützten BS-Releases für den spezifischen VM-Typ anzuzeigen.  

1. Erstellen Sie eine Verfügbarkeitsgruppe für SAP HANA. Stellen Sie sicher, dass Sie die maximale Updatedomäne einrichten.  

2. Erstellen Sie drei virtuelle Computer (**hanadb1**, **hanadb2**, **hanadb3**).  
   - Verwenden Sie ein SLES4SAP-Image im Azure-Katalog, das für SAP HANA unterstützt wird. In diesem Beispiel wurde das Image „SLES4SAP 12 SP4“ verwendet.  
   - Wählen Sie die zuvor für SAP HANA erstellte Verfügbarkeitsgruppe aus.  
   - Wählen Sie das Azure Virtual Network-Speichersubnetz aus. Wählen Sie [Beschleunigter Netzwerkbetrieb](https://docs.microsoft.com/azure/virtual-network/create-vm-accelerated-networking-cli) aus.  
Der Name der Netzwerkschnittstelle wird automatisch beim Bereitstellen der virtuellen Computer generiert. Die Netzwerkschnittstellen, die an das Azure Virtual Network-Speichersubnetz angefügt werden, bezeichnen wir als **hanadb1-storage**, **hanadb2-storage** und **hanadb3-storage**. 
3. Erstellen Sie für das Subnetz des virtuellen `hana`-Netzwerks drei Netzwerkschnittstellen – eine für jeden virtuellen Computer. In diesem Beispiel: **hanadb1-hana**, **hanadb2-hana** und **hanadb3-hana**.  
4. Erstellen Sie für das Subnetz des virtuellen Netzwerks **client** drei Netzwerkschnittstellen – eine für jeden virtuellen Computer. In diesem Beispiel: **hanadb1-client**, **hanadb2-client**, and **hanadb3-client**.  
5. Fügen Sie die neu erstellten virtuellen Netzwerkschnittstellen an die entsprechenden virtuellen Computer an.  

    1. Wechseln Sie im [Azure-Portal](https://portal.azure.com/#home) zum virtuellen Computer.  
    2. Wählen Sie im linken Navigationsbereich „Virtuelle Computer“ aus. Filtern Sie nach dem Namen des virtuellen Computers, z. B. **hanadb1**. Klicken Sie auf den virtuellen Computer.  
    3. Wenn Sie sich in der Übersicht befinden, klicken Sie auf „Beenden“, um die Zuordnung des virtuellen Computers aufzuheben.  
    4. Wählen Sie „Netzwerk“ und dann „Netzwerkschnittstelle anfügen“ aus. Wählen Sie in der Dropdownliste unter „Netzwerkschnittstelle anfügen“ die bereits erstellten Netzwerkschnittstellen für die Subnetze **`hana`** und **client** aus.  Speichern Sie. 
    5. Wiederholen Sie diesen Vorgang für die verbleibenden virtuellen Computer. In unserem Beispiel **hanadb2** und **hanadb3**.
    6. Belassen Sie die virtuelle Computer für den Moment im Status „Beendet“. Als Nächstes aktivieren wir den [beschleunigten Netzwerkbetrieb](https://docs.microsoft.com/azure/virtual-network/create-vm-accelerated-networking-cli) für alle neu angefügten Netzwerkschnittstellen.  

6. Aktivieren Sie den beschleunigten Netzwerkbetrieb für die zusätzlichen Netzwerkschnittstellen für die Subnetze **`hana`** und **`client`** .  

    1. Öffnen Sie [Cloud Shell](https://azure.microsoft.com/features/cloud-shell/) im [Azure-Portal](https://portal.azure.com/#home).  
    2. Führen Sie die folgenden Befehle aus, um den beschleunigten Netzwerkbetrieb für die zusätzlichen Netzwerkschnittstellen zu aktivieren, die an die Subnetze **`hana`** und **`client`** angefügt wurden.  

    <pre><code>
    az network nic update --id /subscriptions/<b>your subscription</b>/resourceGroups/<b>your resource group</b>/providers/Microsoft.Network/networkInterfaces/<b>hanadb1-hana</b> --accelerated-networking true
    az network nic update --id /subscriptions/<b>your subscription</b>/resourceGroups/<b>your resource group</b>/providers/Microsoft.Network/networkInterfaces/<b>hanadb2-hana</b> --accelerated-networking true
    az network nic update --id /subscriptions/<b>your subscription</b>/resourceGroups/<b>your resource group</b>/providers/Microsoft.Network/networkInterfaces/<b>hanadb3-hana</b> --accelerated-networking true
    
    az network nic update --id /subscriptions/<b>your subscription</b>/resourceGroups/<b>your resource group</b>/providers/Microsoft.Network/networkInterfaces/<b>hanadb1-client</b> --accelerated-networking true
    az network nic update --id /subscriptions/<b>your subscription</b>/resourceGroups/<b>your resource group</b>/providers/Microsoft.Network/networkInterfaces/<b>hanadb2-client</b> --accelerated-networking true
    az network nic update --id /subscriptions/<b>your subscription</b>/resourceGroups/<b>your resource group</b>/providers/Microsoft.Network/networkInterfaces/<b>hanadb3-client</b> --accelerated-networking true
    </code></pre>
7. Starten Sie die virtuellen Computer.  

    1. Wählen Sie im linken Navigationsbereich „Virtuelle Computer“ aus. Filtern Sie nach dem Namen des virtuellen Computers, z. B. **hanadb1**. Klicken Sie auf den virtuellen Computer.  
    2. Da Sie sich in der Übersicht befinden, wählen Sie „Starten“ aus.  

## <a name="operating-system-configuration-and-preparation"></a>Konfiguration und Vorbereitung des Betriebssystems

Die folgenden Elemente sind mit einem der folgenden Präfixe versehen: **[A]** – gilt für alle Knoten, **[1]** – gilt nur für Knoten 1, **[2]** – gilt nur für Knoten 2 oder **[3]** – gilt nur für Knoten 3.

1. **[A]** Verwalten Sie die Hostdateien auf den virtuellen Computern. Schließen Sie Einträge für alle Subnetze ein. Die folgenden Einträge wurden für dieses Beispiel zu `/etc/hosts` hinzugefügt.  
    <pre><code>
    # Storage
    10.23.2.4   hanadb1
    10.23.2.5   hanadb2
    10.23.2.6   hanadb3
    # Client
    10.23.0.5   hanadb1-client
    10.23.0.6   hanadb2-client
    10.23.0.7   hanadb3-client
    # Hana
    10.23.3.4   hanadb1-hana
    10.23.3.5   hanadb2-hana
    10.23.3.6   hanadb3-hana
    </code></pre>

2. **[A]** Ändern Sie die DHCP- und Cloudkonfigurationseinstellungen, um unbeabsichtigte Änderungen des Hostnamens zu vermeiden.  
    <pre><code>
    vi /etc/sysconfig/network/dhcp
    #Change the following DHCP setting to "no"
    DHCLIENT_SET_HOSTNAME="no"
    vi /etc/sysconfig/network/ifcfg-eth0
    # Edit ifcfg-eth0 
    #Change CLOUD_NETCONFIG_MANAGE='yes' to "no"
    CLOUD_NETCONFIG_MANAGE='no'
    </code></pre>

3. **[A]** Bereiten Sie das Betriebssystem für den Betrieb von SAP HANA auf NetApp-Systemen mit NFS vor, wie im [SAP HANA on NetApp AFF Systems with NFS Configuration Guide](https://www.netapp.com/us/media/tr-4435.pdf) (Konfigurationshandbuch für SAP HANA auf NetApp AFF-Systemen mit NFS) beschrieben. Erstellen Sie die Konfigurationsdatei für die NetApp-Konfigurationseinstellungen: `/etc/sysctl.d/netapp-hana.conf`.  

    <pre><code>
    vi /etc/sysctl.d/netapp-hana.conf
    # Add the following entries in the configuration file
    net.core.rmem_max = 16777216
    net.core.wmem_max = 16777216
    net.core.rmem_default = 16777216
    net.core.wmem_default = 16777216
    net.core.optmem_max = 16777216
    net.ipv4.tcp_rmem = 65536 16777216 16777216
    net.ipv4.tcp_wmem = 65536 16777216 16777216
    net.core.netdev_max_backlog = 300000
    net.ipv4.tcp_slow_start_after_idle=0
    net.ipv4.tcp_no_metrics_save = 1
    net.ipv4.tcp_moderate_rcvbuf = 1
    net.ipv4.tcp_window_scaling = 1
    net.ipv4.tcp_timestamps = 1
    net.ipv4.tcp_sack = 1
    </code></pre>

4. **[A]** Erstellen Sie eine Konfigurationsdatei mit Microsoft für Azure-Konfigurationseinstellungen: `/etc/sysctl.d/ms-az.conf`.  

    <pre><code>
    vi /etc/sysctl.d/ms-az.conf
    # Add the following entries in the configuration file
    ipv6.conf.all.disable_ipv6 = 1
    net.ipv4.tcp_max_syn_backlog = 16348
    net.ipv4.ip_local_port_range = 40000 65300
    net.ipv4.conf.all.rp_filter = 0
    sunrpc.tcp_slot_table_entries = 128
    vm.swappiness=10
    </code></pre>

4. **[A]** Passen Sie die sunrpc-Einstellungen entsprechend den Empfehlungen im [SAP HANA on NetApp AFF Systems with NFS Configuration Guide](https://www.netapp.com/us/media/tr-4435.pdf) (Konfigurationshandbuch für SAP HANA auf NetApp AFF-Systemen mit NFS) an.  
    <pre><code>
    vi /etc/modprobe.d/sunrpc.conf
    # Insert the following line
    options sunrpc tcp_max_slot_table_entries=128
    </code></pre>

## <a name="mount-the-azure-netapp-files-volumes"></a>Einbinden der Azure NetApp Files-Volumes

1. **[A]** Erstellen Sie Bereitstellungspunkte für die HANA-Datenbankvolumes.  
    <pre><code>
    mkdir -p /hana/data/<b>HN1</b>/mnt00001
    mkdir -p /hana/data/<b>HN1</b>/mnt00002
    mkdir -p /hana/log/<b>HN1</b>/mnt00001
    mkdir -p /hana/log/<b>HN1</b>/mnt00002
    mkdir -p /hana/shared
    mkdir -p /usr/sap/<b>HN1</b>
    </code></pre>

2. **[1]** Erstellen Sie knotenspezifische Verzeichnisse für `/usr/sap` auf **HN1**-shared.  

    <pre><code>
    # Create a temporary directory to mount  <b>HN1</b>-shared
    mkdir /mnt/tmp
    mount <b>10.23.1.4</b>:/<b>HN1</b>-shared /mnt/tmp
    cd /mnt/tmp
    mkdir shared usr-sap-<b>hanadb1</b> usr-sap-<b>hanadb2</b> usr-sap-<b>hanadb3</b>
    # unmount /hana/shared
    cd
    umount /mnt/tmp
    </code></pre>

3. **[A]** Überprüfen Sie die Einstellung für die NFS-Domäne. Stellen Sie sicher, dass die Domäne als **`localdomain`** konfiguriert und die Zuordnung auf **nobody** festgelegt ist.  
    <pre><code>
    sudo cat  /etc/idmapd.conf
    # Example
    [General]
    Verbosity = 0
    Pipefs-Directory = /var/lib/nfs/rpc_pipefs
    Domain = <b>localdomain</b>
    [Mapping]
    Nobody-User = <b>nobody</b>
    Nobody-Group = <b>nobody</b>
    </code></pre>

4. **[A]** Deaktivieren Sie die NFSv4-ID-Zuordnung. Führen Sie bei `nfs4_disable_idmapping` den Einbindungsbefehl aus, um die Verzeichnisstruktur zu erstellen.  Sie können das Verzeichnis unter“/sys/modules“ nicht manuell erstellen, da der Zugriff für den Kernel bzw. die Treiber reserviert ist.  

    <pre><code>
    
    mkdir /mnt/tmp
    mount 10.23.1.4:/HN1-shared /mnt/tmp
    umount  /mnt/tmp
    # Disable NFSv4 idmapping. 
    echo "N" > /sys/module/nfs/parameters/nfs4_disable_idmapping
    </code></pre>

5. **[A]** Erstellen Sie manuell die SAP HANA-Gruppe und den -Benutzer. Die IDs für die Gruppe „sapsys“ und den Benutzer „**hn1**adm“ müssen auf dieselben IDs festgelegt werden, die während des Onboardings bereitgestellt wurden. In diesem Beispiel werden die IDs auf **1001**festgelegt. Andernfalls kann nicht auf die Volumes zugegriffen werden.  Die IDs für die Gruppe „sapsys“ und die Benutzerkonten **„hn1**adm“ und „sapadm“ müssen auf allen virtuellen Computern identisch sein.  

    <pre><code>
    # Create user group 
    sudo groupadd -g 1001 sapsys
    # Create  users 
    sudo useradd <b>hn1</b>adm -u 1001 -g 1001 -d /usr/sap/<b>HN1</b>/home -c "SAP HANA Database System" -s /bin/sh
    sudo useradd sapadm -u 1002 -g 1001 -d /home/sapadm -c "SAP Local Administrator" -s /bin/sh
    # Set the password  for both user ids
    sudo passwd hn1adm
    sudo passwd sapadm
    </code></pre>

6. **[A]** Binden Sie die freigegebenen Azure NetApp Files-Volumes ein.  

    <pre><code>
    sudo vi /etc/fstab
    # Add the following entries
    10.23.1.5:/<b>HN1</b>-data-mnt00001 /hana/data/<b>HN1</b>/mnt00001  nfs   rw,vers=4,minorversion=1,hard,timeo=600,rsize=262144,wsize=262144,intr,noatime,lock,_netdev,sec=sys  0  0
    10.23.1.6:/<b>HN1</b>-data-mnt00002 /hana/data/<b>HN1</b>/mnt00002  nfs   rw,vers=4,minorversion=1,hard,timeo=600,rsize=262144,wsize=262144,intr,noatime,lock,_netdev,sec=sys  0  0
    10.23.1.4:/<b>HN1</b>-log-mnt00001 /hana/log/<b>HN1</b>/mnt00001  nfs   rw,vers=4,minorversion=1,hard,timeo=600,rsize=262144,wsize=262144,intr,noatime,lock,_netdev,sec=sys  0  0
    10.23.1.6:/<b>HN1</b>-log-mnt00002 /hana/log/HN1/mnt00002  nfs   rw,vers=4,minorversion=1,hard,timeo=600,rsize=262144,wsize=262144,intr,noatime,lock,_netdev,sec=sys  0  0
    10.23.1.4:/<b>HN1</b>-shared/shared /hana/shared  nfs   rw,vers=4,minorversion=1,hard,timeo=600,rsize=262144,wsize=262144,intr,noatime,lock,_netdev,sec=sys  0  0
    # Mount all volumes
    sudo mount -a 
    </code></pre>

7. **[1]** Binden Sie die knotenspezifischen Volumes auf **hanadb1** ein.  
    <pre><code>
    sudo vi /etc/fstab
    # Add the following entries
    10.23.1.4:/<b>HN1</b>-shared/usr-sap-<b>hanadb1</b> /usr/sap/<b>HN1</b>  nfs   rw,vers=4,minorversion=1,hard,timeo=600,rsize=262144,wsize=262144,intr,noatime,lock,_netdev,sec=sys  0  0
    # Mount the volume
    sudo mount -a 
    </code></pre>

8. **[2]** Binden Sie die knotenspezifischen Volumes auf **hanadb2** ein.  
    <pre><code>
    sudo vi /etc/fstab
    # Add the following entries
    10.23.1.4:/<b>HN1</b>-shared/usr-sap-<b>hanadb2</b> /usr/sap/<b>HN1</b>  nfs   rw,vers=4,minorversion=1,hard,timeo=600,rsize=262144,wsize=262144,intr,noatime,lock,_netdev,sec=sys  0  0
    # Mount the volume
    sudo mount -a 
    </code></pre>

9. **[3]** Binden Sie die knotenspezifischen Volumes auf **hanadb3** ein.  
    <pre><code>
    sudo vi /etc/fstab
    # Add the following entries
    10.23.1.4:/<b>HN1</b>-shared/usr-sap-<b>hanadb3</b> /usr/sap/<b>HN1</b>  nfs   rw,vers=4,minorversion=1,hard,timeo=600,rsize=262144,wsize=262144,intr,noatime,lock,_netdev,sec=sys  0  0
    # Mount the volume
    sudo mount -a 
    </code></pre>

10. **[A]** Überprüfen Sie, ob alle HANA-Volumes mit der NFS-Protokollversion **NFSv4** eingebunden wurden.  
    <pre><code>
    sudo nfsstat -m
    # Verify that flag vers is set to <b>4.1</b> 
    # Example from <b>hanadb1</b>
    /hana/data/<b>HN1</b>/mnt00001 from 10.23.1.5:/<b>HN1</b>-data-mnt00001
     Flags: rw,noatime,vers=<b>4.1</b>,rsize=262144,wsize=262144,namlen=255,hard,proto=tcp,timeo=600,retrans=2,sec=sys,clientaddr=10.23.2.4,local_lock=none,addr=10.23.1.5
    /hana/log/<b>HN1</b>/mnt00002 from 10.23.1.6:/<b>HN1</b>-log-mnt00002
     Flags: rw,noatime,vers=<b>4.1</b>,rsize=262144,wsize=262144,namlen=255,hard,proto=tcp,timeo=600,retrans=2,sec=sys,clientaddr=10.23.2.4,local_lock=none,addr=10.23.1.6
    /hana/data/<b>HN1</b>/mnt00002 from 10.23.1.6:/<b>HN1</b>-data-mnt00002
     Flags: rw,noatime,vers=<b>4.1</b>,rsize=262144,wsize=262144,namlen=255,hard,proto=tcp,timeo=600,retrans=2,sec=sys,clientaddr=10.23.2.4,local_lock=none,addr=10.23.1.6
    /hana/log/<b>HN1</b>/mnt00001 from 10.23.1.4:/<b>HN1</b>-log-mnt00001
    Flags: rw,noatime,vers=<b>4.1</b>,rsize=262144,wsize=262144,namlen=255,hard,proto=tcp,timeo=600,retrans=2,sec=sys,clientaddr=10.23.2.4,local_lock=none,addr=10.23.1.4
    /usr/sap/<b>HN1</b> from 10.23.1.4:/<b>HN1</b>-shared/usr-sap-<b>hanadb1</b>
     Flags: rw,noatime,vers=<b>4.1</b>,rsize=262144,wsize=262144,namlen=255,hard,proto=tcp,timeo=600,retrans=2,sec=sys,clientaddr=10.23.2.4,local_lock=none,addr=10.23.1.4
    /hana/shared from 10.23.1.4:/<b>HN1</b>-shared/shared
     Flags: rw,noatime,vers=<b>4.1</b>,rsize=262144,wsize=262144,namlen=255,hard,proto=tcp,timeo=600,retrans=2,sec=sys,clientaddr=10.23.2.4,local_lock=none,addr=10.23.1.4
    </code></pre>

## <a name="installation"></a>Installation  

In diesem Beispiel für die Bereitstellung von SAP HANA in einer Konfiguration mit horizontaler Skalierung mit Standbyknoten mit Azure haben wir HANA 2.0 SP4 verwendet.  

### <a name="prepare-for-hana-installation"></a>Vorbereiten der HANA-Installation

1. **[A]** Legen Sie vor der HANA-Installation das Stammkennwort fest (nach Abschluss der Installation können Sie das Stammkennwort deaktivieren). Führen Sie den Befehl `passwd` als `root` aus.  

2. **[1]** Vergewissern Sie sich, dass Sie eine SSH-Verbindung mit **hanadb2** und **hanadb3** herstellen können, ohne zur Eingabe eines Kennworts aufgefordert zu werden.  
    <pre><code>
    ssh root@<b>hanadb2</b>
    ssh root@<b>hanadb3</b>
    </code></pre>

3. **[A]** Installieren Sie zusätzliche Pakete, die für HANA 2.0 SP4 erforderlich sind. Weitere Informationen finden Sie in SAP-Hinweis [2593824](https://launchpad.support.sap.com/#/notes/2593824). 
    <pre><code>
    sudo zypper install libgcc_s1 libstdc++6 libatomic1 
    </code></pre>

4. **[2, 3]** Übertragen Sie den Besitz der SAP HANA-Verzeichnisse `data` und `log` an „**hn1**adm“.   
    <pre><code>
    # Execute as root
    sudo chown hn1adm:sapsys /hana/data/<b>HN1</b>
    sudo chown hn1adm:sapsys /hana/log/<b>HN1</b>
    </code></pre>

### <a name="hana-installation"></a>HANA-Installation

1. **[1]** Installieren Sie SAP HANA, und befolgen Sie dabei die Anweisungen unter [SAP HANA 2.0 Installation and Update Guide](https://help.sap.com/viewer/2c1988d620e04368aa4103bf26f17727/2.0.04/en-US/7eb0167eb35e4e2885415205b8383584.html) (Installations- und Updateleitfaden für SAP HANA 2.0). In diesem Beispiel installieren wir eine SAP HANA-Konfiguration mit horizontaler Skalierung mit einem Master-, einem Worker- und einem Standbyknoten.  
   Starten Sie das Programm **hdblcm** über das HANA-Installationssoftwareverzeichnis. Verwenden Sie den `internal_network`-Parameter, und übergeben Sie den Adressraum für das Subnetz, das für die interne Kommunikation zwischen HANA-Knoten verwendet wird.  

    <pre><code>./hdblcm --internal_network=10.23.3.0/24
    </code></pre>

   Geben Sie an der Eingabeaufforderung folgende Werte ein.

     * Aktion auswählen:  Geben Sie **1** ein (für die Installation).
     * Wählen Sie weitere Komponenten für die Installation aus: Geben Sie **2, 3** ein.
     * Geben Sie den Installationspfad ein:  Drücken Sie die EINGABETASTE (Standardwert „/hana/shared“).
     * Geben Sie den Namen des lokalen Hosts ein: Drücken Sie die EINGABETASTE, um die Standardeinstellung zu übernehmen.
     * Möchten Sie dem System Hosts hinzufügen? Geben Sie **y** ein.
     * Geben Sie die Hostnamen, die Sie hinzufügen möchten, durch Kommas getrennt ein: **hanadb2, hanadb3**.
     * Geben Sie den Root-Benutzernamen [root] ein: Drücken Sie die EINGABETASTE, um die Standardeinstellung zu übernehmen.
     * Geben Sie das Root-Benutzerkennwort ein: Geben Sie das Root-Kennwort ein.
     * Wählen Sie Rollen für den Host „hanadb2“ aus: Geben Sie **1** ein (für den Worker).
     * Geben Sie die Hostfailovergruppe für den Host „hanadb2“ [Standard] ein:  Drücken Sie die EINGABETASTE, um die Standardeinstellung zu übernehmen.
     * Geben Sie die Nummer der Speicherpartition für den Host „hanadb2“ [<<assign automatically>>] ein: Drücken Sie die EINGABETASTE, um die Standardeinstellung zu übernehmen.
     * Geben Sie die Workergruppe für den Host „hanadb2“ [Standard] ein: Drücken Sie die EINGABETASTE, um die Standardeinstellung zu übernehmen.
     * Wählen Sie Rollen für den Host „hanadb3“ aus: Geben Sie **2** ein (für den Standbyknoten).
     * Geben Sie die Hostfailovergruppe für den Host „hanadb3“ [Standard] ein: Drücken Sie die EINGABETASTE, um die Standardeinstellung zu übernehmen.
     * Geben Sie die Workergruppe für den Host „hanadb3“ [Standard] ein: Drücken Sie die EINGABETASTE, um die Standardeinstellung zu übernehmen.
     * Geben Sie die SAP HANA-System-ID ein: Geben Sie **HN1** ein.
     * Geben Sie die Instanznummer [00] ein: Geben Sie **03** ein.
     * Geben Sie die lokale Workergruppe des Hosts [Standard] ein: Drücken Sie die EINGABETASTE, um die Standardeinstellung zu übernehmen.
     * Wählen Sie die Systemnutzung/Geben Sie den Index [4] ein: Geben Sie **4** ein (für benutzerdefiniert).
     * Geben Sie den Speicherort der Datenvolumes ein [/hana/data/HN1]:  Drücken Sie die EINGABETASTE, um die Standardeinstellung zu übernehmen.
     * Geben Sie den Speicherort der Protokollvolumes ein [/hana/log/HN1]: Drücken Sie die EINGABETASTE, um die Standardeinstellung zu übernehmen.
     * Möchten Sie die maximale Speicherbelegung beschränken? [n]: Geben Sie **n** ein.
     * Geben Sie den Zertifikathostnamen für Host „hanadb1“ [hanadb1] ein: Drücken Sie die EINGABETASTE, um die Standardeinstellung zu übernehmen.
     * Geben Sie den Zertifikathostnamen für Host „hanadb2“ [hanadb2] ein: Drücken Sie die EINGABETASTE, um die Standardeinstellung zu übernehmen.
     * Geben Sie den Zertifikathostnamen für Host „hanadb3“ [hanadb3] ein: Drücken Sie die EINGABETASTE, um die Standardeinstellung zu übernehmen.
     * Geben Sie das Kennwort für den Systemadministrator ein (hn1adm): Kennwort eingeben
     * Geben Sie das Kennwort für den Systemdatenbankbenutzer ein (SYSTEM): Geben Sie das Kennwort für das System ein.
     * Bestätigen Sie das Kennwort für den Systemdatenbankbenutzer (SYSTEM): Geben Sie das Kennwort für das System ein.
     * Soll das System nach dem Neustart des Computers neu starten? [n]: Geben Sie **n** ein. 
     * Möchten Sie fortfahren (y/n)? Überprüfen Sie die Zusammenfassung. Wenn alle Werte korrekt sind, geben Sie **y** ein.


2. **[1]** Überprüfen Sie die „global.ini“.  

   Zeigen Sie die „global. ini“ an, und stellen Sie sicher, dass die Konfiguration für die interne SAP HANA-Kommunikation zwischen Knoten eingerichtet ist. Überprüfen Sie den Abschnitt **communication**. Dieser sollte den Adressraum für das **`hana`** -Subnetz enthalten, und `listeninterface` sollte auf `.internal` festgelegt sein. Überprüfen Sie den Abschnitt **internal_hostname_resolution**. Er sollte die IP-Adressen für die virtuellen HANA-Computer enthalten, die zum **`hana`** -Subnetz gehören.  

   <pre><code>
    sudo cat /usr/sap/<b>HN1</b>/SYS/global/hdb/custom/config/global.ini
    # Example 
    #global.ini last modified 2019-09-10 00:12:45.192808 by hdbnameserve
    [communication]
    internal_network = <b>10.23.3/24</b>
    listeninterface = .internal
    [internal_hostname_resolution]
    <b>10.23.3.4</b> = <b>hanadb1</b>
    <b>10.23.3.5</b> = <b>hanadb2</b>
    <b>10.23.3.6</b> = <b>hanadb3</b>
   </code></pre>

3. **[1]** Fügen Sie die Hostzuordnung hinzu, um sicherzustellen, dass die Client-IP-Adressen für die Clientkommunikation verwendet werden. Fügen Sie Abschnitt `public_host_resolution` hinzu, und fügen Sie die entsprechenden IP-Adressen aus dem Clientsubnetz hinzu.  

   <pre><code>
    sudo vi /usr/sap/HN1/SYS/global/hdb/custom/config/global.ini
    #Add the section
    [public_hostname_resolution]
    map_<b>hanadb1</b> = <b>10.23.0.5</b>
    map_<b>hanadb2</b> = <b>10.23.0.6</b>
    map_<b>hanadb3</b> = <b>10.23.0.7</b>
   </code></pre>

4. **[1]** Starten Sie SAP HANA neu, um die Änderungen zu aktivieren.  
   <pre><code>
    sudo -u <b>hn1</b>adm /usr/sap/hostctrl/exe/sapcontrol -nr <b>03</b> -function StopSystem HDB
    sudo -u <b>hn1</b>adm /usr/sap/hostctrl/exe/sapcontrol -nr <b>03</b> -function StartSystem HDB
   </code></pre>

5. **[1]** Stellen Sie sicher, dass die Clientschnittstelle zur Kommunikation die IP-Adressen aus dem Subnetz **client** verwendet.  
   <pre><code>
    sudo -u hn1adm /usr/sap/HN1/HDB03/exe/hdbsql -u SYSTEM -p "<b>password</b>" -i 03 -d SYSTEMDB 'select * from SYS.M_HOST_INFORMATION'|grep net_publicname
    # Expected result
    "<b>hanadb3</b>","net_publicname","<b>10.23.0.7</b>"
    "<b>hanadb2</b>","net_publicname","<b>10.23.0.6</b>"
    "<b>hanadb1</b>","net_publicname","<b>10.23.0.5</b>"
   </code></pre>

   Weitere Informationen zum Überprüfen der Konfiguration finden Sie im SAP-Hinweis [2183363 – Konfiguration des internen SAP HANA-Netzwerks](https://launchpad.support.sap.com/#/notes/2183363).  

6. Um SAP HANA für den zugrunde liegenden Azure NetApp Files-Speicher zu optimieren, legen Sie die folgenden SAP HANA-Parameter fest:

   - `max_parallel_io_requests` **128**
   - `async_read_submit` **ein**
   - `async_write_submit_active` **ein**
   - `async_write_submit_blocks` **alle**

   Weitere Informationen finden Sie im [SAP HANA on NetApp AFF Systems with NFS Configuration Guide](https://www.netapp.com/us/media/tr-4435.pdf) (Konfigurationshandbuch für SAP HANA auf NetApp AFF-Systemen mit NFS). 

   Ab SAP HANA 2.0 können Sie die Parameter in `global.ini` festlegen. Weitere Informationen finden Sie im SAP-Hinweis [1999930](https://launchpad.support.sap.com/#/notes/1999930).  
   Für SAP HANA 1.0-Systeme, Versionen bis SPS12, können diese Parameter bei der Installation festgelegt werden, wie im SAP-Hinweis [2267798](https://launchpad.support.sap.com/#/notes/2267798) beschrieben.  

7. Die Größe des von Azure NetApp Files verwendeten Speichers ist auf 16 TB begrenzt. SAP HANA erkennt die Speicherbegrenzung nicht implizit und erstellt nicht automatisch eine neue Datendatei, wenn die Dateigröße von 16 TB erreicht ist. Wenn SAP HANA versucht, die Datei über 16 TB hinaus zu vergrößern, führt dies zu Fehlern und schließlich zum Absturz des Indexservers. 

   > [!IMPORTANT]
   > Um zu verhindern, dass SAP HANA versucht, Datendateien über die [16-TB-Begrenzung](https://docs.microsoft.com/azure/azure-netapp-files/azure-netapp-files-resource-limits) des Speichersubsystems hinaus zu vergrößern, legen Sie die folgenden Parameter in `global.ini` fest.  
   > -  datavolume_striping=true
   > - datavolume_striping_size_gb = 15000. Weitere Informationen finden Sie im SAP-Hinweis [2400005](https://launchpad.support.sap.com/#/notes/2400005).
   > Beachten Sie den SAP-Hinweis [2631285](https://launchpad.support.sap.com/#/notes/2631285). 

## <a name="test-sap-hana-failover"></a>Testen des SAP HANA-Failovers 

1. Simulieren Sie den Knotenabsturz auf einem SAP HANA-Workerknoten.  

   Führen Sie die folgenden Befehle als „**hn1**adm“ aus, um den Status der Umgebung zu erfassen, bevor Sie den Knotenabsturz simulieren.  

   <pre><code>
    # Check the landscape status
    python /usr/sap/<b>HN1</b>/HDB<b>03</b>/exe/python_support/landscapeHostConfiguration.py
    | Host    | Host   | Host   | Failover | Remove | Storage   | Storage   | Failover | Failover | NameServer | NameServer | IndexServer | IndexServer | Host    | Host    | Worker  | Worker  |
    |         | Active | Status | Status   | Status | Config    | Actual    | Config   | Actual   | Config     | Actual     | Config      | Actual      | Config  | Actual  | Config  | Actual  |
    |         |        |        |          |        | Partition | Partition | Group    | Group    | Role       | Role       | Role        | Role        | Roles   | Roles   | Groups  | Groups  |
    | ------- | ------ | ------ | -------- | ------ | --------- | --------- | -------- | -------- | ---------- | ---------- | ----------- | ----------- | ------- | ------- | ------- | ------- |
    | hanadb1 | yes    | ok     |          |        |         1 |         1 | default  | default  | master 1   | master     | worker      | master      | worker  | worker  | default | default |
    | hanadb2 | yes    | ok     |          |        |         2 |         2 | default  | default  | master 2   | slave      | worker      | slave       | worker  | worker  | default | default |
    | hanadb3 | yes    | ignore |          |        |         0 |         0 | default  | default  | master 3   | slave      | standby     | standby     | standby | standby | default | -       |
    # Check the instance status
    sapcontrol -nr <b>03</b>  -function GetSystemInstanceList
    GetSystemInstanceList
    OK
    hostname, instanceNr, httpPort, httpsPort, startPriority, features, dispstatus
    hanadb2, 3, 50313, 50314, 0.3, HDB|HDB_WORKER, GREEN
    hanadb1, 3, 50313, 50314, 0.3, HDB|HDB_WORKER, GREEN
    hanadb3, 3, 50313, 50314, 0.3, HDB|HDB_STANDBY, GREEN
   </code></pre>

   Führen Sie den folgenden Befehl als „root“ auf dem Workerknoten aus (in diesem Fall **hanadb2**), um den Knotenabsturz zu simulieren.  
   <pre><code>
    echo b > /proc/sysrq-trigger
   </code></pre>

   Überwachen Sie das System, bis das Failover abgeschlossen ist. Erfassen Sie den Status nach Abschluss des Failovers – dieser sollte wie der Status unten aussehen.  
   <pre><code>
    # Check the instance status
    sapcontrol -nr <b>03</b>  -function GetSystemInstanceList
    GetSystemInstanceList
    OK
    hostname, instanceNr, httpPort, httpsPort, startPriority, features, dispstatus
    hanadb1, 3, 50313, 50314, 0.3, HDB|HDB_WORKER, GREEN
    hanadb3, 3, 50313, 50314, 0.3, HDB|HDB_STANDBY, GREEN
    hanadb2, 3, 50313, 50314, 0.3, HDB|HDB_WORKER, GRAY
    # Check the landscape status
    /usr/sap/HN1/HDB03/exe/python_support> python landscapeHostConfiguration.py
    | Host    | Host   | Host   | Failover | Remove | Storage   | Storage   | Failover | Failover | NameServer | NameServer | IndexServer | IndexServer | Host    | Host    | Worker  | Worker  |
    |         | Active | Status | Status   | Status | Config    | Actual    | Config   | Actual   | Config     | Actual     | Config      | Actual      | Config  | Actual  | Config  | Actual  |
    |         |        |        |          |        | Partition | Partition | Group    | Group    | Role       | Role       | Role        | Role        | Roles   | Roles   | Groups  | Groups  |
    | ------- | ------ | ------ | -------- | ------ | --------- | --------- | -------- | -------- | ---------- | ---------- | ----------- | ----------- | ------- | ------- | ------- | ------- |
    | hanadb1 | yes    | ok     |          |        |         1 |         1 | default  | default  | master 1   | master     | worker      | master      | worker  | worker  | default | default |
    | hanadb2 | no     | info   |          |        |         2 |         0 | default  | default  | master 2   | slave      | worker      | standby     | worker  | standby | default | -       |
    | hanadb3 | yes    | info   |          |        |         0 |         2 | default  | default  | master 3   | slave      | standby     | slave       | standby | worker  | default | default |
   </code></pre>

   > [!IMPORTANT]
   > Um Verzögerungen beim SAP HANA-Failover zu vermeiden, legen Sie bei einem Kernel Panic-Status eines Knotens `kernel.panic` auf **allen** virtuellen HANA-Computern auf 20 Sekunden fest. Die Konfiguration wird in `/etc/sysctl` ausgeführt. Starten Sie die virtuellen Computer neu, um die Änderung zu aktivieren. Wenn ein Knoten einen Kernel Panic-Status aufweist, kann ein Failover 10 Minuten oder länger dauern, wenn diese Änderung nicht vorgenommen wird.  

2. Beenden Sie den Namenserver.  
   Führen Sie die folgenden Befehle als „**hn1**adm“ aus, um den Status der Umgebung vor dem Test zu überprüfen:  

   <pre><code>
    #Landscape status 
    python /usr/sap/HN1/HDB03/exe/python_support/landscapeHostConfiguration.py
    | Host    | Host   | Host   | Failover | Remove | Storage   | Storage   | Failover | Failover | NameServer | NameServer | IndexServer | IndexServer | Host    | Host    | Worker | Worker  |
    |         | Active | Status | Status   | Status | Config    | Actual    | Config   | Actual   | Config     | Actual     | Config      | Actual      | Config  | Actual  | Config  | Actual  |
    |         |        |        |          |        | Partition | Partition | Group    | Group    | Role       | Role       | Role        | Role        | Roles   | Roles   | Groups  | Groups  |
    | ------- | ------ | ------ | -------- | ------ | --------- | --------- | -------- | -------- | ---------- | ---------- | ----------- | ----------- | ------- | ------- | ------- | ------- |
    | hanadb1 | yes    | ok     |          |        |         1 |         1 | default  | default  | master 1   | master     | worker      | master      | worker  | worker  | default | default |
    | hanadb2 | yes    | ok     |          |        |         2 |         2 | default  | default  | master 2   | slave      | worker      | slave       | worker  | worker  | default | default |
    | hanadb3 | no     | ignore |          |        |         0 |         0 | default  | default  | master 3   | slave      | standby     | standby     | standby | standby | default | -       |
    # Check the instance status
    sapcontrol -nr 03  -function GetSystemInstanceList
    GetSystemInstanceList
    OK
    hostname, instanceNr, httpPort, httpsPort, startPriority, features, dispstatus
    hanadb2, 3, 50313, 50314, 0.3, HDB|HDB_WORKER, GREEN
    hanadb1, 3, 50313, 50314, 0.3, HDB|HDB_WORKER, GREEN
    hanadb3, 3, 50313, 50314, 0.3, HDB|HDB_STANDBY, GRAY
   </code></pre>

   Führen Sie die folgenden Befehle als „**hn1**adm“ auf dem aktiven Masterknoten aus (in diesem Fall **hanadb1**).  

   <pre><code>
    hn1adm@hanadb1:/usr/sap/HN1/HDB03> HDB kill
   </code></pre>

   Der Standbyknoten **hanadb3** wird zum Masterknoten. Ressourcenstatus nach Abschluss des Failovertests:  

   <pre><code>
    # Check the instance status
    sapcontrol -nr 03 -function GetSystemInstanceList
    GetSystemInstanceList
    OK
    hostname, instanceNr, httpPort, httpsPort, startPriority, features, dispstatus
    hanadb2, 3, 50313, 50314, 0.3, HDB|HDB_WORKER, GREEN
    hanadb1, 3, 50313, 50314, 0.3, HDB|HDB_WORKER, GRAY
    hanadb3, 3, 50313, 50314, 0.3, HDB|HDB_STANDBY, GREEN
    # Check the landscape status
    python /usr/sap/HN1/HDB03/exe/python_support/landscapeHostConfiguration.py
    | Host    | Host   | Host   | Failover | Remove | Storage   | Storage   | Failover | Failover | NameServer | NameServer | IndexServer | IndexServer | Host    | Host    | Worker  | Worker  |
    |         | Active | Status | Status   | Status | Config    | Actual    | Config   | Actual   | Config     | Actual     | Config      | Actual      | Config  | Actual  | Config  | Actual  |
    |         |        |        |          |        | Partition | Partition | Group    | Group    | Role       | Role       | Role        | Role        | Roles   | Roles   | Groups  | Groups  |
    | ------- | ------ | ------ | -------- | ------ | --------- | --------- | -------- | -------- | ---------- | ---------- | ----------- | ----------- | ------- | ------- | ------- | ------- |
    | hanadb1 | no     | info   |          |        |         1 |         0 | default  | default  | master 1   | slave      | worker      | standby     | worker  | standby | default | -       |
    | hanadb2 | yes    | ok     |          |        |         2 |         2 | default  | default  | master 2   | slave      | worker      | slave       | worker  | worker  | default | default |
    | hanadb3 | yes    | info   |          |        |         0 |         1 | default  | default  | master 3   | master     | standby     | master      | standby | worker  | default | default |
   </code></pre>

   Starten Sie die HANA-Instanz erneut auf **hanadb1**, d. h. auf demselben virtuellen Computer, auf dem der Namenserver beendet wurde. Der Knoten **hanadb1** tritt wieder der Umgebung bei und behält seine Standbyrolle.  
   <pre><code>
    hn1adm@hanadb1:/usr/sap/HN1/HDB03> HDB start
   </code></pre>

   Es wird der folgende Status erwartet, nachdem SAP HANA auf **hanadb1** gestartet wurde:  
   <pre><code>
    # Check the instance status
    sapcontrol -nr 03 -function GetSystemInstanceList
    GetSystemInstanceList
    OK
    hostname, instanceNr, httpPort, httpsPort, startPriority, features, dispstatus
    hanadb1, 3, 50313, 50314, 0.3, HDB|HDB_WORKER, GREEN
    hanadb2, 3, 50313, 50314, 0.3, HDB|HDB_WORKER, GREEN
    hanadb3, 3, 50313, 50314, 0.3, HDB|HDB_STANDBY, GREEN
    # Check the landscape status
    python /usr/sap/HN1/HDB03/exe/python_support/landscapeHostConfiguration.py
    | Host    | Host   | Host   | Failover | Remove | Storage   | Storage   | Failover | Failover | NameServer | NameServer | IndexServer | IndexServer | Host    | Host    | Worker  | Worker  |
    |         | Active | Status | Status   | Status | Config    | Actual    | Config   | Actual   | Config     | Actual     | Config      | Actual      | Config  | Actual  | Config  | Actual  |
    |         |        |        |          |        | Partition | Partition | Group    | Group    | Role       | Role       | Role        | Role        | Roles   | Roles   | Groups  | Groups  |
    | ------- | ------ | ------ | -------- | ------ | --------- | --------- | -------- | -------- | ---------- | ---------- | ----------- | ----------- | ------- | ------- | ------- | ------- |
    | hanadb1 | yes    | info   |          |        |         1 |         0 | default  | default  | master 1   | slave      | worker      | standby     | worker  | standby | default | -       |
    | hanadb2 | yes    | ok     |          |        |         2 |         2 | default  | default  | master 2   | slave      | worker      | slave       | worker  | worker  | default | default |
    | hanadb3 | yes    | info   |          |        |         0 |         1 | default  | default  | master 3   | master     | standby     | master      | standby | worker  | default | default |
   </code></pre>

   Beenden Sie nun den Namenserver wieder auf dem aktuell aktiven Masterknoten, also auf „hanadb3“.  
   <pre><code>
    hn1adm@hanadb3:/usr/sap/HN1/HDB03> HDB kill
   </code></pre>

   Knoten **hanadb1** übernimmt wieder die Rolle des Masterknotens. Nachdem der Failovertest abgeschlossen ist, sieht der Status wie folgt aus:
   <pre><code>
    # Check the instance status
    sapcontrol -nr 03  -function GetSystemInstanceList & python /usr/sap/HN1/HDB03/exe/python_support/landscapeHostConfiguration.py
    GetSystemInstanceList
    OK
    hostname, instanceNr, httpPort, httpsPort, startPriority, features, dispstatus
    GetSystemInstanceList
    OK
    hostname, instanceNr, httpPort, httpsPort, startPriority, features, dispstatus
    hanadb1, 3, 50313, 50314, 0.3, HDB|HDB_WORKER, GREEN
    hanadb2, 3, 50313, 50314, 0.3, HDB|HDB_WORKER, GREEN
    hanadb3, 3, 50313, 50314, 0.3, HDB|HDB_STANDBY, GRAY
    # Check the landscape status
    python /usr/sap/HN1/HDB03/exe/python_support/landscapeHostConfiguration.py
    | Host    | Host   | Host   | Failover | Remove | Storage   | Storage   | Failover | Failover | NameServer | NameServer | IndexServer | IndexServer | Host    | Host    | Worker  | Worker  |
    |         | Active | Status | Status   | Status | Config    | Actual    | Config   | Actual   | Config     | Actual     | Config      | Actual      | Config  | Actual  | Config  | Actual  |
    |         |        |        |          |        | Partition | Partition | Group    | Group    | Role       | Role       | Role        | Role        | Roles   | Roles   | Groups  | Groups  |
    | ------- | ------ | ------ | -------- | ------ | --------- | --------- | -------- | -------- | ---------- | ---------- | ----------- | ----------- | ------- | ------- | ------- | ------- |
    | hanadb1 | yes    | ok     |          |        |         1 |         1 | default  | default  | master 1   | master     | worker      | master      | worker  | worker  | default | default |
    | hanadb2 | yes    | ok     |          |        |         2 |         2 | default  | default  | master 2   | slave      | worker      | slave       | worker  | worker  | default | default |
    | hanadb3 | no     | ignore |          |        |         0 |         0 | default  | default  | master 3   | slave      | standby     | standby     | standby | standby | default | -       |
   </code></pre>

   Starten Sie SAP HANA auf **hanadb3** – dieser Knoten kann jetzt als Standbyknoten verwendet werden.  
   <pre><code>
    hn1adm@hanadb3:/usr/sap/HN1/HDB03> HDB start
   </code></pre>

   Status, nachdem SAP HANA auf **hanadb3** gestartet wurde.  
   <pre><code>
    # Check the instance status
    sapcontrol -nr 03  -function GetSystemInstanceList & python /usr/sap/HN1/HDB03/exe/python_support/landscapeHostConfiguration.py
    GetSystemInstanceList
    OK
    hostname, instanceNr, httpPort, httpsPort, startPriority, features, dispstatus
    GetSystemInstanceList
    OK
    hostname, instanceNr, httpPort, httpsPort, startPriority, features, dispstatus
    hanadb1, 3, 50313, 50314, 0.3, HDB|HDB_WORKER, GREEN
    hanadb2, 3, 50313, 50314, 0.3, HDB|HDB_WORKER, GREEN
    hanadb3, 3, 50313, 50314, 0.3, HDB|HDB_STANDBY, GRAY
    # Check the landscape status
    python /usr/sap/HN1/HDB03/exe/python_support/landscapeHostConfiguration.py
    | Host    | Host   | Host   | Failover | Remove | Storage   | Storage   | Failover | Failover | NameServer | NameServer | IndexServer | IndexServer | Host    | Host    | Worker  | Worker  |
    |         | Active | Status | Status   | Status | Config    | Actual    | Config   | Actual   | Config     | Actual     | Config      | Actual      | Config  | Actual  | Config  | Actual  |
    |         |        |        |          |        | Partition | Partition | Group    | Group    | Role       | Role       | Role        | Role        | Roles   | Roles   | Groups  | Groups  |
    | ------- | ------ | ------ | -------- | ------ | --------- | --------- | -------- | -------- | ---------- | ---------- | ----------- | ----------- | ------- | ------- | ------- | ------- |
    | hanadb1 | yes    | ok     |          |        |         1 |         1 | default  | default  | master 1   | master     | worker      | master      | worker  | worker  | default | default |
    | hanadb2 | yes    | ok     |          |        |         2 |         2 | default  | default  | master 2   | slave      | worker      | slave       | worker  | worker  | default | default |
    | hanadb3 | no     | ignore |          |        |         0 |         0 | default  | default  | master 3   | slave      | standby     | standby     | standby | standby | default | -       |
   </code></pre>

## <a name="next-steps"></a>Nächste Schritte

* [Azure Virtual Machines – Planung und Implementierung für SAP][planning-guide]
* [Azure Virtual Machines – Bereitstellung für SAP][deployment-guide]
* [Azure Virtual Machines – DBMS-Bereitstellung für SAP][dbms-guide]
* Informationen zum Erreichen von Hochverfügbarkeit und zum Planen der Notfallwiederherstellung für SAP 
* HANA in Azure (große Instanzen) finden Sie unter [Hochverfügbarkeit und Notfallwiederherstellung für SAP HANA in Azure (große Instanzen)](hana-overview-high-availability-disaster-recovery.md).
* Informationen zur Erzielung von Hochverfügbarkeit und zur Planung der Notfallwiederherstellung für SAP HANA auf Azure-VMs finden Sie unter [Hochverfügbarkeit für SAP HANA auf Azure Virtual Machines (VMs)][sap-hana-ha].
