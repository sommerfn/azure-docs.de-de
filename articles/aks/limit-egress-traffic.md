---
title: Einschränken des ausgehenden Datenverkehrs in Azure Kubernetes Service (AKS)
description: Erfahren Sie, welche Ports und Adressen zur Steuerung des ausgehenden Datenverkehrs in Azure Kubernetes Service (AKS) erforderlich sind.
services: container-service
author: mlearned
ms.service: container-service
ms.topic: article
ms.date: 06/06/2019
ms.author: mlearned
ms.openlocfilehash: 369729f10de4a55cd14bb866795ea1aa15b3d9da
ms.sourcegitcommit: 36e9cbd767b3f12d3524fadc2b50b281458122dc
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 08/20/2019
ms.locfileid: "69639787"
---
# <a name="preview---limit-egress-traffic-for-cluster-nodes-and-control-access-to-required-ports-and-services-in-azure-kubernetes-service-aks"></a>Vorschau: Einschränken des ausgehenden Datenverkehrs für Clusterknoten und Steuern des Zugriffs auf erforderliche Ports und Dienste in Azure Kubernetes Service (AKS)

Standardmäßig haben AKS-Cluster uneingeschränkten ausgehenden Internetzugriff. Diese Ebene des Netzwerkzugriffs ermöglicht, dass ausgeführte Knoten und Dienste nach Bedarf auf externe Ressourcen zugreifen können. Wenn Sie den ausgehenden Datenverkehr einschränken möchten, muss eine begrenzte Anzahl von Ports und Adressen zugänglich sein, um fehlerfreie Clusterwartungsaufgaben verwalten zu können. Der Cluster wird dann so konfiguriert, dass nur Containerimages des Basissystems aus Microsoft Container Registry (MCR) oder Azure Container Registry (ACR), jedoch aus keinen externen öffentlichen Repositorys verwendet werden. Sie müssen Ihre bevorzugten Firewall- und Sicherheitsregeln konfigurieren, um diese erforderlichen Ports und Adressen zuzulassen.

In diesem Artikel wird erläutert, welche Netzwerkports und vollqualifizierten Domänennamen (FQDNs) erforderlich oder optional sind, wenn Sie den ausgehenden Datenverkehr in einem AKS-Cluster einschränken.  Diese Funktion steht derzeit als Vorschau zur Verfügung.

> [!IMPORTANT]
> AKS-Previewfunktionen stehen gemäß dem Self-Service- und Aktivierungsprinzip zur Verfügung. Vorschauversionen werden „wie besehen“ und „wie verfügbar“ bereitgestellt und sind von den Vereinbarungen zum Service Level und der eingeschränkten Garantie ausgeschlossen. AKS-Vorschauen werden teilweise vom Kundensupport auf der Grundlage der bestmöglichen Leistung abgedeckt. Daher sind diese Funktionen nicht für die Verwendung in der Produktion vorgesehen. Weitere Informationen finden Sie in den folgenden Supportartikeln:
>
> * [Unterstützungsrichtlinien für Azure Kubernetes Service][aks-support-policies]
> * [Häufig gestellte Fragen zum Azure-Support][aks-faq]

## <a name="before-you-begin"></a>Voraussetzungen

Azure CLI-Version 2.0.66 oder höher muss installiert und konfiguriert sein. Führen Sie `az --version` aus, um die Version zu finden. Informationen zum Durchführen einer Installation oder eines Upgrades finden Sei bei Bedarf unter [Installieren der Azure CLI][install-azure-cli].

Um einen AKS-Cluster zu erstellen, für den der ausgehende Datenverkehr eingeschränkt werden kann, aktivieren Sie zuerst ein Featureflag für Ihr Abonnement. Mit dieser Featureregistrierung werden alle AKS-Cluster, die Sie erstellen, so konfiguriert, dass Containerimages des Basissystems aus MCR oder ACR verwendet werden. Um das Featureflag *AKSLockingDownEgressPreview* zu registrieren, verwenden Sie den Befehl [az feature register][az-feature-register] wie im folgenden Beispiel gezeigt:

> [!CAUTION]
> Wenn Sie eine Funktion in einem Abonnement registrieren, können Sie die Registrierung dieser Funktion derzeit nicht aufheben. Nachdem Sie einige Vorschaufeatures aktiviert haben, können Standardwerte für alle AKS-Cluster verwendet werden, die dann im Abonnement erstellt werden. Aktivieren Sie keine Vorschaufeatures für Produktionsabonnements. Verwenden Sie ein separates Abonnement, um Previewfunktionen zu testen und Feedback zu erhalten.

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

Sie können [Azure Firewall][azure-firewall] oder eine Firewallappliance eines Drittanbieters verwenden, um den ausgehenden Datenverkehr zu schützen und diese erforderlichen Ports und Adressen zu definieren. AKS erstellt diese Regeln nicht automatisch für Sie. Die folgenden Ports und Adressen dienen als Referenz, wenn Sie die entsprechenden Regeln in Ihrer Netzwerkfirewall erstellen.

> [!IMPORTANT]
> Wenn Sie Azure Firewall verwenden, um ausgehenden Datenverkehr einzuschränken, und eine benutzerdefinierte Route (UDR) erstellen, um den gesamten ausgehenden Datenverkehr zu erzwingen, stellen Sie sicher, dass Sie in der Firewall eine entsprechende DNAT-Regel erstellen, um ausgehenden Datenverkehr zuzulassen. Durch die Verwendung von Azure Firewall mit einer UDR wird das Setup für eingehenden Datenverkehr aufgrund von asymmetrischem Routing unterbrochen. (Das Problem tritt auf, weil das AKS-Subnetz über eine Standardroute verfügt, die zur privaten IP-Adresse der Firewall wechselt, Sie aber einen öffentlichen Load Balancer verwenden – Eingangs- oder Kubernetes-Dienst vom Typ „LoadBalancer“.) In diesem Fall wird der eingehende Load Balancer-Datenverkehr über die öffentliche IP-Adresse empfangen, während der Rückpfad über die private IP-Adresse der Firewall verläuft. Die Firewall löscht das zurückgegebene Paket, weil sie zustandsbehaftet ist und nichts über eine vorhandene Sitzung weiß. Informationen zur Integration von Azure Firewall in ihren Eingangs- oder Service Dienst-Load Balancer finden Sie unter [Integrieren von Azure Firewall mit Azure Load Balancer Standard](https://docs.microsoft.com/en-us/azure/firewall/integrate-lb).
>

In AKS gibt es zwei Gruppen von Ports und Adressen:

* Unter [Erforderliche Ports und Adressen für AKS-Cluster](#required-ports-and-addresses-for-aks-clusters) werden die Mindestanforderungen für autorisierten ausgehenden Datenverkehr erläutert.
* Die [optionalen empfohlenen Adressen und Ports für AKS-Cluster](#optional-recommended-addresses-and-ports-for-aks-clusters) sind nicht in allen Fällen erforderlich, die Integration in andere Dienste wie z. B. Azure Monitor funktioniert jedoch ohne sie nicht ordnungsgemäß. Überprüfen Sie diese Liste der optionalen Ports und FQDNs, und autorisieren Sie alle in Ihrem AKS-Cluster verwendeten Dienste und Komponenten.

> [!NOTE]
> Die Einschränkung des ausgehenden Datenverkehrs funktioniert nur bei neuen AKS-Clustern, die erstellt werden, nachdem Sie die Registrierung des Featureflags aktiviert haben. Bei vorhandenen Clustern müssen Sie mit dem Befehl `az aks upgrade` [ein Clusterupgrade ausführen][aks-upgrade], bevor Sie den ausgehenden Datenverkehr einschränken.

## <a name="required-ports-and-addresses-for-aks-clusters"></a>Erforderliche Ports und Adressen für AKS-Cluster

Die folgenden ausgehenden Ports und Netzwerkregeln sind für einen AKS-Cluster erforderlich:

* TCP-Port *443*
* Die TCP-Ports *9000* und *22* für die Kommunikation zwischen dem Tunnelfrontpod und dem Tunnelende auf dem API-Server.
    * Um genauere Informationen zu erhalten, sehen Sie sich die Adressen * *.hcp.\<location\>.azmk8s.io* und * *.tun.\<location\>.azmk8s.io* in der folgenden Tabelle an.

Die folgenden vollqualifizierten Domänennamen und Anwendungsregeln sind erforderlich:

| FQDN                       | Port      | Zweck      |
|----------------------------|-----------|----------|
| *.hcp.\<location\>.azmk8s.io | HTTPS:443, TCP:22, TCP:9000 | Diese Adresse ist der Endpunkt des API-Servers. Ersetzen Sie *\<location\>* durch die Region, in der Ihr AKS-Cluster bereitgestellt wurde. |
| *.tun.\<location\>.azmk8s.io | HTTPS:443, TCP:22, TCP:9000 | Diese Adresse ist der Endpunkt des API-Servers. Ersetzen Sie *\<location\>* durch die Region, in der Ihr AKS-Cluster bereitgestellt wurde. |
| aksrepos.azurecr.io        | HTTPS: 443 | Diese Adresse ist für den Zugriff auf Images in Azure Container Registry (ACR) erforderlich. |
| *.blob.core.windows.net    | HTTPS: 443 | Diese Adresse ist der Back-End-Speicher für in ACR gespeicherte Images. |
| mcr.microsoft.com          | HTTPS: 443 | Diese Adresse ist für den Zugriff auf Images in Microsoft Container Registry (MCR) erforderlich. |
| *.cdn.mscr.io              | HTTPS: 443 | Diese Adresse ist für den von Azure CDN (Content Delivery Network) unterstützten MCR-Speicher erforderlich. |
| management.azure.com       | HTTPS: 443 | Diese Adresse ist für GET/PUT-Vorgänge in Kubernetes erforderlich. |
| login.microsoftonline.com  | HTTPS: 443 | Diese Adresse ist für die Azure Active Directory-Authentifizierung erforderlich. |
| api.snapcraft.io           | HTTPS:443, HTTP:80 | Diese Adresse ist für die Installation von Snap-Paketen auf Linux-Knoten erforderlich. |
| ntp.ubuntu.com             | UDP:123   | Diese Adresse ist für die NTP-Zeitsynchronisierung auf Linux-Knoten erforderlich. |
| *.docker.io                | HTTPS: 443 | Diese Adresse ist zum Abrufen von Containerimages per Pull für den Tunnelfrontpod erforderlich. |

## <a name="optional-recommended-addresses-and-ports-for-aks-clusters"></a>Optionale empfohlene Adressen und Ports für AKS-Cluster

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
[aks-support-policies]: support-policies.md
[aks-faq]: faq.md
