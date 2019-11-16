---
title: Verwenden verwalteter Identitäten in Azure Kubernetes Service
description: Erfahren Sie, wie Sie verwaltete Identitäten in Azure Kubernetes Service (AKS) verwenden.
services: container-service
author: saudas
manager: saudas
ms.service: container-service
ms.topic: article
ms.date: 09/11/2019
ms.author: saudas
ms.openlocfilehash: 16a1f3b8f9ef036bcdc9af122243d313186f99f1
ms.sourcegitcommit: cf36df8406d94c7b7b78a3aabc8c0b163226e1bc
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 11/09/2019
ms.locfileid: "73885333"
---
# <a name="preview---use-managed-identities-in-azure-kubernetes-service"></a>Vorschau – Verwenden verwalteter Identitäten in Azure Kubernetes Service

Derzeit erfordert ein AKS-Cluster (Azure Kubernetes Service) und insbesondere der Kubernetes-Cloudanbieter einen *Dienstprinzipal*, um zusätzliche Ressourcen wie Lastenausgleich und verwaltete Datenträger in Azure zu erstellen. Entweder müssen Sie einen Dienstprinzipal bereitstellen, oder AKS erstellt einen Dienstprinzipal in Ihrem Namen. Dienstprinzipale haben in der Regel ein Ablaufdatum. Cluster erreichen letztendlich einen Zustand, in dem der Dienstprinzipal erneuert werden muss, damit der Cluster ordnungsgemäß funktioniert. Die Verwaltung von Dienstprinzipalen erhöht die Komplexität.

*Verwaltete Identitäten* stellen im Wesentlichen einen Wrapper um Dienstprinzipale dar und vereinfachen deren Verwaltung. Weitere Informationen finden Sie im Artikel über [verwaltete Identitäten für Azure-Ressourcen](https://docs.microsoft.com/azure/active-directory/managed-identities-azure-resources/overview).

AKS erstellt zwei verwaltete Identitäten:

- **Systemseitig zugewiesene verwaltete Identität**: die Identität, die der Kubernetes-Cloudanbieter zum Erstellen von Azure-Ressourcen im Auftrag des Benutzers verwendet. Der Lebenszyklus der systemseitig zugewiesenen Identität ist an den des Clusters gebunden. Die Identität wird gelöscht, wenn der Cluster gelöscht wird.
- **Benutzerseitig zugewiesene verwaltete Identität**: die Identität, die für die Autorisierung im Cluster verwendet wird. Die benutzerseitig zugewiesene Identität wird beispielsweise verwendet, um AKS zum Verwenden von Access Control-Datensätzen (Access Control Records, ACRs) oder das Kubelet zum Abrufen von Metadaten aus Azure zu autorisieren.

Während dieses Vorschauzeitraums ist ein Dienstprinzipal immer noch erforderlich. Er wird für die Autorisierung von Add-Ons wie Überwachung, virtuelle Knoten, Azure Policy und HTTP-Anwendungsrouting verwendet. Die Arbeit wird ausgeführt, um die Abhängigkeit von Add-Ons für den Dienstprinzipalnamen (Service Principal Name, SPN) zu entfernen. Schließlich wird die Anforderung eines SPN in AKS vollständig entfernt.

> [!IMPORTANT]
> AKS-Previewfunktionen stehen gemäß dem Self-Service- und Aktivierungsprinzip zur Verfügung. Vorschauversionen werden „wie besehen“ und „wie verfügbar“ bereitgestellt und sind von den Vereinbarungen zum Service Level und der eingeschränkten Garantie ausgeschlossen. AKS-Vorschauversionen werden teilweise vom Kundensupport auf der Grundlage der bestmöglichen Leistung abgedeckt. Daher sind diese Funktionen nicht für die Verwendung in der Produktion vorgesehen. Weitere Informationen finden Sie in den folgenden Supportartikeln:
>
> - [Unterstützungsrichtlinien für Azure Kubernetes Service](support-policies.md)
> - [Häufig gestellte Fragen zum Azure-Support](faq.md)

## <a name="before-you-begin"></a>Voraussetzungen

Die folgenden Ressourcen müssen installiert sein:

- Die Azure CLI, Version 2.0.70 oder höher
- Die aks-preview 0.4.14-Erweiterung

Verwenden Sie zum Installieren der aks-preview 0.4.14-Erweiterung oder höher die folgenden Azure CLI-Befehle:

```azurecli
az extension update --name aks-preview
az extension list
```

> [!CAUTION]
> Nachdem Sie ein Feature für ein Abonnement registriert haben, können Sie die Registrierung dieses Features momentan nicht mehr aufheben. Wenn Sie einige Previewfunktionen aktivieren, können Standardwerte für alle AKS-Cluster verwendet werden, die anschließend im Abonnement erstellt werden. Aktivieren Sie keine Vorschaufeatures für Produktionsabonnements. Verwenden Sie stattdessen ein separates Abonnement, um Previewfunktionen zu testen und Feedback zu erfassen.

```azurecli-interactive
az feature register --name MSIPreview --namespace Microsoft.ContainerService
```

Es kann einige Minuten dauern, bis der Status als **Registriert** angezeigt wird. Sie können den Registrierungsstatus mithilfe des Befehls [az feature list](https://docs.microsoft.com/cli/azure/feature?view=azure-cli-latest#az-feature-list) überprüfen:

```azurecli-interactive
az feature list -o table --query "[?contains(name, 'Microsoft.ContainerService/MSIPreview')].{Name:name,State:properties.state}"
```

Wenn der Status als registriert angezeigt wird, können Sie die Registrierung des `Microsoft.ContainerService`-Ressourcenanbieters mit dem Befehl [az provider register](https://docs.microsoft.com/cli/azure/provider?view=azure-cli-latest#az-provider-register) aktualisieren:

```azurecli-interactive
az provider register --namespace Microsoft.ContainerService
```

## <a name="create-an-aks-cluster-with-managed-identities"></a>Erstellen eines AKS-Clusters mit verwalteten Identitäten

Sie können jetzt einen AKS-Cluster mit verwalteten Identitäten erstellen, indem Sie die folgenden CLI-Befehle verwenden.

Erstellen Sie als Erstes eine Azure-Ressourcengruppe:

```azurecli-interactive
# Create an Azure resource group
az group create --name myResourceGroup --location westus2
```

Erstellen Sie anschließend einen AKS-Cluster:

```azurecli-interactive
az aks create -g MyResourceGroup -n MyManagedCluster --enable-managed-identity
```

Rufen Sie schließlich Anmeldeinformationen für den Zugriff auf den Cluster ab:

```azurecli-interactive
az aks get-credentials --resource-group myResourceGroup --name MyManagedCluster
```

Der Cluster wird in wenigen Minuten erstellt. Sie können Ihre Anwendungsworkloads im neuen Cluster bereitstellen und damit interagieren, wie Sie dies mit dienstprinzipalbasierten AKS-Clustern getan haben.

> [!IMPORTANT]
>
> - AKS-Cluster mit verwalteten Identitäten können nur während der Erstellung des Clusters aktiviert werden.
> - Es ist nicht möglich, AKS-Cluster zu aktualisieren oder ein Upgrade für sie durchzuführen, um verwaltete Identitäten zu aktivieren.
