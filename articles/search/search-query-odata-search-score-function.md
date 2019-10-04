---
title: Referenz zur search.score-Funktion von OData – Azure Search
description: search.score-Funktion in Azure Search-Abfragen.
ms.date: 06/13/2019
services: search
ms.service: search
ms.topic: conceptual
author: brjohnstmsft
ms.author: brjohnst
manager: nitinme
translation.priority.mt:
- de-de
- es-es
- fr-fr
- it-it
- ja-jp
- ko-kr
- pt-br
- ru-ru
- zh-cn
- zh-tw
ms.openlocfilehash: b6bf56a61ca685b306a15e474623336216ba531b
ms.sourcegitcommit: bb8e9f22db4b6f848c7db0ebdfc10e547779cccc
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 08/20/2019
ms.locfileid: "69647512"
---
# <a name="odata-searchscore-function-in-azure-search"></a>OData-Funktion `search.score` in Azure Search

Wenn Sie eine Abfrage an Azure Search ohne den [Parameter **$orderby**](search-query-odata-orderby.md) senden, werden die zurückgegebenen Ergebnisse in absteigender Reihenfolge nach Relevanz sortiert. Selbst wenn Sie **$orderby** verwenden, wird die Relevanzbewertung standardmäßig verwendet, um Verbindungen aufzuheben. Manchmal ist es jedoch sinnvoll, die Relevanzbewertung als erstes Sortierkriterium und einige andere Kriterien als Entscheidungshilfe zu verwenden. Die `search.score`-Funktion ermöglicht dies.

## <a name="syntax"></a>Syntax

Die Syntax für `search.score` in **$orderby** ist `search.score()`. Die Funktion `search.score` akzeptiert keine Parameter. Sie kann mit dem Spezifizierer `asc` oder `desc` für die Sortierreihenfolge verwendet werden, genau wie jede andere Klausel im Parameter **$orderby**. Sie kann an einer beliebigen Stelle in der Liste der Sortierkriterien platziert werden.

## <a name="example"></a>Beispiel

Sortieren von Hotels in absteigender Reihenfolge nach `search.score` und `rating` und anschließend in aufsteigender Reihenfolge nach der Entfernung von den angegebenen Koordinaten, sodass von zwei Hotels mit identischen Bewertungen das nächstgelegene zuerst aufgeführt wird:

    search.score() desc,rating desc,geo.distance(location, geography'POINT(-122.131577 47.678581)') asc

## <a name="next-steps"></a>Nächste Schritte  

- [Übersicht über die OData-Ausdruckssprache für Azure Search](query-odata-filter-orderby-syntax.md)
- [Referenz zur OData-Ausdruckssyntax für Azure Search](search-query-odata-syntax-reference.md)
- [Search Documents &#40;Azure Search Service REST API&#41;](https://docs.microsoft.com/rest/api/searchservice/Search-Documents) (Suchen nach Dokumenten: REST-API für den Azure Search-Dienst)
