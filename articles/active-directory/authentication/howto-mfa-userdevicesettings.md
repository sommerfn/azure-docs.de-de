---
title: Verwaltung von Benutzern und Geräten durch Administratoren – Azure MFA – Azure Active Directory
description: Beschreibt, wie Administratoren Benutzereinstellungen ändern und beispielsweise Benutzer dazu zwingen können, den Nachweisprozess noch einmal zu durchlaufen.
services: multi-factor-authentication
ms.service: active-directory
ms.subservice: authentication
ms.topic: conceptual
ms.date: 08/29/2019
ms.author: joflore
author: MicrosoftGuyJFlo
manager: daveba
ms.reviewer: michmcla
ms.collection: M365-identity-device-management
ms.openlocfilehash: 190d697dca56fa51d92987f32db0146aa79881aa
ms.sourcegitcommit: e0e6663a2d6672a9d916d64d14d63633934d2952
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 10/21/2019
ms.locfileid: "70162396"
---
# <a name="manage-user-settings-with-azure-multi-factor-authentication-in-the-cloud"></a>Verwalten von Benutzereinstellungen mit Azure Multi-Factor Authentication (MFA) in der Cloud

Als Administrator können Sie die folgenden Einstellungen für Benutzer und Geräte verwalten:

* Benutzer auffordern, Kontaktmethoden erneut bereitzustellen
* App-Kennwörter löschen
* MFA auf allen vertrauenswürdigen Geräten erfordern

## <a name="manage-authentication-methods"></a>Verwalten von Authentifizierungsmethoden

Als Administrator, dem die Rolle „Authentifizierungsadministrator“ zugewiesen wurde, können Sie festlegen, dass Benutzer ihr Kennwort zurücksetzen, sich für die mehrstufige Authentifizierung erneut registrieren oder vorhandene MFA-Sitzungen von ihrem Benutzerobjekt widerrufen müssen.

![Verwalten von Authentifizierungsmethoden im Azure-Portal](./media/howto-mfa-userdevicesettings/manage-authentication-methods.png)

1. Melden Sie sich beim [Azure-Portal](https://portal.azure.com) an.
1. Wählen Sie auf der linken Seite **Azure Active Directory** > **Benutzer** > **Alle Benutzer** aus.
1. Wählen Sie den Benutzer aus, für den Sie eine Aktion ausführen möchten, und wählen Sie anschließend **Authentifizierungsmethoden** aus.
   - **Kennwort zurücksetzen**: Das Kennwort des Benutzers wird zurückgesetzt, und ein temporäres Kennwort wird zugewiesen, das bei der nächsten Anmeldung geändert werden muss.
   - **Erneute MFA-Registrierung erforderlich**: Sorgt dafür, dass der Benutzer bei der nächsten Anmeldung zur Einrichtung einer neuen MFA-Authentifizierungsmethode aufgefordert wird.
   - **MFA-Sitzungen widerrufen**: Löscht die gespeicherten MFA-Sitzungen des Benutzers. Wenn auf dem Gerät gemäß Richtlinie das nächste Mal eine MFA erforderlich ist, muss der Benutzer diese durchführen.

## <a name="require-users-to-provide-contact-methods-again"></a>Benutzer auffordern, Kontaktmethoden erneut bereitzustellen

Durch diese Einstellung wird der Benutzer gezwungen, den Registrierungsprozess erneut durchzuführen. Nicht-Browser-Apps funktionieren weiterhin, wenn der Benutzer über App-Kennwörter dafür verfügt.  Sie können die App-Kennwörter eines Benutzers löschen, indem Sie zusätzlich **Löschen aller vorhandener App-Kennwörter, die von ausgewählten Benutzern generiert wurden**auswählen.

### <a name="how-to-require-users-to-provide-contact-methods-again"></a>Benutzer dazu bringen, Kontaktmethoden erneut bereitzustellen

1. Melden Sie sich beim [Azure-Portal](https://portal.azure.com) an.
2. Wählen Sie auf der linken Seite **Azure Active Directory** > **Benutzer** > **Alle Benutzer** aus.
3. Wählen Sie auf der rechten Seite auf der Symbolleiste die Option **Multi-Factor Authentication** aus. Die Seite „Multi-Factor Authentication“ wird geöffnet.
4. Aktivieren Sie das Kontrollkästchen neben den Benutzern, die Sie verwalten möchten. Auf der rechten Seite wird eine Liste mit Optionen für schnelle Schritte angezeigt.
5. Wählen Sie **Benutzereinstellungen verwalten** aus.
6. Aktivieren Sie das Kontrollkästchen **Bereitstellen der Kontaktmethoden bei ausgewählten Benutzern erneut anfordern**.
   ![Benutzer auffordern, Kontaktmethoden erneut bereitzustellen](./media/howto-mfa-userdevicesettings/reproofup.png)
7. Klicken Sie auf **Speichern**.
8. Klicken Sie auf **Schließen**.

Organisationen können diese Schritte mit PowerShell ausführen und dabei den folgenden Code als Leitfaden zum Löschen des `StrongAuthenticationMethods` Attributs verwenden:

```PowerShell
$Upn = "theuser@domain.com"
$noMfaConfig = @()
Set-MsolUser -UserPrincipalName $Upn -StrongAuthenticationMethods $noMfaConfig
```

## <a name="delete-users-existing-app-passwords"></a>Löschen vorhandener App-Kennwörter eines Benutzers

Diese Einstellung löscht alle App-Kennwörter, die ein Benutzer erstellt hat. Nicht-Browser-Apps, die diesen App-Kennwörtern zugeordnet waren, sind bis zur Erstellung eines neuen App-Kennworts nicht mehr funktionsfähig.

### <a name="how-to-delete-users-existing-app-passwords"></a>Löschen vorhandener App-Kennwörter eines Benutzers

1. Melden Sie sich beim [Azure-Portal](https://portal.azure.com) an.
2. Wählen Sie auf der linken Seite **Azure Active Directory** > **Benutzer** > **Alle Benutzer** aus.
3. Wählen Sie auf der rechten Seite auf der Symbolleiste die Option **Multi-Factor Authentication** aus. Die Seite „Multi-Factor Authentication“ wird geöffnet.
4. Aktivieren Sie das Kontrollkästchen neben den Benutzern, die Sie verwalten möchten. Auf der rechten Seite wird eine Liste mit Optionen für schnelle Schritte angezeigt.
5. Wählen Sie **Benutzereinstellungen verwalten** aus.
6. Aktivieren Sie das Kontrollkästchen **Alle vorhandenen App-Kennwörter löschen, die von den ausgewählten Benutzern erstellt wurden**.
   ![Alle vorhandenen App-Kennwörter löschen](./media/howto-mfa-userdevicesettings/deleteapppasswords.png)
7. Klicken Sie auf **Speichern**.
8. Klicken Sie auf **Schließen**.

## <a name="restore-mfa-on-all-remembered-devices-for-a-user"></a>Wiederherstellen der MFA auf allen gespeicherten Geräten für einen Benutzer

Eines der konfigurierbaren Features von Azure Multi-Factor Authentication besteht darin, Ihren Benutzern die Option zum Markieren von Geräten als vertrauenswürdig bereitzustellen. Weitere Informationen finden Sie unter [Konfigurieren der Einstellungen von Azure Multi-Factor Authentication](howto-mfa-mfasettings.md#remember-multi-factor-authentication).

Benutzer können die zweistufige Überprüfung für eine konfigurierbare Anzahl von Tagen auf ihren regulären Geräten deaktivieren. Wenn ein Konto kompromittiert wird oder ein vertrauenswürdiges Gerät verloren geht, müssen Sie in der Lage sein, den Status „Vertrauenswürdig“ aufzuheben und die zweistufige Überprüfung wieder anzufordern.

Wenn **Wiederherstellen der mehrstufigen Authentifizierung für alle gespeicherten Geräte** aktiviert ist, müssen die Benutzer bei der nächsten Anmeldung die zweistufige Überprüfung ausführen, auch wenn ihr Gerät als vertrauenswürdig gekennzeichnet wurde.

### <a name="how-to-restore-mfa-on-all-suspended-devices-for-a-user"></a>MFA auf allen ausgesetzten Geräten für einen Benutzer wiederherstellen

1. Melden Sie sich beim [Azure-Portal](https://portal.azure.com) an.
2. Wählen Sie auf der linken Seite **Azure Active Directory** > **Benutzer** > **Alle Benutzer** aus.
3. Wählen Sie auf der rechten Seite auf der Symbolleiste die Option **Multi-Factor Authentication** aus. Die Seite „Multi-Factor Authentication“ wird geöffnet.
4. Aktivieren Sie das Kontrollkästchen neben den Benutzern, die Sie verwalten möchten. Auf der rechten Seite wird eine Liste mit Optionen für schnelle Schritte angezeigt.
5. Wählen Sie **Benutzereinstellungen verwalten** aus.
6. Aktivieren Sie das Kontrollkästchen **Mehrstufige Authentifizierung für alle gespeicherten Geräte wiederherstellen**.
   ![Mehrstufige Authentifizierung für alle gespeicherten Geräte wiederherstellen](./media/howto-mfa-userdevicesettings/rememberdevices.png)
7. Klicken Sie auf **Speichern**.
8. Klicken Sie auf **Schließen**.

## <a name="next-steps"></a>Nächste Schritte

- Erfahren Sie mehr über das [Konfigurieren der Einstellungen von Azure Multi-Factor Authentication](howto-mfa-mfasettings.md).
- Wenn Ihre Benutzer Hilfe benötigen, verweisen Sie sie auf das [Benutzerhandbuch für die zweistufige Überprüfung](../user-help/multi-factor-authentication-end-user.md).
