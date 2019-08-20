---
title: 'Schnellstart: Erstellen einer iOS-App zum Starten des plastischen Readers (Swift)'
titlesuffix: Azure Cognitive Services
description: In dieser Schnellstartanleitung erstellen Sie eine iOS-App von Grund auf neu und fügen die Funktion für Plastischer Reader hinzu.
services: cognitive-services
author: MeganRoach
ms.service: cognitive-services
ms.subservice: immersive-reader
ms.topic: quickstart
ms.date: 08/01/2019
ms.author: t-meroa
ms.openlocfilehash: 64b3cab857a541d0bede88e7fdf21c00526b9a43
ms.sourcegitcommit: 5d6c8231eba03b78277328619b027d6852d57520
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 08/13/2019
ms.locfileid: "68965148"
---
# <a name="quickstart-create-an-ios-app-that-launches-the-immersive-reader-swift"></a>Schnellstart: Erstellen einer iOS-App zum Starten von Plastischer Reader (Swift)

Der [plastische Reader](https://www.onenote.com/learningtools) ist ein inklusiv konzipiertes Tool, das bewährte Techniken zur Verbesserung des Leseverständnisses implementiert.

In dieser Schnellstartanleitung erstellen Sie eine iOS-App von Grund auf neu und integrieren den plastischen Reader anhand des SDK für den plastischen Reader. Eine vollständiges Arbeitsbeispiel für diesen Schnellstart finden Sie [hier](https://github.com/microsoft/immersive-reader-sdk/tree/master/iOS/samples/quickstart-swift).

Wenn Sie kein Azure-Abonnement besitzen, können Sie ein [kostenloses Konto](https://azure.microsoft.com/free/?WT.mc_id=A261C142F) erstellen, bevor Sie beginnen.

## <a name="prerequisites"></a>Voraussetzungen

* [Xcode](https://apps.apple.com/us/app/xcode/id497799835?mt=12)
* Eine Ressource des plastischen Readers, die für die Authentifizierung mit Azure Active Directory (Azure AD) konfiguriert ist. Folgen Sie [diesen Anweisungen](./azure-active-directory-authentication.md) für die Einrichtung. Einige der hier erstellten Werte benötigen Sie bei der Konfiguration der Beispielprojekteigenschaften. Speichern Sie die Ausgabe Ihrer Sitzung zur späteren Verwendung in einer Textdatei.

## <a name="create-an-xcode-project"></a>Erstellen eines Xcode-Projekts

Erstellen Sie ein neues Projekt in Xcode.

![Neues Projekt](./media/ios/xcode-create-project.png)

Wählen Sie **Einzelansicht-App**.

![Neue Einzelansicht-App](./media/ios/xcode-single-view-app.png)

## <a name="get-the-sdk-cocoapod"></a>Abrufen der SDK-CocoaPod-Instanz
Das SDK für den plastischen Reader lässt sich am einfachsten über CocoaPods verwenden. Gehen Sie zum Installieren über CocoaPods wie folgt vor:
1. Führen Sie die unter [Getting Started](http://guides.cocoapods.org/using/getting-started.html) (Erste Schritte) beschriebenen Schritte aus, um CocoaPods zu installieren.
2. Führen Sie `pod init` im Stammverzeichnis Ihres Xcode-Projekts aus, um eine Podfile-Datei zu erstellen.
3.  Fügen Sie `pod 'immersive-reader-sdk', :path => 'https://github.com/microsoft/immersive-reader-sdk/tree/master/iOS/immersive-reader-sdk'` hinzu, um die CocoaPod-Instanz Ihrer Podfile-Datei hinzuzufügen. Ihr Profil sollte wie folgt aussehen (mit Ersetzung von „quickstart-swift“ durch den Namen Ihres Ziels):
 ```ruby
  platform :ios, '9.0'

  target 'quickstart-swift' do
  use_frameworks!
  # Pods for quickstart-swift
  pod 'immersive-reader-sdk', :path => 'https://github.com/microsoft/immersive-reader-sdk/tree/master/iOS/immersive-reader-sdk'
  end
```
4. Führen Sie im Terminal im Verzeichnis Ihres Xcode-Projekts den Befehl `pod install` aus, um den Pod für das SDK für den plastischen Reader zu installieren.
5. Fügen Sie `import immersive_reader_sdk` allen Dateien hinzu, die auf das SDK verweisen müssen.
6. Achten Sie beim Öffnen des Projekts darauf, dass Sie die Datei `.xcworkspace` verwenden, und nicht die Datei `.xcodeproj`.

## <a name="acquire-an-azure-ad-authentication-token"></a>Abrufen eines Azure AD-Authentifizierungstokens

Für diesen Teil benötigen Sie einige Werte aus dem obigen Schritt zur Konfiguration der Azure AD-Authentifizierung. Greifen Sie auf die Textdatei zurück, die Sie in dieser Sitzung gespeichert haben.

````text
TenantId     => Azure subscription TenantId
ClientId     => Azure AD ApplicationId
ClientSecret => Azure AD Application Service Principal password
Subdomain    => Immersive Reader resource subdomain (resource 'Name' if the resource was created in the Azure portal, or 'CustomSubDomain' option if the resource was created with Azure CLI Powershell. Check the Azure portal for the subdomain on the Endpoint in the resource Overview page, for example, 'https://[SUBDOMAIN].cognitiveservices.azure.com/')
````

Erstellen Sie im Hauptprojektordner, der die Datei „ViewController.swift“ enthält, eine Swift-Klassendatei namens „Constants.swift“. Ersetzen Sie die Klasse durch den folgenden Code, und fügen Sie nach Bedarf Ihre eigenen Werte hinzu. Diese Datei darf nur als lokale Datei auf Ihrem Computer vorhanden sein. Committen Sie sie nicht in Ihre Quellcodeverwaltung, da sie Geheimnisse enthält, die nicht für die Öffentlichkeit bestimmt sind. Es wird empfohlen, keine Geheimnisse in Ihrer App zu speichern. Verwenden Sie stattdessen einen Back-End-Dienst, um das Token abzurufen, sodass die Geheimnisse außerhalb der App und außerhalb des Geräts gespeichert werden können. Der Back-End-API-Endpunkt muss durch eine Authentifizierung (beispielsweise [OAuth](https://oauth.net/2/)) geschützt werden, um zu verhindern, dass nicht autorisierte Benutzer Token abrufen und für Ihren Dienst „Plastischer Reader“ bzw. für die Abrechnung verwenden. Diese Schritte sind in dieser Schnellstartanleitung nicht beschrieben.

[!code-swift[Constants](~/ImmersiveReaderSdk/iOS/samples/quickstart-swift/quickstart-swift/Constants.swift)]

## <a name="set-up-the-app-to-run-without-a-storyboard"></a>Einrichten der App für die Ausführung ohne Storyboard

Öffnen Sie „AppDelegate.swift“, und ersetzen Sie die Datei durch den folgenden Code:

[!code-swift[AppDelegate](~/ImmersiveReaderSdk/iOS/samples/quickstart-swift/quickstart-swift/AppDelegate.swift)]

## <a name="create-the-launch-view-controller-and-add-sample-content"></a>Erstellen des Launch View-Controllers und Hinzufügen von Beispielinhalten

Benennen Sie „ViewController.swift“ in „LaunchViewController.swift“ um, und ersetzen Sie die Datei durch den folgenden Code.

[!code-swift[LaunchViewController](~/ImmersiveReaderSdk/iOS/samples/quickstart-swift/quickstart-swift/LaunchViewController.swift)]

## <a name="build-and-run-the-app"></a>Erstellen und Ausführen der App

Legen Sie in Xcode das Archivschema fest, indem Sie einen Simulator oder ein Geräteziel auswählen.
![Archivschema](./media/ios/xcode-archive-scheme.png)<br/>
![Auswählen des Ziels](./media/ios/xcode-select-target.png)

Drücken Sie in Xcode die Tastenkombination STRG+R, oder klicken Sie auf die Wiedergabeschaltfläche, um das Projekt auszuführen. Daraufhin sollte die App im angegebenen Simulator oder auf dem angegebenen Gerät gestartet werden.

In Ihrer App sollte Folgendes angezeigt werden:

![Beispiel-App](./media/ios/sample-app-ipad.png)

Wenn Sie auf die Schaltfläche „Plastischer Reader“ klicken, sehen Sie, dass der plastische Reader mit dem Inhalt in der App gestartet wurde.

![Plastischer Reader](./media/ios/immersive-reader-ipad.png)

## <a name="next-steps"></a>Nächste Schritte

* Machen Sie sich mit dem [Immersive Reader iOS SDK](https://github.com/microsoft/immersive-reader-sdk/tree/master/iOS) und der [zugehörigen Referenz](./ios-reference.md) vertraut.
