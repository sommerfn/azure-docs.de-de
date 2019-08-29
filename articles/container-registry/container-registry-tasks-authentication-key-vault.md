---
title: Externe Authentifizierung in einer Azure Container Registry-Aufgabe
description: Aktivieren Sie eine verwaltete Identität für Azure-Ressourcen in einer Azure Container Registry-Aufgabe (ACR), damit in der Aufgabe die in einem Azure-Schlüsseltresor gespeicherten Docker Hub-Anmeldeinformationen gelesen werden können.
services: container-registry
author: dlepow
ms.service: container-registry
ms.topic: article
ms.date: 07/12/2019
ms.author: danlep
ms.openlocfilehash: bcaf2918c92ec7b8223d394290a1d7c624fc451c
ms.sourcegitcommit: 0e59368513a495af0a93a5b8855fd65ef1c44aac
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 08/15/2019
ms.locfileid: "69509233"
---
# <a name="external-authentication-in-an-acr-task-using-an-azure-managed-identity"></a>Externe Authentifizierung in einer ACR-Aufgabe unter Verwendung einer in Azure verwalteten Identität 

In einer [ACR-Aufgabe](container-registry-tasks-overview.md) können Sie [eine verwaltete Identität für Azure-Ressourcen aktivieren](container-registry-tasks-authentication-managed-identity.md). In der Aufgabe kann die Identität für den Zugriff auf weitere Azure-Ressourcen verwendet werden, ohne dass Anmeldeinformationen angegeben oder verwaltet werden müssen. 

In diesem Artikel erfahren Sie, wie Sie eine verwaltete Identität in einer Aufgabe aktivieren, die auf Geheimnisse zugreift, die in einem Azure-Schlüsseltresor gespeichert sind. 

Um die in diesem Artikel verwendeten Azure-Ressourcen zu erstellen, müssen Sie mindestens Version 2.0.68 der Azure-Befehlszeilenschnittstelle (Azure CLI) ausführen. Führen Sie `az --version` aus, um die Version zu finden. Informationen zum Durchführen einer Installation oder eines Upgrades finden Sei bei Bedarf unter [Installieren der Azure CLI][azure-cli].

## <a name="scenario-overview"></a>Übersicht über das Szenario

Die Beispielaufgabe liest Docker Hub-Anmeldeinformationen, die in einem Azure-Schlüsseltresor gespeichert sind. Die Anmeldeinformationen gelten für ein Docker Hub-Konto mit Schreibberechtigungen (Push) für ein privates Repository in Docker Hub. Zum Lesen der Anmeldeinformationen konfigurieren Sie die Aufgabe mit einer verwalteten Identität und weisen ihr entsprechende Berechtigungen zu. Die mit der Identität verknüpfte Aufgabe erstellt ein Image und meldet sich bei Docker Hub an, um das Image an das private Repository zu übertragen. 

In diesem Beispiel werden die Schritte unter Verwendung einer vom Benutzer zugewiesenen oder einer vom System zugewiesenen verwalteten Identität beschrieben. Die Wahl der Identität hängt von den Anforderungen Ihrer Organisation ab.

In der Praxis kann ein Unternehmen Images für ein privates Repository in Docker Hub als Teil des Buildprozesses veröffentlichen. 

## <a name="prerequisites"></a>Voraussetzungen

Sie benötigen eine Azure-Containerregistrierung, in der Sie die Aufgabe ausführen. In diesem Artikel wird die Registrierung *myregistry* verwendet. Diese ersetzen Sie in späteren Schritten durch Ihre eigene Registrierung.

Wenn Sie noch über keine Azure-Containerregistrierung verfügen, finden Sie entsprechende Informationen unter [Schnellstart: Erstellen einer privaten Containerregistrierung mit der Azure CLI](container-registry-get-started-azure-cli.md). Sie müssen noch keine Images in die Registrierung pushen.

Außerdem benötigen Sie ein privates Repository in Docker Hub und ein Docker Hub-Konto mit Berechtigungen zum Schreiben in das Repository. In diesem Beispiel wird das Repository *hubuser/hubrepo* verwendet. 

## <a name="create-a-key-vault-and-store-secrets"></a>Erstellen eines Schlüsseltresors und Speichern von Geheimnissen

Erstellen Sie bei Bedarf zunächst mithilfe des folgenden Befehls [az group create][az-group-create] eine Ressourcengruppe namens *myResourceGroup* am Standort *eastus*:

```azurecli-interactive
az group create --name myResourceGroup --location eastus
```

Verwenden Sie den Befehl [az keyvault create][az-keyvault-create], um einen Schlüsseltresor zu erstellen. Geben Sie hierbei einen eindeutigen Namen für die Key Vault-Instanz an. 

```azurecli-interactive
az keyvault create --name mykeyvault --resource-group myResourceGroup --location eastus
```

Speichern Sie mit dem Befehl [az keyvault secret set][az-keyvault-secret-set] die erforderlichen Docker Hub-Anmeldeinformationen im Schlüsseltresor. In diesen Befehlen werden die Werte in Umgebungsvariablen übergeben:

```azurecli
# Store Docker Hub user name
az keyvault secret set \
  --name UserName \
  --value $USERNAME \
  --vault-name mykeyvault

# Store Docker Hub password
az keyvault secret set \
  --name Password \
  --value $PASSWORD \
  --vault-name mykeyvault
```

In der Praxis werden Geheimnisse wahrscheinlich in einem separaten Prozess festgelegt und verwaltet.

## <a name="define-task-steps-in-yaml-file"></a>Definieren von Aufgabenschritten in einer YAML-Datei

Die Schritte für diese Beispielaufgabe werden in einer [YAML-Datei](container-registry-tasks-reference-yaml.md) definiert. Erstellen Sie eine Datei mit dem Namen `dockerhubtask.yaml` in einem lokalen Arbeitsverzeichnis, und fügen Sie Folgendes ein. Achten Sie darauf, dass Sie den Namen des Schlüsseltresors in der Datei durch den Namen Ihres Schlüsseltresors ersetzen.

```yml
version: v1.0.0
# Replace mykeyvault with the name of your key vault
secrets:
  - id: username
    keyvault: https://mykeyvault.vault.azure.net/secrets/UserName
  - id: password
    keyvault: https://mykeyvault.vault.azure.net/secrets/Password
steps:
# Log in to Docker Hub
  - cmd: docker login --username '{{.Secrets.username}}' --password '{{.Secrets.password}}'
# Build image
  - build: -t {{.Values.PrivateRepo}}:{{.Run.ID}} https://github.com/Azure-Samples/acr-tasks.git -f hello-world.dockerfile
# Push image to private repo in Docker Hub
  - push:
    - {{.Values.PrivateRepo}}:{{.Run.ID}}
```

Mit den Aufgabenschritten wird Folgendes ausgeführt:

* Verwalten geheimer Anmeldeinformationen für die Authentifizierung bei Docker Hub
* Authentifizieren bei Docker Hub durch Übergeben der Geheimnisse an den Befehl `docker login`
* Erstellen eines Images mithilfe eines Beispiel-Dockerfile im Repository [Azure-Samples/acr-tasks](https://github.com/Azure-Samples/acr-tasks.git)
* Übertragen des Images an das private Docker Hub-Repository

## <a name="option-1-create-task-with-user-assigned-identity"></a>Option 1: Erstellen einer Aufgabe mit benutzerseitig zugewiesener Identität

Mit den Schritten in diesem Abschnitt wird eine Aufgabe erstellt und eine benutzerseitig zugewiesene Identität aktiviert. Wenn Sie stattdessen eine systemseitig zugewiesene Identität aktivieren möchten, navigieren Sie zu [Option 2: Erstellen einer Aufgabe mit systemseitig zugewiesener Identität](#option-2-create-task-with-system-assigned-identity). 

[!INCLUDE [container-registry-tasks-user-assigned-id](../../includes/container-registry-tasks-user-assigned-id.md)]

### <a name="create-task"></a>Erstellen der Aufgabe

Führen Sie den folgenden Befehl [az acr task create][az-acr-task-create] aus, um die Aufgabe *dockerhubtask* zu erstellen. Die Aufgabe läuft ohne Quellcode-Kontext, und der Befehl verweist auf die Datei `dockerhubtask.yaml` im Arbeitsverzeichnis. Der Parameter `--assign-identity` übergibt die Ressourcen-ID der benutzerseitig zugewiesenen Identität. 

```azurecli
az acr task create \
  --name dockerhubtask \
  --registry myregistry \
  --context /dev/null \
  --file dockerhubtask.yaml \
  --assign-identity $resourceID
```

[!INCLUDE [container-registry-tasks-user-id-properties](../../includes/container-registry-tasks-user-id-properties.md)]

## <a name="option-2-create-task-with-system-assigned-identity"></a>Option 2: Erstellen einer Aufgabe mit systemseitig zugewiesener Identität

Mit den Schritten in diesem Abschnitt wird eine Aufgabe erstellt und eine systemseitig zugewiesene Identität aktiviert. Wenn Sie stattdessen eine benutzerseitig zugewiesene Identität aktivieren möchten, navigieren Sie zu [Option 1: Erstellen einer Aufgabe mit benutzerseitig zugewiesener Identität](#option-1-create-task-with-user-assigned-identity). 

### <a name="create-task"></a>Erstellen der Aufgabe

Führen Sie den folgenden Befehl [az acr task create][az-acr-task-create] aus, um die Aufgabe *dockerhubtask* zu erstellen. Die Aufgabe läuft ohne Quellcode-Kontext, und der Befehl verweist auf die Datei `dockerhubtask.yaml` im Arbeitsverzeichnis. Der Parameter `--assign-identity` ohne Wert aktiviert die systemseitig zugewiesene verwaltete Identität für die Aufgabe.  

```azurecli
az acr task create \
  --name dockerhubtask \
  --registry myregistry \
  --context /dev/null \
  --file dockerhubtask.yaml \
  --assign-identity 
```

[!INCLUDE [container-registry-tasks-system-id-properties](../../includes/container-registry-tasks-system-id-properties.md)]

## <a name="grant-identity-access-to-key-vault"></a>Gewähren des Zugriffs auf den Schlüsseltresor für die Identität

Führen Sie den folgenden Befehl vom Typ [az keyvault set-policy][az-keyvault-set-policy] aus, um eine Zugriffsrichtlinie für den Schlüsseltresor festzulegen. Das folgende Beispiel ermöglicht der Identität das Lesen von Geheimnissen aus dem Schlüsseltresor. 

```azurecli
az keyvault set-policy --name mykeyvault --resource-group myResourceGroup --object-id $principalID --secret-permissions get
```

## <a name="manually-run-the-task"></a>Manuelles Ausführen der Aufgabe

Um zu überprüfen, ob die Aufgabe, in der Sie eine verwaltete Identität aktiviert haben, erfolgreich ausgeführt wird, führen Sie die Aufgabe mit dem Befehl [az acr task run][az-acr-task-run] manuell aus. Der Parameter `--set` wird verwendet, um den Namen des privaten Repositorys an die Aufgabe zu übergeben. In diesem Beispiel lautet der Name des Platzhalterrepositorys *hubuser/hubrepo*.

```azurecli
az acr task run --name dockerhubtask --registry myregistry --set PrivateRepo=hubuser/hubrepo 
```

Wenn die Aufgabe erfolgreich ausgeführt wurde, wird in der Ausgabe die erfolgreiche Authentifizierung bei Docker Hub angezeigt und das Image erfolgreich erstellt und an das private Repository übergeben:

```console
Queued a run with ID: cf24
Waiting for an agent...
2019/06/20 18:05:55 Using acb_vol_b1edae11-30de-4f2b-a9c7-7d743e811101 as the home volume
2019/06/20 18:05:58 Creating Docker network: acb_default_network, driver: 'bridge'
2019/06/20 18:05:58 Successfully set up Docker network: acb_default_network
2019/06/20 18:05:58 Setting up Docker configuration...
2019/06/20 18:05:59 Successfully set up Docker configuration
2019/06/20 18:05:59 Logging in to registry: myregistry.azurecr.io
2019/06/20 18:06:00 Successfully logged into myregistry.azurecr.io
2019/06/20 18:06:00 Executing step ID: acb_step_0. Timeout(sec): 600, Working directory: '', Network: 'acb_default_network'
2019/06/20 18:06:00 Launching container with name: acb_step_0
[...]
Login Succeeded
2019/06/20 18:06:02 Successfully executed container: acb_step_0
2019/06/20 18:06:02 Executing step ID: acb_step_1. Timeout(sec): 600, Working directory: '', Network: 'acb_default_network'
2019/06/20 18:06:02 Scanning for dependencies...
2019/06/20 18:06:04 Successfully scanned dependencies
2019/06/20 18:06:04 Launching container with name: acb_step_1
Sending build context to Docker daemon    129kB
[...]
2019/06/20 18:06:07 Successfully pushed image: hubuser/hubrepo:cf24
2019/06/20 18:06:07 Step ID: acb_step_0 marked as successful (elapsed time in seconds: 2.064353)
2019/06/20 18:06:07 Step ID: acb_step_1 marked as successful (elapsed time in seconds: 2.594061)
2019/06/20 18:06:07 Populating digests for step ID: acb_step_1...
2019/06/20 18:06:09 Successfully populated digests for step ID: acb_step_1
2019/06/20 18:06:09 Step ID: acb_step_2 marked as successful (elapsed time in seconds: 2.743923)
2019/06/20 18:06:09 The following dependencies were found:
2019/06/20 18:06:09
- image:
    registry: registry.hub.docker.com
    repository: hubuser/hubrepo
    tag: cf24
    digest: sha256:92c7f9c92844bbbb5d0a101b22f7c2a7949e40f8ea90c8b3bc396879d95e899a
  runtime-dependency:
    registry: registry.hub.docker.com
    repository: library/hello-world
    tag: latest
    digest: sha256:0e11c388b664df8a27a901dce21eb89f11d8292f7fca1b3e3c4321bf7897bffe
  git:
    git-head-revision: b0ffa6043dd893a4c75644c5fed384c82ebb5f9e

Run ID: cf24 was successful after 15s
```

Um zu überprüfen, ob das Image übertragen wird, überprüfen Sie das private Docker Hub-Repository auf das Tag (`cf24` in diesem Beispiel).

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
[az-identity-create]: /cli/azure/identity#az-identity-create
[az-identity-show]: /cli/azure/identity#az-identity-show
[azure-cli]: /cli/azure/install-azure-cli
[az-acr-task-create]: /cli/azure/acr/task#az-acr-task-create
[az-acr-task-show]: /cli/azure/acr/task#az-acr-task-show
[az-acr-task-run]: /cli/azure/acr/task#az-acr-task-run
[az-acr-task-list-runs]: /cli/azure/acr/task#az-acr-task-list-runs
[az-acr-task-credential-add]: /cli/azure/acr/task/credential#az-acr-task-credential-add
[az-group-create]: /cli/azure/group?#az-group-create
[az-keyvault-create]: /cli/azure/keyvault?#az-keyvault-create
[az-keyvault-secret-set]: /cli/azure/keyvault/secret#az-keyvault-secret-set
[az-keyvault-set-policy]: /cli/azure/keyvault#az-keyvault-set-policy
