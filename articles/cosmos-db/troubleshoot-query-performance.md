---
title: Diagnostizieren und Behandeln von Problemen bei Verwendung von Azure Cosmos DB
description: Erfahren Sie, wie Sie Probleme mit Azure Cosmos DB SQL-Abfragen identifizieren, diagnostizieren und beheben können.
author: ginamr
ms.service: cosmos-db
ms.topic: troubleshooting
ms.date: 07/10/2019
ms.author: girobins
ms.subservice: cosmosdb-sql
ms.reviewer: sngun
ms.openlocfilehash: 079e8677febfe6683d4f0e60a0e7ba6b06ea549d
ms.sourcegitcommit: 64798b4f722623ea2bb53b374fb95e8d2b679318
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 07/11/2019
ms.locfileid: "67835834"
---
# <a name="troubleshoot-query-performance-for-azure-cosmos-db"></a>Behandeln von Problemen bei der Abfrageleistung für Azure Cosmos DB
In diesem Artikel wird beschrieben, wie Sie Probleme mit Azure Cosmos DB SQL-Abfragen identifizieren, diagnostizieren und beheben können. Um eine optimale Leistung bei Azure Cosmos DB-Abfragen zu erreichen, folgen Sie den folgenden Schritten zur Fehlerbehebung. 

## <a name="collocate-clients-in-same-azure-region"></a>Platzieren der Clients in der gleichen Azure-Region 
Die geringste Latenz erzielen Sie, wenn sich die aufrufende Anwendung in der gleichen Azure-Region wie der bereitgestellte Azure Cosmos DB-Endpunkt befindet. Eine Liste mit den verfügbaren Regionen finden Sie im Artikel [„Azure-Regionen“](https://azure.microsoft.com/global-infrastructure/regions/#services).

## <a name="check-consistency-level"></a>Überprüfen der Konsistenzebene
Die [Konsistenzebene](consistency-levels.md) kann sich auf die Leistung und die Kosten auswirken. Stellen Sie sicher, dass Ihre Konsistenzebene für das jeweilige Szenario geeignet ist. Weitere Informationen finden Sie unter [„Auswählen der Konsistenzebene“](consistency-levels-choosing.md).

## <a name="log-query-metrics"></a>Metrik zur Protokollabfrage
Verwenden Sie `QueryMetrics`, um Fehler bei langsamen oder teuren Anfragen zu beheben. 

  * Richten Sie `FeedOptions.PopulateQueryMetrics = true` so ein, dass `QueryMetrics` in der Antwort erscheinen.
  * Die Klasse `QueryMetrics` hat eine überladene `.ToString()`-Funktion, die aufgerufen werden kann, um die Zeichenfolge von `QueryMetrics` darzustellen. 
  * Aus den Metriken lassen sich unter anderem folgende Erkenntnisse ableiten: 
  
      * Ob eine bestimmte Komponente der Abfragepipeline ungewöhnlich lange gedauert hat (in der Größenordnung von Hunderten von Millisekunden oder mehr). 

          * Betrachten Sie `TotalExecutionTime`.
          * Wenn die `TotalExecutionTime` der Abfrage kleiner als die End-to-End-Ausführungszeit ist, dann wird die Zeit auf der Clientseite oder im Netzwerk verbraucht. Überprüfen Sie nochmals, ob der Client und die Azure-Region zusammenpassen.
      
      * Ob es in den analysierten Dokumenten falsche Positivmeldungen gab (wenn die Anzahl der Ausgabedokumente viel geringer ist als die Anzahl der abgerufenen Dokumente).  

          * Betrachten Sie `Index Utilization`.
          * `Index Utilization` = (Anzahl der zurückgegebenen Dokumente/Anzahl der geladenen Dokumente)
          * Wenn die Anzahl der zurückgegebenen Dokumente viel geringer ist als die Anzahl der geladenen Dokumente, werden falsche Positivmeldungen analysiert.
          * Schränken Sie die Anzahl der abgerufenen Dokumente mit engeren Filtervorgaben ein.  

      * Wie sich die einzelnen Roundtrips entwickelt haben (siehe `Partition Execution Timeline` aus der Zeichenfolgendarstellung von `QueryMetrics`). 
      * Ob die Anfrage eine hohe Anforderungsgebühr verursacht hat. 

Weitere Informationen finden Sie im Artikel [„Aufrufen von SQL-Abfragemetriken](profile-sql-api-query.md).
      
## <a name="tune-query-feed-options-parameters"></a>Optimieren von Feedoptionen für Abfrageparameter 
Die Abfrageleistung kann über die Parameter [„Feedoptionen“](https://docs.microsoft.com/dotnet/api/microsoft.azure.documents.client.feedoptions?view=azure-dotnet) der Anforderung optimiert werden. Versuchen Sie, die folgenden Optionen einzustellen:

  * Setzen Sie zuerst `MaxDegreeOfParallelism` auf -1 und vergleichen Sie dann die Leistung über verschiedene Werte hinweg. 
  * Setzen Sie zuerst `MaxBufferedItemCount` auf -1 und vergleichen Sie dann die Leistung über verschiedene Werte hinweg. 
  * Setzen Sie `MaxItemCount` auf -1.

Wenn Sie die Leistung verschiedener Werte vergleichen, verwenden Sie dafür Werte wie 2, 4, 8, 16 und andere.
 
## <a name="read-all-results-from-continuations"></a>Alle Ergebnisse aus Fortsetzungen lesen
Wenn Sie denken, dass Sie nicht alle Ergebnisse erhalten, stellen Sie sicher, dass die Fortsetzung vollständig ausgeglichen wird. Sprich, Sie sollten die Ergebnisse weiterhin lesen, solange das Fortsetzungs-Token noch über weitere Dokumente verfügt.

Der vollständige Ausgleich wird durch eines der folgenden Muster erreicht:

  * Setzen Sie die Verarbeitung der Ergebnisse fort, solange die Fortsetzung nicht ausgeglichen ist.
  * Setzen Sie die Verarbeitung fort, wenn die Abfrage noch mehr Ergebnisse liefert. 

    ```csharp
    // using AsDocumentQuery you get access to whether or not the query HasMoreResults
    // If it does, just call ExecuteNextAsync until there are no more results
    // No need to supply a continuation token here as the server keeps track of progress
    var query = client.CreateDocumentQuery<Family>(collectionLink, options).AsDocumentQuery();
    while (query.HasMoreResults)
    {
        foreach (Family family in await query.ExecuteNextAsync())
        {
            families.Add(family);
        }
    }
    ```

## <a name="choose-system-functions-that-utilize-index"></a>Auswahl von Systemfunktionen, die den Index verwenden
Der Ausdruck kann nur den Index verwenden, wenn er in einen Bereich von Zeichenfolgenwerte übersetzt werden kann. 

Hier ist die Liste der Zeichenfolgenfunktionen, die den Index verwenden können: 
    
  * STARTSWITH(str_expr, str_expr) 
  * LEFT(str_expr, num_expr) = str_expr 
  * SUBSTRING(str_expr, num_expr, num_expr) = str_expr, aber nur, wenn der erste num_expr 0 ist 
    
    Hier sind einige Abfragebeispiele: 
    
    ```sql

    -- If there is a range index on r.name, STARTSWITH will utilize the index while ENDSWITH won't 
    SELECT * 
    FROM c 
    WHERE STARTSWITH(c.name, 'J') AND ENDSWITH(c.name, 'n')

    ```
    
    ```sql

    -- LEFT will utilize the index while RIGHT won't 
    SELECT * 
    FROM c 
    WHERE LEFT(c.name, 2) = 'Jo' AND RIGHT(c.name, 2) = 'hn'

    ```

  * Vermeiden Sie Systemfunktionen im Filter (oder die WHERE-Klausel), die nicht vom Index erfüllt werden. Beispiele dafür sind Systemfunktionen wie „Contains“, „Upper“, „Lower“.
  * Schreiben Sie nach Möglichkeit Abfragen, um für einen Partitionsschlüssel einen Filter zu verwenden.
  * Um leistungsfähigere Abfragen zu erreichen, vermeiden Sie den Aufruf von UPPER/LOWER im Filter. Normalisieren Sie stattdessen die Schreibweise der Werte beim Einfügen. Für jeden der Werte geben Sie den Wert in der gewünschten Schreibweise ein. Alternativ können Sie sowohl den Originalwert als auch den Wert mit der gewünschten Schreibweise eingeben. 

    Beispiel:
    
    ```sql

    SELECT * FROM c WHERE UPPER(c.name) = "JOE"

    ```
    
    In diesem Fall können Sie „JOE“ in Großbuchstaben speichern, oder sowohl „Joe“, als ursprünglichen Wert und auch als „JOE“ speichern. 
    
    Wenn die Schreibweise der JSON-Daten normalisiert ist, wird die Abfrage zu Folgendem:
    
    ```sql

    SELECT * FROM c WHERE c.name = "JOE"

    ```

    Die zweite Abfrage ist leistungsfähiger, da sie keine Durchführung von Transformationen für jeden der Werte erfordert, um die Werte mit „JOE“ zu vergleichen.

Weitere Informationen zu Systemfunktionen finden Sie im Artikel [„Systemfunktionen“](sql-query-system-functions.md).

## <a name="check-indexing-policy"></a>Überprüfen der Indizierungsrichtlinie
So überprüfen Sie, ob die aktuelle [Indizierungsrichtlinie](index-policy.md) optimal ist:

  * Stellen Sie sicher, dass alle in den Abfragen verwendeten JSON-Pfade in der Indizierungsrichtlinie enthalten sind, um schnellere Lesevorgänge zu erzielen.
  * Schließen Sie Pfade aus, die nicht in Abfragen verwendet werden, um die Leistung für die Schreibvorgänge zu erhöhen.

Weitere Informationen finden Sie im Artikel [„Verwalten von Indizierungsrichtlinien“](how-to-manage-indexing-policy.md).

## <a name="spatial-data-check-ordering-of-points"></a>Räumliche Daten: Überprüfen Sie die Reihenfolge der Punkte
Punkte innerhalb eines Polygons müssen gegen den Uhrzeigersinn nacheinander angegeben werden. Ein Polygon, das im Uhrzeigersinn angegeben wird, stellt die Umkehrung der darin enthaltenen Region dar.

## <a name="optimize-join-expressions"></a>Optimieren von JOIN-Ausdrücken
`JOIN`-Ausdrücke können in große, übergreifende Produkte erweitert werden. Wenn möglich, führen Sie die Abfrage in einem kleineren Suchbereich mit einem engeren Filter aus.

Mehrwertige Unterabfragen können `JOIN`-Ausdrücke optimieren, indem Prädikate nach jedem „Select-Many“-Ausdruck statt nach allen Cross Joins in der `WHERE`-Klausel gepusht werden. Ein detailliertes Beispiel finden Sie im Artikel [„Optimieren von JOIN-Ausdrücken“](https://docs.microsoft.com/azure/cosmos-db/sql-query-subquery#optimize-join-expressions).

## <a name="optimize-order-by-expressions"></a>Optimieren von „ORDER BY“-Ausdrücken 
Die Abfrageleistung von `ORDER BY` kann beeinträchtigt werden, wenn die Felder eine geringe Dichte aufweisen oder nicht in der Indexrichtlinie enthalten sind.

  * Schränken Sie den Suchbereich für Felder mit geringer Datendichte, wie „Zeit“, so weit wie möglich mit Filtern ein. 
  * Wenn Sie eine einzelne `ORDER BY`-Eigenschaft haben, fügen Sie diese der Indexrichtlinie hinzu. 
  * Definieren Sie für mehrere Ausdrücke der Eigenschaft `ORDER BY` einen [zusammengesetzten Index](https://docs.microsoft.com/azure/cosmos-db/index-policy#composite-indexes) für die Felder, die sortiert werden.  

## <a name="many-large-documents-being-loaded-and-processed"></a>Viele große Dokumente werden geladen und verarbeitet.
Die Zeit und die RUs, die für eine Abfrage benötigt werden, hängen nicht nur von der Größe der Antwort ab, sondern auch von der Arbeit, die von der Abfrageverarbeitungspipeline geleistet wird. Zeit und RUs steigen proportional mit dem Arbeitsaufwand der gesamten Abfrageverarbeitungspipeline an. Bei großen Dokumente wird mehr Arbeit geleistet, so dass mehr Zeit und RUs für das Laden und Bearbeiten großer Dokumente benötigt werden.

## <a name="low-provisioned-throughput"></a>Wenig bereitgestellter Durchsatz
Stellen Sie sicher, dass der bereitgestellte Durchsatz die Arbeitsauslastung bewältigen kann. Steigern Sie das RU-Budget für betroffene Sammlungen.

## <a name="try-upgrading-to-the-latest-sdk-version"></a>Probieren Sie, ob ein Upgrade auf die neuste SDK-Version hilft.
Im Artikel [“SDK-Download und Anmerkungen zur Version“](sql-api-sdk-dotnet.md) erfahren Sie, wie Sie die neuste SKD-Version finden.

## <a name="next-steps"></a>Nächste Schritte
Die unten angegebenen Dokumente enthalten Informationen dazu, wie Sie RUs pro Abfrage messen, Ausführungsstatistiken zum Optimieren Ihrer Abfragen erhalten und mehr:

* [Abrufen von SQL-Abfrageausführungsmetriken mit dem .NET SDK](profile-sql-api-query.md)
* [Optimieren der Abfrageleistung mit Azure Cosmos DB](sql-api-sql-query-metrics.md)
* [Tipps zur Leistungssteigerung für das .NET SDK](performance-tips.md)