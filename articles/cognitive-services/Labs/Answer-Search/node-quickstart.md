---
title: 'Schnellstart: Projekt: Suche nach Antworten, Node'
description: 'Erste Schritte mit dem „Projekt: Suche nach Antworten“ mit Node.'
services: cognitive-services
author: mikedodaro
manager: nitinme
ms.service: cognitive-services
ms.subservice: answer-search
ms.topic: quickstart
ms.date: 04/13/2018
ms.author: rosh
ms.openlocfilehash: 21dda323cf465f56fb3f43160dd04c27a81d5590
ms.sourcegitcommit: 800f961318021ce920ecd423ff427e69cbe43a54
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 07/31/2019
ms.locfileid: "68698075"
---
# <a name="quickstart-project-answer-search-with-node"></a>Schnellstart: „Projekt: Suche nach Antworten“ mit Node

Im folgenden Node-Beispiel wird eine Abfrage für Informationen zum Yosemite-Nationalpark erstellt.

## <a name="prerequisites"></a>Voraussetzungen

Rufen Sie einen Zugriffsschlüssel für die kostenlose Testversion von [Cognitive Services Labs](https://labs.cognitive.microsoft.com/en-us/project-answer-search) ab.

In diesem Beispiel wird Node v8.9.4 verwendet.

## <a name="code-scenario"></a>Codeszenario 

Mit dem folgenden Code werden Antworten abgerufen.
Er wird in den folgenden Schritten implementiert:
1. Deklarieren Sie Variablen zum Angeben des Endpunkts nach Host und Pfad.
2. Geben Sie die Abfrage-URL zum Anzeigen einer Vorschau an, und fügen Sie den Abfrageparameter hinzu.  
3. Erstellen Sie eine Handlerfunktion für die Antwort.
4. Definieren Sie die Suchfunktion, mit der die Anforderung erstellt und der Header *Ocp-Apim-Subscription-Key* hinzugefügt wird.
5. Führen Sie die Suchfunktion aus. 

Der vollständige Code für diese Demo sieht wie folgt aus:

```
'use strict';

let https = require('https');

// Replace the subscriptionKey string value with your valid subscription key.
let subscriptionKey = 'YOUR-SUBSCRIPTION-KEY'; 

let host = 'api.labs.cognitive.microsoft.com';
let path = '/answerSearch/v7.0/search';

let mkt = 'en-us';
let q = 'Yosemite National Park';

let params = '?q=' + encodeURI(q) + '&mkt= + mkt;

let response_handler = function (response) {
    let body = '';
    response.on('data', function (d) {
        body += d;
    });
    response.on('end', function () {
        let body_ = JSON.parse(body);
        let body__ = JSON.stringify(body_, null, '  ');
        console.log(body__);
    });
    response.on('error', function (e) {
        console.log('Error: ' + e.message);
    });
};

let Search = function () {
    let request_params = {
        method: 'GET',
        hostname: host,
        path: path + params,
        headers: {
            'Ocp-Apim-Subscription-Key': subscriptionKey,
        }
    };

    let req = https.request(request_params, response_handler);
    req.end();
}

Search();

```

## <a name="next-steps"></a>Nächste Schritte
- [C#-Beispielcode](c-sharp-quickstart.md)
- [Java-Schnellstart](java-quickstart.md)
- [Python-Schnellstart](python-quickstart.md)
