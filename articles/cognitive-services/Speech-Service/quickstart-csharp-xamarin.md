---
title: 'Schnellstart: Erkennen von Sprache, C# (Xamarin) – Speech-Dienst'
titleSuffix: Azure Cognitive Services
description: In diesem Artikel erstellen Sie mithilfe des Cognitive Services Speech SDK eine plattformübergreifende C#-Xamarin-Anwendung (Universelle Windows-Plattform) für Windows UWP, Android und iOS. Sie transkribieren über das Mikrofon des Geräts oder Simulators Sprache in Echtzeit in Text. Die Anwendung basiert auf dem NuGet-Paket für das Speech SDK und auf Microsoft Visual Studio 2019.
services: cognitive-services
author: jhakulin
manager: robch
ms.service: cognitive-services
ms.subservice: speech-service
ms.topic: quickstart
ms.date: 09/16/2019
ms.author: jhakulin
ms.custom: ''
ms.openlocfilehash: c9467bac8b5998252c021faca4eb4177c42a1736
ms.sourcegitcommit: bb65043d5e49b8af94bba0e96c36796987f5a2be
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 10/16/2019
ms.locfileid: "72387383"
---
# <a name="quickstart-recognize-speech-using-cross-platform-xamarin-app-by-using-the-speech-sdk"></a>Schnellstart: Erkennen von Sprache mithilfe einer plattformübergreifenden Xamarin-App unter Verwendung des Speech SDK

Schnellstarts sind auch für [Spracherkennung](quickstart-csharp-uwp.md), [Sprachsynthese](quickstart-text-to-speech-csharp-uwp.md) und [Sprachübersetzung](quickstart-translate-speech-uwp.md) verfügbar.

In diesem Artikel entwickeln Sie mithilfe von Xamarin für die universelle Windows-Plattform (Universal Windows Platform, UWP), Android und iOS unter Verwendung des [Speech SDK](speech-sdk.md) eine plattformübergreifende C#-Anwendung. Spracheingaben über das Gerätemikrofon werden von dem Programm in Echtzeit in Text transkribiert. Die Anwendung basiert auf dem [NuGet-Paket für das Speech SDK](https://aka.ms/csspeech/nuget) und auf Microsoft Visual Studio 2019 (beliebige Edition).

## <a name="prerequisites"></a>Voraussetzungen

Für diese Schnellstartanleitung ist Folgendes erforderlich:

* [Visual Studio 2019](https://visualstudio.microsoft.com/downloads/).
* Ein Azure-Abonnementschlüssel für den Speech-Dienst. [Hier erhalten Sie einen kostenlosen Schlüssel.](get-started.md)
* Ein Windows-PC mit Windows 10 Fall Creators Update (10.0, Build 16299 oder höher) mit funktionsfähigem Mikrofon
* [Xamarin-Installation in Visual Studio](https://docs.microsoft.com/xamarin/get-started/installation/?pivots=windows)
* [Xamarin Android-Installation unter Windows](https://docs.microsoft.com/xamarin/android/get-started/installation/windows)
* [Xamarin iOS-Installation unter Windows](https://docs.microsoft.com/xamarin/ios/get-started/installation/windows/?pivots=windows)
* Für Android: 
   * ein Android-Gerät (ARM32/64, x86; API 23: Android 6.0 Marshmallow oder höher), [aktiviert für die Entwicklung](https://developer.android.com/studio/debug/dev-options), mit einem funktionsfähigen Mikrofon
* Für iOS: ein iOS-Gerät (ARM64) oder ein iOS-Simulator (x64) [aktiviert für die Entwicklung](https://docs.microsoft.com/xamarin/ios/get-started/installation/device-provisioning/), mit einem funktionsfähigen Mikrofon
* Installieren Sie zur Unterstützung von Windows ARM64 die [optionalen Buildtools und das Windows 10 SDK für ARM/ARM64](https://blogs.windows.com/buildingapps/2018/11/15/official-support-for-windows-10-on-arm-development/).

## <a name="create-a-visual-studio-project"></a>Erstellen eines Visual Studio-Projekts

[!INCLUDE [](../../../includes/cognitive-services-speech-service-quickstart-xamarin-create-proj.md)]

## <a name="add-sample-code-for-the-common-helloworld-project"></a>Hinzufügen von Beispielcode für das allgemeine Projekt `helloworld`

Das allgemeine `helloworld`-Projekt enthält plattformunabhängige Implementierungen für Ihre plattformübergreifende Anwendung.
Fügen Sie nun den XAML-Code, der die Benutzeroberfläche der Anwendung definiert, sowie die C#-CodeBehind-Implementierung hinzu.

1. Öffnen Sie im **Projektmappen-Explorer** unter dem allgemeinen `helloworld`-Projekt die Datei `MainPage.xaml`.

1. Fügen Sie in der XAML-Ansicht des Designers den folgenden XAML-Ausschnitt in das **Grid**-Tag (zwischen `<StackLayout>` und `</StackLayout>`) ein.

   [!code-xml[UI elements](~/samples-cognitive-services-speech-sdk/quickstart/csharp-xamarin/helloworld/helloworld/MainPage.xaml)]

1. Öffnen Sie im **Projektmappen-Explorer** die CodeBehind-Quelldatei `MainPage.xaml.cs`. (Sie befindet sich unter `MainPage.xaml`.)

1. Ersetzen Sie den gesamten darin enthaltenen Code durch den folgenden Codeausschnitt:

   [!code-csharp[Quickstart Code](~/samples-cognitive-services-speech-sdk/quickstart/csharp-xamarin/helloworld/helloworld/MainPage.xaml.cs)]

1. Suchen Sie im Handler `OnRecognitionButtonClicked` der Quelldatei nach der Zeichenfolge `YourSubscriptionKey`, und ersetzen Sie sie durch Ihren Abonnementschlüssel.

1. Suchen Sie im Handler `OnRecognitionButtonClicked` nach der Zeichenfolge `YourServiceRegion`, und ersetzen Sie sie durch die [Region](regions.md), die mit Ihrem Abonnement verknüpft ist. (Verwenden Sie beispielsweise `westus` für das kostenlose Testabonnement.)

1. Als Nächstes müssen Sie einen [Xamarin-Dienst](https://docs.microsoft.com/xamarin/android/app-fundamentals/services/creating-a-service/) erstellen. Dieser Dienst wird zum Abfragen von Mikrofonberechtigungen über Projekte verschiedener Plattformen (UWP, Android und iOS) verwendet. Fügen Sie hierzu unter dem Projekt `helloworld` den neuen Ordner `Services` hinzu, und erstellen Sie darunter eine neue C#-Quelldatei. (Klicken Sie mit der rechten Maustaste auf `Services`, und klicken Sie dann auf **Hinzufügen** > **Neues Element** > **Codedatei**.) Benennen Sie sie in `IMicrophoneService.cs` um, und fügen Sie den gesamten Code aus dem folgenden Codeausschnitt in die Datei ein:

[!code-csharp[Quickstart Code](~/samples-cognitive-services-speech-sdk/quickstart/csharp-xamarin/helloworld/helloworld/Services/IMicrophoneService.cs)]

## <a name="add-sample-code-for-the-helloworldandroid-project"></a>Hinzufügen von Beispielcode für das Projekt `helloworld.Android`

Fügen Sie nun den C#-Code hinzu, der den Android-spezifischen Teil der Anwendung definiert.

1. Öffnen Sie im **Projektmappen-Explorer** unter dem Projekt `helloworld.Android` die Datei `MainActivity.cs`.

1. Ersetzen Sie den gesamten darin enthaltenen Code durch den folgenden Codeausschnitt:

   [!code-csharp[Quickstart Code](~/samples-cognitive-services-speech-sdk/quickstart/csharp-xamarin/helloworld/helloworld.Android/MainActivity.cs)]

1. Fügen Sie als Nächstes eine Android-spezifische Implementierung für `MicrophoneService` hinzu, indem Sie unter dem Projekt `helloworld.Android` den neuen Ordner `Services` erstellen. Erstellen Sie anschließend eine neue C#-Quelldatei, benennen Sie sie in `MicrophoneService.cs` um, und fügen Sie den folgenden Codeausschnitt in diese Datei ein:

   [!code-csharp[Quickstart Code](~/samples-cognitive-services-speech-sdk/quickstart/csharp-xamarin/helloworld/helloworld.Android/Services/MicrophoneService.cs)]

1. Öffnen Sie dann unter dem Ordner `Properties` die Datei `AndroidManifest.xml`, und fügen Sie die folgende Einstellung für „uses-permission“ für das Mikrofon zwischen `<manifest>` und `</manifest>` ein.
```xml
   <uses-permission android:name="android.permission.RECORD_AUDIO" />
```

## <a name="add-sample-code-for-the-helloworldios-project"></a>Hinzufügen von Beispielcode für das Projekt `helloworld.iOS`

Fügen Sie nun den C#-Code hinzu, der den iOS-spezifischen Teil der Anwendung definiert, und erstellen Sie außerdem für Apple-Geräte spezifische Konfigurationen für das Projekt „helloworld.iOS“.

1. Öffnen Sie im **Projektmappen-Explorer** unter dem Projekt `helloworld.iOS` die Datei `AppDelegate.cs`.

1. Ersetzen Sie den gesamten darin enthaltenen Code durch den folgenden Codeausschnitt:

   [!code-csharp[Quickstart Code](~/samples-cognitive-services-speech-sdk/quickstart/csharp-xamarin/helloworld/helloworld.iOS/AppDelegate.cs)]

1. Fügen Sie als Nächstes eine iOS-spezifische Implementierung für `MicrophoneService` hinzu, indem Sie unter dem Projekt `helloworld.iOS` den neuen Ordner `Services` erstellen. Erstellen Sie anschließend eine neue C#-Quelldatei, benennen Sie sie in `MicrophoneService.cs` um, und fügen Sie den folgenden Codeausschnitt in diese Datei ein:

   [!code-csharp[Quickstart Code](~/samples-cognitive-services-speech-sdk/quickstart/csharp-xamarin/helloworld/helloworld.iOS/Services/MicrophoneService.cs)]

1. Öffnen Sie unter dem Projekt `helloworld.iOS` die Datei „Info.plist“ im Text-Editor, und fügen Sie das folgende Schlüssel-Wert-Paar unter dem Wörterbuchabschnitt <key>NSMicrophoneUsageDescription</key> hinzu. 
   <string>Für diese Beispiel-App ist Mikrofonzugriff erforderlich.</string>
   > Hinweis: Stellen Sie bei der Entwicklung für iPhone-Geräte sicher, dass `Bundle Identifier` mit der Bereitstellungsprofil-App-ID des Geräts übereinstimmt, andernfalls tritt bei der Erstellung ein Fehler auf. Bei der Verwendung von iPhoneSimulator können Sie den Wert beibehalten.

1. Bei der Entwicklung auf einem Windows-PC müssen Sie über **Tools** > **iOS** > **Mit Mac koppeln** eine Verbindung mit dem Mac-Gerät herstellen. Führen Sie die Schritte des von Visual Studio bereitgestellten Assistenten aus, um eine Verbindung mit dem Mac-Gerät zu ermöglichen.

## <a name="add-sample-code-for-the-helloworlduwp-project"></a>Hinzufügen von Beispielcode für das Projekt `helloworld.UWP`

Fügen Sie nun den C#-Code hinzu, der den UWP-spezifischen Teil der Anwendung definiert.

1. Öffnen Sie im **Projektmappen-Explorer** unter dem Projekt `helloworld.UWP` die Datei `MainPage.xaml.cs`.

1. Ersetzen Sie den gesamten darin enthaltenen Code durch den folgenden Codeausschnitt:

   [!code-csharp[Quickstart Code](~/samples-cognitive-services-speech-sdk/quickstart/csharp-xamarin/helloworld/helloworld.UWP/MainPage.xaml.cs)]

1. Fügen Sie als Nächstes eine UWP-spezifische Implementierung für `MicrophoneService` hinzu, indem Sie unter dem Projekt `helloworld.UWP` den neuen Ordner `Services` erstellen. Erstellen Sie anschließend eine neue C#-Quelldatei, benennen Sie sie in `MicrophoneService.cs` um, und fügen Sie den folgenden Codeausschnitt in diese Datei ein:

   [!code-csharp[Quickstart Code](~/samples-cognitive-services-speech-sdk/quickstart/csharp-xamarin/helloworld/helloworld.UWP/Services/MicrophoneService.cs)]

1. Doppelklicken Sie dann in Visual Studio unter dem Projekt `helloworld.UWP` auf die Datei `Package.appxmanifest`, und überprüfen Sie, ob **Mikrofon** unter **Funktionen** aktiviert ist. Speichern Sie anschließend die Datei.
   > Hinweis: Falls die Warnung „Die Zertifikatdatei ist nicht vorhanden: helloworld.UWP_TemporaryKey.pfx“ angezeigt wird, finden Sie im Beispiel für die [Spracherkennung](quickstart-csharp-uwp.md) weitere Informationen.

1. Wählen Sie auf der Menüleiste **Datei** > **Alle speichern** aus, um Ihre Änderungen zu speichern.

## <a name="build-and-run-the-uwp-application"></a>Erstellen und Ausführen der UWP-Anwendung

1. Legen Sie `helloworld.UWP` als Startprojekt fest, klicken Sie mit der rechten Maustaste auf das Projekt `helloworld.UWP`, und wählen Sie zum Erstellen der Anwendung **Erstellen** aus. 

1. Wählen Sie **Debuggen** > **Debuggen starten** aus (oder drücken Sie**F5**), um die Anwendung zu starten. Das Fenster **helloworld** wird angezeigt.

   ![UWP-Beispielanwendung für die Spracherkennung in C#: Schnellstart](media/sdk/qs-csharp-xamarin-helloworld-uwp-window.png)

1. Wählen Sie **Mikrofon aktivieren** und dann in der Zugriffsberechtigungsanforderung **Ja** aus.

   ![Berechtigungsanforderung für den Mikrofonzugriff](media/sdk/qs-csharp-xamarin-uwp-access-prompt.png)

1. Wählen Sie **Spracherkennung starten** aus, und sprechen Sie einen englischen Ausdruck oder Satz in das Mikrofon Ihres Geräts. Ihre Spracheingabe wird an die Speech-Dienste übermittelt, in Text transkribiert und im Fenster angezeigt.

   ![Spracherkennungsbenutzeroberfläche](media/sdk/qs-csharp-xamarin-uwp-ui-result.png)

## <a name="build-and-run-the-android-and-ios-applications"></a>Erstellen und Ausführen der Android- und iOS-Anwendungen

Android- und iOS-Anwendungen werden auf dem Gerät oder im Simulator auf ähnliche Weise wie UWP-Anwendungen erstellt und ausgeführt. Es muss unbedingt sichergestellt werden, dass alle im Abschnitt `Prerequisites` dieses Dokuments aufgeführten SDKs ordnungsgemäß installiert werden.

## <a name="next-steps"></a>Nächste Schritte

> [!div class="nextstepaction"]
> [C#-Beispiele auf GitHub](https://aka.ms/csspeech/samples)
