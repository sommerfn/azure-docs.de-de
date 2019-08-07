---
title: Azure Migrate – Häufig gestellte Fragen (FAQ) | Microsoft-Dokumentation
description: Beantwortet häufig gestellte Fragen zu Azure Migrate
author: snehaamicrosoft
ms.service: azure-migrate
ms.topic: conceptual
ms.date: 07/17/2019
ms.author: snehaa
ms.openlocfilehash: 0708502087ae6880d9559cf17f0ba9982b2ba040
ms.sourcegitcommit: 57a7d4f67635212f5bf0c56e58fd87c8ec366f2c
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 07/22/2019
ms.locfileid: "68372489"
---
# <a name="azure-migrate---frequently-asked-questions-faq"></a>Azure Migrate – Häufig gestellte Fragen (FAQ)

Dieser Artikel enthält häufig gestellte Fragen zu Azure Migrate. Sollten Sie nach der Lektüre dieses Artikels noch Fragen haben, stellen Sie diese bitte im [Azure Migrate-Forum](https://aka.ms/AzureMigrateForum).

## <a name="general"></a>Allgemein

### <a name="which-azure-geographies-are-supported-by-azure-migrate"></a>Welche Azure-Geografien werden von Azure Migrate unterstützt?

Die Liste für VMware finden Sie [hier](https://docs.microsoft.com/azure/migrate/migrate-support-matrix-vmware#azure-migrate-projects). Die Liste für Hyper-V finden Sie [hier](https://docs.microsoft.com/azure/migrate/migrate-support-matrix-hyper-v#azure-migrate-projects).

### <a name="how-is-azure-migrate-different-from-azure-site-recovery"></a>Was unterscheidet Azure Migrate von Azure Site Recovery?

Azure Migrate bietet einen zentralisierten Hub zum Starten, Ausführen und Nachverfolgen der Ermittlung, Bewertung und Migration von Computern und Workloads zu Azure. [Azure Site Recovery](https://docs.microsoft.com/azure/site-recovery/migrate-tutorial-on-premises-azure) ist eine Lösung für die Notfallwiederherstellung. Azure Migrate Server Migration nutzt Azure Site Recovery im Back-End, um Migrationsszenarien für die Migration mit Lift & Shift von lokalen Computern zu ermöglichen.

## <a name="azure-migrate-appliance-vmwarephysical-servers"></a>Azure Migrate-Appliance (VMware/physische Server)

### <a name="how-does-the-azure-migrate-appliance-connect-to-azure"></a>Wie stellt die Azure Migrate-Appliance eine Verbindung mit Azure her?

Die Verbindung kann über das Internet erfolgen oder Sie können ExpressRoute mit öffentlichem/Microsoft-Peering nutzen.

### <a name="what-network-connectivity-requirements-are-needed-for-azure-migrate-server-assessment-and-migration"></a>Welche Netzwerkverbindungsanforderungen sind für Azure Migrate Server Assessment und Migration erforderlich?

Weitere Informationen zu den URLs und Ports, die dazu erforderlich sind, dass Azure Migrate und Azure kommunizieren können, finden Sie in den Supportmatrizen für [VMWare](migrate-support-matrix-vmware.md) und [Hyper-V](migrate-support-matrix-hyper-v.md).

### <a name="can-i-harden-the-appliance-vm-i-set-up-with-the-template"></a>Kann ich die von mir eingerichtete VM der Appliance mit der Vorlage verstärkt schützen?

Zusätzliche Komponenten (z. B. Antivirensoftware) können in der Vorlage hinzugefügt werden, solange die für die Azure Migrate-Appliance erforderlichen Kommunikations- und Firewallregeln unverändert bleiben.   

### <a name="what-data-is-collected-by-azure-migrate-appliance"></a>Welche Daten werden von der Azure Migrate-Appliance gesammelt?

Weitere Informationen zu den von der Azure Migrate-Appliance erfassten Daten finden Sie [hier](https://docs.microsoft.com/azure/migrate/migrate-appliance#collected-performance-data-vmware).

### <a name="is-there-any-performance-impact-on-the-analyzed-vmware-or-hyper-v-environment"></a>Gibt es Auswirkungen auf die Leistung der analysierten VMware- oder Hyper-V-Umgebung?

Mit der kontinuierlichen Profilerstellung von Leistungsdaten erstellt die Azure Migrate-Appliance Profile von lokalen Computern zur Messung von VM-Leistungsdaten. Dies hat fast keinerlei Auswirkungen auf die Leistung der Hyper-V/ESXi-Hosts wie auch auf vCenter Server.

### <a name="where-is-the-collected-data-stored-and-for-how-long"></a>Wo und wie lange werden die gesammelten Daten gespeichert?

Die von der Azure Migrate-Appliance gesammelten Daten werden im Azure-Speicherort gespeichert, den Sie beim Erstellen des Migrationsprojekts angeben. Die Daten werden sicher in einem Microsoft-Abonnement gespeichert und gelöscht, wenn Sie das Azure Migrate-Projekt löschen.

Wenn Sie für die Abhängigkeitsvisualisierung Agents auf den VMs installieren, werden die von den Dependency-Agents gesammelten Daten in den USA in einem Log Analytics-Arbeitsbereich gespeichert, der im Azure-Abonnement erstellt wurde. Diese Daten werden gelöscht, wenn Sie den Log Analytics-Arbeitsbereich in Ihrem Abonnement löschen. [Weitere Informationen](concepts-dependency-visualization.md)

### <a name="what-is-the-volume-of-data-uploaded-by-azure-migrate-appliance-during-continuous-profiling"></a>Wie hoch ist das Datenvolumen, das von der Azure Migrate-Appliance bei kontinuierlicher Profilerstellung hochgeladen wird?

Das an Azure Migrate gesendete Datenvolumen hängt von verschiedenen Parametern ab. Um eine indikative Zahl zu nennen, sendet ein Azure Migrate-Projekt mit 10 Computern (jeweils mit einem Datenträger und einem NIC) rund 50 MB pro Tag. Dies ist ein ungefährer Wert, der sich auf Grundlage der Anzahl von Datenpunkten für die NICS und Datenträger ändert (die gesendeten Daten sind nicht linear, wenn die Anzahl der Computer, NICs oder Datenträger zunimmt).

### <a name="is-the-data-encrypted-at-rest-and-in-transit"></a>Sind die Daten bei der Übertragung und im Ruhezustand verschlüsselt?

Ja, sowohl als auch. Die Metadaten werden sicher über das Internet über https an den Azure Migrate-Dienst gesendet. Die Metadaten werden in [Cosmos DB](https://docs.microsoft.com/azure/cosmos-db/database-encryption-at-rest) und [Azure Blob Storage](https://docs.microsoft.com/azure/storage/common/storage-service-encryption) in einem Microsoft-Abonnement gespeichert und im Ruhezustand verschlüsselt.

Die von den Dependency-Agents gesammelten Daten werden auch bei der Übertragung verschlüsselt (sicheres HTTPS) und im Log Analytics-Arbeitsbereich im Abonnement des Benutzers gespeichert. Außerdem erfolgt eine Verschlüsselung im Ruhezustand.

### <a name="how-does-the-azure-migrate-appliance-communicate-with-the-vcenter-server-and-the-azure-migrate-service"></a>Wie kommuniziert die Azure Migrate-Appliance mit dem vCenter Server und dem Azure Migrate-Dienst?

Die Appliance stellt mithilfe der von Ihnen in der Appliance angegebenen Anmeldeinformationen eine Verbindung mit dem vCenter Server (Port 443) her. Anschließend erfolgt eine Abfrage von vCenter Server mithilfe der VMware PowerCLI, um Metadaten über die von vCenter Server verwalteten Metadaten zu sammeln. Es werden sowohl Konfigurationsdaten zu virtuellen Computern (Kerne, Arbeitsspeicher, Datenträger, NIC usw.) als auch der Leistungsverlauf für jeden virtuellen Computer für den letzten Monat gesammelt. Die gesammelte Metadaten werden dann zur Bewertung an Azure Migrate Server Assessment (über das Internet über HTTPS) gesendet.

### <a name="can-i-connect-the-same-appliance-to-multiple-vcenter-servers"></a>Kann ich eine Appliance mit mehreren vCenter-Servern verbinden?

Ja, eine einzelne Azure Migrate-Appliance kann für die Ermittlung mehrerer vCenter-Server verwendet werden, jedoch nicht gleichzeitig. Sie müssen die Ermittlungen nacheinander durchführen.

### <a name="i-changed-my-machine-size-can-i-rerun-an-assessment"></a>Ich habe meine Computergröße geändert. Kann ich eine Bewertung erneut ausführen?

Die Azure Migrate-Appliance sammelt kontinuierlich Informationen über die lokale Umgebung. Eine Bewertung ist jedoch eine Point-in-Time-Momentaufnahme von lokalen VMs. Wenn Sie die Einstellungen einer VM, die Sie bewerten möchten, ändern, verwenden Sie die Option „Neu berechnen“, um die Bewertung mit den neuesten Änderungen zu aktualisieren.

### <a name="how-can-i-discover-a-multi-tenant-environment-in-azure-migrate-server-assessment"></a>Wie kann ich eine Umgebung mit mehreren Mandanten in der Azure Migrate-Serverbewertung ermitteln?

Wenn Sie für VMware eine Umgebung haben, die für alle Mandanten gemeinsam genutzt wird, und Sie die VMs eines Mandanten nicht im Abonnement eines anderen Mandanten ermitteln möchten, erstellen Sie vCenter Server-Anmeldeinformationen mit Zugriff nur auf die VMs, die Sie ermitteln möchten. Verwenden Sie dann die Anmeldeinformationen, wenn Sie mit der Suche in der Azure Migrate-Appliance beginnen.

Für Hyper-V verwendet die Ermittlung Hyper-V Host-Anmeldeinformationen, wenn die VMs den gleichen Hyper-V-Host verwenden. Derzeigt gibt es keine Möglichkeit, die Ermittlung zu trennen.  

### <a name="how-many-vms-can-be-discovered-using-a-single-migration-appliance"></a>Wie viele VMs können mit einer einzigen Migrations-Appliance erkannt werden?

Sie können bis zu 10.000 VMware-VMs und bis zu 5.000 Hyper-V-VMs mit einer einzigen Migrations-Appliance erkennen.  Wenn Sie mehr Computer in Ihrer lokalen Umgebung haben, erfahren Sie, wie Sie die [Hyper-V](scale-hyper-v-assessment.md)- und [VMware](scale-vmware-assessment.md)-Bewertung skalieren können.

## <a name="azure-migrate-server-assessment"></a>Azure Migrate: Serverbewertung

### <a name="does-azure-migrate-server-assessment-support-assessment-of-physical-servers"></a>Unterstützt Azure Migrate: Serverbewertung die Bewertung physischer Server?

Nein, Azure Migrate unterstützt derzeit keine Bewertung von physischen Servern.

### <a name="does-azure-migrate-need-vcenter-server-to-discover-a-vmware-environment"></a>Benötigt Azure Migrate vCenter Server zum Ermitteln einer VMware-Umgebung?

Ja, Azure Migrate benötigt vCenter Server zum Ermitteln einer VMware-Umgebung. Die Ermittlung von ESXi-Hosts, die nicht von einem vCenter Server verwaltet werden, wird nicht unterstützt.

### <a name="whats-the-difference-between-using-azure-migrate-server-assessment-and-the-map-toolkit"></a>Was ist der Unterschied zwischen Azure Migrate: Serverbewertung und dem MAP-Toolkit?

Azure Migrate: Serverbewertung bietet eine Migrationsbewertung zur Unterstützung der Migrationsbereitschaft und eine Bewertung der Arbeitslasten für die Migration nach Azure. Das [Microsoft Assessment and Planning (MAP) Toolkit](https://www.microsoft.com/download/details.aspx?id=7826) bietet weitere Funktionen, beispielsweise die Migrationsplanung für neuere Versionen von Windows-Client- und -Serverbetriebssystemen sowie die Nachverfolgung der Softwarenutzung. Verwenden Sie für diese Szenarien weiterhin das MAP Toolkit.

### <a name="how-is-azure-migrate-server-assessment-different-from-azure-site-recovery-deployment-planner"></a>Wie unterscheidet sich Azure Migrate: Serverbewertung von Azure Site Recovery-Bereitstellungsplaner?

Azure Migrate: Serverbewertung ist ein Tool zur Migrationsplanung. Azure Site Recovery-Bereitstellungsplaner ist ein Tool zur Notfallwiederherstellung.

- **Migration von VMware/Hyper-V zu Azure:** Wenn Sie beabsichtigen, Ihre lokalen Server nach Azure zu migrieren, verwenden Sie das Azure Migrate: Serverbewertungstool für die Migrationsplanung. Das Tool bewertet lokale Workloads und bietet Anleitungen, Einblicke und Mechanismen, um Sie bei der Migration zu Azure zu unterstützen. Sobald Sie mit Ihrem Migrationsplan fertig sind, können Sie Tools wie Azure Migrate: Serverbewertung verwenden, um die Computer zu Azure zu migrieren.
- **Notfallwiederherstellung von VMware/Hyper-V zu Azure:** Verwenden Sie für die Notfallwiederherstellung auf Azure mit Site Recovery den Site Recovery-Bereitstellungsplanner für die Notfallwiederherstellungsplanung. Der Site Recovery-Bereitstellungsplaner führt eine umfassende, Site Recovery-spezifische Bewertung Ihrer lokalen Umgebung durch. Er enthält Empfehlungen, die von Site Recovery für erfolgreiche DR-Vorgänge wie Replikation und Failover von VMs benötigt werden.

### <a name="does-azure-migrate-support-enterprise-agreement-ea-based-cost-estimation"></a>Unterstützt Azure Migrate EA-basierte (Enterprise Agreement) Kostenschätzungen?

Azure Migrate unterstützt derzeit keine Kostenschätzung für das [Enterprise Agreement-Angebot](https://azure.microsoft.com/offers/enterprise-agreement-support/). Die Problemumgehung besteht darin, als Angebot „Nutzungsbasierte Zahlung“ anzugeben und den (für das Abonnement geltenden) Rabattprozentsatz manuell in das Feld „Rabatt“ der Bewertungseigenschaften einzugeben.

  ![Discount](./media/resources-faq/discount.png)

### <a name="whats-the-difference-between-as-on-premises-sizing-and-performance-based-sizing"></a>Was ist der Unterschied zwischen den Größenanpassungen „Wie lokal“ und „Leistungsbasiert“?

- Bei der lokalen Größenanpassung berücksichtigt Azure Migrate keine VM-Leistungsdaten. Es dimensioniert die VMs basierend auf der lokalen Konfiguration. – Bei der leistungsbasierten Größenanpassung basiert diese auf Nutzungsdaten.
- Wenn beispielsweise eine lokale VM 4 Kerne und 8 GB Speicher mit 50 % CPU-Auslastung und 50 % Arbeitsspeicherverwendung aufweist, empfiehlt die lokale Größenanpassung eine Azure-VM-SKU mit 4 Kernen und 8 GB Speicher. Die leistungsbasierte Größenanpassung empfiehlt jedoch eine VM-SKU mit 2 Kernen und 4 GB, da der Auslastungsprozentsatz berücksichtigt wird.
- Entsprechend hängt die Größenanpassung von zwei Bewertungseigenschaften ab: dem Größenkriterium und dem Speichertyp.
Wenn das Größenkriterium „Leistungsbasiert“ ist und der Speichertyp „Automatisch“ lautet, werden die IOPS- und Durchsatzwerte des Datenträgers berücksichtigt, um den Typ des Zieldatenträgers zu ermitteln (Standard oder Premium).
- Wenn das Größenkriterium leistungsabhängig ist und der Speichertyp „Premium“ ist, wird ein Premium-Datenträger empfohlen. Die SKU für den Premium-Datenträger wird basierend auf der Größe der lokalen Festplatte ausgewählt. Die gleiche Logik wird verwendet, um die Größenanpassung für Datenträger durchzuführen, wenn das Größenkriterium „Wie lokal“ verwendet wird und der Speichertyp „Standard“ oder „Premium“ lautet.

### <a name="what-impact-does-performance-history-and-percentile-utilization-have-on-the-size-recommendations"></a>Welche Auswirkungen haben der Leistungsverlauf und der Quantilwert der Nutzung auf die Größenempfehlungen?

Diese Eigenschaften gelten nur für die leistungsbasierte Größenanpassung.

- Azure Migrate sammelt den Leistungsverlauf von lokalen Computern und nutzt diese Daten, um die VM-Größe und den Datenträgertyp in Azure zu empfehlen.
- Die Appliance erstellt für die lokale Umgebung kontinuierlich Profildaten, um alle 20 Sekunden Echtzeit-Verwendungsdaten erfassen zu können. Die Appliance führt für die 20-Sekunden-Stichproben einen Rollup aus und erstellt für jeweils 15 Minuten einen Datenpunkt. Zum Erstellen jedes Datenpunkts wählt die Appliance den Spitzenwert aus allen 20-Sekunden-Stichproben aus und sendet diesen an Azure.
- Beim Erstellen einer Bewertung in Azure berechnet Azure Migrate basierend auf der Leistungsdauer und dem Quantilwert des Leistungsverlaufs den geltenden Auslastungswert und verwendet ihn für die Größenanpassung.
- Wenn Sie die Leistungsdauer beispielsweise auf einen Tag und den Quantilwert auf 95 festgelegt haben, verwendet Azure Migrate die vom Collector gesendeten 15-Minuten-Stichprobenpunkte für den gesamten letzten Tag, sortiert sie in aufsteigender Reihenfolge und wählt den Wert des 95. Quantils als gültige Auslastung aus.
- Mit dem Wert des 95. Quantils wird sichergestellt, dass Ausreißer ignoriert werden. Diese können auftreten, wenn Sie das 99. Quantil verwenden. Falls Sie die Spitzenauslastung für den Zeitraum wählen möchten und keine Ausreißer verpassen möchten, sollten Sie das 99. Quantil wählen.

### <a name="what-is-dependency-visualization"></a>Was ist die Visualisierung von Abhängigkeiten?

Die Visualisierung von Abhängigkeiten ermöglicht es Ihnen, Gruppen von VMs für die Migration mit größerer Sicherheit zu bewerten. Sie hilft Ihnen, die Abhängigkeiten zwischen Computern zu überprüfen, bevor Sie eine Bewertung durchführen. Die Visualisierung der Abhängigkeiten trägt dazu bei, dass nichts übersehen wird. Dadurch vermeiden Sie unerwartete Ausfälle bei der Migration zu Azure. Azure Migrate nutzt für die Visualisierung Abhängigkeiten die Dienstzuordnungslösung in Azure Monitor-Protokollen.

> [!NOTE]
> Die Funktion zur Visualisierung von Abhängigkeiten ist in Azure Government nicht verfügbar.

### <a name="do-i-need-to-pay-to-use-the-dependency-visualization-feature"></a>Ist die Nutzung des Features zur Visualisierung von Abhängigkeiten kostenpflichtig?

Nein. Weitere Informationen zu den Preisen von Azure Migrate finden Sie [hier](https://azure.microsoft.com/pricing/details/azure-migrate/).

### <a name="do-i-need-to-install-anything-for-dependency-visualization"></a>Muss ich für die Visualisierung von Abhängigkeiten Installationen durchführen?

Sie müssen für die Verwendung der Visualisierung von Abhängigkeiten Agents auf alle lokalen Computer, die Sie bewerten möchten, herunterladen und dort installieren.

- Der [Microsoft Monitoring Agent (MMA)](https://docs.microsoft.com/azure/log-analytics/log-analytics-agent-windows) muss auf jedem Computer installiert werden.
- Der [Dependency-Agent](https://docs.microsoft.com/azure/monitoring/monitoring-service-map-configure) muss auf jedem Computer installiert werden.
- Falls Sie über Computer ohne Internetverbindung verfügen, ist es außerdem erforderlich, auf diesen das Log Analytics-Gateway herunterzuladen und zu installieren.

Sie benötigen diese Agents nur, wenn Sie die Abhängigkeitsvisualisierung verwenden.

### <a name="can-i-use-an-existing-workspace-for-dependency-visualization"></a>Kann ich für die Visualisierung von Abhängigkeiten einen vorhandenen Arbeitsbereich verwenden?

Ja, Sie können einen vorhandenen Arbeitsbereich an das Migrationsprojekt anfügen und für die Abhängigkeitsvisualisierung zu nutzen. [Weitere Informationen](concepts-dependency-visualization.md#how-does-it-work)

### <a name="can-i-export-the-dependency-visualization-report"></a>Kann ich den Bericht über die Abhängigkeitsvisualisierung exportieren?

Nein. Der Bericht über die Abhängigkeitsvisualisierung kann nicht exportiert werden. Da Azure Migrate die Dienstzuordnung zur Abhängigkeitsvisualisierung verwendet, können Sie die Abhängigkeiten jedoch mithilfe der [Dienstzuordnungs-REST-APIs](https://docs.microsoft.com/rest/api/servicemap/machines/listconnections) in einem JSON-Format abrufen.

### <a name="how-can-i-automate-the-installation-of-microsoft-monitoring-agent-mma-and-dependency-agent"></a>Wie kann ich die Installation von Microsoft Monitoring Agent (MMA) und des Dependency-Agents automatisieren?

[Verwenden Sie dieses Skript](https://docs.microsoft.com/azure/monitoring/monitoring-service-map-configure#installation-script-examples) für die Installation der Agenten. [Befolgen Sie diese Anweisungen](https://docs.microsoft.com/azure/azure-monitor/platform/log-analytics-agent#install-and-configure-agent), um MMA über die Befehlszeile oder die Automatisierung zu installieren. Für MMA, nutzen Sie [dieses Skript](https://gallery.technet.microsoft.com/scriptcenter/Install-OMS-Agent-with-2c9c99ab).

Zusätzlich zu Skripts können Sie auch Bereitstellungstools wie System Center Configuration Manager, [Intigua](https://www.intigua.com/getting-started-intigua-for-azure-migration) usw. zum Bereitstellen der Agents verwenden.

### <a name="what-operating-systems-are-supported-by-mma"></a>Welche Betriebssysteme werden von MMA unterstützt?

- [Überprüfen](https://docs.microsoft.com/azure/log-analytics/log-analytics-concept-hybrid#supported-windows-operating-systems) Sie die Liste der von MMA unterstützten Windows-Betriebssysteme.
- [Überprüfen](https://docs.microsoft.com/azure/log-analytics/log-analytics-concept-hybrid#supported-linux-operating-systems) Sie die Liste der von MMA unterstützten Linux-Betriebssysteme.

### <a name="what-are-the-operating-systems-supported-by-the-dependency-agent"></a>Welche Betriebssysteme werden vom Anhängigkeitsagent unterstützt?

[Überprüfen](https://docs.microsoft.com/azure/monitoring/monitoring-service-map-configure#supported-windows-operating-systems) Sie die Windows-Betriebssystemen, die vom Abhängigkeitsagent unterstützt werden.
[Überprüfen](https://docs.microsoft.com/azure/monitoring/monitoring-service-map-configure#supported-linux-operating-systems) Sie die Linux-Betriebssystemen, die vom Abhängigkeitsagent unterstützt werden.

### <a name="can-i-visualize-dependencies-in-azure-migrate-for-more-than-an-hour"></a>Kann ich Abhängigkeiten in Azure Migrate für länger als eine Stunde visualisieren?
Nein, Sie können Abhängigkeiten bis zu einer Stunde visualisieren. Sie können zu einem bestimmten Datum im Verlauf zurückkehren (bis zum letzten Monat) aber die maximale Dauer der Visualisierung beträgt eine Stunde. So können Sie beispielsweise mithilfe des Zeitraums im Abhängigkeitsdiagramm Abhängigkeiten für gestern, jedoch nur für ein Zeitfenster von einer Stunde anzeigen. Sie können jedoch zum [Abfragen der Abhängigkeitsdaten](https://docs.microsoft.com/azure/migrate/how-to-create-group-machine-dependencies) über einen längeren Zeitraum Azure Monitor-Protokolle verwenden.

### <a name="is-dependency-visualization-supported-for-groups-with-more-than-10-vms"></a>Wird die Abhängigkeitsvisualisierung für Gruppen mit mehr als zehn VMs unterstützt?
Sie können [Abhängigkeiten für Gruppen](https://docs.microsoft.com/azure/migrate/how-to-create-group-dependencies) mit bis zu 10 VMs visualisieren. Falls eine Gruppe aus mehr als 10 VMs besteht, sollten Sie die Gruppe in kleinere Gruppen aufteilen und die Abhängigkeiten anschließend visualisieren.

## <a name="azure-migrate-server-migration"></a>Azure Migrate: Serverbewertung

### <a name="how-is-azure-migrate-server-migration-different-from-azure-site-recovery"></a>Wie unterscheidet sich Azure Migrate: Servermigration von Azure Site Recovery?

Azure Migrate: Servermigration nutzt die Replikationsmaschine von Site Recovery für die agentenbasierte Migration von VMware-VMs, die Migration von Hyper-V-VMs und die Migration physischer Server auf Azure. Die Option zur Migration von VMware-VMs ohne Agent ist nativ in der Servermigration integriert.

## <a name="next-steps"></a>Nächste Schritte
Lesen Sie die [Übersicht zu Azure Migrate](migrate-services-overview.md).
