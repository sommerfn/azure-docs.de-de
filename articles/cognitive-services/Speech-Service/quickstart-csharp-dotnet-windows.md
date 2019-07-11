---
title: 'Schnellstart: Erkennen von Sprache, .NET Framework (Windows): Speech-Dienste'
titleSuffix: Azure Cognitive Services
description: In dieser Anleitung erfahren Sie, wie Sie unter Verwendung von .NET Framework für Windows und dem Speech SDK eine Konsolenanwendung zur Spracherkennung erstellen. Danach können Sie das Mikrofon Ihres Computers verwenden, um Sprache in Echtzeit zu transkribieren.
services: cognitive-services
author: wolfma61
manager: nitinme
ms.service: cognitive-services
ms.subservice: speech-service
ms.topic: quickstart
ms.date: 07/05/2019
ms.author: wolfma
ms.openlocfilehash: d8738357a3bad6626ef6d79248aef1c4d2cb1ead
ms.sourcegitcommit: f10ae7078e477531af5b61a7fe64ab0e389830e8
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 07/05/2019
ms.locfileid: "67603091"
---
# <a name="quickstart-recognize-speech-with-the-speech-sdk-for-net-framework-windows"></a>Schnellstart: Erkennen von Sprache mit dem Speech SDK für .NET Framework (Windows)

Schnellstarts sind auch für [Sprachsynthese](quickstart-text-to-speech-dotnet-windows.md) und [Sprachübersetzung](quickstart-translate-speech-dotnetframework-windows.md) verfügbar.

Wählen Sie bei Bedarf eine andere Programmiersprache und/oder Umgebung:<br/>
[!INCLUDE [Selector](../../../includes/cognitive-services-speech-service-quickstart-selector.md)]

In dieser Anleitung erfahren Sie, wie Sie unter Verwendung von .NET Framework für Windows und dem Speech SDK eine Konsolenanwendung zur Spracherkennung erstellen. Danach können Sie das Mikrofon Ihres Computers verwenden, um Sprache in Echtzeit zu transkribieren.

Eine schnelle Demonstration (ohne eigene Erstellung des Visual Studio-Projekts, wie weiter unten gezeigt) erhalten Sie wie folgt:

Laden Sie die neuesten [Beispiele für das Cognitive Services Speech SDK](https://github.com/Azure-Samples/cognitive-services-speech-sdk) von GitHub herunter.

## <a name="prerequisites"></a>Voraussetzungen

Für dieses Projekt benötigen Sie Folgendes:

* [Visual Studio 2017](https://visualstudio.microsoft.com/downloads/)
* Ein Abonnementschlüssel für den Speech-Dienst. [Hier erhalten Sie einen kostenlosen Schlüssel.](get-started.md)
* Zugriff auf das Mikrofon Ihres Computers

## <a name="create-a-visual-studio-project"></a>Erstellen eines Visual Studio-Projekts

[!INCLUDE [Create project](../../../includes/cognitive-services-speech-service-create-speech-project-vs-csharp.md)]

## <a name="add-sample-code"></a>Hinzufügen von Beispielcode

1. Öffnen Sie `Program.cs`, und ersetzen Sie den automatisch generierten Code durch dieses Beispiel:

    [!code-csharp[Quickstart Code](~/samples-cognitive-services-speech-sdk/quickstart/csharp-dotnet-windows/helloworld/Program.cs#code)]

1. Suchen Sie nach der Zeichenfolge `YourSubscriptionKey`, und ersetzen Sie sie durch Ihren Abonnementschlüssel für die Speech-Dienste.

1. Suchen Sie nach der Zeichenfolge `YourServiceRegion`, und ersetzen Sie sie durch die [Region](regions.md), die mit Ihrem Abonnement verknüpft ist. Bei Verwendung der kostenlosen Testversion ist die Region beispielsweise `westus`.

1. Speichern Sie die Änderungen am Projekt.

## <a name="build-and-run-the-app"></a>Erstellen und Ausführen der App

1. Wählen Sie in der Menüleiste **Build** > **Projektmappe erstellen** aus. Der Code sollte nun ohne Fehler kompiliert werden.

    ![Screenshot der Visual Studio-Anwendung mit hervorgehobener Option „Projektmappe erstellen](media/sdk/qs-csharp-dotnet-windows-08-build.png "Erfolgreicher Build“")

1. Wählen Sie auf der Menüleiste **Debuggen** > **Debuggen starten** aus, oder drücken Sie**F5**, um die Anwendung zu starten.

    ![Screenshot der Visual Studio-Anwendung mit hervorgehobener Option „Debuggen starten](media/sdk/qs-csharp-dotnet-windows-09-start-debugging.png "Debuggen der App starten“")

1. Ein Konsolenfenster mit einer Sprechaufforderung wird angezeigt. Sagen Sie etwas auf Englisch. Ihre Spracheingabe wird an die Speech-Dienste übermittelt und in Echtzeit in Text umgewandelt. Das Ergebnis wird in der Konsole ausgegeben.

    ![Screenshot der Konsolenausgabe nach erfolgreicher Erkennung](media/sdk/qs-csharp-dotnet-windows-10-console-output.png "Konsolenausgabe nach erfolgreicher Erkennung")

## <a name="next-steps"></a>Nächste Schritte

> [!div class="nextstepaction"]
> [C#-Beispiele auf GitHub](https://aka.ms/csspeech/samples)

## <a name="see-also"></a>Weitere Informationen

- [Tutorial: Erstellen eines benutzerdefinierten Akustikmodells](how-to-customize-acoustic-models.md)
- [Tutorial: Erstellen eines benutzerdefinierten Sprachmodells](how-to-customize-language-model.md)
