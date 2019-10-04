---
title: Konfigurieren von Lastenausgleichs- und Ausgangsregeln mit Azure PowerShell
titlesuffix: Azure Load Balancer
description: In diesem Artikel wird gezeigt, wie Sie Lastenausgleichs- und Ausgangsregeln in Load Balancer Standard mithilfe von Azure PowerShell konfigurieren.
services: load-balancer
author: asudbring
ms.service: load-balancer
ms.topic: article
ms.date: 09/24/2019
ms.author: allensu
ms.openlocfilehash: 6e85d31e40e35b9d796fc66394a6eb446c7302ad
ms.sourcegitcommit: 55f7fc8fe5f6d874d5e886cb014e2070f49f3b94
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 09/25/2019
ms.locfileid: "71263057"
---
# <a name="configure-load-balancing-and-outbound-rules-in-standard-load-balancer-using-azure-powershell"></a>Konfigurieren von Lastenausgleichs- und Ausgangsregeln in Load Balancer Standard mithilfe von Azure PowerShell

In diesem Artikel erfahren Sie, wie Sie Ausgangsregeln in Load Balancer Standard mithilfe von Azure PowerShell konfigurieren.  

Anschließend enthält die Load Balancer-Ressource zwei Front-Ends mit zugeordneten Regeln: eines für eingehenden und eines für ausgehenden Datenverkehr.  Jedes Front-End verfügt über einen Verweis auf eine öffentliche IP-Adresse. In diesem Szenario werden für Eingangs- und Ausgangsdatenverkehr unterschiedliche öffentliche IP-Adressen verwendet.   Die Lastenausgleichsregel bietet nur Lastenausgleich für eingehenden Datenverkehr, und die Ausgangsregel steuert die für den virtuellen Computer angegebene NAT für ausgehenden Datenverkehr.  Dieser Artikel verwendet zwei separate Back-End-Pools: einen für den eingehenden und einen für den ausgehenden Datenverkehr, um die Leistungsfähigkeit zu veranschaulichen und Flexibilität für dieses Szenario zu ermöglichen.

[!INCLUDE [cloud-shell-try-it.md](../../includes/cloud-shell-try-it.md)] 

[!INCLUDE [updated-for-az](../../includes/updated-for-az.md)]

## <a name="connect-to-azure-account"></a>Verbinden mit Azure-Konto
Melden Sie sich mit dem Befehl [Connect-AzAccount](https://docs.microsoft.com/powershell/module/az.accounts/connect-azaccount?view=azps-2.5.0) bei Ihrem Azure-Abonnement an, und befolgen Sie die Anweisungen auf dem Bildschirm:
    
```azurepowershell-interactive
Connect-AzAccount
```
## <a name="create-resource-group"></a>Ressourcengruppe erstellen

Erstellen Sie mit [New-AzResourceGroup](https://docs.microsoft.com/powershell/module/az.resources/new-azresourcegroup?view=azps-2.6.0) eine Ressourcengruppe. Eine Azure-Ressourcengruppe ist ein logischer Container, in dem Azure-Ressourcen bereitgestellt und verwaltet werden.

Im folgenden Beispiel wird eine Ressourcengruppe namens *myresourcegroupoutbound* am Standort *eastus2* erstellt:

```azurepowershell-interactive
New-AzResourceGroup -Name myresourcegroupoutbound -Location eastus
```
## <a name="create-virtual-network"></a>Virtuelles Netzwerk erstellen
Erstellen Sie ein virtuelles Netzwerk namens *myvnetoutbound* mit dem Subnetz *mysubnetoutbound* in *myresourcegroupoutbound*, indem Sie die Befehle [New-AzVirtualNetwork](https://docs.microsoft.com/powershell/module/az.network/new-azvirtualnetwork?view=azps-2.6.0) und [New-AzVirtualNetworkSubnetConfig](https://docs.microsoft.com/powershell/module/az.network/new-azvirtualnetworksubnetconfig?view=azps-2.6.0) verwenden:

```azurepowershell-interactive
$subnet = New-AzVirtualNetworkSubnetConfig -Name mysubnetoutbound -AddressPrefix "192.168.0.0/24"

New-AzVirtualNetwork -Name myvnetoutbound -ResourceGroupName myresourcegroupoutbound -Location eastus -AddressPrefix "192.168.0.0/16" -Subnet $subnet
```

## <a name="create-inbound-public-ip-address"></a>Erstellen einer öffentlichen IP-Eingangsadresse 

Um über das Internet auf Ihre Web-App zugreifen zu können, benötigen Sie eine öffentliche IP-Adresse für den Lastenausgleich. Ein Load Balancer im Standard-Tarif unterstützt nur öffentliche IP-Standardadressen. Verwenden Sie [New-AzPublicIpAddress](https://docs.microsoft.com/powershell/module/az.network/new-azpublicipaddress?view=azps-2.6.0) zum Erstellen einer öffentlichen IP-Standardadresse namens *mypublicipinbound* in *myresourcegroupoutbound*.

```azurepowershell-interactive
$pubIPin = New-AzPublicIpAddress -ResourceGroupName myresourcegroupoutbound -Name mypublicipinbound -AllocationMethod Static -Sku Standard -Location eastus
```

## <a name="create-outbound-public-ip-address"></a>Erstellen einer öffentlichen Ausgangs-IP-Adresse 

Erstellen Sie mit [New-AzPublicIpAddress](https://docs.microsoft.com/powershell/module/az.network/new-azpublicipaddress?view=azps-2.6.0) eine IP-Standardadresse für die Ausgangskonfiguration des Load Balancer-Front-Ends.

```azurepowershell-interactive
$pubIPout = New-AzPublicIpAddress -ResourceGroupName myresourcegroupoutbound -Name mypublicipoutbound -AllocationMethod Static -Sku Standard -Location eastus
```

## <a name="create-azure-load-balancer"></a>Erstellen eines Azure Load Balancers

In diesem Abschnitt erfahren Sie, wie Sie die folgenden Komponenten des Lastenausgleichs erstellen und konfigurieren:
  - Front-End-IP-Adresse, die den eingehenden Netzwerkdatenverkehr für den Lastenausgleich empfängt
  - Back-End-Pool, an den die Front-End-IP-Adresse den Netzwerkdatenverkehr sendet, für den ein Lastenausgleich durchgeführt wurde
  - Back-End-Pool für ausgehende Verbindungen 
  - Integritätstest zum Ermitteln der Integrität der Back-End-VM-Instanzen
  - Eingangsregel des Lastenausgleichs, mit der definiert wird, wie der Datenverkehr auf die virtuellen Computer verteilt werden soll
  - Ausgangsregel des Lastenausgleichs, mit der definiert wird, wie der von den virtuellen Computern ausgehende Datenverkehr verteilt werden soll

### <a name="create-inbound-frontend-ip"></a>Erstellen einer IP-Adresse für das Eingangs-Front-End
Erstellen Sie mit [New-AzLoadBalancerFrontendIpConfig](https://docs.microsoft.com/powershell/module/az.network/new-azloadbalancerfrontendipconfig?view=azps-2.6.0) die IP-Konfiguration für das Ausgangs-Front-End der Load Balancer-Instanz. Diese IP-Konfiguration des Eingangs-Front-Ends wird *myfrontendinbound* genannt und der öffentlichen IP-Adresse *mypublicipinbound* zugeordnet.

```azurepowershell-interactive
$frontendIPin = New-AzLoadBalancerFrontendIPConfig -Name "myfrontendinbound" -PublicIpAddress $pubIPin
```
### <a name="create-outbound-frontend-ip"></a>Erstellen einer IP-Adresse für das Ausgangs-Front-End
Erstellen Sie mit [New-AzLoadBalancerFrontendIpConfig](https://docs.microsoft.com/powershell/module/az.network/new-azloadbalancerfrontendipconfig?view=azps-2.6.0) die IP-Konfiguration für das Ausgangs-Front-End der Load Balancer-Instanz. Diese IP-Konfiguration des Ausgangs-Front-Ends wird *myfrontendoutbound* genannt und der öffentlichen IP-Adresse *mypublicipoutbound* zugeordnet:

```azurepowershell-interactive
$frontendIPout = New-AzLoadBalancerFrontendIPConfig -Name "myfrontendoutbound" -PublicIpAddress $pubIPout
```
### <a name="create-inbound-backend-pool"></a>Erstellen eines Back-End-Eingangspools
Erstellen Sie den Back-End-Eingangspool für den Load Balancer mit [New-AzLoadBalancerBackendAddressPoolConfig](https://docs.microsoft.com/powershell/module/az.network/new-azloadbalancerbackendaddresspoolconfig?view=azps-2.6.0) mit dem Namen *bepoolinbound*:

```azurepowershell-interactive
$bepoolin = New-AzLoadBalancerBackendAddressPoolConfig -Name bepoolinbound
``` 

### <a name="create-outbound-backend-pool"></a>Erstellen eines Back-End-Ausgangspools
Erstellen Sie einen zusätzlichen Back-End-Adresspool, um die Ausgangskonnektivität für einen Pool von VMs mit [New-AzLoadBalancerBackendAddressPoolConfig](https://docs.microsoft.com/powershell/module/az.network/new-azloadbalancerbackendaddresspoolconfig?view=azps-2.6.0) mit dem Namen *bepooloutbound* zu definieren. Die Erstellung eines separaten ausgehenden Pools bietet maximale Flexibilität, aber Sie können diesen Schritt auslassen und auch nur *bepoolinbound* für den eingehenden Datenverkehr verwenden.  :

```azurepowershell-interactive
$bepoolout = New-AzLoadBalancerBackendAddressPoolConfig -Name bepooloutbound
``` 

### <a name="create-health-probe"></a>Erstellen eines Integritätstests

Ein Integritätstest überprüft alle VM-Instanzen, um sicherzustellen, dass sie Netzwerkdatenverkehr senden können. VM-Instanzen mit Fehlern beim Test werden aus dem Load Balancer entfernt, bis sie wieder online geschaltet werden und beim Test überprüft wurde, dass sie fehlerfrei sind. Erstellen Sie mit [New-AzLoadBalancerProbeConfig](https://docs.microsoft.com/powershell/module/az.network/new-azloadbalancerprobeconfig?view=azps-2.6.0) einen Integritätstest zur Überwachung der Integrität der virtuellen Computer. 

```azurepowershell-interactive
$probe = New-AzLoadBalancerProbeConfig -Name http -Protocol "http" -Port 80 -IntervalInSeconds 15 -ProbeCount 2 -RequestPath /
```
### <a name="create-load-balancing-rule"></a>Erstellen einer Lastenausgleichsregel

Mit einer Lastenausgleichsregel werden die Front-End-IP-Konfiguration für den eingehenden Datenverkehr und der Back-End-Pool zum Empfangen des Datenverkehrs zusammen mit dem erforderlichen Quell- und Zielport definiert. Erstellen Sie mit [New-AzLoadBalancerRuleConfig](https://docs.microsoft.com/powershell/module/az.network/new-azloadbalancerruleconfig?view=azps-2.6.0) eine Lastenausgleichsregel mit dem Namen *myinboundlbrule*, die an Port 80 des Front-End-Pools *myfrontendinbound* lauscht und den Netzwerkdatenverkehr nach erfolgtem Lastenausgleich an den Back-End-Adresspool *bepoolinbound* sendet, wobei ebenfalls Port 80 verwendet wird. 

>[!NOTE]
>Diese Lastenausgleichsregel deaktiviert die automatische Ausgangs-(S)NAT mit dem Parameter **-DisableOutboundSNAT**. Die Ausgangs-NAT wird nur von der Ausgangsregel bereitgestellt.

```azurepowershell-interactive
$inboundRule = New-AzLoadBalancerRuleConfig -Name inboundlbrule -FrontendIPConfiguration $frontendIPin -BackendAddressPool $bepoolin -Probe $probe -Protocol "Tcp" -FrontendPort 80 -BackendPort 80 -IdleTimeoutInMinutes 15 -EnableFloatingIP -LoadDistribution SourceIP -DisableOutboundSNAT
```

### <a name="create-outbound-rule"></a>Erstellen einer Ausgangsregel

Eine Ausgangsregel definiert die öffentliche Front-End-IP-Adresse, dargestellt durch das Front-End *myfrontendoutbound*, das für den gesamten ausgehenden NAT-Datenverkehr verwendet wird, sowie den Back-End-Pool, für den diese Regel gilt.  Erstellen Sie eine Ausgangsregel *myoutboundrule* für die Ausgangsnetzwerkübersetzung aller virtuellen Computer (NIC-IP-Konfigurationen) im Back-End-Pool *bepool*.  Durch den folgenden Befehl wird außerdem das Leerlaufzeitlimit für ausgehenden Datenverkehr von 4 in 15 Minuten geändert, und es werden 10000 SNAT-Ports anstelle von 1024 zugeordnet.  Weitere Informationen finden Sie unter [New-AzLoadBalancerOutboundRuleConfig](https://docs.microsoft.com/powershell/module/az.network/new-azloadbalanceroutboundruleconfig?view=azps-2.7.0).

```azurepowershell-interactive
 $outboundRule = New-AzLoadBalancerOutBoundRuleConfig -Name outboundrule -FrontendIPConfiguration $frontendIPout -BackendAddressPool $bepoolout -Protocol All -IdleTimeoutInMinutes 15 -AllocatedOutboundPort 10000
```
Wenn Sie keinen separaten Ausgangspool verwenden möchten, können Sie das Adresspoolargument im vorherigen Befehl ändern, um stattdessen *$bepoolin* anzugeben.  Es wird empfohlen, separate Pools zu verwenden, um die Flexibilität und Lesbarkeit der sich ergebenden Konfiguration sicherzustellen.

### <a name="create-load-balancer"></a>Erstellen eines Load Balancers

Erstellen Sie mit [New-AzLoadBalancer](https://docs.microsoft.com/powershell/module/az.network/new-azloadbalancer?view=azps-2.6.0) eine Load Balancer-Instanz mit der IP-Eingangsadresse mit dem Namen *lb*, die eine IP-Adresskonfiguration des Eingangs-Front-Ends und einen Back-End-Pool *bepoolinbound* enthält (mit Zuordnung zur öffentlichen IP-Adresse *mypublicipinbound*, die Sie im vorherigen Schritt erstellt haben).

```azurepowershell-interactive
New-AzLoadBalancer -Name lb -Sku Standard -ResourceGroupName myresourcegroupoutbound -Location eastus -FrontendIpConfiguration $frontendIPin,$frontendIPout -BackendAddressPool $bepoolin,$bepoolout -Probe $probe -LoadBalancingRule $inboundrule -OutboundRule $outboundrule 
```

Jetzt können Sie mit dem Hinzufügen Ihrer virtuellen Computer zum Back-End-Pool *bepoolinbound* __und__ *bepooloutbound* fortfahren, indem Sie die IP-Konfiguration der entsprechenden NIC-Ressourcen mit [Add-AzNetworkInterfaceIpConfig](https://docs.microsoft.com/cli/azure/network/lb/rule?view=azure-cli-latest) aktualisieren.

## <a name="clean-up-resources"></a>Bereinigen von Ressourcen

Wenn Sie die Ressourcen nicht mehr benötigen, führen Sie den Befehl [Remove-AzResourceGroup](https://docs.microsoft.com/powershell/module/az.resources/remove-azresourcegroup?view=azps-2.7.0) aus, um die Ressourcengruppe, den Load Balancer und alle zugehörigen Ressourcen zu löschen.

```azurepowershell-interactive 
  Remove-AzResourceGroup -Name myresourcegroupoutbound
```

## <a name="next-steps"></a>Nächste Schritte
In diesem Artikel haben Sie eine Load Balancer Standard-Instanz erstellt und Regeln für den beim Lastenausgleich eingehenden Datenverkehr konfiguriert. Zudem haben Sie einen Integritätstest für die virtuellen Computer im Back-End-Pool erstellt. Weitere Informationen zu Azure Load Balancer finden Sie in den Tutorials zu Azure Load Balancer.

> [!div class="nextstepaction"]
> [Azure Load Balancer-Tutorials](tutorial-load-balancer-standard-public-zone-redundant-portal.md)
