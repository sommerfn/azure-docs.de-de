---
title: 'Schnellstart: Erkennen von Sprache aus einer Audiodatei, Python – Speech-Dienst'
titleSuffix: Azure Cognitive Services
description: In dieser Anleitung erfahren Sie, wie Sie eine Konsolenanwendung zur Spracherkennung erstellen, die das Speech SDK für Python verwendet. Danach können Sie das Mikrofon Ihres Computers verwenden, um Sprache in Echtzeit zu transkribieren.
services: cognitive-services
author: chlandsi
manager: nitinme
ms.service: cognitive-services
ms.subservice: speech-service
ms.topic: quickstart
ms.date: 07/05/2019
ms.author: chlandsi
ms.openlocfilehash: 91b136b7986dc3551ebc28b7e35bfe206b273497
ms.sourcegitcommit: c22327552d62f88aeaa321189f9b9a631525027c
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 11/04/2019
ms.locfileid: "73506443"
---
## <a name="prerequisites"></a>Voraussetzungen

Führen Sie die folgenden Schritte aus, bevor Sie beginnen:

> [!div class="checklist"]
> * [Erstellen einer Azure Speech-Ressource](../../../../get-started.md)
> * [Erstellen einer LUIS-Anwendung und Abrufen eines Endpunktschlüssels](../../../../quickstarts/create-luis.md)
> * [Einrichten Ihrer Entwicklungsumgebung](../../../../quickstarts/setup-platform.md)
> * [Erstellen eines leeren Beispielprojekts](../../../../quickstarts/create-project.md)

## <a name="support-and-updates"></a>Support und Updates

Updates für das Speech SDK-Paket für Python werden über PyPI verteilt und in den [Versionshinweisen](~/articles/cognitive-services/Speech-Service/releasenotes.md) angekündigt.
Wenn eine neue Version verfügbar ist, können Sie mit dem Befehl `pip install --upgrade azure-cognitiveservices-speech` auf diese Version aktualisieren.
Die derzeit installierte Version können Sie der Variablen `azure.cognitiveservices.speech.__version__` entnehmen.

Falls Probleme auftreten oder ein Feature fehlt, sehen Sie sich die [Support- und Hilfeoptionen](~/articles/cognitive-services/Speech-Service/support.md) an.

## <a name="create-a-python-application-that-uses-the-speech-sdk"></a>Erstellen einer Python-Anwendung, die das Speech SDK verwendet

### <a name="run-the-sample"></a>Ausführen des Beispiels

Sie können den [Beispielcode](#sample-code) aus diesem Schnellstart in eine Quelldatei (`quickstart.py`) kopieren und in Ihrer integrierten Entwicklungsumgebung (Integrated Development Environment, IDE) oder in der Konsole ausführen:

```sh
python quickstart.py
```

Alternativ können Sie dieses Schnellstarttutorial als [Jupyter](https://jupyter.org) Notebook aus dem [Beispielrepository für das Speech SDK](https://github.com/Azure-Samples/cognitive-services-speech-sdk/) herunterladen und als Notebook ausführen.

### <a name="sample-code"></a>Beispielcode

````Python

import azure.cognitiveservices.speech as speechsdk

# Creates an instance of a speech config with specified subscription key and service region.
# Replace with your own subscription key and service region (e.g., "westus").
speech_key, service_region = "YourSubscriptionKey", "YourServiceRegion"
speech_config = speechsdk.SpeechConfig(subscription=speech_key, region=service_region)

# Creates an audio configuration that points to an audio file.
# Replace with your own audio filename.
audio_filename = "whatstheweatherlike.wav"
audio_input = speechsdk.AudioConfig(filename=audio_filename)

# Creates a recognizer with the given settings
speech_recognizer = speechsdk.SpeechRecognizer(speech_config=speech_config, audio_config=audio_input)

print("Recognizing first result...")


# Starts speech recognition, and returns after a single utterance is recognized. The end of a
# single utterance is determined by listening for silence at the end or until a maximum of 15
# seconds of audio is processed.  The task returns the recognition text as result. 
# Note: Since recognize_once() returns only a single utterance, it is suitable only for single
# shot recognition like command or query. 
# For long-running multi-utterance recognition, use start_continuous_recognition() instead.
result = speech_recognizer.recognize_once()

# Checks result.
if result.reason == speechsdk.ResultReason.RecognizedSpeech:
    print("Recognized: {}".format(result.text))
elif result.reason == speechsdk.ResultReason.NoMatch:
    print("No speech could be recognized: {}".format(result.no_match_details))
elif result.reason == speechsdk.ResultReason.Canceled:
    cancellation_details = result.cancellation_details
    print("Speech Recognition canceled: {}".format(cancellation_details.reason))
    if cancellation_details.reason == speechsdk.CancellationReason.Error:
        print("Error details: {}".format(cancellation_details.error_details))

````

### <a name="install-and-use-the-speech-sdk-with-visual-studio-code"></a>Installieren und Verwenden des Speech SDK mit Visual Studio Code

1. Laden Sie eine 64-Bit-Version (3.5 oder höher) von [Python](https://www.python.org/downloads/) herunter, und installieren Sie sie auf Ihrem Computer.
1. Laden Sie [Visual Studio Code](https://code.visualstudio.com/Download) herunter, und installieren Sie die Software.
1. Öffnen Sie Visual Studio Code, und installieren Sie die Python-Erweiterung. Wählen Sie im Menü **Datei** > **Einstellungen** > **Erweiterungen**. Suchen Sie nach **Python**.

   ![Installieren der Python-Erweiterung](~/articles/cognitive-services/Speech-Service/media/sdk/qs-python-vscode-python-extension.png)

1. Erstellen Sie einen Ordner zum Speichern des Projekts. Ein Beispiel ist die Verwendung von Windows-Explorer.
1. Wählen Sie in Visual Studio Code das Symbol **Datei**. Öffnen Sie dann den von Ihnen erstellten Ordner.

   ![Öffnen eines Ordners](~/articles/cognitive-services/Speech-Service/media/sdk/qs-python-vscode-python-open-folder.png)

1. Erstellen Sie eine neue Python-Quelldatei (`speechsdk.py`), indem Sie das Symbol „Neue Datei“ auswählen.

   ![Erstellen von Dateien](~/articles/cognitive-services/Speech-Service/media/sdk/qs-python-vscode-python-newfile.png)

1. Kopieren Sie den [Python-Code](#sample-code), fügen Sie ihn in die neu erstellte Datei ein, und speichern Sie die Datei.
1. Fügen Sie Ihre Abonnementinformationen für die Speech-Dienste ein.
1. Wenn ein Python-Interpreter ausgewählt wurde, wird er auf der linken Seite der Statusleiste am unteren Rand des Fensters angezeigt.
   Zeigen Sie andernfalls eine Liste der verfügbaren Python-Interpreter an. Öffnen Sie die Befehlspalette (STRG+UMSCHALT+P), und geben Sie **Python: Select Interpreter** ein. Wählen Sie einen passenden Interpreter aus.
1. Sie können das Speech SDK-Paket für Python über Visual Studio Code installieren. Führen Sie diesen Schritt aus, wenn es für den ausgewählten Python-Interpreter noch nicht installiert wurde.
   Öffnen Sie zum Installieren des Speech SDK-Pakets ein Terminal. Rufen Sie erneut die Befehlspalette (STRG+UMSCHALT+P) auf, und geben Sie **Terminal: Neues integriertes Terminal erstellen** eingeben.
   Geben Sie im Terminal, das geöffnet wird, den Befehl `python -m pip install azure-cognitiveservices-speech` bzw. den entsprechenden Befehl für Ihr System ein.
1. Klicken Sie zum Ausführen des Beispielcodes mit der rechten Maustaste an einer beliebigen Stelle im Editor. Wählen Sie **Run Python File in Terminal** (Python-Datei im Terminal ausführen) aus.
   Die ersten 15 Sekunden der Spracheingabe aus Ihrer Audiodatei werden erkannt und im Konsolenfenster protokolliert.

   ```text
   Recognizing first result...
   We recognized: What's the weather like?
   ```

Falls Sie Probleme beim Ausführen dieser Anweisungen haben, können Sie das ausführlichere [Visual Studio Code-Tutorial für Python](https://code.visualstudio.com/docs/python/python-tutorial) zurate ziehen.

## <a name="next-steps"></a>Nächste Schritte

[!INCLUDE [footer](./footer.md)]
