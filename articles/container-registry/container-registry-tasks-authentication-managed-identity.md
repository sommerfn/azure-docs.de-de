---
title: Verwenden einer verwalteten Identität mit Azure Container Registry Tasks
description: Bieten Sie einer Azure Container Registry-Aufgabe durch Zuweisung einer verwalteten Identität für Azure-Ressourcen Zugriff auf Azure-Ressourcen einschließlich anderer privater Containerregistrierungen.
services: container-registry
author: dlepow
ms.service: container-registry
ms.topic: article
ms.date: 06/12/2019
ms.author: danlep
ms.openlocfilehash: 5b60727472a06aaac8ccd3dce8609461e8972311
ms.sourcegitcommit: 72f1d1210980d2f75e490f879521bc73d76a17e1
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 06/14/2019
ms.locfileid: "67148030"
---
# <a name="use-an-azure-managed-identity-in-acr-tasks"></a>Verwenden einer verwalteten Azure-Identität in ACR Tasks 

Verwenden Sie eine [verwaltete Identität für Azure-Ressourcen](../active-directory/managed-identities-azure-resources/overview.md), um sich von ACR Tasks aus bei einer Azure Container Registry-Instanz oder anderen Azure-Ressourcen zu authentifizieren, ohne Anmeldeinformationen im Code angeben oder verwalten zu müssen. Verwenden Sie beispielsweise eine verwaltete Identität, um Containerimages als Schritt in einer Aufgabe in eine andere Registrierung zu pullen oder pushen.

In diesem Artikel erfahren Sie mehr über verwaltete Identitäten und lernen Folgendes:

> [!div class="checklist"]
> * Aktivieren einer systemseitig oder benutzerseitig zugewiesenen Identität auf einer ACR-Aufgabe
> * Gewähren des Identitätszugriffs auf Azure-Ressourcen wie andere Azure-Containerregistrierungen
> * Verwenden der verwalteten Identität für den Zugriff auf die Ressourcen aus einer Aufgabe 

Um die in diesem Artikel verwendeten Azure-Ressourcen zu erstellen, müssen Sie mindestens Version 2.0.66 der Azure-Befehlszeilenschnittstelle (Azure CLI) ausführen. Führen Sie `az --version` aus, um die Version zu finden. Wenn Sie eine Installation oder ein Upgrade ausführen müssen, finden Sie unter [Installieren von Azure CLI 2.0][azure-cli] Informationen dazu.

## <a name="why-use-a-managed-identity"></a>Gründe für die Verwendung einer verwalteten Identität

Über eine verwaltete Identität für Azure-Ressourcen wird für Azure-Dienste eine automatisch verwaltete Identität in Azure Active Directory (Azure AD) bereitgestellt. Sie können [bestimmte Azure-Ressourcen](../active-directory/managed-identities-azure-resources/services-support-managed-identities.md) einschließlich ACR Tasks mit einer verwalteten Identität konfigurieren. Anschließend verwenden Sie die Identität für den Zugriff auf weitere Ressourcen, ohne dass Anmeldeinformationen in Code oder Skripts übergeben werden.

Es gibt zwei Arten von verwalteten Identitäten:

* *Benutzerseitig zugewiesene Identitäten* können mehreren Ressourcen zugewiesen werden und werden für die gewünschte Dauer beibehalten. Benutzerseitig zugewiesene Identitäten befinden sich zurzeit in der Vorschau.

* Vom *System verwaltete Identitäten* sind für eine spezifische Ressource wie eine ACR-Aufgabe eindeutig und für die Lebensdauer dieser Ressource gültig.

Nachdem Sie eine Azure-Ressource mit einer verwalteten Identität eingerichtet haben, können Sie der verwalteten Identität genau wie bei jedem anderen Sicherheitsprinzipal Zugriff auf eine andere Ressource erteilen. Weisen Sie beispielsweise einer verwalteten Identität eine Rolle mit Pullberechtigungen, Push- und Pullberechtigungen oder anderen Berechtigungen für eine private Containerregistrierung in Azure zu. (Eine vollständige Liste der Rollen finden Sie unter [Azure Container Registry – Rollen und Berechtigungen](container-registry-roles.md).) Sie können einer Identität Zugriff auf eine oder mehrere Ressourcen gewähren.

## <a name="create-container-registries"></a>Erstellen von Containerregistrierungen

Für dieses Tutorial benötigen Sie drei Containerregistrierungen:

* Sie verwenden die erste Registrierung zum Erstellen und Ausführen von ACR-Aufgaben. In diesem Artikel heißt diese Quellregistrierung *myregistry*. 
* Die zweiten und dritten Registrierungen sind Zielregistrierungen für die erste Beispielaufgabe zum Pushen eines Images, das sie erstellt. In diesem Artikel sind die Zielregistrierungen mit *customregistry1* und *customregistry2* benannt.

Ersetzen Sie in den weiteren Schritten diese Namen durch Ihre eigenen Registrierungsnamen.

Wenn Sie nicht bereits die erforderlichen Azure-Containerregistrierungen haben, siehe [Schnellstart: Erstellen einer privaten Containerregistrierung mit der Azure CLI](container-registry-get-started-azure-cli.md). Sie müssen noch keine Images in die Registrierung pushen.

## <a name="example-task-with-a-system-assigned-identity"></a>Beispiel: Aufgabe mit einer systemseitig zugewiesenen Identität

Dieses Beispiel zeigt Ihnen, wie Sie eine [Aufgabe in mehreren Schritten](container-registry-tasks-multi-step.md) mit einer vom System zugewiesenen Identität erstellen. Die Aufgabe erstellt ein Image und verwendet dann die Identität zur Authentifizierung mit zwei Zielregistrierungen, um das Image zu verschieben.

Die Schritte für diese Beispielaufgabe sind in einer [YAML-Datei](container-registry-tasks-reference-yaml.md) mit dem Namen `testtask.yaml` definiert. Die Datei befindet sich im multipleRegistries-Verzeichnis des [acr-tasks](https://github.com/Azure-Samples/acr-tasks)-Beispielrepositorys. Die Datei wird hier reproduziert:

```yml
version: v1.0.0
steps:
  - build: -t {{.Values.REGISTRY1}}/hello-world:{{.Run.ID}} . -f hello-world.dockerfile
  - push: ["{{.Values.REGISTRY1}}/hello-world:{{.Run.ID}}"]
  - build: -t {{.Values.REGISTRY2}}/hello-world:{{.Run.ID}} . -f hello-world.dockerfile
  - push: ["{{.Values.REGISTRY2}}/hello-world:{{.Run.ID}}"]
```

### <a name="create-task-with-system-assigned-identity"></a>Erstellen einer Aufgabe mit systemseitig zugewiesener Identität

Erstellen Sie die Aufgabe *multiple-reg* durch Ausführen des folgenden [az acr task create][az-acr-task-create]-Befehls. Der Aufgabenkontext ist der multipleRegistries-Ordner des Beispielrepositorys, und der Befehl verweist auf die Datei `testtask.yaml` im Repository. Der `--assign-identity`-Parameter ohne zusätzlichen Wert erstellt eine vom System zugewiesene Identität für die Aufgabe. Diese Aufgabe ist so eingerichtet, dass Sie sie manuell auslösen müssen, aber Sie könnten sie so einrichten, dass sie ausgeführt wird, wenn Commits in das Repository gepusht werden oder eine Pullanforderung gestellt wird. 

```azurecli
az acr task create \
  --registry myregistry \
  --name multiple-reg \
  --context https://github.com/Azure-Samples/acr-tasks.git#:multipleRegistries \
  --file testtask.yaml \
  --commit-trigger-enabled false \
  --pull-request-trigger-enabled false \
  --assign-identity
```

In der Befehlsausgabe zeigt der Abschnitt `identity` an, dass eine Identität vom Typ `SystemAssigned` in der Aufgabe festgelegt ist. Die `principalId` sollte die Dienstprinzipal-ID der Identität sein:

```console
[...]
  "identity": {
    "principalId": "xxxxxxxx-2703-42f9-97d0-xxxxxxxxxxxx",
    "tenantId": "xxxxxxxx-86f1-41af-91ab-xxxxxxxxxxxx",
    "type": "SystemAssigned",
    "userAssignedIdentities": null
  },
  "location": "eastus",
[...]
``` 

Verwenden Sie den Befehl [az acr task show][az-acr-task-show], um die `principalId` in einer Variablen zu speichern und in späteren Befehlen zu verwenden:

```azurecli
principalID=$(az acr task show --name multiple-reg --registry myregistry --query identity.principalId --output tsv)
```

### <a name="give-identity-push-permissions-to-two-target-container-registries"></a>Erteilen von Identitätspushberechtigungen für zwei Zielcontainerregistrierungen

Erteilen Sie in diesem Abschnitt der vom System zugewiesenen Identität Berechtigungen, die beiden Zielregistrierungen mit den Namen *customregistry1* und *customregistry2* zu pushen.

Zunächst verwenden Sie den Befehl [az acr show][az-acr-show], um die Ressourcen-ID jeder Registrierung abzurufen und die IDs in Variablen zu speichern:

```azurecli
reg1_id=$(az acr show --name customregistry1 --query id --output tsv)
reg2_id=$(az acr show --name customregistry2 --query id --output tsv)
```

Verwenden Sie den Befehl [az role assignment create][az-role-assignment-create], um der Identität die `acrpush`-Rolle für jede Registrierung zuzuweisen. Diese Rolle besitzt Berechtigungen zum Pullen und Pushen von Images an eine Containerregistrierung.

```azurecli
az role assignment create --assignee $principalID --scope $reg1_id --role acrpush
az role assignment create --assignee $principalID --scope $reg2_id --role acrpush
```

### <a name="add-target-registry-credentials-to-task"></a>Hinzufügen von Zielregistrierungs-Anmeldeinformationen zur Aufgabe

Verwenden Sie nun den Befehl [az acr task credential add][az-acr-task-credential-add], um die Anmeldeinformationen der Identität zur Aufgabe hinzuzufügen, sodass sie sich bei beiden Zielregistrierungen authentifizieren kann.

```azurecli
az acr task credential add \
  --name multiple-reg \
  --registry myregistry \
  --login-server customregistry1.azurecr.io \
  --use-identity [system]

az acr task credential add \
  --name multiple-reg \
  --registry myregistry \
  --login-server customregistry2.azurecr.io \
  --use-identity [system]
```

### <a name="manually-run-the-task"></a>Manuelles Ausführen der Aufgabe

Lösen Sie die Aufgabe mit dem Befehl [az acr task run][az-acr-task-run] manuell aus. Mit dem Parameter `--set` werden die Anmeldeservernamen der beiden Zielregistrierungen als Werte für die Aufgabenvariablen `REGISTRY1` und `REGISTRY2` übergeben.

```azurecli
az acr task run \
  --name multiple-reg \
  --registry myregistry \
  --set REGISTRY1=customregistry1.azurecr.io \
  --set REGISTRY2=customregistry2.azurecr.io
```

Die Ausgabe Die Ausgabe lautet in etwa wie folgt:

```console
Sending build context to Docker daemon  4.096kB
Step 1/1 : FROM hello-world
 ---> fce289e99eb9
Successfully built fce289e99eb9
Successfully tagged customregistry2.azurecr.io/hello-world:cf31
2019/05/31 22:16:02 Successfully executed container: acb_step_0
2019/05/31 22:16:02 Executing step ID: acb_step_1. Timeout(sec): 600, Working directory: 'multipleRegistries', Network: 'acb_default_network'
2019/05/31 22:16:02 Pushing image: customregistry2.azurecr.io/hello-world:cf31, attempt 1
The push refers to repository [customregistry2.azurecr.io/hello-world]
af0b15c8625b: Preparing
af0b15c8625b: Pushed
cf31: digest: sha256:92c7f9c92844bbbb5d0a101b22f7c2a7949e40f8ea90c8b3bc396879d95e899a size: 524
2019/05/31 22:16:08 Successfully pushed image: customregistry2.azurecr.io/hello-world:cf31
2019/05/31 22:16:08 Executing step ID: acb_step_2. Timeout(sec): 600, Working directory: 'multipleRegistries', Network: 'acb_default_network'
2019/05/31 22:16:08 Scanning for dependencies...
2019/05/31 22:16:08 Successfully scanned dependencies
2019/05/31 22:16:08 Launching container with name: acb_step_2
Sending build context to Docker daemon  4.096kB
Step 1/1 : FROM hello-world
 ---> fce289e99eb9
Successfully built fce289e99eb9
Successfully tagged customregistry1.azurecr.io/hello-world:cf31
2019/05/31 22:16:09 Successfully executed container: acb_step_2
2019/05/31 22:16:09 Executing step ID: acb_step_3. Timeout(sec): 600, Working directory: 'multipleRegistries', Network: 'acb_default_network'
2019/05/31 22:16:09 Pushing image: customregistry1.azurecr.io/hello-world:cf31, attempt 1
The push refers to repository [customregistry1.azurecr.io/hello-world]
af0b15c8625b: Preparing
af0b15c8625b: Pushed
cf31: digest: sha256:92c7f9c92844bbbb5d0a101b22f7c2a7949e40f8ea90c8b3bc396879d95e899a size: 524
2019/05/31 22:16:21 Successfully pushed image: customregistry1.azurecr.io/hello-world:cf31
2019/05/31 22:16:21 Step ID: acb_step_0 marked as successful (elapsed time in seconds: 1.985291)
2019/05/31 22:16:21 Populating digests for step ID: acb_step_0...
2019/05/31 22:16:22 Successfully populated digests for step ID: acb_step_0
2019/05/31 22:16:22 Step ID: acb_step_1 marked as successful (elapsed time in seconds: 5.743225)
2019/05/31 22:16:22 Step ID: acb_step_2 marked as successful (elapsed time in seconds: 1.925959)
2019/05/31 22:16:22 Populating digests for step ID: acb_step_2...
2019/05/31 22:16:24 Successfully populated digests for step ID: acb_step_2
2019/05/31 22:16:24 Step ID: acb_step_3 marked as successful (elapsed time in seconds: 11.061057)
2019/05/31 22:16:24 The following dependencies were found:
2019/05/31 22:16:24
- image:
    registry: customregistry2.azurecr.io
    repository: hello-world
    tag: cf31
    digest: sha256:92c7f9c92844bbbb5d0a101b22f7c2a7949e40f8ea90c8b3bc396879d95e899a
  runtime-dependency:
    registry: registry.hub.docker.com
    repository: library/hello-world
    tag: latest
    digest: sha256:6f744a2005b12a704d2608d8070a494ad1145636eeb74a570c56b94d94ccdbfc
  git:
    git-head-revision: 05275dca2bc61f584085ca913c39d509236f576b
- image:
    registry: customregistry1.azurecr.io
    repository: hello-world
    tag: cf31
    digest: sha256:92c7f9c92844bbbb5d0a101b22f7c2a7949e40f8ea90c8b3bc396879d95e899a
  runtime-dependency:
    registry: registry.hub.docker.com
    repository: library/hello-world
    tag: latest
    digest: sha256:6f744a2005b12a704d2608d8070a494ad1145636eeb74a570c56b94d94ccdbfc
  git:
    git-head-revision: 05275dca2bc61f584085ca913c39d509236f576b

Run ID: cf31 was successful after 35s
```

## <a name="example-task-with-a-user-assigned-identity"></a>Beispiel: Aufgabe mit einer benutzerseitig zugewiesenen Identität

In diesem Beispiel erstellen Sie eine vom Benutzer zugewiesene Identität mit Berechtigungen zum Lesen von Geheimnissen aus einem Azure-Schlüsseltresor. Sie weisen dieser Identität eine aus mehreren Schritten bestehende Aufgabe zu, die das Geheimnis liest, ein Image erstellt und sich bei der Azure CLI anmeldet, um das Imagetag zu lesen.

### <a name="create-a-key-vault-and-store-a-secret"></a>Erstellen eines Schlüsseltresors und Speichern eines Geheimnisses

Erstellen Sie zunächst – falls erforderlich – mit dem folgenden Befehl [az group create][az-group-create] eine Ressourcengruppe namens *myResourceGroup* am Speicherort *eastus*:

```azurecli-interactive
az group create --name myResourceGroup --location eastus
```

Verwenden Sie den Befehl [az keyvault create][az-keyvault-create], um eine Key Vault-Instanz zu erstellen. Geben Sie hierbei einen eindeutigen Namen für die Key Vault-Instanz an. 

```azurecli-interactive
az keyvault create --name mykeyvault --resource-group myResourceGroup --location eastus
```

Speichern Sie mithilfe des Befehls [az keyvault secret set][az-keyvault-secret-set] ein Beispielgeheimnis in der Key Vault-Instanz:

```azurecli
az keyvault secret set \
  --name SampleSecret \
  --value "Hello ACR Tasks!" \
  --description ACRTasksecret  \
  --vault-name mykeyvault
```

Beispielsweise könnten Sie Anmeldeinformationen speichern, um sich bei einer privaten Docker-Registrierung zu authentifizieren, damit Sie ein privates Image pullen können.

### <a name="create-an-identity"></a>Erstellen einer Identität

Erstellen Sie mit dem Befehl [az identity create][az-identity-create] eine Identität namens *myACRTasksId* in Ihrem Abonnement. Sie können entweder dieselbe Ressourcengruppe verwenden, die Sie zuvor zum Erstellen einer Containerregistrierung oder eines Schlüsseltresors verwendet haben, oder Sie verwenden eine andere Ressourcengruppe.

```azurecli-interactive
az identity create --resource-group myResourceGroup --name myACRTasksId
```

Um die Identität in den folgenden Schritten zu konfigurieren, verwenden Sie den Befehl [az identity show][az-identity-show], um die Ressourcen-ID und die Dienstprinzipal-ID der Identität in Variablen zu speichern.

```azurecli
# Get resource ID of the user-assigned identity
resourceID=$(az identity show --resource-group myResourceGroup --name myACRTasksId --query id --output tsv)

# Get service principal ID of the user-assigned identity
principalID=$(az identity show --resource-group myResourceGroup --name myACRTasksId --query principalId --output tsv)
```

### <a name="grant-identity-access-to-keyvault-to-read-secret"></a>Gewähren des Zugriffs auf den Schlüsseltresor für die Identität, um ein Geheimnis zu lesen

Führen Sie den folgenden Befehl [az keyvault set-policy][az-keyvault-set-policy] aus, um eine Zugriffsrichtlinie für den Schlüsseltresor festzulegen. Das folgende Beispiel ermöglicht der vom Benutzer zugewiesenen Identität das Abrufen von Geheimnissen aus dem Schlüsseltresor. Dieser Zugriff wird später benötigt, um eine Aufgabe mit mehreren Schritten erfolgreich auszuführen.

```azurecli-interactive
 az keyvault set-policy --name mykeyvault --resource-group myResourceGroup --object-id $principalID --secret-permissions get
```

### <a name="grant-identity-reader-access-to-the-resource-group-for-registry"></a>Gewähren des Leserzugriffs auf die Ressourcengruppe für die Registrierung für die Identität

Führen Sie den folgenden Befehl [az role assignment create][az-role-assignment-create] aus, um der Identität eine Leserrolle zuzuweisen, in diesem Fall für die Ressourcengruppe, die die Quellregistrierung enthält. Diese Rolle wird später benötigt, um eine Aufgabe mit mehreren Schritten erfolgreich auszuführen.

```azurecli
az role assignment create --role reader --resource-group myResourceGroup --assignee $principalID
```

### <a name="create-task-with-user-assigned-identity"></a>Erstellen einer Aufgabe mit benutzerseitig zugewiesener Identität

Erstellen Sie jetzt eine [Aufgabe mit mehreren Schritten](container-registry-tasks-multi-step.md), und weisen sie ihr die vom Benutzer zugewiesene Identität zu. Erstellen Sie für diese Beispielaufgabe eine [YAML-Datei](container-registry-tasks-reference-yaml.md) namens `managed-identities.yaml` in einem lokalen Arbeitsverzeichnis, und fügen Sie den folgenden Inhalt ein. Achten Sie darauf, dass Sie den Namen des Schlüsseltresors in der Datei durch den Namen Ihres Schlüsseltresors ersetzen.

```yml
version: v1.0.0
# Replace mykeyvault with the name of your key vault
secrets:
  - id: name
    keyvault: https://mykeyvault.vault.azure.net/secrets/SampleSecret
steps:
  # Verify that the task can access the secret in the key vault
  - cmd: bash -c 'if [ -z "$MY_SECRET" ]; then echo "Secret not resolved"; else echo "Secret resolved"; fi'
    env: 
      - MY_SECRET='{{.Secrets.name}}' 

  # Build/push the website image to source registry, using dockerfile in the Azure-Samples repo
  - cmd: docker build -t {{.Run.Registry}}/my-website:{{.Run.ID}} https://github.com/Azure-Samples/aci-helloworld.git
  - push: 
    - "{{.Run.Registry}}/my-website:{{.Run.ID}}"
  
  # Login to Azure CLI with identity and list the tags to verify that the image is in the registry
  - cmd: microsoft/azure-cli az login --identity
  - cmd: microsoft/azure-cli az acr repository show-tags -n {{.Values.registryName}} --repository my-website
```

Diese Aufgabe führt Folgendes aus:

* Sicherstellen, dass sie auf das Geheimnis in Ihrem Schlüsseltresor zugreifen kann. Dieser Schritt dient zu Demonstrationszwecken. In einem realen Szenario könnten Sie einen Aufgabenschritt zum Abrufen der Anmeldeinformationen für den Zugriff auf ein privates Docker Hub-Repository benötigen.
* Erstellen des `mywebsite`-Images und Pushen in die Quellregistrierung.
* Anmelden bei der Azure-Befehlszeilenschnittstelle zum Auflisten der `my-website`-Imagetags in der Quellregistrierung.

Erstellen einer Aufgabe namens *msitask* und Übergeben an die Ressourcen-ID der zuvor erstellten vom Benutzer zugewiesenen Identität. Diese Beispielaufgabe wird aus der Datei `managed-identities.yaml` erstellt, die Sie in Ihrem lokalen Arbeitsverzeichnis gespeichert haben, sodass Sie sie manuell auslösen müssen.

```azurecli
az acr task create \
  --name msitask \
  --registry myregistry \
  --context /dev/null \
  --file managed-identities.yaml \
  --pull-request-trigger-enabled false \
  --commit-trigger-enabled false \
  --assign-identity $resourceID
```

In der Befehlsausgabe zeigt der Abschnitt `identity` an, dass eine Identität vom Typ `UserAssigned` in der Aufgabe festgelegt ist. Die `principalId` sollte die Dienstprinzipal-ID der Identität sein:

```console
[...]
"identity": {
    "principalId": null,
    "tenantId": null,
    "type": "UserAssigned",
    "userAssignedIdentities": {
      "/subscriptions/xxxxxxxx-d12e-4760-9ab6-xxxxxxxxxxxx/resourcegroups/myResourceGroup/providers/Microsoft.ManagedIdentity/userAssignedIdentities/myACRTasksId": {
        "clientId": "xxxxxxxx-f17e-4768-bb4e-xxxxxxxxxxxx",
        "principalId": "xxxxxxxx-1335-433d-bb6c-xxxxxxxxxxxx"
      }
[...]
```

### <a name="manually-run-the-task"></a>Manuelles Ausführen der Aufgabe

Lösen Sie die Aufgabe mit dem Befehl [az acr task run][az-acr-task-run] manuell aus. Der `--set`-Parameter wird verwendet, um den Namen der Quellregistrierung an die Aufgabe zu übergeben:

```azurecli
az acr task run --name msitask --registry myregistry --set registryName=myregistry  
```

Die Ausgabe zeigt, dass das Geheimnis aufgelöst, das Image erfolgreich erstellt und gepusht wurde und die Aufgabe sich mit der Identität bei Azure CLI anmeldet, um das Imagetag aus der Quellregistrierung zu lesen:

```console
Queued a run with ID: cf32
Waiting for an agent...
2019/06/10 23:25:37 Downloading source code...
2019/06/10 23:25:38 Finished downloading source code
2019/06/10 23:25:39 Using acb_vol_4e4a0a7c-b6ef-4ec5-b40f-3436fc5eb0f5 as the home volume
2019/06/10 23:25:41 Creating Docker network: acb_default_network, driver: 'bridge'
2019/06/10 23:25:41 Successfully set up Docker network: acb_default_network
2019/06/10 23:25:41 Setting up Docker configuration...
2019/06/10 23:25:42 Successfully set up Docker configuration
2019/06/10 23:25:42 Logging in to registry: myregistry.azurecr.io
2019/06/10 23:25:43 Successfully logged into myregistry.azurecr.io
2019/06/10 23:25:43 Executing step ID: acb_step_0. Timeout(sec): 600, Working directory: '', Network: 'acb_default_network'
2019/06/10 23:25:43 Launching container with name: acb_step_0
Secret resolved
2019/06/10 23:25:44 Successfully executed container: acb_step_0
2019/06/10 23:25:44 Executing step ID: acb_step_1. Timeout(sec): 600, Working directory: '', Network: 'acb_default_network'
2019/06/10 23:25:44 Launching container with name: acb_step_1
Sending build context to Docker daemon  74.75kB

[...]

cf32: digest: sha256:cbb4aa83b33f6959d83e84bfd43ca901084966a9f91c42f111766473dc977f36 size: 1577
2019/06/10 23:26:05 Successfully pushed image: myregistry.azurecr.io/my-website:cf32
2019/06/10 23:26:05 Executing step ID: acb_step_3. Timeout(sec): 600, Working directory: '', Network: 'acb_default_network'
2019/06/10 23:26:05 Launching container with name: acb_step_3
[
  {
    "environmentName": "AzureCloud",
    "id": "xxxxxxxx-d12e-4760-9ab6-xxxxxxxxxxxx",
    "isDefault": true,
    "name": "DanLep Internal Consumption",
    "state": "Enabled",
    "tenantId": "xxxxxxxx-86f1-41af-91ab-xxxxxxxxxxxx",
      "user": {
      "assignedIdentityInfo": "MSI",
      "name": "systemAssignedIdentity",
      "type": "servicePrincipal"
    }
  }
]
2019/06/10 23:26:09 Successfully executed container: acb_step_3
2019/06/10 23:26:09 Executing step ID: acb_step_4. Timeout(sec): 600, Working directory: '', Network: 'acb_default_network'
2019/06/10 23:26:09 Launching container with name: acb_step_4
[
  "cf32"
]
2019/06/10 23:26:14 Successfully executed container: acb_step_4
2019/06/10 23:26:14 Step ID: acb_step_0 marked as successful (elapsed time in seconds: 1.025312)
2019/06/10 23:26:14 Step ID: acb_step_1 marked as successful (elapsed time in seconds: 13.703823)
2019/06/10 23:26:14 Step ID: acb_step_2 marked as successful (elapsed time in seconds: 6.791506)
2019/06/10 23:26:14 Step ID: acb_step_3 marked as successful (elapsed time in seconds: 3.852972)
2019/06/10 23:26:14 Step ID: acb_step_4 marked as successful (elapsed time in seconds: 5.079079)
```


## <a name="next-steps"></a>Nächste Schritte

In diesem Artikel haben Sie mehr über die Verwendung verwalteter Identitäten mit Azure Container Registry Tasks erfahren und Folgendes gelernt:

> [!div class="checklist"]
> * Aktivieren einer systemseitig oder benutzerseitig zugewiesenen Identität auf einer ACR-Aufgabe
> * Gewähren des Identitätszugriffs auf Azure-Ressourcen wie andere Azure-Containerregistrierungen
> * Verwenden der verwalteten Identität für den Zugriff auf die Ressourcen aus einer Aufgabe  

* Erfahren Sie mehr über [verwaltete Identitäten für Azure-Ressourcen](/azure/active-directory/managed-identities-azure-resources/).

<!-- LINKS - Internal -->
[az-login]: /cli/azure/reference-index#az-login
[az-acr-login]: /cli/azure/acr#az-acr-login
[az-acr-show]: /cli/azure/acr#az-acr-show
[az-role-assignment-create]: /cli/azure/role/assignment#az-role-assignment-create
[az-acr-login]: /cli/azure/acr#az-acr-login
[az-identity-create]: /cli/azure/identity?view=azure-cli-latest#az-identity-create
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
