---
title: Zurücksetzen der Anmeldeinformationen eines Azure Kubernetes Service-Clusters (AKS)
description: Erfahren Sie, wie Sie die Dienstprinzipal-Anmeldeinformationen für einen Cluster in Azure Kubernetes Service (AKS) aktualisieren oder zurücksetzen.
services: container-service
author: mlearned
ms.service: container-service
ms.topic: article
ms.date: 05/31/2019
ms.author: mlearned
ms.openlocfilehash: bda0ab50b829fa2e6d58e73b51e3a0a0f6c9e2af
ms.sourcegitcommit: 77bfc067c8cdc856f0ee4bfde9f84437c73a6141
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 10/16/2019
ms.locfileid: "72432918"
---
# <a name="update-or-rotate-the-credentials-for-a-service-principal-in-azure-kubernetes-service-aks"></a>Aktualisieren oder Rotieren der Anmeldeinformationen für einen Dienstprinzipal in Azure Kubernetes Service (AKS)

Standardmäßig werden AKS-Cluster mit einem Dienstprinzipal mit einer Ablaufzeit von einem Jahr erstellt. Wenn Sie sich dem Ablaufdatum nähern, können Sie die Anmeldeinformationen zurücksetzen, um den Dienstprinzipal um einen zusätzlichen Zeitraum zu verlängern. Sie können die Anmeldeinformationen auch im Rahmen einer definierten Sicherheitsrichtlinie aktualisieren oder rotieren. Dieser Artikel beschreibt, wie diese Anmeldeinformationen für einen AKS-Cluster aktualisiert werden.

## <a name="before-you-begin"></a>Voraussetzungen

Azure CLI-Version 2.0.65 oder höher muss installiert und konfiguriert sein. Führen Sie  `az --version` aus, um die Version zu ermitteln. Wenn Sie eine Installation oder ein Upgrade ausführen müssen, finden Sie weitere Informationen unter  [Installieren der Azure CLI][install-azure-cli].

## <a name="choose-to-update-or-create-a-service-principal"></a>Entscheidung zur Aktualisierung oder Erstellung eines Dienstprinzipals

Wenn Sie die Anmeldeinformationen für einen AKS-Cluster aktualisieren möchten, haben Sie folgende Möglichkeiten:

* Aktualisieren der Anmeldeinformationen für den vorhandenen, vom Cluster verwendeten Dienstprinzipal ODER
* Erstellen eines Dienstprinzipals und Aktualisieren des Clusters zur Verwendung der neuen Anmeldeinformationen

### <a name="update-existing-service-principal-expiration"></a>Aktualisieren eines vorhandenen Dienstprinzipals vor Ablauf

Um die Anmeldeinformationen für den vorhandenen Dienstprinzipal zu aktualisieren, rufen Sie über den Befehl [az aks show][az-aks-show] die Dienstprinzipal-ID für Ihren Cluster ab. Im folgenden Beispiel wird die ID für den Cluster *myAKSCluster* in der Ressourcengruppe *myResourceGroup* abgerufen. Die Dienstprinzipal-ID wird als Variable mit dem Namen *SP_ID* zur Verwendung in einem zusätzlichen Befehl festgelegt.

```azurecli-interactive
SP_ID=$(az aks show --resource-group myResourceGroup --name myAKSCluster \
    --query servicePrincipalProfile.clientId -o tsv)
```

Mit einer Variablen, die die Dienstprinzipal-ID enthält, können Sie nun die Anmeldeinformationen über den Befehl [az ad sp credential reset][az-ad-sp-credential-reset] zurücksetzen. Im folgenden Beispiel wird über die Azure-Plattform ein neues sicheres Geheimnis für den Dienstprinzipal erstellt. Dieses neue sichere Geheimnis wird ebenfalls als Variable gespeichert.

```azurecli-interactive
SP_SECRET=$(az ad sp credential reset --name $SP_ID --query password -o tsv)
```

Fahren Sie jetzt mit dem Abschnitt [Aktualisieren des AKS-Clusters mit den neuen Anmeldeinformationen](#update-aks-cluster-with-new-credentials) fort. Dieser Schritt ist notwendig, damit die Änderungen am Dienstprinzipal vom AKS-Cluster übernommen werden können.

### <a name="create-a-new-service-principal"></a>Erstellen eines neuen Dienstprinzipals

Wenn Sie im vorherigen Abschnitt die vorhandenen Anmeldeinformationen für den vorhandenen Dienstprinzipal aktualisiert haben, überspringen Sie diesen Schritt. Fahren Sie mit dem Abschnitt [Aktualisieren des AKS-Clusters mit den neuen Anmeldeinformationen](#update-aks-cluster-with-new-credentials) fort.

Um einen Dienstprinzipal zu erstellen und anschließend den AKS-Cluster für die Verwendung dieser neuen Anmeldeinformationen zu aktualisieren, verwenden Sie den Befehl [az ad sp create-for-rbac][az-ad-sp-create]. Im folgenden Beispiel verhindert der `--skip-assignment`-Parameter, dass zusätzliche Standardzuweisungen durchgeführt werden:

```azurecli-interactive
az ad sp create-for-rbac --skip-assignment
```

Die Ausgabe sieht in etwa wie das folgende Beispiel aus: Notieren Sie sich Ihre persönlichen Angaben für `appId` und `password`. Diese Werte werden im nächsten Schritt benötigt.

```json
{
  "appId": "7d837646-b1f3-443d-874c-fd83c7c739c5",
  "name": "7d837646-b1f3-443d-874c-fd83c7c739c",
  "password": "a5ce83c9-9186-426d-9183-614597c7f2f7",
  "tenant": "a4342dc8-cd0e-4742-a467-3129c469d0e5"
}
```

Definieren Sie nun Variablen für die Dienstprinzipal-ID und das Clientgeheimnis unter Verwendung der Ausgabe Ihres eigenen Befehls [az ad sp create-for-rbac][az-ad-sp-create], wie im folgenden Beispiel gezeigt. Der Wert *SP_ID* steht für Ihre *App-ID*, der Wert *SP_SECRET* gibt Ihr *Kennwort* an:

```azurecli-interactive
SP_ID=7d837646-b1f3-443d-874c-fd83c7c739c5
SP_SECRET=a5ce83c9-9186-426d-9183-614597c7f2f7
```

Fahren Sie jetzt mit dem Abschnitt [Aktualisieren des AKS-Clusters mit den neuen Anmeldeinformationen](#update-aks-cluster-with-new-credentials) fort. Dieser Schritt ist notwendig, damit die Änderungen am Dienstprinzipal vom AKS-Cluster übernommen werden können.

## <a name="update-aks-cluster-with-new-credentials"></a>Aktualisieren des AKS-Clusters mit den neuen Anmeldeinformationen

Aktualisieren Sie nun unabhängig davon, ob Sie die Anmeldeinformationen für den vorhandenen Dienstprinzipal aktualisieren oder einen Dienstprinzipal erstellen, den AKS-Cluster mit Ihren neuen Anmeldeinformationen. Verwenden Sie dazu den Befehl[az aks update-credentials][az-aks-update-credentials]. Es werden die Variablen für *--service-principal* und *--client-secret* verwendet:

```azurecli-interactive
az aks update-credentials \
    --resource-group myResourceGroup \
    --name myAKSCluster \
    --reset-service-principal \
    --service-principal $SP_ID \
    --client-secret $SP_SECRET
```

Es dauert einige Augenblicke, bis die Anmeldeinformationen für den Dienstprinzipal in AKS aktualisiert werden.

## <a name="next-steps"></a>Nächste Schritte

In diesem Artikel wurde der Dienstprinzipal für den AKS-Cluster aktualisiert. Weitere Informationen zum Verwalten der Identität für Workloads in einem Cluster finden Sie unter [Best Practices für die Authentifizierung und Autorisierung in Azure Kubernetes Service (AKS)][best-practices-identity].

<!-- LINKS - internal -->
[install-azure-cli]: /cli/azure/install-azure-cli
[az-aks-show]: /cli/azure/aks#az-aks-show
[az-aks-update-credentials]: /cli/azure/aks#az-aks-update-credentials
[best-practices-identity]: operator-best-practices-identity.md
[az-ad-sp-create]: /cli/azure/ad/sp#az-ad-sp-create-for-rbac
[az-ad-sp-credential-reset]: /cli/azure/ad/sp/credential#az-ad-sp-credential-reset
