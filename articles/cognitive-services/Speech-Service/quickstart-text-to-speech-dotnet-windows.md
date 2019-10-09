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
ms.date: 08/27/2019
ms.author: yinhew
ms.openlocfilehash: 1a411455e4a6dea22e092cdfc8e70ee23b656435
ms.sourcegitcommit: e9936171586b8d04b67457789ae7d530ec8deebe
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 09/27/2019
ms.locfileid: "71327449"
---
# <a name="quickstart-synthesize-speech-with-the-speech-sdk-for-net-framework-windows"></a>Schnellstart: Synthetisieren von Sprache mit dem Speech SDK für .NET Framework (Windows)

Schnellstarts sind auch für [Spracherkennung](quickstart-csharp-dotnet-windows.md) und [Sprachübersetzung](quickstart-translate-speech-dotnetframework-windows.md) verfügbar.

In dieser Anleitung erfahren Sie, wie Sie mit .NET Framework für Windows und dem Speech SDK eine Konsolenanwendung für die Sprachsynthese erstellen. Anschließend können Sie Sprache aus Text synthetisieren und in Echtzeit über Ihren Lautsprecher ausgeben.

Eine kurze Demonstration (ohne eigene Erstellung des Visual Studio-Projekts, wie in diesem Artikel beschrieben) finden Sie auf GitHub in den aktuellen [Beispielen für das Cognitive Services Speech SDK](https://github.com/Azure-Samples/cognitive-services-speech-sdk).

## <a name="prerequisites"></a>Voraussetzungen

Für dieses Projekt benötigen Sie Folgendes:

* [Visual Studio 2019](https://visualstudio.microsoft.com/downloads/).
* Ein Abonnementschlüssel für den Speech-Dienst. [Hier erhalten Sie einen kostenlosen Schlüssel.](get-started.md)
* Einen Lautsprecher oder ein Headset.

## <a name="create-a-visual-studio-project"></a>Erstellen eines Visual Studio-Projekts

[!INCLUDE [Create project](../../../includes/cognitive-services-speech-service-create-speech-project-vs-csharp.md)]

## <a name="add-sample-code"></a>Hinzufügen von Beispielcode

1. Öffnen Sie **Program.cs**, und ersetzen Sie den automatisch generierten Code durch dieses Beispiel:

   [!code-csharp[Quickstart Code](~/samples-cognitive-services-speech-sdk/quickstart/text-to-speech/csharp-dotnet-windows/helloworld/Program.cs#code)]

1. Suchen Sie nach der Zeichenfolge `YourSubscriptionKey`, und ersetzen Sie sie durch Ihren Abonnementschlüssel für die Speech-Dienste.

1. Suchen Sie nach der Zeichenfolge `YourServiceRegion`, und ersetzen Sie sie durch die [Region](regions.md), die mit Ihrem Abonnement verknüpft ist. Bei Verwendung des kostenlosen Testabonnements ist die Region beispielsweise `westus`.

1. Wählen Sie auf der Menüleiste **Datei** > **Alle speichern** aus.

## <a name="build-and-run-the-application"></a>Erstellen und Ausführen der Anwendung

1. Wählen Sie auf der Menüleiste **Erstellen** > **Projektmappe erstellen** aus, um die Anwendung zu erstellen. Der Code sollte nun ohne Fehler kompiliert werden.

1. Wählen Sie **Debuggen** > **Debuggen starten** aus (oder drücken Sie**F5**), um die Anwendung **helloworld** zu starten.

1. Geben Sie einen englischen Ausdruck oder Satz ein. Die Anwendung überträgt Ihren Text an die Speech-Dienste, die die synthetisierte Sprache zur Wiedergabe über den Lautspreche an die Anwendung sendet.

   ![Benutzeroberfläche für die Sprachsynthese](media/sdk/qs-tts-csharp-dotnet-windows-console-output.png)

## <a name="next-steps"></a>Nächste Schritte

> [!div class="nextstepaction"]
> [C#-Beispiele auf GitHub](https://aka.ms/csspeech/samples)

## <a name="see-also"></a>Weitere Informationen

- [Create a Custom Voice](how-to-custom-voice-create-voice.md) (Erstellen einer benutzerdefinierten Stimme)
- [Aufzeichnen benutzerdefinierter Stimmbeispiele](record-custom-voice-samples.md)
