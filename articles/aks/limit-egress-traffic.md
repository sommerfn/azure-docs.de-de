---
title: Einschränken des ausgehenden Datenverkehrs in Azure Kubernetes Service (AKS)
description: Erfahren Sie, welche Ports und Adressen zur Steuerung des ausgehenden Datenverkehrs in Azure Kubernetes Service (AKS) erforderlich sind.
services: container-service
author: iainfoulds
ms.service: container-service
ms.topic: article
ms.date: 05/14/2019
ms.author: iainfou
ms.openlocfilehash: de0ba13a527569e446a44c275b7323d4487f53b6
ms.sourcegitcommit: 36c50860e75d86f0d0e2be9e3213ffa9a06f4150
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 05/16/2019
ms.locfileid: "65780295"
---
# <a name="preview---limit-egress-traffic-for-cluster-nodes-and-control-access-to-required-ports-and-services-in-azure-kubernetes-service-aks"></a>Vorschau: Einschränken des ausgehenden Datenverkehrs für Clusterknoten und Steuern des Zugriffs auf erforderliche Ports und Dienste in Azure Kubernetes Service (AKS)

Standardmäßig haben AKS-Cluster uneingeschränkten ausgehenden Internetzugriff. Diese Ebene des Netzwerkzugriffs ermöglicht, dass ausgeführte Knoten und Dienste nach Bedarf auf externe Ressourcen zugreifen können. Wenn Sie den ausgehenden Datenverkehr einschränken möchten, muss eine begrenzte Anzahl von Ports und Adressen zugänglich sein, um fehlerfreie Clusterwartungsaufgaben verwalten zu können. Der Cluster wird dann so konfiguriert, dass nur Containerimages des Basissystems aus Microsoft Container Registry (MCR) oder Azure Container Registry (ACR), jedoch aus keinen externen öffentlichen Repositorys verwendet werden.

In diesem Artikel wird erläutert, welche Netzwerkports und vollqualifizierten Domänennamen (FQDNs) erforderlich oder optional sind, wenn Sie den ausgehenden Datenverkehr in einem AKS-Cluster einschränken.  Diese Funktion steht derzeit als Vorschau zur Verfügung.

> [!IMPORTANT]
> AKS-Previewfunktionen stehen gemäß dem Self-Service- und Aktivierungsprinzip zur Verfügung. Vorschauversionen werden zum Sammeln von Feedback und Fehlern mithilfe unserer Community bereitgestellt. Allerdings werden sie vom technischen Support von Azure nicht unterstützt. Wenn Sie einen Cluster erstellen oder diese Features zu einem vorhandenen Cluster hinzufügen, wird der entsprechende Cluster erst dann unterstützt, wenn das Feature sich nicht mehr in der Vorschau befindet und in die allgemeine Verfügbarkeit übergegangen ist.
>
> Wenn Sie Probleme mit Vorschaufunktionen haben, [eröffnen Sie ein Ticket im GitHub-Repository von AKS ][aks-github], und geben Sie den Namen des Vorschaufeatures im Fehlertitel an.

## <a name="before-you-begin"></a>Voraussetzungen

Azure CLI-Version 2.0.61 oder höher muss installiert und konfiguriert sein. Führen Sie `az --version` aus, um die Version zu finden. Wenn Sie eine Installation oder ein Upgrade ausführen müssen, finden Sie unter [Installieren von Azure CLI 2.0][install-azure-cli] Informationen dazu.

Um einen AKS-Cluster zu erstellen, für den der ausgehende Datenverkehr eingeschränkt werden kann, aktivieren Sie zuerst ein Featureflag für Ihr Abonnement. Mit dieser Featureregistrierung werden alle AKS-Cluster, die Sie erstellen, so konfiguriert, dass Containerimages des Basissystems aus MCR oder ACR verwendet werden. Um das Featureflag *AKSLockingDownEgressPreview* zu registrieren, verwenden Sie den Befehl [az feature register][az-feature-register], wie im folgenden Beispiel gezeigt:

```azurecli-interactive
az feature register --name AKSLockingDownEgressPreview --namespace Microsoft.ContainerService
```

Es dauert einige Minuten, bis der Status *Registered (Registriert)* angezeigt wird. Sie können den Registrierungsstatus mithilfe des Befehls [az feature list][az-feature-list] überprüfen:

```azurecli-interactive
az feature list -o table --query "[?contains(name, 'Microsoft.ContainerService/AKSLockingDownEgressPreview')].{Name:name,State:properties.state}"
```

Wenn der Vorgang abgeschlossen ist, können Sie die Registrierung des *Microsoft.ContainerService*-Ressourcenanbieters mit dem Befehl [az provider register][az-provider-register] aktualisieren:

```azurecli-interactive
az provider register --namespace Microsoft.ContainerService
```

## <a name="egress-traffic-overview"></a>Übersicht über ausgehenden Datenverkehr

Zu Verwaltungs- und Betriebszwecken müssen Knoten in einem AKS-Cluster auf bestimmte Ports und vollqualifizierte Domänennamen (FQDNs) zugreifen. Mögliche Aktionen sind die Kommunikation mit dem API-Server oder das Herunterladen und anschließende Installieren von Kernkomponenten des Kubernetes-Clusters und Sicherheitsupdates für Knoten. Standardmäßig ist der ausgehende Internetdatenverkehr für Knoten in einem AKS-Cluster nicht eingeschränkt. Der Cluster kann Containerimages des Basissystems aus externen Repositorys pullen.

Um die Sicherheit Ihres AKS-Clusters zu erhöhen, können Sie den ausgehenden Datenverkehr einschränken. Der Cluster wird so konfiguriert, dass Containerimages des Basissystems aus MCR oder ACR gepullt werden. Wenn Sie den ausgehenden Datenverkehr auf diese Weise einschränken, müssen Sie bestimmte Ports und FQDNs definieren, damit AKS-Knoten ordnungsgemäß mit erforderlichen externen Diensten kommunizieren können. Ohne diese autorisierten Ports und FQDNs können AKS-Knoten nicht mit dem API-Server kommunizieren und keine Kernkomponenten installieren.

Sie können [Azure Firewall][azure-firewall] oder eine Firewallappliance eines Drittanbieters verwenden, um den ausgehenden Datenverkehr zu schützen und diese erforderlichen Ports und Adressen zu definieren.

In AKS gibt es zwei Gruppen von Ports und Adressen:

* Unter [Erforderliche Ports und Adressen für AKS-Cluster](#required-ports-and-addresses-for-aks-clusters) werden die Mindestanforderungen für autorisierten ausgehenden Datenverkehr erläutert.
* Die [optionalen empfohlenen Adressen und Ports für AKS-Cluster](#optional-recommended-addresses-and-ports-for-aks-clusters) sind nicht in allen Fällen erforderlich, die Integration in andere Dienste wie z. B. Azure Monitor funktioniert jedoch ohne sie nicht ordnungsgemäß. Überprüfen Sie diese Liste der optionalen Ports und FQDNs, und autorisieren Sie alle in Ihrem AKS-Cluster verwendeten Dienste und Komponenten.

> [!NOTE]
> Die Einschränkung des ausgehenden Datenverkehrs funktioniert nur bei neuen AKS-Clustern, die erstellt werden, nachdem Sie die Registrierung des Featureflags aktiviert haben. Bei vorhandenen Clustern müssen Sie mit dem Befehl `az aks upgrade` [ein Clusterupgrade ausführen][aks-upgrade], bevor Sie den ausgehenden Datenverkehr einschränken.

## <a name="required-ports-and-addresses-for-aks-clusters"></a>Erforderliche Ports und Adressen für AKS-Cluster

Die folgenden ausgehenden Ports und Netzwerkregeln sind für einen AKS-Cluster erforderlich:

* TCP-Port *443*
* TCP-Port *9000*

Die folgenden vollqualifizierten Domänennamen und Anwendungsregeln sind erforderlich:

| FQDN                      | Port      | Zweck      |
|---------------------------|-----------|----------|
| *.azmk8s.io               | HTTPS: 443 | Diese Adresse ist der Endpunkt des API-Servers. |
| aksrepos.azurecr.io       | HTTPS: 443 | Diese Adresse ist für den Zugriff auf Images in Azure Container Registry (ACR) erforderlich. |
| *.blob.core.windows.net   | HTTPS: 443 | Diese Adresse ist der Back-End-Speicher für in ACR gespeicherte Images. |
| mcr.microsoft.com         | HTTPS: 443 | Diese Adresse ist für den Zugriff auf Images in Microsoft Container Registry (MCR) erforderlich. |
| management.azure.com      | HTTPS: 443 | Diese Adresse ist für GET/PUT-Vorgänge in Kubernetes erforderlich. |
| login.microsoftonline.com | HTTPS: 443 | Diese Adresse ist für die Azure Active Directory-Authentifizierung erforderlich. |

## <a name="optional-recommended-addresses-and-ports-for-aks-clusters"></a>Optionale empfohlene Adressen und Ports für AKS-Cluster

Die folgenden ausgehenden Ports und Netzwerkregeln sind für die ordnungsgemäße Ausführung von AKS-Clustern nicht erforderlich, werden jedoch empfohlen:

* UDP-Port *123* für die NTP-Zeitsynchronisierung
* UDP-Port *53* für DNS

Die folgenden vollqualifizierten Domänennamen und Anwendungsregeln werden für die ordnungsgemäße Ausführung von AKS-Clustern empfohlen:

| FQDN                                    | Port      | Zweck      |
|-----------------------------------------|-----------|----------|
| *.ubuntu.com                            | HTTP: 80   | Über diese Adresse können die Linux-Clusterknoten die erforderlichen Sicherheitspatches und Updates herunterladen. |
| packages.microsoft.com                  | HTTPS: 443 | Diese Adresse ist das Microsoft-Paketrepository, das für zwischengespeicherte *apt-get*-Vorgänge verwendet wird. |
| dc.services.visualstudio.com            | HTTPS: 443 | Empfohlen für korrekte Metriken und Überwachung mithilfe von Azure Monitor. |
| *.opinsights.azure.com                  | HTTPS: 443 | Empfohlen für korrekte Metriken und Überwachung mithilfe von Azure Monitor. |
| *.monitoring.azure.com                  | HTTPS: 443 | Empfohlen für korrekte Metriken und Überwachung mithilfe von Azure Monitor. |
| gov-prod-policy-data.trafficmanager.net | HTTPS: 443 | Diese Adresse wird für den ordnungsgemäßen Betrieb von Azure Policy (derzeit als Vorschauversion in AKS) verwendet. |
| apt.dockerproject.org                   | HTTPS: 443 | Diese Adresse wird für die ordnungsgemäße Treiberinstallation und -ausführung für GPU-basierte Knoten verwendet. |
| nvidia.github.io                        | HTTPS: 443 | Diese Adresse wird für die ordnungsgemäße Treiberinstallation und -ausführung für GPU-basierte Knoten verwendet. |

## <a name="next-steps"></a>Nächste Schritte

In diesem Artikel haben Sie erfahren, welche Ports und Adressen zugelassen werden müssen, wenn Sie den ausgehenden Datenverkehr für den Cluster einschränken. Sie können auch definieren, wie die Pods selbst kommunizieren können und welche Einschränkungen für sie im Cluster gelten. Weitere Informationen finden Sie unter [Sicherer Datenverkehr zwischen Pods durch Netzwerkrichtlinien in Azure Kubernetes Service (AKS)][network-policy].

<!-- LINKS - external -->
[aks-github]: https://github.com/azure/aks/issues]

<!-- LINKS - internal -->
[aks-quickstart-cli]: kubernetes-walkthrough.md
[aks-quickstart-portal]: kubernetes-walkthrough-portal.md
[install-azure-cli]: /cli/azure/install-azure-cli
[network-policy]: use-network-policies.md
[azure-firewall]: ../firewall/overview.md
[az-feature-register]: /cli/azure/feature#az-feature-register
[az-feature-list]: /cli/azure/feature#az-feature-list
[az-provider-register]: /cli/azure/provider#az-provider-register
[aks-upgrade]: upgrade-cluster.md
