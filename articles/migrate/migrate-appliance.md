---
title: Architektur der Azure Migrate-Appliance | Microsoft-Dokumentation
description: Enthält eine Übersicht über die Azure Migrate-Appliance
author: rayne-wiselman
ms.service: azure-migrate
ms.topic: conceptual
ms.date: 07/04/2019
ms.author: raynew
ms.openlocfilehash: 249cbea173afe1671118446e0714b721b8c7f72b
ms.sourcegitcommit: 609d4bdb0467fd0af40e14a86eb40b9d03669ea1
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 11/06/2019
ms.locfileid: "73685096"
---
# <a name="azure-migrate-appliance"></a>Azure Migrate-Appliance

Dieser Artikel beschreibt die Azure Migrate-Appliance. Sie stellen die Appliance bereit, wenn Sie mit den Bewertungs- und Migrationstools von Azure Migrate Anwendungen, Infrastrukturen und Workloads ermitteln, bewerten und zu Microsoft Azure migrieren möchten. 

[Azure Migrate](migrate-services-overview.md) ist ein zentraler Hub zum Nachverfolgen von Ermittlung und Bewertung Ihrer lokalen Apps und Workloads und von virtuellen Computern in der privaten/öffentlichen Cloud sowie deren Migration zu Azure. Der Hub stellt Azure Migrate-Tools für die Bewertung und Migration sowie Angebote von unabhängigen Drittanbietern (Independent Software Vendors, ISVs) bereit.



## <a name="appliance-overview"></a>Appliance – Übersicht

Es gibt folgende Typen und Verwendungsmöglichkeiten von Azure Migrate-Appliances.

**Bereitgestellt als** | **Verwendung** | **Details**
--- | --- |  ---
Virtueller VMware-Computer | VMware-VM-Bewertung mit dem Azure Migrate-Bewertungstool.<br/><br/> Migration von VMware-VMs ohne Agent mit dem Tool für die Azure Migrate-Servermigration | Laden Sie die OVA-Vorlage herunter, und importieren Sie sie in vCenter Server, um die Appliance-VM zu erstellen.
Virtueller Hyper-V-Computer | Bewertung virtueller Hyper-V-Computer mit dem Azure Migrate-Bewertungstool. | Laden Sie die gezippte VHD-Datei herunter, und importieren Sie sie in Hyper-V, um die Appliance-VM zu erstellen.

## <a name="appliance-access"></a>Appliancezugriff

Nachdem Sie die Appliance konfiguriert haben, können Sie über den TCP-Port 3389 mit Remote-Zugriff auf die Appliance-VM zugreifen. Sie können auch über Port 44368 mit der URL `https://<appliance-ip-or-name>:44368` über Remote-Zugriff auf die Webverwaltungsanwendung für die Appliance zugreifen.

## <a name="appliance-license"></a>Appliancelizenz
Die Appliance ist mit einer Windows Server 2016-Evaluierungslizenz ausgestattet, die 180 Tage lang gültig ist. Wenn der Evaluierungszeitraum fast abgelaufen ist, empfiehlt es sich, eine neue Appliance herunterzuladen und bereitzustellen oder die Betriebssystemlizenz der Appliance-VM zu aktivieren.

## <a name="appliance-agents"></a>Appliance-Agents
Auf dieser Appliance sind Agents installiert.

**Agent** | **Details**
--- | ---
Ermittlungs-Agent | Sammelt Konfigurationsdaten von lokalen virtuellen Computern.
Bewertungs-Agent | Erstellt ein Profil der lokalen Umgebung, um VM-Leistungsdaten zu sammeln.
Migrationsadapter | Orchestriert die VM-Replikation und koordiniert die Kommunikation zwischen virtuellen Computern und Azure.
Migrations-Gateway | Sendet replizierte VM-Daten an Azure.


## <a name="appliance-deployment-requirements"></a>Anforderungen für die Appliancebereitstellung

- [Überprüfen](migrate-support-matrix-vmware.md#assessment-appliance-requirements) Sie die Bereitstellungsanforderungen für eine VMware-Appliance und die URLs, auf die die Appliance zugreifen muss.
- [Überprüfen](migrate-support-matrix-hyper-v.md#assessment-appliance-requirements) Sie die Bereitstellungsanforderungen für eine Hyper-V-Appliance und die URLs, auf die die Appliance zugreifen muss.


## <a name="collected-performance-data-vmware"></a>Gesammelte Leistungsdaten von VMware

Nachstehend finden Sie die VMware-VM-Leistungsdaten, die die Appliance sammelt und an Azure sendet.

**Daten** | **Leistungsindikator** | **Auswirkungen auf die Bewertung**
--- | --- | ---
CPU-Auslastung | cpu.usage.average | Empfohlene VM-Größe/Kosten
Arbeitsspeichernutzung | mem.usage.average | Empfohlene VM-Größe/Kosten
Datenträgerlesedurchsatz (MB pro Sekunde) | virtualDisk.read.average | Berechnung von Datenträgergröße, Speicherkosten und VM-Größe
Datenträgerschreibdurchsatz (MB pro Sekunde) | virtualDisk.write.average | Berechnung von Datenträgergröße, Speicherkosten und VM-Größe
Datenträgerlesevorgänge pro Sekunde | virtualDisk.numberReadAveraged.average | Berechnung von Datenträgergröße, Speicherkosten und VM-Größe
Datenträgerschreibvorgänge pro Sekunde | virtualDisk.numberWriteAveraged.average  | Berechnung von Datenträgergröße, Speicherkosten und VM-Größe
NIC-Lesedurchsatz (MB pro Sekunde) | net.received.average | Berechnung der VM-Größe
NIC-Schreibdurchsatz (MB pro Sekunde) | net.transmitted.average  |Berechnung der VM-Größe


## <a name="collected-metadata-vmware"></a>Gesammelte Metadaten für VMware

> [!NOTE]
> Die von der Azure Migrate-Appliance ermittelten Metadaten werden verwendet, damit Sie Ihre Anwendungen bei der Migration zu Azure richtig skalieren können. Außerdem sollen sie bei der Durchführung von Azure-Eignungsanalysen, Analysen der Anwendungsabhängigkeit und bei der Kostenplanung helfen. Microsoft verwendet diese Daten nicht zur Überprüfung der Compliance von Lizenzen.

Nachstehend finden Sie die vollständige Liste der VMware-VM-Metadaten, die die Appliance sammelt und an Azure sendet.

**Daten** | **Leistungsindikator**
--- | --- 
**Computerdetails** | 
VM-ID | vm.Config.InstanceUuid 
Name des virtuellen Computers | vm.Config.Name
vCenter Server-ID | VMwareClient.Instance.Uuid
VM-Beschreibung | vm.Summary.Config.Annotation
Lizenzproduktname | vm.Client.ServiceContent.About.LicenseProductName
Betriebssystemtyp | vm.SummaryConfig.GuestFullName
Starttyp | vm.Config.Firmware
Anzahl von Kernen | vm.Config.Hardware.NumCPU
Arbeitsspeicher (MB) | vm.Config.Hardware.MemoryMB
Anzahl der Datenträger | vm.Config.Hardware.Device.ToList().FindAll(x => is VirtualDisk).count
Liste der Datenträgergrößen | vm.Config.Hardware.Device.ToList().FindAll(x => is VirtualDisk)
Liste der Netzwerkadapter | vm.Config.Hardware.Device.ToList().FindAll(x => is VirtualEthernet).count
CPU-Auslastung | cpu.usage.average
Arbeitsspeichernutzung |mem.usage.average
**Details pro Datenträger** | 
Datenträgerschlüsselwert | disk.Key
Diskunit-Nummer | disk.UnitNumber
Datenträgercontroller-Schlüsselwert | disk.ControllerKey.Value
Bereitgestellte Gigabytes | virtualDisk.DeviceInfo.Summary
Name des Datenträgers | Mithilfe von disk.UnitNumber, disk.Key und disk.ControllerKey.Value generierter Wert
Lesevorgänge pro Sekunde | virtualDisk.numberReadAveraged.average
Schreibvorgänge pro Sekunde | virtualDisk.numberWriteAveraged.average
Lesedurchsatz (MB pro Sekunde) | virtualDisk.read.average
Schreibdurchsatz (MB pro Sekunde) | virtualDisk.write.average
**Details pro NIC** | 
Netzwerkadaptername | nic.Key
MAC-Adresse | ((VirtualEthernetCard)nic).MacAddress
IPv4-Adressen | vm.Guest.Net
IPv6-Adressen | vm.Guest.Net
Lesedurchsatz (MB pro Sekunde) | net.received.average
Schreibdurchsatz (MB pro Sekunde) | net.transmitted.average
**Inventurpfaddetails** | 
NAME | container.GetType().Name
Typ des untergeordneten Objekts | container.ChildType
Referenzdetails | container.MoRef
Details des übergeordneten Objekts | Container.Parent
Ordnerdetails pro VM | ((Folder)container).ChildEntity.Type
Datencenterdetails pro VM | ((Datacenter)container).VmFolder
Details zum Rechenzentrum pro Hostordner | ((Datacenter)container).HostFolder
Clusterdetails pro Host | ((ClusterComputeResource)container).Host
Hostdetails pro VM | ((HostSystem)container).VM



## <a name="collected-performance-data-hyper-v"></a>Gesammelte Leistungsdaten von Hyper-V

> [!NOTE]
> Die von der Azure Migrate-Appliance ermittelten Metadaten werden verwendet, damit Sie Ihre Anwendungen bei der Migration zu Azure richtig skalieren können. Außerdem sollen sie bei der Durchführung von Azure-Eignungsanalysen, Analysen der Anwendungsabhängigkeit und bei der Kostenplanung helfen. Microsoft verwendet diese Daten nicht zur Überprüfung der Compliance von Lizenzen.

Nachstehend finden Sie die Hyper-VM-Leistungsdaten, die die Appliance sammelt und an Azure sendet.

**Leistungsindikatorklasse** | **Leistungsindikator** | **Auswirkungen auf die Bewertung**
--- | --- | ---
Virtueller Hyper-V-Hypervisor-Prozessor | % Gastausführungszeit | Empfohlene VM-Größe/Kosten
Hyper-V-VM mit dynamischem Arbeitsspeicher | Aktueller Druck (%)<br/> Sichtbarer physischer Speicher des Gastcomputers (MB) | Empfohlene VM-Größe/Kosten
Virtuelles Hyper-V-Speichergerät | Lesebytes/Sekunde | Berechnung von Datenträgergröße, Speicherkosten und VM-Größe
Virtuelles Hyper-V-Speichergerät | Schreibbytes/Sekunde | Berechnung von Datenträgergröße, Speicherkosten und VM-Größe
Virtueller Hyper-V-Netzwerkadapter | Empfangene Bytes/Sekunde | Berechnung der VM-Größe
Virtueller Hyper-V-Netzwerkadapter | Gesendete Bytes/Sekunde | Berechnung der VM-Größe

- Die CPU-Auslastung ist die Summe aller Auslastungen für alle an einen virtuellen Computer angeschlossenen virtuellen Prozessoren.
- Die Speichernutzung ist (Aktueller Druck * Sichtbarer physischer Speicher des Gastcomputers) / 100.
- Die Werte für die Festplatten- und Netzwerkauslastung werden von den aufgeführten Hyper-V-Leistungsindikatoren gesammelt.

## <a name="collected-metadata-hyper-v"></a>Gesammelte Metadaten von Hyper-V

Nachstehend finden Sie die vollständige Liste der Hyper-V-VM-Metadaten, die die Appliance sammelt und an Azure sendet.

**Daten** | **WMI-Klasse** | **WMI-Klasseneigenschaft**
--- | --- | ---
**Computerdetails** | 
Seriennummer von BIOS _ Msvm_BIOSElement | BIOSSerialNumber
VM-Typ (Gen 1 oder 2) | Msvm_VirtualSystemSettingData | VirtualSystemSubType
Anzeigename der VM | Msvm_VirtualSystemSettingData | ElementName
VM-Version | Msvm_ProcessorSettingData | VirtualQuantity
Arbeitsspeicher (Bytes) | Msvm_MemorySettingData | VirtualQuantity
Maximaler Arbeitsspeicher, der von der VM genutzt werden kann | Msvm_MemorySettingData | Begrenzung
Dynamischer Arbeitsspeicher aktiviert | Msvm_MemorySettingData | DynamicMemoryEnabled
Name/Version/FQDN des Betriebssystems | Msvm_KvpExchangeComponent | GuestIntrinsicExchangeItems-Namensdaten
Betriebszustand der VM | Msvm_ComputerSystem | EnabledState
**Details pro Datenträger** | 
Datenträgeridentifikator | Msvm_VirtualHardDiskSettingData | VirtualDiskId
Virtueller Datenträgertyp | Msvm_VirtualHardDiskSettingData | type
Virtuelle Datenträgergröße | Msvm_VirtualHardDiskSettingData | MaxInternalSize
Virtueller übergeordneter Datenträger | Msvm_VirtualHardDiskSettingData | ParentPath
**Details pro NIC** | 
IP-Adressen (synthetische NICs) | Msvm_GuestNetworkAdapterConfiguration | IPAddresses
DHCP aktiviert (synthetische NICs) | Msvm_GuestNetworkAdapterConfiguration | DHCPEnabled
NIC-ID (synthetische NICs) | Msvm_SyntheticEthernetPortSettingData | InstanceID
NIC-MAC-Adresse (synthetische NICs) | Msvm_SyntheticEthernetPortSettingData | Adresse
NIC-ID (Legacy-NICs) | MsvmEmulatedEthernetPortSetting-Daten | InstanceID
NIC-MAC-ID (Legacy-NICs) | MsvmEmulatedEthernetPortSetting-Daten | Adresse




## <a name="discovery-and-collection-process"></a>Ermittlungs- und Sammlungsprozess

Die Appliance kommuniziert mit vCenter-Servern und Hyper-V-Hosts/Clustern über den folgenden Prozess.


1. **Start der Ermittlung**:
    - Wenn Sie die Ermittlung auf der Hyper-V-Appliance starten, kommuniziert sie mit den Hyper-V-Hosts über die WinRM-Ports 5985 (HTTP) und 5986 (HTTPS).
    - Wenn Sie die Ermittlung auf der VMware-Appliance starten, kommuniziert sie standardmäßig mit dem vCenter-Server über TCP-Port 443. Wenn der vCenter-Server an einem anderen Port lauscht, können Sie ihn in der Appliance-Webanwendung konfigurieren.
2. **Sammeln von Metadaten und Leistungsdaten**:
    - Die Appliance verwendet eine Common Information Model (CIM)-Sitzung, um Hyper-V-VM-Daten vom Hyper-V-Host an Port 5985 und 5986 zu sammeln.
    - Die Appliance kommuniziert standardmäßig über Port 443, um VMware-VM-Daten vom vCenter Server zu sammeln.
3. **Senden von Daten**: Die Appliance sendet die gesammelten Daten über SSL-Port 443 an die Azure Migrate-Serverbewertung und die Azure Migrate-Servermigration.
    - Bei Leistungsdaten sammelt die Appliance Nutzungsdaten in Echtzeit.
        - Die Leistungsdaten werden für VMware alle 20 Sekunden und für Hyper-V alle 30 Sekunden für jede Leistungsmetrik erfasst.
        - Die gesammelten Daten werden zu einem einzigen 10-Minuten-Datenpunkt zusammengeführt.
        - Der Spitzenauslastungswert wird aus allen 20/30-Sekunden-Datenpunkten ausgewählt und zur Berechnung der Bewertung an Azure gesendet.
        - Basierend auf dem in den Bewertungseigenschaften angegebenen Perzentilwert (50./90./95./99.) werden die Zehn-Minuten-Punkte in aufsteigender Reihenfolge sortiert, und der entsprechende Perzentilwert wird zur Berechnung der Bewertung verwendet
    - Bei der Servermigration beginnt die Appliance mit der Ermittlung von VM-Daten und repliziert diese nach Azure.
4. **Bewerten und Migrieren**: Sie können nun mit der Azure Migrate-Serverbewertung aus den von der Appliance gesammelten Metadaten Bewertungen erstellen. Darüber hinaus können Sie auch mit der Migration von VMware-VMs mit der Azure Migrate-Servermigration beginnen, um die VM-Replikation ohne Agent zu organisieren.


![Architecture](./media/migrate-appliance/architecture.png)


## <a name="appliance-upgrades"></a>Appliance-Upgrades

Die Appliance wird aktualisiert, wenn die auf der Appliance ausgeführten Azure Migrate-Agents aktualisiert werden.

- Dies geschieht automatisch, da auf der Appliance standardmäßig das automatische Update aktiviert ist.
- Sie können diese Standardeinstellung ändern, um die Agents manuell zu aktualisieren.
- Um die automatische Aktualisierung zu deaktivieren, wechseln Sie im Registrierungs-Editor zu „HKEY_LOCAL_MACHINE\SOFTWARE\Microsoft\AzureAppliance“, und legen Sie den AutoUpdate-Registrierungsschlüssel auf 0 (DWORD) fest.
 
### <a name="set-agent-updates-to-manual"></a>Festlegen der Agent-Updates auf manuelle Updates

Stellen Sie bei manuellen Updates sicher, dass Sie alle Agents auf der Appliance gleichzeitig aktualisieren, indem Sie für jeden veralteten Agent auf der Appliance die Schaltfläche **Update** verwenden. Sie können die Update-Einstellung jederzeit wieder auf automatische Updates zurücksetzen.

## <a name="next-steps"></a>Nächste Schritte

[Erfahren Sie, wie](tutorial-assess-vmware.md#set-up-the-appliance-vm) Sie die Appliance für VMware einrichten.
[Erfahren Sie, wie](tutorial-assess-hyper-v.md#set-up-the-appliance-vm) Sie die Appliance für Hyper-V einrichten.

