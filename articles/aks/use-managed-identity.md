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
ms.openlocfilehash: a5717d8ee44e4d2e086a6e7bc1b7c3d0deb614c8
ms.sourcegitcommit: 7c2dba9bd9ef700b1ea4799260f0ad7ee919ff3b
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 10/02/2019
ms.locfileid: "71827544"
---
# <a name="preview---use-managed-identities-in-azure-kubernetes-service"></a>Vorschau – Verwenden verwalteter Identitäten in Azure Kubernetes Service

Derzeit müssen Benutzer einen Dienstprinzipal bereitstellen, oder AKS erstellt einen Dienstprinzipal in Ihrem Auftrag, damit der AKS-Cluster (insbesondere der Kubernetes-Cloudanbieter) zusätzliche Ressourcen wie Load Balancer und verwaltete Datenträger in Azure erstellen kann. Dienstprinzipale werden in der Regel mit einem Ablaufdatum erstellt. Cluster erreichen letztendlich einen Zustand, in dem der Dienstprinzipal erneuert werden muss, da der Cluster andernfalls nicht mehr funktioniert. Die Verwaltung von Dienstprinzipalen erhöht die Komplexität. Verwaltete Identitäten stellen im Wesentlichen einen Wrapper um Dienstprinzipale dar und vereinfachen deren Verwaltung. Weitere Informationen zu [verwalteten Identitäten](https://docs.microsoft.com/azure/active-directory/managed-identities-azure-resources/overview).

AKS erstellt zwei verwaltete Identitäten, eine vom System zugewiesene verwaltete Identität und eine andere vom Benutzer zugewiesene Identität. Eine vom System zugewiesene verwaltete Identität wird vom Kubernetes-Cloudanbieter zum Erstellen von Azure-Ressourcen im Auftrag des Benutzers verwendet. Der Lebenszyklus dieser vom System zugewiesenen verwalteten Identität ist an den des Clusters gebunden und wird gelöscht, wenn der Cluster gelöscht wird. AKS erstellt außerdem eine vom Benutzer zugewiesene verwaltete Identität, die im Cluster zum Autorisieren von AKS für den Zugriff auf ACRs, Kubelet zum Abrufen von Metadaten aus Azure usw. verwendet wird.

Während dieses Vorschauzeitraums ist ein Dienstprinzipal immer noch erforderlich. Er wird für die Autorisierung von Add-Ons verwendet wie Überwachung, virtueller Knoten, Azure-Richtlinie und HTTP-Anwendungsrouting. Es wird noch daran gearbeitet, die Abhängigkeit der Add-Ons von dem Dienstprinzipalnamen zu entfernen, und in letzter Konsequenz werden SPN-Anforderungen in AKS vollständig entfernt.

> [!IMPORTANT]
> AKS-Previewfunktionen stehen gemäß dem Self-Service- und Aktivierungsprinzip zur Verfügung. Vorschauversionen werden „wie besehen“ und „wie verfügbar“ bereitgestellt und sind von den Vereinbarungen zum Service Level und der eingeschränkten Garantie ausgeschlossen. AKS-Vorschauen werden teilweise vom Kundensupport auf der Grundlage der bestmöglichen Leistung abgedeckt. Daher sind diese Funktionen nicht für die Verwendung in der Produktion vorgesehen. Weitere Informationen finden Sie in den folgenden Supportartikeln:
>
> * [Unterstützungsrichtlinien für Azure Kubernetes Service](support-policies.md)
> * [Häufig gestellte Fragen zum Azure-Support](faq.md)

## <a name="before-you-begin"></a>Voraussetzungen

Sie benötigen Folgendes:

* Außerdem benötigen Sie die Azure CLI-Version 2.0.70 oder höher sowie die Erweiterung „aks-preview 0.4.14“.

## <a name="install-latest-aks-cli-preview-extension"></a>Installieren der neuesten AKS-CLI-Vorschauerweiterung

Sie benötigen die Erweiterung **aks-preview 0.4.14** oder höher.

```azurecli
az extension update --name aks-preview 
az extension list
```

> [!CAUTION]
> Wenn Sie ein Feature für ein Abonnement registrieren, können Sie die Registrierung dieses Features momentan nicht mehr aufheben. Nachdem Sie einige Vorschaufeatures aktiviert haben, können Standardwerte für alle AKS-Cluster verwendet werden, die dann im Abonnement erstellt werden. Aktivieren Sie keine Vorschaufeatures für Produktionsabonnements. Verwenden Sie ein separates Abonnement, um Vorschaufeatures zu testen und Feedback zu sammeln.

```azurecli-interactive
az feature register --name MSIPreview --namespace Microsoft.ContainerService
```

Es kann einige Minuten dauern, bis der Status *Registriert* angezeigt wird. Sie können den Registrierungsstatus mithilfe des Befehls „[az feature list][az-feature-list]“ überprüfen:

```azurecli-interactive
az feature list -o table --query "[?contains(name, 'Microsoft.ContainerService/MSIPreview')].{Name:name,State:properties.state}"
```

Wenn der Zustand registriert ist, aktualisieren Sie die Registrierung des *Microsoft.ContainerService*-Ressourcenanbieters mit dem Befehl „[az provider register][az-provider-register]“:

```azurecli-interactive
az provider register --namespace Microsoft.ContainerService
```

## <a name="create-an-aks-cluster-with-managed-identity"></a>Erstellen eines AKS-Clusters mit verwalteter Identität

Sie können jetzt einen AKS-Cluster mit verwalteten Identitäten erstellen, indem Sie den folgenden CLI-Befehl verwenden.
```azurecli-interactive
# Create an Azure resource group
az group create --name myResourceGroup --location westus2
```

## <a name="create-an-aks-cluster"></a>Erstellen eines AKS-Clusters
```azurecli-interactive
az aks create -g MyResourceGroup -n MyManagedCluster --enable-managed-identity
```

## <a name="get-credentials-to-access-the-cluster"></a>Abrufen von Anmeldeinformationen für den Zugriff auf den Cluster
```azurecli-interactive
az aks get-credentials --resource-group myResourceGroup --name MyManagedCluster
```
Nachdem der Cluster in ein paar Minuten erstellt wurde, können Sie Ihre Anwendungsworkloads bereitstellen und damit interagieren, wie Sie dies mit dienstprinzipalbasierten AKS-Clustern getan haben. 

> [!IMPORTANT]
> * AKS-Cluster mit verwalteten Identitäten können nur während der Erstellung des Clusters aktiviert werden.
> * Um verwaltete Identitäten zu aktivieren, können vorhandene AKS-Cluster weder aktualisiert noch ein Upgrade für sie ausgeführt werden.