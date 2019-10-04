---
title: Azure PowerShell-Skriptbeispiel – Einschränken des Webdatenverkehrs | Microsoft-Dokumentation
description: Azure PowerShell-Skriptbeispiel – Erstellen Sie ein Anwendungsgateway mit einer Web Application Firewall und einer VM-Skalierungsgruppe, die OWASP-Regeln verwendet, um den Datenverkehr einzuschränken.
services: application-gateway
documentationcenter: networking
author: vhorne
manager: jpconnock
editor: tysonn
tags: azure-resource-manager
ms.service: application-gateway
ms.topic: sample
ms.tgt_pltfrm: vm-windows
ms.workload: infrastructure
ms.date: 01/29/2018
ms.author: victorh
ms.custom: mvc
ms.openlocfilehash: d3dfb9708e22a86af8fb9854e424f7da7d1f410a
ms.sourcegitcommit: 0568c7aefd67185fd8e1400aed84c5af4f1597f9
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 05/06/2019
ms.locfileid: "65202856"
---
# <a name="restrict-web-traffic-using-azure-powershell"></a>Einschränken von Webdatenverkehr mithilfe von Azure PowerShell

Dieses Skript erstellt ein Anwendungsgateway mit einer Web Application Firewall, das eine VM-Skalierungsgruppe als Back-End-Server verwendet. Die Web Application Firewall schränkt den Webdatenverkehr anhand von OWASP-Regeln ein. Nach dem Ausführen des Skripts können Sie das Anwendungsgateway mit dessen öffentlicher IP-Adresse testen.

[!INCLUDE [sample-powershell-install](../../../includes/sample-powershell-install-no-ssh-az.md)]

[!INCLUDE [quickstarts-free-trial-note](../../../includes/quickstarts-free-trial-note.md)]

## <a name="sample-script"></a>Beispielskript

[!code-powershell[main](../../../powershell_scripts/application-gateway/create-vmss/create-vmss-waf.ps1 "Create application gateway with WAF")]

## <a name="clean-up-deployment"></a>Bereinigen der Bereitstellung 

Führen Sie den folgenden Befehl aus, um die Ressourcengruppe, das Anwendungsgateway und alle zugehörigen Ressourcen zu entfernen.

```powershell
Remove-AzResourceGroup -Name myResourceGroupAG
```

## <a name="script-explanation"></a>Erläuterung des Skripts

Dieses Skript verwendet die folgenden Befehle zum Erstellen der Bereitstellung. Jedes Element in der Tabelle ist mit der befehlsspezifischen Dokumentation verknüpft.

| Get-Help | Notizen |
|---|---|
| [New-AzResourceGroup](/powershell/module/az.resources/new-azresourcegroup) | Erstellt eine Ressourcengruppe, in der alle Ressourcen gespeichert sind. |
| [New-AzVirtualNetworkSubnetConfig](/powershell/module/az.network/new-azvirtualnetworksubnetconfig) | Erstellt die Subnetzkonfiguration. |
| [New-AzVirtualNetwork](/powershell/module/az.network/new-azvirtualnetwork) | Erstellt das virtuelle Netzwerk mit den Subnetzkonfigurationen. |
| [New-AzPublicIpAddress](/powershell/module/az.network/new-azpublicipaddress) | Erstellt die öffentliche IP-Adresse für das Anwendungsgateway. |
| [New-AzApplicationGatewayIPConfiguration](/powershell/module/az.network/new-azapplicationgatewayipconfiguration) | Erstellt die Konfiguration, die einem Subnetz das Anwendungsgateway zuordnet. |
| [New-AzApplicationGatewayFrontendIPConfig](/powershell/module/az.network/new-azapplicationgatewayfrontendipconfig) | Erstellt die Konfiguration, die dem Anwendungsgateway eine öffentliche IP-Adresse zuordnet. |
| [New-AzApplicationGatewayFrontendPort](/powershell/module/az.network/new-azapplicationgatewayfrontendport) | Weist einen Port zu, der für den Zugriff auf das Anwendungsgateway verwendet wird. |
| [New-AzApplicationGatewayBackendAddressPool](/powershell/module/az.network/new-azapplicationgatewaybackendaddresspool) | Erstellt einen Back-End-Pool für ein Anwendungsgateway. |
| [New-AzApplicationGatewayBackendHttpSettings](/powershell/module/az.network/new-azapplicationgatewaybackendhttpsetting) | Konfiguriert Einstellungen für einen Back-End-Pool. |
| [New-AzApplicationGatewayHttpListener](/powershell/module/az.network/new-azapplicationgatewayhttplistener) | Erstellt einen Listener. |
| [New-AzApplicationGatewayRequestRoutingRule](/powershell/module/az.network/new-azapplicationgatewayrequestroutingrule) | Erstellt eine Routingregel. |
| [New-AzApplicationGatewaySku](/powershell/module/az.network/new-azapplicationgatewaysku) | Angeben von Ebene und Kapazität für ein Anwendungsgateway. |
| [New-AzApplicationGatewayWebApplicationFirewallConfiguration](/powershell/module/az.network/new-azapplicationgatewaywebapplicationfirewallconfiguration) | Erstellt die Web Application Firewall-Konfiguration. |
| [New-AzApplicationGateway](/powershell/module/az.network/new-azapplicationgateway) | Erstellen eines Anwendungsgateways |
| [Set-AzVmssStorageProfile](/powershell/module/az.compute/set-azvmssstorageprofile) | Erstellen eines Speicherprofils für die Skalierungsgruppe. |
| [Set-AzVmssOsProfile](/powershell/module/az.compute/set-azvmssosprofile) | Definieren des Betriebssystems für die Skalierungsgruppe. |
| [Add-AzVmssNetworkInterfaceConfiguration](/powershell/module/az.compute/add-azvmssnetworkinterfaceconfiguration) | Definieren der Netzwerkschnittstelle für die Skalierungsgruppe. |
| [New-AzVmss](/powershell/module/az.compute/new-azvm) | Erstellen einer VM-Skalierungsgruppe. |
| [New-AzStorageAccount](/powershell/module/az.storage/new-azstorageaccount) | Erstellt ein Speicherkonto. |
| [Set-AzDiagnosticSetting](/powershell/module/az.monitor/set-azdiagnosticsetting) | Konfiguriert die Diagnose, um Daten aufzuzeichnen. |
| [Get-AzPublicIPAddress](/powershell/module/az.network/get-azpublicipaddress) | Ruft die öffentliche IP-Adresse für ein Anwendungsgateway ab. |
|[Remove-AzResourceGroup](/powershell/module/az.resources/remove-azresourcegroup) | Entfernt eine Ressourcengruppe und alle darin enthaltenen Ressourcen. | 
## <a name="next-steps"></a>Nächste Schritte

Weitere Informationen zum Azure PowerShell-Modul finden Sie in der [Azure PowerShell-Dokumentation](/powershell/azure/overview).

Zusätzliche PowerShell-Skriptbeispiele zu Anwendungsgateways finden Sie in der [Dokumentation zu Azure Application Gateway](../powershell-samples.md).
