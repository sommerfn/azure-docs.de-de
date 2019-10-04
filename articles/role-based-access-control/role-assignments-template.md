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
ms.date: 07/19/2019
ms.author: rolyon
ms.reviewer: bagovind
ms.openlocfilehash: e6511ff84c251577a5ff483f892387ab7d3d4d41
ms.sourcegitcommit: 4b647be06d677151eb9db7dccc2bd7a8379e5871
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 07/19/2019
ms.locfileid: "68360449"
---
# <a name="manage-access-to-azure-resources-using-rbac-and-azure-resource-manager-templates"></a>Verwalten des Zugriffs auf Azure-Ressourcen mit RBAC und Azure Resource Manager-Vorlagen

Der Zugriff auf Azure-Ressourcen wird mithilfe der [rollenbasierten Zugriffssteuerung (RBAC)](overview.md) verwaltet. Neben der Verwendung von Azure PowerShell oder der Azure CLI können Sie den Zugriff auf Azure-Ressourcen mit der RBAC und [Azure Resource Manager-Vorlagen](../azure-resource-manager/resource-group-authoring-templates.md) verwalten. Vorlagen können hilfreich sein, wenn Sie Ressourcen konsistent und wiederholt bereitstellen müssen. In diesem Artikel wird beschrieben, wie Sie den Zugriff mit der RBAC und Vorlagen verwalten können.

## <a name="assign-role-to-resource-group-or-subscription"></a>Zuweisen einer Rolle zu einer Ressourcengruppe oder einem Abonnement

In RBAC erstellen Sie zum Gewähren des Zugriffs eine Rollenzuweisung. Die folgende Vorlage veranschaulicht Folgendes:
- Zuweisen einer Rolle zu einem Benutzer, einer Gruppe oder einer Anwendung im Ressourcengruppen- oder Abonnementkontext
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

Das folgende Beispiel veranschaulicht die Zuweisung der Rolle „Leser“ zu einem Benutzer für eine Ressourcengruppe nach der Bereitstellung der Vorlage.

![Rollenzuweisung mithilfe einer Vorlage](./media/role-assignments-template/role-assignment-template.png)

Der Bereich der Rollenzuweisung wird von der Bereitstellungsebene bestimmt. In diesem Artikel werden sowohl die Bereitstellungsbefehle für die Ressourcengruppe als auch für die Abonnementebene aufgeführt.

## <a name="assign-role-to-resource"></a>Zuweisen einer Rolle zu einer Ressource

Wenn Sie eine Rollenzuweisung auf der Ebene einer Ressource erstellen müssen, ist das Format der Rollenzuweisung anders. Sie stellen den Namespace des Ressourcenanbieters und den Ressourcentyp der Ressource bereit, der die Rolle zugewiesen werden soll. Außerdem fügen Sie den Namen der Ressource in den Namen der Rollenzuweisung ein.

Verwenden Sie für den Typ und den Namen der Rollenzuweisung das folgende Format:

```json
"type": "{resource-provider-namespace}/{resource-type}/providers/roleAssignments",
"name": "{resource-name}/Microsoft.Authorization/{role-assign-GUID}"
```

Mit der folgenden Vorlage wird ein Speicherkonto bereitgestellt und diesem eine Rolle zugewiesen. Die Bereitstellung erfolgt mit den Ressourcengruppenbefehlen.

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
      "name": "[concat(variables('storageName'), '/Microsoft.Authorization/', parameters('roleNameGuid'))]",
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

Das folgende Beispiel veranschaulicht die Zuweisung der Rolle „Mitwirkender“ zu einem Benutzer für ein Speicherkonto nach dem Bereitstellen der Vorlage.

![Rollenzuweisung mithilfe einer Vorlage](./media/role-assignments-template/role-assignment-template-resource.png)

## <a name="deploy-template-using-azure-powershell"></a>Bereitstellen einer Vorlage mit Azure PowerShell

[!INCLUDE [az-powershell-update](../../includes/updated-for-az.md)]

Führen Sie die folgenden Schritte aus, um die vorherige Vorlage mithilfe von Azure PowerShell in einer Ressourcengruppe oder einem Abonnement bereitzustellen.

1. Erstellen Sie eine neue Datei namens „rbac-rg.json“, und kopieren Sie die obige Vorlage.

1. Melden Sie sich bei [Azure PowerShell](/powershell/azure/authenticate-azureps) an.

1. Rufen Sie den eindeutigen Bezeichner eines Benutzers, einer Gruppe oder einer Anwendung ab. Beispielsweise können Sie mit dem Befehl [Get-AzADUser](/powershell/module/az.resources/get-azaduser) Azure AD-Benutzer auflisten.

    ```azurepowershell
    $userid = (Get-AzADUser -DisplayName "{name}").id
    ```

1. Die Vorlage generiert einen Standardwert für die GUID, die zur Identifizierung der Rollenzuweisung verwendet wird. Wenn Sie eine bestimmte GUID angeben müssen, übergeben Sie diesen Wert für den roleNameGuid-Parameter. Das Format des Bezeichners ist: `11111111-1111-1111-1111-111111111111`

Führen Sie die folgenden Schritte aus, um die Rolle auf der Ebene einer Ressource oder Ressourcengruppe zuzuweisen:

1. Erstellen Sie eine Beispielressourcengruppe.

    ```azurepowershell
    New-AzResourceGroup -Name ExampleGroup -Location "Central US"
    ```

1. Verwenden Sie den Befehl [New-AzResourceGroupDeployment](/powershell/module/az.resources/new-azresourcegroupdeployment), um die Bereitstellung zu starten.

    ```azurepowershell
    New-AzResourceGroupDeployment -ResourceGroupName ExampleGroup -TemplateFile rbac-rg.json -principalId $userid -builtInRoleType Reader
    ```

    Nachfolgend sehen Sie ein Beispiel für die Ausgabe:

    ```Output
    PS /home/user> New-AzResourceGroupDeployment -ResourceGroupName ExampleGroup -TemplateFile rbac-rg.json -principalId $userid -builtInRoleType Reader
    
    DeploymentName          : rbac-rg
    ResourceGroupName       : ExampleGroup
    ProvisioningState       : Succeeded
    Timestamp               : 7/17/2018 7:46:32 PM
    Mode                    : Incremental
    TemplateLink            :
    Parameters              :
                              Name             Type                       Value
                              ===============  =========================  ==========
                              principalId      String                     22222222-2222-2222-2222-222222222222
                              builtInRoleType  String                     Reader
                              roleNameGuid     String                     11111111-1111-1111-1111-111111111111
    
    Outputs                 :
    DeploymentDebugLogLevel :
    ```

Verwenden Sie den Befehl [New-AzDeployment](/powershell/module/az.resources/new-azdeployment), und geben Sie einen Speicherort für die Bereitstellung an, um die Rolle auf der Ebene eines Abonnements zuzuweisen.

```azurepowershell
New-AzDeployment -Location centralus -TemplateFile rbac-rg.json -principalId $userid -builtInRoleType Reader
```

Sie hat eine ähnliche Ausgabe wie der Bereitstellungsbefehl für Ressourcengruppen.

## <a name="deploy-template-using-the-azure-cli"></a>Bereitstellen der Vorlage mithilfe der Azure CLI

Führen Sie die folgenden Schritte aus, um die vorherige Vorlage mithilfe der Azure CLI in einer Ressourcengruppe oder einem Abonnement bereitzustellen.

1. Erstellen Sie eine neue Datei namens „rbac-rg.json“, und kopieren Sie die obige Vorlage.

1. Melden Sie sich bei der [Azure CLI](/cli/azure/authenticate-azure-cli) an.

1. Rufen Sie den eindeutigen Bezeichner eines Benutzers, einer Gruppe oder einer Anwendung ab. Beispielsweise können Sie mit dem Befehl [az ad user show](/cli/azure/ad/user#az-ad-user-show) einen Azure AD-Benutzer anzeigen lassen.

    ```azurecli
    userid=$(az ad user show --upn-or-object-id "{email}" --query objectId --output tsv)
    ```

1. Die Vorlage generiert einen Standardwert für die GUID, die zur Identifizierung der Rollenzuweisung verwendet wird. Wenn Sie eine bestimmte GUID angeben müssen, übergeben Sie diesen Wert für den roleNameGuid-Parameter. Das Format des Bezeichners ist: `11111111-1111-1111-1111-111111111111`

Führen Sie die folgenden Schritte aus, um die Rolle auf der Ebene einer Ressource oder Ressourcengruppe zuzuweisen:

1. Erstellen Sie eine Beispielressourcengruppe.

    ```azurecli
    az group create --name ExampleGroup --location "Central US"
    ```

1. Starten Sie mit dem Befehl [az group deployment create](/cli/azure/group/deployment#az-group-deployment-create) die Bereitstellung.

    ```azurecli
    az group deployment create --resource-group ExampleGroup --template-file rbac-rg.json --parameters principalId=$userid builtInRoleType=Reader
    ```

    Nachfolgend sehen Sie ein Beispiel für die Ausgabe:

    ```Output
    C:\Azure\Templates>az group deployment create --resource-group ExampleGroup --template-file rbac-rg.json --parameters principalId=$userid builtInRoleType=Reader
    
    {
      "id": "/subscriptions/00000000-0000-0000-0000-000000000000/resourceGroups/ExampleGroup/providers/Microsoft.Resources/deployments/rbac-rg",
      "name": "rbac-rg",
      "properties": {
        "additionalProperties": {
          "duration": "PT9.5323924S",
          "outputResources": [
            {
              "id": "/subscriptions/00000000-0000-0000-0000-000000000000/resourceGroups/ExampleGroup/providers/Microsoft.Authorization/roleAssignments/11111111-1111-1111-1111-111111111111",
              "resourceGroup": "ExampleGroup"
            }
          ],
          "templateHash": "0000000000000000000"
        },
        "correlationId": "33333333-3333-3333-3333-333333333333",
        "debugSetting": null,
        "dependencies": [],
        "mode": "Incremental",
        "outputs": null,
        "parameters": {
          "builtInRoleType": {
            "type": "String",
            "value": "Reader"
          },
          "principalId": {
            "type": "String",
            "value": "22222222-2222-2222-2222-222222222222"
          },
          "roleNameGuid": {
            "type": "String",
            "value": "11111111-1111-1111-1111-111111111111"
          }
        },
        "parametersLink": null,
        "providers": [
          {
            "id": null,
            "namespace": "Microsoft.Authorization",
            "registrationState": null,
            "resourceTypes": [
              {
                "aliases": null,
                "apiVersions": null,
                "locations": [
                  null
                ],
                "properties": null,
                "resourceType": "roleAssignments"
              }
            ]
          }
        ],
        "provisioningState": "Succeeded",
        "template": null,
        "templateLink": null,
        "timestamp": "2018-07-17T19:00:31.830904+00:00"
      },
      "resourceGroup": "ExampleGroup"
    }
    ```

Verwenden Sie den Befehl [az deployment create](/cli/azure/deployment#az-deployment-create), und geben Sie einen Speicherort für die Bereitstellung an, um die Rolle auf der Ebene eines Abonnements zuzuweisen.

```azurecli
az deployment create --location centralus --template-file rbac-rg.json --parameters principalId=$userid builtInRoleType=Reader
```

Sie hat eine ähnliche Ausgabe wie der Bereitstellungsbefehl für Ressourcengruppen.

## <a name="next-steps"></a>Nächste Schritte

- [Schnellstart: Erstellen und Bereitstellen von Azure Resource Manager-Vorlagen über das Azure-Portal](../azure-resource-manager/resource-manager-quickstart-create-templates-use-the-portal.md)
- [Grundlegendes zur Struktur und Syntax von Azure Resource Manager-Vorlagen](../azure-resource-manager/resource-group-authoring-templates.md)
- [Azure-Schnellstartvorlagen](https://azure.microsoft.com/resources/templates/?term=rbac)
