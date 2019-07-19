---
title: Referenz zu benutzerdefinierten Ressourcenproxys
description: Referenz zum benutzerdefinierten Ressourcenproxy für benutzerdefinierte Azure-Ressourcenanbieter. In diesem Artikel werden die Anforderungen an Endgeräte zur Implementierung benutzerdefinierter Proxyressourcen erläutert.
services: managed-applications
ms.service: managed-applications
ms.topic: conceptual
ms.author: jobreen
author: jjbfour
ms.date: 06/20/2019
ms.openlocfilehash: bbb907a7d73036352d4f5b8f36ccefacd89681ae
ms.sourcegitcommit: f811238c0d732deb1f0892fe7a20a26c993bc4fc
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 06/29/2019
ms.locfileid: "67479091"
---
# <a name="custom-resource-proxy-reference"></a>Referenz zu benutzerdefinierten Ressourcenproxys

In diesem Artikel werden die Anforderungen an Endgeräte zur Implementierung benutzerdefinierter Proxyressourcen erläutert. Wenn Sie mit benutzerdefinierten Azure-Ressourcenanbietern nicht vertraut sind, siehe die [Übersicht über benutzerdefinierte Ressourcenanbieter](./custom-providers-overview.md).

## <a name="how-to-define-a-proxy-resource-endpoint"></a>Definieren eines Endpunkts einer Proxyressource

Eine Proxyressource kann erstellt werden, indem **routingType** als „Proxy“ angegeben wird.

Beispiel eines benutzerdefinierten Ressourcenanbieters:

```JSON
{
  "properties": {
    "resourceTypes": [
      {
        "name": "myCustomResources",
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

## <a name="building-proxy-resource-endpoint"></a>Erstellen des Ressourcenendpunkts des Proxys

Ein **Endpunkt**, der einen **Endpunkt** einer Ressource des Typs „Proxy“ implementiert, muss die Anforderung und Antwort für die neue API in Azure verarbeiten. In diesem Fall erzeugt **resourceType** eine neue Azure-Ressourcen-API für `PUT`, `GET` und `DELETE`, um CRUD-Vorgänge (Create [Erstellen], Retrieve [Abrufen], Update [Aktualisieren], Delete [Löschen]) für eine einzelne Ressource auszuführen, sowie `GET`, um alle vorhandenen Ressourcen abzurufen.

> [!NOTE]
> Die Felder `id`, `name` und `type` sind nicht erforderlich, werden aber benötigt, um die benutzerdefinierte Ressource in das bestehende Azure-Ökosystem zu integrieren.

Beispielressource:

``` JSON
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

Parameterreferenz:

Eigenschaft | Beispiel | BESCHREIBUNG
---|---|---
name | '{myCustomResourceName}' | Der Name der benutzerdefinierten Ressource.
type | 'Microsoft.CustomProviders/resourceProviders/{resourceTypeName}' | Der Namespace des Ressourcentyps.
id | '/subscriptions/{subscriptionId}/resourceGroups/{resourceGroupName}/<br>providers/Microsoft.CustomProviders/resourceProviders/{resourceProviderName}/<br>myCustomResources/{myCustomResourceName}' | Die Ressourcen-ID.

### <a name="create-a-custom-resource"></a>Erstellen einer benutzerdefinierten Ressource

Azure-API – Eingehende Anforderung:

``` HTTP
PUT https://management.azure.com/subscriptions/{subscriptionId}/resourceGroups/{resourceGroupName}/providers/Microsoft.CustomProviders/resourceProviders/{resource-provider-name}/myCustomResources/{myCustomResourceName}?api-version=2018-09-01-preview
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

Antwort des **Endpunkts**:

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

Diese Anforderung wird dann in folgender Form an den **Endpunkt** weitergeleitet:

``` HTTP
GET https://{endpointURL}/?api-version=2018-09-01-preview
Content-Type: application/json
X-MS-CustomProviders-RequestPath: /subscriptions/{subscriptionId}/resourceGroups/{resourceGroupName}/providers/Microsoft.CustomProviders/resourceProviders/{resourceProviderName}/myCustomResources/{myCustomResourceName}
```

Ebenso wird die Antwort vom **Endpunkt** dann an den Kunden zurückgesendet. Die Antwort des Endpunkts sollte Folgendes zurückgeben:

- Ein gültiges JSON-Objektdokument. Alle Arrays und Zeichenfolgen müssen unter einem obersten Objekt geschachtelt sein.
- Der Header `Content-Type` muss auf „application/json; charset=utf-8“ festgelegt werden.

Antwort des **Endpunkts**:

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

Diese Anforderung wird dann in folgender Form an den **Endpunkt** weitergeleitet:

``` HTTP
GET https://{endpointURL}/?api-version=2018-09-01-preview
Content-Type: application/json
X-MS-CustomProviders-RequestPath: /subscriptions/{subscriptionId}/resourceGroups/{resourceGroupName}/providers/Microsoft.CustomProviders/resourceProviders/{resourceProviderName}/myCustomResources
```

Ebenso wird die Antwort vom **Endpunkt** dann an den Kunden zurückgesendet. Die Antwort des Endpunkts sollte Folgendes zurückgeben:

- Ein gültiges JSON-Objektdokument. Alle Arrays und Zeichenfolgen müssen unter einem obersten Objekt geschachtelt sein.
- Der Header `Content-Type` muss auf „application/json; charset=utf-8“ festgelegt werden.
- Die Liste der Ressourcen sollte unter der Eigenschaft `value` der obersten Ebene platziert werden.

Antwort des **Endpunkts**:

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
- [Tutorial: Erstellen eines benutzerdefinierten Azure-Ressourcenanbieters und Bereitstellen benutzerdefinierter Ressourcen](./create-custom-provider.md)
- [How To: Hinzufügen benutzerdefinierter Aktionen zur Azure-REST-API](./custom-providers-action-endpoint-how-to.md)
- [Referenz: Referenz zum benutzerdefinierten Ressourcencache](./custom-providers-proxy-cache-resource-endpoint-reference.md)
