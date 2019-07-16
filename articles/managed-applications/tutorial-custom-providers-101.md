---
title: Erstellen von benutzerdefinierten Aktionen und Ressourcen in Azure
description: In diesem Tutorial wird beschrieben, wie Sie benutzerdefinierte Aktionen und Ressourcen in Azure Resource Manager erstellen und in benutzerdefinierte Workflows für Azure Resource Manager-Vorlagen, Azure CLI, Azure Policy und Aktivitätsprotokolle integrieren.
author: jjbfour
ms.service: managed-applications
ms.topic: tutorial
ms.date: 06/19/2019
ms.author: jobreen
ms.openlocfilehash: 4bbfcf070611e3df5c0fe47070f2ab6961111e07
ms.sourcegitcommit: 66237bcd9b08359a6cce8d671f846b0c93ee6a82
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 07/11/2019
ms.locfileid: "67799189"
---
# <a name="create-custom-actions-and-resources-in-azure"></a>Erstellen von benutzerdefinierten Aktionen und Ressourcen in Azure

Benutzerdefinierte Anbieter ermöglichen Ihnen die Anpassung von Workflows in Azure. Ein benutzerdefinierter Anbieter ist ein Vertrag zwischen Azure und einem Endpunkt (`endpoint`). Er ermöglicht das Hinzufügen von neuen benutzerdefinierten APIs in Azure Resource Manager, um neue Bereitstellungs- und Verwaltungsfunktionen zu nutzen. In diesem Tutorial wird anhand eines einfachen Beispiels beschrieben, wie Sie Azure neue Aktionen und Ressourcen hinzufügen und diese integrieren.

Dieses Tutorial ist in die folgenden Schritte unterteilt:

- Einrichten von Azure-Funktionen für benutzerdefinierte Azure-Anbieter
- Erstellen eines RESTful-Endpunkts für benutzerdefinierte Anbieter
- Erstellen und Verwenden des benutzerdefinierten Anbieters

## <a name="setup-azure-functions-for-azure-custom-providers"></a>Einrichten von Azure-Funktionen für benutzerdefinierte Azure-Anbieter

In diesem Teil des Tutorials wird ausführlich beschrieben, wie Sie eine Azure-Funktion für benutzerdefinierte Anbieter einrichten. Für benutzerdefinierte Anbieter kann eine beliebige öffentliche URL verwendet werden.

- [Einrichten von Azure-Funktionen für benutzerdefinierte Azure-Anbieter](./tutorial-custom-providers-function-setup.md)

## <a name="authoring-a-restful-endpoint-for-custom-providers"></a>Erstellen eines RESTful-Endpunkts für benutzerdefinierte Anbieter

In diesem Teil des Tutorials wird die Erstellung eines RESTful-Endpunkts für benutzerdefinierte Anbieter ausführlich beschrieben.

- [Erstellen eines RESTful-Endpunkts für benutzerdefinierte Anbieter](./tutorial-custom-providers-function-authoring.md)

## <a name="creating-and-utilizing-the-custom-provider"></a>Erstellen und Verwenden des benutzerdefinierten Anbieters

In diesem Teil des Tutorials wird ausführlich beschrieben, wie Sie einen benutzerdefinierten Anbieter erstellen und seine benutzerdefinierten Aktionen und Ressourcen nutzen.

- [Erstellen und Nutzen eines benutzerdefinierten Azure-Anbieters](./tutorial-custom-providers-create.md)

## <a name="next-steps"></a>Nächste Schritte

In diesem Artikel wurden benutzerdefinierte Anbieter und deren Erstellung beschrieben. Fahren Sie mit dem nächsten Schritt des Tutorials fort:

- [Tutorial: Einrichten von Azure-Funktionen für benutzerdefinierte Azure-Anbieter](./tutorial-custom-providers-function-setup.md)

Hier sind einige nützliche Links zu Referenzen und Schnellstartanleitungen angegeben:

- [Schnellstart: Erstellen eines benutzerdefinierten Azure-Ressourcenanbieters und Bereitstellen benutzerdefinierter Ressourcen](./create-custom-provider.md)
- [How To: Hinzufügen benutzerdefinierter Aktionen zur Azure-REST-API](./custom-providers-action-endpoint-how-to.md)
- [How To: Hinzufügen benutzerdefinierter Ressourcen zur Azure-REST-API](./custom-providers-resources-endpoint-how-to.md)
