---
title: Anzeigen von Aktivitäten und des Überwachungsverlaufs für Azure-Ressourcenrollen in PIM – Azure Active Directory | Microsoft-Dokumentation
description: Zeigen Sie Aktivitäten und den Überwachungsverlauf für Azure-Ressourcenrollen in Azure AD Privileged Identity Management (PIM) an.
services: active-directory
documentationcenter: ''
author: curtand
manager: daveba
editor: ''
ms.assetid: ''
ms.service: active-directory
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: identity
ms.subservice: pim
ms.date: 04/09/2019
ms.author: curtand
ms.collection: M365-identity-device-management
ms.openlocfilehash: 0c9a58fb6cd8591f571b4e65c3ab88cc59e38cda
ms.sourcegitcommit: 8074f482fcd1f61442b3b8101f153adb52cf35c9
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 10/22/2019
ms.locfileid: "72756328"
---
# <a name="view-activity-and-audit-history-for-azure-resource-roles-in-privileged-identity-management"></a>Anzeigen von Aktivitäten und des Überwachungsverlaufs für Azure-Ressourcenrollen in Privileged Identity Management

Mit Azure Active Directory (Azure AD) Privileged Identity Management (PIM) können Sie Aktivitäten, Aktivierungen und den Überwachungsverlauf für Azure-Ressourcenrollen innerhalb Ihrer Organisation anzeigen. Dies schließt Abonnements, Ressourcengruppen und sogar virtuelle Computer ein. Alle Ressourcen im Azure-Portal, die die Azure-Funktion für die rollenbasierte Zugriffssteuerung (Role Based Access Control, RBAC) nutzen, können von den Sicherheits- und Lebenszyklusverwaltungsfunktionen von Privileged Identity Management profitieren.

## <a name="view-activity-and-activations"></a>Anzeigen von Aktivitäten und Aktivierungen

Um zu sehen, welche Aktionen ein bestimmter Benutzer für verschiedene Ressourcen ausgeführt hat, können Sie die Azure-Ressourcenaktivität für einen bestimmten Aktivierungszeitraum anzeigen.

1. Öffnen Sie **Azure AD Privileged Identity Management**.

1. Klicken Sie auf **Azure-Ressourcen**.

1. Klicken Sie auf die Ressource, für die Sie Aktivitäten und Aktivierungen anzeigen möchten.

1. Klicken Sie auf **Rollen** oder **Mitglieder**.

1. Klicken Sie auf einen Benutzer.

    Daraufhin wird eine nach Datum aufgeschlüsselte Grafik mit den Aktionen des Benutzers in Azure-Ressourcen dargestellt. Außerdem sind die letzten Rollenaktivierungen im selben Zeitraum zu sehen.

    ![Benutzerdetails mit Zusammenfassung zur Ressourcenaktivität und Rollenaktivierungen](media/azure-pim-resource-rbac/rbac-user-details.png)

1. Klicken Sie auf eine bestimmte Rollenaktivierung, um Details und die entsprechende Azure-Ressourcenaktivität anzuzeigen, die ausgeführt wurde, während der Benutzer aktiv war.

    ![Rollenaktivierung ausgewählt und Aktivitätsdetails nach Datum angezeigt](media/azure-pim-resource-rbac/rbac-user-resource-activity.png)

## <a name="export-role-assignments-with-children"></a>Exportieren von Rollenzuweisungen mit untergeordneten Elementen

Angenommen, Sie müssen Prüfern aus Konformitätsgründen eine vollständige Liste mit allen Rollenzuweisungen zur Verfügung stellen. Mit Privileged Identity Management können Sie Rollenzuweisungen für eine bestimmte Ressource einschließlich der Rollenzuweisungen für alle untergeordneten Ressourcen abfragen. In der Vergangenheit war es für Administratoren nicht ganz einfach, eine vollständige Liste mit den Rollenzuweisungen für ein Abonnement zu erhalten, und die Rollenzuweisungen mussten für jede spezifische Ressource exportiert werden. Mit Privileged Identity Management können Sie alle aktiven und geeigneten Rollenzuweisungen in einem Abonnement einschließlich der Rollenzuweisungen für alle Ressourcengruppen und Ressourcen abfragen.

1. Öffnen Sie **Azure AD Privileged Identity Management**.

1. Klicken Sie auf **Azure-Ressourcen**.

1. Klicken Sie auf die Ressource, für die Sie Rollenzuweisungen exportieren möchten (beispielsweise ein Abonnement).

1. Klicken Sie auf **Mitglieder**.

1. Klicken Sie auf **Exportieren**, um den Bereich „Mitgliedschaft exportieren“ zu öffnen.

    ![Bereich „Mitgliedschaft exportieren“ zum Exportieren aller Mitglieder](media/azure-pim-resource-rbac/export-membership.png)

1. Klicken Sie auf **Export all members** (Alle Mitglieder exportieren), um alle Rollenzuweisungen in eine CSV-Datei zu exportieren.

    ![Exportierte Rollenzuweisungen in CSV-Datei wie in Excel angezeigt](media/azure-pim-resource-rbac/export-csv.png)

## <a name="view-resource-audit-history"></a>Anzeigen des Ressourcenüberwachungsverlaufs

Die Ressourcenüberwachung bietet einen Überblick über alle Rollenaktivitäten für eine Ressource.

1. Öffnen Sie **Azure AD Privileged Identity Management**.

1. Klicken Sie auf **Azure-Ressourcen**.

1. Klicken Sie auf die Ressource, für die Sie den Überwachungsverlauf anzeigen möchten.

1. Klicken Sie auf **Ressourcenüberwachung**.

1. Filtern Sie den Verlauf nach einem vordefinierten Datum oder nach einem benutzerdefinierten Bereich.

    ![Ressourcenüberwachungsliste mit Filtern](media/azure-pim-resource-rbac/rbac-resource-audit.png)

1. Wählen Sie unter **Überwachungstyp** die Option **Aktivieren** (zugewiesen und aktiviert) aus.

    ![Nach Überwachungstyp „Aktivieren“ gefilterte Ressourcenüberwachungsliste](media/azure-pim-resource-rbac/rbac-audit-activity.png)

1. Klicken Sie unter **Aktion** für einen Benutzer auf **(Aktivität)** , um Details zur Aktivität des Benutzers in Azure-Ressourcen anzuzeigen.

    ![Benutzeraktivitätsdetails für eine bestimmte Aktion](media/azure-pim-resource-rbac/rbac-audit-activity-details.png)

## <a name="view-my-audit"></a>Anzeigen der eigenen Überwachung

In der eigenen Überwachung können Sie Ihre persönliche Rollenaktivität anzeigen.

1. Öffnen Sie **Azure AD Privileged Identity Management**.

1. Klicken Sie auf **Azure-Ressourcen**.

1. Klicken Sie auf die Ressource, für die Sie den Überwachungsverlauf anzeigen möchten.

1. Klicken Sie auf **My audit** (Meine Überwachung).

1. Filtern Sie den Verlauf nach einem vordefinierten Datum oder nach einem benutzerdefinierten Bereich.

    ![Überwachungsliste für den aktuellen Benutzer](media/azure-pim-resource-rbac/my-audit-time.png)

## <a name="next-steps"></a>Nächste Schritte

- [Zuweisen von Azure-Ressourcenrollen in PIM](pim-resource-roles-assign-roles.md)
- [Genehmigen oder Ablehnen von Anforderungen für Azure-Ressourcenrollen in PIM](pim-resource-roles-approval-workflow.md)
- [Anzeigen des Überwachungsverlaufs für Azure AD-Rollen in PIM](pim-how-to-use-audit-log.md)
