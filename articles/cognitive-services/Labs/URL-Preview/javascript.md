---
title: 'Schnellstart: Projekt: URL-Vorschau, JavaScript'
titlesuffix: Azure Cognitive Services
description: Skriptbeispiel für den schnellen Einstieg in die Verwendung der Bing-URL-Vorschau-API mit JavaScript.
services: cognitive-services
author: mikedodaro
manager: nitinme
ms.service: cognitive-services
ms.subservice: url-preview
ms.topic: quickstart
ms.date: 03/16/2018
ms.author: rosh
ROBOTS: NOINDEX
ms.openlocfilehash: ec8a7ceef8cd938984b8eedc8397f6f67294c990
ms.sourcegitcommit: ad9120a73d5072aac478f33b4dad47bf63aa1aaa
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 08/01/2019
ms.locfileid: "68706930"
---
# <a name="quickstart-url-preview-in-javascript"></a>Schnellstart: URL-Vorschau in JavaScript 

Die folgende einseitige Anwendung verwendet JavaScript zum Erstellen einer URL-Vorschau für die SwiftKey-Website: https://swiftkey.com/en. 

## <a name="prerequisites"></a>Voraussetzungen

Rufen Sie einen Zugriffsschlüssel für die kostenlose Testversion von [Cognitive Services Labs](https://labs.cognitive.microsoft.com/en-us/project-url-preview) ab.

## <a name="code-scenario"></a>Codeszenario
Das folgende Javascript-Beispiel enthält ein Textfeld-Eingabeobjekt, in das der Benutzer die URL eingibt, für die eine Vorschau ausgeführt werden soll.  Wenn der Benutzer auf die Schaltfläche **Preview** (Vorschau) klickt, wird die onclick-Methode an `getPreview` weitergeleitet, wo Code eine Webanforderung für den **UrlPreview**-Endpunkt generiert.

Der Code erstellt eine *XMLHttpRequest*, fügt den *Ocp-Apim-Subscription-Key*-Header und -Schlüssel hinzu und sendet die Anforderung.  Er fügt einen asynchronen Ereignishandler zum Verarbeiten der Antwort hinzu.

Wenn die Antwort erfolgreich zurückgegeben wird, weist der Handler den JSON-Text der Antwort dem `demo`-Absatz auf der Seite zu. Andere Elemente der Antwort sind für die Anzeige auf die folgenden Absätze festgelegt.

**Unformatierte JSON-Antwort**

```
{
  "_type": "WebPage",
  "name": "SwiftKey - Smart prediction technology for easier mobile typing",
  "url": "https://swiftkey.com/en",
  "description": "Discover the best Android and iPhone and iPad apps for faster, easier typing with emoji, colorful themes and more - download SwiftKey Keyboard free today.",
  "isFamilyFriendly": true,
  "primaryImageOfPage": {
    "contentUrl": "https://swiftkey.com/images/og/default.jpg"
  }
}

```

**Die ausgeführte Demo**

![JavaScript-Beispiel für eine URL-Vorschau](./media/java-script-demo.png)

## <a name="running-the-application"></a>Ausführen der Anwendung

So führen Sie die Anwendung aus:

1. Ersetzen Sie den `YOUR-SUBSCRIPTION-KEY`-Wert durch einen für Ihr Abonnement gültigen Zugriffsschlüssel.
2. Speichern Sie den HTML-Code und das Skript in einer Datei mit HTML-Erweiterung.
3. Führen Sie die Webseite im Browser aus.
4. Verwenden Sie die vorhandene URL, oder geben Sie eine andere in das Textfeld ein.
5. Klicken Sie auf die Schaltfläche **Vorschau**.

**Quellcode:**

```
<!DOCTYPE html>

<html lang="en" xmlns="http://www.w3.org/1999/xhtml">
<head>
    <meta charset="utf-8" />
    <title>urlPreview Demo</title>
</head>
<body>
    <h3>URL Preview Demo</h3>

    Page to preview: <input type="url" id="myURL" value="https://swiftkey.com/en">
    <button onclick="getPreview()">Preview</button>

    <p id="demo"></p>
    <br />
    <p id="jsonDesc"></p>
    <p><a id="familyFriendly"></a></p>
    <a id="contentUrl"></a>
    <p />
    <img id="jsonImage" />

    <script>

        var BING_ENDPOINT = "https://api.labs.cognitive.microsoft.com/urlpreview/v7.0/search"; 
        var key = "YOUR-SUBSCRIPTION-KEY";
        var xhr;

        function getPreview() {
            xhr = new XMLHttpRequest();

            var queryUrl = BING_ENDPOINT + "?q=" +
                encodeURIComponent(document.getElementById("myURL").value);
            xhr.open('GET', queryUrl, true);
            xhr.setRequestHeader("Ocp-Apim-Subscription-Key", key);

            xhr.send();

            xhr.addEventListener("readystatechange", processRequest, false);
        }

        function processRequest(e) {

            if (xhr.readyState == 4 && xhr.status == 200) {
                document.getElementById("demo").innerHTML = xhr.responseText;
                var obj = JSON.parse(xhr.responseText);
                document.getElementById("jsonDesc").innerHTML = obj.description;
                document.getElementById("familyFriendly").innerHTML = "Family Friendly: " + obj.isFamilyFriendly;
                document.getElementById("contentUrl").innerHTML = obj.url;
                document.getElementById("contentUrl").href = obj.url;
                document.getElementById("jsonImage").width = 350;
                document.getElementById("jsonImage").src = obj.primaryImageOfPage.contentUrl;

            }

        }

    </script>

</body>
</html>

```

## <a name="next-steps"></a>Nächste Schritte
- [C#-Schnellstart](csharp.md)
- [Java-Schnellstart](java-quickstart.md)
- [Node-Schnellstart](node-quickstart.md)
- [Python-Schnellstart](python-quickstart.md)
