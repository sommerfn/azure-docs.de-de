---
title: Verwalten von App-Kennwörtern – Azure Active Directory | Microsoft-Dokumentation
description: Erfahren Sie mehr zu App-Kennwörtern und deren Verwendung im Zusammenhang mit der zweistufigen Überprüfung.
services: active-directory
author: eross-msft
manager: daveba
ms.reviewer: richagi
ms.assetid: 345b757b-5a2b-48eb-953f-d363313be9e5
ms.workload: identity
ms.service: active-directory
ms.subservice: user-help
ms.topic: conceptual
ms.date: 08/13/2019
ms.author: lizross
ms.custom: user-help, seo-update-azuread-jan
ms.collection: M365-identity-device-management
ms.openlocfilehash: 1a9d997f9ced0fbd921abfb13cda5a4845eb3763
ms.sourcegitcommit: e42c778d38fd623f2ff8850bb6b1718cdb37309f
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 08/19/2019
ms.locfileid: "69616298"
---
# <a name="manage-app-passwords-for-two-step-verification"></a>Verwalten von App-Kennwörtern für die zweistufige Überprüfung

>[!Important]
>Möglicherweise gestattet Ihr Administrator die Verwendung von App-Kennwörtern nicht. Wenn die Option **App-Kennwörter** nicht angezeigt wird, steht sie in Ihrer Organisation nicht zur Verfügung.

Wenn Sie App-Kennwörter verwenden, müssen Sie unbedingt Folgendes beachten:

- App-Kennwörter werden automatisch generiert und nur einmal pro Anwendung eingegeben.

- Pro Benutzer können maximal 40 Kennwörter festgelegt werden. Wenn Sie nach Erreichen dieses Maximalwerts versuchen, ein Kennwort zu erstellen, werden Sie aufgefordert, ein vorhandenes Kennwort zu löschen, bevor Sie ein neues erstellen dürfen.

- Verwenden Sie ein App-Kennwort pro Gerät, nicht pro App. Erstellen Sie z.B. ein einziges Kennwort für alle Apps auf Ihrem Laptop und dann ein weiteres einziges Kennwort für alle Apps auf Ihrem Desktopcomputer.

    >[!Note]
    >Office 2013-Clients (einschließlich Outlook) unterstützen neue Authentifizierungsprotokolle und können für die zweistufige Überprüfung verwendet werden. Diese Unterstützung bedeutet, dass Sie nach Aktivierung der zweistufigen Überprüfung keine App-Kennwörter für Office 2013-Clients mehr benötigen. Weitere Informationen finden Sie im Artikel [Funktionsweise der modernen Authentifizierung in Office 2013- und Office 2016-Client-Apps](https://support.office.com/article/how-modern-authentication-works-for-office-2013-and-office-2016-client-apps-e4c45989-4b1a-462e-a81b-2a13191cf517).

## <a name="create-new-app-passwords"></a>Erstellen von neuen App-Kennwörtern

Während Ihres ersten Registrierungsprozesses für die zweistufige Überprüfung erhalten Sie ein einzelnes App-Kennwort. Wenn Sie mehrere Kennwörter benötigen, müssen Sie diese selbst erstellen. Sie können App-Kennwörter aus mehreren Bereichen erstellen, je nachdem, wie die zweistufige Überprüfung in Ihrem Unternehmen eingerichtet ist. Weitere Informationen zur Registrierung für die Verwendung der zweistufigen Überprüfung mit Ihrem Geschäfts-, Schul- oder Unikonto finden Sie unter [Übersicht über die zweistufige Überprüfung und Ihr Geschäfts-, Schul- oder Unikonto](multi-factor-authentication-end-user-first-time.md) und in den damit verbundenen Artikeln.

### <a name="where-to-create-and-delete-your-app-passwords"></a>Erstellen und Löschen von App-Kennwörtern

Sie können App-Kennwörter erstellen und löschen, je nachdem, wie Sie die zweistufige Überprüfung verwenden:

- **Ihr Unternehmen verwendet die zweistufige Überprüfung und die Seite „Zusätzliche Sicherheitsüberprüfung“.** Wenn Sie Ihr Geschäfts-, Schul- oder Unikonto (z. B. alain@contoso.com) mit zweistufiger Überprüfung in Ihrer Organisation verwenden, können Sie Ihre App-Kennwörter auf der Seite [Zusätzliche Sicherheitsprüfung](https://account.activedirectory.windowsazure.com/Proofup.aspx) verwalten. Ausführliche Anweisungen finden Sie unter [Erstellen und Löschen von App-Kennwörtern mithilfe der Seite „Zusätzliche Sicherheitsüberprüfung“](#create-and-delete-app-passwords-from-the-additional-security-verification-page) in diesem Artikel.

- **Ihr Unternehmen verwendet die zweistufige Überprüfung und das Office 365-Portal.** Wenn Sie in Ihrem Unternehmen Ihr Geschäfts-, Schul- oder Unikonto (z. B. alain@contoso.com) die zweistufige Überprüfung und Office 365-Anwendungen verwenden, können Sie Ihre App-Kennwörter auf der [Office 365-Portalseite](https://office.portal.com) verwalten. Ausführliche Anweisungen finden Sie unter [Erstellen und Löschen von App-Kennwörtern mithilfe des Office 365-Portals](#create-and-delete-app-passwords-using-the-office-365-portal) in diesem Artikel.

- **Sie verwenden eine zweistufige Überprüfung mit einem privaten Microsoft-Konto.** Wenn Sie ein privates Microsoft-Konto (z. B. alain@outlook.com) mit zweistufiger Überprüfung verwenden, können Sie Ihre App-Kennwörter auf der Seite [Grundlegendes zur Sicherheit](https://account.microsoft.com/security/) verwalten. Ausführliche Anweisungen finden Sie unter [Verwenden von App-Kennwörtern mit Apps, die keine zweistufige Überprüfung unterstützen](https://support.microsoft.com/help/12409/microsoft-account-app-passwords-and-two-step-verification).

## <a name="create-and-delete-app-passwords-from-the-additional-security-verification-page"></a>Erstellen und Löschen von App-Kennwörtern auf der Seite „Zusätzliche Sicherheitsüberprüfung“

Sie können App-Kennwörter auf der Seite **Zusätzliche Sicherheitsüberprüfung** für Ihr Geschäfts-, Schul- oder Unikonto erstellen und löschen.

1. Melden Sie sich auf der Seite [Zusätzliche Sicherheitsüberprüfung](https://account.activedirectory.windowsazure.com/Proofup.aspx) an, und wählen Sie dann **App-Kennwörter** aus.

    ![Seite „App-Kennwörter“ mit hervorgehobener Registerkarte „App-Kennwörter“](media/multi-factor-authentication-end-user-app-passwords/mfa-app-passwords-page.png)

2. Wählen Sie **Erstellen** aus, geben Sie den Namen der App ein, für die das App-Kennwort benötigt wird, und klicken Sie dann auf **Weiter**.

    ![Erstellen Sie die Seite mit den App-Kennwörtern, mit dem Namen der App, die das Kennwort benötigt.](media/multi-factor-authentication-end-user-app-passwords/mfa-create-app-password-page.png)

3. Kopieren Sie das Kennwort von der Seite **Ihr App-Kennwort**, und wählen Sie dann **Schließen** aus.

    ![Ihre Seite „App-Kennwort“ mit dem Kennwort für Ihre angegebene App.](media/multi-factor-authentication-end-user-app-passwords/mfa-your-app-password-page.png)

4. Stellen Sie auf der Seite **App-Kennwörter** sicher, dass Ihre App aufgelistet ist.

     ![Seite „App-Kennwörter“ mit in der Liste angezeigter neuer App](media/multi-factor-authentication-end-user-app-passwords/mfa-app-passwords-page-with-new-password.png)  

5. Öffnen Sie die App, für die Sie das App-Kennwort erstellt haben (z. B. Outlook 2010), und fügen Sie dann das App-Kennwort ein, wenn Sie dazu aufgefordert werden. Dies sollte nur einmal pro App erforderlich sein.

### <a name="to-delete-an-app-password-using-the-app-passwords-page"></a>So löschen Sie ein App-Kennwort über die Seite „App-Kennwörter“

1. Wählen Sie auf der Seite **App-Kennwörter** die Option **Löschen** neben dem zu löschenden App-Kennwort aus.

   ![Löschen eines App-Kennworts](media/multi-factor-authentication-end-user-app-passwords/mfa-app-passwords-page-delete.png)

2. Klicken Sie auf **Ja**, um das Löschen des Kennworts zu bestätigen, und klicken Sie dann auf **Schließen**.

    Das App-Kennwort wurde erfolgreich gelöscht.

## <a name="create-and-delete-app-passwords-using-the-office-365-portal"></a>Erstellen und Löschen von App-Kennwörtern über das Office 365-Portal

Wenn Sie die zweistufige Überprüfung mit Ihrem Geschäfts-, Schul- oder Unikonto und Ihren Office 365-Apps verwenden, können Sie Ihre App-Kennwörter über das Office 365-Portal erstellen und löschen.

### <a name="to-create-app-passwords-using-the-office-365-portal"></a>So erstellen Sie App-Kennwörter im Office 365-Portal

1. Melden Sie sich bei Office 365 an, und wechseln Sie dann zur Seite [Mein Konto](https://portal.office.com), wählen Sie **Sicherheit und Datenschutz** aus, und erweitern Sie dann **Zusätzliche Sicherheitsüberprüfung**.

    ![Office-Portal mit erweitertem Bereich „Zusätzliche Sicherheitsüberprüfung“](media/multi-factor-authentication-end-user-app-passwords/mfa-app-passwords-o365-my-account-page.png)

2. Klicken Sie auf den Text **App-Kennwörter erstellen und verwalten**, um die Seite **App-Kennwörter** zu öffnen.

    ![Seite „App-Kennwörter“ mit hervorgehobener Registerkarte „App-Kennwörter“](media/multi-factor-authentication-end-user-app-passwords/mfa-app-passwords-page.png)

3. Wählen Sie **Erstellen** aus, geben Sie den Namen der App ein, für die das App-Kennwort benötigt wird, und klicken Sie dann auf **Weiter**.

    ![Erstellen Sie die Seite mit den App-Kennwörtern, mit dem Namen der App, die das Kennwort benötigt.](media/multi-factor-authentication-end-user-app-passwords/mfa-create-app-password-page.png)

4. Kopieren Sie das Kennwort von der Seite **Ihr App-Kennwort**, und wählen Sie dann **Schließen** aus.

    ![Ihre Seite „App-Kennwort“ mit dem Kennwort für Ihre angegebene App.](media/multi-factor-authentication-end-user-app-passwords/mfa-your-app-password-page.png)

5. Stellen Sie auf der Seite **App-Kennwörter** sicher, dass Ihre App aufgelistet ist.

     ![Seite „App-Kennwörter“ mit in der Liste angezeigter neuer App](media/multi-factor-authentication-end-user-app-passwords/mfa-app-passwords-page-with-new-password.png)  

6. Öffnen Sie die App, für die Sie das App-Kennwort erstellt haben (z. B. Outlook 2010), und fügen Sie dann das App-Kennwort ein, wenn Sie dazu aufgefordert werden. Dies sollte nur einmal pro App erforderlich sein.

### <a name="to-delete-app-passwords-using-the-app-passwords-page"></a>So löschen Sie App-Kennwörter über die Seite „App-Kennwörter“

1. Wählen Sie auf der Seite **App-Kennwörter** die Option **Löschen** neben dem zu löschenden App-Kennwort aus.

   ![Löschen eines App-Kennworts](media/multi-factor-authentication-end-user-app-passwords/mfa-app-passwords-page-delete.png)

2. Wählen Sie **Ja** im Bestätigungsfeld und dann **Schließen** aus.

    Das App-Kennwort wurde erfolgreich gelöscht.

## <a name="if-your-app-passwords-arent-working-properly"></a>Wenn Ihre App-Kennwörter nicht funktionieren

Stellen Sie sicher, dass Sie Ihr Kennwort richtig eingegeben haben. Wenn Sie sicher sind, dass Sie das Kennwort richtig eingegeben haben, versuchen Sie, sich erneut anzumelden und ein neues App-Kennwort zu erstellen. Wenn sich Ihr Problem mit keiner dieser Möglichkeiten beheben lässt, wenden Sie sich an das Helpdesk Ihres Unternehmens, damit die Supportmitarbeiter Ihre vorhandenen App-Kennwörter löschen und Sie neue Kennwörter erstellen können.

## <a name="next-steps"></a>Nächste Schritte

- [Verwalten der Einstellungen für die zweistufige Überprüfung](multi-factor-authentication-end-user-manage-settings.md)

- Probieren Sie die [Microsoft Authenticator-App](user-help-auth-app-download-install.md) aus, um Ihre Anmeldungen mithilfe von App-Benachrichtigungen zu verifizieren, anstatt SMS oder Anrufe zu erhalten.
