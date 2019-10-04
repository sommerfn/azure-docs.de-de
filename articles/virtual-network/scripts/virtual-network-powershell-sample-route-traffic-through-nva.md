---
title: Azure PowerShell-Beispielskript – Weiterleiten von Datenverkehr über ein virtuelles Netzwerkgerät | Microsoft-Dokumentation
description: Azure PowerShell-Beispielskript – Weiterleiten von Datenverkehr über ein virtuelles Firewall-Netzwerkgerät.
services: virtual-network
documentationcenter: virtual-network
author: KumudD
manager: twooley
editor: ''
tags: ''
ms.assetid: ''
ms.service: virtual-network
ms.devlang: powershell
ms.topic: sample
ms.tgt_pltfrm: ''
ms.workload: infrastructure
ms.date: 03/20/2018
ms.author: kumud
ms.openlocfilehash: 21ee52f0a9c3b5a6cc64bdaf92c05db4a18903ca
ms.sourcegitcommit: 44a85a2ed288f484cc3cdf71d9b51bc0be64cc33
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 04/28/2019
ms.locfileid: "64717535"
---
# <a name="route-traffic-through-a-network-virtual-appliance-script-sample"></a>Skriptbeispiel zum Weiterleiten von Datenverkehr über ein virtuelles Netzwerkgerät

In diesem Skriptbeispiel wird ein virtuelles Netzwerk mit Front-End- und Back-End-Subnetz erstellt. Es wird auch ein virtueller Computer mit IP-Weiterleitung erstellt, der zur Weiterleitung von Datenverkehr zwischen den zwei Subnetzen aktiviert ist. Nach dem Ausführen des Skripts können Sie dem virtuellen Computer Netzwerksoftware bereitstellen, z.B. eine Firewallanwendung.

Sie können das Skript über Azure [Cloud Shell](https://shell.azure.com/powershell) oder eine lokale PowerShell-Installation ausführen. Wenn Sie PowerShell lokal verwenden, müssen Sie für dieses Skript mindestens Version 5.4.1 des Azure PowerShell-Moduls verwenden. Führen Sie `Get-Module -ListAvailable Az` aus, um die installierte Version zu ermitteln. Wenn Sie ein Upgrade ausführen müssen, finden Sie unter [Installieren des Azure PowerShell-Moduls](/powershell/azure/install-Az-ps) Informationen dazu. Wenn Sie PowerShell lokal ausführen, müssen Sie auch `Connect-AzAccount` ausführen, um eine Verbindung mit Azure herzustellen.

[!INCLUDE [quickstarts-free-trial-note](../../../includes/quickstarts-free-trial-note.md)]

## <a name="sample-script"></a>Beispielskript

[!INCLUDE [updated-for-az](../../../includes/updated-for-az.md)]

[!code-azurepowershell-interactive[main](../../../powershell_scripts/virtual-network/route-traffic-through-nva/route-traffic-through-nva.ps1 "Route traffic through a network virtual appliance")]

## <a name="clean-up-deployment"></a>Bereinigen der Bereitstellung

Führen Sie den folgenden Befehl aus, um die Ressourcengruppe, den virtuellen Computer und alle zugehörigen Ressourcen zu entfernen:

```powershell
Remove-AzResourceGroup -Name myResourceGroup -Force
```

## <a name="script-explanation"></a>Erläuterung des Skripts

In diesem Skript werden die folgenden Befehle verwendet, um eine Ressourcengruppe, ein virtuelles Netzwerk und Netzwerksicherheitsgruppen zu erstellen. Jeder Befehl in der folgenden Tabelle ist mit der zugehörigen Dokumentation verknüpft:

| Get-Help | Notizen |
|---|---|
| [New-AzResourceGroup](/powershell/module/az.resources/new-azresourcegroup)  | Erstellt eine Ressourcengruppe, in der alle Ressourcen gespeichert sind. |
| [New-AzVirtualNetwork](/powershell/module/az.network/new-azvirtualnetwork) | Erstellt ein virtuelles Azure-Netzwerk und ein Front-End-Subnetz. |
| [New-AzVirtualNetworkSubnetConfig](/powershell/module/az.network/new-azvirtualnetworksubnetconfig) | Erstellt Back-End- und DMZ-Subnetz. |
| [New-AzPublicIpAddress](/powershell/module/az.network/new-azpublicipaddress) | Erstellt eine öffentliche IP-Adresse für den Zugriff auf den virtuellen Computer aus dem Internet. |
| [New-AzNetworkInterface](/powershell/module/az.network/new-aznetworkinterface) | Erstellt eine virtuelle Netzwerkschnittstelle und aktiviert die IP-Weiterleitung dafür. |
| [New-AzNetworkSecurityGroup](/powershell/module/az.network/new-aznetworksecuritygroup) | Erstellt eine Netzwerksicherheitsgruppe (NSG). |
| [New-AzNetworkSecurityRuleConfig](/powershell/module/az.network/new-aznetworksecurityruleconfig) | Erstellt NSG-Regeln, die eingehende HTTP- und HTTPS-Ports für den virtuellen Computer ermöglichen. |
| [Set-AzVirtualNetworkSubnetConfig](/powershell/module/az.network/set-azvirtualnetworksubnetconfig)| Ordnet die NSGs und Routentabellen Subnetzen zu. |
| [New-AzRouteTable](/powershell/module/az.network/new-azroutetable)| Erstellt eine Routingtabelle für alle Routen. |
| [New-AzRouteConfig](/powershell/module/az.network/new-azrouteconfig)| Erstellt die Routen zum Weiterleiten von Datenverkehr zwischen Subnetzen und dem Internet über den virtuellen Computer. |
| [New-AzVM](/powershell/module/az.compute/new-azvm) | Erstellt einen virtuellen Computer und fügt ihm eine NIC an. Dieser Befehl legt außerdem das zu verwendende VM-Image und die Administratoranmeldeinformationen fest. |
| [Remove-AzResourceGroup](/powershell/module/az.resources/remove-azresourcegroup)  | Löscht eine Ressourcengruppe und alle darin enthaltenen Ressourcen. |

## <a name="next-steps"></a>Nächste Schritte

Weitere Informationen zu Azure PowerShell finden Sie in der [Azure PowerShell-Dokumentation](https://docs.microsoft.com/powershell/azure/overview).

Weitere PowerShell-Skriptbeispiele für virtuelle Netzwerke finden Sie unter [PowerShell-Beispiele für virtuelle Netzwerke](../powershell-samples.md).