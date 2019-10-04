---
title: Ausgaben in einer Azure Resource Manager-Vorlage
description: Beschreibt, wie Ausgabewerte in einer Azure Resource Manager-Vorlage definiert werden.
author: tfitzmac
ms.service: azure-resource-manager
ms.topic: conceptual
ms.date: 09/05/2019
ms.author: tomfitz
ms.openlocfilehash: 0ff1e3cb71bd1bf5ee947eb5204839d48103628b
ms.sourcegitcommit: 7c2dba9bd9ef700b1ea4799260f0ad7ee919ff3b
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 10/02/2019
ms.locfileid: "71827943"
---
# <a name="outputs-in-azure-resource-manager-template"></a>Ausgaben in einer Azure Resource Manager-Vorlage

Dieser Artikel beschreibt, wie Sie Ausgabewerte in Ihrer Azure Resource Manager-Vorlage definieren. Sie verwenden Ausgaben, wenn Werte von den bereitgestellten Ressourcen zurückgegeben werden müssen.

## <a name="define-output-values"></a>Definieren von Ausgabewerten

Das folgende Beispiel zeigt die Vorgehensweise zum Zurückgeben der Ressourcen-ID für eine öffentliche IP-Adresse:

```json
"outputs": {
  "resourceID": {
    "type": "string",
    "value": "[resourceId('Microsoft.Network/publicIPAddresses', parameters('publicIPAddresses_name'))]"
  }
}
```

## <a name="conditional-output"></a>Bedingte Ausgabe

Im Ausgabeabschnitt können Sie einen Wert bedingt zurückgeben. In der Regel verwenden Sie eine Bedingung in den Ausgaben, wenn Sie eine Ressource [bedingt bereitgestellt](conditional-resource-deployment.md) haben. Das folgende Beispiel zeigt, wie die Ressourcen-ID für eine öffentliche IP-Adresse abhängig davon zurückgegeben wird, ob eine neue Ressourcen-ID bereitgestellt wurde:

```json
"outputs": {
  "resourceID": {
    "condition": "[equals(parameters('publicIpNewOrExisting'), 'new')]",
    "type": "string",
    "value": "[resourceId('Microsoft.Network/publicIPAddresses', parameters('publicIPAddresses_name'))]"
  }
}
```

Ein einfaches Beispiel der bedingten Ausgabe finden Sie unter der [Vorlage für die bedingte Ausgabe](https://github.com/bmoore-msft/AzureRM-Samples/blob/master/conditional-output/azuredeploy.json).

## <a name="linked-templates"></a>Verknüpfte Vorlagen

Mit der Funktion [reference](resource-group-template-functions-resource.md#reference) in der übergeordneten Vorlage können Sie den Ausgabewert aus einer verknüpften Vorlage abrufen. Die Syntax in der übergeordneten Vorlage lautet wie folgt:

```json
"[reference('<deploymentName>').outputs.<propertyName>.value]"
```

Wenn Sie eine Ausgabeeigenschaft von einer verknüpften Vorlage abrufen, kann der Name der Eigenschaft keinen Bindestrich enthalten.

Das folgende Beispiel zeigt, wie Sie die IP-Adresse auf einen Lastenausgleich festlegen, indem Sie einen Wert aus einer verknüpften Vorlage abrufen.

```json
"publicIPAddress": {
  "id": "[reference('linkedTemplate').outputs.resourceID.value]"
}
```

Sie können die Funktion `reference` nicht im Ausgabeabschnitt einer [geschachtelten Vorlage](resource-group-linked-templates.md#nested-template) verwenden. Um die Werte für eine bereitgestellte Ressource in einer geschachtelten Vorlage zurückzugeben, konvertieren Sie Ihre geschachtelte Vorlage in eine verknüpfte Vorlage.

## <a name="get-output-values"></a>Abrufen von Ausgabewerten

Bei erfolgreicher Bereitstellung werden die Ausgabewerte automatisch in den Ergebnissen der Bereitstellung zurückgegeben.

Sie können ein Skript verwenden, um Ausgabewerte aus dem Bereitstellungsverlauf abzurufen.

# <a name="powershelltabazure-powershell"></a>[PowerShell](#tab/azure-powershell)

```azurepowershell-interactive
(Get-AzResourceGroupDeployment `
  -ResourceGroupName <resource-group-name> `
  -Name <deployment-name>).Outputs.resourceID.value
```

# <a name="azure-clitabazure-cli"></a>[Azure-Befehlszeilenschnittstelle](#tab/azure-cli)

```azurecli-interactive
az group deployment show \
  -g <resource-group-name> \
  -n <deployment-name> \
  --query properties.outputs.resourceID.value
```

---

## <a name="example-templates"></a>Beispielvorlagen

In den folgenden Beispielen werden Szenarien für die Verwendung von Ausgaben veranschaulicht.

|Vorlage  |BESCHREIBUNG  |
|---------|---------|
|[Variablen kopieren](https://github.com/Azure/azure-docs-json-samples/blob/master/azure-resource-manager/multipleinstance/copyvariables.json) | Erstellt komplexe Variablen und gibt die entsprechenden Werte aus. Stellt keine Ressourcen bereit. |
|[Öffentliche IP-Adresse](https://github.com/Azure/azure-docs-json-samples/blob/master/azure-resource-manager/linkedtemplates/public-ip.json) | Erstellt eine öffentliche IP-Adresse und gibt die Ressourcen-ID aus. |
|[Load Balancer](https://github.com/Azure/azure-docs-json-samples/blob/master/azure-resource-manager/linkedtemplates/public-ip-parentloadbalancer.json) | Stellt eine Verknüpfung mit der vorherigen Vorlage her. Verwendet beim Erstellen des Lastenausgleichs die Ressourcen-ID in der Ausgabe. |

## <a name="next-steps"></a>Nächste Schritte

* Weitere Informationen zu den verfügbaren Eigenschaften für Ausgaben finden Sie unter [Verstehen der Struktur und Syntax von Azure Resource Manager-Vorlagen](resource-group-authoring-templates.md).
* Empfehlungen zum Erstellen von Ausgaben finden Sie unter [Bewährte Methoden: Ausgaben](template-best-practices.md#outputs).
