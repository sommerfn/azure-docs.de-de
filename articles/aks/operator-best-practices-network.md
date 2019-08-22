---
title: Best Practices für Operator – Netzwerkkonnektivität in Azure Kubernetes Service (AKS)
description: Lernen Sie die Best Practices für virtuelle Netzwerkressourcen und -konnektivität in Azure Kubernetes Service (AKS) für Clusteroperator kennen.
services: container-service
author: mlearned
ms.service: container-service
ms.topic: conceptual
ms.date: 12/10/2018
ms.author: mlearned
ms.openlocfilehash: d1bc865b38b52c8a7c3ac6ec4dab6408a1d0430c
ms.sourcegitcommit: 0f54f1b067f588d50f787fbfac50854a3a64fff7
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 08/12/2019
ms.locfileid: "67614748"
---
# <a name="best-practices-for-network-connectivity-and-security-in-azure-kubernetes-service-aks"></a>Best Practices für Netzwerkkonnektivität und Sicherheit in Azure Kubernetes Service (AKS)

Beim Erstellen und Verwalten von Clustern in Azure Kubernetes Service (AKS) stellen Sie die Netzwerkkonnektivität für Ihre Knoten und Anwendungen bereit. Die Netzwerkressourcen umfassen IP-Adressbereiche, Lastenausgleichsmodule und Controller für eingehenden Datenverkehr. Um eine hohe Servicequalität für Ihre Anwendungen zu gewährleisten, müssen Sie diese Ressourcen planen und konfigurieren.

In diesem Artikel zu bewährten Methoden liegt der Schwerpunkt auf der Netzwerkkonnektivität und der Sicherheit für Clusteroperatoren. In diesem Artikel werden folgende Vorgehensweisen behandelt:

> [!div class="checklist"]
> * Vergleichen der Netzwerkmodi „kubenet“ und „Azure CNI“ in AKS
> * Planen der erforderlichen IP-Adressierung und -Konnektivität
> * Verteilen von Datenverkehr mit Lastenausgleichsmodulen, Controllern für eingehenden Datenverkehr oder einer Web Application Firewall (WAF)
> * Herstellen sicherer Verbindungen mit Clusterknoten

## <a name="choose-the-appropriate-network-model"></a>Auswählen des geeigneten Netzwerkmodells

**Best Practice-Anleitung** – Für die Integration in bestehende virtuelle Netzwerke oder lokale Netzwerke verwenden Sie Azure CNI-Netzwerke in AKS. Dieses Netzwerkmodell ermöglicht zudem eine stärkere Trennung von Ressourcen und Steuerelementen in einer Unternehmensumgebung.

Virtuelle Netzwerke bieten die grundlegende Konnektivität für AKS-Knoten und Kunden für den Zugriff auf Ihre Anwendungen. Es gibt zwei verschiedene Möglichkeiten, AKS-Cluster in virtuellen Netzwerken bereitzustellen:

* **kubenet-Netzwerk**: Azure verwaltet die virtuellen Netzwerkressourcen während der Bereitstellung des Clusters und verwendet das Kubernetes-Plug-In [kubenet][kubenet].
* **Azure CNI-Netzwerk**: Führt die Bereitstellung in einem bestehenden virtuellen Netzwerk aus und verwendet das Kubernetes-Plug-In [Azure Container Networking Interface (CNI)][cni-networking]. Pods erhalten einzelne IP-Adressen, die an anderen Netzwerkdienste oder lokale Ressourcen weitergeleitet werden können.

Die Container Networking Interface (CNI) ist ein herstellerneutrales Protokoll, mit dem die Containerlaufzeit Anfragen an einen Netzwerkanbieter richten kann. Azure CNI weist Pods und Knoten IP-Adressen zu und stellt Features zur IP-Adressverwaltung (IPAM) bereit, wenn Sie die Verbindung zu bestehenden virtuellen Azure-Netzwerken herstellen. Jeder Knoten und jede Podressource erhält eine IP-Adresse im virtuellen Azure-Netzwerk, und es ist kein zusätzliches Routing erforderlich, um mit anderen Ressourcen oder Diensten zu kommunizieren.

![Diagramm mit zwei Knoten jeweils mit Bridges für die Verbindungsherstellung mit einem Azure VNET](media/operator-best-practices-network/advanced-networking-diagram.png)

Für die meisten Produktionsbereitstellungen sollten Sie Azure CNI-Netzwerke verwenden. Dieses Netzwerkmodell ermöglicht eine Trennung von Steuerung und Verwaltung von Ressourcen. Aus Sicherheitssicht möchten Sie diese Ressourcen oft von verschiedenen Teams verwalten und absichern lassen. Mit Azure CNI-Netzwerken können Sie die Verbindung zu vorhandenen Azure-Ressourcen, lokalen Ressourcen oder anderen Diensten direkt über die jedem Pod zugeordneten IP-Adressen herstellen.

Wenn Sie Azure CNI-Netzwerke verwenden, befindet sich die virtuelle Netzwerkressource in einer vom AKS-Cluster getrennten Ressourcengruppe. Delegieren Sie Berechtigungen für den AKS-Dienstprinzipal, um auf diese Ressourcen zuzugreifen und sie zu verwalten. Der vom AKS-Cluster verwendete Dienstprinzipal muss zumindest über Berechtigungen [Netzwerkmitwirkender](../role-based-access-control/built-in-roles.md#network-contributor) für das Subnetz in Ihrem virtuellen Netzwerk verfügen. Wenn Sie eine [benutzerdefinierte Rolle](../role-based-access-control/custom-roles.md) anstelle der integrierten Rolle des Netzwerkmitwirkenden definieren möchten, sind die folgenden Berechtigungen erforderlich:
  * `Microsoft.Network/virtualNetworks/subnets/join/action`
  * `Microsoft.Network/virtualNetworks/subnets/read`

Weitere Informationen zur AKS-Dienstprinzipaldelegierung finden Sie unter [Delegieren des Zugriffs auf andere Azure-Ressourcen][sp-delegation].

Da jeder Knoten und Pod seine eigene IP-Adresse erhält, planen Sie die Adressbereiche für die AKS-Subnetze. Das Subnetz muss groß genug sein, um IP-Adressen für jeden Knoten, jeden Pod und jede bereitgestellte Netzwerkressource zu bieten. Jeder AKS-Cluster muss in einem eigenen Subnetz platziert werden. Um die Konnektivität zu lokalen oder Peernetzwerken in Azure zu ermöglichen, sollten Sie keine IP-Adressbereiche verwenden, die sich mit bestehenden Netzwerkressourcen überschneiden. Es gibt Standardbegrenzungen für die Anzahl der Pods, die jeder Knoten in einem kubernet- bzw. Azure CNI-Netzwerk ausführen kann. Um Skalierungsereignisse oder Clusterupgrades behandeln zu können, benötigen Sie außerdem zusätzliche IP-Adressen, die für die Verwendung im zugewiesenen Subnetz zur Verfügung stehen. Dieser zusätzliche Adressraum ist besonders wichtig, wenn Sie Windows Server-Container (zurzeit als Vorschauversion in AKS) verwenden, da diese Knotenpools ein Upgrade erfordern, damit die neuesten Sicherheitspatches angewandt werden. Weitere Informationen zu Windows Server-Knoten finden Sie unter [Durchführen eines Upgrades für einen Knotenpool in AKS][nodepool-upgrade].

Informationen zum Berechnen der erforderlichen IP-Adresse finden Sie unter [Konfigurieren von Azure CNI-Netzwerken in AKS][advanced-networking].

### <a name="kubenet-networking"></a>Kubenet-Netzwerke

Für „kubernet“ ist es zwar nicht erforderlich, dass Sie die virtuellen Netzwerke vor der Bereitstellung des Clusters einrichten, aber es gibt Nachteile:

* Knoten und Pods werden in unterschiedliche IP-Subnetze platziert. Benutzerdefiniertes Routing (UDR) und IP-Weiterleitung werden verwendet, um den Datenverkehr zwischen Pods und Knoten zu routen. Dieses zusätzliche Routing kann die Netzwerkleistung reduzieren.
* Verbindungen zu bestehenden lokalen Netzwerken oder das Peering mit anderen virtuellen Netzwerken von Azure können komplex sein.

„Kubernet“ eignet sich für kleine Entwicklungs- oder Testworkloads, da Sie das virtuelle Netzwerk und die Subnetze nicht getrennt vom AKS-Cluster erstellen müssen. Einfache Websites mit geringem Datenverkehr oder das Verschieben von Workloads in Container per Lift & Shift können ebenfalls von der Einfachheit der mit kubernet-Netzwerken bereitgestellten AKS-Cluster profitieren. Für die meisten Produktionsbereitstellungen sollten Sie Azure CNI-Netzwerke planen und verwenden. Sie können auch [Ihre eigenen IP-Adressbereiche und virtuellen Netzwerke mit kubenet][aks-configure-kubenet-networking] konfigurieren.

## <a name="distribute-ingress-traffic"></a>Verteilen des Eingangsdatenverkehrs

**Best Practice-Anleitung** – Um HTTP- oder HTTPS-Datenverkehr an Ihre Anwendungen zu verteilen, verwenden Sie Eingangsressourcen und -controller. Eingangscontroller bieten zusätzliche Features gegenüber einem normalen Azure Load Balancer und können als native Kubernetes-Ressourcen verwaltet werden.

Ein Azure Load Balancer kann den Kundendatenverkehr auf Anwendungen in Ihrem AKS-Cluster verteilen, aber er ist in seinem Verständnis dieses Datenverkehrs eingeschränkt. Eine Lastenausgleichsressource wird auf Ebene 4 ausgeführt und verteilt den Datenverkehr basierend auf dem Protokoll oder den Ports. Die meisten Webanwendungen, die HTTP oder HTTPS verwenden, sollten Kubernetes-Eingangsressourcen und -controller verwenden, die auf Ebene 7 ausgeführt werden. Eingangsdatenverkehr kann den Datenverkehr basierend auf der URL der Anwendung verteilen und die TLS/SSL-Terminierung behandeln. Diese Fähigkeit reduziert auch die Anzahl der IP-Adressen, die Sie freigeben und zuordnen. Bei einem Load Balancer benötigt jede Anwendung typischerweise eine öffentliche IP-Adresse, die dem Dienst im AKS-Cluster zugewiesen und zugeordnet wird. Bei Verwendung einer Eingangsressource kann eine einzige IP-Adresse den Datenverkehr auf mehrere Anwendungen verteilen.

![Diagramm mit Eingangsdatenverkehrsfluss in einem AKS-Cluster](media/operator-best-practices-network/aks-ingress.png)

 Es gibt zwei Komponenten für den Eingangsdatenverkehr:

 * Eine *Eingangsressource* und
 * Ein *Eingangscontroller*

Die Eingangsressource ist ein YAML-Manifest von `kind: Ingress`, das den Host, die Zertifikate und die Regeln zum Weiterleiten von Datenverkehr an die in Ihrem AKS-Cluster ausgeführten Dienste definiert. Das folgende YAML-Beispielmanifest verteilt Datenverkehr für *myapp.com* an einen von zwei Diensten, *blogservice* oder *storeservice*. Der Kunde wird basierend auf der URL, auf die zugegriffen wird, an den einen oder anderen Dienst weitergeleitet.

```yaml
kind: Ingress
metadata:
 name: myapp-ingress
   annotations: kubernetes.io/ingress.class: "PublicIngress"
spec:
 tls:
 - hosts:
   - myapp.com
   secretName: myapp-secret
 rules:
   - host: myapp.com
     http:
      paths:
      - path: /blog
        backend:
         serviceName: blogservice
         servicePort: 80
      - path: /store
        backend:
         serviceName: storeservice
         servicePort: 80
```

Ein Eingangscontroller ist ein Daemon, der auf einem AKS-Knoten ausgeführt wird und diesen auf eingehende Anforderungen überwacht. Datenverkehr wird dann entsprechend den in der Eingangsressource definierten Regeln verteilt. Der am häufigsten verwendete Eingangscontroller basiert auf [NGINX]. AKS beschränkt Sie nicht auf einen bestimmten Controller, Sie können daher auch andere Controller wie [Contour][contour], [HAProxy][haproxy] oder [Traefik][traefik] verwenden.

Eingangscontroller müssen auf einem Linux-Knoten geplant werden. Windows Server-Knoten (derzeit in der Vorschau in AKS) dürfen nicht auf dem Eingangscontroller ausgeführt werden. Verwenden Sie einen Knotenselektor in Ihrem YAML-Manifest oder der Bereitstellung des Helm-Charts, um anzugeben, dass die Ressource auf einem Linux-basierten Knoten ausgeführt werden soll. Weitere Informationen finden Sie unter [Verwenden von Knotenselektoren, um zu steuern, wo Pods in AKS geplant werden][concepts-node-selectors].

Es gibt viele Szenarien für Eingangsdatenverkehr, einschließlich der folgenden Anleitungen:

* [Erstellen eines einfachen Eingangscontrollers mit Konnektivität mit einem externen Netzwerk][aks-ingress-basic]
* [Erstellen eines Eingangscontrollers, der ein internes, privates Netzwerk und eine IP-Adresse verwendet][aks-ingress-internal]
* [Erstellen eines Eingangscontrollers, der Ihre eigenen TLS-Zertifikate verwendet][aks-ingress-own-tls]
* Erstellen eines Eingangscontrollers, der Let's Encrypt für das automatische Generieren von TLS-Zertifikaten [mit einer dynamischen öffentlichen IP-Adresse][aks-ingress-tls] oder [mit einer statischen öffentlichen IP-Adresse][aks-ingress-static-tls] verwendet

## <a name="secure-traffic-with-a-web-application-firewall-waf"></a>Absichern des Datenverkehrs mit einer Web Application Firewall (WAF)

**Best Practices-Anleitung**: Verwenden Sie eine Web Application Firewall (WAF) wie [Barracuda WAF für Azure][barracuda-waf] oder Azure Application Gateway, um eingehenden Datenverkehr auf mögliche Angriffe zu überprüfen. Diese fortschrittlicheren Netzwerkressourcen können den Datenverkehr auch über reine HTTP- und HTTPS-Verbindungen oder eine einfache SSL-Terminierung hinaus routen.

Ein Eingangscontroller, der den Datenverkehr an Dienste und Anwendungen verteilt, ist normalerweise eine Kubernetes-Ressource in Ihrem AKS-Cluster. Der Controller wird als Daemon auf einem AKS-Knoten ausgeführt und verbraucht einige der Ressourcen des Knotens wie CPU, Speicher und Netzwerkbandbreite. In größeren Umgebungen möchten Sie einen Teil dieses Datenverkehrsroutings oder der TLS-Terminierung häufig auf eine Netzwerkressource außerhalb des AKS-Clusters auslagern. Zudem möchten Sie den eingehenden Datenverkehr auf mögliche Angriffe überprüfen.

![Eine Web Application Firewall (WAF) wie Azure App Gateway kann den Datenverkehr für Ihren AKS-Cluster schützen und verteilen.](media/operator-best-practices-network/web-application-firewall-app-gateway.png)

Eine Web Application Firewall (WAF) bietet eine zusätzliche Sicherheitsebene, indem sie den eingehenden Datenverkehr filtert. Das Open Web Application Security Project (OWASP) bietet eine Reihe von Regeln, um den Datenverkehr auf Angriffe wie websiteübergreifende Skripts oder Cookie-Poisoning zu überwachen. [Azure Application Gateway][app-gateway] (aktuell in AKS in der Vorschauphase) ist eine WAF, die in AKS-Cluster integriert werden kann, um diese Sicherheitsfeatures bereitzustellen, bevor der Datenverkehr Ihren AKS-Cluster und Ihre Anwendungen erreicht. Auch andere Lösungen von Drittanbietern bieten diese Funktionen, sodass Sie bestehende Investitionen in ein bestimmtes Produkt oder vorhandenes Know-how weiterhin nutzen können.

Load Balancer oder Eingangsressourcen werden in Ihrem AKS-Cluster weiterhin ausgeführt, um die Verteilung des Datenverkehrs weiter zu optimieren. App Gateway kann zentral als Eingangscontroller mit einer Ressourcendefinition verwaltet werden. [Erstellen Sie zunächst einen Application Gateway-Eingangscontroller.][app-gateway-ingress]

## <a name="control-traffic-flow-with-network-policies"></a>Steuern des Datenverkehrsflusses mit Netzwerkrichtlinien

**Best Practice-Anleitung**: Verwenden Sie Netzwerkrichtlinien, um Datenverkehr zu den Pods zuzulassen oder zu verweigern. Standardmäßig ist der gesamte Datenverkehr zwischen den Pods in einem Cluster zulässig. Aus Sicherheitsgründen sollten Sie Regeln definieren, um die Kommunikation zwischen den Pods einzuschränken.

Netzwerkrichtlinien sind ein Kubernetes-Feature, mit dem Sie den Datenverkehrsfluss zwischen Pods steuern können. Anhand von Einstellungen wie zugewiesene Bezeichnungen, Namespace oder Port für den Datenverkehr können Sie Datenverkehr zulassen oder verweigern. Die Verwendung von Netzwerkrichtlinien ist eine cloudnative Möglichkeit, den Datenverkehrsfluss zu steuern. Wenn Pods in einem AKS-Cluster dynamisch erstellt werden, können automatisch die erforderlichen Netzwerkrichtlinien angewendet werden. Verwenden Sie zum Steuern der Kommunikation zwischen den Pods keine Azure-Netzwerksicherheitsgruppen, sondern Netzwerkrichtlinien.

Um Netzwerkrichtlinien verwenden zu können, muss das Feature beim Erstellen eines AKS-Clusters aktiviert werden. Ohne einen vorhandenen AKS-Cluster können Sie keine Netzwerkrichtlinie aktivieren. Planen Sie im Voraus, und stellen Sie sicher, dass Sie in den Clustern Netzwerkrichtlinien aktivieren und diese bei Bedarf verwenden können. Netzwerkrichtlinien sollten nur für Linux-basierte Knoten und Pods in AKS verwendet werden.

Eine Netzwerkrichtlinie wird mit einem YAML-Manifest als Kubernetes-Ressource erstellt. Die Richtlinien werden auf definierte Pods angewendet. Regeln für den ein- und ausgehenden Datenverkehr definieren dann, wie der Datenverkehr fließen kann. Im folgenden Beispiel wird eine Netzwerkrichtlinie auf Pods mit der zugewiesenen Bezeichnung *app: backend* angewendet. Die Eingangsregel erlaubt dann nur Datenverkehr von Pods mit der Bezeichnung *app: frontend*:

```yaml
kind: NetworkPolicy
apiVersion: networking.k8s.io/v1
metadata:
  name: backend-policy
spec:
  podSelector:
    matchLabels:
      app: backend
  ingress:
  - from:
    - podSelector:
        matchLabels:
          app: frontend
```

Eine Anleitung für die ersten Schritte mit Richtlinien finden Sie unter [Sicherer Datenverkehr zwischen Pods durch Netzwerkrichtlinien in Azure Kubernetes Service (AKS)][use-network-policies].

## <a name="securely-connect-to-nodes-through-a-bastion-host"></a>Herstellen einer sicheren Verbindung zu Knoten über einen Bastionhost

**Best Practice-Anleitung** – Machen Sie für Ihre AKS-Knoten keine Remotekonnektivität verfügbar. Erstellen Sie einen Bastionhost oder eine Jumpbox in einem virtuellen Verwaltungsnetzwerk. Verwenden Sie den Bastionhost, um den Datenverkehr sicher in Ihren AKS-Cluster für Remoteverwaltungsaufgaben zu routen.

Die meisten Operationen in AKS können mit den Azure-Verwaltungstools oder über den Kubernetes API-Server ausgeführt werden. AKS-Knoten sind nicht mit dem öffentlichen Internet verbunden und nur in einem privaten Netzwerk verfügbar. Um eine Verbindung zu Knoten herzustellen und Wartungsarbeiten durchzuführen oder Probleme zu beheben, leiten Sie Ihre Verbindungen über einen Bastionhost oder eine Jumpbox weiter. Dieser Host sollte sich in einem separaten virtuellen Verwaltungsnetzwerk befinden, das sicher per Peering mit dem virtuellen AKS-Clusternetzwerk verbunden ist.

![Verbinden mit AKS-Knoten über einen Bastionhost oder eine Jumpbox](media/operator-best-practices-network/connect-using-bastion-host-simplified.png)

Auch das Verwaltungsnetzwerk für den Bastionhost sollte abgesichert sein. Verwenden Sie ein [Azure ExpressRoute][expressroute] oder [VPN-Gateway][vpn-gateway], um sich mit einem lokalen Netzwerk zu verbinden und den Zugriff über Netzwerksicherheitsgruppen zu steuern.

## <a name="next-steps"></a>Nächste Schritte

Dieser Artikel konzentriert sich auf Netzwerkkonnektivität und Sicherheit. Weitere Informationen zu Netzwerkgrundlagen in Kubernetes finden Sie unter [Netzwerkkonzepte für Anwendungen in Azure Kubernetes Service (AKS)][aks-concepts-network].

<!-- LINKS - External -->
[cni-networking]: https://github.com/Azure/azure-container-networking/blob/master/docs/cni.md
[kubenet]: https://kubernetes.io/docs/concepts/cluster-administration/network-plugins/#kubenet
[app-gateway-ingress]: https://github.com/Azure/application-gateway-kubernetes-ingress
[nginx]: https://www.nginx.com/products/nginx/kubernetes-ingress-controller
[contour]: https://github.com/heptio/contour
[haproxy]: https://www.haproxy.org
[traefik]: https://github.com/containous/traefik
[barracuda-waf]: https://www.barracuda.com/products/webapplicationfirewall/models/5

<!-- INTERNAL LINKS -->
[aks-concepts-network]: concepts-network.md
[sp-delegation]: kubernetes-service-principal.md#delegate-access-to-other-azure-resources
[expressroute]: ../expressroute/expressroute-introduction.md
[vpn-gateway]: ../vpn-gateway/vpn-gateway-about-vpngateways.md
[aks-ingress-internal]: ingress-internal-ip.md
[aks-ingress-static-tls]: ingress-static-ip.md
[aks-ingress-basic]: ingress-basic.md
[aks-ingress-tls]: ingress-tls.md
[aks-ingress-own-tls]: ingress-own-tls.md
[app-gateway]: ../application-gateway/overview.md
[use-network-policies]: use-network-policies.md
[advanced-networking]: configure-azure-cni.md
[aks-configure-kubenet-networking]: configure-kubenet.md
[concepts-node-selectors]: concepts-clusters-workloads.md#node-selectors
[nodepool-upgrade]: use-multiple-node-pools.md#upgrade-a-node-pool