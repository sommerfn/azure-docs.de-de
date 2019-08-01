---
title: 'Schnellstart: Projekt: Suche nach Antworten, Python'
titlesuffix: Azure Cognitive Services
description: 'Python-Beispiel für die ersten Schritte mit dem „Projekt: Suche nach Antworten“.'
services: cognitive-services
author: mikedodaro
manager: nitinme
ms.service: cognitive-services
ms.subservice: answer-search
ms.topic: quickstart
ms.date: 04/13/2018
ms.author: rosh
ms.openlocfilehash: c0da596e84ac827b55affd5545c516e7623980f5
ms.sourcegitcommit: 800f961318021ce920ecd423ff427e69cbe43a54
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 07/31/2019
ms.locfileid: "68698064"
---
# <a name="quickstart-project-answer-search-with-python"></a>Schnellstart: „Projekt: Suche nach Antworten“ mit Python

Das folgende Python-Beispiel veranschaulicht das Erstellen und Senden einer Anforderung für Informationen zu „Felsen von Gibraltar“.

## <a name="prerequisites"></a>Voraussetzungen

Rufen Sie einen Zugriffsschlüssel für die kostenlose Testversion von [Cognitive Services Labs](https://labs.cognitive.microsoft.com/en-us/project-answer-search) ab.

In diesem Beispiel wird Python 3.6.4 verwendet.

## <a name="code-scenario"></a>Codeszenario 

Der folgende Code erstellt eine URL-Vorschau.
Er wird in den folgenden Schritten implementiert:
1. Deklarieren Sie Variablen zum Angeben des Endpunkts nach Host und Pfad.
2. Geben Sie die Abfrage-URL zum Anzeigen einer Vorschau an, und fügen Sie den Abfrageparameter hinzu.  
3. Legen Sie den Abfrageparameter fest.
4. Definieren Sie die Suchfunktion, mit der die Anforderung erstellt und der Header *Ocp-Apim-Subscription-Key* hinzugefügt wird.
5. Legen Sie den Header *Ocp-Apim-Subscription-Key* fest. 
6. Stellen Sie die Verbindung her, und senden Sie die Anforderung.
7. Drucken Sie die JSON-Ergebnisse.

Der vollständige Code für diese Demo sieht wie folgt aus:

```
import http.client, urllib.parse
import json

# Replace the subscriptionKey string value with your valid subscription key.
subscriptionKey = 'YOUR-SUBSCRIPTION-KEY'

host = 'https://api.labs.cognitive.microsoft.com'
path = '/answerSearch/v7.0/search '

query = 'Rock of Gibraltar'

params = '?q=' + urllib.parse.quote (query) + '&mkt=en-us'

def get_local():
    headers = {'Ocp-Apim-Subscription-Key': subscriptionKey}
    conn = http.client.HTTPSConnection (host)
    conn.request ("GET", path + params, None, headers)
    response = conn.getresponse ()
    return response.read ()

result = get_local()
print (json.dumps(json.loads(result), indent=4))

```
## <a name="next-steps"></a>Nächste Schritte
- [C#-Schnellstart](c-sharp-quickstart.md)
- [Java-Schnellstart](java-quickstart.md)
- [Node-Schnellstart](node-quickstart.md)
