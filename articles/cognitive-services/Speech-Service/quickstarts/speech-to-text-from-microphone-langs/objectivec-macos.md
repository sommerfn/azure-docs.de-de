---
title: 'Schnellstart: Erkennen von Spracheingaben per Mikrofon in Objective-C – Spracherkennungsdienst'
titleSuffix: Azure Cognitive Services
description: Hier erfahren Sie, wie Sie Sprache in Objective-C unter macOS mit dem Speech SDK erkennen.
services: cognitive-services
author: chlandsi
manager: nitinme
ms.service: cognitive-services
ms.subservice: speech-service
ms.topic: quickstart
ms.date: 07/05/2019
ms.author: chlandsi
ms.openlocfilehash: 0025e6e84a8a76ad3a332056f51f802ca95da23f
ms.sourcegitcommit: c22327552d62f88aeaa321189f9b9a631525027c
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 11/04/2019
ms.locfileid: "73505659"
---
# <a name="quickstart-recognize-speech-in-objective-c-on-macos-by-using-the-speech-sdk"></a>Schnellstart: Erkennen von Sprache in Objective-C unter macOS mit dem Speech SDK

Schnellstarts sind auch für [Sprachsynthese](~/articles/cognitive-services/Speech-Service/quickstarts/text-to-speech-langs/objectivec-macos.md) verfügbar.

In diesem Artikel erfahren Sie, wie Sie eine macOS-App in Objective-C mit dem Azure Cognitive Services Speech SDK erstellen, um mit dem Mikrofon aufgezeichnete Sprache in Text zu transkribieren.

## <a name="prerequisites"></a>Voraussetzungen

Bevor Sie beginnen, benötigen Sie Folgendes:

* Ein [Abonnementschlüssel](~/articles/cognitive-services/Speech-Service/get-started.md) für den Speech-Dienst.
* Einen macOS-Computer mit [Xcode 9.4.1](https://geo.itunes.apple.com/us/app/xcode/id497799835?mt=12) oder höher und macOS 10.13 oder höher

## <a name="get-the-speech-sdk-for-macos"></a>Herunterladen des Speech SDK für macOS

[!INCLUDE [License notice](~/includes/cognitive-services-speech-service-license-notice.md)]

Das Cognitive Services Speech SDK für Mac wird als Frameworkpaket bereitgestellt. Es kann in Xcode-Projekten als [CocoaPod](https://cocoapods.org/) verwendet oder von https://aka.ms/csspeech/macosbinary heruntergeladen und manuell verknüpft werden. In diesem Artikel wird CocoaPods verwendet.

## <a name="create-an-xcode-project"></a>Erstellen eines Xcode-Projekts

Starten Sie Xcode, und starten Sie ein neues Projekt, indem Sie **Datei** > **Neu** > **Projekt** auswählen. Wählen Sie im Dialogfeld zum Auswählen der Vorlage die Vorlage **Cocoa App** aus.

Treffen Sie in den nachfolgenden Dialogfeldern die folgende Auswahl:

1. Im Dialogfeld **Projektoptionen**:
    1. Geben Sie einen Namen für die Schnellstart-App ein, z. B. *helloworld*.
    1. Geben Sie einen entsprechenden Organisationsnamen und eine Organisations-ID ein, wenn Sie bereits über ein Apple-Entwicklerkonto verfügen. Verwenden Sie zu Testzwecken einen Namen wie *testorg*. Um die App zu signieren, benötigen Sie ein ordnungsgemäßes Bereitstellungsprofil. Weitere Informationen finden Sie auf der [App Developer-Website](https://developer.apple.com/).
    1. Stellen Sie sicher, dass **Objective-C** als Sprache für das Projekt ausgewählt ist.
    1. Deaktivieren Sie die Kontrollkästchen zum Verwenden von Storyboards und Erstellen einer dokumentbasierten Anwendung. Die einfache Benutzeroberfläche für die Beispiel-App wird programmgesteuert erstellt.
    1. Deaktivieren Sie alle Kontrollkästchen für Tests und Kerndaten.

    ![Projekteinstellungen](~/articles/cognitive-services/Speech-Service/media/sdk/qs-objectivec-macos-project-settings.png)

1. Auswählen eines Projektverzeichnisses:
    1. Wählen Sie ein Verzeichnis aus, in dem das Projekt gespeichert werden soll. In diesem Schritt wird im Basisverzeichnis das Verzeichnis „helloworld“ erstellt, das alle Dateien für das Xcode-Projekt enthält.
    1. Deaktivieren Sie die Erstellung eines Git-Repositorys für dieses Beispielprojekt.
1. Legen Sie die Berechtigungen für den Netzwerk- und Mikrofonzugriff fest. Wählen Sie auf der linken Seite in der ersten Zeile der Übersicht den App-Namen aus, um die App-Konfiguration anzuzeigen. Wählen Sie dann die Registerkarte **Funktionen** aus.
    1. Aktivieren Sie die Einstellung **App sandbox** (App-Sandbox) für die App.
    1. Aktivieren Sie die Kontrollkästchen **Outgoing Connections** (Ausgehende Verbindungen) und **Microphone** (Mikrofon).

    ![Sandboxeinstellungen](~/articles/cognitive-services/Speech-Service/media/sdk/qs-objectivec-macos-sandbox.png)

1. Die App muss die Verwendung des Mikrofons zudem in der Datei `Info.plist` deklarieren. Wählen Sie in der Übersicht die Datei aus, und fügen Sie den Schlüssel **Privacy – Microphone Usage Description** (Datenschutz – Beschreibung der Mikrofonverwendung) mit einem Wert wie *Mikrofon ist zur Spracherkennung erforderlich* hinzu.

    ![Einstellungen in der Datei „Info.plist“](~/articles/cognitive-services/Speech-Service/media/sdk/qs-objectivec-macos-info-plist.png)

1. Schließen Sie das Xcode-Projekt. Sie verwenden später nach dem Einrichten von CocoaPods eine andere Instanz des Projekts.

## <a name="install-the-sdk-as-a-cocoapod"></a>Installieren des SDK als CocoaPod

1. Installieren Sie den CocoaPod-Abhängigkeits-Manager entsprechend den Anweisungen in der [Installationsanleitung](https://guides.cocoapods.org/using/getting-started.html).
1. Navigieren Sie zum Verzeichnis Ihrer Beispiel-App (helloworld). Speichern Sie eine Textdatei mit dem Namen *Podfile* und dem folgenden Inhalt in diesem Verzeichnis:

   [!code-ruby[Quickstart code](~/samples-cognitive-services-speech-sdk/quickstart/objectivec/macos/from-microphone/helloworld/Podfile)]
1. Navigieren Sie in einem Terminal zum Verzeichnis „helloworld“, und führen Sie den Befehl `pod install` aus. Mit diesem Befehl wird ein Xcode-Arbeitsbereich `helloworld.xcworkspace` generiert, der die Beispiel-App und das Speech SDK als Abhängigkeit enthält. Dieser Arbeitsbereich wird in den folgenden Schritten verwendet.

## <a name="add-the-sample-code"></a>Hinzufügen des Beispielcodes

1. Öffnen Sie den Arbeitsbereich `helloworld.xcworkspace` in Xcode.
1. Ersetzen Sie den Inhalt der automatisch generierten Datei `AppDelegate.m` durch den folgenden Code:

   [!code-objectivec[Quickstart code](~/samples-cognitive-services-speech-sdk/quickstart/objectivec/macos/from-microphone/helloworld/helloworld/AppDelegate.m#code)]
1. Ersetzen Sie die Zeichenfolge `YourSubscriptionKey` durch Ihren Abonnementschlüssel.
1. Ersetzen Sie die Zeichenfolge `YourServiceRegion` durch die mit Ihrem Abonnement verknüpfte [Region](~/articles/cognitive-services/Speech-Service/regions.md). Verwenden Sie beispielsweise `westus` für das kostenlose Testabonnement.

## <a name="build-and-run-the-sample"></a>Erstellen und Ausführen des Beispiels

1. Zeigen Sie die Debugausgabe an (**Ansicht** > **Debugbereich** > **Konsole aktivieren**).
1. Erstellen Sie den Beispielcode, und führen Sie ihn aus, indem Sie im Menü **Product** > **Run** (Produkt > Ausführen) auswählen. Sie können auch **Play** (Wiedergeben) auswählen.
1. Nachdem Sie die Schaltfläche ausgewählt und ein paar Worte gesagt haben, sollte der gesprochene Text im unteren Bildschirmbereich angezeigt werden. Bei der erstmaligen Ausführung der App sollten Sie dazu aufgefordert werden, der App Zugriff auf das Mikrofon Ihres Computers zu gewähren.

## <a name="next-steps"></a>Nächste Schritte

> [!div class="nextstepaction"]
> [Objective-C-Beispiele auf GitHub](https://aka.ms/csspeech/samples)
