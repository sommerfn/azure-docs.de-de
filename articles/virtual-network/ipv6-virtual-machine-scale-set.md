---
title: Bereitstellen von VM-Skalierungsgruppen mit IPv6 in Azure
titlesuffix: Azure Virtual Network
description: In diesem Artikel wird gezeigt, wie Sie VM-Skalierungsgruppen mit IPv6 in einem virtuellen Azure-Netzwerk bereitstellen können.
services: virtual-network
documentationcenter: na
author: KumudD
manager: twooley
ms.service: virtual-network
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 10/29/2019
ms.author: kumud
ms.openlocfilehash: b90910614bcd86a54198b1a0961a3378427ea87e
ms.sourcegitcommit: 0b1a4101d575e28af0f0d161852b57d82c9b2a7e
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 10/30/2019
ms.locfileid: "73164152"
---
# <a name="deploy-virtual-machine-scale-sets-with-ipv6-in-azure-preview"></a>Bereitstellen von VM-Skalierungsgruppen mit IPv6 in Azure (Vorschau)

In diesem Artikel erfahren Sie, wie Sie eine Dual-Stack-VM-Skalierungsgruppe (IPv4 + IPv6) mit einem externen Dual-Stack-Lastenausgleich in einem virtuellen Azure-Netzwerk bereitstellen. Der Prozess zur Erstellung einer IPv6-fähigen VM-Skalierungsgruppe ist nahezu identisch mit dem Prozess zur Erstellung einzelner virtueller Computer, der [hier](ipv6-configure-standard-load-balancer-template-json.md) beschrieben wird. Sie beginnen mit den Schritten, die denen ähneln, die für einzelne VMs beschrieben sind:
1.  Erstellen Sie öffentliche IPv4- und IPv6-Adressen.
2.  Erstellen Sie einen Dual-Stack-Lastenausgleich.  
3.  Erstellen Sie NSG-Regeln (Netzwerksicherheitsgruppe).  

Der einzige Schritt, der sich von einzelnen VMs unterscheidet, ist die Erstellung der NIC--Konfiguration (Netzwerkschnittstellenkarte), die die Ressourcen der VM-Skalierungsgruppe verwendet: „networkProfile/networkInterfaceConfigurations“. Die JSON-Struktur ähnelt der des „Microsoft.Network/networkInterfaces“-Objekts, das für einzelne VMs verwendet wird, mit der Ergänzung, dass die NIC und die „IPv4 IpConfiguration“ als primäre Schnittstelle mit dem Attribut **„primary“: true** eingestellt werden, wie im folgenden Beispiel gezeigt:

```json
          "networkProfile": {
            "networkInterfaceConfigurations": [
              {
                "name": "[variables('nicName')]",
                "properties": {
                  "primary": true,
          "networkSecurityGroup": {
            "id": "[resourceId('Microsoft.Network/networkSecurityGroups','VmssNsg')]"
          },                  
                  "ipConfigurations": [
                    {
                      "name": "[variables('ipConfigName')]",
                      "properties": {
                        "primary": true,
                        "subnet": {
                          "id": "[resourceId('Microsoft.Network/virtualNetworks/subnets', 'MyvirtualNetwork','Mysubnet')]"
                        },
                        "privateIPAddressVersion":"IPv4",                       
                        "publicipaddressconfiguration": {
                          "name": "pub1",
                          "properties": {
                            "idleTimeoutInMinutes": 15
                          }
                        },
                        "loadBalancerBackendAddressPools": [
                          {
                            "id": "[resourceId('Microsoft.Network/loadBalancers/backendAddressPools', 'loadBalancer', 'bePool'))]"
                          }
                        ],
                        "loadBalancerInboundNatPools": [
                          {
                            "id": "[resourceId('Microsoft.Network/loadBalancers/inboundNatPools', 'loadBalancer', 'natPool')]"
                          }
                        ]
                      }
                    },
                    {
                      "name": "[variables('ipConfigNameV6')]",
                      "properties": {
                        "subnet": {
                          "id": "[resourceId('Microsoft.Network/virtualNetworks/subnets','MyvirtualNetwork','Mysubnet')]"
                        },
                        "privateIPAddressVersion":"IPv6",
                        "loadBalancerBackendAddressPools": [
                          {
                            "id": "[resourceId('Microsoft.Network/loadBalancers/backendAddressPools', 'loadBalancer','bePoolv6')]"
                          }
                        ],                        
                      }
                    }
                  ]
                }
              }
            ]
          }

```


## <a name="sample-virtual-machine-scale-set-template-json"></a>JSON-Beispielvorlage für VM-Skalierungsgruppen

Informationen zum Bereitstellen einer Dual-Stack-VM-Skalierungsgruppe (IPv4 + IPv6) mit einem externen Dual-Stack-Lastenausgleich und einem virtuellen Netzwerk finden Sie [hier](https://azure.microsoft.com/resources/templates/ipv6-in-vnet-vmss/) in der Beispielvorlage.
## <a name="next-steps"></a>Nächste Schritte

Weitere Informationen zur IPv6-Unterstützung in virtuellen Azure-Netzwerken finden Sie unter [Was ist IPv6 für Azure Virtual Network?](ipv6-overview.md).
