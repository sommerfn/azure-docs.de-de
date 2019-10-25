---
title: Überwachen von Python-Anwendungen mit Azure Monitor (Vorschau) | Microsoft-Dokumentation
description: Enthält eine Anleitung zum Verknüpfen von OpenCensus Python mit Azure Monitor.
services: application-insights
keywords: ''
author: reyang
ms.author: reyang
ms.date: 10/11/2019
ms.service: application-insights
ms.topic: conceptual
ms.reviewer: mbullwin
manager: carmonm
ms.openlocfilehash: ed61cb1bc88c48fe89c4a9390f04747749bd48c5
ms.sourcegitcommit: 1d0b37e2e32aad35cc012ba36200389e65b75c21
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 10/15/2019
ms.locfileid: "72329478"
---
# <a name="set-up-azure-monitor-for-your-python-application-preview"></a>Einrichten von Azure Monitor für Ihre Python-Anwendung (Vorschau)

Azure Monitor unterstützt durch die Integration mit [OpenCensus](https://opencensus.io) die verteilte Ablaufverfolgung, Metrikerfassung und Protokollierung für Python-Anwendungen. In diesem Artikel wird Schritt für Schritt der Prozess zur Einrichtung von OpenCensus für Python und die Weiterleitung Ihrer Überwachungsdaten an Azure Monitor beschrieben.

## <a name="prerequisites"></a>Voraussetzungen

- Sie benötigen ein Azure-Abonnement.
- Python muss installiert sein. In diesem Artikel wird [Python 3.7.0](https://www.python.org/downloads/) verwendet, aber auch frühere Versionen können mit kleineren Anpassungen ggf. verwendet werden.

Wenn Sie kein Azure-Abonnement besitzen, können Sie ein [kostenloses Konto](https://azure.microsoft.com/free/) erstellen, bevor Sie beginnen.

## <a name="sign-in-to-the-azure-portal"></a>Melden Sie sich auf dem Azure-Portal an.

Melden Sie sich beim [Azure-Portal](https://portal.azure.com/) an.

## <a name="create-application-insights-resource-in-azure-monitor"></a>Erstellen einer Application Insights-Ressource in Azure Monitor

Zuerst müssen Sie eine Application Insights-Ressource in Azure Monitor erstellen, die einen Instrumentierungsschlüssel (ikey) generiert. Der ikey wird zum Konfigurieren des OpenCensus SDK für das Senden von Telemetriedaten an Azure Monitor verwendet.

1. Klicken Sie auf **Ressource erstellen** > **Entwicklertools** > **Application Insights**.

   ![Hinzufügen einer Application Insights-Ressource](./media/opencensus-python/0001-create-resource.png)

   Ein Dialogfeld für die Konfiguration wird geöffnet. Füllen Sie die Eingabefelder anhand der Informationen in der folgenden Tabelle aus:

    | Einstellungen        | Wert           | BESCHREIBUNG  |
   | ------------- |:-------------|:-----|
   | **Name**      | Global eindeutiger Wert | Der Name, der die zu überwachende App identifiziert. |
   | **Ressourcengruppe**     | myResourceGroup      | Der Name der neuen Ressourcengruppe, die Application Insights-Daten hosten soll. |
   | **Location** | East US | Wählen Sie einen Standort in Ihrer Nähe oder in der Nähe des Standorts, in dem Ihre App gehostet wird. |

2. Klicken Sie auf **Create**.

## <a name="instrumenting-with-opencensus-python-sdk-for-azure-monitor"></a>Instrumentieren mit dem OpenCensus Python SDK für Azure Monitor

1. Installieren Sie die OpenCensus Azure Monitor Exporters:

    ```console
    python -m pip install opencensus-ext-azure
    ```

    > [!NOTE]
    > `python -m pip install opencensus-ext-azure` geht davon aus, dass Sie eine PATH-Umgebungsvariable für die Python-Installation festgelegt haben. Wenn Sie diese nicht konfiguriert haben, müssen Sie den vollständigen Verzeichnispfad zu Ihrer ausführbaren Python-Datei angeben, wodurch der Befehl in etwa so aussehen würde: `C:\Users\Administrator\AppData\Local\Programs\Python\Python37-32\python.exe -m pip install opencensus-ext-azure`.

2. Vom SDK werden drei Azure Monitor-Exportprogramme genutzt, um unterschiedliche Arten von Telemetriedaten an Azure Monitor zu senden: Ablaufverfolgung, Metriken und Protokolle. Ausführlichere Informationen zu diesen unterschiedlichen Typen finden Sie auf der Seite mit der [Übersicht über die Azure Monitor-Datenplattform](https://docs.microsoft.com/azure/azure-monitor/platform/data-platform). Befolgen Sie die unten angegebene Anleitung, um diese unterschiedlichen Typen mit den drei Exportoptionen zu senden.

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

2. Durch Ausführen des Codes werden Sie wiederholt aufgefordert, einen Wert einzugeben. Mit jedem Eintrag wird der Wert in die Shell gedruckt, und vom OpenCensus Python-Modul wird ein entsprechendes **SpanData**-Element generiert. Das OpenCensus-Projekt definiert eine [_Ablaufverfolgung als Struktur von Spannen (Spans)_ ](https://opencensus.io/core-concepts/tracing/).
    
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

3. Dies ist zu Demonstrationszwecken zwar hilfreich, aber wir möchten eigentlich `SpanData` für Azure Monitor ausgeben. Ändern Sie Ihren Code aus dem vorherigen Schritt basierend auf dem folgenden Codebeispiel:

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

4. Wenn Sie das Python-Skript jetzt ausführen, sollten Sie weiterhin aufgefordert werden, Werte einzugeben, aber jetzt wird nur der Wert in der Shell ausgegeben. Das erstellte `SpanData`-Element wird an Azure Monitor gesendet. Sie finden die ausgegebenen Span-Daten unter `dependencies`.

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
2. Bei Ausführung des Codes werden Sie wiederholt aufgefordert, die EINGABETASTE zu drücken. Es wird eine Metrik erstellt, um die Anzahl von Betätigungen der EINGABETASTE nachzuverfolgen. Bei jedem Eintrag wird der Wert inkrementiert, und die Metrikinformationen – der aktuelle Wert und der aktuelle Zeitstempel mit dem Aktualisierungszeitpunkt der Metrik – werden in der Konsole angezeigt.

    ```
    Press enter.
    Point(value=ValueLong(5), timestamp=2019-10-09 20:58:04.930426)
    Press enter.
    Point(value=ValueLong(6), timestamp=2019-10-09 20:58:06.570167)
    Press enter.
    Point(value=ValueLong(7), timestamp=2019-10-09 20:58:07.138614)
    ```

3. Dies ist zu Demonstrationszwecken zwar hilfreich, aber wir möchten eigentlich die Metrikdaten für Azure Monitor ausgeben. Ändern Sie Ihren Code aus dem vorherigen Schritt basierend auf dem folgenden Codebeispiel:

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

4. Das Exportprogramm sendet die Metrikdaten nach einem bestimmten Intervall an Azure Monitor. Die Standardeinstellung ist hierbei „alle 15 Sekunden“. Wir verfolgen nur eine Metrik nach. Diese Metrikdaten werden mit dem jeweiligen Wert und Zeitstempel also nach jedem Intervall gesendet. Sie finden die Daten unter `customMetrics`.

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

2.  Vom Code wird fortlaufend eine Aufforderung zum Eingeben eines Werts angezeigt. Für jeden Wert, der eingegeben wird, wird ein Protokolleintrag ausgegeben.

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

3. Dies ist zu Demonstrationszwecken zwar hilfreich, aber wir möchten eigentlich die Metrikdaten für Azure Monitor ausgeben. Ändern Sie Ihren Code aus dem vorherigen Schritt basierend auf dem folgenden Codebeispiel:

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

## <a name="start-monitoring-in-the-azure-portal"></a>Starten der Überwachung im Azure-Portal

1. Sie können jetzt im Azure-Portal erneut die Seite **Übersicht** für Application Insights öffnen, um Details zu Ihrer aktuell ausgeführten Anwendung anzuzeigen. Wählen Sie **Live Metrics Stream** aus.

   ![Screenshot des Bereichs "Übersicht" mit Live Metrics Stream in einem roten Kasten](./media/opencensus-python/0005-overview-live-metrics-stream.png)

2. Navigieren Sie zurück zur Seite **Übersicht**, wählen Sie **Anwendungsübersicht** aus, um ein visuelles Layout der Abhängigkeitsbeziehungen und der zeitlichen Steuerung der Aufrufe zwischen den Komponenten Ihrer Anwendung zu erhalten.

    ![Screenshot der Basisanwendungsübersicht](./media/opencensus-python/0007-application-map.png)

    Da nur ein Methodenaufruf überwacht wurde, ist die Anwendungsübersicht nicht so interessant. Die Anwendungsübersicht kann jedoch skaliert werden, um wesentlich mehr verteilte Anwendungen zu visualisieren:

   ![Anwendungszuordnung](media/opencensus-python/application-map.png)

3. Wählen Sie **Leistung untersuchen** aus, um detaillierte Leistungsanalysen durchzuführen und die Hauptursache von Leistungseinbußen zu ermitteln.

    ![Screenshot des Leistungsbereichs](./media/opencensus-python/0008-performance.png)

4. Indem Sie **Beispiele** auswählen und dann klicken auf eines der im rechten Bereich angezeigten Beispiele klicken, wird die Detailansicht der End-to-End-Transaktionen gestartet. Von unserer Beispiel-App wird zwar nur ein einzelnes Ereignis angezeigt, eine komplexere Anwendung ermöglicht Ihnen jedoch, die End-to-End-Transaktion bis zur Ebene der Aufrufliste eines einzelnen Ereignisses zu untersuchen.

     ![Screenshot der Schnittstelle der End-to-End-Transaktion](./media/opencensus-python/0009-end-to-end-transaction.png)

## <a name="view-your-data-with-queries"></a>Anzeigen Ihrer Daten mit Abfragen

1. Sie können die Telemetriedaten anzeigen, die von Ihrer Anwendung über die Registerkarte „Protokolle (Analytics)“ gesendet wurden.

    ![Screenshot: Übersichtsbereich mit Auswahl von „Protokolle (Analytics)“ in roter Umrandung](./media/opencensus-python/0010-logs-query.png)

2. Für Telemetriedaten, die mit dem Exportprogramm der Azure Monitor-Ablaufverfolgung gesendet werden, werden eingehende Anforderungen unter `requests` und ausgehende bzw. in Bearbeitung befindliche Anforderungen unter `dependencies` angezeigt.

3. Für Telemetriedaten, die mit dem Exportprogramm für Azure Monitor-Metriken gesendet werden, werden die gesendeten Metriken unter `customMetrics` angezeigt.

4. Für Telemetriedaten, die mit dem Exportprogramm für Azure Monitor-Protokolle gesendet werden, werden die Protokolle unter `traces` und Ausnahmen unter `exceptions` angezeigt.

5. Ausführlichere Informationen zur Verwendung von Abfragen und Protokollen finden Sie unter [Protokolle in Azure Monitor](https://docs.microsoft.com/azure/azure-monitor/platform/data-platform-logs).

## <a name="opencensus-for-python"></a>OpenCensus für Python

* [OpenCensus Python auf GitHub](https://github.com/census-instrumentation/opencensus-python)
* [Anpassung](https://github.com/census-instrumentation/opencensus-python/blob/master/README.rst#customization)
* [Flask-Integration](https://github.com/census-instrumentation/opencensus-python/tree/master/contrib/opencensus-ext-flask)
* [Django-Integration](https://github.com/census-instrumentation/opencensus-python/tree/master/contrib/opencensus-ext-django)
* [MySQL-Integration](https://github.com/census-instrumentation/opencensus-python/tree/master/contrib/opencensus-ext-mysql)
* [PostgreSQL](https://github.com/census-instrumentation/opencensus-python/tree/master/contrib/opencensus-ext-postgresql)

## <a name="next-steps"></a>Nächste Schritte

* [API-Zusammenfassung](./../../azure-monitor/app/api-custom-events-metrics.md)
* [Anwendungszuordnung](./../../azure-monitor/app/app-map.md)
* [End-to-End-Leistungsüberwachung](./../../azure-monitor/learn/tutorial-performance.md)

### <a name="alerts"></a>Alerts

* [Verfügbarkeitstests](../../azure-monitor/app/monitor-web-app-availability.md): Erstellen Sie Tests, um sicherzustellen, dass Ihre Website im Web sichtbar ist.
* [Intelligente Diagnose](../../azure-monitor/app/proactive-diagnostics.md): Diese Tests werden automatisch ausgeführt, sodass Sie keinerlei Einrichtungsschritte ausführen müssen. Sie werden darüber benachrichtigt, ob für Ihre App eine ungewöhnlich hohe Zahl von Anforderungen mit Fehlern vorliegt.
* [Metrikwarnungen](../../azure-monitor/app/alerts.md): Richten Sie Warnungen ein, damit Sie gewarnt werden, wenn für eine Metrik ein Schwellenwert überschritten wird. Sie können diese für benutzerdefinierte Metriken festlegen, die Sie in Ihrer App codieren.