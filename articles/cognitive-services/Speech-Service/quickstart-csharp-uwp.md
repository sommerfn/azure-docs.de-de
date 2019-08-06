---
title: 'Schnellstart: Erkennen von Sprache, C# (UWP) – Speech-Dienst'
titleSuffix: Azure Cognitive Services
description: In diesem Artikel erstellen Sie eine C#-UWP-Anwendung (Universelle Windows-Plattform) mithilfe des Cognitive Services Speech SDK. Sie transkribieren über das Mikrofon des Geräts Sprache in Echtzeit in Text. Die Anwendung basiert auf dem NuGet-Paket für das Speech SDK und auf Microsoft Visual Studio 2017.
services: cognitive-services
author: lisaweixu
manager: nitinme
ms.service: cognitive-services
ms.subservice: speech-service
ms.topic: quickstart
ms.date: 07/23/2019
ms.author: lisaweixu
ms.custom: seodec18
ms.openlocfilehash: b4c3c0979f3fea491ad8b04285973517423eaeeb
ms.sourcegitcommit: 6cff17b02b65388ac90ef3757bf04c6d8ed3db03
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 07/29/2019
ms.locfileid: "68607810"
---
# <a name="quickstart-recognize-speech-in-a-uwp-app-by-using-the-speech-sdk"></a>Schnellstart: Erkennen von Sprache in einer UWP-App mit dem Speech SDK

Schnellstarts sind auch für [Sprachsynthese](quickstart-text-to-speech-csharp-uwp.md), [Sprachübersetzung](quickstart-translate-speech-uwp.md) und den [virtuellen Voice-First-Assistenten](quickstart-virtual-assistant-csharp-uwp.md) verfügbar.

Wählen Sie bei Bedarf eine andere Programmiersprache und/oder Umgebung:<br/>
[!INCLUDE [Selector](../../../includes/cognitive-services-speech-service-quickstart-selector.md)]

In diesem Artikel entwickeln Sie eine C#-UWP-Anwendung (Universelle Windows-Plattform; Windows-Version 1709 oder höher) mithilfe des Cognitive Services [Speech SDK](speech-sdk.md). Das Programm transkribiert über das Mikrofon des Geräts Sprache in Echtzeit in Text. Die Anwendung basiert auf dem [NuGet-Paket für das Speech SDK](https://aka.ms/csspeech/nuget) und auf Microsoft Visual Studio 2017 oder höher (beliebige Edition).

> [!NOTE]
> Mithilfe der universellen Windows-Plattform können Sie Apps entwickeln, die auf einem beliebigen Gerät mit Unterstützung für Windows 10 (z.B. PCs, Xbox, Surface Hub und andere Geräte) ausgeführt werden.

## <a name="prerequisites"></a>Voraussetzungen

Für diese Schnellstartanleitung ist Folgendes erforderlich:

* Mindestens [Visual Studio 2017](https://visualstudio.microsoft.com/downloads/)
* Ein Azure-Abonnementschlüssel für den Speech-Dienst. [Hier erhalten Sie einen kostenlosen Schlüssel.](get-started.md)

## <a name="create-a-visual-studio-project"></a>Erstellen eines Visual Studio-Projekts

[!INCLUDE [](../../../includes/cognitive-services-speech-service-quickstart-uwp-create-proj.md)]

## <a name="add-sample-code"></a>Hinzufügen von Beispielcode

1. Die Benutzeroberfläche der Anwendung wird mithilfe von XAML definiert. Öffnen Sie `MainPage.xaml` im Projektmappen-Explorer. Fügen Sie in der XAML-Ansicht des Designers den folgenden XAML-Ausschnitt in das Grid-Tag (zwischen `<Grid>` und `</Grid>`) ein.

   [!code-xml[UI elements](~/samples-cognitive-services-speech-sdk/quickstart/csharp-uwp/helloworld/MainPage.xaml#StackPanel)]

1. Öffnen Sie die CodeBehind-Quelldatei `MainPage.xaml.cs` (sie ist unter `MainPage.xaml` gruppiert). Ersetzen Sie den gesamten darin enthaltenen Code wie folgt.

   [!code-csharp[Quickstart Code](~/samples-cognitive-services-speech-sdk/quickstart/csharp-uwp/helloworld/MainPage.xaml.cs#code)]

1. Ersetzen Sie im `SpeechRecognitionFromMicrophone_ButtonClicked`-Handler in dieser Datei die Zeichenfolge `YourSubscriptionKey` durch Ihren Abonnementschlüssel.

1. Ersetzen Sie im Handler `SpeechRecognitionFromMicrophone_ButtonClicked` die Zeichenfolge `YourServiceRegion` durch die [Region](regions.md), die mit Ihrem Abonnement verknüpft ist (z.B. `westus` für das kostenlose Testabonnement).

1. Speichern Sie alle Änderungen am Projekt.

## <a name="build-and-run-the-app"></a>Erstellen und Ausführen der App

1. Erstellen Sie die Anwendung. Wählen Sie in der Menüleiste **Build** > **Projektmappe erstellen** aus. Der Code sollte nun ohne Fehler kompiliert werden.

    ![Screenshot der Visual Studio-Anwendung mit hervorgehobener Option „Projektmappe erstellen](media/sdk/qs-csharp-uwp-08-build.png "Erfolgreicher Build“")

1. Starten Sie die Anwendung. Wählen Sie in der Menüleiste **Debuggen** > **Debuggen starten** aus, oder drücken Sie **F5**.

    ![Screenshot der Visual Studio-Anwendung mit hervorgehobener Option „Debuggen starten](media/sdk/qs-csharp-uwp-09-start-debugging.png "Debuggen der App starten“")

1. Ein Fenster wird angezeigt. Wählen Sie **Mikrofon aktivieren** aus, und bestätigen Sie die Berechtigungsanforderung, die angezeigt wird.

    ![Screenshot der Berechtigungsanforderung](media/sdk/qs-csharp-uwp-10-access-prompt.png "Debuggen der App starten")

1. Wählen Sie **Spracherkennung durch Mikrofoneingabe** aus, und sprechen Sie einen englischen Ausdruck oder Satz in das Mikrofon Ihres Geräts. Ihre Spracheingabe wird an die Speech-Dienste übermittelt und in Text transkribiert, der im Fenster angezeigt wird.

    ![Screenshot der Spracherkennungsbenutzeroberfläche](media/sdk/qs-csharp-uwp-11-ui-result.png)

## <a name="next-steps"></a>Nächste Schritte

> [!div class="nextstepaction"]
> [C#-Beispiele auf GitHub](https://aka.ms/csspeech/samples)

## <a name="see-also"></a>Weitere Informationen

- [Übersetzen von Sprache](how-to-translate-speech-csharp.md)
- [Tutorial: Erstellen eines benutzerdefinierten Akustikmodells](how-to-customize-acoustic-models.md)
- [Tutorial: Erstellen eines benutzerdefinierten Sprachmodells](how-to-customize-language-model.md)
