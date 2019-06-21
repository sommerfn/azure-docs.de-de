---
title: Konfigurieren von benutzerdefinierten WAF-Regeln mit Azure PowerShell
description: Es wird beschrieben, wie Sie benutzerdefinierte WAF-Regeln mit Azure PowerShell konfigurieren.
services: application-gateway
author: vhorne
ms.service: application-gateway
ms.topic: article
ms.date: 6/5/2019
ms.author: victorh
ms.openlocfilehash: 0700341d5410710e2187e775e772da922dfaf86a
ms.sourcegitcommit: 6932af4f4222786476fdf62e1e0bf09295d723a1
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 06/05/2019
ms.locfileid: "66688929"
---
# <a name="configure-web-application-firewall-with-a-custom-rule-using-azure-powershell"></a>Konfigurieren einer Web Application Firewall mit einer benutzerdefinierten Regel mit Azure PowerShell

<!--- If you make any changes to the PowerShell in this article, also make the change in the corresponding Sample file: azure-docs-powershell-samples/application-gateway/waf-rules/waf-custom-rules.ps1 --->

Benutzerdefinierte Regeln ermöglichen die Erstellung eigener Regeln, die für jede Anforderung ausgewertet werden, für die die Web Application Firewall (WAF) durchlaufen wird. Diese Regeln haben eine höhere Priorität als die restlichen Regeln in den verwalteten Regelsätzen. Die benutzerdefinierten Regeln verfügen über eine Aktion (Zulassen oder Blockieren), eine Übereinstimmungsbedingung und einen Operator zum Zulassen der vollständigen Anpassung.

In diesem Artikel wird eine Application Gateway-WAF erstellt, für die eine benutzerdefinierte Regel verwendet wird. Die benutzerdefinierte Regel blockiert Datenverkehr, wenn der Anforderungsheader den Benutzer-Agent *evilbot* enthält.

Weitere Beispiele für benutzerdefinierte Regeln finden Sie unter [Create and use Web Application Firewall custom rules](create-custom-waf-rules.md) (Erstellen und Verwenden benutzerdefinierter Web Application Firewall-Regeln).

<!--- If you want run the Azure PowerShell in this article in one continuous script that you can copy, paste, and run, [see link to Samples]. --->

## <a name="prerequisites"></a>Voraussetzungen

### <a name="azure-powershell-module"></a>Azure PowerShell-Modul

Wenn Sie Azure PowerShell lokal installieren und verwenden möchten, müssen Sie für dieses Skript mindestens Version 2.1.0 des Azure PowerShell-Moduls verwenden.

1. Führen Sie `Get-Module -ListAvailable Az` aus, um die Version zu finden. Wenn Sie ein Upgrade ausführen müssen, finden Sie unter [Installieren des Azure PowerShell-Moduls](/powershell/azure/install-az-ps) Informationen dazu.
2. Führen Sie `Connect-AzAccount` aus, um eine Verbindung mit Azure herzustellen.

[!INCLUDE [quickstarts-free-trial-note](../../includes/quickstarts-free-trial-note.md)]

## <a name="example-script"></a>Beispielskript

### <a name="set-up-variables"></a>Einrichten von Variablen

```azurepowershell
$rgname = "CustomRulesTest"

$location = "East US"

$appgwName = "WAFCustomRules"
```

### <a name="create-a-resource-group"></a>Erstellen einer Ressourcengruppe

```azurepowershell
$resourceGroup = New-AzResourceGroup -Name $rgname -Location $location
```

### <a name="create-a-vnet"></a>Erstellen eines VNET

```azurepowershell
$sub1 = New-AzVirtualNetworkSubnetConfig -Name "appgwSubnet" -AddressPrefix "10.0.0.0/24"

$sub2 = New-AzVirtualNetworkSubnetConfig -Name "backendSubnet" -AddressPrefix "10.0.1.0/24"

$vnet = New-AzvirtualNetwork -Name "Vnet1" -ResourceGroupName $rgname -Location $location `
  -AddressPrefix "10.0.0.0/16" -Subnet @($sub1, $sub2)
```

### <a name="create-a-static-public-vip"></a>Erstellen einer statischen öffentlichen VIP

```azurepowershell
$publicip = New-AzPublicIpAddress -ResourceGroupName $rgname -name "AppGwIP" `
  -location $location -AllocationMethod Static -Sku Standard
```

### <a name="create-pool-and-frontend-port"></a>Erstellen eines Pools und eines Front-End-Ports

```azurepowershell
$gwSubnet = Get-AzVirtualNetworkSubnetConfig -Name "appgwSubnet" -VirtualNetwork $vnet

$gipconfig = New-AzApplicationGatewayIPConfiguration -Name "AppGwIpConfig" -Subnet $gwSubnet

$fipconfig01 = New-AzApplicationGatewayFrontendIPConfig -Name "fipconfig" -PublicIPAddress $publicip

$pool = New-AzApplicationGatewayBackendAddressPool -Name "pool1" `
  -BackendIPAddresses testbackend1.westus.cloudapp.azure.com, testbackend2.westus.cloudapp.azure.com

$fp01 = New-AzApplicationGatewayFrontendPort -Name "port1" -Port 80
```

### <a name="create-a-listener-http-setting-rule-and-autoscale"></a>Erstellen eines Listeners, einer HTTP-Einstellung, einer Regel und der Autoskalierung

```azurepowershell
$listener01 = New-AzApplicationGatewayHttpListener -Name "listener1" -Protocol Http `
  -FrontendIPConfiguration $fipconfig01 -FrontendPort $fp01

$poolSetting01 = New-AzApplicationGatewayBackendHttpSettings -Name "setting1" -Port 80 `
  -Protocol Http -CookieBasedAffinity Disabled

$rule01 = New-AzApplicationGatewayRequestRoutingRule -Name "rule1" -RuleType basic `
  -BackendHttpSettings $poolSetting01 -HttpListener $listener01 -BackendAddressPool $pool

$autoscaleConfig = New-AzApplicationGatewayAutoscaleConfiguration -MinCapacity 3

$sku = New-AzApplicationGatewaySku -Name WAF_v2 -Tier WAF_v2
```

### <a name="create-the-custom-rule-and-apply-it-to-waf-policy"></a>Erstellen der benutzerdefinierten Regel und Anwenden auf die WAF-Richtlinie

```azurepowershell
$variable = New-AzApplicationGatewayFirewallMatchVariable -VariableName RequestHeaders -Selector User-Agent

$condition = New-AzApplicationGatewayFirewallCondition -MatchVariable $variable -Operator Contains -MatchValue "evilbot" -Transform Lowercase -NegationCondition $False  

$rule = New-AzApplicationGatewayFirewallCustomRule -Name blockEvilBot -Priority 2 -RuleType MatchRule -MatchCondition $condition -Action Block

$wafPolicy = New-AzApplicationGatewayFirewallPolicy -Name wafPolicy -ResourceGroup $rgname -Location $location -CustomRule $rule

$wafConfig = New-AzApplicationGatewayWebApplicationFirewallConfiguration -Enabled $true -FirewallMode "Prevention"
```

### <a name="create-the-application-gateway"></a>Erstellen des Anwendungsgateways

```azurepowershell
$appgw = New-AzApplicationGateway -Name $appgwName -ResourceGroupName $rgname `
   -Location $location -BackendAddressPools $pool `
   -BackendHttpSettingsCollection  $poolSetting01 `
   -GatewayIpConfigurations $gipconfig -FrontendIpConfigurations $fipconfig01 `
   -FrontendPorts $fp01 -HttpListeners $listener01 `
   -RequestRoutingRules $rule01 -Sku $sku -AutoscaleConfiguration $autoscaleConfig `
   -WebApplicationFirewallConfig $wafConfig `
   -FirewallPolicy $wafPolicy
```

## <a name="next-steps"></a>Nächste Schritte

[Erfahren Sie mehr über die Web Application Firewall](waf-overview.md).