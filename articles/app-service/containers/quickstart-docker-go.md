---
title: Erstellen von Docker/Go-Apps unter Linux – Azure App Service
description: Es wird beschrieben, wie Sie ein Docker-Image zum Ausführen einer Go-Anwendung für die Web-App für Container bereitstellen.
keywords: Azure App Service, Web-App, Go, Docker, Container
services: app-service
author: msangapu
manager: jeconnoc
ms.assetid: b97bd4e6-dff0-4976-ac20-d5c109a559a8
ms.service: app-service
ms.devlang: go
ms.topic: quickstart
ms.date: 03/28/2019
ms.author: msangapu
ms.custom: seodec18
ms.openlocfilehash: ec2b974e008ea4c7e266f5ae0d46cd67d2133e54
ms.sourcegitcommit: 470041c681719df2d4ee9b81c9be6104befffcea
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 07/12/2019
ms.locfileid: "67854003"
---
# <a name="run-a-custom-linux-container-in-azure-app-service"></a>Ausführen einen benutzerdefinierten Linux-Containers in Azure App Service

Bei [App Service Linux](app-service-linux-intro.md) werden vordefinierte Anwendungsstapel unter Linux mit Unterstützung für verschiedene Sprachen bereitgestellt, z.B. .NET, PHP, Node.js und andere. Sie können auch ein benutzerdefiniertes Docker-Image verwenden, um Ihre Web-App in einem Anwendungsstapel auszuführen, der nicht bereits in Azure definiert ist. In diesem Schnellstart wird erläutert, wie Sie eine Web-App erstellen und ein Go-Image per Docker Hub bereitstellen. Sie erstellen die Web-App mithilfe der [Azure CLI](https://docs.microsoft.com/cli/azure/get-started-with-azure-cli).

![In Azure ausgeführte Beispiel-App](media/quickstart-docker-go/hello-world-in-browser.png)

[!INCLUDE [quickstarts-free-trial-note](../../../includes/quickstarts-free-trial-note.md)]

[!INCLUDE [cloud-shell-try-it.md](../../../includes/cloud-shell-try-it.md)]

[!INCLUDE [Create resource group](../../../includes/app-service-web-create-resource-group-linux.md)]

[!INCLUDE [Create app service plan](../../../includes/app-service-web-create-app-service-plan-linux.md)]

## <a name="create-a-web-app"></a>Erstellen einer Web-App

Erstellen Sie eine [Web-App](../overview.md) im App Service-Plan `myAppServicePlan` mit dem Befehl [az webapp create](/cli/azure/webapp?view=azure-cli-latest#az-webapp-create). Vergessen Sie nicht, `<app name>` durch einen global eindeutigen App-Namen zu ersetzen (gültige Zeichen sind `a-z`, `0-9` und `-`).

```azurecli-interactive
az webapp create --resource-group myResourceGroup --plan myAppServicePlan --name <app name> --deployment-container-image-name microsoft/azure-appservices-go-quickstart
```

Im obigen Befehl zeigt `--deployment-container-image-name` auf das öffentliche Docker Hub-Image [microsoft/azure-appservices-go-quickstart](https://hub.docker.com/r/microsoft/azure-appservices-go-quickstart/).

Nach Erstellung der Web-App zeigt die Azure CLI eine Ausgabe wie im folgenden Beispiel an:

```json
{
  "availabilityState": "Normal",
  "clientAffinityEnabled": true,
  "clientCertEnabled": false,
  "cloningInfo": null,
  "containerSize": 0,
  "dailyMemoryTimeQuota": 0,
  "defaultHostName": "<app name>.azurewebsites.net",
  "deploymentLocalGitUrl": "https://<username>@<app name>.scm.azurewebsites.net/<app name>.git",
  "enabled": true,
  < JSON data removed for brevity. >
}
```

## <a name="browse-to-the-app"></a>Navigieren zur App

```bash
http://<app_name>.azurewebsites.net/hello
```

![In Azure ausgeführte Beispiel-App](media/quickstart-docker-go/hello-world-in-browser.png)

**Glückwunsch!** Sie haben ein benutzerdefiniertes Docker-Image zur Ausführung einer Go-Anwendung für Web-App für Container bereitgestellt.

[!INCLUDE [Clean-up section](../../../includes/cli-script-clean-up.md)]

## <a name="next-steps"></a>Nächste Schritte

> [!div class="nextstepaction"]
> [Tutorial: Bereitstellen aus privatem Containerrepository](tutorial-custom-docker-image.md)

> [!div class="nextstepaction"]
> [Konfigurieren eines benutzerdefinierten Containers](configure-custom-container.md)

> [!div class="nextstepaction"]
> [Tutorial: WordPress-App mit mehreren Containern](tutorial-multi-container-app.md)
