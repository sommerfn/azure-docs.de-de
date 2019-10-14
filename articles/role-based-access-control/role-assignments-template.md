---
title: Verwalten des Zugriffs auf Azure-Ressourcen mit RBAC und Azure Resource Manager-Vorlagen | Microsoft-Dokumentation
description: Hier erfahren Sie, wie Sie den Zugriff auf Azure-Ressourcen für Benutzer, Gruppen und Anwendungen mit der rollenbasierten Zugriffssteuerung (Role-Based Access Control, RBAC) und Azure Resource Manager-Vorlagen verwalten.
services: active-directory
documentationcenter: ''
author: rolyon
manager: mtillman
ms.service: role-based-access-control
ms.devlang: na
ms.topic: conceptual
ms.tgt_pltfrm: na
ms.workload: identity
ms.date: 09/20/2019
ms.author: rolyon
ms.reviewer: bagovind
ms.openlocfilehash: b4eebf7dac4d388411f570b1546c96e3b82b2a98
ms.sourcegitcommit: 4f7dce56b6e3e3c901ce91115e0c8b7aab26fb72
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 10/04/2019
ms.locfileid: "71950058"
---
# <a name="manage-access-to-azure-resources-using-rbac-and-azure-resource-manager-templates"></a>Verwalten des Zugriffs auf Azure-Ressourcen mit RBAC und Azure Resource Manager-Vorlagen

Der Zugriff auf Azure-Ressourcen wird mithilfe der [rollenbasierten Zugriffssteuerung (RBAC)](overview.md) verwaltet. Neben der Verwendung von Azure PowerShell oder der Azure CLI können Sie den Zugriff auf Azure-Ressourcen mit [Azure Resource Manager-Vorlagen](../azure-resource-manager/resource-group-authoring-templates.md) verwalten. Vorlagen können hilfreich sein, wenn Sie Ressourcen konsistent und wiederholt bereitstellen müssen. In diesem Artikel wird beschrieben, wie Sie den Zugriff mit der RBAC und Vorlagen verwalten können.

## <a name="create-a-role-assignment-at-a-resource-group-scope-without-parameters"></a>Erstellen einer Rollenzuweisung in einem Ressourcengruppenbereich (ohne Parameter)

In RBAC erstellen Sie zum Gewähren des Zugriffs eine Rollenzuweisung. Die folgende Vorlage zeigt eine einfache Möglichkeit zum Erstellen einer Rollenzuweisung. Einige Werte werden in der Vorlage angegeben. Die folgende Vorlage veranschaulicht Folgendes:

-  Zuweisen der Rolle [Leser](built-in-roles.md#reader) zu einem Benutzer, einer Gruppe oder einer Anwendung in einem Ressourcengruppenbereich

Um die Vorlage zu verwenden, müssen Sie folgendermaßen vorgehen:

- Erstellen Sie eine neue JSON-Datei, und kopieren Sie die Vorlage.
- Ersetzen Sie `<your-principal-id>` durch den eindeutigen Bezeichner eines Benutzers, einer Gruppe oder einer Anwendung, dem bzw. der die Rolle zugewiesen wird. Das Format des Bezeichners ist: `11111111-1111-1111-1111-111111111111`

```json
{
    "$schema": "https://schema.management.azure.com/schemas/2015-01-01/deploymentTemplate.json#",
    "contentVersion": "1.0.0.0",
    "resources": [
        {
            "type": "Microsoft.Authorization/roleAssignments",
            "apiVersion": "2018-09-01-preview",
            "name": "[guid(resourceGroup().id)]",
            "properties": {
                "roleDefinitionId": "[concat('/subscriptions/', subscription().subscriptionId, '/providers/Microsoft.Authorization/roleDefinitions/', 'acdd72a7-3385-48ef-bd42-f606fba81ae7')]",
                "principalId": "<your-principal-id>"
            }
        }
    ]
}
```

Im Folgenden finden Sie Beispiele für die Befehle [New-AzResourceGroupDeployment](/powershell/module/az.resources/new-azresourcegroupdeployment) und [az group deployment create](/cli/azure/group/deployment#az-group-deployment-create) zum Starten der Bereitstellung in einer Ressourcengruppe mit dem Namen „ExampleGroup“.

```azurepowershell
New-AzResourceGroupDeployment -ResourceGroupName ExampleGroup -TemplateFile rbac-test.json
```

```azurecli
az group deployment create --resource-group ExampleGroup --template-file rbac-test.json
```

Das folgende Beispiel veranschaulicht die Zuweisung der Rolle „Leser“ zu einem Benutzer für eine Ressourcengruppe nach der Bereitstellung der Vorlage.

![Rollenzuweisung in einem Ressourcengruppenbereich](./media/role-assignments-template/role-assignment-template.png)

## <a name="create-a-role-assignment-at-a-resource-group-or-subscription-scope"></a>Erstellen einer Rollenzuweisung in einem Ressourcengruppen- oder Abonnementbereich

Die vorherige Vorlage ist nicht sehr flexibel. Die folgende Vorlage enthält Parameter, die in unterschiedlichen Bereichen verwendet werden können. Die folgende Vorlage veranschaulicht Folgendes:

- Zuweisen einer Rolle zu einem Benutzer, einer Gruppe oder einer Anwendung in einem Ressourcengruppen- oder Abonnementbereich
- Angeben der Rollen „Besitzer“, „Mitwirkender“ und „Leser“ als Parameter

Um die Vorlage zu verwenden, müssen Sie Folgendes eingeben:

- Den eindeutigen Bezeichner eines Benutzers, einer Gruppe oder einer Anwendung, dem bzw. der die Rolle zugewiesen wird
- Die zuzuweisende Rolle
- Einen eindeutigen Bezeichner, der für die Rollenzuweisung verwendet wird (oder alternativ den Standardbezeichner)

```json
{
    "$schema": "https://schema.management.azure.com/schemas/2015-01-01/deploymentTemplate.json#",
    "contentVersion": "1.0.0.0",
    "parameters": {
        "principalId": {
            "type": "string",
            "metadata": {
                "description": "The principal to assign the role to"
            }
        },
        "builtInRoleType": {
            "type": "string",
            "allowedValues": [
                "Owner",
                "Contributor",
                "Reader"
            ],
            "metadata": {
                "description": "Built-in role to assign"
            }
        },
        "roleNameGuid": {
            "type": "string",
            "defaultValue": "[newGuid()]",
            "metadata": {
                "description": "A new GUID used to identify the role assignment"
            }
        }
    },
    "variables": {
        "Owner": "[concat('/subscriptions/', subscription().subscriptionId, '/providers/Microsoft.Authorization/roleDefinitions/', '8e3af657-a8ff-443c-a75c-2fe8c4bcb635')]",
        "Contributor": "[concat('/subscriptions/', subscription().subscriptionId, '/providers/Microsoft.Authorization/roleDefinitions/', 'b24988ac-6180-42a0-ab88-20f7382dd24c')]",
        "Reader": "[concat('/subscriptions/', subscription().subscriptionId, '/providers/Microsoft.Authorization/roleDefinitions/', 'acdd72a7-3385-48ef-bd42-f606fba81ae7')]"
    },
    "resources": [
        {
            "type": "Microsoft.Authorization/roleAssignments",
            "apiVersion": "2018-09-01-preview",
            "name": "[parameters('roleNameGuid')]",
            "properties": {
                "roleDefinitionId": "[variables(parameters('builtInRoleType'))]",
                "principalId": "[parameters('principalId')]"
            }
        }
    ]
}
```

Um den eindeutigen Bezeichner eines Benutzers abzurufen, dem die Rolle zugewiesen werden soll, können Sie den Befehl [Get-AzADUser](/powershell/module/az.resources/get-azaduser) oder [az ad user show](/cli/azure/ad/user#az-ad-user-show) verwenden.

```azurepowershell
$userid = (Get-AzADUser -DisplayName "{name}").id
```

```azurecli
userid=$(az ad user show --upn-or-object-id "{email}" --query objectId --output tsv)
```

Der Bereich der Rollenzuweisung wird von der Bereitstellungsebene bestimmt. Im Folgenden finden Sie Beispiele für die Befehle [New-AzResourceGroupDeployment](/powershell/module/az.resources/new-azresourcegroupdeployment) und [az group deployment create](/cli/azure/group/deployment#az-group-deployment-create) zum Starten der Bereitstellung in einem Ressourcengruppenbereich.

```azurepowershell
New-AzResourceGroupDeployment -ResourceGroupName ExampleGroup -TemplateFile rbac-test.json -principalId $userid -builtInRoleType Reader
```

```azurecli
az group deployment create --resource-group ExampleGroup --template-file rbac-test.json --parameters principalId=$userid builtInRoleType=Reader
```

Im Folgenden finden Sie Beispiele für die Befehle [New-AzDeployment](/powershell/module/az.resources/new-azdeployment) und [az deployment create](/cli/azure/deployment#az-deployment-create) zum Starten der Bereitstellung in einem Abonnementbereich und zum Angeben des Standorts.

```azurepowershell
New-AzDeployment -Location centralus -TemplateFile rbac-test.json -principalId $userid -builtInRoleType Reader
```

```azurecli
az deployment create --location centralus --template-file rbac-test.json --parameters principalId=$userid builtInRoleType=Reader
```

> [!NOTE]
> Diese Vorlage ist nicht idempotent, es sei denn, derselbe `roleNameGuid`-Wert wird als Parameter bei jeder Bereitstellung der Vorlage bereitgestellt. Wenn keine `roleNameGuid` angegeben wird, wird standardmäßig für jede Bereitstellung eine neue GUID generiert, und bei nachfolgenden Bereitstellungen tritt ein `Conflict: RoleAssignmentExists`-Fehler auf.

## <a name="create-a-role-assignment-at-a-resource-scope"></a>Erstellen einer Rollenzuweisung in einem Ressourcenbereich

Wenn Sie eine Rollenzuweisung auf der Ebene einer Ressource erstellen müssen, ist das Format der Rollenzuweisung anders. Sie stellen den Namespace des Ressourcenanbieters und den Ressourcentyp der Ressource bereit, der die Rolle zugewiesen werden soll. Außerdem fügen Sie den Namen der Ressource in den Namen der Rollenzuweisung ein.

Verwenden Sie für den Typ und den Namen der Rollenzuweisung das folgende Format:

```json
"type": "{resource-provider-namespace}/{resource-type}/providers/roleAssignments",
"name": "{resource-name}/Microsoft.Authorization/{role-assign-GUID}"
```

Die folgende Vorlage veranschaulicht Folgendes:

- Erstellen eines neuen Speicherkontos
- Zuweisen einer Rolle zu einem Benutzer, einer Gruppe oder einer Anwendung im Speicherkontobereich
- Angeben der Rollen „Besitzer“, „Mitwirkender“ und „Leser“ als Parameter

Um die Vorlage zu verwenden, müssen Sie Folgendes eingeben:

- Den eindeutigen Bezeichner eines Benutzers, einer Gruppe oder einer Anwendung, dem bzw. der die Rolle zugewiesen wird
- Die zuzuweisende Rolle

```json
{
    "$schema": "https://schema.management.azure.com/schemas/2015-01-01/deploymentTemplate.json#",
    "contentVersion": "1.0.0.0",
    "parameters": {
        "principalId": {
            "type": "string",
            "metadata": {
                "description": "The principal to assign the role to"
            }
        },
        "builtInRoleType": {
            "type": "string",
            "allowedValues": [
                "Owner",
                "Contributor",
                "Reader"
            ],
            "metadata": {
                "description": "Built-in role to assign"
            }
        },
        "location": {
            "type": "string",
            "defaultValue": "[resourceGroup().location]"
        }
    },
    "variables": {
        "Owner": "[concat('/subscriptions/', subscription().subscriptionId, '/providers/Microsoft.Authorization/roleDefinitions/', '8e3af657-a8ff-443c-a75c-2fe8c4bcb635')]",
        "Contributor": "[concat('/subscriptions/', subscription().subscriptionId, '/providers/Microsoft.Authorization/roleDefinitions/', 'b24988ac-6180-42a0-ab88-20f7382dd24c')]",
        "Reader": "[concat('/subscriptions/', subscription().subscriptionId, '/providers/Microsoft.Authorization/roleDefinitions/', 'acdd72a7-3385-48ef-bd42-f606fba81ae7')]",
        "storageName": "[concat('storage', uniqueString(resourceGroup().id))]"
    },
    "resources": [
        {
            "apiVersion": "2019-04-01",
            "type": "Microsoft.Storage/storageAccounts",
            "name": "[variables('storageName')]",
            "location": "[parameters('location')]",
            "sku": {
                "name": "Standard_LRS"
            },
            "kind": "Storage",
            "properties": {}
        },
        {
            "type": "Microsoft.Storage/storageAccounts/providers/roleAssignments",
            "apiVersion": "2018-09-01-preview",
            "name": "[concat(variables('storageName'), '/Microsoft.Authorization/', guid(uniqueString(parameters('storageName'))))]",
            "dependsOn": [
                "[variables('storageName')]"
            ],
            "properties": {
                "roleDefinitionId": "[variables(parameters('builtInRoleType'))]",
                "principalId": "[parameters('principalId')]"
            }
        }
    ]
}
```

Zum Bereitstellen der vorherigen Vorlage verwenden Sie die Befehle für die Ressourcengruppe. Im Folgenden finden Sie Beispiele für die Befehle [New-AzResourceGroupDeployment](/powershell/module/az.resources/new-azresourcegroupdeployment) und [az group deployment create](/cli/azure/group/deployment#az-group-deployment-create) zum Starten der Bereitstellung in einem Ressourcenbereich.

```azurepowershell
New-AzResourceGroupDeployment -ResourceGroupName ExampleGroup -TemplateFile rbac-test.json -principalId $userid -builtInRoleType Contributor
```

```azurecli
az group deployment create --resource-group ExampleGroup --template-file rbac-test.json --parameters principalId=$userid builtInRoleType=Contributor
```

Das folgende Beispiel veranschaulicht die Zuweisung der Rolle „Mitwirkender“ zu einem Benutzer für ein Speicherkonto nach dem Bereitstellen der Vorlage.

![Rollenzuweisung im Ressourcenbereich](./media/role-assignments-template/role-assignment-template-resource.png)

## <a name="create-a-role-assignment-for-a-new-service-principal"></a>Erstellen einer Rollenzuweisung für einen neuen Dienstprinzipal

Wenn Sie einen neuen Dienstprinzipal erstellen und sofort versuchen, diesem eine Rolle zuzuweisen, kann die Rollenzuweisung in einigen Fällen fehlschlagen. Wenn Sie z. B. eine neue verwaltete Identität erstellen und dann versuchen, in derselben Azure Resource Manager-Vorlage dem Dienstprinzipal eine Rolle zuzuweisen, kann die Rollenzuweisung fehlschlagen. Der Grund für diesen Fehler ist wahrscheinlich eine Replikationsverzögerung. Der Dienstprinzipal wird in einer Region erstellt, die Rollenzuweisung kann aber in einer anderen Region stattfinden, in die der Dienstprinzipal noch nicht repliziert wurde. Um dieses Szenario zu beheben, sollten Sie beim Erstellen der Rollenzuweisung die `principalType`-Eigenschaft auf `ServicePrincipal` festlegen.

Die folgende Vorlage veranschaulicht Folgendes:

- Erstellen eines neuen Dienstprinzipals für verwaltete Identitäten
- Angeben des `principalType`
- Zuweisen der Rolle „Mitwirkender“ zu dem Dienstprinzipal in einem Ressourcengruppenbereich

Um die Vorlage zu verwenden, müssen Sie Folgendes eingeben:

- Den Basisnamen der verwalteten Identität, oder Sie können die Standardzeichenfolge verwenden.

```json
{
    "$schema": "http://schema.management.azure.com/schemas/2015-01-01/deploymentTemplate.json#",
    "contentVersion": "1.0.0.0",
    "parameters": {
        "baseName": {
            "type": "string",
            "defaultValue": "msi-test"
        }
    },
    "variables": {
        "identityName": "[concat(parameters('baseName'), '-bootstrap')]",
        "bootstrapRoleAssignmentId": "[guid(concat(resourceGroup().id, 'contributor'))]",
        "contributorRoleDefinitionId": "[concat('/subscriptions/', subscription().subscriptionId, '/providers/Microsoft.Authorization/roleDefinitions/', 'b24988ac-6180-42a0-ab88-20f7382dd24c')]"
    },
    "resources": [
        {
            "type": "Microsoft.ManagedIdentity/userAssignedIdentities",
            "name": "[variables('identityName')]",
            "apiVersion": "2018-11-30",
            "location": "[resourceGroup().location]"
        },
        {
            "type": "Microsoft.Authorization/roleAssignments",
            "apiVersion": "2018-09-01-preview",
            "name": "[variables('bootstrapRoleAssignmentId')]",
            "dependsOn": [
                "[resourceId('Microsoft.ManagedIdentity/userAssignedIdentities', variables('identityName'))]"
            ],
            "properties": {
                "roleDefinitionId": "[variables('contributorRoleDefinitionId')]",
                "principalId": "[reference(resourceId('Microsoft.ManagedIdentity/userAssignedIdentities', variables('identityName')), '2018-11-30').principalId]",
                "scope": "[resourceGroup().id]",
                "principalType": "ServicePrincipal"
            }
        }
    ]
}
```

Im Folgenden finden Sie Beispiele für die Befehle [New-AzResourceGroupDeployment](/powershell/module/az.resources/new-azresourcegroupdeployment) und [az group deployment create](/cli/azure/group/deployment#az-group-deployment-create) zum Starten der Bereitstellung in einem Ressourcengruppenbereich.

```azurepowershell
New-AzResourceGroupDeployment -ResourceGroupName ExampleGroup2 -TemplateFile rbac-test.json
```

```azurecli
az group deployment create --resource-group ExampleGroup2 --template-file rbac-test.json
```

Das folgende Beispiel veranschaulicht die Zuweisung der Rolle „Mitwirkender“ zu einem neuen Dienstprinzipal für verwaltete Identitäten nach dem Bereitstellen der Vorlage.

![Rollenzuweisung für einen neuen Dienstprinzipal für verwaltete Identitäten](./media/role-assignments-template/role-assignment-template-msi.png)

## <a name="next-steps"></a>Nächste Schritte

- [Schnellstart: Erstellen und Bereitstellen von Azure Resource Manager-Vorlagen über das Azure-Portal](../azure-resource-manager/resource-manager-quickstart-create-templates-use-the-portal.md)
- [Grundlegendes zur Struktur und Syntax von Azure Resource Manager-Vorlagen](../azure-resource-manager/resource-group-authoring-templates.md)
- [Erstellen von Ressourcengruppen und Ressourcen auf Abonnementebene](../azure-resource-manager/deploy-to-subscription.md)
- [Azure-Schnellstartvorlagen](https://azure.microsoft.com/resources/templates/?term=rbac)
