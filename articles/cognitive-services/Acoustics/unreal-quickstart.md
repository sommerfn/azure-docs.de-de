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
ms.openlocfilehash: 927ce403130460c302f546038ff3a0c3a16e0368
ms.sourcegitcommit: 13a289ba57cfae728831e6d38b7f82dae165e59d
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 08/09/2019
ms.locfileid: "68933015"
---
# <a name="project-acoustics-unrealwwise-quickstart"></a>Schnellstart für Projekt Akustik mit Unreal/Wwise
In diesem Schnellstart experimentieren Sie mit Entwurfssteuerelementen von Projekt Akustik mithilfe der bereitgestellten Beispielinhalte für die Unreal Engine und Wwise.

Softwareanforderungen für die Verwendung der Beispielinhalte:
* [Unreal Engine](https://www.unrealengine.com/) 4.22
* [AudioKinetic Wwise](https://www.audiokinetic.com/products/wwise/) 2019.1.2

## <a name="download-the-sample-package"></a>Herunterladen des Beispielpakets
Laden Sie das [Beispielpaket für Unreal und Wwise für Projekt Akustik](https://www.microsoft.com/download/details.aspx?id=58090) herunter. Das Beispielpaket enthält ein Unreal Engine-Projekt, das Wwise-Projekt für dieses Unreal-Projekt und das Projekt Akustik-Plug-In für Wwise.

## <a name="set-up-the-project-acoustics-sample-project"></a>Einrichten des Projekt Akustik-Beispielprojekts
Um das Projekt Akustik-Beispielprojekt für Unreal/Wwise einzurichten, müssen Sie zuerst das Projekt Akustik-Plug-In für Wwise installieren. Stellen Sie dann die Wwise-Binärdateien für das Unreal-Projekt bereit, und passen Sie das Unreal-Plug-In von Wwise so an, dass es Projekt Akustik unterstützt.

### <a name="install-the-project-acoustics-wwise-plugin"></a>Installieren des Wwise-Plug-Ins für Projekt Akustik
Öffnen Sie das Wwise-Startprogramm, und wählen Sie auf der Registerkarte **Plugins** (Plug-Ins) unter **Install New Plugins** (Neue Plug-Ins installieren) die Option **Add From Directory** (Aus Verzeichnis hinzufügen) aus. Wählen Sie das Verzeichnis `AcousticsWwisePlugin\ProjectAcoustics` aus, das im heruntergeladenen Paket enthalten war.

![Screenshot des Wwise-Startprogramms mit der Option zum Installieren des Wwise-Plug-Ins](media/wwise-install-new-plugin.png)

### <a name="add-wwise-binaries-to-the-project-acoustics-unreal-sample-project"></a>Hinzufügen der Wwise-Binärdateien zum Unreal-Beispielprojekt für Projekt Akustik
Klicken Sie im Wwise-Startprogramm auf die Registerkarte **Unreal Engine**, klicken Sie dann auf das Hamburger-Menü neben **Recent Unreal Engine Projects** (Letzte Unreal Engine Projekte), und wählen Sie **Browse for project** (Nach Projekt suchen) aus. Öffnen Sie die `.uproject`-Datei des Unreal-Beispielprojekts im Paket: `AcousticsSample\AcousticsGame\AcousticsGame.uproject`.

![Screenshot der Unreal-Registerkarte im Wwise-Startprogramm](media/wwise-unreal-tab.png)

Klicken Sie nun neben dem Projekt Akustik-Beispielprojekt auf **Integrate Wwise in Project** (Wwise in das Projekt integrieren).

![Screenshot des Wwise-Startprogramms mit dem Unreal-Akustikspielprojekt](media/wwise-acoustics-game-project.png)

### <a name="extend-wwises-unreal-plugin-functionality"></a>Erweitern der Funktionen des Unreal-Plug-Ins von Wwise
Für das Unreal-Plug-In für Projekt Akustik müssen zusätzliche Funktionen aus der Unreal-Plug-In-API für Wwise verfügbar gemacht werden. Führen Sie die im Unreal-Plug-In für Projekt Akustik bereitgestellte Batchdatei aus, um diese Änderungen zu automatisieren:
* Führen Sie in `AcousticsGame\Plugins\ProjectAcoustics\Resources` die Datei `PatchWwise.bat` aus.

    ![Screenshot des Windows-Explorer-Fensters mit dem Skript zum Patchen des Wwise-Projekts](media/patch-wwise-script.png)

* Wenn das DirectX SDK nicht installiert ist, müssen Sie in `AcousticsGame\Plugins\Wwise\Source\AkAudio\AkAudio.Build.cs` abhängig von der verwendeten Wwise-Version ggf. die Zeile mit `DXSDK_DIR` auskommentieren:

    ![Screenshot des Code-Editors mit der auskommentierten DXSDK-Zeile](media/directx-sdk-comment.png)

* Wenn Sie mit Visual Studio 2019 kompilieren, umgehen Sie einen Verknüpfungsfehler bei Wwise, indem Sie den `VSVersion`-Standardwert in `AcousticsGame\Plugins\Wwise\Source\AkAudio\AkAudio.Build.cs` manuell zu `vc150` ändern:

    ![Screenshot des Code-Editors mit geändertem VSVersion-Wert: vc150](media/vsversion-comment.png)

### <a name="open-the-unreal-project"></a>Öffnen Sie das Unreal-Projekt. 
Sie werden gefragt, ob die Module neu erstellt werden sollen. Klicken Sie auf „Ja“.

>Wenn beim Öffnen des Projekts Buildfehler auftreten, vergewissern Sie sich, dass Sie das Wwise-Plug-In für Projekt Akustik in derselben Wwise-Version installiert haben, die auch im Projekt Akustik-Beispielprojekt verwendet wird.

>Wenn Sie eine frühere [AudioKinetic Wwise](https://www.audiokinetic.com/products/wwise/)-Version als 2019.1 verwenden, können Sie mit dem Projekt Akustik-Beispielprojekt keine Soundbanken erstellen.  Um das Beispielprojekt ordnungsgemäß zu verwenden, ist die Integration von Wwise Version 2019.1 erforderlich.

## <a name="experiment-with-project-acoustics-design-controls"></a>Experimentieren mit den Entwurfssteuerelementen von Projekt Akustik
Hören Sie sich die Szene an, indem Sie im Unity-Editor auf die Wiedergabeschaltfläche klicken. Navigieren Sie auf dem Desktop mit den Tasten W, A, S, D und mit der Maus. Um Tastenkombinationen für weitere Steuerelemente anzuzeigen, drücken Sie **F1**. Hier sind einige Entwurfsaktivitäten, die Sie ausprobieren können:

### <a name="modify-occlusion-and-transmission"></a>Ändern der Verdeckung und Übertragung
Für jeden Unreal-Soundakteur gibt es quellenbezogene Entwurfssteuerelemente in Projekt Akustik:

![Screenshot der Akustik-Entwurfssteuerelemente in Unreal](media/demo-scene-sound-source-design-controls.png)

Wenn der Multiplikator **Occlusion** (Verdeckung) größer als 1 ist (der Standardwert ist 1), tritt eine übertriebene Verdeckung auf. Bei einer Festlegung unter 1 ist der Verdeckungseffekt geringer.

Um die Übertragung durch die Wand zu aktivieren, schieben Sie den Schieberegler **Transmission (dB)** (Übertragung (dB)) von der untersten Stufe nach oben. 

### <a name="modify-wetness-for-a-source"></a>Anpassen der Verwässerung für eine Quelle
Um anzupassen, wie stark die Verwässerung mit der Entfernung zunimmt, verwenden Sie **Perceptual Distance Warp** (Verzerrung für Wahrnungsentfernung). Projekt Akustik berechnet Verwässerungspegel im gesamten simulierten Raum, die mit der Entfernung stufenlos variieren und wahrnehmbare Entfernungshinweise liefern. Durch Erhöhen der Entfernungsverzerrung wird dieser Effekt intensiviert, da die Entfernungsverwässerung erhöht wird. Verzerrungswerte kleiner als 1 verringern den entfernungsbasierten Hall. Dieser Effekt kann mit **Wetness (dB)** (Verwässerung (dB)) auch feiner abgestuft angepasst werden.

Erhöhen Sie die Verfallszeit im gesamten Raum durch Anpassen von **Decay Time Scale** (Verfallszeitskala). Angenommen, das Simulationsergebnis ist eine Verfallszeit von 1,5 s. Durch Festlegen von **Decay Time Scale** (Verfallszeitskala) auf 2 wird eine Verfallszeit von 3 s auf die Quelle angewandt.

### <a name="modify-distance-based-attenuation"></a>Ändern der distanzabhängigen Dämpfung
Das Wwise-Mixer-Plug-In für Projekt Akustik berücksichtigt die in Wwise integrierte distanzabhängige Dämpfung pro Quelle. Durch das Ändern dieser Kurve wird der Pegel der Trockenstrecke geändert. Das Projekt Akustik-Plug-In passt den Verwässerungspegel so an, dass die durch die Simulation und die Entwurfssteuerelemente festgelegte Verwässerungs-Trockenheits-Mischung aufrechterhalten bleibt.

![Screenshot des Bereichs mit der Wwise-Dämpfungskurve mit Dämpfung gegen Null vor Simulationsgrenze](media/demo-sounds-attenuation.png)

Projekt Akustik führt die Berechnung in einem „Simulationsbereichsfeld“ durch, das um jede simulierte Spielerposition herum angeordnet ist. Die Akustikressourcen im Beispielpaket wurden mit einem Simulationsbereichsradius von 45 m erstellt, und die Dämpfung ist so ausgelegt, dass sie vor 45 m auf 0 sinkt. Dieser Abfall ist nicht zwingend erforderlich und weist den Nachteil auf, dass nur in der Geometrie innerhalb von 45 m um den Zuhörer herum Töne verdeckt werden.

## <a name="next-steps"></a>Nächste Schritte
* [Integrieren des Projekt Akustik](unreal-integration.md)-Plug-Ins in Ihr Unreal-Projekt
* [Erstellen eines Azure-Kontos](create-azure-account.md) für eigene Bake-Vorgänge


