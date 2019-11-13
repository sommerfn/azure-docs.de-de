---
title: Python-Entwicklerreferenz für Azure Functions
description: Entwickeln von Funktionen mit Python
services: functions
documentationcenter: na
author: ggailey777
manager: cfowler
keywords: Azure Functions, Funktionen, Ereignisverarbeitung, dynamisches Compute, serverlose Architektur, Python
ms.service: azure-functions
ms.devlang: python
ms.topic: article
ms.tgt_pltfrm: multiple
ms.workload: na
ms.date: 04/16/2018
ms.author: glenga
ms.openlocfilehash: e0e649045e3efe488804fd37c030fe01991ad232
ms.sourcegitcommit: ac56ef07d86328c40fed5b5792a6a02698926c2d
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 11/08/2019
ms.locfileid: "73803621"
---
# <a name="azure-functions-python-developer-guide"></a>Python-Entwicklerhandbuch für Azure Functions

Dieser Artikel ist eine Einführung in die Entwicklung von Azure Functions mithilfe von Python. Der folgende Inhalt geht davon aus, dass Sie das [Azure Functions: Entwicklerhandbuch](functions-reference.md) bereits gelesen haben. 

Informationen zu eigenständigen Functions-Beispielprojekten in Python finden Sie in den [Functions-Beispielen für Python](/samples/browse/?products=azure-functions&languages=python). 

## <a name="programming-model"></a>Programmiermodell

Azure Functions geht davon aus, dass eine Funktion eine zustandslose Methode in Ihrem Python-Skript ist, die Eingaben verarbeitet und Ausgaben erzeugt. Standardmäßig erwartet die Runtime, dass die Methode als globale Methode namens `main()` in der `__init__.py`-Datei implementiert ist. Sie können auch [einen alternativen Einstiegspunkt angeben](#alternate-entry-point).

Daten von Triggern und Bindungen werden mittels Methodenattributen an die Funktion gebunden, die die in der Konfigurationsdatei *function.json* definierte Eigenschaft `name` verwenden. Beispielsweise beschreibt die unten stehende _function.json_ eine einfache Funktion, die von einer HTTP-Anforderung namens `req` ausgelöst wird:

```json
{
  "bindings": [
    {
      "name": "req",
      "direction": "in",
      "type": "httpTrigger",
      "authLevel": "anonymous"
    },
    {
      "name": "$return",
      "direction": "out",
      "type": "http"
    }
  ]
}
```

Die Datei `__init__.py` enthält folgenden Funktionscode:

```python
def main(req):
    user = req.params.get('user')
    return f'Hello, {user}!'
```

Sie können auch Python-Typanmerkungen verwenden, um die Attributtypen und den Rückgabetyp in der Funktion explizit zu deklarieren. Auf diese Weise können Sie die IntelliSense-und AutoVervollständigen-Funktionen nutzen, die von vielen Python-Code-Editoren bereitgestellt werden.

```python
import azure.functions


def main(req: azure.functions.HttpRequest) -> str:
    user = req.params.get('user')
    return f'Hello, {user}!'
```

Verwenden Sie die Python-Anmerkungen im Paket [azure.functions.*](/python/api/azure-functions/azure.functions?view=azure-python), um Eingaben und Ausgaben an Ihre Methoden zu binden.

## <a name="alternate-entry-point"></a>Alternativer Einstiegspunkt

Sie können das Standardverhalten einer Funktion ändern, indem Sie die Eigenschaften `scriptFile` und `entryPoint` in der *function.json*-Datei angeben. Beispielsweise weist die unten stehende _function.json_ die Runtime an, die Methode `customentry()` in der _main.py_-Datei als Einstiegspunkt für Ihre Azure-Funktion zu verwenden.

```json
{
  "scriptFile": "main.py",
  "entryPoint": "customentry",
  "bindings": [
      ...
  ]
}
```

## <a name="folder-structure"></a>Ordnerstruktur

Die Ordnerstruktur für ein Python Functions-Projekt sieht wie im folgenden Beispiel aus:

```
 FunctionApp
 | - MyFirstFunction
 | | - __init__.py
 | | - function.json
 | | - example.py
 | - MySecondFunction
 | | - __init__.py
 | | - function.json
 | - SharedCode
 | | - myFirstHelperFunction.py
 | | - mySecondHelperFunction.py
 | - host.json
 | - requirements.txt
```

Sie können die freigegebene Datei [host.json](functions-host-json.md) zum Konfigurieren der Funktions-App verwenden. Jede Funktion verfügt über eine eigene Codedatei sowie über eine eigene Bindungskonfigurationsdatei (function.json). 

Freigegebener Code sollte in einem separaten Ordner gespeichert werden. Um auf Module im Ordner „SharedCode“ zu verweisen, können Sie die folgende Syntax verwenden:

```
from __app__.SharedCode import myFirstHelperFunction
```

Um auf Module zu verweisen, die lokal für eine Funktion sind, können Sie die relative Importsyntax wie folgt verwenden:

```
from . import example
```

Wenn Sie ein Functions-Projekt in Ihrer Funktions-App in Azure bereitstellen, sollte der gesamte Inhalt des Ordners *FunctionApp* in das Paket aufgenommen werden, jedoch nicht der Ordner selbst.

## <a name="triggers-and-inputs"></a>Trigger und Eingaben

Eingaben werden in Azure Functions in zwei Kategorien unterteilt: Triggereingaben und zusätzliche Eingaben. Obwohl sie sich in der Datei `function.json` unterscheiden, ist ihre Verwendung im Python-Code identisch.  Verbindungszeichenfolgen oder Geheimnisse für Trigger- und Eingabequellen werden bei lokaler Ausführung Werten in der Datei `local.settings.json` und bei der Ausführung in Azure den Anwendungseinstellungen zugeordnet. 

Im folgenden Code wird beispielsweise der Unterschied zwischen beiden dargestellt:

```json
// function.json
{
  "scriptFile": "__init__.py",
  "bindings": [
    {
      "name": "req",
      "direction": "in",
      "type": "httpTrigger",
      "authLevel": "anonymous",
      "route": "items/{id}"
    },
    {
      "name": "obj",
      "direction": "in",
      "type": "blob",
      "path": "samples/{id}",
      "connection": "AzureWebJobsStorage"
    }
  ]
}
```

```json
// local.settings.json
{
  "IsEncrypted": false,
  "Values": {
    "FUNCTIONS_WORKER_RUNTIME": "python",
    "AzureWebJobsStorage": "<azure-storage-connection-string>"
  }
}
```

```python
# __init__.py
import azure.functions as func
import logging


def main(req: func.HttpRequest,
         obj: func.InputStream):

    logging.info(f'Python HTTP triggered function processed: {obj.read()}')
```

Bei Aufruf der Funktion wird die HTTP-Anforderung als `req` an die Funktion übergeben. Es wird ein Eintrag, der auf der _ID_ in der Routen-URL basiert, aus Azure Blob Storage abgerufen und als `obj` im Funktionstext verfügbar gemacht.  Hier ist das angegebene Speicherkonto die Verbindungszeichenfolge für dasselbe Speicherkonto, das von der Funktions-App verwendet wird.


## <a name="outputs"></a>Ausgaben

Ausgaben können sowohl im Rückgabewert als auch in Ausgabeparametern angegeben werden. Wenn es nur eine Ausgabe gibt, empfehlen wir, den Rückgabewert zu verwenden. Bei mehreren Ausgaben müssen Sie Ausgabeparameter verwenden.

Um den Rückgabewert einer Funktion als Wert für eine Ausgabebindung zu verwenden, sollte die `name`-Eigenschaft der Bindung in `function.json` auf `$return` festgelegt werden.

Um mehrere Ausgaben zu erzeugen, verwenden Sie die `set()`-Methode, die von der [`azure.functions.Out`](/python/api/azure-functions/azure.functions.out?view=azure-python)-Schnittstelle bereitgestellt wird, um der Bindung einen Wert zuzuweisen. Die folgende Funktion kann z. B. mithilfe von Push eine Nachricht an eine Warteschlange übertragen und auch eine HTTP-Antwort zurückgeben.

```json
{
  "scriptFile": "__init__.py",
  "bindings": [
    {
      "name": "req",
      "direction": "in",
      "type": "httpTrigger",
      "authLevel": "anonymous"
    },
    {
      "name": "msg",
      "direction": "out",
      "type": "queue",
      "queueName": "outqueue",
      "connection": "AzureWebJobsStorage"
    },
    {
      "name": "$return",
      "direction": "out",
      "type": "http"
    }
  ]
}
```

```python
import azure.functions as func


def main(req: func.HttpRequest,
         msg: func.Out[func.QueueMessage]) -> str:

    message = req.params.get('body')
    msg.set(message)
    return message
```

## <a name="logging"></a>Protokollierung

Zugriff auf die Azure Functions-Runtimeprotokollierung ist über einen Stamm-[`logging`](https://docs.python.org/3/library/logging.html#module-logging)-Handler in Ihrer Funktions-App verfügbar. Diese Protokollierung ist an Application Insights gebunden und ermöglicht es Ihnen, während der Funktionsausführung aufgetretene Warnungen und Fehler zu kennzeichnen.

Im folgenden Beispiel wird eine Informationsmeldung protokolliert, wenn die Funktion über einen HTTP-Trigger aufgerufen wird.

```python
import logging


def main(req):
    logging.info('Python HTTP trigger function processed a request.')
```

Es sind zusätzliche Protokollierungsmethoden verfügbar, mit denen Sie auf anderen Ablaufverfolgungsebenen in die Konsole schreiben können:

| Methode                 | BESCHREIBUNG                                |
| ---------------------- | ------------------------------------------ |
| **`critical(_message_)`**   | Schreibt eine Meldung mit der Stufe KRITISCH in die Stammprotokollierung.  |
| **`error(_message_)`**   | Schreibt eine Meldung mit der Stufe ERROR in die Stammprotokollierung.    |
| **`warning(_message_)`**    | Schreibt eine Meldung mit der Stufe WARNUNG in die Stammprotokollierung.  |
| **`info(_message_)`**    | Schreibt eine Meldung mit der Stufe INFO in die Stammprotokollierung.  |
| **`debug(_message_)`** | Schreibt eine Meldung mit der Stufe DEBUG in die Stammprotokollierung.  |

Weitere Informationen über Protokollierung finden Sie unter [Überwachen von Azure Functions](functions-monitoring.md).

## <a name="http-trigger-and-bindings"></a>HTTP-Trigger und -Bindungen

Der HTTP-Trigger ist in der Datei „function.jon“ definiert. Der `name` der Bindung muss mit dem benannten Parameter in der Funktion identisch sein. In den vorherigen Beispielen wird der Bindungsname `req` verwendet. Dieser Parameter ist ein [HttpRequest]-Objekt, und es wird ein [HttpResponse]-Objekt zurückgegeben.

Aus dem [HttpRequest]-Objekt können Sie Anforderungsheader, Abfrageparameter, Routenparameter und den Nachrichtentext extrahieren. 

Das folgende Beispiel stammt aus der [HTTP-Trigger-Vorlage für Python](https://github.com/Azure/azure-functions-templates/tree/dev/Functions.Templates/Templates/HttpTrigger-Python). 

```python
def main(req: func.HttpRequest) -> func.HttpResponse:
    headers = {"my-http-header": "some-value"}

    name = req.params.get('name')
    if not name:
        try:
            req_body = req.get_json()
        except ValueError:
            pass
        else:
            name = req_body.get('name')
            
    if name:
        return func.HttpResponse(f"Hello {name}!", headers=headers)
    else:
        return func.HttpResponse(
             "Please pass a name on the query string or in the request body",
             headers=headers, status_code=400
        )
```

In dieser Funktion wird der Wert des `name`-Abfrageparameters aus dem `params`-Parameter des [HttpRequest]-Objekts ermittelt. Der JSON-codierte Nachrichtentext wird mit der `get_json`-Methode gelesen. 

Außerdem können Sie `status_code` und `headers` für die Antwortnachricht im zurückgegebenen [HttpResponse]-Objekt festlegen.

## <a name="concurrency"></a>Parallelität

Standardmäßig kann die Python-Runtime von Functions nur einen Aufruf einer Funktion zu einem Zeitpunkt verarbeiten. Dieser Grad an Parallelität ist in den folgenden Situationen jedoch u. U. nicht ausreichend:

+ Sie versuchen, eine große Anzahl von Aufrufen gleichzeitig zu verarbeiten.
+ Sie verarbeiten eine große Anzahl von E/A-Ereignissen.
+ Ihre Anwendung ist E/A-gebunden.

In diesen Fällen können Sie die Leistung durch asynchrones Ausführen und Verwenden von mehreren Sprachworkerprozessen verbessern.  

### <a name="async"></a>Async

Es wird empfohlen, dass Sie mit der `async def`-Anweisung die Ausführung der Funktion als asynchrone Coroutine festlegen.

```python
# Runs with asyncio directly

async def main():
    await some_nonblocking_socket_io_op()
```

Wenn die `main()`-Funktion synchron ist (ohne den `async`-Qualifizierer), wird die Funktion automatisch in einem `asyncio`-Threadpool ausgeführt.

```python
# Runs in an asyncio thread-pool

def main():
    some_blocking_socket_io()
```

### <a name="use-multiple-language-worker-processes"></a>Verwenden mehrerer Sprachworkerprozesse

Standardmäßig verfügt jede Functions-Hostinstanz über einen einzigen Sprachworkerprozess. Mehrere Sprachworkerprozesse pro Hostinstanz werden jedoch unterstützt. Funktionsaufrufe werden dann gleichmäßig auf diese Sprachworkerprozesse verteilt. Ändern Sie diesen Wert mithilfe der Anwendungseinstellung [FUNCTIONS_WORKER_PROCESS_COUNT](functions-app-settings.md#functions_worker_process_count). 

## <a name="context"></a>Kontext

Um den Aufrufkontext einer Funktion während der Ausführung abzurufen, nehmen Sie das [`context`](/python/api/azure-functions/azure.functions.context?view=azure-python)-Argument in ihre Signatur auf. 

Beispiel:

```python
import azure.functions


def main(req: azure.functions.HttpRequest,
         context: azure.functions.Context) -> str:
    return f'{context.invocation_id}'
```

Die [**Context**](/python/api/azure-functions/azure.functions.context?view=azure-python)-Klasse weist die folgenden Zeichenfolgenattribute auf:

`function_directory`  
Das Verzeichnis, in dem die Funktion ausgeführt wird.

`function_name`  
Name der Funktion.

`invocation_id`  
ID des aktuellen Funktionsaufrufs.

## <a name="global-variables"></a>Globale Variablen

Es besteht keine Garantie, dass der Status Ihrer App für zukünftige Ausführungen beibehalten wird. Jedoch verwendet die Azure Functions-Runtime oft wieder den gleichen Prozess für mehrere Ausführungen derselben App. Zum Zwischenspeichern der Ergebnisse einer teuren Berechnung deklarieren Sie diese als globale Variable. 

```python
CACHED_DATA = None


def main(req):
    global CACHED_DATA
    if CACHED_DATA is None:
        CACHED_DATA = load_json()

    # ... use CACHED_DATA in code
```

## <a name="environment-variables"></a>Umgebungsvariablen

In Functions werden [Anwendungseinstellung](functions-app-settings.md), z.B. Dienstverbindungszeichenfolgen, während der Ausführung als Umgebungsvariablen verfügbar gemacht. Sie können auf diese Einstellungen zugreifen, indem Sie `import os` deklarieren und dann `setting = os.environ["setting-name"]` verwenden.

Im folgenden Beispiel wird die [Anwendungseinstellung](functions-how-to-use-azure-function-app-settings.md#settings) mit dem Schlüssel `myAppSetting` ermittelt:

```python
import logging
import os
import azure.functions as func

def main(req: func.HttpRequest) -> func.HttpResponse:

    # Get the setting named 'myAppSetting'
    my_app_setting_value = os.environ["myAppSetting"]
    logging.info(f'My app setting value:{my_app_setting_value}')
```

Für die lokale Entwicklung werden Anwendungseinstellungen [in der Datei „local.settings.json“ verwaltet](functions-run-local.md#local-settings-file).  

## <a name="python-version-and-package-management"></a>Python-Version und Paketverwaltung

Zurzeit unterstützt Azure Functions nur Python 3.6.x (offizielle CPython-Verteilung).

Bei der lokalen Entwicklung mithilfe der Azure Functions Core Tools oder von Visual Studio Code fügen Sie die Namen und Versionen der erforderlichen Pakete in der `requirements.txt`-Datei hinzu, und installieren Sie sie mithilfe von `pip`.

Beispielsweise können die folgende Datei mit Anforderungen und der pip-Befehl verwendet werden, um das `requests`-Paket aus PyPI zu installieren.

```txt
requests==2.19.1
```

```bash
pip install -r requirements.txt
```

## <a name="publishing-to-azure"></a>Veröffentlichen in Azure

Wenn Sie zur Veröffentlichung bereit sind, stellen Sie sicher, dass alle Ihre Abhängigkeiten in der Datei *requirements.txt* aufgeführt sind, die sich im Stamm Ihres Projektverzeichnisses befindet. Azure Functions kann diese Abhängigkeiten [remote erstellen](functions-deployment-technologies.md#remote-build).

Projektdateien und -ordner, die von der Veröffentlichung ausgeschlossen sind, einschließlich des Ordners der virtuellen Umgebung, werden in der .funcignore-Datei aufgelistet. 

Die [Azure Functions Core Tools](functions-run-local.md#v2) und die [Azure Functions-Erweiterung für VS Code](functions-create-first-function-vs-code.md#publish-the-project-to-azure) führen standardmäßig einen Remotebuild aus. Verwenden Sie z.B. den folgenden Befehl:

```bash
func azure functionapp publish <app name>
```

Wenn Sie Ihre App lokal anstatt in Azure erstellen möchten, [installieren Sie Docker](https://docs.docker.com/install/) auf Ihrem lokalen Computer, und führen Sie den folgenden Befehl aus, um mithilfe der [Azure Functions Core Tools](functions-run-local.md#v2) (func) zu veröffentlichen. Denken Sie daran, `<app name>` durch den Namen Ihrer Funktions-App in Azure zu ersetzen. 

```bash
func azure functionapp publish <app name> --build-native-deps
```

Im Hintergrund verwenden die Core Tools Docker, um das [mcr.microsoft.com/azure-functions/python](https://hub.docker.com/r/microsoft/azure-functions/)-Image als Container auf Ihrem lokalen Computer auszuführen. Unter Verwendung diese Umgebung werden dann die erforderlichen Module aus der Quellverteilung erstellt und installiert, bevor sie zur endgültigen Bereitstellung in Azure gepackt werden.

[Verwenden Sie Azure-Pipelines](functions-how-to-azure-devops.md), um Ihre Abhängigkeiten zu erstellen und mithilfe eines Continuous Delivery-Systems zu veröffentlichen. 

## <a name="unit-testing"></a>Komponententests

In Python geschriebene Funktionen können wie anderer Python-Code mithilfe von Standardtestframeworks getestet werden. Bei den meisten Bindungen können Sie ein Pseudoeingabeobjekt erstellen, indem Sie eine Instanz einer geeigneten Klasse aus dem `azure.functions`-Paket erstellen. Da das [`azure.functions`](https://pypi.org/project/azure-functions/)-Paket nicht sofort verfügbar ist, müssen Sie es über Ihre Datei `requirements.txt` installieren. Die Vorgehensweise wird oben im Abschnitt [Python-Version und Paketverwaltung](#python-version-and-package-management) beschrieben.

Beim Folgenden handelt es sich beispielsweise um einen Pseudotest einer durch HTTP ausgelösten Funktion:

```json
{
  "scriptFile": "httpfunc.py",
  "entryPoint": "my_function",
  "bindings": [
    {
      "authLevel": "function",
      "type": "httpTrigger",
      "direction": "in",
      "name": "req",
      "methods": [
        "get",
        "post"
      ]
    },
    {
      "type": "http",
      "direction": "out",
      "name": "$return"
    }
  ]
}
```

```python
# myapp/httpfunc.py
import azure.functions as func
import logging

def my_function(req: func.HttpRequest) -> func.HttpResponse:
    logging.info('Python HTTP trigger function processed a request.')

    name = req.params.get('name')
    if not name:
        try:
            req_body = req.get_json()
        except ValueError:
            pass
        else:
            name = req_body.get('name')

    if name:
        return func.HttpResponse(f"Hello {name}")
    else:
        return func.HttpResponse(
             "Please pass a name on the query string or in the request body",
             status_code=400
        )
```

```python
# myapp/test_httpfunc.py
import unittest

import azure.functions as func
from httpfunc import my_function


class TestFunction(unittest.TestCase):
    def test_my_function(self):
        # Construct a mock HTTP request.
        req = func.HttpRequest(
            method='GET',
            body=None,
            url='/api/HttpTrigger',
            params={'name': 'Test'})

        # Call the function.
        resp = my_function(req)

        # Check the output.
        self.assertEqual(
            resp.get_body(),
            b'Hello Test',
        )
```

Im Folgenden finden Sie ein weiteres Beispiel mit einer durch eine Warteschlange ausgelösten Funktion:

```python
# myapp/__init__.py
import azure.functions as func


def my_function(msg: func.QueueMessage) -> str:
    return f'msg body: {msg.get_body().decode()}'
```

```python
# myapp/test_func.py
import unittest

import azure.functions as func
from . import my_function


class TestFunction(unittest.TestCase):
    def test_my_function(self):
        # Construct a mock Queue message.
        req = func.QueueMessage(
            body=b'test')

        # Call the function.
        resp = my_function(req)

        # Check the output.
        self.assertEqual(
            resp,
            'msg body: test',
        )
```
## <a name="temporary-files"></a>Temporäre Dateien

Die `tempfile.gettempdir()`-Methode gibt einen temporären Ordner zurück, unter Linux `/tmp`. Die Anwendung kann dieses Verzeichnis zum Speichern von temporären Dateien verwenden, die von ihren Funktionen während der Ausführung generiert und verwendet werden. 

> [!IMPORTANT]
> Für Dateien, die in das temporäre Verzeichnis geschrieben werden, wird nicht garantiert, dass sie über Aufrufe hinweg beibehalten werden. Beim horizontalen Hochskalieren werden temporäre Dateien nicht von Instanzen gemeinsam verwendet. 

Im folgenden Beispiel wird eine benannte temporäre Datei im temporären Verzeichnis (`/tmp`) erstellt:

```python
import logging
import azure.functions as func
import tempfile
from os import listdir

#---
   tempFilePath = tempfile.gettempdir()   
   fp = tempfile.NamedTemporaryFile()     
   fp.write(b'Hello world!')              
   filesDirListInTemp = listdir(tempFilePath)     
```   

## <a name="known-issues-and-faq"></a>Bekannte Probleme und FAQ

Alle bekannten Probleme und Funktionsanfragen werden mithilfe der [GitHub-Probleme](https://github.com/Azure/azure-functions-python-worker/issues)liste nachverfolgt. Wenn Sie auf ein Problem stoßen, das in GitHub nicht zu finden ist, öffnen Sie ein neues Problem mit einer ausführlichen Problembeschreibung.

### <a name="cross-origin-resource-sharing"></a>Cross-Origin Resource Sharing

Azure Functions unterstützt jetzt die Ressourcenfreigabe zwischen verschiedenen Ursprüngen (Cross-Origin Resource Sharing, CORS). CORS wird [im Portal](functions-how-to-use-azure-function-app-settings.md#cors) und über die [Azure-Befehlszeilenschnittstelle](/cli/azure/functionapp/cors) konfiguriert. Die CORS-Liste der zulässigen Ursprünge gilt auf Funktions-App-Ebene. Wenn CORS aktiviert ist, enthalten Antworten den `Access-Control-Allow-Origin`-Header. Weitere Informationen finden Sie unter [Cross-Origin Resource Sharing (CORS)](functions-how-to-use-azure-function-app-settings.md#cors).

Die Liste der zulässigen Ursprünge wird für Python-Funktions-Apps [derzeit nicht unterstützt](https://github.com/Azure/azure-functions-python-worker/issues/444). Wegen dieser Einschränkung müssen Sie den `Access-Control-Allow-Origin`-Header in Ihren HTTP-Funktionen explizit festlegen, wie dies im folgenden Beispiel gezeigt ist:

```python
def main(req: func.HttpRequest) -> func.HttpResponse:

    # Define the allow origin headers.
    headers = {"Access-Control-Allow-Origin": "https://contoso.com"}

    # Set the headers in the response.
    return func.HttpResponse(
            f"Allowed origin '{headers}'.",
            headers=headers, status_code=200
    )
``` 

Sie dürfen nicht vergessen, Ihre „function.json“-Datei so zu aktualisieren, dass sie die HTTP-Methode OPTIONS unterstützt:

```json
    ...
      "methods": [
        "get",
        "post",
        "options"
      ]
    ...
```

Diese Methode wird vom Chrome-Browser verwendet, um die Liste der zulässigen Ursprünge auszuhandeln. 

## <a name="next-steps"></a>Nächste Schritte

Weitere Informationen finden Sie in den folgenden Ressourcen:

* [Dokumentation zur Azure Functions-Paket-API](/python/api/azure-functions/azure.functions?view=azure-python)
* [Bewährte Methoden für Azure Functions](functions-best-practices.md)
* [Trigger und Bindungen in Azure Functions](functions-triggers-bindings.md)
* [Blobspeicherbindungen](functions-bindings-storage-blob.md)
* [HTTP- und Webhook-Bindungen](functions-bindings-http-webhook.md)
* [Warteschlangenspeicherbindungen](functions-bindings-storage-queue.md)
* [Trigger mit Timer](functions-bindings-timer.md)


[HttpRequest]: /python/api/azure-functions/azure.functions.httprequest?view=azure-python
[HttpResponse]: /python/api/azure-functions/azure.functions.httpresponse?view=azure-python
