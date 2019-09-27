---
title: Berechtigungen in Azure Security Center | Microsoft-Dokumentation
description: In diesem Artikel wird beschrieben, wie Azure Security Center die rollenbasierte Zugriffssteuerung zum Zuweisen von Berechtigungen für Benutzer verwendet und die zulässigen Aktionen für jede Rolle identifiziert.
services: security-center
cloud: na
documentationcenter: na
author: memildin
manager: rkarlin
ms.assetid: ''
ms.service: security-center
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: conceptual
ms.date: 10/28/2018
ms.author: memildin
ms.openlocfilehash: f52d518d2ed1dfb62eed72cf9c0b839a37b7f856
ms.sourcegitcommit: 8a717170b04df64bd1ddd521e899ac7749627350
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 09/23/2019
ms.locfileid: "71201641"
---
# <a name="permissions-in-azure-security-center"></a>Berechtigungen in Azure Security Center

Azure Security Center verwendet die [rollenbasierte Zugriffssteuerung (Role-Based Access Control, RBAC)](../role-based-access-control/role-assignments-portal.md). Diese stellt [integrierte Rollen](../role-based-access-control/built-in-roles.md) bereit, die Benutzern, Gruppen und Diensten in Azure zugewiesen werden können.

Security Center bewertet die Konfiguration Ihrer Ressourcen, um die Sicherheitsprobleme und Sicherheitsrisiken zu identifizieren. In Security Center werden Ihnen nur dann Informationen zu einer Ressource angezeigt, wenn Ihnen für das Abonnement oder die Ressourcengruppe, der eine Ressource angehört, die Rolle „Besitzer“, „Mitwirkender“ oder „Leser“ zugewiesen ist.

Neben diesen Rollen gibt es zwei spezifische Security Center-Rollen:

* **Sicherheitsleseberechtigter**: Ein Benutzer, der dieser Rolle angehört, verfügt über Anzeigerechte für Security Center. Der Benutzer kann Empfehlungen, Warnungen, Sicherheitsrichtlinien und Sicherheitszustände anzeigen, jedoch keine Änderungen vornehmen.
* **Sicherheitsadministrator**: Ein Benutzer, der dieser Rolle angehört, hat die gleichen Rechte wie der Sicherheitsleseberechtigte und kann darüber hinaus die Sicherheitsrichtlinie aktualisieren sowie Warnungen und Empfehlungen verwerfen.

> [!NOTE]
> Die beiden Sicherheitsrollen (Sicherheitsleseberechtigter und Sicherheitsadministrator) haben nur Zugriff in Security Center. Die Sicherheitsrollen haben keinen Zugriff auf andere Dienstbereiche von Azure wie Storage, Web & Mobile oder Internet der Dinge (IoT).
>
>

## <a name="roles-and-allowed-actions"></a>Rollen und zulässige Aktionen

In der folgenden Tabelle sind die Rollen und zulässigen Aktionen von Security Center aufgeführt. Mit „X“ wird angegeben, dass die Aktion für die jeweilige Rolle zulässig ist.

| Role | Sicherheitsrichtlinie bearbeiten | Sicherheitsempfehlungen für eine Ressource anwenden | Warnungen und Empfehlungen verwerfen | Warnungen und Empfehlungen anzeigen |
|:--- |:---:|:---:|:---:|:---:|
| Besitzer des Abonnements | X | X | X | X |
| Mitwirkender des Abonnements | -- | X | X | X |
| Ressourcengruppenbesitzer | -- | X | -- | X |
| Ressourcengruppenmitwirkender | -- | X | -- | X |
| Leser | -- | -- | -- | X |
| Sicherheitsadministrator | X | -- | X | X |
| Sicherheitsleseberechtigter | -- | -- | -- | X |

> [!NOTE]
> Es empfiehlt sich, den Benutzern eine Rolle zuzuweisen, die jeweils nur so viele Berechtigungen umfasst wie für die Erfüllung ihrer Aufgaben erforderlich sind. Weisen Sie die Rolle „Leser“ etwa Benutzern zu, die nur Informationen zur Sicherheitsintegrität einer Ressource anzeigen, aber keine Aktionen durchführen müssen (also beispielsweise keine Empfehlungen umsetzen oder Richtlinien bearbeiten).
>
>

## <a name="next-steps"></a>Nächste Schritte
In diesem Artikel wurde beschrieben, wie Security Center die rollenbasierte Zugriffssteuerung (RBAC) zum Zuweisen von Berechtigungen für Benutzer nutzt und welche Aktionen für die einzelnen Rollen zulässig sind. Informieren Sie sich als Nächstes über Folgendes, nachdem Sie sich mit den Rollenzuweisungen zum Überwachen des Sicherheitszustands Ihres Abonnements, Bearbeiten von Sicherheitsrichtlinien und Anwenden von Empfehlungen vertraut gemacht haben:

- [Festlegen von Sicherheitsrichtlinien in Azure Security Center](tutorial-security-policy.md)
- [Verwalten von Sicherheitsempfehlungen in Azure Security Center](security-center-recommendations.md)
- [Überwachen der Sicherheitsintegrität in Azure Security Center](security-center-monitoring.md)
- [Verwalten von und Reagieren auf Sicherheitswarnungen in Azure Security Center](security-center-managing-and-responding-alerts.md)
- [Überwachen von Partnerlösungen mit Azure Security Center](security-center-partner-solutions.md)
