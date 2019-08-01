---
title: Azure Migrate-Architektur | Microsoft-Dokumentation
description: Enthält eine Übersicht über den Azure Migrate-Dienst.
author: rayne-wiselman
ms.service: azure-migrate
ms.topic: conceptual
ms.date: 07/01/2019
ms.author: raynew
ms.openlocfilehash: 16fbf8d3523ac2ab36447aa51a93a0bb5a9fad54
ms.sourcegitcommit: 47ce9ac1eb1561810b8e4242c45127f7b4a4aa1a
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 07/11/2019
ms.locfileid: "67810105"
---
# <a name="azure-migrate-architecture-and-processes"></a>Azure Migrate-Architektur und Prozesse

[Azure Migrate](migrate-overview.md) stellt einen Hub mit Tools bereit, die Ihnen dabei helfen, Apps, Infrastrukturen und Workloads zu ermitteln, zu bewerten und zu Microsoft Azure zu migrieren. Der Hub umfasst Azure Migrate-Tools sowie Angebote von unabhängigen Drittanbietern (Independent Software Vendors, ISVs). 

 Dieser Artikel fasst die Bewertungs- und Migrationsarchitektur und -prozesse für Azure Migrate-Serverbewertung und Azure Migrate-Servermigration zusammen.

## <a name="assessment-with-azure-migrate-server-assessment"></a>Bewertungen mit der Azure Migrate-Serverbewertung

Mit der Azure Migrate-Serverbewertung können Sie lokale VMware-VMs und Hyper-V-VMs für die Migration zu Azure ermitteln und bewerten. Die Bewertung funktioniert wie folgt:

1. **Vorbereiten für die Bewertung**: Sie richten lokal eine einfache Azure Migrate-Appliance als VMware-VM oder Hyper-V-VM ein und registrieren die Appliance bei Azure Migrate.
2. **VMs ermitteln**: Die Azure Migrate-Appliance benötigt Berechtigungen zum Ermitteln lokaler VMs. 
    - Auf ermittelten virtuellen Computern muss nichts installiert werden.
    - VM-Metadaten enthalten Informationen über Kerne, Arbeitsspeicher, Datenträger, Datenträgergrößen und Netzwerkadapter.
    - Zu den Leistungsdaten gehören CPU- und Arbeitsspeicherauslastung, Datenträger-IOPS, Datenträgerdurchsatz (MBit/s) und Netzwerkausgabe (MBit/s).
- **Sammeln und Bewerten von Computern**: Nach Abschluss der Ermittlung fassen Sie im Azure-Portal ermittelte VMs in Gruppen zusammen, die typischerweise aus VMs bestehen, die Sie gemeinsam migrieren möchten. Sie führen eine Bewertung für eine Gruppe aus.


## <a name="vmware-assessment"></a>VMware-Bewertung 

Das Diagramm fasst zusammen, wie VMware VM-Assessment mit Azure Migrate-Serverbewertung funktioniert.

![Architektur für die VMware-Bewertung](./media/migrate-architecture/sas-architecture-vmware.png)

Dieser Prozess verläuft wie folgt:

1. **Bereitstellen der Azure Migrate-Appliance**:

    a. Laden Sie die (OVA-)Vorlage aus dem Azure-Portal herunter.

    b. Importieren Sie sie auf den vCenter Server-Computer, um die VM zu erstellen.

    c. Stellen Sie eine Verbindung zu dieser VM her, konfigurieren Sie die Grundeinstellungen dafür und registrieren Sie sie bei Azure Migrate.

2. **Ermittlung starten**:

    a. Verbinden Sie sich mit der Collectoranwendung, die auf der Appliance läuft, um die Ermittlung zu starten.

    b. Die Appliance sendet kontinuierlich Metadaten und Leistungsdaten von den VMs an Azure.

    - Das Gerät ist immer mit dem Azure Migrate-Dienst verbunden.
    - Umgebungsänderungen während der kontinuierlichen Ermittlung werden erfasst. Zum Beispiel das Hinzufügen von VMs im Ermittlungsbereich oder das Hinzufügen von VMs oder NICs.

3. **Bewerten von Computern**:

    a. Nachdem die Ermittlung abgeschlossen ist, fassen Sie im Azure-Portal die ermittelten VMs in Gruppen zusammen.  Eine Gruppe besteht in der Regel aus VMs, die Sie gemeinsam migrieren möchten.

    b. Führen Sie für jede Gruppe eine Bewertung durch.

4. **Bewertung überprüfen**: 

    a. Nachdem die Bewertung abgeschlossen ist, finden Sie sie im Portal.

    b. Für weitere Analysen können Sie die Bewertung im Excel-Format herunterladen.



### <a name="vmware-ports"></a>VMware-Ports
Azure Migrate verwendet die folgenden Verbindungsports für VMware:

**Quelle** | **Ziel** | **Port** | **Details**
--- | --- | --- | ---
Azure Migrate-Appliance | Azure Migrate-Dienst | Target-TCP 443 | Senden Sie Metadaten und Leistungsdaten an Azure Migrate.
Azure Migrate-Appliance | vCenter Server | Target-TCP 443 | Verbinden Sie sich mit vCenter Server, um Metadaten und Leistungsdaten zu erhalten. 443 ist standardmäßig voreingestellt, kann aber geändert werden, wenn vCenter an einem anderen Port lauscht. 
RDP-Client | Azure Migrate-Appliance | Target-TCP3389 | RDP-Verbindung, um die Ermittlung durch die Appliance auszulösen.

### <a name="collected-vmware-metadata"></a>Gesammelte VMware-Metadaten

Die Appliance sendet Metadaten und Leistungsdaten von den VMs an Azure.

**Aktion** | **Details**
--- | ---
**Gesammelte Metadaten** | vCenter-VM-Name<br/> vCenter-VM-Pfad (Host-/Clusterordner)<br/> IP- und MAC-Adressen<br/> Betriebssystem<br/> Anzahl der Kerne/Datenträger/NICs<br/> Arbeitsspeicher und Datenträgergröße.
**Gesammelte Leistungsdaten** | CPU/Speicherauslastung<br/> Pro Datenträgerdaten (Datenträgerlese-/Schreibdurchsatz; Datenträgerlese-/Schreibvorgänge pro Sekunde)<br/> Nic-Daten (eingehend/ausgehend).<br/><br/> Leistungsdaten werden kontinuierlich gesammelt, nachdem sich die Appliance mit dem vCenter Server verbunden hat. Verlaufsdaten werden nicht gesammelt.

## <a name="hyper-v-assessment"></a>Hyper-V-Bewertung

Das Diagramm fasst zusammen, wie die Bewertung von Hyper-V mit der Azure Migrate-Serverbewertung funktioniert.

![Architektur für die Bewertung von Hyper-V](./media/migrate-architecture/sas-architecture-hyper-v.png)

Dieser Prozess verläuft wie folgt:

1. **Erstellen der Azure Migrate-Appliance**:

    a. Laden Sie die VM in komprimiertem Format vom Azure-Portal herunter.

    b. Importieren Sie sie auf einen Hyper-V-Host.

    c. Verbinden Sie sie mit der Appliance-VM. Konfigurieren Sie die Grundeinstellungen dafür und registrieren Sie sie bei Azure Migrate.

2. **Ermittlung starten**:

    a. Verbinden Sie sich mit der Azure Migrate-Appliance, um die Ermittlung zu starten. Auf ermittelten virtuellen Computern muss nichts installiert werden.

    b. Die Appliance sendet kontinuierlich VM-Metadaten und Leistungsdaten an Azure Migrate.

    - Das Gerät ist immer mit dem Azure Migrate-Dienst verbunden (mit CIM-Sitzungen).
    - Umgebungsänderungen während der kontinuierlichen Ermittlung werden erfasst. Zum Beispiel das Hinzufügen von VMs im Ermittlungsbereich oder das Hinzufügen von VMs oder NICs.


3. **Bewerten von Computern**:

    a. Nachdem die Ermittlung abgeschlossen ist, fassen Sie im Azure-Portal die ermittelten VMs in Gruppen zusammen.  Eine Gruppe besteht in der Regel aus VMs, die Sie gemeinsam migrieren möchten.

    b. Führen Sie für jede Gruppe eine Bewertung durch.

4. **Bewertung überprüfen**:

    a. Nachdem die Bewertung abgeschlossen ist, finden Sie sie im Portal.

    b. Für weitere Analysen können Sie die Bewertung im Excel-Format herunterladen.

### <a name="hyper-v-ports"></a>Hyper-V-Ports

Azure Migrate-Dienste verwenden die folgenden Verbindungsports für Hyper-V:

**Quelle** | **Ziel** | **Port** | **Details**
--- | --- | --- | ---
Azure Migrate-Appliance | Azure Migrate-Dienst | Target-TCP 443 | Senden Sie Metadaten und Leistungsdaten an Azure Migrate.
Azure Migrate-Appliance | Hyper-V-Host/Cluster | Target-Default WinRM ports-HTTP:5985; HTTPS:5986 | Verbinden Sie sich mit dem Host, um Metadaten und Leistungsdaten zu erhalten. CIM-Sitzung für Verbindungen
RDP-Client | Azure Migrate-Appliance | Target-TCP3389 | RDP-Verbindung, um die Ermittlung durch die Appliance auszulösen.

## <a name="collected-hyper-v-vm-metadata"></a>Gesammelte Hyper-V VM-Metadaten

Die Appliance sendet Metadaten und Leistungsdaten von den VMs an Azure.


**Aktion** | **Details**
--- | ---
**Gesammelte Metadaten** | Name des virtuellen Computers<br/> VM-Pfad (Host-/Clusterordner)<br/> IP- und MAC-Adressen<br/> Betriebssystem<br/> Anzahl der Kerne/Datenträger/NICs<br/> Arbeitsspeicher und Datenträgergröße<br/> Datenträger-IOPS, Datenträgerdurchsatz (MBps), Netzwerkausgabe (MBps)
**Gesammelte Leistungsdaten** |  CPU/Speicherauslastung<br/> Pro Datenträgerdaten (Datenträgerlese-/Schreibdurchsatz; Datenträgerlese-/Schreibvorgänge pro Sekunde)<br/> Nic-Daten (eingehend/ausgehend).<br/><br/> Leistungsdaten werden kontinuierlich gesammelt, nachdem sich die Appliance mit dem Hyper-V-Host verbunden hat. Verlaufsdaten werden nicht gesammelt.




## <a name="migration-with-azure-migrate-server-migration"></a>Migration mit Azure Migrate-Servermigration

Mit der Azure Migrate-Servermigration können Sie Folgendes nach Azure migrieren:

- Lokale VMware-VMs
- Lokale Hyper-V-VMs
- Lokale physische Server
- AWS-Windows-VM-Instanzen
- GCP-Windows-VM-Instanzen

Der Migrationsprozess unterscheidet sich geringfügig, je nachdem, was Sie migrieren.


## <a name="migrate-vmware-vms"></a>Migrieren von virtuellen VMware-Computern

Azure Migrate-Servermigration bietet zwei Methoden zum Migrieren von lokalen VMware-VMs:

- **Replikation ohne Agent**: Migrieren von VMs, ohne dass darauf Software installiert werden muss.
- **Agentbasierte Replikation**. Installieren eines Agents für die Replikation auf der VM.

Obwohl die Replikation ohne Agents im Hinblick auf die Bereitstellung einfacher ist, besteht derzeit eine Reihe von Einschränkungen. [Erfahren Sie mehr](server-migrate-overview.md) über diese Einschränkungen.
 

### <a name="agent-based-migration"></a>Agentenbasierte Migration

Die agentenbasierte Migration von VMware-VMs erfordert die Bereitstellung einer Reihe von Komponenten, wie in der Tabelle zusammengefasst.

**Komponente** | **Anforderung** | **Details**
--- | --- | ---
**Konfigurationsservercomputer** | Eine einzelne VMware-VM vor Ort, die von einer heruntergeladenen OVF-Vorlage bereitgestellt wird.<br/><br/> Auf dem Computer werden alle lokalen Site Recovery-Komponenten ausgeführt, einschließlich Konfigurationsserver und Prozessserver. | **Konfigurationsserver**: Koordiniert die Kommunikation zwischen der lokalen Umgebung und Azure und verwaltet die Datenreplikation.<br/><br/> **Prozessserver** Wird standardmäßig auf dem Konfigurationsserver installiert. Sie empfängt Replikationsdaten, optimiert sie durch Zwischenspeicherung, Komprimierung und Verschlüsselung und sendet sie an Azure. Der Prozessserver installiert auch Azure Site Recovery Mobility Service auf VMs und führt auf lokalen Computern eine automatische Ermittlung durch.
**Mobilitätsdienst** | Der Mobility Service muss auf jedem virtuellen VMware-Computer installiert sein, den Sie replizieren. | Es wird empfohlen, dass Sie die automatische Installation vom Prozessserver zulassen. Alternativ können Sie den Dienst manuell installieren oder eine automatisierte Bereitstellungsmethode wie System Center Configuration Manager verwenden.





### <a name="agent-based-replication-process"></a>Agentbasierter Replikationsprozess

![Replikationsprozess](./media/migrate-architecture/v2a-architecture-henry.png)

1. Wenn Sie die Replikation für eine VM aktivieren, beginnt die erste Replikation zu Azure. 
    - Die Replikation findet auf Blockebene statt und ist nahezu kontinuierlich mithilfe des Mobility Service-Agents, der auf dem virtuellen Computer ausgeführt wird.
    - Es werden Replikationsrichtlinieneinstellungen angewendet:
        - **RPO-Schwellenwert**: Diese Einstellung hat keine Auswirkungen auf die Replikation. Sie unterstützt bei der Überwachung. Ein Ereignis wird ausgelöst, und optional wird eine E-Mail gesendet, wenn das aktuelle RPO den von Ihnen angegebenen Schwellenwert überschreitet.
        - **Aufbewahrung des Wiederherstellungspunkts**. Diese Einstellung gibt an, wie weit Sie zurückgehen möchten, wenn es zu einer Unterbrechung kommt. Bei Premium-Speicher beträgt die maximale Aufbewahrungsdauer 24 Stunden. Bei Standardspeicher beträgt sie 72 Stunden. 
        - **App-konsistente Momentaufnahmen**: Abhängig von Ihren App-Anforderungen können alle 1 bis 12 Stunden App-konsistente Momentaufnahmen erstellt werden. Bei den Momentaufnahmen handelt es sich um Azure-Blob-Standardmomentaufnahmen. Der auf einem virtuellen Computer ausgeführte Mobility Service-Agent fordert eine VSS-Momentaufnahme in Übereinstimmung mit dieser Einstellung an und markiert diesen Zeitpunkt als anwendungskonsistenten Punkt im Replikationsstrom.

2. Der Datenverkehr wird über das Internet auf öffentliche Endpunkte von Azure Storage repliziert.

    - Alternativ hierzu können Sie Azure ExpressRoute mit [Microsoft-Peering](../expressroute/expressroute-circuit-peerings.md#microsoftpeering) verwenden.
    - Das Replizieren von Datenverkehr über ein virtuelles privates Site-to-Site-Netzwerk (VPN) von einem lokalen Standort nach Azure wird nicht unterstützt.
3. Die Replikation von Deltaänderungen in Azure beginnt, nachdem die erste Replikation abgeschlossen wurde. Nachverfolgte Änderungen für einen Computer werden an den Prozessserver gesendet.
2. Die Kommunikation erfolgt folgendermaßen:

    - Virtuelle Computer kommunizieren mit dem lokalen Konfigurationsserver über den Port HTTPS 443 für eingehenden Datenverkehr, um die Replikationsverwaltung auszuführen.
    - Der Konfigurationsserver orchestriert die Replikation mit Azure über Port HTTPS 443 für ausgehenden Datenverkehr.
    - Virtuelle Computer senden Replikationsdaten an den Prozessserver (der auf dem Konfigurationsserver ausgeführt wird) über Port HTTPS 9443 für eingehenden Datenverkehr. Dieser Port kann geändert werden.
    - Der Prozessserver empfängt Replikationsdaten, optimiert und verschlüsselt sie und sendet sie über den ausgehenden Port 443 an den Azure-Speicher.
5. Die Replikationsdatenprotokolle werden zunächst in einem Cachespeicherkonto in Azure gespeichert. Diese Protokolle werden dann verarbeitet, und die Daten werden auf einem verwalteten Azure-Datenträger (dem Azure Site Recovery-Seed-Datenträger) gespeichert. Die Wiederherstellungspunkte werden auf diesem Datenträger erstellt.







## <a name="next-steps"></a>Nächste Schritte

- Sehen Sie sich die [häufig gestellten Fragen](resources-faq.md) zu Azure Migrate an.
- Hilfe von der Community erhalten Sie im [Azure Migrate MSDN-Forum](https://social.msdn.microsoft.com/Forums/home?forum=AzureMigrate&filter=alltypes&sort=lastpostdesc) oder bei [Stack Overflow](https://stackoverflow.com/search?q=azure+migrate).

