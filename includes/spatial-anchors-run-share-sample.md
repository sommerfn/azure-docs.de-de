---
author: ramonarguelles
ms.service: spatial-anchors
ms.topic: include
ms.date: 1/30/2019
ms.author: rgarcia
ms.openlocfilehash: ec8fb6efab126dcf5556a9abfdf58d1fd69d4212
ms.sourcegitcommit: 7efb2a638153c22c93a5053c3c6db8b15d072949
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 10/24/2019
ms.locfileid: "72882315"
---
## <a name="androidtabandroid"></a>[Android](#tab/Android)

Das Java-Android-Beispiel unterstützt die geräteübergreifende Freigabe.
Öffnen Sie die Datei `SharedActivity.java` aus dem Ordner mit den Beispielen in Android Studio. Geben Sie die URL, die Sie im vorherigen Schritt (aus der Azure-Bereitstellung Ihrer ASP.NET-Web-App) abgerufen haben, in der Datei `SharedActivity.java` als Wert für `SharingAnchorsServiceUrl` ein. Ersetzen Sie `index.html` in der URL durch `api/anchors`. Diese sollte wie folgt aussehen: `https://<app_name>.azurewebsites.net/api/anchors`.

[!INCLUDE [Run shared sample](spatial-anchors-run-sample.md)]

## <a name="iostabios"></a>[iOS](#tab/iOS)

Das Objective-C-iOS-Beispiel unterstützt die geräteübergreifende Freigabe.
Öffnen Sie die Datei `SharedDemoViewController.m` im Ordner mit den Beispielen. Geben Sie die URL, die Sie im vorherigen Schritt (aus der Azure-Bereitstellung Ihrer ASP.NET-Web-App) abgerufen haben, in der Datei `SharedActivity.java` als Wert für `SharingAnchorsServiceUrl` ein. Ersetzen Sie `index.html` in der URL durch `api/anchors`. Diese sollte wie folgt aussehen: `https://<app_name>.azurewebsites.net/api/anchors`.

[!INCLUDE [Run shared sample](spatial-anchors-run-sample.md)]

## <a name="xamarintabxamarin"></a>[Xamarin](#tab/Xamarin)

Die Android- und iOS-Beispiele für Xamarin unterstützen die geräteübergreifende Freigabe.
Öffnen Sie die Datei `AccountDetails.cs` im Ordner mit den Beispielen. Geben Sie die URL, die Sie im vorherigen Schritt (aus der Azure-Bereitstellung Ihrer ASP.NET-Web-App) abgerufen haben, in der Datei `SharedActivity.java` als Wert für `AnchorSharingServiceUrl` ein. Ersetzen Sie `index.html` in der URL durch `api/anchors`. Diese sollte wie folgt aussehen: `https://<app_name>.azurewebsites.net/api/anchors`.

[!INCLUDE [Run shared sample](spatial-anchors-run-sample.md)]

## <a name="unitytabunity"></a>[Unity](#tab/Unity)

[!INCLUDE [Open Unity Project](spatial-anchors-open-unity-project.md)]

### <a name="set-up-an-android-device"></a>Einrichten eines Android-Geräts

[!INCLUDE [Android Unity Build Settings](spatial-anchors-unity-android-build-settings.md)]

### <a name="set-up-an-ios-device"></a>Einrichten eines iOS-Geräts

[!INCLUDE [iOS Unity Build Settings](spatial-anchors-unity-ios-build-settings.md)]

## <a name="configure-the-account-identifier-and-key"></a>Konfigurieren des Kontobezeichners und -schlüssels

Navigieren Sie im Bereich **Projekt** zu `Assets/AzureSpatialAnchorsPlugin/Examples`, und öffnen Sie die Szenendatei `AzureSpatialAnchorsLocalSharedDemo.unity`.

[!INCLUDE [Configure Unity Scene](spatial-anchors-unity-configure-scene.md)]

Navigieren Sie im Bereich **Projekt** zu `Assets\AzureSpatialAnchors.Examples\Resources`. Wählen Sie `SpatialAnchorSamplesConfig` aus. Geben Sie ebenfalls im Bereich **Inspector** den Wert für `Sharing Anchors Service url` (aus der Azure-Bereitstellung Ihrer ASP.NET-Web-App) als Wert für `Base Sharing Url` ein, und ersetzen Sie `index.html` durch `api/anchors`. Diese sollte wie folgt aussehen: `https://<app_name>.azurewebsites.net/api/anchors`.

Speichern Sie die Szene, indem Sie **Datei** > **Speichern** wählen.

## <a name="deploy-to-your-device"></a>Bereitstellen auf Ihrem Gerät

### <a name="deploy-to-android-device"></a>Bereitstellen auf einem Android-Gerät

Melden Sie sich auf Ihrem Android-Gerät an, und verbinden Sie es mit einem USB-Kabel mit Ihrem Computer.

Wählen Sie **Datei** > **Buildeinstellungen** aus, um **Buildeinstellungen** zu öffnen.

Stellen Sie unter **Scenes In Build** (Szenen im Build) sicher, dass sich neben allen Szenen ein Häkchen befindet.

Stellen Sie sicher, dass für **Projekt exportieren** kein Häkchen gesetzt ist. Wählen Sie **Erstellen und ausführen** aus. Sie werden aufgefordert, die Datei vom Typ `.apk` zu speichern. Sie können einen beliebigen Namen auswählen.

[!INCLUDE [Run shared sample](spatial-anchors-run-sample.md)]

### <a name="deploy-to-an-ios-device"></a>Bereitstellen auf einem iOS-Gerät

Wählen Sie **Datei** > **Buildeinstellungen** aus, um **Buildeinstellungen** zu öffnen.

Stellen Sie unter **Scenes In Build** (Szenen im Build) sicher, dass sich neben allen Szenen ein Häkchen befindet.

[!INCLUDE [Configure Xcode](spatial-anchors-unity-ios-xcode.md)]

[!INCLUDE [Run shared sample](spatial-anchors-run-sample.md)]

Beenden Sie die App in Xcode, indem Sie **Beenden** auswählen.
