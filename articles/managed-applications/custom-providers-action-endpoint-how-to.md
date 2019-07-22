---
title: Hinzufügen benutzerdefinierter Aktionen zur Azure-REST-API
description: Erfahren Sie, wie Sie der Azure-REST-API benutzerdefinierte Aktionen hinzufügen. In diesem Artikel werden die Anforderungen und bewährten Methoden für Endpunkte erläutert, bei denen benutzerdefinierte Aktionen implementiert werden sollen.
services: managed-applications
ms.service: managed-applications
ms.topic: conceptual
ms.author: jobreen
author: jjbfour
ms.date: 06/20/2019
ms.openlocfilehash: 6fbd20c201e1b141b7276e3283599b00cdefd118
ms.sourcegitcommit: 66237bcd9b08359a6cce8d671f846b0c93ee6a82
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 07/11/2019
ms.locfileid: "67795297"
---
# <a name="adding-custom-actions-to-azure-rest-api"></a>Hinzufügen benutzerdefinierter Aktionen zur Azure-REST-API

In diesem Artikel werden die Anforderungen und bewährten Methoden für das Erstellen von Endpunkte für benutzerdefinierte Azure-Ressourcenanbieter erläutert, bei denen benutzerdefinierte Aktionen implementiert werden. Wenn Sie mit benutzerdefinierten Azure-Ressourcenanbietern nicht vertraut sind, siehe die [Übersicht über benutzerdefinierte Ressourcenanbieter](./custom-providers-overview.md).

## <a name="how-to-define-an-action-endpoint"></a>Definieren einer Aktionsendpunkts

Ein **Endpunkt** ist eine URL, die auf einen Dienst verweist, der den zugrunde liegenden Vertrag zwischen ihm und Azure implementiert. Der Endpunkt ist im benutzerdefinierten Ressourcenanbieter definiert und kann jede öffentlich zugängliche URL sein. Das folgende Beispiel enthält eine **Aktion** namens `myCustomAction`, die von `endpointURL` implementiert wurde.

Beispiel eines **Ressourcenanbieters**:

```JSON
{
  "properties": {
    "actions": [
      {
        "name": "myCustomAction",
        "routingType": "Proxy",
        "endpoint": "https://{endpointURL}/"
      }
    ]
  },
  "location": "eastus",
  "type": "Microsoft.CustomProviders/resourceProviders",
  "id": "/subscriptions/{subscriptionId}/resourceGroups/{resourceGroupName}/providers/Microsoft.CustomProviders/resourceProviders/{resourceProviderName}",
  "name": "{resourceProviderName}"
}
```

## <a name="building-an-action-endpoint"></a>Erstellen einen Aktionsendpunkts

Ein **Endpunkt**, der eine **Aktion** implementiert, muss die Anforderung und Antwort für die neue API in Azure bearbeiten. Wenn ein benutzerdefinierter Ressourcenanbieter mit einer **Aktion** erstellt wird, generiert er in Azure einen neuen Satz von APIs. In diesem Fall generiert die Aktion eine neue Azure-Aktions-API für Aufrufe von `POST`:

``` JSON
/subscriptions/{subscriptionId}/resourceGroups/{resourceGroupName}/providers/Microsoft.CustomProviders/resourceProviders/{resourceProviderName}/myCustomAction
```

Azure-API – Eingehende Anforderung:

``` HTTP
POST https://management.azure.com/subscriptions/{subscriptionId}/resourceGroups/{resourceGroupName}/providers/Microsoft.CustomProviders/resourceProviders/{resourceProviderName}/myCustomAction?api-version=2018-09-01-preview
Authorization: Bearer eyJ0e...
Content-Type: application/json

{
    "myProperty1": "myPropertyValue1",
    "myProperty2": {
        "myProperty3" : "myPropertyValue3"
    }
}
```

Diese Anforderung wird dann in folgender Form an den **Endpunkt** weitergeleitet:

``` HTTP
POST https://{endpointURL}/?api-version=2018-09-01-preview
Content-Type: application/json
X-MS-CustomProviders-RequestPath: /subscriptions/{subscriptionId}/resourceGroups/{resourceGroupName}/providers/Microsoft.CustomProviders/resourceProviders/{resourceProviderName}/myCustomAction

{
    "myProperty1": "myPropertyValue1",
    "myProperty2": {
        "myProperty3" : "myPropertyValue3"
    }
}
```

Ebenso wird die Antwort vom **Endpunkt** dann an den Kunden zurückgesendet. Die Antwort des Endpunkts sollte Folgendes zurückgeben:

- Ein gültiges JSON-Objektdokument. Alle Arrays und Zeichenfolgen müssen unter einem obersten Objekt geschachtelt sein.
- Der Header `Content-Type` muss auf „application/json; charset=utf-8“ festgelegt werden.

``` HTTP
HTTP/1.1 200 OK
Content-Type: application/json; charset=utf-8

{
    "myProperty1": "myPropertyValue1",
    "myProperty2": {
        "myProperty3" : "myPropertyValue3"
    }
}
```

Antwort des benutzerdefinierten Azure-Ressourcenanbieters:

``` HTTP
HTTP/1.1 200 OK
Content-Type: application/json; charset=utf-8

{
    "myProperty1": "myPropertyValue1",
    "myProperty2": {
        "myProperty3" : "myPropertyValue3"
    }
}
```

## <a name="calling-a-custom-action"></a>Aufrufen einer benutzerdefinierten Aktion

Es gibt prinzipiell zwei Möglichkeiten, eine benutzerdefinierte Aktion aus einem benutzerdefinierten Ressourcenanbieter aufzurufen:

- Azure-Befehlszeilenschnittstelle
- Azure Resource Manager-Vorlagen

### <a name="azure-cli"></a>Azure-Befehlszeilenschnittstelle

```azurecli-interactive
az resource invoke-action --action {actionName} \
                          --ids /subscriptions/{subscriptionId}/resourceGroups/{resourceGroupName}/providers/Microsoft.CustomProviders/resourceProviders/{resourceProviderName} \
                          --request-body \
                            '{
                                "myProperty1": "myPropertyValue1",
                                "myProperty2": {
                                    "myProperty3": "myPropertyValue3"
                                }
                            }'
```

Parameter | Erforderlich | BESCHREIBUNG
---|---|---
action | *Ja* | Name der Aktion, der im **Ressourcenanbieter** definiert ist.
ids | *Ja* | Ressourcen-ID des **Ressourcenanbieters**.
request-body | *Nein* | Der Anforderungstext, der an den **Endpunkt** gesendet wird.

### <a name="azure-resource-manager-template"></a>Azure Resource Manager-Vorlage

> [!NOTE]
> Aktionen werden in Azure Resource Manager-Vorlagen eingeschränkt unterstützt. Damit die Aktion innerhalb einer Vorlage aufgerufen werden kann, muss ihr Name das Präfix [`list`](../azure-resource-manager/resource-group-template-functions-resource.md#list) enthalten.

Beispiel eines **Ressourcenanbieters** mit der Aktion „List“:

```JSON
{
  "properties": {
    "actions": [
      {
        "name": "listMyCustomAction",
        "routingType": "Proxy",
        "endpoint": "https://{endpointURL}/"
      }
    ]
  },
  "location": "eastus"
}
```

Beispiel einer Azure Resource Manager-Vorlage:

``` JSON
{
    "$schema": "http://schema.management.azure.com/schemas/2015-01-01/deploymentTemplate.json#",
    "contentVersion": "1.0.0.0",
    "variables": {
        "resourceIdentifier": "/subscriptions/{subscriptionId}/resourceGroups/{resourceGroupName}/providers/Microsoft.CustomProviders/resourceProviders/{resourceProviderName}",
        "apiVersion": "2018-09-01-preview",
        "functionValues": {
            "myProperty1": "myPropertyValue1",
            "myProperty2": {
                "myProperty3": "myPropertyValue3"
            }
        }
    },
    "resources": [],
    "outputs": {
        "myCustomActionOutput": {
            "type": "object",
            "value": "[listMyCustomAction(variables('resourceIdentifier'), variables('apiVersion'), variables('functionValues'))]"
        }
    }
}
```

Parameter | Erforderlich | BESCHREIBUNG
---|---|---
resourceIdentifier | *Ja* | Ressourcen-ID des **Ressourcenanbieters**.
apiVersion | *Ja* | Die API-Version der Ressourcenlaufzeit. Diese muss stets „2018-09-01-Preview“ sein.
functionValues | *Nein* | Der Anforderungstext, der an den **Endpunkt** gesendet wird.

## <a name="next-steps"></a>Nächste Schritte

- [Übersicht über benutzerdefinierte Azure-Ressourcenanbieter](./custom-providers-overview.md)
- [Schnellstart: Erstellen eines benutzerdefinierten Azure-Ressourcenanbieters und Bereitstellen benutzerdefinierter Ressourcen](./create-custom-provider.md)
- [Tutorial: Erstellen von benutzerdefinierten Aktionen und Ressourcen in Azure](./tutorial-custom-providers-101.md)
- [How To: Hinzufügen benutzerdefinierter Ressourcen zur Azure-REST-API](./custom-providers-resources-endpoint-how-to.md)
