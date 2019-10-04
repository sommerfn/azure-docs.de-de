---
title: Erstellen von benutzerdefinierten Aktionen und Ressourcen in Azure
description: In diesem Tutorial wird erläutert, wie Sie benutzerdefinierte Aktionen und Ressourcen in Azure Resource Manager erstellen. Außerdem wird gezeigt, wie benutzerdefinierte Workflows Interoperabilität mit Azure Resource Manager-Vorlagen, der Azure CLI, Azure Policy und dem Azure-Aktivitätsprotokoll ermöglichen.
author: jjbfour
ms.service: managed-applications
ms.topic: tutorial
ms.date: 06/19/2019
ms.author: jobreen
ms.openlocfilehash: dc1601e344c371a5f0feaadd272a2c6ff40af031
ms.sourcegitcommit: f2771ec28b7d2d937eef81223980da8ea1a6a531
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 09/20/2019
ms.locfileid: "71172938"
---
# <a name="create-custom-actions-and-resources-in-azure"></a>Erstellen von benutzerdefinierten Aktionen und Ressourcen in Azure

Ein benutzerdefinierter Anbieter ist ein Vertrag zwischen Azure und einem Endpunkt. Mit benutzerdefinierten Anbietern können Sie Workflows in Azure ändern, indem Sie neue APIs in Azure Resource Manager hinzufügen. Dank dieser benutzerdefinierten APIs kann Resource Manager neue Bereitstellungs- und Verwaltungsfunktionen verwenden.

In diesem Tutorial wird anhand eines einfachen Beispiels beschrieben, wie Sie Azure neue Aktionen und Ressourcen hinzufügen und diese integrieren.

## <a name="set-up-azure-functions-for-azure-custom-providers"></a>Einrichten von Azure-Funktionen für benutzerdefinierte Azure-Anbieter

In Teil 1 dieses Tutorials wird beschrieben, wie Sie eine Azure-Funktions-App für die Verwendung mit benutzerdefinierten Anbietern einrichten:

- [Einrichten von Azure-Funktionen für benutzerdefinierte Azure-Anbieter](./tutorial-custom-providers-function-setup.md)

Für benutzerdefinierte Anbieter kann eine beliebige öffentliche URL verwendet werden.

## <a name="author-a-restful-endpoint-for-custom-providers"></a>Erstellen eines RESTful-Endpunkts für benutzerdefinierte Anbieter

In Teil 2 dieses Tutorials wird beschrieben, wie Sie einen RESTful-Endpunkt für benutzerdefinierte Anbieter erstellen:

- [Erstellen eines RESTful-Endpunkts für benutzerdefinierte Anbieter](./tutorial-custom-providers-function-authoring.md)

## <a name="create-and-use-a-custom-provider"></a>Erstellen und Verwenden eines benutzerdefinierten Anbieters

In Teil 3 dieses Tutorials wird beschrieben, wie Sie einen benutzerdefinierten Anbieter erstellen und seine benutzerdefinierten Aktionen und Ressourcen nutzen:

- [Erstellen und Verwenden eines benutzerdefinierten Anbieters](./tutorial-custom-providers-create.md)

## <a name="next-steps"></a>Nächste Schritte

In diesem Tutorial wurden benutzerdefinierte Anbieter und deren Erstellung beschrieben. Fahren Sie mit dem nächsten Tutorial fort: [Tutorial: Einrichten von Azure-Funktionen für benutzerdefinierte Azure-Anbieter](./tutorial-custom-providers-function-setup.md).

Hier sind einige nützliche Links zu Referenzen und Schnellstartanleitungen:

- [Schnellstart: Erstellen eines benutzerdefinierten Azure-Ressourcenanbieters und Bereitstellen benutzerdefinierter Ressourcen](./create-custom-provider.md)
- [Gewusst wie: Hinzufügen benutzerdefinierter Aktionen zur Azure-REST-API](./custom-providers-action-endpoint-how-to.md)
- [Gewusst wie: Hinzufügen benutzerdefinierter Ressourcen zur Azure-REST-API](./custom-providers-resources-endpoint-how-to.md)
