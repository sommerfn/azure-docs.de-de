---
title: 'Beispiel: Überwachung, wenn Anwendungen nicht auf virtuellen Linux-Computern installiert sind'
description: Die Beispielinitiative und -definitionen dieser Policy-Gastkonfiguration überwachen, ob die angegebenen Anwendungen nicht auf virtuellen Linux-Computern installiert sind.
author: DCtheGeek
manager: carmonm
ms.service: azure-policy
ms.topic: sample
ms.date: 05/02/2019
ms.author: dacoulte
ms.openlocfilehash: eda5a2a6d2dae58f8da72deccbb89a34c7f21dae
ms.sourcegitcommit: 0568c7aefd67185fd8e1400aed84c5af4f1597f9
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 05/06/2019
ms.locfileid: "65204014"
---
# <a name="sample---audit-if-specified-applications-arent-installed-inside-linux-vms"></a>Beispiel: Überwachung, wenn die angegebenen Anwendungen nicht auf virtuellen Linux-Computern installiert sind

Diese Initiative der Policy-Gastkonfiguration erstellt ein Überwachungsereignis, wenn die angegebenen Anwendungen nicht auf virtuellen Linux-Computern installiert sind. Die ID der dieser integrierten Initiative ist `/providers/Microsoft.Authorization/policySetDefinitions/c937dcb4-4398-4b39-8d63-4a6be432252e`.

> [!IMPORTANT]
> Alle Initiativen für Gastkonfigurationen bestehen aus den Richtliniendefinitionen **audit** und **deployIfNotExists**. Wenn nur eine der Richtliniendefinitionen zugewiesen wird, funktionieren Gastkonfigurationen nicht ordnungsgemäß.

Sie können dieses Beispiel wie folgt zuweisen:

- Das [Azure-Portal](#azure-portal)
- [Azure PowerShell](#azure-powershell)

[!INCLUDE [quickstarts-free-trial-note](../../../../includes/quickstarts-free-trial-note.md)]

## <a name="components-of-the-initiative"></a>Komponenten der Initiative

Diese [Gastkonfigurationsinitiative](../concepts/guest-configuration.md) umfasst die folgenden Richtlinien:

- [Überwachung](#audit-definition): Überwachung, wenn Anwendungen nicht auf virtuellen Linux-Computern installiert sind
  - ID: `/providers/Microsoft.Authorization/policyDefinitions/fee5cb2b-9d9b-410e-afe3-2902d90d0004`
- [deployIfNotExists](#deployIfNotExists-definition): Bereitstellen einer VM-Erweiterung zur Überwachung auf nicht in Linux-VMs installierte Anwendungen
  - ID: `/providers/Microsoft.Authorization/policyDefinitions/4d1c04de-2172-403f-901b-90608c35c721`

### <a name="initiative-definition"></a>Initiativdefinition

Die Initiative wird erstellt, indem Sie die Definitionen **audit** und **deployIfNotExists** mit dem [initiative-Parametern](#initiative-parameters) verknüpfen. Dies ist der JSON-Code der Definition.

[!code-json[initiative-definition](../../../../policy-templates/samples/GuestConfiguration/installed-application-linux/azurepolicyset.json "Initiative definition (JSON)")]

### <a name="initiative-parameters"></a>Initiativparameter

|NAME |Type |BESCHREIBUNG |
|---|---|---|
|applicationName |Zeichenfolge |Anwendungsnamen. Beispiel: „python“, „powershell“ oder eine durch Trennzeichen getrennte Liste, z. B. „python,powershell“. Verwenden Sie \* als Platzhalterzeichen, z.B. „power\*“. |

Beim Erstellen einer Zuweisung über PowerShell oder die Azure CLI können die Parameterwerte mithilfe von `-PolicyParameter` (PowerShell) bzw. `--params` (Azure CLI) als JSON-Code in einer Zeichenfolge oder über eine Datei übergeben werden.
PowerShell unterstützt darüber hinaus das `-PolicyParameterObject`-Element. Dafür muss an das Cmdlet eine Name-Wert-Hashtabelle übergeben werden. Dabei steht **Name** für den Parameternamen und **Wert** für den einzelnen Wert bzw. das Array von Werten, der bzw. das während der Zuweisung übergeben wird.

In diesem Beispielparameter wird die Installation der Anwendungen _Python_ und _PowerShell_ überwacht.

```json
{
    "applicationName": {
        "value": "python,powershell"
    }
}
```

Nur die Richtliniendefinition **deployIfNotExists** nutzt die Parameter der Initiative.

### <a name="audit-definition"></a>Definition von „audit“

Der folgende JSON-Code definiert die Regeln für die Richtliniendefinition **audit**.

[!code-json[audit-definition](../../../../policy-templates/samples/GuestConfiguration/installed-application-linux/audit/azurepolicy.rules.json "audit policy rules (JSON)")]

### <a name="deployifnotexists-definition"></a>Definition von „deployIfNotExists“

Der folgende JSON-Code definiert die Regeln für die Richtliniendefinition **deployIfNotExists**.

[!code-json[deployIfNotExists-definition](../../../../policy-templates/samples/GuestConfiguration/installed-application-linux/deployIfNotExists/azurepolicy.rules.json "deployIfNotExists policy rules (JSON)")]

Die Richtliniendefinition **deployIfNotExists** definiert die Azure-Images, für die die Richtlinie validiert wurde:

|Herausgeber |Angebot |SKU |
|-|-|-|
|OpenLogic |CentOS\* |Alle außer 6\* |
|RedHat |RHEL |Alle außer 6\* |
|RedHat |osa | Alle |
|credativ |Debian | Alle außer 7\* |
|SUSE |SLES\* |Alle außer 11\* |
|Canonical| UbuntuServer |Alle außer 12\* |
|microsoft-dsvm |linux-data-science-vm-ubuntu |Alle |
|microsoft-dsvm |azureml |Alle |
|cloudera |cloudera-centos-os |Alle außer 6\* |
|cloudera |cloudera-altus-centos-os |Alle |
|microsoft-ads |linux\* |Alle |
|microsoft-aks |Alle |Alle |
|AzureDatabricks |Alle |Alle |
|qubole-inc |Alle |Alle |
|datastax |Alle |Alle |
|couchbase |Alle |Alle |
|scalegrid |Alle |Alle |
|checkpoint |Alle |Alle |
|paloaltonetworks |Alle |Alle |

Der Abschnitt **deployment** der Regel übergibt den _installedApplication_-Parameter an den Gastkonfigurations-Agent auf dem virtuellen Computer. Mit dieser Konfiguration kann der Agent die Überprüfungen ausführen und die Compliance über die Richtliniendefinition **audit** zurückmelden.

## <a name="azure-portal"></a>Azure-Portal

Nachdem die Definitionen **audit** und **deployIfNotExists** im Portal erstellt wurden, wird empfohlen, diese für die Zuweisung in einer [Initiative](../concepts/definition-structure.md#initiatives) zusammenzufassen.

### <a name="create-copy-of-audit-definition"></a>Erstellen einer Kopie der Definition von „audit“

[![Bereitstellen des Policy-Beispiels in Azure](https://azuredeploy.net/deploybutton.png)](https://portal.azure.com/?#blade/Microsoft_Azure_Policy/CreatePolicyDefinitionBlade/uri/https%3A%2F%2Fraw.githubusercontent.com%2FAzure%2Fazure-policy%2Fmaster%2Fsamples%2FGuestConfiguration%2Finstalled-application-linux%2Faudit%2Fazurepolicy.json)
[![Bereitstellen des Policy-Beispiels in Azure Gov](https://docs.microsoft.com/azure/governance/policy/media/deploy/deployGovbutton.png)](https://portal.azure.us/?#blade/Microsoft_Azure_Policy/CreatePolicyDefinitionBlade/uri/https%3A%2F%2Fraw.githubusercontent.com%2FAzure%2Fazure-policy%2Fmaster%2Fsamples%2FGuestConfiguration%2Finstalled-application-linux%2Faudit%2Fazurepolicy.json)

Bei der Verwendung dieser Schaltflächen für die Bereitstellung über das Portal wird eine Kopie der Richtliniendefinition **audit** erstellt.
Ohne die gekoppelte Richtliniendefinition **deployIfNotExists** würde die Gastkonfiguration nicht ordnungsgemäß funktionieren.

### <a name="create-copy-of-deployifnotexists-definition"></a>Erstellen einer Kopie der Definition von „deployIfNotExists“

[![Bereitstellen des Policy-Beispiels in Azure](https://azuredeploy.net/deploybutton.png)](https://portal.azure.com/?#blade/Microsoft_Azure_Policy/CreatePolicyDefinitionBlade/uri/https%3A%2F%2Fraw.githubusercontent.com%2FAzure%2Fazure-policy%2Fmaster%2Fsamples%2FGuestConfiguration%2Finstalled-application-linux%2FdeployIfNotExists%2Fazurepolicy.json)
[![Bereitstellen des Policy-Beispiels in Azure Gov](https://docs.microsoft.com/azure/governance/policy/media/deploy/deployGovbutton.png)](https://portal.azure.us/?#blade/Microsoft_Azure_Policy/CreatePolicyDefinitionBlade/uri/https%3A%2F%2Fraw.githubusercontent.com%2FAzure%2Fazure-policy%2Fmaster%2Fsamples%2FGuestConfiguration%2Finstalled-application-linux%2FdeployIfNotExists%2Fazurepolicy.json)

Bei der Verwendung dieser Schaltflächen für die Bereitstellung über das Portal wird eine Kopie der Richtliniendefinition **deployIfNotExists** erstellt. Ohne die gekoppelte Richtliniendefinition **audit** würde die Gastkonfiguration nicht ordnungsgemäß funktionieren.

## <a name="azure-powershell"></a>Azure PowerShell

[!INCLUDE [sample-powershell-install](../../../../includes/sample-powershell-install-no-ssh-az.md)]

### <a name="deploy-with-azure-powershell"></a>Bereitstellen mit Azure PowerShell

#### <a name="copy-and-assign-the-initiative"></a>Kopieren und Zuweisen der Initiative

Mit den folgenden Schritten erstellen Sie eine Kopie der Initiative, die die integrierten Richtlinien für **audit** und **deployIfNotExists** enthält, und weisen die Initiative einer Ressourcengruppe zu.

```azurepowershell-interactive
# Create the policy initiative (Subscription scope)
$initDef = New-AzPolicySetDefinition -Name 'guestconfig-installed-application-linux' -DisplayName 'GuestConfig - Audit that an application is installed inside Linux VMs' -description 'This initiative will both deploy the policy requirements and audit that the specified application is installed inside Linux virtual machines.' -Policy 'https://raw.githubusercontent.com/Azure/azure-policy/master/samples/GuestConfiguration/installed-application-linux/azurepolicyset.definitions.json' -Parameter 'https://raw.githubusercontent.com/Azure/azure-policy/master/samples/GuestConfiguration/installed-application-linux/azurepolicyset.parameters.json' -Mode All

# Set the scope to a resource group; may also be a resource, subscription, or management group
$scope = Get-AzResourceGroup -Name 'YourResourceGroup'

# Set the initiative parameter (JSON format)
$initParam = '{ "applicationName": { "value": "python,powershell" } }'

# Create the initiative assignment
$assignment = New-AzPolicyAssignment -Name 'guestconfig-installed-application-linux-assignment' -DisplayName 'GuestConfig - Python and PowerShell apps on Linux' -Scope $scope.ResourceID -PolicySetDefinition $initDef -PolicyParameter $initParam -AssignIdentity -Location 'westus2'

# Get the system-assigned managed identity created by the assignment with -AssignIdentity
$saIdentity = $assignment.Identity.principalId

# Give the system-assigned managed identity the 'Contributor' role on the scope (needed by deployIfNotExists)
$roleAssignment = New-AzRoleAssignment -ObjectId $saIdentity -Scope $scope.ResourceId -RoleDefinitionName 'Contributor'
```

Führen Sie die folgenden Befehle aus, um die vorherige Zuweisung und Definition zu entfernen:

```azurepowershell-interactive
# Remove the initiative assignment
Remove-AzPolicyAssignment -Id $assignment.ResourceId

# Remove the 'Contributor' role from the system-assigned managed identity
Remove-AzRoleAssignment -ObjectId $saIdentity -Scope $scope.ResourceId -RoleDefinitionName 'Contributor'

# Remove the initiative definition
Remove-AzPolicySetDefinition -Id $initDef
```

#### <a name="copy-and-assign-the-audit-definition"></a>Kopieren und Zuweisen der Definition von „audit“

Mit diesen Schritten erstellen Sie eine Kopie der Definition **audit** und weisen sie einer Ressourcengruppe zu. Diese Definition funktioniert nur dann ordnungsgemäß, wenn sie zusammen mit der gekoppelten Definition **deployIfNotExists** zugewiesen wird.

```azurepowershell-interactive
# Create the policy definition (Subscription scope)
$definition = New-AzPolicyDefinition -Name 'guestconfig-installed-application-linux-audit' -DisplayName 'GuestConfig - Audit that an application is installed inside Linux VMs' -description 'This policy audits that the specified application is installed inside Linux virtual machines. This policy should only be used along with its corresponding deploy policy in an initiative/policy set.' -Policy 'https://raw.githubusercontent.com/Azure/azure-policy/master/samples/GuestConfiguration/installed-application-linux/audit/azurepolicy.rules.json' -Mode All

# Set the scope to a resource group; may also be a resource, subscription, or management group
$scope = Get-AzResourceGroup -Name 'YourResourceGroup'

# Create the policy assignment
$assignment = New-AzPolicyAssignment -Name 'guestconfig-installed-application-linux-audit-assignment' -DisplayName 'GuestConfig - Python and PowerShell apps on Linux' -Scope $scope.ResourceID -PolicyDefinition $definition
```

Führen Sie die folgenden Befehle aus, um die vorherige Zuweisung und Definition zu entfernen:

```azurepowershell-interactive
# Remove the policy definition
Remove-AzPolicyAssignment -Id $assignment.ResourceId

# Remove the policy definition
Remove-AzPolicyDefinition -Id $definition
```

#### <a name="copy-and-assign-the-deployifnotexists-definition"></a>Kopieren und Zuweisen der Definition „deployIfNotExists“

Mit diesen Schritten erstellen Sie eine Kopie der Definition **deployIfNotExists** und weisen sie einer Ressourcengruppe zu.
Diese Definition funktioniert nur dann ordnungsgemäß, wenn sie zusammen mit der gekoppelten Definition **audit** zugewiesen wird.

```azurepowershell-interactive
# Create the policy definition (Subscription scope)
$definition = New-AzPolicyDefinition -Name 'guestconfig-installed-application-linux-deployIfNotExists' -DisplayName 'GuestConfig - Deploy VM extension to audit that an application is installed inside Linux VMs' -description 'Include this rule to deploy the VM extension for Microsoft Guest Configuration, the VM extension for Microsoft Azure Managed Service Identity, and the content required to audit that an application is installed inside Linux virtual machines. This policy should only be used along with its corresponding audit policy in an initiative/policy set.' -Policy 'https://raw.githubusercontent.com/Azure/azure-policy/master/samples/GuestConfiguration/installed-application-linux/deployIfNotExists/azurepolicy.rules.json' -Parameter 'https://raw.githubusercontent.com/Azure/azure-policy/master/samples/GuestConfiguration/installed-application-linux/deployIfNotExists/azurepolicy.parameters.json' -Mode All

# Set the scope to a resource group; may also be a resource, subscription, or management group
$scope = Get-AzResourceGroup -Name 'YourResourceGroup'

# Set the definition parameter (JSON format)
$policyParam  = '{ "applicationName": { "value": "python,powershell" } }'

# Create the policy assignment
$assignment = New-AzPolicyAssignment -Name 'guestconfig-installed-application-linux-deployIfNotExists-assignment' -DisplayName 'GuestConfig - Deploy VM extension to audit that Python and PowerShell are installed inside Linux VMs' -Scope $scope.ResourceID -PolicyDefinition $definition -PolicyParameter $policyParam -AssignIdentity -Location 'westus2'

# Get the system-assigned managed identity created by the assignment with -AssignIdentity
$saIdentity = $assignment.Identity.principalId

# Give the system-assigned managed identity the 'Contributor' role on the scope (needed by deployIfNotExists)
$roleAssignment = New-AzRoleAssignment -ObjectId $saIdentity -Scope $scope.ResourceId -RoleDefinitionName 'Contributor'
```

Führen Sie die folgenden Befehle aus, um die vorherige Zuweisung und Definition zu entfernen:

```azurepowershell-interactive
# Remove the policy assignment
Remove-AzPolicyAssignment -Id $assignment.ResourceId

# Remove the 'Contributor' role from the system-assigned managed identity
Remove-AzRoleAssignment -ObjectId $saIdentity -Scope $scope.ResourceId -RoleDefinitionName 'Contributor'

# Remove the policy definition
Remove-AzPolicyDefinition -Id $definition
```

### <a name="azure-powershell-explanation"></a>Azure PowerShell: Erläuterung

In den Skripts zum Bereitstellen und Entfernen werden die folgenden Befehle verwendet. Jeder Befehl in der folgenden Tabelle ist mit der zugehörigen Dokumentation verknüpft:

| Get-Help | Notizen |
|---|---|
| [New-AzPolicySetDefinition](/powershell/module/az.resources/New-AzPolicySetDefinition) | Erstellt eine Azure Policy-Initiative. |
| [New-AzPolicyDefinition](/powershell/module/az.resources/New-AzPolicyDefinition) | Erstellt eine Azure Policy-Definition. |
| [Get-AzResourceGroup](/powershell/module/az.resources/Get-AzResourceGroup) | Ruft eine einzelne Ressourcengruppe ab. |
| [New-AzPolicyAssignment](/powershell/module/az.resources/New-AzPolicyAssignment) | Erstellt eine neue Azure Policy-Zuweisung für eine Initiative oder Definition. |
| [New-AzRoleAssignment](/powershell/module/az.resources/New-AzRoleAssignment) | Gibt eine vorhandene Rollenzuweisung zu einem bestimmten Prinzipal zurück. |
| [Remove-AzPolicyAssignment](/powershell/module/az.resources/Remove-AzPolicyAssignment) | Entfernt eine vorhandene Azure Policy-Zuweisung. |
| [Remove-AzPolicySetDefinition](/powershell/module/az.resources/Remove-AzPolicySetDefinition) | Entfernt eine Initiative. |
| [Remove-AzPolicyDefinition](/powershell/module/az.resources/Remove-AzPolicyDefinition) | Entfernt eine Definition. |

## <a name="next-steps"></a>Nächste Schritte

- Sehen Sie sich weitere [Azure Policy-Beispiele](index.md) an.
- Erfahren Sie mehr über [Azure Policy-Gastkonfigurationen](../concepts/guest-configuration.md).
- Lesen Sie die Informationen unter [Struktur von Azure Policy-Definitionen](../concepts/definition-structure.md).
