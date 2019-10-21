---
title: 'Schnellstart: Erkennen von Sprache, Objective-C – Speech-Dienst'
titleSuffix: Azure Cognitive Services
description: Hier finden Sie Informationen zum Erkennen von Sprache in Objective-C unter iOS mit dem Speech SDK.
services: cognitive-services
author: chlandsi
manager: nitinme
ms.service: cognitive-services
ms.subservice: speech-service
ms.topic: quickstart
ms.date: 07/05/2019
ms.author: chlandsi
ms.openlocfilehash: f943f47cdef901f80aa455d3d1e02a753e0f06e4
ms.sourcegitcommit: 1d0b37e2e32aad35cc012ba36200389e65b75c21
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 10/15/2019
ms.locfileid: "72327755"
---
# <a name="quickstart-recognize-speech-in-objective-c-on-ios-by-using-the-speech-sdk"></a>Schnellstart: Erkennen von Sprache in Objective-C unter iOS mit dem Speech SDK

Schnellstarts sind auch für [Sprachsynthese](quickstart-text-to-speech-objectivec-ios.md) verfügbar.

[!INCLUDE [Selector](../../../includes/cognitive-services-speech-service-quickstart-selector.md)]

In diesem Artikel erfahren Sie, wie Sie eine iOS-App in Objective-C mit dem Azure Cognitive Services Speech SDK erstellen, um Sprache über das Mikrofon oder aus einer Datei mit Audioaufzeichnung in Text zu transkribieren.

## <a name="prerequisites"></a>Voraussetzungen

Bevor Sie beginnen, benötigen Sie Folgendes:

* Ein [Abonnementschlüssel](get-started.md) für den Speech-Dienst.
* Einen macOS-Computer mit [Xcode 9.4.1](https://geo.itunes.apple.com/us/app/xcode/id497799835?mt=12) oder einer höheren Version
* Ziel mindestens auf die iOS-Version 9.3 festgelegt

## <a name="get-the-speech-sdk-for-ios"></a>Abrufen des Speech SDK für iOS

[!INCLUDE [License notice](../../../includes/cognitive-services-speech-service-license-notice.md)]

Das Cognitive Services Speech SDK für iOS wird derzeit als Cocoa-Framework bereitgestellt.
Es kann von [dieser Website](https://aka.ms/csspeech/iosbinary) heruntergeladen werden. Laden Sie die Datei in Ihr Basisverzeichnis herunter.

## <a name="create-an-xcode-project"></a>Erstellen eines Xcode-Projekts

Starten Sie Xcode, und starten Sie ein neues Projekt, indem Sie **Datei** > **Neu** > **Projekt** auswählen.
Wählen Sie im Dialogfeld zum Auswählen der Vorlage die Vorlage **Einzelansicht-App (iOS)** aus.

Treffen Sie in den nachfolgenden Dialogfeldern die folgende Auswahl:

1. Im Dialogfeld **Projektoptionen**:
    1. Geben Sie einen Namen für die Schnellstart-App ein, z. B. *helloworld*.
    1. Geben Sie einen entsprechenden Organisationsnamen und die Organisations-ID ein, wenn Sie bereits über ein Apple-Entwicklerkonto verfügen. Verwenden Sie zu Testzwecken einen Namen wie *testorg*. Um die App zu signieren, benötigen Sie ein ordnungsgemäßes Bereitstellungsprofil. Weitere Informationen finden Sie auf der [App Developer-Website](https://developer.apple.com/).
    1. Stellen Sie sicher, dass **Objective-C** als Sprache für das Projekt ausgewählt ist.
    1. Deaktivieren Sie alle Kontrollkästchen für Tests und Kerndaten.

    ![Projekteinstellungen](media/sdk/qs-objectivec-project-settings.png)

1. Auswählen eines Projektverzeichnisses:
   1. Wählen Sie das Basisverzeichnis aus, in dem das Projekt gespeichert werden soll. In diesem Schritt wird im Basisverzeichnis das Verzeichnis „helloworld“ erstellt, das alle Dateien für das Xcode-Projekt enthält.
   1. Deaktivieren Sie die Erstellung eines Git-Repositorys für dieses Beispielprojekt.
   1. Passen Sie die Pfade zum SDK auf dem Bildschirm mit den Projekteinstellungen an.
      1. Fügen Sie auf der Registerkarte **Allgemein** unter dem Header **Eingebettete Binärdateien** die SDK-Bibliothek als Framework hinzu, indem Sie **Eingebettete Binärdateien hinzufügen** > **Andere hinzufügen...** auswählen. Navigieren Sie zu Ihrem Basisverzeichnis, und wählen Sie die Datei `MicrosoftCognitiveServicesSpeech.framework` aus. Mit dieser Aktion wird dem Header **Verknüpftes Framework und Bibliotheken** automatisch die SDK-Bibliothek hinzugefügt.
         ![Hinzugefügtes Framework](media/sdk/qs-objectivec-framework.png)
      1. Navigieren Sie zur Registerkarte **Buildeinstellungen**, und wählen Sie die Einstellung **Alle** aus.
      1. Fügen Sie das Verzeichnis „$(SRCROOT)/..“ den **Frameworksuchpfaden** unter der Überschrift **Suchpfade** hinzu.

      ![Einstellung „Frameworksuchpfade“](media/sdk/qs-objectivec-framework-search-paths.png)

## <a name="set-up-the-ui"></a>Einrichten der Benutzeroberfläche

Die Benutzeroberfläche der Beispiel-App ist sehr einfach: Sie umfasst lediglich zwei Schaltflächen, um die Spracherkennung entweder mit einer Datei oder mit einer Mikrofoneingabe zu starten, sowie eine Beschriftung zum Anzeigen des Ergebnisses. Die Benutzeroberfläche wird im `Main.storyboard`-Teil des Projekts eingerichtet. Öffnen Sie die XML-Ansicht des Storyboards, indem Sie mit der rechten Maustaste auf den Eintrag `Main.storyboard` der Projektstruktur klicken und **Öffnen als** > **Quellcode** auswählen.

Ersetzen Sie den automatisch generierten XML-Code durch diesen Code:

[!code-xml[](~/samples-cognitive-services-speech-sdk/quickstart/objectivec-ios/helloworld/helloworld/Base.lproj/Main.storyboard)]

## <a name="add-the-sample-code"></a>Hinzufügen des Beispielcodes

1. Laden Sie die [WAV-Beispieldatei](https://raw.githubusercontent.com/Azure-Samples/cognitive-services-speech-sdk/f9807b1079f3a85f07cbb6d762c6b5449d536027/samples/cpp/windows/console/samples/whatstheweatherlike.wav) herunter, indem Sie mit der rechten Maustaste auf den Link klicken und dann **Ziel speichern unter** auswählen.
   Fügen Sie die WAV-Datei dem Projekt als Ressource hinzu, indem Sie sie aus einem Suchfenster in die Stammebene der Projektansicht ziehen.
   Wählen Sie im nachfolgenden Dialogfeld die Option **Fertig stellen** aus, ohne die Einstellungen zu ändern.
1. Ersetzen Sie den Inhalt der automatisch generierten Datei `ViewController.m` durch den folgenden Code:

   [!code-objectivec[Quickstart code](~/samples-cognitive-services-speech-sdk/quickstart/objectivec-ios/helloworld/helloworld/ViewController.m#code)]
1. Ersetzen Sie die Zeichenfolge `YourSubscriptionKey` durch Ihren Abonnementschlüssel.
1. Ersetzen Sie die Zeichenfolge `YourServiceRegion` durch die mit Ihrem Abonnement verknüpfte [Region](regions.md). Verwenden Sie beispielsweise `westus` für das kostenlose Testabonnement.
1. Fügen Sie die Anforderung für den Zugriff auf das Mikrofon hinzu. Klicken Sie mit der rechten Maustaste auf den Eintrag `Info.plist` der Projektstruktur, und wählen Sie anschließend **Öffnen als** > **Quellcode** aus. Fügen Sie die folgenden Zeilen im Abschnitt `<dict>` hinzu, und speichern Sie die Datei dann.

    ```xml
    <key>NSMicrophoneUsageDescription</key>
    <string>Need microphone access for speech recognition from microphone.</string>
    ```

## <a name="build-and-run-the-sample"></a>Erstellen und Ausführen des Beispiels

1. Zeigen Sie die Debugausgabe an (**Ansicht** > **Debugbereich** > **Konsole aktivieren**).
1. Wählen Sie als Ziel für die App in der Liste des Menüs **Produkt** > **Ziel** entweder den iOS-Simulator oder ein mit dem Entwicklungscomputer verbundenes iOS-Gerät aus.
1. Erstellen Sie den Beispielcode im iOS-Simulator, und führen Sie ihn aus, indem Sie im Menü **Produkt** > **Ausführen** auswählen. Sie können auch die Schaltfläche **Play** (Wiedergeben) auswählen.
1. Nachdem Sie in der App die Schaltfläche **Recognize (File)** (Erkennen (Datei)) ausgewählt haben, sollte der Inhalt der Audiodatei „What's the weather like?“ (Wie ist das Wetter?) im unteren Teil des Bildschirms angezeigt werden.

   ![Simulierte iOS-App](media/sdk/qs-objectivec-simulated-app.png)

1. Nachdem Sie in der App die Schaltfläche **Recognize (Microphone)** (Erkennen (Mikrofon)) ausgewählt und ein paar Worte gesagt haben, sollte der gesprochene Text im unteren Bildschirmbereich angezeigt werden.

## <a name="next-steps"></a>Nächste Schritte

> [!div class="nextstepaction"]
> [Objective-C-Beispiele auf GitHub](https://aka.ms/csspeech/samples)
