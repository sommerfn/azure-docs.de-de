---
title: Registrierungsübergreifende Authentifizierung in einer Azure Container Registry-Aufgabe
description: Aktivieren Sie eine verwaltete Identität für Azure-Ressourcen in einer ACR-Aufgabe (Azure Container Registry), damit die Aufgabe auf eine andere private Containerregistrierung zugreifen kann.
services: container-registry
author: dlepow
ms.service: container-registry
ms.topic: article
ms.date: 07/12/2019
ms.author: danlep
ms.openlocfilehash: 07fa7f3df5274ae88c93deac75093ead3f32f036
ms.sourcegitcommit: 0e59368513a495af0a93a5b8855fd65ef1c44aac
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 08/15/2019
ms.locfileid: "69509095"
---
# <a name="cross-registry-authentication-in-an-acr-task-using-an-azure-managed-identity"></a>Registrierungsübergreifende Authentifizierung in einer ACR-Aufgabe unter Verwendung einer in Azure verwalteten Identität 

In einer [ACR-Aufgabe](container-registry-tasks-overview.md) können Sie [eine verwaltete Identität für Azure-Ressourcen aktivieren](container-registry-tasks-authentication-managed-identity.md). Die Identität kann von der Aufgabe für den Zugriff auf weitere Azure-Ressourcen verwendet werden, ohne dass Anmeldeinformationen angegeben oder verwaltet werden müssen. 

In diesem Artikel erfahren Sie, wie Sie eine verwaltete Identität in einer Aufgabe aktivieren, die ein Image aus einer Registrierung pullt, bei der es sich nicht um die Registrierung handelt, die zum Ausführen der Aufgabe verwendet wird.

Für die Erstellung der in diesem Artikel verwendeten Azure-Ressourcen muss mindestens die Version 2.0.68 der Azure-Befehlszeilenschnittstelle (Azure CLI) verwendet werden. Führen Sie `az --version` aus, um die Version zu finden. Informationen zum Durchführen einer Installation oder eines Upgrades finden Sei bei Bedarf unter [Installieren der Azure CLI][azure-cli].

## <a name="scenario-overview"></a>Übersicht über das Szenario

Die Beispielaufgabe pullt ein Basisimage aus einer anderen Azure-Containerregistrierung, um ein Anwendungsimage zu erstellen und zu pushen. Zum Pullen des Basisimages konfigurieren Sie die Aufgabe mit einer verwalteten Identität und weisen ihr entsprechende Berechtigungen zu. 

In diesem Beispiel werden die Schritte unter Verwendung einer benutzerseitig zugewiesenen oder einer systemseitig zugewiesenen verwalteten Identität beschrieben. Die Wahl der Identität hängt von den Anforderungen Ihrer Organisation ab.

In der Praxis verfügt eine Organisation unter Umständen über eine Reihe von Basisimages, die von allen Entwicklungsteams für die Anwendungserstellung verwendet werden. Diese Basisimages werden in einer Unternehmensregistrierung gespeichert, und die einzelnen Entwicklungsteams verfügen jeweils nur über Pullberechtigungen. 

## <a name="prerequisites"></a>Voraussetzungen

Für diesen Artikel benötigen Sie zwei Azure-Containerregistrierungen:

* Sie verwenden die erste Registrierung zum Erstellen und Ausführen von ACR-Aufgaben. In diesem Artikel heißt diese Registrierung *myregistry*. 
* Die zweite Registrierung hostet ein Basisimage, das für die Aufgabe verwendet wird, um ein Image zu erstellen. In diesem Artikel heißt die zweite Registrierung *mybaseregistry*. 

Ersetzen Sie in den weiteren Schritten diese Namen durch Ihre eigenen Registrierungsnamen.

Wenn Sie nicht bereits die erforderlichen Azure-Containerregistrierungen haben, siehe [Schnellstart: Erstellen einer privaten Containerregistrierung mit der Azure CLI](container-registry-get-started-azure-cli.md). Sie müssen noch keine Images in die Registrierung pushen.

## <a name="prepare-base-registry"></a>Vorbereiten der Basisregistrierung

Erstellen Sie zunächst ein Arbeitsverzeichnis und anschließend eine Datei namens „Dockerfile“ mit folgendem Inhalt. In diesem einfachen Beispiel wird ein Node.js-Basisimage auf der Grundlage eines öffentlichen Images in Docker Hub erstellt.
    
```bash
echo FROM node:9-alpine > Dockerfile
```
Führen Sie im aktuellen Verzeichnis den Befehl [az acr build][az-acr-build] aus, um das Basisimage zu erstellen und an die Basisregistrierung zu pushen. In der Praxis kann die Basisregistrierung von einem anderen Team oder Prozess in der Organisation verwaltet werden.
    
```azurecli
az acr build --image baseimages/node:9-alpine --registry mybaseregistry --file Dockerfile .
```

## <a name="define-task-steps-in-yaml-file"></a>Definieren von Aufgabenschritten in einer YAML-Datei

Die Schritte für diese exemplarische [Aufgabe mit mehreren Schritten](container-registry-tasks-multi-step.md) werden in einer [YAML-Datei](container-registry-tasks-reference-yaml.md) definiert. Erstellen Sie in Ihrem lokalen Arbeitsverzeichnis eine Datei namens `helloworldtask.yaml`, und fügen Sie Folgendes ein. Aktualisieren Sie im Buildschritt den Wert von `REGISTRY_NAME` mit dem Servernamen Ihrer Basisregistrierung.

```yml
version: v1.0.0
steps:
# Replace mybaseregistry with the name of your registry containing the base image
  - build: -t {{.Run.Registry}}/hello-world:{{.Run.ID}}  https://github.com/Azure-Samples/acr-build-helloworld-node.git -f Dockerfile-app --build-arg REGISTRY_NAME=mybaseregistry.azurecr.io
  - push: ["{{.Run.Registry}}/hello-world:{{.Run.ID}}"]
```

Im Buildschritt wird die Datei `Dockerfile-app` im Repository [Azure-Samples/acr-build-helloworld-node](https://github.com/Azure-Samples/acr-build-helloworld-node.git) verwendet, um ein Image zu erstellen. `--build-arg` verweist auf die Basisregistrierung, aus der das Basisimage gepullt werden soll. Nach erfolgreicher Erstellung wird das Image an die Registrierung gepusht, die zum Ausführen der Aufgabe verwendet wird.

## <a name="option-1-create-task-with-user-assigned-identity"></a>Option 1: Erstellen einer Aufgabe mit benutzerseitig zugewiesener Identität

Mit den Schritten in diesem Abschnitt wird eine Aufgabe erstellt und eine benutzerseitig zugewiesene Identität aktiviert. Wenn Sie stattdessen eine systemseitig zugewiesene Identität aktivieren möchten, navigieren Sie zu [Option 2: Erstellen einer Aufgabe mit systemseitig zugewiesener Identität](#option-2-create-task-with-system-assigned-identity). 

[!INCLUDE [container-registry-tasks-user-assigned-id](../../includes/container-registry-tasks-user-assigned-id.md)]

### <a name="create-task"></a>Erstellen der Aufgabe

Führen Sie den folgenden Befehl vom Typ [az acr task create][az-acr-task-create] aus, um die Aufgabe *helloworldtask* zu erstellen. Die Aufgabe läuft ohne Quellcode-Kontext, und der Befehl verweist auf die Datei `helloworldtask.yaml` im Arbeitsverzeichnis. Der Parameter `--assign-identity` übergibt die Ressourcen-ID der benutzerseitig zugewiesenen Identität. 

```azurecli
az acr task create \
  --registry myregistry \
  --name helloworldtask \
  --context /dev/null \
  --file helloworldtask.yaml \
  --assign-identity $resourceID
```

[!INCLUDE [container-registry-tasks-user-id-properties](../../includes/container-registry-tasks-user-id-properties.md)]

## <a name="option-2-create-task-with-system-assigned-identity"></a>Option 2: Erstellen einer Aufgabe mit systemseitig zugewiesener Identität

Mit den Schritten in diesem Abschnitt wird eine Aufgabe erstellt und eine systemseitig zugewiesene Identität aktiviert. Wenn Sie stattdessen eine benutzerseitig zugewiesene Identität aktivieren möchten, navigieren Sie zu [Option 1: Erstellen einer Aufgabe mit benutzerseitig zugewiesener Identität](#option-1-create-task-with-user-assigned-identity). 

### <a name="create-task"></a>Erstellen der Aufgabe

Führen Sie den folgenden Befehl vom Typ [az acr task create][az-acr-task-create] aus, um die Aufgabe *helloworldtask* zu erstellen. Die Aufgabe läuft ohne Quellcode-Kontext, und der Befehl verweist auf die Datei `helloworldtask.yaml` im Arbeitsverzeichnis. Der Parameter `--assign-identity` ohne Wert aktiviert die systemseitig zugewiesene verwaltete Identität für die Aufgabe. 

```azurecli
az acr task create \
  --registry myregistry \
  --name helloworldtask \
  --context /dev/null \
  --file helloworldtask.yaml \
  --assign-identity 
```
[!INCLUDE [container-registry-tasks-system-id-properties](../../includes/container-registry-tasks-system-id-properties.md)]

## <a name="give-identity-pull-permissions-to-the-base-registry"></a>Erteilen von Basisregistrierungs-Pullberechtigungen für die Identität

In diesem Abschnitt werden der verwalteten Identität Berechtigungen zum Pullen von Images aus der Basisregistrierung *mybaseregistry* erteilt.

Verwenden Sie den Befehl [az acr show][az-acr-show], um die Ressourcen-ID der Basisregistrierung abzurufen und in einer Variablen zu speichern:

```azurecli
baseregID=$(az acr show --name mybaseregistry --query id --output tsv)
```

Verwenden Sie den Befehl [az role assignment create][az-role-assignment-create], um der Identität die Rolle `acrpull` für die Basisregistrierung zuzuweisen. Diese Rolle ist nur zum Pullen von Images aus der Registrierung berechtigt.

```azurecli
az role assignment create --assignee $principalID --scope $baseregID --role acrpull
```

## <a name="add-target-registry-credentials-to-task"></a>Hinzufügen von Zielregistrierungs-Anmeldeinformationen zur Aufgabe

Verwenden Sie nun den Befehl [az acr task credential add][az-acr-task-credential-add], um der Aufgabe die Anmeldeinformationen der Identität hinzuzufügen, sodass sie sich bei der Basisregistrierung authentifizieren kann. Führen Sie den passenden Befehl für die Art der verwalteten Identität aus, die Sie in der Aufgabe aktiviert haben. Wenn Sie eine benutzerseitig zugewiesene Identität aktiviert haben, übergeben Sie `--use-identity` mit der Client-ID der Identität. Wenn Sie eine systemseitig zugewiesene Identität aktiviert haben, übergeben Sie `--use-identity [system]`.

```azurecli
# Add credentials for user-assigned identity to the task
az acr task credential add \
  --name helloworldtask \
  --registry myregistry \
  --login-server mybaseregistry.azurecr.io \
  --use-identity $clientID

# Add credentials for system-assigned identity to the task
az acr task credential add \
  --name helloworldtask \
  --registry myregistry \
  --login-server mybaseregistry.azurecr.io \
  --use-identity [system]
```

## <a name="manually-run-the-task"></a>Manuelles Ausführen der Aufgabe

Um zu überprüfen, ob die Aufgabe, in der Sie eine verwaltete Identität aktiviert haben, erfolgreich ausgeführt wird, führen Sie die Aufgabe manuell mit dem Befehl [az acr task run][az-acr-task-run] aus. 

```azurecli
az acr task run \
  --name helloworldtask \
  --registry myregistry
```

Bei erfolgreicher Ausführung sieht die Ausgabe in etwa wie folgt aus:

```
Queued a run with ID: cf10
Waiting for an agent...
2019/06/14 22:47:32 Using acb_vol_dbfbe232-fd76-4ca3-bd4a-687e84cb4ce2 as the home volume
2019/06/14 22:47:39 Creating Docker network: acb_default_network, driver: 'bridge'
2019/06/14 22:47:40 Successfully set up Docker network: acb_default_network
2019/06/14 22:47:40 Setting up Docker configuration...
2019/06/14 22:47:41 Successfully set up Docker configuration
2019/06/14 22:47:41 Logging in to registry: myregistry.azurecr.io
2019/06/14 22:47:42 Successfully logged into myregistry.azurecr.io
2019/06/14 22:47:42 Logging in to registry: mybaseregistry.azurecr.io
2019/06/14 22:47:43 Successfully logged into mybaseregistry.azurecr.io
2019/06/14 22:47:43 Executing step ID: acb_step_0. Timeout(sec): 600, Working directory: '', Network: 'acb_default_network'
2019/06/14 22:47:43 Scanning for dependencies...
2019/06/14 22:47:45 Successfully scanned dependencies
2019/06/14 22:47:45 Launching container with name: acb_step_0
Sending build context to Docker daemon   25.6kB
Step 1/6 : ARG REGISTRY_NAME
Step 2/6 : FROM ${REGISTRY_NAME}/baseimages/node:9-alpine
9-alpine: Pulling from baseimages/node
[...]
Successfully built 41b49a112663
Successfully tagged myregistry.azurecr.io/hello-world:cf10
2019/06/14 22:47:56 Successfully executed container: acb_step_0
2019/06/14 22:47:56 Executing step ID: acb_step_1. Timeout(sec): 600, Working directory: '', Network: 'acb_default_network'
2019/06/14 22:47:56 Pushing image: myregistry.azurecr.io/hello-world:cf10, attempt 1
The push refers to repository [myregistry.azurecr.io/hello-world]
[...]
2019/06/14 22:48:00 Step ID: acb_step_1 marked as successful (elapsed time in seconds: 2.517011)
2019/06/14 22:48:00 The following dependencies were found:
2019/06/14 22:48:00
- image:
    registry: myregistry.azurecr.io
    repository: hello-world
    tag: cf10
    digest: sha256:611cf6e3ae3cb99b23fadcd89fa144e18aa1b1c9171ad4a0da4b62b31b4e38d1
  runtime-dependency:
    registry: mybaseregistry.azurecr.io
    repository: baseimages/node
    tag: 9-alpine
    digest: sha256:e8e92cffd464fce3be9a3eefd1b65dc9cbe2484da31c11e813a4effc6105c00f
  git:
    git-head-revision: 0f988779c97fe0bfc7f2f74b88531617f4421643

Run ID: cf10 was successful after 32s
```

Führen Sie den Befehl [az acr repository show-tags][az-acr-repository-show-tags] aus, um sich zu vergewissern, dass das erstellte Image erfolgreich an *myregistry* gepusht wurde:

```azurecli
az acr repository show-tags --name myregistry --repository hello-world --output tsv
```

Beispielausgabe:

```console
cf10
```

## <a name="next-steps"></a>Nächste Schritte

* Weitere Informationen zum Aktivieren einer verwalteten Identität in einer ACR-Aufgabe finden Sie [hier](container-registry-tasks-authentication-managed-identity.md).
* Die YAML-Referenz für ACR Tasks finden Sie [hier](container-registry-tasks-reference-yaml.md).

<!-- LINKS - Internal -->
[az-login]: /cli/azure/reference-index#az-login
[az-acr-login]: /cli/azure/acr#az-acr-login
[az-acr-show]: /cli/azure/acr#az-acr-show
[az-acr-build]: /cli/azure/acr#az-acr-build
[az-acr-repository-show-tags]: /cli/azure/acr/repository#az-acr-repository-show-tags
[az-role-assignment-create]: /cli/azure/role/assignment#az-role-assignment-create
[az-acr-login]: /cli/azure/acr#az-acr-login
[azure-cli]: /cli/azure/install-azure-cli
[az-acr-task-create]: /cli/azure/acr/task#az-acr-task-create
[az-acr-task-show]: /cli/azure/acr/task#az-acr-task-show
[az-acr-task-run]: /cli/azure/acr/task#az-acr-task-run
[az-acr-task-list-runs]: /cli/azure/acr/task#az-acr-task-list-runs
[az-acr-task-credential-add]: /cli/azure/acr/task/credential#az-acr-task-credential-add
[az-group-create]: /cli/azure/group?#az-group-create
