---
title: Azure Migrate-Matrix für die VMware-Bewertung und -Migration
description: Hier finden Sie eine Zusammenfassung der Supporteinstellungen und -einschränkungen für die Bewertung und Migration von VMware-VMs zu Azure mit dem Azure Migrate-Dienst.
services: backup
author: rayne-wiselman
manager: carmonm
ms.service: azure-migrate
ms.topic: conceptual
ms.date: 09/17/2019
ms.author: raynew
ms.openlocfilehash: 2a8a19dfd2cdc7a64a5ea90b96808963b19f73bb
ms.sourcegitcommit: c22327552d62f88aeaa321189f9b9a631525027c
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 11/04/2019
ms.locfileid: "73498651"
---
# <a name="support-matrix-for-vmware-assessment-and-migration"></a>Unterstützungsmatrix für die VMware-Bewertung und -Migration

Mit [Azure Migrate](migrate-overview.md) können Sie Computer bewerten und zur Microsoft Azure-Cloud migrieren. In diesem Artikel sind die Supporteinstellungen und -einschränkungen für die Bewertung und Migration lokaler VMware-VMs zusammengefasst.


## <a name="vmware-scenarios"></a>VMware-Szenarien

In der Tabelle sind die unterstützten Szenarien für VMware-VMs zusammengefasst.

**Bereitstellung** | **Details**
--- | ---
**Bewertung lokaler VMware-VMs** | [Einrichten](tutorial-prepare-vmware.md) Ihrer ersten Bewertung<br/><br/> [Ausführen](scale-vmware-assessment.md) einer umfangreichen Bewertung
**Migrieren von VMware-VMs zu Azure (ohne Agent)** | Sie können eine Migration ohne Agent durchführen oder eine Agent-basierte Migration verwenden. [Weitere Informationen](server-migrate-overview.md)


## <a name="azure-migrate-projects"></a>Azure Migrate-Projekte

**Unterstützung** | **Details**
--- | ---
**Azure-Berechtigungen** | Sie benötigen Berechtigungen für Mitwirkende oder Eigentümer im Abonnement, um ein Azure Migrate-Projekt zu erstellen.
**VMware-Einschränkungen**  | Bewerten Sie bis zu 35.000 VMware-VMs in einem einzigen Projekt. Sie können mehrere Projekte in einem Azure-Abonnement erstellen. Ein Projekt kann im Rahmen der Bewertungseinschränkungen sowohl VMware-VMs als auch Hyper-V-VMs umfassen.
**Geografie** | [Überprüfen](migrate-support-matrix.md#supported-geographies) Sie die unterstützten geografischen Regionen.

**Geografie** | **Speicherort der Metadaten**
--- | ---
Azure Government | US Government, Virginia
Asien-Pazifik | „Asien, Osten“ und „Asien, Südosten“
Australien | „Australien, Osten“ oder „Australien, Südosten“
Brasilien | Brasilien Süd
Kanada | „Kanada, Mitte“ oder „Kanada, Osten“
Europa | „Europa, Norden“ oder „Europa, Westen“
Frankreich | Frankreich, Mitte
Indien | „Indien, Mitte“ oder „Indien, Süden“
Japan |  „Japan, Osten“ oder „Japan, Westen“
Korea | „Südkorea, Mitte“ oder „Südkorea, Süden“
Vereinigtes Königreich | „Vereinigtes Königreich, Süden“ oder „Vereinigtes Königreich, Westen“
USA | „USA, Mitte“ oder „USA, Westen 2“


 > [!NOTE]
 > Der Support für Azure Government ist derzeit nur für die [ältere Version](https://docs.microsoft.com/azure/migrate/migrate-services-overview#azure-migrate-versions) von Azure Migrate verfügbar.


## <a name="application-discovery"></a>Anwendungsermittlung

Von der Azure Die Serverbewertung kann Apps, Rollen und Features ermitteln. Durch das Ermitteln Ihres App-Bestands können Sie einen maßgeschneiderten Migrationspfad für Ihre lokalen Workloads festlegen und planen. Von der Azure Die Serverbewertung ermöglicht die Ermittlung ohne Agent mithilfe von Gastanmeldeinformationen auf Computern, wobei mithilfe der WMI und von SSH-Aufrufen remote auf Computer zugegriffen wird.

**Unterstützung** | **Details**
--- | ---
Unterstützte Computer | Lokale VMware-VMs
Betriebssystem des Computers | Alle Windows- und Linux-Versionen
Anmeldeinformationen | Unterstützt derzeit die Verwendung von Anmeldeinformationen für alle Windows-Server sowie einen Satz Anmeldeinformationen für alle Linux-Server. Sie erstellen ein Gastbenutzerkonto für Windows-VMs und ein normales Benutzerkonto (ohne sudo-Zugriff) für alle Linux-VMs.
Computergrenzwerte für die App-Ermittlung | 10.000 pro Appliance 35.000 pro Projekt

## <a name="assessment-vcenter-server-requirements"></a>Anforderungen an vCenter Server für die Bewertung

In dieser Tabelle sind die Unterstützung und Einschränkungen der Bewertung für VMware-Virtualisierungsserver zusammengefasst.

**Unterstützung** | **Details**
--- | ---
**vCenter-Server** | VMware-VMs, die Sie bewerten möchten, müssen von mindestens einer vCenter Server-Instanz mit 5.5, 6.0, 6.5 oder 6.7 verwaltet werden.

## <a name="assessment-vcenter-server-permissions"></a>vCenter Server-Berechtigungen für die Bewertung

Für die Bewertung wird ein schreibgeschütztes Konto benötigt, um auf vCenter Server zugreifen zu können.

## <a name="assessment-appliance-requirements"></a>Anforderungen an die Appliances für die Bewertung

Azure Migrate führt eine einfache Appliance aus, die virtuelle VMware-Computer ermittelt und VM-Metadaten sowie Leistungsdaten an Azure Migrate sendet. Die Appliance für VMware wird über eine in vCenter Server importierte OVA-Vorlage bereitgestellt. Die folgende Tabelle fasst die Anforderungen an die Appliance zusammen.

**Unterstützung** | **Details**
--- | ---
**Bereitstellung einer Appliance** | Sie stellen die Appliance als VMware-VM bereit. Sie benötigen genügend Ressourcen auf dem vCenter-Server, um einen virtuellen Computer mit 32 GB RAM, 8 vCPUs, etwa 80 GB Datenspeicher und einem externen virtuellen Switch zuzuordnen.<br/><br/> Die Appliance erfordert Internetzugriff, entweder direkt oder über einen Proxy.<br/> Die Appliance-VM muss auf einem ESXi-Host mit Version 5.5 oder höher bereitgestellt werden.
**Azure Migrate-Projekt** | Eine Appliance kann einem einzelnen Projekt zugeordnet werden. <br/> Einem einzelnen Projekt kann eine beliebige Anzahl von Appliances zugeordnet werden.<br/> Sie können bis zu 35.000 VMs in einem Projekt bewerten.
**Ermittlung** | Eine Appliance kann bis zu 10.000 VMware-VMs in einer vCenter Server-Instanz ermitteln.<br/> Eine Appliance kann eine Verbindung mit einer einzelnen vCenter Server-Instanz herstellen.
**Bewertungsgruppe** | Sie können einer einzelnen Gruppe bis zu 35.000 Computer hinzufügen.
**Bewertung** | Sie können bis zu 35.000 virtuelle Computer in einem einzelnen Vorgang bewerten.


## <a name="assessment-url-access-requirements"></a>URL-Zugriffsanforderungen für die Bewertung

Das Azure Migrate-Gerät muss mit dem Internet verbunden sein.

- Bei der Bereitstellung der Appliance führt Azure Migrate eine Verbindungsüberwachung für die in der folgenden Tabelle zusammengefassten URLs durch.
- Wenn Sie einen URL-basierten Proxy für die Internetverbindung verwenden, lassen Sie den Zugriff auf diese URLs zu, und stellen Sie sicher, dass der Proxy alle CNAME-Einträge auflöst, die beim Durchsuchen der URLs empfangen werden.

**URL** | **Details**  
--- | --- |
*.portal.azure.com  | Navigieren zu Azure Migrate im Azure-Portal
*.windows.net <br/> *.msftauth.net <br/> *.msauth.net <br/> *.microsoft.com <br/> *.live.com | Melden Sie sich bei Ihrem Azure-Abonnement an.
*.microsoftonline.com <br/> *.microsoftonline-p.com | Erstellen von Active Directory-Anwendungen, damit die Appliance mit dem Azure Migrate-Service kommunizieren kann
management.azure.com | Erstellen von Active Directory-Anwendungen, damit die Appliance mit dem Azure Migrate-Service kommunizieren kann
dc.services.visualstudio.com | Laden Sie App-Protokolle hoch, die für die interne Überwachung verwendet werden.
*.vault.azure.net | Verwalten von Geheimnissen in Azure Key Vault
*.servicebus.windows.net | Kommunikation zwischen der Appliance und dem Azure Migrate-Dienst
*.discoverysrv.windowsazure.com <br/> *.migration.windowsazure.com <br/> *.hypervrecoverymanager.windowsazure.com | Herstellen einer Verbindung mit Azure Migrate-Dienst-URLs
*.blob.core.windows.net | Hochladen von Daten in Speicherkonten
http://aka.ms/latestapplianceservices<br/><br/> https://download.microsoft.com/download | Wird für Updates der Azure Migrate-Appliance verwendet.

## <a name="assessment-port-requirements"></a>Portanforderungen für die Bewertung

**Device** | **Connection**
--- | ---
Appliance | Eingehende Verbindungen an TCP-Port 3389, um Remotedesktopverbindungen mit der Appliance zu ermöglichen<br/><br/> Eingehende Verbindungen an Port 44368, um über Remotezugriff über die URL ```https://<appliance-ip-or-name>:44368``` auf die Applianceverwaltungs-App zugreifen zu können. <br/><br/>Ausgehende Verbindungen an Port 443, 5671 und 5672, um Ermittlungs- und Leistungsmetadaten an Azure Migrate zu senden
vCenter-Server | Eingehende Verbindungen an TCP-Port 443, damit die Appliance Konfigurations- und Leistungsmetadaten für Bewertungen sammeln kann <br/><br/> Die Appliance stellt standardmäßig über Port 443 eine Verbindung mit vCenter her. Wenn der vCenter-Server an einem anderen Port lauscht, können Sie den Port beim Einrichten der Ermittlung ändern.

## <a name="assessment-dependency-visualization"></a>Bewertung von Abhängigkeitsvisualisierungen

Mit der Abhängigkeitsvisualisierung können Sie Abhängigkeiten zwischen Computern visualisieren, die Sie bewerten und migrieren möchten. Sie verwenden die Abhängigkeitszuordnung normalerweise, wenn Sie auf Computer mit höheren Zuverlässigkeitsgraden zugreifen möchten. Für VMware-VMs wird die Abhängigkeitsvisualisierung wie folgt unterstützt:

- **Visualisierung von Abhängigkeiten ohne Agent:** Diese Option befindet sich zurzeit in der Vorschau. Sie erfordert keine Installation von Agents auf Computern.
    - Sie erfasst TCP-Verbindungsdaten von den Computern, für die sie aktiviert wurde. Nachdem die Abhängigkeitsermittlung gestartet wurde, sammelt die Appliance Daten von Computern in einem Abrufintervall von fünf Minuten.
    - Die folgenden Daten werden gesammelt:
        - TCP-Verbindungen
        - Namen von Prozessen mit aktiven Verbindungen
        - Namen der installierten Anwendungen, die die obigen Prozesse ausführen
        - Nein. von Verbindungen, die in jedem Abrufintervall erkannt werden
- **Agent-basierte Visualisierung von Abhängigkeiten:** Zur Verwendung der Agent-basierten Visualisierung von Abhängigkeiten müssen Sie die folgenden Agents auf alle lokalen Computer, die Sie analysieren möchten, herunterladen und dort installieren.
    - Der Microsoft Monitoring Agent (MMA) muss auf jedem Computer installiert werden. [Weitere Informationen](how-to-create-group-machine-dependencies.md#install-the-mma) zum Installieren des MMA-Agent.
    - Der Dependency-Agent muss auf jedem Computer installiert werden. [Weitere Informationen](how-to-create-group-machine-dependencies.md#install-the-dependency-agent) zum Installieren des Abhängigkeits-Agent.
    - Falls Sie über Computer ohne Internetverbindung verfügen, ist es außerdem erforderlich, auf diesen das Log Analytics-Gateway herunterzuladen und zu installieren.

## <a name="migration---limitations"></a>Migration – Einschränkungen
Sie können bis zu 10 VMs gleichzeitig für die Replikation auswählen. Wenn Sie weitere Computer migrieren möchten, replizieren Sie sie in Gruppen von 10. Bei der VMware-Migration ohne Agents können Sie bis zu 100 Replikationsvorgänge gleichzeitig ausführen.

## <a name="agentless-migration-vmware-server-requirements"></a>VMware-Serveranforderungen für die Migration ohne Agent

In dieser Tabelle sind die Unterstützung und Einschränkungen der Bewertung für VMware-Virtualisierungsserver zusammengefasst.

**Unterstützung** | **Details**
--- | ---
vCenter Server | Version 5.5, 6.0, 6.5 oder 6.7
VMware vSphere | Version 5.5, 6.0, 6.5 oder 6.7

## <a name="agentless-migration-vcenter-server-permissions"></a>vCenter Server-Berechtigungen für die Migration ohne Agent

**Berechtigungen** | **Details**
--- | ---
Datastore.Browse | Ermöglicht das Durchsuchen von VM-Protokolldateien zur Fehlerbehebung bei der Erstellung und Löschung von Momentaufnahmen.
Datastore.LowLevelFileOperations | Ermöglicht Lese-/Schreib-/Lösch-/Umbenennungsvorgänge im Datenspeicherbrowser, um Probleme bei der Erstellung und Löschung von Momentaufnahmen zu beheben.
VirtualMachine.Configuration.DiskChangeTracking | Ermöglicht die Aktivierung oder Deaktivierung der Änderungsverfolgung von VM-Datenträgern, um geänderte Datenblöcke zwischen Momentaufnahmen abzurufen.
VirtualMachine.Configuration.DiskLease | Ermöglicht Datenträger-Leasevorgänge für eine VM, damit der Datenträger mit dem VMware vSphere Virtual Disk Development Kit (VDDK) gelesen werden kann.
VirtualMachine.Provisioning.AllowReadOnlyDiskAccess | Ermöglicht das Öffnen eines Datenträgers auf einer VM, damit der Datenträger mit dem VDDK gelesen werden kann.
VirtualMachine.Provisioning.AllowVirtualMachineDownload  | Ermöglicht Lesevorgänge für Dateien, die einer VM zugeordnet sind, um die Protokolle herunterzuladen und ggf. Fehler zu beheben.
VirtualMachine.SnapshotManagement.* | Ermöglicht das Erstellen und Verwalten von VM-Momentaufnahmen zur Replikation.
Virtual Machine.Interaction.Power Off | Ermöglicht das Ausschalten der VM während der Migration zu Azure.


## <a name="agentless-migration-vmware-vm-requirements"></a>VMware-VM-Anforderungen für die Migration ohne Agent

**Unterstützung** | **Details**
--- | ---
**Unterstützte Betriebssysteme** | Von Azure unterstützte [Windows-](https://support.microsoft.com/help/2721672/microsoft-server-software-support-for-microsoft-azure-virtual-machines) und [Linux-](https://docs.microsoft.com/azure/virtual-machines/linux/endorsed-distros)Betriebssysteme können mithilfe der Migration ohne Agent migriert werden.
**Erforderliche Änderungen für Azure** | Einige VMs erfordern möglicherweise Änderungen, damit sie in Azure ausgeführt werden können. Bei den folgenden Betriebssystemen nimmt Azure Migrate diese Änderungen automatisch vor:<br/> - Red Hat Enterprise Linux 6.5+, 7.0+<br/> - CentOS 6.5+, 7.0+</br> - SUSE Linux Enterprise Server 12 SP1+<br/> - Ubuntu 14.04LTS, 16.04LTS, 18.04LTS<br/> – Debian 7, 8<br/><br/> Bei anderen Betriebssystemen müssen Sie vor der Migration manuell Anpassungen vornehmen. Die entsprechenden Artikel enthalten Anweisungen zur Vorgehensweise.
**Linux-Start** | Wenn sich „/boot“ in einer dedizierten Partition befindet, sollte diese auf dem Betriebssystemdatenträger und nicht auf mehrere Datenträger verteilt vorhanden sein.<br/> Wenn „/boot“ Teil der Stammpartition (/) ist, sollte sich diese auf dem Betriebssystemdatenträger befinden und sich nicht auf andere Datenträger erstrecken.
**UEFI-Start** | Die Migration von VMs mit UEFI-Start wird nicht unterstützt.
**Datenträgergröße** | 2 TB für den Betriebssystemdatenträger, 4 TB für Datenträger
**Einschränkungen für Datenträger** |  Bis zu 60 Datenträger pro virtuellem Computer
**Verschlüsselte Datenträger/Volumes** | Die Migration von VMs mit verschlüsselten Datenträgern/Volumes wird nicht unterstützt.
**Freigegebener Datenträgercluster** | Nicht unterstützt.
**Unabhängige Datenträger** | Nicht unterstützt.
**RDM-Datenträger/Pass-Through-Datenträger** | Wenn VMs über RDM- oder Pass-Through-Datenträger verfügen, werden diese nicht in Azure repliziert.
**NFS** | NFS-Volumes, die als Volumes auf den VMs bereitgestellt sind, werden nicht repliziert.
**iSCSI-Ziele** | Virtuelle Computer mit iSCSI-Zielen werden bei der Migration ohne Agent nicht unterstützt.
**Multipfad-E/A** | Nicht unterstützt.
**Storage vMotion** | Nicht unterstützt. Die Replikation funktioniert nicht, wenn auf einem virtuellen Computer Storage vMotion verwendet wird.
**Kombinierte Netzwerkadapter** | Nicht unterstützt.
**IPv6** | Nicht unterstützt.
**Zieldatenträger** | Virtuelle Computer können nur zu verwalteten Datenträgern (HDD Standard, SSD Premium) in Azure migriert werden.
**Gleichzeitige Replikation** | 100 virtuelle Computer pro vCenter Server. Bei mehr als 100 virtuellen Computern sind diese in Batches von 100 zu migrieren.


## <a name="agentless-migration-appliance-requirements"></a>Appliance-Anforderungen für die Migration ohne Agent


**Unterstützung** | **Details**
--- | ---
**ESXi** | Die Appliance-VM muss auf einem ESXi-Host mit Version 5.5 oder höher bereitgestellt werden.
**Azure Migrate-Projekt** | Eine Appliance kann einem einzelnen Projekt zugeordnet werden.
**vCenter Server** | Eine Appliance kann bis zu 10.000 VMware-VMs in einer vCenter Server-Instanz ermitteln.<br/> Eine Appliance kann eine Verbindung mit einer vCenter Server-Instanz herstellen.
**VDDK** | Wenn Sie eine Migration ohne Agent mit der Azure Migrate-Servermigration durchführen, muss das VMware vSphere VDDK auf der Appliance-VM installiert sein.

## <a name="agentless-migration-url-access-requirements"></a>URL-Zugriffsanforderungen für die Migration ohne Agent

Die Azure Migrate-Appliance muss mit dem Internet verbunden sein.

- Bei der Bereitstellung der Appliance führt Azure Migrate eine Verbindungsüberwachung für die in der folgenden Tabelle zusammengefassten URLs durch.
- Wenn Sie einen URL-basierten Proxy verwenden, lassen Sie den Zugriff auf diese URLs zu, und stellen Sie sicher, dass der Proxy alle CNAME-Einträge auflöst, die beim Durchsuchen der URLs empfangen werden.

**URL** | **Details**  
--- | ---
*.portal.azure.com | Navigieren zu Azure Migrate im Azure-Portal
*.windows.net <br/> *.msftauth.net <br/> *.msauth.net <br/> *.microsoft.com <br/> *.live.com  | Melden Sie sich bei Ihrem Azure-Abonnement an.
*.microsoftonline.com <br/> *.microsoftonline-p.com | Erstellen von Active Directory-Anwendungen, damit die Appliance mit dem Azure Migrate-Service kommunizieren kann
management.azure.com | Erstellen von Active Directory-Anwendungen, damit die Appliance mit dem Azure Migrate-Service kommunizieren kann
dc.services.visualstudio.com | Laden Sie App-Protokolle hoch, die für die interne Überwachung verwendet werden.
*.vault.azure.net | Verwalten von Geheimnissen in Azure Key Vault
*.servicebus.windows.net | Kommunikation zwischen der Appliance und dem Azure Migrate-Dienst
*.discoverysrv.windowsazure.com <br/> *.migration.windowsazure.com <br/> *.hypervrecoverymanager.windowsazure.com | Herstellen einer Verbindung mit Azure Migrate-Dienst-URLs
*.blob.core.windows.net | Hochladen von Daten in Speicherkonten
http://aka.ms/latestapplianceservices<br/><br/> https://download.microsoft.com/download | Wird für Updates der Azure Migrate-Appliance verwendet.


## <a name="agentless-migration-port-requirements"></a>Portanforderungen für die Migration ohne Agent

**Device** | **Connection**
--- | ---
Appliance | Ausgehende Verbindungen an Port 443 zum Hochladen von replizierten Daten in Azure und zur Kommunikation mit Azure Migrate-Diensten zum Orchestrieren der Replikation und der Migration.
vCenter-Server | Eingehende Verbindungen an Port 443 zum Orchestrieren der Replikation durch die Appliance – Erstellen von Momentaufnahmen, Kopieren von Daten, Freigeben von Momentaufnahmen.
vSphere/EXSI-Host | Eingehend an TCP-Port 902 zum Replizieren der Daten aus Momentaufnahmen durch die Appliance


## <a name="agent-based-migration-vmware-server-requirements"></a>VMware-Serveranforderungen für die Migration mit Agent

In dieser Tabelle sind die Unterstützung und Einschränkungen der Bewertung für VMware-Virtualisierungsserver zusammengefasst.

**Unterstützung** | **Details**
--- | ---
vCenter Server | Version 5.5, 6.0, 6.5 oder 6.7
VMware vSphere | Version 5.5, 6.0, 6.5 oder 6.7

### <a name="agent-based-migration-vcenter-server-permissions"></a>vCenter Server-Berechtigungen für die Migration mit Agent

Ein schreibgeschütztes Konto für vCenter Server.

## <a name="agent-based-migration-replication-appliance-requirements"></a>Anforderungen an die Replikationsappliance für die Migration mit Agent

Die Anforderungen an die [Replikationsappliance](migrate-replication-appliance.md) für die agentenbasierte Migration von VMware-VMs und physischen Servern mit Azure Migrate-Servermigration sind in der Tabelle unten zusammengefasst.

> [!NOTE]
> Wenn Sie die Replikationsappliance mit der OVA-Vorlage im Azure Migrate-Hub einrichten, wird die Appliance unter Windows Server 2016 ausgeführt und erfüllt die Supportanforderungen. Stellen Sie bei der manuellen Einrichtung der Replikationsappliance auf einem physischen Server sicher, dass sie die Anforderungen erfüllt.



**Komponente** | **Anforderung**
--- | ---
 | **VMware-Einstellungen** (VMware-VM-Appliance)
PowerCLI | [PowerCLI Version 6.0](https://my.vmware.com/web/vmware/details?productId=491&downloadGroup=PCLI600R1) sollte installiert werden, wenn die Replikationsappliance auf einer VMware-VM ausgeführt wird.
NIC-Typ | VMXNET3 (wenn es sich bei der Appliance um eine VMware-VM handelt)
 | **Hardwareeinstellungen**
CPU-Kerne | 8
RAM | 16 GB
Anzahl der Datenträger | Drei: Betriebssystem-Datenträger, Prozessservercache-Datenträger, Aufbewahrungslaufwerk
Freier Speicherplatz (Cache) | 600 GB
Freier Speicherplatz (Aufbewahrungslaufwerk) | 600 GB
**Softwareeinstellungen** |
Betriebssystem | Windows Server 2016 oder Windows Server 2012 R2
Gebietsschema des Betriebssystems | Englisch (en-us)
TLS | TLS 1.2 muss aktiviert sein.
.NET Framework | NET Framework 4.6 oder höher sollte auf dem Computer installiert sein (mit aktivierter starker Kryptografie).
MySQL | MySQL sollte auf der Appliance installiert sein.<br/> MySQL sollte installiert sein. Sie können MySQL manuell installieren oder bei der Bereitstellung der Appliance durch Site Recovery installieren lassen.
Andere Apps | Führen Sie keine anderen Apps auf der Replikationsappliance aus.
Windows Server-Rollen | Aktivieren Sie die folgenden Rollen nicht: <br> - Active Directory Domain Services <br>- Internetinformationsdienste <br> - Hyper-V
Gruppenrichtlinien | Aktivieren Sie die folgenden Gruppenrichtlinien nicht: <br> - Zugriff auf Eingabeaufforderung verhindern <br> - Zugriff auf Programme zum Bearbeiten der Registrierung verhindern <br> - Vertrauenslogik für Dateianlagen <br> - Skriptausführung aktivieren <br> [Weitere Informationen](https://technet.microsoft.com/library/gg176671(v=ws.10).aspx)
IIS | - Keine bereits vorhandene Standardwebsite <br> - Keine bereits vorhandene Website/Anwendung sollte an Port 443 lauschen <br>- Die [anonyme Authentifizierung](https://technet.microsoft.com/library/cc731244(v=ws.10).aspx) ist aktiviert. <br> - Aktivieren der Einstellung [FastCGI](https://technet.microsoft.com/library/cc753077(v=ws.10).aspx)
**Netzwerkeinstellungen** |
Art der IP-Adresse | statischen
Ports | 443 (Steuerkanalorchestrierung)<br>9443 (Datentransport)
NIC-Typ | VMXNET3

### <a name="replication-appliance-url-access"></a>URL-Zugriff durch die Replikationsappliance

Die Replikationsappliance benötigt Zugriff auf die folgenden URLs:

**URL** | **Details**
--- | ---
\*.backup.windowsazure.com | Wird für die Übertragung und Koordinierung replizierter Daten verwendet
\*.store.core.windows.net | Wird für die Übertragung und Koordinierung replizierter Daten verwendet
\*.blob.core.windows.net | Wird für den Zugriff auf das Speicherkonto verwendet, in dem replizierte Daten gespeichert werden
\*.hypervrecoverymanager.windowsazure.com | Für Vorgänge und Koordinierung der Replikationsverwaltung verwendet
https:\//management.azure.com | Für Vorgänge und Koordinierung der Replikationsverwaltung verwendet
*.services.visualstudio.com | Wird für Telemetriezwecke (optional) verwendet
time.nist.gov | Wird zum Überprüfen der Zeitsynchronisierung zwischen Systemzeit und der globalen Zeit verwendet.
time.windows.com | Wird zum Überprüfen der Zeitsynchronisierung zwischen Systemzeit und der globalen Zeit verwendet.
https:\//login.microsoftonline.com <br/> https:\//secure.aadcdn.microsoftonline-p.com <br/> https:\//login.live.com <br/> https:\//graph.windows.net <br/> https:\//login.windows.net <br/> https:\//www.live.com <br/> https:\//www.microsoft.com  | Bei der OVF-Einrichtung ist Zugriff auf diese URLs erforderlich. Die URLs werden für die Zugriffssteuerung und die Identitätsverwaltung durch Azure Active Directory verwendet.
https:\//dev.mysql.com/get/Downloads/MySQLInstaller/mysql-installer-community-5.7.20.0.msi | So schließen Sie den MySQL-Download ab


#### <a name="mysql-installation-options"></a>MySQL-Installationsoptionen

MySQL kann mit einer der folgenden Methoden auf der Replikationsappliance installiert werden:

**Methode** | **Details**
--- | ---
Manuell herunterladen und installieren | Laden Sie die Anwendung MySQL manuell herunter, legen Sie sie im Ordner „C:\Temp\ASRSetup“ ab, und installieren Sie sie manuell.<br/> Bei der Einrichtung der Appliance wird MySQL als bereits installiert angezeigt.
Ohne Onlinedownload | Legen Sie das MySQL-Installationsprogramm im Ordner „C:\Temp\ASRSetup“ ab. Wenn Sie die Appliance installieren und zum Herunterladen und Installieren von MySQL klicken, verwendet das Setup das von Ihnen hinzugefügte Installationsprogramm.
Herunterladen und Installieren in Azure Migrate | Wenn Sie die Appliance installieren und nach MySQL gefragt werden, wählen Sie **Herunterladen und installieren** aus.



## <a name="agent-based-migration-vmware-vm-requirements"></a>VMware-VM-Anforderungen für die Migration mit Agent

**Unterstützung** | **Details**
--- | ---
**Computerworkload** | Azure Migrate unterstützt die Migration beliebiger Workloads (u. a. Active Directory- und SQL Server-Workloads), die auf einem unterstützten Computer ausgeführt werden.
**Betriebssysteme** | Die neuesten Informationen finden Sie im Abschnitt zur [Betriebssystemunterstützung](../site-recovery/vmware-physical-azure-support-matrix.md#replicated-machines) für Site Recovery. Azure Migrate bietet identische VM-Betriebssystemunterstützung.
**Linux-Dateisystem/-Gastspeicher** | Die neuesten Informationen finden Sie im Abschnitt zur [Linux-Dateisystemunterstützung](../site-recovery/vmware-physical-azure-support-matrix.md#linux-file-systemsguest-storage) für Site Recovery. Azure Migrate hat die gleiche Linux-Dateisystemunterstützung.
**Netzwerk/Speicher** | Die neuesten Informationen finden Sie in den Bedingungen für [Netzwerk](../site-recovery/vmware-physical-azure-support-matrix.md#network) und [Speicher](../site-recovery/vmware-physical-azure-support-matrix.md#storage) für Site Recovery. Für Azure Migrate gelten die gleichen Netzwerk-/Speicheranforderungen.
**Anforderungen für Azure** | Die neuesten Informationen finden Sie in den Bedingungen für [Azure-Netzwerk](../site-recovery/vmware-physical-azure-support-matrix.md#azure-vm-network-after-failover), [Speicher](../site-recovery/vmware-physical-azure-support-matrix.md#azure-storage) und [Compute](../site-recovery/vmware-physical-azure-support-matrix.md#azure-compute) für Site Recovery. Für Azure Migrate gelten die gleichen Anforderungen für die VMware-Migration.
**Mobilitätsdienst** | Der Mobilitätsdienst-Agent muss auf jeder VM installiert sein, die Sie migrieren möchten.
**UEFI-Start** | Der migrierte virtuelle Computer in Azure wird automatisch in einen virtuellen Computer mit BIOS-Start konvertiert.<br/><br/> Der Betriebssystemdatenträger sollte bis zu vier Partitionen aufweisen, und Volumes sollten mit NTFS formatiert sein.
**Zieldatenträger** | Virtuelle Computer können nur zu verwalteten Datenträgern (HDD Standard, SSD Premium) in Azure migriert werden.
**Datenträgergröße** | 2 TB für den Betriebssystemdatenträger, 8 TB für Datenträger
**Einschränkungen für Datenträger** |  Bis zu 63 Datenträger pro virtuellem Computer
**Verschlüsselte Datenträger/Volumes** | Die Migration von VMs mit verschlüsselten Datenträgern/Volumes wird nicht unterstützt.
**Freigegebener Datenträgercluster** | Nicht unterstützt.
**Unabhängige Datenträger** | Unterstützt.
**Pass-Through-Datenträger** | Unterstützt.
**NFS** | NFS-Volumes, die als Volumes auf den VMs bereitgestellt sind, werden nicht repliziert.
iSCSI-Ziele | Virtuelle Computer mit iSCSI-Zielen werden bei der Migration ohne Agent nicht unterstützt.
**Multipfad-E/A** | Nicht unterstützt.
**Storage vMotion** | Unterstützt
**Kombinierte Netzwerkadapter** | Nicht unterstützt.
**IPv6** | Nicht unterstützt.




## <a name="agent-based-migration-url-access-requirements"></a>URL-Zugriffsanforderungen für die Migration mit Agent

Für den Mobilitätsdienst, der auf virtuellen VMware-Computern ausgeführt wird, ist eine Internetverbindung erforderlich.

Bei der Bereitstellung des Mobilitätsdiensts führt er eine Verbindungsüberwachung für die in der folgenden Tabelle zusammengefassten URLs durch:


**URL** | **Details**  
--- | ---
*.portal.azure.com | Navigieren zu Azure Migrate im Azure-Portal
*.windows.net | Melden Sie sich bei Ihrem Azure-Abonnement an.
*.microsoftonline.com | Erstellen von Active Directory-Anwendungen, damit die Appliance mit dem Azure Migrate-Service kommunizieren kann
management.azure.com | Erstellen von Active Directory-Anwendungen, damit die Appliance mit dem Azure Migrate-Service kommunizieren kann
dc.services.visualstudio.com | Laden Sie App-Protokolle hoch, die für die interne Überwachung verwendet werden.
*.vault.azure.net | Verwalten von Geheimnissen in Azure Key Vault
*.servicebus.windows.net | Kommunikation zwischen der Appliance und dem Azure Migrate-Dienst
*.discoverysrv.windowsazure.com <br/> *.migration.windowsazure.com <br/> *.hypervrecoverymanager.windowsazure.com | Herstellen einer Verbindung mit Azure Migrate-Dienst-URLs
*.blob.core.windows.net | Hochladen von Daten in Speicherkonten

## <a name="agent-based-migration-port-requirements"></a>Portanforderungen für die Migration mit Agent

**Device** | **Connection**
--- | ---
VMs | Der Mobilitätsdienst auf virtuellen Computern kommuniziert mit der lokalen Replikationsappliance (Konfigurationsserver) über den HTTPS-Port 443 für eingehenden Datenverkehr, um die Replikationsverwaltung auszuführen.<br/><br/> Virtuelle Computer senden Replikationsdaten an den Prozessserver (der auf dem Konfigurationsserver ausgeführt wird) über Port HTTPS 9443 für eingehenden Datenverkehr. Dieser Port kann geändert werden.
Replikationsappliance | Die Replikationsappliance orchestriert die Replikation mit Azure über Port HTTPS 443 für ausgehenden Datenverkehr.
Prozessserver | Der Prozessserver empfängt Replikationsdaten, optimiert und verschlüsselt sie und sendet sie über den ausgehenden Port 443 an Azure Storage.<br/> Der Prozessserver wird standardmäßig auf der Replikationsappliance ausgeführt.

## <a name="azure-vm-requirements"></a>Azure-VM-Anforderungen

Alle lokalen VMs, die in Azure repliziert werden, müssen die in dieser Tabelle zusammengefassten Azure-VM-Anforderungen erfüllen. Wenn Site Recovery eine Voraussetzungsprüfung für die Replikation durchführt, ist diese nicht erfolgreich, wenn einige der Anforderungen nicht erfüllt sind.

**Komponente** | **Anforderungen** | **Details**
--- | --- | ---
Gastbetriebssystem | Überprüfen Sie die unterstützten Betriebssysteme für [VMware-VMs mit Replikation ohne Agent](#agentless-migration-vmware-vm-requirements) und für [VMware-VMs mit Replikation mit Agent](#agent-based-migration-vmware-vm-requirements).<br/> Sie können alle Workloads migrieren, die unter einem unterstützten Betriebssystem ausgeführt werden. | Beim Überprüfen tritt ein Fehler auf, wenn keine Unterstützung vorhanden ist.
Architektur des Gastbetriebssystems | 64 Bit. | Beim Überprüfen tritt ein Fehler auf, wenn keine Unterstützung vorhanden ist.
Größe des Betriebssystem-Datenträgers | Bis zu 2.048 GB. | Beim Überprüfen tritt ein Fehler auf, wenn keine Unterstützung vorhanden ist.
Anzahl von Betriebssystem-Datenträgern | 1 | Beim Überprüfen tritt ein Fehler auf, wenn keine Unterstützung vorhanden ist.
Anzahl von Datenträgern für Daten | Maximal 64. | Beim Überprüfen tritt ein Fehler auf, wenn keine Unterstützung vorhanden ist.
Datenträgergröße | Bis zu 4.095 GB | Beim Überprüfen tritt ein Fehler auf, wenn keine Unterstützung vorhanden ist.
Netzwerkadapter | Es werden mehrere Adapter unterstützt. |
Freigegebene VHD | Nicht unterstützt. | Beim Überprüfen tritt ein Fehler auf, wenn keine Unterstützung vorhanden ist.
Fiber-Channel-Datenträger | Nicht unterstützt. | Beim Überprüfen tritt ein Fehler auf, wenn keine Unterstützung vorhanden ist.
BitLocker | Nicht unterstützt. | BitLocker muss deaktiviert sein, bevor Sie die Replikation für einen Computer aktivieren.
Name des virtuellen Computers | 1 bis 63 Zeichen.<br/> Ist auf Buchstaben, Zahlen und Bindestriche beschränkt.<br/><br/> Der Computername muss mit einem Buchstaben oder einer Ziffer beginnen und enden. |  Aktualisieren Sie den Wert in den Computereigenschaften in Site Recovery.
Herstellen einer Verbindung nach der Migration: Windows | So stellen Sie nach der Migration eine Verbindung mit Azure-VMs unter Windows her:<br/> – Aktivieren Sie vor der Migration RDP auf dem lokalen virtuellen Computer. Stellen Sie sicher, dass TCP- und UDP-Regeln für das Profil **Öffentlich** hinzugefügt werden und dass RDP unter **Windows-Firewall** > **Zugelassene Apps** für alle Profile zugelassen ist.<br/> Aktivieren Sie bei Site-to-Site-VPN-Zugriff das RDP unter **Windows-Firewall** -> **Zugelassene Apps und Features** für **private Netzwerke und Domänennetzwerke**. Achten Sie außerdem darauf, dass die SAN-Richtlinie des Betriebssystems auf **OnlineAll** festgelegt ist. [Weitere Informationen](prepare-for-migration.md) |
Herstellen einer Verbindung nach der Migration: Linux | So stellen Sie nach der Migration per SSH eine Verbindung mit Azure-VMs her:<br/> Überprüfen Sie vor der Migration auf dem lokalen Computer, ob der Secure Shell-Dienst auf „Start“ festgelegt ist und ob die Firewallregeln eine SSH-Verbindung zulassen.<br/> Lassen Sie nach dem Failover auf der Azure-VM eingehende Verbindungen am SSH-Port für die Netzwerksicherheitsgruppen-Regeln auf der VM, für die ein Failover ausgeführt wurde, sowie für das Azure-Subnetz zu, mit dem die VM verbunden ist. Fügen Sie außerdem der VM eine öffentliche IP-Adresse hinzu. |  


## <a name="next-steps"></a>Nächste Schritte

[Vorbereiten von VMware](tutorial-prepare-vmware.md) auf die Bewertung und Migration
