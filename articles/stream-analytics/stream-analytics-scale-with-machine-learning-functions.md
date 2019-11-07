---
title: Skalieren von Machine Learning-Funktionen in Azure Stream Analytics
description: In diesem Artikel wird beschrieben, wie Sie Stream Analytics-Aufträge mit Machine Learning-Funktionen skalieren, indem Sie die Partitionierung und Streamingeinheiten konfigurieren.
services: stream-analytics
author: jseb225
ms.author: jeanb
ms.reviewer: jasonh
ms.service: stream-analytics
ms.topic: conceptual
ms.date: 06/21/2019
ms.openlocfilehash: 28734e5eaa693ca4ee31603863b69605a1d92c88
ms.sourcegitcommit: c22327552d62f88aeaa321189f9b9a631525027c
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 11/04/2019
ms.locfileid: "73467873"
---
# <a name="scale-your-stream-analytics-job-with-azure-machine-learning-studio-classic-functions"></a>Skalieren eines Stream Analytics-Auftrags mit Azure Machine Learning Studio-Funktionen (klassisch)

In diesem Artikel wird beschrieben, wie Sie Azure Stream Analytics-Aufträge effizient skalieren, die Azure Machine Learning-Funktionen nutzen. Allgemeine Informationen zum Skalieren von Stream Analytics-Aufträgen finden Sie im Artikel [Skalieren von Aufträgen](stream-analytics-scale-jobs.md).

## <a name="what-is-an-azure-machine-learning-function-in-stream-analytics"></a>Was ist in Stream Analytics eine Azure Machine Learning-Funktion?

Eine Machine Learning-Funktion kann in Stream Analytics wie ein normaler Funktionsaufruf in der Stream Analytics-Abfragesprache verwendet werden. Im Hintergrund handelt es sich bei den Funktionsaufrufen aber um Azure Machine Learning-Webdienstanforderungen.

Sie können den Durchsatz von Machine Learning-Webdienstanforderungen verbessern, indem Sie mehrere Zeilen im selben Webdienst-API-Aufruf zusammen im Batch verarbeiten. Diese Gruppierung wird als Mini-Batch bezeichnet. Weitere Informationen finden Sie unter [Azure Machine Learning Studio-Webdienste (klassisch)](../machine-learning/studio/consume-web-services.md). Die Unterstützung für Azure Machine Learning Studio (klassisch) in Stream Analytics befindet sich in der Vorschau.

## <a name="configure-a-stream-analytics-job-with-machine-learning-functions"></a>Konfigurieren eines Stream Analytics-Auftrags mit Machine Learning-Funktionen

Die vom Stream Analytics-Auftrag verwendete Machine Learning-Funktion wird mit zwei Parametern konfiguriert:

* Der Batchgröße der Machine Learning-Funktionsaufrufe
* Der Anzahl der Streamingeinheiten (Streaming Units, SUs), die für den Stream Analytics-Auftrag bereitgestellt werden

Um die geeigneten Werte für SUs zu ermitteln, entscheiden Sie, ob Sie die Latenz des Stream Analytics-Auftrags oder den Durchsatz der einzelnen SUs optimieren möchten. SUs können einem Auftrag immer hinzugefügt werden, um den Durchsatz einer gut partitionierten Stream Analytics-Abfrage zu erhöhen. Durch zusätzliche SUs erhöhen sich aber die Kosten für die Ausführung des Auftrags.

Ermitteln Sie die *Latenztoleranz* für Ihren Stream Analytics-Auftrag. Durch eine höhere Batchgröße erhöht sich auch die Latenz Ihrer Azure Machine Learning-Anforderungen und die Latenz des Stream Analytics-Auftrags.

Eine höhere Batchgröße ermöglicht es dem Stream Analytics-Auftrag, **mehr Ereignisse** mit **derselben Anzahl** von Machine Learning-Webdienstanforderungen zu verarbeiten. Die wachsende Latenz des Machine Learning-Webdiensts verhält sich in der Regel sublinear zum Anstieg der Batchgröße. 

Es ist wichtig, in der jeweiligen Situation immer die kostengünstigste Batchgröße für einen Machine Learning-Webdienst zu ermitteln. Die Standardbatchgröße für Webdienstanforderungen beträgt 1.000 Ereignisse. Sie können die Standardgröße mit der [Stream Analytics-REST-API](https://docs.microsoft.com/previous-versions/azure/mt653706(v=azure.100) "Stream Analytics-REST-API") oder dem [PowerShell-Client für Stream Analytics](stream-analytics-monitor-and-manage-jobs-use-powershell.md) ändern.

Nachdem Sie sich für eine Batchgröße entschieden haben, können Sie die Anzahl von Streamingeinheiten (SUs) basierend auf der Anzahl von Ereignissen festlegen, die von der Funktion pro Sekunde verarbeitet werden müssen. Weitere Informationen zu Streamingeinheiten finden Sie unter [Stream Analytics-Skalierungsaufträge](stream-analytics-scale-jobs.md).

Für jeweils 6 SUs werden 20 gleichzeitige Verbindungen mit dem Machine Learning-Webdienst bereitgestellt. Allerdings werden für einen 1-SU-Auftrag und 3-SU-Aufträge 20 gleichzeitige Verbindungen bereitgestellt.  

Wenn Ihre Anwendung 200.000 Ereignisse pro Sekunde generiert und die Batchgröße 1.000 beträgt, ergibt sich eine Webdienstlatenz von 200 ms. Dieser Wert bedeutet, dass über jede Verbindung pro Sekunde fünf Anforderungen an den Machine Learning-Webdienst übermittelt werden können. Bei 20 Verbindungen kann der Stream Analytics-Auftrag 20.000 Ereignisse in 200 ms verarbeiten, also 100.000 Ereignisse pro Sekunde.

Zum Verarbeiten von 200.000 Ereignissen pro Sekunde benötigt der Stream Analytics-Auftrag 40 gleichzeitige Verbindungen und somit 12 SUs. Im folgenden Diagramm ist der Verlauf der Anforderungen vom Stream Analytics-Auftrag zum Machine Learning-Webdienst-Endpunkt dargestellt. Für 6 SUs sind jeweils maximal 20 gleichzeitige Verbindungen mit dem Machine Learning-Webdienst vorhanden.

![Skalieren von Stream Analytics mit Machine Learning-Funktionen – Beispiel mit zwei Aufträgen](./media/stream-analytics-scale-with-ml-functions/stream-analytics-scale-with-ml-functions-00.png "Skalieren von Stream Analytics mit Machine Learning-Funktionen – Beispiel mit zwei Aufträgen")

Wenn ***B*** die Batchgröße und ***L*** die Webdienstlatenz bei Batchgröße B in Millisekunden ist, beträgt der Durchsatz eines Stream Analytics-Auftrags mit ***N*** SUs:

![Skalieren von Stream Analytics mit Machine Learning-Funktionen – Formel](./media/stream-analytics-scale-with-ml-functions/stream-analytics-scale-with-ml-functions-02.png "Skalieren von Stream Analytics mit Machine Learning-Funktionen – Formel")

Sie können auch den Parameter „Max. gleichzeitige Aufrufe“ für den Machine Learning-Webdienst konfigurieren. Es wird empfohlen, diesen Parameter auf den maximalen Wert (derzeit 200) festzulegen.

Weitere Informationen zu dieser Einstellung finden Sie im [Artikel zur Skalierung für Machine Learning-Webdienste](../machine-learning/studio/scaling-webservice.md).

## <a name="example--sentiment-analysis"></a>Beispiel: Stimmungsanalyse
Das folgende Beispiel enthält einen Stream Analytics-Auftrag mit der Machine Learning-Funktion für die Stimmungsanalyse, die im [Tutorial zur Machine Learning-Integration für Stream Analytics](stream-analytics-machine-learning-integration-tutorial.md)beschrieben ist.

Die Abfrage umfasst eine einfache vollständig partitionierte Abfrage gefolgt von der Funktion **sentiment**, wie im folgenden Beispiel dargestellt:

```SQL
    WITH subquery AS (
        SELECT text, sentiment(text) as result from input
    )

    Select text, result.[Score]
    Into output
    From subquery
```

Betrachten wir nun die erforderlichen Konfigurationsschritte zum Erstellen eines Stream Analytics-Auftrags, mit dem eine Standpunktanalyse für 10.000 Tweets pro Sekunde durchgeführt wird.

Könnte der Datenverkehr von diesem Stream Analytics-Auftrag mit 1 SU bewältigt werden? Bei Verwendung der Standardbatchgröße von 1.000 können die Eingabedaten vom Auftrag verarbeitet werden. Bei der Standardlatenz des Machine Learning-Webdiensts für die Standpunktanalyse (mit einer Standardbatchgröße von 1.000) entsteht eine Latenz von nur einer Sekunde.

Die **gesamte** Latenz bzw. End-to-End-Latenz des Stream Analytics-Auftrags würde normalerweise einige Sekunden betragen. Es ist ratsam, sich diesen Stream Analytics-Auftrag genauer anzusehen, *vor allem* die Machine Learning-Funktionsaufrufe. Bei einer Batchgröße von 1.000 werden bei einem Durchsatz von 10.000 Ereignissen etwa 10 Anforderungen an den Webdienst benötigt. Auch bei nur einer SU sind genügend gleichzeitige Verbindungen vorhanden, um diesen Datenverkehrseingang abzudecken.

Wenn sich die Eingangsrate der Ereignisse um das Hundertfache erhöht, muss der Stream Analytics-Auftrag 1.000.000 Tweets pro Sekunde verarbeiten. Es gibt zwei Möglichkeiten, um die höhere Skalierung zu erreichen:

1. Erhöhen Sie die Batchgröße.
2. Partitionieren Sie den Eingabestream, um die Ereignisse parallel zu verarbeiten.

Bei der ersten Option erhöht sich die **Latenz** des Auftrags.

Bei der zweiten Option müssen mehr SUs bereitgestellt werden, damit mehr gleichzeitige Machine Learning-Webdienstanforderungen unterstützt werden. Durch die größere Anzahl von SUs erhöhen sich die **Kosten** des Auftrags.

Betrachten wir die Skalierung anhand der folgenden Latenzmessungen für die einzelnen Batchgrößen:

| Latency | Batchgröße |
| --- | --- |
| 200 ms | Batches mit maximal 1.000 Ereignissen |
| 250 ms | Batches mit 5.000 Ereignissen |
| 300 ms | Batches mit 10.000 Ereignissen |
| 500 ms | Batches mit 25.000 Ereignissen |

1. Mit der ersten Option (**keine** Bereitstellung von mehr SUs) könnte die Batchgröße auf **25.000** erhöht werden. Durch die höhere Batchgröße könnten vom Auftrag dann 1.000.000 Ereignisse mit 20 gleichzeitigen Verbindungen mit dem Machine Learning-Webdienst verarbeitet werden (bei einer Latenz von 500 ms pro Aufruf). Die zusätzliche Latenz des Stream Analytics-Auftrags würde sich also aufgrund der Anforderungen der sentiment-Funktion an die Machine Learning-Webdienstanforderungen von **200 ms** auf **500 ms** erhöhen. Die Batchgröße kann jedoch **nicht** unendlich erhöht werden, da die Machine Learning-Webdienste erfordern, dass die Nutzlast einer Anforderung maximal 4 MB beträgt. Für Webdienstanforderungen tritt nach 100 Sekunden eine Zeitüberschreitung auf.
1. Bei der zweiten Option wird die Batchgröße von 1000 beibehalten. Bei einer Webdienstlatenz von 200 ms können mit 20 gleichzeitigen Verbindungen mit dem Webdienst also jeweils Ereignisse in folgendem Umfang verarbeitet werden: 1000 × 20 × 5 Ereignisse = 100.000 pro Sekunde. Es sind also 60 SUs für den Auftrag erforderlich, damit 1.000.000 Ereignisse pro Sekunde verarbeitet werden können. Verglichen mit der ersten Option fallen für den Stream Analytics-Auftrag mehr Webdienst-Batchanforderungen an, sodass sich die Kosten erhöhen.

Unten ist eine Tabelle mit Informationen zum Durchsatz des Stream Analytics-Auftrags für unterschiedliche SUs und Batchgrößen angegeben (Anzahl von Ereignissen pro Sekunde).

| Batchgröße (ML-Latenz) | 500 (200 ms) | 1\.000 (200 ms) | 5\.000 (250 ms) | 10.000 (300 ms) | 25.000 (500 ms) |
| --- | --- | --- | --- | --- | --- |
| **1 SU** |2\.500 |5\.000 |20.000 |30.000 |50.000 |
| **3 SUs** |2\.500 |5\.000 |20.000 |30.000 |50.000 |
| **6 SUs** |2\.500 |5\.000 |20.000 |30.000 |50.000 |
| **12 SUs** |5\.000 |10.000 |40.000 |60.000 |100.000 |
| **18 SUs** |7\.500 |15.000 |60.000 |90.000 |150.000 |
| **24 SUs** |10.000 |20.000 |80.000 |120.000 |200.000 |
| **…** |… |… |… |… |… |
| **60 SUs** |25.000 |50.000 |200.000 |300.000 |500.000 |

Sie sollten nun bereits über gute Grundlagenkenntnisse verfügen und wissen, wie Machine Learning-Funktionen in Stream Analytics funktionieren. Sie wissen vermutlich auch, dass bei Stream Analytics-Aufträgen Daten aus Datenquellen abgerufen werden („Pull“) und bei jedem Vorgang dieser Art ein Batch mit Ereignissen zur Verarbeitung durch den Stream Analytics-Auftrag zurückgegeben wird. Wie wirkt sich dieses Abrufmodell auf die Machine Learning-Webdienstanforderungen aus?

Normalerweise lässt sich die Batchgröße, die wir für Machine Learning-Funktionen festlegen, nicht genau durch die Anzahl von Ereignissen teilen, die bei jedem Abrufvorgang eines Stream Analytics-Auftrags zurückgegeben werden. In diesem Fall wird der Machine Learning-Webdienst mit „Teilbatches“ aufgerufen. Durch die Verwendung von Teilbatches wird verhindert, dass ein Auftrag zusätzliche Latenzen verursacht und Ereignisse von einem zum nächsten Abruf gebündelt werden.

## <a name="new-function-related-monitoring-metrics"></a>Neue funktionsbezogene Überwachungsmetriken
Im Überwachungsbereich eines Stream Analytics-Auftrags wurden drei zusätzliche funktionsbezogene Metriken hinzugefügt. Dies sind **FUNKTIONSANFORDERUNGEN**, **FUNKTIONSEREIGNISSE** und **FEHLERHAFTE FUNKTIONSANFORDERUNGEN**, wie in der folgenden Grafik dargestellt.

![Skalieren von Stream Analytics mit Machine Learning-Funktionen – Metriken](./media/stream-analytics-scale-with-ml-functions/stream-analytics-scale-with-ml-functions-01.png "Skalieren von Stream Analytics mit Machine Learning-Funktionen – Metriken")

Diese sind wie folgt definiert:

**FUNKTIONSANFORDERUNGEN**: Die Anzahl von Funktionsanforderungen.

**FUNKTIONSEREIGNISSE**: Die Anzahl von Ereignissen in den Funktionsanforderungen.

**FEHLER BEI FUNKTIONSANFORDERUNGEN**: Die Anzahl von Funktionsanforderungen mit Fehlern.

## <a name="key-takeaways"></a>Wesentliche Punkte

Beim Skalieren eines Stream Analytics-Auftrags mit Machine Learning-Funktionen sollten die folgenden Faktoren beachtet werden:

1. Die Eingangsrate der Ereignisse
2. Die tolerierte Latenz für den ausgeführten Stream Analytics-Auftrag (und somit die Batchgröße der Machine Learning-Webdienstanforderungen)
3. Die bereitgestellten Stream Analytics-Streamingeinheiten (SUs) und die Anzahl von Machine Learning-Webdienstanforderungen (zusätzliche funktionsbezogene Kosten)

Als Beispiel wurde eine vollständig partitionierte Stream Analytics-Abfrage verwendet. Falls Sie eine komplexere Abfrage benötigen, ist das [Azure Stream Analytics-Forum](https://social.msdn.microsoft.com/Forums/azure/home?forum=AzureStreamAnalytics) eine hervorragende Ressource, um vom Stream Analytics-Team weitere Hilfe zu erhalten.

## <a name="next-steps"></a>Nächste Schritte
Weitere Informationen zu Stream Analytics finden Sie unter:

* [Erste Schritte mit Azure Stream Analytics](stream-analytics-real-time-fraud-detection.md)
* [Skalieren von Azure Stream Analytics-Aufträgen](stream-analytics-scale-jobs.md)
* [Stream Analytics Query Language Reference (in englischer Sprache)](https://docs.microsoft.com/stream-analytics-query/stream-analytics-query-language-reference)
* [Referenz zur Azure Stream Analytics-Verwaltungs-REST-API](https://msdn.microsoft.com/library/azure/dn835031.aspx)
