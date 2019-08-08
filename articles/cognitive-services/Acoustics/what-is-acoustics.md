---
title: Übersicht über Projekt Akustik
titlesuffix: Azure Cognitive Services
description: Projekt Akustik ist eine Akustik-Engine für interaktive 3D-Erfahrungen, in der auf Wellen basierende Physiksimulationen mit Baking mit interaktiven Entwurfssteuerelementen integriert sind.
services: cognitive-services
author: kegodin
manager: nitinme
ms.service: cognitive-services
ms.subservice: acoustics
ms.topic: overview
ms.date: 03/20/2019
ms.author: kegodin
ROBOTS: NOINDEX
ms.openlocfilehash: a654dd966c40b5b079f92b910ade52f4eda10344
ms.sourcegitcommit: ad9120a73d5072aac478f33b4dad47bf63aa1aaa
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 08/01/2019
ms.locfileid: "68704758"
---
# <a name="what-is-project-acoustics"></a>Was ist „Projekt: Akustik“?
Projekt Akustik ist eine Wellenakustik-Engine für interaktive 3D-Erfahrungen. Sie modelliert Welleneffekte wie Beugung, Portale und Hall in komplexen Szenen ohne manuelle Zonenmarkierung. Außerdem schließt sie eine Integration von Spiele-Engines und Audiomiddleware ein. Die Philosophie hinter Projekt Akustik ist vergleichbar mit einer statischen Beleuchtung: Das Baking einer detaillierten Physik wird offline ausgeführt, um eine physische Baseline bereitzustellen, und die künstlerischen Ziele werden mithilfe einer einfachen Runtime mit ausdrucksstarken Entwurfssteuerelementen erreicht.

![Screenshot von Gears of War 4 mit Akustikvoxeln](media/gears-with-voxels.jpg)

## <a name="using-wave-physics-for-interactive-acoustics"></a>Verwenden von Wellenphysik für interaktive Akustik
Auf Strahlen basierende Akustikmethoden können eine Überprüfung auf Verdeckung mit einer einzelnen Quelle-zu-Zuhörer-Strahlenausdehnung überprüfen oder den Hall durch Abschätzen des Volumens der lokalen Szene mit wenigen Strahlen erzeugen. Diese Techniken sind jedoch nicht immer zuverlässig, da ein Kieselstein für eine ebenso starke Verdeckung sorgt wie ein großer Felsblock. Mit Strahlen lässt sich die Krümmung von Tönen um Objekte herum – ein als Beugung bezeichnetes Phänomen – nicht abbilden. Die Simulation in Projekt Akustik erfasst diese Effekte mithilfe einer auf Wellen basierenden Simulation. Das Ergebnis ist besser vorhersagbar und zuverlässiger.

Die zentrale Innovation von Projekt Akustik besteht darin, akustische Simulation mit herkömmlichen Konzepten des Sounddesigns zu verbinden. Die Simulationsergebnisse werden in herkömmliche Audio-DSP-Parameter für Verdeckung, Portale und Hall übersetzt. Der Designer nutzt Steuerelemente für diesen Übersetzungsvorgang. Weitere Informationen zu den Kerntechnologien hinter Projekt Akustik finden Sie auf der [Forschungsprojektseite](https://www.microsoft.com/en-us/research/project/project-triton/).

![Animation mit einem horizontalen 2D-Segment mit Wellenausbreitung in einer Szene](media/wave-simulation.gif)

## <a name="setup"></a>Einrichtung
Die [Unity-Integration von Projekt Akustik](unity-integration.md) unterstützt Drag & Drop und enthält ein Unity-Audio-Engine-Plug-In. Erweitern Sie die Audioquellen-Steuerelemente von Unity, indem Sie eine Komponente für Projekt Akustik-C#-Steuerelemente an jedes Audioobjekt anfügen.

Die [Unreal-Integration von Projekt Akustik](unreal-integration.md) enthält Editor- und Spiel-Plug-Ins für Unreal sowie ein Wwise-Mixer-Plug-In. Eine benutzerdefinierte Audiokomponente erweitert die vertrauten Wwise-Funktionen in Unreal um Liveakustik-Entwurfssteuerelemente. Die Entwurfssteuerelemente sind außerdem in Wwise im Mixer-Plug-In verfügbar.

## <a name="workflow"></a>Workflow
* **Vorabbaking:** Beginnen Sie mit der Einrichtung des Baking, indem Sie auswählen, welche Geometrie auf Akustik reagieren soll, z. B. durch Ignorieren von Lichtschächten. Bearbeiten Sie dann automatische Materialzuweisungen, und wählen Sie Navigationsbereiche aus, um das Zuhörersampling zu steuern. Es findet keine manuelle Markierung für Hall-/Portal-/Raumzonen statt.
* **Baking:** In einem lokal ausgeführten Analyseschritt werden die Umwandlung in Voxel und andere geometrische Analysen entsprechend der Auswahl oben in der Szene umgesetzt. Die Ergebnisse werden im Editor visualisiert, um die Szeneneinrichtung zu überprüfen. Bei der Bakingübermittlung werden die Voxeldaten an Azure gesendet, und Sie erhalten eine Akustikspielressource.
* **Runtime:** Laden Sie die Ressource in Ihren Level, dann können Sie sich die Akustik im Level anhören. Entwerfen Sie die Akustik live im Editor mit abgestuften Steuerelementen für jede Quelle. Die Steuerelemente können auch über Levelskripts gesteuert werden.

## <a name="platforms"></a>Plattformen
Die Projekt Akustik-Runtime-Plug-Ins können derzeit auf den folgenden Plattformen bereitgestellt werden:
* Windows
* Android
* Xbox One

## <a name="download"></a>Download
* [Projekt Akustik (Unity-Plug-Ins und -Beispiele)](https://www.microsoft.com/en-us/download/details.aspx?id=57346)
* [Projekt Akustik (Unreal- und Wwise-Plug-Ins und -Beispiele)](https://www.microsoft.com/download/details.aspx?id=58090)
  * Um Binärdateien und Support für die Xbox zu erhalten, kontaktieren Sie uns über das nachstehende Registrierungsformular.

## <a name="contact-us"></a>Kontakt
* [Erläuterung zum Projekt Akustik und Meldung von Problemen](https://github.com/microsoft/ProjectAcoustics/issues)
* [Melden Sie sich an, um aktuelle Informationen zu Projekt Akustik zu erhalten.](https://forms.office.com/Pages/ResponsePage.aspx?id=v4j5cvGGr0GRqy180BHbRwMoAEhDCLJNqtVIPwQN6rpUOFRZREJRR0NIQllDOTQ1U0JMNVc4OFNFSy4u)

## <a name="next-steps"></a>Nächste Schritte
* Probieren Sie einen [Projekt Akustik-Schnellstart für Unity](unity-quickstart.md) oder [Unreal](unreal-quickstart.md) aus.
* Erkunden Sie die [Sounddesignphilosophie von Projekt Akustik](design-process.md).

