---
title: Agentenbasierte Migrationsarchitektur in Azure Migrate-Servermigration
description: Bietet eine Übersicht über die agentenbasierte VMware VM-Migration mit Azure Migrate-Servermigration.
author: rayne-wiselman
ms.service: azure-migrate
ms.topic: conceptual
ms.date: 07/10/2019
ms.author: raynew
ms.openlocfilehash: 21c779587842c976ba93d7fa592a91ee714bc55c
ms.sourcegitcommit: 47ce9ac1eb1561810b8e4242c45127f7b4a4aa1a
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 07/11/2019
ms.locfileid: "67810021"
---
# <a name="agent-based-migration-architecture"></a>Agent-basierte Migrationsarchitektur

Dieser Artikel gibt einen Überblick über die Architektur und Prozesse, die für die agentenbasierte Replikation mit dem Azure Migrate-Servermigrationstool verwendet werden.

[Azure Migrate](migrate-services-overview.md) ist ein zentraler Hub zum Nachverfolgen der Ermittlung, Bewertung und Migration Ihrer lokalen Apps und Workloads sowie von AWS/GCP-VM-Instanzen zu Azure. Der Hub stellt Azure Migrate-Tools für die Bewertung und Migration sowie Angebote von unabhängigen Drittanbietern (Independent Software Vendors, ISVs) bereit.

## <a name="agent-based-replication"></a>Agentenbasierte Replikation

Die agentenbasierte Replikation im Azure Migrate-Serverreplikationstool wird verwendet, um VMware-VMs und lokale physische Server nach Azure zu migrieren. Sie kann auch zur Migration anderer lokaler virtueller Server sowie privater und öffentlicher Cloud-VMs, einschließlich AWS-Instanzen und GCP-VMs, verwendet werden.

Für die VMware-Migration bietet das Tool Azure Migrate-Servermigration eine Reihe von Optionen:

- Migration mit agentenbasierter Replikation, wie in diesem Artikel beschrieben.
- Agentenlose Replikation, um VMs zu migrieren, ohne etwas darauf installieren zu müssen.

Erfahren Sie mehr über die [Auswahl einer Migrationsmethode für VMware](server-migrate-overview.md).

## <a name="server-migration-and-azure-site-recovery"></a>Servermigration und Azure Site Recovery

Azure Migrate-Servermigration ist ein Tool zum Migrieren lokaler und öffentlicher cloudbasierter Workloads zu Azure. Es ist für die Migration optimiert. Site Recovery ist ein Tool für die Notfallwiederherstellung. Azure-Servermigration und Site Recovery verfügen über einige gemeinsame Technologiekomponenten, die für die Datenreplikation verwendet werden, aber erfüllen unterschiedliche Zwecke.

## <a name="architectural-components"></a>Komponenten der Architektur

![Architecture](./media/agent-based-replication-architecture/architecture.png)

Die Tabelle fasst die Komponenten zusammen, die für die agentenbasierte Migration verwendet werden.

**Komponente** | **Details** | **Installation**
--- | --- | ---
**Replikationsappliance** | Die Replikationsappliance (Konfigurationsserver) ist ein lokaler Computer, der als Brücke zwischen der lokalen Umgebung und dem Azure Migrate-Servermigrationstool fungiert. Die Appliance erkennt das lokale VM-Inventar, sodass Azure-Servermigration die Replikation und Migration orchestrieren kann. Die Appliance verfügt über zwei Komponenten:<br/><br/> **Konfigurationsserver**: Verbindet sich mit Azure Migrate-Servermigration und koordiniert die Replikation.<br/> **Prozessserver** Führt die Datenreplikation durch. Er empfängt VM-Daten, komprimiert und verschlüsselt sie und sendet sie an das Azure-Abonnement. Dort schreibt Servermigration die Daten auf verwaltete Datenträger. | Standardmäßig wird der Prozessserver zusammen mit dem Konfigurationsserver auf der Replikationsappliance installiert.
**Mobilitätsdienst** | Der Mobilitätsdienst ist ein Agent, der auf jedem Computer installiert ist, den Sie replizieren und migrieren möchten. Es sendet Replikationsdaten von dem Computer an den Prozessserver. Es gibt eine Reihe von verschiedenen Mobilitätsdienstagents. | Installationsdateien für den Mobilitätsdienst befinden sich auf der Replikationsappliance. Sie laden den benötigten Agent herunter und installieren ihn entsprechend dem Betriebssystem und der Version des zu replizierenden Computers.

### <a name="mobility-service-installation"></a>Installation des Mobilitätsdiensts

Sie können die den Mobilitätsdienst mithilfe der folgenden Methoden bereitstellen:

- **Pushinstallation**: Der Mobilitätsdienst wird vom Prozessserver installiert, wenn Sie den Schutz für einen Computer aktivieren. 
- **Manuelle Installation**: Über die Benutzeroberfläche oder die Eingabeaufforderung können Sie den Mobilitätsdienst manuell auf jedem Computer installieren.

Der Mobilitätsdienst kommuniziert mit der Replikationsappliance und den replizierten Computern. Wenn Sie Antivirensoftware auf der Replikationsappliance, den Prozessservern oder den replizierten Computern ausführen, sollten die folgenden Ordner von der Überprüfung ausgeschlossen werden:


- C:\Programme\Microsoft Azure Recovery Services Agent
- C:\ProgramData\ASR
- C:\ProgramData\ASRLogs
- C:\ProgramData\ASRSetupLogs
- C:\ProgramData\LogUploadServiceLogs
- C:\ProgramData\Microsoft Azure Site Recovery
- C:\Programme (x86)\Microsoft Azure Site Recovery
- C:\ProgramData\ASR\agent (auf Windows-Computern mit installiertem Mobilitätsdienst)

## <a name="replication-process"></a>Replikationsprozess

1. Wenn Sie die Replikation für eine VM aktivieren, beginnt die erste Replikation zu Azure.
2. Während der ersten Replikation liest der Mobilitätsdienst Daten von den Datenträgern des Computers und sendet sie an den Prozessserver.
3. Diese Daten werden verwendet, um eine Kopie des Datenträgers in Ihrem Azure-Abonnement zu speichern. 
4. Die Replikation von Deltaänderungen in Azure beginnt, nachdem die erste Replikation abgeschlossen wurde. Die Replikation findet auf Blockebene nahezu fortlaufend statt.
4. Der Mobilitätsdienst fängt Schreibzugriffe auf den VM-Datenträgerspeicher ab, indem er in das Speichersubsystem des Betriebssystems integriert wird. Dieses Verfahren vermeidet Festplatten-E/A-Vorgänge auf dem Replikationscomputer für die inkrementelle Replikation. 
5. Nachverfolgte Änderungen für einen Computer werden an den Prozessserver über den eingehenden Port HTTPS 9443 gesendet. Dieser Port kann geändert werden. Der Prozessserver komprimiert und verschlüsselt sie und sendet sie an Azure. 

## <a name="ports"></a>Ports

**Device** | **Connection**
--- | --- 
VMs | Der Mobilitätsdienst auf VMs kommuniziert mit der lokalen Replikationsappliance über den Port HTTPS 443 für eingehenden Datenverkehr, um die Replikationsverwaltung auszuführen.<br/><br/> Virtuelle Computer senden Replikationsdaten an den Prozessserver (der standardmäßig auf der Replikationsappliance ausgeführt wird) über Port HTTPS 9443 für eingehenden Datenverkehr. Dieser Port kann geändert werden.
Replikationsappliance | Die Replikationsappliance orchestriert die Replikation mit Azure über Port HTTPS 443 für ausgehenden Datenverkehr.
Prozessserver | Der Prozessserver empfängt Replikationsdaten, optimiert und verschlüsselt sie und sendet sie über den ausgehenden Port 443 an den Azure-Speicher.


## <a name="performance-and-scaling"></a>Leistung und Skalierung

Standardmäßig wird eine einzelne Replikationsappliance bereitgestellt, die sowohl den Konfigurationsserver als auch den Prozessserver ausführt. Wenn Sie nur wenige Computer replizieren, ist diese Bereitstellung ausreichend. Wenn Sie jedoch Hunderte von Maschinen replizieren und migrieren, ist ein einzelner Prozessserver möglicherweise nicht in der Lage, den gesamten Replikationsdatenverkehr zu bewältigen. In diesem Fall können Sie zusätzliche Prozessserver für die horizontale Skalierung einsetzen.

### <a name="site-recovery-deployment-planner-for-vmware"></a>Site Recovery-Bereitstellungsplaner für VMware

Wenn Sie VMware VMs replizieren, können Sie den [Site Recovery-Bereitstellungsplaner](../site-recovery/site-recovery-deployment-planner.md) für VMware verwenden, um die Leistungsanforderungen, einschließlich der täglichen Datenänderungsrate und der benötigten Prozessserver, zu ermitteln.

### <a name="replication-appliance-capacity"></a>Kapazität der Replikationsappliance

Die Werte in dieser Tabelle können verwendet werden, um herauszufinden, ob Sie in Ihrer Bereitstellung einen zusätzlichen Prozessserver benötigen.

- Wenn Ihre tägliche Änderungsrate (Churn Rate) über 2 TB liegt, setzen Sie einen zusätzlichen Prozessserver ein.
- Wenn Sie mehr als 200 Computer replizieren, setzen Sie eine zusätzliche Replikationsappliance ein.

**CPU** | **Memory** | **Freier Speicherplatz für Datenzwischenspeicherung** | **Datenänderungsrate** | **Replikationslimits**
--- | --- | --- | --- | ---
8 vCPUs (2 Sockets * 4 Kerne \@ 2,5 GHz) | 16 GB | 300 GB | 500 GB oder weniger | < 100 Computer 
12 vCPUs (2 Sockets * 6 Kerne \@ 2,5 GHz) | 18 GB | 600 GB | 501 GB bis 1 TB | 100-150 Computer.
16 vCPUs (2 Sockets * 8 Kerne \@ 2,5 GHz) | 32 G1 |  1 TB | 1 TB bis 2 TB | 151-200 Computer.

### <a name="scale-out-process-server-sizing"></a>Horizontal skalierter Prozessserver

Wenn Sie einen horizontal skalierten Prozessserver bereitstellen müssen, kann Ihnen diese Tabelle helfen, die Servergröße zu ermitteln.

**Prozessserver** | **Freier Speicherplatz für Datenzwischenspeicherung** | **Datenänderungsrate** | **Replikationslimits**
--- | --- | --- | --- 
4 vCPUs (2 Sockets * 2 Kerne \@ 2,5 GHz), 8 GB Arbeitsspeicher | 300 GB | 250 GB oder weniger | Bis zu 85 Computer 
8 vCPUs (2 Sockets * 4 Kerne \@ 2,5 GHz), 12 GB Arbeitsspeicher | 600 GB | 251 GB bis 1 TB    | 86-150 Computer.
12 vCPUs (2 Sockets * 6 Kerne \@ 2,5 GHz), 24 GB Arbeitsspeicher | 1 TB | 1–2 TB | 151-225 Computer.

## <a name="control-upload-throughput"></a>Steuern des Uploaddurchsatzes

Sie können die Bandbreite, die zum Hochladen von Daten in Azure verwendet wird, auf jedem Hyper-V-Host einschränken. Aber Vorsicht! Wenn Sie die Werte zu niedrig festlegen, wirkt sich dies negativ auf die Replikation aus und verzögert die Migration.


1. Melden Sie sich beim Hyper-V-Host oder -Clusterknoten an.
2. Führen Sie **C:\Programme\Microsoft Azure Recovery Services Agent\bin\wabadmin.msc** aus, um das MMC-Snap-In Windows Azure Backup zu öffnen.
3. Wählen Sie im Snap-In **Eigenschaften ändern** aus.
4. Wählen Sie unter **Drosselung** die Option **Internet-Bandbreiteneinschränkung für Sicherungsvorgänge aktivieren** aus. Legen Sie die Grenzwerte für Arbeitsstunden und arbeitsfreie Stunden fest. Der gültige Bereich reicht von 512 KBit/s bis 1,023 MBit/s.
I

### <a name="influence-upload-efficiency"></a>Beeinflussen der Effizienz beim Hochladen

Wenn Sie über freie Bandbreite für die Replikation verfügen und Uploads beschleunigen möchten, können Sie die Anzahl der für die Uploadtask reservierten Threads wie folgt erhöhen:

1. Öffnen Sie die Registrierung mit „Regedit“.
2. Navigieren Sie zum Schüssel „HKEY_LOCAL_MACHINE\SOFTWARE\Microsoft\Windows Azure Backup\Replication\UploadThreadsPerVM“.
3. Erhöhen Sie den Wert für die Anzahl der Threads, die für den Datenupload für jede replizierende VM verwendet werden. Der Standardwert beträgt 4, der Maximalwert 32. 

## <a name="next-steps"></a>Nächste Schritte

Testen Sie die [VMware-VM-Migration](tutorial-migrate-vmware-agent.md) mit Agent über Azure Migrate-Servermigration.
