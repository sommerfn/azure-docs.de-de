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
ms.openlocfilehash: fabdd221ef99414eae0156babbd76dedb1f0745d
ms.sourcegitcommit: 824e3d971490b0272e06f2b8b3fe98bbf7bfcb7f
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 10/10/2019
ms.locfileid: "72243019"
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
1. Wechseln Sie in Unity zu **Assets (Ressourcen)**  > **Import Package (Paket importieren)**  > **Custom Package (Benutzerdefiniertes Paket)** .

    ![Die Unity-Optionen zum Importieren von Paketen](media/import-package.png)  

1. Wählen Sie **ProjectAcoustics.unitypackage**.

1. Wählen Sie die Schaltfläche **Importieren** aus, um das Unity-Paket in Ihr Projekt zu integrieren.  
  
    ![Das Unity-Dialogfeld zum Importieren von Paketen](media/import-dialog.png)  

Weitere Schritte und Anmerkungen zum Importieren des Pakets in ein vorhandenes Projekt finden Sie unter [Unity-Integration](unity-integration.md).

>[!NOTE]
>Nach Abschluss des Imports werden im Konsolenprotokoll mehrere Fehlermeldungen angezeigt. Fahren Sie mit dem nächsten Schritt fort, und starten Sie Unity neu.

## <a name="restart-unity"></a>Neustarten von Unity
Für den Bakevorgang des Akustiktoolkits ist die Skriptlaufzeitversion .NET 4.*x* erforderlich. Durch den Paketimport werden die Einstellungen des Unity-Players aktualisiert. Starten Sie Unity neu, damit die Einstellungen übernommen werden. Überprüfen Sie in den Einstellungen des **Players**, ob die Einstellung übernommen wurde.

![Einstellungsmenü des Unity-Projekts](media/player-settings.png)  

![Das Unity-Panel für Playereinstellungen mit der ausgewählten Option „.NET 4.x“](media/net45.png)  

>[!NOTE]
>Dieser Screenshot stammt aus Unity 2018.*x*. Das Bild kann sich in neueren Versionen von Unity unterscheiden.

## <a name="open-the-project-acoustics-bake-window"></a>Öffnen des Bakingfensters in Projekt Akustik
Wählen Sie in Unity im Menü **Window** (Fenster) die Option **Acoustics** (Akustik).

![Unity-Editor mit hervorgehobener Option „Acoustics“ im Menü „Window“](media/window-acoustics.png)

Das unverankerte Fenster **Acoustics** wird geöffnet. In diesem Fenster legen Sie die Eigenschaften der Akustiksimulation fest.

![Der Unity-Editor mit geöffnetem Fenster „Acoustics“ (Akustik)](media/unity-editor-plugin-window.png)  

## <a name="experiment-with-the-design-controls"></a>Experimentieren mit den Entwurfssteuerelementen
Öffnen Sie die Beispielszene im Ordner *ProjectAcousticsSample*, und wählen Sie die Wiedergabeschaltfläche im Unity-Editor aus. Navigieren Sie auf dem Desktop mit den Tasten W, A, S, D und mit der Maus. Um zu vergleichen, wie die Szene mit und ohne Akustik klingt, drücken Sie so lange die Taste R, bis der Überlagerungstext rot wird und „Akustik: Deaktiviert“ lautet. Zum Anzeigen von Tastenkombinationen für weitere Steuerelemente drücken Sie F1. Sie können auch mit der rechten Maustaste klicken, um eine Aktion auszuwählen, und dann mit der linken Maustaste klicken, um diese Aktion auszuführen.

Das Skript *AcousticsAdjust* ist an die Soundquellen in der Beispielszene angefügt. Es aktiviert die Designparameter pro Quelle.

![Das Unity-Skript „AcousticsAdjust“](media/acoustics-adjust.png)

In den folgenden Abschnitten werden einige der Effekte erläutert, die Sie mithilfe der verfügbaren Steuerelemente erstellen können. Ausführliche Informationen zu den einzelnen Steuerelementen finden Sie im [Unity-Entwurfstutorial für Projekt Akustik](unity-workflow.md).

### <a name="modify-distance-based-attenuation"></a>Ändern der distanzabhängigen Dämpfung
Die vom Unity-Spatializer-Plug-In von **Projekt Akustik** bereitgestellte digitale Signalverarbeitung (DSP) für Audio berücksichtigt die in den Unity-Editor integrierte distanzabhängige Dämpfung pro Quelle. Die Steuerelemente für die distanzabhängige Dämpfung befinden sich in der Komponente **Audioquelle** im Bereich **Inspektor** von Tonquellen unter **3D Sound Settings** (3D-Soundeinstellungen):

![Das Unity-Panel mit Optionen für die Distanzdämpfung](media/distance-attenuation.png)

Projekt Akustik führt Berechnungen in einem „Simulationsbereichsfeld“ durch, das um die Position des Players herum angeordnet ist. Für die Akustikressourcen im Musterpaket wurde ein Baking in einer Simulationsregion von 45 Metern um den Player herum ausgeführt. Daher sollte die Schallschwächung so gestaltet werden, dass Sie bei ungefähr 45 m auf 0 fällt.

### <a name="modify-occlusion-and-transmission"></a>Ändern der Verdeckung und Übertragung
* Wenn der Multiplikator **Occlusion** (Verdeckung) größer als 1 ist (der Standardwert ist 1), tritt eine übertriebene Verdeckung auf. Bei einer Festlegung unter 1 ist der Verdeckungseffekt geringer.

* Um die Übertragung durch die Wand zu aktivieren, schieben Sie den Schieberegler **Transmission (dB)** (Übertragung (dB)) von der untersten Stufe nach oben.

### <a name="modify-wetness-for-a-source"></a>Anpassen der Verwässerung für eine Quelle
* Um anzupassen, wie stark die Verwässerung mit der Entfernung zunimmt, verwenden Sie **Perceptual Distance Warp** (Verzerrung für Wahrnehmungsentfernung). Projekt Akustik berechnet durch Simulation Verwässerungspegel, die wahrnehmbare Entfernungshinweise liefern und mit der Entfernung stufenlos variieren. Durch Erhöhen der Entfernungsverzerrung wird dieser Effekt intensiviert, da die Entfernungsverwässerung erhöht wird. Verzerrungswerte kleiner als 1 verringern den entfernungsbasierten Hall.

   Ändern Sie die Einstellung **Wetness (DB)** (Verwässerung (DB)), um eine präzisere Anpassung dieses Effekts vorzunehmen.

* Erhöhen Sie die Verfallszeit im gesamten Raum durch Anpassen von **Decay Time Scale** (Verfallszeitskala). Wenn das Simulationsergebnis für ein bestimmtes Quelle-Zuhörer-Positionspaar eine Verfallszeit von 1,5  Sekunden und **Decay Time Scale** (Verfallszeitskala) auf 2 festgelegt ist, wird eine Verfallszeit von 3  Sekunden auf die Quelle angewendet.

## <a name="next-steps"></a>Nächste Schritte
* Lesen Sie Details zu den [Unity-basierten Entwurfssteuerelementen für Projekt Akustik](unity-workflow.md).
* Erkunden Sie die Konzepte hinter dem [Entwurfsvorgang](design-process.md) weiter.
* [Erstellen Sie ein Azure-Konto](create-azure-account.md), um die Vorabbaking- und Bakingvorgänge zu erkunden.
