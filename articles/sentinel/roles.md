---
title: Berechtigungen in Azure Sentinel | Microsoft-Dokumentation
description: In diesem Artikel wird beschrieben, wie Azure Sentinel die rollenbasierte Zugriffssteuerung zum Zuweisen von Berechtigungen für Benutzer verwendet und die zulässigen Aktionen für jede Rolle identifiziert.
services: sentinel
cloud: na
documentationcenter: na
author: rkarlin
manager: angrobe
ms.assetid: ''
ms.service: azure-sentinel
ms.subservice: azure-sentinel
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: conceptual
ms.date: 09/26/2019
ms.author: rkarlin
ms.openlocfilehash: 0bf95b499a7366dad1e7b78fa4298aa6a42bb5fe
ms.sourcegitcommit: 9fba13cdfce9d03d202ada4a764e574a51691dcd
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 09/26/2019
ms.locfileid: "71316800"
---
# <a name="permissions-in-azure-sentinel"></a>Berechtigungen in Azure Sentinel

Azure Sentinel verwendet die  [rollenbasierte Zugriffssteuerung (Role-Based Access Control, RBAC)](../role-based-access-control/role-assignments-portal.md). Dabei werden [integrierte Rollen](../role-based-access-control/built-in-roles.md) bereitgestellt, die Benutzern, Gruppen und Diensten in Azure zugewiesen werden können.

Mit RBAC können Sie Rollen innerhalb Ihres Security Operations-Teams verwenden und erstellen, um einen angemessenen Zugriff auf Azure Sentinel zu ermöglichen. Basierend auf den Rollen haben Sie eine detaillierte Kontrolle darüber, was den Benutzern mit Zugriff auf Azure Sentinel angezeigt wird. Sie können RBAC-Rollen direkt im Azure Sentinel-Arbeitsbereich oder einem Abonnement oder einer Ressourcengruppe zuordnen, zu der der Arbeitsbereich gehört.

Es gibt drei spezielle integrierte Azure Sentinel-Rollen.  
**Alle in Azure Sentinel integrierten Rollen gewähren Lesezugriff auf die Daten in Ihrem Azure Sentinel-Arbeitsbereich.**
- **Azure Sentinel-Leser**: Ein Benutzer, der dieser Rolle zugeordnet ist, verfügt über Anzeigeberechtigungen für Azure Sentinel. Der Benutzer kann Incidents und Daten anzeigen, aber keine Änderungen vornehmen.
- **Azure Sentinel-Responder**: Ein Benutzer, der dieser Rolle zugewiesen ist, kann Aktionen zu Incidents wie Zuordnungs- und Schweregradänderungen lesen und durchführen.
- **Azure Sentinel-Mitwirkender**: Ein Benutzer, der dieser Rolle zugeordnet ist, kann Aktionen zu Incidents lesen und durchführen sowie Analyseregeln erstellen und löschen.

Zusätzlich zu den dedizierten RBAC-Rollen von Azure Sentinel gibt es RBAC-Rollen von Azure und Log Analytics, die eine größere Auswahl an Berechtigungen gewähren können, einschließlich des Zugriffs auf Ihren Azure Sentinel-Arbeitsbereich und auf andere Ressourcen:

- **Azure-Rollen:** [Besitzer](../role-based-access-control/built-in-roles.md#owner), [Mitwirkender](../role-based-access-control/built-in-roles.md#contributor) und [Leser](../role-based-access-control/built-in-roles.md#reader). Azure-Rollen gewähren Zugriff auf alle Ihre Azure-Ressourcen, einschließlich Log Analytics-Arbeitsbereiche und Azure Sentinel-Ressourcen.

-   **Log Analytics-Rollen:** [Log Analytics-Mitwirkender](../role-based-access-control/built-in-roles.md#log-analytics-contributor), [Log Analytics-Leser](../role-based-access-control/built-in-roles.md#log-analytics-reader). Log Analytics-Rollen gewähren Zugriff auf Ihre gesamten Log Analytics-Arbeitsbereiche. 

> [!NOTE]
> Log Analytics-Rollen gewähren auch Lesezugriff auf alle Azure-Ressourcen, weisen aber nur den Log Analytics-Ressourcen Schreibrechte zu.


Ein Benutzer, dem z. B. die Rollen **Azure Sentinel-Leser** und **Azure-Mitwirkender** (nicht **Azure Sentinel-Mitwirkender**) zugewiesen sind, kann Daten in Azure Sentinel bearbeiten, obwohl er nur über die Berechtigungen für **Sentinel-Leser** verfügt. Wenn Sie daher Berechtigungen nur in Azure Sentinel gewähren möchten, sollten Sie die vorherigen Berechtigungen dieses Benutzers sorgfältig entfernen und sicherstellen, dass Sie keine erforderliche Berechtigungsrolle für eine andere Ressource aufheben.

> [!NOTE]
>- Azure Sentinel verwendet Playbooks für die automatische Reaktion auf Bedrohungen. Playbooks nutzen Azure Logic Apps und sind eine separate Azure-Ressource. Sie können bestimmten Mitgliedern Ihres Security Operations-Teams die Option zuweisen, Logic Apps für SOAR-Vorgänge (Sicherheitsorchestrierung, Automatisierung und Reaktion) zu verwenden. Sie können die Rolle [Logik-App-Mitwirkender](../role-based-access-control/built-in-roles.md#logic-app-contributor) oder die Rolle [Logik-App-Operator](../role-based-access-control/built-in-roles.md#logic-app-operator) verwenden, um eine explizite Berechtigung für die Verwendung von Playbooks zu erteilen.
>- Um Dataconnectors hinzuzufügen, gelten die erforderlichen Rollen für die einzelnen Connectors pro Connectortyp und sind auf der jeweiligen Connectorseite aufgeführt. Darüber hinaus müssen Sie, um eine Verbindung mit einer beliebigen Datenquelle herstellen zu können, über die Schreibberechtigung für den Azure Sentinel-Arbeitsbereich verfügen.



## <a name="roles-and-allowed-actions"></a>Rollen und zulässige Aktionen

In der folgenden Tabelle sind die Rollen und zulässigen Aktionen in Azure Sentinel aufgeführt. Mit „X“ wird angegeben, dass die Aktion für die jeweilige Rolle zulässig ist.

| Role | Playbooks erstellen und ausführen| Dashboards, Analyseregeln und andere Azure Sentinel-Ressourcen erstellen und bearbeiten | Incidents verwalten (verwerfen, zuweisen usw.) | Daten, Incidents, Dashboards und andere Azure Sentinel-Ressourcen anzeigen |
|--- |---|---|---|---|
| Azure Sentinel-Leser | -- | -- | -- | X |
| Azure Sentinel-Responder|--|--| X | X |
| Azure Sentinel-Mitwirkender | -- | X | X | X |
| Azure Sentinel-Mitwirkender und Logik-App-Mitwirkender | X | X | X | X |


> [!NOTE]
> - Es empfiehlt sich, den Benutzern eine Rolle zuzuweisen, die jeweils nur so viele Berechtigungen umfasst wie für die Erfüllung ihrer Aufgaben erforderlich sind. Weisen Sie z. B. die Rolle „Azure Sentinel-Mitwirkender“ nur Benutzern zu, die Regeln oder Dashboards erstellen müssen.
> - Es wird empfohlen, die Berechtigungen für Azure Sentinel im Ressourcengruppenbereich festzulegen, damit der Benutzer Zugriff auf alle Azure Sentinel-Arbeitsbereiche derselben Ressourcengruppe hat.
>
## <a name="building-custom-rbac-roles"></a>Erstellen benutzerdefinierter RBAC-Rollen

Zusätzlich zu oder anstelle von integrierten RBAC-Rollen können Sie benutzerdefinierte RBAC-Rollen für Azure Sentinel erstellen. Benutzerdefinierte RBAC-Rollen für Azure Sentinel werden auf dieselbe Weise erstellt, wie Sie andere [benutzerdefinierte Azure RBAC](../role-based-access-control/custom-roles-rest.md#create-a-custom-role)-Rollen basierend auf bestimmten Berechtigungen für Azure Sentinel-Ressourcen erstellen.

## <a name="advanced-rbac-on-the-data-you-store-in-azure-sentinel"></a>Erweitertes RBAC für die Daten, die Sie in Azure Sentinel speichern.
  
Sie können die erweiterte rollenbasierte Zugriffssteuerung von Log Analytics für die Daten in Ihrem Azure Sentinel-Arbeitsbereich verwenden. Dies umfasst sowohl die rollenbasierte Zugriffssteuerung pro Datentyp als auch die ressourcenorientierte rollenbasierte Zugriffssteuerung. Weitere Informationen zu den Log Analytics-Rollen finden Sie unter  [Verwalten von Protokolldaten und Arbeitsbereichen in Azure Monitor](../azure-monitor/platform/manage-access.md#manage-access-using-workspace-permissions).

## <a name="next-steps"></a>Nächste Schritte
In diesem Dokument haben Sie erfahren, wie Sie mit Rollen für Azure Sentinel-Benutzer arbeiten und was die einzelnen Rollen den Benutzern ermöglichen.

* [Azure Security-Blog](https://blogs.msdn.com/b/azuresecurity/). Hier finden Sie Blogbeiträge zur Sicherheit und Compliance von Azure.
