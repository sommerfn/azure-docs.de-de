---
title: Tutorial – Vorbereiten einer Containerregistrierung für Azure Container Instances
description: Tutorial für Azure Container Instances (Teil 2 von 3) – Vorbereiten einer Azure-Containerregistrierung und Übertragen eines Images per Pushvorgang
services: container-instances
author: dlepow
manager: gwallace
ms.service: container-instances
ms.topic: tutorial
ms.date: 03/21/2018
ms.author: danlep
ms.custom: seodec18, mvc
ms.openlocfilehash: b3c907eacb14ed65410a60fcf22ebe99fd8cc3bb
ms.sourcegitcommit: 4b431e86e47b6feb8ac6b61487f910c17a55d121
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 07/18/2019
ms.locfileid: "68325603"
---
# <a name="tutorial-deploy-an-azure-container-registry-and-push-a-container-image"></a>Tutorial: Bereitstellen einer Azure-Containerregistrierung und Übertragen eines Containerimages per Pushvorgang

Dies ist der zweite Teil eines dreiteiligen Tutorials. In [Teil 1](container-instances-tutorial-prepare-app.md) des Tutorials wurde ein Docker-Containerimage für eine Node.js-Webanwendung erstellt. In diesem Tutorial übertragen Sie das Image per Pushvorgang an die Azure Container Registry-Instanz. Kehren Sie zu [Tutorial 1 – Erstellen von Containerimages](container-instances-tutorial-prepare-app.md) zurück, falls Sie das Containerimage noch nicht erstellt haben.

Azure Container Registry ist Ihre private Docker-Registrierung in Azure. In diesem Tutorial erstellen Sie eine Azure Container Registry-Instanz in Ihrem Abonnement und übertragen anschließend das zuvor erstellte Containerimage per Pushvorgang in die Instanz. Dieser Artikel ist der zweite Teil der Reihe und umfasst Folgendes:

> [!div class="checklist"]
> * Erstellen einer Azure Container Registry-Instanz
> * Markieren eines Containerimages für Ihre Azure Container Registry-Instanz
> * Hochladen des Images in Ihre Registrierung

Im nächsten Artikel (dem letzten Tutorial der Reihe) stellen Sie den Container aus Ihrer privaten Registrierung für Azure Container Instances bereit.

## <a name="before-you-begin"></a>Voraussetzungen

[!INCLUDE [container-instances-tutorial-prerequisites](../../includes/container-instances-tutorial-prerequisites.md)]

## <a name="create-azure-container-registry"></a>Erstellen einer Azure-Containerregistrierung

Bevor Sie Ihre Containerregistrierung erstellen können, benötigen Sie eine *Ressourcengruppe* für die Bereitstellung. Eine Azure-Ressourcengruppe ist eine logische Sammlung, in der alle Azure-Ressourcen bereitgestellt und verwaltet werden.

Erstellen Sie mit dem Befehl [az group create][az-group-create] eine Ressourcengruppe. Im folgenden Beispiel wird eine Ressourcengruppe mit dem Namen *myResourceGroup* in der Region *eastus* erstellt:

```azurecli
az group create --name myResourceGroup --location eastus
```

Erstellen Sie nach der Erstellung der Ressourcengruppe mit dem Befehl [az acr create][az-acr-create] eine Azure-Containerregistrierung. Der Containerregistrierungsname muss innerhalb von Azure eindeutig sein und zwischen 5 und 50 alphanumerische Zeichen enthalten. Ersetzen Sie `<acrName>` durch einen eindeutigen Namen für die Registrierung:

```azurecli
az acr create --resource-group myResourceGroup --name <acrName> --sku Basic --admin-enabled true
```

Hier ist eine Beispielausgabe für eine neue Azure-Containerregistrierung mit dem Namen *mycontainerregistry082* angegeben (in gekürzter Form):

```console
$ az acr create --resource-group myResourceGroup --name mycontainerregistry082 --sku Basic --admin-enabled true
...
{
  "adminUserEnabled": true,
  "creationDate": "2018-03-16T21:54:47.297875+00:00",
  "id": "/subscriptions/<Subscription ID>/resourceGroups/myResourceGroup/providers/Microsoft.ContainerRegistry/registries/mycontainerregistry082",
  "location": "eastus",
  "loginServer": "mycontainerregistry082.azurecr.io",
  "name": "mycontainerregistry082",
  "provisioningState": "Succeeded",
  "resourceGroup": "myResourceGroup",
  "sku": {
    "name": "Basic",
    "tier": "Basic"
  },
  "status": null,
  "storageAccount": null,
  "tags": {},
  "type": "Microsoft.ContainerRegistry/registries"
}
```

Im restlichen Teil dieses Tutorials wird `<acrName>` als Platzhalter für den von Ihnen in diesem Schritt gewählten Namen der Containerregistrierung bezeichnet.

## <a name="log-in-to-container-registry"></a>Anmelden an der Containerregistrierung

Sie müssen sich bei der Azure-Containerregistrierung anmelden, damit Sie Images mithilfe von Push an sie übertragen können. Verwenden Sie den Befehl [az acr login][az-acr-login], um den Vorgang abzuschließen. Geben Sie den eindeutigen Namen an, den Sie beim Erstellen der Containerregistrierung gewählt haben.

```azurecli
az acr login --name <acrName>
```

Der Befehl gibt nach Abschluss des Vorgangs `Login Succeeded` zurück:

```console
$ az acr login --name mycontainerregistry082
Login Succeeded
```

## <a name="tag-container-image"></a>Markieren von Containerimages

Zum Übertragen eines Containerimages an eine private Registrierung wie Azure Container Registry müssen Sie das Image zuerst mit dem vollständigen Namen des Anmeldeservers für die Registrierung versehen.

Rufen Sie zuerst den vollständigen Namen des Anmeldeservers für Ihre Containerregistrierung ab. Führen Sie den folgenden Befehl [az acr show][az-acr-show] aus, und ersetzen Sie `<acrName>` durch den Namen der gerade erstellten Registrierung:

```azurecli
az acr show --name <acrName> --query loginServer --output table
```

Dies sieht beispielsweise wie folgt aus, wenn Ihre Registrierung den Namen *mycontainerregistry082* hat:

```console
$ az acr show --name mycontainerregistry082 --query loginServer --output table
Result
------------------------
mycontainerregistry082.azurecr.io
```

Zeigen Sie die Liste mit Ihren lokalen Images mit dem Befehl [docker images][docker-images] an:

```bash
docker images
```

Neben den anderen Images, die auf Ihrem Computer vorhanden sind, sollte das Image *aci-tutorial-app* angezeigt werden, das Sie im [vorherigen Tutorial](container-instances-tutorial-prepare-app.md) erstellt haben:

```console
$ docker images
REPOSITORY          TAG       IMAGE ID        CREATED           SIZE
aci-tutorial-app    latest    5c745774dfa9    39 minutes ago    68.1 MB
```

Kennzeichnen Sie das Image *aci-tutorial-app* mit dem loginServer-Namen der Containerregistrierung. Fügen Sie außerdem das Tag `:v1` am Ende des Imagenamens hinzu, um die Versionsnummer für das Image anzugeben. Ersetzen Sie `<acrLoginServer>` durch das Ergebnis des zuvor ausgeführten Befehls [az acr show][az-acr-show].

```bash
docker tag aci-tutorial-app <acrLoginServer>/aci-tutorial-app:v1
```

Führen Sie `docker images` erneut aus, um zu überprüfen, ob die Kennzeichnung erfolgreich war:

```console
$ docker images
REPOSITORY                                            TAG       IMAGE ID        CREATED           SIZE
aci-tutorial-app                                      latest    5c745774dfa9    39 minutes ago    68.1 MB
mycontainerregistry082.azurecr.io/aci-tutorial-app    v1        5c745774dfa9    7 minutes ago     68.1 MB
```

## <a name="push-image-to-azure-container-registry"></a>Übertragen des Images zu Azure Container Registry mithilfe von Push

Nachdem Sie das Image *aci-tutorial-app* jetzt mit dem vollständigen Namen des Anmeldeservers Ihrer privaten Registrierung versehen haben, können Sie es mit dem Befehl [docker push][docker-push] per Pushvorgang an die Registrierung übertragen. Ersetzen Sie `<acrLoginServer>` durch den vollständigen Anmeldeservernamen, den Sie im vorherigen Schritt abgerufen haben.

```bash
docker push <acrLoginServer>/aci-tutorial-app:v1
```

Der `push`-Vorgang dauert abhängig von der Internetverbindung zwischen einigen Sekunden und wenigen Minuten. Die Ausgabe sieht ungefähr wie folgt aus:

```console
$ docker push mycontainerregistry082.azurecr.io/aci-tutorial-app:v1
The push refers to a repository [mycontainerregistry082.azurecr.io/aci-tutorial-app]
3db9cac20d49: Pushed
13f653351004: Pushed
4cd158165f4d: Pushed
d8fbd47558a8: Pushed
44ab46125c35: Pushed
5bef08742407: Pushed
v1: digest: sha256:ed67fff971da47175856505585dcd92d1270c3b37543e8afd46014d328f05715 size: 1576
```

## <a name="list-images-in-azure-container-registry"></a>Auflisten der Images in Azure Container Registry

Um sicherzustellen, dass sich das gerade übertragene Image wirklich in Ihrer Azure-Containerregistrierung befindet, können Sie die in der Registrierung enthaltenen Images mit dem Befehl [az acr repository list][az-acr-repository-list] auflisten. Ersetzen Sie `<acrName>` durch den Namen Ihrer Containerregistrierung.

```azurecli
az acr repository list --name <acrName> --output table
```

Beispiel:

```console
$ az acr repository list --name mycontainerregistry082 --output table
Result
----------------
aci-tutorial-app
```

Verwenden Sie den Befehl [az acr repository show-tags][az-acr-repository-show-tags], um die *Tags* für ein bestimmtes Image anzuzeigen.

```azurecli
az acr repository show-tags --name <acrName> --repository aci-tutorial-app --output table
```

Eine Ausgabe ähnlich der folgenden sollte angezeigt werden:

```console
$ az acr repository show-tags --name mycontainerregistry082 --repository aci-tutorial-app --output table
Result
--------
v1
```

## <a name="next-steps"></a>Nächste Schritte

In diesem Tutorial haben Sie eine Azure-Containerregistrierung für die Verwendung mit Azure Container Instances vorbereitet und ein Containerimage mithilfe von Push an die Registrierung übertragen. Die folgenden Schritte wurden durchgeführt:

> [!div class="checklist"]
> * Bereitstellen einer Azure Container Registry-Instanz
> * Markieren eines Containerimages für Azure Container Registry
> * Hochladen eines Images in Azure Container Registry

Fahren Sie mit dem nächsten Tutorial fort, um zu erfahren, wie Sie den Container in Azure mithilfe von Azure Container Instances bereitstellen:

> [!div class="nextstepaction"]
> [Bereitstellen eines Containers in Azure Container Instances](container-instances-tutorial-deploy-app.md)

<!-- LINKS - External -->
[docker-build]: https://docs.docker.com/engine/reference/commandline/build/
[docker-get-started]: https://docs.docker.com/get-started/
[docker-hub-nodeimage]: https://store.docker.com/images/node
[docker-images]: https://docs.docker.com/engine/reference/commandline/images/
[docker-linux]: https://docs.docker.com/engine/installation/#supported-platforms
[docker-login]: https://docs.docker.com/engine/reference/commandline/login/
[docker-mac]: https://docs.docker.com/docker-for-mac/
[docker-push]: https://docs.docker.com/engine/reference/commandline/push/
[docker-tag]: https://docs.docker.com/engine/reference/commandline/tag/
[docker-windows]: https://docs.docker.com/docker-for-windows/
[nodejs]: https://nodejs.org

<!-- LINKS - Internal -->
[az-acr-create]: /cli/azure/acr#az-acr-create
[az-acr-login]: /cli/azure/acr#az-acr-login
[az-acr-repository-list]: /cli/azure/acr/repository
[az-acr-repository-show-tags]: /cli/azure/acr/repository#az-acr-repository-show-tags
[az-acr-show]: /cli/azure/acr#az-acr-show
[az-group-create]: /cli/azure/group#az-group-create
[azure-cli-install]: /cli/azure/install-azure-cli
