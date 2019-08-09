---
title: Architektur der Azure Migrate-Appliance | Microsoft-Dokumentation
description: Enthält eine Übersicht über den Azure Migrate-Appliance
author: rayne-wiselman
ms.service: azure-migrate
ms.topic: conceptual
ms.date: 07/04/2019
ms.author: raynew
ms.openlocfilehash: 6537bfe5df8de298593428fb21448181ad8075fc
ms.sourcegitcommit: e3b0fb00b27e6d2696acf0b73c6ba05b74efcd85
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 07/30/2019
ms.locfileid: "68663462"
---
# <a name="azure-migrate-appliance"></a>Azure Migrate-Appliance

Dieser Artikel beschreibt die Azure Migrate-Appliance. Sie setzen die Appliance ein, wenn Sie die Tools der Azure Migrate-Bewertung and Migration verwenden, um Anwendungen, Infrastrukturen und Workloads zu ermitteln, zu bewerten und auf Microsoft Azure zu übertragen. 

[Azure Migrate](migrate-services-overview.md) ist ein zentraler Hub zum Nachverfolgen der Ermittlung, Bewertung und Migration Ihrer lokalen Apps und Workloads sowie von VMs in der privaten/öffentlichen Cloud zu Azure. Der Hub stellt Azure Migrate-Tools für die Bewertung und Migration sowie Angebote von unabhängigen Drittanbietern (Independent Software Vendors, ISVs) bereit.



## <a name="appliance-overview"></a>Appliance – Übersicht

Es gibt folgende Typen und Verwendungsmöglichkeiten von Azure Migrate-Appliances.

**Bereitgestellt als** | **Verwendung** | **Details**
--- | --- |  ---
Virtueller VMware-Computer | VMware-VM-Bewertung mit dem Azure Migrate-Bewertungstool.<br/><br/> Vorbereiten von die Migration von VMware-VMs ohne Agent mit dem Tool für die Azure Migrate-Servermigration | Laden Sie die OVA-Vorlage herunter und importieren sie in vCenter Server, um die Appliance-VM zu erstellen.
Virtueller Hyper-V-Computer | Hyper-V-VM-Bewertung mit dem Azure Migrate-Bewertungstool. | Laden Sie gezippte VHD herunter und importieren Sie sie in Hyper-V, um die Appliance-VM zu erstellen.

## <a name="appliance-access"></a>Appliancezugriff

Nachdem Sie die Appliance konfiguriert haben, können Sie über den TCP-Port 3389 mit Remote-Zugriff auf die Appliance-VM zugreifen. Sie können auch über Port 44368 mit der folgenden URL über Remote-Zugriff auf die Webverwaltungsanwendung für die Appliance zugreifen: `https://<appliance-ip-or-name>:44368`.

## <a name="appliance-license"></a>Appliancelizenz
Die Appliance ist mit einer Windows Server 2016-Evaluierungslizenz ausgestattet, die 180 Tage lang gültig ist. Wenn der Evaluierungszeitraum fast abgelaufen ist, empfiehlt es sich, eine neue Appliance herunterzuladen und bereitzustellen oder die Betriebssystemlizenz der Appliance-VM zu aktivieren.

## <a name="appliance-agents"></a>Appliance-Agents
Auf dieser Appliance sind Agents installiert.

**Agent** | **Details**
--- | ---
Ermittlungs-Agent | Sammelt Konfigurationsdaten von lokalen VMs.
Bewertungs-Agent | Erstellt ein Profil der lokalen Umgebung, um VM-Leistungsdaten zu sammeln.
Migrationsadapter | Orchestriert die VM-Replikation und koordiniert die Kommunikation zwischen VMs und Azure.
Migrations-Gateway | Sendet replizierte VM-Daten an Azure.


## <a name="appliance-deployment-requirements"></a>Anforderungen für die Appliancebereitstellung

- [Überprüfen](migrate-support-matrix-vmware.md#assessment-appliance-requirements) Sie die Bereitstellungsanforderungen für eine VMware-Appliance und die URLs, auf die die Appliance zugreifen muss.
- [Überprüfen](migrate-support-matrix-hyper-v.md#assessment-appliance-requirements) Sie die Bereitstellungsanforderungen für eine Hyper-V-Appliance und die URLs, auf die die Appliance zugreifen muss.


## <a name="collected-performance-data-vmware"></a>Gesammelte Leistungsdaten von VMware

Hier sind die VMware-VM-Leistungsdaten, die die Appliance sammelt und an Azure sendet.

**Daten** | **Leistungsindikator** | **Auswirkungen auf die Bewertung**
--- | --- | ---
CPU-Auslastung | cpu.usage.average | Empfohlene VM-Größe/Kosten
Arbeitsspeichernutzung | mem.usage.average | Empfohlene VM-Größe/Kosten
Datenträgerlesedurchsatz (MB pro Sekunde) | virtualDisk.read.average | Berechnung der Datenträgergröße, Speicherkosten und der VM-Größe
Datenträgerschreibdurchsatz (MB pro Sekunde) | virtualDisk.write.average | Berechnung der Datenträgergröße, Speicherkosten und der VM-Größe
Datenträgerlesevorgänge pro Sekunde | virtualDisk.numberReadAveraged.average | Berechnung der Datenträgergröße, Speicherkosten und der VM-Größe
Datenträgerschreibvorgänge pro Sekunde | virtualDisk.numberWriteAveraged.average  | Berechnung der Datenträgergröße, Speicherkosten und der VM-Größe
NIC-Lesedurchsatz (MB pro Sekunde) | net.received.average | Berechnung der VM-Größe
NIC-Schreibdurchsatz (MB pro Sekunde) | net.transmitted.average  |Berechnung der VM-Größe


## <a name="collected-metadata-vmware"></a>Gesammelte Metadaten für VMware

Hier ist die vollständige Liste der VMware-VM-Metadaten, die die Appliance sammelt und an Azure sendet.

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
Dikunit-Nummer | disk.UnitNumber
Datenträgercontroller-Schlüsselwert | disk.ControllerKey.Value
Bereitgestellte Gigabytes | virtualDisk.DeviceInfo.Summary
Name des Datenträgers | Wert, der wird mithilfe von disk.UnitNumber, disk.Key und disk.ControllerKey.Value generiert wird
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
Datencenterdetails pro Hostordner | ((Datacenter)container).HostFolder
Clusterdetails pro Host | ((ClusterComputeResource)container).Host
Hostdetails pro VM | ((HostSystem)container).Vm



## <a name="collected-performance-data-hyper-v"></a>Gesammelte Leistungsdaten von Hyper-V

Hier sind die VMware-VM-Leistungsdaten, die die Appliance sammelt und an Azure sendet.

**Leistungsindikatorklasse** | **Leistungsindikator** | **Auswirkungen auf die Bewertung**
--- | --- | ---
Virtueller Hyper-V-Hypervisor-Prozessor | % Gastausführungszeit | Empfohlene VM-Größe/Kosten
Hyper-V-VM mit dynamischem Arbeitsspeicher | Aktueller Druck (%)<br/> Sichtbarer physischer Speicher des Gastcomputers (MB) | Empfohlene VM-Größe/Kosten
Virtuelles Hyper-V-Speichergerät | Byte lesen/Sekunde | Berechnung der Datenträgergröße, Speicherkosten und der VM-Größe
Virtuelles Hyper-V-Speichergerät | Byte schreiben/Sekunde | Berechnung der Datenträgergröße, Speicherkosten und der VM-Größe
Virtueller Hyper-V-Netzwerkadapter | Empfangene Byte/Sekunde | Berechnung der VM-Größe
Virtueller Hyper-V-Netzwerkadapter | Gesendete Byte/Sekunde | Berechnung der VM-Größe

- Die CPU-Auslastung ist die Summe aller Auslastungen für alle an eine VM angeschlossenen virtuellen Prozessoren.
- Die Speichernutzung ist (Aktueller Druck * Sichtbarer physischer Speicher des Gastcomputers) / 100.
- Die Werte für die Festplatten- und Netzwerkauslastung werden von den aufgeführten Hyper-V-Leistungsindikatoren gesammelt.

## <a name="collected-metadata-hyper-v"></a>Gesammelte Metadaten von Hyper-V

Hier ist die vollständige Liste der Hyper-V-VM-Metadaten, die die Appliance sammelt und an Azure sendet.

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
Nic-ID (synthetische NICs) | Msvm_SyntheticEthernetPortSettingData | InstanceID
NIC-MAC-Adresse (synthetische NICs) | Msvm_SyntheticEthernetPortSettingData | Adresse
Nic-ID (Legacy-NICs) | MsvmEmulatedEthernetPortSetting Data | InstanceID
NIC-MAC-ID (Legacy-NICs) | MsvmEmulatedEthernetPortSetting Data | Adresse




## <a name="discovery-and-collection-process"></a>Ermittlungs-und Sammlungsprozess

Die Appliance kommuniziert mit vCenter Servern und Hyper-V Hosts/Clustern über den folgenden Prozess.


1. **Start der Ermittlung**:
    - Wenn Sie die Ermittlung auf der Hyper-V-Appliance starten, kommuniziert sie mit den Hyper-V-Hosts auf den WinRM-Ports 5985 (HTTP) und 5986 (HTTPS).
    - Wenn Sie die Erkennung auf der VMware-Appliance starten, kommuniziert sie standardmäßig mit dem vCenter-Server auf TCP-Port 443. Wenn der vCenter-Server an einem anderen Port lauscht, können Sie ihn in der Appliance-Webanwendung konfigurieren.
2. **Sammeln von Metadaten und Leistungsdaten**:
    - Die Appliance verwendet eine Common Information Model (CIM)-Sitzung, um Hyper-V VM-Daten vom Hyper-V-Host auf den Ports 5985 und 5986 zu sammeln.
    - Die Appliance kommuniziert standardmäßig mit Port 443, um VMware VM-Daten vom vCenter Server zu sammeln.
3. **Senden von Daten**: Die Appliance sendet die gesammelten Daten an Azure Migrate-Serverbewertung und Azure Migrate-Servermigration über SSL Port 443.
    - Für Leistungsdaten sammelt die Appliance Nutzungsdaten in Echtzeit.
        - Die Leistungsdaten werden für VMware alle 20 Sekunden und für Hyper-V alle 30 Sekunden für jede Leistungsmetrik erfasst.
        - Die gesammelten Daten werden für zehn Minuten zu einem einzigen Datenpunkt zusammengeführt.
        - Der Spitzenauslastungswert wird aus allen 20/30-Sekunden-Datenpunkten ausgewählt und zur Berechnung der Bewertung an Azure gesendet.
        - Basierend auf dem in den Bewertungseigenschaften (50./90./95./99./99.) angegebenen Quantilwert werden die Zehn-Minuten-Punkte in aufsteigender Reihenfolge sortiert und der entsprechende Quantilwert zur Berechnung der Bewertung verwendet
    - Bei der Servermigration beginnt die Appliance mit der Erfassung von VM-Daten und repliziert diese nach Azure.
4. **Bewerten und Migrieren**: Sie können nun mit Azure Migrate-Serverbewertung Beurteilungen aus den von der Appliance gesammelten Metadaten erstellen. Darüber hinaus können Sie auch mit der Migration von VMware-VMs mit Azure Migrate-Servermigration beginnen, um die agentenlose VM-Replikation zu organisieren.


![Architecture](./media/migrate-appliance/architecture.png)


## <a name="appliance-upgrades"></a>Appliance-Upgrades

Die Appliance wird aktualisiert, wenn die auf der Appliance laufenden Azure Migrate-Agents aktualisiert werden.

- Dies geschieht automatisch, da das automatische Update auf dem Gerät standardmäßig aktiviert ist.
- Sie können diese Standardeinstellung ändern, um die Agents manuell zu aktualisieren.
- Um automatische Updates zu deaktivieren, legen Sie den Registrierungsschlüssel „Automatisches Appliance-Update“ in HKLM\SOFTWAREMicrosoft\Azure fest.

### <a name="set-agent-updates-to-manual"></a>Agent-Updates auf manuell festlegen

Für manuelle Updates stellen Sie sicher, dass Sie alle Agents auf der Appliance gleichzeitig aktualisieren, indem Sie die Schaltfläche **„Update“** für jeden veralteten Agent auf der Appliance verwenden. Sie können die Update-Einstellung jederzeit wieder auf automatische Updates zurücksetzen.

## <a name="next-steps"></a>Nächste Schritte

[Erfahren Sie, wie](tutorial-assess-vmware.md#set-up-the-appliance-vm) Sie die Appliance für VMware einrichten.
[Erfahren Sie, wie](tutorial-assess-hyper-v.md#set-up-the-appliance-vm) Sie die Appliance für Hyper-V einrichten.

