---
title: Kennzeichnen von Azure-Ressourcen für die logische Organisation | Microsoft-Dokumentation
description: Zeigt, wie Sie Tags zum Organisieren von Azure-Ressourcen für die Abrechnung und Verwaltung anwenden können.
author: tfitzmac
ms.service: azure-resource-manager
ms.topic: conceptual
ms.date: 10/30/2019
ms.author: tomfitz
ms.openlocfilehash: e7763889ecf69231b7a4daf31e6899b33f3e2b36
ms.sourcegitcommit: fa5ce8924930f56bcac17f6c2a359c1a5b9660c9
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 10/31/2019
ms.locfileid: "73199155"
---
# <a name="use-tags-to-organize-your-azure-resources"></a>Verwenden von Tags zum Organisieren von Azure-Ressourcen

[!INCLUDE [resource-manager-governance-tags](../../includes/resource-manager-governance-tags.md)]

Um Tags auf Ressourcen anwenden zu können, muss der Benutzer Schreibzugriff auf den jeweiligen Ressourcentyp haben. Zum Anwenden von Tags auf alle Ressourcentypen verwenden Sie die Rolle [Mitwirkender](../role-based-access-control/built-in-roles.md#contributor). Zum Anwenden von Tags auf nur einen Ressourcentyp verwenden Sie die Rolle „Mitwirkender“ für die jeweilige Ressource. Zum Anwenden von Tags auf virtuelle Computer beispielsweise verwenden Sie [Mitwirkender von virtuellen Computern](../role-based-access-control/built-in-roles.md#virtual-machine-contributor).

[!INCLUDE [Handle personal data](../../includes/gdpr-intro-sentence.md)]

## <a name="policies"></a>Richtlinien

Sie können [Azure Policy](../governance/policy/overview.md) verwenden, um Taggingregeln und -konventionen zu erzwingen. Durch die Erstellung einer Richtlinie vermeiden Sie das Szenario, dass Ressourcen in Ihrem Abonnement bereitgestellt werden, die nicht den erwarteten Tags für Ihr Unternehmen entsprechen. Anstelle der manuellen Anwendung von Tags oder der Suche nach Ressourcen, die nicht konform sind, können Sie eine Richtlinie erstellen, die die erforderlichen Tags während der Bereitstellung automatisch anwendet. Tags können jetzt auch auf vorhandene Ressourcen mit dem neuen Effekt [Ändern](../governance/policy/concepts/effects.md#modify) und einer [Korrekturaufgabe](../governance/policy/how-to/remediate-resources.md) angewendet werden. Im folgenden Abschnitt werden Beispielrichtlinien für Tags dargestellt.

[!INCLUDE [Tag policies](../../includes/azure-policy-samples-general-tags.md)]

## <a name="powershell"></a>PowerShell

[!INCLUDE [updated-for-az](../../includes/updated-for-az.md)]

Verwenden Sie Folgendes, um die vorhandenen Tags für eine *Ressourcengruppe* anzuzeigen:

```azurepowershell-interactive
(Get-AzResourceGroup -Name examplegroup).Tags
```

Das Skript gibt das folgende Format zurück:

```powershell
Name                           Value
----                           -----
Dept                           IT
Environment                    Test
```

Verwenden Sie Folgendes, um die vorhandenen Tags für eine *Ressource mit einer angegebenen Ressourcen-ID* anzuzeigen:

```azurepowershell-interactive
(Get-AzResource -ResourceId /subscriptions/<subscription-id>/resourceGroups/<rg-name>/providers/Microsoft.Storage/storageAccounts/<storage-name>).Tags
```

Oder verwenden Sie Folgendes, um die vorhandenen Tags für eine *Ressource mit einem angegebenen Namen und einer Ressourcengruppe* anzuzeigen:

```azurepowershell-interactive
(Get-AzResource -ResourceName examplevnet -ResourceGroupName examplegroup).Tags
```

Verwenden Sie Folgendes, um *Ressourcengruppen mit einem bestimmten Tag* abzurufen:

```azurepowershell-interactive
(Get-AzResourceGroup -Tag @{ Dept="Finance" }).ResourceGroupName
```

Verwenden Sie Folgendes, um *Ressourcen mit einem bestimmten Tag* abzurufen:

```azurepowershell-interactive
(Get-AzResource -Tag @{ Dept="Finance"}).Name
```

Verwenden Sie Folgendes, um *Ressourcen mit einem bestimmten Tagnamen* abzurufen:

```azurepowershell-interactive
(Get-AzResource -TagName Dept).Name
```

Wenn Sie Tags auf eine Ressource oder Ressourcengruppe anwenden, werden die bereits vorhandenen Tags für diese Ressource oder Ressourcengruppe überschrieben. Daher müssen Sie Ihre Vorgehensweise darauf abstimmen, ob für die Ressource oder Ressourcengruppe bereits Tags vorhanden sind.

Verwenden Sie Folgendes, um einer *Ressourcengruppe ohne vorhandene Tags* Tags hinzuzufügen:

```azurepowershell-interactive
Set-AzResourceGroup -Name examplegroup -Tag @{ Dept="IT"; Environment="Test" }
```

Rufen Sie die vorhandenen Tags ab, fügen Sie das neue Tag hinzu, und wenden Sie die Tags wieder an, um einer *Ressourcengruppe mit vorhandenen Tags* Tags hinzuzufügen:

```azurepowershell-interactive
$tags = (Get-AzResourceGroup -Name examplegroup).Tags
$tags.Add("Status", "Approved")
Set-AzResourceGroup -Tag $tags -Name examplegroup
```

Verwenden Sie Folgendes, um einer *Ressource ohne vorhandene Tags* Tags hinzuzufügen:

```azurepowershell-interactive
$r = Get-AzResource -ResourceName examplevnet -ResourceGroupName examplegroup
Set-AzResource -Tag @{ Dept="IT"; Environment="Test" } -ResourceId $r.ResourceId -Force
```

Verwenden Sie Folgendes, um einer *Ressource mit vorhandenen Tags* Tags hinzuzufügen:

```azurepowershell-interactive
$r = Get-AzResource -ResourceName examplevnet -ResourceGroupName examplegroup
$r.Tags.Add("Status", "Approved")
Set-AzResource -Tag $r.Tags -ResourceId $r.ResourceId -Force
```

Verwenden Sie das folgende Skript, um alle Tags einer Ressourcengruppe auf die darin enthaltenen Ressourcen anzuwenden und *vorhandene Tags für die Ressourcen nicht beizubehalten*:

```azurepowershell-interactive
$groups = Get-AzResourceGroup
foreach ($g in $groups)
{
    Get-AzResource -ResourceGroupName $g.ResourceGroupName | ForEach-Object {Set-AzResource -ResourceId $_.ResourceId -Tag $g.Tags -Force }
}
```

Verwenden Sie das folgende Skript, um alle Tags einer Ressourcengruppe auf die darin enthaltenen Ressourcen anzuwenden und *vorhandene Tags für Ressourcen beizubehalten, die keine Duplikate sind*:

```azurepowershell-interactive
$group = Get-AzResourceGroup "examplegroup"
if ($null -ne $group.Tags) {
    $resources = Get-AzResource -ResourceGroupName $group.ResourceGroupName
    foreach ($r in $resources)
    {
        $resourcetags = (Get-AzResource -ResourceId $r.ResourceId).Tags
        if ($resourcetags)
        {
            foreach ($key in $group.Tags.Keys)
            {
                if (-not($resourcetags.ContainsKey($key)))
                {
                    $resourcetags.Add($key, $group.Tags[$key])
                }
            }
            Set-AzResource -Tag $resourcetags -ResourceId $r.ResourceId -Force
        }
        else
        {
            Set-AzResource -Tag $group.Tags -ResourceId $r.ResourceId -Force
        }
    }
}
```

Durch Übergeben einer leeren Hashtabelle können Sie alle Tags löschen:

```azurepowershell-interactive
Set-AzResourceGroup -Tag @{} -Name examplegroup
```

## <a name="azure-cli"></a>Azure-Befehlszeilenschnittstelle

Verwenden Sie Folgendes, um die vorhandenen Tags für eine *Ressourcengruppe* anzuzeigen:

```azurecli
az group show -n examplegroup --query tags
```

Das Skript gibt das folgende Format zurück:

```json
{
  "Dept"        : "IT",
  "Environment" : "Test"
}
```

Oder verwenden Sie Folgendes, um die vorhandenen Tags für eine *Ressource mit einem angegebenen Namen, Typen und einer Ressourcengruppe* anzuzeigen:

```azurecli
az resource show -n examplevnet -g examplegroup --resource-type "Microsoft.Network/virtualNetworks" --query tags
```

Beim Durchlaufen einer Ressourcensammlung können Sie die Ressourcen nach Ressourcen-ID anzeigen. Ein vollständiges Beispiel finden Sie weiter unten in diesem Artikel. Verwenden Sie Folgendes, um die vorhandenen Tags für eine *Ressource mit einer angegebenen Ressourcen-ID* anzuzeigen:

```azurecli
az resource show --id <resource-id> --query tags
```

Verwenden Sie `az group list`, um Ressourcengruppen mit einem bestimmten Tag abzurufen:

```azurecli
az group list --tag Dept=IT
```

Zum Abrufen aller Ressourcen mit einem bestimmten Tag und Wert verwenden Sie `az resource list`:

```azurecli
az resource list --tag Dept=Finance
```

Wenn Sie Tags auf eine Ressource oder Ressourcengruppe anwenden, werden die bereits vorhandenen Tags für diese Ressource oder Ressourcengruppe überschrieben. Daher müssen Sie Ihre Vorgehensweise darauf abstimmen, ob für die Ressource oder Ressourcengruppe bereits Tags vorhanden sind.

Verwenden Sie Folgendes, um einer *Ressourcengruppe ohne vorhandene Tags* Tags hinzuzufügen:

```azurecli
az group update -n examplegroup --set tags.Environment=Test tags.Dept=IT
```

Verwenden Sie Folgendes, um einer *Ressource ohne vorhandene Tags* Tags hinzuzufügen:

```azurecli
az resource tag --tags Dept=IT Environment=Test -g examplegroup -n examplevnet --resource-type "Microsoft.Network/virtualNetworks"
```

Zum Hinzufügen von Tags zu einer Ressource, die bereits Tags aufweist, rufen Sie die vorhandenen Tags ab, formatieren den Wert neu und wenden die vorhandenen und neuen Tags erneut an:

```azurecli
jsonrtag=$(az resource show -g examplegroup -n examplevnet --resource-type "Microsoft.Network/virtualNetworks" --query tags)
rt=$(echo $jsonrtag | tr -d '"{},' | sed 's/: /=/g')
az resource tag --tags $rt Project=Redesign -g examplegroup -n examplevnet --resource-type "Microsoft.Network/virtualNetworks"
```

Verwenden Sie das folgende Skript, um alle Tags einer Ressourcengruppe auf die darin enthaltenen Ressourcen anzuwenden und *vorhandene Tags für die Ressourcen nicht beizubehalten*:

```azurecli
groups=$(az group list --query [].name --output tsv)
for rg in $groups
do
  jsontag=$(az group show -n $rg --query tags)
  t=$(echo $jsontag | tr -d '"{},' | sed 's/: /=/g')
  r=$(az resource list -g $rg --query [].id --output tsv)
  for resid in $r
  do
    az resource tag --tags $t --id $resid
  done
done
```

Verwenden Sie das folgende Skript, um alle Tags einer Ressourcengruppe auf die darin enthaltenen Ressourcen anzuwenden und *vorhandene Tags für Ressourcen beizubehalten*:

```azurecli
groups=$(az group list --query [].name --output tsv)
for rg in $groups
do
  jsontag=$(az group show -n $rg --query tags)
  t=$(echo $jsontag | tr -d '"{},' | sed 's/: /=/g')
  r=$(az resource list -g $rg --query [].id --output tsv)
  for resid in $r
  do
    jsonrtag=$(az resource show --id $resid --query tags)
    rt=$(echo $jsonrtag | tr -d '"{},' | sed 's/: /=/g')
    az resource tag --tags $t$rt --id $resid
  done
done
```

## <a name="templates"></a>Vorlagen

Um eine Ressource während der Bereitstellung mit einem Tag zu versehen, fügen Sie der Ressource, die Sie bereitstellen, das `tags`-Element hinzu. Geben Sie den Namen und den Wert des Tags an.

### <a name="apply-a-literal-value-to-the-tag-name"></a>Anwenden eines literalen Werts auf den Tagnamen

Das folgende Beispiel zeigt ein Speicherkonto mit zwei Tags (`Dept` und `Environment`), die auf literale Werte festgelegt sind:

```json
{
    "$schema": "https://schema.management.azure.com/schemas/2015-01-01/deploymentTemplate.json#",
    "contentVersion": "1.0.0.0",
    "parameters": {
        "location": {
            "type": "string",
            "defaultValue": "[resourceGroup().location]"
        }
    },
    "resources": [
        {
            "apiVersion": "2019-04-01",
            "type": "Microsoft.Storage/storageAccounts",
            "name": "[concat('storage', uniqueString(resourceGroup().id))]",
            "location": "[parameters('location')]",
            "tags": {
                "Dept": "Finance",
                "Environment": "Production"
            },
            "sku": {
                "name": "Standard_LRS"
            },
            "kind": "Storage",
            "properties": {}
        }
    ]
}
```

Verwenden Sie zum Festlegen eines Tags für einen datetime-Wert die [utcNow-Funktion](resource-group-template-functions-string.md#utcnow).

### <a name="apply-an-object-to-the-tag-element"></a>Anwenden eines Objekts auf das Tagelement

Sie können einen Objektparameter definieren, der mehrere Tags speichert, und dieses Objekt auf das Tagelement anwenden. Jede Eigenschaft in dem Objekt wird zu einem separaten Tag für die Ressource. Das folgende Beispiel enthält einen Parameter namens `tagValues`, der auf das Tagelement angewendet wird.

```json
{
    "$schema": "https://schema.management.azure.com/schemas/2015-01-01/deploymentTemplate.json#",
    "contentVersion": "1.0.0.0",
    "parameters": {
        "location": {
            "type": "string",
            "defaultValue": "[resourceGroup().location]"
        },
        "tagValues": {
            "type": "object",
            "defaultValue": {
                "Dept": "Finance",
                "Environment": "Production"
            }
        }
    },
    "resources": [
        {
            "apiVersion": "2019-04-01",
            "type": "Microsoft.Storage/storageAccounts",
            "name": "[concat('storage', uniqueString(resourceGroup().id))]",
            "location": "[parameters('location')]",
            "tags": "[parameters('tagValues')]",
            "sku": {
                "name": "Standard_LRS"
            },
            "kind": "Storage",
            "properties": {}
        }
    ]
}
```

### <a name="apply-a-json-string-to-the-tag-name"></a>Anwenden einer JSON-Zeichenfolge auf den Tagnamen

Wenn Sie mehrere Werte in einem einzelnen Tag speichern möchten, wenden Sie eine JSON-Zeichenfolge an, die die gewünschten Werte darstellt. Die gesamte JSON-Zeichenfolge wird als einzelnes Tag gespeichert und darf maximal 256 Zeichen lang sein. Im folgenden Beispiel gibt es ein einzelnes Tag namens `CostCenter`, das mehrere Werte aus einer JSON-Zeichenfolge enthält:  

```json
{
    "$schema": "https://schema.management.azure.com/schemas/2015-01-01/deploymentTemplate.json#",
    "contentVersion": "1.0.0.0",
    "parameters": {
        "location": {
            "type": "string",
            "defaultValue": "[resourceGroup().location]"
        }
    },
    "resources": [
        {
            "apiVersion": "2019-04-01",
            "type": "Microsoft.Storage/storageAccounts",
            "name": "[concat('storage', uniqueString(resourceGroup().id))]",
            "location": "[parameters('location')]",
            "tags": {
                "CostCenter": "{\"Dept\":\"Finance\",\"Environment\":\"Production\"}"
            },
            "sku": {
                "name": "Standard_LRS"
            },
            "kind": "Storage",
            "properties": {}
        }
    ]
}
```

### <a name="apply-tags-from-resource-group"></a>Anwenden von Tags aus der Ressourcengruppe

Wenn Sie Tags aus einer Ressourcengruppe auf eine Ressource anwenden möchten, verwenden Sie die Funktion [resourceGroup](resource-group-template-functions-resource.md#resourcegroup). Wenn Sie den Tagwert abrufen, verwenden Sie die `tags.[tag-name]`-Syntax anstelle der `tags.tag-name`-Syntax, da einige Zeichen in der Punktnotation nicht ordnungsgemäß analysiert werden.

```json
{
    "$schema": "https://schema.management.azure.com/schemas/2015-01-01/deploymentTemplate.json#",
    "contentVersion": "1.0.0.0",
    "parameters": {
        "location": {
            "type": "string",
            "defaultValue": "[resourceGroup().location]"
        }
    },
    "resources": [
        {
            "apiVersion": "2019-04-01",
            "type": "Microsoft.Storage/storageAccounts",
            "name": "[concat('storage', uniqueString(resourceGroup().id))]",
            "location": "[parameters('location')]",
            "tags": {
                "Dept": "[resourceGroup().tags['Dept']]",
                "Environment": "[resourceGroup().tags['Environment']]"
            },
            "sku": {
                "name": "Standard_LRS"
            },
            "kind": "Storage",
            "properties": {}
        }
    ]
}
```

## <a name="portal"></a>Portal

[!INCLUDE [resource-manager-tag-resource](../../includes/resource-manager-tag-resources.md)]

## <a name="rest-api"></a>REST-API

Das Azure-Portal und PowerShell verwenden im Hintergrund die [Resource Manager-REST-API](https://docs.microsoft.com/rest/api/resources/). Wenn Sie das Tagging in eine andere Umgebung integrieren müssen, können Sie Tags mit **GET** für die Ressourcen-ID abrufen und die Tags mit einem **PATCH**-Aufruf aktualisieren.

## <a name="tags-and-billing"></a>Tags und Abrechnung

Abrechnungsdaten können mithilfe von Tags gruppiert werden. Wenn Sie beispielsweise mehrere virtuelle Computer für verschiedene Organisationen betreiben, können Sie die Nutzung mithilfe von Tags nach Kostenstelle organisieren. Mit Tags können Sie auch Kosten nach Runtimeumgebung kategorisieren, beispielsweise zur Abrechnung der Nutzung virtueller Computer in der Produktionsumgebung.

Informationen zu Tags können Sie über die [Azure-Ressourcennutzungs- und RateCard-APIs](../billing/billing-usage-rate-card-overview.md) oder aus der Nutzungsdatei im CSV-Format abrufen. Sie laden die Nutzungsdatei aus dem [Azure-Kontocenter](https://account.azure.com/Subscriptions) oder dem Azure-Portal herunter. Weitere Informationen finden Sie unter [Herunterladen oder Anzeigen Ihrer Azure-Rechnungen und täglichen Nutzungsdaten](../billing/billing-download-azure-invoice-daily-usage-date.md). Wählen Sie beim Herunterladen der Nutzungsdatei aus dem Azure-Kontocenter die Option **Version 2**. Für Dienste, die die Verwendung von Tags für die Abrechnung unterstützen, sind die Tags in der Spalte **Tags** enthalten.

Hinweise zu REST-API-Vorgängen finden Sie unter [Azure Billing REST API Reference (Preview)](/rest/api/billing/)(in englischer Sprache).

## <a name="next-steps"></a>Nächste Schritte

* Nicht alle Ressourcentypen unterstützen Tags. Um festzustellen, ob Sie ein Tag auf einen Ressourcentyp anwenden können, lesen Sie [Tagunterstützung für Azure-Ressourcen](tag-support.md).
* Eine Einführung zum Verwenden des Portals finden Sie unter [Verwenden des Azure-Portals zum Verwalten Ihrer Azure-Ressourcen](manage-resource-groups-portal.md).  
