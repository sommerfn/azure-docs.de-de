---
title: Rollback bei Fehler zu erfolgreicher Azure-Bereitstellung
description: Geben Sie an, dass für eine fehlerhafte Bereitstellung ein Rollback zu einer erfolgreichen Bereitstellung erfolgen soll.
author: tfitzmac
ms.service: azure-resource-manager
ms.topic: conceptual
ms.date: 10/04/2019
ms.author: tomfitz
ms.openlocfilehash: 035b684bb9f5fbc10eb13e642c3fd5945b85c561
ms.sourcegitcommit: c2e7595a2966e84dc10afb9a22b74400c4b500ed
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 10/05/2019
ms.locfileid: "71975377"
---
# <a name="rollback-on-error-to-successful-deployment"></a>Rollback bei Fehler zu erfolgreicher Bereitstellung

Wenn eine Bereitstellung fehlschlägt, können Sie automatisch eine frühere, erfolgreiche Bereitstellung aus Ihrem Bereitstellungsverlauf bereitstellen. Diese Funktionalität ist nützlich, wenn es einen bekannten guten Zustand für die Infrastrukturbereitstellung gibt, der wiederhergestellt werden soll. Es gibt eine Reihe von Vorbehalten und Einschränkungen:

- Die Bereitstellung wird genauso wie zuvor mit denselben Parametern ausgeführt. Sie können die Parameter nicht ändern.
- Die vorherige Bereitstellung wird im [vollständigen Modus](./deployment-modes.md#complete-mode) ausgeführt. Alle in der vorherigen Bereitstellung nicht enthaltenen Ressourcen werden gelöscht, und alle Ressourcenkonfigurationen werden auf ihren vorherigen Zustand zurückgesetzt. Sorgen Sie dafür, dass Sie die [Bereitstellungsmodi](./deployment-modes.md) vollständig verstehen.
- Die erneute Bereitstellung wirkt sich nur auf die Ressourcen aus. Datenänderungen sind davon nicht betroffen.
- Sie können diese Funktion nur in Verbindung mit Ressourcengruppenbereitstellungen verwenden, nicht in Verbindung mit Bereitstellungen auf Abonnement- oder Verwaltungsgruppenebene. Weitere Informationen zu Bereitstellungen auf Abonnementebene finden Sie unter [Erstellen von Ressourcengruppen und Ressourcen auf Abonnementebene](./deploy-to-subscription.md).
- Diese Option kann nur für Bereitstellungen auf Stammebene verwendet werden. Bereitstellungen aus einer geschachtelten Vorlage können nicht erneut bereitgestellt werden.

Zur Verwendung dieser Option müssen die Bereitstellungen eindeutige Namen aufweisen, damit sie im Verlauf identifiziert werden können. Wenn die Bereitstellungen keine eindeutigen Namen aufweisen, wird die vorherige erfolgreich ausgeführte Bereitstellung im Verlauf möglicherweise durch die aktuelle fehlerhafte Bereitstellung überschrieben.

## <a name="powershell"></a>PowerShell

Um die letzte erfolgreiche Bereitstellung erneut bereitzustellen, fügen Sie den Parameter `-RollbackToLastDeployment` als Flag hinzu.

```azurepowershell-interactive
New-AzResourceGroupDeployment -Name ExampleDeployment02 `
  -ResourceGroupName $resourceGroupName `
  -TemplateFile c:\MyTemplates\azuredeploy.json `
  -RollbackToLastDeployment
```

Um eine bestimmte Bereitstellung erneut bereitzustellen, verwenden den Parameter `-RollBackDeploymentName` und geben den Namen der Bereitstellung an. Die angegebene Bereitstellung muss erfolgreich ausgeführt worden sein.

```azurepowershell-interactive
New-AzResourceGroupDeployment -Name ExampleDeployment02 `
  -ResourceGroupName $resourceGroupName `
  -TemplateFile c:\MyTemplates\azuredeploy.json `
  -RollBackDeploymentName ExampleDeployment01
```

## <a name="azure-cli"></a>Azure-Befehlszeilenschnittstelle

Um die letzte erfolgreiche Bereitstellung erneut bereitzustellen, fügen Sie den Parameter `--rollback-on-error` als Flag hinzu.

```azurecli-interactive
az group deployment create \
  --name ExampleDeployment \
  --resource-group ExampleGroup \
  --template-file storage.json \
  --parameters storageAccountType=Standard_GRS \
  --rollback-on-error
```

Um eine bestimmte Bereitstellung erneut bereitzustellen, verwenden den Parameter `--rollback-on-error` und geben den Namen der Bereitstellung an. Die angegebene Bereitstellung muss erfolgreich ausgeführt worden sein.

```azurecli-interactive
az group deployment create \
  --name ExampleDeployment02 \
  --resource-group ExampleGroup \
  --template-file storage.json \
  --parameters storageAccountType=Standard_GRS \
  --rollback-on-error ExampleDeployment01
```

## <a name="rest-api"></a>REST-API

Um die letzte erfolgreich ausgeführte Bereitstellung erneut bereitzustellen, wenn bei der aktuellen Bereitstellung Fehler auftreten, verwenden Sie folgenden Code:

```json
{
  "properties": {
    "templateLink": {
      "uri": "http://mystorageaccount.blob.core.windows.net/templates/template.json",
      "contentVersion": "1.0.0.0"
    },
    "mode": "Incremental",
    "parametersLink": {
      "uri": "http://mystorageaccount.blob.core.windows.net/templates/parameters.json",
      "contentVersion": "1.0.0.0"
    },
    "onErrorDeployment": {
      "type": "LastSuccessful",
    }
  }
}
```

Um eine bestimmte Bereitstellung erneut bereitzustellen, wenn bei der aktuellen Bereitstellung Fehler auftreten, verwenden Sie folgenden Code:

```json
{
  "properties": {
    "templateLink": {
      "uri": "http://mystorageaccount.blob.core.windows.net/templates/template.json",
      "contentVersion": "1.0.0.0"
    },
    "mode": "Incremental",
    "parametersLink": {
      "uri": "http://mystorageaccount.blob.core.windows.net/templates/parameters.json",
      "contentVersion": "1.0.0.0"
    },
    "onErrorDeployment": {
      "type": "SpecificDeployment",
      "deploymentName": "<deploymentname>"
    }
  }
}
```

Die angegebene Bereitstellung muss erfolgreich ausgeführt worden sein.

## <a name="next-steps"></a>Nächste Schritte

- Informationen zum sicheren Rollout Ihres Diensts in mehreren Regionen finden Sie im Artikel zum [Azure-Bereitstellungs-Manager](deployment-manager-overview.md).
- Wenn Sie angeben möchten, wie Ressourcen behandelt werden sollen, die in der Ressourcengruppe enthalten sind, aber nicht in der Vorlage definiert wurden, lesen Sie die Informationen unter [Azure Resource Manager-Bereitstellungsmodi](deployment-modes.md).
- Um zu verstehen, wie Parameter in der Vorlage definiert werden, lesen Sie [Verstehen der Struktur und Syntax von Azure Resource Manager-Vorlagen](resource-group-authoring-templates.md).
- Informationen zum Bereitstellen einer Vorlage, die ein SAS-Token erfordert, finden Sie unter [Bereitstellen einer privaten Vorlage mit SAS-Token](resource-manager-powershell-sas-token.md).
