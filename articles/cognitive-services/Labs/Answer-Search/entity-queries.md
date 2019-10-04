---
title: 'Schnellstart: Entitätsabfrage für „Projekt: Suche nach Antworten“'
titlesuffix: Azure Cognitive Services
description: Abfragen für Entitäten mit Project Answer Search
services: cognitive-services
author: mikedodaro
manager: nitinme
ms.service: cognitive-services
ms.subservice: answer-search
ms.topic: quickstart
ms.date: 04/16/2018
ms.author: rosh
ROBOTS: NOINDEX
ms.openlocfilehash: b1dbfcfe85a7847b593553bd06cbc22cc62d2e2e
ms.sourcegitcommit: ad9120a73d5072aac478f33b4dad47bf63aa1aaa
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 08/01/2019
ms.locfileid: "68705467"
---
# <a name="quickstart-query-for-entities"></a>Schnellstart: Abfragen für Entitäten

Wenn die Abfrage Informationen zu einer Person, einem Ort oder einer Sache anfordert, kann die Antwort eine `entities`-Antwort enthalten.  Abfragen geben immer Webseiten zurück, und [Fakten](fact-queries.md) und/oder [Entitäten](entity-queries.md) hängen von der Abfrage ab.

Entitäten unterstützen drei Abfrageszenarien: 
-   DominantEntity: Es ist nur eine Entität vorhanden, die der Abfrage und der Absicht des Benutzers entspricht. Die Abfrage „Space Needle“ ist ein Beispiel für ein DominantEntity-Szenario. 
-   Mehrdeutigkeit: Es sind mindestens zwei Entitäten vorhanden, die der Abfrage und der Absicht des Benutzers entsprechen, und es obliegt dem Benutzer, die richtige Entität auszuwählen. Die Abfrage „Game of Thrones“ ist ein Beispiel für ein Mehrdeutigkeitsszenario, bei der die Fernsehserie und die Buchreihe zurückgegeben werden. 
-   Liste: Es sind mehrere Entitäten vorhanden, die der Abfrage und der Absicht des Benutzers entsprechen. Die Abfrage „Liste der gefährdeten Arten“ ist ein Beispiel für ein Listenszenario, bei der tabellarische Werte zurückgegeben werden, die für die Anzeige in Zeilen und Zellen formatiert sind. 
 
Verwenden Sie zum Ermitteln des Abfrageszenarios das Feld `queryScenario` des Objekts `entities`. Die Daten, die die Entität enthält, hängen vom Typ der Entität ab. Obwohl Entitäten die gleichen grundlegenden Informationen enthalten, umfassen einige Entitäten wie z. B. Touristenattraktionen/Sehenswürdigkeiten oder Bücher zusätzliche Eigenschaften. Entitäten, die zusätzliche Eigenschaften umfassen, enthalten das Feld `_type`, das einen vom Serialisierungsmodul verwendeten Hinweis enthält. Die folgenden Entitäten enthalten zusätzliche Eigenschaften: 
-   Buch 
-   Musikaufzeichnung 
-   Person 
-   Sehenswürdigkeit 
 
Um den Entitätstyp zu bestimmen, den die Antwort enthält, verwenden Sie das Feld `entityTypeHints` wie in der Abfrage für „Bill Gates“ gezeigt.
```
        },
        "description": "Bill Gates is an American business man and philanthropist, co-founder of Microsoft",
        "entityPresentationInfo": {
          "entityScenario": "DominantEntity",
          "entityTypeHints": [
            "Person"
          ]
        },
        "bingId": "6d7d66a7-2cb8-0ae9-637c-f81fd749dc9a"
      }
```
Im Folgenden finden Sie eine Abfrage für „Space Needle“:
```
https://api.labs.cognitive.microsoft.com/answerSearch/v7.0/search?q=space+needle&mkt=en-us
```
Die Antwort enthält die `entities`-Antwort. Beachten Sie die Felder `entityScenario` und `entityTypeHints`. 
```
  "entities": {
    "value": [
      {
        "id": "https://www.bingapis.com/api/v7/#Entities.0",
        "contractualRules": [
          {
            "_type": "ContractualRules/LicenseAttribution",
            "targetPropertyName": "description",
            "mustBeCloseToContent": true,
            "license": {
              "name": "CC-BY-SA",
              "url": "https://creativecommons.org/licenses/by-sa/3.0/"
            },
            "licenseNotice": "Text under CC-BY-SA license"
          },
          {
            "_type": "ContractualRules/LinkAttribution",
            "targetPropertyName": "description",
            "mustBeCloseToContent": true,
            "text": "Wikipedia",
            "url": "https://en.wikipedia.org/wiki/Space_Needle"
          },
          {
            "_type": "ContractualRules/MediaAttribution",
            "targetPropertyName": "image",
            "mustBeCloseToContent": true,
            "url": "https://en.wikipedia.org/wiki/Space_Needle"
          }
        ],
        "webSearchUrl": "https://www.bing.com/entityexplore?q\u003dSpace+Needle\u0026filters\u003dsid:%22f8dd5b08-206d-2554-6e4a-893f51f4de7e%22\u0026elv\u003dAXXfrEiqqD9r3GuelwApulpmymQx!ODfuQu*veOQHkvP0!Zbvi5F5tVcMSDJvDEWiQWwrdueYTtIszgj03oFQHykYYLYgq3q5!Sf00QxXGIS",
        "name": "Space Needle",
        "image": {
          "name": "Space Needle",
          "thumbnailUrl": "https://www.bing.com/th?id\u003dA15d336cf119b9b5c7e0ab37e271421d3\u0026w\u003d110\u0026h\u003d110\u0026c\u003d7\u0026rs\u003d1\u0026qlt\u003d80\u0026cdv\u003d1\u0026pid\u003d16.1",
          "provider": [
            {
              "_type": "Organization",
              "url": "https://en.wikipedia.org/wiki/Space_Needle"
            }
          ],
          "hostPageUrl": "https://upload.wikimedia.org/wikipedia/commons/2/23/Space_Needle_2011-07-04.jpg",
          "width": 110,
          "height": 110,
          "sourceWidth": 152,
          "sourceHeight": 300
        },
        "description": "The Space Needle is an observation tower in Seattle, Washington, a landmark of the Pacific Northwest, and an icon of Seattle. It was built in the Seattle Center for the 1962 World\u0027s Fair, which drew over 2.3 million visitors, when nearly 20,000 people a day used its elevators.",
        "entityPresentationInfo": {
          "entityScenario": "DominantEntity",
          "entityTypeHints": [
            "Attraction"
          ]
        },
        "bingId": "f8dd5b08-206d-2554-6e4a-893f51f4de7e"
      }
    ]
  },
```

Eine Abfrage kann eine Liste zurückgeben, wenn sie relevant ist.

**Abfrage:** Die folgende Abfrage sucht nach einer Liste gefährdeter Arten:

```
https://api.labs.cognitive.microsoft.com/answerSearch/v7.0/search?q=list+of+endangered+species

```

**Antwort:** Die Antwort enthält eine Liste, die für die Anzeige von tabellarischen Werten formatiert ist:
```
  "facts": {
    "id": "https://www.bingapis.com/api/v7/#Facts",
    "contractualRules": [
      {
        "_type": "ContractualRules/LinkAttribution",
        "text": "www.worldwildlife.org/species/directory?direction=desc&sort=e…",
        "url": "https://www.worldwildlife.org/species/directory?direction=desc&sort=extinction_status"
      }
    ],
    "attributions": [
      {
        "providerDisplayName": "www.worldwildlife.org/species/directory?direction=desc&sort=e…",
        "seeMoreUrl": "https://www.worldwildlife.org/species/directory?direction=desc&sort=extinction_status"
      }
    ],
    "value": [
      {
        "subjectName": "Species Directory",
        "richCaption": {
          "_type": "StructuredValue/TabularData",
          "header": [
            "Common name",
            "Scientific name",
            "Conservation status ↓"
          ],
          "rows": [
            {
              "cells": [
                {
                  "_type": "Properties/Link",
                  "text": "Amur Leopard",
                  "url": "https://www.bing.com/https//www.worldwildlife.org/species/amur-leopard"
                },
                {
                  "text": "Panthera pardus orientalis"
                },
                {
                  "text": "Critically Endangered"
                }
              ]
            },
            {
              "cells": [
                {
                  "_type": "Properties/Link",
                  "text": "Black Rhino",
                  "url": "https://www.bing.com/https//www.worldwildlife.org/species/black-rhino"
                },
                {
                  "text": "Diceros bicornis"
                },
                {
                  "text": "Critically Endangered"
                }
              ]
            },
            {
              "cells": [
                {
                  "_type": "Properties/Link",
                  "text": "Bornean Orangutan",
                  "url": "https://www.bing.com/https//www.worldwildlife.org/species/bornean-orangutan"
                },
                {
                  "text": "Pongo pygmaeus"
                },
                {
                  "text": "Critically Endangered"
                }
              ]
            },
            {
              "cells": [
                {
                  "_type": "Properties/Link",
                  "text": "Cross River Gorilla",
                  "url": "https://www.bing.com/https//www.worldwildlife.org/species/cross-river-gorilla"
                },
                {
                  "text": "Gorilla gorilla diehli"
                },
                {
                  "text": "Critically Endangered"
                }
              ]
            }
          ],
          "seeMoreUrl": {
            "text": "46 more rows",
            "url": "https://www.worldwildlife.org/species/directory?direction=desc&sort=extinction_status"
          }
        }
      }
    ]
  },

```


## <a name="next-steps"></a>Nächste Schritte
- [C#-Schnellstart](c-sharp-quickstart.md)
- [Java-Schnellstart](java-quickstart.md)
- [Node-Schnellstart](node-quickstart.md)
- [Python-Schnellstart](python-quickstart.md)
