---
title: Erstellen einer durch HTTP ausgelösten Funktion in Azure
description: Hier erfahren Sie, wie Sie unter Verwendung von Azure Functions Core Tools und der Azure CLI Ihre erste Python-Funktion in Azure erstellen.
author: ggailey777
ms.author: glenga
ms.date: 04/24/2019
ms.topic: quickstart
ms.service: azure-functions
ms.custom: mvc
ms.devlang: python
manager: gwallace
ms.openlocfilehash: 28169bfb8dead65c543a3752a709f33487854e60
ms.sourcegitcommit: adc1072b3858b84b2d6e4b639ee803b1dda5336a
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 09/10/2019
ms.locfileid: "70844730"
---
# <a name="create-an-http-triggered-function-in-azure"></a>Erstellen einer durch HTTP ausgelösten Funktion in Azure

Dieser Artikel demonstriert die Verwendung von Befehlszeilentools zum Erstellen eines Python-Projekts, das in Azure Functions ausgeführt wird. Die von Ihnen erstellte Funktion wird durch HTTP-Anforderungen ausgelöst. Abschließend veröffentlichen Sie Ihr Projekt zur Ausführung als [serverlose Funktion](functions-scale.md#consumption-plan) in Azure.

Dieser Artikel ist der erste von zwei Schnellstarts für Azure Functions. Nach dem Durcharbeiten dieses Artikels können Sie Ihrer Funktion [eine Ausgabebindung an eine Azure Storage-Warteschlange hinzufügen](functions-add-output-binding-storage-queue-python.md).

## <a name="prerequisites"></a>Voraussetzungen

Für den Einstieg müssen Sie über Folgendes verfügen:

+ Installieren Sie [Python 3.6.x](https://www.python.org/downloads/).

+ Installation von [Azure Functions Core Tools](./functions-run-local.md#v2), Version 2.7.1575 oder höher

+ Installation von [Azure CLI](/cli/azure/install-azure-cli), Version 2.x oder höher.

+ Ein aktives Azure-Abonnement.

[!INCLUDE [quickstarts-free-trial-note](../../includes/quickstarts-free-trial-note.md)]

## <a name="create-and-activate-a-virtual-environment-optional"></a>Erstellen und Aktivieren einer virtuellen Umgebung (optional)

Zum lokalen Entwickeln und Testen von Python-Funktionen wird die Verwendung einer Python 3.6-Umgebung empfohlen. Führen Sie die folgenden Befehle aus, um eine virtuelle Umgebung mit dem Namen `.venv` zu erstellen und zu aktivieren. 

> [!NOTE]
> Wurde venv nicht von Python in Ihrer Linux-Distribution installiert, können Sie dies mit dem folgenden Befehl nachholen:
> ```command
> sudo apt-get install python3-venv
>

### <a name="bash"></a>Bash:

```bash
python3.6 -m venv .venv
source .venv/bin/activate
```

### <a name="powershell-or-a-windows-command-prompt"></a>PowerShell oder eine Windows-Eingabeaufforderung:

```powershell
py -3.6 -m venv .venv
.venv\scripts\activate
```

Die verbleibenden Befehle werden in der virtuellen Umgebung ausgeführt.

## <a name="create-a-local-functions-project"></a>Erstellen eines lokalen Functions-Projekts

Ein Functions-Projekt stellt das Äquivalent einer Funktions-App in Azure dar. Es kann mehrere Funktionen aufweisen, die die gleichen lokalen und Hostingkonfigurationen gemein haben.

Führen Sie in der virtuellen Umgebung den folgenden Befehl aus, und wählen Sie dabei **python** als Workerruntime aus.

```console
func init MyFunctionProj
```

Es wird ein Ordner mit dem Namen _MyFunctionProj_ erstellt, der die folgenden drei Dateien enthält:

* `local.settings.json` wird verwendet, um bei der lokalen Ausführung App-Einstellungen und Verbindungszeichenfolgen zu speichern. Diese Datei wird nicht in Azure veröffentlicht.
* `requirements.txt` enthält die Liste der bei der Veröffentlichung in Azure zu installierenden Pakete.
* `host.json` enthält globale Konfigurationsoptionen, die sich auf alle Funktionen in einer Funktions-App auswirken. Diese Datei wird in Azure veröffentlicht.

Navigieren Sie zum neuen MyFunctionProj-Ordner:

```console
cd MyFunctionProj
```

## <a name="create-a-function"></a>Erstellen einer Funktion

Führen Sie den folgenden Befehl aus, um Ihrem Projekt eine Funktion hinzuzufügen:

```console
func new
```

Wählen Sie die **HTTP-Trigger**-Vorlage aus, geben Sie `HttpTrigger` als Namen der Funktion ein, und drücken Sie dann die EINGABETASTE.

Es wird ein Unterordner mit dem Namen _HttpTrigger_ erstellt, der die folgenden Dateien enthält:

* **function.json**: Konfigurationsdatei, die die Funktion, Trigger und weitere Bindungen definiert. Überprüfen Sie diese Datei, und beachten Sie, dass der Wert für `scriptFile` auf die Datei verweist, die die Funktion enthält, während der Aufruftrigger und die Bindungen im Array `bindings` definiert sind.

  Für jede Bindung sind eine Richtung, ein Typ und ein eindeutiger Name erforderlich. Der HTTP-Trigger weist eine Eingabebindung vom Typ [`httpTrigger`](functions-bindings-http-webhook.md#trigger) und eine Ausgabebindung vom Typ [`http`](functions-bindings-http-webhook.md#output) auf.

* **\_\_Init\_\_.py**: Skriptdatei, die Ihre per HTTP ausgelöste Funktion darstellt. Überprüfen Sie dieses Skript, und beachten Sie, dass es eine `main()`-Standardfunktion enthält. HTTP-Daten aus dem Trigger werden mithilfe des benannten Bindungsparameters `req` an diese Funktion übergeben. `req` ist in function.json definiert und stellt eine Instanz der [azure.functions.HttpRequest-Klasse](/python/api/azure-functions/azure.functions.httprequest) dar. 

    Das Rückgabeobjekt, das in function.json als `$return` definiert ist, ist eine Instanz der [azure.functions.HttpResponse-Klasse](/python/api/azure-functions/azure.functions.httpresponse). Weitere Informationen finden Sie unter [HTTP-Trigger und -Bindungen in Azure Functions](functions-bindings-http-webhook.md).

## <a name="run-the-function-locally"></a>Lokales Ausführen der Funktion

Der folgende Befehl startet die Funktions-App, die lokal mithilfe der gleichen Azure Functions-Runtime ausgeführt wird, die in Azure vorhanden ist.

```console
func host start
```

Wenn der Functions-Host startet, schreibt er eine Ausgabe ähnlich der folgenden, die zur besseren Lesbarkeit beschnitten wurde:

```output

                  %%%%%%
                 %%%%%%
            @   %%%%%%    @
          @@   %%%%%%      @@
       @@@    %%%%%%%%%%%    @@@
     @@      %%%%%%%%%%        @@
       @@         %%%%       @@
         @@      %%%       @@
           @@    %%      @@
                %%
                %

...

Content root path: C:\functions\MyFunctionProj
Now listening on: http://0.0.0.0:7071
Application started. Press Ctrl+C to shut down.

...

Http Functions:

        HttpTrigger: http://localhost:7071/api/HttpTrigger

[8/27/2018 10:38:27 PM] Host started (29486ms)
[8/27/2018 10:38:27 PM] Job host started
```

Kopieren Sie die URL Ihrer `HttpTrigger`-Funktion aus der Runtimeausgabe, und fügen Sie sie in die Adressleiste Ihres Browsers ein. Hängen Sie anschließend die Abfragezeichenfolge `?name=<yourname>` an diese URL an, und führen Sie die Anforderung aus. Hier ist die Antwort des Browsers auf die von der lokalen Funktion zurückgegebenen GET-Anforderung abgebildet:

![Lokales Testen im Browser](./media/functions-create-first-function-python/function-test-local-browser.png)

Jetzt haben Sie die Funktion lokal ausgeführt und können die Funktions-App und andere erforderliche Ressourcen in Azure erstellen.

[!INCLUDE [functions-create-resource-group](../../includes/functions-create-resource-group.md)]

[!INCLUDE [functions-create-storage-account](../../includes/functions-create-storage-account.md)]

## <a name="create-a-function-app-in-azure"></a>Erstellen einer Funktions-App in Azure

Eine Funktions-App stellt eine Umgebung für die Ausführung Ihres Funktionscodes bereit. Mit ihr können Sie Funktionen zu logischen Einheiten gruppieren. Dies erleichtert die Verwaltung, Bereitstellung und Freigabe von Ressourcen.

Führen Sie den folgenden Befehl aus, indem Sie den Platzhalter `<APP_NAME>` durch einen eindeutigen Namen für die Funktions-App und `<STORAGE_NAME>` durch den Speicherkontonamen ersetzen. `<APP_NAME>` ist gleichzeitig die DNS-Standarddomäne für die Funktions-App. Dieser Name muss für alle Apps in Azure eindeutig sein.

```azurecli-interactive
az functionapp create --resource-group myResourceGroup --os-type Linux \
--consumption-plan-location westeurope  --runtime python \
--name <APP_NAME> --storage-account  <STORAGE_NAME>
```
> [!NOTE]
> Linux- und Windows-Apps können nicht in derselben Ressourcengruppe gehostet werden. Wenn Sie über eine bestehende Ressourcengruppe mit dem Namen `myResourceGroup` und einer Windows-Funktions-App oder -Web-App verfügen, müssen Sie eine andere Ressourcengruppe verwenden.

Mit diesem Befehl wird außerdem eine zugeordnete Azure Application Insights-Instanz in derselben Ressourcengruppe bereitgestellt, die zum Überwachen und Anzeigen von Protokollen verwendet werden kann.

Nun können Sie Ihr lokales Funktions-Projekt in der Funktions-App in Azure veröffentlichen.

## <a name="deploy-the-function-app-project-to-azure"></a>Bereitstellen des Funktions-App-Projekts in Azure

Nach dem Erstellen der Funktions-App in Azure können Sie den Core Tools-Befehl [`func azure functionapp publish`](functions-run-local.md#project-file-deployment) verwenden, um Ihren Projektcode in Azure bereitzustellen. Ersetzen Sie in diesen Beispielen `<APP_NAME>` durch den Namen Ihrer App aus dem vorherigen Schritt.

```command
func azure functionapp publish <APP_NAME> --build remote
```

Mit der Option `--build remote` wird das Python-Projekt auf der Grundlage der Dateien im Bereitstellungspaket remote in Azure erstellt. 

Es wird in etwa die folgende Ausgabe angezeigt, die zur Verbesserung der Lesbarkeit gekürzt wurde:

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

Kopieren Sie den Wert `Invoke url` für `HttpTrigger`, den Sie nun zum Testen Ihrer Funktion in Azure verwenden können. Die URL enthält einen `code`-Abfragezeichenfolgenwert, der Ihr Funktionsschlüssel ist. Dieser Schlüssel erschwert es anderen Benutzern, Ihren HTTP-Triggerendpunkt in Azure aufzurufen.

[!INCLUDE [functions-test-function-code](../../includes/functions-test-function-code.md)]

> [!NOTE]
> Zum Anzeigen von Protokollen für eine veröffentlichte Python-App nahezu in Echtzeit empfiehlt sich die Verwendung des [Live Metrics Stream-Features von Application Insights](functions-monitoring.md#streaming-logs).

## <a name="next-steps"></a>Nächste Schritte

Sie haben ein Python-Funktionsprojekt mit einer per HTTP ausgelösten Funktion erstellt, es auf Ihrem lokalen Computer ausgeführt und es in Azure bereitgestellt. Erweitern Sie nun Ihre Funktion durch...

> [!div class="nextstepaction"]
> [Hinzufügen einer Azure Storage-Warteschlangen-Ausgabebindung](functions-add-output-binding-storage-queue-python.md)
