---
title: 'Schnellstart: Projekt: URL-Vorschau, Node.js'
titlesuffix: Azure Cognitive Services
description: Hier erhalten Sie Informationen zu den ersten Schritten mit URL Preview in Microsoft Cognitive Services in Azure.
services: cognitive-services
author: mikedodaro
manager: nitinme
ms.service: cognitive-services
ms.subservice: url-preview
ms.topic: quickstart
ms.date: 03/16/2018
ms.author: rosh
ms.openlocfilehash: 92124a53c871be7a83d7b276e31f000d7c674358
ms.sourcegitcommit: 800f961318021ce920ecd423ff427e69cbe43a54
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 07/31/2019
ms.locfileid: "68698011"
---
# <a name="quickstart-url-preview-with-nodejs"></a>Schnellstart: URL-Vorschau mit Node.js 

Im folgenden Node-Beispiel wird eine URL-Vorschau für die SwiftKey-Website erstellt: https://swiftkey.com/en.

## <a name="prerequisites"></a>Voraussetzungen

Rufen Sie einen Zugriffsschlüssel für die kostenlose Testversion von [Cognitive Services Labs](https://labs.cognitive.microsoft.com/en-us/project-answer-search) ab.

## <a name="code-scenario"></a>Codeszenario 

Der folgende Code ruft URL Preview-Daten ab.
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
let subscriptionKey = 'YOUR-ACCESS-KEY'; 
let host = 'api.labs.cognitive.microsoft.com';
let path = '/urlpreview/v7.0/search';

let mkt = 'en-US';
let q = 'https://swiftkey.com/';

let params = '?q=' + encodeURI(q);

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
- [C#-Beispielcode](csharp.md)
- [Java-Schnellstart](java-quickstart.md)
- [JavaScript-Schnellstart](javascript.md)
- [Python-Schnellstart](python-quickstart.md)
