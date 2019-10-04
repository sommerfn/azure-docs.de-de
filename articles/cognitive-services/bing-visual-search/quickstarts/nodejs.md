---
title: 'Schnellstart: Gewinnen von Erkenntnissen zu Bildern mit der REST-API für die visuelle Bing-Suche und Node.js'
titleSuffix: Azure Cognitive Services
description: Es wird beschrieben, wie Sie ein Bild in die API für die visuelle Bing-Suche hochladen und Erkenntnisse dazu gewinnen.
services: cognitive-services
author: swhite-msft
manager: nitinme
ms.service: cognitive-services
ms.subservice: bing-visual-search
ms.topic: quickstart
ms.date: 4/02/2019
ms.author: scottwhi
ms.openlocfilehash: 9414bac220d928618b403aa2f7df7748772e0e9a
ms.sourcegitcommit: c174d408a5522b58160e17a87d2b6ef4482a6694
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 04/18/2019
ms.locfileid: "59047567"
---
# <a name="quickstart-get-image-insights-using-the-bing-visual-search-rest-api-and-nodejs"></a>Schnellstart: Gewinnen von Erkenntnissen zu Bildern mit der REST-API für die visuelle Bing-Suche und Node.js

Verwenden Sie diese Schnellstartanleitung, um die API für die visuelle Bing-Suche zum ersten Mal aufzurufen und die Suchergebnisse anzuzeigen. Mit dieser einfachen JavaScript-Anwendung wird ein Bild in die API hochgeladen, und es werden die zurückgegebenen Informationen angezeigt. Diese Anwendung ist zwar in JavaScript geschrieben, aber die API ist ein RESTful-Webdienst, der mit den meisten Programmiersprachen kompatibel ist.

Wenn Sie ein lokales Bild hochladen, müssen die Formulardaten den Header `Content-Disposition` enthalten. Der Parameter `name` muss auf „image“ festgelegt werden. Den Parameter `filename` können Sie auf eine beliebige Zeichenfolge festlegen. Der Inhalt des Formulars umfasst die Binärdaten des Bilds. Sie können eine maximale Bildgröße von 1 MB hochladen.

```
--boundary_1234-abcd
Content-Disposition: form-data; name="image"; filename="myimagefile.jpg"

ÿØÿà JFIF ÖÆ68g-¤CWŸþ29ÌÄøÖ‘º«™æ±èuZiÀ)"óÓß°Î= ØJ9á+*G¦...

--boundary_1234-abcd--
```

## <a name="prerequisites"></a>Voraussetzungen

* [Node.js](https://nodejs.org/en/download/)
* Das Anforderungsmodul für JavaScript. Dieses Modul kann mithilfe des Befehls `npm install request` installiert werden.
* Das Formulardatenmodul. Dieses Modul kann mithilfe des Befehls `npm install form-data` installiert werden. 

[!INCLUDE [cognitive-services-bing-visual-search-signup-requirements](../../../../includes/cognitive-services-bing-image-search-signup-requirements.md)]

## <a name="initialize-the-application"></a>Initialisieren der Anwendung

1. Erstellen Sie eine JavaScript-Datei in Ihrer bevorzugten IDE oder Ihrem bevorzugten Editor, und legen Sie die folgenden Anforderungen fest:

    ```javascript
    var request = require('request');
    var FormData = require('form-data');
    var fs = require('fs');
    ```

2. Erstellen Sie Variablen für Ihren API-Endpunkt, den Abonnementschlüssel und den Pfad zu Ihrem Bild:

    ```javascript
    var baseUri = 'https://api.cognitive.microsoft.com/bing/v7.0/images/visualsearch';
    var subscriptionKey = 'your-api-key';
    var imagePath = "path-to-your-image";
    ```

3. Erstellen Sie eine Funktion mit dem Namen `requestCallback()`, um die Antwort der API auszugeben:

    ```javascript
    function requestCallback(err, res, body) {
        console.log(JSON.stringify(JSON.parse(body), null, '  '))
    }
    ```

## <a name="construct-and-send-the-search-request"></a>Erstellen und Senden der Suchanforderung

1. Erstellen Sie mit `FormData()` ein neues Objekt vom Typ **FormData**, und fügen Sie mit `fs.createReadStream()` Ihren Bildpfad an:
    
    ```javascript
    var form = new FormData();
    form.append("image", fs.createReadStream(imagePath));
    ```

2. Verwenden Sie die Anforderungsbibliothek zum Hochladen des Bilds, und rufen Sie `requestCallback()` auf, um die Antwort auszugeben. Achten Sie darauf, dass Sie dem Anforderungsheader Ihren Abonnementschlüssel hinzufügen:

    ```javascript
    form.getLength(function(err, length){
      if (err) {
        return requestCallback(err);
      }
      var r = request.post(baseUri, requestCallback);
      r._form = form; 
      r.setHeader('Ocp-Apim-Subscription-Key', subscriptionKey);
    });
    ```

## <a name="next-steps"></a>Nächste Schritte

> [!div class="nextstepaction"]
> [Erstellen einer Single-Page-Web-App für die visuelle Suche](../tutorial-bing-visual-search-single-page-app.md)
