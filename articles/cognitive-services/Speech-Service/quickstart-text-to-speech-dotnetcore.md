---
title: 'Schnellstart: Erkennen von Sprache, C# (.NET Core): Spracherkennungsdienste'
titleSuffix: Azure Cognitive Services
description: Hier erfahren Sie, wie die Sprachsynthese in C# mit .NET Core unter Windows mit dem Speech SDK funktioniert.
services: cognitive-services
author: yinhew
manager: nitinme
ms.service: cognitive-services
ms.subservice: speech-service
ms.topic: quickstart
ms.date: 6/24/2019
ms.author: yinhew
ms.openlocfilehash: 7b4a018e38ca625e38dc1658a95d3ce0e677f711
ms.sourcegitcommit: c63e5031aed4992d5adf45639addcef07c166224
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 06/28/2019
ms.locfileid: "67467193"
---
# <a name="quickstart-synthesize-speech-with-the-speech-sdk-for-net-core"></a>Schnellstart: Synthetisieren von Sprache mit dem Speech SDK für .NET Core

Schnellstarts sind auch für [Spracherkennung](quickstart-csharp-dotnetcore-windows.md) und [Sprachübersetzung](quickstart-translate-speech-dotnetcore-windows.md) verfügbar.

In diesem Artikel erstellen Sie eine C#-Konsolenanwendung für .NET Core unter Windows mithilfe des Cognitive Services [Speech SDK](speech-sdk.md). Sie synthetisieren Sprache aus Text in Echtzeit zum Lautsprecher Ihres PC. Die Anwendung basiert auf dem [NuGet-Paket für das Speech SDK](https://aka.ms/csspeech/nuget) und auf Microsoft Visual Studio 2017 (beliebige Edition).

> [!NOTE]
> .NET Core ist eine Open-Source- und plattformübergreifende .NET-Plattform, die die [.NET Standard](https://docs.microsoft.com/dotnet/standard/net-standard)-Spezifikation implementiert.

Für diese Schnellstartanleitung benötigen Sie einen Abonnementschlüssel für die Speech-Dienste. Sie können einen solchen Schlüssel kostenlos abrufen. Ausführliche Informationen finden Sie unter [Kostenloses Testen von Speech Services](get-started.md).

## <a name="prerequisites"></a>Voraussetzungen

Für diese Schnellstartanleitung ist Folgendes erforderlich:

* [.NET Core SDK](https://dotnet.microsoft.com/download)
* [Visual Studio 2017](https://visualstudio.microsoft.com/downloads/)
* Ein Azure-Abonnementschlüssel für den Speech-Dienst. [Hier erhalten Sie einen kostenlosen Schlüssel.](get-started.md)

## <a name="create-a-visual-studio-project"></a>Erstellen eines Visual Studio-Projekts

[!INCLUDE [](../../../includes/cognitive-services-speech-service-quickstart-dotnetcore-create-proj.md)]

## <a name="add-sample-code"></a>Hinzufügen von Beispielcode

1. Öffnen Sie `Program.cs`, und ersetzen Sie den gesamten in der Datei enthaltenen Code wie folgt.

    [!code-csharp[Quickstart Code](~/samples-cognitive-services-speech-sdk/quickstart/text-to-speech/csharp-dotnetcore/helloworld/Program.cs#code)]

1. Ersetzen Sie in der gleichen Datei die Zeichenfolge `YourSubscriptionKey` durch Ihren Abonnementschlüssel.

1. Ersetzen Sie außerdem die Zeichenfolge `YourServiceRegion` durch die [Region](regions.md), die mit Ihrem Abonnement verknüpft ist (z.B. `westus` für das kostenlose Testabonnement).

1. Speichern Sie die Änderungen am Projekt.

## <a name="build-and-run-the-app"></a>Erstellen und Ausführen der App

1. Erstellen Sie die Anwendung. Wählen Sie in der Menüleiste **Erstellen** > **Projektmappe erstellen** aus. Der Code sollte ohne Fehler kompiliert werden.

    ![Screenshot der Visual Studio-Anwendung mit hervorgehobener Option „Projektmappe erstellen](media/sdk/qs-csharp-dotnetcore-windows-05-build.png "Erfolgreicher Build“")

1. Starten Sie die Anwendung. Wählen Sie in der Menüleiste **Debuggen** > **Debuggen starten** aus, oder drücken Sie **F5**.

    ![Screenshot der Visual Studio-Anwendung mit hervorgehobener Option „Debuggen starten](media/sdk/qs-csharp-dotnetcore-windows-06-start-debugging.png "Debuggen der App starten“")

1. Ein Konsolenfenster mit einer Eingabeaufforderung wird angezeigt. Geben Sie einige Wörter oder einen Satz ein. Der eingegebene Text wird an die Speech-Dienste übermittelt und in Sprache synthetisiert, die dann über Ihren Lautsprecher ausgegeben wird.

    ![Screenshot der Konsolenausgabe nach erfolgreicher Sprachsynthese](media/sdk/qs-tts-csharp-dotnet-windows-console-output.png "Konsolenausgabe nach erfolgreicher Sprachsynthese")

## <a name="next-steps"></a>Nächste Schritte

Weitere Beispiele, etwa zum Synthetisieren von Sprache in eine Audiodatei, sind auf GitHub verfügbar.

> [!div class="nextstepaction"]
> [C#-Beispiele auf GitHub](https://aka.ms/csspeech/samples)

## <a name="see-also"></a>Weitere Informationen

- [Anpassen von Voicefonts](how-to-customize-voice-font.md)
- [Aufnehmen von Sprachbeispielen](record-custom-voice-samples.md)
