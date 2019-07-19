---
title: Hinzufügen benutzerdefinierter Ressourcen zur Azure-REST-API
description: Erfahren Sie, wie Sie der Azure-REST-API benutzerdefinierte Ressourcen hinzufügen. In diesem Artikel werden die Anforderungen und bewährten Methoden für Endpunkte erläutert, bei denen benutzerdefinierte Ressourcen implementiert werden sollen.
services: managed-applications
ms.service: managed-applications
ms.topic: conceptual
ms.author: jobreen
author: jjbfour
ms.date: 06/20/2019
ms.openlocfilehash: b94d59b55a62797e142768dc84ec499d714bd067
ms.sourcegitcommit: f811238c0d732deb1f0892fe7a20a26c993bc4fc
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 06/29/2019
ms.locfileid: "67479139"
---
# <a name="adding-custom-resources-to-azure-rest-api"></a>Hinzufügen benutzerdefinierter Ressourcen zur Azure-REST-API

In diesem Artikel werden die Anforderungen und bewährten Methoden für das Erstellen von Endpunkten für benutzerdefinierte Azure-Ressourcenanbieter erläutert, bei denen benutzerdefinierte Ressourcen implementiert werden. Wenn Sie mit benutzerdefinierten Azure-Ressourcenanbietern nicht vertraut sind, siehe die [Übersicht über benutzerdefinierte Ressourcenanbieter](./custom-providers-overview.md).

## <a name="how-to-define-a-resource-endpoint"></a>Definieren eines Endpunkts einer Ressource

Ein **Endpunkt** ist eine URL, die auf einen Dienst verweist, der den zugrunde liegenden Vertrag zwischen ihm und Azure implementiert. Der Endpunkt ist im benutzerdefinierten Ressourcenanbieter definiert und kann jede öffentlich zugängliche URL sein. Das folgende Beispiel enthält einen **Ressourcentyp** namens `myCustomResource`, der von `endpointURL` implementiert wurde.

Beispiel eines **Ressourcenanbieters**:

```JSON
{
  "properties": {
    "resourceTypes": [
      {
        "name": "myCustomResource",
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

## <a name="building-a-resource-endpoint"></a>Erstellen eines Ressourcenendpunkts

Ein **Endpunkt**, der einen **Ressourcentyp** implementiert, muss die Anforderung und Antwort für die neue API in Azure bearbeiten. Wenn ein benutzerdefinierter Ressourcenanbieter mit einem **Ressourcentyp** erstellt wird, generiert er in Azure einen neuen Satz von APIs. In diesem Fall erzeugt der **Ressourcentyp** eine neue Azure-Ressourcen-API für `PUT`, `GET` und `DELETE`, um CRUD-Vorgänge (Create [Erstellen], Retrieve [Abrufen], Update [Aktualisieren], Delete [Löschen]) für eine einzelne Ressource auszuführen, sowie `GET`, um alle vorhandenen Ressourcen abzurufen:

Bearbeiten Sie eine einzelne Ressource (`PUT`, `GET` und `DELETE`):

``` JSON
/subscriptions/{subscriptionId}/resourceGroups/{resourceGroupName}/providers/Microsoft.CustomProviders/resourceProviders/{resourceProviderName}/myCustomResource/{myCustomResourceName}
```

Rufen Sie alle Ressourcen ab (`GET`):

``` JSON
/subscriptions/{subscriptionId}/resourceGroups/{resourceGroupName}/providers/Microsoft.CustomProviders/resourceProviders/{resourceProviderName}/myCustomResource
```

Für benutzerdefinierte Ressourcen bieten benutzerdefinierte Ressourcenanbieter zwei **Routingtypen** an: `Proxy` und `Proxy, Cache`.

### <a name="proxy-routing-type"></a>Proxyroutingtyp

Der `Proxy`-**Routingtyp** leitet alle Anforderungsmethoden zu dem im benutzerdefinierten Ressourcenprovider angegebenen **Endpunkt**. `Proxy` sollte in folgenden Fällen verwendet werden:

- Vollständige Kontrolle über die Antwort ist erforderlich.
- Integrieren von Systemen mit bestehenden Ressourcen.

Weitere Informationen über `Proxy`-Ressourcen finden Sie in der [Referenz zum benutzerdefinierten Ressourcenproxy](./custom-providers-proxy-resource-endpoint-reference.md).

### <a name="proxy-cache-routing-type"></a>Proxycache-Routingtyp

Der `Proxy, Cache`-**Routingtyp** leitet nur `PUT`- und `DELETE`-Anforderungsmethoden zu dem im benutzerdefinierten Ressourcenanbieter angegebenen **Endpunkt**. Der benutzerdefinierte Ressourcenanbieter gibt automatisch `GET`-Anforderungen auf Basis dessen zurück, was er im Cache gespeichert hat. Wenn eine benutzerdefinierte Ressource mit einem Cache markiert ist, fügt der benutzerdefinierte Ressourcenanbieter auch Felder in der Antwort hinzu bzw. überschreibt sie, um die APIs zu Azure konform zu machen. `Proxy, Cache` sollte in folgenden Fällen verwendet werden:

- Erstellen eines neuen Systems, das über keine vorhandenen Ressourcen verfügt.
- Arbeiten mit vorhandenem Azure-Ökosystem.

Weitere Informationen über `Proxy, Cache`-Ressourcen finden Sie in der [Referenz zum benutzerdefinierten Ressourcencache](./custom-providers-proxy-cache-resource-endpoint-reference.md).

## <a name="creating-a-custom-resource"></a>Erstellen einer benutzerdefinierten Ressource

Es gibt prinzipiell zwei Möglichkeiten, eine benutzerdefinierte Ressource aus einem benutzerdefinierten Ressourcenanbieter zu erstellen:

- Azure-Befehlszeilenschnittstelle
- Azure Resource Manager-Vorlagen

### <a name="azure-cli"></a>Azure-Befehlszeilenschnittstelle

Erstellen einer benutzerdefinierten Ressource:

```azurecli-interactive
az resource create --is-full-object \
                   --id /subscriptions/{subscriptionId}/resourceGroups/{resourceGroupName}/providers/Microsoft.CustomProviders/resourceProviders/{resourceProviderName}/{resourceTypeName}/{customResourceName} \
                   --properties \
                    '{
                        "location": "eastus",
                        "properties": {
                            "myProperty1": "myPropertyValue1",
                            "myProperty2": {
                                "myProperty3": "myPropertyValue3"
                            }
                        }
                    }'
```

Parameter | Erforderlich | BESCHREIBUNG
---|---|---
is-full-object | *Ja* | Gibt an, dass das properties-Objekt noch andere Optionen enthält, z.B. „location“, „tags“, „sku“ und/oder „plan“.
id | *Ja* | Die Ressourcen-ID der benutzerdefinierten Ressource. Diese Ressource sollte außerhalb des **Ressourcenanbieters** vorhanden sein.
properties | *Ja* | Der Anforderungstext, der an den **Endpunkt** gesendet wird.

Löschen einer benutzerdefinierten Azure-Ressource:

```azurecli-interactive
az resource delete --id /subscriptions/{subscriptionId}/resourceGroups/{resourceGroupName}/providers/Microsoft.CustomProviders/resourceProviders/{resourceProviderName}/{resourceTypeName}/{customResourceName}
```

Parameter | Erforderlich | BESCHREIBUNG
---|---|---
id | *Ja* | Die Ressourcen-ID der benutzerdefinierten Ressource. Diese Ressource sollte außerhalb des **Ressourcenanbieters** vorhanden sein.

Abrufen einer benutzerdefinierten Azure-Ressource:

```azurecli-interactive
az resource show --id /subscriptions/{subscriptionId}/resourceGroups/{resourceGroupName}/providers/Microsoft.CustomProviders/resourceProviders/{resourceProviderName}/{resourceTypeName}/{customResourceName}
```

Parameter | Erforderlich | BESCHREIBUNG
---|---|---
id | *Ja* | Die Ressourcen-ID der benutzerdefinierten Ressource. Diese Ressource sollte außerhalb des **Ressourcenanbieters** vorhanden sein.

### <a name="azure-resource-manager-template"></a>Azure Resource Manager-Vorlage

> [!NOTE]
> Ressourcen erfordern, dass die Antwort einen entsprechenden `id`, `name` und `type` aus dem **Endpunkt** enthält.

Azure Resource Manager-Vorlagen erfordern, dass `id`, `name` und `type` ordnungsgemäß vom Downstreamendpunkt zurückgegeben werden. Eine zurückgegebene Ressourcenantwort sollte folgende Form haben:

Beispiel einer **Endpunkt**antwort:

``` JSON
{
  "properties": {
    "myProperty1": "myPropertyValue1",
    "myProperty2": {
        "myProperty3": "myPropertyValue3"
    }
  },
  "id": "/subscriptions/{subscriptionId}/resourceGroups/{resourceGroupName}/providers/Microsoft.CustomProviders/resourceProviders/{customResourceName}",
  "name": "{customResourceName}",
  "type": "Microsoft.CustomProviders/resourceProviders/{resourceTypeName}"
}
```

Beispiel für eine Azure Resource Manager-Vorlage:

```JSON
{
    "$schema": "http://schema.management.azure.com/schemas/2015-01-01/deploymentTemplate.json#",
    "contentVersion": "1.0.0.0",
    "resources": [
        {
            "type": "Microsoft.CustomProviders/resourceProviders/{resourceTypeName}",
            "name": "{resourceProviderName}/{customResourceName}",
            "apiVersion": "2018-09-01-preview",
            "location": "eastus",
            "properties": {
                "myProperty1": "myPropertyValue1",
                "myProperty2": {
                    "myProperty3": "myPropertyValue3"
                }
            }
        }
    ]
}
```

Parameter | Erforderlich | BESCHREIBUNG
---|---|---
resourceTypeName | *Ja* | Der **Name** des im benutzerdefinierten Anbieter definierten **Ressourcentyps**.
resourceProviderName | *Ja* | Der Name der Instanz des benutzerdefinierten Ressourcenanbieters.
customResourceName | *Ja* | Der Name der benutzerdefinierten Ressource.

## <a name="next-steps"></a>Nächste Schritte

- [Übersicht über benutzerdefinierte Azure-Ressourcenanbieter](./custom-providers-overview.md)
- [Tutorial: Erstellen eines benutzerdefinierten Azure-Ressourcenanbieters und Bereitstellen benutzerdefinierter Ressourcen](./create-custom-provider.md)
- [How To: Hinzufügen benutzerdefinierter Aktionen zur Azure-REST-API](./custom-providers-action-endpoint-how-to.md)
- [Referenz: Referenz zu benutzerdefinierten Ressourcenproxys](./custom-providers-proxy-resource-endpoint-reference.md)
- [Referenz: Referenz zum benutzerdefinierten Ressourcencache](./custom-providers-proxy-cache-resource-endpoint-reference.md)
