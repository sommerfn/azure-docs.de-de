---
title: Azure Migrate-Matrix für die VMware-Bewertung und -Migration
description: Fasst die Supporteinstellungen und ‑einschränkungen für die Bewertung und Migration von VMware-VMs auf Azure mit dem Azure Migrate-Dienst zusammen.
services: backup
author: rayne-wiselman
manager: carmonm
ms.service: backup
ms.topic: conceptual
ms.date: 06/24/2019
ms.author: raynew
ms.openlocfilehash: 567a6582e193208a7ff4aa37bafefe1dec4f4e8f
ms.sourcegitcommit: 47ce9ac1eb1561810b8e4242c45127f7b4a4aa1a
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 07/11/2019
ms.locfileid: "67810153"
---
# <a name="support-matrix-for-vmware-assessment-and-migration"></a>Unterstützungsmatrix für die VMware-Bewertung und -Migration

Mit dem [Azure Migrate-Dienst](migrate-overview.md) können Sie Computer bewerten und in die Microsoft Azure-Cloud migrieren. Dieser Artikel fasst die Supporteinstellungen und -einschränkungen für die Bewertung und Migration von lokalen VMware-VMs zusammen.


## <a name="vmware-scenarios"></a>VMware-Szenarien

Die Tabelle fasst die unterstützten Szenarien für VMware-VMs zusammen.

**Bereitstellung** | **Details** 
--- | --- 
**Bewertung lokaler VMware-VMs** | [Richten Sie](tutorial-prepare-vmware.md) Ihre erste Bewertung ein.<br/><br/> [Führen](scale-vmware-assessment.md) Sie eine umfangreiche Bewertung aus.
**Migrieren von VMware-VMs** | Sie können eine Migration ohne Agent verwenden, die jedoch einige Einschränkungen hat, oder eine agentenbasierte Migration verwenden. [Weitere Informationen](server-migrate-overview.md)


    


## <a name="azure-migrate-projects"></a>Azure Migrate-Projekte

**Unterstützung** | **Details**
--- | ---
Azure-Berechtigungen | Sie benötigen Berechtigungen für Mitwirkende oder Eigentümer im Abonnement, um ein Azure Migrate-Projekt zu erstellen.
VMware-Einschränkungen  | Bewerten Sie bis zu 35.000 VMware-VMs in einem einzigen Projekt.

Ein Projekt kann im Rahmen der Bewertungseinschränkungen sowohl VMware-VMs als auch Hyper-V-VMs umfassen.

## <a name="assessment-vmware-server-requirements"></a>Bewertung der Anforderungen für VMware-Server

Diese Tabelle fasst die Unterstützung und Einschränkungen der Bewertung für VMware-Virtualisierungsserver zusammen.

**Unterstützung** | **Details**
--- | ---
**vCenter-Server** | VMware-VMs, die Sie bewerten möchten, müssen von einem oder mehreren vCenter Servern mit 5.5, 6.0, 6.5 oder 6.7 verwaltet werden.

## <a name="assessment-vcenter-server-permissions"></a>vCenter Server-Berechtigungen für die Bewertung

Für die Bewertung wird ein schreibgeschütztes Konto benötigt, um auf vCenter Server zugreifen zu können.

## <a name="assessment-appliance-requirements"></a>Anforderungen an die Geräte für die Bewertung

**Unterstützung** | **Details**
--- | ---
**ESXi** | Die Appliance-VM muss auf einem ESXi-Host mit Version 5.5 oder höher bereitgestellt werden.
**Azure Migrate-Projekt** | Ein Gerät kann einem einzelnen Projekt zugeordnet werden.
**vCenter Server** | Ein Gerät kann bis zu 10.000 VMware-VMs auf einem vCenter Server ermitteln.<br/> Ein Gerät kann sich mit einem vCenter Server verbinden.


## <a name="assessment-url-access-requirements"></a>URL-Zugriffsanforderungen für die Bewertung

Das Azure Migrate-Gerät muss mit dem Internet verbunden sein.

- Wenn Sie das Gerät bereitstellen, führt Azure Migrate eine Verbindungsüberwachung zu den in der folgenden Tabelle zusammengefassten URLs durch.
- Sollten Sie einen URL-basierten Firewallproxy verwenden, lassen Sie den Zugriff auf diese URLs zu und stellen Sie sicher, dass der Proxy alle CNAME-Einträge auflöst, die er beim Durchsuchen der URLs empfängt.

**URL** | **Details**  
--- | --- |
*.portal.azure.com  | Navigieren Sie im Azure-Portal zu Azure Migrate.
*.windows.net | Melden Sie sich bei Ihrem Azure-Abonnement an.
*.microsoftonline.com | Erstellen Sie Active Directory-Anwendungen, damit die Appliance mit dem Azure Migrate-Service kommunizieren kann.
management.azure.com | Erstellen Sie Active Directory-Anwendungen, damit die Appliance mit dem Azure Migrate-Service kommunizieren kann.
dc.services.visualstudio.com | Laden Sie Anwendungsprotokolle hoch, die für die interne Überwachung verwendet werden.
*.vault.azure.net | Verwalten von Geheimnissen in Azure Key Vault.
*.servicebus.windows.net | Kommunikation zwischen der Appliance und dem Azure Migrate-Dienst.
*.discoverysrv.windowsazure.com <br/> *.migration.windowsazure.com <br/> *.hypervrecoverymanager.windowsazure.com | Verbindung mit Azure Migrate-Dienst-URLs herstellen.
*.blob.core.windows.net | Hochladen von Daten in Speicherkonten.


## <a name="assessment-port-requirements"></a>Portanforderungen für die Bewertung

**Device** | **Connection**
--- | --- 
Appliance | Eingehende Verbindungen auf dem TCP-Port 3389, damit Remotedesktopverbindungen eine Verbindung mit der Appliance aufbauen können.<br/> Eingehende Verbindungen auf Port 44368, um über Remotezugriff über die URL: https://<appliance-ip-or-name>:44368 auf die Appliance-Verwaltungsanwendung zugreifen zu können. <br/>Ausgehende Verbindungen auf Port 443, um Ermittlungs- und Leistungsmetadaten an Azure Migrate zu senden.
vCenter-Server | Eingehende Verbindungen auf dem TCP-Port 443, damit die Appliance Konfigurations- und Leistungsmetadaten für Bewertungen sammeln kann. <br/> Die Appliance verbindet sich standardmäßig mit vCenter auf Port 443. Wenn der vCenter-Server an einem anderen Port lauscht, können Sie den Port beim Einrichten der Ermittlung ändern.


## <a name="agentless-migration-vmware-server-requirements"></a>Migration ohne Agent – VMware-Serveranforderungen

Diese Tabelle fasst die Unterstützung und Einschränkungen der Bewertung für VMware-Virtualisierungsserver zusammen.

**Unterstützung** | **Details**
--- | ---
**vCenter-Server** | VMware-VMs, die Sie per Migration ohne Agent migrieren möchten, müssen von einem oder mehreren vCenter Servern mit 5.5, 6.0, 6.5 oder 6.7 verwaltet werden.

## <a name="agentless-migration-vcenter-server-permissions"></a>vCenter Server-Berechtigungen für Migration ohne Agent

**Berechtigungen** | **Details**
--- | --- 
Datastore.Browse | Ermöglicht das Durchsuchen von VM-Protokolldateien zur Fehlerbehebung bei der Erstellung und Löschung von Momentaufnahmen.
Datastore.LowLevelFileOperations | Ermöglicht Lese-/Schreib-/Lösch-/Umbenennungsvorgänge im Datenspeicherbrowser, um Probleme bei der Erstellung und Löschung von Momentaufnahmen zu beheben.
VirtualMachine.Configuration.DiskChangeTracking | Aktivieren oder Deaktivieren der Änderungsverfolgung von VM-Laufwerken, um geänderte Datenblöcke zwischen Momentaufnahmen abzurufen.
VirtualMachine.Configuration.DiskLease | Ermöglicht Datenträger-Leasevorgängen für eine VM, damit der Datenträger mit dem VMware vSphere Virtual Disk Development Kit (VDDK) gelesen werden kann.
VirtualMachine.Provisioning.AllowReadOnlyDiskAccess | Ermöglicht das Öffnen eines Datenträgers auf einer VM, damit der Datenträger mit VDDK gelesen werden kann.
VirtualMachine.Provisioning.AllowVirtualMachineDownload  | Ermöglicht Lesevorgänge bei Dateien, die einer VM zugeordnet sind, um die Protokolle herunterzuladen und beim Auftreten von Fehlern diese zu beheben. 
VirtualMachine.SnapshotManagement.* | Ermöglicht Erstellen und Verwalten von VM-Momentaufnahmen zur Replikation. 
Virtual Machine.Interaction.Power Off | Ermöglicht das Ausschalten der VM während der Migration zu Azure.


## <a name="agentless-migration-vmware-vm-requirements"></a>VMware-VM-Anforderungen für die Migration ohne Agent

**Unterstützung** | **Details**
--- | ---
**Unterstützte Betriebssysteme** | [Windows-](https://support.microsoft.com/help/2721672/microsoft-server-software-support-for-microsoft-azure-virtual-machines) und [Linux-](https://docs.microsoft.com/azure/virtual-machines/linux/endorsed-distros)Betriebssysteme, die von Azure unterstützt werden, können durch Migration ohne Agent migriert werden.
**Erforderliche Änderungen für Azure** | Einige VMs erfordern möglicherweise Änderungen, damit sie in Azure ausgeführt werden können. Bei den folgenden Betriebssystemen führt Azure Migrate diese Änderungen automatisch durch:<br/> – Red Hat Enterprise Linux 6.5+, 7.0+<br/> – CentOS 6.5+, 7.0+</br> – SUSE Linux Enterprise Server 12 SP1+<br/> – Ubuntu 14.04 LTS, 16.04 LTS, 18.04 LTS<br/> – Debian 7,8<br/><br/> Bei anderen Betriebssystemen müssen Sie vor der Migration manuell Anpassungen vornehmen. Die entsprechenden Artikel enthalten eine Anleitung, wie dies zu tun ist.
**Linux-Start** | Wenn sich „/boot“ in einer dedizierten Partition befindet, sollte diese auf dem Betriebssystemdatenträger und nicht auf mehrere Datenträger verteilt vorhanden sein.<br/> Wenn „/boot“ Teil der Stammpartition (/) ist, sollte sich diese auf dem Betriebssystemdatenträger befinden und sich nicht auf andere Datenträger erstrecken.
**UEFI-Start** | Die Migration von VMs mit UEFI-Start wird nicht unterstützt.
**Verschlüsselte Datenträger/Volumes** | Die Migration von VMs mit verschlüsselten Datenträgern/Volumes wird nicht unterstützt.
**RDM-Datenträger/Pass-Through-Datenträger** | Wenn VMs über RDM-Datenträger oder Pass-Through-Datenträger verfügen, werden diese nicht in Azure repliziert.
**NFS** | NFS-Volumes, die als Volumes auf den VMs bereitgestellt sind, werden nicht repliziert.
**Zieldatenträger** | VMs können nur auf verwaltete Datenträger (HHD Standard, SSD Premium) in Azure migriert werden.


## <a name="agentless-migration-appliance-requirements"></a>Appliance-Anforderungen für Migration ohne Agent


**Unterstützung** | **Details**
--- | ---
**ESXi** | Die Appliance-VM muss auf einem ESXi-Host mit Version 5.5 oder höher bereitgestellt werden.
**Azure Migrate-Projekt** | Ein Gerät kann einem einzelnen Projekt zugeordnet werden.
**vCenter Server** | Ein Gerät kann bis zu 10.000 VMware-VMs auf einem vCenter Server ermitteln.<br/> Ein Gerät kann sich mit einem vCenter Server verbinden.
**VDDK** | Wenn Sie eine Migration ohne Agent mit Azure Migrate-Servermigration durchführen, muss das VMware vSphere Virtual Disk Development Kit (VDDK) auf der Appliance-VM installiert sein.

## <a name="agentless-migration-url-access-requirements"></a>URL-Zugriffsanforderungen für Migration ohne Agent

Das Azure Migrate-Gerät muss mit dem Internet verbunden sein.

- Wenn Sie das Gerät bereitstellen, führt Azure Migrate eine Verbindungsüberwachung zu den in der folgenden Tabelle zusammengefassten URLs durch.
- Sollten Sie einen URL-basierten Firewallproxy verwenden, lassen Sie den Zugriff auf diese URLs zu und stellen Sie sicher, dass der Proxy alle CNAME-Einträge auflöst, die er beim Durchsuchen der URLs empfängt.

**URL** | **Details**  
--- | --- 
*.portal.azure.com | Navigieren Sie im Azure-Portal zu Azure Migrate.
*.windows.net | Melden Sie sich bei Ihrem Azure-Abonnement an.
*.microsoftonline.com | Erstellen Sie Active Directory-Anwendungen, damit die Appliance mit dem Azure Migrate-Service kommunizieren kann.
management.azure.com | Erstellen Sie Active Directory-Anwendungen, damit die Appliance mit dem Azure Migrate-Service kommunizieren kann.
dc.services.visualstudio.com | Laden Sie Anwendungsprotokolle hoch, die für die interne Überwachung verwendet werden.
*.vault.azure.net | Verwalten von Geheimnissen in Azure Key Vault.
*.servicebus.windows.net | Kommunikation zwischen der Appliance und dem Azure Migrate-Dienst.
*.discoverysrv.windowsazure.com<br/> *.migration.windowsazure.com<br/> *.hypervrecoverymanager.windowsazure.com | Verbindung mit Azure Migrate-Dienst-URLs herstellen.
*.blob.core.windows.net | Hochladen von Daten in Speicherkonten.


## <a name="agentless-migration-port-requirements"></a>Portanforderungen für Migration ohne Agent

**Device** | **Connection**
--- | --- 
Appliance | Ausgehender TCP-Port 3389 zum Hochladen von replizierten Daten in Azure und zur Kommunikation mit Azure Migrate für die Replikation und die Migration.
vCenter-Server | Eingehende Verbindungen auf TCP-Port 443 zum Orchestrieren der Replikation durch die Appliance – Erstellen von Momentaufnahmen, Kopieren von Daten, freigeben von Momentaufnahmen.
vSphere/EXSI-Host | Eingehend auf TCP-Port 902 zum Replizieren der Daten aus den Momentaufnahmen durch die Appliance. 


## <a name="agent-based-migration-vmware-server-requirements"></a>VMware-Serveranforderungen für die Migration mit Agent

Diese Tabelle fasst die Unterstützung und Einschränkungen der Bewertung für VMware-Virtualisierungsserver zusammen.

**Unterstützung** | **Details**
--- | ---
**vCenter server/ESXI** | VMware-VMs, die Sie migrieren, müssen von einem oder mehreren vCenter-Servern mit 5.5, 6.0, 6.5 oder 6.7 oder auf einem ESXI-Host mit vSphere Version 5.5, 6.0, 6.5 oder 6.7 verwaltet werden.

### <a name="agent-based-migration-vcenter-server-permissions"></a>vCenter Server-Berechtigungen für Migration mit Agent

**Berechtigungen** | **Details**
--- | --- 
Datastore.AllocateSpace | Erlaubt die Speicherplatzzuweisung auf einem Datenspeicher für eine VM, eine Momentaufnahme, einen Klon oder einen virtuellen Datenträger. 
Datastore.Browse | Ermöglicht das Durchsuchen von VM-Protokolldateien zur Fehlerbehebung bei der Erstellung und Löschung von Momentaufnahmen.
Datastore.LowLevelFileOperations | Ermöglicht Lese-, Schreibe-, Lösch- und Umbenennungsvorgänge im Datenspeicherbrowser, um Probleme bei der Erstellung und Löschung von Momentaufnahmen zu beheben.
Datastore.UpdateVirtualMachineFiles | Ermöglicht die Aktualisierung der Pfade zu VM-Dateien in einem Datenspeicher, nachdem der Datenspeicher mit einer neuen Signatur versehen wurde.
Network.AssignNetwork | Ermöglicht die Zuordnung eines Netzwerks zu einer VM-Ressource.
AssignVirtualMachineToResourcePool | Ermöglicht die Zuordnung einer VM zu einem Ressourcenpool.
Resource.MigratePoweredOffVirtualMachine | Ermöglicht die Migration einer abgeschalteten VM zu einem anderen Ressourcenpool oder Host.
Resource.MigratePoweredOnVirtualMachine | Ermöglicht die Migration einer eingeschalteten VM mit vMotion auf einen anderen Ressourcenpool oder Host.
Tasks.CreateTask | Ermöglicht einer Erweiterung, eine benutzerdefinierte Aufgabe zu erstellen.
Tasks.UpdateTask | Ermöglicht einer Erweiterung, eine benutzerdefinierte Aufgabe zu aktualisieren.
VirtualMachine.Configuration. | Ermöglicht die Konfiguration von VM-Optionen und -Geräten.
Virtual Machine.Interaction.AnswerQuestion | Ermöglicht die Behebung von Problemen mit VM-Statusübergängen oder Laufzeitfehlern.
Virtual Machine.Interaction.DeviceConnection | Ermöglicht das Ändern des Verbindungszustands der trennbaren virtuellen Geräte einer VM.
Virtual Machine.Interaction.ConfigureCDMedia | Ermöglicht die Konfiguration eines virtuellen DVD- oder CD-ROM-Geräts.
Virtual Machine.Interaction.ConfigureFloppyMedia | Ermöglicht die Konfiguration eines virtuellen Floppy-Gerätes.
Virtual Machine.Interaction.PowerOff | Ermöglicht das Ausschalten der VM während der Migration nach Azure.
Virtual Machine.Interaction.PowerOn | Ermöglicht das Einschalten einer abgeschalteten VM und die Wiederaufnahme einer angehaltenen VM.
Virtual Machine.Interaction.VMwareToolsInstall | Ermöglicht das Einbinden und Aufhebens des VMware Tools- CD-Installationsprogramms als CD-ROM für das Gastbetriebssystem.
VirtualMachine.Inventory.CreateNew | Ermöglicht die Erstellung einer VM und die Zuweisung der benötigten Ressourcen.
VirtualMachine.Inventory.Register | Ermöglicht das Hinzufügen einer vorhandenen VM zu einem vCenter-Server- oder Host-Inventar.
VirtualMachine.Inventory.Unregister | Ermöglicht das Aufheben der Registrierung einer VMe von einem vCenter Server oder Host-Inventar.
VirtualMachine.Provisioning.AllowVirtualMachineFilesUpload | Ermöglicht Schreibvorgänge bei Dateien, die einer VM zugeordnet sind, einschließlich vmx, Festplatten, Protokollen und nvram.
VirtualMachine.Provisioning.AllowVirtualMachineDownload | Ermöglicht Lesevorgänge für Dateien, die mit einer VM verknüpft sind, um die Protokolle zur Fehlerbehebung herunterzuladen.
VirtualMachine.SnapshotManagement.RemoveSnapshot | Ermöglicht das Entfernen einer Momentaufnahme aus dem Momentaufnahmeverlauf.

## <a name="agent-based-migration-replication-appliance-requirements"></a>Anforderungen an die Replikations-Appliance für Migration mit Agent

Die Anforderungen an die [Replikations-Appliance](migrate-replication-appliance.md) für die agentenbasierte Migration von VMware VMs und physischen Servern mit Azure Migrate-Servermigration sind in der Tabelle zusammengefasst.

> [!NOTE]
> Wenn Sie die Replikations-Appliance mit der OVA-Vorlage im Azure Migrate-Hub einrichten, läuft die Appliance unter Windows Server 2016 und erfüllt die Support-Anforderungen. Wenn Sie die Replikations-Appliance manuell auf einem physischen Server einrichten, stellen Sie sicher, dass sie den Anforderungen entspricht.



**Komponente** | **Anforderung** 
--- | ---
 | **VMware settings** (VMware VM-Appliance)
**PowerCLI** | [PowerCLI Version 6.0](https://my.vmware.com/web/vmware/details?productId=491&downloadGroup=PCLI600R1) sollte installiert werden, wenn die Replikations-Appliance auf einer VMware-VM ausgeführt wird.
**NIC-Typ** | VMXNET3 (wenn es sich bei der Appliance um eine VMware-VM handelt)
 | **Hardwareeinstellungen** 
CPU-Kerne | 8 
RAM | 16 GB
Anzahl der Datenträger | Drei: Betriebssystem-Datenträger, Prozessservercache-Datenträger, Aufbewahrungslaufwerk.
Freier Speicherplatz (Cache) | 600 GB
Freier Speicherplatz (Aufbewahrungslaufwerk) | 600 GB
**Softwareeinstellungen** | 
Betriebssystem | Windows Server 2016 oder Windows Server 2012 R2
Gebietsschema des Betriebssystems | Englisch (en-us)
TLS | TLS 1.2 sollte aktiviert sein.
.NET Framework | NET Framework 4.6 oder höher sollte auf dem Computer installiert sein (mit aktivierter starker Kryptographie).
MySQL | MySQL sollte auf der Appliance installiert sein.<br/> MySQL sollte installiert sein. Sie können es manuell installieren oder bei der Bereitstellung der Appliance durch Site Recovery installieren lassen. 
Andere Apps | Sie sollten keine anderen Anwendungen auf der Replikations-Appliance ausführen.
Windows Server-Rollen | Aktivieren Sie die folgenden Rollen nicht: <br> - Active Directory Domain Services <br>- Internetinformationsdienste <br> - Hyper-V 
Gruppenrichtlinien | Aktivieren Sie die folgenden Gruppenrichtlinien nicht: <br> - Zugriff auf Eingabeaufforderung verhindern <br> - Zugriff auf Programme zum Bearbeiten der Registrierung verhindern <br> - Vertrauenslogik für Dateianlagen <br> - Skriptausführung aktivieren <br> [Weitere Informationen](https://technet.microsoft.com/library/gg176671(v=ws.10).aspx)
IIS | - Keine bereits vorhandene Standardwebsite <br> - Keine bereits vorhandene Website/Anwendung sollte an Port 443 lauschen <br>- Die [anonyme Authentifizierung](https://technet.microsoft.com/library/cc731244(v=ws.10).aspx) ist aktiviert. <br> - Aktivieren der Einstellung [FastCGI](https://technet.microsoft.com/library/cc753077(v=ws.10).aspx) 
**Netzwerkeinstellungen** | 
Art der IP-Adresse | statischen 
Ports | 443 (Steuerkanalorchestrierung)<br>9443 (Datentransport) 
NIC-Typ | VMXNET3 

### <a name="replication-appliance-url-access"></a>URL-Adresse der Replikations-Appliance

Die Replikations-Appliance benötigt Zugriff auf diese URLs.

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
https:\//login.microsoftonline.com <br/> https:\//secure.aadcdn.microsoftonline-p.com <br/> https:\//login.live.com <br/> https:\//graph.windows.net <br/> https:\//login.windows.net <br/> https:\//www.live.com <br/> https:\//www.microsoft.com  | Beim OVF-Setup wird Zugriff auf diese URLs benötigt. Die URLs werden für die Zugriffssteuerung und die Identitätsverwaltung durch Azure Active Directory verwendet.
https:\//dev.mysql.com/get/Downloads/MySQLInstaller/mysql-installer-community-5.7.20.0.msi | So schließen Sie den MySQL-Download ab


#### <a name="mysql-installation-options"></a>MySQL-Installationsoptionen

MySQL kann auf der Replikations-Appliance mit einer dieser Methoden installiert werden.

**Installieren** | **Details**
--- | ---
Manuell herunterladen und installieren | Laden Sie die Anwendung MySQL manuell herunter, platzieren Sie sie im Ordner C:\Temp\ASRSetup, und installieren Sie sie manuell.<br/> Wenn Sie die Appliance einrichten, wird MySQL als bereits installiert angezeigt. 
Nicht online herunterladen | Platzieren Sie das MySQL-Installationsprogramm im Ordner C:\Temp\ASRSetup. Wenn Sie die Appliance installieren und auf MySQL herunterladen und installieren klicken, verwendet das Setup das von Ihnen hinzugefügte Installationsprogramm. 
Herunterladen aus Azure Migrate | Wenn Sie die Appliance installieren und zur Eingabe von MySQL aufgefordert werden, wählen Sie **„Herunterladen und installieren“** .



## <a name="agent-based-migration-vmware-vm-requirements"></a>VMware-VM-Anforderungen für die Migration mit Agent

**Unterstützung** | **Details**
--- | ---
**Computerworkload** | Azure Migrate unterstützt die Migration beliebiger Workloads (u. a. Active Directory- und SQL Server-Workloads), die auf einem unterstützten Computer ausgeführt werden.
**Betriebssysteme** | Die neuesten Informationen finden Sie in der [Betriebssystemunterstützung](../site-recovery/vmware-physical-azure-support-matrix.md#replicated-machines) für Site Recovery. Azure Migrate bietet identische VM-Betriebssystemunterstützung.
**Linux-Dateisystem/-Gastspeicher** | Die neuesten Informationen finden Sie in der [Linuxdateisystemunterstützung](../site-recovery/vmware-physical-azure-support-matrix.md#linux-file-systemsguest-storage) für Site Recovery. Azure Migrate hat die gleiche Linux-Dateisystemunterstützung.
**Netzwerk/Speicher** | Die neuesten Informationen finden Sie in den Bedingungen für [Netzwerk](../site-recovery/vmware-physical-azure-support-matrix.md#network) und [Speicher](../site-recovery/vmware-physical-azure-support-matrix.md#storage) für Site Recovery. Azure Migrate bietet identische Netzwerk-/Speicheranforderungen.
**Anforderungen für Azure** | Die neuesten Informationen finden Sie in den Bedingungen für [Azure-Netzwerk](../site-recovery/vmware-physical-azure-support-matrix.md#azure-vm-network-after-failover), [Speicher](../site-recovery/vmware-physical-azure-support-matrix.md#azure-storage) und [Compute](../site-recovery/vmware-physical-azure-support-matrix.md#azure-compute) für Site Recovery. Azure Migrate hat identische Anforderungen für die VMware-Migration.
**Mobilitätsdienst** | Der Mobilitätsdienstagent muss auf jeder VMs installiert sein, den Sie migrieren möchten.
**Zieldatenträger** | VMs können nur auf verwaltete Datenträger (HHD Standard, SSD Premium) in Azure migriert werden.

   

## <a name="agent-based-migration-url-access-requirements"></a>URL-Zugriffsanforderungen für Migration mit Agent

Der Mobilitätsdienst, der auf VMware-VMs läuft, benötigt eine Verbindung zum Internet.

- Wenn Sie den Mobilitätsdienst bereitstellen, führt er eine Verbindungsüberwachung zu den in der folgenden Tabelle zusammengefassten URLs durch.
- Sollten Sie einen URL-basierten Firewallproxy verwenden, lassen Sie den Zugriff auf diese URLs zu und stellen Sie sicher, dass der Proxy alle CNAME-Einträge auflöst, die er beim Durchsuchen der URLs empfängt.

**URL** | **Details**  
--- | --- 
*.portal.azure.com | Navigieren Sie im Azure-Portal zu Azure Migrate.
*.windows.net | Melden Sie sich bei Ihrem Azure-Abonnement an.
*.microsoftonline.com | Erstellen Sie Active Directory-Anwendungen, damit die Appliance mit dem Azure Migrate-Service kommunizieren kann. 
management.azure.com | Erstellen Sie Active Directory-Anwendungen, damit die Appliance mit dem Azure Migrate-Service kommunizieren kann.
dc.services.visualstudio.com | Laden Sie Anwendungsprotokolle hoch, die für die interne Überwachung verwendet werden.
*.vault.azure.net | Verwalten von Geheimnissen in Azure Key Vault.
*.servicebus.windows.net | Kommunikation zwischen der Appliance und dem Azure Migrate-Dienst.
*.discoverysrv.windowsazure.com <br/> *.migration.windowsazure.com <br/> *.hypervrecoverymanager.windowsazure.com | Verbindung mit Azure Migrate-Dienst-URLs herstellen.
*.blob.core.windows.net | Hochladen von Daten in Speicherkonten.

## <a name="agent-based-migration-port-requirements"></a>Portanforderungen für Migration mit Agent

**Device** | **Connection**
--- | --- 
VMs | Der Mobilitätsdienst auf VMs kommuniziert mit dem lokalen Konfigurationsserver über den Port HTTPS 443 für eingehenden Datenverkehr, um die Replikationsverwaltung auszuführen.<br/><br/> Virtuelle Computer senden Replikationsdaten an den Prozessserver (der auf dem Konfigurationsserver ausgeführt wird) über Port HTTPS 9443 für eingehenden Datenverkehr. Dieser Port kann geändert werden.
Replikationsappliance | Die Replikationsappliance orchestriert die Replikation mit Azure über Port HTTPS 443 für ausgehenden Datenverkehr.
Prozessserver | Der Prozessserver empfängt Replikationsdaten, optimiert und verschlüsselt sie und sendet sie über den ausgehenden Port 443 an den Azure-Speicher.<br/> Der Prozessserver wird standardmäßig auf der Replikations-Appliance ausgeführt.

## <a name="azure-vm-requirements"></a>Azure-VM-Anforderungen

Alle lokalen virtuellen Computer, die in Azure repliziert werden, müssen die in dieser Tabelle zusammengefassten Azure-VM-Anforderungen erfüllen. Wenn Site Recovery eine Voraussetzungsprüfung für die Replikation durchführt, ist diese nicht erfolgreich, wenn einige der Anforderungen nicht erfüllt sind.

**Komponente** | **Anforderungen** | **Details**
--- | --- | ---
Gastbetriebssystem | Überprüfen Sie unterstützte Betriebssysteme für [VMware-VMs mit Replikation ohne Agent](#agentless-migration-vmware-vm-requirements), und für [VMware-VMs mit Replikation mit Agent](#agent-based-migration-vmware-vm-requirements).<br/> Sie können alle Workloads, die auf einem unterstützten Betriebssystem ausgeführt werden, migrieren. | Beim Überprüfen tritt ein Fehler auf, wenn keine Unterstützung vorhanden ist.
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
Verbindung nach der Migration unter Windows | Zur Verbindung mit Azure-VMs, die nach der Migration unter Windows laufen:<br/> – Aktivieren Sie vor der Migration Sie RDP auf der lokalen VM. Stellen Sie sicher, dass TCP- und UDP-Regeln für das Profil **Öffentlich** hinzugefügt werden und dass RDP unter **Windows-Firewall** > **Zugelassene Apps** für alle Profile zugelassen ist.<br/> Aktivieren Sie das RDP bei Site-to-Site-VPN-Zugang RDP unter **Windows-Firewall** -> **Zugelassene Apps und Features** für **private und Domänennetzwerke**. Achten Sie außerdem darauf, dass die SAN-Richtlinie des Betriebssystems auf **OnlineAll** festgelegt ist. [Weitere Informationen](https://support.microsoft.com/kb/3031135) | 
Verbindung nach der Migration unter Linux | Zur Verbindung mit Azure VMs nach der Migration über SSH:<br/> Überprüfen Sie vor der Migration auf dem lokalen Computer, ob der Secure Shell-Dienst auf „Start“ gesetzt ist und ob die Firewallregeln eine SSH-Verbindung zulassen.<br/> Erlauben Sie nach dem Failover auf der Azure-VM in den Netzwerksicherheitsgruppen-Regeln eingehende Verbindungen am SSH-Port und für das Azure-Subnetz, mit dem der virtuelle Computer verbunden ist. Fügen Sie außerdem der VM eine öffentliche IP-Adresse hinzu. |  


## <a name="next-steps"></a>Nächste Schritte

[Vorbereiten von VMware](tutorial-prepare-vmware.md) auf die Bewertung und Migration.








