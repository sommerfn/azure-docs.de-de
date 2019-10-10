---
title: 'Beispiel: Genehmigte VM-Images'
description: Diese Beispielrichtliniendefinition erzwingt, dass nur genehmigte benutzerdefinierte Images in Ihrer Umgebung bereitgestellt werden.
author: DCtheGeek
ms.service: azure-policy
ms.topic: sample
ms.date: 01/26/2019
ms.author: dacoulte
ms.openlocfilehash: 34a3cf54879dcf6fc25fab50bcbabde28fff653a
ms.sourcegitcommit: d7689ff43ef1395e61101b718501bab181aca1fa
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 10/06/2019
ms.locfileid: "71980847"
---
# <a name="sample---approved-virtual-machine-images"></a>Beispiel: Unterstützte Images virtueller Computer

Diese Richtlinie erfordert, dass nur genehmigte benutzerdefinierte Images in Ihrer Umgebung bereitgestellt werden. Sie geben ein Array mit genehmigten Image-IDs an.

Sie können für die Bereitstellung dieser Beispielrichtlinie Folgendes verwenden:

- Das [Azure-Portal](#azure-portal)
- [Azure PowerShell](#azure-powershell)
- [Azure-Befehlszeilenschnittstelle](#azure-cli)
- [REST-API](#rest-api)

[!INCLUDE [quickstarts-free-trial-note](../../../../includes/quickstarts-free-trial-note.md)]

## <a name="sample-policy"></a>Beispielrichtlinie

### <a name="policy-definition"></a>Richtliniendefinition

Die vollständige erstellte JSON-Richtliniendefinition, die von der REST-API, den Schaltflächen zum Bereitstellen in Azure und manuell im Portal verwendet wird:

[!code-json[full](../../../../policy-templates/samples/compute/allowed-custom-images/azurepolicy.json "Complete policy definition (JSON)")]

> [!NOTE]
> Verwenden Sie zum manuellen Erstellen einer Richtlinie im Portal die Abschnitte **properties.parameters** und **properties.policyRule** der obigen Definition. Umschließen Sie die beiden Abschnitte mit geschweiften Klammern (`{}`), damit sie gültigen JSON-Code darstellen.

### <a name="policy-rules"></a>Richtlinienregeln

Der JSON-Code, der die Regeln der Richtlinie definiert und von der Azure-Befehlszeilenschnittstelle und Azure PowerShell verwendet wird:

[!code-json[rule](../../../../policy-templates/samples/compute/allowed-custom-images/azurepolicy.rules.json "Policy rules (JSON)")]

### <a name="policy-parameters"></a>Richtlinienparameter

Der JSON-Code, der die Richtlinienparameter definiert und von der Azure-Befehlszeilenschnittstelle und Azure PowerShell verwendet wird:

[!code-json[parameters](../../../../policy-templates/samples/compute/allowed-custom-images/azurepolicy.parameters.json "Policy parameters (JSON)")]

## <a name="parameters"></a>Parameter

|NAME |type |Feld |BESCHREIBUNG |
|---|---|---|---|
|imageIds |Array |Microsoft.Compute/imageIds |Die Liste der genehmigten VM-Images|

Beim Erstellen einer Zuweisung über PowerShell oder die Azure CLI können die Parameterwerte mithilfe von `-PolicyParameter` (PowerShell) bzw. `--params` (Azure CLI) als JSON-Code in einer Zeichenfolge oder über eine Datei übergeben werden.
PowerShell unterstützt darüber hinaus das `-PolicyParameterObject`-Element. Dafür muss an das Cmdlet eine Name-Wert-Hashtabelle übergeben werden. Dabei steht **Name** für den Parameternamen und **Wert** für den einzelnen Wert bzw. das Array von Werten, der bzw. das während der Zuweisung übergeben wird.

In diesem Beispielparameter ist nur das Element _ContosoStdImage_ in der Ressourcengruppe _YourResourceGroup_ oder die Imageversion von Windows Server 2016 Datacenter aus dem Mai 2018 in der Region „USA, Mitte“ zulässig.

```json
{
    "imageIds": {
        "value": [
            "/subscriptions/<subscriptionId>/resourceGroups/YourResourceGroup/providers/Microsoft.Compute/images/ContosoStdImage",
            "/Subscriptions/<subscriptionId>/Providers/Microsoft.Compute/Locations/centralus/Publishers/MicrosoftWindowsServer/ArtifactTypes/VMImage/Offers/WindowsServer/Skus/2016-Datacenter/Versions/2016.127.20180510"
        ]
    }
}
```

## <a name="azure-portal"></a>Azure-Portal

[![Bereitstellen des Policy-Beispiels in Azure](../media/deploy/deploybutton.png)](https://portal.azure.com/?#blade/Microsoft_Azure_Policy/CreatePolicyDefinitionBlade/uri/https%3A%2F%2Fraw.githubusercontent.com%2FAzure%2Fazure-policy%2Fmaster%2Fsamples%2FCompute%2Fallowed-custom-images%2Fazurepolicy.json)
[![Bereitstellen des Policy-Beispiels in Azure Gov](../media/deploy/deployGovbutton.png)](https://portal.azure.us/?#blade/Microsoft_Azure_Policy/CreatePolicyDefinitionBlade/uri/https%3A%2F%2Fraw.githubusercontent.com%2FAzure%2Fazure-policy%2Fmaster%2Fsamples%2FCompute%2Fallowed-custom-images%2Fazurepolicy.json)

## <a name="azure-powershell"></a>Azure PowerShell

[!INCLUDE [sample-powershell-install](../../../../includes/sample-powershell-install-no-ssh-az.md)]

### <a name="deploy-with-azure-powershell"></a>Bereitstellen mit Azure PowerShell

```azurepowershell-interactive
# Create the Policy Definition (Subscription scope)
$definition = New-AzPolicyDefinition -Name 'allowed-custom-images' -DisplayName 'Approved VM images' -description 'This policy governs the approved VM images' -Policy 'https://raw.githubusercontent.com/Azure/azure-policy/master/samples/Compute/allowed-custom-images/azurepolicy.rules.json' -Parameter 'https://raw.githubusercontent.com/Azure/azure-policy/master/samples/Compute/allowed-custom-images/azurepolicy.parameters.json' -Mode All

# Set the scope to a resource group; may also be a resource, subscription, or management group
$scope = Get-AzResourceGroup -Name 'YourResourceGroup'

# Set the Policy Parameter (JSON format)
$policyparam = '{ "imageIds": { "value": [ "/subscriptions/<subscriptionId>/resourceGroups/YourResourceGroup/providers/Microsoft.Compute/images/ContosoStdImage", "/Subscriptions/<subscriptionId>/Providers/Microsoft.Compute/Locations/centralus/Publishers/MicrosoftWindowsServer/ArtifactTypes/VMImage/Offers/WindowsServer/Skus/2016-Datacenter/Versions/2016.127.20180510" ] } }'

# Create the Policy Assignment
$assignment = New-AzPolicyAssignment -Name 'allowed-custom-images-assignment' -DisplayName 'Approved VM images Assignment' -Scope $scope.ResourceId -PolicyDefinition $definition -PolicyParameter $policyparam
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
definition=$(az policy definition create --name 'allowed-custom-images' --display-name 'Approved VM images' --description 'This policy governs the approved VM images' --rules 'https://raw.githubusercontent.com/Azure/azure-policy/master/samples/Compute/allowed-custom-images/azurepolicy.rules.json' --params 'https://raw.githubusercontent.com/Azure/azure-policy/master/samples/Compute/allowed-custom-images/azurepolicy.parameters.json' --mode All)

# Set the scope to a resource group; may also be a resource, subscription, or management group
scope=$(az group show --name 'YourResourceGroup')

# Set the Policy Parameter (JSON format)
policyparam='{ "imageIds": { "value": [ "/subscriptions/<subscriptionId>/resourceGroups/YourResourceGroup/providers/Microsoft.Compute/images/ContosoStdImage", "/Subscriptions/<subscriptionId>/Providers/Microsoft.Compute/Locations/centralus/Publishers/MicrosoftWindowsServer/ArtifactTypes/VMImage/Offers/WindowsServer/Skus/2016-Datacenter/Versions/2016.127.20180510" ] } }'

# Create the Policy Assignment
assignment=$(az policy assignment create --name 'allowed-custom-images-assignment' --display-name 'Approved VM images Assignment' --scope `echo $scope | jq '.id' -r` --policy `echo $definition | jq '.name' -r` --params "$policyparam")
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

Es gibt mehrere Tools, die für die Interaktion mit der Resource Manager-REST-API verwendet werden können, etwa [ARMClient](https://github.com/projectkudu/ARMClient) oder PowerShell. Ein Beispiel zum Aufrufen der REST-API über PowerShell finden Sie unter [Struktur von Azure Policy-Definitionen](../concepts/definition-structure.md#aliases) im Abschnitt **Aliase**.

### <a name="deploy-with-rest-api"></a>Bereitstellen über die REST-API

- Erstellen Sie die Richtliniendefinition (Abonnementbereich). Verwenden Sie als Anforderungstext den JSON-Code der [Richtliniendefinition](#policy-definition).

  ```http
  PUT https://management.azure.com/subscriptions/{subscriptionId}/providers/Microsoft.Authorization/policyDefinitions/allowed-custom-images?api-version=2016-12-01
  ```

- Erstellen Sie die Richtlinienzuweisung (Ressourcengruppenbereich).

  ```http
  PUT https://management.azure.com/subscriptions/{subscriptionId}/resourceGroups/YourResourceGroup/providers/Microsoft.Authorization/policyAssignments/allowed-custom-images-assignment?api-version=2017-06-01-preview
  ```

  Verwenden Sie als Anforderungstext das folgende JSON-Beispiel:

  ```json
  {
      "properties": {
          "displayName": "Approved VM images Assignment",
          "policyDefinitionId": "/subscriptions/<subscriptionId>/providers/Microsoft.Authorization/policyDefinitions/allowed-custom-images",
          "parameters": {
              "imageIds": {
                  "value": [
                      "/subscriptions/<subscriptionId>/resourceGroups/YourResourceGroup/providers/Microsoft.Compute/images/ContosoStdImage",
                      "/Subscriptions/<subscriptionId>/Providers/Microsoft.Compute/Locations/centralus/Publishers/MicrosoftWindowsServer/ArtifactTypes/VMImage/Offers/WindowsServer/Skus/2016-Datacenter/Versions/2016.127.20180510"
                  ]
              }
          }
      }
  }
  ```

### <a name="remove-with-rest-api"></a>Entfernen mit der REST-API

- Entfernen der Richtlinienzuweisung

  ```http
  DELETE https://management.azure.com/subscriptions/{subscriptionId}/providers/Microsoft.Authorization/policyAssignments/allowed-custom-images-assignment?api-version=2017-06-01-preview
  ```

- Entfernen der Richtliniendefinition

  ```http
  DELETE https://management.azure.com/subscriptions/{subscriptionId}/providers/Microsoft.Authorization/policyDefinitions/allowed-custom-images?api-version=2016-12-01
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
- Weitere Azure Policy-Beispiele für Virtual Machines finden Sie unter [Anwenden von Richtlinien auf virtuelle Windows-Computer mit Azure Resource Manager](../../../virtual-machines/windows/policy.md).