---
title: Grenzwerte für Azure Cosmos DB Gremlin
description: Referenzdokumentation zu Laufzeitbeschränkungen der Graph-Engine
author: olignat
ms.service: cosmos-db
ms.subservice: cosmosdb-graph
ms.topic: reference
ms.date: 09/10/2019
ms.author: olignat
ms.openlocfilehash: f1fe4cfac22a651f44338986d0a767fe3e1a360b
ms.sourcegitcommit: d70c74e11fa95f70077620b4613bb35d9bf78484
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 09/11/2019
ms.locfileid: "70910745"
---
# <a name="azure-cosmos-db-gremlin-limits"></a>Azure Cosmos DB: Gremlin-Grenzwerte
In diesem Artikel werden die Grenzwerte der Azure Cosmos DB Gremlin-Engine beschrieben, und es wird erläutert, wie sie sich auf Kundendurchläufe auswirken können.

Da Cosmos DB Gremlin auf der Cosmos DB-Infrastruktur basiert, gelten alle Grenzwerte von [Azure Cosmos DB](https://docs.microsoft.com/azure/cosmos-db/concepts-limits) auch hier. 

## <a name="limits"></a>Einschränkungen

Wenn der Gremlin-Grenzwert erreicht wird, wird der Durchlauf mit „**x-ms-status-code** = 429“ abgebrochen. Dies ist ein Hinweis auf einen Drosselungsfehler.

**Ressource**    | **Standardlimit** | **Erklärung**
--- | --- | ---
*Arbeitsspeicher pro Anforderung* | **2 GB** | Maximaler Arbeitsspeicher, der von einer Anforderung während der Verarbeitung genutzt werden kann. Anforderungen, die große Datasets berechnen müssen, verbrauchen zusätzlichen Arbeitsspeicher. Legen Sie den Bereich von Anforderungen auf kleinere Datasets fest, um zu vermeiden, dass dieser Grenzwert überschritten wird, oder verwenden Sie OLAP-Lösungen.
*Skriptlänge* | **64 KB** | Maximale Länge eines Gremlin-Durchlaufskripts pro Anforderung.
*Operatortiefe* | **400** |  Die Gesamtzahl eindeutiger Schritte eines Durchlaufs. Beispielsweise weist ```g.V().out()``` die Operatoranzahl 2 auf: V() und out(), ```g.V('label').repeat(out()).times(100)``` weist eine Operatortiefe von 3 auf: V(), repeat() und out(), weil ```.times(100)``` ein Parameter für den Operator ```.repeat()``` ist.
*Parallelitätsgrad* | **32** | Maximale Anzahl von Speicherpartitionen, die mit einer einzelnen Anforderung an die Speicherebene abgefragt werden. Graphen mit Hunderten von Partitionen werden durch diesen Grenzwert beeinträchtigt.
*Grenzwert für Wiederholungen* | **32** | Maximale Anzahl von Durchläufen, die mit dem Operator ```.repeat()``` ausgeführt werden können. Bei jeder Iteration des ```.repeat()```-Schritts wird in den meisten Fällen ein Durchlauf mit „breitem Ansatz“ ausgeführt. Dies bedeutet, dass jeder Durchlauf auf maximal 32 Hops zwischen Vertices begrenzt ist.
*Durchlauftimeout* | **30 Sekunden** | Der Durchlauf wird abgebrochen, wenn dieser Zeitraum überschritten wird. Cosmos DB Graph ist eine OLTP-Datenbank, bei der der Großteil der Durchläufe innerhalb von Millisekunden durchgeführt wird. Verwenden Sie zum Ausführen von OLAP-Abfragen für Cosmos DB Graph [Apache Spark](https://azure.microsoft.com/services/cosmos-db/) mit [Graph-Dataframes](https://spark.apache.org/docs/latest/sql-programming-guide.html#datasets-and-dataframes) und dem [Cosmos DB Spark-Connector](https://github.com/Azure/azure-cosmosdb-spark).
*Grenzwert für Prädikate* | **20** | Anzahl von ```.has()```- oder ```.hasNot()```-Schritten, die auf einen einzelnen Scheitelpunkt oder eine Kante angewendet werden. Wenn dieser Grenzwert erreicht wird, wird in der Anwendung der Fehler ```The SQL query exceeded the maximum number of joins. The allowed limit is 20``` angezeigt. Dies ist nur eine vorübergehende Unannehmlichkeit, da das zuständige Team bereits an der Beseitigung dieses Grenzwerts arbeitet. 
*Verbindungstimeout bei Leerlauf* | **5 Stunden** | Die Zeitspanne, die der Graph-Server die Websocketverbindung ohne Datenverkehr geöffnet lässt. TCP-Keep-Alive-Pakete oder HTTP-Keep-Alive-Anforderungen erweitern die Verbindungslebensdauer nicht über diesen Grenzwert hinaus. Wenn sie jedoch nicht gesendet werden, kann die zugrunde liegende Azure-Infrastruktur die Verbindung noch früher schließen. Die Cosmos DB Graph-Engine wird als im Leerlauf befindlich betrachtet, wenn keine Gremlin-Durchläufe in ihr ausgeführt werden.
*Ressourcentoken pro Stunde* | **100** | Die Anzahl eindeutiger Ressourcentoken, die von Gremlin-Clients zum Herstellen einer Verbindung mit dem Gremlin-Konto in einer Region verwendet werden. Wenn die Anwendung das stündliche eindeutige Tokenlimit überschreitet, wird `"Exceeded allowed resource token limit of 100 that can be used concurrently"` bei der nächsten Authentifizierungsanforderung zurückgegeben.

## <a name="next-steps"></a>Nächste Schritte
* [Azure Cosmos DB: Gremlin-Antwortheader](gremlin-headers.md) 
* [Azure Cosmos DB-Ressourcentoken mit Gremlin](how-to-use-resource-tokens-gremlin.md)
