---
title: 'Schnellstart: Erkennen von Sprache aus einer Audiodatei, C++ (Windows) – Spracherkennungsdienst'
titleSuffix: Azure Cognitive Services
description: Hier erfahren Sie, wie Sie mit dem Speech SDK Sprache in C++ unter Windows Desktop erkennen.
services: cognitive-services
author: wolfma61
manager: nitinme
ms.service: cognitive-services
ms.subservice: speech-service
ms.topic: quickstart
ms.date: 08/19/2019
ms.author: wolfma
ms.openlocfilehash: 09e0b6fbfcb8541a0756a855d4548e456a7b5c90
ms.sourcegitcommit: c22327552d62f88aeaa321189f9b9a631525027c
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 11/04/2019
ms.locfileid: "73506379"
---
## <a name="prerequisites"></a>Voraussetzungen

Führen Sie die folgenden Schritte aus, bevor Sie beginnen:

> [!div class="checklist"]
> * [Erstellen einer Azure Speech-Ressource](../../../../get-started.md)
> * [Einrichten Ihrer Entwicklungsumgebung](../../../../quickstarts/setup-platform.md?tabs=windows)
> * [Erstellen eines leeren Beispielprojekts](../../../../quickstarts/create-project.md?tabs=windows)

## <a name="add-sample-code"></a>Hinzufügen von Beispielcode

1. Öffnen Sie die Quelldatei **helloworld.cpp**.

1. Ersetzen Sie den gesamten Code durch den folgenden Codeausschnitt:

   ````C++

    // Creates an instance of a speech config with specified subscription key and service region.
    // Replace with your own subscription key and service region (e.g., "westus").
    auto config = SpeechConfig::FromSubscription("YourSubscriptionKey", "YourServiceRegion");

    // Creates a speech recognizer using a WAV file. The default language is "en-us".
    // Replace with your own audio file name.
    auto audioInput = AudioConfig::FromWavFileInput("whatstheweatherlike.wav");
    auto recognizer = SpeechRecognizer::FromConfig(config, audioInput);
    cout << "Recognizing first result...\n";

    // Starts speech recognition, and returns after a single utterance is recognized. The end of a
    // single utterance is determined by listening for silence at the end or until a maximum of 15
    // seconds of audio is processed.  The task returns the recognition text as result.
    // Note: Since RecognizeOnceAsync() returns only a single utterance, it is suitable only for single
    // shot recognition like command or query.
    // For long-running multi-utterance recognition, use StartContinuousRecognitionAsync() instead.
    auto result = recognizer->RecognizeOnceAsync().get();

    // Checks result.
    if (result->Reason == ResultReason::RecognizedSpeech)
    {
        cout << "RECOGNIZED: Text=" << result->Text << std::endl;
    }
    else if (result->Reason == ResultReason::NoMatch)
    {
        cout << "NOMATCH: Speech could not be recognized." << std::endl;
    }
    else if (result->Reason == ResultReason::Canceled)
    {
        auto cancellation = CancellationDetails::FromResult(result);
        cout << "CANCELED: Reason=" << (int)cancellation->Reason << std::endl;

        if (cancellation->Reason == CancellationReason::Error)
        {
            cout << "CANCELED: ErrorCode=" << (int)cancellation->ErrorCode << std::endl;
            cout << "CANCELED: ErrorDetails=" << cancellation->ErrorDetails << std::endl;
            cout << "CANCELED: Did you update the subscription info?" << std::endl;
        }
    }

   ````

1. Ersetzen Sie in der gleichen Datei die Zeichenfolge `YourSubscriptionKey` durch Ihren Abonnementschlüssel.

1. Ersetzen Sie die Zeichenfolge `YourServiceRegion` durch die [Region](~/articles/cognitive-services/Speech-Service/regions.md), die mit Ihrem Abonnement verknüpft ist (z. B. `westus` für das kostenlose Testabonnement).

1. Ersetzen Sie die Zeichenfolge `whatstheweatherlike.wav` durch ihren eigenen Dateinamen.

1. Wählen Sie auf der Menüleiste **Datei** > **Alle speichern** aus.

## <a name="build-and-run-the-application"></a>Erstellen und Ausführen der Anwendung

1. Wählen Sie auf der Menüleiste **Erstellen** > **Projektmappe erstellen** aus, um die Anwendung zu erstellen. Der Code sollte nun ohne Fehler kompiliert werden.

1. Wählen Sie **Debuggen** > **Debuggen starten** aus (oder drücken Sie**F5**), um die Anwendung **helloworld** zu starten.

1. Ihre Audiodatei wird an die Spracherkennungsdienste übermittelt, und die erste Äußerung in der Datei wird in Text transkribiert, der im gleichen Fenster angezeigt wird.

   ```text
   Recognizing first result...
   We recognized: What's the weather like?
   ```

## <a name="next-steps"></a>Nächste Schritte

[!INCLUDE [footer](./footer.md)]