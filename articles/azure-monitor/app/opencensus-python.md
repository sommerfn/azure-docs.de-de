---
title: Überwachen von Python-Anwendungen mit Azure Monitor (Vorschau) | Microsoft-Dokumentation
description: Enthält eine Anleitung zum Verknüpfen von OpenCensus Python mit Azure Monitor.
ms.service: azure-monitor
ms.subservice: application-insights
ms.topic: conceptual
author: reyang
ms.author: reyang
ms.date: 10/11/2019
ms.reviewer: mbullwin
ms.openlocfilehash: 7fb436ef8d915898bc8f36dd10766e71f63e4a59
ms.sourcegitcommit: f4d8f4e48c49bd3bc15ee7e5a77bee3164a5ae1b
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 11/04/2019
ms.locfileid: "73575568"
---
# <a name="set-up-azure-monitor-for-your-python-application-preview"></a>Einrichten von Azure Monitor für Ihre Python-Anwendung (Vorschau)

Azure Monitor unterstützt durch die Integration mit [OpenCensus](https://opencensus.io) die verteilte Ablaufverfolgung, Metrikerfassung und Protokollierung für Python-Anwendungen. In diesem Artikel werden der Prozess zur Einrichtung von OpenCensus für Python und das Senden Ihrer Überwachungsdaten an Azure Monitor beschrieben.

## <a name="prerequisites"></a>Voraussetzungen

- Ein Azure-Abonnement. Wenn Sie kein Azure-Abonnement besitzen, können Sie ein [kostenloses Konto](https://azure.microsoft.com/free/) erstellen, bevor Sie beginnen.
- Python-Installation. In diesem Artikel wird [Python 3.7.0](https://www.python.org/downloads/) verwendet, aber auch frühere Versionen können mit kleineren Änderungen wahrscheinlich verwendet werden.

## <a name="sign-in-to-the-azure-portal"></a>Anmelden beim Azure-Portal

Melden Sie sich beim [Azure-Portal](https://portal.azure.com/) an.

## <a name="create-an-application-insights-resource-in-azure-monitor"></a>Erstellen einer Application Insights-Ressource in Azure Monitor

Zuerst müssen Sie eine Application Insights-Ressource in Azure Monitor erstellen, die einen Instrumentierungsschlüssel (ikey) generiert. Der ikey wird zum Konfigurieren des OpenCensus SDK für das Senden von Telemetriedaten an Azure Monitor verwendet.

1. Klicken Sie auf **Ressource erstellen** > **Entwicklertools** > **Application Insights**.

   ![Hinzufügen einer Application Insights-Ressource](./media/opencensus-python/0001-create-resource.png)

1. Ein Konfigurationsfeld wird angezeigt. Füllen Sie die Eingabefelder anhand der Informationen in der folgenden Tabelle aus.

   | Einstellung        | Wert           | BESCHREIBUNG  |
   | ------------- |:-------------|:-----|
   | **Name**      | Global eindeutiger Wert | Der Name, der die zu überwachende App identifiziert. |
   | **Ressourcengruppe**     | myResourceGroup      | Der Name der neuen Ressourcengruppe, die Application Insights-Daten hosten soll. |
   | **Location** | East US | Ein Standort in Ihrer Nähe oder in der Nähe des Standorts, an dem Ihre App gehostet wird. |

1. Klicken Sie auf **Erstellen**.

## <a name="instrument-with-opencensus-python-sdk-for-azure-monitor"></a>Instrumentieren mit dem OpenCensus Python SDK für Azure Monitor

Installieren Sie OpenCensus Azure Monitor Exporters:

```console
python -m pip install opencensus-ext-azure
```

Eine vollständige Liste der Pakete und Integrationen finden Sie unter [OpenCensus-Pakete](https://docs.microsoft.com/azure/azure-monitor/app/nuget#common-packages-for-python-using-opencensus).

> [!NOTE]
> Beim Befehl `python -m pip install opencensus-ext-azure` wird davon ausgegangen, dass Sie eine `PATH`-Umgebungsvariable für die Python-Installation festgelegt haben. Wenn Sie diese Variable nicht konfiguriert haben, müssen Sie den vollständigen Verzeichnispfad zum Speicherort Ihrer ausführbaren Python-Datei angeben. Das Ergebnis ist ein Befehl, der Folgendem ähnelt: `C:\Users\Administrator\AppData\Local\Programs\Python\Python37-32\python.exe -m pip install opencensus-ext-azure`

Das SDK verwendet drei Azure Monitor-Exportprogramme, um unterschiedliche Arten von Telemetriedaten an Azure Monitor zu senden: Ablaufverfolgung, Metriken und Protokolle. Weitere Informationen zu diesen Telemetrietypen finden Sie in der [Übersicht über die Datenplattform](https://docs.microsoft.com/azure/azure-monitor/platform/data-platform). Befolgen Sie die nachstehenden Anweisungen, um diese Telemetrietypen über die drei Exportprogramme zu senden.

### <a name="trace"></a>Trace

1. Zuerst generieren wir lokal einige Ablaufverfolgungsdaten. Geben Sie in Python IDLE oder einem Editor Ihrer Wahl den unten angegebenen Code ein.

    ```python
    from opencensus.trace.samplers import ProbabilitySampler
    from opencensus.trace.tracer import Tracer

    tracer = Tracer(sampler=ProbabilitySampler(1.0))

    def valuePrompt():
        with tracer.span(name="test") as span:
            line = input("Enter a value: ")
            print(line)

    def main():
        while True:
            valuePrompt()

    if __name__ == "__main__":
        main()
    ```

2. Durch Ausführen des Codes werden Sie wiederholt aufgefordert, einen Wert einzugeben. Mit jedem Eintrag wird der Wert in die Shell gedruckt, und vom OpenCensus Python-Modul wird ein entsprechendes `SpanData`-Element generiert. Das OpenCensus-Projekt definiert eine [Ablaufverfolgung als Struktur von Spannen (Spans)](https://opencensus.io/core-concepts/tracing/).
    
    ```
    Enter a value: 4
    4
    [SpanData(name='test', context=SpanContext(trace_id=8aa41bc469f1a705aed1bdb20c342603, span_id=None, trace_options=TraceOptions(enabled=True), tracestate=None), span_id='15ac5123ac1f6847', parent_span_id=None, attributes=BoundedDict({}, maxlen=32), start_time='2019-06-27T18:21:22.805429Z', end_time='2019-06-27T18:21:44.933405Z', child_span_count=0, stack_trace=None, annotations=BoundedList([], maxlen=32), message_events=BoundedList([], maxlen=128), links=BoundedList([], maxlen=32), status=None, same_process_as_parent_span=None, span_kind=0)]
    Enter a value: 25
    25
    [SpanData(name='test', context=SpanContext(trace_id=8aa41bc469f1a705aed1bdb20c342603, span_id=None, trace_options=TraceOptions(enabled=True), tracestate=None), span_id='2e512f846ba342de', parent_span_id=None, attributes=BoundedDict({}, maxlen=32), start_time='2019-06-27T18:21:44.933405Z', end_time='2019-06-27T18:21:46.156787Z', child_span_count=0, stack_trace=None, annotations=BoundedList([], maxlen=32), message_events=BoundedList([], maxlen=128), links=BoundedList([], maxlen=32), status=None, same_process_as_parent_span=None, span_kind=0)]
    Enter a value: 100
    100
    [SpanData(name='test', context=SpanContext(trace_id=8aa41bc469f1a705aed1bdb20c342603, span_id=None, trace_options=TraceOptions(enabled=True), tracestate=None), span_id='f3f9f9ee6db4740a', parent_span_id=None, attributes=BoundedDict({}, maxlen=32), start_time='2019-06-27T18:21:46.157732Z', end_time='2019-06-27T18:21:47.269583Z', child_span_count=0, stack_trace=None, annotations=BoundedList([], maxlen=32), message_events=BoundedList([], maxlen=128), links=BoundedList([], maxlen=32), status=None, same_process_as_parent_span=None, span_kind=0)]
    ```

3. Das Eingeben von Werten ist zu Demonstrationszwecken zwar hilfreich, aber wir möchten eigentlich `SpanData` für Azure Monitor ausgeben. Ändern Sie Ihren Code aus dem vorherigen Schritt basierend auf dem folgenden Codebeispiel:

    ```python
    from opencensus.ext.azure.trace_exporter import AzureExporter
    from opencensus.trace.samplers import ProbabilitySampler
    from opencensus.trace.tracer import Tracer
    
    # TODO: replace the all-zero GUID with your instrumentation key.
    tracer = Tracer(
        exporter=AzureExporter(
            connection_string='InstrumentationKey=00000000-0000-0000-0000-000000000000'),
        ),
        sampler=ProbabilitySampler(1.0),
    )

    def valuePrompt():
        with tracer.span(name="test") as span:
            line = input("Enter a value: ")
            print(line)

    def main():
        while True:
            valuePrompt()

    if __name__ == "__main__":
        main()
    ```

4. Wenn Sie das Python-Skript jetzt ausführen, sollten Sie weiterhin zur Eingabe von Werten aufgefordert werden, doch wird nur der Wert in die Shell gedruckt. Das erstellte `SpanData`-Element wird an Azure Monitor gesendet. Sie finden die ausgegebenen Span-Daten unter `dependencies`.

5. Informationen zur Stichprobenerstellung in OpenCensus finden Sie unter [Stichprobenerstellung in OpenCensus](https://docs.microsoft.com/azure/azure-monitor/app/sampling#configuring-fixed-rate-sampling-in-opencensus-python).

6. Ausführliche Informationen zur Telemetriekorrelation in den Ablaufverfolgungsdaten finden Sie unter [Telemetriekorrelation](https://docs.microsoft.com/azure/azure-monitor/app/correlation#telemetry-correlation-in-opencensus-python) für OpenCensus.

### <a name="metrics"></a>metrics

1. Zuerst generieren wir einige lokale Metrikdaten. Wir erstellen eine einfache Metrik, um nachzuverfolgen, wie häufig der Benutzer die EINGABETASTE betätigt.

    ```python
    from datetime import datetime
    from opencensus.stats import aggregation as aggregation_module
    from opencensus.stats import measure as measure_module
    from opencensus.stats import stats as stats_module
    from opencensus.stats import view as view_module
    from opencensus.tags import tag_map as tag_map_module

    stats = stats_module.stats
    view_manager = stats.view_manager
    stats_recorder = stats.stats_recorder
    
    prompt_measure = measure_module.MeasureInt("prompts",
                                               "number of prompts",
                                               "prompts")
    prompt_view = view_module.View("prompt view",
                                   "number of prompts",
                                   [],
                                   prompt_measure,
                                   aggregation_module.CountAggregation())
    view_manager.register_view(prompt_view)
    mmap = stats_recorder.new_measurement_map()
    tmap = tag_map_module.TagMap()

    def prompt():
        input("Press enter.")
        mmap.measure_int_put(prompt_measure, 1)
        mmap.record(tmap)
        metrics = list(mmap.measure_to_view_map.get_metrics(datetime.utcnow()))
        print(metrics[0].time_series[0].points[0])

    def main():
        while True:
            prompt()

    if __name__ == "__main__":
        main()
    ```
2. Bei Ausführung des Codes werden Sie wiederholt aufgefordert, die EINGABETASTE zu drücken. Es wird eine Metrik erstellt, um die Anzahl von Betätigungen der EINGABETASTE nachzuverfolgen. Bei jedem Eintrag wird der Wert erhöht, und die Metrikinformationen werden in der Konsole angezeigt. Die Informationen umfassen den aktuellen Wert und den aktuellen Zeitstempel für den Aktualisierungszeitpunkt der Metrik.

    ```
    Press enter.
    Point(value=ValueLong(5), timestamp=2019-10-09 20:58:04.930426)
    Press enter.
    Point(value=ValueLong(6), timestamp=2019-10-09 20:58:06.570167)
    Press enter.
    Point(value=ValueLong(7), timestamp=2019-10-09 20:58:07.138614)
    ```

3. Das Eingeben von Werten ist zu Demonstrationszwecken zwar hilfreich, aber wir möchten eigentlich die Metrikdaten für Azure Monitor ausgeben. Ändern Sie Ihren Code aus dem vorherigen Schritt basierend auf dem folgenden Codebeispiel:

    ```python
    from datetime import datetime
    from opencensus.ext.azure import metrics_exporter
    from opencensus.stats import aggregation as aggregation_module
    from opencensus.stats import measure as measure_module
    from opencensus.stats import stats as stats_module
    from opencensus.stats import view as view_module
    from opencensus.tags import tag_map as tag_map_module

    stats = stats_module.stats
    view_manager = stats.view_manager
    stats_recorder = stats.stats_recorder
    
    prompt_measure = measure_module.MeasureInt("prompts",
                                               "number of prompts",
                                               "prompts")
    prompt_view = view_module.View("prompt view",
                                   "number of prompts",
                                   [],
                                   prompt_measure,
                                   aggregation_module.CountAggregation())
    view_manager.register_view(prompt_view)
    mmap = stats_recorder.new_measurement_map()
    tmap = tag_map_module.TagMap()

    # TODO: replace the all-zero GUID with your instrumentation key.
    exporter = metrics_exporter.new_metrics_exporter(
        connection_string='InstrumentationKey=00000000-0000-0000-0000-000000000000')
    )
    view_manager.register_exporter(exporter)

    def prompt():
        input("Press enter.")
        mmap.measure_int_put(prompt_measure, 1)
        mmap.record(tmap)
        metrics = list(mmap.measure_to_view_map.get_metrics(datetime.utcnow()))
        print(metrics[0].time_series[0].points[0])

    def main():
        while True:
            prompt()

    if __name__ == "__main__":
        main()
    ```

4. Das Exportprogramm sendet die Metrikdaten in einem festen Intervall an Azure Monitor. Die Standardeinstellung ist alle 15 Sekunden. Wir verfolgen nur eine einzelne Metrik. Diese Metrikdaten werden also mit dem jeweiligen Wert und Zeitstempel in jedem Intervall gesendet. Sie finden die Daten unter `customMetrics`.

### <a name="logs"></a>Protokolle

1. Zuerst generieren wir einige lokale Protokolldaten.

    ```python
    import logging

    logger = logging.getLogger(__name__)

    def valuePrompt():
        line = input("Enter a value: ")
        logger.warning(line)

    def main():
        while True:
            valuePrompt()

    if __name__ == "__main__":
        main()
    ```

2.  Der Code fordert fortlaufend zur Eingabe eines Werts auf. Es wird ein Protokolleintrag für jeden eingegebenen Wert ausgegeben.

    ```
    Enter a value: 24
    24
    Enter a value: 55
    55
    Enter a value: 123
    123
    Enter a value: 90
    90
    ```

3. Das Eingeben von Werten ist zu Demonstrationszwecken zwar hilfreich, aber wir möchten eigentlich die Metrikdaten für Azure Monitor ausgeben. Ändern Sie Ihren Code aus dem vorherigen Schritt basierend auf dem folgenden Codebeispiel:

    ```python
    import logging
    from opencensus.ext.azure.log_exporter import AzureLogHandler
    
    logger = logging.getLogger(__name__)
    
    # TODO: replace the all-zero GUID with your instrumentation key.
    logger.addHandler(AzureLogHandler(
        connection_string='InstrumentationKey=00000000-0000-0000-0000-000000000000')
    )
    
    def valuePrompt():
        line = input("Enter a value: ")
        logger.warning(line)
    
    def main():
        while True:
            valuePrompt()
    
    if __name__ == "__main__":
        main()
    ```

4. Das Exportprogramm sendet Protokolldaten an Azure Monitor. Sie finden die Daten unter `traces`.

5. Ausführliche Informationen dazu, wie Sie die Protokolle um Daten im Ablaufverfolgungskontext erweitern können, finden Sie unter [Integration von Protokollen](https://docs.microsoft.com/azure/azure-monitor/app/correlation#logs-correlation) für OpenCensus Python.

## <a name="start-monitoring-in-the-azure-portal"></a>Starten der Überwachung im Azure-Portal

1. Sie können jetzt im Azure-Portal erneut den Bereich **Übersicht** für Application Insights öffnen, um Details zu Ihrer aktuell ausgeführten Anwendung anzuzeigen. Wählen Sie **Live Metrics Stream** aus.

   ![Screenshot des Übersichtsbereichs mit „Live Metrics Stream“ in einem roten Kasten](./media/opencensus-python/0005-overview-live-metrics-stream.png)

2. Kehren Sie zum Bereich **Übersicht** zurück. Wählen Sie **Anwendungsübersicht** aus, um ein visuelles Layout der Abhängigkeitsbeziehungen und der zeitlichen Steuerung der Aufrufe zwischen den Komponenten Ihrer Anwendung zu erhalten.

   ![Screenshot einer einfachen Anwendungsübersicht](./media/opencensus-python/0007-application-map.png)

   Da nur ein Methodenaufruf überwacht wurde, ist die Anwendungsübersicht nicht so interessant. Eine Anwendungsübersicht kann jedoch skaliert werden, um wesentlich mehr verteilte Anwendungen zu visualisieren:

   ![Anwendungszuordnung](media/opencensus-python/application-map.png)

3. Wählen Sie **Leistung untersuchen** aus, um die Leistung detailliert zu analysieren und die Hauptursache von Leistungseinbußen zu ermitteln.

   ![Screenshot der Leistungsdetails](./media/opencensus-python/0008-performance.png)

4. Zum Öffnen einer umfassenden Anzeige von Transaktionsdetails wählen Sie **Beispiele** und dann eines der im rechten Bereich angezeigten Beispiele aus. 

   Von unserer Beispiel-App wird zwar nur ein einzelnes Ereignis angezeigt, eine komplexere Anwendung ermöglicht Ihnen jedoch, die End-to-End-Transaktion bis zur Ebene der Aufrufliste eines einzelnen Ereignisses zu untersuchen.

   ![Screenshot der Schnittstelle der End-to-End-Transaktion](./media/opencensus-python/0009-end-to-end-transaction.png)

## <a name="view-your-data-with-queries"></a>Anzeigen Ihrer Daten mit Abfragen

Sie können die von Ihrer Anwendung gesendeten Telemetriedaten über die Registerkarte **Protokolle (Analytics)** anzeigen.

![Screenshot des Übersichtsbereichs mit „Protokolle (Analytics)“ in einem roten Kasten](./media/opencensus-python/0010-logs-query.png)

In der Liste unter **Aktiv** ist Folgendes angegeben:

- Für Telemetriedaten, die mit dem Exportprogramm der Azure Monitor-Ablaufverfolgung gesendet werden, werden eingehende Anforderungen unter `requests` angezeigt. Ausgehende bzw. in Bearbeitung befindliche Anforderungen werden unter `dependencies` angezeigt.
- Für Telemetriedaten, die mit dem Exportprogramm für Azure Monitor-Metriken gesendet werden, werden die gesendeten Metriken unter `customMetrics` angezeigt.
- Für Telemetriedaten, die mit dem Exportprogramm für Azure Monitor-Protokolle gesendet werden, werden die Protokolle unter `traces` angezeigt. Ausnahmen werden unter `exceptions` angezeigt.

Ausführlichere Informationen zur Verwendung von Abfragen und Protokollen finden Sie unter [Protokolle in Azure Monitor](https://docs.microsoft.com/azure/azure-monitor/platform/data-platform-logs).

## <a name="learn-more-about-opencensus-for-python"></a>Weitere Informationen zu OpenCensus für Python

* [OpenCensus Python auf GitHub](https://github.com/census-instrumentation/opencensus-python)
* [Anpassung](https://github.com/census-instrumentation/opencensus-python/blob/master/README.rst#customization)
* [Flask-Integration](https://github.com/census-instrumentation/opencensus-python/tree/master/contrib/opencensus-ext-flask)
* [Django-Integration](https://github.com/census-instrumentation/opencensus-python/tree/master/contrib/opencensus-ext-django)
* [MySQL-Integration](https://github.com/census-instrumentation/opencensus-python/tree/master/contrib/opencensus-ext-mysql)
* [PostgreSQL](https://github.com/census-instrumentation/opencensus-python/tree/master/contrib/opencensus-ext-postgresql)

## <a name="next-steps"></a>Nächste Schritte

* [Anwendungszuordnung](./../../azure-monitor/app/app-map.md)
* [End-to-End-Leistungsüberwachung](./../../azure-monitor/learn/tutorial-performance.md)

### <a name="alerts"></a>Alerts

* [Verfügbarkeitstests](../../azure-monitor/app/monitor-web-app-availability.md): Erstellen Sie Tests, um sicherzustellen, dass Ihre Website im Web sichtbar ist.
* [Intelligente Diagnose](../../azure-monitor/app/proactive-diagnostics.md): Diese Tests werden automatisch ausgeführt, sodass Sie keinerlei Einrichtungsschritte ausführen müssen. Sie werden darüber benachrichtigt, ob für Ihre App eine ungewöhnlich hohe Zahl von Anforderungen mit Fehlern vorliegt.
* [Metrikwarnungen](../../azure-monitor/app/alerts.md): Richten Sie Warnungen ein, damit Sie gewarnt werden, wenn für eine Metrik ein Schwellenwert überschritten wird. Sie können diese für benutzerdefinierte Metriken festlegen, die Sie in Ihrer App codieren.
