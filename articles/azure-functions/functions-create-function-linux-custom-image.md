---
title: Erstellen von Azure-Funktionen unter Linux mit einem benutzerdefinierten Image
description: Hier erfahren Sie, wie Sie Azure Functions erstellen, die auf einem benutzerdefinierten Linux-Image ausgeführt werden.
services: functions
keywords: ''
author: ggailey777
ms.author: glenga
ms.date: 06/25/2019
ms.topic: tutorial
ms.service: azure-functions
ms.custom: mvc
ms.devlang: azure-cli
manager: jeconnoc
ms.openlocfilehash: 525cb444ad7b1b78de867f83539ac338ddd144e9
ms.sourcegitcommit: 39d95a11d5937364ca0b01d8ba099752c4128827
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 08/16/2019
ms.locfileid: "69562933"
---
# <a name="create-a-function-on-linux-using-a-custom-image"></a>Erstellen einer Funktion unter Linux mit einem benutzerdefinierten Image

Mit Azure Functions können Sie Funktionen unter Linux in einem eigenen benutzerdefinierten Container hosten. Das [Hosten in einem Azure App Service-Standardcontainer](functions-create-first-azure-function-azure-cli-linux.md) ist ebenfalls möglich. Für diese Funktion wird die [Functions-Runtime 2.x](functions-versions.md) benötigt.

In diesem Tutorial erfahren Sie, wie Sie Ihre Funktionen als benutzerdefiniertes Docker-Image in Azure bereitstellen. Dieses Muster ist hilfreich, wenn Sie das integrierte Containerimage anpassen müssen. Sie sollten ein benutzerdefiniertes Image verwenden, wenn Sie für Ihre Funktionen eine bestimmte Sprachversion benötigen oder eine bestimmte Abhängigkeit oder Konfiguration erforderlich ist, die nicht mit dem integrierten Image bereitgestellt wird. Unterstützte Basisimages für Azure Functions finden Sie im [Azure Functions-Repository für Basisimages](https://hub.docker.com/_/microsoft-azure-functions-base). 

Dieses Tutorial führt Sie durch die Schritte zum Erstellen einer Funktion in einem benutzerdefinierten Linux-Image unter Verwendung von Azure Functions Core Tools. Sie veröffentlichen dieses Image in einer Funktions-App in Azure, die über die Azure-Befehlszeilenschnittstelle erstellt wurde. Später aktualisieren Sie Ihre Funktion, um eine Verbindung mit Azure Queue Storage herzustellen. Sie nehmen auch eine Aktivierung vor.  

In diesem Tutorial lernen Sie Folgendes:

> [!div class="checklist"]
> * Erstellen einer Funktions-App und einer Dockerfile-Datei mithilfe von Core Tools.
> * Erstellen eines benutzerdefinierten Images mit Docker.
> * Veröffentlichen eines benutzerdefinierten Images in einer Containerregistrierung.
> * Erstellen eines Azure-Speicherkontos.
> * Erstellen Sie einen Premium-Hostingplan.
> * Bereitstellen einer Funktions-App über Docker Hub.
> * Hinzufügen von Anwendungseinstellungen für die Funktions-App.
> * Aktivieren Sie Continuous Deployment.
> * Fügen Sie die Application Insights-Überwachung hinzu.

Die folgenden Schritte werden für Mac-, Windows- oder Linux-Computer unterstützt. 

## <a name="prerequisites"></a>Voraussetzungen

Vor dem Ausführen dieses Beispiels benötigen Sie Folgendes:

* Installieren Sie [Azure Core Tools-Version 2.x](functions-run-local.md#v2).

* Installieren Sie die [Azure CLI]( /cli/azure/install-azure-cli). Für diesen Artikel ist die Azure CLI-Version 2.0 oder höher erforderlich. Führen Sie `az --version` aus, um herauszufinden, welche Version Sie haben.  
Sie können ebenso [Azure Cloud Shell](https://shell.azure.com/bash) verwenden.

* Ein aktives Azure-Abonnement.

[!INCLUDE [quickstarts-free-trial-note](../../includes/quickstarts-free-trial-note.md)]

## <a name="create-the-local-function-app-project"></a>Erstellen des lokalen Funktions-App-Projekts

Führen Sie den folgenden Befehl zum Erstellen eines Funktions-App-Projekts im `MyFunctionProj`-Ordner des aktuellen lokalen Verzeichnisses über die Befehlszeile aus.

```bash
func init MyFunctionProj --docker
```

Wenn Sie die Option `--docker` einschließen, wird eine Dockerfile-Datei für das Projekt generiert. Diese Datei dient zum Erstellen eines benutzerdefinierten Containers für die Projektausführung. Welches Basisimage verwendet wird, hängt von der gewählten Worker-Runtimesprache ab.  

Wählen Sie bei entsprechender Aufforderung eine der folgenden Worker-Runtimesprachen aus:

* `dotnet`: Erstellt ein .NET Core-Klassenbibliotheksprojekt (.csproj).
* `node`: Erstellt ein JavaScript-Projekt.
* `python`: Erstellt ein Python-Projekt.

Wenn der Befehl ausgeführt wird, sehen Sie etwa folgende Ausgabe:

```output
Writing .gitignore
Writing host.json
Writing local.settings.json
Writing Dockerfile
```

Verwenden Sie den folgenden Befehl, um zu dem neuen Projektordner `MyFunctionProj` zu navigieren.

```bash
cd MyFunctionProj
```

[!INCLUDE [functions-create-function-core-tools](../../includes/functions-create-function-core-tools.md)]

[!INCLUDE [functions-run-function-test-local](../../includes/functions-run-function-test-local.md)]

## <a name="build-the-image-from-the-docker-file"></a>Erstellen des Images aus der Docker-Datei

Sehen Sie sich die _Dockerfile-Datei_ im Stammordner des Projekts an. Diese Datei beschreibt die Umgebung, die zur Ausführung der Funktions-App erforderlich ist. Bei dem folgenden Beispiel handelt es sich um eine Dockerfile-Datei, die einen Container zum Ausführen einer Funktions-App in der JavaScript-Workerruntime (Node.js) erstellt: 

```Dockerfile
FROM mcr.microsoft.com/azure-functions/node:2.0

ENV AzureWebJobsScriptRoot=/home/site/wwwroot
COPY . /home/site/wwwroot
```

> [!NOTE]
> Die vollständige Liste mit den unterstützten Basisimages für Azure Functions finden Sie auf der [Seite mit Azure Functions-Basisimages](https://hub.docker.com/_/microsoft-azure-functions-base).

### <a name="run-the-build-command"></a>Führen Sie den Befehl `build` aus.
Führen Sie im Stammordner den Befehl [docker build`v1.0.0` aus, und geben Sie einen Namen (](https://docs.docker.com/engine/reference/commandline/build/)) und ein Tag (`mydockerimage`) an. Ersetzen Sie `<docker-id>` durch Ihre Docker Hub-Konto-ID. Dieser Befehl erstellt das Docker-Image für den Container.

```bash
docker build --tag <docker-id>/mydockerimage:v1.0.0 .
```

Wenn der Befehl ausgeführt wird, wird eine Ausgabe wie die folgende angezeigt (in diesem Fall für eine JavaScript-Workerruntime):

```bash
Sending build context to Docker daemon  17.41kB
Step 1/3 : FROM mcr.microsoft.com/azure-functions/node:2.0
2.0: Pulling from azure-functions/node
802b00ed6f79: Pull complete
44580ea7a636: Pull complete
73eebe8d57f9: Pull complete
3d82a67477c2: Pull complete
8bd51cd50290: Pull complete
7bd755353966: Pull complete
Digest: sha256:480e969821e9befe7c61dda353f63298f2c4b109e13032df5518e92540ea1d08
Status: Downloaded newer image for mcr.microsoft.com/azure-functions/node:2.0
 ---> 7c71671b838f
Step 2/3 : ENV AzureWebJobsScriptRoot=/home/site/wwwroot
 ---> Running in ed1e5809f0b7
Removing intermediate container ed1e5809f0b7
 ---> 39d9c341368a
Step 3/3 : COPY . /home/site/wwwroot
 ---> 5e196215935a
Successfully built 5e196215935a
Successfully tagged <docker-id>/mydockerimage:v1.0.0
```

### <a name="test-the-image-locally"></a>Lokales Testen des Images
Vergewissern Sie sich, dass das von Ihnen erstellte Image funktioniert, indem Sie das Docker-Image in einem lokalen Container ausführen. Rufen Sie den Befehl [docker run](https://docs.docker.com/engine/reference/commandline/run/) auf, und übergeben Sie ihm Namen und Tag des Images. Stellen Sie sicher, dass Sie auch den Port mit dem `-p`-Argument angeben.

```bash
docker run -p 8080:80 -it <docker-ID>/mydockerimage:v1.0.0
```

Wenn das benutzerdefinierte Image in einem lokalen Docker-Container ausgeführt wird, überprüfen Sie, ob die Funktions-App und der Container korrekt funktionieren, indem Sie zu <http://localhost:8080> navigieren.

![Funktions-App lokal testen.](./media/functions-create-function-linux-custom-image/run-image-local-success.png)

Sie können Ihre Funktion optional auch erneut testen – diesmal im lokalen Container über die folgende URL:

`http://localhost:8080/api/myhttptrigger?name=<yourname>`

Nachdem Sie die Funktions-App im Container überprüft haben, beenden Sie die Ausführung. Jetzt können Sie das benutzerdefinierte Image per Push zu Ihrem Docker Hub-Konto übertragen.

## <a name="push-the-custom-image-to-docker-hub"></a>Pushübertragung des benutzerdefinierten Images zu Docker Hub

Eine Registrierung ist eine Anwendung, die Images hostet und Image- sowie Container-Dienste für Dienste bereitstellt. Wenn Sie Ihr Image freigeben möchten, müssen Sie es an eine Registrierung pushen. Docker Hub ist eine Registrierung für Docker-Images, die Ihnen ermöglicht, Ihre eigenen Repositorys – entweder öffentlich oder privat – zu hosten.

Bevor Sie ein Image mithilfe von Push übertragen können, müssen Sie sich mit dem Befehl [docker login](https://docs.docker.com/engine/reference/commandline/login/) in Docker Hub anmelden. Ersetzen Sie `<docker-id>` durch den Namen Ihres Kontos, und geben Sie Ihr Kennwort in der Konsole an der Eingabeaufforderung ein. Weitere Kennwortoptionen für Docker Hub finden Sie in der [Dokumentation zum Docker Login-Befehl](https://docs.docker.com/engine/reference/commandline/login/) (in englischer Sprache).

```bash
docker login --username <docker-id>
```

Eine „login succeeded“-Meldung bestätigt Ihre erfolgreiche Anmeldung. Nach der Anmeldung können Sie das Image mit dem [docker push](https://docs.docker.com/engine/reference/commandline/push/)-Befehl per Push an Docker-Hub übertragen.

```bash
docker push <docker-id>/mydockerimage:v1.0.0
```

Überprüfen Sie anhand der Befehlsausgabe, ob der Pushvorgang erfolgreich war.

```bash
The push refers to a repository [docker.io/<docker-id>/mydockerimage:v1.0.0]
24d81eb139bf: Pushed
fd9e998161c9: Mounted from <docker-id>/mydockerimage
e7796c35add2: Mounted from <docker-id>/mydockerimage
ae9a05b85848: Mounted from <docker-id>/mydockerimage
45c86e20670d: Mounted from <docker-id>/mydockerimage
v1.0.0: digest: sha256:be080d80770df71234eb893fbe4d... size: 1796
```

Jetzt können Sie dieses Image als Bereitstellungsquelle für eine neue Funktions-App in Azure verwenden.

[!INCLUDE [functions-create-resource-group](../../includes/functions-create-resource-group.md)]

[!INCLUDE [functions-create-storage-account](../../includes/functions-create-storage-account.md)]

## <a name="create-a-premium-plan"></a>Erstellen eines Premium-Plans

Linux-Hosting für benutzerdefinierte Functions-Container, die von [dedizierten (App Service) Plänen](functions-scale.md#app-service-plan) und [Premium-Plänen](functions-premium-plan.md#features) unterstützt werden. Dieses Tutorial verwendet einen Premium-Plan, der nach Bedarf skaliert werden kann. Weitere Informationen zum Hosting finden Sie unter [Vergleich von Hostingplänen für Azure Functions](functions-scale.md).

Das folgende Beispiel erstellt einen Premium-Plan namens `myPremiumPlan` in der Preisstufe **Elastic Premium 1** (`--sku EP1`), in der Region „USA, Westen“ (`-location WestUS`) und in einem Linux-Container (`--is-linux`).

```azurecli-interactive
az functionapp plan create --resource-group myResourceGroup --name myPremiumPlan \
--location WestUS --number-of-workers 1 --sku EP1 --is-linux
```

## <a name="create-and-deploy-the-custom-image"></a>Erstellen und Bereitstellen des benutzerdefinierten Images

Die Funktions-App verwaltet die Ausführung Ihrer Funktionen in Ihrem Hostingplan. Erstellen Sie mit dem Befehl [az functionapp create](/cli/azure/functionapp#az-functionapp-create) eine Funktions-App aus einem Docker Hub-Image.

Ersetzen Sie im folgenden Befehl den Platzhalter `<app_name>` durch einen eindeutigen Namen der Funktionen-App und den Platzhalter `<storage_name>` durch den Speicherkontonamen. Da `<app_name>` als DNS-Standarddomäne für die Funktionen-App verwendet wird, muss der Name für alle Apps in Azure eindeutig sein. Wie zuvor steht `<docker-id>` für den Namen Ihres Docker-Kontos.

```azurecli-interactive
az functionapp create --name <app_name> --storage-account  <storage_name>  --resource-group myResourceGroup \
--plan myPremiumPlan --deployment-container-image-name <docker-id>/mydockerimage:v1.0.0
```

Der Parameter _deployment-container-image-name_ gibt das in Docker Hub gehostete Image an, das zum Erstellen der Funktions-App verwendet werden soll. Verwenden Sie den Befehl [az functionapp config container show](/cli/azure/functionapp/config/container#az-functionapp-config-container-show), um Informationen zum für die Bereitstellung verwendeten Image anzuzeigen. Verwenden Sie den Befehl [az functionapp config container set](/cli/azure/functionapp/config/container#az-functionapp-config-container-set), um ein anderes Image für die Bereitstellung zu verwenden.

## <a name="configure-the-function-app"></a>Konfigurieren der Funktions-App

Die Funktion benötigt die Verbindungszeichenfolge zum Herstellen der Verbindung mit dem Standardspeicherkonto. Beim Veröffentlichen des benutzerdefinierten Images in einem privaten Containerkonto müssen Sie diese Anwendungseinstellungen als Umgebungsvariablen in der Dockerfile-Datei festlegen, indem Sie die [ENV-Anweisung](https://docs.docker.com/engine/reference/builder/#env) oder etwas Ähnliches verwenden.

In diesem Fall ist `<storage_name>` der Name des Speicherkontos, das Sie erstellt haben. Rufen Sie die Verbindungszeichenfolge mit dem Befehl [az storage account show-connection-string](/cli/azure/storage/account) ab. Fügen Sie diese Anwendungseinstellungen in der Funktions-App mit dem Befehl [az functionapp config appsettings set](/cli/azure/functionapp/config/appsettings#az-functionapp-config-appsettings-set) hinzu.

```azurecli-interactive
storageConnectionString=$(az storage account show-connection-string \
--resource-group myResourceGroup --name <storage_name> \
--query connectionString --output tsv)

az functionapp config appsettings set --name <app_name> \
--resource-group myResourceGroup \
--settings AzureWebJobsDashboard=$storageConnectionString \
AzureWebJobsStorage=$storageConnectionString
```

> [!NOTE]
> Ist Ihr Container privat, müssen Sie außerdem die folgenden Anwendungseinstellungen festlegen:  
> - DOCKER_REGISTRY_SERVER_USERNAME  
> - DOCKER_REGISTRY_SERVER_PASSWORD  
>
> Sie müssen Ihre Funktions-App beenden und dann wieder starten, damit diese Werte übernommen werden.

Sie können jetzt Ihre Funktionen unter Linux in Azure testen.

[!INCLUDE [functions-test-function-code](../../includes/functions-test-function-code.md)]

## <a name="enable-continuous-deployment"></a>Aktivieren von Continuous Deployment

Die Verwendung von Containern hat unter anderem den Vorteil, dass Updates automatisch bereitgestellt werden können, wenn Container in der Registrierung aktualisiert werden. Verwenden Sie den Befehl [az functionapp deployment container config](/cli/azure/functionapp/deployment/container#az-functionapp-deployment-container-config), um Continuous Deployment zu aktivieren.

```azurecli-interactive
az functionapp deployment container config --enable-cd \
--query CI_CD_URL --output tsv \
--name <app_name> --resource-group myResourceGroup
```

Nach Aktivierung von Continuous Deployment gibt dieser Befehl die Webhook-URL der Bereitstellung zurück. Diese URL kann auch mithilfe des Befehls [az functionapp deployment container show-cd-url](/cli/azure/functionapp/deployment/container#az-functionapp-deployment-container-show-cd-url) zurückgegeben werden. 

Kopieren Sie die Bereitstellungs-URL, navigieren Sie zu Ihrem DockerHub-Repository, wählen Sie die Registerkarte **Webhooks** aus, geben Sie unter **Webhookname** einen Namen für den Webhook ein, fügen Sie unter **Webhook-URL** Ihre URL ein, und wählen Sie anschließend das Pluszeichen ( **+** ) aus.

![Hinzufügen des Webhooks in Ihrem DockerHub-Repository](media/functions-create-function-linux-custom-image/dockerhub-set-continuous-webhook.png)  

Nachdem Sie den Webhook festgelegt haben, lädt die Funktions-App nach jeder Aktualisierung des verknüpften Images in DockerHub das neueste Image herunter und installiert es.

## <a name="enable-application-insights"></a>Aktivieren von Application Insights

Die empfohlene Methode zum Überwachen der Ausführung Ihrer Funktionen ist die Integration Ihrer Funktions-App in Azure Application Insights. Wenn Sie eine Funktions-App im Azure-Portal erstellen, wird diese Integration standardmäßig für Sie erledigt. Wenn Sie Ihre Funktions-App mithilfe der Azure CLI erstellen, erfolgt die Integration in Ihre Funktions-App in Azure nicht.

So aktivieren Sie Application Insights für Ihre Funktions-App

[!INCLUDE [functions-connect-new-app-insights.md](../../includes/functions-connect-new-app-insights.md)]

Weitere Informationen finden Sie unter [Überwachen von Azure Functions](functions-monitoring.md).

[!INCLUDE [functions-cleanup-resources](../../includes/functions-cleanup-resources.md)]

## <a name="next-steps"></a>Nächste Schritte

In diesem Tutorial haben Sie Folgendes gelernt:

> [!div class="checklist"]
> * Erstellen einer Funktions-App und einer Dockerfile-Datei mithilfe von Core Tools.
> * Erstellen eines benutzerdefinierten Images mit Docker.
> * Veröffentlichen eines benutzerdefinierten Images in einer Containerregistrierung.
> * Erstellen eines Azure-Speicherkontos.
> * Erstellen Sie einen Linux-Premium-Plan.
> * Bereitstellen einer Funktions-App über Docker Hub.
> * Hinzufügen von Anwendungseinstellungen für die Funktions-App.
> * Aktivieren Sie Continuous Deployment.
> * Fügen Sie die Application Insights-Überwachung hinzu.

> [!div class="nextstepaction"] 
> [Erfahren Sie mehr über Optionen für die Bereitstellung von Funktionen in Azure](functions-deployment-technologies.md)
