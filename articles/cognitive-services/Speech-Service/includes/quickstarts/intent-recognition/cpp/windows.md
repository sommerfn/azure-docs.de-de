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
ms.openlocfilehash: 76b7c9436b8d1d466a69df7e5427991df0a9c63e
ms.sourcegitcommit: 5a8c65d7420daee9667660d560be9d77fa93e9c9
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 11/15/2019
ms.locfileid: "74125490"
---
## <a name="prerequisites"></a>Voraussetzungen

Führen Sie die folgenden Schritte aus, bevor Sie beginnen:

> [!div class="checklist"]
>
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

## <a name="create-a-speech-configuration"></a>Erstellen einer Speech-Konfiguration

Damit Sie ein `IntentRecognizer`-Objekt initialisieren können, müssen Sie eine Konfiguration erstellen, in der Ihr LUIS-Endpunktschlüssel und Ihre Region verwendet werden. Fügen Sie diesen Code in die Methode `recognizeIntent()` ein.

In diesem Beispiel wird die Methode `FromSubscription()` zum Erstellen von `SpeechConfig` verwendet. Eine vollständige Liste der verfügbaren Methoden finden Sie unter [SpeechConfig-Klasse](https://docs.microsoft.com/cpp/cognitive-services/speech/speechconfig).

> [!NOTE]
> Sie dürfen weder den Start- noch den Erstellungsschlüssel, sondern müssen den LUIS-Endpunktschlüssel verwenden, da nur der Endpunktschlüssel für die Erkennung vom Typ „Sprache-Absichts-Umsetzung“ zulässig ist. Eine Anleitung zum Abrufen des richtigen Schlüssels finden Sie unter [Erstellen einer LUIS-Anwendung und Abrufen eines Endpunktschlüssels](~/articles/cognitive-services/Speech-Service/quickstarts/create-luis.md).

[!code-cpp[](~/samples-cognitive-services-speech-sdk/quickstart/cpp/windows/intent-recognition/helloworld/helloworld.cpp?range=25)]

## <a name="initialize-an-intentrecognizer"></a>Initialisieren eines IntentRecognizer-Objekts

Als Nächstes erstellen wir ein Objekt vom Typ `IntentRecognizer`. Fügen Sie diesen Code in die Methode `recognizeIntent()` direkt unterhalb der Sprachkonfiguration ein.
[!code-cpp[](~/samples-cognitive-services-speech-sdk/quickstart/cpp/windows/intent-recognition/helloworld/helloworld.cpp?range=28)]

## <a name="add-a-languageunderstandingmodel-and-intents"></a>Hinzufügen von LanguageUnderstandingModel und Absichten

Nun müssen Sie der Absichtserkennung ein `LanguageUnderstandingModel` zuordnen und die zu erkennenden Absichten hinzufügen.
[!code-cpp[](~/samples-cognitive-services-speech-sdk/quickstart/cpp/windows/intent-recognition/helloworld/helloworld.cpp?range=31-34)]

## <a name="recognize-an-intent"></a>Erkennen einer Absicht

Rufen Sie die Methode `RecognizeOnceAsync()` über das Objekt `IntentRecognizer` auf. Diese Methode teilt dem Spracherkennungsdienst mit, dass Sie einen einzelnen Ausdruck zur Erkennung senden, und dass die Spracherkennung beendet werden soll, sobald der Ausdruck ermittelt wurde.
Der Einfachheit halber warten wir auf die kommende Rückgabe, um fortzufahren.

Fügen Sie in der using-Anweisung den folgenden Code hinzu: [!code-cpp[](~/samples-cognitive-services-speech-sdk/quickstart/cpp/windows/intent-recognition/helloworld/helloworld.cpp?range=44)]

## <a name="display-the-recognition-results-or-errors"></a>Anzeigen der Erkennungsergebnisse (oder Fehler)

Wenn das Erkennungsergebnis vom Speech-Dienst zurückgegeben wird, möchten Sie es natürlich in irgendeiner Form verwenden. In diesem Beispiel geben wir das Ergebnis einfach in der Konsole aus.

Fügen Sie in der using-Anweisung unterhalb von `RecognizeOnceAsync()` den folgenden Code ein: [!code-cpp[](~/samples-cognitive-services-speech-sdk/quickstart/cpp/windows/intent-recognition/helloworld/helloworld.cpp?range=47-72)]

## <a name="check-your-code"></a>Überprüfen des Codes

Ihr Code sollte nun wie folgt aussehen:  
(Wir haben dieser Version einige Kommentare hinzugefügt.) [!code-cpp[](~/samples-cognitive-services-speech-sdk/quickstart/cpp/windows/intent-recognition/helloworld/helloworld.cpp?range=6-81)]

## <a name="build-and-run-your-app"></a>Erstellen und Ausführen der App

Sie können nun Ihre App erstellen und die Spracherkennung mit dem Speech-Dienst testen.

1. **Kompilieren des Codes**: Wählen Sie auf der Menüleiste von Visual Studio **Build** > **Projektmappe erstellen** aus.
2. **Starten der App**: Wählen Sie auf der Menüleiste **Debuggen** > **Debuggen starten** aus, oder drücken Sie **F5**.
3. **Starten der Erkennung**: Sie werden aufgefordert, etwas auf Englisch zu sagen. Ihre Spracheingabe wird an den Speech-Dienst gesendet, in Text transkribiert und in der Konsole ausgegeben.

## <a name="next-steps"></a>Nächste Schritte

[!INCLUDE [footer](./footer.md)]