---
title: 'Schnellstart: Erkennen von Sprache, Node.js – Speech-Dienst'
titleSuffix: Azure Cognitive Services
description: In dieser Anleitung erfahren Sie, wie Sie unter Verwendung des Speech SDK für Node.js eine Konsolenanwendung zur Spracherkennung erstellen. Danach können Sie das Mikrofon Ihres Computers verwenden, um Sprache in Echtzeit zu transkribieren.
services: cognitive-services
author: fmegen
manager: nitinme
ms.service: cognitive-services
ms.subservice: speech-service
ms.topic: quickstart
ms.date: 07/05/2019
ms.author: fmegen
ms.openlocfilehash: 1a3d1bc62e995aff43d9538f49b436a7de16f1e9
ms.sourcegitcommit: 7c4de3e22b8e9d71c579f31cbfcea9f22d43721a
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 07/26/2019
ms.locfileid: "68554078"
---
# <a name="quickstart-recognize-speech-with-the-speech-sdk-for-nodejs"></a>Schnellstart: Erkennen von Sprache mit dem Speech SDK für Node.js

[!INCLUDE [Selector](../../../includes/cognitive-services-speech-service-quickstart-selector.md)]

In diesem Artikel wird gezeigt, wie Sie mithilfe der JavaScript-Bindung des Speech SDK für Azure Cognitive Services ein Node.js-Projekt zum Umwandeln von Sprache in Text erstellen.
Die Anwendung basiert auf dem [Speech SDK für JavaScript](https://aka.ms/csspeech/npmpackage).

## <a name="prerequisites"></a>Voraussetzungen

* Ein Azure-Abonnementschlüssel für den Speech-Dienst. [Hier erhalten Sie einen kostenlosen Schlüssel.](get-started.md)
* Eine aktuelle Version von [Node.js](https://nodejs.org).

## <a name="create-a-new-project"></a>Erstellen eines neuen Projekts

Erstellen Sie einen neuen Ordner, und initialisieren Sie das Projekt:

```sh
npm init -f
```

Mit diesem Befehl wird die Datei **package.json** mit Standardwerten initiiert. Diese Datei möchten Sie später wahrscheinlich noch bearbeiten.

## <a name="install-the-speech-sdk"></a>Installieren des Speech SDK

Fügen Sie das Speech SDK Ihrem Node.js-Projekt hinzu:

```
npm install microsoft-cognitiveservices-speech-sdk
```

Mit diesem Befehl werden die neueste Version des Speech SDK sowie alle ggf. erforderlichen Komponenten von **npmjs** herunterladen und installiert. Das SDK wird im Verzeichnis `node_modules` in Ihrem Projektordner installiert.

## <a name="use-the-speech-sdk"></a>Verwenden des Speech SDK

Erstellen Sie im Ordner eine neue Datei namens `index.js`, und öffnen Sie diese Datei mit einem Text-Editor.

> [!NOTE]
> In Node.js unterstützt das Speech SDK weder das Mikrofon noch den Datentyp **Datei**. Beides wird nur in Browsern unterstützt. Verwenden Sie stattdessen die `AudioInputStream.createPushStream()`Streamschnittstelle`AudioInputStream.createPullStream()` für das Speech SDK (entweder über **oder über**).

In diesem Beispiel verwenden wir die Schnittstelle `PushAudioInputStream`.

Fügen Sie den folgenden JavaScript-Code hinzu:

[!code-javascript[Quickstart Code](~/samples-cognitive-services-speech-sdk/quickstart/js-node/index.js#code)]

## <a name="run-the-sample"></a>Ausführen des Beispiels

Passen Sie zum Öffnen der App `YourSubscriptionKey`, `YourServiceRegion` und `YourAudioFile.wav` an Ihre Konfiguration an. Führen Sie sie anschließend durch Aufruf des folgenden Befehls aus:

```sh
node index.js
```

Dadurch wird mithilfe des angegebenen Dateinamens eine Erkennung ausgelöst. Die Ausgabe wird in der Konsole angezeigt.

Beispielsweise wird die folgende Ausgabe zurückgegeben, wenn Sie `index.js` nach der Aktualisierung des Abonnementschlüssels ausführen und die Datei `whatstheweatherlike.wav` verwenden:

```json
SpeechRecognitionResult {
  "privResultId": "9E30EEBD41AC4571BB77CF9164441F46",
  "privReason": 3,
  "privText": "What's the weather like?",
  "privDuration": 15900000,
  "privOffset": 300000,
  "privErrorDetails": null,
  "privJson": {
    "RecognitionStatus": "Success",
    "DisplayText": "What's the weather like?",
    "Offset": 300000,
    "Duration": 15900000
  },
  "privProperties": null
}
```

## <a name="install-and-use-the-speech-sdk-with-visual-studio-code"></a>Installieren und Verwenden des Speech SDK mit Visual Studio Code

Das Beispiel kann auch über Visual Studio Code ausgeführt werden. Gehen Sie wie folgt vor, um den Schnellstart zu installieren, zu öffnen und auszuführen:

1. Starten Sie Visual Studio Code. Wählen Sie **Ordner öffnen**. Navigieren Sie dann zum Schnellstartordner.

   ![Ordner öffnen](media/sdk/qs-js-node-01-open_project.png)

1. Öffnen Sie in Visual Studio Code ein Terminalfenster.

   ![Terminalfenster](media/sdk/qs-js-node-02_open_terminal.png)

1. Führen Sie `npm` aus, um die Abhängigkeiten zu installieren.

   ![Installation mit npm](media/sdk/qs-js-node-03-npm_install.png)

1. Nun können Sie `index.js` öffnen und einen Breakpoint festlegen.

   ![„index.js“ mit einem Breakpoint in Zeile 16](media/sdk/qs-js-node-04-setup_breakpoint.png)

1. Drücken Sie zum Starten des Debuggens entweder F5, oder wählen Sie im Menü „Debuggen“ die Option **Debuggen starten** aus.

   ![Menü „Debuggen“](media/sdk/qs-js-node-05-start_debugging.png)

1. Wenn ein Breakpoint erreicht wird, können Sie die Aufrufliste und die Variablen untersuchen.

   ![Debugger](media/sdk/qs-js-node-06-hit_breakpoint.png)

1. Ausgaben werden im Fenster der Debugging-Konsole angezeigt.

   ![Debugging-Konsole](media/sdk/qs-js-node-07-debug_output.png)

## <a name="next-steps"></a>Nächste Schritte

> [!div class="nextstepaction"]
> [Node.js-Beispiele auf GitHub](https://aka.ms/csspeech/samples)
