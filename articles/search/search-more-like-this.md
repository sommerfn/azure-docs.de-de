---
title: moreLikeThis-Abfragefunktion (Vorschauversion)
titleSuffix: Azure Cognitive Search
description: In diesem Artikel wird die moreLikeThis-Funktion (Vorschauversion) beschrieben, die in Vorschauversionen der REST-API der kognitiven Azure-Suche verfügbar ist.
manager: nitinme
author: brjohnstmsft
ms.author: brjohnst
ms.devlang: rest-api
ms.service: cognitive-search
ms.topic: conceptual
ms.date: 11/04/2019
ms.openlocfilehash: fdde89f9ff88b15c464af805b81708b268e5ddf5
ms.sourcegitcommit: bc7725874a1502aa4c069fc1804f1f249f4fa5f7
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 11/07/2019
ms.locfileid: "73721731"
---
# <a name="morelikethis-preview-in-azure-cognitive-search"></a>moreLikeThis (Vorschauversion) in der kognitiven Azure-Suche

> [!IMPORTANT] 
> Dieses Feature ist zurzeit als öffentliche Preview verfügbar. Die Vorschaufunktion wird ohne Vereinbarung zum Servicelevel bereitgestellt und ist nicht für Produktionsworkloads vorgesehen. Weitere Informationen finden Sie unter [Zusätzliche Nutzungsbestimmungen für Microsoft Azure-Vorschauen](https://azure.microsoft.com/support/legal/preview-supplemental-terms/). Dieses Feature wird durch die [REST-API-Version 2019-05-06-Preview](search-api-preview.md) bereitgestellt. Derzeit werden weder das Portal noch das .NET SDK unterstützt.

`moreLikeThis=[key]` ist ein Abfrageparameter in der [API zum Durchsuchen von Dokumenten](https://docs.microsoft.com/rest/api/searchservice/search-documents), über den Dokumente gesucht werden, die dem mit dem Dokumentschlüssel angegebenen Dokument ähneln. Beim Ausführen einer Suchanforderung mit `moreLikeThis` wird eine Abfrage mit Suchbegriffen generiert, die aus dem angegebenen Dokument extrahiert werden und das Dokument am besten beschreiben. Anhand der generierten Abfrage wird dann die Suchanforderung erstellt. Standardmäßig werden die Inhalte aller durchsuchbaren Felder berücksichtigt, mit Ausnahme aller eingeschränkten Felder, die Sie mit dem Parameter `searchFields` angegeben haben. Der Parameter `moreLikeThis` kann nicht mit dem Suchparameter `search=[string]` verwendet werden.

Standardmäßig werden die Inhalte aller durchsuchbaren Felder auf oberster Ebene berücksichtigt. Wenn Sie stattdessen bestimmte Felder angeben möchten, können Sie den Parameter `searchFields` verwenden. 

Sie können moreLikeThis für durchsuchbare untergeordnete Felder eines [komplexen Typs](search-howto-complex-data-types.md) verwenden.

## <a name="examples"></a>Beispiele 

Unten ist ein Beispiel für eine moreLikeThis-Abfrage angegeben. Mit der Abfrage werden Dokumente gesucht, deren Beschreibungsfelder dem Feld des Quelldokuments entsprechend der Angabe durch den Parameter `moreLikeThis` am ähnlichsten sind.

```
Get /indexes/hotels/docs?moreLikeThis=1002&searchFields=description&api-version=2019-05-06-Preview
```

```
POST /indexes/hotels/docs/search?api-version=2019-05-06-Preview
    {
      "moreLikeThis": "1002",
      "searchFields": "description"
    }
```


## <a name="next-steps"></a>Nächste Schritte

Sie können diese Funktion mit einem beliebigen Webtesttool ausprobieren.  Es empfiehlt sich die Verwendung von Postman.

> [!div class="nextstepaction"]
> [Erkunden von REST-APIs der kognitiven Azure-Suche mit Postman](search-get-started-postman.md)