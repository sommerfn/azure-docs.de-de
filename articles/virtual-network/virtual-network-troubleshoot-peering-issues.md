---
title: Beheben von Problemen mit dem Peering virtueller Netzwerke
description: Schritte, mit denen sich die meisten Probleme eines Peerings virtueller Netzwerke beheben lassen
services: virtual-network
documentationcenter: na
author: v-miegge
manager: dcscontentpm
editor: ''
ms.assetid: 1a3d1e84-f793-41b4-aa04-774a7e8f7719
ms.service: virtual-network
ms.devlang: na
ms.topic: troubleshooting
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 08/28/2019
ms.author: kaushika
ms.openlocfilehash: fc01a20a077a1c624ed490600db919fe3197556c
ms.sourcegitcommit: 3e7646d60e0f3d68e4eff246b3c17711fb41eeda
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 09/11/2019
ms.locfileid: "70901394"
---
# <a name="troubleshoot-virtual-network-peering-issues"></a>Beheben von Problemen mit dem Peering virtueller Netzwerke

In diesem Handbuch zur Problembehandlung finden Sie Schritte zum Beheben der meisten Probleme eines [Peerings virtueller Netzwerke](https://docs.microsoft.com/azure/virtual-network/virtual-network-peering-overview).

![IMAGE](./media/virtual-network-troubleshoot-peering-issues/4489538_en_1.png)

## <a name="scenario-1-configure-virtual-network-peering-between-two-virtual-networks"></a>Szenario 1: Konfigurieren des Peerings zwischen zwei virtuellen Netzwerken.

Gehören die virtuellen Netzwerke zum selben Abonnement oder zu verschiedenen Abonnements?

### <a name="connection-type-1-the-virtual-networks-are-in-the-same-subscription"></a>Verbindungstyp 1: Die virtuellen Netzwerke gehören zum selben Abonnement.

Verwenden Sie die Methoden, die in den folgenden Artikeln beschrieben sind, um ein Peering für virtuelle Netzwerke zu konfigurieren, die zum selben Abonnement gehören:

* Befinden sich die virtuellen Netzwerke in **derselben Region**, führen Sie die Schritte zum [Erstellen eines Peerings für virtuelle Netzwerke im selben Abonnement](https://docs.microsoft.com/azure/virtual-network/virtual-network-manage-peering#create-a-peering) aus.
* Befinden sich die virtuellen Netzwerke in **verschiedenen Regionen**, führen Sie die Schritte zum Einrichten eines [globalen Peerings virtueller Netzwerke](https://docs.microsoft.com/azure/virtual-network/virtual-network-peering-overview) aus.  

> [!Note]
> Für die folgenden Ressourcen funktioniert Konnektivität über globales Peering virtueller Netzwerke (Globales VNet-Peering) nicht: 
>
> * Virtuelle Computer hinter Basic ILB SKU (Basic-Tarif mit internem Lastenausgleich)
> * Redis Cache (verwendet Basic ILB SKU)
> * Application Gateway (verwendet Basic ILB SKU)
> * Skalierungsgruppen (verwenden Basic ILB SKU)
> * Service Fabric-Cluster (verwenden Basic ILB SKU)
> * SQL Always On (verwendet Basic ILB SKU)
> * App Service-Umgebungen (verwenden Basic ILB SKU)
> * API Management (verwendet Basic ILB SKU)
> * Azure Active Directory Domain Services (AD DS) (verwendet Basic ILB SKU)

Weitere Informationen hierzu finden Sie unter [Anforderungen und Einschränkungen](https://docs.microsoft.com/azure/virtual-network/virtual-network-peering-overview#requirements-and-constraints) von globalem Peering.

### <a name="connection-type-2-the-virtual-networks-are-in-different-subscriptions-or-ad-tenants"></a>Verbindungstyp 2: Die virtuellen Netzwerke gehören zu unterschiedlichen Abonnements.

Um ein Peering virtueller Netzwerke in verschiedenen Abonnements oder Active Directory-Mandanten zu konfigurieren, führen Sie die Schritte unter [Erstellen eines Peerings für unterschiedliche Abonnements über Azure CLI](https://docs.microsoft.com/azure/virtual-network/create-peering-different-subscriptions#cli) aus.

> [!Note]
> Damit Sie ein Netzwerkpeering konfigurieren können, benötigen Sie in beiden Abonnements die Berechtigung **Netzwerkmitwirkender**. Weitere Informationen hierzu finden Sie unter [Peering-Berechtigungen](https://docs.microsoft.com/azure/virtual-network/virtual-network-manage-peering%23permissions).

## <a name="scenario-2-configure-virtual-network-peering-with-hub-spoke-topology-that-uses-on-premises-resources"></a>Szenario 2: Konfigurieren eines Peerings virtueller Netzwerke mit Hub-Spoke-Topologie, in der lokale Ressourcen verwendet werden

![IMAGE](./media/virtual-network-troubleshoot-peering-issues/4488712_en_1a.png)

### <a name="connection-type-1-for-site-to-site-connection-or-expressroute-connection"></a>Verbindungstyp 1: Für Standort-zu-Standort-Verbindung oder ExpressRoute-Verbindung

Führen Sie die Schritte aus, die im folgenden Artikel aufgeführt sind: [Konfigurieren des VPN-Gatewaytransits für ein Peering virtueller Netzwerke](https://docs.microsoft.com/azure/vpn-gateway/vpn-gateway-peering-gateway-transit?toc=/azure/virtual-network/toc.json).

### <a name="connection-type-2-for-point-to-site-connections"></a>Verbindungstyp 2: Für Punkt-zu-Standort-Verbindungen

1. Führen Sie die Schritte aus, die im folgenden Artikel aufgeführt sind: [Konfigurieren des VPN-Gatewaytransits für ein Peering virtueller Netzwerke](https://docs.microsoft.com/azure/vpn-gateway/vpn-gateway-peering-gateway-transit?toc=/azure/virtual-network/toc.json).
2. Nachdem das Peering virtueller Netzwerke eingerichtet oder geändert wurde, muss das Punkt-zu-Standort-Paket heruntergeladen und erneut installiert werden, damit die Punkt-zu-Standort-Clients die aktualisierten Routen zum virtuellen Spoke-Netzwerk erhalten.

## <a name="scenario-3-configure-virtual-network-peering-with-hub-spoke-topology-for-azure-virtual-network"></a>Szenario 3: Konfigurieren eines Peerings virtueller Netzwerke mit Hub-Spoke-Topologie für Azure Virtual Network

![IMAGE](./media/virtual-network-troubleshoot-peering-issues/4488712_en_1b.png)

### <a name="connection-type-1-the-virtual-networks-are-in-the-same-region"></a>Verbindungstyp 1: Die virtuellen Netzwerke gehören zur selben Region.

Sie müssen eine virtuelle Netzwerkappliance (Network Virtual Appliance, NVA) im virtuellen Hub-Netzwerk konfigurieren, und Sie müssen in den virtuellen Spoke-Netzwerken benutzerdefinierte Routen haben, auf die „Network Virtual Appliance“ als nächster Hop angewendet ist. Weitere Informationen finden Sie unter [Dienstverkettung](https://docs.microsoft.com/azure/virtual-network/virtual-network-peering-overview#service-chaining).

> [!Note]
> Wenn Sie Hilfe für das Einrichten einer NVA benötigen, [wenden Sie sich an den NVA-Anbieter](https://support.microsoft.com/help/2984655/support-for-azure-market-place-for-virtual-machines).

Informationen zur Problembehandlung beim Einrichten einer NVA und zum Routing finden Sie unter [Probleme mit virtuellen Netzwerkappliances in Azure](https://docs.microsoft.com/azure/virtual-network/virtual-network-troubleshoot-nva).

### <a name="connection-type-2-the-virtual-networks-are-in-different-regions"></a>Verbindungstyp 2: Die virtuellen Netzwerke gehören zu verschiedenen Regionen.

Datenverkehr über globales VNet-Peering wird nicht unterstützt. Für die folgenden Ressourcen funktioniert Konnektivität über globales Peering virtueller Netzwerke (Globales VNet-Peering) nicht:

* Virtuelle Computer hinter Basic ILB SKU (Basic-Tarif mit internem Lastenausgleich)
* Redis Cache (verwendet Basic ILB SKU)
* Application Gateway (verwendet Basic ILB SKU)
* Skalierungsgruppen (verwenden Basic ILB SKU)
* Service Fabric-Cluster (verwenden Basic ILB SKU)
* SQL Always On (verwendet Basic ILB SKU)
* App Service-Umgebungen (verwenden Basic ILB SKU)
* API Management (verwendet Basic ILB SKU)
* Azure Active Directory Domain Services (AD DS) (verwendet Basic ILB SKU)

Informationen zu den Anforderungen und Einschränkungen für globales Peering finden Sie unter [Peering virtueller Netzwerke: Anforderungen und Einschränkungen](https://docs.microsoft.com/azure/virtual-network/virtual-network-peering-overview#requirements-and-constraints).

## <a name="scenario-4-i-have-a-connectivity-issue-between-two-peered-virtual-networks"></a>Szenario 4: Konnektivitätsproblem zwischen zwei virtuellen Netzwerken, die über Peering verbunden sind

Melden Sie sich beim [Azure-Portal](https://portal.azure.com/) mit einem Konto an, dem die erforderlichen [Rollen und Berechtigungen](https://docs.microsoft.com/azure/virtual-network/virtual-network-manage-peering#roles-permissions) zugewiesen sind. Wählen Sie das virtuelle Netzwerk aus, wählen Sie **Peering** aus, und überprüfen Sie dann das Feld **Status**. Welcher Status wird angezeigt?

### <a name="connection-type-1-the-peering-status-shows-connected"></a>Verbindungstyp 1: Als Status für Peering wird „Verbunden“ angezeigt.

Um das Problem zu beheben, führen Sie die folgenden Schritte aus:

1. Überprüfen Sie den Netzwerkdatenverkehr:

   Verwenden Sie [Problembehandlung für Verbindung](https://docs.microsoft.com/azure/network-watcher/network-watcher-connectivity-overview) und [Überprüfen des IP-Flusses](https://docs.microsoft.com/azure/network-watcher/network-watcher-ip-flow-verify-overview) vom virtuellen Quellcomputer zum virtuellen Zielcomputer, um zu bestimmen, ob es eine Netzwerksicherheitsgruppe oder benutzerdefinierte Route (User Defined Route, UDR) gibt, die zu Störungen im Datenverkehr führt.

   Wenn Sie eine Firewall oder eine NVA-Appliance verwenden, führen Sie die folgenden Schritte aus: 
   1. Dokumentieren Sie die UDR-Parameter, damit Sie diese nach Abschluss dieses Schritts wiederherstellen können.
   2. Entfernen Sie die UDR im virtuellen Quellcomputer aus dessen Subnetz oder Netzwerkadapter, in dem auf die NVA als nächster Hop verweisen wird. Überprüfen Sie die Verbindung vom virtuellen Quellcomputer mit dem Ziel direkt, d. h., die Verbindung umgeht die NVA. Wenn dieser Schritt funktioniert, lesen Sie [Probleme mit virtuellen Netzwerkappliances in Azure](https://docs.microsoft.com/azure/virtual-network/virtual-network-troubleshoot-nva).

2. Führen Sie eine Netzwerküberwachung aus: 
   1. Starten Sie eine Netzwerküberwachung auf dem virtuellen Zielcomputer. Unter Windows können Sie **Netsh** verwenden. Unter Linux verwenden Sie **TCPDump**.
   2. Führen Sie **TcpPing** oder **PsPing** von der Quell- zur Ziel-IP aus.

   * Dies ist ein Beispiel für einen **TcpPing**-Befehl: `tcping64.exe -t <destination VM address> 3389`

   3. Nachdem der **TcpPing**-Befehl abgeschlossen ist, beenden Sie die Netzwerküberwachung auf dem Zielcomputer.
   4. Wenn Pakete von der Quelle eintreffen, gibt es kein Netzwerkproblem. Überprüfen Sie sowohl die Firewall des virtuellen Computers als auch die an diesem Port lauschende Anwendung, um das Konfigurationsproblem zu finden.

   > [!Note]
   > Über globales Peering virtueller Netzwerke (virtuelle Netzwerke in unterschiedlichen Regionen) können Sie keine Verbindungen mit den folgenden Ressourcentypen herstellen:
   >
   > * Virtuelle Computer hinter Basic ILB SKU (Basic-Tarif mit internem Lastenausgleich)
   > * Redis Cache (verwendet Basic ILB SKU)
   > * Application Gateway (verwendet Basic ILB SKU)
   > * Skalierungsgruppen (verwenden Basic ILB SKU)
   > * Service Fabric-Cluster (verwenden Basic ILB SKU)
   > * SQL Always On (verwendet Basic ILB SKU)
   > * App Service-Umgebungen (verwenden Basic ILB SKU)
   > * API Management (verwendet Basic ILB SKU)
   > * Azure Active Directory Domain Services (AD DS) (verwendet Basic ILB SKU)

Weitere Informationen hierzu finden Sie unter [Anforderungen und Einschränkungen](https://docs.microsoft.com/azure/virtual-network/virtual-network-peering-overview#requirements-and-constraints) von globalem Peering.

### <a name="connection-type-2-the-peering-status-shows-disconnected"></a>Verbindungstyp 2: Als Status für Peering wird „Getrennt“ angezeigt.

Sie müssen die Peerings aus beiden virtuellen Netzwerken löschen und die Peerings dann neu erstellen.

## <a name="scenario-5-i-have-a-connectivity-issue-between-a-hub-spoke-virtual-network-and-on-premises-resource"></a>Szenario 5: Konnektivitätsproblem zwischen einem virtuellen Hub-Spoke-Netzwerk und einer lokalen Ressource

Wird eine NVA oder ein VPN-Gateway eines Drittanbieters verwendet?

### <a name="connection-type-1-my-network-uses-a-third-party-nva-or-vpn-gateway"></a>Verbindungstyp 1: In dem Netzwerk wird eine NVA oder ein VPN-Gateway eines Drittanbieters verwendet.

Informationen zum Beheben von Konnektivitätsproblemen, die sich auf eine NVA oder ein VPN-Gateway eines Drittanbieters auswirken, finden Sie in den folgenden Artikeln:

* [Probleme mit virtuellen Netzwerkappliances in Azure](https://docs.microsoft.com/azure/virtual-network/virtual-network-troubleshoot-nva)
* [Dienstverkettung](https://docs.microsoft.com/azure/virtual-network/virtual-network-peering-overview#service-chaining)

### <a name="connection-type-2-my-network-does-not-a-third-party-nva-or-vpn-gateway"></a>Verbindungstyp 2: In dem Netzwerk wird weder eine NVA noch ein VPN-Gateway eines Drittanbieters verwendet.

Haben sowohl das virtuelle Hub- als auch das virtuelle Spoke-Netzwerk ein VPN-Gateway?

#### <a name="both-the-hub-and-spoke-virtual-networks-have-a-vpn-gateway"></a>Sowohl das virtuelle Hub- als auch das virtuelle Spoke-Netzwerk haben ein VPN-Gateway.

Die Verwendung eines Remotegateways wird nicht unterstützt.

Aufgrund einer Einschränkung des VNet-Peerings wird **Remotegateways verwenden** im virtuellen Spoke-Netzwerk nicht unterstützt, wenn das virtuelle Spoke-Netzwerk bereits ein VPN-Gateway hat.

#### <a name="both-the-hub-and-spoke-virtual-networks-dont-have-a-vpn-gateway"></a>Sowohl das virtuelle Hub- als auch das virtuelle Spoke-Netzwerk haben kein VPN-Gateway.

Überprüfen Sie für Standort-zu-Standort- oder ExpressRoute-Verbindungen diese Hauptursachen für Konnektivitätsprobleme mit dem virtuellen Remotenetzwerk von einem lokalen Standort aus.

* Vergewissern Sie sich, dass das Kontrollkästchen **Weitergeleiteten Datenverkehr zulassen** für das virtuelle Netzwerk aktiviert ist, das ein Gateway hat.
* Vergewissern Sie sich, dass das Kontrollkästchen **Remotegateways verwenden** für das virtuelle Netzwerk aktiviert ist, das kein Gateway hat.
* Veranlassen Sie, dass der Netzwerkadministrator Ihre lokalen Geräte überprüft, um sicherzustellen, dass allen Geräten Adressraum des virtuellen Remotenetzwerks hinzugefügt wurde.

Für Punkt-zu-Standort-Verbindungen:

* Vergewissern Sie sich, dass das Kontrollkästchen **Weitergeleiteten Datenverkehr zulassen** für das virtuelle Netzwerk aktiviert ist, das ein Gateway hat.
* Vergewissern Sie sich, dass das Kontrollkästchen **Remotegateways verwenden** für das virtuelle Netzwerk aktiviert ist, das kein Gateway hat.
* Laden Sie das Punkt-zu-Standort-Clientpaket herunter, und installieren Sie es erneut. Für Routen in virtuellen Netzwerken, für die ein erneutes Peering vorgenommen wird, werden nicht automatisch Routen zu Punkt-zu-Standort-Clients hinzugefügt.

## <a name="scenario-6-i-have-a-hub-spoke-network-connectivity-issue-between-spoke-virtual-networks-in-the-same-region"></a>Szenario 6: Hub-Spoke-Netzwerkverbindungsproblem zwischen virtuellen Spoke-Netzwerken in derselben Region

Sie benötigen eine NVA in einem Hub-Netzwerk, Sie müssen UDRs in Spokes konfigurieren, für die eine NVA als nächster Hop festgelegt ist, und Sie müssen **Weitergeleiteten Datenverkehr zulassen** im virtuellen Hub-Netzwerk aktivieren.

Weitere Informationen finden Sie unter [Dienstverkettung](https://docs.microsoft.com/azure/virtual-network/virtual-network-peering-overview#service-chaining), und besprechen Sie diese Anforderungen mit dem jeweiligen [NVA-Anbieter](https://support.microsoft.com/help/2984655/support-for-azure-market-place-for-virtual-machines).

## <a name="scenario-7-i-have-a-hub-spoke-network-connectivity-issue-between-spoke-virtual-networks-in-different-regions"></a>Szenario 7: Hub-Spoke-Netzwerkverbindungsproblem zwischen virtuellen Spoke-Netzwerken in unterschiedlichen Regionen

Datenverkehr über globales VNet-Peering wird nicht unterstützt. Für die folgenden Ressourcen funktioniert Konnektivität über globales Peering virtueller Netzwerke (Globales VNet-Peering) nicht:

* Virtuelle Computer hinter Basic ILB SKU (Basic-Tarif mit internem Lastenausgleich)
* Redis Cache (verwendet Basic ILB SKU)
* Application Gateway (verwendet Basic ILB SKU)
* Skalierungsgruppen (verwenden Basic ILB SKU)
* Service Fabric-Cluster (verwenden Basic ILB SKU)
* SQL Always On (verwendet Basic ILB SKU)
* App Service-Umgebungen (verwenden Basic ILB SKU)
* API Management (verwendet Basic ILB SKU)
* Azure Active Directory Domain Services (AD DS) (verwendet Basic ILB SKU)

Weitere Informationen hierzu finden Sie unter [Anforderungen und Einschränkungen](https://docs.microsoft.com/azure/virtual-network/virtual-network-peering-overview#requirements-and-constraints) von globalem Peering und unter [Hub&Spoke, Daisy-Chain and Full-Mesh VNET topologies in Azure ARM](https://blogs.msdn.microsoft.com/igorpag/2016/02/11/hubspoke-daisy-chain-and-full-mesh-vnet-topologies-in-azure-arm-v2/).

## <a name="scenario-8-i-have-a-hub-spoke-network-connectivity-issue-between-a-web-app-and-the-spoke-virtual-network"></a>Szenario 8: Hub-Spoke-Netzwerkverbindungsproblem zwischen einer Web-App und dem virtuellen Spoke-Netzwerk

Um dieses Problem zu beheben, führen Sie die folgenden Schritte aus:

1. Melden Sie sich beim Azure-Portal an. Wechseln Sie zur Web-App, wählen Sie **Netzwerk** aus, und wählen Sie dann **VNET-Integration** aus.
2. Überprüfen Sie, ob das virtuelle Remotenetzwerk angezeigt wird. Geben Sie den Adressraum des virtuellen Remotenetzwerks manuell ein (**Netzwerk synchronisieren** und **Route hinzufügen**).

Weitere Informationen finden Sie in den folgenden Artikeln:

* [Integrieren Ihrer App in ein Azure Virtual Network](https://docs.microsoft.com/azure/app-service/web-sites-integrate-with-vnet)
* [Informationen zu Point-to-Site-VPN-Routing](https://docs.microsoft.com/azure/vpn-gateway/vpn-gateway-about-point-to-site-routing)

## <a name="scenario-9-i-receive-an-error-when-configuring-virtual-network-peering"></a>Szenario 9: Fehler beim Konfigurieren des Peerings virtueller Netzwerke

### <a name="error-1-current-tenant-tenant-id-isnt-authorized-to-access-linked-subscription"></a>Fehler 1: Der aktuelle Mandant `<TENANT ID>` ist nicht für den Zugriff auf verknüpfte Abonnements autorisiert.

Um dieses Problem zu beheben, führen Sie die Schritte unter [Erstellen eines Peerings: Azure CLI](https://docs.microsoft.com/azure/virtual-network/create-peering-different-subscriptions#cli) aus.

### <a name="error-2-not-connected"></a>Fehler 2: Nicht verbunden

Sie müssen die Peerings aus beiden virtuellen Netzwerken löschen und die Peerings neu erstellen.

### <a name="error-3-failed-to-peer-a-databricks-virtual-network"></a>Fehler 3: Fehler beim Peering eines virtuellen Databricks-Netzwerks

Um dieses Problem zu beheben, konfigurieren Sie das Peering virtueller Netzwerke über das Blatt **Azure Databricks**, und geben Sie dann das virtuelle Zielnetzwerk über die **Ressourcen-ID** an. Weitere Informationen finden Sie unter [Peer a Databricks virtual network to a remote virtual network](https://docs.azuredatabricks.net/administration-guide/cloud-configurations/azure/vnet-peering.html#id2).

## <a name="next-steps"></a>Nächste Schritte

* [Problembehandlung bei Konnektivitätsproblemen zwischen Azure-VMs](https://docs.microsoft.com/azure/virtual-network/virtual-network-troubleshoot-connectivity-problem-between-vms)