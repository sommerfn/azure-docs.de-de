---
title: Anmelden mit der zweistufigen Überprüfung oder mit Sicherheitsinformationen – Azure Active Directory | Microsoft-Dokumentation
description: Hier erfahren Sie, wie Sie sich mithilfe der verschiedenen Methoden zur Identitätsüberprüfung in den Sicherheitsinformationen anmelden können.
services: active-directory
author: eross-msft
manager: daveba
ms.reviewer: librown
ms.service: active-directory
ms.workload: identity
ms.subservice: user-help
ms.topic: conceptual
ms.date: 07/18/2019
ms.author: lizross
ms.custom: user-help
ms.collection: M365-identity-device-management
ms.openlocfilehash: b20ea1131ceda0527ed35d1a1082d05f25da6bac
ms.sourcegitcommit: 04ec7b5fa7a92a4eb72fca6c6cb617be35d30d0c
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 07/22/2019
ms.locfileid: "68382317"
---
# <a name="sign-in-using-two-step-verification-or-security-info"></a>Anmelden mit der zweistufige Überprüfung oder einer Sicherheitsinformation

Nachdem Sie die zweistufige Überprüfung oder eine Sicherheitsinformation eingerichtet haben, können Sie sich mit der von Ihnen angegebenen Authentifizierungsmethode bei Ihrem Konto anmelden.

> [!Note]
> Wenn Sie noch die zweistufige Überprüfung verwenden, müssen Sie die Authentifizierungsmethoden einrichten. Befolgen Sie dazu die Anweisungen im Artikel [Einrichten meines Kontos für die zweistufige Überprüfung](multi-factor-authentication-end-user-first-time.md).
>
> Wenn Ihr Administrator die Benutzeroberfläche für Sicherheitsinformationen aktiviert hat, müssen Sie Ihre Authentifizierungsmethoden mithilfe der schrittweisen Anleitungen in folgenden Artikeln einrichten:<ul><li>[Einrichten der Sicherheitsinformationen zur Verwendung einer Authenticator-App](security-info-setup-auth-app.md)</li><li>[Einrichten der Sicherheitsinformationen zur Verwendung von SMS](security-info-setup-text-msg.md)</li><li>[Einrichten der Sicherheitsinformationen zur Verwendung von Telefonanrufen](security-info-setup-phone-number.md)</li><li>[Einrichten der Sicherheitsinformationen zur Verwendung eines Sicherheitsschlüssels](security-info-setup-security-key.md)</li></ul>

## <a name="sign-in-using-an-authenticator-app-notification-on-your-mobile-device"></a>Anmelden mithilfe einer Authenticator-App-Benachrichtigung auf dem Mobilgerät

1. Melden Sie sich mit Ihrem Benutzernamen und Kennwort bei Ihrem Konto an.

2. Wählen Sie in der Benachrichtigung, die an Ihr mobiles Gerät gesendet wird, die Option **Genehmigen** aus.

## <a name="sign-in-using-an-authenticator-app-code-on-your-mobile-device"></a>Anmelden mithilfe eines Authenticator-App-Codes auf dem Mobilgerät

1. Melden Sie sich mit Ihrem Benutzernamen und Kennwort bei Ihrem Konto an.

2. Öffnen Sie die Authenticator-App, und geben Sie den zufällig generierten Code für Ihr Konto in das Feld **Code eingeben** ein.

## <a name="sign-in-using-your-phone-number"></a>Anmelden mithilfe Ihrer Telefonnummer

1. Melden Sie sich mit Ihrem Benutzernamen und Kennwort bei Ihrem Konto an.

2. Nehmen Sie das Gespräch entgegen, und befolgen Sie die Anweisungen.

## <a name="sign-in-using-a-text-message"></a>Anmelden mithilfe einer SMS

1. Melden Sie sich mit Ihrem Benutzernamen und Kennwort bei Ihrem Konto an.

2. Öffnen Sie die SMS, und geben Sie den Code in der SMS in das Feld **Code eingeben** ein.

## <a name="sign-in-using-a-security-key-at-the-lock-screen"></a>Anmelden mithilfe eines Sicherheitsschlüssels auf dem Sperrbildschirm

1. Nachdem Sie Ihren Sicherheitsschlüssel registriert haben, wählen Sie auf dem Windows 10-Sperrbildschirm das Sicherheitsschlüsselbild aus.

2. Verbinden Sie Ihren Sicherheitsschlüssel mit dem USB-Anschluss Ihres Geräts, und melden Sie sich mit Ihrer Sicherheitsschlüssel-PIN bei Windows an.

    ![Anmeldung mit Sicherheitsschlüssel auf dem Windows 10-Sperrbildschirm](./media/security-info/security-info-windows-10-lock-screen-security-key.png)

## <a name="sign-in-using-a-security-key-and-the-microsoft-edge-browser"></a>Anmelden mithilfe eines Sicherheitsschlüssels und des Microsoft Edge-Browsers

1. Nachdem Sie Ihren Sicherheitsschlüssel registriert haben, öffnen Sie den Microsoft Edge-Browser.

2. Wenn Sie zur Anmeldung aufgefordert werden, verbinden Sie Ihren Sicherheitsschlüssel mit dem USB-Anschluss Ihres Geräts, und melden Sie sich mit Ihrer Sicherheitsschlüssel-PIN bei Windows an.

    ![Anmeldung mit Sicherheitsschlüssel unter Verwendung des Microsoft Edge-Browsers](./media/security-info/security-info-edge-security-key.png)

    >[!NOTE]
    >Informationen zum Anmelden mithilfe der Microsoft Authenticator-App finden Sie im Artikel [Sign in to your accounts using the Microsoft Authenticator app (Anmelden bei einem Konto mithilfe der Microsoft Authenticator-App)](user-help-auth-app-sign-in.md).

## <a name="sign-in-using-another-verification-method"></a>Anmelden mit einer anderen Überprüfungsmethode

Wenn Sie aus einem bestimmten Grund Ihre primäre Anmeldemethode nicht verwenden können, können Sie eine andere zuvor eingerichtete Überprüfungsmethode verwenden.

1. Melden Sie sich wie gewohnt bei Ihrem Konto an, und wählen Sie dann auf der Seite **Zweistufige Überprüfung** den Link **Auf andere Weise anmelden** aus.

    ![Ändern der Überprüfungsmethode für die Anmeldung](media/security-info/two-factor-auth-signin-another-way.png)

    >[!Note]
    >Wenn der Link **Auf andere Weise anmelden** nicht angezeigt wird, heißt das, dass Sie keine anderen Überprüfungsmethoden eingerichtet haben und dass Sie sich für die Anmeldung bei Ihrem Konto an den Administrator wenden müssen. Nachdem Sie sich mit Unterstützung des Administrators angemeldet haben, sollten Sie zusätzliche Überprüfungsmethoden hinzufügen. Weitere Informationen zum Hinzufügen von Überprüfungsmethoden finden Sie im Artikel [Verwalten der Einstellungen für die zweistufige Überprüfung](multi-factor-authentication-end-user-manage-settings.md).
    >
    >Wenn zwar der Link **Auf andere Weise anmelden**, aber keine anderen Überprüfungsmethoden angezeigt werden, müssen Sie sich für die Anmeldung bei Ihrem Konto an den Administrator wenden.

2. Wählen Sie Ihre alternative Überprüfungsmethode aus, und fahren Sie mit dem zweistufigen Überprüfungsprozess fort.

3. Wenn Sie wieder zu Ihrem Konto zurückgekehrt sind, können Sie die Überprüfungsmethoden (bei Bedarf) aktualisieren. Weitere Informationen zum Hinzufügen oder Ändern von Methoden finden Sie im Artikel [Verwalten der Einstellungen für die zweistufige Überprüfung](multi-factor-authentication-end-user-manage-settings.md).

## <a name="next-steps"></a>Nächste Schritte

- Unter [Übersicht über die Sicherheitsinformationen (Vorschau)](user-help-security-info-overview.md) erfahren Sie mehr über die Sicherheitsinformationen.

- Informationen zur zweistufigen Überprüfung finden Sie im Artikel [Übersicht über die zweistufige Überprüfung](user-help-two-step-verification-overview.md).

- Setzen Sie Ihr Kennwort im [Portal für die Kennwortzurücksetzung](https://passwordreset.microsoftonline.com/) zurück, falls Sie es verloren oder vergessen haben.

- Informieren Sie sich im Artikel [Wenn Sie sich nicht bei Ihrem Microsoft-Konto anmelden können](https://support.microsoft.com/help/12429/microsoft-account-sign-in-cant) über Tipps zur Behandlung von Anmeldeproblemen.
