---
title: Einschränken des ausgehenden Datenverkehrs in Azure Kubernetes Service (AKS)
description: Erfahren Sie, welche Ports und Adressen zur Steuerung des ausgehenden Datenverkehrs in Azure Kubernetes Service (AKS) erforderlich sind.
services: container-service
author: mlearned
ms.service: container-service
ms.topic: article
ms.date: 08/29/2019
ms.author: mlearned
ms.openlocfilehash: cfef8ff79f62eca9946dcbb49cafc253f36ab7bb
ms.sourcegitcommit: c22327552d62f88aeaa321189f9b9a631525027c
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 11/04/2019
ms.locfileid: "73472748"
---
# <a name="control-egress-traffic-for-cluster-nodes-in-azure-kubernetes-service-aks"></a>Steuern des ausgehenden Datenverkehrs für Clusterknoten in Azure Kubernetes Service (AKS)

Standardmäßig haben AKS-Cluster uneingeschränkten ausgehenden Internetzugriff. Diese Ebene des Netzwerkzugriffs ermöglicht, dass ausgeführte Knoten und Dienste nach Bedarf auf externe Ressourcen zugreifen können. Wenn Sie den ausgehenden Datenverkehr einschränken möchten, muss eine begrenzte Anzahl von Ports und Adressen zugänglich sein, um fehlerfreie Clusterwartungsaufgaben verwalten zu können. Ihr Cluster ist standardmäßig so konfiguriert, dass nur Containerimages des Basissystems aus Microsoft Container Registry (MCR) oder Azure Container Registry (ACR) verwendet werden. Konfigurieren Sie Ihre bevorzugten Firewall- und Sicherheitsregeln so, dass diese erforderlichen Ports und Adressen zulässig sind.

In diesem Artikel wird erläutert, welche Netzwerkports und vollqualifizierten Domänennamen (FQDNs) erforderlich oder optional sind, wenn Sie den ausgehenden Datenverkehr in einem AKS-Cluster einschränken.

> [!IMPORTANT]
> In diesem Dokument wird lediglich erläutert, wie der ausgehende Datenverkehr aus dem AKS-Subnetz gesperrt wird. Für AKS bestehen keine Eingangsanforderungen.  Das Blockieren von internem Datenverkehr im Subnetz mithilfe von Netzwerksicherheitsgruppen (NSGs) und Firewalls wird nicht unterstützt. Zum Steuern und Blockieren des Datenverkehrs innerhalb des Clusters verwenden Sie [Netzwerkrichtlinien][network-policy].

## <a name="before-you-begin"></a>Voraussetzungen

Azure CLI-Version 2.0.66 oder höher muss installiert und konfiguriert sein. Führen Sie `az --version` aus, um die Version zu finden. Informationen zum Durchführen einer Installation oder eines Upgrades finden Sei bei Bedarf unter [Installieren der Azure CLI][install-azure-cli].

## <a name="egress-traffic-overview"></a>Übersicht über ausgehenden Datenverkehr

Zu Verwaltungs- und Betriebszwecken müssen Knoten in einem AKS-Cluster auf bestimmte Ports und vollqualifizierte Domänennamen (FQDNs) zugreifen. Mögliche Aktionen sind die Kommunikation mit dem API-Server oder das Herunterladen und anschließende Installieren von Kernkomponenten des Kubernetes-Clusters und Sicherheitsupdates für Knoten. Standardmäßig ist der ausgehende Internetdatenverkehr für Knoten in einem AKS-Cluster nicht eingeschränkt. Der Cluster kann Containerimages des Basissystems aus externen Repositorys pullen.

Um die Sicherheit Ihres AKS-Clusters zu erhöhen, können Sie den ausgehenden Datenverkehr einschränken. Der Cluster wird so konfiguriert, dass Containerimages des Basissystems aus MCR oder ACR gepullt werden. Wenn Sie den ausgehenden Datenverkehr auf diese Weise einschränken, definieren Sie bestimmte Ports und FQDNs, damit AKS-Knoten ordnungsgemäß mit erforderlichen externen Diensten kommunizieren können. Ohne diese autorisierten Ports und FQDNs können AKS-Knoten nicht mit dem API-Server kommunizieren und keine Kernkomponenten installieren.

Sie können [Azure Firewall][azure-firewall] oder eine Firewallappliance eines Drittanbieters verwenden, um den ausgehenden Datenverkehr zu schützen und diese erforderlichen Ports und Adressen zu definieren. AKS erstellt diese Regeln nicht automatisch für Sie. Die folgenden Ports und Adressen dienen als Referenz, wenn Sie die entsprechenden Regeln in Ihrer Netzwerkfirewall erstellen.

> [!IMPORTANT]
> Wenn Sie Azure Firewall verwenden, um ausgehenden Datenverkehr einzuschränken, und eine benutzerdefinierte Route (UDR) erstellen, um den gesamten ausgehenden Datenverkehr zu erzwingen, stellen Sie sicher, dass Sie in der Firewall eine entsprechende DNAT-Regel erstellen, um ausgehenden Datenverkehr zuzulassen. Durch die Verwendung von Azure Firewall mit einer UDR wird das Setup für eingehenden Datenverkehr aufgrund von asymmetrischem Routing unterbrochen. (Das Problem tritt auf, wenn das AKS-Subnetz über eine Standardroute verfügt, die zur privaten IP-Adresse der Firewall wechselt, Sie aber einen öffentlichen Load Balancer verwenden – Eingangs- oder Kubernetes-Dienst vom Typ LoadBalancer). In diesem Fall wird der eingehende Load Balancer-Datenverkehr über die öffentliche IP-Adresse empfangen, während der Rückpfad über die private IP-Adresse der Firewall verläuft. Die Firewall löscht das zurückgegebene Paket, weil sie zustandsbehaftet ist und nichts über eine vorhandene Sitzung weiß. Informationen zur Integration von Azure Firewall in ihren Eingangs- oder Service Dienst-Load Balancer finden Sie unter [Integrieren von Azure Firewall mit Azure Load Balancer Standard](https://docs.microsoft.com/azure/firewall/integrate-lb).
> Sie können den Datenverkehr für TCP-Port 9000 und TCP-Port 22 mithilfe einer Netzwerkregel zwischen den IP-Adressen der ausgehenden Workerknoten und der IP-Adresse für den API-Server sperren.

In AKS gibt es zwei Gruppen von Ports und Adressen:

* Unter [Erforderliche Ports und Adressen für AKS-Cluster](#required-ports-and-addresses-for-aks-clusters) werden die Mindestanforderungen für autorisierten ausgehenden Datenverkehr erläutert.
* Die [optionalen empfohlenen Adressen und Ports für AKS-Cluster](#optional-recommended-addresses-and-ports-for-aks-clusters) sind nicht in allen Fällen erforderlich, die Integration in andere Dienste wie z. B. Azure Monitor funktioniert jedoch ohne sie nicht ordnungsgemäß. Überprüfen Sie diese Liste der optionalen Ports und FQDNs, und autorisieren Sie alle in Ihrem AKS-Cluster verwendeten Dienste und Komponenten.

> [!NOTE]
> Das Einschränken von ausgehendem Datenverkehr funktioniert nur bei neuen AKS-Clustern. Bei vorhandenen Clustern müssen Sie mit dem Befehl `az aks upgrade` [ein Clusterupgrade ausführen][aks-upgrade], bevor Sie den ausgehenden Datenverkehr einschränken.

## <a name="required-ports-and-addresses-for-aks-clusters"></a>Erforderliche Ports und Adressen für AKS-Cluster

Die folgenden ausgehenden Ports und Netzwerkregeln sind für einen AKS-Cluster erforderlich:

* TCP-Port *443*
* TCP [IPAddrOfYourAPIServer]:443 ist erforderlich, wenn Sie über eine App verfügen, die mit dem API-Server kommunizieren muss.  Diese Änderung kann festgelegt werden, nachdem der Cluster erstellt wurde.
* Die TCP-Ports *9000* und *22* für die Kommunikation zwischen dem Tunnelfrontpod und dem Tunnelende auf dem API-Server.
    * Um genauere Informationen zu erhalten, sehen Sie sich die Adressen * *.hcp.\<location\>.azmk8s.io* und * *.tun.\<location\>.azmk8s.io* in der folgenden Tabelle an.
* UDP-Port *53* für DNS ist ebenfalls erforderlich, wenn Sie über Pods verfügen, die direkt auf den API-Server zugreifen.

Die folgenden vollqualifizierten Domänennamen und Anwendungsregeln sind erforderlich:
- Azure Global

| FQDN                       | Port      | Zweck      |
|----------------------------|-----------|----------|
| *.hcp.\<location\>.azmk8s.io | HTTPS:443, TCP:22, TCP:9000 | Diese Adresse ist der Endpunkt des API-Servers. Ersetzen Sie *\<location\>* durch die Region, in der Ihr AKS-Cluster bereitgestellt wurde. |
| *.tun.\<location\>.azmk8s.io | HTTPS:443, TCP:22, TCP:9000 | Diese Adresse ist der Endpunkt des API-Servers. Ersetzen Sie *\<location\>* durch die Region, in der Ihr AKS-Cluster bereitgestellt wurde. |
| aksrepos.azurecr.io        | HTTPS: 443 | Diese Adresse ist für den Zugriff auf Images in Azure Container Registry (ACR) erforderlich. Diese Registrierung enthält Images/Diagramme von Drittanbietern (z. B. metrics server, core dns usw.), die für die Funktion des Clusters während des Upgrades und der Skalierung des Clusters erforderlich sind.|
| *.blob.core.windows.net    | HTTPS: 443 | Diese Adresse ist der Back-End-Speicher für in ACR gespeicherte Images. |
| mcr.microsoft.com          | HTTPS: 443 | Diese Adresse ist für den Zugriff auf Images in Microsoft Container Registry (MCR) erforderlich. Diese Registrierung enthält Images/Diagramme von Erstanbietern (z. B. moby usw.), die für die Funktion des Clusters während des Upgrades und der Skalierung des Clusters erforderlich sind. |
| *.cdn.mscr.io              | HTTPS: 443 | Diese Adresse ist für den von Azure CDN (Content Delivery Network) unterstützten MCR-Speicher erforderlich. |
| management.azure.com       | HTTPS: 443 | Diese Adresse ist für GET/PUT-Vorgänge in Kubernetes erforderlich. |
| login.microsoftonline.com  | HTTPS: 443 | Diese Adresse ist für die Azure Active Directory-Authentifizierung erforderlich. |
| ntp.ubuntu.com             | UDP:123   | Diese Adresse ist für die NTP-Zeitsynchronisierung auf Linux-Knoten erforderlich. |
| packages.microsoft.com     | HTTPS: 443 | Diese Adresse ist das Microsoft-Paketrepository, das für zwischengespeicherte *apt-get*-Vorgänge verwendet wird.  Beispielpakete sind Moby, PowerShell und Azure CLI. |
| acs-mirror.azureedge.net   | HTTPS: 443 | Diese Adresse ist für das Repository, das zum Installieren erforderlicher Binärdateien wie kubenet und Azure CNI benötigt wird. |
- Azure China

| FQDN                       | Port      | Zweck      |
|----------------------------|-----------|----------|
| *.hcp.\<location\>.cx.prod.service.azk8s.cn | HTTPS:443, TCP:22, TCP:9000 | Diese Adresse ist der Endpunkt des API-Servers. Ersetzen Sie *\<location\>* durch die Region, in der Ihr AKS-Cluster bereitgestellt wurde. |
| *.tun.\<location\>.cx.prod.service.azk8s.cn | HTTPS:443, TCP:22, TCP:9000 | Diese Adresse ist der Endpunkt des API-Servers. Ersetzen Sie *\<location\>* durch die Region, in der Ihr AKS-Cluster bereitgestellt wurde. |
| *.azk8s.cn        | HTTPS: 443 | Diese Adresse wird benötigt, um erforderliche Binärdateien und Images herunterzuladen.|
| mcr.microsoft.com          | HTTPS: 443 | Diese Adresse ist für den Zugriff auf Images in Microsoft Container Registry (MCR) erforderlich. Diese Registrierung enthält Images/Diagramme von Erstanbietern (z. B. moby usw.), die für die Funktion des Clusters während des Upgrades und der Skalierung des Clusters erforderlich sind. |
| *.cdn.mscr.io              | HTTPS: 443 | Diese Adresse ist für den von Azure CDN (Content Delivery Network) unterstützten MCR-Speicher erforderlich. |
| management.chinacloudapi.cn       | HTTPS: 443 | Diese Adresse ist für GET/PUT-Vorgänge in Kubernetes erforderlich. |
| login.chinacloudapi.cn  | HTTPS: 443 | Diese Adresse ist für die Azure Active Directory-Authentifizierung erforderlich. |
| ntp.ubuntu.com             | UDP:123   | Diese Adresse ist für die NTP-Zeitsynchronisierung auf Linux-Knoten erforderlich. |
| packages.microsoft.com     | HTTPS: 443 | Diese Adresse ist das Microsoft-Paketrepository, das für zwischengespeicherte *apt-get*-Vorgänge verwendet wird.  Beispielpakete sind Moby, PowerShell und Azure CLI. |
- Azure Government

| FQDN                       | Port      | Zweck      |
|----------------------------|-----------|----------|
| *.hcp.\<location\>.cx.aks.containerservice.azure.us | HTTPS:443, TCP:22, TCP:9000 | Diese Adresse ist der Endpunkt des API-Servers. Ersetzen Sie *\<location\>* durch die Region, in der Ihr AKS-Cluster bereitgestellt wurde. |
| *.tun.\<location\>.cx.aks.containerservice.azure.us | HTTPS:443, TCP:22, TCP:9000 | Diese Adresse ist der Endpunkt des API-Servers. Ersetzen Sie *\<location\>* durch die Region, in der Ihr AKS-Cluster bereitgestellt wurde. |
| aksrepos.azurecr.io        | HTTPS: 443 | Diese Adresse ist für den Zugriff auf Images in Azure Container Registry (ACR) erforderlich. Diese Registrierung enthält Images/Diagramme von Drittanbietern (z. B. metrics server, core dns usw.), die für die Funktion des Clusters während des Upgrades und der Skalierung des Clusters erforderlich sind.|
| *.blob.core.windows.net    | HTTPS: 443 | Diese Adresse ist der Back-End-Speicher für in ACR gespeicherte Images. |
| mcr.microsoft.com          | HTTPS: 443 | Diese Adresse ist für den Zugriff auf Images in Microsoft Container Registry (MCR) erforderlich. Diese Registrierung enthält Images/Diagramme von Erstanbietern (z. B. moby usw.), die für die Funktion des Clusters während des Upgrades und der Skalierung des Clusters erforderlich sind. |
| *.cdn.mscr.io              | HTTPS: 443 | Diese Adresse ist für den von Azure CDN (Content Delivery Network) unterstützten MCR-Speicher erforderlich. |
| management.usgovcloudapi.net       | HTTPS: 443 | Diese Adresse ist für GET/PUT-Vorgänge in Kubernetes erforderlich. |
| login.microsoftonline.us  | HTTPS: 443 | Diese Adresse ist für die Azure Active Directory-Authentifizierung erforderlich. |
| ntp.ubuntu.com             | UDP:123   | Diese Adresse ist für die NTP-Zeitsynchronisierung auf Linux-Knoten erforderlich. |
| packages.microsoft.com     | HTTPS: 443 | Diese Adresse ist das Microsoft-Paketrepository, das für zwischengespeicherte *apt-get*-Vorgänge verwendet wird.  Beispielpakete sind Moby, PowerShell und Azure CLI. |
| acs-mirror.azureedge.net   | HTTPS: 443 | Diese Adresse ist für das Repository, das zum Installieren erforderlicher Binärdateien wie kubenet und Azure CNI benötigt wird. |
## <a name="optional-recommended-addresses-and-ports-for-aks-clusters"></a>Optionale empfohlene Adressen und Ports für AKS-Cluster

Die folgenden ausgehenden Ports und Netzwerkregeln sind für einen AKS-Cluster optional:

Die folgenden vollqualifizierten Domänennamen und Anwendungsregeln werden für die ordnungsgemäße Ausführung von AKS-Clustern empfohlen:

| FQDN                                    | Port      | Zweck      |
|-----------------------------------------|-----------|----------|
| security.ubuntu.com, azure.archive.ubuntu.com, changelogs.ubuntu.com | HTTP: 80   | Über diese Adresse können die Linux-Clusterknoten die erforderlichen Sicherheitspatches und Updates herunterladen. |

## <a name="required-addresses-and-ports-for-gpu-enabled-aks-clusters"></a>Erforderliche Adressen und Ports für GPU-fähige AKS-Cluster

Die folgenden vollqualifizierten Domänennamen und Anwendungsregeln sind für AKS-Cluster mit aktivierter GPU erforderlich:

| FQDN                                    | Port      | Zweck      |
|-----------------------------------------|-----------|----------|
| nvidia.github.io | HTTPS: 443 | Diese Adresse wird für die ordnungsgemäße Treiberinstallation und -ausführung für GPU-basierte Knoten verwendet. |
| us.download.nvidia.com | HTTPS: 443 | Diese Adresse wird für die ordnungsgemäße Treiberinstallation und -ausführung für GPU-basierte Knoten verwendet. |
| apt.dockerproject.org | HTTPS: 443 | Diese Adresse wird für die ordnungsgemäße Treiberinstallation und -ausführung für GPU-basierte Knoten verwendet. |

## <a name="required-addresses-and-ports-with-azure-monitor-for-containers-enabled"></a>Erforderliche Adressen und Ports mit aktiviertem Azure Monitor für Container

Die folgenden vollqualifizierten Domänennamen und Anwendungsregeln sind für AKS-Cluster mit aktiviertem Azure Monitor für Container erforderlich:

| FQDN                                    | Port      | Zweck      |
|-----------------------------------------|-----------|----------|
| dc.services.visualstudio.com | HTTPS: 443  | Wird für korrekte Metriken und Überwachung von Telemetriedaten mithilfe von Azure Monitor verwendet. |
| *.ods.opinsights.azure.com    | HTTPS: 443 | Wird von Azure Monitor zum Erfassen von Log Analytics-Daten verwendet. |
| *.oms.opinsights.azure.com | HTTPS: 443 | Diese Adresse wird von omsagent verwendet, mit dem der Log Analytics-Dienst authentifiziert wird. |
|*.microsoftonline.com | HTTPS: 443 | Wird zum Authentifizieren und Senden von Metriken an Azure Monitor verwendet. |
|*.monitoring.azure.com | HTTPS: 443 | Wird zum Senden von Metrikdaten an Azure Monitor verwendet. |

## <a name="required-addresses-and-ports-for-aks-clusters-with-azure-policy-in-public-preview-enabled"></a>Erforderliche Adressen und Ports für AKS-Cluster mit aktiviertem Azure Policy (in der öffentlichen Vorschau)

> [!CAUTION]
> Einige der unten aufgeführten Features befinden sich in der Vorschauphase.  Die Vorschläge in diesem Artikel unterliegen Änderungen, während das Feature die öffentliche Vorschau und zukünftige Releasestufen durchläuft.

Die folgenden vollqualifizierten Domänennamen und Anwendungsregeln sind für AKS-Cluster mit aktiviertem Azure Policy erforderlich.

| FQDN                                    | Port      | Zweck      |
|-----------------------------------------|-----------|----------|
| gov-prod-policy-data.trafficmanager.net | HTTPS: 443 | Diese Adresse wird für den ordnungsgemäßen Betrieb von Azure Policy verwendet. (Derzeit als Vorschauversion in AKS) |
| raw.githubusercontent.com | HTTPS: 443 | Diese Adresse wird verwendet, um die integrierten Richtlinien aus GitHub abzurufen und so den ordnungsgemäßen Betrieb von Azure Policy sicherzustellen. (Derzeit als Vorschauversion in AKS) |
| *.gk.<location>.azmk8s.io | HTTPS: 443 | Das Azure Policy-Add-On kommuniziert mit dem Gatekeeper-Überwachungsendpunkt, der auf dem Masterserver ausgeführt wird, um die Überwachungsergebnisse abzurufen. |
| dc.services.visualstudio.com | HTTPS: 443 | Das Azure Policy-Add-On sendet Telemetriedaten an den Application Insights-Endpunkt. |

## <a name="required-by-windows-server-based-nodes-in-public-preview-enabled"></a>Erforderlich für aktivierte Windows Server-basierte Knoten (in der öffentlichen Vorschau)

> [!CAUTION]
> Einige der unten aufgeführten Features befinden sich in der Vorschauphase.  Die Vorschläge in diesem Artikel unterliegen Änderungen, während das Feature die öffentliche Vorschau und zukünftige Releasestufen durchläuft.

Die folgenden vollqualifizierten Domänennamen und Anwendungsregeln sind für Windows Server-basierte AKS-Cluster erforderlich:

| FQDN                                    | Port      | Zweck      |
|-----------------------------------------|-----------|----------|
| onegetcdn.azureedge.net, winlayers.blob.core.windows.net, winlayers.cdn.mscr.io, go.microsoft.com | HTTPS: 443 | Zum Installieren Windows-bezogener Binärdateien |
| mp.microsoft.com, www<span></span>.msftconnecttest.com, ctldl.windowsupdate.com | HTTP: 80 | Zum Installieren Windows-bezogener Binärdateien |
| kms.core.windows.net | TCP:1688 | Zum Installieren Windows-bezogener Binärdateien |


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
