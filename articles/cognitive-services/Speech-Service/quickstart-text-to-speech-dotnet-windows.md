---
title: 'Schnellstart: Synthetisieren von Sprache, .NET Framework (Windows): Speech-Dienst'
titleSuffix: Azure Cognitive Services
description: In dieser Anleitung erfahren Sie, wie Sie mit .NET Framework für Windows und dem Speech SDK eine Konsolenanwendung für die Sprachsynthese erstellen. Anschließend können Sie Sprache aus Text synthetisieren und in Echtzeit über Ihren Lautsprecher ausgeben.
services: cognitive-services
author: yinhew
manager: nitinme
ms.service: cognitive-services
ms.subservice: speech-service
ms.topic: quickstart
ms.date: 07/05/2019
ms.author: yinhew
ms.openlocfilehash: 65db847c5df1a05671e3b1c0ac4541f00506c8fd
ms.sourcegitcommit: 670c38d85ef97bf236b45850fd4750e3b98c8899
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 08/08/2019
ms.locfileid: "68854699"
---
# <a name="quickstart-synthesize-speech-with-the-speech-sdk-for-net-framework-windows"></a>Schnellstart: Synthetisieren von Sprache mit dem Speech SDK für .NET Framework (Windows)

Schnellstarts sind auch für [Spracherkennung](quickstart-csharp-dotnet-windows.md) und [Sprachübersetzung](quickstart-translate-speech-dotnetframework-windows.md) verfügbar.

In dieser Anleitung erfahren Sie, wie Sie mit .NET Framework für Windows und dem Speech SDK eine Konsolenanwendung für die Sprachsynthese erstellen. Anschließend können Sie Sprache aus Text synthetisieren und in Echtzeit über Ihren Lautsprecher ausgeben.

Eine schnelle Demonstration (ohne eigene Erstellung des Visual Studio-Projekts, wie weiter unten gezeigt) erhalten Sie wie folgt:

Laden Sie die neuesten [Beispiele für das Cognitive Services Speech SDK](https://github.com/Azure-Samples/cognitive-services-speech-sdk) von GitHub herunter.

## <a name="prerequisites"></a>Voraussetzungen

Für dieses Projekt benötigen Sie Folgendes:

* [Visual Studio 2019](https://visualstudio.microsoft.com/downloads/)
* Ein Abonnementschlüssel für den Speech-Dienst. [Hier erhalten Sie einen kostenlosen Schlüssel.](get-started.md)
* Einen Lautsprecher oder ein Headset.

## <a name="create-a-visual-studio-project"></a>Erstellen eines Visual Studio-Projekts

[!INCLUDE [Create project](../../../includes/cognitive-services-speech-service-create-speech-project-vs-csharp.md)]

## <a name="add-sample-code"></a>Hinzufügen von Beispielcode

1. Öffnen Sie `Program.cs`, und ersetzen Sie den automatisch generierten Code durch dieses Beispiel:

    [!code-csharp[Quickstart Code](~/samples-cognitive-services-speech-sdk/quickstart/text-to-speech/csharp-dotnet-windows/helloworld/Program.cs#code)]

1. Suchen Sie nach der Zeichenfolge `YourSubscriptionKey`, und ersetzen Sie sie durch Ihren Abonnementschlüssel für die Speech-Dienste.

1. Suchen Sie nach der Zeichenfolge `YourServiceRegion`, und ersetzen Sie sie durch die [Region](regions.md), die mit Ihrem Abonnement verknüpft ist. Bei Verwendung der kostenlosen Testversion ist die Region beispielsweise `westus`.

1. Speichern Sie die Änderungen am Projekt.

## <a name="build-and-run-the-app"></a>Erstellen und Ausführen der App

1. Wählen Sie in der Menüleiste **Build** > **Projektmappe erstellen** aus. Der Code sollte nun ohne Fehler kompiliert werden.

    ![Screenshot der Visual Studio-Anwendung mit hervorgehobener Option „Projektmappe erstellen](media/sdk/qs-csharp-dotnet-windows-08-build.png "Erfolgreicher Build“")

1. Wählen Sie auf der Menüleiste **Debuggen** > **Debuggen starten** aus, oder drücken Sie**F5**, um die Anwendung zu starten.

    ![Screenshot der Visual Studio-Anwendung mit hervorgehobener Option „Debuggen starten](media/sdk/qs-csharp-dotnet-windows-09-start-debugging.png "Debuggen der App starten“")

1. Ein Konsolenfenster mit einer Eingabeaufforderung wird angezeigt. Geben Sie einige Wörter oder einen Satz ein. Der eingegebene Text wird an die Speech-Dienste übermittelt und in Sprache synthetisiert, die dann über Ihren Lautsprecher ausgegeben wird.

    ![Screenshot der Konsolenausgabe nach erfolgreicher Erkennung](media/sdk/qs-tts-csharp-dotnet-windows-console-output.png "Konsolenausgabe nach erfolgreicher Erkennung")

## <a name="next-steps"></a>Nächste Schritte

> [!div class="nextstepaction"]
> [C#-Beispiele auf GitHub](https://aka.ms/csspeech/samples)

## <a name="see-also"></a>Weitere Informationen

- [Anpassen von Voicefonts](how-to-customize-voice-font.md)
- [Aufnehmen von Sprachbeispielen](record-custom-voice-samples.md)
