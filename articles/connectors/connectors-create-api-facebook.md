---
title: Herstellen einer Verbindung mit Facebook – Azure Logic Apps
description: Verwalten Ihrer Chronik und Ihrer Seite mit Facebook-REST-APIs und Azure Logic Apps
services: logic-apps
ms.service: logic-apps
ms.suite: integration
author: ecfan
ms.author: estfan
manager: carmonm
ms.reviewer: klam, LADocs
ms.topic: conceptual
ms.date: 11/07/2016
tags: connectors
ms.openlocfilehash: 83431184d7e9c5970ece6af143ee9b5166da96d5
ms.sourcegitcommit: bba811bd615077dc0610c7435e4513b184fbed19
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 08/27/2019
ms.locfileid: "70050956"
---
# <a name="manage-your-facebook-timeline-and-page-by-using-azure-logic-apps"></a>Verwalten Ihrer Facebook-Chronik und -Seite mithilfe von Azure Logic Apps

Verbinden Sie sich mit Facebook, um z. B. die Chronik zu ergänzen oder einen Seitenfeed abzurufen. Facebook ermöglicht Folgendes:

* Erstellen eines Geschäftsworkflows basierend auf den Daten, die aus Facebook abgerufen werden. 
* Verwenden eines Triggers, wenn ein neuer Beitrag empfangen wird.
* Verwenden von Aktionen, die z. B. die Chronik ergänzen oder einen Seitenfeed abrufen. Diese Aktionen erhalten eine Antwort und stellen anschließend die Ausgabe anderen Aktionen zur Verfügung. Wenn ein neuer Beitrag in Ihrer Chronik vorhanden ist, können Sie diesen Beitrag per Push an Ihren Twitter-Feed übertragen. 

Erstellen Sie zu Beginn eine Logik-App, wie unter [Erstellen einer Logik-App](../logic-apps/quickstart-create-first-logic-app-workflow.md) beschrieben.

## <a name="create-a-connection-to-facebook"></a>Herstellen einer Verbindung mit Facebook

Wenn Sie diesen Connector Ihren Logik-Apps hinzufügen, müssen Sie ihnen das Herstellen einer Verbindung mit Ihrem Facebook-Konto erlauben.

1. Melden Sie sich bei Ihrem Facebook-Konto an.

2. Wählen Sie **Autorisieren**aus, um zu erlauben, dass Ihre Logik-Apps sich mit Ihrem Facebook-Konto verbinden und dieses nutzen. 

> [!INCLUDE [Steps to create a connection to Facebook](../../includes/connectors-create-api-facebook.md)]
> 

## <a name="connector-reference"></a>Connector-Referenz

Technische Details wie Trigger, Aktionen und Limits, wie sie in der OpenAPI-Datei (ehemals Swagger) des Connectors beschrieben werden, finden Sie auf der [Referenzseite des Connectors](/connectors/facebook/).

## <a name="next-steps"></a>Nächste Schritte

* Informationen zu anderen [Logic Apps-Connectors](../connectors/apis-list.md)