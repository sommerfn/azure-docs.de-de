---
title: Einführung in die Azure Stream Analytics-Windowing-Funktionen
description: In diesem Artikel werden die vier Windowing-Funktionen (Rollierend, Springend, Gleitend, Sitzung) beschrieben, die in Azure Stream Analytics-Aufträgen verwendet werden.
services: stream-analytics
author: jseb225
ms.author: jeanb
ms.reviewer: jasonh
ms.service: stream-analytics
ms.topic: conceptual
ms.date: 06/11/2019
ms.openlocfilehash: 56b6f11d226f25e3094a90d8646fa13860ee306e
ms.sourcegitcommit: 41ca82b5f95d2e07b0c7f9025b912daf0ab21909
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 06/13/2019
ms.locfileid: "67066761"
---
# <a name="introduction-to-stream-analytics-windowing-functions"></a>Einführung in die Stream Analytics-Windowing-Funktionen

Bei Szenarien mit „Time Streaming“ ist das Durchführen von Vorgängen für die Daten in temporalen Fenstern ein häufiges Muster. Stream Analytics verfügt über native Unterstützung für Windowing-Funktionen, sodass Entwickler komplexe Streaming-Verarbeitungsaufträge mit sehr geringem Aufwand erstellen können.

Vier Arten temporaler Fenster stehen zur Auswahl: Fenster vom Typ [**Rollierend**](https://msdn.microsoft.com/azure/stream-analytics/reference/tumbling-window-azure-stream-analytics), [**Springend**](https://msdn.microsoft.com/azure/stream-analytics/reference/hopping-window-azure-stream-analytics), [**Gleitend**](https://msdn.microsoft.com/azure/stream-analytics/reference/sliding-window-azure-stream-analytics) und [**Sitzung**](https://msdn.microsoft.com/azure/stream-analytics/reference/session-window-azure-stream-analytics).  Sie verwenden die Fensterfunktionen in der [**GROUP BY**](https://msdn.microsoft.com/azure/stream-analytics/reference/group-by-azure-stream-analytics)-Klausel der Abfragesyntax in Ihren Stream Analytics-Aufträgen. Sie können Ereignisse auch über mehrere Fenster hinweg aggregieren, indem Sie die [**Windows()** -Funktion](https://docs.microsoft.com/stream-analytics-query/windows-azure-stream-analytics) verwenden.

Für alle [Windowing](https://msdn.microsoft.com/azure/stream-analytics/reference/windowing-azure-stream-analytics)-Vorgänge werden am **Ende** des Fensters Ergebnisse ausgegeben. Die Ausgabe des Fensters ist ein einzelnes Ereignis, das auf der verwendeten Aggregatfunktion basiert. Das Ausgabeereignis verfügt über den Zeitstempel vom Ende des Fensters, und alle Fensterfunktionen werden mit einer festen Länge definiert. 

![Stream Analytics-Fensterfunktionen – Konzepte](media/stream-analytics-window-functions/stream-analytics-window-functions-conceptual.png)

## <a name="tumbling-window"></a>Rollierendes Fenster
Rollierende Fensterfunktionen werden verwendet, um einen Datenstrom in einzelne Zeitsegmente zu unterteilen und dafür eine Funktion durchzuführen, z.B. wie im Beispiel unten. Die wichtigsten Unterscheidungsmerkmale eines rollierenden Fensters sind, dass sie wiederholt werden, sich nicht überlappen und ein Ereignis nicht zu mehr als einem rollierenden Fenster gehören kann.

![Stream Analytics – Rollierendes Fenster](media/stream-analytics-window-functions/stream-analytics-window-functions-tumbling-intro.png)

## <a name="hopping-window"></a>Springendes Fenster
Bei den Funktionen von springenden Fenstern wird für einen festen Zeitraum ein Sprung nach vorn durchgeführt. Sie können sich dies wie rollierende Fenster vorstellen, die sich überlappen können, damit die Ereignisse mehr als einem Resultset für springende Fenster angehören können. Um ein springendes Fenster an ein rollierendes Fenster anzugleichen, passen Sie die Sprunggröße an die Fenstergröße an. 

![Stream Analytics – Springendes Fenster](media/stream-analytics-window-functions/stream-analytics-window-functions-hopping-intro.png)

## <a name="sliding-window"></a>Gleitendes Fenster
Bei Schiebefensterfunktionen wird im Gegensatz zu rollierenden oder springenden Fenstern **nur** dann ein Ereignis produziert, wenn ein Ereignis eintritt. Jedes Fenster verfügt über mindestens ein Ereignis, und das Fenster wird fortlaufend um „€“ (Epsilon) nach vorn verschoben. Wie bei springenden Fenstern auch, können Ereignisse zu mehr als einem gleitenden Fenster gehören.

![Stream Analytics – Gleitendes Fenster](media/stream-analytics-window-functions/stream-analytics-window-functions-sliding-intro.png)

## <a name="session-window"></a>Sitzungsfenster
Mit Funktionen für Sitzungsfenster werden Ereignisse gruppiert, die zu ähnlichen Zeiten eingehen. Zeiträume, in denen keine Daten anfallen, werden herausgefiltert. Es werden drei Hauptparameter verwendet: Timeout, maximale Dauer und Partitionierungsschlüssel (optional).

![Stream Analytics – Sitzungsfenster](media/stream-analytics-window-functions/stream-analytics-window-functions-session-intro.png)

Ein Sitzungsfenster beginnt, wenn das erste Ereignis eintritt. Wenn ein anderes Ereignis innerhalb des angegebenen Zeitlimits für das letzte erfasste Ereignis eintritt, wird das Fenster erweitert, damit es das neue Ereignis enthält. Falls innerhalb des Zeitlimits keine Ereignisse eintreten, wird das Fenster zum Zeitlimit-Endzeitpunkt geschlossen.

Wenn innerhalb des angegebenen Zeitraums weiter Ereignisse eintreten, wird das Sitzungsfenster so lange erweitert, bis die maximale Dauer erreicht ist. Die Überprüfungsintervalle für die maximale Dauer werden auf die gleiche Größe wie die angegebene maximale Dauer festgelegt. Wenn die maximale Dauer beispielsweise 10 beträgt, werden die Überprüfungen, ob für das Fenster die maximale Dauer überschritten wird, nach dem Muster t = 0, 10, 20, 30 usw. durchgeführt.

Wenn ein Partitionsschlüssel angegeben wird, werden die Ereignisse nach dem Schlüssel gruppiert, und das Sitzungsfenster wird auf jede Gruppe separat angewendet. Diese Partitionierung ist für Fälle nützlich, in denen Sie unterschiedliche Sitzungsfenster für unterschiedliche Benutzer oder Geräte benötigen.


## <a name="next-steps"></a>Nächste Schritte
* [Einführung in Azure Stream Analytics](stream-analytics-introduction.md)
* [Erste Schritte mit Azure Stream Analytics](stream-analytics-real-time-fraud-detection.md)
* [Skalieren von Azure Stream Analytics-Aufträgen](stream-analytics-scale-jobs.md)
* [Stream Analytics Query Language Reference (in englischer Sprache)](https://msdn.microsoft.com/library/azure/dn834998.aspx)
* [Referenz zur Azure Stream Analytics-Verwaltungs-REST-API](https://msdn.microsoft.com/library/azure/dn835031.aspx)

