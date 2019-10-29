---
title: Aktivieren meiner Azure AD-Rollen in PIM – Azure Active Directory | Microsoft-Dokumentation
description: Erfahren Sie, wie Sie Azure AD-Rollen in Azure AD Privileged Identity Management (PIM) aktivieren.
services: active-directory
documentationcenter: ''
author: curtand
manager: mtillman
editor: ''
ms.service: active-directory
ms.topic: conceptual
ms.workload: identity
ms.subservice: pim
ms.date: 06/28/2019
ms.author: curtand
ms.custom: pim
ms.collection: M365-identity-device-management
ms.openlocfilehash: 6007762c897337170dec69c3486302aa62723480
ms.sourcegitcommit: 8074f482fcd1f61442b3b8101f153adb52cf35c9
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 10/22/2019
ms.locfileid: "72756285"
---
# <a name="activate-my-azure-ad-roles-in-pim"></a>Aktivieren meiner Azure AD-Rollen in PIM

Azure Active Directory (Azure AD) Privileged Identity Management (PIM) vereinfacht die Art und Weise, in der Unternehmen den privilegierten Zugriff auf Ressourcen in Azure AD und anderen Onlinediensten von Microsoft wie Office 365 oder Microsoft Intune verwalten.  

Wenn Sie für eine Administratorrolle berechtigt sind, können Sie diese Rolle aktivieren, um privilegierte Aufgaben durchzuführen. Wenn Sie z. B. gelegentlich Office 365-Funktionen verwalten, dürfen Ihre privilegierte Rollenadministratoren Ihrer Organisation Sie nicht als permanenten globalen Administrator festlegen, da sich diese Rolle auf andere Dienste auswirkt. Stattdessen gewähren sie Ihnen Berechtigungen für Azure AD-Rollen wie z. B. Exchange Online-Administrator. Sie können eine Aktivierung dieser Rolle anfordern, wenn Sie diese Berechtigungen benötigen, und verfügen damit für einen bestimmten Zeitraum über Administratorkontrolle.

Dieser Artikel richtet sich an Administratoren, die ihre Azure AD-Rolle in Privileged Identity Management aktivieren müssen.

## <a name="activate-a-role"></a>Aktivieren einer Rolle

Wenn Sie eine Azure AD-Rolle übernehmen müssen, können Sie in Privileged Identity Management über die Navigationsoption **Meine Rollen** die Aktivierung anfordern.

1. Melden Sie sich beim [Azure-Portal](https://portal.azure.com/) an.

1. Öffnen Sie **Azure AD Privileged Identity Management**. Informationen zum Hinzufügen der Privileged Identity Management-Kachel zu Ihrem Dashboard finden Sie unter [Einstieg in die Verwendung von PIM](pim-getting-started.md).

1. Klicken Sie auf **Azure AD-Rollen**.

1. Klicken Sie auf **Meine Rollen**, um eine Liste der Azure AD-Rollen anzuzeigen, für die Sie berechtigt sind.

    ![„Azure AD-Rollen – Meine Rollen“ zeigt eine Liste mit berechtigten oder aktiven Rollen](./media/pim-how-to-activate-role/directory-roles-my-roles.png)

1. Suchen Sie nach einer Rolle, die Sie aktivieren möchten.

    ![„Azure AD-Rollen – Meine berechtigten Rollen“ zeigt eine Liste mit „Link aktivieren“](./media/pim-how-to-activate-role/directory-roles-my-roles-activate.png)

1. Klicken Sie auf **Aktivieren**, um den Detailbereich für die Rollenaktivierung zu öffnen.

1. Wenn Ihre Rolle eine mehrstufige Authentifizierung erfordert, klicken Sie auf **Überprüfen Sie Ihre Identität, bevor Sie den Vorgang fortsetzen**. Sie müssen sich nur einmal pro Sitzung authentifizieren.

    ![Überprüfung meiner Identität mit MFA vor der Rollenaktivierung](./media/pim-how-to-activate-role/directory-roles-my-roles-mfa.png)

1. Klicken Sie auf **Meine Identität überprüfen**, und folgen Sie den Anweisungen zur Bereitstellung einer zusätzlichen Sicherheitsüberprüfung.

    ![Zusätzliche Sicherheitsüberprüfung mit der Frage nach Kontaktmöglichkeiten](./media/pim-how-to-activate-role/additional-security-verification.png)

1. Klicken Sie auf **Aktivieren**, um den Bereich für die Aktivierung zu öffnen.

    ![Aktivierungsfeld zum Angeben einer Startzeit, Dauer, eines Tickets und eines Grunds](./media/pim-how-to-activate-role/directory-roles-activate.png)

1. Falls erforderlich, geben Sie einen Startzeitpunkt für die Aktivierung an.

1. Geben Sie die Dauer der Aktivierung an.

1. Geben Sie im Feld **Grund für Aktivierung** den Grund für die Aktivierungsanforderung ein. Für einige Rollen müssen Sie eine Ticketnummer zur Problembehebung angeben.

    ![Ausgefülltes Aktivierungsfeld mit benutzerdefinierter Startzeit, Dauer, Ticket und Grund](./media/pim-how-to-activate-role/directory-roles-activation-pane.png)

1. Klicken Sie auf **Aktivieren**.

    Wenn die Rolle nicht genehmigungspflichtig ist, wird ein Bereich **Aktivierungsstatus** angezeigt, der den Status der Aktivierung angibt.

    ![Die Statusaktivierungsseite zeigt die drei Phasen der Aktivierung](./media/pim-how-to-activate-role/activation-status.png)

    Nachdem alle Phasen abgeschlossen wurden, klicken Sie auf den Link **Abmelden**, um sich vom Azure-Portal abzumelden. Wenn Sie sich erneut am Portal anmelden, können Sie die Rolle nun verwenden.

    Wenn für die Aktivierung der [Rolle eine Genehmigung erforderlich ist](./azure-ad-pim-approval-workflow.md), werden Sie über eine Benachrichtigung in der oberen rechten Ecke des Browsers darüber informiert, dass die Genehmigung der Anforderung aussteht.

    ![Ausstehende Genehmigungsbenachrichtigung für die Aktivierungsanforderung](./media/pim-how-to-activate-role/directory-roles-activate-notification.png)

## <a name="view-the-status-of-your-requests"></a>Anzeigen des Status Ihrer Anforderungen

Sie können den Status Ihrer ausstehenden Aktivierungsanforderungen anzeigen.

1. Öffnen Sie Azure AD Privileged Identity Management.

1. Klicken Sie auf **Azure AD-Rollen**.

1. Klicken Sie auf **Meine Anforderungen**, um eine Liste Ihrer Anforderungen anzuzeigen.

    ![Azure AD-Rollen – Meine Anforderungsliste](./media/pim-how-to-activate-role/directory-roles-my-requests.png)

## <a name="deactivate-a-role"></a>Deaktivieren einer Rolle

Wenn eine Rolle aktiviert wurde, wird sie nach Erreichen des Zeitlimits (berechtigte Dauer) automatisch deaktiviert.

Wenn Sie Ihre Administratoraufgaben früher als geplant beendet haben, können Sie eine Rolle auch manuell in Azure AD Privileged Identity Management deaktivieren.

1. Öffnen Sie Azure AD Privileged Identity Management.

1. Klicken Sie auf **Azure AD-Rollen**.

1. Klicken Sie auf **Meine Rollen**.

1. Klicken sie auf **Aktive Rollen**, um die Liste Ihrer aktiven Rollen anzuzeigen.

1. Suchen Sie nach der Rolle, die Sie nicht mehr benötigen, und klicken Sie auf **Deaktivieren**.

## <a name="cancel-a-pending-request"></a>Abbrechen einer ausstehenden Anforderung

Sollten Sie die Aktivierung einer Rolle, für die eine Genehmigung erforderlich ist, nicht mehr benötigen, können Sie eine ausstehende Anforderung jederzeit abbrechen.

1. Öffnen Sie Azure AD Privileged Identity Management.

1. Klicken Sie auf **Azure AD-Rollen**.

1. Klicken Sie auf **Meine Anforderungen**.

1. Klicken Sie für die Rolle, für die Sie eine ausstehende Anforderung abbrechen möchten, auf **Abbrechen**.

    Durch das Klicken auf „Abbrechen“ wird die Anforderung abgebrochen. Um die Rolle erneut zu aktivieren, müssen Sie eine neue Anforderung zur Aktivierung übermitteln.

   ![Meine Anforderungsliste mit hervorgehobener Schaltfläche „Abbrechen“](./media/pim-how-to-activate-role/directory-role-cancel.png)

## <a name="troubleshoot"></a>Problembehandlung

### <a name="permissions-are-not-granted-after-activating-a-role"></a>Keine Gewährung von Berechtigungen nach der Aktivierung einer Rolle

Wenn Sie in Privileged Identity Management eine Rolle aktivieren, wird die Aktivierung möglicherweise nicht sofort an alle Portale weitergegeben, für die diese privilegierte Rolle benötigt wird. In einigen Fällen kann die Web-Zwischenspeicherung im Portal auch dann dazu führen, dass die Änderung nicht sofort wirksam ist, wenn diese weitergegeben wurde. Falls die Aktivierung verzögert ist, sollten Sie Folgendes tun.

1. Melden Sie sich vom Azure-Portal ab und dann wieder an.

    Wenn Sie eine Azure AD-Rolle aktivieren, werden die Phasen Ihrer Aktivierung angezeigt. Nach Abschluss aller Phasen wird der Link **Abmelden** angezeigt. Sie können diesen Link verwenden, um sich abzumelden. Hiermit werden die meisten Fälle von Verzögerungen bei der Aktivierung gelöst.

1. Überprüfen Sie in Privileged Identity Management, ob Sie als Mitglied der Rolle aufgeführt sind.

## <a name="next-steps"></a>Nächste Schritte

- [Aktivieren von Azure-Ressourcenrollen in PIM](pim-resource-roles-activate-your-roles.md)
