---
title: Unity-Entwurfstutorial für Projekt Akustik
titlesuffix: Azure Cognitive Services
description: Dieses Tutorial beschreibt den Entwurfsworkflow für Projekt Akustik in Unity.
services: cognitive-services
author: kegodin
manager: nitinme
ms.service: cognitive-services
ms.subservice: acoustics
ms.topic: tutorial
ms.date: 03/20/2019
ms.author: kegodin
ROBOTS: NOINDEX
ms.openlocfilehash: f9ff4225e7e855ed666d3554631015b8ce51df37
ms.sourcegitcommit: ad9120a73d5072aac478f33b4dad47bf63aa1aaa
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 08/01/2019
ms.locfileid: "68706589"
---
# <a name="project-acoustics-unity-design-tutorial"></a>Unity-Entwurfstutorial für Projekt Akustik
Dieses Tutorial beschreibt die Entwurfstools und den -workflow für Projekt Akustik in Unity.

Voraussetzungen:
* Unity 2018.2+ für Windows
* Eine Unity-Szene mit einer Akustikressource, für die bereits ein Baking ausgeführt wurde

Für dieses Tutorial können Sie eine Unity-Szene mit einer Akustikressource mit Baking auf zwei Arten abrufen:
* [Fügen Sie Ihrem Unity-Projekt Projekt Akustik hinzu](unity-integration.md), [legen Sie ein Azure Batch-Konto an](create-azure-account.md), und [führen Sie dann das Baking für Ihre Unity-Szene aus](unity-baking.md).
* Oder verwenden Sie die [Unity-Beispielinhalte für Projekt Akustik](unity-quickstart.md).

## <a name="review-design-process-concepts"></a>Überprüfen der Konzepte für den Entwurfsvorgang
Projekt Akustik verwendet allgemeine Methoden der digitalen Signalverarbeitung (DSP) für Audio, um Ihre Quellen zu verarbeiten. Sie haben die Kontrolle über vertraute Akustikeigenschaften wie Verdeckung, Verwässerungs-Trockenheits-Mischung und Hallpfadlänge (RT60). Das [Hauptkonzept des Projekt Akustik-Entwurfsvorgangs](design-process.md) besteht jedoch darin, diese Eigenschaften nicht direkt festzulegen, sondern zu steuern, wie diese Eigenschaften mithilfe der Simulationsergebnisse beeinflusst werden sollen. Die Standardeinstellungen für jedes Steuerelement geben physisch präzise Akustik wieder.

## <a name="design-acoustics-for-each-source"></a>Entwurf der Akustik für jede Quelle
Projekt Akustik bietet eine Reihe von datenquellenspezifischen Akustikentwurfs-Steuerelementen. Dadurch können Sie die Mischung in einer Szene steuern, indem Sie einige Quellen hervorheben und andere in den Hintergrund stellen.

### <a name="adjust-distance-based-attenuation"></a>Anpassen der distanzabhängigen Dämpfung
Der vom Unity-Spatializer-Plug-In von **Projekt Akustik** bereitgestellte Audio-DSP berücksichtigt die in den Unity-Editor integrierte distanzabhängige Dämpfung pro Quelle. Steuerelemente für die distanzabhängige Dämpfung befinden sich in der Komponente **Audioquelle** im Bereich **Inspektor** von Tonquellen unter **3D Sound Settings** (3D-Soundeinstellungen):

![Screenshot des Unity-Panels mit Optionen für die Distanzdämpfung](media/distance-attenuation.png)

Acoustics führt die Berechnung in einem „Simulationsbereichsfeld“ durch, das um die Position des Players herum angeordnet ist. Wenn eine Tonquelle vom Player entfernt ist, die sich außerhalb dieses Simulationsbereichs befindet, beeinflusst nur die Geometrie innerhalb des Felds die Schallausbreitung (z. B. durch Okklusion), was recht gut funktioniert, wenn sich Okkluder in der Nähe des Players befinden. In Fällen, in denen sich der Player jedoch im offenen Raum befindet, sich die Okkluder aber in der Nähe der fernen Tonquelle befinden, kann der Ton unrealistisch disokkludiert werden. Als Umgehung wird empfohlen, in solchen Fällen sicherzustellen, dass die Schallschwächung bei ca. 45 m auf 0 absinkt, dem voreingestellten horizontalen Abstand des Players zum Feldrand.

![Screenshot des Unity-Panels mit SpeakerMode-Optionen](media/speaker-mode.png)

### <a name="adjust-occlusion-and-transmission"></a>Anpassen der Verdeckung und Übertragung
Durch Anfügen des **AcousticsAdjust**-Skripts an eine Quelle können die Parameter für diese Quelle optimiert werden. Wenn Sie das Skript anfügen möchten, klicken Sie auf **Komponente hinzufügen** im unteren Bereich des Abschnitts **Inspektor** und navigieren zu **Skripts > Acoustics Adjust** (Anpassung der Akustik). Das Skript verfügt über sechs Steuerelemente:

![Screenshot des Unity-Skripts „AcousticsAdjust“](media/acoustics-adjust.png)

* **Enable Acoustics** (Akustik aktivieren): Bestimmt, ob auf diese Quelle Akustikeffekte angewendet werden. Ist diese Option deaktiviert, wird die Quelle mithilfe von HRTFs oder der Panoramafunktion räumlich gemacht, es sind dann aber keine Akustikeinstellungen möglich. Dies bedeutet, dass keine Parameter für Hindernisse, Verdeckung oder dynamischen Hall wie z. B. Stärke und Abfallzeit verfügbar sind. Halleffekte werden dennoch angewendet, jedoch mit einem festen Pegel und einer festen Abklingzeit.
* **Occlusion** (Okklusion): Wenden Sie einen Multiplikator auf den vom Akustiksystem berechneten dB-Wert der Okklusion an. Wenn dieser Multiplikator größer als 1 ist, wird die Okklusion überspitzt, während Werte kleiner als 1 zu einem subtileren Okklusionseffekt führen, und ein Wert von 0 deaktiviert die Okklusion.
* **Transmission (dB)** (Übertragung): Legen Sie die Schallschwächung (in dB) fest, die durch die Übertragung durch die Geometrie verursacht wird. Stellen Sie diesen Schieberegler auf die niedrigste Stufe, um die Übertragung zu deaktivieren. Die Akustik legt das anfänglich trockene Audio räumlich so fest, dass es um die Szenengeometrie herum eintrifft (Portalisierung). Die Übertragung bietet einen zusätzlichen trockenen Eingang, der in Sichtrichtung verräumlicht ist. Beachten Sie, dass auch die Distanzdämpfungskurve für die Quelle angewendet wird.

### <a name="adjust-reverberation"></a>Anpassen des Halls
* **Wetness (dB)** (Verwässerung): Stellt die Hallleistung in dB entsprechend der Entfernung zur Quelle ein. Positive Werte verstärken den Hall, während negative Werte für einen Ton mit weniger Hall sorgen. Klicken Sie auf das Kurvensteuerelement (grüne Linie), um den Kurveneditor aufzurufen. Ändern Sie die Kurve, indem Sie mit der linken Maustaste klicken, um Punkte hinzuzufügen, und diese Punkte ziehen, um die gewünschte Funktion zu bilden. Die x-Achse ist der Abstand zur Quelle und die y-Achse die Hallkorrektur in dB. Weitere Informationen zur Bearbeitung von Kurven finden Sie in diesem [Unity-Leitfaden](https://docs.unity3d.com/Manual/EditingCurves.html). Um die Kurve wieder auf den Standardwert zurückzusetzen, klicken Sie mit der rechten Maustaste auf **Wetness** (Verwässerung) und wählen Sie **Reset** (Zurücksetzen) aus.
* **Decay Time Scale** (Abklingzeitskala): Passt den Multiplikator für die Abklingzeit an. Wenn im Ergebnis des Bake-Vorgangs beispielsweise eine Abklingzeit von 750 Millisekunden angegeben ist, dieser Wert jedoch auf 1,5 festgelegt wurde, beträgt die auf die Quelle angewendete Abklingzeit 1,125 Millisekunden.
* **Outdoorness** (Außenbereichswert): eine additive Anpassung der Schätzung des Akustiksystems, wie stark der Hall für eine Quelle nach „im Freien“ klingen soll. Wenn Sie diesen Wert auf 1 festlegen, klingt eine Quelle immer vollständig so, als ob sie sich im Freien befinden würde, während eine Quelle bei der Einstellung -1 vollständig nach Innenbereich klingt.

Durch Anfügen des **AcousticsAdjustExperimental**-Skripts an eine Quelle können die zusätzlichen experimentellen Parameter für diese Quelle optimiert werden. Wenn Sie das Skript anfügen möchten, klicken Sie auf **Komponente hinzufügen** im unteren Bereich des Abschnitts **Inspektor** und navigieren zu **Skripts > Acoustics Adjust Experimental** (experimentelle Anpassung der Akustik). Es gibt derzeit ein experimentelles Steuerelement:

![Screenshot des Unity-Skripts „AcousticsAdjustExperimental“](media/acoustics-adjust-experimental.png)

* **Perceptual Distance Warp** (Wahrnehmungsdistanzverzerrung): Wenden Sie eine exponentielle Verzerrung auf die Entfernung an, die zur Berechnung des Dry-Wet-Verhältnisses (Verhältnis von Trockenheit zur Verwässerung) verwendet wird. Das Akustiksystem berechnet Verwässerungspegel im gesamten Raum, die mit der Entfernung stufenlos variieren und wahrnehmbare Entfernungshinweise liefern. Verzerrungswerte größer als 1 verstärken diesen Effekt noch, da die entfernungsbedingte Hallstärke erhöht wird, sodass der Sound „entfernt“ klingt. Verzerrungswerte kleiner als 1 verringern den entfernungsbasierten Hall, sodass der Sound „präsenter“ klingt.

## <a name="design-acoustics-for-all-sources"></a>Entwurf der Akustik für alle Quellen
Klicken Sie zum Anpassen von Parametern für alle Quellen auf den Kanalstreifen im **Audiomixer** von Unity, und passen Sie die Parameter für den Effekt **Project Acoustics Mixer** (Projektakustikmixer) an.

![Screenshot des Unity-Anpassungspanels für den Projekt Akustik-Mixer](media/mixer-parameters.png)

* **Wetness Adjust** (Feuchtigkeitsanpassung): passt die Hallstärke (in dB) für alle Quellen in der Szene basierend auf der Entfernung des Hörers an. Positive Werte verstärken den Hall, während negative Werte für einen Ton mit weniger Hall sorgen.
* **RT60 Scale** (RT60-Skala): multiplikatives Skalar für die Hallzeit.
* **Use Panning** (Panorama verwenden): steuert, ob die Audioausgabe binaural (0) oder als Mehrkanalpanorama (1) erfolgt. Sämtliche Werte außer 1 stehen für eine binaurale Ausgabe. Die Räumlichkeit der binauralen Ausgabe wird für die Verwendung mit Kopfhörern mithilfe von HRTFs und bei der Mehrkanalausgabe für die Verwendung mit Mehrkanal-Surround-Lautsprechersystemen mithilfe von VBAP erreicht. Wenn Sie das Mehrkanalpanorama verwenden, wählen Sie einen Lautsprechermodus entsprechend Ihren Geräteeinstellungen aus, den Sie unter **Projekteinstellungen** > **Audio** finden.

## <a name="check-proper-sound-source-placement"></a>Überprüfen der Platzierung der Tonquelle
Innerhalb von belegten Voxeln platzierte Tonquellen werden nicht akustisch behandelt. Da Voxel sich über die sichtbare Szenengeometrie ausdehnen, ist es möglich, eine Quelle in einem Voxel zu platzieren, obwohl sie visuell nicht verdeckt erscheint. Sie können Projekt Akustik-Voxel anzeigen, indem Sie das Kontrollkästchen für das Voxelraster im Menü **Gizmos** im rechten oberen Bereich der Ansicht **Scene** aktivieren.

![Screenshot des Gizmos-Menüs von Unity](media/gizmos-menu.png)  

Mithilfe der Voxelanzeige können Sie auch ermitteln, ob auf visuelle Komponenten im Spiel eine Transformation angewandt wurde. Ist dies der Fall, wenden Sie dieselbe Transformation auf das GameObject an, in dem der **Acoustics Manager** gehostet wird.

### <a name="bake-time-vs-run-time-voxels"></a>Voxel zur Bakingzeit und zur Laufzeit
Sie können Voxel im Editor-Fenster zur Spielentwurfszeit und im Spielfenster zur Laufzeit anzeigen. Die Größe der Voxel unterscheidet sich in diesen Ansichten. Dies liegt daran, dass die Laufzeitinterpolation der Akustik ein genaueres Voxelraster verwendet, um feinere Interpolationsergebnisse zu erzielen. Die Tonquellenplatzierung sollte mithilfe der Laufzeitvoxel überprüft werden.

Entwurfszeitvoxel:

![Screenshot der Projekt Akustik-Voxel zur Entwurfszeit](media/voxels-design-time.png)

Laufzeitvoxel:

![Screenshot der Projekt Akustik-Voxel zur Laufzeit](media/voxels-runtime.png)

## <a name="next-steps"></a>Nächste Schritte
* Erkunden von Fallstudien zu den Konzepten hinter dem [Entwurfsvorgang](design-process.md)

