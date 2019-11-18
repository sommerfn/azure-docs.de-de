---
title: Ressourcenonboarding mit benutzerdefinierten Azure-Anbietern
description: Erfahren Sie mehr über das Ressourcenonboarding, mit benutzerdefinierten Azure-Anbietern, um Verwaltungsfunktionen oder die Konfiguration auf andere Azure-Ressourcentypen anzuwenden.
author: jjbfour
ms.service: managed-applications
ms.topic: conceptual
ms.date: 09/06/2019
ms.author: jobreen
ms.openlocfilehash: 65e0f795a2b0efa327aec02c01cdb3706bf91d29
ms.sourcegitcommit: ac56ef07d86328c40fed5b5792a6a02698926c2d
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 11/08/2019
ms.locfileid: "73818780"
---
# <a name="azure-custom-providers-resource-onboarding-overview"></a>Ressourcenonboarding mit benutzerdefinierten Azure-Anbietern – Übersicht

Das Ressourcenonboarding für benutzerdefinierte Azure-Anbietern ist ein Erweiterbarkeitsmodell für Azure-Ressourcentypen. Damit können Sie Vorgänge oder Verwaltungsfunktionen im gewünschten Umfang auf vorhandene Azure-Ressourcen anwenden. Weitere Informationen finden Sie unter [Erweitern von Azure mit benutzerdefinierten Azure-Anbietern](./custom-providers-overview.md). Dieser Artikel beschreibt Folgendes:

- Welche Möglichkeiten haben Sie mit dem Ressourcenonboarding?
- Grundlagen des Ressourcenonboardings und dessen Verwendung.
- Auffinden von Anleitungen und Codebeispielen für den Einstieg

> [!IMPORTANT]
> Die benutzerdefinierten Anbieter sind derzeit als öffentliche Vorschauversion verfügbar.
> Diese Vorschauversion wird ohne Vereinbarung zum Servicelevel bereitgestellt und ist nicht für Produktionsworkloads vorgesehen. Manche Features werden möglicherweise nicht unterstützt oder sind nur eingeschränkt verwendbar.
> Weitere Informationen finden Sie unter [Zusätzliche Nutzungsbestimmungen für Microsoft Azure-Vorschauen](https://azure.microsoft.com/support/legal/preview-supplemental-terms/).

## <a name="what-can-resource-onboarding-do"></a>Welche Möglichkeiten haben Sie mit dem Ressourcenonboarding?

Ähnlich wie bei [benutzerdefinierten Ressourcen von benutzerdefinierten Azure-Anbietern](./custom-providers-resources-endpoint-how-to.md) wird beim Ressourcenonboarding ein Vertrag definiert, der Onboardinganforderungen per Proxy an einen Endpunkt weiterleitet. Aber im Gegensatz zu benutzerdefinierten Ressourcen wird durch das Onboarding von Ressource kein neuer Ressourcentyp erstellt. Stattdessen können vorhandene Ressourcentypen erweitert werden. Darüber hinaus funktioniert das Ressourcenonboarding mit Azure Policy, sodass die Verwaltung und die Konfiguration von Ressourcen im gewünschten Umfang erfolgen kann. Einige Beispiele für Workflows für das Ressourcenonboarding:

- Installieren und Verwalten von VM-Erweiterungen.
- Hochladen und Konfigurieren von Standardeinstellungen in Azure Storage-Konten.
- Aktivieren von grundlegenden Diagnoseeinstellungen im gewünschten Umfang.

## <a name="resource-onboarding-basics"></a>Grundlagen des Ressourcenonboardings

Sie konfigurieren das Ressourcenonboarding über benutzerdefinierte Azure-Anbieter mit den Ressourcentypen „Microsoft.CustomProviders/resourceProviders“ und „Microsoft.CustomProviders/associations“. Um das Ressourcenonboarding für einen benutzerdefinierten Anbieter zu ermöglichen, erstellen Sie während der Konfiguration einen **resourceType** mit der Bezeichnung „associations“ mit einem **resourceType**, der „Extension“ enthält. Die Ressourcentypen „Microsoft.CustomProviders/resourceProviders“ und „Microsoft.CustomProviders/associations“ müssen nicht zur selben Ressourcengruppe gehören.

Hier ist ein Beispiel für einen benutzerdefinierten Azure-Anbieter:

```JSON
{
  "properties": {
    "resourceTypes": [
      {
        "name": "associations",
        "routingType": "Proxy,Cache,Extension",
        "endpoint": "https://microsoft.com/"
      }
    ]
  },
  "location": "eastus"
}
```

Eigenschaft | Erforderlich? | BESCHREIBUNG
---|---|---
name | Ja | Der Name der Endpunktdefinition. Für das Ressourcenonboarding muss der Name „associations“ lauten.
routingType | Ja | Bestimmt den Typ des Vertrags mit dem Endpunkt. Gültige **routingTypes** für das Ressourcenonboarding sind „Proxy,Cache,Extension“ und „Webhook,Cache,Extension“.
endpoint | Ja | Der Endpunkt, an den die Anforderungen geleitet werden. Hiermit werden die Antwort sowie alle Nebenwirkungen der Anforderung verarbeitet.

Nachdem Sie den benutzerdefinierten Anbieter mit dem Ressourcentyp „associations“ erstellt haben, können Sie das Ziel mithilfe von „Microsoft.CustomProviders/associations“ konfigurieren. „Microsoft.CustomProviders/associations“ ist eine Erweiterungsressource zur Erweiterung jeder beliebigen Azure-Ressource. Wenn eine Instanz von „Microsoft.CustomProviders/associations“ erstellt wird, verwendet diese die Eigenschaft **targetResourceId**. Das muss eine gültig Ressourcen-ID von „Microsoft.CustomProviders/resourceProviders“ oder „Microsoft.Solutions/applications“ sein. In diesen Fällen wird die Anforderung an den Ressourcentyp „associations“ auf der von Ihnen erstellen Instanz von „Microsoft.CustomProviders/resourceProviders“ weitergeleitet.

> [!NOTE]
> Wenn eine Ressourcen-ID von „Microsoft.Solutions/applications“ als **targetResourceId** angegeben wird, muss eine Instanz von „Microsoft.CustomProviders/resourceProviders“ in der verwalteten Ressourcengruppe mit dem Namen „public“ bereitgestellt werden.

Beispielzuordnung für benutzerdefinierte Azure-Anbieter:

```JSON
{
  "properties": {
    "targetResourceId": "/subscriptions/{subscriptionId}/resourceGroups/{resourceGroupName}/providers/Microsoft.CustomProviders/resourceProviders/{resourceProviderName}",
    ...
  }
}
```

Eigenschaft | Erforderlich? | BESCHREIBUNG
---|---|---
targetResourceId | Ja | Die Ressourcen-ID von „Microsoft.CustomProviders/resourceProviders“ oder „Microsoft.Solutions/applications“.

## <a name="how-to-use-resource-onboarding"></a>Verwenden des Ressourcenonboardings

Beim Ressourcenonboarding werden andere Ressourcen mit der Erweiterungsressource „Microsoft.CustomProviders/associations“ erweitert. Im folgenden Beispiel wird die Anforderung für eine VM erstellt, es können aber beliebige Ressourcen erweitert werden.

Zunächst müssen Sie eine benutzerdefinierte Anbieterressource mit einem Ressourcentyp „associations“ erstellen. Dadurch wird die Rückruf-URL deklariert, die beim Erstellen einer dazugehörigen Ressource „Microsoft.CustomProviders/associations“ verwendet wird, die den benutzerdefinierten Anbieter definiert.

Beispielanforderungen zum Erstellen von „Microsoft.CustomProviders/resourceProviders“:

``` HTTP
PUT https://management.azure.com/subscriptions/{subscriptionId}/resourceGroups/{resourceGroupName}/providers/Microsoft.CustomProviders/resourceProviders/{resourceProviderName}?api-version=2018-09-01-preview
Authorization: Bearer eyJ0e...
Content-Type: application/json

{
  "properties": {
    "resourceTypes": [
      {
        "name": "associations",
        "routingType": "Proxy,Cache,Extension",
        "endpoint": "https://{myCustomEndpoint}/"
      }
    ]
  },
  "location": "{location}"
}
```

Nachdem Sie den benutzerdefinierten Anbieter erstellt haben, können Sie andere Ressourcen als Ziel konfigurieren und die Nebenwirkungen des benutzerdefinierten Anbieters darauf anwenden.

Beispielanforderungen zum Erstellen von „Microsoft.CustomProviders/associations“:

``` HTTP
PUT https://management.azure.com/subscriptions/{subscriptionId}/resourceGroups/{resourceGroupName}/providers/Microsoft.Compute/virtualMachines/{virtualMachineName}/providers/Microsoft.CustomProviders/associations/{associationName}?api-version=2018-09-01-preview
Authorization: Bearer eyJ0e...
Content-Type: application/json

{
  "properties": {
    "targetResourceId": "/subscriptions/{subscriptionId}/resourceGroups/{resourceGroupName}/providers/Microsoft.CustomProviders/resourceProviders/{resourceProviderName}",
    "myProperty1": "myPropertyValue1",
    "myProperty2": {
        "myProperty3" : "myPropertyValue3"
    }
  }
}
```

Die Anforderung wird dann an den Endpunkt weitergeleitet, der im von Ihnen erstellten benutzerdefinierten Anbieter angegeben ist. Dieser wird von **targetResourceId** in dieser Form referenziert wird:

``` HTTP
PUT https://{endpointURL}/?api-version=2018-09-01-preview
Content-Type: application/json
X-MS-CustomProviders-RequestPath: /subscriptions/{subscriptionId}/resourceGroups/{resourceGroupName}/providers/Microsoft.CustomProviders/resourceProviders/{resourceProviderName}/associations/{associationName}
X-MS-CustomProviders-ExtensionPath: /subscriptions/{subscriptionId}/resourceGroups/{resourceGroupName}/providers/Microsoft.Compute/virtualMachines/{virtualMachineName}/providers/Microsoft.CustomProviders/associations/{associationName}
X-MS-CustomProviders-ExtendedResource: /subscriptions/{subscriptionId}/resourceGroups/{resourceGroupName}/providers/Microsoft.Compute/virtualMachines/{virtualMachineName}

{
  "properties": {
    "myProperty1": "myPropertyValue1",
    "myProperty2": {
        "myProperty3" : "myPropertyValue3"
    }
  }
}
```

Der Endpunkt muss den `Content-Type` „application/json“ und einen gültigen JSON-Antworttext zurückgeben. Felder, die im JSON-Objekt **properties** zurückgegeben werden, werden zur Rückantwort von „association“ hinzugefügt.

## <a name="getting-help"></a>Hilfe

Wenn Sie Fragen zur Entwicklung benutzerdefinierter Azure-Ressourcenanbieter haben, stellen Sie sie auf [Stack Overflow](https://stackoverflow.com/questions/tagged/azure-custom-providers). Eine ähnliche Frage wurde möglicherweise bereits gestellt und beantwortet, weshalb Sie dies zunächst prüfen sollten, bevor Sie etwas posten. Fügen Sie das Tag ```azure-custom-providers``` hinzu, um schnell eine Antwort zu erhalten!

## <a name="next-steps"></a>Nächste Schritte

In diesem Artikel haben Sie Grundlegendes über benutzerdefiniert Anbieter gelernt. Weitere Informationen finden Sie in diesen Artikeln:

- [Tutorial: Ressourcenonboarding mit benutzerdefinierten Anbietern](./tutorial-custom-providers-resource-onboarding.md)
- [Tutorial: Erstellen von benutzerdefinierten Aktionen und Ressourcen in Azure](./tutorial-custom-providers-101.md)
- [Schnellstart: Erstellen eines benutzerdefinierten Ressourcenanbieters und Bereitstellen benutzerdefinierter Ressourcen](./create-custom-provider.md)
- [Gewusst wie: Hinzufügen benutzerdefinierter Aktionen zur Azure-REST-API](./custom-providers-action-endpoint-how-to.md)
- [Gewusst wie: Hinzufügen benutzerdefinierter Ressourcen zur Azure-REST-API](./custom-providers-resources-endpoint-how-to.md)
