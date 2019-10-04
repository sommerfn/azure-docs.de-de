---
title: Überwachen von Python-Anwendungen mit Azure Application Insights | Microsoft-Dokumentation
description: Anweisungen zur Verknüpfung von OpenCensus Python mit Application Insights
services: application-insights
keywords: ''
author: reyang
ms.author: reyang
ms.date: 07/02/2019
ms.service: application-insights
ms.topic: conceptual
ms.reviewer: mbullwin
manager: carmonm
ms.openlocfilehash: 2c043ad793dcf5e59eaf460d1ec4aa7a3b48810d
ms.sourcegitcommit: 5bdd50e769a4d50ccb89e135cfd38b788ade594d
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 07/03/2019
ms.locfileid: "67541437"
---
# <a name="collect-distributed-traces-from-python-preview"></a>Sammeln verteilter Ablaufverfolgungsdaten von Python (Vorschau)

Application Insights unterstützt jetzt die verteilte Ablaufverfolgung von Python-Anwendungen durch die Integration in [OpenCensus](https://opencensus.io). Dieser Artikel führt Sie Schritt für Schritt durch den Prozess der Einrichtung von OpenCensus für Python und die Weiterleitung Ihrer Überwachungsdaten an Application Insights.

## <a name="prerequisites"></a>Voraussetzungen

- Sie benötigen ein Azure-Abonnement.
- Python muss installiert sein. In diesem Artikel wird [Python 3.7.0](https://www.python.org/downloads/) verwendet, aber auch frühere Versionen können mit kleineren Anpassung wahrscheinlich verwendet werden.

Wenn Sie kein Azure-Abonnement besitzen, können Sie ein [kostenloses Konto](https://azure.microsoft.com/free/) erstellen, bevor Sie beginnen.

## <a name="sign-in-to-the-azure-portal"></a>Melden Sie sich auf dem Azure-Portal an.

Melden Sie sich beim [Azure-Portal](https://portal.azure.com/) an.

## <a name="create-application-insights-resource"></a>Erstellen einer Application Insights-Ressource

Zuerst müssen Sie eine Application Insights-Ressource erstellen, die einen Instrumentierungsschlüssel (ikey) generiert. Der ikey wird zum Konfigurieren des OpenCensus SDK für das Senden von Telemetriedaten an Application Insights verwendet.

1. Klicken Sie auf **Ressource erstellen** > **Entwicklertools** > **Application Insights**.

   ![Hinzufügen einer Application Insights-Ressource](./media/opencensus-python/0001-create-resource.png)

   Ein Dialogfeld für die Konfiguration wird geöffnet. Füllen Sie die Eingabefelder anhand der Informationen in der folgenden Tabelle aus:

    | Einstellungen        | Wert           | BESCHREIBUNG  |
   | ------------- |:-------------|:-----|
   | **Name**      | Global eindeutiger Wert | Der Name, der die zu überwachende App identifiziert. |
   | **Ressourcengruppe**     | myResourceGroup      | Der Name der neuen Ressourcengruppe, die Application Insights-Daten hosten soll. |
   | **Location** | East US | Wählen Sie einen Standort in Ihrer Nähe oder in der Nähe des Standorts, in dem Ihre App gehostet wird. |

2. Klicken Sie auf **Create**.

## <a name="install-opencensus-azure-monitor-exporters"></a>Installieren von OpenCensus Azure Monitor Exporters

1. Installieren Sie die OpenCensus Azure Monitor Exporters:

    ```console
    python -m pip install opencensus-ext-azure
    ```

    > [!NOTE]
    > `python -m pip install opencensus-ext-azure` geht davon aus, dass Sie eine PATH-Umgebungsvariable für die Python-Installation festgelegt haben. Wenn Sie diese nicht konfiguriert haben, müssen Sie den vollständigen Verzeichnispfad zu Ihrer ausführbaren Python-Datei angeben, wodurch der Befehl in etwa so aussehen würde: `C:\Users\Administrator\AppData\Local\Programs\Python\Python37-32\python.exe -m pip install opencensus-ext-azure`.

2. Zuerst generieren wir lokal einige Ablaufverfolgungsdaten. Geben Sie in Python IDLE oder einem Editor Ihrer Wahl den folgenden Code ein:

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

3. Durch Ausführen des Codes werden Sie wiederholt aufgefordert, einen Wert einzugeben. Mit jedem Eintrag wird der Wert in die Shell gedruckt, und vom OpenCensus Python-Modul wird ein entsprechendes **SpanData**-Element generiert. Das OpenCensus-Projekt definiert eine [_Ablaufverfolgung als Struktur von Spannen (Spans)_ ](https://opencensus.io/core-concepts/tracing/).
    
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

4. Obwohl für Demonstrationszwecke hilfreich, möchten wir SpanData letztlich an Application Insights ausgeben. Ändern Sie Ihren Code aus dem vorherigen Schritt basierend auf dem folgenden Codebeispiel:

    ```python
    from opencensus.ext.azure.trace_exporter import AzureExporter
    from opencensus.trace.samplers import ProbabilitySampler
    from opencensus.trace.tracer import Tracer
    
    # TODO: replace the all-zero GUID with your instrumentation key.
    tracer = Tracer(
        exporter=AzureExporter(
            instrumentation_key='00000000-0000-0000-0000-000000000000',
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
5. Wenn Sie das Python-Skript jetzt ausführen, sollten Sie weiterhin aufgefordert werden, Werte einzugeben, aber jetzt wird nur der Wert in der Shell ausgegeben.

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

## <a name="opencensus-for-python"></a>OpenCensus für Python

* [Anpassung](https://github.com/census-instrumentation/opencensus-python/blob/master/README.rst#customization)
* [Flask-Integration](https://github.com/census-instrumentation/opencensus-python/tree/master/contrib/opencensus-ext-flask)
* [Django-Integration](https://github.com/census-instrumentation/opencensus-python/tree/master/contrib/opencensus-ext-django)
* [MySQL-Integration](https://github.com/census-instrumentation/opencensus-python/tree/master/contrib/opencensus-ext-mysql)
* [PostgreSQL](https://github.com/census-instrumentation/opencensus-python/tree/master/contrib/opencensus-ext-postgresql)

## <a name="next-steps"></a>Nächste Schritte

* [OpenCensus Python auf GitHub](https://github.com/census-instrumentation/opencensus-python)
* [Anwendungszuordnung](./../../azure-monitor/app/app-map.md)
* [End-to-End-Leistungsüberwachung](./../../azure-monitor/learn/tutorial-performance.md)