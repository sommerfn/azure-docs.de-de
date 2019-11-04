---
title: 'Schnellstart: Sprachsynthese, Swift – Speech-Dienst'
titleSuffix: Azure Cognitive Services
description: Hier erfahren Sie, wie Sie unter macOS mit dem Speech SDK Sprache in Swift synthetisieren.
services: cognitive-services
author: yulin-li
manager: nitinme
ms.service: cognitive-services
ms.subservice: speech-service
ms.topic: quickstart
ms.date: 09/20/2019
ms.author: yulili
ms.openlocfilehash: 727929efb499f32dcd42a964b6b6c8e762803590
ms.sourcegitcommit: c22327552d62f88aeaa321189f9b9a631525027c
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 11/04/2019
ms.locfileid: "73505515"
---
# <a name="quickstart-synthesize-speech-in-swift-on-macos-using-the-speech-sdk"></a>Schnellstart: Sprachsynthese in Swift unter macOS mit dem Speech SDK

In diesem Artikel erfahren Sie, wie Sie eine macOS-App in Swift mit dem Cognitive Services Speech SDK erstellen, um Sprache aus Text zu synthetisieren und über die Standardaudioausgabe wiederzugeben.

## <a name="prerequisites"></a>Voraussetzungen

Folgendes wird vorausgesetzt:

* Ein [Abonnementschlüssel](~/articles/cognitive-services/Speech-Service/get-started.md) für den Speech-Dienst.
* Ein macOS-Computer mit [Xcode 9.4.1](https://geo.itunes.apple.com/us/app/xcode/id497799835?mt=12) oder höher und installierte [CocoaPods](https://cocoapods.org/).

## <a name="get-the-speech-sdk-for-macos"></a>Herunterladen des Speech SDK für macOS

[!INCLUDE [License Notice](~/includes/cognitive-services-speech-service-license-notice.md)]

Das Cognitive Services Speech SDK für macOS wird als Frameworkpaket bereitgestellt.
Es kann in Xcode-Projekten als [CocoaPod](https://cocoapods.org/) verwendet oder von https://aka.ms/csspeech/macosbinary heruntergeladen und manuell verknüpft werden. In dieser Anleitung wird ein CocoaPod verwendet.

> [!NOTE] 
> Ältere Versionen als 1.7.0 des SDK können für dieses Tutorial nicht verwendet werden.

## <a name="create-an-xcode-project"></a>Erstellen eines Xcode-Projekts

Starten Sie Xcode, und starten Sie ein neues Projekt, indem Sie auf **Datei** > **Neu** > **Projekt** klicken.
Wählen Sie im Dialogfeld zum Auswählen der Vorlage die Vorlage „Cocoa App“ aus.

Treffen Sie in den nachfolgenden Dialogfeldern die folgende Auswahl:

1. Dialogfeld „Projektoptionen“
    1. Geben Sie einen Namen für die Schnellstart-App ein, z.B. `helloworld`.
    1. Geben Sie einen entsprechenden Organisationsnamen und eine Organisations-ID ein, wenn Sie bereits über ein Apple-Entwicklerkonto verfügen. Zu Testzwecken können Sie einfach einen beliebigen Namen wie `testorg` auswählen. Um die App zu signieren, benötigen Sie ein ordnungsgemäßes Bereitstellungsprofil. Details finden Sie auf der [Apple Developer-Website](https://developer.apple.com/).
    1. Stellen Sie sicher, dass Swift als Sprache für das Projekt ausgewählt wird.
    1. Deaktivieren Sie die Kontrollkästchen zum Verwenden von Storyboards und Erstellen einer dokumentbasierten Anwendung. Die einfache Benutzeroberfläche für die Beispiel-App wird programmgesteuert erstellt.
    1. Deaktivieren Sie alle Kontrollkästchen für Tests und Kerndaten.
1. Auswählen des Projektverzeichnisses
    1. Wählen Sie ein Verzeichnis aus, in dem das Projekt gespeichert werden soll. Dadurch wird im ausgewählten Verzeichnis das Verzeichnis `helloworld` erstellt, das alle Dateien für das Xcode-Projekt enthält.
    1. Deaktivieren Sie die Erstellung eines Git-Repositorys für dieses Beispielprojekt.
1. Schließen Sie das Xcode-Projekt. Sie verwenden später nach dem Einrichten von CocoaPods eine andere Instanz des Projekts.

## <a name="add-the-sample-code"></a>Hinzufügen des Beispielcodes

1. Platzieren Sie innerhalb des HelloWorld-Projekts eine neue Headerdatei mit dem Namen `MicrosoftCognitiveServicesSpeech-Bridging-Header.h` im Verzeichnis `helloworld`, und fügen Sie den folgenden Code in diese ein:

   [!code-objectivec[Quickstart Code](~/samples-cognitive-services-speech-sdk/quickstart/swift/macos/text-to-speech/helloworld/helloworld/MicrosoftCognitiveServicesSpeech-Bridging-Header.h#code)]
1. Fügen Sie in den Swift-Projekteinstellungen für das HelloWorld-Ziel im Feld ![Headereigenschaften](~/articles/cognitive-services/Speech-Service/media/sdk/qs-swift-macos-bridging-header.png) von *Objective-C: Bridgingheader* den relativen Pfad `helloworld/MicrosoftCognitiveServicesSpeech-Bridging-Header.h` dem Bridgingheader hinzu.
1. Ersetzen Sie den Inhalt der automatisch generierten Datei `AppDelegate.swift` durch Folgendes:

   [!code-swift[Quickstart Code](~/samples-cognitive-services-speech-sdk/quickstart/swift/macos/text-to-speech/helloworld/helloworld/AppDelegate.swift#code)]
1. Ersetzen Sie in `AppDelegate.swift` die Zeichenfolge `YourSubscriptionKey` durch Ihren Abonnementschlüssel.
1. Ersetzen Sie die Zeichenfolge `YourServiceRegion` durch die Region, die mit Ihrem Abonnement verknüpft ist (z.B. `westus` für das kostenlose Testabonnement).

## <a name="install-the-sdk-as-a-cocoapod"></a>Installieren des SDK als CocoaPod

1. Installieren Sie den CocoaPod-Abhängigkeits-Manager entsprechend den Anweisungen in der [Installationsanleitung](https://guides.cocoapods.org/using/getting-started.html).
1. Navigieren Sie zum Verzeichnis Ihrer Beispiel-App (`helloworld`). Speichern Sie eine Textdatei mit dem Namen `Podfile` und dem folgenden Inhalt in diesem Verzeichnis:

   [!code-ruby[Quickstart Code](~/samples-cognitive-services-speech-sdk/quickstart/swift/macos/text-to-speech/helloworld/Podfile)]
1. Navigieren Sie in einem Terminal zum Verzeichnis `helloworld`, und führen Sie den Befehl `pod install` aus. Dadurch wird ein Xcode-Arbeitsbereich `helloworld.xcworkspace` generiert, der die Beispiel-App und das Speech SDK als Abhängigkeit enthält. Dieser Arbeitsbereich wird im Folgenden verwendet.

## <a name="build-and-run-the-sample"></a>Erstellen und Ausführen des Beispiels

1. Öffnen Sie den Arbeitsbereich `helloworld.xcworkspace` in Xcode.
1. Zeigen Sie die Debugausgabe an (**Ansicht** > **Debugbereich** > **Konsole aktivieren**).
1. Erstellen Sie den Beispielcode, und führen Sie ihn aus, indem Sie im Menü **Product** > **Run** (Produkt > Ausführen) auswählen oder auf die Schaltfläche **Play** (Wiedergeben) klicken.
1. Nachdem Sie Text eingegeben und in der App auf die Schaltfläche geklickt haben, sollten die synthetisierten Audiodaten wiedergegeben werden.

## <a name="next-steps"></a>Nächste Schritte

> [!div class="nextstepaction"]
> [Erkunden unserer Beispiele auf GitHub](https://aka.ms/csspeech/samples)

