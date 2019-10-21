---
title: Erstellen und Verwenden eines benutzerdefinierten Anbieters
description: In diesem Tutorial wird beschrieben, wie Sie einen benutzerdefinierten Azure-Anbieter erstellen und nutzen. Mit benutzerdefinierten Anbietern können Sie Workflows in Azure ändern.
author: jjbfour
ms.service: managed-applications
ms.topic: tutorial
ms.date: 06/19/2019
ms.author: jobreen
ms.openlocfilehash: 6217e9007f20cb365aff0837f58f1a6074a3e6ce
ms.sourcegitcommit: 1d0b37e2e32aad35cc012ba36200389e65b75c21
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 10/15/2019
ms.locfileid: "72330058"
---
# <a name="create-and-use-a-custom-provider"></a>Erstellen und Verwenden eines benutzerdefinierten Anbieters

Ein benutzerdefinierter Anbieter ist ein Vertrag zwischen Azure und einem Endpunkt. Mit benutzerdefinierten Anbietern können Sie Workflows in Azure ändern. In diesem Tutorial wird der Prozess zum Erstellen eines benutzerdefinierten Anbieters veranschaulicht. Sollten Sie noch nicht mit benutzerdefinierten Azure-Anbietern vertraut sein, sehen Sie sich die [Übersicht über benutzerdefinierte Azure-Ressourcenanbieter](./custom-providers-overview.md) an.

## <a name="create-a-custom-provider"></a>Erstellen eines benutzerdefinierten Anbieters

> [!NOTE]
> In diesem Tutorial wird nicht gezeigt, wie Sie einen Endpunkt erstellen. Falls Sie keinen RESTful-Endpunkt besitzen, arbeiten Sie das [Tutorial zur Erstellung von RESTful-Endpunkten](./tutorial-custom-providers-function-authoring.md) durch. Dieses Tutorial dient als Grundlage für das aktuelle Tutorial.

Nach der Erstellung eines Endpunkts können Sie einen benutzerdefinierten Anbieter erstellen, um einen Vertrag zwischen dem Anbieter und dem Endpunkt zu generieren. Mit einem benutzerdefinierten Anbieter können Sie eine Liste mit Endpunktdefinitionen angeben:

```JSON
{
  "name": "myEndpointDefinition",
  "routingType": "Proxy",
  "endpoint": "https://<yourapp>.azurewebsites.net/api/<funcname>?code=<functionkey>"
}
```

Eigenschaft | Erforderlich | BESCHREIBUNG
---|---|---
**name** | Ja | Der Name der Endpunktdefinition. Azure macht diesen Namen über seine API unter „/subscriptions/{subscriptionId}/resourceGroups/{resourceGroupName}/providers/Microsoft.CustomProviders<br>/resourceProviders/{resourceProviderName}/{endpointDefinitionName}“ verfügbar.
**routingType** | Nein | Der Endpunktvertragstyp. Wird kein Wert angegeben, wird standardmäßig „Proxy“ verwendet.
**endpoint** | Ja | Der Endpunkt, an den die Anforderungen geleitet werden. Der Endpunkt verarbeitet die Antwort sowie alle Nebenwirkungen der Anforderung.

Der Wert von **endpoint** ist die Trigger-URL der Azure-Funktions-App. Die Platzhalter `<yourapp>`, `<funcname>` und `<functionkey>` müssen durch Werte für Ihre erstellte Funktions-App ersetzt werden.

## <a name="define-custom-actions-and-resources"></a>Definieren von benutzerdefinierten Aktionen und Ressourcen

Der benutzerdefinierte Anbieter enthält eine Liste mit Endpunktdefinitionen, die unter den Eigenschaften **actions** und **resourceTypes** modelliert sind. Die Eigenschaft **actions** ist den benutzerdefinierten Aktionen zugeordnet, die vom benutzerdefinierten Anbieter verfügbar gemacht werden, und bei der Eigenschaft **resourceTypes** handelt es sich um die benutzerdefinierten Ressourcen. In diesem Tutorial verfügt der benutzerdefinierte Anbieter über die Eigenschaft **actions** mit dem Namen `myCustomAction` und die Eigenschaft **resourceTypes** mit dem Namen `myCustomResources`.

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

## <a name="deploy-the-custom-provider"></a>Bereitstellen des benutzerdefinierten Anbieters

> [!NOTE]
> Sie müssen die **endpoint**-Werte durch die Trigger-URL aus der Funktions-App ersetzen, die im vorherigen Tutorial erstellt wurde.

Sie können den vorherigen benutzerdefinierten Anbieter mit einer Azure Resource Manager-Vorlage bereitstellen:

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

## <a name="use-custom-actions-and-resources"></a>Verwenden benutzerdefinierter Aktionen und Ressourcen

Nach dem Erstellen eines benutzerdefinierten Anbieters können Sie die neuen Azure-APIs verwenden. Auf den folgenden Registerkarten wird das Aufrufen und Verwenden eines benutzerdefinierten Anbieters erläutert:

### <a name="custom-actions"></a>Benutzerdefinierte Aktionen

# <a name="azure-clitabazure-cli"></a>[Azure-Befehlszeilenschnittstelle](#tab/azure-cli)

> [!NOTE]
> Sie müssen die Platzhalter `{subscriptionId}` und `{resourceGroupName}` durch das Abonnement und die Ressourcengruppe ersetzen, die Sie für die Bereitstellung des benutzerdefinierten Anbieters verwendet haben.

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
*action* | Ja | Der Name der Aktion, die im benutzerdefinierten Anbieter definiert ist
*ids* | Ja | Die Ressourcen-ID des benutzerdefinierten Anbieters
*request-body* | Nein | Der Anforderungstext, der an den Endpunkt gesendet wird

# <a name="templatetabtemplate"></a>[Vorlage](#tab/template)

None (Keine):

---

### <a name="custom-resources"></a>Benutzerdefinierte Ressourcen

# <a name="azure-clitabazure-cli"></a>[Azure-Befehlszeilenschnittstelle](#tab/azure-cli)

> [!NOTE]
> Sie müssen die Platzhalter `{subscriptionId}` und `{resourceGroupName}` durch das Abonnement und die Ressourcengruppe ersetzen, die Sie für die Bereitstellung des benutzerdefinierten Anbieters verwendet haben.

#### <a name="create-a-custom-resource"></a>Erstellen einer benutzerdefinierten Ressource

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
*is-full-object* | Ja | Gibt an, dass das properties-Objekt noch andere Optionen enthält, z. B. „location“, „tags“, „SKU“ oder „plan“.
*id* | Ja | Die Ressourcen-ID der benutzerdefinierten Ressource. Diese ID ist eine Erweiterung der Ressourcen-ID des benutzerdefinierten Anbieters.
*properties* | Ja | Der Anforderungstext, der an den Endpunkt gesendet wird

#### <a name="delete-a-custom-resource"></a>Löschen einer benutzerdefinierten Ressource

```azurecli-interactive
az resource delete --id /subscriptions/{subscriptionId}/resourceGroups/{resourceGroupName}/providers/Microsoft.CustomProviders/resourceProviders/myCustomProvider/myCustomResources/myTestResourceName1
```

Parameter | Erforderlich | BESCHREIBUNG
---|---|---
*id* | Ja | Die Ressourcen-ID der benutzerdefinierten Ressource. Diese ID ist eine Erweiterung der Ressourcen-ID des benutzerdefinierten Anbieters.

#### <a name="retrieve-a-custom-resource"></a>Abrufen einer benutzerdefinierten Ressource

```azurecli-interactive
az resource show --id /subscriptions/{subscriptionId}/resourceGroups/{resourceGroupName}/providers/Microsoft.CustomProviders/resourceProviders/myCustomProvider/myCustomResources/myTestResourceName1
```

Parameter | Erforderlich | BESCHREIBUNG
---|---|---
*id* | Ja | Die Ressourcen-ID der benutzerdefinierten Ressource. Diese ID ist eine Erweiterung der Ressourcen-ID des benutzerdefinierten Anbieters.

# <a name="templatetabtemplate"></a>[Vorlage](#tab/template)

Resource Manager-Beispielvorlage:

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
*resourceTypeName* | Ja | Der Wert für `name` der im benutzerdefinierten Anbieter definierten Eigenschaft **resourceTypes**
*resourceProviderName* | Ja | Name der Instanz des benutzerdefinierten Anbieters.
*customResourceName* | Ja | Der Name der benutzerdefinierten Ressource.

---

> [!NOTE]
> Denken Sie nach Abschluss der Bereitstellung und Nutzung des benutzerdefinierten Anbieters daran, alle erstellten Ressourcen zu bereinigen, einschließlich der Azure-Funktions-App.

## <a name="next-steps"></a>Nächste Schritte

In diesem Artikel haben Sie Grundlegendes über benutzerdefiniert Anbieter gelernt. Weitere Informationen finden Sie unter

- [Gewusst wie: Hinzufügen benutzerdefinierter Aktionen zur Azure-REST-API](./custom-providers-action-endpoint-how-to.md)
- [Gewusst wie: Hinzufügen benutzerdefinierter Ressourcen zur Azure-REST-API](./custom-providers-resources-endpoint-how-to.md)
