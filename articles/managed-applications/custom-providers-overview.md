---
title: Übersicht über benutzerdefinierte Azure-Ressourcenanbieter
description: Erfahren Sie, was benutzerdefinierte Azure-Ressourcenanbieter sind und wie Sie die Azure-API-Ebene entsprechend Ihren Workflows erweitern.
author: jjbfour
ms.service: managed-applications
ms.topic: conceptual
ms.date: 06/19/2019
ms.author: jobreen
ms.openlocfilehash: f418cd6c5470740ce123448ddbbe54cb6e89dabe
ms.sourcegitcommit: f811238c0d732deb1f0892fe7a20a26c993bc4fc
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 06/29/2019
ms.locfileid: "67475961"
---
# <a name="azure-custom-resource-providers-overview"></a>Übersicht über benutzerdefinierte Azure-Ressourcenanbieter

Benutzerdefinierte Azure-Ressourcenanbieter sind eine Erweiterbarkeitsplattform für Azure. Sie ermöglichen Ihnen, benutzerdefinierte APIs zu definieren, mit denen Sie die standardmäßige Azure-Umgebung erweitern können. In diesem Artikel wird Folgendes beschrieben:

- Erstellen und Bereitstellen eines benutzerdefinierten Azure-Ressourcenanbieters
- Nutzen eines benutzerdefinierten Azure-Ressourcenanbieters zum Erweitern vorhandener Workflows
- Auffinden von Anleitungen und Codebeispielen für den Einstieg

![Übersicht zu benutzerdefinierten Anbietern](./media/custom-providers-overview/overview.png)

> [!IMPORTANT]
> Die benutzerdefinierten Anbieter sind derzeit als öffentliche Vorschauversion verfügbar.
> Diese Vorschauversion wird ohne Vereinbarung zum Servicelevel bereitgestellt und ist nicht für Produktionsworkloads vorgesehen. Manche Features werden möglicherweise nicht unterstützt oder sind nur eingeschränkt verwendbar.
> Weitere Informationen finden Sie unter [Zusätzliche Nutzungsbestimmungen für Microsoft Azure-Vorschauen](https://azure.microsoft.com/support/legal/preview-supplemental-terms/).

## <a name="what-can-custom-resource-providers-do"></a>Aufgaben benutzerdefinierter Ressourcenanbieter

Es folgen einige Beispiele der Aufgaben benutzerdefinierter Azure-Ressourcenanbieter:

- Erweitern der Azure Resource Manager-REST-API mit internen und externen Diensten
- Ermöglichen benutzerdefinierter Szenarien, die auf vorhandenen Azure-Workflows basieren
- Anpassen der Steuerung und Wirkung von Azure Resource Manager-Vorlagen

## <a name="what-is-a-custom-resource-provider"></a>Was ist ein benutzerdefinierter Ressourcenanbieter?

Benutzerdefinierte Azure-Ressourcenanbieter werden durch Abschluss eines Vertrags zwischen Azure und einem Endpunkt erstellt. Dieser Vertrag definiert eine Liste neuer Ressourcen und Aktionen mittels einer neuen Ressource, **Microsoft.CustomProviders/ResourceProviders**. Der benutzerdefinierte Ressourcenanbieter stellt diese neuen APIs anschließend in Azure zur Verfügung. Benutzerdefinierte Azure-Ressourcenanbieter bestehen aus drei Teilen: benutzerdefinierter Ressourcenanbieter, **Endpunkte** und benutzerdefinierte Ressourcen.

## <a name="how-to-build-custom-resource-providers"></a>Erstellen benutzerdefinierter Ressourcenanbieter

Benutzerdefinierte Ressourcenanbieter bestehen aus einer Liste von Verträgen zwischen Azure und Endpunkten. Der Vertrag beschreibt, wie Azure mit einem Endpunkt interagieren soll. Der Ressourcenanbieter fungiert als Proxy und leitet Anforderungen und Antworten an und vom angegebenen **Endpunkt** weiter. Ein Ressourcenanbieter kann zwei Arten von Verträgen angeben: [**resourceTypes**](./custom-providers-resources-endpoint-how-to.md) und [**actions**](./custom-providers-action-endpoint-how-to.md). Diese werden über Endpunktdefinitionen aktiviert. Eine Endpunktdefinition besteht aus drei Feldern: **name**, **routingType** und **endpoint**.

Beispielendpunkt:

```JSON
{
  "name": "{endpointDefinitionName}",
  "routingType": "Proxy",
  "endpoint": "https://{endpointURL}/"
}
```

Eigenschaft | Erforderlich | BESCHREIBUNG
---|---|---
name | *Ja* | Der Name der Endpunktdefinition. Azure macht diesen Namen über seine API unter „/subscriptions/{subscriptionId}/resourceGroups/{resourceGroupName}/providers/Microsoft.CustomProviders/<br>resourceProviders/{resourceProviderName}/{endpointDefinitionName}“ verfügbar.
routingType | *Nein* | Bestimmt den Typ des Vertrags mit **Endpunkt**. Falls nicht angegeben, wird standardmäßig „Proxy“ verwendet.
endpoint | *Ja* | Der Endpunkt, an den die Anforderungen geleitet werden. Hiermit werden die Antwort sowie alle Nebenwirkungen der Anforderung verarbeitet.

### <a name="building-custom-resources"></a>Erstellen benutzerdefinierter Ressourcen

**resourceTypes** beschreibt neue benutzerdefinierte Ressourcen, die Azure hinzugefügt werden. Diese machen einfache RESTful CRUD-Methoden verfügbar. Erfahren Sie mehr über das [Erstellen benutzerdefinierter Ressourcen](./custom-providers-resources-endpoint-how-to.md).

Beispiel eines benutzerdefinierten Ressourcenanbieters mit **resourceTypes**:

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
  "location": "eastus"
}
```

APIs, die Azure für das obige Beispiel hinzugefügt wurden:

HttpMethod | Beispiel-URI | BESCHREIBUNG
---|---|---
PUT | /subscriptions/{subscriptionId}/resourceGroups/{resourceGroupName}/<br>providers/Microsoft.CustomProviders/resourceProviders/{resourceProviderName}/<br>myCustomResources/{customResourceName}?api-version=2018-09-01-preview | Der Azure-REST-API-Aufruf zum Erstellen einer neuen Ressource.
DELETE | /subscriptions/{subscriptionId}/resourceGroups/{resourceGroupName}/<br>providers/Microsoft.CustomProviders/resourceProviders/{resourceProviderName}/<br>myCustomResources/{customResourceName}?api-version=2018-09-01-preview | Der Azure-REST-API-Aufruf zum Löschen einer vorhandenen Ressource.
GET | /subscriptions/{subscriptionId}/resourceGroups/{resourceGroupName}/<br>providers/Microsoft.CustomProviders/resourceProviders/{resourceProviderName}/<br>myCustomResources/{customResourceName}?api-version=2018-09-01-preview | Der Azure-REST-API-Aufruf zum Abrufen einer vorhandenen Ressource.
GET | /subscriptions/{subscriptionId}/resourceGroups/{resourceGroupName}/<br>providers/Microsoft.CustomProviders/resourceProviders/{resourceProviderName}/<br>myCustomResources?api-version=2018-09-01-preview | Der Azure-REST-API-Aufruf zum Abrufen der Liste vorhandener Ressourcen.

### <a name="building-custom-actions"></a>Erstellen benutzerdefinierter Aktionen

**actions** beschreibt neue Aktionen, die Azure hinzugefügt werden. Diese können zusätzlich zum Ressourcenanbieter verfügbar gemacht oder unter **resourceType** geschachtelt werden. Erfahren Sie mehr über das [Erstellen benutzerdefinierter Aktionen](./custom-providers-action-endpoint-how-to.md).

Beispiel eines benutzerdefinierten Ressourcenanbieters mit **actions**:

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
  "location": "eastus"
}
```

APIs, die Azure für das obige Beispiel hinzugefügt wurden:

HttpMethod | Beispiel-URI | BESCHREIBUNG
---|---|---
POST | /subscriptions/{subscriptionId}/resourceGroups/{resourceGroupName}/<br>providers/Microsoft.CustomProviders/resourceProviders/{resourceProviderName}/<br>myCustomAction?api-version=2018-09-01-preview | Der Azure-REST-API-Aufruf zum Aktivieren der Aktion.

## <a name="looking-for-help"></a>Wenn Hilfe benötigt wird

Wenn Sie Fragen zur Entwicklung benutzerdefinierter Azure-Ressourcenanbieter haben, stellen Sie sie auf [Stack Overflow](https://stackoverflow.com/questions/tagged/azure-custom-providers). Eine ähnliche Frage wurde möglicherweise bereits gestellt und beantwortet, weshalb Sie dies zunächst prüfen sollten, bevor Sie etwas posten. Fügen Sie das Tag ```azure-custom-providers``` hinzu, um schnell eine Antwort zu erhalten!

## <a name="next-steps"></a>Nächste Schritte

In diesem Artikel haben Sie Grundlegendes über benutzerdefiniert Anbieter gelernt. Im nächsten Artikel erstellen Sie einen benutzerdefinierten Anbieter.

- [Tutorial: Erstellen eines benutzerdefinierten Azure-Ressourcenanbieters und Bereitstellen benutzerdefinierter Ressourcen](./create-custom-provider.md)
- [How To: Hinzufügen benutzerdefinierter Aktionen zur Azure-REST-API](./custom-providers-action-endpoint-how-to.md)
- [How To: Hinzufügen benutzerdefinierter Ressourcen zur Azure-REST-API](./custom-providers-resources-endpoint-how-to.md)
