---
title: Häufig gestellte Fragen zu Projekt Akustik
titlesuffix: Azure Cognitive Services
description: Diese Seite enthält Antworten auf häufig gestellte Fragen zu Project Acoustics, darunter auch Fragen zu Downloadanweisungen sowie zum Bake-Vorgang.
services: cognitive-services
author: kegodin
manager: nitinme
ms.service: cognitive-services
ms.subservice: acoustics
ms.topic: conceptual
ms.date: 03/20/2019
ms.author: kegodin
ROBOTS: NOINDEX
ms.openlocfilehash: a965fc020c5c534616459ad661b71ac67dbc2425
ms.sourcegitcommit: ad9120a73d5072aac478f33b4dad47bf63aa1aaa
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 08/01/2019
ms.locfileid: "68704807"
---
# <a name="project-acoustics-frequently-asked-questions"></a>Häufig gestellte Fragen zu Projekt Akustik

## <a name="what-is-project-acoustics"></a>Was ist „Projekt: Akustik“?

Bei den Plug-Ins der Projekt Akustik-Suite handelt es sich um ein Akustiksystem, mit dem das Verhalten von Akustikwellen ähnlich wie bei der statischen Beleuchtung vor der Laufzeit berechnet wird. Der Großteil der Wellenphysikberechnungen wird von der Cloud erledigt, sodass die CPU-Kosten zur Laufzeit niedrig sind.  

## <a name="where-can-i-download-the-plugin"></a>Wo kann ich das Plug-In herunterladen?

Sie können das Plug-In [Projekt Akustik mit Unity](https://www.microsoft.com/download/details.aspx?id=57346) oder das Plug-In [Projekt Akustik mit Unreal](https://www.microsoft.com/download/details.aspx?id=58090) herunterladen.

## <a name="does-project-acoustics-support-ltxgt-platform"></a>Unterstützt Projekt Akustik die &lt;x&gt;-Plattform?

Die Unterstützung der Projekt Akustik-Plattform wird basierend auf den Kundenbedürfnissen entwickelt. Kontaktieren Sie uns in den [Projekt Akustik-Foren zu Problemen](https://github.com/microsoft/ProjectAcoustics/issues), um nach Unterstützung für zusätzliche Plattformen zu fragen.

## <a name="is-azure-used-at-runtime"></a>Wird Azure zur Laufzeit verwendet?

Nein, die Cloudintegration wird nur während der Vorausberechnungsphase als Teil der Szeneneinrichtung verwendet.
 
## <a name="what-is-simulation-input"></a>Was ist eine Simulationseingabe? 

Die Simulationseingabe ist Ihre 3D-Szene, die virtuelle Umgebung oder die Spielstufe. Von Project Acoustics werden volumetrische Wellensimulationen in 3D durchgeführt, mit denen die Schallphysik wie etwa eine weiche Okklusion und Streuung genau modelliert wird.
 
## <a name="what-is-the-runtime-cost"></a>Was sind Laufzeitkosten?

Acoustics belegt etwa 0,01 % der CPU pro Quelle pro Frame. Die Speicherauslastung hängt von der Größe der Szene ab und kann zwischen 10 und 100 MB liegen.
 
## <a name="do-i-need-to-simplify-the-level-geometry-control-triangle-count-make-meshes-watertight"></a>Muss ich die Pegelgeometrie vereinfachen? Muss ich die Anzahl der Dreiecke steuern? Muss ich Gittermodelle wasserdicht machen?

Nein. Das System erfasst die detaillierte Pegelgeometrie direkt. Sie wird zur internen Verarbeitung in Voxels konvertiert.
 
## <a name="whats-in-the-runtime-lookup-table"></a>Was befindet sich in der Nachschlagetabelle für die Runtime?

Die ACE-Datei enthält eine Tabelle der akustischen Parameter zwischen zahlreichen Quell- und Listener-Positionspaaren sowie eine in Voxel konvertierte Szenengeometrie, die für die Parameterinterpolation verwendet wird.
 
## <a name="can-project-acoustics-handle-moving-sources"></a>Kann Projekt Akustik mit beweglichen Quellen umgehen?

Ja, Projekt Akustik konsultiert die Nachschlagetabelle und aktualisiert den Audio-DSP bei jedem Tick, sodass bewegliche Quellen und Listener verarbeitet werden können.
 
## <a name="can-project-acoustics-handle-dynamic-geometry-closing-doors-walls-blown-away"></a>Kann Projekt Akustik mit dynamischer Geometrie umgehen? Oder schließende Türen? Oder Wände, die entfernt werden?

Nein. Die akustischen Parameter werden basierend auf dem statischen Zustand einer Spielstufe vorausberechnet. Wir empfehlen, die Türgeometrie aus der Akustik auszuklammern und anschließend basierend auf dem Zustand von zerstörbaren und beweglichen Spielobjekten mithilfe bewährter Verfahren zusätzliche Okklusion anzuwenden.
 
## <a name="does-project-acoustics-use-acoustic-materials"></a>Verwendet Projekt Akustik akustische Materialien?

Ja. Materialien werden zur Steuerung des Absorptionsgrads aus den physischen Materialnamen in Ihrer Stufe ausgewählt.
 
## <a name="what-do-the-probes-represent"></a>Was bedeuten die „Tests“?

Tests sind Stichproben von möglichen Spielerpositionen. Jeder Test stellt eine separate Wellensimulation der Szene dar, die an der Testposition entsteht. Zur Laufzeit werden akustische Parameter für die Zuhörerposition aus nahegelegenen Testpositionen interpoliert.
 
## <a name="why-spend-so-much-compute-in-the-cloud-what-does-it-buy-me"></a>Warum sollen so viele Computestunden in die Cloud verlegt werden? Was bringt mir das?

Project Acoustics stellt exakte und zuverlässige akustische Parameter selbst für extrem komplexe virtuelle Umgebungen dar und berücksichtigt dabei sämtliche architektonischen Aspekte. Das Plug-In stellt eine weiche Okklusion und Verzögerung sowie dynamische Hallvariationen ohne manuelles Eingreifen in Form von Volumenabzug bereit. Und dabei belastet es die CPU während der Laufzeit kaum.

## <a name="what-exactly-happens-during-baking"></a>Was geschieht beim „Bake-Vorgang“ genau?

Akustikbaking besteht aus akustischen Wellensimulationen von kubischen Simulationsbereichen in der Mitte des Listenertests.

## <a name="next-steps"></a>Nächste Schritte
* Testen der [Unity-Beispielinhalte](unity-quickstart.md) oder [Unreal-Beispielinhalte](unreal-quickstart.md) für Projekt Akustik

