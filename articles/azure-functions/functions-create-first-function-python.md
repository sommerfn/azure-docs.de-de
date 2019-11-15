---
title: Erstellen einer durch HTTP ausgelösten Python-Funktion in Azure
description: Hier erfahren Sie, wie Sie unter Verwendung von Azure Functions Core Tools und der Azure CLI Ihre erste Python-Funktion in Azure erstellen.
author: ggailey777
ms.author: glenga
ms.date: 09/11/2019
ms.topic: quickstart
ms.service: azure-functions
ms.custom: mvc
ms.devlang: python
manager: gwallace
ms.openlocfilehash: 791348088d909785b36934c3b9a2ae00fc0acbb7
ms.sourcegitcommit: 6c2c97445f5d44c5b5974a5beb51a8733b0c2be7
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 11/05/2019
ms.locfileid: "73622033"
---
# <a name="create-an-http-triggered-python-function-in-azure"></a>Erstellen einer durch HTTP ausgelösten Python-Funktion in Azure

Dieser Artikel demonstriert die Verwendung von Befehlszeilentools zum Erstellen eines Python-Projekts, das in Azure Functions ausgeführt wird. Sie können auch eine Funktion erstellen, die von einer HTTP-Anforderung ausgelöst wird. Abschließend veröffentlichen Sie Ihr Projekt zur Ausführung als [serverlose Funktion](functions-scale.md#consumption-plan) in Azure.

Dieser Artikel enthält die erste von zwei Python-Schnellstartanleitungen für Azure Functions. Nach dem Durcharbeiten dieser Schnellstartanleitung können Sie Ihrer Funktion [eine Ausgabebindung an eine Azure Storage-Warteschlange hinzufügen](functions-add-output-binding-storage-queue-python.md).

## <a name="prerequisites"></a>Voraussetzungen

Bevor Sie beginnen, müssen Sie die folgenden Schritte ausführen:

+ Installieren Sie [Python 3.6.8](https://www.python.org/downloads/). Diese Version von Python wird mit Functions überprüft. 3.7 und höhere Versionen werden noch nicht unterstützt.

+ Installation von [Azure Functions Core Tools](./functions-run-local.md#v2), Version 2.7.1575 oder höher

+ Installation von [Azure CLI](/cli/azure/install-azure-cli), Version 2.x oder höher.

+ Ein aktives Azure-Abonnement

    [!INCLUDE [quickstarts-free-trial-note](../../includes/quickstarts-free-trial-note.md)]

## <a name="create-and-activate-a-virtual-environment-optional"></a>Erstellen und Aktivieren einer virtuellen Umgebung (optional)

Sie sollten eine Python 3.6.x-Umgebung verwenden, um Python-Funktionen lokal zu entwickeln. Führen Sie die folgenden Befehle aus, um eine virtuelle Umgebung mit dem Namen `.venv` zu erstellen und zu aktivieren.

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

Ein Funktionsprojekt ist das Äquivalent einer Funktions-App in Azure. Es kann mehrere Funktionen aufweisen, die die gleichen lokalen und Hostingkonfigurationen gemein haben.

1. Führen Sie in der virtuellen Umgebung den folgenden Befehl aus:

    ```console
    func init MyFunctionProj
    ```

1. Wählen Sie **python** als Workerruntime aus.

    Mit dem Befehl wird der Ordner _MyFunctionProj_ erstellt. Er enthält die folgenden drei Dateien:

    * *local.settings.json*: Wird verwendet, um bei der lokalen Ausführung App-Einstellungen und Verbindungszeichenfolgen zu speichern. Diese Datei wird nicht in Azure veröffentlicht.
    * *requirements.txt*: Enthält die Liste mit den Paketen, die vom System beim Veröffentlichen in Azure installiert werden.
    * *host.json*: Enthält globale Konfigurationsoptionen, die sich auf alle Funktionen in einer Funktions-App auswirken. Diese Datei wird in Azure veröffentlicht.

1. Navigieren Sie zum neuen Ordner *MyFunctionProj*:

    ```console
    cd MyFunctionProj
    ```

## <a name="create-a-function"></a>Erstellen einer Funktion

Fügen Sie dem neuen Projekt eine Funktion hinzu.

1. Führen Sie den folgenden Befehl aus, um Ihrem Projekt eine Funktion hinzuzufügen:

    ```console
    func new
    ```

1. Verwenden Sie den Abwärtspfeil, um die Vorlage **HTTP-Trigger** auszuwählen.

1. Geben Sie nach der Aufforderung zum Angeben eines Funktionsnamens den Namen *HttpTrigger* ein, und drücken Sie die EINGABETASTE.

Mit diesen Befehlen wird ein Unterordner mit dem Namen _HttpTrigger_ erstellt. Er enthält die folgenden Dateien:

* *function.json*: Konfigurationsdatei, die die Funktion, Trigger und weitere Bindungen definiert. Beachten Sie Folgendes: Der Wert für `scriptFile` verweist auf die Datei, die die Funktion enthält, und der Aufruftrigger und die Bindungen sind im Array `bindings` definiert.

    Für jede Bindung sind eine Richtung, ein Typ und ein eindeutiger Name erforderlich. Der HTTP-Trigger weist eine Eingabebindung vom Typ [`httpTrigger`](functions-bindings-http-webhook.md#trigger) und eine Ausgabebindung vom Typ [`http`](functions-bindings-http-webhook.md#output) auf.

* *\_\_Init\_\_.py*: Skriptdatei, die Ihre per HTTP ausgelöste Funktion darstellt. Beachten Sie, dass dieses Skript über ein `main()`-Standardelement verfügt. HTTP-Daten des Triggers werden an die Funktion übergeben, indem das `req`-Element mit dem Namen `binding parameter` verwendet wird. Das `req`-Element, das in „function.json“ definiert ist, stellt eine Instanz der [azure.functions.HttpRequest-Klasse](/python/api/azure-functions/azure.functions.httprequest) dar. 

    Das Rückgabeobjekt, das in *function.json* als `$return` definiert ist, ist eine Instanz der [azure.functions.HttpResponse-Klasse](/python/api/azure-functions/azure.functions.httpresponse). Weitere Informationen finden Sie unter [HTTP-Trigger und -Bindungen in Azure Functions](functions-bindings-http-webhook.md).

## <a name="run-the-function-locally"></a>Lokales Ausführen der Funktion

Die Funktion wird lokal mit der Azure Functions-Runtime ausgeführt.

1. Mit diesem Befehl wird die Funktions-App gestartet:

    ```console
    func host start
    ```

    Nachdem der Azure Functions-Host gestartet wurde, erfolgt in etwa die folgende Ausgabe. Sie ist hier verkürzt angegeben, um die Lesbarkeit zu verbessern:

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

1. Kopieren Sie die URL Ihrer `HttpTrigger`-Funktion aus der Runtimeausgabe, und fügen Sie sie in die Adressleiste Ihres Browsers ein.

1. Hängen Sie anschließend die Abfragezeichenfolge `?name=<yourname>` an diese URL an, und führen Sie die Anforderung aus. Im folgenden Screenshot ist die Antwort auf die GET-Anforderung dargestellt, die von der lokalen Funktion an den Browser zurückgegeben wird:

    ![Lokales Überprüfen im Browser](./media/functions-create-first-function-python/function-test-local-browser.png)

1. Drücken Sie STRG+C, um Ihre Funktions-App herunterzufahren.

Jetzt haben Sie die Funktion lokal ausgeführt und können die Funktions-App und andere erforderliche Ressourcen in Azure erstellen.

[!INCLUDE [functions-create-resource-group](../../includes/functions-create-resource-group.md)]

[!INCLUDE [functions-create-storage-account](../../includes/functions-create-storage-account.md)]

## <a name="create-a-function-app-in-azure"></a>Erstellen einer Funktions-App in Azure

Eine Funktions-App stellt eine Umgebung für die Ausführung Ihres Funktionscodes bereit. Mit ihr können Sie Funktionen zu logischen Einheiten gruppieren. Dies erleichtert die Verwaltung, Bereitstellung und Freigabe von Ressourcen.

Führen Sie den folgenden Befehl aus: Ersetzen Sie `<APP_NAME>` durch einen eindeutigen Funktions-App-Namen. Ersetzen Sie `<STORAGE_NAME>` durch einen Speicherkontonamen. `<APP_NAME>` ist gleichzeitig die DNS-Standarddomäne für die Funktions-App. Dieser Name muss für alle Apps in Azure eindeutig sein.

> [!NOTE]
> In derselben Ressourcengruppe können nicht gleichzeitig Linux- und Windows-Apps gehostet werden. Wenn Sie über eine bestehende Ressourcengruppe mit dem Namen `myResourceGroup` und einer Windows-Funktions-App oder -Web-App verfügen, müssen Sie eine andere Ressourcengruppe verwenden.

```azurecli-interactive
az functionapp create --resource-group myResourceGroup --os-type Linux \
--consumption-plan-location westeurope  --runtime python \
--name <APP_NAME> --storage-account  <STORAGE_NAME>
```

Mit dem obigen Befehl wird in derselben Ressourcengruppe auch eine zugeordnete Azure Application Insights-Instanz bereitgestellt. Sie können diese Instanz verwenden, um Ihre Funktions-App zu überwachen und Protokolle anzuzeigen.

Nun können Sie Ihr lokales Funktions-Projekt in der Funktions-App in Azure veröffentlichen.

## <a name="deploy-the-function-app-project-to-azure"></a>Bereitstellen des Funktions-App-Projekts in Azure

Nachdem Sie die Funktions-App in Azure erstellt haben, können Sie den Core Tools-Befehl [func azure functionapp publish](functions-run-local.md#project-file-deployment) verwenden, um Ihren Projektcode in Azure bereitzustellen. Ersetzen Sie in diesem Beispiel `<APP_NAME>` durch den Namen Ihrer App.

```console
func azure functionapp publish <APP_NAME> --build remote
```

Mit der Option `--build remote` wird das Python-Projekt auf der Grundlage der Dateien im Bereitstellungspaket remote in Azure erstellt. 

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
