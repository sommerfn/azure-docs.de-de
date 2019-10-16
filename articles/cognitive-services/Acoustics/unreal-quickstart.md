---
title: Schnellstart für Projekt Akustik mit Unreal
titlesuffix: Azure Cognitive Services
description: Experimentieren Sie mithilfe von Beispielinhalten mit Entwurfssteuerelementen von Projekt Akustik in Unreal und Wwise, und führen Sie eine Bereitstellung auf dem Windows-Desktop aus.
services: cognitive-services
author: NoelCross
manager: nitinme
ms.service: cognitive-services
ms.subservice: acoustics
ms.topic: quickstart
ms.date: 03/20/2019
ms.author: noelc
ROBOTS: NOINDEX
ms.openlocfilehash: d3afcded894f72626a4f24bcbe85c34ac1329c29
ms.sourcegitcommit: 824e3d971490b0272e06f2b8b3fe98bbf7bfcb7f
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 10/10/2019
ms.locfileid: "72242920"
---
# <a name="project-acoustics-unrealwwise-quickstart"></a>Schnellstart für Projekt Akustik mit Unreal/Wwise
In diesem Schnellstart experimentieren Sie mit Entwurfssteuerelementen von Projekt Akustik mithilfe der bereitgestellten Beispielinhalte für die Unreal Engine und Wwise.

Softwareanforderungen für die Verwendung der Beispielinhalte:
* [Unreal Engine](https://www.unrealengine.com/) 4.22
* [AudioKinetic Wwise](https://www.audiokinetic.com/products/wwise/) 2019.1.2

## <a name="download-the-sample-package"></a>Herunterladen des Beispielpakets
Laden Sie das [Beispielpaket für Unreal und Wwise für Projekt Akustik](https://www.microsoft.com/download/details.aspx?id=58090) herunter. Das Beispielpaket enthält:
- Unreal Engine-Projekt
- Wwise-Projekt für das Unreal-Projekt
- Projekt Akustik-Plug-In für Wwise

## <a name="set-up-the-project-acoustics-sample-project"></a>Einrichten des Projekt Akustik-Beispielprojekts
Installieren Sie zunächst das Projekt Akustik-Plug-In in Wwise. Stellen Sie als nächstes die Wwise-Binärdateien für das Unreal-Projekt bereit. Passen Sie dann das Wwise-Plug-an, um Projekt Akustik zu unterstützen.

### <a name="install-the-project-acoustics-wwise-plug-in"></a>Installieren des Wwise-Plug-Ins für Projekt Akustik
Öffnen Sie das Wwise-Start Programm. Wählen Sie auf der Registerkarte **Plugins** (Plug-Ins) unter **Install New Plugins** (Neue Plug-Ins installieren) die Option **Add From Directory** (Aus Verzeichnis hinzufügen) aus. Wählen Sie das Verzeichnis *AcousticsWwisePlugin\ProjectAcoustics*, das in dem Paket enthalten war, das Sie heruntergeladen haben.

![Die Option zum Installieren des Wwise-Plug-Ins im Wwise-Startprogramm](media/wwise-install-new-plugin.png)

### <a name="add-wwise-binaries-to-the-project-acoustics-unreal-sample-project"></a>Hinzufügen der Wwise-Binärdateien zum Unreal-Beispielprojekt für Projekt Akustik
1. Wählen Sie im Wwise-Startprogramm die Registerkarte **Unreal Engine** aus. 
1. Wählen Sie das Hamburger-Menü (Symbol) neben **Recent Unreal Engine Projects** (Letzte Unreal-Engine-Projekte) aus, und wählen Sie dann **Browse for project** (Nach Projekt suchen) aus. Öffnen Sie die *UPROJECT*-Beispieldatei für das Unreal-Projekt im Paket *AcousticsSample\AcousticsGame\AcousticsGame.uproject*.

   ![Die Registerkarte „Unreal“ im Wwise-Startprogramm](media/wwise-unreal-tab.png)

3. Wählen Sie die Option **Integrate Wwise in Project** (Wwise in das Projekt integrieren) neben dem Projekt Akustik-Beispielprojekt aus.

   ![Das Wwise-Startprogramm zeigt das Unreal-Akustikspielprojekt mit der hervorgehobenen Integrationsoption an.](media/wwise-acoustics-game-project.png)

### <a name="extend-wwise-unreal-plug-in-functionality"></a>Erweitern der Funktionen des Unreal-Plug-Ins von Wwise
Für das Unreal-Plug-In für Projekt Akustik müssen zusätzliche Funktionen aus der Unreal-Plug-In-API für Wwise verfügbar gemacht werden. Führen Sie die im Unreal-Plug-In für Projekt Akustik bereitgestellte Batchdatei aus, um diese Änderungen zu automatisieren.
* Führen Sie in *AcousticsGame\Plugins\ProjectAcoustics\Resources* die Datei *PatchWwise.bat* aus.

    ![Das Skript zum Patchen des Wwise-Projekts, hervorgehoben in einem Windows-Explorer-Fenster](media/patch-wwise-script.png)

* Wenn das DirectX SDK nicht installiert ist: Je nach der verwendeten Version von Wwise müssen Sie ggf. die Zeile mit `DXSDK_DIR` in *AcousticsGame\Plugins\Wwise\Source\AkAudio\AkAudio.Build.cs* auskommentieren:

    ![Code-Editor mit der auskommentierten DXSDK-Zeile](media/directx-sdk-comment.png)

* Beim Kompilieren mit Visual Studio 2019: Zum Umgehen eines Verknüpfungsfehlers mit Wwise ändern Sie manuell den Standardwert *VSVersion* in *AcousticsGame\Plugins\Wwise\Source\AkAudio\AkAudio.Build.cs* in den Wert **vc150**:

    ![Code-Editor mit Änderung von „VSVersion“ in „vc150“](media/vsversion-comment.png)

### <a name="open-the-unreal-project"></a>Öffnen des Unreal-Projekts 
Wenn Sie das Unreal-Projekt öffnen, werden Sie aufgefordert, Module neu zu erstellen. Wählen Sie **Ja** aus.

Wenn das Projekts aufgrund von Buildfehlern nicht geöffnet werden kann, vergewissern Sie sich, dass Sie das Wwise-Plug-In für Projekt Akustik in derselben Wwise-Version installiert haben, die auch im Projekt Akustik-Beispielprojekt verwendet wurde.

Wenn Sie eine frühere [AudioKinetic Wwise](https://www.audiokinetic.com/products/wwise/)-Version als 2019.1 verwenden, können Sie mit dem Projekt Akustik-Beispielprojekt keine Soundbanken erstellen. Sie müssen Wwise Version 2019.1 in das Beispielprojekt integrieren.

## <a name="experiment-with-project-acoustics-design-controls"></a>Experimentieren mit den Entwurfssteuerelementen von Projekt Akustik
Hören Sie sich die Szene an, indem Sie im Unity-Editor die Wiedergabeschaltfläche auswählen. Navigieren Sie auf dem Desktop mit den Tasten W, A, S, D und mit der Maus. Zum Anzeigen von Tastenkombinationen für zusätzliche Steuerelemente drücken Sie F1.

Die folgenden Informationen beschreiben einige Entwurfsaktivitäten, die Sie ausprobieren können.

### <a name="modify-occlusion-and-transmission"></a>Ändern der Verdeckung und Übertragung
Für jeden Unreal-Soundakteur gibt es quellenbezogene Entwurfssteuerelemente in Projekt Akustik.

![Die Akustik-Entwurfssteuerelemente im Unreal-Editor](media/demo-scene-sound-source-design-controls.png)

Wenn der Multiplikator **Occlusion** (Verdeckung) größer als 1 ist (der Standardwert ist 1), tritt eine übertriebene Verdeckung auf. Bei einer Festlegung unter 1 ist der Verdeckungseffekt geringer.

Um die Übertragung durch die Wand zu aktivieren, schieben Sie den Schieberegler **Transmission (dB)** (Übertragung (dB)) von der untersten Stufe nach oben.

### <a name="modify-wetness-for-a-source"></a>Anpassen der Verwässerung für eine Quelle
Um anzupassen, wie stark die Verwässerung mit der Entfernung zunimmt, verwenden Sie **Perceptual Distance Warp** (Verzerrung für Wahrnehmungsentfernung). Projekt Akustik berechnet Verwässerungspegel im gesamten Raum durch Simulation. Die Pegel variieren nahtlos mit der Entfernung und liefern wahrnehmbare Entfernungshinweise. Zum Intensivieren dieses Effekts erhöhen Sie die Entfernungsverzerrung, um die Entfernungsverwässerung zu erhöhen. Verzerrungswerte kleiner als 1 verringern den entfernungsbasierten Hall. Sie können diesen Effekt auch mit der Einstellung **Wetness (dB)** (Verwässerung (dB)) feiner abstufen.

Erhöhen Sie die Verfallszeit im gesamten Raum durch Anpassen von **Decay Time Scale** (Verfallszeitskala). Angenommen, das Simulationsergebnis ist eine Verfallszeit von 1,5 Sekunden. Durch Festlegen von **Decay Time Scale** (Verfallszeitskala) auf 2 wird eine Verfallszeit von 3 Sekunden auf die Quelle angewendet.

### <a name="modify-distance-based-attenuation"></a>Ändern der distanzabhängigen Dämpfung
Das Wwise-Mixer-Plug-In für Projekt Akustik berücksichtigt die in Wwise integrierte distanzabhängige Dämpfung pro Quelle. Durch das Ändern dieser Kurve wird der Pegel der Trockenstrecke geändert. Das Projekt Akustik-Plug-In passt den Verwässerungspegel so an, dass die durch die Simulation und die Entwurfssteuerelemente festgelegte Verwässerungs-Trockenheits-Mischung aufrechterhalten bleibt.

![Bereich mit der Wwise-Dämpfungskurve mit Dämpfung gegen 0 vor Simulationsgrenze](media/demo-sounds-attenuation.png)

Projekt Akustik führt Berechnungen in einem „Simulationsbereichsfeld“ durch, das um die Position jedes simulierten Players herum angeordnet ist. Für die Akustikressourcen im Musterpaket wurde ein Baking mit einem Simulationsregionsradius von 45 Metern ausgeführt. Dämpfungen sind so ausgelegt, dass sie vor 45 Metern auf 0 sinken. Dieser Abfall ist nicht zwingend erforderlich und weist den Nachteil auf, dass nur in der Geometrie innerhalb von 45 Meter um den Zuhörer herum Töne verdeckt werden.

## <a name="next-steps"></a>Nächste Schritte
* [Integrieren Sie das Projekt Akustik](unreal-integration.md)-Plug-In in Ihr Unreal-Projekt.
* [Erstellen Sie ein Azure-Konto](create-azure-account.md) für eigene Bake-Vorgänge.
