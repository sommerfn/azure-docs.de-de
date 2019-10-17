---
title: Kompatibilität von Azure Cosmos DB Gremlin mit TinkerPop-Features
description: Referenzdokumentation zu Kompatibilitätsproblemen der Graph-Engine
author: SnehaGunda
ms.service: cosmos-db
ms.subservice: cosmosdb-graph
ms.topic: reference
ms.date: 09/10/2019
ms.author: sngun
ms.openlocfilehash: 0ed5824859b8463919a809861993f9f98a4f9251
ms.sourcegitcommit: 1d0b37e2e32aad35cc012ba36200389e65b75c21
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 10/15/2019
ms.locfileid: "72327030"
---
# <a name="azure-cosmos-db-gremlin-compatibility"></a>Kompatibilität von Azure Cosmos DB Gremlin
Die Azure Cosmos DB Graph-Engine folgt genau den Durchlaufschritten der Spezifikation von [Apache TinkerPop](https://tinkerpop.apache.org/docs/current/reference/#graph-traversal-steps), aber es gibt Unterschiede.

## <a name="behavior-differences"></a>Unterschiede im Verhalten

* Die Azure Cosmos DB Graph-Engine führt einen Durchlauf mit ***breitem*** Ansatz aus, während TinkerPop Gremlin einen „tiefen“ Ansatz verfolgt. Dieses Verhalten erzielt eine bessere Leistung in einem horizontal skalierbaren System wie Cosmos DB. 

## <a name="unsupported-features"></a>Nicht unterstützte Funktionen

* ***[Gremlin Bytecode](http://tinkerpop.apache.org/docs/current/tutorials/gremlin-language-variants/)*** ist eine von der Programmiersprache unabhängige Spezifikation für Diagrammdurchläufe. Cosmos DB Graph unterstützt dies noch nicht. Verwenden Sie ```GremlinClient.SubmitAsync()```, und übergeben Sie den Durchlauf als eine Textzeichenfolge.

* ***```property(set, 'xyz', 1)```***: Das Festlegen der Kardinalität wird noch nicht unterstützt. Verwenden Sie stattdessen ```property(list, 'xyz', 1)```.

* ***```match()```*** ermöglicht das Abfragen von Graphen mithilfe von deklarativem Musterabgleich. Diese Funktion ist nicht verfügbar.

* ***Objekte als Eigenschaften*** werden für Vertices und Edges nicht unterstützt. Eigenschaften können nur primitive Typen oder Arrays sein.

* ***Sortieren nach Arrayeigenschaften***: ```.order().by(<array property>)``` wird nicht unterstützt. Die Sortierung wird nur von primitiven Typen unterstützt.

* ***Nicht primitive JSON-Typen*** werden nicht unterstützt. Verwenden Sie die Typen ```string```, ```number``` oder ```true```/```false```. ```null```-Werte werden nicht unterstützt. 

* Das ***GraphSONv3***-Serialisierungsmodul ist noch nicht verfügbar.

* ***Transaktionen*** werden aufgrund der verteilten Natur des Systems nicht unterstützt.  Konfigurieren Sie ein geeignetes Konsistenzmodell für das Gremlin-Konto, um „Ihre eigenen Schreibvorgänge zu lesen“, und verwenden Sie optimistische Nebenläufigkeit, um Schreibvorgänge aufzulösen, zu einen Konflikt verursachen.

## <a name="next-steps"></a>Nächste Schritte
* Besuchen Sie die Seite [Cosmos DB User Voice](https://feedback.azure.com/forums/263030-azure-cosmos-db), um Feedback zu übermitteln und dem Team dabei zu helfen, sich auf die für Sie wichtigen Features zu konzentrieren.
