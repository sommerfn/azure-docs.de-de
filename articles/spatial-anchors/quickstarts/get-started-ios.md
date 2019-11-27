---
title: 'Schnellstart: Erstellen einer iOS-App mit Azure Spatial Anchors | Microsoft-Dokumentation'
description: In dieser Schnellstartanleitung wird beschrieben, wie Sie mit Spatial Anchors eine iOS-App erstellen.
author: craigktreasure
manager: aliemami
services: azure-spatial-anchors
ms.author: crtreasu
ms.date: 02/24/2019
ms.topic: quickstart
ms.service: azure-spatial-anchors
ms.openlocfilehash: a1d2fa083c0083423767484b20e296a3080e4ebe
ms.sourcegitcommit: a170b69b592e6e7e5cc816dabc0246f97897cb0c
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 11/14/2019
ms.locfileid: "74092115"
---
# <a name="quickstart-create-an-ios-app-with-azure-spatial-anchors-in-either-swift-or-objective-c"></a>Schnellstart: Erstellen einer iOS-App mit Azure Spatial Anchors in Swift oder Objective-C

In dieser Schnellstartanleitung wird beschrieben, wie Sie mit [Azure Spatial Anchors](../overview.md) in Swift oder Objective-C eine iOS-App erstellen. Azure Spatial Anchors ist ein plattformübergreifender Entwicklerdienst, mit dem Sie Mixed Reality-Umgebungen mit Objekten erstellen können, die ihre Position im Zeitverlauf geräteübergreifend beibehalten. Nach Abschluss des Vorgangs verfügen Sie über eine ARKit-iOS-App, mit der ein räumlicher Anker gespeichert und abgerufen werden kann.

Sie lernen Folgendes:

> [!div class="checklist"]
> * Erstellen eines Spatial Anchors-Kontos
> * Konfigurieren des Bezeichners und Kontoschlüssels für das Spatial Anchors-Konto
> * Bereitstellen und Ausführen auf einem iOS-Gerät

[!INCLUDE [quickstarts-free-trial-note](../../../includes/quickstarts-free-trial-note.md)]

## <a name="prerequisites"></a>Voraussetzungen

Stellen Sie für diese Schnellstartanleitung sicher, dass Sie über Folgendes verfügen:

- Einen für Entwickler geeigneten macOS-Computer mit der aktuellen Version von <a href="https://geo.itunes.apple.com/us/app/xcode/id497799835?mt=12" target="_blank">Xcode</a> und <a href="https://cocoapods.org" target="_blank">CocoaPods</a>.
- Git-Installation über Homebrew. Geben Sie den folgenden Befehl in einer einzelnen Terminalzeile ein: `/usr/bin/ruby -e "$(curl -fsSL https://raw.githubusercontent.com/Homebrew/install/master/install)"`. Führen Sie anschließend `brew install git` aus.
- Ein für Entwickler geeignetes <a href="https://developer.apple.com/documentation/arkit/verifying_device_support_and_user_permission" target="_blank">ARKit-kompatibles</a> iOS-Gerät.

[!INCLUDE [Create Spatial Anchors resource](../../../includes/spatial-anchors-get-started-create-resource.md)]

## <a name="open-the-sample-project"></a>Öffnen des Beispielprojekts

Führen Sie über das Terminal die folgenden Aktionen aus.

[!INCLUDE [Clone Sample Repo](../../../includes/spatial-anchors-clone-sample-repository.md)]

Installieren Sie die erforderlichen Pods, indem Sie CocoaPods verwenden:

# <a name="swifttabopenproject-swift"></a>[Swift](#tab/openproject-swift)

Navigieren Sie zu `iOS/Swift/`.

```bash
cd ./iOS/Swift/
```

# <a name="objective-ctabopenproject-objc"></a>[Objective-C](#tab/openproject-objc)

Navigieren Sie zu `iOS/Objective-C/`.

```bash
cd ./iOS/Objective-C/
```

---

Führen Sie `pod install --repo-update` aus, um die CocoaPods für das Projekt zu installieren.

Öffnen Sie nun `.xcworkspace` in Xcode.

> [!NOTE]
> Sehen Sie sich die [Schritte zur Problembehandlung](#cocoapods-issues-on-macos-catalina-1015) an, wenn nach dem Upgrade auf macOS Catalina (10.15) Probleme mit CocoaPod auftreten.

# <a name="swifttabopenproject-swift"></a>[Swift](#tab/openproject-swift)

```bash
open ./SampleSwift.xcworkspace
```

# <a name="objective-ctabopenproject-objc"></a>[Objective-C](#tab/openproject-objc)

```bash
open ./SampleObjC.xcworkspace
```

---

## <a name="configure-account-identifier-and-key"></a>Konfigurieren des Kontobezeichners und -schlüssels

Im nächsten Schritt wird die App zur Verwendung des Kontobezeichners und Kontoschlüssels konfiguriert. Diese haben Sie beim [Einrichten der Spatial Anchors-Ressource](#create-a-spatial-anchors-resource) in einen Text-Editor kopiert.

# <a name="swifttabopenproject-swift"></a>[Swift](#tab/openproject-swift)

Öffnen Sie `iOS/Swift/SampleSwift/ViewControllers/BaseViewController.swift`.

Suchen Sie nach dem Feld `spatialAnchorsAccountKey`, und ersetzen Sie `Set me` durch den Kontoschlüssel.

Suchen Sie nach dem Feld `spatialAnchorsAccountId`, und ersetzen Sie `Set me` durch den Kontobezeichner.

# <a name="objective-ctabopenproject-objc"></a>[Objective-C](#tab/openproject-objc)

Öffnen Sie `iOS/Objective-C/SampleObjC/BaseViewController.m`.

Suchen Sie nach dem Feld `SpatialAnchorsAccountKey`, und ersetzen Sie `Set me` durch den Kontoschlüssel.

Suchen Sie nach dem Feld `SpatialAnchorsAccountId`, und ersetzen Sie `Set me` durch den Kontobezeichner.

---

## <a name="deploy-the-app-to-your-ios-device"></a>Bereitstellen der App auf Ihrem iOS-Gerät

Verbinden Sie das iOS-Gerät mit dem Macintosh-Computer, und legen Sie das **aktive Schema** auf Ihr iOS-Gerät fest.

![Auswählen des Geräts](./media/get-started-ios/select-device.png)

Wählen Sie **Build and then run the current scheme** (Aktuelles Schema erstellen und dann ausführen) aus.

![Bereitstellen und ausführen](./media/get-started-ios/deploy-run.png)

> [!NOTE]
> Wenn Sie den Fehler `library not found for -lPods-SampleObjC` sehen, haben Sie wahrscheinlich die `.xcodeproj`-Datei anstelle der `.xcworkspace`-Datei geöffnet. Öffnen Sie die `.xcworkspace`-Datei, und wiederholen Sie den Vorgang.

Beenden Sie die App in Xcode, indem Sie **Beenden** wählen.

## <a name="troubleshooting"></a>Problembehandlung

### <a name="cocoapods-issues-on-macos-catalina-1015"></a>Cocoapods-Probleme unter macOS Catalina (10.15)

Wenn Sie vor kurzem auf macOS Catalina (10.15) aktualisiert haben und CocoaPods zuvor installiert waren, kann sich CocoaPods in einem fehlerhaften Zustand befinden, sodass Ihre Pods und `.xcworkspace`-Projektdateien möglicherweise nicht ordnungsgemäß konfiguriert werden. Um dieses Problem zu beheben, müssen Sie die CocoaPods neu installieren, indem Sie die folgenden Befehle ausführen:

```shell
brew update
brew install cocoapods --build-from-source
brew link --overwrite cocoapods
```

[!INCLUDE [Clean-up section](../../../includes/clean-up-section-portal.md)]

[!INCLUDE [Next steps](../../../includes/spatial-anchors-quickstarts-nextsteps.md)]

> [!div class="nextstepaction"]
> [Tutorial: Freigeben für Sitzungen und Geräte mit Azure Spatial Anchors](../tutorials/tutorial-share-anchors-across-devices.md)
