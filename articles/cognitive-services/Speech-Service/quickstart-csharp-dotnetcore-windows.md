---
title: 'Schnellstart: Erkennen von Sprache, C# (.NET Core): Speech-Dienste'
titleSuffix: Azure Cognitive Services
description: Hier erfahren Sie, wie die Spracherkennung in C# mit .NET Core unter Windows oder macOS mit dem Speech SDK funktioniert.
services: cognitive-services
author: wolfma61
manager: nitinme
ms.service: cognitive-services
ms.subservice: speech-service
ms.topic: quickstart
ms.date: 07/05/2019
ms.author: wolfma
ms.openlocfilehash: 9c445ea685518cec77089ce644a79305ff7f3e9e
ms.sourcegitcommit: f10ae7078e477531af5b61a7fe64ab0e389830e8
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 07/05/2019
ms.locfileid: "67606583"
---
# <a name="quickstart-recognize-speech-with-the-speech-sdk-for-net-core"></a>Schnellstart: Erkennen von Sprache mit dem Speech-SDK für .NET Core

Schnellstarts sind auch für [Sprachsynthese](quickstart-text-to-speech-dotnetcore.md) und [Sprachübersetzung](quickstart-translate-speech-dotnetcore-windows.md) verfügbar.

Wählen Sie bei Bedarf eine andere Programmiersprache und/oder Umgebung:<br/>
[!INCLUDE [Selector](../../../includes/cognitive-services-speech-service-quickstart-selector.md)]

In diesem Artikel erstellen Sie eine C#-Konsolenanwendung für .NET Core unter Windows oder macOS mithilfe des Cognitive Services [Speech SDK](speech-sdk.md). Sie transkribieren über das Mikrofon Ihres Computers Sprache in Echtzeit in Text. Die Anwendung basiert auf dem [NuGet-Paket für das Speech SDK](https://aka.ms/csspeech/nuget) und auf Microsoft Visual Studio 2017 (beliebige Edition).

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

    [!code-csharp[Quickstart Code](~/samples-cognitive-services-speech-sdk/quickstart/csharp-dotnetcore/helloworld/Program.cs#code)]

1. Ersetzen Sie in der gleichen Datei die Zeichenfolge `YourSubscriptionKey` durch Ihren Abonnementschlüssel.

1. Ersetzen Sie außerdem die Zeichenfolge `YourServiceRegion` durch die [Region](regions.md), die mit Ihrem Abonnement verknüpft ist (z.B. `westus` für das kostenlose Testabonnement).

1. Speichern Sie die Änderungen am Projekt.

## <a name="build-and-run-the-app"></a>Erstellen und Ausführen der App

1. Erstellen Sie die Anwendung. Wählen Sie in der Menüleiste **Erstellen** > **Projektmappe erstellen** aus. Der Code sollte ohne Fehler kompiliert werden.

    ![Screenshot der Visual Studio-Anwendung mit hervorgehobener Option „Projektmappe erstellen](media/sdk/qs-csharp-dotnetcore-windows-05-build.png "Erfolgreicher Build“")

1. Starten Sie die Anwendung. Wählen Sie in der Menüleiste **Debuggen** > **Debuggen starten** aus, oder drücken Sie **F5**.

    ![Screenshot der Visual Studio-Anwendung mit hervorgehobener Option „Debuggen starten](media/sdk/qs-csharp-dotnetcore-windows-06-start-debugging.png "Debuggen der App starten“")

1. Ein Konsolenfenster wird angezeigt, in dem Sie aufgefordert werden, etwas zu sagen. Sprechen Sie einen englischen Ausdruck oder Satz. Ihre Spracheingabe wird an die Speech-Dienste übermittel, in Text transkribiert und im gleichen Fenster angezeigt.

    ![Screenshot der Konsolenausgabe nach erfolgreicher Erkennung](media/sdk/qs-csharp-dotnetcore-windows-07-console-output.png "Konsolenausgabe nach erfolgreicher Erkennung")

## <a name="next-steps"></a>Nächste Schritte

Weitere Beispiele, etwa das Lesen von Sprache aus einer Audiodatei, sind auf GitHub verfügbar.

> [!div class="nextstepaction"]
> [C#-Beispiele auf GitHub](https://aka.ms/csspeech/samples)

## <a name="see-also"></a>Weitere Informationen

- [Tutorial: Erstellen eines benutzerdefinierten Akustikmodells](how-to-customize-acoustic-models.md)
- [Tutorial: Erstellen eines benutzerdefinierten Sprachmodells](how-to-customize-language-model.md)
