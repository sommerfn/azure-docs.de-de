---
title: 'Schnellstart: Erstellen einer Unity HoloLens-App mit Azure Spatial Anchors | Microsoft-Dokumentation'
description: In dieser Schnellstartanleitung wird beschrieben, wie Sie eine HoloLens-App mit Unity erstellen, indem Sie Spatial Anchors verwenden.
author: craigktreasure
manager: aliemami
services: azure-spatial-anchors
ms.author: crtreasu
ms.date: 02/24/2019
ms.topic: quickstart
ms.service: azure-spatial-anchors
ms.openlocfilehash: c29819d817138f2512420584947763247837a9ea
ms.sourcegitcommit: 009334a842d08b1c83ee183b5830092e067f4374
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 05/29/2019
ms.locfileid: "67135206"
---
# <a name="quickstart-create-a-unity-hololens-app-that-uses-azure-spatial-anchors"></a>Schnellstart: Erstellen einer Unity HoloLens-App, die Azure Spatial Anchors verwendet

In dieser Schnellstartanleitung erstellen Sie eine Unity HoloLens-App, die [Azure Spatial Anchors](../overview.md) verwendet. Spatial Anchors ist ein plattformübergreifender Entwicklerdienst, mit dem Sie Mixed Reality-Umgebungen mit Objekten erstellen können, die ihre Position im Zeitverlauf geräteübergreifend beibehalten. Nach Abschluss des Vorgangs verfügen Sie über eine mit Unity erstellte HoloLens-App, mit der ein räumlicher Anker gespeichert und abgerufen werden kann.

Sie lernen Folgendes:

- Erstellen eines Spatial Anchors-Kontos
- Vorbereiten von Unity-Buildeinstellungen
- Konfigurieren des Bezeichners und Schlüssels für das Spatial Anchors-Konto
- Exportieren des HoloLens-Visual Studio-Projekts
- Bereitstellen und Ausführen der App auf einem HoloLens-Gerät

[!INCLUDE [quickstarts-free-trial-note](../../../includes/quickstarts-free-trial-note.md)]

## <a name="prerequisites"></a>Voraussetzungen

So führen Sie diesen Schnellstart durch:


- Sie benötigen einen Windows-Computer, auf dem <a href="https://unity3d.com/get-unity/download" target="_blank">Unity 2018.3</a> oder höher und <a href="https://www.visualstudio.com/downloads/" target="_blank">Visual Studio 2019</a> oder höher installiert sind. Ihre Visual Studio-Installation muss die Workload **Entwicklung für die universelle Windows-Plattform** enthalten. Installieren von <a href="https://git-scm.com/download/win" target="_blank">Git für Windows</a>.
- Sie benötigen ein HoloLens-Gerät, auf dem der [Entwicklermodus](https://docs.microsoft.com/windows/mixed-reality/using-visual-studio) aktiviert ist. Das [Windows 10 October 2018 Update](https://docs.microsoft.com/windows/mixed-reality/release-notes-october-2018) (auch als RS5 bekannt) muss auf dem Gerät installiert sein. Öffnen Sie zum Aktualisieren auf das neueste HoloLens-Release die App **Einstellungen**, navigieren Sie zu **Update und Sicherheit**, und wählen Sie dann **Nach Updates suchen** aus.
- Sie müssen in der App die Funktion **SpatialPerception** aktivieren. Diese Einstellung befindet sich unter **Buildeinstellungen** > **Playereinstellungen** > **Veröffentlichungseinstellungen** > **Funktionen**.
- In der App müssen Sie **Virtual Reality unterstützt** mit **Windows Mixed Reality SDK** aktivieren. Diese Einstellung befindet sich unter **Buildeinstellungen** > **Playereinstellungen** > **XR-Einstellungen**.

[!INCLUDE [Create Spatial Anchors resource](../../../includes/spatial-anchors-get-started-create-resource.md)]

## <a name="download-and-open-the-unity-sample-project"></a>Herunterladen und Öffnen des Unity-Beispielprojekts

[!INCLUDE [Clone Sample Repo](../../../includes/spatial-anchors-clone-sample-repository.md)]

[!INCLUDE [Open Unity Project](../../../includes/spatial-anchors-open-unity-project.md)]

Wählen Sie **Datei** > **Buildeinstellungen** aus, um **Buildeinstellungen** zu öffnen.

Wählen Sie im Abschnitt **Plattform** die Option **Universelle Windows-Plattform**. Ändern Sie das **Zielgerät** zu **HoloLens**.

Wählen Sie **Plattform wechseln**, um die Plattform in **Universelle Windows-Plattform** zu ändern. Unity fordert Sie möglicherweise zur Installation fehlender unterstützender UWP-Komponenten auf.

![Fenster für Unity-Buildeinstellungen](./media/get-started-unity-hololens/unity-build-settings.png)

Schließen Sie das Fenster **Buildeinstellungen**.

## <a name="configure-the-account-identifier-and-key"></a>Konfigurieren des Kontobezeichners und -schlüssels

Navigieren Sie im Bereich **Projekt** zu `Assets/AzureSpatialAnchorsPlugin/Examples`, und öffnen Sie die Szenendatei `AzureSpatialAnchorsBasicDemo.unity`.

[!INCLUDE [Configure Unity Scene](../../../includes/spatial-anchors-unity-configure-scene.md)]

Speichern Sie die Szene, indem Sie **Datei** > **Speichern** wählen.

## <a name="export-the-hololens-visual-studio-project"></a>Exportieren des HoloLens-Visual Studio-Projekts

[!INCLUDE [Export Unity Project](../../../includes/spatial-anchors-unity-export-project-snip.md)]

Wählen Sie **Build** aus. Wählen Sie im Dialogfeld einen Ordner aus, in den das HoloLens-Visual Studio-Projekt exportiert werden soll.

Nach Abschluss des Exports wird ein Ordner angezeigt, der das exportierte HoloLens-Projekt enthält.

## <a name="deploy-the-hololens-application"></a>Bereitstellen der HoloLens-Anwendung

Doppelklicken Sie im Ordner auf **HelloAR U3D.sln**, um das Projekt in Visual Studio zu öffnen.

Ändern Sie die **Projektmappenkonfiguration** zu **Release** und die **Projektmappenplattform** zu **x86**, und wählen Sie für das Bereitstellungsziel die Option **Gerät** aus.

Verwenden Sie bei Einsatz von HoloLens 2 **ARM** als **Projektmappenplattform** anstelle von **x86**.

   ![Visual Studio-Konfiguration](./media/get-started-unity-hololens/visual-studio-configuration.png)

Schalten Sie das HoloLens-Gerät ein, melden Sie sich an, und stellen Sie per USB-Kabel eine Verbindung mit dem PC her.

Wählen Sie **Debuggen** > **Debuggen starten**, um Ihre App bereitzustellen und den Debugvorgang zu starten.

Befolgen Sie in der App die Anleitung zum Anordnen und Abrufen eines Ankers.

Beenden Sie die App in Visual Studio, indem Sie entweder **Debuggen beenden** auswählen oder UMSCHALT+F5 drücken.

[!INCLUDE [Clean-up section](../../../includes/clean-up-section-portal.md)]

[!INCLUDE [Next steps](../../../includes/spatial-anchors-quickstarts-nextsteps.md)]

> [!div class="nextstepaction"]
> [Tutorial: Freigeben von Raumankern für Geräte](../tutorials/tutorial-share-anchors-across-devices.md)
