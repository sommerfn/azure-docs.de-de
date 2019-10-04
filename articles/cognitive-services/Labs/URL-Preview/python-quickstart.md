---
title: 'Schnellstart: Projekt: URL-Vorschau, Python'
titlesuffix: Azure Cognitive Services
description: 'Skriptbeispiel für den schnellen Einstieg in die Verwendung des „Projekts: URL-Vorschau“ mit Python.'
services: cognitive-services
author: mikedodaro
manager: nitinme
ms.service: cognitive-services
ms.subservice: url-preview
ms.topic: quickstart
ms.date: 03/29/2018
ms.author: rosh
ROBOTS: NOINDEX
ms.openlocfilehash: e9b145b0786b2ab59552e5dfda6f0db63677443e
ms.sourcegitcommit: ad9120a73d5072aac478f33b4dad47bf63aa1aaa
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 08/01/2019
ms.locfileid: "68706881"
---
# <a name="quickstart-url-preview-with-python"></a>Schnellstart: URL-Vorschau mit Python

Im folgenden Python-Beispiel wird eine URL-Vorschau für die SwiftKey-Website erstellt: https://swiftkey.com/en.

## <a name="prerequisites"></a>Voraussetzungen

Rufen Sie einen Zugriffsschlüssel für die kostenlose Testversion von [Cognitive Services Labs](https://labs.cognitive.microsoft.com/en-us/project-answer-search) ab.

In diesem Beispiel wird Python 3.6 verwendet.

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
subscriptionKey = 'your-subscription-key'

host = 'api.labs.cognitive.microsoft.com'
path = '/urlpreview/v7.0/search'

query = 'https://SwiftKey.com'

params = '?q=' + urllib.parse.quote (query)

def get_preview ():
    headers = {'Ocp-Apim-Subscription-Key': subscriptionKey}
    conn = http.client.HTTPSConnection (host)
    conn.request ("GET", path + params, None, headers)
    response = conn.getresponse ()
    return response.read ()

result = get_preview ()
print (json.dumps(json.loads(result), indent=4))
```
## <a name="next-steps"></a>Nächste Schritte
- [C#-Schnellstart](csharp.md)
- [Java-Schnellstart](java-quickstart.md)
- [JavaScript-Schnellstart](javascript.md)
- [Node-URL-Schnellstart](node-quickstart.md)
