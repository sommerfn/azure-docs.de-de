---
title: 'Schnellstart: Übersetzen von Sprache, C# (UWP) – Speech-Dienste'
titleSuffix: Azure Cognitive Services
description: In dieser Schnellstartanleitung erstellen Sie eine einfache UWP-Anwendung (Universelle Windows-Plattform) zum Erfassen der Benutzersprache, Übersetzen in eine andere Sprache und Ausgeben des Texts in der Befehlszeile. Dieser Leitfaden ist für Windows-Benutzer bestimmt.
services: cognitive-services
author: wolfma61
manager: nitinme
ms.service: cognitive-services
ms.subservice: speech-service
ms.topic: quickstart
ms.date: 03/13/2019
ms.author: erhopf
ms.openlocfilehash: 3b8f0c93e596185f4446d4058641b2efc45ce4a5
ms.sourcegitcommit: c63e5031aed4992d5adf45639addcef07c166224
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 06/28/2019
ms.locfileid: "67467071"
---
# <a name="quickstart-translate-speech-with-the-speech-sdk-for-c-uwp"></a>Schnellstart: Übersetzen von Sprache mit dem Speech-SDK für C# (UWP)

Schnellstarts sind auch für [Sprachsynthese](quickstart-csharp-uwp.md), [Spracherkennung](quickstart-text-to-speech-csharp-uwp.md) und den [virtuellen Voice-First-Assistenten](quickstart-virtual-assistant-csharp-uwp.md) verfügbar.

In dieser Schnellstartanleitung erstellen Sie eine einfache UWP-Anwendung (Universelle Windows-Plattform), mit der die Spracheingabe des Benutzers über das Mikrofon Ihres Computers erfasst, die Sprache übersetzt und der übersetzte Text in Echtzeit in der Befehlszeile transkribiert wird. Diese Anwendung ist für die Ausführung unter Windows (64 Bit) konzipiert und basiert auf dem [Speech-SDK-NuGet-Paket](https://aka.ms/csspeech/nuget) und Microsoft Visual Studio 2017.

Eine vollständige Liste mit den verfügbaren Sprachen für die Sprachübersetzung finden Sie auf der Seite zur [Sprachunterstützung](language-support.md).

> [!NOTE]
> Per UWP können Sie Apps entwickeln, die auf einem beliebigen Gerät mit Unterstützung für Windows 10 (z.B. PCs, Xbox, Surface Hub und andere Geräte) ausgeführt werden.

## <a name="prerequisites"></a>Voraussetzungen

Für diese Schnellstartanleitung ist Folgendes erforderlich:

* [Visual Studio 2017](https://visualstudio.microsoft.com/downloads/)
* Ein Azure-Abonnementschlüssel für den Speech-Dienst. [Hier erhalten Sie einen kostenlosen Schlüssel.](get-started.md)

## <a name="create-a-visual-studio-project"></a>Erstellen eines Visual Studio-Projekts

[!INCLUDE [](../../../includes/cognitive-services-speech-service-quickstart-uwp-create-proj.md)]

## <a name="add-sample-code"></a>Hinzufügen von Beispielcode

1. Die Benutzeroberfläche der Anwendung wird mithilfe von XAML definiert. Öffnen Sie `MainPage.xaml` im Projektmappen-Explorer. Fügen Sie in der XAML-Ansicht des Designers den folgenden XAML-Ausschnitt zwischen `<Grid>` und `</Grid>` ein.

    [!code-xml[UI elements](~/samples-cognitive-services-speech-sdk/quickstart/speech-translation/csharp-uwp/helloworld/MainPage.xaml#StackPanel)]

1. Öffnen Sie die CodeBehind-Quelldatei `MainPage.xaml.cs` (sie ist unter `MainPage.xaml` gruppiert). Ersetzen Sie den gesamten darin enthaltenen Code wie folgt.

    [!code-csharp[Quickstart Code](~/samples-cognitive-services-speech-sdk/quickstart/speech-translation/csharp-uwp/helloworld/MainPage.xaml.cs#code)]

1. Ersetzen Sie im `SpeechTranslationFromMicrophone_ButtonClicked`-Handler in dieser Datei die Zeichenfolge `YourSubscriptionKey` durch Ihren Abonnementschlüssel.

1. Ersetzen Sie im Handler `SpeechTranslationFromMicrophone_ButtonClicked` die Zeichenfolge `YourServiceRegion` durch die [Region](regions.md), die mit Ihrem Abonnement verknüpft ist (z.B. `westus` für das kostenlose Testabonnement).

1. Speichern Sie alle Änderungen am Projekt.

## <a name="build-and-run-the-app"></a>Erstellen und Ausführen der App

1. Erstellen Sie die Anwendung. Wählen Sie in der Menüleiste **Build** > **Projektmappe erstellen** aus. Der Code sollte nun ohne Fehler kompiliert werden.

    ![Screenshot der Visual Studio-Anwendung mit hervorgehobener Option „Projektmappe erstellen](media/sdk/qs-csharp-uwp-08-build.png "Erfolgreicher Build“")

1. Starten Sie die Anwendung. Wählen Sie in der Menüleiste **Debuggen** > **Debuggen starten** aus, oder drücken Sie **F5**.

    ![Screenshot der Visual Studio-Anwendung mit hervorgehobener Option „Debuggen starten](media/sdk/qs-csharp-uwp-09-start-debugging.png "Debuggen der App starten“")

1. Ein Fenster wird angezeigt. Wählen Sie **Mikrofon aktivieren** aus, und bestätigen Sie die Berechtigungsanforderung, die angezeigt wird.

    ![Screenshot der Berechtigungsanforderung](media/sdk/qs-csharp-uwp-10-access-prompt.png "Debuggen der App starten")

1. Wählen Sie **Spracherkennung durch Mikrofoneingabe** aus, und sprechen Sie einen englischen Ausdruck oder Satz in das Mikrofon Ihres Geräts. Ihre Spracheingabe wird an den Spracherkennungsdienst übermittelt und in Text transkribiert, der im Fenster angezeigt wird.

    ![Screenshot der Spracherkennungsbenutzeroberfläche](media/sdk/qs-translate-csharp-uwp-ui-result.png)

## <a name="next-steps"></a>Nächste Schritte

> [!div class="nextstepaction"]
> [C#-Beispiele auf GitHub](https://aka.ms/csspeech/samples)

## <a name="see-also"></a>Weitere Informationen

- [Tutorial: Erstellen eines benutzerdefinierten Akustikmodells](how-to-customize-acoustic-models.md)
- [Tutorial: Erstellen eines benutzerdefinierten Sprachmodells](how-to-customize-language-model.md)
