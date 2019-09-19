---
title: Aktivieren von Azure-Ressourcenrollen in PIM – Azure Active Directory | Microsoft-Dokumentation
description: Erfahren Sie, wie Sie Ihre Azure-Ressourcenrollen in Azure AD Privileged Identity Management (PIM) aktivieren.
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
ms.date: 06/28/2019
ms.author: curtand
ms.custom: pim
ms.collection: M365-identity-device-management
ms.openlocfilehash: 4e2bf1253f1c2f53b839a73034fcdb33f3aab463
ms.sourcegitcommit: 95b180c92673507ccaa06f5d4afe9568b38a92fb
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 09/08/2019
ms.locfileid: "70804339"
---
# <a name="activate-my-azure-resource-roles-in-pim"></a>Aktivieren von Azure-Ressourcenrollen in PIM

Mit Azure Active Directory (Azure AD) Privileged Identity Management (PIM) können für Azure-Ressourcen berechtigte Rollenmitglieder die Aktivierung für ein künftiges Datum und eine künftige Uhrzeit planen. Zudem können sie eine bestimmte Aktivierungsdauer innerhalb des (von Administratoren konfigurierten) Bereichs auswählen.

Dieser Artikel richtet sich an Mitglieder, die ihre Azure-Ressourcenrolle in PIM aktivieren müssen.

## <a name="activate-a-role"></a>Aktivieren einer Rolle

Wenn Sie eine Azure-Ressourcenrolle übernehmen müssen, können Sie in PIM über die Navigationsoption **Meine Rollen** deren Aktivierung anfordern.

1. Melden Sie sich beim [Azure-Portal](https://portal.azure.com/) an.

1. Öffnen Sie **Azure AD Privileged Identity Management**. Informationen dazu, wie Sie die PIM-Kachel zu Ihrem Dashboard hinzufügen, finden Sie unter [Einstieg in die Verwendung von PIM](pim-getting-started.md).

1. Klicken Sie auf **Meine Rollen**.

    ![Seite „Meine Rollen“ mit den Rollen, die aktiviert werden können](./media/pim-resource-roles-activate-your-roles/resources-my-roles.png)

1. Klicken Sie auf **Azure-Ressourcenrollen**, um eine Liste der Azure-Ressourcenrollen anzuzeigen, für die Sie berechtigt sind.

   ![Meine Rollen – Azure-Ressourcenrollen-Seite](./media/pim-resource-roles-activate-your-roles/resources-my-roles-azure-resources.png) 

1. Wechseln Sie in der Liste **Azure-Ressourcenrollen** zur Rolle, die Sie aktivieren möchten.

    ![Azure-Ressourcenrollen: Liste meiner berechtigten Rollen](./media/pim-resource-roles-activate-your-roles/resources-my-roles-activate.png)

1. Klicken Sie auf **Aktivieren**, um den Bereich „Aktivieren“ zu öffnen.

1. Wenn Ihre Rolle eine mehrstufige Authentifizierung erfordert, klicken Sie auf **Überprüfen Sie Ihre Identität, bevor Sie den Vorgang fortsetzen**. Sie müssen sich nur einmal pro Sitzung authentifizieren.

    ![Überprüfung meiner Identität mit MFA vor der Rollenaktivierung](./media/pim-resource-roles-activate-your-roles/resources-my-roles-mfa.png)

1. Klicken Sie auf **Meine Identität überprüfen**, und folgen Sie den Anweisungen zur Bereitstellung einer zusätzlichen Sicherheitsüberprüfung.

    ![Anzeige, die eine Sicherheitsüberprüfung wie z. B. einen PIN-Code verlangt](./media/pim-resource-roles-activate-your-roles/resources-mfa-enter-code.png)

1. Wenn Sie einen reduzierten Bereich angeben möchten, klicken Sie auf **Bereich**, um „Ressourcenfilter“ zu öffnen.

    Es wird empfohlen, nur Zugriff auf die Ressourcen anzufordern, die Sie benötigen. Im Bereich „Ressourcenfilter“ können Sie die Ressourcengruppen bzw. angeben, auf die Sie Zugriff haben möchten.

    ![Aktivieren – Ressourcenfilterbereich, der den Umfang spezifiziert](./media/pim-resource-roles-activate-your-roles/resources-my-roles-resource-filter.png)

1. Falls erforderlich, geben Sie einen Startzeitpunkt für die Aktivierung an. Das Mitglied wird nach dem ausgewählten Zeitpunkt aktiviert.

1. Geben Sie im Feld **Grund** den Grund für die Aktivierungsanforderung ein.

    ![Vervollständigter Aktivierungsbereich mit Umfang, Startzeit, Dauer und Grund](./media/pim-resource-roles-activate-your-roles/resources-my-roles-activate-done.png)

1. Klicken Sie auf **Aktivieren**.

    Wenn für die Rolle keine Genehmigung erforderlich ist, wird sie aktiviert und der Liste der aktiven Rollen hinzugefügt. Wenn Sie die Rolle verwenden möchten, führen Sie die im nächsten Abschnitt beschriebenen Schritte aus.

    Wenn für die Aktivierung der [Rolle eine Genehmigung erforderlich ist](pim-resource-roles-approval-workflow.md), werden Sie über eine Benachrichtigung in der oberen rechten Ecke des Browsers darüber informiert, dass die Genehmigung der Anforderung aussteht.

    ![Ausstehende Genehmigung für die Aktivierungsanforderung](./media/pim-resource-roles-activate-your-roles/resources-my-roles-activate-notification.png)

## <a name="use-a-role-immediately-after-activation"></a>Verwenden einer Rolle sofort nach der Aktivierung

Im Falle einer Verzögerung nach der Aktivierung führen Sie diese Schritte nach der Aktivierung aus, um Ihre Azure-Ressourcenrollen sofort zu nutzen.

1. Öffnen Sie Azure AD Privileged Identity Management.

1. Klicken Sie auf **Meine Rollen**, um eine Liste der Azure AD-Rollen und Azure-Ressourcenrollen anzuzeigen, für die Sie berechtigt sind.

1. Klicken Sie auf **Azure-Ressourcenrollen**.

1. Klicken Sie auf die Registerkarte **Aktive Rollen**.

1. Sobald die Rolle aktiv ist, melden Sie sich vom Portal ab und dann erneut an.

    Die Rolle sollte jetzt zur Verwendung zur Verfügung stehen.

## <a name="view-the-status-of-your-requests"></a>Anzeigen des Status Ihrer Anforderungen

Sie können den Status Ihrer ausstehenden Aktivierungsanforderungen anzeigen.

1. Öffnen Sie Azure AD Privileged Identity Management.

1. Klicken Sie auf **Meine Anforderungen**, um eine Liste mit Ihren Anforderungen von Azure AD-Rollen und Azure-Ressourcenrollen anzuzeigen.

    ![Meine Anforderungen – Azure-Ressourcenseite, die Ihre ausstehenden Anforderungen zeigt](./media/pim-resource-roles-activate-your-roles/resources-my-requests.png)

1. Scrollen Sie nach rechts, um die Spalte **Anforderungsstatus** anzuzeigen.

## <a name="cancel-a-pending-request"></a>Abbrechen einer ausstehenden Anforderung

Sollten Sie die Aktivierung einer Rolle, für die eine Genehmigung erforderlich ist, nicht mehr benötigen, können Sie eine ausstehende Anforderung jederzeit abbrechen.

1. Öffnen Sie Azure AD Privileged Identity Management.

1. Klicken Sie auf **Meine Anforderungen**.

1. Klicken Sie für die Rolle, für die Sie eine ausstehende Anforderung abbrechen möchten, auf den Link **Abbrechen**.

    Durch das Klicken auf „Abbrechen“ wird die Anforderung abgebrochen. Um die Rolle erneut zu aktivieren, müssen Sie eine neue Anforderung zur Aktivierung übermitteln.

   ![Liste „Meine Anforderungen“ mit hervorgehobener Aktion „Abbrechen“](./media/pim-resource-roles-activate-your-roles/resources-my-requests-cancel.png)

## <a name="troubleshoot"></a>Problembehandlung

### <a name="permissions-are-not-granted-after-activating-a-role"></a>Keine Gewährung von Berechtigungen nach der Aktivierung einer Rolle

Wenn Sie in PIM ein Rolle aktivieren, wird die Aktivierung ggf. nicht sofort an alle Portale weitergegeben, für die die privilegierte Rolle benötigt wird. In einigen Fällen kann die Web-Zwischenspeicherung im Portal auch dann dazu führen, dass die Änderung nicht sofort wirksam ist, wenn diese weitergegeben wurde. Falls die Aktivierung verzögert ist, sollten Sie Folgendes tun.

1. Melden Sie sich vom Azure-Portal ab und dann wieder an.

    Wenn Sie eine Azure-Ressourcenrolle aktivieren, werden die Phasen Ihrer Aktivierung angezeigt. Nach Abschluss aller Phasen wird der Link **Abmelden** angezeigt. Sie können diesen Link verwenden, um sich abzumelden. Hiermit werden die meisten Fälle von Verzögerungen bei der Aktivierung gelöst.

1. Überprüfen Sie in PIM, ob Sie als Mitglied der Rolle aufgeführt sind.

## <a name="next-steps"></a>Nächste Schritte

- [Verlängern oder Erneuern von Azure-Ressourcenrollen in PIM](pim-resource-roles-renew-extend.md)
- [Aktivieren meiner Azure AD-Rollen in PIM](pim-how-to-activate-role.md)
