---
title: 'Deaktivieren von Netzwerkrichtlinien für die Quell-IP-Adresse eines Azure Private Link-Diensts '
description: Erfahren Sie, wie Sie Netzwerkrichtlinien für Azure Private Link deaktivieren.
services: private-link
author: KumudD
ms.service: private-link
ms.topic: article
ms.date: 09/16/2019
ms.author: kumud
ms.openlocfilehash: 53df209d080cf91be9c558b43edaa618c0748fc5
ms.sourcegitcommit: b45ee7acf4f26ef2c09300ff2dba2eaa90e09bc7
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 10/30/2019
ms.locfileid: "73101534"
---
# <a name="disable-network-policies-for-private-link-service-source-ip"></a>Deaktivieren von Netzwerkrichtlinien für die Quell-IP-Adresse eines Private Link-Diensts

Damit Sie eine Quell-IP-Adresse für den Private Link-Dienst auswählen können, ist im Subnetz die explizite Einstellung zum Deaktivieren (`privateLinkServiceNetworkPolicies`) erforderlich. Diese Einstellung gilt nur für die spezifische private IP-Adresse, die Sie als Quell-IP-Adresse des Private Link-Diensts ausgewählt haben. Für andere Ressourcen im Subnetz wird der Zugriff basierend auf der Definition von Sicherheitsregeln der Netzwerksicherheitsgruppen (NSG) gesteuert. 
 
Wenn Sie einen Azure-Client (PowerShell, CLI oder Vorlagen) verwenden, ist ein zusätzlicher Schritt erforderlich, um diese Eigenschaft zu ändern. Sie können die Richtlinie mithilfe von Cloud Shell aus dem Azure-Portal oder lokalen Installationen von Azure PowerShell, der Azure CLI oder von Azure Resource Manager Vorlagen deaktivieren.  
 
Befolgen Sie die nachfolgenden Schritte, um Netzwerkrichtlinien eines Private Link-Diensts für ein virtuelles Netzwerk namens *myVirtualNetwork* mit einem *standardmäßigen* Subnetz zu deaktivieren, das in einer Ressourcengruppe namens *myResourceGroup* gehostet wird. 

## <a name="using-azure-powershell"></a>Verwenden von Azure PowerShell
In diesem Abschnitt wird beschrieben, wie Sie Subnetzrichtlinien für private Endpunkte mit Azure PowerShell deaktivieren können.

```azurepowershell
$virtualNetwork= Get-AzVirtualNetwork `
  -Name "myVirtualNetwork" ` 
  -ResourceGroupName "myResourceGroup"  
   
($virtualNetwork | Select -ExpandProperty subnets | Where-Object  {$_.Name -eq 'default'} ).privateLinkServiceNetworkPolicies = "Disabled"  
 
$virtualNetwork | Set-AzVirtualNetwork 
```
## <a name="using-azure-cli"></a>Verwenden der Azure-Befehlszeilenschnittstelle
In diesem Abschnitt wird beschrieben, wie Sie Subnetzrichtlinien für private Endpunkte mit der Azure CLI deaktivieren können.
```azurecli
az network vnet subnet update \ 
  --name default \ 
  --resource-group myResourceGroup \ 
  --vnet-name myVirtualNetwork \ 
  --disable-private-link-service-network-policies true 
```
## <a name="using-a-template"></a>Verwenden einer Vorlage
In diesem Abschnitt wird beschrieben, wie Sie Subnetzrichtlinien für private Endpunkte mit einer Azure Resource Manager-Vorlage deaktivieren können.
```json
{ 
    "name": "myVirtualNetwork", 
    "type": "Microsoft.Network/virtualNetworks", 
    "apiVersion": "2019-04-01", 
    "location": "WestUS", 
    "properties": { 
        "addressSpace": { 
            "addressPrefixes": [ 
                "10.0.0.0/16" 
             ] 
        }, 
        "subnets": [ 
               { 
                 "name": "default", 
                 "properties": { 
                        "addressPrefix": "10.0.0.0/24", 
                        "privateLinkServiceNetworkPolicies": "Disabled" 
                    } 
                } 
        ] 
    } 
} 
 
```
## <a name="next-steps"></a>Nächste Schritte
- Weitere Informationen zu [privaten Azure-Endpunkten](private-endpoint-overview.md)
 
