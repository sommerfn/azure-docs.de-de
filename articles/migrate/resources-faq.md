---
title: Gängige Fragen zu Azure Migrate
description: Beantwortet gängige und häufig gestellte Fragen zu Azure Migrate
author: snehaamicrosoft
ms.service: azure-migrate
ms.topic: conceptual
ms.date: 10/22/2019
ms.author: snehaa
ms.openlocfilehash: 1663e1d418610bcfa2ada28770944bfc8cbf908f
ms.sourcegitcommit: c22327552d62f88aeaa321189f9b9a631525027c
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 11/04/2019
ms.locfileid: "73480071"
---
# <a name="azure-migrate-common-questions"></a>Von der Azure Häufig gestellte Fragen

In diesem Artikel werden Fragen zu Azure Migrate beantwortet. Sollten Sie nach der Lektüre dieses Artikels noch Fragen haben, stellen Sie diese bitte im [Azure Migrate-Forum](https://aka.ms/AzureMigrateForum).

## <a name="general"></a>Allgemein

### <a name="which-azure-geographies-are-supported"></a>Welche Azure-Geografien werden unterstützt?

Überprüfen Sie die von Azure Migrate unterstützten geografischen Regionen für [VMware-VMs](https://docs.microsoft.com/azure/migrate/migrate-support-matrix-vmware#azure-migrate-projects) und [Hyper-V-VMs](https://docs.microsoft.com/azure/migrate/migrate-support-matrix-hyper-v#azure-migrate-projects).

### <a name="whats-the-difference-between-azure-migrate-and-site-recovery"></a>Was ist der Unterschied zwischen Azure Migrate und Site Recovery?

Azure Migrate stellt einen zentralisierten Hub zum Verwalten und Nachverfolgen der Ermittlung, Bewertung und Migration von lokalen Computern und Workloads zu Azure zur Verfügung. [Azure Site Recovery](../site-recovery/site-recovery-overview.md) ist eine Lösung für die Notfallwiederherstellung. Von der Azure Das Server Migrationstool nutzt gewisse Site Recovery-Back-End-Funktionen für die Lift & Shift-Migration von einigen lokalen Computern.

### <a name="how-do-i-delete-an-azure-migrate-project"></a>Wie lösche ich ein Azure Migrate-Projekt?

Befolgen Sie [diese Anleitung](how-to-delete-project.md) befolgen, um ein Projekt zu löschen. [Informieren Sie sich über die Ressourcen](how-to-delete-project.md#created-resources), die beim Ermitteln, Bewerten und Migrieren von Computern und Workloads in einem Azure Migrate-Projekt erstellt werden.


## <a name="azure-migrate-appliance"></a>Azure Migrate-Appliance

### <a name="how-does-the-appliance-connect-to-azure"></a>Wie stellt die Appliance eine Verbindung mit Azure her?

Die Verbindung kann über das Internet erfolgen. Sie können auch Azure ExpressRoute mit öffentlichem/Microsoft-Peering verwenden.

### <a name="what-network-connectivity-is-needed-for-azure-migrate-server-assessment-and-migration"></a>Welche Netzwerkkonnektivität ist für die Serverbewertung und Migration mit Azure Migrate erforderlich?

Informationen zu den URLs und Ports, die für die Kommunikation von Azure Migrate mit Azure erforderlich sind, finden Sie in den Supportmatrizen für [VMware](migrate-support-matrix-vmware.md) und [Hyper-V](migrate-support-matrix-hyper-v.md).

### <a name="can-i-harden-the-appliance-vm-created-with-the-template"></a>Kann ich die erstellte Appliance-VM mit der Vorlage verstärkt schützen?

Sie können der Vorlage zusätzliche Komponenten (z. B. Antivirensoftware) hinzufügen, solange die für die Azure Migrate-Appliance erforderlichen Kommunikations- und Firewallregeln unverändert bleiben.

### <a name="what-data-is-collected-by-the-azure-migrate-appliance"></a>Welche Daten werden von der Azure Migrate-Appliance gesammelt?

Von der Appliance werden folgende Daten gesammelt:
- [Leistungsdaten](migrate-appliance.md#collected-performance-data-vmware) und [Metadaten](migrate-appliance.md#collected-metadata-vmware) für VMware-VMs.
- [Leistungsdaten](migrate-appliance.md#collected-performance-data-hyper-v) und [Metadaten](migrate-appliance.md#collected-metadata-hyper-v) für Hyper-V-VMs.


### <a name="does-appliance-analysis-impact-performance"></a>Wirkt sich die Applianceanalyse auf die Leistung aus?

Die Azure Migrate-Appliance erstellt fortlaufend Profile von lokalen Computern zur Messung von VM-Leistungsdaten. Diese Profilerstellung hat fast keine Auswirkungen auf die Leistung der Hyper-V-/ESXi-Hosts oder auf vCenter Server.

### <a name="where-and-how-long-is-collected-data-stored"></a>Wo und wie lange werden die gesammelten Daten gespeichert?

Die von der Azure Migrate-Appliance gesammelten Daten werden an dem Azure-Speicherort gespeichert, den Sie beim Erstellen des Migrationsprojekts auswählen. Die Daten werden sicher in einem Microsoft-Abonnement gespeichert und gelöscht, wenn Sie das Azure Migrate-Projekt löschen.

Für die Visualisierung von Abhängigkeiten werden die gesammelten Daten in den USA in einem Log Analytics-Arbeitsbereich gespeichert, der im Azure-Abonnement erstellt wurde. Diese Daten werden gelöscht, wenn Sie den Log Analytics-Arbeitsbereich in Ihrem Abonnement löschen. [Erfahren Sie mehr](concepts-dependency-visualization.md) über die Visualisierung von Abhängigkeiten.

### <a name="what-volume-of-data-is-uploaded-during-continuous-profiling"></a>Wie hoch ist das Datenvolumen, das bei der kontinuierlichen Profilerstellung hochgeladen wird?

Das an Azure Migrate gesendete Datenvolumen hängt von verschiedenen Parametern ab. Sie können dabei davon ausgehen, dass ein Azure Migrate-Projekt mit 10 Computern (jeweils mit einem Datenträger und einem NIC) pro Tag rund 50 MB übermittelt. Dieser Wert ist ungefähr und hängt von der Anzahl der Datenpunkte für die NICs und Datenträger ab. Die Menge der gesendeten Daten steigt nicht linear, wenn sich die Anzahl der Computer, NICs oder Datenträger erhöht.

### <a name="is-the-data-encrypted-at-rest-and-in-transit"></a>Werden die Daten im Ruhezustand und bei der Übertragung verschlüsselt?

Ja, bei beidem.
- Metadaten werden sicher über das Internet per HTTPS an den Azure Migrate-Dienst gesendet.
- Die Metadaten werden in einer [Azure Cosmos](../cosmos-db/database-encryption-at-rest.md)-Datenbank und in [Azure Blob Storage](../storage/common/storage-service-encryption.md) in einem Microsoft-Abonnement gespeichert. Die Metadaten werden im Ruhezustand verschlüsselt.
- Die für die Abhängigkeitsanalyse gesammelten Daten werden auch während der Übertragung verschlüsselt (sicheres HTTPS). Sie werden in einem Log Analytics-Arbeitsbereich in Ihrem Abonnement gespeichert. Außerdem erfolgt eine Verschlüsselung im Ruhezustand.

### <a name="how-does-the-appliance-communicate-with-vcenter-server-and-azure-migrate"></a>Wie kommuniziert die Appliance mit vCenter Server und Azure Migrate?

1. Die Appliance stellt mit den bei der Einrichtung der Appliance angegebenen Anmeldeinformationen eine Verbindung zu vCenter Server (Port 443) her.
2. Die Appliance nutzt VMware PowerCLI, um vCenter Server abzufragen und Metadaten über die von vCenter Server verwalteten VMs zu sammeln. Es werden Konfigurationsdaten zu virtuellen Computern (Kerne, Arbeitsspeicher, Datenträger, NICs) sowie der Leistungsverlauf für jeden virtuellen Computer für den letzten Monat gesammelt.
3. Die gesammelten Metadaten werden dann an Azure Migrate: Serverbewertung (über das Internet mit HTTPS) zur Bewertung gesendet.

### <a name="can-i-connect-the-same-appliance-to-multiple-vcenter-server-instances"></a>Kann ich eine Appliance mit mehreren vCenter Server-Instanzen verbinden?

Nein. Zwischen einer Appliance und vCenter Server besteht eine 1: 1-Zuordnung. Wenn Sie VMs in mehreren vCenter Server-Instanzen ermitteln möchten, müssen Sie mehrere Appliances bereitstellen.


### <a name="machine-size-changed-can-i-run-the-assessment-again"></a>Computergröße geändert. Kann ich die Bewertung erneut ausführen?

Die Azure Migrate-Appliance sammelt kontinuierlich Informationen zur lokalen Umgebung. Eine Bewertung ist jedoch eine Point-in-Time-Momentaufnahme von lokalen VMs. Wenn Sie die Einstellungen für eine VM, die Sie bewerten möchten, ändern, verwenden Sie die Option „Neu berechnen“, um die Bewertung mit den neuesten Änderungen zu aktualisieren.

### <a name="how-can-i-perform-discovery-in-a-multitenant-environment"></a>Wie kann ich eine Ermittlung in einer Umgebung mit mehreren Mandanten durchführen?

- Wenn Sie für VMware eine Umgebung haben, die von Mandanten gemeinsam genutzt wird, und Sie die VMs eines Mandanten nicht im Abonnement eines anderen Mandanten ermitteln möchten, erstellen Sie vCenter Server-Anmeldeinformationen mit Zugriff nur auf die VMs, die Sie ermitteln möchten. Verwenden Sie diese Anmeldeinformationen dann, wenn Sie die Ermittlung in der Azure Migrate-Appliance starten.
- Bei Hyper-V werden bei der Ermittlung Anmeldeinformationen für den Hyper-V-Host verwendet. Wenn virtuelle Computer denselben Hyper-V-Host nutzen, gibt es derzeit keine Möglichkeit, die Ermittlung zu trennen.  

### <a name="how-many-vms-can-i-discover-with-a-single-appliance"></a>Wie viele VMs können mit einer einzigen Appliance ermittelt werden?

Sie können bis zu 10.000 VMware-VMs und bis zu 5.000 Hyper-V-VMs mit einer einzigen Migration-Appliance erkennen. Wenn Sie mehr Computer in Ihrer lokalen Umgebung haben, informieren Sie sich, wie Sie die [Hyper-V](scale-hyper-v-assessment.md)- und [VMware](scale-vmware-assessment.md)-Bewertung skalieren können.

### <a name="can-i-delete-the-azure-migrate-appliance-from-the-project"></a>Kann ich die Azure Migrate-Appliance aus dem Projekt löschen?

Das Löschen der Appliance aus dem Projekt wird derzeit nicht unterstützt.

- Die einzige Möglichkeit zum Löschen der Appliance besteht darin, die Ressourcengruppe zu löschen, die das Azure Migrate-Projekt enthält, das mit der Appliance verknüpft ist.
- Beim Löschen der Ressourcengruppe werden jedoch auch andere registrierte Appliances, der ermittelte Bestand, Bewertungen und alle anderen Azure-Komponenten, die dem Projekt in der Ressourcengruppe zugeordnet sind, gelöscht.

## <a name="azure-migrate-server-assessment"></a>Azure Migrate-Serverbewertung


### <a name="does-azure-migrate-need-vcenter-server-for-vmware-vm-discovery"></a>Benötigt Azure Migrate vCenter Server zum Ermitteln von VMware-VMs?

Ja, Azure Migrate benötigt für die Ermittlung in einer VMware-Umgebung vCenter Server. Die Ermittlung von ESXi-Hosts, die nicht von vCenter Server verwaltet werden, wird nicht unterstützt.

### <a name="whats-the-difference-between-azure-migrate-server-assessment-and-the-map-toolkit"></a>Was ist der Unterschied zwischen der Azure Migrate-Serverbewertungen und dem MAP Toolkit?

Die Serverbewertung ermöglicht eine Bewertung zum Feststellen der Migrationsbereitschaft und eine Auswertung der Workloads für die Migration nach Azure. Das [Microsoft Assessment and Planning (MAP) Toolkit](https://www.microsoft.com/download/details.aspx?id=7826) unterstützt Sie bei anderen Aufgaben, beispielsweise der Migrationsplanung für neuere Versionen von Windows-Client-/Serverbetriebssystemen sowie die Nachverfolgung der Softwarenutzung. Verwenden Sie für diese Szenarien weiterhin das MAP Toolkit.

### <a name="whats-the-difference-between-server-assessment-and-the-site-recovery-deployment-planner"></a>Was ist der Unterschied zwischen der Serverbewertung und dem Site Recovery-Bereitstellungsplaner?

Die Serverbewertung ist ein Tool zur Migrationsplanung. Der Site Recovery-Bereitstellungsplaner ist ein Tool zur Notfallwiederherstellung. 

- **Planen der lokalen Migration zu Azure**: Wenn Sie planen, Ihre lokalen Server zu Azure zu migrieren, verwenden Sie für die Migrationsplanung die Serverbewertung. Sie bewertet lokale Workloads und stellt Anleitungen und Tools zur Verfügung, die Sie bei der Migration unterstützen. Nachdem der Migrationsplan steht, können Sie Tools wie die Azure Migrate-Servermigration verwenden, um die Computer zu Azure zu migrieren.
- **Planen der Notfallwiederherstellung in Azure**: Wenn Sie planen, mit Site Recovery eine Notfallwiederherstellung lokaler Computer in Azure einzurichten, verwenden Sie die Site Recovery-Bereitstellungsplaner. Der Bereitstellungsplaner führt eine umfassende, Site Recovery-spezifische Bewertung Ihrer lokalen Umgebung für die Notfallwiederherstellung durch. Er stellt Empfehlungen für die Notfallwiederherstellung zur Verfügung, z. B. Replikation und Failover.

### <a name="does-azure-migrate-estimate-costs-for-the-enterprise-agreement-ea-program"></a>Kann Azure Migrate die Kosten für das Enterprise Agreement-Programm (EA) abschätzen?

Die Azure Migrate-Serverbewertung unterstützt derzeit keine Kostenschätzung für das [Enterprise Agreement-Programm](https://azure.microsoft.com/offers/enterprise-agreement-support/). Als Problemumgehung können Sie beim Erstellen der Bewertung **Nutzungsbasierte Bezahlung** als **Angebot** angeben und dann den (für das Abonnement geltenden) Rabattprozentsatz manuell in das Feld **Rabatt** der Bewertungseigenschaften eingeben:

  ![Bewertungseigenschaften](./media/resources-faq/discount.png)

### <a name="whats-the-difference-between-as-on-premises-and-performance-based-sizing"></a>Was ist der Unterschied zwischen der lokal orientierten und der leistungsbasierten Größenanpassung?

Bei der lokal orientierten Größenanpassung berücksichtigt Azure Migrate keine VM-Leistungsdaten für die Bewertung. Die VM-Größen werden basierend auf der lokalen Konfiguration dimensioniert. Bei der leistungsbasierten Größenanpassung basiert diese auf Auslastungsdaten.

- Wenn eine lokale VM beispielsweise über 4 Kerne und 8 GB Arbeitsspeicher verfügt, tritt bei einer CPU- und Arbeitsspeicherauslastung von jeweils 50 % der folgende Fehler auf:
    - Bei der lokal orientierten Größenanpassung wird eine Azure-VM-SKU mit vier Kernen und 8 GB Arbeitsspeicher empfohlen.
    - Bei der leistungsbasierten Größenanpassung wird jedoch eine VM-SKU mit zwei Kernen und 4 GB Arbeitsspeicher empfohlen, da auch der Auslastungsprozentsatz berücksichtigt wird.

- Entsprechend hängt die Größenanpassung der Datenträger von zwei Bewertungseigenschaften ab: dem Größenkriterium und dem Speichertyp.
    - Wenn das leistungsbasierte Größenkriterium und der Speichertyp „Automatisch“ verwendet werden, berücksichtigt Azure Migrate die IOPS- und Durchsatzwerte des Datenträgers, um den Typ des Zieldatenträgers zu ermitteln (Standard oder Premium).
    - Wenn das leistungsbasierte Größenkriterium und der Speichertyp „Premium“ verwendet werden, empfiehlt Azure Migrate eine Premium-Datenträger-SKU basierend auf dem lokalen Datenträger. Die gleiche Logik wird verwendet, um die Größenanpassung für Datenträger durchzuführen, wenn die lokal orientierte Größenanpassung verwendet wird und der Speichertyp „Standard“ oder „Premium“ lautet.

### <a name="does-performance-history-and-utilization-percentile-impact-size-recommendations"></a>Wirken sich der Leistungsverlauf und der Perzentilwert der Auslastung auf die Größenempfehlungen aus?

Diese Eigenschaften gelten nur für die leistungsbasierte Größenanpassung.

- Azure Migrate sammelt den Leistungsverlauf von lokalen Computern und nutzt diese Daten, um die VM-Größe und den Datenträgertyp in Azure zu empfehlen.
- Die Appliance erstellt für die lokale Umgebung kontinuierlich Profildaten, um alle 20 Sekunden in Echtzeit Auslastungsdaten erfassen zu können.
- Die Appliance führt für die 20-Sekunden-Stichproben ein Rollup aus und erstellt alle 15 Minuten einen einzelnen Datenpunkt.
- Zum Erstellen des Datenpunkts wählt die Appliance den Spitzenwert aus den 20-Sekunden-Stichproben aus.
- Diesen Datenpunkt sendet die Appliance an Azure.

Beim Erstellen einer Bewertung in Azure berechnet Azure Migrate basierend auf der Leistungsdauer und dem Perzentilwert des Leistungsverlaufs den geltenden Auslastungswert und verwendet ihn für die Größenanpassung.

- Wenn Sie die Leistungsdauer beispielsweise auf einen Tag und den Perzentilwert auf 95 festgelegt haben, sortiert Azure Migrate die vom Collector gesendeten 15-Minuten-Stichprobenpunkte für den gesamten letzten Tag in aufsteigender Reihenfolge und wählt den Wert des 95. Perzentils als gültige Auslastung aus.
- Die Verwendung des Werts des 95. Perzentils stellt sicher, dass Ausreißer ignoriert werden. Ausreißer können eingeschlossen werden, indem Sie das 99. Perzentil verwenden. Falls Sie die Spitzenauslastung für den Zeitraum erfassen möchten, ohne Ausreißer zu verpassen, sollten Sie das 99. Perzentil verwenden.

## <a name="server-assessment---dependency-visualization"></a>Serverbewertung – Visualisierung von Abhängigkeiten


### <a name="what-is-dependency-visualization"></a>Was ist die Visualisierung von Abhängigkeiten?

Verwenden Sie die Visualisierung von Abhängigkeiten, um Gruppen von VMs für die Migration mit größerer Sicherheit zu bewerten. Die Visualisierung von Abhängigkeiten überprüft die Abhängigkeiten zwischen Computern, bevor Sie eine Bewertung ausführen. Sie stellt sicher, dass nichts übersehen wird, und vermeidet so unerwartete Ausfälle bei der Migration zu Azure. Azure Migrate verwendet die Dienstzuordnungslösung in Azure Monitor, um Abhängigkeitsvisualisierung zu ermöglichen. [Weitere Informationen](concepts-dependency-visualization.md).

> [!NOTE]
> Abhängigkeitsvisualisierung ist in Azure Government nicht verfügbar.

### <a name="do-i-need-to-pay-to-use-dependency-visualization"></a>Ist die Visualisierung von Abhängigkeiten kostenpflichtig?
Nein. Weitere Informationen zu den Preisen von Azure Migrate finden Sie [hier](https://azure.microsoft.com/pricing/details/azure-migrate/).

### <a name="do-i-need-to-install-anything-for-dependency-visualization"></a>Muss ich für die Visualisierung von Abhängigkeiten Installationen durchführen?

Sie müssen für die Verwendung der Visualisierung von Abhängigkeiten Agents auf alle lokalen Computer, die Sie bewerten möchten, herunterladen und dort installieren.

Sie müssen die folgenden Agents auf jedem Computer installieren:
- [Microsoft Monitoring Agent (MMA)](https://docs.microsoft.com/azure/log-analytics/log-analytics-agent-windows)
- [Dependency-Agent](../azure-monitor/platform/agents-overview.md#dependency-agent)
- Falls Sie über Computer ohne Internetverbindung verfügen, ist es erforderlich, auf diesen das Log Analytics-Gateway herunterzuladen und zu installieren.

Sie benötigen diese Agents nur, wenn Sie die Visualisierung von Abhängigkeiten verwenden.

### <a name="can-i-use-an-existing-workspace-for-dependency-visualization"></a>Kann ich für die Visualisierung von Abhängigkeiten einen vorhandenen Arbeitsbereich verwenden?

Ja, Sie können einen vorhandenen Arbeitsbereich an das Migrationsprojekt anfügen und für die Visualisierung von Abhängigkeiten nutzen. [Weitere Informationen](concepts-dependency-visualization.md#how-does-it-work)

### <a name="can-i-export-the-dependency-visualization-report"></a>Kann ich den Bericht über die Abhängigkeitsvisualisierung exportieren?

Nein, Visualisierungen von Abhängigkeiten können nicht exportiert werden. Azure Migrate verwendet jedoch die Dienstzuordnung, sodass Sie die Abhängigkeiten mithilfe der [Service Map-REST-API](https://docs.microsoft.com/rest/api/servicemap/machines/listconnections) im JSON-Format abrufen können.

### <a name="how-can-i-automate-the-installation-of-microsoft-monitoring-agent-mma-and-the-dependency-agent"></a>Wie kann ich die Installation von Microsoft Monitoring Agent (MMA) und des Dependency-Agents automatisieren?

Verwenden Sie [dieses Skript für die Installation des Dependency-Agents](../azure-monitor/insights/vminsights-enable-hybrid-cloud.md#installation-script-examples). [Befolgen Sie diese Anweisungen](https://docs.microsoft.com/azure/azure-monitor/platform/log-analytics-agent#install-and-configure-agent), um MMA über die Befehlszeile oder automatisiert zu installieren. Für MMA verwenden Sie [dieses Skript](https://gallery.technet.microsoft.com/scriptcenter/Install-OMS-Agent-with-2c9c99ab).

Zusätzlich zu Skripts können Sie auch Bereitstellungstools wie System Center Configuration Manager und [Intigua](https://www.intigua.com/getting-started-intigua-for-azure-migration) zum Bereitstellen der Agents verwenden.


### <a name="what-operating-systems-are-supported-by-mma"></a>Welche Betriebssysteme werden von MMA unterstützt?

- Überprüfen Sie die Liste der [von MMA unterstützten Windows-Betriebssysteme](https://docs.microsoft.com/azure/log-analytics/log-analytics-concept-hybrid#supported-windows-operating-systems).
- Überprüfen Sie die Liste der [von MMA unterstützten Linux-Betriebssysteme](https://docs.microsoft.com/azure/log-analytics/log-analytics-concept-hybrid#supported-linux-operating-systems).

### <a name="can-i-visualize-dependencies-for-more-than-an-hour"></a>Kann ich Abhängigkeiten für mehr als eine Stunde visualisieren?
Nein. Sie können Abhängigkeiten bis zu einer Stunde visualisieren. Sie können zu einem bestimmten Datum im Verlauf zurückkehren (höchstens einen Monat), aber die maximale Dauer der Visualisierung beträgt eine Stunde. Sie können beispielsweise den Zeitraum im Abhängigkeitsdiagramm verwenden, um Abhängigkeiten für den gestrigen Tag anzuzeigen. Sie können die Abhängigkeiten allerdings nur für ein Zeitfenster von einer Stunde anzeigen. Sie können allerdings zum [Abfragen der Abhängigkeitsdaten](https://docs.microsoft.com/azure/migrate/how-to-create-group-machine-dependencies) über einen längeren Zeitraum Azure Monitor-Protokolle verwenden.

### <a name="can-i-use-dependency-visualization-for-groups-of-more-than-10-vms"></a>Kann ich die Visualisierung von Abhängigkeiten für Gruppen mit mehr als 10 VMs verwenden?
Sie können [Abhängigkeiten für Gruppen visualisieren](https://docs.microsoft.com/azure/migrate/how-to-create-group-dependencies), die bis zu 10 VMs umfassen. Falls eine Gruppe aus mehr als 10 VMs besteht, sollten Sie die Gruppe in kleinere Gruppen aufteilen und die Abhängigkeiten anschließend visualisieren.

## <a name="azure-migrate-server-migration"></a>Azure Migrate-Servermigration

### <a name="whats-the-difference-between-azure-migrate-server-migration-and-site-recovery"></a>Was ist der Unterschied zwischen der Azure Migrate-Servermigration und Site Recovery?

- Die Migration ohne Agent von lokalen VMware-VMs ist eine native Funktionalität der Azure Migrate-Servermigration.
- Für die Migration von Hyper-V-VMs, von physischen Servern und Agent-basierten VMware-VMs verwendet die Azure Migrate-Servermigration die Azure Site Recovery-Replikationsengine.


## <a name="next-steps"></a>Nächste Schritte
Sehen Sie sich die [Übersicht zu Azure Migrate](migrate-services-overview.md) an.
