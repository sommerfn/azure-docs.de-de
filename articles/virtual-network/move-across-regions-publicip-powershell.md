---
title: Verschieben einer öffentlichen Azure-IP-Adresse in eine andere Azure-Region mit Azure PowerShell
description: Verwenden einer Azure Resource Manager-Vorlage, um eine öffentliche Azure-IP-Adresse mit Azure PowerShell aus einer Azure-Region in eine andere zu verschieben.
author: asudbring
ms.service: virtual-network
ms.topic: article
ms.date: 08/29/2019
ms.author: allensu
ms.openlocfilehash: d18dfa7ebed3aefbf6fdb3ffdb6fdd2cf2160cb4
ms.sourcegitcommit: 0fab4c4f2940e4c7b2ac5a93fcc52d2d5f7ff367
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 09/17/2019
ms.locfileid: "71038880"
---
# <a name="move-azure-public-ip-to-another-region-using-azure-powershell"></a>Verschieben einer öffentlichen Azure-IP-Adresse in eine andere Region mit Azure PowerShell

Es gibt verschiedene Szenarien, in denen Sie Ihre vorhandenen öffentlichen Azure-IP-Adressen aus einer Region in eine andere verschieben möchten. Beispielsweise könnte es sein, dass Sie für Testzwecke eine öffentliche IP-Adresse mit derselben Konfiguration und demselben Tarif erstellen möchten. Möglicherweise möchten Sie eine öffentliche IP-Adresse auch im Rahmen der Planung einer Notfallwiederherstellung in eine andere Region verschieben.

Öffentliche Azure-IP-Adressen sind regionsspezifisch und können nicht aus einer Region in eine andere verschoben werden. Sie können jedoch eine Azure Resource Manager-Vorlage verwenden, um die vorhandene Konfiguration einer öffentlichen IP-Adresse zu exportieren.  Anschließend können Sie die Ressource in einer anderen Region stagen, indem Sie die öffentliche IP-Adresse in eine Vorlage exportieren, die Parameter so ändern, dass sie der Zielregion entsprechen, und die Vorlage dann in der neuen Region bereitstellen.  Weitere Informationen zu Resource Manager und Vorlagen finden Sie unter [Exportieren von Ressourcengruppen in Vorlagen](https://docs.microsoft.com/azure/azure-resource-manager/manage-resource-groups-powershell#export-resource-groups-to-templates).


## <a name="prerequisites"></a>Voraussetzungen

- Stellen Sie sicher, dass sich die öffentliche IP-Adresse in der Azure-Region befindet, aus der Sie verschieben möchten.

- Öffentliche Azure-IP-Adressen können nicht zwischen Regionen verschoben werden.  Sie müssen die neue öffentliche IP-Adresse Ressourcen in der Zielregion zuordnen.

- Damit Sie eine öffentliche IP-Adresse exportieren und eine Vorlage bereitstellen können, um die öffentliche IP-Adresse in einer anderen Region zu erstellen, benötigen Sie mindestens die Rolle „Netzwerkmitwirkender“.
   
- Identifizieren Sie das Layout des Quellnetzwerks und alle Ressourcen, die Sie aktuell verwenden. Dieses Layout umfasst Load Balancer, Netzwerksicherheitsgruppen (NSGs) und virtuelle Netzwerke, ohne darauf beschränkt zu sein.

- Vergewissern Sie sich, dass Sie mit Ihrem Azure-Abonnement öffentliche IP-Adressen in der verwendeten Zielregion erstellen können. Wenden Sie sich an den Support, um das erforderliche Kontingent zu aktivieren.

- Stellen Sie sicher, dass Ihr Abonnement genügend Ressourcen hat, um das Hinzufügen von öffentlichen IP-Adressen für diesen Prozess zu unterstützen.  Weitere Informationen finden Sie unter [Einschränkungen für Azure-Abonnements und Dienste, Kontingente und Einschränkungen](https://docs.microsoft.com/azure/azure-subscription-service-limits#networking-limits).


## <a name="prepare-and-move"></a>Vorbereiten und Verschieben
In den folgenden Schritten wird gezeigt, wie Sie die öffentliche IP-Adresse für die Konfigurationsverschiebung mit einer Resource Manager-Vorlage vorbereiten und die Konfiguration der öffentlichen IP-Adresse dann über Azure PowerShell in die Zielregion verschieben.


[!INCLUDE [updated-for-az](../../includes/updated-for-az.md)]

### <a name="export-the-template-and-deploy-from-a-script"></a>Exportieren der Vorlage und Bereitstellen über ein Skript

1. Melden Sie sich mit dem Befehl [Connect-AzAccount](https://docs.microsoft.com/powershell/module/az.accounts/connect-azaccount?view=azps-2.5.0) bei Ihrem Azure-Abonnement an, und befolgen Sie die Anweisungen auf dem Bildschirm:
    
    ```azurepowershell-interactive
    Connect-AzAccount
    ```

2. Rufen Sie die Ressourcen-ID der öffentlichen IP-Adresse ab, die Sie in die Zielregion verschieben möchten, und platzieren Sie sie mithilfe von [Get-AzPublicIPAddress](https://docs.microsoft.com/powershell/module/az.network/get-azpublicipaddress?view=azps-2.6.0) in einer Variablen:

    ```azurepowershell-interactive
    $sourcePubIPID = (Get-AzPublicIPaddress -Name <source-public-ip-name> -ResourceGroupName <source-resource-group-name>).Id

    ```
3. Exportieren Sie das virtuelle Quellnetzwerk in eine JSON-Datei in dem Verzeichnis, in dem Sie den Befehl [Export-AzResourceGroup](https://docs.microsoft.com/powershell/module/az.resources/export-azresourcegroup?view=azps-2.6.0) ausführen:
   
   ```azurepowershell-interactive
   Export-AzResourceGroup -ResourceGroupName <source-resource-group-name> -Resource $sourceVNETID -IncludeParameterDefaultValue
   ```

4. Die heruntergeladene Datei wird nach der Ressourcengruppe benannt, aus der die Ressource exportiert wurde.  Suchen Sie nach der Datei, die mit dem Befehl exportiert wurde und den Namen **\<source-resource-goup-name>.json** hat, und öffnen Sie sie in einem Editor Ihrer Wahl:
   
   ```azurepowershell
   notepad <source-resource-group-name>.json
   ```

5. Um den Parameter für den Namen der öffentlichen IP-Adresse zu bearbeiten, ändern Sie die Eigenschaft **defaultValue** des Namens der öffentlichen IP-Adresse der Quelle in den Namen der öffentlichen IP-Adresse des Ziels. Stellen Sie sicher, dass der Name in Anführungszeichen eingeschlossen ist:
    
    ```json
        {
    "$schema": "https://schema.management.azure.com/schemas/2015-01-01/deploymentTemplate.json#",
    "contentVersion": "1.0.0.0",
    "parameters": {
        "publicIPAddresses_myVM1pubIP_name": {
        "defaultValue": "<target-publicip-name>",
        "type": "String"
        }
    }

    ```

6. Zum Bearbeiten der Zielregion, in die die öffentliche IP-Adresse verschoben wird, ändern Sie die Eigenschaft **location** unter „resources“:

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
  
7. Zum Abrufen von Regionsstandortcodes können Sie das Azure PowerShell-Cmdlet [Get-AzLocation](https://docs.microsoft.com/powershell/module/az.resources/get-azlocation?view=azps-1.8.0) verwenden, indem Sie den folgenden Befehl ausführen:

    ```azurepowershell-interactive

    Get-AzLocation | format-table
    
    ```
8. Sie können wahlweise auch andere Parameter in der Vorlage ändern. Diese sind abhängig von Ihren Anforderungen optional:

    * **SKU**: Sie können die SKU (Tarif) der öffentlichen IP-Adresse in der Konfiguration aus „standard“ in „basic“ oder aus „basic“ in „standard“ ändern, indem Sie die Eigenschaft **sku** > **name** in der Datei **\<resource-group-name>.json** ändern:

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

         Weitere Informationen zu den Unterschieden zwischen den SKUs „basic“ und „standard“ für öffentliche IP-Adressen finden Sie unter [Erstellen, Ändern oder Löschen einer öffentlichen IP-Adresse](https://docs.microsoft.com/azure/virtual-network/virtual-network-public-ip-address).

    * **Zuweisungsmethode für öffentliche IP** und **Leerlauftimeout**: Sie können beide Optionen in der Vorlage ändern, indem Sie die Eigenschaft **publicIPAllocationMethod** aus **Dynamic** in **Static** oder aus **Static** in **Dynamic** ändern. Das Leerlauftimeout kann geändert werden, indem Sie die Eigenschaft **idleTimeoutInMinutes** auf den gewünschten Wert festlegen.  Der Standardwert ist **4**:

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
         ```

        Weitere Informationen zu den Zuweisungsmethoden und den Werten für das Leerlauftimeout finden Sie unter [Erstellen, Ändern oder Löschen einer öffentlichen IP-Adresse](https://docs.microsoft.com/azure/virtual-network/virtual-network-public-ip-address).


9. Speichern Sie die Datei **\<resource-group-name>.json**.

10. Erstellen Sie mithilfe von [New-AzResourceGroup](https://docs.microsoft.com/powershell/module/az.resources/new-azresourcegroup?view=azps-2.6.0) eine Ressourcengruppe in der Zielregion für die öffentliche IP-Zieladresse, die bereitgestellt werden soll.
    
    ```azurepowershell-interactive
    New-AzResourceGroup -Name <target-resource-group-name> -location <target-region>
    ```
11. Stellen Sie die bearbeitete Datei **\<resource-group-name>.json** in der Ressourcengruppe bereit, die Sie im vorherigen Schritt mit [New-AzResourceGroupDeployment](https://docs.microsoft.com/powershell/module/az.resources/new-azresourcegroupdeployment?view=azps-2.6.0) erstellt haben:

    ```azurepowershell-interactive

    New-AzResourceGroupDeployment -ResourceGroupName <target-resource-group-name> -TemplateFile <source-resource-group-name>.json
    
    ```

12. Um zu überprüfen, ob die Ressourcen in der Zielregion erstellt wurden, verwenden Sie [Get-AzResourceGroup](https://docs.microsoft.com/powershell/module/az.resources/get-azresourcegroup?view=azps-2.6.0) und [Get-AzPublicIPAddress](https://docs.microsoft.com/powershell/module/az.network/get-azpublicipaddress?view=azps-2.6.0):
    
    ```azurepowershell-interactive

    Get-AzResourceGroup -Name <target-resource-group-name>

    ```

    ```azurepowershell-interactive

    Get-AzPublicIPAddress -Name <target-publicip-name> -ResourceGroupName <target-resource-group-name>

    ```
## <a name="discard"></a>Verwerfen 

Wenn Sie nach der Bereitstellung neu starten oder die öffentliche IP-Adresse im Ziel verwerfen möchten, löschen Sie die im Ziel erstellte Ressourcengruppe. Die verschobene öffentliche IP-Adresse wird dann gelöscht.  Um die Ressourcengruppe zu entfernen, verwenden Sie [Remove-AzResourceGroup](https://docs.microsoft.com/powershell/module/az.resources/remove-azresourcegroup?view=azps-2.6.0):

```azurepowershell-interactive

Remove-AzResourceGroup -Name <target-resource-group-name>

```

## <a name="clean-up"></a>Bereinigen

Um die Änderungen zu übernehmen und die Verschiebung des virtuellen Netzwerks abzuschließen, löschen Sie das virtuelle Quellnetzwerk oder die Quellressourcengruppe mit [Remove-AzResourceGroup](https://docs.microsoft.com/powershell/module/az.resources/remove-azresourcegroup?view=azps-2.6.0) oder [Remove-AzPublicIPAddress](https://docs.microsoft.com/powershell/module/az.network/remove-azpublicipaddress?view=azps-2.6.0):

```azurepowershell-interactive

Remove-AzResourceGroup -Name <source-resource-group-name>

```

``` azurepowershell-interactive

Remove-AzPublicIpAddress -Name <source-publicip-name> -ResourceGroupName <resource-group-name>

```

## <a name="next-steps"></a>Nächste Schritte

In diesem Tutorial haben Sie eine öffentliche Azure-IP-Adresse aus einer Region in eine andere verschoben und die Quellressourcen bereinigt.  Weitere Informationen zum Verschieben von Ressourcen zwischen Regionen und zur Notfallwiederherstellung in Azure finden Sie unter:


- [Verschieben von Ressourcen in eine neue Ressourcengruppe oder ein neues Abonnement](https://docs.microsoft.com/azure/azure-resource-manager/resource-group-move-resources)
- [Verschieben virtueller Azure-Computer in eine andere Region](https://docs.microsoft.com/azure/site-recovery/azure-to-azure-tutorial-migrate)
