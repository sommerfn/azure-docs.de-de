---
title: Ermitteln von Azure-Ressourcen zur Verwaltung in PIM – Azure Active Directory | Microsoft-Dokumentation
description: Erfahren Sie, wie Sie Azure-Ressourcen zur Verwaltung in Azure AD Privileged Identity Management (PIM) ermitteln.
services: active-directory
documentationcenter: ''
author: curtand
manager: mtillman
ms.service: active-directory
ms.devlang: na
ms.topic: conceptual
ms.tgt_pltfrm: na
ms.workload: identity
ms.subservice: pim
ms.date: 04/09/2019
ms.author: curtand
ms.collection: M365-identity-device-management
ms.openlocfilehash: ef1f1712ab7474747829c8744a39e74b78777668
ms.sourcegitcommit: 95b180c92673507ccaa06f5d4afe9568b38a92fb
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 09/08/2019
ms.locfileid: "70804169"
---
# <a name="discover-azure-resources-to-manage-in-pim"></a>Ermitteln von Azure-Ressourcen zur Verwaltung in PIM

Mit Azure Active Directory (Azure AD) Privileged Identity Management (PIM) können Sie den Schutz Ihrer Azure-Ressourcen verbessern. Dies ist hilfreich für Organisationen, die PIM bereits zum Schutz von Azure AD-Rollen verwenden, sowie für Verwaltungsgruppen- und Abonnementbesitzer, die Produktionsressourcen schützen möchten.

Beim erstmaligen Einrichten von PIM für Azure-Ressourcen müssen Sie die Ressourcen ermitteln und auswählen, die mit PIM geschützt werden sollen. Die Anzahl der Ressourcen, die Sie mit PIM verwalten können, ist nicht beschränkt. Es empfiehlt sich aber, mit Ihren wichtigsten Ressourcen (für die Produktion) zu beginnen.

## <a name="discover-resources"></a>Ermitteln von Ressourcen

1. Melden Sie sich beim [Azure-Portal](https://portal.azure.com/) an.

1. Öffnen Sie **Azure AD Privileged Identity Management**.

1. Klicken Sie auf **Azure-Ressourcen**.

    Falls Sie PIM für Azure-Ressourcen zum ersten Mal verwenden, wird der Bereich „Ressourcen ermitteln“ angezeigt.

    ![Bereich „Ressourcen ermitteln“ ohne aufgeführte Ressourcen bei der ersten Verwendung](./media/pim-resource-roles-discover-resources/discover-resources-first-run.png)

    Wenn ein anderer Ressourcen- oder Verzeichnisadministrator in Ihrer Organisation bereits Azure-Ressourcen in PIM verwaltet, sehen Sie eine Liste der Ressourcen, die derzeit verwaltet werden.

    ![Bereich „Ressourcen ermitteln“ mit einer Auflistung der derzeit verwalteten Ressourcen](./media/pim-resource-roles-discover-resources/discover-resources.png)

1. Klicken Sie auf **Ressourcen ermitteln**, um den Ermittlungsvorgang zu starten.

    ![Bereich „Ermittlung“ mit einer Auflistung von verwaltbaren Ressourcen wie Abonnements und Verwaltungsgruppen](./media/pim-resource-roles-discover-resources/discovery-pane.png)

1. Verwenden Sie im Bereich „Ermittlung“ **Ressourcenzustandsfilter** und **Ressourcentyp auswählen** zum Filtern der Verwaltungsgruppen oder Abonnements, für die Sie eine Schreibberechtigung haben. An einfachsten ist es wohl, mit **Alle** zu starten.

    Sie können nur Verwaltungsgruppen oder Abonnementressourcen suchen und auswählen, um sie mithilfe von PIM zu verwalten. Wenn Sie eine Verwaltungsgruppe oder ein Abonnement in PIM verwalten, können Sie auch deren untergeordnete Ressourcen verwalten.

1. Fügen Sie ein Häkchen neben nicht verwalteten Ressourcen hinzu, die Sie verwalten möchten.

1. Klicken Sie auf **Ressource verwalten**, um mit dem Verwalten der ausgewählten Ressourcen zu beginnen.

    > [!NOTE]
    > Sobald eine Verwaltungsgruppe oder ein Abonnement auf „Verwaltet“ festgelegt ist, kann dies nicht mehr rückgängig gemacht werden. Dadurch wird verhindert, dass andere Ressourcenadministratoren PIM-Einstellungen entfernen.

    ![Bereich „Ermittlung“, in dem eine Ressource ausgewählt und die Option „Ressource verwalten“ hervorgehoben ist](./media/pim-resource-roles-discover-resources/discovery-manage-resource.png)

1. Wenn eine Meldung angezeigt wird, in der Sie das Onboarding der ausgewählten Ressource zum Verwalten bestätigen sollen, klicken Sie auf **Ja**.

    ![Meldung zur Bestätigung des Onboardings der ausgewählten Ressourcen zur Verwaltung](./media/pim-resource-roles-discover-resources/discovery-manage-resource-message.png)

## <a name="next-steps"></a>Nächste Schritte

- [Konfigurieren von Einstellungen für Azure-Ressourcenrollen in PIM](pim-resource-roles-configure-role-settings.md)
- [Zuweisen von Azure-Ressourcenrollen in PIM](pim-resource-roles-assign-roles.md)
