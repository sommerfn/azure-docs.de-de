---
title: 'Schnellstart: Erkennen von Sprache, Absichten und Entitäten, Java – Speech-Dienst'
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
ms.openlocfilehash: 441c938e9ab74eafecaf0f9188c0884696eccfae
ms.sourcegitcommit: c22327552d62f88aeaa321189f9b9a631525027c
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 11/04/2019
ms.locfileid: "73506115"
---
## <a name="prerequisites"></a>Voraussetzungen

Führen Sie unbedingt die folgenden Schritte aus, bevor Sie beginnen:

> [!div class="checklist"]
> * [Erstellen einer Azure Speech-Ressource](../../../../get-started.md)
> * [Erstellen einer LUIS-Anwendung und Abrufen eines Endpunktschlüssels](../../../../quickstarts/create-luis.md)
> * [Einrichten Ihrer Entwicklungsumgebung](../../../../quickstarts/setup-platform.md?tabs=jre)
> * [Erstellen eines leeren Beispielprojekts](../../../../quickstarts/create-project.md?tabs=jre)

## <a name="open-your-project"></a>Öffnen Ihres Projekts

Laden Sie Ihr Projekt, und öffnen Sie `Main.java`.

## <a name="start-with-some-boilerplate-code"></a>Beginnen mit Codebausteinen

Fügen Sie Code hinzu, der als Gerüst für das Projekt fungiert.
[!code-java[](~/samples-cognitive-services-speech-sdk/quickstart/java/jre/intent-recognition/src/speechsdk/quickstart/Main.java?range=6-20,69-76)]

## <a name="create-a-speech-configuration"></a>Erstellen einer Speech-Konfiguration

Um ein Objekt vom Typ `IntentRecognizer` initialisieren zu können, müssen Sie eine Konfiguration mit Ihrem LUIS-Endpunktschlüssel und der entsprechenden Region erstellen. Fügen Sie den Code unter „Main“ in den Block „try/catch“ ein.

In diesem Beispiel wird die Methode `FromSubscription()` zum Erstellen von `SpeechConfig` verwendet. Eine vollständige Liste der verfügbaren Methoden finden Sie unter [SpeechConfig-Klasse](https://docs.microsoft.com/dotnet/api/microsoft.cognitiveservices.speech.speechconfig?view=azure-dotnet).

> [!NOTE]
> Wichtig: Verwenden Sie den LUIS-Endpunktschlüssel und keinen Start- oder Erstellungsschlüssel, da nur der Endpunktschlüssel für die Erkennung vom Typ „Sprache-Absichts-Umsetzung“ geeignet ist. Eine Anleitung zum Abrufen des richtigen Schlüssels finden Sie unter [Erstellen einer LUIS-Anwendung und Abrufen eines Endpunktschlüssels](~/articles/cognitive-services/Speech-Service/quickstarts/create-luis.md).

[!code-java[](~/samples-cognitive-services-speech-sdk/quickstart/java/jre/intent-recognition/src/speechsdk/quickstart/Main.java?range=27)]

## <a name="initialize-a-intentrecognizer"></a>Initialisieren eines IntentRecognizer-Objekts

Als Nächstes erstellen wir ein Objekt vom Typ `IntentRecognizer`. Fügen Sie den folgenden Code direkt unterhalb Ihrer Speech-Konfiguration ein:
[!code-java[](~/samples-cognitive-services-speech-sdk/quickstart/java/jre/intent-recognition/src/speechsdk/quickstart/Main.java?range=30)]

## <a name="add-a-languageunderstandingmodel-and-intents"></a>Hinzufügen eines LanguageUnderstandingModel-Objekts und von Absichten

Nun müssen Sie der Absichtserkennung ein Objekt vom Typ `LanguageUnderstandingModel` zuordnen und die zu erkennenden Absichten hinzufügen.
[!code-java[](~/samples-cognitive-services-speech-sdk/quickstart/java/jre/intent-recognition/src/speechsdk/quickstart/Main.java?range=33-36)]

## <a name="recognize-an-intent"></a>Erkennen einer Absicht

Über das Objekt `IntentRecognizer` muss die Methode `recognizeOnceAsync()` aufgerufen werden. Diese Methode teilt dem Speech-Dienst mit, dass Sie einen einzelnen Ausdruck zur Erkennung senden und dass die Spracherkennung beendet werden soll, sobald der Ausdruck ermittelt wurde.

[!code-java[](~/samples-cognitive-services-speech-sdk/quickstart/java/jre/intent-recognition/src/speechsdk/quickstart/Main.java?range=41)]

## <a name="display-the-recognition-results-or-errors"></a>Anzeigen der Erkennungsergebnisse (oder Fehler)

Wenn das Erkennungsergebnis vom Speech-Dienst zurückgegeben wird, möchten Sie es natürlich in irgendeiner Form verwenden. In diesem Beispiel geben wir das Ergebnis einfach in der Konsole aus.

Fügen Sie unterhalb des Aufrufs von `recognizeOnceAsync()` den folgenden Code hinzu: [!code-java[](~/samples-cognitive-services-speech-sdk/quickstart/java/jre/intent-recognition/src/speechsdk/quickstart/Main.java?range=44-65)]

## <a name="release-resources"></a>Freigeben von Ressourcen

Es ist wichtig, die Speech-Ressourcen freizugeben, wenn Sie sie nicht mehr benötigen. Fügen Sie den folgenden Code am Ende des Blocks „try/catch“ ein: [!code-java[](~/samples-cognitive-services-speech-sdk/quickstart/java/jre/intent-recognition/src/speechsdk/quickstart/Main.java?range=67-68)]

## <a name="check-your-code"></a>Überprüfen Ihres Codes

Ihr Code sollte nun wie folgt aussehen: (Wir haben dieser Version einige Kommentare hinzugefügt.) [!code-java[](~/samples-cognitive-services-speech-sdk/quickstart/java/jre/intent-recognition/src/speechsdk/quickstart/Main.java?range=6-76)]

## <a name="build-and-run-your-app"></a>Erstellen und Ausführen Ihrer App

Drücken Sie F11, oder wählen Sie **Ausführen** > **Debuggen** aus.
Die nächsten 15 Sekunden der Spracheingabe vom Mikrofon werden erkannt und im Konsolenfenster protokolliert.

## <a name="next-steps"></a>Nächste Schritte

[!INCLUDE [footer](./footer.md)]