---
title: Erstellen von Azure-Funktionen unter Linux mit einem benutzerdefinierten Image
description: Hier erfahren Sie, wie Sie Azure Functions erstellen, die auf einem benutzerdefinierten Linux-Image ausgeführt werden.
ms.date: 09/27/2019
ms.topic: tutorial
ms.custom: mvc
ms.openlocfilehash: 7bf079f84978539735f3bbf5bb13b18130871fb1
ms.sourcegitcommit: 8cf199fbb3d7f36478a54700740eb2e9edb823e8
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 11/25/2019
ms.locfileid: "74484384"
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
> * Aktivieren Sie SSH-Verbindungen mit dem Container.
> * Fügen Sie eine Queue Storage-Ausgabebindung hinzu. 

Die folgenden Schritte werden für Mac-, Windows- oder Linux-Computer unterstützt. 

## <a name="prerequisites"></a>Voraussetzungen

Vor dem Ausführen dieses Beispiels benötigen Sie Folgendes:

* Installieren Sie [Azure Core Tools-Version 2.x](functions-run-local.md#v2).

* Installieren Sie die [Azure CLI]( /cli/azure/install-azure-cli). Für diesen Artikel ist die Azure CLI-Version 2.0 oder höher erforderlich. Führen Sie `az --version` aus, um herauszufinden, welche Version Sie haben.  
Sie können ebenso [Azure Cloud Shell](https://shell.azure.com/bash) verwenden.

* Ein aktives Azure-Abonnement.

[!INCLUDE [quickstarts-free-trial-note](../../includes/quickstarts-free-trial-note.md)]

[!INCLUDE [functions-cloud-shell-note](../../includes/functions-cloud-shell-note.md)]

## <a name="create-the-local-project"></a>Erstellen des lokalen Projekts

Führen Sie den folgenden Befehl zum Erstellen eines Funktions-App-Projekts im `MyFunctionProj`-Ordner des aktuellen lokalen Verzeichnisses über die Befehlszeile aus. Für ein Python-Projekt muss eine [virtuelle Umgebung](functions-create-first-function-python.md#create-and-activate-a-virtual-environment) verwendet werden.

```bash
func init MyFunctionProj --docker
```

Wenn Sie die Option `--docker` einschließen, wird eine Dockerfile-Datei für das Projekt generiert. Diese Datei dient zum Erstellen eines benutzerdefinierten Containers für die Projektausführung. Welches Basisimage verwendet wird, hängt von der gewählten Worker-Runtimesprache ab.  

Wählen Sie bei entsprechender Aufforderung eine der folgenden Worker-Runtimesprachen aus:

* `dotnet`: Erstellt ein .NET Core-Klassenbibliotheksprojekt (.csproj).
* `node`: Erstellt ein JavaScript-Projekt.
* `python`: Erstellt ein Python-Projekt.  

Verwenden Sie den folgenden Befehl, um zu dem neuen Projektordner `MyFunctionProj` zu navigieren.

```bash
cd MyFunctionProj
```

[!INCLUDE [functions-create-function-core-tools](../../includes/functions-create-function-core-tools.md)]

[!INCLUDE [functions-run-function-test-local](../../includes/functions-run-function-test-local.md)]

## <a name="build-from-the-docker-file"></a>Erstellen aus der Docker-Datei

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

Nachdem der Befehl abgeschlossen wurde, können Sie den neuen Container lokal ausführen.

### <a name="run-the-image-locally"></a>Lokales Ausführen des Images
Vergewissern Sie sich, dass das von Ihnen erstellte Image funktioniert, indem Sie das Docker-Image in einem lokalen Container ausführen. Rufen Sie den Befehl [docker run](https://docs.docker.com/engine/reference/commandline/run/) auf, und übergeben Sie ihm Namen und Tag des Images. Stellen Sie sicher, dass Sie auch den Port mit dem `-p`-Argument angeben.

```bash
docker run -p 8080:80 -it <docker-ID>/mydockerimage:v1.0.0
```

Wenn das benutzerdefinierte Image in einem lokalen Docker-Container ausgeführt wird, überprüfen Sie, ob die Funktions-App und der Container korrekt funktionieren, indem Sie zu <http://localhost:8080> navigieren.

![Führen Sie die Funktions-App lokal aus.](./media/functions-create-function-linux-custom-image/run-image-local-success.png)

> [!NOTE]
> Wenn Sie nun versuchen, Ihre spezielle HTTP-Funktion aufzurufen, wird der Fehler „HTTP 401“ zurückgegeben. Das liegt daran, dass die Funktion im lokalen Container genau wie in Azure ausgeführt wird, was bedeutet, dass der Funktionsschlüssel erforderlich ist. Da der Container noch nicht in einer Funktions-App veröffentlicht wurde, ist kein Funktionsschlüssel verfügbar. Die Funktionsschlüssel werden Ihnen später angezeigt, wenn Sie Ihren Container mit Core Tools veröffentlichen. Wenn Sie die im lokalen Container ausgeführte Funktion testen möchten, können Sie den [Autorisierungsschlüssel](functions-bindings-http-webhook.md#authorization-keys) in `anonymous` ändern. 

Nachdem Sie die Funktions-App im Container überprüft haben, beenden Sie die Ausführung. Jetzt können Sie das benutzerdefinierte Image per Push zu Ihrem Docker Hub-Konto übertragen.

## <a name="push-to-docker-hub"></a>An Docker Hub pushen

Eine Registrierung ist eine Anwendung, die Images hostet und Image- sowie Container-Dienste für Dienste bereitstellt. Wenn Sie Ihr Image freigeben möchten, müssen Sie es an eine Registrierung pushen. Docker Hub ist eine Registrierung für Docker-Images, die Ihnen ermöglicht, Ihre eigenen Repositorys – entweder öffentlich oder privat – zu hosten.

Bevor Sie ein Image mithilfe von Push übertragen können, müssen Sie sich mit dem Befehl [docker login](https://docs.docker.com/engine/reference/commandline/login/) in Docker Hub anmelden. Ersetzen Sie `<docker-id>` durch den Namen Ihres Kontos, und geben Sie Ihr Kennwort in der Konsole an der Eingabeaufforderung ein. Weitere Kennwortoptionen für Docker Hub finden Sie in der [Dokumentation zum Docker Login-Befehl](https://docs.docker.com/engine/reference/commandline/login/) (in englischer Sprache).

```bash
docker login --username <docker-id>
```

Die Meldung „login succeeded“ bestätigt Ihre erfolgreiche Anmeldung. Nach der Anmeldung können Sie das Image mit dem [docker push](https://docs.docker.com/engine/reference/commandline/push/)-Befehl per Push an Docker-Hub übertragen.

```bash
docker push <docker-id>/mydockerimage:v1.0.0
```

Nach der erfolgreichen Push-Übertragung können Sie das Image als Bereitstellungsquelle für eine neue Funktions-App in Azure verwenden.

[!INCLUDE [functions-create-resource-group](../../includes/functions-create-resource-group.md)]

[!INCLUDE [functions-create-storage-account](../../includes/functions-create-storage-account.md)]

## <a name="create-a-premium-plan"></a>Erstellen eines Premium-Plans

Linux-Hosting für benutzerdefinierte Functions-Container, die von [dedizierten (App Service) Plänen](functions-scale.md#app-service-plan) und [Premium-Plänen](functions-premium-plan.md#features) unterstützt werden. Dieses Tutorial verwendet einen Premium-Plan, der nach Bedarf skaliert werden kann. Weitere Informationen zum Hosting finden Sie unter [Vergleich von Hostingplänen für Azure Functions](functions-scale.md).

Das folgende Beispiel erstellt einen Premium-Plan namens `myPremiumPlan` in der Preisstufe **Elastic Premium 1** (`--sku EP1`), in der Region „USA, Westen“ (`-location WestUS`) und in einem Linux-Container (`--is-linux`).

```azurecli-interactive
az functionapp plan create --resource-group myResourceGroup --name myPremiumPlan \
--location WestUS --number-of-workers 1 --sku EP1 --is-linux
```

## <a name="create-an-app-from-the-image"></a>App aus dem Image erstellen

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

## <a name="verify-your-functions"></a>Überprüfen der Funktionen

<!-- we should replace this with a CLI or API-based approach, when we get something better than REST -->

Die von Ihnen erstellte über HTTP ausgelöste Funktion erfordert beim Aufrufen des Endpunkts einen [Funktionsschlüssel](functions-bindings-http-webhook.md#authorization-keys). Zu diesem Zeitpunkt ist die einfachste Methode zum Abrufen Ihrer Funktions-URL, einschließlich des Schlüssels, die Verwendung des [Azure-Portal]. 

> [!TIP]
> Sie können Ihre Funktionsschlüsseln auch über die [Schlüsselverwaltungs-APIs](https://github.com/Azure/azure-functions-host/wiki/Key-management-API) beziehen, für die Sie ein [Bearertoken zur Authentifizierung](/cli/azure/account#az-account-get-access-token) vorlegen müssen.

Suchen Sie Ihre neue Funktions-App im [Azure-Portal], indem Sie den Namen Ihrer Funktions-App in das **Suchfeld** oben auf der Seite eingeben und die Ressource **App-Service** auswählen.

Wählen Sie die Funktion **MyHttpTrigger** aus, wählen Sie **</> Funktions-URL abrufen** > **Standard (Funktionsschlüssel)**  > **Kopieren** aus.

![Kopieren der URL der Funktion aus dem Azure-Portal](./media/functions-create-function-linux-custom-image/functions-portal-get-url-key.png)

In dieser URL ist der Funktionsschlüssel der Abfrageparameter `code`. 

> [!NOTE]  
> Da Ihre Funktions-App als Container bereitgestellt ist, können Sie im Portal keine Änderungen an Ihrem Funktionscode vornehmen. Stattdessen müssen Sie das Projekt im lokalen Container aktualisieren und erneut in Azure veröffentlichen.

Fügen Sie die URL der Funktion in die Adressleiste Ihres Browsers ein. Fügen Sie den Wert der Abfragezeichenfolge `&name=<yourname>` am Ende der URL hinzu, und drücken Sie die Taste `Enter` auf Ihrer Tastatur, um die Anforderung auszuführen. Daraufhin sollte die von der Funktion zurückgegebene Antwort im Browser angezeigt werden.

Das folgende Beispiel zeigt die Antwort im Browser:

![Funktionsantwort im Browser.](./media/functions-create-function-linux-custom-image/function-app-browser-testing.png)

Die Anfrage-URL enthält einen Schlüssel, der standardmäßig über HTTP auf Ihre Funktion zugreifen muss. 

## <a name="enable-continuous-deployment"></a>Aktivieren von Continuous Deployment

Einer der Vorteile der Verwendung von Containern ist die Unterstützung von Continuous Deployment. Mit Functions können Sie Updates automatisch bereitstellen, wenn Ihr Container in der Registrierung aktualisiert wird. Verwenden Sie den Befehl [az functionapp deployment container config](/cli/azure/functionapp/deployment/container#az-functionapp-deployment-container-config), um Continuous Deployment zu aktivieren.

```azurecli-interactive
az functionapp deployment container config --enable-cd \
--query CI_CD_URL --output tsv \
--name <app_name> --resource-group myResourceGroup
```

Nach Aktivierung von Continuous Deployment gibt dieser Befehl die Webhook-URL der Bereitstellung zurück. Diese URL kann auch mithilfe des Befehls [az functionapp deployment container show-cd-url](/cli/azure/functionapp/deployment/container#az-functionapp-deployment-container-show-cd-url) zurückgegeben werden. 

Kopieren Sie die Bereitstellungs-URL, navigieren Sie zu Ihrem DockerHub-Repository, wählen Sie die Registerkarte **Webhooks** aus, geben Sie unter **Webhookname** einen Namen für den Webhook ein, fügen Sie unter **Webhook-URL** Ihre URL ein, und wählen Sie anschließend das Pluszeichen ( **+** ) aus.

![Hinzufügen des Webhooks in Ihrem DockerHub-Repository](./media/functions-create-function-linux-custom-image/dockerhub-set-continuous-webhook.png)  

Nachdem Sie den Webhook festgelegt haben, lädt die Funktions-App nach jeder Aktualisierung des verknüpften Images in DockerHub das neueste Image herunter und installiert es.

## <a name="enable-ssh-connections"></a>Aktivieren von SSH-Verbindungen

SSH ermöglicht die sichere Kommunikation zwischen einem Container und einem Client. Wenn SSH aktiviert ist, können Sie mithilfe der erweiterten App Service-Tools (Kudu) eine Verbindung mit Ihrem Container herstellen. Functions stellt ein Basisimage bereit, für das SSH bereits aktiviert ist, um das Herstellen einer Verbindung mit Ihrem Container mithilfe von SSH zu vereinfachen. 

### <a name="change-the-base-image"></a>Ändern des Basisimages

Fügen Sie in Ihrer Dockerfile die Zeichenfolge `-appservice` an das Basisimage in Ihrer `FROM`-Anweisung an, die für ein JavaScript-Projekt wie folgt aussieht.

```docker
FROM mcr.microsoft.com/azure-functions/node:2.0-appservice
```

Die Unterschiede in den beiden Basisimages ermöglichen SSH-Verbindungen in Ihren Container. Diese Unterschiede werden in [diesem App Services-Tutorial](../app-service/containers/tutorial-custom-docker-image.md#enable-ssh-connections) ausführlich erläutert.

### <a name="rebuild-and-redeploy-the-image"></a>Neuerstellen und erneutes Bereitstellen des Images

Führen Sie im Stammordner den Befehl [docker build](https://docs.docker.com/engine/reference/commandline/build/) erneut aus, und ersetzen Sie `<docker-id>` wie zuvor durch die ID Ihres Docker Hub-Kontos. 

```bash
docker build --tag <docker-id>/mydockerimage:v1.0.0 .
```

Pushen Sie das aktualisierte Image zurück in Docker Hub.

```bash
docker push <docker-id>/mydockerimage:v1.0.0
```

Das aktualisierte Image wird in ihrer Funktions-App erneut bereitgestellt.

### <a name="connect-to-your-container-in-azure"></a>Herstellen einer Verbindung mit Ihrem Container in Azure

Navigieren Sie im Browser zum folgenden `scm.`-Endpunkt der erweiterten Tools (Kudu) für Ihren Funktions-App-Container, und ersetzen Sie `<app_name>` dabei durch den Namen Ihrer Funktions-App.

```
https://<app_name>.scm.azurewebsites.net/
```

Melden Sie sich bei Ihrem Azure-Konto an, und wählen Sie dann die Registerkarte **SSH** aus, um eine SSH-Verbindung mit Ihrem Container herzustellen.

Nachdem die Verbindung hergestellt wurde, führen Sie den Befehl `top` aus, um die derzeit ausgeführten Prozesse anzuzeigen. 

![Linux-Befehl „top“, der in einer SSH-Sitzung ausgeführt wird](media/functions-create-function-linux-custom-image/linux-custom-kudu-ssh-top.png)

## <a name="write-to-queue-storage"></a>Schreiben in Queue Storage

Functions ermöglicht Ihnen das Verbinden von Azure-Diensten und anderen Ressourcen mit Funktionen, ohne dass Sie Ihren eigenen Integrationscode schreiben müssen. Diese *Bindungen*, die sowohl Eingabe als auch Ausgabe darstellen, werden innerhalb der Funktionsdefinition deklariert. Daten von Bindungen werden der Funktion als Parameter bereitgestellt. Ein *Trigger* ist ein spezieller Typ von Eingabebindung. Eine Funktion hat zwar nur einen Trigger, kann aber mehrere Ein- und Ausgabebindungen haben. Weitere Informationen finden Sie unter [Konzepte der Trigger und Bindungen in Azure Functions](functions-triggers-bindings.md).

In diesem Abschnitt wird gezeigt, wie Sie Ihre Funktion in eine Azure Storage-Warteschlange integrieren. Die Ausgabebindung, die Sie dieser Funktion hinzufügen, schreibt Daten aus einer HTTP-Anforderung in eine Nachricht in der Warteschlange.

### <a name="download-the-function-app-settings"></a>Herunterladen der Funktions-App-Einstellungen

[!INCLUDE [functions-app-settings-download-local-cli](../../includes/functions-app-settings-download-local-cli.md)]

### <a name="enable-extension-bundles"></a>Aktivieren von Erweiterungsbundles

Da Sie eine Queue Storage-Ausgabebindung verwenden, müssen Sie vor dem Ausführen des Projekts die Storage-Bindungserweiterung installieren. 


# <a name="javascript--pythontabnodejspython"></a>[JavaScript/Python](#tab/nodejs+python)

[!INCLUDE [functions-extension-bundles](../../includes/functions-extension-bundles.md)]

# <a name="ctabcsharp"></a>[C\#](#tab/csharp)

Mit Ausnahme von HTTP- und Timertriggern werden Bindungen als Erweiterungspakete implementiert. Führen Sie den folgenden [dotnet add package](/dotnet/core/tools/dotnet-add-package)-Befehl im Terminalfenster aus, um Ihrem Projekt das Storage-Erweiterungspaket hinzuzufügen.

```bash
dotnet add package Microsoft.Azure.WebJobs.Extensions.Storage --version 3.0.4
```

> [!TIP]
> Wenn Sie Visual Studio verwenden, können Sie dieses Paket auch mithilfe des NuGet-Paket-Managers hinzufügen.

---

Dann können Sie dem Projekt eine Storage-Ausgabebindung hinzufügen.

### <a name="add-an-output-binding"></a>Hinzufügen einer Ausgabebindung

In Functions muss für jeden Typ von Bindung eine `direction`, ein `type` und ein eindeutiger `name` in der Datei „function.json“ definiert werden. Wie Sie diese Attribute definieren, hängt von der Sprache der Funktions-App ab.

# <a name="javascript--pythontabnodejspython"></a>[JavaScript/Python](#tab/nodejs+python)

[!INCLUDE [functions-add-output-binding-json](../../includes/functions-add-output-binding-json.md)]

# <a name="ctabcsharp"></a>[C\#](#tab/csharp)

[!INCLUDE [functions-add-storage-binding-csharp-library](../../includes/functions-add-storage-binding-csharp-library.md)]

---

### <a name="add-code-that-uses-the-output-binding"></a>Hinzufügen von Code, der die Ausgabebindung verwendet

Nachdem die Bindung definiert wurde, können Sie den `name` der Bindung verwenden, um auf sie als Attribut in der Funktionssignatur zuzugreifen. Durch die Verwendung einer Ausgabebindung müssen Sie weder den Azure Storage-SDK-Code für die Authentifizierung verwenden, noch einen Warteschlangenverweis abrufen oder Daten schreiben. Die Functions-Runtime und die Warteschlangenausgabebindung übernehmen diese Aufgaben für Sie.

# <a name="javascripttabnodejs"></a>[JavaScript](#tab/nodejs)

[!INCLUDE [functions-add-output-binding-js](../../includes/functions-add-output-binding-js.md)]

# <a name="pythontabpython"></a>[Python](#tab/python)

[!INCLUDE [functions-add-output-binding-python](../../includes/functions-add-output-binding-python.md)]

# <a name="ctabcsharp"></a>[C\#](#tab/csharp)

[!INCLUDE [functions-add-storage-binding-csharp-library-code](../../includes/functions-add-storage-binding-csharp-library-code.md)]

---

### <a name="update-the-hosted-container"></a>Aktualisieren des gehosteten Containers

Führen Sie im Stammordner den Befehl [docker build](https://docs.docker.com/engine/reference/commandline/build/) erneut aus, und aktualisieren Sie dieses Mal die Version im Tag auf `v1.0.2`. Ersetzen Sie `<docker-id>` wie zuvor durch Ihre Docker Hub-Konto-ID. 

```bash
docker build --tag <docker-id>/mydockerimage:v1.0.0 .
```

Pushen Sie das aktualisierte Image zurück in das Repository.

```bash
docker push <docker-id>/mydockerimage:v1.0.0
```

### <a name="verify-the-updates-in-azure"></a>Überprüfen der Updates in Azure

Verwenden Sie im Browser dieselbe URL wie zuvor, um ihre Funktion auszulösen. Daraufhin sollte dieselbe Antwort angezeigt werden. Diesmal wird die Zeichenfolge, die Sie als `name`-Parameter übergeben, jedoch in die Speicherwarteschlange `outqueue` geschrieben.

[!INCLUDE [functions-storage-account-set-cli](../../includes/functions-storage-account-set-cli.md)]

[!INCLUDE [functions-query-storage-cli](../../includes/functions-query-storage-cli.md)]

[!INCLUDE [functions-cleanup-resources](../../includes/functions-cleanup-resources.md)]

## <a name="next-steps"></a>Nächste Schritte

Nachdem Sie Ihren benutzerdefinierten Container nun erfolgreich in einer Funktions-App in Azure bereitgestellt haben, sollten Sie mehr über die folgenden Themen lesen:

+ [Überwachen von Funktionen](functions-monitoring.md)
+ [Skalierungs- und Hostingoptionen](functions-scale.md)
+ [Kubernetes-basiertes serverloses Hosting](functions-kubernetes-keda.md)

[Azure-Portal]: https://portal.azure.com
