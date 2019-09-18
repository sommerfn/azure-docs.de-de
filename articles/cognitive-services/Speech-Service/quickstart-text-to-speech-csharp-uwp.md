---
title: 'Schnellstart: Synthetisieren von Sprache, C# (UWP) – Speech-Dienst'
titleSuffix: Azure Cognitive Services
description: In diesem Artikel erstellen Sie eine C#-UWP-Anwendung (Universelle Windows-Plattform) mithilfe des Cognitive Services Speech SDK. Sie synthetisieren Sprache aus Text in Echtzeit zum Lautsprecher Ihres Geräts. Die Anwendung basiert auf dem NuGet-Paket für das Speech SDK und auf Microsoft Visual Studio 2019.
services: cognitive-services
author: yinhew
manager: nitinme
ms.service: cognitive-services
ms.subservice: speech-service
ms.topic: quickstart
ms.date: 08/19/2019
ms.author: yinhew
ms.openlocfilehash: 65b65c9af377b6a9951f9f328e0732850d3b9c1d
ms.sourcegitcommit: 49c4b9c797c09c92632d7cedfec0ac1cf783631b
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 09/05/2019
ms.locfileid: "70382215"
---
# <a name="quickstart-synthesize-speech-in-a-uwp-app-by-using-the-speech-sdk"></a>Schnellstart: Synthetisieren von Sprache in einer UWP-App mit dem Speech SDK

Schnellstarts sind auch für [Spracherkennung](quickstart-csharp-uwp.md), [Sprachübersetzung](quickstart-translate-speech-uwp.md) und den [virtuellen Voice-First-Assistenten](quickstart-virtual-assistant-csharp-uwp.md) verfügbar.

In diesem Artikel entwickeln Sie eine C#-UWP-Anwendung (Universelle Windows-Plattform) mithilfe des Cognitive Services [Speech SDK](speech-sdk.md). Das Programm synthetisiert Sprache aus Text in Echtzeit zum Lautsprecher Ihres Geräts. Sie entwickeln die Anwendung basierend auf dem [NuGet-Paket für das Speech SDK](https://aka.ms/csspeech/nuget) und auf Microsoft Visual Studio 2019 (beliebige Edition).

> [!NOTE]
> Mithilfe der universellen Windows-Plattform können Sie Apps entwickeln, die auf einem beliebigen Gerät mit Unterstützung für Windows 10 (z.B. PCs, Xbox, Surface Hub und andere Geräte) ausgeführt werden.

## <a name="prerequisites"></a>Voraussetzungen

Für diese Schnellstartanleitung ist Folgendes erforderlich:

* [Visual Studio 2019](https://visualstudio.microsoft.com/downloads/).
* Ein Azure-Abonnementschlüssel für den Speech-Dienst. [Hier erhalten Sie einen kostenlosen Schlüssel.](get-started.md)

## <a name="create-a-visual-studio-project"></a>Erstellen eines Visual Studio-Projekts

[!INCLUDE [](../../../includes/cognitive-services-speech-service-quickstart-uwp-create-proj.md)]

## <a name="add-sample-code"></a>Hinzufügen von Beispielcode

Fügen Sie nun den XAML-Code, der die Benutzeroberfläche der Anwendung definiert, sowie die C#-CodeBehind-Implementierung hinzu.

1. Öffnen Sie `MainPage.xaml` im **Projektmappen-Explorer**.

1. Fügen Sie in der XAML-Ansicht des Designers den folgenden XAML-Ausschnitt in das **Grid**-Tag (zwischen `<Grid>` und `</Grid>`) ein.

   [!code-xml[UI elements](~/samples-cognitive-services-speech-sdk/quickstart/text-to-speech/csharp-uwp/helloworld/MainPage.xaml#StackPanel)]

1. Öffnen Sie im **Projektmappen-Explorer** die CodeBehind-Quelldatei `MainPage.xaml.cs`. (Sie befindet sich unter `MainPage.xaml`.)

1. Ersetzen Sie den gesamten darin enthaltenen Code durch den folgenden Codeausschnitt:

   [!code-csharp[Quickstart Code](~/samples-cognitive-services-speech-sdk/quickstart/text-to-speech/csharp-uwp/helloworld/MainPage.xaml.cs#code)]

1. Suchen Sie im Handler `Speak_ButtonClicked` der Quelldatei nach der Zeichenfolge `YourSubscriptionKey`, und ersetzen Sie sie durch Ihren Abonnementschlüssel.

1. Suchen Sie im Handler `Speak_ButtonClicked` nach der Zeichenfolge `YourServiceRegion`, und ersetzen Sie sie durch die [Region](regions.md), die mit Ihrem Abonnement verknüpft ist. (Verwenden Sie beispielsweise `westus` für das kostenlose Testabonnement.)

1. Wählen Sie auf der Menüleiste **Datei** > **Alle speichern** aus, um Ihre Änderungen zu speichern.

## <a name="build-and-run-the-application"></a>Erstellen und Ausführen der Anwendung

Nun können Sie Ihre Anwendung erstellen und testen.

1. Wählen Sie auf der Menüleiste **Erstellen** > **Projektmappe erstellen** aus, um die Anwendung zu erstellen. Der Code sollte nun ohne Fehler kompiliert werden.

1. Wählen Sie **Debuggen** > **Debuggen starten** aus (oder drücken Sie**F5**), um die Anwendung zu starten. Das Fenster **helloworld** wird angezeigt.

   ![UWP-Beispielanwendung für die Sprachsynthese in C#: Schnellstart](media/sdk/qs-text-to-speech-uwp-helloworld-window.png)

1. Geben Sie Text in das Textfeld ein, und klicken Sie auf **Sprechen**. Ihr Text wird an die Spracherkennungsdienste übermittelt und in Sprache synthetisiert, die dann über Ihren Lautsprecher ausgegeben wird.

    ![Benutzeroberfläche für die Sprachsynthese](media/sdk/qs-tts-csharp-uwp-ui-result.png)

## <a name="next-steps"></a>Nächste Schritte

> [!div class="nextstepaction"]
> [C#-Beispiele auf GitHub](https://aka.ms/csspeech/samples)

## <a name="see-also"></a>Weitere Informationen

- [Erstellen und Verwenden von Custom Voice-Modellen](how-to-custom-voice-create-voice.md)
- [Aufnehmen von Sprachbeispielen](record-custom-voice-samples.md)
