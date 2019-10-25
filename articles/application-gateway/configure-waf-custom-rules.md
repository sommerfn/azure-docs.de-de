---
title: Konfigurieren von Web Application Firewall v2 mit benutzerdefinierten Regeln unter Verwendung von Azure PowerShell
description: Hier erfahren Sie, wie Sie Web Application Firewall v2 unter Verwendung von Azure PowerShell mit benutzerdefinierten Regeln konfigurieren.
services: application-gateway
author: vhorne
ms.service: application-gateway
ms.topic: article
ms.date: 6/18/2019
ms.author: victorh
ms.openlocfilehash: 9e50b47e22f5760c213cd0cafad82ecca592dec8
ms.sourcegitcommit: b4665f444dcafccd74415fb6cc3d3b65746a1a31
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 10/11/2019
ms.locfileid: "72263748"
---
# <a name="configure-web-application-firewall-v2-custom-rules-by-using-azure-powershell"></a>Konfigurieren von Web Application Firewall v2 mit benutzerdefinierten Regeln unter Verwendung von Azure PowerShell

<!--- If you make any changes to the PowerShell in this article, also make the change in the corresponding Sample file: azure-docs-powershell-samples/application-gateway/waf-rules/waf-custom-rules.ps1 --->

Benutzerdefinierte Regeln ermöglichen die Erstellung eigener Regeln, die für jede Anforderung ausgewertet werden, die die Web Application Firewall (WAF) durchlaufen. Diese Regeln haben eine höhere Priorität als die restlichen Regeln in den verwalteten Regelsätzen. Die benutzerdefinierten Regeln verfügen über eine Aktion (Zulassen oder Blockieren), eine Übereinstimmungsbedingung und einen Operator, um eine vollständige Anpassung zu ermöglichen.

In diesem Artikel wird eine Azure Application Gateway-WAF v2 erstellt, die eine benutzerdefinierte Regel verwendet. Die benutzerdefinierte Regel blockiert Datenverkehr, wenn der Anforderungsheader den Benutzer-Agent *evilbot* enthält.

Weitere Beispiele für benutzerdefinierte Regeln finden Sie unter [Erstellen und Verwenden von benutzerdefinierten Regeln für die Web Application Firewall v2](create-custom-waf-rules.md).

Wenn Sie den Azure PowerShell-Code in diesem Artikel in einem einzelnen fortlaufenden Skript benötigen, das Sie kopieren, einfügen und ausführen können, lesen Sie [Azure PowerShell-Beispiele für Azure Application Gateway](powershell-samples.md).

## <a name="prerequisites"></a>Voraussetzungen
* [!INCLUDE [quickstarts-free-trial-note](../../includes/quickstarts-free-trial-note.md)]

* Sie benötigen ein Azure PowerShell-Modul. Wenn Sie Azure PowerShell lokal installieren und verwenden möchten, müssen Sie für dieses Skript mindestens die Version 2.1.0 des Azure PowerShell-Moduls verwenden. Gehen Sie wie folgt vor:

  1. Führen Sie `Get-Module -ListAvailable Az` aus, um die Version zu finden. Wenn Sie ein Upgrade ausführen müssen, finden Sie unter [Installieren des Azure PowerShell-Moduls](/powershell/azure/install-az-ps) Informationen dazu.
  2. Führen Sie `Connect-AzAccount` aus, um eine Verbindung mit Azure herzustellen.

## <a name="example-script"></a>Beispielskript

### <a name="set-up-variables"></a>Einrichten von Variablen

Führen Sie den folgenden Code aus:

```azurepowershell
$rgname = "CustomRulesTest"

$location = "East US"

$appgwName = "WAFCustomRules"
```

### <a name="create-a-resource-group"></a>Erstellen einer Ressourcengruppe

Führen Sie den folgenden Code aus:

```azurepowershell
$resourceGroup = New-AzResourceGroup -Name $rgname -Location $location
```

### <a name="create-a-virtual-network"></a>Erstellen eines virtuellen Netzwerks

Führen Sie den folgenden Code aus:

```azurepowershell
$sub1 = New-AzVirtualNetworkSubnetConfig -Name "appgwSubnet" -AddressPrefix "10.0.0.0/24"

$sub2 = New-AzVirtualNetworkSubnetConfig -Name "backendSubnet" -AddressPrefix "10.0.1.0/24"

$vnet = New-AzvirtualNetwork -Name "Vnet1" -ResourceGroupName $rgname -Location $location `
  -AddressPrefix "10.0.0.0/16" -Subnet @($sub1, $sub2)
```

### <a name="create-a-static-public-vip"></a>Erstellen einer statischen öffentlichen VIP

Führen Sie den folgenden Code aus:

```azurepowershell
$publicip = New-AzPublicIpAddress -ResourceGroupName $rgname -name "AppGwIP" `
  -location $location -AllocationMethod Static -Sku Standard
```

### <a name="create-a-pool-and-front-end-port"></a>Erstellen eines Pools und eines Front-End-Ports

Führen Sie den folgenden Code aus:

```azurepowershell
$gwSubnet = Get-AzVirtualNetworkSubnetConfig -Name "appgwSubnet" -VirtualNetwork $vnet

$gipconfig = New-AzApplicationGatewayIPConfiguration -Name "AppGwIpConfig" -Subnet $gwSubnet

$fipconfig01 = New-AzApplicationGatewayFrontendIPConfig -Name "fipconfig" -PublicIPAddress $publicip

$pool = New-AzApplicationGatewayBackendAddressPool -Name "pool1" `
  -BackendIPAddresses testbackend1.westus.cloudapp.azure.com, testbackend2.westus.cloudapp.azure.com

$fp01 = New-AzApplicationGatewayFrontendPort -Name "port1" -Port 80
```

### <a name="create-a-listener-http-setting-rule-and-autoscale"></a>Erstellen eines Listeners, einer HTTP-Einstellung, einer Regel und der Autoskalierung

Führen Sie den folgenden Code aus:

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

Führen Sie den folgenden Code aus:

```azurepowershell
$variable = New-AzApplicationGatewayFirewallMatchVariable -VariableName RequestHeaders -Selector User-Agent

$condition = New-AzApplicationGatewayFirewallCondition -MatchVariable $variable -Operator Contains -MatchValue "evilbot" -Transform Lowercase -NegationCondition $False  

$rule = New-AzApplicationGatewayFirewallCustomRule -Name blockEvilBot -Priority 2 -RuleType MatchRule -MatchCondition $condition -Action Block

$wafPolicy = New-AzApplicationGatewayFirewallPolicy -Name wafPolicy -ResourceGroup $rgname -Location $location -CustomRule $rule

$wafConfig = New-AzApplicationGatewayWebApplicationFirewallConfiguration -Enabled $true -FirewallMode "Prevention"
```

### <a name="create-an-application-gateway"></a>Erstellen eines Anwendungsgateways

Führen Sie den folgenden Code aus:

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

[Weitere Informationen zur Web Application Firewall](waf-overview.md)
