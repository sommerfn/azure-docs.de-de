---
title: 'Tutorial: Bereitstellen einer Container-App in Azure Container Instances'
description: Tutorial für Azure Container Instances (Teil 3 von 3) – Bereitstellen einer Containeranwendung in Azure Container Instances
services: container-instances
author: dlepow
manager: gwallace
ms.service: container-instances
ms.topic: tutorial
ms.date: 03/21/2018
ms.author: danlep
ms.custom: seodec18, mvc
ms.openlocfilehash: f2890948dd15fa972104e4ef11001e83a2abd4f8
ms.sourcegitcommit: 16c5374d7bcb086e417802b72d9383f8e65b24a7
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 11/08/2019
ms.locfileid: "73846594"
---
# <a name="tutorial-deploy-a-container-application-to-azure-container-instances"></a>Tutorial: Bereitstellen einer Containeranwendung in Azure Container Instances

Dies ist das letzte Tutorial einer dreiteiligen Reihe. In den vorhergehenden Teilen der Reihe wurde [ein Containerimage erstellt](container-instances-tutorial-prepare-app.md) und [mithilfe von Push an eine Azure Container Registry-Instanz übermittelt](container-instances-tutorial-prepare-acr.md). Zum Abschluss der Reihe wird der Container in diesem Tutorial für Azure Container Instances bereitgestellt.

In diesem Tutorial führen Sie Folgendes durch:

> [!div class="checklist"]
> * Bereitstellen des Containers aus Azure Container Registry in Azure Container Instances
> * Anzeigen der ausgeführten Anwendung im Browser
> * Anzeigen der Protokolle des Containers

## <a name="before-you-begin"></a>Voraussetzungen

[!INCLUDE [container-instances-tutorial-prerequisites](../../includes/container-instances-tutorial-prerequisites.md)]

## <a name="deploy-the-container-using-the-azure-cli"></a>Bereitstellen des Containers mithilfe der Azure-Befehlszeilenschnittstelle

In diesem Abschnitt verwenden Sie die Azure CLI zum Bereitstellen des Images, das im [ersten Tutorial](container-instances-tutorial-prepare-app.md) erstellt und im [zweiten Tutorial](container-instances-tutorial-prepare-acr.md) per Pushvorgang in die Azure Container Registry übertragen wurde. Arbeiten Sie diese Tutorials durch, bevor Sie fortfahren.

### <a name="get-registry-credentials"></a>Abrufen von Registrierungsanmeldeinformationen

Wenn Sie ein Image bereitstellen, das in einer privaten Azure-Containerregistrierung gehostet wird (wie im [zweiten Tutorial](container-instances-tutorial-prepare-acr.md)), müssen Sie Anmeldeinformationen für den Zugriff auf die Registrierung angeben. 

Eine bewährte Methode für viele Szenarien ist das Erstellen und Konfigurieren eines Azure Active Directory-Dienstprinzipals mit *Pull*-Berechtigungen für Ihre Registrierung. Beispielskripts zum Erstellen eines Dienstprinzipals mit den erforderlichen Berechtigungen finden Sie unter [Authentifizieren per Azure Container Registry über Azure Container Instances](../container-registry/container-registry-auth-aci.md). Notieren Sie sich die *Dienstprinzipal-ID* und das zugehörige *Kennwort*. Sie nutzen diese Anmeldeinformationen zum Zugreifen auf die Registrierung, wenn Sie den Container bereitstellen.

Darüber hinaus benötigen Sie den vollständigen Namen des Anmeldeservers für die Containerregistrierung (ersetzen Sie `<acrName>` durch den Namen Ihrer Registrierung):

```azurecli
az acr show --name <acrName> --query loginServer
```

### <a name="deploy-container"></a>Bereitstellen des Containers

Verwenden Sie nun den Befehl [az container create][az-container-create], um den Container bereitzustellen. Ersetzen Sie `<acrLoginServer>` durch den Wert, den Sie mit dem obigen Befehl abgerufen haben. Ersetzen Sie `<service-principal-ID>` und `<service-principal-password>` durch die Dienstprinzipal-ID und das Kennwort, die Sie für den Zugriff auf die Registrierung erstellt haben. Ersetzen Sie `<aciDnsLabel>` durch einen gewünschten DNS-Namen.

```azurecli
az container create --resource-group myResourceGroup --name aci-tutorial-app --image <acrLoginServer>/aci-tutorial-app:v1 --cpu 1 --memory 1 --registry-login-server <acrLoginServer> --registry-username <service-principal-ID> --registry-password <service-principal-password> --dns-name-label <aciDnsLabel> --ports 80
```

Innerhalb weniger Sekunden sollten Sie eine erste Antwort von Azure erhalten. Der Wert `--dns-name-label` muss innerhalb der Azure-Region, in der Sie die Containerinstanz erstellen, eindeutig sein. Ändern Sie den Wert im vorherigen Befehl, wenn Sie beim Ausführen des Befehls eine Fehlermeldung bezüglich der **DNS-Namensbezeichnung** erhalten.

### <a name="verify-deployment-progress"></a>Überprüfen des Bereitstellungsstatus

Verwenden Sie zum Anzeigen des Status der Bereitstellung [az container show][az-container-show]:

```azurecli
az container show --resource-group myResourceGroup --name aci-tutorial-app --query instanceView.state
```

Führen Sie den Befehl [az container show][az-container-show] erneut aus, bis der Status nicht mehr *Ausstehend*, sondern *Wird ausgeführt* lautet. Das sollte nach maximal einer Minute der Fall sein. Wenn für den Container *Wird ausgeführt* angezeigt wird, fahren Sie mit dem nächsten Schritt fort.

## <a name="view-the-application-and-container-logs"></a>Anzeigen der Anwendungs- und Containerprotokolle

Wenn die Bereitstellung erfolgreich war, zeigen Sie mit dem Befehl [az container show][az-container-show] den vollqualifizierten Domänennamen (Fully Qualified Domain Name, FQDN) des Containers an:

```bash
az container show --resource-group myResourceGroup --name aci-tutorial-app --query ipAddress.fqdn
```

Beispiel:
```console
$ az container show --resource-group myResourceGroup --name aci-tutorial-app --query ipAddress.fqdn
"aci-demo.eastus.azurecontainer.io"
```

Navigieren Sie zum Anzeigen der ausgeführten Anwendung in Ihrem bevorzugten Browser zum angezeigten DNS-Namen:

![„Hello World“-Anwendung im Browser][aci-app-browser]

Sie können auch die Protokollausgabe des Containers anzeigen:

```azurecli
az container logs --resource-group myResourceGroup --name aci-tutorial-app
```

Beispielausgabe:

```bash
$ az container logs --resource-group myResourceGroup --name aci-tutorial-app
listening on port 80
::ffff:10.240.0.4 - - [21/Jul/2017:06:00:02 +0000] "GET / HTTP/1.1" 200 1663 "-" "Mozilla/5.0 (Macintosh; Intel Mac OS X 10_12_5) AppleWebKit/537.36 (KHTML, like Gecko) Chrome/59.0.3071.115 Safari/537.36"
::ffff:10.240.0.4 - - [21/Jul/2017:06:00:02 +0000] "GET /favicon.ico HTTP/1.1" 404 150 "http://aci-demo.eastus.azurecontainer.io/" "Mozilla/5.0 (Macintosh; Intel Mac OS X 10_12_5) AppleWebKit/537.36 (KHTML, like Gecko) Chrome/59.0.3071.115 Safari/537.36"
```

## <a name="clean-up-resources"></a>Bereinigen von Ressourcen

Wenn Sie in dieser Tutorialreihe erstellte Ressourcen nicht mehr benötigen, können Sie den Befehl [az group delete][az-group-delete] ausführen, um die Ressourcengruppe und alle darin enthaltenen Ressourcen zu entfernen. Dieser Befehl löscht die von Ihnen erstellte Containerregistrierung sowie den ausgeführten Container und alle zugehörigen Ressourcen.

```azurecli-interactive
az group delete --name myResourceGroup
```

## <a name="next-steps"></a>Nächste Schritte

In diesem Tutorial haben Sie Ihren Container in Azure Container Instances bereitgestellt. Die folgenden Schritte wurden durchgeführt:

> [!div class="checklist"]
> * Bereitstellen des Containers aus der Azure Container Registry-Instanz mithilfe der Azure-Befehlszeilenschnittstelle
> * Anzeigen der Anwendung im Browser
> * Anzeigen der Containerprotokolle

Nachdem Sie sich mit den Grundlagen vertraut gemacht haben, können Sie sich jetzt eingehender über Azure Container Instances informieren, z.B. über die Funktionsweise von Containergruppen:

> [!div class="nextstepaction"]
> [Containergruppen in Azure Container Instances](container-instances-container-groups.md)

<!-- IMAGES -->
[aci-app-browser]: ./media/container-instances-quickstart/aci-app-browser.png

<!-- LINKS - external -->
[docker-linux]: https://docs.docker.com/engine/installation/#supported-platforms
[docker-login]: https://docs.docker.com/engine/reference/commandline/login/
[docker-mac]: https://docs.docker.com/docker-for-mac/
[docker-push]: https://docs.docker.com/engine/reference/commandline/push/
[docker-tag]: https://docs.docker.com/engine/reference/commandline/tag/
[docker-windows]: https://docs.docker.com/docker-for-windows/

<!-- LINKS - internal -->
[az-container-create]: /cli/azure/container#az-container-create
[az-container-show]: /cli/azure/container#az-container-show
[az-group-delete]: /cli/azure/group#az-group-delete
[azure-cli-install]: /cli/azure/install-azure-cli
[prepare-app]: ./container-instances-tutorial-prepare-app.md
