---
title: Iteratives Entwickeln und Debuggen in Azure Data Factory
description: Hier erfahren Sie, wie Sie Data Factory-Pipelines im Azure-Portal iterativ entwickeln und debuggen.
ms.date: 09/26/2018
ms.topic: conceptual
ms.service: data-factory
services: data-factory
documentationcenter: ''
ms.workload: data-services
ms.tgt_pltfrm: na
author: djpmsft
ms.author: daperlov
manager: craigg
ms.openlocfilehash: 38e51e33ef487da8879132eb1ce9cf1e63a814e4
ms.sourcegitcommit: 609d4bdb0467fd0af40e14a86eb40b9d03669ea1
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 11/06/2019
ms.locfileid: "73672844"
---
# <a name="iterative-development-and-debugging-with-azure-data-factory"></a>Iteratives Entwickeln und Debuggen mit Azure Data Factory

Mithilfe von Azure Data Factory können Sie Data Factory-Pipelines iterativ entwickeln und debuggen.

Das folgende Video enthält eine achtminütige Einführung und Demonstration dieses Features:

> [!VIDEO https://channel9.msdn.com/Shows/Azure-Friday/Iterative-development-and-debugging-with-Azure-Data-Factory/player]

## <a name="iterative-debugging-features"></a>Funktionen für iteratives Debuggen
Mit der Funktion **Debuggen** auf der Pipelinecanvas können Sie Pipelines erstellen und Testläufe ausführen, ohne eine einzige Codezeile schreiben zu müssen.

![Funktion zum Debuggen auf der Pipelinecanvas](media/iterative-development-debugging/iterative-development-image1.png)

Zeigen Sie die Ergebnisse der Testläufe auf der Pipelinecanvas im Fenster **Ausgabe** an.

![Ausgabefenster der Pipelinecanvas](media/iterative-development-debugging/iterative-development-image2.png)

Fügen Sie nach der erfolgreichen Ausführung eines Testlaufs weitere Aktivitäten zur Pipeline hinzu, und setzen Sie das iterative Debuggen fort. Sie können einen aktiven Testlauf auch über **Abbrechen** beenden.

![Abbrechen eines Testlaufs](media/iterative-development-debugging/iterative-development-image3.png)

Beim Ausführen von Testläufen müssen Sie Ihre Änderungen nicht in der Data Factory veröffentlichen, bevor Sie auf **Debuggen** klicken. Dieses Feature ist in Szenarios praktisch, bei denen Sie sicherstellen möchten, dass die Änderungen wie erwartet funktionieren, bevor Sie den Data Factory-Workflow aktualisieren.

> [!IMPORTANT]
> Durch Auswählen von **Debuggen** wird die Pipeline ausgeführt. Wenn die Pipeline also beispielsweise die copy-Aktivität enthält, werden beim Testlauf Daten aus der Quelle zum Ziel kopiert. Daher wird empfohlen, beim Debuggen Testordner in Ihrer copy-Aktivität und Ihren anderen Aktivitäten zu verwenden. Wechseln Sie nach dem Debuggen der Pipeline zu den Ordnern, die Sie tatsächlich in normalen Vorgängen verwenden möchten.

## <a name="visualizing-debug-runs"></a>Visualisieren von Debugausführungen

Sie können alle Debugausführungen visualisieren, die an einem Ort für Ihre Data Factory verarbeitet werden. Wählen Sie in der oberen rechten Ecke der Seite **View debug runs** (Debugausführungen anzeigen) aus. Dieses Feature ist in Szenarios nützlich, in denen Masterpipelines Debugausführungen für untergeordnete Pipelines auslösen und Sie möchten, dass alle aktiven Debugausführungen in einer einzelnen Ansicht angezeigt werden.

![Auswählen des Symbols zum Anzeigen aktiver Debugausführungen](media/iterative-development-debugging/view-debug-runs-image1.png)

![Beispielauflistung aktiver Debugausführungen](media/iterative-development-debugging/view-debug-runs-image2.png)

Wenn Sie über aktive Datenfluss-Debugsitzungen verfügen, werden diese Sitzungen im unteren Teil des aktiven Debugfensters angezeigt. Sie können eine aktive Datenflusssitzung auswählen und den jeweiligen Cluster beenden.

![Beispielauflistung aktiver Datenfluss-Debugausführungen](media/data-flow/dfsessions.png)

## <a name="monitoring-debug-runs"></a>Überwachen von Debugausführungen

Die mit der Funktion **Debuggen** initiierten Testläufe sind nicht in der Liste auf der Registerkarte **Überwachen** verfügbar. Es werden nur Testläufe angezeigt, die mithilfe der Trigger **Jetzt auslösen**, **Zeitplan** oder **Rollierendes Fenster** auf der Registerkarte **Überwachen** ausgelöst wurden. Der letzte mit der Funktion **Debuggen** initiierte Testlauf wird im Fenster **Ausgabe** der Pipelinecanvas angezeigt.

## <a name="setting-breakpoints-for-debugging"></a>Festlegen von Breakpoints für das Debuggen

Mit Data Factory können Sie das Debuggen auch bis zum Erreichen einer bestimmten Aktivität in der Pipelinecanvas ausführen. Legen Sie einfach einen Breakpoint für die Aktivität fest, bis zu dem der Test ausgeführt werden soll, und klicken Sie auf **Debuggen**. Data Factory stellt sicher, dass die Testläufe nur bis zur Breakpoint-Aktivität auf der Pipelinecanvas ausgeführt werden. Dieses Feature namens *Debug Until* (Debuggen bis) ist nützlich, wenn Sie nicht die gesamte Pipeline, sondern nur eine Teilmenge der Aktivitäten in der Pipeline testen möchten.

![Breakpoints auf der Pipelinecanvas](media/iterative-development-debugging/iterative-development-image4.png)

Um einen Haltepunkt festzulegen, wählen Sie ein Element auf der Pipelinecanvas aus. Eine Option *Debug Until* wird als ein leerer roter Kreis in der oberen rechten Ecke des Elements angezeigt.

![Vor dem Festlegen eines Haltepunkts für das ausgewählte Element](media/iterative-development-debugging/iterative-development-image5.png)

Nachdem Sie die Option *Debug Until* ausgewählt haben, ändert sich der leere Kreis in einen ausgefüllten roten Kreis, um anzugeben, dass der Breakpoint aktiviert ist.

![Nach dem Festlegen eines Haltepunkts für das ausgewählte Element](media/iterative-development-debugging/iterative-development-image6.png)

## <a name="next-steps"></a>Nächste Schritte
[Continuous Integration und Continuous Deployment in Azure Data Factory](continuous-integration-deployment.md)
