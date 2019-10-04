---
title: Übersicht über Azure Data Factory Mapping Data Flow
description: Eine zusammenfassende Erläuterung von Mapping Data Flows in Azure Data Factory
author: kromerm
ms.author: makromer
ms.service: data-factory
ms.topic: conceptual
ms.date: 01/31/2019
ms.openlocfilehash: 6f4c124c59584c8538d85ac61650661ae559a77b
ms.sourcegitcommit: 07700392dd52071f31f0571ec847925e467d6795
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 08/28/2019
ms.locfileid: "70123931"
---
# <a name="what-are-mapping-data-flows"></a>Was sind Mapping Data Flows?

[!INCLUDE [notes](../../includes/data-factory-data-flow-preview.md)]

Mapping Data Flows sind visuell entworfene Datentransformationen in Azure Data Factory. Mit Data Flows können Data Engineers grafische Datentransformationslogik entwickeln, ohne Code schreiben zu müssen. Die daraus resultierenden Datenflüsse werden als Aktivitäten in Azure Data Factory-Pipelines mit horizontal skalierten Azure Databricks-Clustern ausgeführt.

Die Absicht von Azure Data Factory-Datenfluss ist es, eine vollständig visuelle Erfahrung ohne Codeerstellung zu ermöglichen. Ihre Datenflüsse werden in Ihrem eigenen Ausführungscluster für die horizontal skalierte Datenverarbeitung ausgeführt. Azure Data Factory übernimmt die gesamte Codeübersetzung, Pfadoptimierung und Ausführung Ihrer Datenflussaufträge.

Erstellen Sie zunächst Datenflüsse im Debugmodus, damit Sie Ihre Transformationslogik interaktiv überprüfen können. Fügen Sie anschließend Ihrer Pipeline eine Datenflussaktivität zum Ausführen und Testen Ihres Datenflusses in der Pipeline im Debugmodus hinzu, oder verwenden Sie in der Pipeline die Option „Trigger Now“ (Jetzt auslösen), um den Datenfluss in einer Pipelineaktivität zu testen.

Dann können Sie Ihre Datenflussaktivitäten mithilfe von Azure Data Factory-Pipelines planen und überwachen, die die Datenflussaktivität ausführen.

Der Umschalter „Debugmodus“ auf der Datenfluss-Entwurfsoberfläche ermöglicht die interaktive Erstellung von Datentransformationen. Der Debugmodus bietet eine Umgebung für die Datenvorbereitung und Datenvorschau zur Erstellung von Datenflüssen.

## <a name="begin-building-your-data-flow-logical-graph"></a>Erstellen eines logischen Datenflussdiagramms

Beginnen Sie mit dem Erstellen von Datenflüssen, indem Sie mit dem Pluszeichen (+) unterhalb der Factory-Ressourcen einen neuen Datenfluss erstellen.

![neuer Datenfluss](media/data-flow/newdataflow2.png "neuer Datenfluss")

Zunächst konfigurieren Sie die Quelltransformation. Anschließend fügen Sie mithilfe des Pluszeichens (+) jedem nachfolgenden Schritt eine Datentransformation hinzu. Beim Erstellen des logischen Diagramms können Sie mithilfe der Schaltflächen „Diagramm anzeigen“ und „Diagramm ausblenden“ zwischen dem Diagramm- und Konfigurationsmodus wechseln.

![Diagramm anzeigen](media/data-flow/showg.png "Diagramm anzeigen")

## <a name="configure-transformation-logic"></a>Konfigurieren der Transformationslogik

![Diagramm ausblenden](media/data-flow/hideg.png "Diagramm ausblenden")

Wenn Sie das Diagramm ausblenden, können Sie seitwärts durch die Transformationsknoten navigieren.

![Navigieren](media/data-flow/showhide.png "Navigieren")

## <a name="next-steps"></a>Nächste Schritte

* [Beginnen mit einer Quelltransformation](data-flow-source.md)
