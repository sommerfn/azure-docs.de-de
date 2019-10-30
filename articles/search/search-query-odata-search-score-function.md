---
title: Referenz für die OData-Funktion „search.score“
titleSuffix: Azure Cognitive Search
description: OData-Funktion „search.score“ in Abfragen der kognitiven Azure-Suche.
manager: nitinme
author: brjohnstmsft
ms.author: brjohnst
ms.service: cognitive-search
ms.topic: conceptual
ms.date: 11/04/2019
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
ms.openlocfilehash: 500ac4f3a44d54e367ddc4ee5efc9514d603cab6
ms.sourcegitcommit: b050c7e5133badd131e46cab144dd5860ae8a98e
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 10/23/2019
ms.locfileid: "72793273"
---
# <a name="odata-searchscore-function-in-azure-cognitive-search"></a>OData-Funktion `search.score` in der kognitiven Azure-Suche

Wenn Sie eine Abfrage ohne den [Parameter **$orderby**](search-query-odata-orderby.md) an die kognitive Azure-Suche senden, werden die zurückgegebenen Ergebnisse in absteigender Reihenfolge nach Relevanz sortiert. Selbst wenn Sie **$orderby** verwenden, wird die Relevanzbewertung standardmäßig verwendet, um Verbindungen aufzuheben. Manchmal ist es jedoch sinnvoll, die Relevanzbewertung als erstes Sortierkriterium und einige andere Kriterien als Entscheidungshilfe zu verwenden. Die `search.score`-Funktion ermöglicht dies.

## <a name="syntax"></a>Syntax

Die Syntax für `search.score` in **$orderby** ist `search.score()`. Die Funktion `search.score` akzeptiert keine Parameter. Sie kann mit dem Spezifizierer `asc` oder `desc` für die Sortierreihenfolge verwendet werden, genau wie jede andere Klausel im Parameter **$orderby**. Sie kann an einer beliebigen Stelle in der Liste der Sortierkriterien platziert werden.

## <a name="example"></a>Beispiel

Sortieren von Hotels in absteigender Reihenfolge nach `search.score` und `rating` und anschließend in aufsteigender Reihenfolge nach der Entfernung von den angegebenen Koordinaten, sodass von zwei Hotels mit identischen Bewertungen das nächstgelegene zuerst aufgeführt wird:

    search.score() desc,rating desc,geo.distance(location, geography'POINT(-122.131577 47.678581)') asc

## <a name="next-steps"></a>Nächste Schritte  

- [Übersicht über die OData-Ausdruckssprache für die kognitive Azure-Suche](query-odata-filter-orderby-syntax.md)
- [Referenz zur OData-Ausdruckssyntax für die kognitive Azure-Suche](search-query-odata-syntax-reference.md)
- [Suchen von Dokumenten &#40;REST-API für die kognitive Azure-Suche&#41;](https://docs.microsoft.com/rest/api/searchservice/Search-Documents)
