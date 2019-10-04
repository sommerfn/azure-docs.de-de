---
title: Erstellen einer Ressourcengruppe und von Ressourcen für das Abonnement – Azure Resource Manager-Vorlage
description: In diesem Artikel wird beschrieben, wie Sie eine Ressourcengruppe in einer Azure Resource Manager-Vorlage erstellen. Außerdem wird veranschaulicht, wie Sie Ressourcen für den Bereich des Azure-Abonnements bereitstellen.
author: tfitzmac
ms.service: azure-resource-manager
ms.topic: conceptual
ms.date: 09/06/2019
ms.author: tomfitz
ms.openlocfilehash: 37f2b04a62d94cce42b095540380460c38bc5b79
ms.sourcegitcommit: a4b5d31b113f520fcd43624dd57be677d10fc1c0
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 09/06/2019
ms.locfileid: "70772955"
---
# <a name="create-resource-groups-and-resources-at-the-subscription-level"></a>Erstellen von Ressourcengruppen und Ressourcen auf Abonnementebene

In der Regel stellen Sie Azure-Ressourcen für eine Ressourcengruppe in Ihrem Azure-Abonnement bereit. Sie können jedoch auch Azure-Ressourcengruppen erstellen und Azure-Ressourcen auf Abonnementebene erstellen. Um Vorlagen auf Abonnementebene bereitzustellen, verwenden Sie die Azure CLI und Azure PowerShell. Das Azure-Portal unterstützt keine Bereitstellung auf Abonnementebene.

Um eine Ressourcengruppe in einer Azure Resource Manager-Vorlage zu erstellen, definieren Sie eine Ressource [Microsoft.Resources/resourceGroups](/azure/templates/microsoft.resources/allversions) mit einem Namen und einem Speicherort für die Ressourcengruppe. Sie können eine Ressourcengruppe erstellen und Ressourcen für diese Ressourcengruppe in derselben Vorlage bereitstellen. Folgende Ressourcen können auf Abonnementebene bereitgestellt werden: [Richtlinien](../governance/policy/overview.md) und Ressourcen für die [rollenbasierte Zugriffssteuerung](../role-based-access-control/overview.md).

## <a name="deployment-considerations"></a>Überlegungen zur Bereitstellung

Die Bereitstellung auf Abonnementebene unterscheidet sich von der Ressourcengruppenbereitstellung in folgender Weise:

### <a name="schema-and-commands"></a>Schema und Befehle

Das Schema und die Befehle, die Sie für Bereitstellungen auf Abonnementebene verwenden, unterscheiden sich von Ressourcengruppenbereitstellungen. 

Verwenden Sie für das Schema `https://schema.management.azure.com/schemas/2018-05-01/subscriptionDeploymentTemplate.json#`.

Verwenden Sie als Azure CLI-Bereitstellungsbefehl [az deployment create](/cli/azure/deployment?view=azure-cli-latest#az-deployment-create). Beispielsweise wird mit dem folgenden CLI-Befehl eine Vorlage zum Erstellen einer Ressourcengruppe bereitgestellt:

```azurecli-interactive
az deployment create \
  --name demoDeployment \
  --location centralus \
  --template-uri https://raw.githubusercontent.com/Azure/azure-docs-json-samples/master/azure-resource-manager/emptyRG.json \
  --parameters rgName=demoResourceGroup rgLocation=centralus
```

Verwenden Sie als PowerShell-Bereitstellungsbefehl [New-AzDeployment](/powershell/module/az.resources/new-azdeployment). Mit dem folgenden PowerShell-Befehl wird beispielsweise eine Vorlage zum Erstellen einer Ressourcengruppe bereitgestellt:

```azurepowershell-interactive
New-AzDeployment `
  -Name demoDeployment `
  -Location centralus `
  -TemplateUri https://raw.githubusercontent.com/Azure/azure-docs-json-samples/master/azure-resource-manager/emptyRG.json `
  -rgName demoResourceGroup `
  -rgLocation centralus
```

### <a name="deployment-name-and-location"></a>Name und Speicherort der Bereitstellung

Bei einer Bereitstellung in Ihrem Abonnement müssen Sie einen Speicherort für die Bereitstellung angeben. Sie können auch einen Namen für die Bereitstellung angeben. Wenn Sie keinen Namen für die Bereitstellung angeben, wird der Name der Vorlage als Bereitstellungsname verwendet. Wenn Sie z.B. eine Vorlage mit dem Namen **azuredeploy.json** bereitstellen, wird **azuredeploy** als Standardname für die Bereitstellung erstellt.

Der Speicherort von Bereitstellungen auf Abonnementebene ist unveränderlich. Sie können keine Bereitstellung an einem Speicherort erstellen, wenn bereits eine Bereitstellung mit demselben Namen an einem anderen Speicherort vorhanden ist. Wenn Sie den Fehlercode `InvalidDeploymentLocation` erhalten, verwenden Sie entweder einen anderen Namen oder denselben Speicherort wie bei der vorherigen Bereitstellung für diesen Namen.

### <a name="use-template-functions"></a>Verwenden von Vorlagenfunktionen

Bei Bereitstellungen auf Abonnementebene müssen bei der Verwendung von Vorlagenfunktionen einige wichtige Aspekte berücksichtigt werden:

* Die Funktion [resourceGroup()](resource-group-template-functions-resource.md#resourcegroup) wird **nicht** unterstützt.
* Die Funktion [resourceId()](resource-group-template-functions-resource.md#resourceid) wird unterstützt. Verwenden sie diese Funktion zum Abrufen der Ressourcen-ID für Ressourcen, die in Bereitstellungen auf Abonnementebene verwendet werden. Rufen Sie beispielsweise die Ressourcen-ID für eine Richtliniendefinition mit `resourceId('Microsoft.Authorization/roleDefinitions/', parameters('roleDefinition'))` ab.
* Die Funktionen [reference()](resource-group-template-functions-resource.md#reference) und [list()](resource-group-template-functions-resource.md#list) werden unterstützt.

## <a name="create-resource-groups"></a>Erstellen von Ressourcengruppe

Mit der folgenden Vorlage wird eine leere Ressourcengruppe erstellt.

```json
{
    "$schema": "https://schema.management.azure.com/schemas/2018-05-01/subscriptionDeploymentTemplate.json#",
    "contentVersion": "1.0.0.1",
    "parameters": {
        "rgName": {
            "type": "string"
        },
        "rgLocation": {
            "type": "string"
        }
    },
    "variables": {},
    "resources": [
        {
            "type": "Microsoft.Resources/resourceGroups",
            "apiVersion": "2018-05-01",
            "location": "[parameters('rgLocation')]",
            "name": "[parameters('rgName')]",
            "properties": {}
        }
    ],
    "outputs": {}
}
```

Das Vorlagenschema finden Sie [hier](/azure/templates/microsoft.resources/allversions). Ähnliche Vorlagen sind auf [GitHub](https://github.com/Azure/azure-quickstart-templates/tree/master/subscription-level-deployments) verfügbar.

## <a name="create-multiple-resource-groups"></a>Erstellen mehrerer Ressourcengruppen

Verwenden Sie das [copy-Element](resource-group-create-multiple.md) mit Ressourcengruppen, um mehrere Ressourcengruppen erstellen zu können. 

```json
{
    "$schema": "https://schema.management.azure.com/schemas/2018-05-01/subscriptionDeploymentTemplate.json#",
    "contentVersion": "1.0.0.1",
    "parameters": {
        "rgNamePrefix": {
            "type": "string"
        },
        "rgLocation": {
            "type": "string"
        },
        "instanceCount": {
            "type": "int"
        }
    },
    "variables": {},
    "resources": [
        {
            "type": "Microsoft.Resources/resourceGroups",
            "apiVersion": "2018-05-01",
            "location": "[parameters('rgLocation')]",
            "name": "[concat(parameters('rgNamePrefix'), copyIndex())]",
            "copy": {
                "name": "rgCopy",
                "count": "[parameters('instanceCount')]"
            },
            "properties": {}
        }
    ],
    "outputs": {}
}
```

Weitere Informationen zur Ressourceniteration finden Sie unter [Bereitstellen mehrerer Instanzen einer Ressource oder Eigenschaft in Azure Resource Manager-Vorlagen](./resource-group-create-multiple.md) sowie unter [Tutorial: Erstellen mehrerer Ressourceninstanzen mit Resource Manager-Vorlagen](./resource-manager-tutorial-create-multiple-instances.md).

## <a name="create-resource-group-and-deploy-resources"></a>Erstellen einer Ressourcengruppe und Bereitstellen von Ressourcen

Verwenden Sie zum Erstellen der Ressourcengruppe und Bereitstellen von Ressourcen für diese eine geschachtelte Vorlage. Die geschachtelte Vorlage definiert die Ressourcen, die für die Ressourcengruppe bereitgestellt werden sollen. Legen Sie die geschachtelte Vorlage vor der Ressourcenbereitstellung als von der Ressourcengruppe abhängig fest, um sicherzustellen, dass die Ressourcengruppe vorhanden ist.

Das folgende Beispiel erstellt eine Ressourcengruppe und stellt ein Speicherkonto in der Ressourcengruppe bereit.

```json
{
    "$schema": "https://schema.management.azure.com/schemas/2018-05-01/subscriptionDeploymentTemplate.json#",
    "contentVersion": "1.0.0.1",
    "parameters": {
        "rgName": {
            "type": "string"
        },
        "rgLocation": {
            "type": "string"
        },
        "storagePrefix": {
            "type": "string",
            "maxLength": 11
        }
    },
    "variables": {
        "storageName": "[concat(parameters('storagePrefix'), uniqueString(subscription().id, parameters('rgName')))]"
    },
    "resources": [
        {
            "type": "Microsoft.Resources/resourceGroups",
            "apiVersion": "2018-05-01",
            "location": "[parameters('rgLocation')]",
            "name": "[parameters('rgName')]",
            "properties": {}
        },
        {
            "type": "Microsoft.Resources/deployments",
            "apiVersion": "2018-05-01",
            "name": "storageDeployment",
            "resourceGroup": "[parameters('rgName')]",
            "dependsOn": [
                "[resourceId('Microsoft.Resources/resourceGroups/', parameters('rgName'))]"
            ],
            "properties": {
                "mode": "Incremental",
                "template": {
                    "$schema": "https://schema.management.azure.com/schemas/2015-01-01/deploymentTemplate.json#",
                    "contentVersion": "1.0.0.0",
                    "parameters": {},
                    "variables": {},
                    "resources": [
                        {
                            "type": "Microsoft.Storage/storageAccounts",
                            "apiVersion": "2017-10-01",
                            "name": "[variables('storageName')]",
                            "location": "[parameters('rgLocation')]",
                            "kind": "StorageV2",
                            "sku": {
                                "name": "Standard_LRS"
                            }
                        }
                    ],
                    "outputs": {}
                }
            }
        }
    ],
    "outputs": {}
}
```

## <a name="create-policies"></a>Erstellen von Richtlinien

### <a name="assign-policy"></a>Zuweisen der Richtlinie

Im folgenden Beispiel wird dem Abonnement eine vorhandene Richtliniendefinition zugewiesen. Wenn die Richtlinie Parameter unterstützt, stellen Sie diese als Objekt bereit. Wenn die Richtlinie keine Parameter unterstützt, verwenden Sie das standardmäßige leere Objekt.

```json
{
    "$schema": "https://schema.management.azure.com/schemas/2018-05-01/subscriptionDeploymentTemplate.json#",
    "contentVersion": "1.0.0.0",
    "parameters": {
        "policyDefinitionID": {
            "type": "string"
        },
        "policyName": {
            "type": "string"
        },
        "policyParameters": {
            "type": "object",
            "defaultValue": {}
        }
    },
    "variables": {},
    "resources": [
        {
            "type": "Microsoft.Authorization/policyAssignments",
            "name": "[parameters('policyName')]",
            "apiVersion": "2018-03-01",
            "properties": {
                "scope": "[subscription().id]",
                "policyDefinitionId": "[parameters('policyDefinitionID')]",
                "parameters": "[parameters('policyParameters')]"
            }
        }
    ]
}
```

Stellen Sie diese Vorlage mit der Azure CLI wie folgt bereit:

```azurecli-interactive
# Built-in policy that accepts parameters
definition=$(az policy definition list --query "[?displayName=='Allowed locations'].id" --output tsv)

az deployment create \
  --name demoDeployment \
  --location centralus \
  --template-uri https://raw.githubusercontent.com/Azure/azure-docs-json-samples/master/azure-resource-manager/policyassign.json \
  --parameters policyDefinitionID=$definition policyName=setLocation policyParameters="{'listOfAllowedLocations': {'value': ['westus']} }"
```

Um diese Vorlage mit PowerShell bereitzustellen, verwenden Sie:

```azurepowershell-interactive
$definition = Get-AzPolicyDefinition | Where-Object { $_.Properties.DisplayName -eq 'Allowed locations' }

$locations = @("westus", "westus2")
$policyParams =@{listOfAllowedLocations = @{ value = $locations}}

New-AzDeployment `
  -Name policyassign `
  -Location centralus `
  -TemplateUri https://raw.githubusercontent.com/Azure/azure-docs-json-samples/master/azure-resource-manager/policyassign.json `
  -policyDefinitionID $definition.PolicyDefinitionId `
  -policyName setLocation `
  -policyParameters $policyParams
```

### <a name="define-and-assign-policy"></a>Definieren und Zuweisen einer Richtlinie

Sie können eine Richtlinie in derselben Vorlage [definieren](../governance/policy/concepts/definition-structure.md) und zuweisen.

```json
{
    "$schema": "https://schema.management.azure.com/schemas/2018-05-01/subscriptionDeploymentTemplate.json#",
    "contentVersion": "1.0.0.0",
    "parameters": {},
    "variables": {},
    "resources": [
        {
            "type": "Microsoft.Authorization/policyDefinitions",
            "name": "locationpolicy",
            "apiVersion": "2018-05-01",
            "properties": {
                "policyType": "Custom",
                "parameters": {},
                "policyRule": {
                    "if": {
                        "field": "location",
                        "equals": "northeurope"
                    },
                    "then": {
                        "effect": "deny"
                    }
                }
            }
        },
        {
            "type": "Microsoft.Authorization/policyAssignments",
            "name": "location-lock",
            "apiVersion": "2018-05-01",
            "dependsOn": [
                "locationpolicy"
            ],
            "properties": {
                "scope": "[subscription().id]",
                "policyDefinitionId": "[resourceId('Microsoft.Authorization/policyDefinitions', 'locationpolicy')]"
            }
        }
    ]
}
```

Mit dem folgenden CLI-Befehl können Sie die Richtliniendefinition im Abonnement erstellen und auf das Abonnement anwenden:

```azurecli
az deployment create \
  --name demoDeployment \
  --location centralus \
  --template-uri https://raw.githubusercontent.com/Azure/azure-docs-json-samples/master/azure-resource-manager/policydefineandassign.json
```

Um diese Vorlage mit PowerShell bereitzustellen, verwenden Sie:

```azurepowershell
New-AzDeployment `
  -Name definePolicy `
  -Location centralus `
  -TemplateUri https://raw.githubusercontent.com/Azure/azure-docs-json-samples/master/azure-resource-manager/policydefineandassign.json
```

## <a name="next-steps"></a>Nächste Schritte

* Informationen über das Zuweisen von Rollen finden Sie unter [Verwalten des Zugriffs auf Azure-Ressourcen mit RBAC und Azure Resource Manager-Vorlagen](../role-based-access-control/role-assignments-template.md).
* Unter [deployASCwithWorkspaceSettings.json](https://github.com/krnese/AzureDeploy/blob/master/ARM/deployments/deployASCwithWorkspaceSettings.json) finden Sie ein Beispiel für die Bereitstellung von Arbeitsbereichseinstellungen für Azure Security Center.
* Weitere Informationen zum Erstellen von Azure-Ressourcen-Manager-Vorlagen finden Sie unter [Erstellen von Vorlagen](resource-group-authoring-templates.md). 
* Eine Liste der verfügbaren Funktionen in einer Vorlage finden Sie unter [Funktionen von Azure Resource Manager-Vorlagen](resource-group-template-functions.md).
