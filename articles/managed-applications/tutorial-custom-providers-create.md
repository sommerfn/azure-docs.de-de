---
title: Erstellen und Nutzen eines benutzerdefinierten Azure-Anbieters
description: In diesem Tutorial wird beschrieben, wie Sie einen benutzerdefinierten Anbieter erstellen und nutzen.
author: jjbfour
ms.service: managed-applications
ms.topic: tutorial
ms.date: 06/19/2019
ms.author: jobreen
ms.openlocfilehash: 65a8e60d8216e1da16af987c9e699e24ecaec3ec
ms.sourcegitcommit: 66237bcd9b08359a6cce8d671f846b0c93ee6a82
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 07/11/2019
ms.locfileid: "67799129"
---
# <a name="authoring-a-restful-endpoint-for-custom-providers"></a>Erstellen eines RESTful-Endpunkts für benutzerdefinierte Anbieter

Benutzerdefinierte Anbieter ermöglichen Ihnen die Anpassung von Workflows in Azure. Ein benutzerdefinierter Anbieter ist ein Vertrag zwischen Azure und einem Endpunkt (`endpoint`). In diesem Tutorial wird der Prozess zum Erstellen eines benutzerdefinierten Anbieters durchlaufen. Falls Sie noch nicht mit benutzerdefinierten Azure-Anbietern vertraut sind, hilft Ihnen die [Übersicht über benutzerdefinierte Ressourcenanbieter](./custom-providers-overview.md) weiter.

Dieses Tutorial ist in die folgenden Schritte unterteilt:

- Was ist ein benutzerdefinierter Anbieter?
- Definieren von benutzerdefinierten Aktionen und Ressourcen
- Bereitstellen des benutzerdefinierten Anbieters

Dieses Tutorial baut auf den folgenden Tutorials auf:

- [Erstellen eines RESTful-Endpunkts für benutzerdefinierte Anbieter](./tutorial-custom-providers-function-authoring.md)

## <a name="creating-a-custom-provider"></a>Erstellen eines benutzerdefinierten Anbieters

> [!NOTE]
> Die Erstellung eines Endpunkts wird in diesem Tutorial nicht beschrieben. Verwenden Sie das [Tutorial zur Erstellung von RESTful-Endpunkten](./tutorial-custom-providers-function-authoring.md), falls Sie nicht über einen RESTful-Endpunkt verfügen.

Nach der Erstellung des Endpunkts (`endpoint`) können Sie einen benutzerdefinierten Anbieter erstellen, um einen Vertrag zwischen dem Anbieter und dem `endpoint`-Element zu generieren. Bei einem benutzerdefinierten Anbieter können Sie eine Liste mit Endpunktdefinitionen angeben.

```JSON
{
  "name": "myEndpointDefinition",
  "routingType": "Proxy",
  "endpoint": "https://<yourapp>.azurewebsites.net/api/<funcname>?code=<functionkey>"
}
```

Eigenschaft | Erforderlich | BESCHREIBUNG
---|---|---
name | *Ja* | Der Name der Endpunktdefinition. Azure macht diesen Namen über seine API unter „/subscriptions/{subscriptionId}/resourceGroups/{resourceGroupName}/providers/Microsoft.CustomProviders/<br>resourceProviders/{resourceProviderName}/{endpointDefinitionName}“ verfügbar.
routingType | *Nein* | Bestimmt den Typ des Vertrags mit `endpoint`. Wenn nichts angegeben ist, wird standardmäßig „Proxy“ verwendet.
endpoint | *Ja* | Der Endpunkt, an den die Anforderungen geleitet werden. Hiermit werden die Antwort sowie alle Nebenwirkungen der Anforderung verarbeitet.

In diesem Fall ist `endpoint` die Trigger-URL der Azure-Funktion. `<yourapp>`, `<funcname>` und `<functionkey>` sollten durch Werte für Ihre erstellte Funktion ersetzt werden.

## <a name="defining-custom-actions-and-resources"></a>Definieren von benutzerdefinierten Aktionen und Ressourcen

Der benutzerdefinierte Anbieter enthält eine Liste mit Endpunktdefinitionen, die unter `actions` und `resourceTypes` modelliert sind. Aktionen (`actions`) werden den benutzerdefinierten Aktionen zugeordnet, die vom benutzerdefinierten Anbieter verfügbar gemacht werden, während `resourceTypes` für die benutzerdefinierten Ressourcen steht. Für dieses Tutorial definieren wir einen benutzerdefinierten Anbieter mit einem `action`-Element mit dem Namen `myCustomAction` und einem `resourceType`-Element mit dem Namen `myCustomResources`.

```JSON
{
  "properties": {
    "actions": [
      {
        "name": "myCustomAction",
        "routingType": "Proxy",
        "endpoint": "https://<yourapp>.azurewebsites.net/api/<funcname>?code=<functionkey>"
      }
    ],
    "resourceTypes": [
      {
        "name": "myCustomResources",
        "routingType": "Proxy",
        "endpoint": "https://<yourapp>.azurewebsites.net/api/<funcname>?code=<functionkey>"
      }
    ]
  },
  "location": "eastus"
}
```

Ersetzen Sie `endpoint` durch die Trigger-URL aus der Funktion, die Sie im vorherigen Tutorial erstellt haben.

## <a name="deploying-the-custom-provider"></a>Bereitstellen des benutzerdefinierten Anbieters

> [!NOTE]
> `endpoint` sollte durch die Funktions-URL ersetzt werden.

Der obige benutzerdefinierte Anbieter kann mit einer Azure Resource Manager-Vorlage bereitgestellt werden.

```JSON
{
    "$schema": "http://schema.management.azure.com/schemas/2015-01-01/deploymentTemplate.json#",
    "contentVersion": "1.0.0.0",
    "resources": [
        {
            "type": "Microsoft.CustomProviders/resourceProviders",
            "name": "myCustomProvider",
            "apiVersion": "2018-09-01-preview",
            "location": "eastus",
            "properties": {
                "actions": [
                    {
                        "name": "myCustomAction",
                        "routingType": "Proxy",
                        "endpoint": "https://<yourapp>.azurewebsites.net/api/<funcname>?code=<functionkey>"
                    }
                ],
                "resourceTypes": [
                    {
                        "name": "myCustomResources",
                        "routingType": "Proxy",
                        "endpoint": "https://<yourapp>.azurewebsites.net/api/<funcname>?code=<functionkey>"
                    }
                ]
            }
        }
    ]
}
```

## <a name="using-custom-actions-and-resources"></a>Verwenden von benutzerdefinierten Aktionen und Ressourcen

Nachdem wir einen benutzerdefinierten Anbieter erstellt haben, können wir die neuen Azure-APIs nutzen. Im nächsten Abschnitt wird erläutert, wie Sie einen benutzerdefinierten Anbieter aufrufen und verwenden.

### <a name="custom-actions"></a>Benutzerdefinierte Aktionen

# <a name="azure-clitabazure-cli"></a>[Azure-Befehlszeilenschnittstelle](#tab/azure-cli)

> [!NOTE]
> `{subscriptionId}` und `{resourceGroupName}` sollten durch das Abonnement und die Ressourcengruppe ersetzt werden, das bzw. die für die Bereitstellung des benutzerdefinierten Anbieters verwendet wurde.

```azurecli-interactive
az resource invoke-action --action myCustomAction \
                          --ids /subscriptions/{subscriptionId}/resourceGroups/{resourceGroupName}/providers/Microsoft.CustomProviders/resourceProviders/myCustomProvider \
                          --request-body
                            '{
                                "hello": "world"
                            }'
```

Parameter | Erforderlich | BESCHREIBUNG
---|---|---
action | *Ja* | Der Name der Aktion, die im erstellten benutzerdefinierten Anbieter definiert ist.
ids | *Ja* | Die Ressourcen-ID des erstellten benutzerdefinierten Anbieters.
request-body | *Nein* | Der Anforderungstext, der an `endpoint` gesendet wird.

# <a name="templatetabtemplate"></a>[Vorlage](#tab/template)

None (Keine):

---

### <a name="custom-resources"></a>Benutzerdefinierte Ressourcen

# <a name="azure-clitabazure-cli"></a>[Azure-Befehlszeilenschnittstelle](#tab/azure-cli)

> [!NOTE]
> `{subscriptionId}` und `{resourceGroupName}` sollten durch das Abonnement und die Ressourcengruppe ersetzt werden, das bzw. die für die Bereitstellung des benutzerdefinierten Anbieters verwendet wurde.

Erstellen einer benutzerdefinierten Ressource:

```azurecli-interactive
az resource create --is-full-object \
                   --id /subscriptions/{subscriptionId}/resourceGroups/{resourceGroupName}/providers/Microsoft.CustomProviders/resourceProviders/myCustomProvider/myCustomResources/myTestResourceName1 \
                   --properties
                    '{
                        "location": "eastus",
                        "properties": {
                            "hello" : "world"
                        }
                    }'
```

Parameter | Erforderlich | BESCHREIBUNG
---|---|---
is-full-object | *Ja* | Gibt an, dass das properties-Objekt noch andere Optionen enthält, z. B. „location“, „tags“, „sku“ bzw. „plan“.
id | *Ja* | Die Ressourcen-ID der benutzerdefinierten Ressource. Diese Ressource sollte außerhalb des benutzerdefinierten Anbieters angeordnet sein.
properties | *Ja* | Der Anforderungstext, der an `endpoint` gesendet wird.

Löschen einer benutzerdefinierten Azure-Ressource:

```azurecli-interactive
az resource delete --id /subscriptions/{subscriptionId}/resourceGroups/{resourceGroupName}/providers/Microsoft.CustomProviders/resourceProviders/myCustomProvider/myCustomResources/myTestResourceName1
```

Parameter | Erforderlich | BESCHREIBUNG
---|---|---
id | *Ja* | Die Ressourcen-ID der benutzerdefinierten Ressource. Diese Ressource sollte außerhalb des benutzerdefinierten Anbieters angeordnet sein.

Abrufen einer benutzerdefinierten Azure-Ressource:

```azurecli-interactive
az resource show --id /subscriptions/{subscriptionId}/resourceGroups/{resourceGroupName}/providers/Microsoft.CustomProviders/resourceProviders/myCustomProvider/myCustomResources/myTestResourceName1
```

Parameter | Erforderlich | BESCHREIBUNG
---|---|---
id | *Ja* | Die Ressourcen-ID der benutzerdefinierten Ressource. Diese Ressource sollte außerhalb des benutzerdefinierten Anbieters angeordnet sein.

# <a name="templatetabtemplate"></a>[Vorlage](#tab/template)

Beispiel für eine Azure Resource Manager-Vorlage:

```JSON
{
    "$schema": "http://schema.management.azure.com/schemas/2015-01-01/deploymentTemplate.json#",
    "contentVersion": "1.0.0.0",
    "resources": [
        {
            "type": "Microsoft.CustomProviders/resourceProviders/myCustomResources",
            "name": "myCustomProvider/myTestResourceName1",
            "apiVersion": "2018-09-01-preview",
            "location": "eastus",
            "properties": {
                "hello": "world"
            }
        }
    ]
}
```

Parameter | Erforderlich | BESCHREIBUNG
---|---|---
resourceTypeName | *Ja* | `name` des *resourceType*-Elements, das im benutzerdefinierten Anbieter definiert ist.
resourceProviderName | *Ja* | Name der Instanz des benutzerdefinierten Anbieters.
customResourceName | *Ja* | Name der benutzerdefinierten Ressource.

---

> [!NOTE]
> Denken Sie nach Abschluss der Bereitstellung und Nutzung des benutzerdefinierten Anbieters daran, alle erstellten Ressourcen zu bereinigen, einschließlich der Azure-Funktion.

## <a name="next-steps"></a>Nächste Schritte

In diesem Artikel haben Sie Grundlegendes über benutzerdefiniert Anbieter gelernt. Im nächsten Artikel erstellen Sie einen benutzerdefinierten Anbieter.

- [How To: Hinzufügen benutzerdefinierter Aktionen zur Azure-REST-API](./custom-providers-action-endpoint-how-to.md)
- [How To: Hinzufügen benutzerdefinierter Ressourcen zur Azure-REST-API](./custom-providers-resources-endpoint-how-to.md)
