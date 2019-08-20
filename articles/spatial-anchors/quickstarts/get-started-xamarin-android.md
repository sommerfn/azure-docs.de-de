---
title: 'Schnellstart: Erstellen einer Xamarin-Android-App mit Azure Spatial Anchors | Microsoft-Dokumentation'
description: In dieser Schnellstartanleitung erfahren Sie, wie Sie unter Verwendung von Spatial Anchors und Xamarin eine Android-App erstellen.
author: craigktreasure
manager: virivera
services: azure-spatial-anchors
ms.author: crtreasu
ms.date: 07/22/2019
ms.topic: quickstart
ms.service: azure-spatial-anchors
ms.openlocfilehash: 0958961f1e66be130dd2be816f6002dd34465dc6
ms.sourcegitcommit: 13a289ba57cfae728831e6d38b7f82dae165e59d
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 08/09/2019
ms.locfileid: "68931421"
---
# <a name="quickstart-create-a-xamarin-android-app-with-azure-spatial-anchors"></a>Schnellstart: Erstellen einer Xamarin-Android-App mit Azure Spatial Anchors

In dieser Schnellstartanleitung wird beschrieben, wie Sie mithilfe von [Azure Spatial Anchors](../overview.md) eine Xamarin-Android-App erstellen. Azure Spatial Anchors ist ein plattformübergreifender Entwicklerdienst, mit dem Sie Mixed Reality-Umgebungen mit Objekten erstellen können, die ihre Position im Zeitverlauf geräteübergreifend beibehalten. Nach Abschluss dieser Anleitung verfügen Sie über eine Android-App, die einen räumlichen Anker speichern und abrufen kann.

Sie lernen Folgendes:

> [!div class="checklist"]
> * Erstellen eines Spatial Anchors-Kontos
> * Konfigurieren des Bezeichners und Kontoschlüssels für das Spatial Anchors-Konto
> * Bereitstellen und Ausführen auf einem Android-Gerät

[!INCLUDE [quickstarts-free-trial-note](../../../includes/quickstarts-free-trial-note.md)]

## <a name="prerequisites"></a>Voraussetzungen

Stellen Sie für diese Schnellstartanleitung sicher, dass Sie über Folgendes verfügen:
- Einen Windows- oder macOS-Computer:
  - Bei Verwendung von Windows:
    - Eine aktuelle Version von <a href="https://www.visualstudio.com/downloads/" target="_blank">Visual Studio 2019 16.2 oder höher</a>.
    - <a href="https://git-scm.com/download/win" target="_blank">Git für Windows</a>
  - Bei Verwendung von macOS:
    - Eine aktuelle Version von <a href="https://docs.microsoft.com/visualstudio/mac/installation?view=vsmac-2019" target="_blank">Visual Studio für Mac 8.1 oder höher</a>.
    - <a href="https://git-scm.com/download/mac" target="_blank">Git für macOS</a>
- Die neueste Version von Xamarin.Android, installiert und ausgeführt auf einer Plattform Ihrer Wahl. Eine Anleitung zum Installieren von Xamarin.Android finden Sie im [Installationsleitfaden zu Xamarin.Android](https://docs.microsoft.com/xamarin/android/get-started/installation/index).
- Ein <a href="https://developer.android.com/studio/debug/dev-options" target="_blank">für Entwickler geeignetes</a> und <a href="https://developers.google.com/ar/discover/supported-devices" target="_blank">ARCore-fähiges</a> Android-Gerät.
  - Möglicherweise sind zusätzliche Gerätetreiber erforderlich, damit Ihr Computer mit Ihrem Android-Gerät kommunizieren kann. Weitere Informationen finden Sie [hier](https://developer.android.com/studio/run/device.html).
- Ihre App muss für ARCore **1.8** geeignet sein.

[!INCLUDE [Create Spatial Anchors resource](../../../includes/spatial-anchors-get-started-create-resource.md)]

## <a name="open-the-sample-project"></a>Öffnen des Beispielprojekts

[!INCLUDE [Clone Sample Repo](../../../includes/spatial-anchors-clone-sample-repository.md)]

Öffnen Sie `Xamarin/SampleXamarin.sln` in Visual Studio.

## <a name="configure-account-identifier-and-key"></a>Konfigurieren des Kontobezeichners und -schlüssels

Im nächsten Schritt wird die App zur Verwendung des Kontobezeichners und Kontoschlüssels konfiguriert. Diese haben Sie beim [Einrichten der Spatial Anchors-Ressource](#create-a-spatial-anchors-resource) in einen Text-Editor kopiert.

Öffnen Sie `Xamarin/SampleXamarin.Common/AccountDetails.cs`.

Suchen Sie nach dem Feld `SpatialAnchorsAccountKey`, und ersetzen Sie `Set me` durch den Kontoschlüssel.

Suchen Sie nach dem Feld `SpatialAnchorsAccountId`, und ersetzen Sie `Set me` durch den Kontobezeichner.

## <a name="deploy-the-app-to-your-android-device"></a>Bereitstellen der App auf Ihrem Android-Gerät

Schalten Sie das Android-Gerät ein, melden Sie sich an, und verbinden Sie es per USB-Kabel mit dem Computer.

Legen Sie das Startprojekt als **SampleXamarin.Android** fest, ändern Sie die **Projektmappenkonfiguration** in **Release**, und wählen Sie in der Dropdownliste der Geräteauswahl das Gerät aus, auf dem die Bereitstellung erfolgen soll.

# <a name="windowstabdeploy-windows"></a>[Windows](#tab/deploy-windows)

![Visual Studio-Konfiguration](./media/get-started-xamarin-android/visual-studio-windows-configuration.png)

Wählen Sie **Debuggen** > **Debuggen starten** aus, um Ihre App bereitzustellen und zu starten.

# <a name="macostabdeploy-macos"></a>[macOS](#tab/deploy-macos)

![Visual Studio-Konfiguration](./media/get-started-xamarin-android/visual-studio-macos-configuration.jpg)

Wählen Sie **Ausführen** > **Ohne Debuggen starten** aus, um Ihre App bereitzustellen und zu starten.

---

Wählen Sie in der App **Basic** aus, um die Demo auszuführen, und befolgen Sie die Anweisungen zum Platzieren und Widerrufen eines Ankers.

> ![Screenshot 1](./media/get-started-xamarin-android/screenshot-1.jpg)
> ![Screenshot 2](./media/get-started-xamarin-android/screenshot-2.jpg)
> ![Screenshot 3](./media/get-started-xamarin-android/screenshot-3.jpg)

[!INCLUDE [Clean-up section](../../../includes/clean-up-section-portal.md)]

[!INCLUDE [Next steps](../../../includes/spatial-anchors-quickstarts-nextsteps.md)]

> [!div class="nextstepaction"]
> [Tutorial: Freigeben für Sitzungen und Geräte mit Azure Spatial Anchors](../tutorials/tutorial-share-anchors-across-devices.md)
