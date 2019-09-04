---
title: Azure Stream Analytics-Ausgabe an Cosmos DB
description: Dieser Artikel beschreibt, wie Sie Azure Stream Analytics für die JSON-Ausgabe zum Speichern der Ausgabe in Azure Cosmos DB verwenden können, um eine Datenarchivierung und Abfragen unstrukturierter JSON-Daten mit geringer Latenz zu ermöglichen.
services: stream-analytics
author: mamccrea
ms.author: mamccrea
ms.reviewer: mamccrea
ms.service: stream-analytics
ms.topic: conceptual
ms.date: 01/11/2019
ms.custom: seodec18
ms.openlocfilehash: 52bbb52b13a3606e3ddc8deca2da8505233c9352
ms.sourcegitcommit: 388c8f24434cc96c990f3819d2f38f46ee72c4d8
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 08/27/2019
ms.locfileid: "70062014"
---
# <a name="azure-stream-analytics-output-to-azure-cosmos-db"></a>Azure Stream Analytics-Ausgabe an Azure Cosmos DB  
Stream Analytics kann für die JSON-Ausgabe auf [Azure Cosmos DB](https://azure.microsoft.com/services/documentdb/) ausgerichtet werden, was eine Datenarchivierung und Abfragen unstrukturierter JSON-Daten mit geringer Latenz ermöglicht. In diesem Dokument werden einige bewährte Implementierungsmethoden für diese Konfiguration behandelt.

Falls Sie noch nicht mit Cosmos DB vertraut sind, sehen Sie sich zum Einstieg den [Lernpfad für Azure Cosmos DB](https://azure.microsoft.com/documentation/learning-paths/documentdb/) an. 

> [!Note]
> Derzeit unterstützt Azure Stream Analytics nur die Verbindung mit Cosmos DB nur über die **SQL-API**.
> Andere Azure Cosmos DB-APIs werden noch nicht unterstützt. Wenn Sie Azure Stream Analytics auf die mit anderen APIs erstellten Azure Cosmos DB-Konten verweisen, werden die Daten unter Umständen nicht richtig gespeichert. 

## <a name="basics-of-cosmos-db-as-an-output-target"></a>Grundlagen von Cosmos DB als Ausgabeziel
Die Azure Cosmos DB-Ausgabe in Stream Analytics ermöglicht das Schreiben der Ergebnisse Ihrer Datenstromverarbeitung als JSON-Ausgabe in Ihre(n) Cosmos DB-Container. Stream Analytics erstellt keine Container in Ihrer Datenbank, sondern fordert deren vorherige Erstellung an. Dies ist so, damit die Abrechnungskosten von Cosmos DB-Containern von Ihnen gesteuert werden können und Sie die Leistung, Konsistenz und Kapazität Ihrer Container direkt mithilfe der [Cosmos DB-APIs](https://msdn.microsoft.com/library/azure/dn781481.aspx) optimieren können.

> [!Note]
> Sie müssen der Liste der zulässigen IP-Adressen Ihrer Azure Cosmos DB-Firewall „0.0.0.0“ hinzufügen.

Nachstehend sind einige der Cosmos DB-Containeroptionen beschrieben.

## <a name="tune-consistency-availability-and-latency"></a>Optimieren von Konsistenz, Verfügbarkeit und Latenz
Entsprechend Ihren Anwendungsanforderungen lässt Azure Cosmos DB das Optimieren der Datenbank und Container sowie Kompromisse zwischen Konsistenz, Verfügbarkeit, Latenz und Durchsatz zu. Abhängig von den Lesekonsistenzebenen, die Ihr Szenario hinsichtlich Lese- und Schreiblatenz benötigt, können Sie in Ihrem Datenbankkonto eine Konsistenzebene wählen. Den Durchsatz können Sie verbessern, indem Sie Anforderungseinheiten (Request Units, RUs) für den Container zentral hochskalieren. Außerdem aktiviert Azure Cosmos DB für jeden CRUD-Vorgang in Ihrem Container die synchrone Indizierung. Dies ist eine andere nützliche Option zum Steuern der Schreib-/Leseleistung in Azure Cosmos DB. Weitere Informationen finden Sie im Artikel [Ändern der Datenbank- und Abfragekonsistenzebenen](../cosmos-db/consistency-levels.md).

## <a name="upserts-from-stream-analytics"></a>Einfügen/Aktualisieren über Stream Analytics
Die Stream Analytics-Integration mit Azure Cosmos DB ermöglicht das Einfügen oder Aktualisieren von Datensätzen in Ihrem Container auf der Grundlage einer angegebenen Dokument-ID-Spalte. Dies wird auch als *Upsert*(Kunstwort aus Update und Insert) bezeichnet.

Stream Analytics verwendet einen optimistischen Upsert-Ansatz, bei dem Aktualisierungen nur erfolgen, wenn eine Einfügung aufgrund eines Dokument-ID-Konflikts keinen Erfolg hat. Bei Kompatibilitätsgrad 1.0 wird diese Aktualisierung mit einem PATCH-Befehl ausgeführt. Dadurch sind Teilaktualisierungen des Dokuments möglich, was bedeutet, dass das Hinzufügen neuer Eigenschaften oder Austauschen einer vorhandenen Eigenschaft schrittweise erfolgt. Änderungen der Werte von Arrayeigenschaften in Ihrem JSON-Dokument können jedoch dazu führen, dass das gesamte Array überschrieben wird, d.h., nicht zusammengeführt wird. Für Kompatibilitätsgrad 1.2 wurde das Upsert-Verhalten so geändert, dass das Dokument eingefügt oder ersetzt wird. Dies ist im folgenden Abschnitt „Kompatibilitätsgrad 1.2“ ausführlich beschrieben.

Wenn das eingehende JSON-Dokument bereits ein ID-Feld hat, wird dieses Feld automatisch als Dokument-ID-Spalte in Cosmos DB verwendet, und alle nachfolgenden Schreibvorgänge werden entsprechend behandelt. Daraus resultiert eines der folgenden Szenarien:
- Eindeutige IDs lösen eine Einfügung aus
- Doppelte IDs und die Angabe „ID“ für „Dokument-ID“ lösen einen Upsertvorgang aus
- Doppelte IDs und eine nicht angegebene Dokument-ID lösen einen Fehler aus, der nach dem ersten Dokument auftritt

Wenn Sie <i>alle</i> Dokumente speichern möchten, einschließlich derjenigen mit einer doppelten ID, benennen Sie das ID-Feld in Ihrer Abfrage um (mit dem AS-Schlüsselwort). Lassen Sie Cosmos DB das ID-Feld erstellen, oder ersetzen Sie die ID durch den Wert einer anderen Spalte (mit dem AS-Schlüsselwort oder mit der Einstellung „Dokument-ID“).

## <a name="data-partitioning-in-cosmos-db"></a>Partitionieren von Daten in CosmosDB
[Unbegrenzte](../cosmos-db/partition-data.md) Azure Cosmos DB-Container werden als Ansatz für die Partitionierung Ihrer Daten empfohlen, da Azure Cosmos DB Partitionen automatisch basierend auf Ihrer Arbeitsauslastung skaliert. Beim Schreiben in unbegrenzte Container verwendet Stream Analytics so viele parallele Writer wie im vorherigen Abfrageschritt oder im eingegebenen Partitionierungsschema.
> [!NOTE]
> Derzeit unterstützt Azure Stream Analytics nur unbegrenzte Container mit Partitionsschlüsseln auf der obersten Ebene. Beispielsweise wird `/region` unterstützt. Geschachtelte Partitionsschlüssel (z.B. `/region/name`) werden nicht unterstützt. 

Abhängig von Ihrer Partitionsschlüsselauswahl wird möglicherweise diese _Warnung_ angezeigt:

`CosmosDB Output contains multiple rows and just one row per partition key. If the output latency is higher than expected, consider choosing a partition key that contains at least several hundred records per partition key.`

Es ist wichtig, eine Partitionsschlüsseleigenschaft zu wählen, die eine Reihe von unterschiedlichen Werten hat und Ihnen ermöglicht, Ihre Workload gleichmäßig auf diese Werte zu verteilen. Als natürliches Artefakt der Partitionierung werden Anfragen mit dem gleichen Partitionsschlüssel durch den maximalen Durchsatz einer einzelnen Partition begrenzt. Darüber hinaus ist die Speichergröße für Dokumente mit dem gleichen Partitionsschlüssel auf 10 GB beschränkt. Ein idealer Partitionsschlüssel ist einer, der häufig als Filter in Ihren Abfragen erscheint und genügend Kardinalität besitzt, um sicherzustellen, dass Ihre Lösung skalierbar ist.

Ein Partitionsschlüssel ist auch die Grenze für Transaktionen in DocumentDB gespeicherter Prozeduren und Trigger. Sie sollten den Partitionsschlüssel so auswählen, dass Dokumente, die gemeinsam in Transaktionen auftreten, denselben Partitionsschlüsselwert haben. Der Artikel [Partitionierung in Azure Cosmos DB](../cosmos-db/partitioning-overview.md) bietet ausführlichere Informationen zur Auswahl eines Partitionsschlüssels.

Bei festen Azure Cosmos DB-Containern lässt Stream Analytics kein zentrales oder horizontales Hochskalieren zu, sobald diese vollständig sind. Sie haben eine Obergrenze von 10 GB und einen Durchsatz von 10.000 RU/s.  Zum Migrieren der Daten aus einem festen Container zu einem unbegrenzten Container (beispielsweise mit mindestens 1.000 RU/s und einem Partitionsschlüssel) müssen Sie das [Datenmigrationstool](../cosmos-db/import-data.md) oder die [Änderungsfeedbibliothek](../cosmos-db/change-feed.md) verwenden.

Die Möglichkeit, in mehrere feste Container schreiben zu können, ist veraltet und wird nicht zur horizontalen Skalierung Ihres Stream Analytics-Auftrags empfohlen.

## <a name="improved-throughput-with-compatibility-level-12"></a>Verbesserter Durchsatz mit Kompatibilitätsgrad 1.2
Bei Kompatibilitätsgrad 1.2 unterstützt Stream Analytics die native Integration in den Massenschreibvorgang in Cosmos DB. Dies ermöglicht ein effektives Schreiben in Cosmos DB mit maximaler Durchsatzleistung und effizientem Umgang mit Drosselungsanforderungen. Der verbesserte Schreibmechanismus ist unter einem neuen Kompatibilitätsgrad aufgrund eines Unterschieds bei Upsert-Vorgängen verfügbar.  Vor Kompatibilitätsgrad 1.2 sieht das Upsert-Verhalten ein Einfügen oder Zusammenführen des Dokuments vor. Für Kompatibilitätsgrad 1.2 wurde das Upsert-Verhalten so geändert, dass das Dokument eingefügt oder ersetzt wird.

Vor Kompatibilitätsgrad 1.2 wird eine benutzerdefinierte gespeicherte Prozedur verwendet, um Upsert-Vorgänge in einem Massenvorgang gemäß dem Partitionsschlüssel in Cosmos DB durchzuführen, wobei ein Batch als Transaktion geschrieben wird. Sogar wenn ein einzelner Datensatz einen vorübergehenden Fehler (Drosselung) aufweist, muss der ganze Batch wiederholt werden. Dadurch wurden Szenarien mit selbst angemessener Drosselung relativ langsamer. Der folgende Vergleich zeigt, wie sich solche Aufträge bei Kompatibilitätsgrad 1.2 verhalten.

Das folgende Beispiel zeigt zwei identische Stream Analytics-Aufträge, die aus der gleichen Event Hub-Eingabe lesen. Beide Stream Analytics-Aufträge sind [vollständig partitioniert](https://docs.microsoft.com/azure/stream-analytics/stream-analytics-parallelization#embarrassingly-parallel-jobs), weisen eine Passthrough-Abfrage auf und schreiben in identische Cosmos DB-Container. Die Metriken auf der linken Seite stammen aus dem Auftrag, der mit Kompatibilitätsgrad 1.0 konfiguriert ist, während die auf der rechten Seite mit Kompatibilitätsgrad 1.2 konfiguriert sind. Der Partitionsschlüssel von Cosmos DB-Containern ist eine eindeutige GUID, die aus dem Eingabeereignis stammt.

![Vergleich von Stream Analytics-Metriken](media/stream-analytics-documentdb-output/stream-analytics-documentdb-output-3.png)

Die Rate der im Event Hub eingehenden Ereignisse ist zweimal höher als die der Cosmos DB-Container (20.000 RUs), die für die Aufnahme konfiguriert sind, weshalb in Cosmos DB eine Drosselung erwartet wird. Der Auftrag mit Kompatibilitätsgrad 1.2 schreibt jedoch konsistent mit einem höheren Durchsatz (Ausgabeereignisse/Minute) und mit einer niedrigeren durchschnittlichen Auslastung von Speichereinheiten (SUs in %). In Ihrer Umgebung hängt dieser Unterschied von einigen weiteren Faktoren ab, wie z.B. Wahl des Ereignisformats, Größe der Eingangsereignisse pro Nachricht, Partitionsschlüssel, Abfrage usw.

![Vergleich von Cosmos DB-Metriken](media/stream-analytics-documentdb-output/stream-analytics-documentdb-output-2.png)

Mit Kompatibilitätsgrad 1.2 ist Stream Analytics intelligenter in der Lage, 100% des verfügbaren Durchsatzes in Cosmos DB zu nutzen, mit sehr wenigen erneuten Übermittlungen aufgrund von Drosselung/Ratenbegrenzung. Dies ermöglicht eine bessere Erfahrung für andere Workloads, wie z.B. Abfragen, die gleichzeitig auf den Container angewendet werden. Für den Fall, dass Sie ausprobieren müssen, wie ASA mit Cosmos DB als Senke für 1.000 bis 10.000 Nachrichten pro Sekunde skaliert, finden Sie hier ein [Azure-Beispielprojekt](https://github.com/Azure-Samples/streaming-at-scale/tree/master/eventhubs-streamanalytics-cosmosdb), das Ihnen dies ermöglicht.
Beachten Sie, dass der Cosmos DB-Ausgabedurchsatz bei Kompatibilitätsgrad 1.0 und 1.1 identisch ist. Da Kompatibilitätsgrad 1.2 nicht die Standardeinstellung ist, können Sie den [Kompatibilitätsgrad](https://docs.microsoft.com/azure/stream-analytics/stream-analytics-compatibility-level) für einen Stream Analytics-Auftrag über das Portal oder mithilfe des [REST-API-Aufrufs „create job“](https://docs.microsoft.com/rest/api/streamanalytics/stream-analytics-job) festlegen. Es wird *ausdrücklich empfohlen*, Kompatibilitätsgrad 1.2 in ASA mit Cosmos DB zu verwenden.



## <a name="cosmos-db-settings-for-json-output"></a>Cosmos DB-Einstellungen für die JSON-Ausgabe

Beim Erstellen einer Cosmos DB-Datenbank als Ausgabe in Stream Analytics wird eine Aufforderung zur Eingabe von Informationen eingeblendet. Dieser Abschnitt enthält eine Erklärung der Definition der Eigenschaften.

![Bildschirm mit DocumentDB als Stream Analytics-Ausgabe](media/stream-analytics-documentdb-output/stream-analytics-documentdb-output-1.png)

|Feld           | BESCHREIBUNG|
|-------------   | -------------|
|Ausgabealias    | Ein Alias zum Verweisen auf diese Ausgabe in Ihrer ASA-Abfrage.|
|Subscription    | Wählen Sie das Azure-Abonnement.|
|Konto-ID      | Der Name oder Endpunkt-URI des Azure Cosmos DB-Kontos.|
|Kontoschlüssel     | Der Schlüssel für den gemeinsamen Zugriff für das Azure Cosmos DB-Konto.|
|Datenbank        | Der Name der Azure Cosmos DB-Datenbank.|
|Containername | Der zu verwendende Containername. `MyContainer` ist eine gültige Beispieleingabe. Ein Container mit dem Namen `MyContainer` muss vorhanden sein.  |
|Dokument-ID     | Optional. Der Spaltenname in Ausgabeergebnissen, der als eindeutiger Schlüssel verwendet wird, auf dem Einfüge- oder Aktualisierungsvorgänge basieren müssen. Wenn das Feld leer gelassen wird, werden sämtliche Ereignisse eingefügt, ohne Update-Option.|

## <a name="error-handling-and-retries"></a>Fehlerbehandlung und Wiederholungsversuche

Bei einem vorübergehenden Fehler werden bei Nichtverfügbarkeit oder Drosselung des Diensts beim Senden von Ereignissen an Cosmos DB in Stream Analytics unbegrenzt viele Wiederholungen ausgeführt, um den Vorgang erfolgreich abzuschließen. Es gibt jedoch einige Fehler, für die keine Wiederholungsversuche ausgeführt werden:

- Unauthorized (HTTP-Fehlercode 401)
- NotFound (HTTP-Fehlercode 404)
- Forbidden (HTTP-Fehlercode 403)
- BadRequest (HTTP-Fehlercode 400)
