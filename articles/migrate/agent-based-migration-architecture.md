---
title: Agent-basierte Migrationsarchitektur bei der Azure Migrate-Servermigration
description: Bietet eine Übersicht über die Agent-basierte VMware VM-Migration mit der Azure Migrate-Servermigration.
author: rayne-wiselman
ms.service: azure-migrate
ms.topic: conceptual
ms.date: 07/10/2019
ms.author: raynew
ms.openlocfilehash: f5ad3aa0fc51f47942750d3745ffef1d6e4a087d
ms.sourcegitcommit: 2aefdf92db8950ff02c94d8b0535bf4096021b11
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 09/03/2019
ms.locfileid: "70232587"
---
# <a name="agent-based-migration-architecture"></a>Agent-basierte Migrationsarchitektur

Dieser Artikel gibt einen Überblick über die Architektur und die Prozesse, die für die Agent-basierte Replikation mit dem Azure Migrate-Servermigrationstool verwendet werden.

[Azure Migrate](migrate-services-overview.md) ist ein zentraler Hub zum Nachverfolgen von Ermittlung und Bewertung sowie der Migration von lokalen Apps und Workloads sowie AWS/GCP-VM-Instanzen zu Azure. Der Hub stellt Azure Migrate-Tools für die Bewertung und Migration sowie Angebote von unabhängigen Drittanbietern (Independent Software Vendors, ISVs) bereit.

## <a name="agent-based-replication"></a>Agent-basierte Replikation

Die Agent-basierte Replikation im Azure Migrate-Serverreplikationstool wird verwendet, um VMware-VMs und lokale physische Server zu Azure zu migrieren. Sie kann auch zur Migration anderer lokaler virtueller Server sowie virtueller Computer (VMs) in privaten und öffentlichen Clouds (einschließlich AWS-Instanzen und GCP-VMs) verwendet werden.

Für die VMware-Migration bietet das Azure Migrate-Servermigrationstool eine Reihe von Optionen:

- Migration mit Agent-basierter Replikation, wie in diesem Artikel beschrieben.
- Replikation ohne Agent für die Migration von VMs, ohne etwas darauf installieren zu müssen.

Erfahren Sie mehr über die [Auswahl einer Migrationsmethode für VMware](server-migrate-overview.md).

## <a name="server-migration-and-azure-site-recovery"></a>Servermigration und Azure Site Recovery

Die Azure Migrate-Servermigration ist ein Tool zum Migrieren lokaler und öffentlicher cloudbasierter Workloads zu Azure. Es ist für die Migration optimiert. Site Recovery ist ein Tool für die Notfallwiederherstellung. Die Azure-Servermigration und Site Recovery verfügen über einige gemeinsame Technologiekomponenten, die für die Datenreplikation verwendet werden, jedoch unterschiedliche Zwecke erfüllen.

## <a name="architectural-components"></a>Komponenten der Architektur

![Architektur](./media/agent-based-replication-architecture/architecture.png)

In der Tabelle sind die Komponenten zusammengefasst, die für die Agent-basierte Migration verwendet werden.

**Komponente** | **Details** | **Installation**
--- | --- | ---
**Replikationsappliance** | Die Replikationsappliance (Konfigurationsserver) ist ein lokaler Computer, der als Brücke zwischen der lokalen Umgebung und dem Azure Migrate-Servermigrationstool fungiert. Die Appliance erkennt den lokalen VM-Bestand, sodass die Azure-Servermigration die Replikation und Migration orchestrieren kann. Die Appliance verfügt über zwei Komponenten:<br/><br/> **Konfigurationsserver**: Stellt eine Verbindung mit der Azure Migrate-Servermigration her und koordiniert die Replikation.<br/> **Prozessserver** Führt die Datenreplikation durch. Er empfängt VM-Daten, komprimiert und verschlüsselt sie und sendet sie an das Azure-Abonnement. Dort schreibt die Servermigration die Daten auf verwaltete Datenträger. | Standardmäßig wird der Prozessserver zusammen mit dem Konfigurationsserver auf der Replikationsappliance installiert.
**Mobilitätsdienst** | Mobility Service ist ein Agent, der auf jedem Computer installiert ist, den Sie replizieren und migrieren möchten. Er sendet Replikationsdaten vom Computer an den Prozessserver. Es gibt eine Reihe verschiedener Mobility Service-Agents. | Die Installationsdateien für den Mobility Service-Agent befinden sich auf der Replikationsappliance. Sie laden entsprechend dem Betriebssystem und der Version des zu replizierenden Computers den benötigten Agent herunter und installieren ihn.

### <a name="mobility-service-installation"></a>Installation des Mobilitätsdiensts

Sie können die den Mobilitätsdienst mithilfe der folgenden Methoden bereitstellen:

- **Pushinstallation**: Mobility Service wird vom Prozessserver installiert, wenn Sie den Schutz für einen Computer aktivieren. 
- **Manuelle Installation**: Über die Benutzeroberfläche oder die Eingabeaufforderung können Sie Mobility Service manuell auf jedem Computer installieren.

Mobility Service kommuniziert mit der Replikationsappliance und den replizierten Computern. Wenn Sie Antivirensoftware auf der Replikationsappliance, den Prozessservern oder den replizierten Computern ausführen, sollten die folgenden Ordner von der Überprüfung ausgeschlossen werden:


- C:\Programme\Microsoft Azure Recovery Services Agent
- C:\ProgramData\ASR
- C:\ProgramData\ASRLogs
- C:\ProgramData\ASRSetupLogs
- C:\ProgramData\LogUploadServiceLogs
- C:\ProgramData\Microsoft Azure Site Recovery
- C:\Programme (x86)\Microsoft Azure Site Recovery
- C:\ProgramData\ASR\agent (auf Windows-Computern mit installiertem Mobility Service)

## <a name="replication-process"></a>Replikationsprozess

1. Wenn Sie die Replikation für einen virtuellen Computer aktivieren, beginnt die erste Replikation nach Azure.
2. Während der ersten Replikation liest Mobility Service Daten von den Datenträgern des Computers und sendet sie an den Prozessserver.
3. Diese Daten werden verwendet, um eine Kopie des Datenträgers in Ihrem Azure-Abonnement zu speichern. 
4. Die Replikation von Deltaänderungen in Azure beginnt, nachdem die erste Replikation abgeschlossen wurde. Die Replikation findet auf Blockebene und nahezu fortlaufend statt.
4. Mobility Service fängt Schreibzugriffe auf den VM-Datenträgerspeicher ab, indem er in das Speichersubsystem des Betriebssystems integriert wird. Dieses Verfahren vermeidet Festplatten-E/A-Vorgänge auf dem Replikationscomputer bei der inkrementellen Replikation. 
5. Nachverfolgte Änderungen für einen Computer werden über den eingehenden Port HTTPS 9443 an den Prozessserver gesendet. Dieser Port kann geändert werden. Der Prozessserver komprimiert und verschlüsselt sie und sendet sie an Azure. 

## <a name="ports"></a>Ports

**Device** | **Connection**
--- | --- 
VMs | Der Mobility Service-Agent auf den virtuellen Computern kommuniziert mit der lokalen Replikationsappliance über den eingehenden Port HTTPS 443, um die Replikationsverwaltung auszuführen.<br/><br/> Virtuelle Computer senden über den eingehenden Port HTTPS 9443 Replikationsdaten an den Prozessserver (der standardmäßig auf der Replikationsappliance ausgeführt wird). Dieser Port kann geändert werden.
Replikationsappliance | Die Replikationsappliance orchestriert die Replikation mit Azure über Port HTTPS 443 für ausgehenden Datenverkehr.
Prozessserver | Der Prozessserver empfängt Replikationsdaten, optimiert und verschlüsselt sie und sendet sie über den ausgehenden Port 443 an den Azure-Speicher.


## <a name="performance-and-scaling"></a>Leistung und Skalierung

Standardmäßig wird eine einzelne Replikationsappliance bereitgestellt, die sowohl den Konfigurationsserver als auch den Prozessserver ausführt. Wenn Sie nur wenige Computer replizieren, ist diese Bereitstellung ausreichend. Wenn Sie jedoch Hunderte von Maschinen replizieren und migrieren, ist ein einzelner Prozessserver möglicherweise nicht in der Lage, den gesamten Replikationsdatenverkehr zu bewältigen. In diesem Fall können Sie zusätzliche Prozessserver für die horizontale Skalierung einsetzen.

### <a name="site-recovery-deployment-planner-for-vmware"></a>Site Recovery-Bereitstellungsplaner für VMware

Wenn Sie VMware-VMs replizieren, können Sie den [Site Recovery-Bereitstellungsplaner](../site-recovery/site-recovery-deployment-planner.md) für VMware verwenden, um die Leistungsanforderungen, einschließlich der täglichen Datenänderungsrate und der benötigten Prozessserver, zu ermitteln.

### <a name="replication-appliance-capacity"></a>Kapazität der Replikationsappliance

Mit den Werten in dieser Tabelle können Sie herausfinden, ob Sie in Ihrer Bereitstellung einen zusätzlichen Prozessserver benötigen.

- Wenn Ihre tägliche Änderungsrate (Churn Rate) über 2 TB liegt, setzen Sie einen zusätzlichen Prozessserver ein.
- Wenn Sie mehr als 200 Computer replizieren, setzen Sie eine zusätzliche Replikationsappliance ein.

**CPU** | **Memory** | **Freier Speicherplatz für Datenzwischenspeicherung** | **Datenänderungsrate** | **Replikationsgrenzwerte**
--- | --- | --- | --- | ---
8 vCPUs (2 Sockets * 4 Kerne \@ 2,5 GHz) | 16 GB | 300 GB | 500 GB oder weniger | < 100 Computer 
12 vCPUs (2 Sockets * 6 Kerne \@ 2,5 GHz) | 18 GB | 600 GB | 501 GB bis 1 TB | 100-150 Computer.
16 vCPUs (2 Sockets * 8 Kerne \@ 2,5 GHz) | 32 G1 |  1 TB | 1 TB bis 2 TB | 151-200 Computer.

### <a name="scale-out-process-server-sizing"></a>Dimensionierung horizontal skalierter Prozessserver

Wenn Sie einen horizontal skalierten Prozessserver bereitstellen müssen, können Sie mithilfe dieser Tabelle die Servergröße ermitteln.

**Prozessserver** | **Freier Speicherplatz für Datenzwischenspeicherung** | **Datenänderungsrate** | **Replikationsgrenzwerte**
--- | --- | --- | --- 
4 vCPUs (2 Sockets * 2 Kerne \@ 2,5 GHz), 8 GB Arbeitsspeicher | 300 GB | 250 GB oder weniger | Bis zu 85 Computer 
8 vCPUs (2 Sockets * 4 Kerne \@ 2,5 GHz), 12 GB Arbeitsspeicher | 600 GB | 251 GB bis 1 TB    | 86-150 Computer.
12 vCPUs (2 Sockets * 6 Kerne \@ 2,5 GHz), 24 GB Arbeitsspeicher | 1 TB | 1–2 TB | 151-225 Computer.

## <a name="control-upload-throughput"></a>Steuern des Uploaddurchsatzes


 VMware-Datenverkehr, der nach Azure repliziert wird, wird über einen speziellen Prozessserver geleitet. Durch Drosselung der Bandbreite auf den Computern, die als Prozessserver ausgeführt werden, können Sie den Upload-Durchsatz begrenzen. Sie können die Bandbreite mithilfe dieses Registrierungsschlüssels beeinflussen:

- Der Registrierungswert „HKEY_LOCAL_MACHINE\SOFTWARE\Microsoft\Windows Azure Backup\Replication\UploadThreadsPerVM“ gibt die Anzahl von Threads an, die für die Datenübertragung (erste Replikation oder Deltareplikation) eines Datenträgers verwendet werden. Bei einem höheren Wert wird die Netzwerkbandbreite für die Replikation erhöht. Der Standardwert ist 4. Der Höchstwert ist 32. Überwachen Sie den Datenverkehr, um den Wert zu optimieren.
- Außerdem können Sie die Bandbreite auf dem Computer, der als Prozessserver verwendet wird, wie folgt drosseln:

    1. Öffnen Sie das Azure Backup-MMC-Snap-In auf dem Computer, der als Prozessserver fungiert. Auf dem Desktop oder im Ordner „C:\Programme\Microsoft Azure Recovery Services Agent\bin\“ ist eine Verknüpfung verfügbar. 
    2. Wählen Sie im Snap-In **Eigenschaften ändern** aus.
    3. Wählen Sie unter **Drosselung** die Option **Internet-Bandbreiteneinschränkung für Sicherungsvorgänge aktivieren** aus. Legen Sie die Grenzwerte für Arbeitsstunden und arbeitsfreie Stunden fest. Der gültige Bereich reicht von 512 KBit/s bis 1,023 MBit/s.


## <a name="next-steps"></a>Nächste Schritte

Testen Sie die Agent-basierte [VMware-VM-Migration](tutorial-migrate-vmware-agent.md) mit der Azure Migrate-Servermigration.
