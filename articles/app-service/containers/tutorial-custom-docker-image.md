---
title: 'Erstellen eines benutzerdefinierten Images für Web-App für Container: Azure App Service | Microsoft-Dokumentation'
description: Verwenden eines benutzerdefinierten Docker-Images für Web-App für Container.
keywords: Azure App Service, Web-App, Linux, Docker, Container
services: app-service
documentationcenter: ''
author: msangapu
manager: jeconnoc
editor: ''
ms.assetid: b97bd4e6-dff0-4976-ac20-d5c109a559a8
ms.service: app-service
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: tutorial
ms.date: 03/27/2019
ms.author: msangapu
ms.custom: seodec18
ms.openlocfilehash: b48ec72a1f0a4178dad66ed31c544399e90c5293
ms.sourcegitcommit: 9b80d1e560b02f74d2237489fa1c6eb7eca5ee10
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 07/01/2019
ms.locfileid: "67484495"
---
# <a name="tutorial-build-a-custom-image-and-run-in-app-service-from-a-private-registry"></a>Tutorial: Erstellen eines benutzerdefinierten Images und Ausführen in App Service über eine private Registrierung

[App Service](app-service-linux-intro.md) verfügt über integrierte Docker-Images unter Linux mit Unterstützung für bestimmte Versionen, z.B. PHP 7.3 und Node.js 10.14. Für App Service wird die Containertechnologie von Docker genutzt, um sowohl integrierte als auch benutzerdefinierte Images als „Platform-as-a-Service“ zu hosten. In diesem Tutorial wird beschrieben, wie Sie ein benutzerdefiniertes Image erstellen und in App Service ausführen. Dieses Muster ist hilfreich, wenn die integrierten Images nicht die Sprache Ihrer Wahl verwenden oder Ihre Anwendung eine bestimmte Konfiguration erfordert, die nicht in den integrierten Images bereitgestellt wird.

In diesem Tutorial lernen Sie Folgendes:

> [!div class="checklist"]
> * Bereitstellen eines benutzerdefinierten Images in einer privaten Containerregistrierung
> * Ausführen des benutzerdefinierten Images in App Service
> * Konfigurieren von Umgebungsvariablen
> * Aktualisieren und erneutes Bereitstellen des Images
> * Zugreifen auf Diagnoseprotokolle
> * Herstellen einer Verbindung mit dem Container per SSH

[!INCLUDE [Free trial note](../../../includes/quickstarts-free-trial-note.md)]

## <a name="prerequisites"></a>Voraussetzungen

Für dieses Tutorial benötigen Sie Folgendes:

* [Git-Client](https://git-scm.com/downloads)
* [Docker](https://docs.docker.com/get-started/#setup)

## <a name="download-the-sample"></a>Herunterladen des Beispiels

Führen Sie in einem Terminalfenster den folgenden Befehl aus, um das Beispiel-App-Repository auf Ihren lokalen Computer zu klonen, und navigieren Sie dann zum Verzeichnis mit dem Beispielcode.

```bash
git clone https://github.com/Azure-Samples/docker-django-webapp-linux.git --config core.autocrlf=input
cd docker-django-webapp-linux
```

## <a name="build-the-image-from-the-docker-file"></a>Erstellen des Images aus der Docker-Datei

Betrachten Sie _Dockerfile_ im Git-Repository. Diese Datei beschreibt die Python-Umgebung, die zur Ausführung Ihrer Anwendung erforderlich ist. Darüber hinaus richtet das Image einen [SSH](https://www.ssh.com/ssh/protocol/)-Server für eine sichere Kommunikation zwischen dem Container und dem Host ein.

```Dockerfile
FROM python:3.4

RUN mkdir /code
WORKDIR /code
ADD requirements.txt /code/
RUN pip install -r requirements.txt
ADD . /code/

# ssh
ENV SSH_PASSWD "root:Docker!"
RUN apt-get update \
        && apt-get install -y --no-install-recommends dialog \
        && apt-get update \
    && apt-get install -y --no-install-recommends openssh-server \
    && echo "$SSH_PASSWD" | chpasswd 

COPY sshd_config /etc/ssh/
COPY init.sh /usr/local/bin/
    
RUN chmod u+x /usr/local/bin/init.sh
EXPOSE 8000 2222
#CMD ["python", "/code/manage.py", "runserver", "0.0.0.0:8000"]
ENTRYPOINT ["init.sh"]
```

Erstellen Sie das Docker-Image mit dem Befehl `docker build`.

```bash
docker build --tag mydockerimage .
```

Testen Sie, ob der Build funktioniert, indem Sie den Docker-Container ausführen. Führen Sie den Befehl [`docker run`](https://docs.docker.com/engine/reference/commandline/run/) aus, und übergeben Sie dafür den Namen und das Tag des Images. Stellen Sie sicher, dass Sie auch den Port mit dem `-p`-Argument angeben.

```bash
docker run -p 8000:8000 mydockerimage
```

Überprüfen Sie, ob Web-App und Container ordnungsgemäß funktionieren, indem Sie `http://localhost:8000` lokal aufrufen.

![Lokales Testen der Web-App](./media/app-service-linux-using-custom-docker-image/app-service-linux-browse-local.png)

[!INCLUDE [Try Cloud Shell](../../../includes/cloud-shell-try-it.md)]

## <a name="deploy-app-to-azure"></a>Bereitstellen von Apps in Azure

Zum Erstellen einer App, für die das gerade erstellte Image verwendet wird, führen Sie Azure CLI-Befehle aus, mit denen eine Ressourcengruppe erstellt, das Image per Pushvorgang übertragen und anschließend die Web-App des App Service-Plans für die Ausführung erstellt wird.

### <a name="create-a-resource-group"></a>Erstellen einer Ressourcengruppe

[!INCLUDE [Create resource group](../../../includes/app-service-web-create-resource-group-linux-no-h.md)] 

### <a name="create-an-azure-container-registry"></a>Erstellen einer Azure-Containerregistrierung

Erstellen Sie mit dem Befehl [`az acr create`](/cli/azure/acr?view=azure-cli-latest#az-acr-create) in der Cloud Shell eine Azure Container Registry.

```azurecli-interactive
az acr create --name <azure-container-registry-name> --resource-group myResourceGroup --sku Basic --admin-enabled true
```

### <a name="sign-in-to-azure-container-registry"></a>Anmelden bei Azure Container Registry

Zum Übertragen eines Images an die Registrierung per Pushvorgang müssen Sie die Authentifizierung für die private Registrierung durchführen. Verwenden Sie in der Cloud Shell den Befehl [`az acr show`](/cli/azure/acr?view=azure-cli-latest#az-acr-show), um die Anmeldeinformationen aus der von Ihnen erstellten Registrierung abzurufen.

```azurecli-interactive
az acr credential show --name <azure-container-registry-name>
```

Die Ausgabe enthält zwei Kennwörter und den Benutzernamen.

```json
<
  "passwords": [
    {
      "name": "password",
      "value": "{password}"
    },
    {
      "name": "password2",
      "value": "{password}"
    }
  ],
  "username": "<registry-username>"
}
```

Melden Sie sich über Ihr lokales Terminalfenster mit dem Befehl `docker login` an der Azure Container Registry-Instanz an. Dies ist im folgenden Beispiel dargestellt. Ersetzen Sie *\<azure-container-registry-name>* und *\<registry-username>* durch die Werte für Ihre Registrierung. Geben Sie bei entsprechender Aufforderung eines der Kennwörter aus dem vorherigen Schritt ein.

```bash
docker login <azure-container-registry-name>.azurecr.io --username <registry-username>
```

Vergewissern Sie sich, dass die Anmeldung erfolgreich war.

### <a name="push-image-to-azure-container-registry"></a>Übertragen des Images zu Azure Container Registry mithilfe von Push

Kennzeichnen Sie Ihr lokales Image für die Azure Container Registry. Beispiel:
```bash
docker tag mydockerimage <azure-container-registry-name>.azurecr.io/mydockerimage:v1.0.0
```

Übertragen Sie das Image mithilfe des Befehls `docker push`. Kennzeichnen Sie das Image mit dem Namen der Registrierung, gefolgt von Name und Tag des Image.

```bash
docker push <azure-container-registry-name>.azurecr.io/mydockerimage:v1.0.0
```

Stellen Sie in der Cloud Shell sicher, dass der Pushvorgang erfolgreich war.

```azurecli-interactive
az acr repository list -n <azure-container-registry-name>
```

Sie sollten die folgende Ausgabe erhalten.

```json
[
  "mydockerimage"
]
```

### <a name="create-app-service-plan"></a>App Service-Plan erstellen

[!INCLUDE [Create app service plan](../../../includes/app-service-web-create-app-service-plan-linux-no-h.md)]

### <a name="create-web-app"></a>Web-App erstellen

Erstellen Sie in der Cloud Shell im App Service-Plan `myAppServicePlan` mit dem Befehl [`az webapp create`](/cli/azure/webapp?view=azure-cli-latest#az-webapp-create) eine [Web-App](app-service-linux-intro.md). Ersetzen Sie _\<app-name>_ durch einen eindeutigen App-Namen und _\<azure-container-registry-name>_ durch Ihren Registrierungsnamen.

```azurecli-interactive
az webapp create --resource-group myResourceGroup --plan myAppServicePlan --name <app-name> --deployment-container-image-name <azure-container-registry-name>.azurecr.io/mydockerimage:v1.0.0
```

Nach Erstellung der Web-App zeigt die Azure CLI eine Ausgabe wie im folgenden Beispiel an:

```json
{
  "availabilityState": "Normal",
  "clientAffinityEnabled": true,
  "clientCertEnabled": false,
  "cloningInfo": null,
  "containerSize": 0,
  "dailyMemoryTimeQuota": 0,
  "defaultHostName": "<app-name>.azurewebsites.net",
  "deploymentLocalGitUrl": "https://<username>@<app-name>.scm.azurewebsites.net/<app-name>.git",
  "enabled": true,
  < JSON data removed for brevity. >
}
```

### <a name="configure-registry-credentials-in-web-app"></a>Konfigurieren von Anmeldeinformationen für die Registrierung in der Web-App

Damit App Service das private Image per Pullvorgang abrufen kann, sind Informationen zu Ihrer Registrierung und zum Image erforderlich. Stellen Sie diese Informationen in der Cloud Shell mit dem Befehl [`az webapp config container set`](/cli/azure/webapp/config/container?view=azure-cli-latest#az-webapp-config-container-set) bereit. Ersetzen Sie *\<app-name>* , *\<azure-container-registry-name>* , _\<registry-username>_ und _\<password>_ .

```azurecli-interactive
az webapp config container set --name <app-name> --resource-group myResourceGroup --docker-custom-image-name <azure-container-registry-name>.azurecr.io/mydockerimage:v1.0.0 --docker-registry-server-url https://<azure-container-registry-name>.azurecr.io --docker-registry-server-user <registry-username> --docker-registry-server-password <password>
```

> [!NOTE]
> Bei Verwendung einer anderen Registrierung als Docker Hub muss `--docker-registry-server-url` das Format `https://` haben, gefolgt vom vollqualifizierten Domänennamen der Registrierung.
>

### <a name="configure-environment-variables"></a>Konfigurieren von Umgebungsvariablen

Für die meisten Docker-Images werden benutzerdefinierte Umgebungsvariablen verwendet, z. B. ein anderer Port als 80. Sie teilen App Service den von Ihrem Image verwendeten Port über die App-Einstellung `WEBSITES_PORT` mit. Die GitHub-Seite für das [Python-Beispiel in diesem Tutorial](https://github.com/Azure-Samples/docker-django-webapp-linux) zeigt an, dass Sie für `WEBSITES_PORT` den Wert _8000_ festlegen müssen.

Verwenden Sie zum Festlegen der App-Einstellungen den Befehl [`az webapp config appsettings set`](/cli/azure/webapp/config/appsettings?view=azure-cli-latest#az-webapp-config-appsettings-set) in Cloud Shell. Bei App-Einstellungen wird die Groß-/Kleinschreibung berücksichtigt und die Einstellungen sind durch Leerzeichen getrennt.

```azurecli-interactive
az webapp config appsettings set --resource-group myResourceGroup --name <app-name> --settings WEBSITES_PORT=8000
```

### <a name="test-the-web-app"></a>Testen der Web-App

Stellen Sie sicher, dass die Web-App funktioniert, indem Sie zu ihr navigieren (`http://<app-name>.azurewebsites.net`).

> [!NOTE]
> Beim ersten Zugreifen auf die App kann es einige Zeit dauern, da App Service das gesamte Image per Pullvorgang abrufen muss. Aktualisieren Sie einfach die Seite, wenn für den Browser eine Zeitüberschreitung eintritt.

![Testen der Konfiguration des Web-App-Ports](./media/app-service-linux-using-custom-docker-image/app-service-linux-browse-azure.png)

## <a name="change-web-app-and-redeploy"></a>Ändern der Web-App und erneutes Bereitstellen

Öffnen Sie „app/templates/app/index.html“ in Ihrem lokalen Git-Repository. Suchen Sie das erste HTML-Element und passen Sie es an.

```python
<nav class="navbar navbar-inverse navbar-fixed-top">
    <div class="container">
      <div class="navbar-header">
        <a class="navbar-brand" href="#">Azure App Service - Updated Here!</a>
      </div>
    </div>
  </nav>
```

Nachdem Sie die Python-Datei geändert und gespeichert haben, müssen Sie sie neu erstellen und das neue Docker-Image per Pushübertragung senden. Starten Sie dann die Web-App neu, damit die Änderungen wirksam werden. Verwenden Sie die gleichen Befehle, die Sie zuvor in diesem Tutorial verwendet haben. Weitere Informationen finden Sie unter [Erstellen des Images aus der Docker-Datei](#build-the-image-from-the-docker-file) und [Übertragen des Images zu Azure Container Registry mithilfe von Push](#push-image-to-azure-container-registry). Testen Sie die Web-App, indem Sie die Anweisungen in [Testen der Web-App](#test-the-web-app) befolgen.

## <a name="access-diagnostic-logs"></a>Zugreifen auf Diagnoseprotokolle

[!INCLUDE [Access diagnostic logs](../../../includes/app-service-web-logs-access-no-h.md)]

## <a name="enable-ssh-connections"></a>Aktivieren von SSH-Verbindungen

SSH ermöglicht die sichere Kommunikation zwischen einem Container und einem Client. Zum Aktivieren der SSH-Verbindung mit Ihrem Container muss Ihr benutzerdefiniertes Image dafür konfiguriert werden. Wir sehen uns nun das Beispielrepository an, das bereits die erforderliche Konfiguration enthält.

* In der [Dockerfile](https://github.com/Azure-Samples/docker-django-webapp-linux/blob/master/Dockerfile) wird mit dem folgenden Code der SSH-Server installiert, und außerdem werden die Anmeldeinformationen festgelegt.

    ```Dockerfile
    ENV SSH_PASSWD "root:Docker!"
    RUN apt-get update \
            && apt-get install -y --no-install-recommends dialog \
            && apt-get update \
      && apt-get install -y --no-install-recommends openssh-server \
      && echo "$SSH_PASSWD" | chpasswd 
    ```

    > [!NOTE]
    > Diese Konfiguration erlaubt keine externen Verbindungen zum Container. SSH ist nur über die Kudu/SCM-Website verfügbar. Die Authentifizierung für die Kudu/SCM-Website wird über Ihr Azure-Konto durchgeführt.

* Die [Dockerfile](https://github.com/Azure-Samples/docker-django-webapp-linux/blob/master/Dockerfile#L18) kopiert die Datei [sshd_config](https://github.com/Azure-Samples/docker-django-webapp-linux/blob/master/sshd_config) in das Repository in das Verzeichnis */etc/ssh/* .

    ```Dockerfile
    COPY sshd_config /etc/ssh/
    ```

* Die [Dockerfile](https://github.com/Azure-Samples/docker-django-webapp-linux/blob/master/Dockerfile#L22) macht Port 2222 im Container verfügbar. Es handelt sich um einen internen Port, auf den nur von Containern innerhalb des Brückennetzwerks eines privaten virtuellen Netzwerks zugegriffen werden kann. 

    ```Dockerfile
    EXPOSE 8000 2222
    ```

* Mit dem [Eingangsskript](https://github.com/Azure-Samples/docker-django-webapp-linux/blob/master/init.sh#L5) wird der SSH-Server gestartet.

      ```bash
      #!/bin/bash
      service ssh start
    ```

### Open SSH connection to container

SSH connection is available only through the Kudu site, which is accessible at `https://<app-name>.scm.azurewebsites.net`.

To connect, browse to `https://<app-name>.scm.azurewebsites.net/webssh/host` and sign in with your Azure account.

You are then redirected to a page displaying an interactive console.

You may wish to verify that certain applications are running in the container. To inspect the container and verify running processes, issue the `top` command at the prompt.

```bash
top
```

Der `top`-Befehl macht alle aktiven Prozesse in einem Container verfügbar.

```
PID USER      PR  NI    VIRT    RES    SHR S %CPU %MEM     TIME+ COMMAND
 1 root      20   0  945616  35372  15348 S  0.0  2.1   0:04.63 node
20 root      20   0   55180   2776   2516 S  0.0  0.2   0:00.00 sshd
42 root      20   0  944596  33340  15352 S  0.0  1.9   0:05.80 node /opt/s+
56 root      20   0   59812   5244   4512 S  0.0  0.3   0:00.93 sshd
58 root      20   0   20228   3128   2664 S  0.0  0.2   0:00.00 bash
62 root      20   0   21916   2272   1944 S  0.0  0.1   0:03.15 top
63 root      20   0   59812   5344   4612 S  0.0  0.3   0:00.03 sshd
65 root      20   0   20228   3140   2672 S  0.0  0.2   0:00.00 bash
71 root      20   0   59812   5380   4648 S  0.0  0.3   0:00.02 sshd
73 root      20   0   20228   3160   2696 S  0.0  0.2   0:00.00 bash
77 root      20   0   21920   2304   1972 R  0.0  0.1   0:00.00 top
```

Glückwunsch! Sie haben in App Service einen benutzerdefinierten Linux-Container konfiguriert.

[!INCLUDE [Clean-up section](../../../includes/cli-script-clean-up.md)]

## <a name="next-steps"></a>Nächste Schritte

Sie haben Folgendes gelernt:

> [!div class="checklist"]
> * Bereitstellen eines benutzerdefinierten Images in einer privaten Containerregistrierung
> * Ausführen des benutzerdefinierten Images in App Service
> * Konfigurieren von Umgebungsvariablen
> * Aktualisieren und erneutes Bereitstellen des Images
> * Zugreifen auf Diagnoseprotokolle
> * Herstellen einer Verbindung mit dem Container per SSH

Fahren Sie mit dem nächsten Tutorial fort, um zu erfahren, wie Sie Ihrer App einen benutzerdefinierten DNS-Namen zuordnen.

> [!div class="nextstepaction"]
> [Tutorial: Zuordnen eines benutzerdefinierten DNS-Namens zu Ihrer App](../app-service-web-tutorial-custom-domain.md)

Oder sehen Sie sich weitere Ressourcen an:

> [!div class="nextstepaction"]
> [Konfigurieren eines benutzerdefinierten Containers](configure-custom-container.md)

> [!div class="nextstepaction"]
> [Tutorial: WordPress-App mit mehreren Containern](tutorial-multi-container-app.md)
