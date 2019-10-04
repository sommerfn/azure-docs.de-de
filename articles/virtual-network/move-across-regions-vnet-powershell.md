---
title: Verschieben eines virtuellen Azure-Netzwerks in eine andere Azure-Region mit Azure PowerShell
description: Verwenden einer Azure Resource Manager-Vorlage, um ein virtuelles Azure-Netzwerk mit Azure PowerShell aus einer Azure-Region in eine andere zu verschieben.
author: asudbring
ms.service: virtual-network
ms.topic: article
ms.date: 08/26/2019
ms.author: allensu
ms.openlocfilehash: fe4c3fe131eb763ef8875cced91ab3ae22abca08
ms.sourcegitcommit: 8ef0a2ddaece5e7b2ac678a73b605b2073b76e88
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 09/17/2019
ms.locfileid: "71077954"
---
# <a name="move-azure-virtual-network-to-another-region-using-azure-powershell"></a>Verschieben eines virtuellen Azure-Netzwerks in eine andere Region mit Azure PowerShell

Es gibt verschiedene Szenarien, in denen Sie Ihre vorhandenen virtuellen Azure-Netzwerke (VNets) aus einer Region in eine andere verschieben möchten. Es könnte beispielsweise sein, dass Sie für Testzwecke oder Verfügbarkeitsprüfungen ein virtuelles Netzwerk erstellen möchten, das dieselbe Konfiguration hat wie Ihr vorhandenes virtuelles Netzwerk. Möglicherweise möchten Sie auch im Rahmen der Planung der Notfallwiederherstellung ein virtuelles Produktionsnetzwerk in eine andere Region verschieben.

Sie können eine Azure Resource Manager-Vorlage verwenden, um das Verschieben des virtuellen Netzwerks in eine andere Region auszuführen. Dazu exportieren Sie das virtuelle Netzwerk in eine Vorlage, ändern die Parameter entsprechend der Zielregion und stellen die Vorlage dann in der neuen Region bereit.  Weitere Informationen zu Resource Manager und Vorlagen finden Sie unter [Exportieren von Ressourcengruppen in Vorlagen](https://docs.microsoft.com/azure/azure-resource-manager/manage-resource-groups-powershell#export-resource-groups-to-templates).


## <a name="prerequisites"></a>Voraussetzungen

- Stellen Sie sicher, dass sich das virtuelle Azure-Netzwerk in der Azure-Region befindet, aus der Sie es verschieben möchten.

- Damit Sie ein virtuelles Netzwerk exportieren und eine Vorlage bereitstellen können, um ein virtuelles Netzwerk in einer anderen Region zu erstellen, benötigen Sie mindestens die Rolle „Netzwerkmitwirkender“.

- Peerings virtueller Netzwerke werden nicht neu erstellt und schlagen fehl, wenn sie noch in der Vorlage vorhanden sind.  Sie müssen alle virtuellen Netzwerkpeers vor dem Export der Vorlage entfernen und die Peers nach dem Verschieben des virtuellen Netzwerks erneut einrichten.
    
- Identifizieren Sie das Layout des Quellnetzwerks und alle Ressourcen, die Sie aktuell verwenden. Dieses Layout schließt u. a. Load Balancers, Netzwerksicherheitsgruppen (NSGs) und öffentliche IP-Adressen ein.

- Vergewissern Sie sich, dass Sie mit Ihrem Azure-Abonnement virtuelle Netzwerke in der gewählten Zielregion erstellen können. Wenden Sie sich an den Support, um das erforderliche Kontingent zu aktivieren.

- Stellen Sie sicher, dass Ihr Abonnement über genügend Ressourcen verfügt, um das Hinzufügen virtueller Netzwerke für diesen Prozess zu unterstützen.  Weitere Informationen finden Sie unter [Einschränkungen für Azure-Abonnements und Dienste, Kontingente und Einschränkungen](https://docs.microsoft.com/azure/azure-subscription-service-limits#networking-limits).


## <a name="prepare-and-move"></a>Vorbereiten und Verschieben
In den folgenden Schritten wird gezeigt, wie Sie das virtuelle Netzwerk auf das Verschieben mit einer Resource Manager-Vorlage vorbereiten und das virtuelle Netzwerk dann mit Azure PowerShell-Befehlen in die Zielregion verschieben.


[!INCLUDE [updated-for-az](../../includes/updated-for-az.md)]

### <a name="export-the-virtual-network-and-deploy-the-target-virtual-network-with-powershell"></a>Exportieren des virtuellen Netzwerks und Bereitstellen des virtuellen Zielnetzwerks mit PowerShell

1. Melden Sie sich mit dem Befehl [Connect-AzAccount](https://docs.microsoft.com/powershell/module/az.accounts/connect-azaccount?view=azps-2.5.0) bei Ihrem Azure-Abonnement an, und befolgen Sie die Anweisungen auf dem Bildschirm:
    
    ```azurepowershell-interactive
    Connect-AzAccount
    ```

2. Rufen Sie die Ressourcen-ID des virtuellen Netzwerks ab, das Sie in die Zielregion verschieben möchten, und platzieren Sie sie mithilfe von [Get-AzVirtualNetwork](https://docs.microsoft.com/powershell/module/az.network/get-azvirtualnetwork?view=azps-2.6.0) in einer Variablen:

    ```azurepowershell-interactive
    $sourceVNETID = (Get-AzVirtualNetwork -Name <source-virtual-network-name> -ResourceGroupName <source-resource-group-name>).Id

    ```
3. Exportieren Sie das virtuelle Quellnetzwerk in eine JSON-Datei in dem Verzeichnis, in dem Sie den Befehl [Export-AzResourceGroup](https://docs.microsoft.com/powershell/module/az.resources/export-azresourcegroup?view=azps-2.6.0) ausführen:
   
   ```azurepowershell-interactive
   Export-AzResourceGroup -ResourceGroupName <source-resource-group-name> -Resource $sourceVNETID -IncludeParameterDefaultValue
   ```

4. Die heruntergeladene Datei wird nach der Ressourcengruppe benannt, aus der die Ressource exportiert wurde.  Suchen Sie nach der Datei, die mit dem Befehl exportiert wurde und den Namen **\<source-resource-goup-name>.json** hat, und öffnen Sie sie in einem Editor Ihrer Wahl:
   
   ```azurepowershell
   notepad <source-resource-group-name>.json
   ```

5. Um den Parameter des Namens des virtuellen Netzwerk zu bearbeiten, ändern Sie die Eigenschaft **defaultValue** des Namens des virtuellen Quellnetzwerks in den Namen des virtuellen Zielnetzwerks. Stellen Sie sicher, dass der Name in Anführungszeichen eingeschlossen ist:
    
    ```json
        "$schema": "https://schema.management.azure.com/schemas/2015-01-01/deploymentmyResourceGroupVNET.json#",
    "contentVersion": "1.0.0.0",
    "parameters": {
        "virtualNetworks_myVNET1_name": {
        "defaultValue": "<target-virtual-network-name>",
        "type": "String"
        }
    ```

6.  Zum Bearbeiten der Zielregion, in die das VNET verschoben wird, ändern Sie die Eigenschaft **location** unter „resources“:

    ```json
    "resources": [
                {
                    "type": "Microsoft.Network/virtualNetworks",
                    "apiVersion": "2019-06-01",
                    "name": "[parameters('virtualNetworks_myVNET1_name')]",
                    "location": "<target-region>",
                    "properties": {
                        "provisioningState": "Succeeded",
                        "resourceGuid": "6e2652be-35ac-4e68-8c70-621b9ec87dcb",
                        "addressSpace": {
                            "addressPrefixes": [
                                "10.0.0.0/16"
                            ]
                        },

    ```
  
7. Zum Abrufen von Regionsstandortcodes können Sie das Azure PowerShell-Cmdlet [Get-AzLocation](https://docs.microsoft.com/powershell/module/az.resources/get-azlocation?view=azps-1.8.0) verwenden, indem Sie den folgenden Befehl ausführen:

    ```azurepowershell-interactive

    Get-AzLocation | format-table
    
    ```
8.  Sie können wahlweise auch andere Parameter in der Datei **\<resource-group-name>.json** ändern. Diese sind abhängig von Ihren Anforderungen optional:

    * **Adressraum**: Der Adressraum des VNETs kann vor dem Speichern geändert werden, indem der Abschnitt **resources** > **addessSpace** und die Eigenschaft **addressPrefixes** in der Datei **\<resource-group-name>.json** geändert werden:

        ```json
                "resources": [
                    {
                    "type": "Microsoft.Network/virtualNetworks",
                    "apiVersion": "2019-06-01",
                    "name": "[parameters('virtualNetworks_myVNET1_name')]",
                    "location": "<target-region",
                    "properties": {
                    "provisioningState": "Succeeded",
                    "resourceGuid": "6e2652be-35ac-4e68-8c70-621b9ec87dcb",
                    "addressSpace": {
                        "addressPrefixes": [
                        "10.0.0.0/16"
                        ]
                    },

        ```

    * **Subnetz**: Der Subnetzname und der Subnetzadressraum können geändert oder hinzugefügt werden, indem Sie den Abschnitt **subnets** der Datei **\<resource-group-name>.json** ändern. Der Name des Subnetzes kann geändert werden, indem Sie die Eigenschaft **name** ändern. Der Subnetzadressraum kann geändert werden, indem Sie die Eigenschaft **addressPrefix** in der Datei **\<resource-group-name>.json** ändern:

        ```json
                "subnets": [
                    {
                    "name": "subnet-1",
                    "etag": "W/\"d9f6e6d6-2c15-4f7c-b01f-bed40f748dea\"",
                    "properties": {
                    "provisioningState": "Succeeded",
                    "addressPrefix": "10.0.0.0/24",
                    "delegations": [],
                    "privateEndpointNetworkPolicies": "Enabled",
                    "privateLinkServiceNetworkPolicies": "Enabled"
                    }
                    },
                    {
                    "name": "GatewaySubnet",
                    "etag": "W/\"d9f6e6d6-2c15-4f7c-b01f-bed40f748dea\"",
                    "properties": {
                    "provisioningState": "Succeeded",
                    "addressPrefix": "10.0.1.0/29",
                    "serviceEndpoints": [],
                    "delegations": [],
                    "privateEndpointNetworkPolicies": "Enabled",
                    "privateLinkServiceNetworkPolicies": "Enabled"
                    }
                    }

                ]
        ```

         Die Datei **\<resource-group-name>.json** muss zum Ändern des Adresspräfixes an zwei Stellen bearbeitet werden: im oben aufgeführten Abschnitt und im unten aufgeführten Abschnitt **type**.  Ändern Sie die Eigenschaft **addressPrefix** so, dass Sie mit der oben genannten Eigenschaft übereinstimmt:

        ```json
         "type": "Microsoft.Network/virtualNetworks/subnets",
           "apiVersion": "2019-06-01",
           "name": "[concat(parameters('virtualNetworks_myVNET1_name'), '/GatewaySubnet')]",
              "dependsOn": [
                 "[resourceId('Microsoft.Network/virtualNetworks', parameters('virtualNetworks_myVNET1_name'))]"
                   ],
              "properties": {
                 "provisioningState": "Succeeded",
                 "addressPrefix": "10.0.1.0/29",
                 "serviceEndpoints": [],
                 "delegations": [],
                 "privateEndpointNetworkPolicies": "Enabled",
                 "privateLinkServiceNetworkPolicies": "Enabled"
                  }
                 },
                  {
                  "type": "Microsoft.Network/virtualNetworks/subnets",
                  "apiVersion": "2019-06-01",
                  "name": "[concat(parameters('virtualNetworks_myVNET1_name'), '/subnet-1')]",
                     "dependsOn": [
                        "[resourceId('Microsoft.Network/virtualNetworks', parameters('virtualNetworks_myVNET1_name'))]"
                          ],
                     "properties": {
                        "provisioningState": "Succeeded",
                        "addressPrefix": "10.0.0.0/24",
                        "delegations": [],
                        "privateEndpointNetworkPolicies": "Enabled",
                        "privateLinkServiceNetworkPolicies": "Enabled"
                         }
                  }
         ]
        ```

9.  Speichern Sie die Datei **\<resource-group-name>.json**.

10. Erstellen Sie mithilfe von [New-AzResourceGroup](https://docs.microsoft.com/powershell/module/az.resources/new-azresourcegroup?view=azps-2.6.0) eine Ressourcengruppe in der Zielregion für das Ziel-VNET, das bereitgestellt werden soll.
    
    ```azurepowershell-interactive
    New-AzResourceGroup -Name <target-resource-group-name> -location <target-region>
    ```
    
11. Stellen Sie die bearbeitete Datei **\<resource-group-name>.json** in der Ressourcengruppe bereit, die Sie im vorherigen Schritt mit [New-AzResourceGroupDeployment](https://docs.microsoft.com/powershell/module/az.resources/new-azresourcegroupdeployment?view=azps-2.6.0) erstellt haben:

    ```azurepowershell-interactive

    New-AzResourceGroupDeployment -ResourceGroupName <target-resource-group-name> -TemplateFile <source-resource-group-name>.json
    
    ```

12. Um zu überprüfen, ob die Ressourcen in der Zielregion erstellt wurden, verwenden Sie [Get-AzResourceGroup](https://docs.microsoft.com/powershell/module/az.resources/get-azresourcegroup?view=azps-2.6.0) und [Get-AzVirtualNetwork](https://docs.microsoft.com/powershell/module/az.network/get-azvirtualnetwork?view=azps-2.6.0):
    
    ```azurepowershell-interactive

    Get-AzResourceGroup -Name <target-resource-group-name>

    ```

    ```azurepowershell-interactive

    Get-AzVirtualNetwork -Name <target-virtual-network-name> -ResourceGroupName <target-resource-group-name>

    ```

## <a name="discard"></a>Verwerfen 

Wenn Sie nach der Bereitstellung neu starten oder das virtuelle Netzwerk im Ziel verwerfen möchten, löschen Sie die im Ziel erstellte Ressourcengruppe. Das virtuelle Netzwerk wird dann gelöscht.  Um die Ressourcengruppe zu entfernen, verwenden Sie [Remove-AzResourceGroup](https://docs.microsoft.com/powershell/module/az.resources/remove-azresourcegroup?view=azps-2.6.0):

```azurepowershell-interactive

Remove-AzResourceGroup -Name <target-resource-group-name>

```

## <a name="clean-up"></a>Bereinigen

Um die Änderungen zu übernehmen und die Verschiebung des virtuellen Netzwerks abzuschließen, löschen Sie das virtuelle Quellnetzwerk oder die Quellressourcengruppe mit [Remove-AzResourceGroup](https://docs.microsoft.com/powershell/module/az.resources/remove-azresourcegroup?view=azps-2.6.0) oder [Remove-AzVirtualNetwork](https://docs.microsoft.com/powershell/module/az.network/remove-azvirtualnetwork?view=azps-2.6.0):

```azurepowershell-interactive

Remove-AzResourceGroup -Name <source-resource-group-name>

```

``` azurepowershell-interactive

Remove-AzVirtualNetwork -Name <source-virtual-network-name> -ResourceGroupName <source-resource-group-name>

```

## <a name="next-steps"></a>Nächste Schritte

In diesem Tutorial haben Sie ein virtuelles Azure-Netzwerk aus einer Region in eine andere verschoben und die Quellressourcen bereinigt.  Weitere Informationen zum Verschieben von Ressourcen zwischen Regionen und zur Notfallwiederherstellung in Azure finden Sie unter:


- [Verschieben von Ressourcen in eine neue Ressourcengruppe oder ein neues Abonnement](https://docs.microsoft.com/azure/azure-resource-manager/resource-group-move-resources)
- [Verschieben virtueller Azure-Computer in eine andere Region](https://docs.microsoft.com/azure/site-recovery/azure-to-azure-tutorial-migrate)
