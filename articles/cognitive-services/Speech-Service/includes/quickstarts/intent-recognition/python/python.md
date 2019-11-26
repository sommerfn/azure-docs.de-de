---
title: 'Schnellstart: Erkennen von Sprache, Absichten und Entitäten, Python – Speech-Dienst'
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
ms.openlocfilehash: 4efa535118d075addf78b2e9be6a645c458d6bb4
ms.sourcegitcommit: 5a8c65d7420daee9667660d560be9d77fa93e9c9
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 11/15/2019
ms.locfileid: "74125489"
---
## <a name="prerequisites"></a>Voraussetzungen

Führen Sie die folgenden Schritte aus, bevor Sie beginnen:

> [!div class="checklist"]
>
> * [Erstellen einer Azure Speech-Ressource](../../../../get-started.md)
> * [Erstellen einer LUIS-Anwendung und Abrufen eines Endpunktschlüssels](../../../../quickstarts/create-luis.md)
> * [Einrichten Ihrer Entwicklungsumgebung](../../../../quickstarts/setup-platform.md)
> * [Erstellen eines leeren Beispielprojekts](../../../../quickstarts/create-project.md)

## <a name="open-your-project"></a>Öffnen Ihres Projekts

Öffnen Sie „Quickstart.py“ in Ihrem Python-Editor.

## <a name="start-with-some-boilerplate-code"></a>Beginnen mit Codebausteinen

Fügen Sie Code hinzu, der als Gerüst für das Projekt fungiert.
[!code-python[](~/samples-cognitive-services-speech-sdk/quickstart/python/intent-recognition/quickstart.py?range=5-7)]

## <a name="create-a-speech-configuration"></a>Erstellen einer Speech-Konfiguration

Damit Sie ein `IntentRecognizer`-Objekt initialisieren können, müssen Sie eine Konfiguration erstellen, in der Ihr LUIS-Endpunktschlüssel und Ihre Region verwendet werden. Fügen Sie diesen Code als Nächstes ein.

In diesem Beispiel wird das Objekt `SpeechConfig` mit dem LUIS-Schlüssel und der entsprechenden Region erstellt. Eine vollständige Liste der verfügbaren Methoden finden Sie unter [SpeechConfig-Klasse](https://docs.microsoft.com/python/api/azure-cognitiveservices-speech/azure.cognitiveservices.speech.speechconfig).

> [!NOTE]
> Sie dürfen weder den Start- noch den Erstellungsschlüssel, sondern müssen den LUIS-Endpunktschlüssel verwenden, da nur der Endpunktschlüssel für die Erkennung vom Typ „Sprache-Absichts-Umsetzung“ zulässig ist. Eine Anleitung zum Abrufen des richtigen Schlüssels finden Sie unter [Erstellen einer LUIS-Anwendung und Abrufen eines Endpunktschlüssels](~/articles/cognitive-services/Speech-Service/quickstarts/create-luis.md).

[!code-python[](~/samples-cognitive-services-speech-sdk/quickstart/python/intent-recognition/quickstart.py?range=12)]

## <a name="initialize-an-intentrecognizer"></a>Initialisieren eines IntentRecognizer-Objekts

Als Nächstes erstellen wir ein Objekt vom Typ `IntentRecognizer`. Fügen Sie den folgenden Code direkt unterhalb Ihrer Speech-Konfiguration ein:
[!code-python[](~/samples-cognitive-services-speech-sdk/quickstart/python/intent-recognition/quickstart.py?range=15)]

## <a name="add-a-languageunderstandingmodel-and-intents"></a>Hinzufügen eines LanguageUnderstandingModel-Objekts und von Absichten

Nun müssen Sie der Absichtserkennung ein `LanguageUnderstandingModel` zuordnen und die zu erkennenden Absichten hinzufügen.
[!code-python[](~/samples-cognitive-services-speech-sdk/quickstart/python/intent-recognition/quickstart.py?range=19-27)]

## <a name="recognize-an-intent"></a>Erkennen einer Absicht

Rufen Sie die Methode `recognize_once()` über das Objekt `IntentRecognizer` auf. Diese Methode teilt dem Spracherkennungsdienst mit, dass Sie einen einzelnen Ausdruck zur Erkennung senden, und dass die Spracherkennung beendet werden soll, sobald der Ausdruck ermittelt wurde.
[!code-python[](~/samples-cognitive-services-speech-sdk/quickstart/python/intent-recognition/quickstart.py?range=35)]

## <a name="display-the-recognition-results-or-errors"></a>Anzeigen der Erkennungsergebnisse (oder Fehler)

Wenn das Erkennungsergebnis vom Speech-Dienst zurückgegeben wird, möchten Sie es natürlich in irgendeiner Form verwenden. In diesem Beispiel geben wir das Ergebnis einfach in der Konsole aus.

Fügen Sie in der using-Anweisung unterhalb des Aufrufs von `recognize_once()` den folgenden Code hinzu: [!code-python[](~/samples-cognitive-services-speech-sdk/quickstart/python/intent-recognition/quickstart.py?range=38-47)]

## <a name="check-your-code"></a>Überprüfen Ihres Codes

Ihr Code sollte nun wie folgt aussehen:  
(Wir haben dieser Version einige Kommentare hinzugefügt.) [!code-python[](~/samples-cognitive-services-speech-sdk/quickstart/python/intent-recognition/quickstart.py?range=5-47)]

## <a name="build-and-run-your-app"></a>Erstellen und Ausführen Ihrer App

Führen Sie das Beispiel über die Konsole oder in Ihrer IDE aus:

    ````
    python quickstart.py
    ````

Die nächsten 15 Sekunden der Spracheingabe vom Mikrofon werden erkannt und im Konsolenfenster protokolliert.

## <a name="next-steps"></a>Nächste Schritte

[!INCLUDE [footer](./footer.md)]
