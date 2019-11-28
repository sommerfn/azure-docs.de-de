---
title: Erstellen einer durch HTTP ausgelösten Python-Funktion in Azure
description: Hier erfahren Sie, wie Sie unter Verwendung von Azure Functions Core Tools und der Azure CLI Ihre erste Python-Funktion in Azure erstellen.
ms.date: 11/07/2019
ms.topic: quickstart
ms.custom: mvc
ms.openlocfilehash: 18ae1ed000ffe61ce1ea9ff5c18aae98a0ffae65
ms.sourcegitcommit: d6b68b907e5158b451239e4c09bb55eccb5fef89
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 11/20/2019
ms.locfileid: "74227192"
---
# <a name="quickstart-create-an-http-triggered-python-function-in-azure"></a>Schnellstart: Erstellen einer durch HTTP ausgelösten Python-Funktion in Azure

Dieser Artikel demonstriert die Verwendung von Befehlszeilentools zum Erstellen eines Python-Projekts, das in Azure Functions ausgeführt wird. Sie können auch eine Funktion erstellen, die von einer HTTP-Anforderung ausgelöst wird. Nach der lokalen Ausführung veröffentlichen Sie Ihr Projekt für die Ausführung als [serverlose Funktion](functions-scale.md#consumption-plan) in Azure. 

Dieser Artikel enthält die erste von zwei Python-Schnellstartanleitungen für Azure Functions. Nach dem Durcharbeiten dieser Schnellstartanleitung können Sie Ihrer Funktion [eine Ausgabebindung an eine Azure Storage-Warteschlange hinzufügen](functions-add-output-binding-storage-queue-python.md).

Es gibt auch eine [Visual Studio Code-basierte Version](/azure/python/tutorial-vs-code-serverless-python-01) dieses Artikels.

## <a name="prerequisites"></a>Voraussetzungen

Bevor Sie beginnen, müssen Sie die folgenden Schritte ausführen:

+ Installieren Sie [Python 3.7.4](https://www.python.org/downloads/). Diese Version von Python wird mit Functions überprüft. Python 3.8 und höhere Versionen werden noch nicht unterstützt.

+ Installieren Sie [Azure Functions Core Tools](./functions-run-local.md#v2) (ab Version 2.7.1846).

+ Installieren Sie die [Azure CLI](/cli/azure/install-azure-cli) (ab Version 2.0.76.x).

+ Ein aktives Azure-Abonnement

    [!INCLUDE [quickstarts-free-trial-note](../../includes/quickstarts-free-trial-note.md)]

## <a name="create-and-activate-a-virtual-environment"></a>Erstellen und Aktivieren einer virtuellen Umgebung

Für die lokale Entwicklung von Python-Funktionen sollte eine Python 3.7.x-Umgebung verwendet werden. Führen Sie die folgenden Befehle aus, um eine virtuelle Umgebung mit dem Namen `.venv` zu erstellen und zu aktivieren.

> [!NOTE]
> Wurde venv nicht von Python in Ihrer Linux-Distribution installiert, können Sie dies mit dem folgenden Befehl nachholen:
> ```command
> sudo apt-get install python3-venv

### <a name="bash"></a>Bash:

```bash
python -m venv .venv
source .venv/bin/activate
```

### <a name="powershell-or-a-windows-command-prompt"></a>PowerShell oder eine Windows-Eingabeaufforderung:

```powershell
py -m venv .venv
.venv\scripts\activate
```

Nachdem Sie die virtuelle Umgebung nun aktiviert haben, müssen Sie die restlichen Befehle darin ausführen. Führen Sie `deactivate` aus, um die virtuelle Umgebung zu verlassen.

## <a name="create-a-local-functions-project"></a>Erstellen eines lokalen Funktionsprojekts

Ein Funktionsprojekt kann über mehrere Funktionen verfügen, die jeweils die gleiche lokale Konfiguration und die gleiche Hostingkonfiguration verwenden.

Führen Sie in der virtuellen Umgebung die folgenden Befehle aus:

```console
func init MyFunctionProj --python
cd MyFunctionProj
```

Mit dem Befehl `func init` wird der Ordner _MyFunctionProj_ erstellt. Das Python-Projekt in diesem Ordner hat noch keine Funktionen. Diese werden im nächsten Schritt hinzugefügt.

## <a name="create-a-function"></a>Erstellen einer Funktion

Führen Sie den folgenden Befehl aus, um Ihrem Projekt eine Funktion hinzuzufügen:

```console
func new --name HttpTrigger --template "HTTP trigger"
```

Mit diesem Befehl wird ein Unterordner namens _HttpTrigger_ mit folgenden Dateien erstellt:

* *function.json*: Konfigurationsdatei, die die Funktion, Trigger und weitere Bindungen definiert. Beachten Sie Folgendes: Der Wert für `scriptFile` verweist auf die Datei, die die Funktion enthält, und der Aufruftrigger und die Bindungen sind im Array `bindings` definiert.

    Für jede Bindung sind eine Richtung, ein Typ und ein eindeutiger Name erforderlich. Der HTTP-Trigger weist eine Eingabebindung vom Typ [`httpTrigger`](functions-bindings-http-webhook.md#trigger) und eine Ausgabebindung vom Typ [`http`](functions-bindings-http-webhook.md#output) auf.

* *\_\_Init\_\_.py*: Skriptdatei, die Ihre per HTTP ausgelöste Funktion darstellt. Beachten Sie, dass dieses Skript über ein `main()`-Standardelement verfügt. HTTP-Daten des Triggers werden an die Funktion übergeben, indem das `req`-Element mit dem Namen `binding parameter` verwendet wird. Das `req`-Element, das in „function.json“ definiert ist, stellt eine Instanz der [azure.functions.HttpRequest-Klasse](/python/api/azure-functions/azure.functions.httprequest) dar. 

    Das Rückgabeobjekt, das in *function.json* als `$return` definiert ist, ist eine Instanz der [azure.functions.HttpResponse-Klasse](/python/api/azure-functions/azure.functions.httpresponse). Weitere Informationen finden Sie unter [HTTP-Trigger und -Bindungen in Azure Functions](functions-bindings-http-webhook.md).

Nun können Sie die neue Funktion auf Ihrem lokalen Computer ausführen.

## <a name="run-the-function-locally"></a>Lokales Ausführen der Funktion

Dieser Befehl startet die Funktions-App unter Verwendung der Azure Functions-Runtime („func.exe“):

```console
func host start
```

Die Ausgabe sollte folgende Informationen enthalten:

```output
Http Functions:

        HttpTrigger: http://localhost:7071/api/HttpTrigger    
```

Kopieren Sie die URL der Funktion `HttpTrigger` aus dieser Ausgabe, und fügen Sie sie in die Adressleiste Ihres Browsers ein. Hängen Sie anschließend die Abfragezeichenfolge `?name=<yourname>` an diese URL an, und führen Sie die Anforderung aus. Im folgenden Screenshot ist die Antwort auf die GET-Anforderung dargestellt, die von der lokalen Funktion an den Browser zurückgegeben wird:

![Lokales Überprüfen im Browser](./media/functions-create-first-function-python/function-test-local-browser.png)

Drücken Sie STRG+C, um die Ausführung Ihrer Funktions-App zu beenden.

Nachdem Sie Ihre Funktion nun lokal ausgeführt haben, können Sie Ihren Funktionscode in Azure bereitstellen.  
Vor der App-Bereitstellung müssen noch einige Azure-Ressourcen erstellt werden.

[!INCLUDE [functions-create-resource-group](../../includes/functions-create-resource-group.md)]

[!INCLUDE [functions-create-storage-account](../../includes/functions-create-storage-account.md)]

## <a name="create-a-function-app-in-azure"></a>Erstellen einer Funktions-App in Azure

Eine Funktions-App stellt eine Umgebung für die Ausführung Ihres Funktionscodes bereit. Mit ihr können Sie Funktionen zu logischen Einheiten gruppieren. Dies erleichtert die Verwaltung, Bereitstellung und Freigabe von Ressourcen. 

Führen Sie den folgenden Befehl aus: Ersetzen Sie `<APP_NAME>` durch einen eindeutigen Funktions-App-Namen. Ersetzen Sie `<STORAGE_NAME>` durch einen Speicherkontonamen. `<APP_NAME>` ist gleichzeitig die DNS-Standarddomäne für die Funktions-App. Dieser Name muss für alle Apps in Azure eindeutig sein.

> [!NOTE]
> In derselben Ressourcengruppe können nicht gleichzeitig Linux- und Windows-Apps gehostet werden. Wenn Sie über eine bestehende Ressourcengruppe mit dem Namen `myResourceGroup` und einer Windows-Funktions-App oder -Web-App verfügen, müssen Sie eine andere Ressourcengruppe verwenden.

```azurecli-interactive
az functionapp create --resource-group myResourceGroup --os-type Linux \
--consumption-plan-location westeurope  --runtime python --runtime-version 3.7 \
--name <APP_NAME> --storage-account  <STORAGE_NAME>
```

Mit dem obigen Befehl wird eine Funktions-App mit Python 3.7.4 erstellt. Außerdem wird in der gleichen Ressourcengruppe eine zugeordnete Azure Application Insights-Instanz bereitgestellt. Sie können diese Instanz verwenden, um Ihre Funktions-App zu überwachen und Protokolle anzuzeigen. 

Nun können Sie Ihr lokales Funktions-Projekt in der Funktions-App in Azure veröffentlichen.

## <a name="deploy-the-function-app-project-to-azure"></a>Bereitstellen des Funktions-App-Projekts in Azure

Nachdem Sie die Funktions-App in Azure erstellt haben, können Sie den Core Tools-Befehl [func azure functionapp publish](functions-run-local.md#project-file-deployment) verwenden, um Ihren Projektcode in Azure bereitzustellen. Ersetzen Sie in diesem Beispiel `<APP_NAME>` durch den Namen Ihrer App.

```console
func azure functionapp publish <APP_NAME> --build remote
```

Mit der Option `--build remote` wird das Python-Projekt auf der Grundlage der Dateien im Bereitstellungspaket remote in Azure erstellt. Dies ist die empfohlene Vorgehensweise. 

Ihnen wird daraufhin eine Ausgabe angezeigt, die in etwa wie die folgende Meldung aussieht. Sie ist hier verkürzt angegeben, um die Lesbarkeit zu verbessern:

```output
Getting site publishing info...
...

Preparing archive...
Uploading content...
Upload completed successfully.
Deployment completed successfully.
Syncing triggers...
Functions in myfunctionapp:
    HttpTrigger - [httpTrigger]
        Invoke url: https://myfunctionapp.azurewebsites.net/api/httptrigger?code=cCr8sAxfBiow548FBDLS1....
```

Sie können den Wert `Invoke url` für Ihr `HttpTrigger`-Element kopieren und verwenden, um Ihre Funktion in Azure zu überprüfen. Die URL enthält den Abfragezeichenfolgenwert `code`, bei dem es sich um Ihren Funktionsschlüssel handelt. Daher ist es für andere Personen schwierig, Ihren HTTP-Triggerendpunkt in Azure aufzurufen.

[!INCLUDE [functions-test-function-code](../../includes/functions-test-function-code.md)]

> [!NOTE]
> Verwenden Sie die [Live Metrics Stream-Features von Application Insights](functions-monitoring.md#streaming-logs), um Protokolle für eine veröffentlichte Python-App nahezu in Echtzeit anzuzeigen.

## <a name="next-steps"></a>Nächste Schritte

Sie haben ein Python-Funktionsprojekt mit einer per HTTP ausgelösten Funktion erstellt, es auf Ihrem lokalen Computer ausgeführt und es in Azure bereitgestellt. Erweitern Sie nun Ihre Funktion durch...

> [!div class="nextstepaction"]
> [Hinzufügen einer Azure Storage-Warteschlangen-Ausgabebindung](functions-add-output-binding-storage-queue-python.md)
