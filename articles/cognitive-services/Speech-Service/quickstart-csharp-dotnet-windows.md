---
title: 'Schnellstart: Erkennen von Sprache, .NET Framework (Windows): Speech-Dienst'
titleSuffix: Azure Cognitive Services
description: In dieser Anleitung erfahren Sie, wie Sie unter Verwendung von .NET Framework für Windows und dem Speech SDK eine Konsolenanwendung zur Spracherkennung erstellen. Danach können Sie das Mikrofon Ihres Computers verwenden, um Sprache in Echtzeit zu transkribieren.
services: cognitive-services
author: wolfma61
manager: nitinme
ms.service: cognitive-services
ms.subservice: speech-service
ms.topic: quickstart
ms.date: 08/28/2019
ms.author: wolfma
ms.openlocfilehash: cb140647394858fbc0a9a00ea125365d5b7a08d5
ms.sourcegitcommit: e9936171586b8d04b67457789ae7d530ec8deebe
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 09/27/2019
ms.locfileid: "71327043"
---
# <a name="quickstart-recognize-speech-with-the-speech-sdk-for-net-framework-windows"></a>Schnellstart: Erkennen von Sprache mit dem Speech SDK für .NET Framework (Windows)

Schnellstarts sind auch für [Sprachsynthese](quickstart-text-to-speech-dotnet-windows.md) und [Sprachübersetzung](quickstart-translate-speech-dotnetframework-windows.md) verfügbar.

Wählen Sie bei Bedarf eine andere Programmiersprache und/oder Umgebung aus:<br/>
[!INCLUDE [Selector](../../../includes/cognitive-services-speech-service-quickstart-selector.md)]

In dieser Anleitung erfahren Sie, wie Sie unter Verwendung von .NET Framework für Windows und dem Speech SDK eine Konsolenanwendung zur Spracherkennung erstellen. Danach können Sie das Mikrofon Ihres Computers verwenden, um Sprache in Echtzeit zu transkribieren.

Eine kurze Demonstration (ohne eigene Erstellung des Visual Studio-Projekts, wie in diesem Artikel beschrieben) finden Sie auf GitHub in den aktuellen [Beispielen für das Cognitive Services Speech SDK](https://github.com/Azure-Samples/cognitive-services-speech-sdk).

## <a name="prerequisites"></a>Voraussetzungen

Für dieses Projekt benötigen Sie Folgendes:

* [Visual Studio 2019](https://visualstudio.microsoft.com/downloads/).
* Ein Abonnementschlüssel für den Speech-Dienst. [Hier erhalten Sie einen kostenlosen Schlüssel.](get-started.md)
* Zugriff auf das Mikrofon Ihres Computers

## <a name="create-a-visual-studio-project"></a>Erstellen eines Visual Studio-Projekts

[!INCLUDE [Create project](../../../includes/cognitive-services-speech-service-create-speech-project-vs-csharp.md)]

## <a name="add-sample-code"></a>Hinzufügen von Beispielcode

1. Öffnen Sie **Program.cs**, und ersetzen Sie den automatisch generierten Code durch dieses Beispiel:

   [!code-csharp[Quickstart Code](~/samples-cognitive-services-speech-sdk/quickstart/csharp-dotnet-windows/helloworld/Program.cs#code)]

1. Suchen Sie nach der Zeichenfolge `YourSubscriptionKey`, und ersetzen Sie sie durch Ihren Abonnementschlüssel für die Speech-Dienste.

1. Suchen Sie nach der Zeichenfolge `YourServiceRegion`, und ersetzen Sie sie durch die [Region](regions.md), die mit Ihrem Abonnement verknüpft ist. Bei Verwendung des kostenlosen Testabonnements ist die Region beispielsweise `westus`.

1. Wählen Sie auf der Menüleiste **Datei** > **Alle speichern** aus.

## <a name="build-and-run-the-app"></a>Erstellen und Ausführen der App

1. Wählen Sie auf der Menüleiste **Erstellen** > **Projektmappe erstellen** aus, um die Anwendung zu erstellen. Der Code sollte nun ohne Fehler kompiliert werden.

1. Wählen Sie **Debuggen** > **Debuggen starten** aus (oder drücken Sie**F5**), um die Anwendung **helloworld** zu starten.

1. Sprechen Sie einen englischen Ausdruck oder Satz in das Mikrofon Ihres Geräts. Die Anwendung überträgt ihn an die Speech-Dienste, die den transkribierten Text zur Anzeige zurück an die Anwendung senden.

   ![Spracherkennungsbenutzeroberfläche](media/sdk/qs-csharp-dotnet-windows-10-console-output.png)

## <a name="next-steps"></a>Nächste Schritte

> [!div class="nextstepaction"]
> [C#-Beispiele auf GitHub](https://aka.ms/csspeech/samples)

## <a name="see-also"></a>Weitere Informationen

- [Trainieren eines Modells für Custom Speech](how-to-custom-speech-train-model.md)
