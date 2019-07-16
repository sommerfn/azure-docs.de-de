---
title: Erstellen eines Ressourcenanbieters mit Azure Custom Providers (Vorschau)
description: Beschreibt das Erstellen eines Ressourcenanbieters und die Bereitstellung seiner benutzerdefinierten Ressourcentypen.
services: managed-applications
author: MSEvanhi
ms.service: managed-applications
ms.topic: tutorial
ms.date: 05/01/2019
ms.author: evanhi
ms.openlocfilehash: 41200139ef55fa1ae441192e2d81b5228cf29bad
ms.sourcegitcommit: 66237bcd9b08359a6cce8d671f846b0c93ee6a82
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 07/11/2019
ms.locfileid: "67795315"
---
# <a name="quickstart-create-custom-provider-and-deploy-custom-resources"></a>Schnellstart: Erstellen eines benutzerdefinierten Anbieters und Bereitstellen von benutzerdefinierten Ressourcen

In dieser Schnellstartanleitung erstellen Sie Ihren eigenen Ressourcenanbieter und stellen benutzerdefinierte Ressourcentypen für diesen Ressourcenanbieter bereit. Weitere Informationen zu benutzerdefinierten Anbietern finden Sie unter [Azure Custom Providers (Vorschau): Übersicht](custom-providers-overview.md).

## <a name="prerequisites"></a>Voraussetzungen

In dieser Schnellstartanleitung müssen REST-Vorgänge aufgerufen werden. Es gibt [verschiedene Möglichkeiten zum Senden von REST-Anforderungen](/rest/api/azure/). Wenn Sie noch nicht über ein Tool für REST-Vorgänge verfügen, installieren Sie [ARMClient](https://github.com/projectkudu/ARMClient). Es handelt sich um ein Open-Source-Befehlszeilentool, das den Aufruf der Azure Resource Manager-API vereinfacht.

## <a name="deploy-custom-provider"></a>Bereitstellen eines benutzerdefinierten Anbieters

Um den benutzerdefinierten Anbieter einzurichten, stellen Sie eine [Beispielvorlage](https://github.com/Azure/azure-docs-json-samples/blob/master/custom-providers/customprovider.json) für Ihr Azure-Abonnement bereit.

Nach dem Bereitstellen der Vorlage weist Ihr Abonnement die folgenden Ressourcen auf:

* Funktions-App mit den Vorgängen für die Ressourcen und Aktionen.
* Speicherkonto zum Speichern von Benutzern, die durch den benutzerdefinierten Anbieter erstellt werden.
* Benutzerdefinierten Anbieter, der die benutzerdefinierten Ressourcentypen und Aktionen definiert. Der Endpunkt der Funktions-App wird zum Senden von Anforderungen verwendet.
* Benutzerdefinierte Ressource vom benutzerdefinierten Anbieter.

Verwenden Sie zum Bereitstellen des benutzerdefinierten Anbieters mit PowerShell Folgendes:

```azurepowershell-interactive
$rgName = "<resource-group-name>"
$funcName = "<function-app-name>"

New-AzResourceGroup -Name $rgName -Location eastus
New-AzResourceGroupDeployment -ResourceGroupName $rgName `
  -TemplateUri https://raw.githubusercontent.com/Azure/azure-docs-json-samples/master/custom-providers/customprovider.json `
  -funcname $funcName
```

Sie können die Lösung auch mithilfe der folgenden Schaltfläche bereitstellen:

<a href="https://portal.azure.com/#create/Microsoft.Template/uri/https%3A%2F%2Fraw.githubusercontent.com%2FAzure%2Fazure-docs-json-samples%2Fmaster%2Fcustom-providers%2Fcustomprovider.json" target="_blank">
    <img src="https://azuredeploy.net/deploybutton.png"/>
</a>

## <a name="view-custom-provider-and-resource"></a>Anzeigen des benutzerdefinierten Anbieters und der Ressource

Im Portal ist der benutzerdefinierte Anbieter ein ausgeblendeten Ressourcentyp. Um zu bestätigen, dass der Ressourcenanbieter bereitgestellt wurde, navigieren Sie zur Ressourcengruppe. Wählen Sie die Option **Ausgeblendete Typen anzeigen** aus.

![Anzeigen von ausgeblendeten Ressourcentypen](./media/create-custom-providers/show-hidden.png)

Um den benutzerdefinierten Ressourcentyp anzuzeigen, den Sie bereitgestellt haben, verwenden Sie den GET-Vorgang für Ihren Ressourcentyp.

```
GET https://management.azure.com/subscriptions/<sub-id>/resourceGroups/<rg-name>/providers/Microsoft.CustomProviders/resourceProviders/<provider-name>/users?api-version=2018-09-01-preview
```

Verwenden Sie mit ARMClient Folgendes:

```powershell
$subID = (Get-AzContext).Subscription.Id
$requestURI = "https://management.azure.com/subscriptions/$subID/resourceGroups/$rgName/providers/Microsoft.CustomProviders/resourceProviders/$funcName/users?api-version=2018-09-01-preview"

armclient GET $requestURI
```

Sie erhalten die folgende Antwort:

```json
{
  "value": [
    {
      "properties": {
        "provisioningState": "Succeeded",
        "FullName": "Santa Claus",
        "Location": "NorthPole"
      },
      "id": "/subscriptions/<sub-id>/resourceGroups/<rg-name>/providers/Microsoft.CustomProviders/resourceProviders/<provider-name>/users/santa",
      "name": "santa",
      "type": "Microsoft.CustomProviders/resourceProviders/users"
    }
  ]
}
```

## <a name="call-action"></a>Aufrufaktion

Der benutzerdefinierte Anbieter verfügt auch über eine Aktion namens **ping**. Der Code, der die Anforderung verarbeitet, wird in der Funktions-App implementiert. Die ping-Aktion antwortet mit einem Gruß.

Um eine ping-Anforderung zu senden, verwenden Sie den POST-Vorgang für Ihren benutzerdefinierten Anbieter.

```
POST https://management.azure.com/subscriptions/<sub-id>/resourceGroups/<rg-name>/providers/Microsoft.CustomProviders/resourceProviders/<provider-name>/ping?api-version=2018-09-01-preview
```

Verwenden Sie mit ARMClient Folgendes:

```powershell
$pingURI = "https://management.azure.com/subscriptions/$subID/resourceGroups/$rgName/providers/Microsoft.CustomProviders/resourceProviders/$funcName/ping?api-version=2018-09-01-preview"

armclient POST $pingURI
```

Sie erhalten die folgende Antwort:

```json
{
  "pingcontent": {
    "source": "<function-name>.azurewebsites.net"
  },
  "message": "hello <function-name>.azurewebsites.net"
}
```

## <a name="create-resource-type"></a>Erstellen des Ressourcentyps

Um den benutzerdefinierten Ressourcentyp zu erstellen, können Sie die Ressource in einer Vorlage bereitstellen. Dieser Ansatz wird in der Vorlage gezeigt, die Sie in dieser Schnellstartanleitung bereitgestellt haben. Sie können auch eine PUT-Anforderung für den Ressourcentyp senden.

```
PUT https://management.azure.com/subscriptions/<sub-id>/resourceGroups/<rg-name>/providers/Microsoft.CustomProviders/resourceProviders/<provider-name>/users/<resource-name>?api-version=2018-09-01-preview

{"properties":{"FullName": "Test User", "Location": "Earth"}}
```

Verwenden Sie mit ARMClient Folgendes:

```powershell
$addURI = "https://management.azure.com/subscriptions/$subID/resourceGroups/$rgName/providers/Microsoft.CustomProviders/resourceProviders/$funcName/users/testuser?api-version=2018-09-01-preview"
$requestBody = "{'properties':{'FullName': 'Test User', 'Location': 'Earth'}}"

armclient PUT $addURI $requestBody
```

Sie erhalten die folgende Antwort:

```json
{
  "properties": {
    "provisioningState": "Succeeded",
    "FullName": "Test User",
    "Location": "Earth"
  },
  "id": "/subscriptions/<sub-ID>/resourceGroups/<rg-name>/providers/Microsoft.CustomProviders/resourceProviders/<provider-name>/users/testuser",
  "name": "testuser",
  "type": "Microsoft.CustomProviders/resourceProviders/users"
}
```

## <a name="next-steps"></a>Nächste Schritte

Eine Einführung in benutzerdefinierte Anbieter finden Sie unter [Azure Custom Providers (Vorschau): Übersicht](custom-providers-overview.md).
