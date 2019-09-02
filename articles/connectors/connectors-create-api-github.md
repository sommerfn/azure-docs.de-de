---
title: Herstellen einer Verbindung mit GitHub – Azure Logic Apps
description: Überwachen von GitHub-Ereignissen mit GitHub-REST-APIs und Azure Logic Apps
services: logic-apps
ms.service: logic-apps
ms.suite: integration
author: ecfan
ms.author: estfan
manager: carmonm
ms.reviewer: klam, LADocs
ms.topic: conceptual
ms.date: 03/02/2018
tags: connectors
ms.openlocfilehash: 6a6e2a803ee2a272189abf0f21796b2305eea40b
ms.sourcegitcommit: bba811bd615077dc0610c7435e4513b184fbed19
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 08/27/2019
ms.locfileid: "70050892"
---
# <a name="connect-to-github-from-azure-logic-apps"></a>Herstellen einer Verbindung mit GitHub aus Azure Logic Apps

GitHub ist ein webbasierter Hostingdienst für Git-Repositorys. Darin finden Sie die gesamte verteilte Versionskontroll- und SCM-Funktionalität (Source Code Management, Quellcodeverwaltung) in Git sowie weitere Features.

Um mit der Verwendung des GitHub-Connectors loszulegen, [erstellen Sie zunächst eine Logik-App](../logic-apps/quickstart-create-first-logic-app-workflow.md).

## <a name="create-a-connection-to-github"></a>Herstellen einer Verbindung mit GitHub

Um den GitHub-Connector in einer Logik-App verwenden zu können, müssen Sie zuerst eine *Verbindung* erstellen, und dann Details für diese Eigenschaften angeben: 

| Eigenschaft | Erforderlich | BESCHREIBUNG | 
| -------- | -------- | ----------- | 
| Tokenverschlüsselung | Ja | Geben Sie Ihre Anmeldeinformationen für GitHub an. |

Nachdem Sie die Verbindung erstellt haben, können Sie die Aktionen ausführen und auf die in diesem Artikel beschriebenen Trigger lauschen.

> [!INCLUDE [Steps to create a connection to GitHub](../../includes/connectors-create-api-github.md)]
> 

## <a name="connector-reference"></a>Connector-Referenz

Technische Details zu Triggern, Aktionen und Beschränkungen aus der OpenAPI-Beschreibung (ehemals Swagger) des Connectors finden Sie auf der [Referenzseite des Connectors](/connectors/github/).

## <a name="next-steps"></a>Nächste Schritte

* Informationen zu anderen [Logic Apps-Connectors](../connectors/apis-list.md)