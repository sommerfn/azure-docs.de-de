---
title: Azure Kubernetes-Netzwerkrichtlinien | Microsoft-Dokumentation
description: Erfahren Sie mehr über Kubernetes-Netzwerkrichtlinien, um Ihren Kubernetes-Cluster zu schützen.
services: virtual-network
documentationcenter: na
author: aanandr
manager: NarayanAnnamalai
editor: ''
tags: azure-resource-manager
ms.assetid: ''
ms.service: virtual-network
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 9/25/2018
ms.author: aanandr
ms.custom: ''
ms.openlocfilehash: 5a6da7e65a9a3e962a2df37b062792fbb990d04d
ms.sourcegitcommit: 0b1a4101d575e28af0f0d161852b57d82c9b2a7e
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 10/30/2019
ms.locfileid: "73159693"
---
# <a name="azure-kubernetes-network-policies-overview"></a>Übersicht über Azure Kubernetes-Netzwerkrichtlinien

Netzwerkrichtlinien bieten Mikrosegmentierung für Pods, genau wie Netzwerksicherheitsgruppen (NSGs) Mikrosegmentierung für VMs bieten. Die Implementierung der Azure-Netzwerkrichtlinie unterstützt die Standardspezifikation der Kubernetes-Netzwerkrichtlinie. Sie können Bezeichnungen verwenden, um eine Gruppe von Pods auszuwählen und eine Liste von Ein- und Ausgangsregeln zu definieren, die den eingehenden und ausgehenden Datenverkehr für diese Pods bestimmen. Weitere Informationen zu Kubernetes-Netzwerkrichtlinien finden Sie in der [Kubernetes-Dokumentation](https://kubernetes.io/docs/concepts/services-networking/network-policies/).

![Übersicht über Kubernetes-Netzwerkrichtlinien](./media/kubernetes-network-policies/kubernetes-network-policies-overview.png)

Azure-Netzwerkrichtlinien funktionieren in Verbindung mit Azure CNI, wodurch die VNET-Integration für Container ermöglicht wird. Derzeit ist die Unterstützung nur für Linux-Knoten verfügbar. Die Implementierungen konfigurieren die Linux-IP-Tabellenregeln basierend auf den definierten Richtlinien, um das Filtern des Datenverkehrs durchzusetzen.

## <a name="planning-security-for-your-kubernetes-cluster"></a>Planen der Sicherheit Ihres Kubernetes-Clusters
Wenn Sie die Sicherheit Ihres Clusters implementieren, verwenden Sie Netzwerksicherheitsgruppen, um den Nord-Süd-Datenverkehr zu filtern, d.h. den ein- und ausgehenden Datenverkehr Ihres Clustersubnetzes, und verwenden Sie Kubernetes-Netzwerkrichtlinien für den Ost-West-Datenverkehr, d.h. den Datenverkehr zwischen den Pods in Ihrem Cluster.

## <a name="using-azure-kubernetes-network-policies"></a>Verwenden der Azure Kubernetes-Netzwerkrichtlinien
Azure-Netzwerkrichtlinien können auf folgende Weise verwendet werden, um eine Mikrosegmentierung für Pods zu ermöglichen.

### <a name="acs-engine"></a>AKS-Engine
Die AKS-Engine (Azure Container Service, Azure Kubernetes Service) ist ein Tool, das eine Azure Resource Manager-Vorlage zum Bereitstellen eines Kubernetes-Clusters in Azure generiert. Die Clusterkonfiguration wird in einer JSON-Datei angegeben, die beim Generieren der Vorlage an das Tool übergeben wird. Weitere Informationen zur gesamten Liste der unterstützten Clustereinstellungen und deren Beschreibungen finden Sie in der Clusterdefinition für die Azure Container Service-Engine (AKS).

Um Richtlinien für Cluster zu aktivieren, die mit der AKS-Engine bereitgestellt werden, geben Sie für den Wert der „networkPolicy“-Einstellung in der Clusterdefinitionsdatei „azure“ an.

#### <a name="example-configuration"></a>Beispielkonfiguration

Die folgende JSON-Beispielkonfiguration erstellt ein neues virtuelles Netzwerk und Subnetz und stellt darin einen Kubernetes-Cluster mit Azure CNI bereit. Wir empfehlen, dass Sie die JSON-Datei im Editor bearbeiten. 
```json
{
  "apiVersion": "vlabs",
  "properties": {
    "orchestratorProfile": {
      "orchestratorType": "Kubernetes",
      "kubernetesConfig": {
         "networkPolicy": "azure"
       }
    },
    "masterProfile": {
      "count": 1,
      "dnsPrefix": "<specify a cluster name>",
      "vmSize": "Standard_D2s_v3"
    },
    "agentPoolProfiles": [
      {
        "name": "agentpool",
        "count": 2,
        "vmSize": "Standard_D2s_v3",
        "availabilityProfile": "AvailabilitySet"
      }
    ],
   "linuxProfile": {
      "adminUsername": "<specify admin username>",
      "ssh": {
        "publicKeys": [
          {
            "keyData": "<cut and paste your ssh key here>"
          }
        ]
      }
    },
    "servicePrincipalProfile": {
      "clientId": "<enter the client ID of your service principal here >",
      "secret": "<enter the password of your service principal here>"
    }
  }
}

```
### <a name="creating-your-own-kubernetes-cluster-in-azure"></a>Erstellen Ihres Kubernetes-Clusters in Azure
Die Implementierung kann verwendet werden, um Netzwerkrichtlinien für Pods in Kubernetes-Clustern bereitzustellen, die Sie selbst implementieren, ohne Tools wie AKS-Engine. Installieren Sie zunächst das CNI-Plug-In, und aktivieren Sie es auf jeder VM in einem Cluster. Genaue Anweisungen finden Sie unter [Bereitstellen eines Plug-Ins für den eigenen Kubernetes-Cluster](deploy-container-networking.md#deploy-plug-in-for-a-kubernetes-cluster).

Wenn der Cluster bereitgestellt wurde, führen Sie den folgenden Befehl `kubectl` aus, um die Azure-Netzwerkrichtlinie *daemonset* herunterzuladen und auf den Cluster anzuwenden.

  ```
  kubectl apply -f https://raw.githubusercontent.com/Azure/acs-engine/master/parts/k8s/addons/kubernetesmasteraddons-azure-npm-daemonset.yaml

  ```
Die Lösung ist ebenfalls Open Source, und der Code ist im [Azure-Containernetzwerkrepository](https://github.com/Azure/azure-container-networking/tree/master/npm) verfügbar.



## <a name="next-steps"></a>Nächste Schritte
- Weitere Informationen zu [Azure Kubernetes Service](../aks/intro-kubernetes.md)
-  Weitere Informationen zu [Containernetzwerken](container-networking-overview.md)
- [Bereitstellen des Plug-Ins](deploy-container-networking.md) für Kubernetes-Cluster oder Docker-Container
