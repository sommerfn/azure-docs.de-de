---
title: 'Schnellstart: Erstellen und Ausführen eines Containerimages in Azure Container Registry'
description: Führen Sie mit Azure Container Registry schnell Aufgaben aus, um bei Bedarf ein Containerimage in der Cloud zu erstellen und auszuführen.
services: container-registry
author: dlepow
manager: gwallace
ms.service: container-registry
ms.topic: quickstart
ms.date: 04/02/2019
ms.author: danlep
ms.custom: ''
ms.openlocfilehash: b97249aa61916975fa641d4620179be33e1d5276
ms.sourcegitcommit: a10074461cf112a00fec7e14ba700435173cd3ef
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 11/12/2019
ms.locfileid: "73931553"
---
# <a name="quickstart-build-and-run-a-container-image-using-azure-container-registry-tasks"></a>Schnellstart: Erstellen und Ausführen eines Containerimages mithilfe von Azure Container Registry Tasks

In dieser Schnellstartanleitung verwenden Sie Befehle von Azure Container Registry Tasks, um ein Docker-Containerimage schnell zu erstellen, mithilfe von Push zu übertragen und nativ in Azure auszuführen. Dabei lernen Sie, wie Sie Ihren Inner Loop-Entwicklungszyklus in die Cloud auslagern. [ACR Tasks][container-registry-tasks-overview] ist eine Suite mit Features in Azure Container Registry, die Sie beim Verwalten und Ändern von Containerimages während des gesamten Containerlebenszyklus unterstützt. 

Im Anschluss an diese Schnellstartanleitung können Sie sich mit komplexeren Features von ACR Tasks beschäftigen. ACR Tasks ermöglicht unter anderem die Automatisierung von Imagebuildvorgängen auf der Grundlage von Codecommits oder Basisimageaktualisierungen sowie das parallele Testen mehrerer Container. 

Wenn Sie kein Azure-Abonnement besitzen, können Sie ein [kostenloses Konto][azure-account] erstellen, bevor Sie beginnen.

[!INCLUDE [cloud-shell-try-it.md](../../includes/cloud-shell-try-it.md)]

Sie können Azure Cloud Shell oder eine lokale Installation der Azure CLI für diesen Schnellstart verwenden. Für die lokale Verwendung wird mindestens die Version 2.0.58 empfohlen. Führen Sie `az --version` aus, um die Version zu finden. Informationen zum Durchführen einer Installation oder eines Upgrades finden Sei bei Bedarf unter [Installieren der Azure CLI][azure-cli-install].

## <a name="create-a-resource-group"></a>Erstellen einer Ressourcengruppe

Sollten Sie noch nicht über eine Containerregistrierung verfügen, erstellen Sie mithilfe des Befehls [az group create][az-group-create] zunächst eine Ressourcengruppe. Eine Azure-Ressourcengruppe ist ein logischer Container, in dem Azure-Ressourcen bereitgestellt und verwaltet werden.

Das folgende Beispiel erstellt eine Ressourcengruppe mit dem Namen *myResourceGroup* am Standort *eastus*.

```azurecli-interactive
az group create --name myResourceGroup --location eastus
```

## <a name="create-a-container-registry"></a>Erstellen einer Containerregistrierung

Erstellen Sie mithilfe des Befehls [az acr create][az-acr-create] eine Containerregistrierung. Der Registrierungsname muss innerhalb von Azure eindeutig sein und zwischen 5 und 50 alphanumerische Zeichen enthalten. Im folgenden Beispiel wird der Name *myContainerRegistry008* verwendet. Ersetzen Sie diesen Namen durch einen eindeutigen Wert.

```azurecli-interactive
az acr create --resource-group myResourceGroup --name myContainerRegistry008 --sku Basic
```

In diesem Beispiel wird eine Registrierung vom Typ *Basic* erstellt. Dabei handelt es sich um eine kostenoptimierte Option für Entwickler, die sich mit Azure Container Registry vertraut machen. Ausführliche Informationen zu verfügbaren Dienstebenen finden Sie unter [Azure Container Registry-SKUs][container-registry-skus].

## <a name="build-an-image-from-a-dockerfile"></a>Erstellen eines Images auf der Grundlage eines Dockerfile

Erstellen Sie nun mithilfe von Azure Container Registry ein Image. Erstellen Sie zunächst ein Arbeitsverzeichnis und anschließend ein Dockerfile namens *Dockerfile* mit folgendem Inhalt. Hierbei handelt es sich um ein einfaches Beispiel für die Erstellung eines Linux-Containerimages. Sie können jedoch Ihr eigenes Standard-Dockerfile sowie Images für andere Plattformen erstellen.

```bash
echo FROM hello-world > Dockerfile
```

Führen Sie den Befehl [az acr build][az-acr-build] aus, um das Image zu erstellen. Nach erfolgreicher Erstellung wird das Image mithilfe von Push an Ihre Registrierung übertragen. Im folgenden Beispiel wird das Image `sample/hello-world:v1` mithilfe von Push übertragen. Der Punkt (`.`) am Ende des Befehls legt den Speicherort des Dockerfile fest (in diesem Fall: das aktuelle Verzeichnis).

```azurecli-interactive
az acr build --image sample/hello-world:v1 --registry myContainerRegistry008 --file Dockerfile . 
```

Die Ausgabe nach erfolgreicher Erstellung und Pushübertragung sieht in etwa wie folgt aus:

```console
Packing source code into tar to upload...
Uploading archived source code from '/tmp/build_archive_b0bc1e5d361b44f0833xxxx41b78c24e.tar.gz'...
Sending context (1.856 KiB) to registry: mycontainerregistry008...
Queued a build with ID: ca8
Waiting for agent...
2019/03/18 21:56:57 Using acb_vol_4c7ffa31-c862-4be3-xxxx-ab8e615c55c4 as the home volume
2019/03/18 21:56:57 Setting up Docker configuration...
2019/03/18 21:56:58 Successfully set up Docker configuration
2019/03/18 21:56:58 Logging in to registry: mycontainerregistry008.azurecr.io
2019/03/18 21:56:59 Successfully logged into mycontainerregistry008.azurecr.io
2019/03/18 21:56:59 Executing step ID: build. Working directory: '', Network: ''
2019/03/18 21:56:59 Obtaining source code and scanning for dependencies...
2019/03/18 21:57:00 Successfully obtained source code and scanned for dependencies
2019/03/18 21:57:00 Launching container with name: build
Sending build context to Docker daemon  13.82kB
Step 1/1 : FROM hello-world
latest: Pulling from library/hello-world
Digest: sha256:2557e3c07ed1e38f26e389462d03ed943586fxxxx21577a99efb77324b0fe535
Successfully built fce289e99eb9
Successfully tagged mycontainerregistry008.azurecr.io/sample/hello-world:v1
2019/03/18 21:57:01 Successfully executed container: build
2019/03/18 21:57:01 Executing step ID: push. Working directory: '', Network: ''
2019/03/18 21:57:01 Pushing image: mycontainerregistry008.azurecr.io/sample/hello-world:v1, attempt 1
The push refers to repository [mycontainerregistry008.azurecr.io/sample/hello-world]
af0b15c8625b: Preparing
af0b15c8625b: Layer already exists
v1: digest: sha256:92c7f9c92844bbbb5d0a101b22f7c2a7949e40f8ea90c8b3bc396879d95e899a size: 524
2019/03/18 21:57:03 Successfully pushed image: mycontainerregistry008.azurecr.io/sample/hello-world:v1
2019/03/18 21:57:03 Step ID: build marked as successful (elapsed time in seconds: 2.543040)
2019/03/18 21:57:03 Populating digests for step ID: build...
2019/03/18 21:57:05 Successfully populated digests for step ID: build
2019/03/18 21:57:05 Step ID: push marked as successful (elapsed time in seconds: 1.473581)
2019/03/18 21:57:05 The following dependencies were found:
2019/03/18 21:57:05
- image:
    registry: mycontainerregistry008.azurecr.io
    repository: sample/hello-world
    tag: v1
    digest: sha256:92c7f9c92844bbbb5d0a101b22f7c2a7949e40f8ea90c8b3bc396879d95e899a
  runtime-dependency:
    registry: registry.hub.docker.com
    repository: library/hello-world
    tag: v1
    digest: sha256:2557e3c07ed1e38f26e389462d03ed943586f744621577a99efb77324b0fe535
  git: {}

Run ID: ca8 was successful after 10s
```

## <a name="run-the-image"></a>Ausführen des Images

Führen Sie nun schnell das Image aus, das Sie erstellt und mithilfe von Push an Ihre Registrierung übertragen haben. In Ihrem Workflow für die Containerentwicklung kann dieser Schritt zur Überprüfung vor der Imagebereitstellung genutzt werden.

Erstellen Sie in einem lokalen Arbeitsverzeichnis eine Datei namens *quickrun.yaml* mit dem folgenden Inhalt für einen einzelnen Schritt. Ersetzen Sie den Namen des Anmeldeservers Ihrer Registrierung durch *\<acrLoginServer\>* . Der Name des Anmeldeservers wird im Format *\<Registrierungsname\>.azurecr.io* (nur Kleinbuchstaben) angegeben (Beispiel: *mycontainerregistry008.azurecr.io*). In diesem Beispiel wird davon ausgegangen, dass Sie im vorherigen Abschnitt das Image `sample/hello-world:v1` erstellt und mithilfe von Push übertragen haben:

```yml
steps:
  - cmd: <acrLoginServer>/sample/hello-world:v1
```

Im Schritt `cmd` dieses Beispiels wird der Container in seiner Standardkonfiguration ausgeführt. `cmd` unterstützt jedoch zusätzliche Parameter vom Typ `docker run` und sogar andere Befehle vom Typ `docker`.

Führen Sie den Container mithilfe des folgenden Befehls aus:

```azurecli-interactive
az acr run --registry myContainerRegistry008 --file quickrun.yaml .
```

Die Ausgabe sieht in etwa wie folgt aus:

```console
Packing source code into tar to upload...
Uploading archived source code from '/tmp/run_archive_ebf74da7fcb04683867b129e2ccad5e1.tar.gz'...
Sending context (1.855 KiB) to registry: mycontainerre...
Queued a run with ID: cab
Waiting for an agent...
2019/03/19 19:01:53 Using acb_vol_60e9a538-b466-475f-9565-80c5b93eaa15 as the home volume
2019/03/19 19:01:53 Creating Docker network: acb_default_network, driver: 'bridge'
2019/03/19 19:01:53 Successfully set up Docker network: acb_default_network
2019/03/19 19:01:53 Setting up Docker configuration...
2019/03/19 19:01:54 Successfully set up Docker configuration
2019/03/19 19:01:54 Logging in to registry: mycontainerregistry008.azurecr.io
2019/03/19 19:01:55 Successfully logged into mycontainerregistry008.azurecr.io
2019/03/19 19:01:55 Executing step ID: acb_step_0. Working directory: '', Network: 'acb_default_network'
2019/03/19 19:01:55 Launching container with name: acb_step_0

Hello from Docker!
This message shows that your installation appears to be working correctly.

To generate this message, Docker took the following steps:
 1. The Docker client contacted the Docker daemon.
 2. The Docker daemon pulled the "hello-world" image from the Docker Hub.
    (amd64)
 3. The Docker daemon created a new container from that image which runs the
    executable that produces the output you are currently reading.
 4. The Docker daemon streamed that output to the Docker client, which sent it
    to your terminal.

To try something more ambitious, you can run an Ubuntu container with:
 $ docker run -it ubuntu bash

Share images, automate workflows, and more with a free Docker ID:
 https://hub.docker.com/

For more examples and ideas, visit:
 https://docs.docker.com/get-started/

2019/03/19 19:01:56 Successfully executed container: acb_step_0
2019/03/19 19:01:56 Step ID: acb_step_0 marked as successful (elapsed time in seconds: 0.843801)

Run ID: cab was successful after 6s
```

## <a name="clean-up-resources"></a>Bereinigen von Ressourcen

Wenn Sie die Ressourcengruppe, die Containerregistrierung und die darin gespeicherten Containerimages nicht mehr benötigen, können Sie sie mit dem Befehl [az group delete][az-group-delete] entfernen.

```azurecli
az group delete --name myResourceGroup
```

## <a name="next-steps"></a>Nächste Schritte

In dieser Schnellstartanleitung haben Sie mithilfe von ACR Tasks-Features schnell ein Docker-Containerimage erstellt, mithilfe von Push übertragen und nativ in Azure ausgeführt. In den Tutorials zu Azure Container Registry erfahren Sie, wie Sie Imagebuildvorgänge und -aktualisierungen mithilfe von ACR Tasks automatisieren.

> [!div class="nextstepaction"]
> [Tutorials zu Azure Container Registry][container-registry-tutorial-quick-task]

<!-- LINKS - external -->
[docker-linux]: https://docs.docker.com/engine/installation/#supported-platforms
[docker-mac]: https://docs.docker.com/docker-for-mac/
[docker-push]: https://docs.docker.com/engine/reference/commandline/push/
[docker-pull]: https://docs.docker.com/engine/reference/commandline/pull/
[docker-rmi]: https://docs.docker.com/engine/reference/commandline/rmi/
[docker-run]: https://docs.docker.com/engine/reference/commandline/run/
[docker-tag]: https://docs.docker.com/engine/reference/commandline/tag/
[docker-windows]: https://docs.docker.com/docker-for-windows/
[azure-account]: https://azure.microsoft.com/free/

<!-- LINKS - internal -->
[az-acr-create]: /cli/azure/acr#az-acr-create
[az-acr-build]: /cli/azure/acr#az-acr-build
[az-group-create]: /cli/azure/group#az-group-create
[az-group-delete]: /cli/azure/group#az-group-delete
[azure-cli]: /cli/azure/install-azure-cli
[container-registry-tasks-overview]: container-registry-tasks-overview.md
[container-registry-tutorial-quick-task]: container-registry-tutorial-quick-task.md
[container-registry-skus]: container-registry-skus.md
[azure-cli-install]: /cli/azure/install-azure-cli
