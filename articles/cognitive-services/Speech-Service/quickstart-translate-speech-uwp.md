---
title: 'Schnellstart: Übersetzen von Sprache, C# (UWP) – Speech-Dienst'
titleSuffix: Azure Cognitive Services
description: In dieser Schnellstartanleitung erstellen Sie eine UWP-Anwendung (Universelle Windows-Plattform) zum Erfassen der Benutzersprache, Übersetzen in eine andere Sprache und Ausgeben des Texts in der Befehlszeile. Dieser Leitfaden ist für Windows-Benutzer bestimmt.
services: cognitive-services
author: lisaweixu
manager: nitinme
ms.service: cognitive-services
ms.subservice: speech-service
ms.date: 08/19/2019
ms.author: erhopf
ms.topic: quickstart
ms.openlocfilehash: e513cbbc615965ef196a830351aab8ac241c3f20
ms.sourcegitcommit: 49c4b9c797c09c92632d7cedfec0ac1cf783631b
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 09/05/2019
ms.locfileid: "70382651"
---
# <a name="quickstart-translate-speech-with-the-speech-sdk-for-c-uwp"></a>Schnellstart: Übersetzen von Sprache mit dem Speech-SDK für C# (UWP)

Schnellstarts sind auch für [Spracherkennung](quickstart-csharp-uwp.md), [Sprachsynthese](quickstart-text-to-speech-csharp-uwp.md) und den [virtuellen Voice-First-Assistenten](quickstart-virtual-assistant-csharp-uwp.md) verfügbar.

In dieser Schnellstartanleitung erstellen Sie eine UWP-Anwendung (Universelle Windows-Plattform), mit der die Spracheingabe des Benutzers über das Mikrofon Ihres Computers erfasst, die Sprache übersetzt und der übersetzte Text in Echtzeit in der Befehlszeile transkribiert wird. Diese Anwendung ist für die Ausführung unter Windows (64 Bit) konzipiert und basiert auf dem [NuGet-Paket für das Speech SDK](https://aka.ms/csspeech/nuget) und Microsoft Visual Studio 2019.

Eine vollständige Liste mit den verfügbaren Sprachen für die Sprachübersetzung finden Sie auf der Seite zur [Sprachunterstützung](language-support.md).

> [!NOTE]
> Per UWP können Sie Apps entwickeln, die auf einem beliebigen Gerät mit Unterstützung für Windows 10 (z.B. PCs, Xbox, Surface Hub und andere Geräte) ausgeführt werden.

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

   [!code-xml[UI elements](~/samples-cognitive-services-speech-sdk/quickstart/speech-translation/csharp-uwp/helloworld/MainPage.xaml#StackPanel)]

1. Öffnen Sie im **Projektmappen-Explorer** die CodeBehind-Quelldatei `MainPage.xaml.cs`. (Sie befindet sich unter `MainPage.xaml`.)

1. Ersetzen Sie den gesamten darin enthaltenen Code durch den folgenden Codeausschnitt:

   [!code-csharp[Quickstart Code](~/samples-cognitive-services-speech-sdk/quickstart/speech-translation/csharp-uwp/helloworld/MainPage.xaml.cs#code)]

1. Suchen Sie in dieser Datei im Handler `SpeechTranslationFromMicrophone_ButtonClicked` nach der Zeichenfolge `YourSubscriptionKey`, und ersetzen Sie sie durch Ihren Abonnementschlüssel.

1. Suchen Sie im Handler `SpeechTranslationFromMicrophone_ButtonClicked` nach der Zeichenfolge `YourServiceRegion`, und ersetzen Sie sie durch die [Region](regions.md), die mit Ihrem Abonnement verknüpft ist. (Verwenden Sie beispielsweise `westus` für das kostenlose Testabonnement.)

1. Wählen Sie auf der Menüleiste **Datei** > **Alle speichern** aus, um Ihre Änderungen zu speichern.

## <a name="build-and-run-the-application"></a>Erstellen und Ausführen der Anwendung

Nun können Sie Ihre Anwendung erstellen und testen.

1. Wählen Sie auf der Menüleiste **Erstellen** > **Projektmappe erstellen** aus, um die Anwendung zu erstellen. Der Code sollte nun ohne Fehler kompiliert werden.

1. Wählen Sie **Debuggen** > **Debuggen starten** aus (oder drücken Sie**F5**), um die Anwendung zu starten. Das Fenster **helloworld** wird angezeigt.

   ![UWP-Beispielanwendung für die Übersetzung in C#: Schnellstart](media/sdk/qs-translate-speech-uwp-helloworld-window.png)

1. Wählen Sie **Mikrofon aktivieren** und dann in der Zugriffsberechtigungsanforderung **Ja** aus.

   ![Berechtigungsanforderung für den Mikrofonzugriff](media/sdk/qs-csharp-uwp-10-access-prompt.png)

1. Wählen Sie **Translate speech from the microphone input** (Sprachübersetzung aus Mikrofoneingabe) aus, und sprechen Sie einen englischen Ausdruck oder Satz in das Mikrofon Ihres Geräts. Die Anwendung überträgt ihn an die Speech-Dienste, die ihn in Text in einer anderen Sprache (in diesem Fall: Deutsch) übersetzen. Der Speech-Dienst sendet den übersetzten Text zurück an die Anwendung, die die Übersetzung im Fenster anzeigt.

   ![Benutzeroberfläche der Sprachübersetzung](media/sdk/qs-translate-csharp-uwp-ui-result.png)

## <a name="next-steps"></a>Nächste Schritte

> [!div class="nextstepaction"]
> [C#-Beispiele auf GitHub](https://aka.ms/csspeech/samples)

## <a name="see-also"></a>Weitere Informationen

- [Trainieren eines Modells für Custom Speech](how-to-custom-speech-train-model.md)
