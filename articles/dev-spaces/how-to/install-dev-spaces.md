---
title: Installieren von Azure Dev Spaces unter AKS und der clientseitigen Tools
titleSuffix: Azure Dev Spaces
services: azure-dev-spaces
ms.service: azure-dev-spaces
author: zr-msft
ms.author: zarhoads
ms.date: 07/24/2019
ms.topic: conceptual
description: Es wird beschrieben, wie Sie Azure Dev Spaces in einem AKS-Cluster und die clientseitigen Tools installieren.
keywords: Docker, Kubernetes, Azure, AKS, Azure Kubernetes Service, Container, Helm, Service Mesh, Service Mesh-Routing, kubectl, k8s
ms.openlocfilehash: 642d4406840f1748c3b6cbb4441227fff0c6d721
ms.sourcegitcommit: 5604661655840c428045eb837fb8704dca811da0
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 07/25/2019
ms.locfileid: "68494848"
---
# <a name="install-azure-dev-spaces-on-aks-and-the-client-side-tooling"></a>Installieren von Azure Dev Spaces unter AKS und der clientseitigen Tools

In diesem Artikel werden verschiedene Möglichkeiten zur Installation von Azure Dev Spaces in einem AKS-Cluster und von clientseitigen Tools beschrieben.

## <a name="install-azure-dev-spaces-using-the-cli"></a>Installieren von Azure Dev Spaces mit der CLI

Bevor Sie Dev Spaces mit der CLI installieren können, benötigen Sie Folgendes:
* Ein Azure-Abonnement. Falls Sie über kein Azure-Abonnement verfügen, können Sie ein [kostenloses Konto][az-portal-create-account] erstellen.
* [Eine Installation der Azure CLI][install-cli].
* [Einen AKS-Cluster][create-aks-cli] in einer [unterstützten Region][supported-regions].

Verwenden Sie den Befehl `use-dev-spaces`, um Dev Spaces in Ihrem AKS-Cluster zu aktivieren, und befolgen Sie die angezeigten Anweisungen.

```cmd
az aks use-dev-spaces -g myResourceGroup -n myAKSCluster
```

Mit dem obigen Befehl wird Dev Spaces im Cluster *myAKSCluster* in der Gruppe *myResourceGroup* aktiviert und der Entwicklerbereich *default* erstellt.

```cmd
$ az aks use-dev-spaces -g myResourceGroup -n myAKSCluster

'An Azure Dev Spaces Controller' will be created that targets resource 'myAKSCluster' in resource group 'myResourceGroup'. Continue? (y/N): y

Creating and selecting Azure Dev Spaces Controller 'myAKSCluster' in resource group 'myResourceGroup' that targets resource 'myAKSCluster' in resource group 'myResourceGroup'...2m 24s

Select a dev space or Kubernetes namespace to use as a dev space.
 [1] default
Type a number or a new name: 1

Kubernetes namespace 'default' will be configured as a dev space. This will enable Azure Dev Spaces instrumentation for new workloads in the namespace. Continue? (Y/n): Y

Configuring and selecting dev space 'default'...3s

Managed Kubernetes cluster 'myAKSCluster' in resource group 'myResourceGroup' is ready for development in dev space 'default'. Type `azds prep` to prepare a source directory for use with Azure Dev Spaces and `azds up` to run.
```

Mit dem Befehl `use-dev-spaces` wird auch die Azure Dev Spaces CLI installiert.

## <a name="install-azure-dev-spaces-using-the-azure-portal"></a>Installieren von Azure Dev Spaces über das Azure-Portal

Bevor Sie Dev Spaces über das Azure-Portal installieren können, benötigen Sie Folgendes:
* Ein Azure-Abonnement. Falls Sie über kein Azure-Abonnement verfügen, können Sie ein [kostenloses Konto][az-portal-create-account] erstellen.
* [Einen AKS-Cluster][create-aks-portal] in einer [unterstützten Region][supported-regions].

Installieren Sie Azure Dev Spaces wie folgt über das Azure-Portal:
1. Melden Sie sich beim [Azure-Portal][az-portal] an.
1. Navigieren Sie zu Ihrem AKS-Cluster.
1. Klicken Sie auf *Dev Spaces*.
1. Ändern Sie *Azure Dev Spaces aktivieren* in *Ja*, und klicken Sie auf *Speichern*.

![Aktivieren von Dev Spaces im Azure-Portal](../media/how-to-setup-dev-spaces/enable-dev-spaces-portal.png)

Bei der Installation von Azure Dev Spaces über das Azure-Portal werden **keine** clientseitigen Tools für Azure Dev Spaces installiert.

## <a name="install-the-client-side-tooling"></a>Installieren der clientseitigen Tools

Sie können die clientseitigen Tools für Azure Dev Spaces verwenden, um mit Dev Spaces über einen AKS-Cluster auf Ihrem lokalen Computer zu interagieren. Es gibt mehrere Möglichkeiten, die clientseitigen Tools zu installieren:

* Installieren Sie in [Visual Studio Code][vscode] die [Azure Dev Spaces-Erweiterung][vscode-extension].
* Installieren Sie in [Visual Studio 2019][visual-studio] die Workload „Azure-Entwicklung“.
* Installieren Sie für Visual Studio 2017 die Workload „Webentwicklung“ sowie die [Visual Studio-Tools für Kubernetes][visual-studio-k8s-tools].
* Laden Sie die [Windows][cli-win]-, [Mac][cli-mac]- oder [Linux][cli-linux]-CLI herunter, und installieren Sie sie.

## <a name="next-steps"></a>Nächste Schritte

Informieren Sie sich darüber, wie Azure Dev Spaces Sie bei der Entwicklung komplexerer containerübergreifender Anwendungen unterstützt und wie Sie die gemeinsame Entwicklung vereinfachen können, indem Sie in verschiedenen Bereichen mit verschiedenen Versionen oder Branches Ihres Codes arbeiten.

> [!div class="nextstepaction"]
> [Schnellstart: Entwicklung im Team mit Java unter Kubernetes mithilfe von Azure Dev Spaces][team-development-qs]

[create-aks-cli]: ../../aks/kubernetes-walkthrough.md#create-a-resource-group
[create-aks-portal]: ../../aks/kubernetes-walkthrough-portal.md#create-an-aks-cluster
[install-cli]: /cli/azure/install-azure-cli?view=azure-cli-latest
[supported-regions]: ../about.md#supported-regions-and-configurations
[team-development-qs]: ../quickstart-team-development.md

[az-portal]: https://portal.azure.com
[az-portal-create-account]: https://azure.microsoft.com/free
[cli-linux]: https://aka.ms/get-azds-linux
[cli-mac]: https://aka.ms/get-azds-mac
[cli-win]: https://aka.ms/get-azds-windows
[visual-studio]: https://aka.ms/vsdownload?utm_source=mscom&utm_campaign=msdocs
[visual-studio-k8s-tools]: https://aka.ms/get-vsk8stools
[vscode]: https://code.visualstudio.com/download
[vscode-extension]: https://marketplace.visualstudio.com/items?itemName=azuredevspaces.azds
