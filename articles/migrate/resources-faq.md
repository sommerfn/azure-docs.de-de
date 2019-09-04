---
title: Azure Migrate – häufig gestellte Fragen (FAQ) | Microsoft-Dokumentation
description: Beantwortet häufig gestellte Fragen zu Azure Migrate
author: snehaamicrosoft
ms.service: azure-migrate
ms.topic: conceptual
ms.date: 08/07/2019
ms.author: snehaa
ms.openlocfilehash: af95ad892b62cb5d8bece554d6026525d9279777
ms.sourcegitcommit: 44e85b95baf7dfb9e92fb38f03c2a1bc31765415
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 08/28/2019
ms.locfileid: "70102872"
---
# <a name="azure-migrate-frequently-asked-questions-faq"></a>Von der Azure Häufig gestellte Fragen (FAQ)

In diesem Artikel finden Sie häufig gestellte Fragen zu Azure Migrate. Sollten Sie nach der Lektüre dieses Artikels noch Fragen haben, stellen Sie diese bitte im [Azure Migrate-Forum](https://aka.ms/AzureMigrateForum).

## <a name="general"></a>Allgemein

### <a name="which-azure-geographies-does-azure-migrate-support"></a>Welche Azure-Geografien werden von Azure Migrate unterstützt?

Weitere Informationen finden Sie in den Listen für [VMware](https://docs.microsoft.com/azure/migrate/migrate-support-matrix-vmware#azure-migrate-projects) und [Hyper-V](https://docs.microsoft.com/azure/migrate/migrate-support-matrix-hyper-v#azure-migrate-projects).

### <a name="whats-the-difference-between-azure-migrate-and-azure-site-recovery"></a>Was ist der Unterschied zwischen Azure Migrate und Azure Site Recovery?

Azure Migrate bietet einen zentralisierten Hub zum Starten der Migration, Ausführen und Nachverfolgen der Ermittlung, Bewerten von Computern und Workloads sowie Ausführen und Nachverfolgen der Migration von Computern und Workloads zu Azure. [Azure Site Recovery](https://docs.microsoft.com/azure/site-recovery/migrate-tutorial-on-premises-azure) ist eine Lösung für die Notfallwiederherstellung. Die Azure Migrate-Servermigration nutzt Azure Site Recovery am Back-End, um Migrationsszenarien für die Lift & Shift-Migration von lokalen Computern zu ermöglichen.

## <a name="azure-migrate-appliance-vmwarephysical-servers"></a>Azure Migrate-Appliance (VMware/physische Server)

### <a name="how-does-the-azure-migrate-appliance-connect-to-azure"></a>Wie stellt die Azure Migrate-Appliance eine Verbindung mit Azure her?

Die Verbindung kann über das Internet erfolgen, oder Sie können Azure ExpressRoute mit öffentlichem/Microsoft-Peering nutzen.

### <a name="what-are-the-network-connectivity-requirements-for-azure-migrate-server-assessment-and-migration"></a>Welche Anforderungen an die Netzwerkkonnektivität bestehen für die Azure Migrate-Serverbewertung und -migration?

Weitere Informationen zu den URLs und Ports, die für die Kommunikation von Azure Migrate mit Azure erforderlich sind, finden Sie in den Supportmatrizen für [VMware](migrate-support-matrix-vmware.md) und [Hyper-V](migrate-support-matrix-hyper-v.md).

### <a name="can-i-harden-the-appliance-vm-that-i-set-up-with-the-template"></a>Kann ich die von mir mit der Vorlage eingerichtete Appliance-VM härten?

Sie können der Vorlage zusätzliche Komponenten (z. B. Antivirensoftware) hinzufügen, solange die für die Azure Migrate-Appliance erforderlichen Kommunikations- und Firewallregeln unverändert bleiben.

### <a name="what-data-is-collected-by-the-azure-migrate-appliance"></a>Welche Daten werden von der Azure Migrate-Appliance gesammelt?

Informationen zu den Daten, die von der Azure Migrate-Appliance gesammelt werden, finden Sie in der [Dokumentation zu Azure Migrate](https://docs.microsoft.com/azure/migrate/migrate-appliance#collected-performance-data-vmware).

### <a name="is-there-any-performance-impact-on-the-analyzed-vmware-or-hyper-v-environment"></a>Gibt es Auswirkungen auf die Leistung der analysierten VMware- oder Hyper-V-Umgebung?

Die Azure Migrate-Appliance erstellt fortlaufend Profile von lokalen Computern zur Messung von VM-Leistungsdaten. Diese Profilerstellung hat fast keine Auswirkungen auf die Leistung der Hyper-V-/ESXi-Hosts oder auf vCenter Server.

### <a name="where-is-the-collected-data-stored-and-for-how-long"></a>Wo und wie lange werden die gesammelten Daten gespeichert?

Die von der Azure Migrate-Appliance gesammelten Daten werden an dem Azure-Speicherort gespeichert, den Sie beim Erstellen des Migrationsprojekts auswählen. Die Daten werden sicher in einem Microsoft-Abonnement gespeichert und gelöscht, wenn Sie das Azure Migrate-Projekt löschen.

Wenn Sie für die Visualisierung von Abhängigkeiten Agents auf den VMs installieren, werden die von den Dependency-Agents gesammelten Daten in den USA in einem Log Analytics-Arbeitsbereich gespeichert, der im Azure-Abonnement erstellt wurde. Diese Daten werden gelöscht, wenn Sie den Log Analytics-Arbeitsbereich in Ihrem Abonnement löschen. Weitere Informationen finden Sie unter [Visualisierung von Abhängigkeiten](concepts-dependency-visualization.md).

### <a name="what-volume-of-data-is-uploaded-by-the-azure-migrate-appliance-during-continuous-profiling"></a>Wie hoch ist das Datenvolumen, das von der Azure Migrate-Appliance bei der kontinuierlichen Profilerstellung hochgeladen wird?

Das an Azure Migrate gesendete Datenvolumen hängt von verschiedenen Parametern ab. Sie können dabei davon ausgehen, dass ein Azure Migrate-Projekt mit 10 Computern (jeweils mit einem Datenträger und einem NIC) pro Tag rund 50 MB übermittelt. Dieser Wert ist eine Schätzung. Er hängt von der Anzahl der Datenpunkte für die NICs und Datenträger ab. (Die Menge der gesendeten Daten steigt nicht linear, wenn sich die Anzahl der Computer, NICs oder Datenträger erhöht.)

### <a name="is-the-data-encrypted-at-rest-and-in-transit"></a>Werden die Daten im Ruhezustand und bei der Übertragung verschlüsselt?

Ja, bei beidem. Die Metadaten werden sicher über das Internet per HTTPS an den Azure Migrate-Dienst gesendet. Die Metadaten werden in einer [Cosmos DB](https://docs.microsoft.com/azure/cosmos-db/database-encryption-at-rest)-Datenbank und in [Azure Blob Storage](https://docs.microsoft.com/azure/storage/common/storage-service-encryption) in einem Microsoft-Abonnement gespeichert. Die Metadaten werden im Ruhezustand verschlüsselt.

Die von den Dependency-Agents gesammelten Daten werden auch während der Übertragung verschlüsselt (sicheres HTTPS). Sie werden in einem Log Analytics-Arbeitsbereich in Ihrem Abonnement gespeichert. Außerdem erfolgt eine Verschlüsselung im Ruhezustand.

### <a name="how-does-the-azure-migrate-appliance-communicate-with-vcenter-server-and-the-azure-migrate-service"></a>Wie kommuniziert die Azure Migrate-Appliance mit vCenter Server und dem Azure Migrate-Dienst?

Die Appliance stellt mithilfe der von Ihnen bei der Einrichtung der Appliance angegebenen Anmeldeinformationen eine Verbindung mit vCenter Server (Port 443) her. Sie nutzt VMware PowerCLI, um vCenter Server abzufragen und Metadaten über die von vCenter Server verwalteten VMs zu sammeln. Es werden sowohl Konfigurationsdaten zu virtuellen Computern (Kerne, Arbeitsspeicher, Datenträger, NIC usw.) als auch der Leistungsverlauf für jeden virtuellen Computer für den letzten Monat gesammelt. Die gesammelte Metadaten werden dann zur Bewertung an die Azure Migrate-Serverbewertung (über das Internet per HTTPS) gesendet.

### <a name="can-i-connect-the-same-appliance-to-multiple-vcenter-server-instances"></a>Kann ich eine Appliance mit mehreren vCenter Server-Instanzen verbinden?

Nein. Zwischen einer Appliance und vCenter Server besteht eine 1: 1-Zuordnung. Wenn Sie virtuelle Computer in mehreren vCenter Server-Instanzen ermitteln möchten, müssen Sie mehrere Appliances bereitstellen.


### <a name="i-changed-my-machine-size-can-i-run-the-assessment-again"></a>Ich habe meine Computergröße geändert. Kann ich die Bewertung erneut ausführen?

Die Azure Migrate-Appliance sammelt kontinuierlich Informationen zur lokalen Umgebung. Eine Bewertung ist jedoch eine Point-in-Time-Momentaufnahme von lokalen VMs. Wenn Sie die Einstellungen einer VM, die Sie bewerten möchten, ändern, verwenden Sie die Option „Neu berechnen“, um die Bewertung mit den neuesten Änderungen zu aktualisieren.

### <a name="how-can-i-perform-discovery-in-a-multitenant-environment-in-azure-migrate-server-assessment"></a>Wie kann ich eine Ermittlung in einer Umgebung mit mehreren Mandanten in der Azure Migrate-Serverbewertung durchführen?

Wenn Sie für VMware eine Umgebung haben, die für alle Mandanten gemeinsam genutzt wird, und Sie die VMs eines Mandanten nicht im Abonnement eines anderen Mandanten ermitteln möchten, erstellen Sie vCenter Server-Anmeldeinformationen mit Zugriff nur auf die VMs, die Sie ermitteln möchten. Verwenden Sie diese Anmeldeinformationen dann, wenn Sie mit der Suche in der Azure Migrate-Appliance beginnen.

Bei Hyper-V werden bei der Ermittlung Anmeldeinformationen für den Hyper-V-Host verwendet. Wenn virtuelle Computer denselben Hyper-V-Host nutzen, gibt es derzeit keine Möglichkeit, die Ermittlung zu trennen.  

### <a name="how-many-vms-can-i-discover-with-a-single-migration-appliance"></a>Wie viele VMs können mit einer einzigen Migration-Appliance erkannt werden?

Sie können bis zu 10.000 VMware-VMs und bis zu 5.000 Hyper-V-VMs mit einer einzigen Migration-Appliance erkennen. Wenn Sie mehr Computer in Ihrer lokalen Umgebung haben, informieren Sie sich, wie Sie die [Hyper-V](scale-hyper-v-assessment.md)- und [VMware](scale-vmware-assessment.md)-Bewertung skalieren können.

## <a name="azure-migrate-server-assessment"></a>Azure Migrate-Serverbewertung

### <a name="does-azure-migrate-server-assessment-support-assessment-of-physical-servers"></a>Unterstützt die Azure Migrate-Serverbewertung die Bewertung physischer Server?

Nein, Azure Migrate unterstützt derzeit keine Bewertung von physischen Servern.

### <a name="does-azure-migrate-need-vcenter-server-to-perform-discovery-in-a-vmware-environment"></a>Benötigt Azure Migrate vCenter Server für die Ermittlung in einer VMware-Umgebung?

Ja, Azure Migrate benötigt für die Ermittlung in einer VMware-Umgebung vCenter Server. Die Ermittlung von ESXi-Hosts, die nicht von vCenter Server verwaltet werden, wird nicht unterstützt.

### <a name="whats-the-difference-between-azure-migrate-server-assessment-and-the-map-toolkit"></a>Was ist der Unterschied zwischen der Azure Migrate-Serverbewertungen und dem MAP Toolkit?

Die Azure Migrate-Serverbewertung bietet eine Migrationsbewertung zur Unterstützung der Migrationsbereitschaft und eine Bewertung der Workloads für die Migration zu Azure. Das [Microsoft Assessment and Planning (MAP) Toolkit](https://www.microsoft.com/download/details.aspx?id=7826) unterstützt Sie bei anderen Aufgaben, beispielsweise der Migrationsplanung für neuere Versionen von Windows-Client- und -Serverbetriebssystemen sowie die Nachverfolgung der Softwarenutzung. Verwenden Sie für diese Szenarien weiterhin das MAP Toolkit.

### <a name="whats-the-difference-between-azure-migrate-server-assessment-and-the-azure-site-recovery-deployment-planner"></a>Was ist der Unterschied zwischen der Azure Migrate-Serverbewertung und dem Azure Site Recovery-Bereitstellungsplaner?

Die Azure Migrate-Serverbewertung ist ein Tool zur Migrationsplanung. Der Azure Site Recovery-Bereitstellungsplaner ist ein Tool zur Planung der Notfallwiederherstellung.

**Migration von VMware/Hyper-V zu Azure:** Wenn Sie planen, Ihre lokalen Server zu Azure zu migrieren, verwenden Sie für die Migrationsplanung die Azure Migrate-Serverbewertung. Sie bewertet lokale Workloads und bietet Anleitungen, Erkenntnisse und Tools, um Sie bei der Migration Ihrer Server zu Azure zu unterstützen. Nachdem Sie den Migrationsplan erstellt haben, können Sie Tools wie die Azure Migrate-Servermigration verwenden, um die Computer zu Azure zu migrieren.

**Notfallwiederherstellung von VMware/Hyper-V zu Azure:** Verwenden Sie für die Planung der Notfallwiederherstellung in Azure mit Site Recovery den Site Recovery-Bereitstellungsplanner. Der Site Recovery-Bereitstellungsplaner führt eine umfassende, Site Recovery-spezifische Bewertung Ihrer lokalen Umgebung durch. Sie enthält Empfehlungen, die von Site Recovery für erfolgreiche Notfallwiederherstellungsvorgänge wie Replikation und Failover von VMs benötigt werden.

### <a name="does-azure-migrate-support-cost-estimation-for-the-enterprise-agreement-ea-program"></a>Unterstützt Azure Migrate Kostenschätzungen für das Enterprise Agreement-Programm (EA)?

Azure Migrate unterstützt derzeit keine Kostenschätzung für das [Enterprise Agreement-Programm](https://azure.microsoft.com/offers/enterprise-agreement-support/). Zur Problemumgehung geben Sie das **Angebot** als **Nutzungsbasierte Bezahlung** an und fügen dann den (für das Abonnement geltenden) Rabattprozentsatz manuell in das Feld **Rabatt** der Bewertungseigenschaften ein:

  ![Bewertungseigenschaften](./media/resources-faq/discount.png)

### <a name="whats-the-difference-between-as-on-premises-sizing-and-performance-based-sizing"></a>Was ist der Unterschied zwischen den Größenanpassungen „Wie lokal“ und „Leistungsbasiert“?

Bei der lokalen Größenanpassung berücksichtigt Azure Migrate keine VM-Leistungsdaten. Die VMs werden basierend auf der lokalen Konfiguration dimensioniert. Bei der leistungsbasierten Größenanpassung basiert diese auf Auslastungsdaten.

Gehen Sie als Beispiel von einer lokalen VM mit 4 Kernen und 8 GB Arbeitsspeicher sowie einer CPU- und Arbeitsspeicherauslastung von jeweils 50 % aus. Für diesen virtuellen Computer wird bei der lokalen Größenanpassung eine Azure-VM-SKU mit 4 Kernen und 8 GB Arbeitsspeicher empfohlen. Bei der leistungsbasierten Größenanpassung wird jedoch eine VM-SKU mit 2 Kernen und 4 GB Arbeitsspeicher empfohlen, da auch der Auslastungsprozentsatz berücksichtigt wird.

Entsprechend hängt die Größenanpassung der Datenträger von zwei Bewertungseigenschaften ab: dem Größenkriterium und dem Speichertyp. Wenn das Dimensionierungskriterium „Leistungsbasiert“ ist und der Speichertyp „Automatisch“ lautet, berücksichtigt Azure Migrate die IOPS- und Durchsatzwerte des Datenträgers, um den Typ des Zieldatenträgers zu ermitteln (Standard oder Premium).

Wenn das Größenkriterium „Leistungsbasiert“ und der Speichertyp „Premium“ ist, empfiehlt Azure Migrate einen Premium-Datenträger. Die Premium-SKU für den Datenträger wird basierend auf der Größe der lokalen Festplatte ausgewählt. Die gleiche Logik wird verwendet, um die Größenanpassung für Datenträger durchzuführen, wenn das Größenkriterium „Wie lokal“ verwendet wird und der Speichertyp „Standard“ oder „Premium“ lautet.

### <a name="what-impact-does-performance-history-and-utilization-percentile-have-on-size-recommendations"></a>Welche Auswirkungen haben der Leistungsverlauf und der Quantilwert der Auslastung auf die Größenempfehlungen?

Diese Eigenschaften gelten nur für die leistungsbasierte Größenanpassung.

Azure Migrate sammelt den Leistungsverlauf von lokalen Computern und nutzt diese Daten, um die VM-Größe und den Datenträgertyp in Azure zu empfehlen.

Die Appliance erstellt für die lokale Umgebung kontinuierlich Profildaten, um alle 20 Sekunden in Echtzeit Auslastungsdaten erfassen zu können. Die Appliance führt für die 20-Sekunden-Stichproben einen Rollup aus und erstellt für jeweils 15 Minuten einen Datenpunkt. Zum Erstellen des Datenpunkts wählt die Appliance den Spitzenwert aus den 20-Sekunden-Stichproben aus. Diesen Datenpunkt sendet die Appliance an Azure.

Beim Erstellen einer Bewertung in Azure berechnet Azure Migrate basierend auf der Leistungsdauer und dem Quantilwert des Leistungsverlaufs den geltenden Auslastungswert und verwendet ihn für die Größenanpassung.

Nehmen Sie beispielsweise an, dass Sie die Leistungsdauer auf einen Tag und den Quantilwert auf das 95. Perzentil festgelegt haben. Azure Migrate sortiert die 15-Minuten-Datenpunkte der Stichproben, die vom Collector für den letzten Tag übermittelt wurde, in aufsteigender Reihenfolge und wählt das 95. Perzentil als effektive Auslastung aus.

Der 95. Perzentilwert stellt sicher, dass Ausreißer ggf. ignoriert werden. Ausreißer können eingeschlossen werden, indem Sie das 99. Perzentil verwenden. Falls Sie die Spitzenauslastung für den Zeitraum auswählen und keine Ausreißer auslassen möchten, sollten Sie das 99. Perzentil verwenden.

### <a name="what-is-dependency-visualization"></a>Was ist die Visualisierung von Abhängigkeiten?

Durch die Visualisierung von Abhängigkeiten können Sie Gruppen von VMs für die Migration mit größerer Sicherheit bewerten. Sie hilft Ihnen, die Abhängigkeiten zwischen Computern zu überprüfen, bevor Sie eine Bewertung durchführen. Die Visualisierung von Abhängigkeiten trägt dazu bei, dass nichts übersehen wird. Dadurch vermeiden Sie unerwartete Ausfälle bei der Migration zu Azure. Azure Migrate verwendet die Dienstzuordnungslösung in Azure Monitor-Protokollen, um die Visualisierung von Abhängigkeiten zu ermöglichen.

> [!NOTE]
> Die Visualisierung von Abhängigkeiten ist in Azure Government nicht verfügbar.

### <a name="do-i-need-to-pay-to-use-dependency-visualization"></a>Ist die Visualisierung von Abhängigkeiten kostenpflichtig?

Nein. Weitere Informationen finden Sie unter [Azure Migrate – Preise](https://azure.microsoft.com/pricing/details/azure-migrate/).

### <a name="do-i-need-to-install-anything-for-dependency-visualization"></a>Muss ich für die Visualisierung von Abhängigkeiten Installationen durchführen?

Sie müssen für die Verwendung der Visualisierung von Abhängigkeiten Agents auf alle lokalen Computer, die Sie bewerten möchten, herunterladen und dort installieren.

Sie müssen die folgenden Agents auf jedem Computer installieren:
- [Microsoft Monitoring Agent (MMA)](https://docs.microsoft.com/azure/log-analytics/log-analytics-agent-windows)
- [Dependency-Agent](../azure-monitor/platform/agents-overview.md#dependency-agent)
- Falls Sie über Computer ohne Internetverbindung verfügen, ist es erforderlich, auf diesen das Log Analytics-Gateway herunterzuladen und zu installieren.

Sie benötigen diese Agents nur, wenn Sie die Visualisierung von Abhängigkeiten verwenden.

### <a name="can-i-use-an-existing-workspace-for-dependency-visualization"></a>Kann ich für die Visualisierung von Abhängigkeiten einen vorhandenen Arbeitsbereich verwenden?

Ja, Sie können einen vorhandenen Arbeitsbereich an das Migrationsprojekt anfügen und für die Visualisierung von Abhängigkeiten nutzen. Weitere Informationen finden Sie unter „Funktionsweise“ im Artikel zur [Visualisierung von Abhängigkeiten](concepts-dependency-visualization.md#how-does-it-work).

### <a name="can-i-export-the-dependency-visualization-report"></a>Kann ich den Bericht über die Abhängigkeitsvisualisierung exportieren?

Nein, Visualisierungen von Abhängigkeiten können nicht exportiert werden. Da Azure Migrate zur Visualisierung von Abhängigkeiten die Dienstzuordnung verwendet, können Sie die Abhängigkeiten jedoch mithilfe der [Dienstzuordnungs-REST-API](https://docs.microsoft.com/rest/api/servicemap/machines/listconnections) in einem JSON-Format abrufen.

### <a name="how-can-i-automate-the-installation-of-microsoft-monitoring-agent-mma-and-the-dependency-agent"></a>Wie kann ich die Installation von Microsoft Monitoring Agent (MMA) und des Dependency-Agents automatisieren?

Verwenden Sie dieses [Skript für die Installation der Agents](https://docs.microsoft.com/azure/monitoring/monitoring-service-map-configure#installation-script-examples). [Befolgen Sie diese Anweisungen](https://docs.microsoft.com/azure/azure-monitor/platform/log-analytics-agent#install-and-configure-agent), um MMA über die Befehlszeile oder automatisiert zu installieren. Für MMA verwenden Sie [dieses Skript](https://gallery.technet.microsoft.com/scriptcenter/Install-OMS-Agent-with-2c9c99ab).

Zusätzlich zu Skripts können Sie auch Bereitstellungstools wie System Center Configuration Manager und [Intigua](https://www.intigua.com/getting-started-intigua-for-azure-migration) zum Bereitstellen der Agents verwenden.

### <a name="what-operating-systems-are-supported-by-mma"></a>Welche Betriebssysteme werden von MMA unterstützt?

- Überprüfen Sie die Liste der [von MMA unterstützten Windows-Betriebssysteme](https://docs.microsoft.com/azure/log-analytics/log-analytics-concept-hybrid#supported-windows-operating-systems).
- Überprüfen Sie die Liste der [von MMA unterstützten Linux-Betriebssysteme](https://docs.microsoft.com/azure/log-analytics/log-analytics-concept-hybrid#supported-linux-operating-systems).

### <a name="what-operating-systems-are-supported-by-the-dependency-agent"></a>Welche Betriebssysteme werden vom Dependency-Agent unterstützt?

- Überprüfen Sie die Liste der [vom Dependency-Agent unterstützten Windows-Betriebssysteme](https://docs.microsoft.com/azure/monitoring/monitoring-service-map-configure#supported-windows-operating-systems).
- Überprüfen Sie die List der [vom Dependency-Agent unterstützten Linux-Betriebssysteme](https://docs.microsoft.com/azure/monitoring/monitoring-service-map-configure#supported-linux-operating-systems).

### <a name="can-i-visualize-dependencies-in-azure-migrate-for-more-than-an-hour"></a>Kann ich Abhängigkeiten in Azure Migrate für länger als eine Stunde visualisieren?
Nein. Sie können Abhängigkeiten bis zu einer Stunde visualisieren. Sie können zu einem bestimmten Datum im Verlauf zurückkehren (höchstens einen Monat), aber die maximale Dauer der Visualisierung beträgt eine Stunde. So können Sie beispielsweise mithilfe des Zeitraums im Abhängigkeitsdiagramm Abhängigkeiten für gestern, jedoch nur für ein Zeitfenster von einer Stunde anzeigen. Sie können allerdings zum [Abfragen der Abhängigkeitsdaten](https://docs.microsoft.com/azure/migrate/how-to-create-group-machine-dependencies) über einen längeren Zeitraum Azure Monitor-Protokolle verwenden.

### <a name="can-i-use-dependency-visualization-for-groups-that-contain-more-than-10-vms"></a>Kann ich die Visualisierung von Abhängigkeiten für Gruppen von mehr als 10 VMs verwenden?
Sie können [Abhängigkeiten für Gruppen visualisieren](https://docs.microsoft.com/azure/migrate/how-to-create-group-dependencies), die bis zu zehn VMs umfassen. Falls eine Gruppe aus mehr als 10 VMs besteht, sollten Sie die Gruppe in kleinere Gruppen aufteilen und die Abhängigkeiten anschließend visualisieren.

## <a name="azure-migrate-server-migration"></a>Azure Migrate-Servermigration

### <a name="whats-the-difference-between-azure-migrate-server-migration-and-azure-site-recovery"></a>Was ist der Unterschied zwischen der Azure Migrate-Servermigration und Azure Site Recovery?

Die Azure Migrate-Servermigration nutzt die Replikations-Engine von Site Recovery für die Agent-basierte Migration von VMware-VMs, die Migration von Hyper-V-VMs und die Migration physischer Server zu Azure. Die Option zur Migration von VMware-VMs ohne Agent ist nativ in die Servermigration integriert.

## <a name="next-steps"></a>Nächste Schritte
Sehen Sie sich die [Übersicht zu Azure Migrate](migrate-services-overview.md) an.
