---
title: Azure-Bereitstellungskontingent überschritten
description: Beschreibt die Behebung des Fehlers, dass im Verlauf der Ressourcengruppe mehr als 800 Bereitstellungen vorkommen.
author: tfitzmac
ms.service: azure-resource-manager
ms.topic: troubleshooting
ms.date: 10/02/2019
ms.author: tomfitz
ms.openlocfilehash: 755383c9d40c104d50ad9bb7a31b3a00f8348313
ms.sourcegitcommit: 7c2dba9bd9ef700b1ea4799260f0ad7ee919ff3b
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 10/02/2019
ms.locfileid: "71827015"
---
# <a name="resolve-error-when-deployment-count-exceeds-800"></a>Beheben des Fehlers, dass die Anzahl der Bereitstellungen 800 überschreitet

Jede Ressourcengruppe ist in ihrem Bereitstellungsverlauf auf 800 Bereitstellungen beschränkt. Dieser Artikel beschreibt den Fehler, den Sie erhalten, wenn bei einer Bereitstellung ein Fehler auftritt, da dadurch die zulässigen 800 Bereitstellungen überschritten würden. Um diesen Fehler zu beheben, löschen Sie Bereitstellungen aus dem Verlauf der Ressourcengruppe. Das Löschen einer Bereitstellung aus dem Verlauf hat keinerlei Auswirkungen auf die bereitgestellten Ressourcen.

## <a name="symptom"></a>Symptom

Während einer Bereitstellung erhalten Sie einen Fehler, dass die aktuelle Bereitstellung das Kontingent von 800 Bereitstellungen überschreitet.

## <a name="solution"></a>Lösung

### <a name="azure-cli"></a>Azure-Befehlszeilenschnittstelle

Verwenden Sie den Befehl [az group deployment delete](/cli/azure/group/deployment#az-group-deployment-delete), um Bereitstellungen aus dem Verlauf zu löschen.

```azurecli-interactive
az group deployment delete --resource-group exampleGroup --name deploymentName
```

Sie können die aktuelle Anzahl im Bereitstellungsverlauf mit dem folgenden Befehl abrufen:

```azurecli-interactive
az group deployment list --resource-group exampleGroup --query "length(@)"
```

### <a name="azure-powershell"></a>Azure PowerShell

Verwenden Sie den Befehl [Remove-AzResourceGroupDeployment](/powershell/module/az.resources/remove-azresourcegroupdeployment), um Bereitstellungen aus dem Verlauf zu löschen.

```azurepowershell-interactive
Remove-AzResourceGroupDeployment -ResourceGroupName exampleGroup -Name deploymentName
```

Sie können die aktuelle Anzahl im Bereitstellungsverlauf mit dem folgenden Befehl abrufen:

```azurepowershell-interactive
(Get-AzResourceGroupDeployment -ResourceGroupName exampleGroup).Count
```

## <a name="third-party-solutions"></a>Lösungen von Drittanbietern

Die folgenden externen Lösungen berücksichtigen spezifische Szenarien:

* [Azure Logic-Apps und PowerShell-Lösungen](https://devkimchi.com/2018/05/30/managing-excessive-arm-deployment-histories-with-logic-apps/)
* [AzDevOps-Erweiterung](https://github.com/christianwaha/AzureDevOpsExtensionCleanRG)
