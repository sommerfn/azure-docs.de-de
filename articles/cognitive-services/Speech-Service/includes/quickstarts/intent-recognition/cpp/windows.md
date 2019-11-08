---
title: 'Schnellstart: Erkennen von Sprache, Absichten und Entitäten, C++ – Spracherkennungsdienst'
titleSuffix: Azure Cognitive Services
description: TBD
services: cognitive-services
author: erhopf
manager: nitinme
ms.service: cognitive-services
ms.subservice: speech-service
ms.topic: quickstart
ms.date: 10/28/2019
ms.author: erhopf
zone_pivot_groups: programming-languages-set-two
ms.openlocfilehash: b26b5edeaac1f6305ed2db920c711f906eb10384
ms.sourcegitcommit: c22327552d62f88aeaa321189f9b9a631525027c
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 11/04/2019
ms.locfileid: "73506019"
---
## <a name="prerequisites"></a>Voraussetzungen

Führen Sie die folgenden Schritte aus, bevor Sie beginnen:

> [!div class="checklist"]
> * [Erstellen einer Azure Speech-Ressource](../../../../get-started.md)
> * [Erstellen einer LUIS-Anwendung und Abrufen eines Endpunktschlüssels](../../../../quickstarts/create-luis.md)
> * [Einrichten Ihrer Entwicklungsumgebung](../../../../quickstarts/setup-platform.md?tabs=windows)
> * [Erstellen eines leeren Beispielprojekts](../../../../quickstarts/create-project.md?tabs=windows)

## <a name="open-your-project-in-visual-studio"></a>Öffnen Sie Ihr Projekt in Visual Studio.

Im ersten Schritt müssen Sie sicherstellen, dass das Projekt in Visual Studio geöffnet ist.

1. Starten Sie Visual Studio 2019.
2. Laden Sie das Projekt, und öffnen Sie `helloworld.cpp`.

## <a name="start-with-some-boilerplate-code"></a>Beginnen mit Codebausteinen

Fügen Sie Code hinzu, der als Gerüst für das Projekt fungiert. Beachten Sie, dass Sie eine asynchrone Methode mit dem Namen `recognizeIntent()` erstellt haben.
[!code-cpp[](~/samples-cognitive-services-speech-sdk/quickstart/cpp/windows/intent-recognition/helloworld/helloworld.cpp?range=6-16,73-81)]

## <a name="create-a-speech-configuration"></a>Erstellen einer Sprachkonfiguration

Um ein Objekt vom Typ `IntentRecognizer` initialisieren zu können, müssen Sie eine Konfiguration mit Ihrem LUIS-Endpunktschlüssel und der entsprechenden Region erstellen. Fügen Sie diesen Code in die Methode `recognizeIntent()` ein.

In diesem Beispiel wird die Methode `FromSubscription()` zum Erstellen von `SpeechConfig` verwendet. Eine vollständige Liste der verfügbaren Methoden finden Sie unter [SpeechConfig-Klasse](https://docs.microsoft.com/cpp/cognitive-services/speech/speechconfig).

> [!NOTE]
> Wichtig: Verwenden Sie den LUIS-Endpunktschlüssel und keinen Starter- oder Erstellungsschlüssel, da nur der Endpunktschlüssel für die Erkennung vom Typ „Sprache-Absichts-Umsetzung“ geeignet ist. Eine Anleitung zum Abrufen des richtigen Schlüssels finden Sie unter [Erstellen einer LUIS-Anwendung und Abrufen eines Endpunktschlüssels](~/articles/cognitive-services/Speech-Service/quickstarts/create-luis.md).

[!code-cpp[](~/samples-cognitive-services-speech-sdk/quickstart/cpp/windows/intent-recognition/helloworld/helloworld.cpp?range=25)]

## <a name="initialize-a-intentrecognizer"></a>Initialisieren eines IntentRecognizer-Objekts

Als Nächstes erstellen wir ein Objekt vom Typ `IntentRecognizer`. Fügen Sie diesen Code in die Methode `recognizeIntent()` direkt unterhalb der Sprachkonfiguration ein.
[!code-cpp[](~/samples-cognitive-services-speech-sdk/quickstart/cpp/windows/intent-recognition/helloworld/helloworld.cpp?range=28)]

## <a name="add-a-languageunderstandingmodel-and-intents"></a>Hinzufügen eines LanguageUnderstandingModel-Objekts und von Absichten

Nun müssen Sie der Absichtserkennung ein Objekt vom Typ `LanguageUnderstandingModel` zuordnen und die zu erkennenden Absichten hinzufügen.
[!code-cpp[](~/samples-cognitive-services-speech-sdk/quickstart/cpp/windows/intent-recognition/helloworld/helloworld.cpp?range=31-34)]

## <a name="recognize-an-intent"></a>Erkennen einer Absicht

Über das Objekt `IntentRecognizer` muss die Methode `RecognizeOnceAsync()` aufgerufen werden. Diese Methode teilt dem Spracherkennungsdienst mit, dass Sie einen einzelnen Ausdruck zur Erkennung senden, und dass die Spracherkennung beendet werden soll, sobald der Ausdruck ermittelt wurde.
Der Einfachheit halber warten wir auf die kommende Rückgabe, um fortzufahren.

Fügen Sie in der using-Anweisung den folgenden Code hinzu: [!code-cpp[](~/samples-cognitive-services-speech-sdk/quickstart/cpp/windows/intent-recognition/helloworld/helloworld.cpp?range=44)]

## <a name="display-the-recognition-results-or-errors"></a>Anzeigen der Erkennungsergebnisse (oder Fehler)

Wenn das Erkennungsergebnis vom Spracherkennungsdienst zurückgegeben wird, möchten Sie es natürlich in irgendeiner Form verwenden. In diesem Beispiel geben wir das Ergebnis einfach in der Konsole aus.

Fügen Sie in der using-Anweisung unterhalb von `RecognizeOnceAsync()` den folgenden Code ein: [!code-cpp[](~/samples-cognitive-services-speech-sdk/quickstart/cpp/windows/intent-recognition/helloworld/helloworld.cpp?range=47-72)]

## <a name="check-your-code"></a>Überprüfen des Codes

Ihr Code sollte nun wie folgt aussehen: (Wir haben dieser Version einige Kommentare hinzugefügt.) [!code-cpp[](~/samples-cognitive-services-speech-sdk/quickstart/cpp/windows/intent-recognition/helloworld/helloworld.cpp?range=6-81)]

## <a name="build-and-run-your-app"></a>Erstellen und Ausführen der App

Sie können nun Ihre App erstellen und die Spracherkennung mit dem Spracherkennungsdienst testen.

1. **Kompilieren des Codes:** Wählen Sie auf der Menüleiste von Visual Studio **Build** > **Projektmappe erstellen** aus.
2. **Starten der App:** Wählen Sie auf der Menüleiste **Debuggen** > **Debuggen starten** aus, oder drücken Sie **F5**.
3. **Starten der Erkennung**: Sie werden aufgefordert, etwas auf Englisch zu sagen. Ihre Spracheingabe wird an den Spracherkennungsdienst gesendet, in Text transkribiert und in der Konsole gerendert.

## <a name="next-steps"></a>Nächste Schritte

[!INCLUDE [footer](./footer.md)]