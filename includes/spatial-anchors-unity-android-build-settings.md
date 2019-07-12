---
author: ramonarguelles
ms.service: azure-spatial-anchors
ms.topic: include
ms.date: 1/29/2019
ms.author: rgarcia
ms.openlocfilehash: 9798e5f76881be38fb27e1f428565caba6e50bf2
ms.sourcegitcommit: 509e1583c3a3dde34c8090d2149d255cb92fe991
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 05/27/2019
ms.locfileid: "67135101"
---
Wählen Sie **Datei** > **Buildeinstellungen** aus, um **Buildeinstellungen** zu öffnen.

Wählen Sie im Abschnitt **Plattform** die Option **Android** aus. Ändern Sie das **Buildsystem** in **Gradle**, und stellen Sie sicher, dass das Kontrollkästchen **Projekt exportieren** kein Häkchen enthält.

Wählen Sie **Plattform wechseln** aus, um die Plattform in **Android** zu ändern. Bei Bedarf werden Sie in Unity zur Installation fehlender unterstützender Android-Komponenten aufgefordert.

![Fenster für Unity-Buildeinstellungen](./media/spatial-anchors-unity/unity-android-build-settings.png)

Schließen Sie das Fenster **Buildeinstellungen**.

### <a name="download-and-import-the-arcore-sdk-for-unity"></a>Herunterladen und Importieren des ARCore SDK für Unity

Laden Sie die Datei `unitypackage` aus den [ARCore SDK-Releases für Unity 1.7](https://github.com/google-ar/arcore-unity-sdk/releases/tag/v1.7.0) herunter. Wählen Sie im Unity-Projekt die Optionen **Ressourcen** > **Paket importieren** > **Benutzerdefiniertes Paket** und anschließend die zuvor heruntergeladene Datei `unitypackage` aus. Vergewissern Sie sich im Dialogfeld zum **Importieren des Unity-Pakets**, dass alle Dateien ausgewählt sind, und wählen Sie dann **Importieren** aus.
