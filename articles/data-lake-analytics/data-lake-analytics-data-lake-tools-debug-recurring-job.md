---
title: Debuggen von periodischen Aufträgen in Azure Data Lake Analytics
description: Hier erfahren Sie, wie Sie mithilfe von Azure Data Lake Tools für Visual Studio einen nicht normalen periodischen Auftrag debuggen.
services: data-lake-analytics
author: yanancai
ms.author: yanacai
ms.reviewer: jasonwhowell
ms.assetid: dc9b21d8-c5f4-4f77-bcbc-eff458f48de2
ms.service: data-lake-analytics
ms.topic: conceptual
ms.date: 05/20/2018
ms.openlocfilehash: 5a2935d559a967151c5bdc01c4b0806fe52179b4
ms.sourcegitcommit: 41ca82b5f95d2e07b0c7f9025b912daf0ab21909
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 06/13/2019
ms.locfileid: "60629756"
---
# <a name="troubleshoot-an-abnormal-recurring-job"></a>Problembehandlung für einen nicht normalen periodischen Auftrag

In diesem Artikel erfahren Sie, wie Sie mithilfe von [Azure Data Lake Tools für Visual Studio](https://aka.ms/adltoolsvs) Probleme mit periodischen Aufträgen behandeln. Im [Blog zu Azure Data Lake und Azure HDInsight](https://blogs.msdn.microsoft.com/azuredatalake/2017/09/19/managing-pipeline-recurring-jobs-in-azure-data-lake-analytics-made-easy/) erfahren Sie mehr zur Pipeline und zu periodischen Aufträgen.

Periodische Aufträge verwenden in der Regel die gleiche Abfragelogik und ähnliche Eingabedaten. Ein Beispiel wäre ein periodischer Auftrag, der jeden Montag um 8 Uhr ausgeführt wird, um die wöchentlich aktiven Benutzer der letzten Woche zu zählen. Die Skripts für diese Aufträge teilen sich eine Skriptvorlage, die die Abfragelogik enthält. Die Eingaben für diese Aufträge sind die Nutzungsdaten der letzten Woche. Die gemeinsame Nutzung derselben Abfragelogik und ähnlicher Eingaben bedeutet in der Regel, dass die Leistung dieser Aufträge vergleichbar und stabil ist. Wenn einer Ihrer periodischen Aufträge plötzlich eine abnormale Leistung liefert, fehlschlägt oder sich stark verlangsamt, sollten Sie Folgendes tun:

- Ermitteln Sie anhand der Statistikberichte für die vorherigen Ausführungen des periodischen Auftrags, was passiert ist.
- Vergleichen Sie die den nicht normalen Auftrag mit einem normalen Auftrag, um zu ermitteln, was sich geändert hat.

**Die Ansicht für zugehörige Aufträge** in Azure Data Lake Tools für Visual Studio trägt in beiden Fällen zu einer schnelleren Problembehandlung bei.

## <a name="step-1-find-recurring-jobs-and-open-related-job-view"></a>Schritt 1: Suchen nach periodischen Aufträgen und Öffnen der Ansicht für zugehörige Aufträge

Um ein Problem mit einem periodischen Auftrag unter Verwendung der Ansicht für zugehörige Aufträge behandeln zu können, müssen Sie den wiederkehrenden Auftrag zunächst in Visual Studio suchen und anschließend die Ansicht für zugehörige Aufträge öffnen.

### <a name="case-1-you-have-the-url-for-the-recurring-job"></a>Fall 1: Sie verfügen über die URL für den periodischen Auftrag.

Über **Tools** > **Data Lake** > **Auftragsansicht** können Sie die Auftrags-URL zum Öffnen der Auftragsansicht in Visual Studio einfügen. Wählen Sie **Zugehörige Aufträge anzeigen**, um die entsprechende Ansicht zu öffnen.

![Link „Zugehörige Aufträge anzeigen“ in Data Lake Analytics-Tools](./media/data-lake-analytics-data-lake-tools-debug-recurring-job/view-related-job.png)
 
### <a name="case-2-you-have-the-pipeline-for-the-recurring-job-but-not-the-url"></a>Fall 2: Sie verfügen über die Pipeline für den periodischen Auftrag, aber nicht über die URL.

In Visual Studio können Sie unter **Server-Explorer > <Ihr Azure Data Lake Analytics-Konto> > Pipelines** den Pipelinebrowser öffnen. (Wenn Sie diesen Knoten im Server-Explorer nicht finden, [laden Sie hier das neueste Plug-In herunter](https://aka.ms/adltoolsvs).) 

![Auswählen des Knotens „Pipelines“](./media/data-lake-analytics-data-lake-tools-debug-recurring-job/pipeline-browser.png)

Links im Pipelinebrowser werden alle Pipelines für das Data Lake Analytics-Konto aufgelistet. Sie können die Pipelines erweitern, um alle periodischen Aufträge zu finden, und dann denjenigen auswählen, der Probleme hat. Die Ansicht „Verwandter Auftrag“ wird auf der rechten Seite geöffnet.

![Auswählen einer Pipeline und Öffnen der Ansicht „Verwandter Auftrag“](./media/data-lake-analytics-data-lake-tools-debug-recurring-job/recurring-job-view.png)

## <a name="step-2-analyze-a-statistics-report"></a>Schritt 2: Analysieren eines Statistikberichts

Eine Zusammenfassung und ein Statistikbericht werden oben in der Ansicht „Verwandter Auftrag“ angezeigt. Dort können Sie die potenzielle Ursache des Problems finden. 

1.  Im Bericht zeigt die X-Achse den Zeitpunkt der Übermittlung des Auftrags an. Ermitteln Sie mit ihrer Hilfe den nicht ordnungsgemäßen Auftrag.
2.  Verwenden Sie den Prozess im folgenden Diagramm, um Statistiken zu überprüfen und Einblicke in das Problem und die möglichen Lösungen zu erhalten.

![Prozessdiagramm zur Überprüfung von Statistiken](./media/data-lake-analytics-data-lake-tools-debug-recurring-job/recurring-job-metrics-debugging-flow.png)

## <a name="step-3-compare-the-abnormal-job-to-a-normal-job"></a>Schritt 3: Vergleichen des nicht normalen Auftrags mit einem normalen Auftrag

Sie finden alle übermittelten periodischen Aufträge in der Auftragsliste im unteren Bereich der Ansicht „Verwandter Auftrag“. Um mehr Einblicke und mögliche Lösungen zu erhalten, klicken Sie mit der rechten Maustaste auf den nicht normalen Auftrag. Verwenden Sie die Ansicht „Auftragsvergleich“, um den nicht normalen Auftrag mit einem früheren normalen Auftrag zu vergleichen.

![Kontextmenü für den Vergleich von Aufträgen](./media/data-lake-analytics-data-lake-tools-debug-recurring-job/compare-job.png)

Achten Sie auf die großen Unterschiede zwischen diesen beiden Aufträgen. Diese Unterschiede sind wahrscheinlich für die Leistungsprobleme verantwortlich. Führen Sie zur weiteren Überprüfung die Schritte in der folgenden Abbildung aus:

![Prozessdiagramm zur Überprüfung von Unterschieden zwischen Aufträgen](./media/data-lake-analytics-data-lake-tools-debug-recurring-job/recurring-job-diff-debugging-flow.png)

## <a name="next-steps"></a>Nächste Schritte

* [Lösen von Datenschiefeproblemen](data-lake-analytics-data-lake-tools-data-skew-solutions.md)
* [Debuggen von benutzerdefiniertem C#-Code für fehlerhafte U-SQL-Aufträge](data-lake-analytics-debug-u-sql-jobs.md)
