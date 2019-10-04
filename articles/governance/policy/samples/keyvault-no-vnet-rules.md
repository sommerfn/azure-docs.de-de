---
title: 'Beispiel: Überwachen von Key Vault-Tresoren auf Instanzen ohne Endpunkte im virtuellen Netzwerk'
description: Diese Beispielrichtliniendefinition überwacht Key Vault-Tresore, um Instanzen VNET-Dienstendpunkte zu ermitteln.
author: DCtheGeek
manager: carmonm
ms.service: azure-policy
ms.topic: sample
ms.date: 01/26/2019
ms.author: dacoulte
ms.openlocfilehash: bc5ce4a6a2e52ed8d21de8db8da1f815293b61f7
ms.sourcegitcommit: c174d408a5522b58160e17a87d2b6ef4482a6694
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 04/18/2019
ms.locfileid: "59276370"
---
# <a name="sample---key-vault-vaults-with-no-virtual-network-endpoints"></a>Beispiel: Key Vault-Tresore mit Instanzen ohne Endpunkte im virtuellen Netzwerk

Diese Richtlinie ermittelt Key Vault-Tresore, die keine Endpunkte im virtuellen Netzwerk aufweisen. Verwenden Sie die Richtlinie, um Ihre Sicherheitsanforderungen zu erzwingen. Weitere Informationen finden Sie unter [VNET-Dienstendpunkte für Azure Key Vault](../../../key-vault/key-vault-overview-vnet-service-endpoints.md).

Sie können für die Bereitstellung dieser Beispielrichtlinie Folgendes verwenden:

- Das [Azure-Portal](#azure-portal)
- [Azure PowerShell](#azure-powershell)
- [Azure-Befehlszeilenschnittstelle](#azure-cli)
- [REST-API](#rest-api)

[!INCLUDE [quickstarts-free-trial-note](../../../../includes/quickstarts-free-trial-note.md)]

## <a name="sample-policy"></a>Beispielrichtlinie

### <a name="policy-definition"></a>Richtliniendefinition

Die vollständige erstellte JSON-Richtliniendefinition, die von der REST-API, den Schaltflächen zum Bereitstellen in Azure und manuell im Portal verwendet wird:

[!code-json[full](../../../../policy-templates/samples/KeyVault/audit-keyvault-vnet-rules/azurepolicy.json "KeyVault vnet rules")]

> [!NOTE]
> Verwenden Sie zum manuellen Erstellen einer Richtlinie im Portal die Abschnitte **properties.parameters** und **properties.policyRule** der obigen Definition. Umschließen Sie die beiden Abschnitte mit geschweiften Klammern (`{}`), damit sie gültigen JSON-Code darstellen.

### <a name="policy-rules"></a>Richtlinienregeln

Der JSON-Code, der die Regeln der Richtlinie definiert und von der Azure-Befehlszeilenschnittstelle und Azure PowerShell verwendet wird:

[!code-json[rule](../../../../policy-templates/samples/KeyVault/audit-keyvault-vnet-rules/azurepolicy.rules.json "Policy rules (JSON)")]

### <a name="policy-parameters"></a>Richtlinienparameter

Für diese Beispielrichtliniendefinition sind keine Parameter definiert.

## <a name="azure-portal"></a>Azure-Portal

[![Bereitstellen des Policy-Beispiels in Azure](https://azuredeploy.net/deploybutton.png)](https://portal.azure.com/#blade/Microsoft_Azure_Policy/CreatePolicyDefinitionBlade/uri/https%3A%2F%2Fraw.githubusercontent.com%2FAzure%2Fazure-policy%2Fmaster%2Fsamples%2FKeyVault%2Faudit-keyvault-vnet-rules%2Fazurepolicy.json)
[![Bereitstellen des Policy-Beispiels in Azure Gov](https://docs.microsoft.com/azure/governance/policy/media/deploy/deployGovbutton.png)](https://portal.azure.us/#blade/Microsoft_Azure_Policy/CreatePolicyDefinitionBlade/uri/https%3A%2F%2Fraw.githubusercontent.com%2FAzure%2Fazure-policy%2Fmaster%2Fsamples%2FKeyVault%2Faudit-keyvault-vnet-rules%2Fazurepolicy.json)

## <a name="azure-powershell"></a>Azure PowerShell

[!INCLUDE [sample-powershell-install](../../../../includes/sample-powershell-install-no-ssh-az.md)]

### <a name="deploy-with-azure-powershell"></a>Bereitstellen mit Azure PowerShell

```azurepowershell-interactive
# Create the Policy Definition (Subscription scope)
$definition = New-AzPolicyDefinition -Name "audit-keyvault-vnet-rules" -DisplayName "Audit if Key Vault has no virtual network rules" -description "Audits Key Vault vaults if they do not have virtual network service endpoints set up. More information on virtual network service endpoints in Key Vault is available here: https://docs.microsoft.com/azure/key-vault/key-vault-overview-vnet-service-endpoints" -Policy 'https://raw.githubusercontent.com/Azure/azure-policy/master/samples/KeyVault/audit-keyvault-vnet-rules/azurepolicy.rules.json' -Mode Indexed

# Set the scope to a resource group; may also be a subscription or management group
$scope = Get-AzResourceGroup -Name 'YourResourceGroup'

# Create the Policy Assignment
$assignment = New-AzPolicyAssignment -Name 'audit-keyvault-vnet-rules-assignment' -DisplayName 'Audit Key Vault Assignment' -Scope $scope.ResourceId -PolicyDefinition $definition
```

### <a name="remove-with-azure-powershell"></a>Entfernen mit Azure PowerShell

Führen Sie die folgenden Befehle aus, um die vorherige Zuweisung und Definition zu entfernen:

```azurepowershell-interactive
# Remove the Policy Assignment
Remove-AzPolicyAssignment -Id $assignment.ResourceId

# Remove the Policy Definition
Remove-AzPolicyDefinition -Id $definition.ResourceId
```

### <a name="azure-powershell-explanation"></a>Azure PowerShell: Erläuterung

In den Skripts zum Bereitstellen und Entfernen werden die folgenden Befehle verwendet. Jeder Befehl in der folgenden Tabelle ist mit der zugehörigen Dokumentation verknüpft:

| Get-Help | Notizen |
|---|---|
| [New-AzPolicyDefinition](/powershell/module/az.resources/New-Azpolicydefinition) | Erstellt eine neue Azure Policy-Definition. |
| [Get-AzResourceGroup](/powershell/module/az.resources/Get-Azresourcegroup) | Ruft eine einzelne Ressourcengruppe ab. |
| [New-AzPolicyAssignment](/powershell/module/az.resources/New-Azpolicyassignment) | Erstellt eine neue Azure Policy-Zuweisung. In diesem Beispiel wird eine Definition bereitgestellt, eine Initiative ist aber ebenfalls zulässig. |
| [Remove-AzPolicyAssignment](/powershell/module/az.resources/Remove-Azpolicyassignment) | Entfernt eine vorhandene Azure Policy-Zuweisung. |
| [Remove-AzPolicyDefinition](/powershell/module/az.resources/Remove-Azpolicydefinition) | Entfernt eine vorhandene Azure Policy-Definition. |

## <a name="azure-cli"></a>Azure-Befehlszeilenschnittstelle

[!INCLUDE [sample-cli-install](../../../../includes/sample-cli-install.md)]

### <a name="deploy-with-azure-cli"></a>Bereitstellen über die Azure-Befehlszeilenschnittstelle

```azurecli-interactive
# Create the Policy Definition (Subscription scope)
definition=$(az policy definition create --name 'audit-keyvault-vnet-rules' --display-name 'Audit if Key Vault has no virtual network rules' --description 'Audits Key Vault vaults if they do not have virtual network service endpoints set up. More information on virtual network service endpoints in Key Vault is available here: https://docs.microsoft.com/azure/key-vault/key-vault-overview-vnet-service-endpoints' --rules 'https://raw.githubusercontent.com/Azure/azure-policy/master/samples/KeyVault/audit-keyvault-vnet-rules/azurepolicy.rules.json' --mode Indexed)

# Set the scope to a resource group; may also be a subscription or management group
scope=$(az group show --name 'YourResourceGroup')

# Create the Policy Assignment
assignment=$(az policy assignment create --name 'audit-keyvault-vnet-rules-assignment' --display-name 'Audit Key Vault Assignment' --scope `echo $scope | jq '.id' -r` --policy `echo $definition | jq '.name' -r`)
```

### <a name="remove-with-azure-cli"></a>Entfernen über die Azure-Befehlszeilenschnittstelle

Führen Sie die folgenden Befehle aus, um die vorherige Zuweisung und Definition zu entfernen:

```azurecli-interactive
# Remove the Policy Assignment
az policy assignment delete --name `echo $assignment | jq '.name' -r`

# Remove the Policy Definition
az policy definition delete --name `echo $definition | jq '.name' -r`
```

### <a name="azure-cli-explanation"></a>Azure-Befehlszeilenschnittstelle: Erläuterung

| Get-Help | Notizen |
|---|---|
| [az policy definition create](/cli/azure/policy/definition?view=azure-cli-latest#az-policy-definition-create) | Erstellt eine neue Azure Policy-Definition. |
| [az group show](/cli/azure/group?view=azure-cli-latest#az-group-show) | Ruft eine einzelne Ressourcengruppe ab. |
| [az policy assignment create](/cli/azure/policy/assignment?view=azure-cli-latest#az-policy-assignment-create) | Erstellt eine neue Azure Policy-Zuweisung. In diesem Beispiel wird eine Definition bereitgestellt, eine Initiative ist aber ebenfalls zulässig. |
| [az policy assignment delete](/cli/azure/policy/assignment?view=azure-cli-latest#az-policy-assignment-delete) | Entfernt eine vorhandene Azure Policy-Zuweisung. |
| [az policy definition delete](/cli/azure/policy/definition?view=azure-cli-latest#az-policy-definition-delete) | Entfernt eine vorhandene Azure Policy-Definition. |

## <a name="rest-api"></a>REST-API

Es gibt mehrere Tools, die für die Interaktion mit der Resource Manager-REST-API verwendet werden können, etwa [ARMClient](https://github.com/projectkudu/ARMClient) oder PowerShell.

### <a name="deploy-with-rest-api"></a>Bereitstellen über die REST-API

- Erstellen Sie die Richtliniendefinition (Abonnementbereich). Verwenden Sie als Anforderungstext den JSON-Code der [Richtliniendefinition](#policy-definition).

  ```http
  PUT https://management.azure.com/subscriptions/{subscriptionId}/providers/Microsoft.Authorization/policyDefinitions/audit-keyvault-vnet-rules?api-version=2018-05-01
  ```

- Erstellen Sie die Richtlinienzuweisung (Ressourcengruppenbereich).

  ```http
  PUT https://management.azure.com/subscriptions/{subscriptionId}/resourceGroups/YourResourceGroup/providers/Microsoft.Authorization/policyAssignments/audit-keyvault-vnet-rules-assignment?api-version=2018-05-01
  ```

  Verwenden Sie als Anforderungstext das folgende JSON-Beispiel:

  ```json
  {
      "properties": {
          "displayName": "Audit Key Vault Assignment",
          "policyDefinitionId": "/subscriptions/{subscriptionId}/providers/Microsoft.Authorization/policyDefinitions/audit-keyvault-vnet-rules"
      }
  }
  ```

### <a name="remove-with-rest-api"></a>Entfernen mit der REST-API

- Entfernen der Richtlinienzuweisung

  ```http
  DELETE https://management.azure.com/subscriptions/{subscriptionId}/providers/Microsoft.Authorization/policyAssignments/audit-keyvault-vnet-rules-assignment?api-version=2018-05-01
  ```

- Entfernen der Richtliniendefinition

  ```http
  DELETE https://management.azure.com/subscriptions/{subscriptionId}/providers/Microsoft.Authorization/policyDefinitions/audit-keyvault-vnet-rules?api-version=2018-05-01
  ```

### <a name="rest-api-explanation"></a>REST-API: Erläuterung

| Dienst | Group | Vorgang | Notizen |
|---|---|---|---|
| Ressourcenverwaltung | Richtliniendefinitionen | [Erstellen](/rest/api/resources/policydefinitions/createorupdate) | Erstellt eine neue Azure Policy-Definition in einem Abonnement. Alternative: [Erstellen einer Verwaltungsgruppe](/rest/api/resources/policydefinitions/createorupdateatmanagementgroup) |
| Ressourcenverwaltung | Richtlinienzuweisungen | [Erstellen](/rest/api/resources/policyassignments/create) | Erstellt eine neue Azure Policy-Zuweisung. In diesem Beispiel wird eine Definition bereitgestellt, eine Initiative ist aber ebenfalls zulässig. |
| Ressourcenverwaltung | Richtlinienzuweisungen | [Löschen](/rest/api/resources/policyassignments/delete) | Entfernt eine vorhandene Azure Policy-Zuweisung. |
| Ressourcenverwaltung | Richtliniendefinitionen | [Löschen](/rest/api/resources/policydefinitions/delete) | Entfernt eine vorhandene Azure Policy-Definition. Alternative: [Löschen einer Verwaltungsgruppe](/rest/api/resources/policydefinitions/deleteatmanagementgroup) |

## <a name="next-steps"></a>Nächste Schritte

- Sehen Sie sich weitere [Azure Policy-Beispiele](index.md) an.
- Lesen Sie die Informationen unter [Struktur von Azure Policy-Definitionen](../concepts/definition-structure.md).