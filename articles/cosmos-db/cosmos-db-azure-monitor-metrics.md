---
title: Abrufen von Azure Cosmos DB-Metriken aus Azure Monitor
description: Erfahren Sie, wie Sie verschiedene Kategorien von Azure Cosmos DB-Metriken aus Azure Monitor über das Azure-Portal anzeigen.
author: SnehaGunda
ms.author: sngun
ms.service: cosmos-db
ms.topic: conceptual
ms.date: 09/01/2019
ms.openlocfilehash: 905eca99c137af2fd40a1243de8fabd15314477c
ms.sourcegitcommit: c2e7595a2966e84dc10afb9a22b74400c4b500ed
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 10/05/2019
ms.locfileid: "71973738"
---
# <a name="monitor-and-debug-azure-cosmos-db-metrics-from-azure-monitor"></a>Überwachen und Abrufen von Azure Cosmos DB-Metriken aus Azure Monitor

Sie können Azure Cosmos DB-Metriken über die Azure Monitor-API anzeigen. Azure Monitor bietet verschiedene Methoden für die Interaktion mit Metriken, z. B. das Azure-Portal, den Zugriff über die REST-API oder die Abfrage über PowerShell oder CLI. Azure Cosmos DB-Metriken sind numerische Werte mit geringer Wartezeit, die standardmäßig im 1-Minuten-Takt erfasst werden. Sie können diese Metriken auch aggregieren. Diese Metriken sind in der Lage, Szenarien in Echtzeit zu unterstützen.  

Dieser Artikel beschreibt die verschiedenen Azure Cosmos DB-Metriken, die Sie mithilfe des Azure-Portals in Azure Monitor anzeigen können. Informationen zu allgemeinen Anwendungsfällen und zum Analysieren und Debuggen dieser Probleme mithilfe von Azure Cosmos DB-Metriken finden Sie im Artikel [Überwachen und Debuggen mit Metriken in Azure Cosmos DB](use-metrics.md). Sie verwenden eins Ihrer vorhandenen Azure Cosmos-Konten und zeigen die verschiedenen Metriken auf Datenbank-, Container-, Regions-, Anforderungs- oder Vorgangsebene an. Stellen Sie daher sicher, dass Sie über ein Azure Cosmos-Konto mit Beispieldaten verfügen und CRUD-Vorgänge für diese Daten ausführen.

## <a name="view-metrics-from-azure-portal"></a>Anzeigen von Metriken über das Azure-Portal

1. Melden Sie sich beim [Azure-Portal](https://portal.azure.com/) an.

1. Wählen Sie auf der Navigationsleiste auf der linken Seite die Option **Monitor** und anschließend **Metrik** aus.

   ![Bereich „Metriken“ in Azure Monitor](./media/cosmos-db-azure-monitor-metrics/monitor-metrics-blade.png)

1. Klicken Sie im Bereich **Metriken** auf **Ressource auswählen**. Wählen Sie dann das erforderliche **Abonnement** und die **Ressourcengruppe** aus. Wählen Sie unter **Ressourcentyp** die Option **Azure Cosmos DB-Konten** aus. Wählen Sie dann eins der vorhandenen Azure Cosmos-Konten und anschließend **Anwenden** aus. 

   ![Auswählen eines Cosmos DB-Kontos zum Anzeigen von Metriken](./media/cosmos-db-azure-monitor-metrics/select-cosmosdb-account.png)

1. Als Nächstes können Sie eine Metrik aus der Liste der verfügbaren Metriken auswählen. Sie können spezifische Metriken für Anforderungseinheiten, Speicher, Wartezeit, Verfügbarkeit, Cassandra usw. auswählen. Ausführliche Informationen zu allen verfügbaren Metriken in dieser Liste finden Sie in diesem Artikel im Abschnitt [Metriken nach Kategorie](#metrics-by-category). In diesem Beispiel wählen Sie **Anforderungseinheiten** und als Aggregationswert **Mittelw.** aus. 

   Zusätzlich zu diesen Angaben können Sie auch **Zeitbereich** und **Zeitgranularität** für die Metriken auswählen. Sie können Metriken maximal für die letzten 30 Tage anzeigen.  Nach Anwendung des Filters wird ein darauf basierendes Diagramm angezeigt. Sie sehen die durchschnittliche Anzahl der pro Minute verbrauchten Anforderungseinheiten für den ausgewählten Zeitraum.  

   ![Auswählen einer Metrik über das Azure-Portal](./media/cosmos-db-azure-monitor-metrics/metric-types.png)

## <a name="add-filters-to-metrics"></a>Hinzufügen von Filtern zu Metriken

Sie können Metriken und das angezeigte Diagramm auch nach bestimmten Werten für **CollectionName**, **DatabaseName**, **OperationType**, **Region** und **StatusCode** filtern. Wählen Sie zum Filtern der Metriken **Filter hinzufügen** und dann die erforderliche Eigenschaft aus, etwa **OperationType**. Wählen Sie anschließend einen Wert aus, beispielsweise **Query**. Im Diagramm werden die Anforderungseinheiten angezeigt, die für den Abfragevorgang im ausgewählten Zeitraum verbraucht wurden. Die über eine gespeicherte Prozedur ausgeführten Vorgänge werden nicht protokolliert und sind daher nicht unter der Metrik „OperationType“ verfügbar.

![Hinzufügen eines Filters zum Auswählen der Metrikgranularität](./media/cosmos-db-azure-monitor-metrics/add-metrics-filter.png)

Sie können Metriken mit der Option **Apply splitting** (Aufteilung anwenden) gruppieren. Beispielsweise können Sie die Anforderungseinheiten nach Vorgangstyp gruppieren und das Diagramm für alle Vorgänge auf einmal anzeigen, wie in der folgenden Abbildung dargestellt: 

![Hinzufügen eines Filters zum Anwenden der Aufteilung](./media/cosmos-db-azure-monitor-metrics/apply-metrics-splitting.png)


## <a id="metrics-by-category"></a>Metriken nach Kategorie

### <a name="request-metrics"></a>Anforderungsmetriken
            
|Metrik (Metrikanzeigename)|Einheit (Aggregationstyp) |BESCHREIBUNG|Dimensionen| Zeitgranularitäten| Zuordnung von Legacymetriken | Verwendung |
|---|---|---|---| ---| ---| ---|
| TotalRequests (Anforderungen insgesamt) | Count (Anzahl) | Anzahl von gesendeten Anforderungen| DatabaseName, CollectionName, Region, StatusCode| Alle | TotalRequests, Http 2xx, Http 3xx, Http 400 "," Http 401, Interner Serverfehler, Dienst nicht verfügbar, Anforderungen gedrosselt, Anforderungen pro Sekunde | Wird verwendet, um Anforderungen nach Statuscode zu überwachen, Container mit einer Granularität von einer Minute. Um einen Durchschnittswert für die Anforderungen pro Sekunde zu erhalten, verwenden Sie die Zähl-Aggregation in einer Minute, und teilen Sie sie durch 60. |
| MetadataRequests (Metadatenanforderungen) |Count (Anzahl) | Anzahl der Metadatenanforderungen. Azure Cosmos DB unterhält einen Container mit Systemmetadaten für jedes Konto, wodurch Sie Sammlungen, Datenbanken usw. und deren Konfigurationen ohne anfallende Kosten auflisten können. | DatabaseName, CollectionName, Region, StatusCode| Alle| |Wird verwendet, um die Drosselung aufgrund von Metadatenanforderungen zu überwachen.|
| MongoRequests (Mongo-Anforderungen) | Count (Anzahl) | Anzahl der ausgegebenen Mongo-Anforderungen | DatabaseName, CollectionName, Region, CommandName, ErrorCode| Alle |Mongo-Abfrage-Anforderungsrate, Mongo-Aktualisieren-Anforderungsrate, Mongo-Löschen-Anforderungsrate, Mongo-Einfügen-Anforderungsrate, Mongo-Zählen-Anforderungsrate| Wird verwendet, um Mongo-Anforderungsfehler zu überwachen, Verwendungen pro Befehlstyp. |

### <a name="request-unit-metrics"></a>Metriken für Anforderungseinheiten

|Metrik (Metrikanzeigename)|Einheit (Aggregationstyp)|BESCHREIBUNG|Dimensionen| Zeitgranularitäten| Zuordnung von Legacymetriken | Verwendung |
|---|---|---|---| ---| ---| ---|
| MongoRequestCharge (Kosten der Mongo-Anforderung) | Count (Gesamt) |Verbrauchte Mongo-Anforderungseinheiten| DatabaseName, CollectionName, Region, CommandName, ErrorCode| Alle |Mongo-Abfrage-Anforderungsgebühr, Mongo-Aktualisieren-Anforderungsgebühr, Mongo-Löschen-Anforderungsgebühr, Mongo-Einfügen-Anforderungsgebühr, Mongo-Zählen-Anforderungsgebühr| Wird verwendet, um die Mongo-Ressourcen-RUs in einer Minute zu überwachen.|
| TotalRequestUnits (Anforderungseinheiten gesamt)| Count (Gesamt) | Verbrauchte Anforderungseinheiten| DatabaseName, CollectionName, Region, StatusCode |Alle| TotalRequestUnits| Wird verwendet, um die gesamte RU-Nutzung mit einer Granularität von einer Minute zu überwachen. Um einen Durchschnittswert für die verbrauchten RU zu erhalten, verwenden Sie die Gesamtaggregation in einer Minute, und teilen Sie sie durch 60.|
| ProvisionedThroughput (Bereitgestellter Durchsatz)| Count (Maximum) |Bereitgestellter Durchsatz bei Containergranularität| DatabaseName, ContainerName| 5M| | Wird verwendet, um den bereitgestellten Durchsatz pro Container zu überwachen.|

### <a name="storage-metrics"></a>Speichermetrik

|Metrik (Metrikanzeigename)|Einheit (Aggregationstyp)|BESCHREIBUNG|Dimensionen| Zeitgranularitäten| Zuordnung von Legacymetriken | Verwendung |
|---|---|---|---| ---| ---| ---|
| AvailableStorage (Verfügbarer Speicher) |Bytes (Gesamt) | Gemeldeter Gesamtspeicher mit 5-Minuten-Granularität pro Region| DatabaseName, CollectionName, Region| 5M| Verfügbarer Speicher| Wird verwendet, um die verfügbare Speicherkapazität zu überwachen (gilt nur für feste Speichersammlungen). Die Mindestgranularität sollte 5 Minuten betragen.| 
| DataUsage (Datennutzung) |Bytes (Gesamt) |Gemeldete Gesamtdatennutzung mit 5-Minuten-Granularität pro Region| DatabaseName, CollectionName, Region| 5M |Datengröße | Wird verwendet, um den gesamten Datenverbrauch für Container und Region zu überwachen. Die Mindestgranularität sollte 5 Minuten betragen.|
| IndexUsage (Indexnutzung) | Bytes (Gesamt) |Gemeldete Gesamtindexnutzung mit 5-Minuten-Granularität pro Region| DatabaseName, CollectionName, Region| 5M| Indexgröße| Wird verwendet, um den gesamten Datenverbrauch für Container und Region zu überwachen. Die Mindestgranularität sollte 5 Minuten betragen. |
| DocumentQuota (Dokumentenkontingent) | Bytes (Gesamt) | Gemeldetes Gesamtspeicherkontingent mit 5-Minuten-Granularität pro Region| DatabaseName, CollectionName, Region| 5M |Speicherkapazität| Wird verwendet, um das gesamte Kontingent für Container und Region zu überwachen. Die Mindestgranularität sollte 5 Minuten betragen.|
| DocumentCount (Dokumentanzahl) | Count (Gesamt) |Gemeldete Gesamtdokumentanzahl mit 5-Minuten-Granularität pro Region| DatabaseName, CollectionName, Region| 5M |Dokumentanzahl|Wird verwendet, um das Gesamtanzahl der Dokumente für Container und Region zu überwachen. Die Mindestgranularität sollte 5 Minuten betragen.|

### <a name="latency-metrics"></a>Latenzmetriken

|Metrik (Metrikanzeigename)|Einheit (Aggregationstyp)|BESCHREIBUNG|Dimensionen| Zeitgranularitäten| Verwendung |
|---|---|---|---| ---| ---|
| ReplicationLatency (Replikationswartezeit)| MilliSeconds (Minimum, Maximum, Durchschnitt) | P99-Replikationswartezeit für Quell- und Zielregionen für geofähiges Konto| SourceRegion, TargetRegion| Alle | Wird verwendet, um die P99-Replikationswartezeit zwischen zwei beliebigen Regionen für ein georepliziertes Konto zu überwachen. |


### <a name="availability-metrics"></a>Verfügbarkeitsmetriken

|Metrik (Metrikanzeigename) |Einheit (Aggregationstyp)|BESCHREIBUNG| Zeitgranularitäten| Zuordnung von Legacymetriken | Verwendung |
|---|---|---|---| ---| ---|
| ServiceAvailability (Dienstverfügbarkeit)| Prozent (Minimum, Maximum) | Verfügbarkeit der Kontoanforderungen mit einer Granularität von einer Stunde| 1H | Dienstverfügbarkeit | Stellt den Prozentsatz der insgesamt übergebenen Anforderungen dar. Eine Anforderung gilt als fehlgeschlagen aufgrund eines Systemfehlers, wenn der Statuscode 410, 500 oder 503 lautet. Wird verwendet, um die Verfügbarkeit des Kontos bei einer Granularität von einer Stunde zu überwachen. |


### <a name="cassandra-api-metrics"></a>Cassandra-API-Metriken

|Metrik (Metrikanzeigename)|Einheit (Aggregationstyp)|BESCHREIBUNG|Dimensionen| Zeitgranularitäten| Verwendung |
|---|---|---|---| ---| ---|
| CassandraRequests (Cassandra-Anforderungen) | Count (Anzahl) | Anzahl der ausgeführten Cassandra-API-Anforderungen| DatabaseName, CollectionName, ErrorCode, Region, OperationType, ResourceType| Alle| Wird verwendet, um Cassandra-Anforderungen mit einer Granularität von einer Minute zu überwachen. Um einen Durchschnittswert für die Anforderungen pro Sekunde zu erhalten, verwenden Sie die Zähl-Aggregation in einer Minute, und teilen Sie sie durch 60.|
| CassandraRequestCharges (Gebühren für Cassandra-Anforderungen) | Count (Summe, Minimum, Maximum, Durchschnitt) | Cassandra-API-Anforderungen verbrauchte Anforderungseinheiten| DatabaseName, CollectionName, Region, OperationType, ResourceType| Alle| Wird verwendet, um die RUs zu überwachen, die pro Minute von einem Cassandra API-Konto genutzt werden.|
| CassandraConnectionClosures (Abschluss von Cassandra-Verbindungen) |Count (Anzahl) |Anzahl der geschlossenen Cassandra-Verbindungen| ClosureReason, Region| Alle | Wird verwendet, um die Verbindung zwischen Clients und der Cassandra-API von Azure Cosmos DB zu überwachen.|

## <a name="next-steps"></a>Nächste Schritte

* [Anzeigen und Überwachen von Metriken über den Bereich „Metriken“ eines Azure Cosmos DB-Kontos](use-metrics.md)

* [Diagnoseprotokollierung in Azure Cosmos DB](logging.md)
