---
title: 'Tutorial: Starten des plastischen Readers mithilfe von Node.js'
titleSuffix: Azure Cognitive Services
description: In diesem Tutorial erstellen Sie eine Node.js-Anwendung, die Plastischer Reader startet.
services: cognitive-services
author: metanMSFT
manager: nitinme
ms.service: cognitive-services
ms.subservice: immersive-reader
ms.topic: tutorial
ms.date: 06/20/2019
ms.author: metan
ms.openlocfilehash: f8697042ed46e0ff333f736454346908d76cf039
ms.sourcegitcommit: dad277fbcfe0ed532b555298c9d6bc01fcaa94e2
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 07/10/2019
ms.locfileid: "67718375"
---
# <a name="tutorial-launch-the-immersive-reader-nodejs"></a>Tutorial: Starten des plastischen Readers (Node.js)

In der [Übersicht](./overview.md) haben Sie gelernt, was Plastischer Reader ist und wie das Tool bewährte Techniken implementiert, um das Leseverständnis von Sprachenlernenden, Leseanfängern und Schülern mit Lernunterschieden zu verbessern. In diesem Tutorial erfahren Sie, wie Sie eine Node.js-Webanwendung erstellen, die Plastischer Reader startet. In diesem Tutorial lernen Sie Folgendes:

> [!div class="checklist"]
> * Erstellen einer Node.js-Web-App mit Express
> * Abrufen eines Zugriffstokens
> * Starten von Plastischer Reader mit Beispielinhalt
> * Angeben der Sprache Ihres Inhalts
> * Angeben der Benutzeroberflächensprache von Plastischer Reader
> * Starten von Plastischer Reader mit mathematischem Inhalt

Wenn Sie kein Azure-Abonnement besitzen, können Sie ein [kostenloses Konto](https://azure.microsoft.com/free/?WT.mc_id=A261C142F) erstellen, bevor Sie beginnen.

## <a name="prerequisites"></a>Voraussetzungen

* Einen Abonnementschlüssel für Plastischer Reader. Wie Sie einen solchen Schlüssel erhalten, erfahren Sie [hier](https://docs.microsoft.com/azure/cognitive-services/cognitive-services-apis-create-account).
* [Node.js](https://nodejs.org/) und [YARN](https://yarnpkg.com)
* Eine IDE (beispielsweise [Visual Studio Code](https://code.visualstudio.com/))

## <a name="create-a-nodejs-web-app-with-express"></a>Erstellen einer Node.js-Web-App mit Express

Erstellen Sie eine Node.js-Web-App mithilfe des Tools `express-generator`.

```bash
npm install express-generator -g
express --view=pug myapp
cd myapp
```

Installieren Sie YARN-Abhängigkeiten, und fügen Sie die Abhängigkeiten `request` und `dotenv` hinzu. Sie werden später in diesem Tutorial benötigt.

```bash
yarn
yarn add request
yarn add dotenv
```

## <a name="acquire-an-access-token"></a>Abrufen eines Zugriffstokens

Schreiben Sie als Nächstes eine Back-End-API zum Abrufen eines Zugriffstokens unter Verwendung Ihres Abonnementschlüssels. Für diesen Schritt benötigen Sie Ihren Abonnementschlüssel und Ihren Endpunkt. Ihren Abonnementschlüssel finden Sie im Azure-Portal auf der Seite „Schlüssel“ der Ressource „Plastischer Reader“. Ihren Endpunkt finden Sie auf der Übersichtsseite.

Wenn Sie über Ihren Abonnementschlüssel und Ihren Endpunkt verfügen, erstellen Sie eine neue Datei mit der Erweiterung _.env_, und fügen Sie den folgenden Code ein. Ersetzen Sie dabei `{YOUR_SUBSCRIPTION_KEY}` und `{YOUR_ENDPOINT}` durch Ihren Abonnementschlüssel bzw. durch Ihren Endpunkt.

```text
SUBSCRIPTION_KEY={YOUR_SUBSCRIPTION_KEY}
ENDPOINT={YOUR_ENDPOINT}
```

Committen Sie diese Datei nicht in Ihre Quellcodeverwaltung, da sie Geheimnisse enthält, die nicht für die Öffentlichkeit bestimmt sind.

Öffnen Sie als Nächstes die Datei _app.js_, und fügen Sie am Anfang der Datei Folgendes hinzu. Dadurch werden der Abonnementschlüssel und der Endpunkt als Umgebungsvariablen in Node.js geladen.

```javascript
require('dotenv').config();
```

Öffnen Sie die Datei _routes\index.js_, und fügen Sie am Anfang der Datei Folgendes hinzu:

```javascript
var request = require('request');
```

Fügen Sie als Nächstes direkt nach dieser Zeile den folgenden Code hinzu. Dieser Code erstellt einen API-Endpunkt, der unter Verwendung Ihres Abonnementschlüssels ein Zugriffstoken bezieht und zurückgibt.

```javascript
router.get('/token', function(req, res, next) {
  request.post({
    headers: {
        'Ocp-Apim-Subscription-Key': process.env.SUBSCRIPTION_KEY,
        'content-type': 'application/x-www-form-urlencoded'
    },
    url: process.env.ENDPOINT
  },
  function(err, resp, token) {
    return res.send(token);
  });
});
```

Dieser API-Endpunkt muss durch eine Authentifizierung (beispielsweise [OAuth](https://oauth.net/2/)) geschützt werden. Dies wird jedoch in diesem Tutorial nicht behandelt.

## <a name="launch-the-immersive-reader-with-sample-content"></a>Starten von Plastischer Reader mit Beispielinhalt

1. Öffnen Sie _views\layout.pug_, und fügen Sie unter dem Tag `head` den folgenden Code vor dem Tag `body` hinzu. Diese Tags vom Typ `script` laden das [Immersive Reader SDK](https://github.com/Microsoft/immersive-reader-sdk) sowie JQuery.

    ```pug
    script(src='https://contentstorage.onenote.office.net/onenoteltir/immersivereadersdk/immersive-reader-sdk.0.0.1.js')
    script(src='https://code.jquery.com/jquery-3.3.1.min.js')
    ```

2. Öffnen Sie die Datei _views\index.pug_, und ersetzen Sie ihren Inhalt durch den folgenden Code. Dieser Code füllt die Seite mit Beispielinhalten und fügt eine Schaltfläche zum Starten von Plastischer Reader hinzu.

    ```pug
    extends layout

    block content
      h2(id='title') Geography
      p(id='content') The study of Earth's landforms is called physical geography. Landforms can be mountains and valleys. They can also be glaciers, lakes or rivers.
      div(class='immersive-reader-button' data-button-style='iconAndText' data-locale='en-US' onclick='launchImmersiveReader()')
      script.
        function launchImmersiveReader() {
          // First, get a token using our /token endpoint
          $.ajax('/token', { success: token => {
            // Second, grab the content from the page
            const content = {
              title: document.getElementById('title').innerText,
              chunks: [ {
                content: document.getElementById('content').innerText + '\n\n',
                lang: 'en'
              } ]
            };

            // Third, launch the Immersive Reader
            ImmersiveReader.launchAsync(token, content);
          }});
        }
    ```

3. Die Web-App ist nun bereit. Führen Sie den folgenden Befehl aus, um die App zu starten:

    ```bash
    npm start
    ```

4. Navigieren Sie in Ihrem Browser zu _http://localhost:3000_ . Auf der Seite sollte der oben angegebene Inhalt angezeigt werden. Klicken Sie auf die Schaltfläche **Immersive Reader** (Plastischer Reader), um Plastischer Reader mit Ihrem Inhalt zu starten.

## <a name="specify-the-language-of-your-content"></a>Angeben der Sprache Ihres Inhalts

Plastischer Reader unterstützt zahlreiche Sprachen. Gehen Sie wie folgt vor, um die Sprache Ihres Inhalts anzugeben:

1. Öffnen Sie _views\index.pug_, und fügen Sie unter dem Tag `p(id=content)`, das Sie im vorherigen Schritt hinzugefügt haben, den folgenden Code hinzu. Dieser Code fügt Ihrer Seite spanischen Inhalt hinzu.

    ```pug
    p(id='content-spanish') El estudio de las formas terrestres de la Tierra se llama geografía física. Los accidentes geográficos pueden ser montañas y valles. También pueden ser glaciares, lagos o ríos.
    ```

2. Fügen Sie im JavaScript-Code über dem Aufruf von `ImmersiveReader.launchAsync` Folgendes hinzu. Dieser Code übergibt den spanischen Inhalt an Plastischer Reader.

    ```pug
    content.chunks.push({
      content: document.getElementById('content-spanish').innerText + '\n\n',
      lang: 'es'
    });
    ```

3. Navigieren Sie erneut zu _http://localhost:3000_ . Daraufhin sollte der spanische Text auf der Seite angezeigt werden, und wenn Sie auf **Immersive Reader** (Plastischer Reader) klicken, wird er auch in Plastischer Reader angezeigt.

## <a name="specify-the-language-of-the-immersive-reader-interface"></a>Angeben der Benutzeroberflächensprache von Plastischer Reader

Die Benutzeroberflächensprache von Plastischer Reader entspricht standardmäßig den Spracheinstellungen des Browsers. Mit dem folgenden Code können Sie auch die Benutzeroberflächensprache von Plastischer Reader angeben.

1. Ersetzen Sie in _views\index.pug_ den Aufruf von `ImmersiveReader.launchAsync(token, content)` durch den folgenden Code:

    ```javascript
    const options = {
        uiLang: 'fr',
    }
    ImmersiveReader.launchAsync(token, content, options);
    ```

2. Navigieren Sie zu _http://localhost:3000_ . Wenn Sie Plastischer Reader starten, wird die Benutzeroberfläche auf Französisch angezeigt.

## <a name="launch-the-immersive-reader-with-math-content"></a>Starten von Plastischer Reader mit mathematischem Inhalt

Mathematische Inhalte können mithilfe von [MathML](https://developer.mozilla.org/en-US/docs/Web/MathML) in Plastischer Reader eingeschlossen werden.

1. Fügen Sie in _views\index.pug_ über dem Aufruf von `ImmersiveReader.launchAsync` den folgenden Code hinzu:

    ```javascript
    const mathML = '<math xmlns="https://www.w3.org/1998/Math/MathML" display="block"> \
      <munderover> \
        <mo>∫</mo> \
        <mn>0</mn> \
        <mn>1</mn> \
      </munderover> \
      <mrow> \
        <msup> \
          <mi>x</mi> \
          <mn>2</mn> \
        </msup> \
        <mo>ⅆ</mo> \
        <mi>x</mi> \
      </mrow> \
    </math>';

    content.chunks.push({
      content: mathML,
      mimeType: 'application/mathml+xml'
    });
    ```

2. Navigieren Sie zu _http://localhost:3000_ . Wenn Sie Plastischer Reader starten und nach unten scrollen, sehen Sie die mathematische Formel.

## <a name="next-steps"></a>Nächste Schritte

* Machen Sie sich mit dem [Immersive Reader SDK](https://github.com/Microsoft/immersive-reader-sdk) und der [zugehörigen Referenz](./reference.md) vertraut.
* Sehen Sie sich Codebeispiele auf [GitHub](https://github.com/microsoft/immersive-reader-sdk/samples/advanced-csharp).