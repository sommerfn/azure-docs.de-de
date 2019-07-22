---
title: Referenz zum benutzerdefinierten Ressourcencache
description: Referenz zum benutzerdefinierten Ressourcencache für benutzerdefinierte Azure-Ressourcenanbieter. In diesem Artikel werden die Anforderungen an Endgeräte zur Implementierung benutzerdefinierter Cacheressourcen erläutert.
services: managed-applications
ms.service: managed-applications
ms.topic: conceptual
ms.author: jobreen
author: jjbfour
ms.date: 06/20/2019
ms.openlocfilehash: 2970e69e825dacb6f548b3e66a830f221ece0b1c
ms.sourcegitcommit: 66237bcd9b08359a6cce8d671f846b0c93ee6a82
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 07/11/2019
ms.locfileid: "67795222"
---
# <a name="custom-resource-cache-reference"></a>Referenz zum benutzerdefinierten Ressourcencache

In diesem Artikel werden die Anforderungen an Endgeräte zur Implementierung benutzerdefinierter Cacheressourcen erläutert. Wenn Sie mit benutzerdefinierten Azure-Ressourcenanbietern nicht vertraut sind, siehe die [Übersicht über benutzerdefinierte Ressourcenanbieter](./custom-providers-overview.md).

## <a name="how-to-define-a-cache-resource-endpoint"></a>Definieren eines Endpunkts einer Cacheressource

Eine Proxyressource kann erstellt werden, indem **RoutingType** als „Proxy, Cache“ angegeben wird.

Beispiel eines benutzerdefinierten Ressourcenanbieters:

```JSON
{
  "properties": {
    "resourceTypes": [
      {
        "name": "myCustomResources",
        "routingType": "Proxy, Cache",
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

## <a name="building-proxy-resource-endpoint"></a>Erstellen des Ressourcenendpunkts des Proxys

Ein **Endpunkt**, der einen **Endpunkt** einer Ressource des Typs „Proxy, Cache“ implementiert, muss die Anforderung und Antwort für die neue API in Azure verarbeiten. In diesem Fall erzeugt **resourceType** eine neue Azure-Ressourcen-API für `PUT`, `GET` und `DELETE`, um CRUD-Vorgänge (Create [Erstellen], Retrieve [Abrufen], Update [Aktualisieren], Delete [Löschen]) für eine einzelne Ressource auszuführen, sowie `GET`, um alle vorhandenen Ressourcen abzurufen:

> [!NOTE]
> Die Azure-API generiert die Anforderungsmethoden `PUT`, `GET` und `DELETE`, aber der **Endpunkt** des Caches muss nur `PUT` und `DELETE` verarbeiten.
> Es wird empfohlen, dass der **Endpunkt** auch `GET` implementiert.

### <a name="create-a-custom-resource"></a>Erstellen einer benutzerdefinierten Ressource

Azure-API – Eingehende Anforderung:

``` HTTP
PUT https://management.azure.com/subscriptions/{subscriptionId}/resourceGroups/{resourceGroupName}/providers/Microsoft.CustomProviders/resourceProviders/{resourceProviderName}/myCustomResources/{myCustomResourceName}?api-version=2018-09-01-preview
Authorization: Bearer eyJ0e...
Content-Type: application/json

{
    "properties": {
        "myProperty1": "myPropertyValue1",
        "myProperty2": {
            "myProperty3" : "myPropertyValue3"
        }
    }
}
```

Diese Anforderung wird dann in folgender Form an den **Endpunkt** weitergeleitet:

``` HTTP
PUT https://{endpointURL}/?api-version=2018-09-01-preview
Content-Type: application/json
X-MS-CustomProviders-RequestPath: /subscriptions/{subscriptionId}/resourceGroups/{resourceGroupName}/providers/Microsoft.CustomProviders/resourceProviders/{resourceProviderName}/myCustomResources/{myCustomResourceName}

{
    "properties": {
        "myProperty1": "myPropertyValue1",
        "myProperty2": {
            "myProperty3" : "myPropertyValue3"
        }
    }
}
```

Ebenso wird die Antwort vom **Endpunkt** dann an den Kunden zurückgesendet. Die Antwort des Endpunkts sollte Folgendes zurückgeben:

- Ein gültiges JSON-Objektdokument. Alle Arrays und Zeichenfolgen müssen unter einem obersten Objekt geschachtelt sein.
- Der Header `Content-Type` muss auf „application/json; charset=utf-8“ festgelegt werden.
- Der benutzerdefinierte Ressourcenanbieter überschreibt die Felder `name`, `type` und `id` für die Anforderung.
- Der benutzerdefinierte Ressourcenanbieter gibt für einen Cacheendpunkt nur Felder unter dem `properties`-Objekt zurück.

Antwort des **Endpunkts**:

``` HTTP
HTTP/1.1 200 OK
Content-Type: application/json; charset=utf-8

{
    "properties": {
        "myProperty1": "myPropertyValue1",
        "myProperty2": {
            "myProperty3" : "myPropertyValue3"
        }
    }
}
```

Die Felder `name`, `id` und `type` werden vom benutzerdefinierten Ressourcenanbieter automatisch für die benutzerdefinierte Ressource generiert.

Antwort des benutzerdefinierten Azure-Ressourcenanbieters:

``` HTTP
HTTP/1.1 200 OK
Content-Type: application/json; charset=utf-8

{
    "name": "{myCustomResourceName}",
    "id": "/subscriptions/{subscriptionId}/resourceGroups/{resourceGroupName}/providers/Microsoft.CustomProviders/resourceProviders/{resourceProviderName}/myCustomResources/{myCustomResourceName}",
    "type": "Microsoft.CustomProviders/resourceProviders/myCustomResources",
    "properties": {
        "myProperty1": "myPropertyValue1",
        "myProperty2": {
            "myProperty3" : "myPropertyValue3"
        }
    }
}
```

### <a name="remove-a-custom-resource"></a>Entfernen einer benutzerdefinierten Ressource

Azure-API – Eingehende Anforderung:

``` HTTP
Delete https://management.azure.com/subscriptions/{subscriptionId}/resourceGroups/{resourceGroupName}/providers/Microsoft.CustomProviders/resourceProviders/{resourceProviderName}/myCustomResources/{myCustomResourceName}?api-version=2018-09-01-preview
Authorization: Bearer eyJ0e...
Content-Type: application/json
```

Diese Anforderung wird dann in folgender Form an den **Endpunkt** weitergeleitet:

``` HTTP
Delete https://{endpointURL}/?api-version=2018-09-01-preview
Content-Type: application/json
X-MS-CustomProviders-RequestPath: /subscriptions/{subscriptionId}/resourceGroups/{resourceGroupName}/providers/Microsoft.CustomProviders/resourceProviders/{resourceProviderName}/myCustomResources/{myCustomResourceName}
```

Ebenso wird die Antwort vom **Endpunkt** dann an den Kunden zurückgesendet. Die Antwort des Endpunkts sollte Folgendes zurückgeben:

- Ein gültiges JSON-Objektdokument. Alle Arrays und Zeichenfolgen müssen unter einem obersten Objekt geschachtelt sein.
- Der Header `Content-Type` muss auf „application/json; charset=utf-8“ festgelegt werden.
- Der benutzerdefinierte Azure-Ressourcenanbieter entfernt das Element nur dann aus seinem Cache, wenn eine Antwort der Stufe 200 zurückgegeben wird. Selbst wenn die Ressource nicht vorhanden ist, sollte der **Endpunkt** 204 zurückgeben.

Antwort des **Endpunkts**:

``` HTTP
HTTP/1.1 200 OK
Content-Type: application/json; charset=utf-8
```

Antwort des benutzerdefinierten Azure-Ressourcenanbieters:

``` HTTP
HTTP/1.1 200 OK
Content-Type: application/json; charset=utf-8
```

### <a name="retrieve-a-custom-resource"></a>Abrufen einer benutzerdefinierten Ressource

Azure-API – Eingehende Anforderung:

``` HTTP
GET https://management.azure.com/subscriptions/{subscriptionId}/resourceGroups/{resourceGroupName}/providers/Microsoft.CustomProviders/resourceProviders/{resourceProviderName}/myCustomResources/{myCustomResourceName}?api-version=2018-09-01-preview
Authorization: Bearer eyJ0e...
Content-Type: application/json
```

Die Anforderung wird **nicht** an den **Endpunkt** weitergeleitet.

Antwort des benutzerdefinierten Azure-Ressourcenanbieters:

``` HTTP
HTTP/1.1 200 OK
Content-Type: application/json; charset=utf-8

{
    "name": "{myCustomResourceName}",
    "id": "/subscriptions/{subscriptionId}/resourceGroups/{resourceGroupName}/providers/Microsoft.CustomProviders/resourceProviders/{resourceProviderName}/myCustomResources/{myCustomResourceName}",
    "type": "Microsoft.CustomProviders/resourceProviders/myCustomResources",
    "properties": {
        "myProperty1": "myPropertyValue1",
        "myProperty2": {
            "myProperty3" : "myPropertyValue3"
        }
    }
}
```

### <a name="enumerate-all-custom-resources"></a>Aufzählen aller benutzerdefinierten Ressourcen

Azure-API – Eingehende Anforderung:

``` HTTP
GET https://management.azure.com/subscriptions/{subscriptionId}/resourceGroups/{resourceGroupName}/providers/Microsoft.CustomProviders/resourceProviders/{resourceProviderName}/myCustomResources?api-version=2018-09-01-preview
Authorization: Bearer eyJ0e...
Content-Type: application/json
```

Diese Anforderung wird **nicht** an den **Endpunkt** weitergeleitet.

Antwort des benutzerdefinierten Azure-Ressourcenanbieters:

``` HTTP
HTTP/1.1 200 OK
Content-Type: application/json; charset=utf-8

{
    "value" : [
        {
            "name": "{myCustomResourceName}",
            "id": "/subscriptions/{subscriptionId}/resourceGroups/{resourceGroupName}/providers/Microsoft.CustomProviders/resourceProviders/{resourceProviderName}/myCustomResources/{myCustomResourceName}",
            "type": "Microsoft.CustomProviders/resourceProviders/myCustomResources",
            "properties": {
                "myProperty1": "myPropertyValue1",
                "myProperty2": {
                    "myProperty3" : "myPropertyValue3"
                }
            }
        }
    ]
}
```

## <a name="next-steps"></a>Nächste Schritte

- [Übersicht über benutzerdefinierte Azure-Ressourcenanbieter](./custom-providers-overview.md)
- [Schnellstart: Erstellen eines benutzerdefinierten Azure-Ressourcenanbieters und Bereitstellen benutzerdefinierter Ressourcen](./create-custom-provider.md)
- [Tutorial: Erstellen von benutzerdefinierten Aktionen und Ressourcen in Azure](./tutorial-custom-providers-101.md)
- [How To: Hinzufügen benutzerdefinierter Aktionen zur Azure-REST-API](./custom-providers-action-endpoint-how-to.md)
- [Referenz: Referenz zu benutzerdefinierten Ressourcenproxys](./custom-providers-proxy-resource-endpoint-reference.md)
