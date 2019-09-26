---
title: Verschieben eines externen Azure Load Balancers mithilfe des Azure-Portals in eine andere Azure-Region
description: Verwenden Sie eine Azure Resource Manager-Vorlage, um den externen Azure Load Balancer mithilfe des Azure-Portals aus einer Azure-Region in eine andere zu verschieben.
author: asudbring
ms.service: load-balancer
ms.topic: article
ms.date: 09/17/2019
ms.author: allensu
ms.openlocfilehash: eda0d6e8fe56b985c3b29fa80cee880444d63741
ms.sourcegitcommit: 1c9858eef5557a864a769c0a386d3c36ffc93ce4
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 09/18/2019
ms.locfileid: "71105294"
---
# <a name="move-azure-external-load-balancer-to-another-region-using-the-azure-portal"></a>Verschieben eines externen Azure Load Balancers mithilfe des Azure-Portals in eine andere Region

Es gibt verschiedene Szenarien, in denen Sie einen vorhandenen externen Azure Load Balancer aus einer Region in eine andere verschieben möchten. Ein Beispiel ist das Erstellen eines externen Load Balancers mit derselben Konfiguration zu Testzwecken. Möglicherweise möchten Sie einen externen Load Balancer auch im Rahmen der Planung der Notfallwiederherstellung in eine andere Region verschieben.

Externe Azure Load Balancer können nicht aus einer Region in eine andere verschoben werden. Sie können jedoch eine Azure Resource Manager-Vorlage verwenden, um die vorhandene Konfiguration und die öffentliche IP-Adresse eines externen Load Balancers zu exportieren.  Anschließend können Sie die Ressource in einer anderen Region bereitstellen, indem Sie den Load Balancer und die öffentliche IP-Adresse in eine Vorlage exportieren, die Parameter so ändern, dass Sie der Zielregion entsprechen, und die Vorlagen dann in der neuen Region bereitstellen.  Weitere Informationen zu Resource Manager und Vorlagen finden Sie unter [Exportieren von Ressourcengruppen in Vorlagen](https://docs.microsoft.com/azure/azure-resource-manager/manage-resource-groups-powershell#export-resource-groups-to-templates).


## <a name="prerequisites"></a>Voraussetzungen

- Stellen Sie sicher, dass sich der externe Azure Load Balancer in der Azure-Region befindet, aus der Sie ihn verschieben möchten.

- Externe Azure Load Balancer können nicht zwischen Regionen verschoben werden.  Sie müssen den neuen Load Balancer zu Ressourcen in der Zielregion zuordnen.

- Zum Exportieren der Konfiguration eines externen Load Balancers und Bereitstellen einer Vorlage zum Erstellen eines externen Load Balancers in einer anderen Region benötigen Sie mindestens die Rolle „Netzwerkmitwirkender“.
   
- Identifizieren Sie das Layout des Quellnetzwerks und alle Ressourcen, die Sie aktuell verwenden. Dieses Layout umfasst, ohne darauf beschränkt zu sein, Load Balancer, Netzwerksicherheitsgruppen, öffentliche IP-Adressen und virtuelle Netzwerke.

- Vergewissern Sie sich, dass Ihr Azure-Abonnement das Erstellen externer Load Balancer in der gewählten Zielregion zulässt. Wenden Sie sich an den Support, um das erforderliche Kontingent zu aktivieren.

- Stellen Sie sicher, dass Ihr Abonnement über genügend Ressourcen verfügt, um das Hinzufügen von Load Balancern für diesen Prozess zu unterstützen.  Weitere Informationen finden Sie unter [Einschränkungen für Azure-Abonnements und Dienste, Kontingente und Einschränkungen](https://docs.microsoft.com/azure/azure-subscription-service-limits#networking-limits).


## <a name="prepare-and-move"></a>Vorbereiten und Verschieben
Die folgenden Schritte zeigen, wie Sie den externen Load Balancer für die Verschiebung mit einer Resource Manager-Vorlage vorbereiten und die Konfiguration des externen Load Balancers dann im Azure-Portal in die Zielregion verschieben.  Im Rahmen dieses Vorgangs muss die Konfiguration der öffentlichen IP-Adresse des externen Load Balancers eingeschlossen und vor dessen Verschieben zuerst durchgeführt werden.


[!INCLUDE [updated-for-az](../../includes/updated-for-az.md)]

### <a name="export-the-public-ip-template-and-deploy-from-the-portal"></a>Exportieren der Vorlage für die öffentliche IP-Adresse und Bereitstellen über das Portal

1. Melden Sie sich bei [Azure-Portal](http://portal.azure.com) > **Ressourcengruppen** an.
2. Suchen Sie die Ressourcengruppe, die die öffentliche IP-Adresse der Quelle enthält, und klicken Sie darauf.
3. Wählen Sie **Einstellungen** > **Vorlage exportieren** aus.
4. Wählen Sie auf dem Blatt **Vorlage exportieren** die Option **Bereitstellen** aus.
5. Klicken Sie auf **VORLAGE** > **Parameter bearbeiten**, um die Datei **parameters.json** im Online-Editor zu öffnen.
8. Um den Parameter für den Namen der öffentlichen IP-Adresse zu bearbeiten, ändern Sie die Eigenschaft unter **Parameter** > **Wert** vom Namen der öffentlichen IP-Adresse der Quelle in den Namen der öffentliche IP-Adresse des Ziels. Setzen Sie den Namen unbedingt in Anführungszeichen:

    ```json
            {
        "$schema": "https://schema.management.azure.com/schemas/2015-01-01/deploymentParameters.json#",
        "contentVersion": "1.0.0.0",
        "parameters": {
            "publicIPAddresses_myVM1pubIP_name": {
            "value": "<target-publicip-name>"
              }
             }
            }

    ```

    Klicken Sie im Editor auf **Speichern**.

9.  Klicken Sie auf **VORLAGE** > **Parameter bearbeiten**, um die Datei **parameters.json** im Online-Editor zu öffnen. 

10. Zum Bearbeiten der Zielregion, in die die öffentliche IP-Adresse verschoben wird, ändern Sie unter **resources** die Eigenschaft **location**:

    ```json
            "resources": [
            {
            "type": "Microsoft.Network/publicIPAddresses",
            "apiVersion": "2019-06-01",
            "name": "[parameters('publicIPAddresses_myPubIP_name')]",
            "location": "<target-region>",
            "sku": {
                "name": "Basic",
                "tier": "Regional"
            },
            "properties": {
                "provisioningState": "Succeeded",
                "resourceGuid": "7549a8f1-80c2-481a-a073-018f5b0b69be",
                "ipAddress": "52.177.6.204",
                "publicIPAddressVersion": "IPv4",
                "publicIPAllocationMethod": "Dynamic",
                "idleTimeoutInMinutes": 4,
                "ipTags": []
               }
               }
             ]             
    ```
  
11. Informationen zum Abrufen von Regionsstandortcodes finden Sie unter [Azure-Standorte](https://azure.microsoft.com/global-infrastructure/locations/).  Der Code für eine Region ist der Regionsname ohne Leerzeichen, **USA, Mitte** = **centralus**.
    
12. Sie können wahlweise auch andere Parameter in der Vorlage ändern, die abhängig von Ihren Anforderungen optional sind:

    * **SKU**: Sie können die SKU der öffentlichen IP-Adresse in der Konfiguration von „standard“ in „basic“ oder von „basic“ in „standard“ ändern, indem Sie die Eigenschaft **sku** > **name** in der Datei **template.json** ändern:

        ```json
          "resources": [
         {
            "type": "Microsoft.Network/publicIPAddresses",
            "apiVersion": "2019-06-01",
            "name": "[parameters('publicIPAddresses_myPubIP_name')]",
            "location": "<target-region>",
            "sku": {
                "name": "Basic",
                "tier": "Regional"
            },
        ```

        Weitere Informationen zu den Unterschieden zwischen den SKUs „basic“ und „standard“ für öffentliche IP-Adressen finden Sie unter [Erstellen, Ändern oder Löschen einer öffentlichen IP-Adresse](https://docs.microsoft.com/azure/virtual-network/virtual-network-public-ip-address):

    * **Zuweisungsmethode für öffentliche IP-Adresse** und **Leerlauftimeout**: Sie können beide Optionen in der Vorlage ändern, indem Sie die Eigenschaft **publicIPAllocationMethod** von **Dynamic** in **Static** oder von **Static** in **Dynamic** ändern. Das Zeitlimit für Leerlauf kann geändert werden, indem Sie die Eigenschaft **idleTimeoutInMinutes** auf den gewünschten Wert festlegen.  Der Standardwert ist **4**:

        ```json
          "resources": [
         {
            "type": "Microsoft.Network/publicIPAddresses",
            "apiVersion": "2019-06-01",
            "name": "[parameters('publicIPAddresses_myPubIP_name')]",
            "location": "<target-region>",
            "sku": {
                "name": "Basic",
                "tier": "Regional"
            },
            "properties": {
                "provisioningState": "Succeeded",
                "resourceGuid": "7549a8f1-80c2-481a-a073-018f5b0b69be",
                "ipAddress": "52.177.6.204",
                "publicIPAddressVersion": "IPv4",
                "publicIPAllocationMethod": "Dynamic",
                "idleTimeoutInMinutes": 4,
                "ipTags": []
        
        ```

        Weitere Informationen zu den Zuweisungsmethoden und Werten für das Zeitlimit für Leerlauf finden Sie unter [Erstellen, Ändern oder Löschen einer öffentlichen IP-Adresse](https://docs.microsoft.com/azure/virtual-network/virtual-network-public-ip-address).

 
13. Klicken Sie im Online-Editor auf **Speichern**.

14. Klicken Sie auf **GRUNDLAGEN** > **Abonnement**,um das Abonnement auszuwählen, in dem die öffentliche IP-Adresse des Ziels bereitgestellt wird.

15. Klicken Sie auf **GRUNDLAGEN** > **Ressourcengruppe**,um die Ressourcengruppe auszuwählen, in der die öffentliche IP-Adresse des Ziels bereitgestellt wird.  Sie können auf **Neu erstellen** klicken, um eine neue Ressourcengruppe für die öffentliche IP-Adresse des Ziels zu erstellen.  Stellen Sie sicher, dass der Name nicht mit dem Namen der Quellressourcengruppe der vorhandenen öffentlichen IP-Adresse der Quelle übereinstimmt. 

16. Stellen Sie sicher, dass **GRUNDLAGEN** > **Standort** auf den Zielstandort festgelegt ist, an dem die öffentliche IP-Adresse bereitgestellt werden soll.

17. Stellen Sie unter **EINSTELLUNGEN** sicher, dass der Name mit dem Namen übereinstimmt, den Sie oben im Parameter-Editor eingegeben haben.

18. Aktivieren Sie das Kontrollkästchen unter **NUTZUNGSBEDINGUNGEN**.

19. Klicken Sie auf die Schaltfläche **Kaufen**, um die öffentliche IP-Adresse des Ziels bereitzustellen.
20. Wenn Sie eine andere öffentliche IP-Adresse haben, die für den zu verschiebenden Load Balancer für ausgehende NAT (Network Address Translation, Netzwerkadressübersetzung) verwendet wird, wiederholen Sie die obigen Schritte, um die zweite ausgehende öffentliche IP-Adresse zu exportieren und in der Zielregion bereitzustellen.

### <a name="export-the-external-load-balancer-template-and-deploy-from-the-azure-portal"></a>Exportieren der Vorlage des externen Load Balancers und Bereitstellen über das Azure-Portal

1. Melden Sie sich bei [Azure-Portal](http://portal.azure.com) > **Ressourcengruppen** an.
2. Suchen Sie die Ressourcengruppe, die den externen Load Balancer der Quelle enthält, und klicken Sie darauf.
3. Wählen Sie **Einstellungen** > **Vorlage exportieren** aus.
4. Wählen Sie auf dem Blatt **Vorlage exportieren** die Option **Bereitstellen** aus.
5. Klicken Sie auf **VORLAGE** > **Parameter bearbeiten**, um die Datei **parameters.json** im Online-Editor zu öffnen.

5. Um den Parameter für den Namen des externen Load Balancers zu bearbeiten, ändern Sie die Eigenschaft **value** des Namens des externen Load Balancers der Quelle in den Namen Ihres externen Load Balancers für das Ziel. Setzen Sie den Namen unbedingt in Anführungszeichen:

    ```json
       "$schema": "https://schema.management.azure.com/schemas/2015-01-01/deploymentParameters.json#",
       "contentVersion": "1.0.0.0",
       "parameters": {
          "loadBalancers_myLoadbalancer_ext_name": {
          "value": "<target-external-lb-name>"
    },
          "publicIPAddresses_myPubIP_in_externalid": {
          "value": "<target-publicIP-resource-ID>"
    },

    ```

6.  Wenn Sie den Wert der zuvor verschobenen öffentlichen IP-Adresse des Ziels bearbeiten möchten, müssen Sie zunächst die Ressourcen-ID abrufen und kopieren und dann in die Datei **parameters.json** einfügen. So rufen Sie die ID ab
    
    1. Melden Sie sich auf einer anderen Browserregisterkarte oder in einem anderen Fenster bei [Azure-Portal](http://portal.azure.com) > **Ressourcengruppen** an.
    2. Suchen Sie die Zielressourcengruppe, die die mit den oben beschriebenen Schritten verschobene öffentliche IP-Adresse enthält, und klicken Sie darauf.
    3. Wählen Sie **Einstellungen** > **Eigenschaften** aus.
    4. Markieren Sie auf dem Blatt auf der rechten Seite die **Ressourcen-ID**, und kopieren Sie sie in die Zwischenablage.  Alternativ können Sie rechts neben dem Pfad der **Ressourcen-ID** auf die Schaltfläche **In Zwischenablage kopieren** klicken.
    5. Fügen Sie die Ressourcen-ID in die Eigenschaft **value** im Editor **Parameter bearbeiten** ein, den Sie in einem anderen Browserfenster oder auf einer anderen Registerkarte geöffnet haben:

        ```json
           ```json
           "$schema": "https://schema.management.azure.com/schemas/2015-01-01/deploymentParameters.json#",
           "contentVersion": "1.0.0.0",
           "parameters": {
              "loadBalancers_myLoadbalancer_ext_name": {
              "value": "<target-external-lb-name>"
        },
              "publicIPAddresses_myPubIP_in_externalid": {
              "value": "<target-publicIP-resource-ID>"
        },

        ```
    6. Klicken Sie im Online-Editor auf **Speichern**.
   

7.  Wenn Sie für den Load Balancer NAT-Ausgangsregeln konfiguriert haben, ist in dieser Datei ein dritter Eintrag für die externe ID der ausgehenden öffentlichen IP-Adresse vorhanden.  Wiederholen Sie die oben genannten Schritte in der **Zielregion**, um die ID der ausgehenden öffentlichen IP-Adresse zu ermitteln, und fügen Sie diesen Eintrag in die Datei **parameters.json** ein:

    ```json
            "$schema": "https://schema.management.azure.com/schemas/2015-01-01/deploymentTemplate.json#",
            "contentVersion": "1.0.0.0",
            "parameters": {
                "loadBalancers_myLoadbalancer_ext_name": {
                "value": "<target-external-lb-name>",
                
            },
                "publicIPAddresses_myPubIP_in_externalid": {
                "value": "<target-publicIP-resource-ID>",
                
            },
                "publicIPAddresses_myPubIP_out_externalid": {
                "defaultValue": "<target-publicIP-outbound-resource-ID>",
                
            }
        },
    ```

8.  Klicken Sie auf **VORLAGE** > **Parameter bearbeiten**, um die Datei **parameters.json** im Online-Editor zu öffnen.
9.  Zum Bearbeiten der Zielregion, in die die Konfiguration des externen Load Balancers verschoben wird, ändern Sie die Eigenschaft **location** unter **resources** in der Datei **template.json**:

    ```json
        "resources": [
            {
                "type": "Microsoft.Network/loadBalancers",
                "apiVersion": "2019-06-01",
                "name": "[parameters('loadBalancers_myLoadBalancer_name')]",
                "location": "<target-external-lb-region>",
                "sku": {
                    "name": "Standard",
                    "tier": "Regional"
                },
    ```

10. Informationen zum Abrufen von Regionsstandortcodes finden Sie unter [Azure-Standorte](https://azure.microsoft.com/global-infrastructure/locations/).  Der Code für eine Region ist der Regionsname ohne Leerzeichen, **USA, Mitte** = **centralus**.

11. Sie können wahlweise auch andere Parameter in der Vorlage ändern, die abhängig von Ihren Anforderungen optional sind:
    
    * **SKU**: Sie können die SKU des externen Load Balancers in der Konfiguration von „standard“ in „basic“ oder von „basic“ in „standard“ ändern, indem Sie die Eigenschaft **sku** > **name** in der Datei **template.json** ändern:

        ```json
        "resources": [
        {
            "type": "Microsoft.Network/loadBalancers",
            "apiVersion": "2019-06-01",
            "name": "[parameters('loadBalancers_myLoadBalancer_name')]",
            "location": "<target-external-lb-region>",
            "sku": {
                "name": "Standard",
                "tier": "Regional"
            },
        ```
      Weitere Informationen zu den Unterschieden zwischen Load Balancern der SKU „basic“ und „standard“ finden Sie unter [Übersicht über Azure Load Balancer Standard](https://docs.microsoft.com/azure/load-balancer/load-balancer-standard-overview).

    * **Lastenausgleichsregeln**: Sie können Lastenausgleichsregeln in der Konfiguration hinzufügen oder entfernen, indem Sie Einträge in der Datei **template.json** zum Abschnitt **loadBalancingRules** hinzufügen oder daraus entfernen:

        ```json
        "loadBalancingRules": [
                    {
                        "name": "myInboundRule",
                        "etag": "W/\"39e5e9cd-2d6d-491f-83cf-b37a259d86b6\"",
                        "properties": {
                            "provisioningState": "Succeeded",
                            "frontendIPConfiguration": {
                                "id": "[concat(resourceId('Microsoft.Network/loadBalancers', parameters('loadBalancers_myLoadBalancer_name')), '/frontendIPConfigurations/myfrontendIPinbound')]"
                            },
                            "frontendPort": 80,
                            "backendPort": 80,
                            "enableFloatingIP": false,
                            "idleTimeoutInMinutes": 4,
                            "protocol": "Tcp",
                            "enableTcpReset": false,
                            "loadDistribution": "Default",
                            "disableOutboundSnat": true,
                            "backendAddressPool": {
                                "id": "[concat(resourceId('Microsoft.Network/loadBalancers', parameters('loadBalancers_myLoadBalancer_name')), '/backendAddressPools/myBEPoolInbound')]"
                            },
                            "probe": {
                                "id": "[concat(resourceId('Microsoft.Network/loadBalancers', parameters('loadBalancers_myLoadBalancer_name')), '/probes/myHTTPProbe')]"
                            }
                        }
                    }
                ]
        ```
       Weitere Informationen zu Lastenausgleichsregeln finden Sie unter [Was ist Azure Load Balancer?](https://docs.microsoft.com/azure/load-balancer/load-balancer-overview)

    * **Tests**: Sie können in der Konfiguration einen Test des Load Balancers hinzufügen oder entfernen, indem Sie der Datei **template.json** Einträge zum Abschnitt **Probes** hinzufügen oder sie daraus entfernen:

        ```json
        "probes": [
                    {
                        "name": "myHTTPProbe",
                        "etag": "W/\"39e5e9cd-2d6d-491f-83cf-b37a259d86b6\"",
                        "properties": {
                            "provisioningState": "Succeeded",
                            "protocol": "Http",
                            "port": 80,
                            "requestPath": "/",
                            "intervalInSeconds": 15,
                            "numberOfProbes": 2
                        }
                    }
                ],
        ```
       Weitere Informationen zu Azure Load Balancer-Integritätstests finden Sie unter [Load Balancer-Integritätstests](https://docs.microsoft.com/azure/load-balancer/load-balancer-custom-probe-overview).

    * **NAT-Regeln für eingehenden Datenverkehr**: Sie können NAT-Regeln für eingehenden Datenverkehr für den Load Balancer hinzufügen oder entfernen, indem Sie der Datei **template.json** Einträge zum Abschnitt **inboundNatRules** hinzufügen oder sie daraus entfernen:

        ```json
        "inboundNatRules": [
                    {
                        "name": "myInboundNATRule",
                        "etag": "W/\"39e5e9cd-2d6d-491f-83cf-b37a259d86b6\"",
                        "properties": {
                            "provisioningState": "Succeeded",
                            "frontendIPConfiguration": {
                                "id": "[concat(resourceId('Microsoft.Network/loadBalancers', parameters('loadBalancers_myLoadBalancer_name')), '/frontendIPConfigurations/myfrontendIPinbound')]"
                            },
                            "frontendPort": 4422,
                            "backendPort": 3389,
                            "enableFloatingIP": false,
                            "idleTimeoutInMinutes": 4,
                            "protocol": "Tcp",
                            "enableTcpReset": false
                        }
                    }
                ]
        ```
        Um das Hinzufügen oder Entfernen einer NAT-Regel für eingehenden Datenverkehr abzuschließen, muss die Regel als Eigenschaft des Typs **type** am Ende der Datei **template.json** vorhanden sein oder aus ihr entfernt werden:

        ```json
        {
            "type": "Microsoft.Network/loadBalancers/inboundNatRules",
            "apiVersion": "2019-06-01",
            "name": "[concat(parameters('loadBalancers_myLoadBalancer_name'), '/myInboundNATRule')]",
            "dependsOn": [
                "[resourceId('Microsoft.Network/loadBalancers', parameters('loadBalancers_myLoadBalancer_name'))]"
            ],
            "properties": {
                "provisioningState": "Succeeded",
                "frontendIPConfiguration": {
                    "id": "[concat(resourceId('Microsoft.Network/loadBalancers', parameters('loadBalancers_myLoadBalancer_name')), '/frontendIPConfigurations/myfrontendIPinbound')]"
                },
                "frontendPort": 4422,
                "backendPort": 3389,
                "enableFloatingIP": false,
                "idleTimeoutInMinutes": 4,
                "protocol": "Tcp",
                "enableTcpReset": false
            }
        }
        ```
        Weitere Informationen zu NAT-Regeln für eingehenden Datenverkehr finden Sie unter [Was ist Azure Load Balancer?](https://docs.microsoft.com/azure/load-balancer/load-balancer-overview)

    * **Ausgangsregeln**: Sie können Regeln für ausgehenden Datenverkehr in der Konfiguration hinzufügen oder aus ihr entfernen, indem Sie die Eigenschaft **outboundRules** in der Datei **template.json** bearbeiten:

        ```json
        "outboundRules": [
                    {
                        "name": "myOutboundRule",
                        "etag": "W/\"39e5e9cd-2d6d-491f-83cf-b37a259d86b6\"",
                        "properties": {
                            "provisioningState": "Succeeded",
                            "allocatedOutboundPorts": 10000,
                            "protocol": "All",
                            "enableTcpReset": false,
                            "idleTimeoutInMinutes": 15,
                            "backendAddressPool": {
                                "id": "[concat(resourceId('Microsoft.Network/loadBalancers', parameters('loadBalancers_myLoadBalancer_name')), '/backendAddressPools/myBEPoolOutbound')]"
                            },
                            "frontendIPConfigurations": [
                                {
                                    "id": "[concat(resourceId('Microsoft.Network/loadBalancers', parameters('loadBalancers_myLoadBalancer_name')), '/frontendIPConfigurations/myfrontendIPoutbound')]"
                                }
                            ]
                        }
                    }
                ]
        ```

         Weitere Informationen zu Regeln für ausgehenden Datenverkehr finden Sie unter [Load Balancer-Ausgangsregeln](https://docs.microsoft.com/azure/load-balancer/load-balancer-outbound-rules-overview).

12. Klicken Sie im Online-Editor auf **Speichern**.
    
13. Klicken Sie auf **GRUNDLAGEN** > **Abonnement**, um das Abonnement auszuwählen, in dem der interne Load Balancer für das Ziel bereitgestellt wird.

15. Klicken Sie auf **GRUNDLAGEN** > **Ressourcengruppe**, um die Ressourcengruppe auszuwählen, in der der Load Balancer für das Ziel bereitgestellt wird.  Sie können auf **Neu erstellen** klicken, um eine neue Ressourcengruppe für den internen Load Balancer für das Ziel zu erstellen, oder Sie können die vorhandene Ressourcengruppe auswählen, die weiter oben für die öffentliche IP-Adresse erstellt wurde.  Stellen Sie sicher, dass der Name nicht mit dem Namen der Quellressourcengruppe des vorhandenen internen Load Balancers des Quelle übereinstimmt. 

16. Stellen Sie sicher, dass **GRUNDLAGEN** > **Standort** auf den Zielstandort festgelegt ist, an dem der interne Load Balancer bereitgestellt werden soll.

17. Stellen Sie unter **EINSTELLUNGEN** sicher, dass der Name mit dem Namen übereinstimmt, den Sie oben im Parameter-Editor eingegeben haben.  Vergewissern Sie sich, dass die Ressourcen-IDs für öffentliche IP-Adressen in der Konfiguration entsprechend aufgefüllt werden.

18. Aktivieren Sie das Kontrollkästchen unter **NUTZUNGSBEDINGUNGEN**.

19. Klicken Sie auf die Schaltfläche **Kaufen**, um die öffentliche IP-Adresse des Ziels bereitzustellen.

## <a name="discard"></a>Verwerfen 

Wenn Sie die öffentliche IP-Adresse des Ziels und den externen Load Balancer verwerfen möchten, löschen Sie die Ressourcengruppe, die die öffentliche IP-Adresse des Ziels und den externen Load Balancer enthält.  Wählen Sie hierzu im Portal die Ressourcengruppe im Dashboard aus, und klicken Sie oben auf der Übersichtsseite auf **Löschen**.

## <a name="clean-up"></a>Bereinigen

Um die Änderungen zu übernehmen und die Verschiebung der öffentlichen IP-Adresse und des externen Load Balancers abzuschließen, löschen Sie die öffentliche IP-Adresse sowie den externen Load Balancer oder die Ressourcengruppe der Quelle. Wählen Sie hierzu im Portal die öffentliche IP-Adresse und den externen Load Balancer oder die Ressourcengruppe im Dashboard aus, und klicken Sie oben auf der Übersichtsseite auf **Löschen**.

## <a name="next-steps"></a>Nächste Schritte

In diesem Tutorial haben Sie einen externen Azure Load Balancer aus einer Region in eine andere verschoben und die Quellressourcen bereinigt.  Weitere Informationen zum Verschieben von Ressourcen zwischen Regionen und zur Notfallwiederherstellung in Azure finden Sie unter:


- [Verschieben von Ressourcen in eine neue Ressourcengruppe oder ein neues Abonnement](https://docs.microsoft.com/azure/azure-resource-manager/resource-group-move-resources)
- [Verschieben virtueller Azure-Computer in eine andere Region](https://docs.microsoft.com/azure/site-recovery/azure-to-azure-tutorial-migrate)
