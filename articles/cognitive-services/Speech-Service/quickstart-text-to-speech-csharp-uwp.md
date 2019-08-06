---
title: 'Schnellstart: Synthetisieren von Sprache, C# (UWP) – Speech-Dienst'
titleSuffix: Azure Cognitive Services
description: In diesem Artikel erstellen Sie eine C#-UWP-Anwendung (Universelle Windows-Plattform) mithilfe des Cognitive Services Speech SDK. Sie synthetisieren Sprache aus Text in Echtzeit zum Lautsprecher Ihres Geräts. Die Anwendung basiert auf dem NuGet-Paket für das Speech SDK und auf Microsoft Visual Studio 2017.
services: cognitive-services
author: yinhew
manager: nitinme
ms.service: cognitive-services
ms.subservice: speech-service
ms.topic: quickstart
ms.date: 6/24/2019
ms.author: yinhew
ms.openlocfilehash: 5e1f8aea1b00cbba7fec6c7ca416a965458ab526
ms.sourcegitcommit: 6cff17b02b65388ac90ef3757bf04c6d8ed3db03
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 07/29/2019
ms.locfileid: "68607756"
---
# <a name="quickstart-synthesize-speech-in-a-uwp-app-by-using-the-speech-sdk"></a>Schnellstart: Synthetisieren von Sprache in einer UWP-App mit dem Speech SDK

Schnellstarts sind auch für [Spracherkennung](quickstart-csharp-uwp.md), [Sprachübersetzung](quickstart-translate-speech-uwp.md) und den [virtuellen Voice-First-Assistenten](quickstart-virtual-assistant-csharp-uwp.md) verfügbar.

In diesem Artikel entwickeln Sie eine C#-UWP-Anwendung (Universelle Windows-Plattform; Windows-Version 1709 oder höher) mithilfe des Cognitive Services [Speech SDK](speech-sdk.md). Das Programm synthetisiert Sprache aus Text in Echtzeit zum Lautsprecher Ihres Geräts. Die Anwendung basiert auf dem [NuGet-Paket für das Speech SDK](https://aka.ms/csspeech/nuget) und auf Microsoft Visual Studio 2017 oder höher (beliebige Edition).

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

   [!code-xml[UI elements](~/samples-cognitive-services-speech-sdk/quickstart/text-to-speech/csharp-uwp/helloworld/MainPage.xaml#StackPanel)]

1. Öffnen Sie die CodeBehind-Quelldatei `MainPage.xaml.cs` (sie ist unter `MainPage.xaml` gruppiert). Ersetzen Sie den gesamten darin enthaltenen Code wie folgt.

   [!code-csharp[Quickstart Code](~/samples-cognitive-services-speech-sdk/quickstart/text-to-speech/csharp-uwp/helloworld/MainPage.xaml.cs#code)]

1. Ersetzen Sie im `Speak_ButtonClicked`-Handler in dieser Datei die Zeichenfolge `YourSubscriptionKey` durch Ihren Abonnementschlüssel.

1. Ersetzen Sie im Handler `Speak_ButtonClicked` die Zeichenfolge `YourServiceRegion` durch die [Region](regions.md), die mit Ihrem Abonnement verknüpft ist (z.B. `westus` für das kostenlose Testabonnement).

1. Speichern Sie alle Änderungen am Projekt.

## <a name="build-and-run-the-app"></a>Erstellen und Ausführen der App

1. Erstellen Sie die Anwendung. Wählen Sie in der Menüleiste **Build** > **Projektmappe erstellen** aus. Der Code sollte nun ohne Fehler kompiliert werden.

    ![Screenshot der Visual Studio-Anwendung mit hervorgehobener Option „Projektmappe erstellen](media/sdk/qs-csharp-uwp-08-build.png "Erfolgreicher Build“")

1. Starten Sie die Anwendung. Wählen Sie in der Menüleiste **Debuggen** > **Debuggen starten** aus, oder drücken Sie **F5**.

    ![Screenshot der Visual Studio-Anwendung mit hervorgehobener Option „Debuggen starten](media/sdk/qs-csharp-uwp-09-start-debugging.png "Debuggen der App starten“")

1. Geben Sie Text in das Textfeld ein, und klicken Sie auf **Sprechen**. Ihr Text wird an die Speech-Dienste übermittelt und in Sprache synthetisiert, die dann über Ihren Lautsprecher ausgegeben wird.

    ![Screenshot der Sprachsynthesebenutzeroberfläche](media/sdk/qs-tts-csharp-uwp-ui-result.png)

## <a name="next-steps"></a>Nächste Schritte

> [!div class="nextstepaction"]
> [C#-Beispiele auf GitHub](https://aka.ms/csspeech/samples)

## <a name="see-also"></a>Weitere Informationen

- [Anpassen von Voicefonts](how-to-customize-voice-font.md)
- [Aufnehmen von Sprachbeispielen](record-custom-voice-samples.md)
