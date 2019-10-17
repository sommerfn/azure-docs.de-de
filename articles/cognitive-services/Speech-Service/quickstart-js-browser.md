---
title: 'Schnellstart: Erkennen von Sprache, JavaScript (Browser) – Speech-Dienst'
titleSuffix: Azure Cognitive Services
description: Hier finden Sie Informationen zum Erkennen von Sprache in JavaScript in einem Browser mit dem Speech SDK.
services: cognitive-services
author: fmegen
manager: nitinme
ms.service: cognitive-services
ms.subservice: speech-service
ms.topic: quickstart
ms.date: 07/05/2019
ms.author: fmegen
ms.openlocfilehash: 77afdb491e4a6cee0a3d6861ea76f57502e56278
ms.sourcegitcommit: 1d0b37e2e32aad35cc012ba36200389e65b75c21
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 10/15/2019
ms.locfileid: "71802430"
---
# <a name="quickstart-recognize-speech-in-javascript-in-a-browser-using-the-speech-sdk"></a>Schnellstart: Erkennen von Sprache in JavaScript in einem Browser mit dem Speech SDK

[!INCLUDE [Selector](../../../includes/cognitive-services-speech-service-quickstart-selector.md)]

Dieser Artikel enthält Informationen zur Erstellung einer Website mithilfe der JavaScript-Bindung des Cognitive Services Speech SDK zum Umwandeln von Sprache in Text.
Die Anwendung basiert auf dem [Speech SDK für JavaScript](https://aka.ms/csspeech/jsbrowserpackage).

## <a name="prerequisites"></a>Voraussetzungen

* Ein Abonnementschlüssel für den Spracherkennungsdienst. Informationen finden Sie unter [Kostenloses Testen von Speech Services](get-started.md).
* Ein PC oder Mac mit einem funktionierenden Mikrofon
* Ein Texteditor
* Eine aktuelle Version von Chrome, Microsoft Edge oder Safari
* Optional einen Webserver, der das Hosten von PHP-Skripts unterstützt

## <a name="create-a-new-website-folder"></a>Erstellen eines neuen Websiteordners

Erstellen Sie einen neuen leeren Ordner. Wenn Sie das Beispiel auf einem Webserver hosten möchten, stellen Sie sicher, dass der Webserver auf den Ordner zugreifen kann.

## <a name="unpack-the-speech-sdk-for-javascript-into-that-folder"></a>Entpacken des Speech SDK für JavaScript in diesen Ordner

[!INCLUDE [License Notice](../../../includes/cognitive-services-speech-service-license-notice.md)]

Laden Sie das Speech SDK als [ZIP-Paket](https://aka.ms/csspeech/jsbrowserpackage) herunter, und entpacken Sie es in den neu erstellten Ordner. Dabei werden zwei Dateien entpackt: `microsoft.cognitiveservices.speech.sdk.bundle.js` und `microsoft.cognitiveservices.speech.sdk.bundle.js.map`.
Die zweite Datei ist optional und hilfreich beim Debuggen im SDK-Code.

## <a name="create-an-indexhtml-page"></a>Erstellen einer Seite „index.html“

Erstellen Sie eine neue Datei im Ordner `index.html`, und öffnen Sie diese Datei mit einem Text-Editor.

1. Erstellen Sie das folgende HTML-Gerüst:

   ```html
   <html>
   <head>
      <title>Speech SDK JavaScript Quickstart</title>
   </head>
   <body>
    <!-- UI code goes here -->

    <!-- SDK reference goes here -->

    <!-- Optional authorization token request goes here -->

    <!-- Sample code goes here -->
   </body>
   </html>
   ```

1. Fügen Sie den folgenden Benutzeroberflächencode unter dem ersten Kommentar in Ihrer Datei hinzu:

   [!code-html[](~/samples-cognitive-services-speech-sdk/quickstart/js-browser/index.html#uidiv)]

1. Hinzufügen eines Verweises auf das Speech SDK

   [!code-html[](~/samples-cognitive-services-speech-sdk/quickstart/js-browser/index.html#speechsdkref)]

1. Verknüpfen Sie die Handler für die Erkennungsschaltfläche, das Erkennungsergebnis und die mit dem Abonnement verknüpften Felder, die durch den Benutzeroberflächencode definiert werden:

   [!code-html[](~/samples-cognitive-services-speech-sdk/quickstart/js-browser/index.html#quickstartcode)]

## <a name="create-the-token-source-optional"></a>Erstellen der Tokenquelle (optional)

Wenn Sie die Webseite auf einem Webserver hosten möchten, können Sie optional eine Tokenquelle für die Demoanwendung bereitstellen.
Auf diese Weise verlässt Ihr Abonnementschlüssel nie den Server, während Benutzer gleichzeitig die Sprachfunktionen verwenden können, ohne selbst Autorisierungscode eingeben zu müssen.

1. Erstellen Sie eine neue Datei mit dem Namen `token.php`. In diesem Beispiel wird davon ausgegangen, dass der Webserver die PHP-Skriptsprache unterstützt. Geben Sie den folgenden Code ein:

   [!code-php[](~/samples-cognitive-services-speech-sdk/quickstart/js-browser/token.php)]

1. Bearbeiten Sie die Datei `index.html`, und fügen Sie folgenden Code in Ihre Datei ein:

   [!code-html[](~/samples-cognitive-services-speech-sdk/quickstart/js-browser/index.html#authorizationfunction)]

> [!NOTE]
> Autorisierungstoken haben nur eine begrenzte Lebensdauer.
> Dieses vereinfachte Beispiel zeigt nicht, wie Autorisierungstoken automatisch aktualisiert werden. Als Benutzer können Sie die Seite manuell laden oder F5 drücken, um sie zu aktualisieren.

## <a name="build-and-run-the-sample-locally"></a>Erstellen und lokales Ausführen des Beispiels

Um die App zu starten, doppelklicken Sie auf die Datei „index.html“, oder öffnen Sie die Datei „index.html“ mit Ihren bevorzugten Browser. Es wird eine einfache Oberfläche angezeigt, auf der Sie Ihren Abonnementschlüssel und die [Region](regions.md) eingeben und eine Erkennung über das Mikrofon auslösen können.

> [!NOTE]
> Diese Methode kann für den Safari-Browser nicht verwendet werden.
> Für Safari muss die Beispielwebseite auf einem Webserver gehostet werden. In Safari können Websites, die aus einer lokalen Datei geladen werden, das Mikrofon nicht verwenden.

## <a name="build-and-run-the-sample-via-a-web-server"></a>Erstellen und Ausführen des Beispiels über einen Webserver

Starten Sie Ihre App, öffnen Sie Ihren bevorzugten Browser, und verweisen Sie in diesem auf die öffentliche URL, unter der Sie den Ordner hosten. Geben Sie Ihre [Region](regions.md) ein, und lösen Sie über das Mikrofon eine Erkennung aus. Wenn dies konfiguriert wurde, wird ein Token aus der Tokenquelle abgerufen.

## <a name="next-steps"></a>Nächste Schritte

> [!div class="nextstepaction"]
> [JavaScript-Beispiele auf GitHub](https://aka.ms/csspeech/samples)
