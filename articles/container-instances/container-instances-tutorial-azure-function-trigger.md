---
title: 'Tutorial: Auslösen von Azure Container Instances durch eine Azure-Funktion'
description: Erstellen einer über HTTP ausgelösten, serverlosen PowerShell-Funktion zum Automatisieren der Erstellung von Azure-Containerinstanzen
services: container-instances
author: dlepow
manager: gwallace
ms.service: container-instances
ms.topic: tutorial
ms.date: 09/20/2019
ms.author: danlep
ms.custom: ''
ms.openlocfilehash: 00bd017b0bcff6386e678802c301087819792744
ms.sourcegitcommit: 83df2aed7cafb493b36d93b1699d24f36c1daa45
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 09/22/2019
ms.locfileid: "71179948"
---
# <a name="tutorial-use-an-http-triggered-azure-function-to-create-a-container-group"></a>Tutorial: Verwenden einer über HTTP ausgelösten Azure-Funktion zum Erstellen einer Containergruppe

[Azure Functions](../azure-functions/functions-overview.md) ist ein serverloser Computedienst, der Skripts oder Code als Reaktion auf eine Vielzahl von Ereignissen ausführen kann, z. B. eine HTTP-Anforderung, einen Timer oder eine Nachricht in einer Azure Storage-Warteschlange.

In diesem Tutorial erstellen Sie eine Azure-Funktion, die über eine HTTP-Anforderung die Bereitstellung einer [Containergruppe](container-instances-container-groups.md) auslöst. In diesem Beispiel werden die Grundlagen der Verwendung von Azure Functions zum automatischen Erstellen von Ressourcen in Azure Container Instances veranschaulicht. Sie können das Beispiel für komplexere Szenarien oder andere Ereignistrigger ändern oder erweitern. 

Folgendes wird vermittelt:

> [!div class="checklist"]
> * Verwenden von Visual Studio Code mit der [Azure Functions-Erweiterung](https://marketplace.visualstudio.com/items?itemName=ms-azuretools.vscode-azurefunctions), um eine einfache über HTTP ausgelöste PowerShell-Funktion zu erstellen
> * Aktivieren einer Identität in der Funktions-App und Erteilen von Berechtigungen zum Erstellen von Azure-Ressourcen
> * Ändern und erneutes Veröffentlichen der PowerShell-Funktion, um die Bereitstellung einer Containergruppe mit nur einem Container zu automatisieren
> * Überprüfen der über HTTP ausgelösten Bereitstellung des Containers

> [!IMPORTANT]
> PowerShell für Azure Functions ist zurzeit als Vorschauversion verfügbar. Vorschauversionen werden Ihnen zur Verfügung gestellt, wenn Sie die [zusätzlichen Nutzungsbedingungen][terms-of-use] akzeptieren. Einige Aspekte dieses Features werden bis zur allgemeinen Verfügbarkeit unter Umständen noch geändert.

## <a name="prerequisites"></a>Voraussetzungen

Die Voraussetzungen für die Installation und Verwendung von Visual Studio Code mit Azure Functions auf Ihrem Betriebssystem finden Sie unter [Erstellen Ihrer ersten PowerShell-Funktion in Azure](../azure-functions/functions-create-first-function-powershell.md#prerequisites).

In einigen Schritten in diesem Artikel wird die Azure-Befehlszeilenschnittstelle (Azure CLI) verwendet. Sie können Azure Cloud Shell oder eine lokale Installation der Azure CLI für diese Schritte verwenden. Informationen zum Durchführen einer Installation oder eines Upgrades finden Sei bei Bedarf unter [Installieren der Azure CLI][azure-cli-install].

## <a name="create-a-basic-powershell-function"></a>Erstellen einer einfachen PowerShell-Funktion

Führen Sie die Schritte unter [Erstellen Ihrer ersten PowerShell-Funktion in Azure](../azure-functions/functions-create-first-function-powershell.md) aus, um mit der Vorlage „HTTP-Trigger“ eine PowerShell-Funktion zu erstellen. Verwenden Sie für die Azure-Funktion den Standardnamen **HttpTrigger**. Folgen Sie den Anweisungen im Schnellstart, um die Funktion lokal zu testen, und veröffentlichen Sie das Projekt in einer Funktions-App in Azure. Dieses Beispiel ist eine einfache, über HTTP ausgelöste Funktion, die eine Textzeichenfolge zurückgibt. In späteren Schritten dieses Artikels ändern Sie die Funktion, um eine Containergruppe zu erstellen.

In diesem Artikel wird davon ausgegangen, dass Sie das Projekt mit dem Namen *myfunctionapp* in einer Azure-Ressourcengruppe veröffentlichen, die automatisch entsprechend dem Namen der Funktions-App benannt wird (auch *myfunctionapp*). Ersetzen Sie diese Namen in späteren Schritten durch den eindeutigen Namen Ihrer Funktions-App und Ressourcengruppe.

## <a name="enable-an-azure-managed-identity-in-the-function-app"></a>Aktivieren einer von Azure verwalteten Identität in der Funktions-App

Aktivieren Sie jetzt eine vom System zugewiesene [verwaltete Identität](../app-service/overview-managed-identity.md?toc=/azure/azure-functions/toc.json#adding-a-system-assigned-identity) in ihrer Funktions-App. Der PowerShell-Host, auf dem die App ausgeführt wird, kann sich automatisch mit dieser Identität authentifizieren, sodass Funktionen Aktionen für Azure-Dienste ausführen können, für die der Identität Zugriff gewährt wurde. In diesem Tutorial erteilen Sie der verwalteten Identität Berechtigungen zum Erstellen von Ressourcen in der Ressourcengruppe der Funktions-App. 

Rufen Sie zunächst mit dem Befehl [az group show][az-group-show] die ID der Ressourcengruppe der Funktions-App ab, und speichern Sie sie in einer Umgebungsvariable. Dieses Beispiel setzt voraus, dass Sie den Befehl in einer Bash-Shell ausführen.

```azurecli
rgID=$(az group show --name myfunctionapp --query id --output tsv)
```

Führen Sie [az functionapp identity app assign][az-functionapp-identity-app-assign] aus, um der Funktions-App eine lokale Identität und der Ressourcengruppe eine Rolle „Mitwirkender“ zuzuweisen. Diese Rolle ermöglicht der Identität das Erstellen zusätzlicher Ressourcen, z. B. Containergruppen in der Ressourcengruppe.

```azurecli
az functionapp identity assign \
  --name myfunctionapp \
  --resource-group myfunctionapp \
  --role contributor --scope $rgID
```

## <a name="modify-httptrigger-function"></a>Ändern der HttpTrigger-Funktion

Ändern Sie den PowerShell-Code für die **HttpTrigger**-Funktion, um eine Containergruppe zu erstellen. Suchen Sie in der Datei `run.ps1` für die Funktion nach dem folgenden Codeblock. Dieser Code zeigt einen Namenswert an, wenn ein solcher Wert in der Funktions-URL als Abfragezeichenfolge übergeben wird:

```powershell
[...]
if ($name) {
    $status = [HttpStatusCode]::OK
    $body = "Hello $name"
}
[...]
```

Ersetzen Sie diesen Code durch den folgenden Beispielblock. Hier wird ein in der Abfragezeichenfolge übergebener Namenswert verwendet, um mit dem Cmdlet [New-AzContainerGroup][new-azcontainergroup] eine Containergruppe zu benennen und zu erstellen. Denken Sie daran, den Ressourcengruppennamen *myfunctionapp* durch den Namen der Ressourcengruppe für Ihre Funktions-App zu ersetzen:

```powershell
[...]
if ($name) {
    $status = [HttpStatusCode]::OK
    New-AzContainerGroup -ResourceGroupName myfunctionapp -Name $name `
        -Image alpine -OsType Linux `
        -Command "echo 'Hello from an Azure container instance triggered by an Azure function'" `
        -RestartPolicy Never
    $body = "Started container group $name"
}
[...]
```

Dieses Beispiel erstellt eine Containergruppe mit einer einzelnen Containerinstanz, die das Image `alpine` ausführt. Der Container führt einen einzigen Befehl `echo` aus und wird dann beendet. In der Praxis könnten Sie beispielsweise die Erstellung einer oder mehrerer Containergruppen zum Ausführen eines Batchauftrags auslösen.
 
## <a name="test-function-app-locally"></a>Lokales Testen der Funktion

Stellen Sie sicher, dass die Funktion lokal korrekt ausgeführt wird, bevor Sie das Funktions-App-Projekt erneut in Azure veröffentlichen. Fügen Sie wie im [PowerShell-Schnellstart](../azure-functions/functions-create-first-function-powershell.md) gezeigt einen lokalen Breakpoint und darüber einen `Wait-Debugger`-Aufruf in das PowerShell-Skript ein. Anleitungen zum Debuggen finden Sie unter [Lokales Debuggen von PowerShell Azure Functions](../azure-functions/functions-debug-powershell-local.md).


## <a name="republish-azure-function-app"></a>Erneutes Veröffentlichen der Azure-Funktions-App

Nachdem Sie sichergestellt haben, dass die Funktion auf Ihrem lokalen Computer richtig ausgeführt wird, können Sie das Projekt erneut in der vorhandenen Funktions-App in Azure veröffentlichen.

> [!NOTE]
> Denken Sie daran, alle Aufrufe von `Wait-Debugger` zu entfernen, bevor Sie Ihre Funktionen in Azure veröffentlichen.

1. Öffnen Sie in Visual Studio Code die Befehlspalette. Suchen Sie nach `Azure Functions: Deploy to function app...`, und wählen Sie diese Option aus.
1. Wählen Sie den aktuellen Arbeitsordner zum Zippen und Bereitstellen aus.
1. Wählen Sie das Abonnement und dann den Namen der vorhandenen Funktions-App (*myfunctionapp*) aus. Bestätigen Sie, dass Sie die vorherige Bereitstellung überschreiben möchten.

Nach der Erstellung der Funktions-App wird eine Benachrichtigung angezeigt, und das Bereitstellungspaket wird angewendet. Wählen Sie in dieser Benachrichtigung **Ausgabe anzeigen** aus, um die Erstellungs- und Bereitstellungsergebnisse (auch für die von Ihnen aktualisierten Azure-Ressourcen) anzuzeigen.

## <a name="run-the-function-in-azure"></a>Ausführen der Funktion in Azure

Rufen Sie nach erfolgreichem Abschluss der Bereitstellung die Funktions-URL ab. Verwenden Sie beispielsweise den Bereich **Azure: Funktionen** in Visual Studio Code, um die **HttpTrigger**-Funktions-URL zu kopieren, oder rufen Sie die URL im [Azure-Portal](../azure-functions/functions-create-first-azure-function.md#test-the-function) ab.

Die Funktions-URL enthält einen eindeutigen Code und hat folgendes Format:

```
https://myfunctionapp.azurewebsites.net/api/HttpTrigger?code=bmF/GljyfFWISqO0GngDPCtCQF4meRcBiHEoaQGeRv/Srx6dRcrk2M==
```

### <a name="run-function-without-passing-a-name"></a>Ausführen der Funktion, ohne einen Namen zu übergeben

Führen Sie als ersten Test den Befehl `curl` aus, und übergeben Sie die Funktions-URL, ohne eine `name`-Abfragezeichenfolge anzufügen. Denken Sie daran, den eindeutigen Code Ihrer Funktion einzuschließen.

```bash
curl --verbose "https://myfunctionapp.azurewebsites.net/api/HttpTrigger?code=bmF/GljyfFWISqO0GngDPCtCQF4meRcBiHEoaQGeRv/Srx6dRcrk2M=="
```

Die Funktion gibt den Statuscode 400 und den Text `Please pass a name on the query string or in the request body` zurück:

```
[...]
> GET /api/HttpTrigger?code=bmF/GljyfFWISqO0GngDPCtCQF4meRcBiHEoaQGeRv/Srx6dRcrk2M== HTTP/2
> Host: myfunctionapp.azurewebsites.net
> User-Agent: curl/7.54.0
> Accept: */*
> 
* Connection state changed (MAX_CONCURRENT_STREAMS updated)!
< HTTP/2 400 
< content-length: 62
< content-type: text/plain; charset=utf-8
< date: Mon, 05 Aug 2019 22:08:15 GMT
< 
* Connection #0 to host myfunctionapp.azurewebsites.net left intact
Please pass a name on the query string or in the request body.
```

### <a name="run-function-and-pass-the-name-of-a-container-group"></a>Ausführen der Funktion und Übergeben des Namens einer Containergruppe

Führen Sie nun den Befehl `curl` aus, indem Sie den Namen einer Containergruppe (*mycontainergroup*) als Abfragezeichenfolge `&name=mycontainergroup` anfügen:

```bash
curl --verbose "https://myfunctionapp.azurewebsites.net/api/HttpTrigger?code=bmF/GljyfFWISqO0GngDPCtCQF4meRcBiHEoaQGeRv/Srx6dRcrk2M==&name=mycontainergroup"
```

Die Funktion gibt den Statuscode 200 zurück und löst die Erstellung der Containergruppe aus:

```
[...]
> GET /api/HttpTrigger?ode=bmF/GljyfFWISqO0GngDPCtCQF4meRcBiHEoaQGeRv/Srx6dRcrk2M==&name=mycontainergroup HTTP/2
> Host: myfunctionapp.azurewebsites.net
> User-Agent: curl/7.54.0
> Accept: */*
> 
* Connection state changed (MAX_CONCURRENT_STREAMS updated)!
< HTTP/2 200 
< content-length: 28
< content-type: text/plain; charset=utf-8
< date: Mon, 05 Aug 2019 22:15:30 GMT
< 
* Connection #0 to host myfunctionapp.azurewebsites.net left intact
Started container group mycontainergroup
```

Vergewissern Sie sich mit dem Befehl [az container logs][az-container-logs], dass der Container ausgeführt wurde:

```azurecli
az container logs --resource-group myfunctionapp --name mycontainergroup
```

Beispielausgabe:

```console
Hello from an Azure container instance triggered by an Azure function
```

## <a name="clean-up-resources"></a>Bereinigen von Ressourcen

Wenn Sie die in diesem Tutorial erstellten Ressourcen nicht mehr benötigen, können Sie den Befehl [az group delete][az-group-delete] ausführen, um die Ressourcengruppe und alle darin enthaltenen Ressourcen zu entfernen. Dieser Befehl löscht die von Ihnen erstellte Containerregistrierung sowie den ausgeführten Container und alle zugehörigen Ressourcen.

```azurecli-interactive
az group delete --name myfunctionapp
```

## <a name="next-steps"></a>Nächste Schritte

In diesem Tutorial haben Sie eine Azure-Funktion erstellt, die über eine HTTP-Anforderung die Bereitstellung einer Containergruppe auslöst. Es wurde Folgendes vermittelt:

> [!div class="checklist"]
> * Verwenden von Visual Studio Code mit der Azure Functions-Erweiterung, um eine einfache über HTTP ausgelöste PowerShell-Funktion zu erstellen
> * Aktivieren einer Identität in der Funktions-App und Erteilen von Berechtigungen zum Erstellen von Azure-Ressourcen
> * Ändern des Codes der PowerShell-Funktion, um die Bereitstellung einer Containergruppe mit nur einem Container zu automatisieren
> * Überprüfen der über HTTP ausgelösten Bereitstellung des Containers

Ein ausführliches Beispiel zum Starten und Überwachen eines Containerauftrags finden Sie im Blogbeitrag [Event-Driven Serverless Containers with PowerShell Azure Functions and Azure Container Instances](https://dev.to/azure/event-driven-serverless-containers-with-powershell-azure-functions-and-azure-container-instances-e9b) (Ereignisgesteuerte serverlose Container mit PowerShell Azure Functions und Azure Container Instances) und dem zugehörigen [Codebeispiel](https://github.com/anthonychu/functions-powershell-run-aci).

Eine detaillierte Anleitung zum Erstellen von Azure-Funktionen und Veröffentlichen eines Functions-Projekts finden Sie in der [Azure Functions-Dokumentation](/azure/azure-functions/). 

<!-- IMAGES -->


<!-- LINKS - external -->
[terms-of-use]: https://azure.microsoft.com/support/legal/preview-supplemental-terms/

<!-- LINKS - internal -->

[azure-cli-install]: /cli/azure/install-azure-cli
[az-group-show]: /cli/azure/group#az-group-show
[az-group-delete]: /cli/azure/group#az-group-delete
[az-functionapp-identity-app-assign]: /cli/azure/functionapp/identity#az-functionapp-identity-assign
[new-azcontainergroup]: /powershell/module/az.containerinstance/new-azcontainergroup
[az-container-logs]: /cli/azure/container#az-container-logs
