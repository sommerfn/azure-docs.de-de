---
title: 'Schnellstart: Synthetisieren von Sprache, C++ (Windows): Speech-Dienst'
titleSuffix: Azure Cognitive Services
description: Hier erfahren Sie, wie Sie mit dem Speech SDK Sprache in C++ unter Windows Desktop synthetisieren.
services: cognitive-services
author: yinhew
manager: nitinme
ms.service: cognitive-services
ms.subservice: speech-service
ms.topic: quickstart
ms.date: 08/24/2019
ms.author: yinhew
ms.openlocfilehash: 686b21d3e02266af77687778c32f0d1ca6d55154
ms.sourcegitcommit: 49c4b9c797c09c92632d7cedfec0ac1cf783631b
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 09/05/2019
ms.locfileid: "70383077"
---
# <a name="quickstart-synthesize-speech-in-c-on-windows-by-using-the-speech-sdk"></a>Schnellstart: Synthetisieren von Sprache in C++ unter Windows mit dem Speech SDK

Schnellstarts sind auch für [Spracherkennung](quickstart-cpp-windows.md) und [Sprachübersetzung](quickstart-translate-speech-cpp-windows.md) verfügbar.

In diesem Artikel erstellen Sie eine C++-Konsolenanwendung für Windows. Mit dem [Speech SDK](speech-sdk.md) von Cognitive Services können Sie Text in Echtzeit in Sprache synthetisieren und das Ergebnis über den Lautsprecher Ihres PCs ausgeben. Die Anwendung basiert auf dem [NuGet-Paket für das Speech SDK](https://aka.ms/csspeech/nuget) und auf Microsoft Visual Studio 2019 (beliebige Edition).

Eine vollständige Liste mit den verfügbaren Sprachen/Stimmen für die Sprachsynthese finden Sie auf der Seite zur [Sprachunterstützung](language-support.md#text-to-speech).

## <a name="prerequisites"></a>Voraussetzungen

Für diese Schnellstartanleitung benötigen Sie einen Abonnementschlüssel für die Speech-Dienste. Sie können einen solchen Schlüssel kostenlos abrufen. Ausführliche Informationen finden Sie unter [Kostenloses Testen der Spracherkennungsdienste](get-started.md).

## <a name="create-a-visual-studio-project"></a>Erstellen eines Visual Studio-Projekts

[!INCLUDE [Quickstart C++ project](../../../includes/cognitive-services-speech-service-quickstart-cpp-create-proj.md)]

## <a name="add-sample-code"></a>Hinzufügen von Beispielcode

1. Öffnen Sie die Quelldatei **helloworld.cpp**.

1. Ersetzen Sie den gesamten Code durch den folgenden Codeausschnitt:

   [!code-cpp[Quickstart Code](~/samples-cognitive-services-speech-sdk/quickstart/text-to-speech/cpp-windows/helloworld/helloworld.cpp#code)]

1. Ersetzen Sie in der gleichen Datei die Zeichenfolge `YourSubscriptionKey` durch Ihren Abonnementschlüssel.

1. Ersetzen Sie die Zeichenfolge `YourServiceRegion` durch die [Region](regions.md), die mit Ihrem Abonnement verknüpft ist (z. B. `westus` für das kostenlose Testabonnement).

1. Wählen Sie auf der Menüleiste **Datei** > **Alle speichern** aus.

## <a name="build-and-run-the-application"></a>Erstellen und Ausführen der Anwendung

1. Wählen Sie auf der Menüleiste **Erstellen** > **Projektmappe erstellen** aus, um die Anwendung zu erstellen. Der Code sollte nun ohne Fehler kompiliert werden.

1. Wählen Sie **Debuggen** > **Debuggen starten** aus (oder drücken Sie**F5**), um die Anwendung **helloworld** zu starten.

1. Geben Sie einen englischen Ausdruck oder Satz ein. Die Anwendung überträgt Ihren Text an die Speech-Dienste, die die synthetisierte Sprache zur Wiedergabe über den Lautspreche an die Anwendung sendet.

   ![Konsolenausgabe nach erfolgreicher Sprachsynthese](media/sdk/qs-tts-cpp-windows-console-output.png)

## <a name="next-steps"></a>Nächste Schritte

Weitere Beispiele (etwa zum Speichern von Sprache aus einer Audiodatei), sind auf GitHub verfügbar.

> [!div class="nextstepaction"]
> [C++-Beispiele auf GitHub](https://aka.ms/csspeech/samples)

## <a name="see-also"></a>Weitere Informationen

- [Create a Custom Voice](how-to-custom-voice-create-voice.md) (Erstellen einer benutzerdefinierten Stimme)
- [Aufzeichnen benutzerdefinierter Stimmbeispiele](record-custom-voice-samples.md)
