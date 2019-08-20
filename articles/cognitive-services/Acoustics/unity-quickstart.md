---
title: Schnellstart für Projekt Akustik mit Unity
titlesuffix: Azure Cognitive Services
description: Experimentieren Sie mithilfe von Beispielinhalten mit Entwurfssteuerelementen von Projekt Akustik in Unity, und führen Sie eine Bereitstellung auf dem Windows-Desktop aus.
services: cognitive-services
author: NoelCross
manager: nitinme
ms.service: cognitive-services
ms.subservice: acoustics
ms.topic: quickstart
ms.date: 03/20/2019
ms.author: noelc
ROBOTS: NOINDEX
ms.openlocfilehash: 0ea020ca76381a4ae5d6b6e480c94e63f9aa2dab
ms.sourcegitcommit: 13a289ba57cfae728831e6d38b7f82dae165e59d
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 08/09/2019
ms.locfileid: "68933135"
---
# <a name="project-acoustics-unity-quickstart"></a>Schnellstart für Projekt Akustik mit Unity
Verwenden Sie die Projekt Akustik-Beispielinhalte für Unity zum Experimentieren mit simulationsgestützten Entwurfssteuerelementen.

Softwareanforderungen:
* [Unity 2018.2+](https://unity3d.com) für Windows
* [Projekt Akustik-Beispielinhaltspaket](https://www.microsoft.com/download/details.aspx?id=57346)

Was ist im Beispielpaket enthalten?
* Unity-Szene mit Geometrie, Tonquellen und Spielsteuerelementen
* Projekt Akustik-Plug-In 
* Akustikressourcen für die Szene mit ausgeführtem Baking

## <a name="import-the-sample-package"></a>Importieren des Beispielpakets
Importieren Sie das Beispielpaket in ein neues Unity-Projekt. 
* Wechseln Sie in Unity zu **Assets (Medienobjekte) > Import Package (Paket importieren) > Custom Package... (Benutzerdefiniertes Paket...)** .

    ![Screenshot der Unity-Option zum Importieren von Paketen](media/import-package.png)  

* Wählen Sie **ProjectAcoustics.unitypackage** aus.

* Klicken Sie auf die Schaltfläche **Importieren**, um das Unity-Paket in Ihr Projekt zu integrieren.  
  
    ![Screenshot des Dialogfelds zum Importieren des Unity-Pakets](media/import-dialog.png)  

Wenn Sie das Paket in ein vorhandenes Projekt importieren, finden Sie weitere Schritte und Anmerkungen unter [Unity-Integration](unity-integration.md).

>[!NOTE] 
>Nach Abschluss des Imports werden im Konsolenprotokoll einige Fehler angezeigt.  Fahren Sie mit dem nächsten Schritt fort, und starten Sie Unity neu.

## <a name="restart-unity"></a>Neustarten von Unity
Für den Bakevorgang des Akustiktoolkits ist die Skriptlaufzeitversion .NET 4.x erforderlich. Durch den Paketimport werden die Einstellungen des Unity-Players aktualisiert. Starten Sie Unity neu, damit die Einstellungen übernommen werden.

Sie können überprüfen, ob diese Einstellung wirksam war, indem Sie die **Playereinstellungen** öffnen:

![Screenshot des Unity-Panels für Playereinstellungen](media/player-settings.png)  

![Screenshot des Unity-Panels für Playereinstellungen mit der ausgewählten Option „.NET 4.5“](media/net45.png)  

>[!NOTE]
>Der Screenshot stammt aus Unity 2018.x. Neuere Versionen von Unity unterscheiden sich möglicherweise.

## <a name="open-the-project-acoustics-bake-window"></a>Öffnen des Bakingfensters in Projekt Akustik
Wählen Sie im Unity-Menü **Window > Acoustics** (Fenster > Akustik) aus:

![Screenshot des Unity-Editors, in dem im Menü „Window“ (Fenster) die Option „Acoustics“ (Akustik) hervorgehoben ist](media/window-acoustics.png)

Ein neues unverankertes Fenster namens **Acoustics** (Akustik) wird geöffnet.  In diesem Fenster werden die Eigenschaften der Akustiksimulation festgelegt.

![Screenshot des Unity-Editors mit geöffnetem Fenster „Acoustics“ (Akustik)](media/unity-editor-plugin-window.png)  

## <a name="experiment-with-design-controls"></a>Experimentieren mit den Entwurfssteuerelementen
Öffnen Sie die Beispielszene im Ordner **ProjectAcousticsSample**, und klicken Sie auf die Wiedergabeschaltfläche im Unity-Editor. Navigieren Sie mit den Tasten W, A, S und D sowie mit der Maus. Um zu vergleichen, wie die Szene mit und ohne Akustik klingt, drücken Sie so lange die Taste **R**, bis der Überlagerungstext rot wird und „Akustik: Deaktiviert“ lautet. Um Tastenkombinationen für weitere Steuerelemente anzuzeigen, drücken Sie **F1**. Die Steuerelemente können auch verwendet werden, indem Sie mit der rechten Maustaste auf sie klicken, um die auszuführende Aktion auszuwählen, und dann durch Klicken mit der linken Maustaste die Aktion ausführen.

Das Skript **AcousticsAdjust** ist an die Tonquellen in der Beispielszene angefügt. Es aktiviert die Entwurfsparameter pro Quelle. 

![Screenshot des Unity-Skripts „AcousticsAdjust“](media/acoustics-adjust.png)

Im Folgenden werden einige der Effekte, die mit den bereitgestellten Steuerelementen erstellt werden können, untersucht. Ausführliche Informationen zu den einzelnen Steuerelementen finden Sie im [Unity-Entwurfstutorial für Projekt Akustik](unity-workflow.md).

### <a name="modify-distance-based-attenuation"></a>Ändern der distanzabhängigen Dämpfung
Der vom Unity-Spatializer-Plug-In von **Projekt Akustik** bereitgestellte Audio-DSP berücksichtigt die in den Unity-Editor integrierte distanzabhängige Dämpfung pro Quelle. Steuerelemente für die distanzabhängige Dämpfung befinden sich in der Komponente **Audioquelle** im Bereich **Inspektor** von Tonquellen unter **3D Sound Settings** (3D-Soundeinstellungen):

![Screenshot des Unity-Panels mit Optionen für die Distanzdämpfung](media/distance-attenuation.png)

Projekt Akustik führt die Berechnung in einem „Simulationsbereichsfeld“ durch, das um die Position des Spielers herum angeordnet ist. Da das Baking der Akustikressourcen im Beispielpaket mit einer Simulationsbereichsgröße von 45 m um den Spieler herum durchgeführt wurde, sollte die Tondämpfung so festgelegt werden, dass sie bei ca. 45 m auf 0 fällt.

### <a name="modify-occlusion-and-transmission"></a>Ändern der Verdeckung und Übertragung
* Wenn der Multiplikator **Occlusion** (Verdeckung) größer als 1 ist (der Standardwert ist 1), tritt eine übertriebene Verdeckung auf. Bei einer Festlegung unter 1 ist der Verdeckungseffekt geringer.

* Um die Übertragung durch die Wand zu aktivieren, schieben Sie den Schieberegler **Transmission (dB)** (Übertragung (dB)) von der untersten Stufe nach oben. 

### <a name="modify-wetness-for-a-source"></a>Anpassen der Verwässerung für eine Quelle
* Um anzupassen, wie stark die Verwässerung mit der Entfernung zunimmt, verwenden Sie **Perceptual Distance Warp** (Verzerrung für Wahrnungsentfernung). **Projekt Akustik** berechnet Verwässerungspegel im gesamten simulierten Raum, die mit der Entfernung stufenlos variieren und wahrnehmbare Entfernungshinweise liefern. Durch Erhöhen der Entfernungsverzerrung wird dieser Effekt intensiviert, da die Entfernungsverwässerung erhöht wird. Verzerrungswerte kleiner als 1 verringern den entfernungsbasierten Hall. Dieser Effekt kann mit **Wetness (dB)** (Verwässerung (dB)) auch feiner abgestuft angepasst werden.

* Erhöhen Sie die Verfallszeit im gesamten Raum durch Anpassen von **Decay Time Scale** (Verfallszeitskala). Wenn das Simulationsergebnis für ein bestimmtes Quelle-Zuhörer-Positionspaar eine Verfallszeit von 1,5 s ergibt und **Decay Time Scale** (Verfallszeitskala) auf 2 festgelegt ist, wird eine Verfallszeit von 3 s auf die Quelle angewandt.

## <a name="next-steps"></a>Nächste Schritte
* Lesen Sie vollständige Details zu den [Unity-basierten Entwurfssteuerelementen für Projekt Akustik](unity-workflow.md).
* Erkunden Sie die Konzepte hinter dem [Entwurfsvorgang](design-process.md) weiter.
* [Erstellen Sie ein Azure-Konto](create-azure-account.md), um die Vorabbaking- und Bakingvorgänge zu erkunden.

