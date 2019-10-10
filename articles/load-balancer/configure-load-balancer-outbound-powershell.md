---
title: Konfigurieren von Lastenausgleichs- und Ausgangsregeln mithilfe von Azure PowerShell
titlesuffix: Azure Load Balancer
description: In diesem Artikel wird gezeigt, wie Sie Lastenausgleichs- und Ausgangsregeln in Load Balancer Standard mithilfe von Azure PowerShell konfigurieren.
services: load-balancer
author: asudbring
ms.service: load-balancer
ms.topic: article
ms.date: 09/24/2019
ms.author: allensu
ms.openlocfilehash: b621d56a76a51f9ce9df10f88f5c3f2f4babfc03
ms.sourcegitcommit: 80da36d4df7991628fd5a3df4b3aa92d55cc5ade
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 10/02/2019
ms.locfileid: "71816029"
---
# <a name="configure-load-balancing-and-outbound-rules-in-standard-load-balancer-by-using-azure-powershell"></a>Konfigurieren von Lastenausgleichs- und Ausgangsregeln in Load Balancer Standard mithilfe von Azure PowerShell

In diesem Artikel erfahren Sie, wie Sie Ausgangsregeln in Load Balancer Standard mithilfe von Azure PowerShell konfigurieren.  

Wenn Sie das Szenario dieses Artikels abgeschlossen haben, enthält die Load Balancer-Ressource zwei Front-Ends und die ihnen zugeordneten Regeln. Sie haben ein Front-End für eingehenden und ein weiteres Front-End für ausgehenden Datenverkehr.  

Jedes Front-End verweist auf eine öffentliche IP-Adresse. In diesem Szenario unterscheidet sich die öffentliche IP-Adresse für eingehenden Datenverkehr von der Adresse für ausgehenden Datenverkehr.   Die Lastenausgleichsregel bietet nur eingehenden Lastenausgleich. Die Ausgangsregel steuert die ausgehende Netzwerkadressenübersetzung (NAT) für die VM.  

Das Szenario verwendet zwei Back-End-Pools: einen für eingehenden Datenverkehr und einen für ausgehenden Datenverkehr. Diese Pools veranschaulichen die Funktion und stellen dem Szenario Flexibilität zur Verfügung.

[!INCLUDE [cloud-shell-try-it.md](../../includes/cloud-shell-try-it.md)] 

[!INCLUDE [updated-for-az](../../includes/updated-for-az.md)]

## <a name="connect-to-your-azure-account"></a>Herstellen einer Verbindung mit Ihrem Azure-Konto
Melden Sie sich mithilfe des Befehls [Connect-AzAccount](https://docs.microsoft.com/powershell/module/az.accounts/connect-azaccount?view=azps-2.5.0) bei Ihrem Azure-Abonnement an. Befolgen Sie dann die Anweisungen auf dem Bildschirm.
    
```azurepowershell-interactive
Connect-AzAccount
```
## <a name="create-a-resource-group"></a>Erstellen einer Ressourcengruppe

Erstellen Sie eine Ressourcengruppe mithilfe von [New-AzResourceGroup](https://docs.microsoft.com/powershell/module/az.resources/new-azresourcegroup?view=azps-2.6.0). Eine Azure-Ressourcengruppe ist ein logischer Container, in dem Azure-Ressourcen bereitgestellt werden. Die Ressourcen werden dann von der Gruppe verwaltet.

Im folgenden Beispiel wird eine Ressourcengruppe namens *myresourcegroupoutbound* am Standort *eastus2* erstellt:

```azurepowershell-interactive
New-AzResourceGroup -Name myresourcegroupoutbound -Location eastus
```
## <a name="create-a-virtual-network"></a>Erstellen eines virtuellen Netzwerks
Erstellen Sie ein virtuelles Netzwerk mit dem Namen *myvnetoutbound*. Benennen Sie sein Subnetz *mysubnetoutbound*. Platzieren Sie es mithilfe von [New-AzVirtualNetwork](https://docs.microsoft.com/powershell/module/az.network/new-azvirtualnetwork?view=azps-2.6.0) und [New-AzVirtualNetworkSubnetConfig](https://docs.microsoft.com/powershell/module/az.network/new-azvirtualnetworksubnetconfig?view=azps-2.6.0) in *myresourcegroupoutbound*.

```azurepowershell-interactive
$subnet = New-AzVirtualNetworkSubnetConfig -Name mysubnetoutbound -AddressPrefix "192.168.0.0/24"

New-AzVirtualNetwork -Name myvnetoutbound -ResourceGroupName myresourcegroupoutbound -Location eastus -AddressPrefix "192.168.0.0/16" -Subnet $subnet
```

## <a name="create-an-inbound-public-ip-address"></a>Erstellen einer öffentlichen IP-Eingangsadresse 

Um über das Internet auf Ihre Web-App zugreifen zu können, benötigen Sie eine öffentliche IP-Adresse für den Lastenausgleich. Der Load Balancer Standard unterstützt nur öffentliche Standard-IP-Adressen. 

Verwenden Sie [New-AzPublicIpAddress](https://docs.microsoft.com/powershell/module/az.network/new-azpublicipaddress?view=azps-2.6.0) zum Erstellen einer öffentlichen IP-Standardadresse namens *mypublicipinbound* in *myresourcegroupoutbound*.

```azurepowershell-interactive
$pubIPin = New-AzPublicIpAddress -ResourceGroupName myresourcegroupoutbound -Name mypublicipinbound -AllocationMethod Static -Sku Standard -Location eastus
```

## <a name="create-an-outbound-public-ip-address"></a>Erstellen einer öffentlichen IP-Ausgangsadresse 

Erstellen Sie mithilfe von [New-AzPublicIpAddress](https://docs.microsoft.com/powershell/module/az.network/new-azpublicipaddress?view=azps-2.6.0) eine IP-Standardadresse für die Front-End-Ausgangskonfiguration des Load Balancers.

```azurepowershell-interactive
$pubIPout = New-AzPublicIpAddress -ResourceGroupName myresourcegroupoutbound -Name mypublicipoutbound -AllocationMethod Static -Sku Standard -Location eastus
```

## <a name="create-an-azure-load-balancer"></a>Erstellen einer Azure Load Balancer-Instanz

In diesem Abschnitt wird erläutert, wie Sie die folgenden Komponenten des Lastenausgleichs erstellen und konfigurieren:
  - Eine Front-End-IP-Adresse, die den eingehenden Netzwerkdatenverkehr für den Lastenausgleich empfängt
  - Einen Back-End-IP-Pool, an den die Front-End-IP den Netzwerkdatenverkehr nach dem Lastenausgleich sendet
  - Einen Back-End-Pool für ausgehende Verbindungen
  - Einen Integritätstest zum Ermitteln der Integrität der Back-End-VM-Instanzen
  - Eine Eingangsregel für den Lastenausgleich, mit der definiert wird, wie der Datenverkehr auf die virtuellen Computer verteilt werden soll
  - Eine Ausgangsregel für den Lastenausgleich, mit der definiert wird, wie der von den virtuellen Computern ausgehende Datenverkehr verteilt werden soll

### <a name="create-an-inbound-front-end-ip"></a>Erstellen einer Front-End-Eingangs-IP
Erstellen Sie die Front-End-IP-Eingangskonfiguration für den Lastenausgleich mithilfe von [New-AzLoadBalancerFrontendIpConfig](https://docs.microsoft.com/powershell/module/az.network/new-azloadbalancerfrontendipconfig?view=azps-2.6.0). Der Load Balancer sollte eine Front-End-IP-Eingangskonfiguration mit dem Namen *myfrontendinbound* beinhalten. Ordnen Sie diese Konfiguration der öffentlichen IP-Adresse *mypublicipinbound* zu.

```azurepowershell-interactive
$frontendIPin = New-AzLoadBalancerFrontendIPConfig -Name "myfrontendinbound" -PublicIpAddress $pubIPin
```
### <a name="create-an-outbound-front-end-ip"></a>Erstellen einer Front-End-Ausgangs-IP
Erstellen Sie die Front-End-IP-Ausgangskonfiguration für den Lastenausgleich mithilfe von [New-AzLoadBalancerFrontendIpConfig](https://docs.microsoft.com/powershell/module/az.network/new-azloadbalancerfrontendipconfig?view=azps-2.6.0). Dieser Load Balancer sollte eine Front-End-IP-Ausgangskonfiguration mit dem Namen *myfrontendoutbound* beinhalten. Ordnen Sie diese Konfiguration der öffentlichen IP-Adresse *mypublicipoutbound* zu.

```azurepowershell-interactive
$frontendIPout = New-AzLoadBalancerFrontendIPConfig -Name "myfrontendoutbound" -PublicIpAddress $pubIPout
```
### <a name="create-an-inbound-back-end-pool"></a>Erstellen eines Back-End-Eingangspools
Erstellen Sie den Back-End-Eingangspool für den Lastenausgleich mithilfe von [New-AzLoadBalancerBackendAddressPoolConfig](https://docs.microsoft.com/powershell/module/az.network/new-azloadbalancerbackendaddresspoolconfig?view=azps-2.6.0). Benennen Sie den Pool *bepoolinbound*.

```azurepowershell-interactive
$bepoolin = New-AzLoadBalancerBackendAddressPoolConfig -Name bepoolinbound
``` 

### <a name="create-an-outbound-back-end-pool"></a>Erstellen eines Back-End-Ausgangspools
Verwenden Sie den folgenden Befehl, um einen weiteren Back-End-Adresspool zu erstellen, um mithilfe von [New-AzLoadBalancerBackendAddressPoolConfig](https://docs.microsoft.com/powershell/module/az.network/new-azloadbalancerbackendaddresspoolconfig?view=azps-2.6.0) die Ausgangsverbindungen für einen Pool aus VMs zu definieren. Benennen Sie diesen Pool *bepooloutbound*. 

Durch Erstellen eines separaten Ausgangspools bieten Sie maximale Flexibilität. Wenn Sie dies vorziehen, können Sie diesen Schritt aber auch auslassen und nur den eingehenden *bepoolinbound* verwenden.  

```azurepowershell-interactive
$bepoolout = New-AzLoadBalancerBackendAddressPoolConfig -Name bepooloutbound
``` 

### <a name="create-a-health-probe"></a>Erstellen eines Integritätstests

Ein Integritätstest überprüft alle VM-Instanzen, um sicherzustellen, dass sie Netzwerkdatenverkehr senden können. VM-Instanzen mit Fehlern beim Test werden aus dem Lastenausgleich entfernt, bis sie wieder online geschaltet werden und ein Test ergibt, dass sie fehlerfrei sind. 

Um die Integrität der VMs zu überwachen, erstellen Sie einen Integritätstest mithilfe von [New-AzLoadBalancerProbeConfig](https://docs.microsoft.com/powershell/module/az.network/new-azloadbalancerprobeconfig?view=azps-2.6.0). 

```azurepowershell-interactive
$probe = New-AzLoadBalancerProbeConfig -Name http -Protocol "http" -Port 80 -IntervalInSeconds 15 -ProbeCount 2 -RequestPath /
```
### <a name="create-a-load-balancer-rule"></a>Erstellen einer Lastenausgleichsregel

Mit einer Lastenausgleichsregel werden die Front-End-IP-Konfiguration für den eingehenden Datenverkehr und der Back-End-IP-Pool zum Empfangen des Datenverkehrs definiert. Außerdem wird der erforderliche Quell- und Zielport definiert. 

Erstellen Sie eine Lastenausgleichsregel mit dem Namen *myinboundlbrule*, und verwenden Sie dazu [new-azloadbalancerruleconfig](https://docs.microsoft.com/powershell/module/az.network/new-azloadbalancerruleconfig?view=azps-2.6.0). Diese Regel lauscht an Port 80 im Front-End-Pool *myfrontendinbound*. Sie verwendet Port 80 darüber hinaus zum Senden von Netzwerkdatenverkehr nach dem Lastenausgleich an den Back-End-Adresspool *bepoolinbound*. 

```azurepowershell-interactive
$inboundRule = New-AzLoadBalancerRuleConfig -Name inboundlbrule -FrontendIPConfiguration $frontendIPin -BackendAddressPool $bepoolin -Probe $probe -Protocol "Tcp" -FrontendPort 80 -BackendPort 80 -IdleTimeoutInMinutes 15 -EnableFloatingIP -LoadDistribution SourceIP -DisableOutboundSNAT
```

>[!NOTE]
>Diese Lastenausgleichsregel deaktiviert aufgrund des Parameters **-DisableOutboundSNAT** die automatische ausgehende SNAT (secure NAT, sichere Netzwerkadressenübersetzung). Die Ausgangs-NAT wird nur von der Ausgangsregel bereitgestellt.

### <a name="create-an-outbound-rule"></a>Erstellen Sie eine Ausgangsregel.

Eine Ausgangsregel definiert die öffentliche IP-Adresse des Front-Ends, die durch *myfrontendoutbound* des Front-Ends dargestellt wird. Dieses Front-End wird für den gesamten NAT-Ausgangsdatenverkehr sowie für den Back-End-Pool verwendet, für den die Regel gilt.  

Verwenden Sie den folgenden Befehl zum Erstellen einer Ausgangsregel *myoutboundrule* für die ausgehende Netzwerkübersetzung aller VMs (in NIC-IP-Konfigurationen) im Back-End-Pool *bepool*.  Mit dem Befehl wird der ausgehende Leerlauftimeout von 4 in 15 Minuten geändert. Er weist 10.000 SNAT-Ports anstelle von 1.024 zu. Weitere Informationen finden Sie unter [New-AzLoadBalancerOutboundRuleConfig](https://docs.microsoft.com/powershell/module/az.network/new-azloadbalanceroutboundruleconfig?view=azps-2.7.0).

```azurepowershell-interactive
 $outboundRule = New-AzLoadBalancerOutBoundRuleConfig -Name outboundrule -FrontendIPConfiguration $frontendIPout -BackendAddressPool $bepoolout -Protocol All -IdleTimeoutInMinutes 15 -AllocatedOutboundPort 10000
```
Wenn Sie keinen separaten Ausgangspool verwenden möchten, können Sie das Adresspoolargument im vorherigen Befehl ändern, um stattdessen *$bepoolin* anzugeben.  Es empfiehlt sich, separate Pools zu verwenden, um die resultierende Konfiguration flexibel und verständlich zu gestalten.

### <a name="create-a-load-balancer"></a>Einrichten eines Load Balancers

Verwenden Sie den folgenden Befehl, um mithilfe von [New-AzLoadBalancer](https://docs.microsoft.com/powershell/module/az.network/new-azloadbalancer?view=azps-2.6.0) einen Lastenausgleich für die eingehende IP-Adresse zu erstellen. Geben Sie dem Lastenausgleich den Namen *lb*. Er sollte eine Front-End-IP-Eingangskonfiguration beinhalten. Sein Back-End-Pool *bepoolinbound* sollte der öffentlichen IP-Adresse *mypublicipinbound* zugeordnet sein, die Sie im vorhergehenden Schritt erstellt haben.

```azurepowershell-interactive
New-AzLoadBalancer -Name lb -Sku Standard -ResourceGroupName myresourcegroupoutbound -Location eastus -FrontendIpConfiguration $frontendIPin,$frontendIPout -BackendAddressPool $bepoolin,$bepoolout -Probe $probe -LoadBalancingRule $inboundrule -OutboundRule $outboundrule 
```

Jetzt können Sie damit fortfahren, den beiden Back-End-Pools *bepoolinbound* und *bepooloutbound* VMs zuzuordnen, indem Sie die IP-Konfiguration der entsprechenden NIC-Ressourcen aktualisieren. Aktualisieren Sie die Ressourcenkonfiguration mithilfe von [Add-AzNetworkInterfaceIpConfig](https://docs.microsoft.com/cli/azure/network/lb/rule?view=azure-cli-latest).

## <a name="clean-up-resources"></a>Bereinigen von Ressourcen

Wenn Sie die Ressourcengruppe, den Lastenausgleich und verwandte Ressourcen nicht mehr benötigen, können Sie sie mithilfe von [Remove-AzResourceGroup](https://docs.microsoft.com/powershell/module/az.resources/remove-azresourcegroup?view=azps-2.7.0) entfernen.

```azurepowershell-interactive 
  Remove-AzResourceGroup -Name myresourcegroupoutbound
```

## <a name="next-steps"></a>Nächste Schritte
In diesem Artikel haben Sie einen Load Balancer Standard erstellt und Regeln sowohl für beim Lastenausgleich eingehenden als auch für ausgehenden Datenverkehr konfiguriert. Zudem haben Sie einen Integritätstest für die virtuellen Computer im Back-End-Pool erstellt. 

Weitere Informationen zum Lastenausgleich finden Sie in den [Tutorials zum Azure Load Balancer](tutorial-load-balancer-standard-public-zone-redundant-portal.md).
