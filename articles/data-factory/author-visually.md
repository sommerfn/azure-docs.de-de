---
title: Visuelles Erstellen in Azure Data Factory
description: Informationen zum Verwenden der visuellen Erstellung in Azure Data Factory
services: data-factory
documentationcenter: ''
ms.service: data-factory
ms.workload: data-services
ms.tgt_pltfrm: na
ms.topic: conceptual
ms.date: 01/09/2019
author: djpmsft
ms.author: daperlov
ms.reviewer: ''
manager: craigg
ms.openlocfilehash: 71f0e283c9609e949cc9b09f20a16c6f5c9c3e50
ms.sourcegitcommit: 609d4bdb0467fd0af40e14a86eb40b9d03669ea1
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 11/06/2019
ms.locfileid: "73681613"
---
# <a name="visual-authoring-in-azure-data-factory"></a>Visuelles Erstellen in Azure Data Factory

Mithilfe der Azure Data Factory-Benutzeroberfläche (UX) können Sie Ressourcen für Ihre Data Factory visuell erstellen und bereitstellen, ohne Code schreiben zu müssen. Sie können Aktivitäten auf eine Pipelinecanvas ziehen, Testläufe ausführen, iterativ debuggen sowie Ihre Pipelineausführungen bereitstellen und überwachen.

## <a name="authoring-canvas"></a>Erstellungsbereich

Um den **Erstellungsbereich** zu öffnen, klicken Sie auf das Stiftsymbol. 

![Erstellungsbereich](media/author-visually/authoring-canvas.png)

Hier erstellen Sie die Pipelines, Aktivitäten, Datasets, verknüpften Dienste, Datenflüsse, Trigger und Integrationslaufzeiten, aus denen Ihre Factory besteht. Informationen zum Einstieg in das Erstellen einer Pipeline im Erstellungsbereich finden Sie unter [Kopieren von Daten mit der Kopieraktivität](tutorial-copy-data-portal.md). 

Die standardmäßige visuelle Erstellungsumgebung arbeitet direkt mit dem Data Factory-Dienst zusammen. Die Integration in Azure Repos Git oder GitHub wird ebenfalls unterstützt, um die Versionskontrolle und Zusammenarbeit an Ihren Data Factory-Pipelines zu ermöglichen. Weitere Informationen zu den Unterschieden dieser Erstellungsumgebungen finden Sie unter [Quellcodeverwaltung in Azure Data Factory](source-control.md).

## <a name="expressions-and-functions"></a>Ausdrücke und Funktionen

Anstelle statischer Werte können Sie Ausdrücke und Funktionen verwenden, um viele Eigenschaften in Azure Data Factory anzugeben.

Zum Angeben eines Ausdrucks für einen Eigenschaftswert wählen Sie **Dynamischen Inhalt hinzufügen** aus oder klicken auf **ALT+P**, wenn sich der Eingabefokus auf dem Feld befindet.

![Dynamischen Inhalt hinzufügen](media/author-visually/dynamic-content-1.png)

Dadurch wird der **Ausdrucks-Generator von Data Factory** geöffnet, in dem Sie Ausdrücke anhand von unterstützten Systemvariablen, Aktivitätsausgaben, Funktionen und benutzerdefinierten Variablen oder Parametern erstellen können. 

![Ausdrucks-Generator](media/author-visually/dynamic-content-2.png)

Informationen zur Ausdruckssprache finden Sie unter [Ausdrücke und Funktionen in Azure Data Factory](control-flow-expression-language-functions.md).

## <a name="provide-feedback"></a>Feedback geben

Wählen Sie **Feedback** aus, um Kommentare zu Funktionen abzugeben oder um Microsoft Probleme mit dem Tool zu melden:

![Feedback](media/author-visually/provide-feedback.png)

## <a name="next-steps"></a>Nächste Schritte

Unter [Programmgesteuertes Überwachen und Verwalten von Pipelines](monitor-programmatically.md) erfahren Sie mehr über das Überwachen und Verwalten von Pipelines.
