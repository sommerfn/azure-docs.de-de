---
title: Abhängigkeitsnachverfolgung in Azure Application Insights mit OpenCensus Python | Microsoft-Dokumentation
description: Überwachen von Abhängigkeitsaufrufen für Ihre Python-Apps über OpenCensus Python
ms.service: azure-monitor
ms.subservice: application-insights
ms.topic: conceptual
author: lzchen
ms.author: lechen
ms.date: 10/15/2019
ms.openlocfilehash: c28ef627e9ef65f9985a520c8c4f3facc7a160da
ms.sourcegitcommit: f4d8f4e48c49bd3bc15ee7e5a77bee3164a5ae1b
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 11/04/2019
ms.locfileid: "73575495"
---
# <a name="track-dependencies-with-opencensus-python"></a>Nachverfolgen von Abhängigkeiten mit OpenCensus Python

Eine Abhängigkeit ist eine externe Komponente, die von Ihrer Anwendung aufgerufen wird. Abhängigkeitsdaten werden mithilfe von OpenCensus Python und den verschiedenen Integrationen erfasst. Die Daten werden dann als `dependencies`-Telemetriedaten an Application Insights unter Azure Monitor gesendet.

Instrumentieren Sie zunächst Ihre Python-Anwendung mit dem aktuellen [OpenCensus Python SDK](../../azure-monitor/app/opencensus-python.md).

## <a name="in-process-dependencies"></a>In-Process-Abhängigkeiten

Mit dem OpenCensus Python SDK für Azure Monitor können Sie In-Process-Abhängigkeitstelemetrie (Informationen und Logiken, die in Ihrer Anwendung auftreten) senden. Bei In-Process-Abhängigkeiten ist das Feld `type` in Analysen `INPROC`.

```python
from opencensus.ext.azure.trace_exporter import AzureExporter
from opencensus.trace.samplers import ProbabilitySampler
from opencensus.trace.tracer import Tracer

tracer = Tracer(exporter=AzureExporter(connection_string="InstrumentationKey=<your-ikey-here>"), sampler=ProbabilitySampler(1.0))

with tracer.span(name='foo'): # <-- A dependency telemetry item will be sent for this span "foo"
    print('Hello, World!')
```

## <a name="dependencies-with-requests-integration"></a>Abhängigkeiten mit requests-Integration

Verfolgen Sie Ihre ausgehenden Anforderungen mit der OpenCensus-`requests`-Integration nach.

Laden Sie `opencensus-ext-requests` von [PyPI](https://pypi.org/project/opencensus-ext-requests/) herunter, installieren Sie es, und fügen Sie es zu den Nachverfolgungsintegrationen hinzu. Anforderungen, die mit der Python-[requests](https://pypi.org/project/requests/)-Bibliothek versendet werden, werden nachverfolgt.

```python
import requests
from opencensus.ext.azure.trace_exporter import AzureExporter
from opencensus.trace import config_integration
from opencensus.trace.samplers import ProbabilitySampler
from opencensus.trace.tracer import Tracer

config_integration.trace_integrations(['requests'])  # <-- this line enables the requests integration

tracer = Tracer(exporter=AzureExporter(connection_string="InstrumentationKey=<your-ikey-here>"), sampler=ProbabilitySampler(1.0))

with tracer.span(name='parent'):
    response = requests.get(url='https://www.wikipedia.org/wiki/Rabbit') # <-- this request will be tracked
```

## <a name="dependencies-with-httplib-integration"></a>Abhängigkeiten mit httplib-Integration

Verfolgen Sie Ihre ausgehenden Anforderungen mit OpenCensus-`httplib`-Integration nach.

Laden Sie `opencensus-ext-httplib` von [PyPI](https://pypi.org/project/opencensus-ext-httplib/) herunter, installieren Sie es, und fügen Sie es zu den Nachverfolgungsintegrationen hinzu. Anforderungen, die mit [http.client](https://docs.python.org/3.7/library/http.client.html) für Python3 oder [httplib](https://docs.python.org/2/library/httplib.html) für Python2 gesendet werden, werden nachverfolgt.

```python
import http.client as httplib
from opencensus.ext.azure.trace_exporter import AzureExporter
from opencensus.trace import config_integration
from opencensus.trace.samplers import ProbabilitySampler
from opencensus.trace.tracer import Tracer

config_integration.trace_integrations(['httplib'])
conn = httplib.HTTPConnection("www.python.org")

tracer = Tracer(
    exporter=AzureExporter(),
    sampler=ProbabilitySampler(1.0)
)

conn.request("GET", "http://www.python.org", "", {})
response = conn.getresponse()
conn.close()
```

## <a name="dependencies-with-django-integration"></a>Abhängigkeiten mit django-Integration

Verfolgen Sie Ihre ausgehenden Django-Anforderungen mit der OpenCensus-`django`-Integration nach.

Laden Sie `opencensus-ext-django` von [PyPI](https://pypi.org/project/opencensus-ext-django/) herunter, installieren Sie es, und fügen Sie die folgende Zeile zum Abschnitt `MIDDLEWARE` in der Django-`settings.py`-Datei hinzu.

```python
MIDDLEWARE = [
    ...
    'opencensus.ext.django.middleware.OpencensusMiddleware',
]
```

Weitere Konfigurationsmöglichkeiten können bereitgestellt werden; umfassende Informationen dazu finden Sie im Abschnitt zu [Anpassungen](https://github.com/census-instrumentation/opencensus-python#customization).

```python
OPENCENSUS = {
    'TRACE': {
        'SAMPLER': 'opencensus.trace.samplers.ProbabilitySampler(rate=1)',
        'EXPORTER': '''opencensus.ext.ocagent.trace_exporter.TraceExporter(
            service_name='foobar',
        )''',
    }
}
```

## <a name="dependencies-with-mysql-integration"></a>Abhängigkeiten mit mysql-Integration

Verfolgen Sie Ihre MYSQL-Anforderungen mit der OpenCensus-`mysql`-Integration nach. Diese Integration unterstützt die [mysql-connector](https://pypi.org/project/mysql-connector-python/)-Bibliothek.

Laden Sie `opencensus-ext-mysql` von [PyPI](https://pypi.org/project/opencensus-ext-mysql/) herunter, installieren Sie es, und fügen Sie Ihrem Code die folgenden Zeilen hinzu.

```python
from opencensus.trace import config_integration

config_integration.trace_integrations(['mysql'])
```

## <a name="dependencies-with-pymysql-integration"></a>Abhängigkeiten mit pymysql-Integration

Verfolgen Sie Ihre PyMySQL-Anforderungen mit der OpenCensus-`pymysql`-Integration nach.

Laden Sie `opencensus-ext-pymysql` von [PyPI](https://pypi.org/project/opencensus-ext-pymysql/) herunter, installieren Sie es, und fügen Sie Ihrem Code die folgenden Zeilen hinzu.

```python
from opencensus.trace import config_integration

config_integration.trace_integrations(['pymysql'])
```

## <a name="dependencies-with-postgresql-integration"></a>Abhängigkeiten mit postgresql-Integration

Verfolgen Sie Ihre PostgreSQL-Anforderungen mit der OpenCensus-`postgresql`-Integration nach. Diese Integration unterstützt die [psycopg2](https://pypi.org/project/psycopg2/)-Bibliothek.

Laden Sie `opencensus-ext-postgresql` von [PyPI](https://pypi.org/project/opencensus-ext-postgresql/) herunter, installieren Sie es, und fügen Sie Ihrem Code die folgenden Zeilen hinzu.

```python
from opencensus.trace import config_integration

config_integration.trace_integrations(['postgresql'])
```

## <a name="dependencies-with-pymongo-integration"></a>Abhängigkeiten mit pymongo-Integration

Verfolgen Sie Ihre MongoDB-Anforderungen mit der OpenCensus-`pymongo`-Integration nach. Diese Integration unterstützt die [pymongo](https://pypi.org/project/pymongo/)-Bibliothek.

Laden Sie `opencensus-ext-pymongo` von [PyPI](https://pypi.org/project/opencensus-ext-pymongo/) herunter, installieren Sie es, und fügen Sie Ihrem Code die folgenden Zeilen hinzu.

```python
from opencensus.trace import config_integration

config_integration.trace_integrations(['pymongo'])
```

### <a name="dependencies-with-sqlalchemy-integration"></a>Abhängigkeiten mit sqlalchemy-Integration

Verfolgen Sie Ihre Abhängigkeiten mithilfe von SQLAlchemy mit der OpenCensus-`sqlalchemy`-Integration nach. Mit dieser Integration wird die Verwendung des [sqlalchemy](https://pypi.org/project/SQLAlchemy/)-Pakets unabhängig von der zugrunde liegenden Datenbank nachverfolgt.

```python
from opencensus.trace import config_integration

config_integration.trace_integrations(['sqlalchemy'])
```

## <a name="next-steps"></a>Nächste Schritte

* [Anwendungszuordnung](../../azure-monitor/app/app-map.md)
* [Verfügbarkeit](../../azure-monitor/app/monitor-web-app-availability.md)
* [Suchen,](../../azure-monitor/app/diagnostic-search.md)
* [Protokollabfragen](../../azure-monitor/log-query/log-query-overview.md)
* [Transaktionsdiagnose](../../azure-monitor/app/transaction-diagnostics.md)