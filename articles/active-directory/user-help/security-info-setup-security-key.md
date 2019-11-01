---
title: Einrichten der Sicherheitsinformationen zur Verwendung eines Sicherheitsschlüssels (Vorschau) – Azure Active Directory | Microsoft-Dokumentation
description: Hier finden Sie Informationen dazu, wie Sie Ihre Sicherheitsinformationen einrichten, um Ihre Identität mithilfe eines FIDO2-Sicherheitsschlüssels (Fast Identity Online) zu bestätigen.
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
ms.collection: M365-identity-device-management
ms.openlocfilehash: 47ae7de87efd787b7d3cba14a33688c4b29637e6
ms.sourcegitcommit: 0b1a4101d575e28af0f0d161852b57d82c9b2a7e
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 10/30/2019
ms.locfileid: "73160705"
---
# <a name="set-up-security-info-to-use-a-security-key-preview"></a>Einrichten der Sicherheitsinformationen zur Verwendung eines Sicherheitsschlüssels (Vorschau)

Sie können Sicherheitsschlüssel als kennwortlose Anmeldemethode in Ihrer Organisation verwenden. Ein Sicherheitsschlüssel ist ein physisches Gerät, das mit einer eindeutigen PIN zum Anmelden bei Ihrem Geschäfts-, Schul- oder Unikonto verwendet wird. Weil Sicherheitsschlüssel erfordern, dass Sie über das physische Gerät und etwas verfügen, das nur Ihnen bekannt ist, wird ein Sicherheitsschlüssel als eine sicherere Authentifizierungsmethode angesehen als ein Benutzername und Kennwort.

[!INCLUDE [preview-notice](../../../includes/active-directory-end-user-preview-notice-security-key.md)]

>[!Important]
>Dieser Inhalt richtet sich an Benutzer. Administratoren finden in der [Azure Active Directory-Dokumentation](https://docs.microsoft.com/azure/active-directory) weitere Informationen zum Einrichten und Verwalten der Azure Active Directory-Umgebung (Azure AD).

## <a name="what-is-a-security-key"></a>Was ist ein Sicherheitsschlüssel?

Wir unterstützen derzeit mehrere Entwürfe und Anbieter von Sicherheitsschlüsseln mit kennwortlosen FIDO2-Authentifizierungsprotokollen (siehe [Fast Identity Online (FIDO)](https://fidoalliance.org/fido2/)). Mit diesen Schlüsseln können Sie sich an Ihrem Geschäfts-, Schul- oder Unikonto anmelden, um auf die cloudbasierten Ressourcen Ihrer Organisation zuzugreifen, wenn diese sich auf einem unterstützten Gerät bzw. in einem Webbrowser befinden.

Ihr Administrator oder Ihre Organisation stellt Ihnen einen Sicherheitsschlüssel zur Verfügung, wenn er für Ihr Geschäfts-, Schul- oder Unikonto erforderlich ist. Es gibt verschiedene Typen von Sicherheitsschlüsseln, die Sie verwenden können. So können Sie z. B. einen USB-Schlüssel an Ihr Gerät anschließen oder einen NFC-Schlüssel an ein NFC-Lesegerät halten. Weitere Informationen zu Ihrem Sicherheitsschlüssel, einschließlich des Typs, finden Sie in der Dokumentation des Herstellers.

> [!Note]
> Wenn Sie keinen FIDO2-Sicherheitsschlüssel verwenden können, sind andere kennwortlose Authentifizierungsmethoden verfügbar, die Sie nutzen können, beispielsweise die Microsoft Authenticator-App oder Windows Hello. Weitere Informationen zur Microsoft Authenticator-App finden Sie im Artikel [Was ist die Microsoft Authenticator-App?](user-help-auth-app-overview.md). Weitere Informationen zu Windows Hello finden Sie auf der Seite mit der [Übersicht über Windows Hello](https://www.microsoft.com/windows/windows-hello).

## <a name="before-you-begin"></a>Voraussetzungen

Bevor Sie Ihren Sicherheitsschlüssel registrieren können, müssen folgende Voraussetzungen erfüllt sein:

- Ihr Administrator hat dieses Feature zur Verwendung in Ihrer Organisation aktiviert.

- Sie verwenden ein Gerät, auf dem das Windows-Update vom 10. Mai 2019 ausgeführt wird, und einen unterstützten Browser.

- Sie verfügen über einen physischen Sicherheitsschlüssel, der von Ihrem Administrator oder Ihrer Organisation genehmigt wurde. Ihr Sicherheitsschlüssel muss sowohl FIDO2- als auch Microsoft-kompatibel sein. Wenn Sie Fragen zu Ihrem Sicherheitsschlüssel haben und wissen möchten, ob er kompatibel ist, wenden Sie sich an den Helpdesk Ihrer Organisation.

## <a name="register-your-security-key"></a>Registrieren Ihres Sicherheitsschlüssels

Sie müssen Ihren Sicherheitsschlüssel erstellen und dabei eine eindeutige PIN angeben, bevor Sie sich mit dem Schlüssel bei Ihrem Geschäfts-, Schul- oder Unikonto anmelden können. Sie können für Ihr Konto maximal zehn Schlüssel registrieren. 

1. Wechseln Sie zur Seite **Mein Profil** unter https://myprofile.microsoft.com, und melden Sie sich an, wenn Sie dies nicht bereits getan haben.

2. Wählen Sie **Sicherheitsinformationen** aus, wählen Sie **Methode hinzufügen** aus, und wählen Sie dann in der Dropdownliste **Methode hinzufügen** den Eintrag **Sicherheitsschlüssel** aus.

    ![Dialogfeld „Methode hinzufügen“, in dem „Sicherheitsschlüssel“ ausgewählt ist](media/security-info/security-info-security-key-add-method.png)

3. Wählen Sie **Hinzufügen** aus, und wählen Sie dann den Typ Ihres Sicherheitsschlüssels (**USB-Gerät** oder **NFC-Gerät**) aus.

    ![Auswählen, ob Sie einen Sicherheitsschlüssel vom Typ USB oder NFC verwenden](media/security-info/security-info-security-key-choose-type.png)

    > [!Note]
    > Wenn Sie nicht genau wissen, über welchen Typ von Sicherheitsschlüssel Sie verfügen, ziehen Sie die Dokumentation des Herstellers zurate. Wenn Sie nicht wissen, um welchen Hersteller es sich handelt, wenden Sie sich an den Helpdesk Ihrer Organisation.

4. Halten Sie Ihren Sicherheitsschlüssel bereit, und wählen Sie dann im Dialogfeld **Sicherheitsschlüssel** die Option **Weiter** aus.

    ![Dialogfeld zum Starten der Registrierung des Sicherheitsschlüssels](media/security-info/security-info-security-key-start-setup.png)

    Es wird ein neues Dialogfeld angezeigt, in dem Sie die Einrichtung Ihrer neuen Anmeldemethode durchlaufen können.

5. Wählen Sie im Dialogfeld **Ihre neue Anmeldemethode wird eingerichtet** die Option **Weiter** aus, und gehen Sie dann folgendermaßen vor:

    - Wenn Ihr Sicherheitsschlüssel ein USB-Gerät ist, schließen Sie Ihren Sicherheitsschlüssel an den USB-Anschluss Ihres Geräts an.

    - Wenn Ihr Sicherheitsschlüssel ein NFC-Gerät ist, halten Sie Ihren Sicherheitsschlüssel an Ihr Lesegerät.

6. Geben Sie im Feld **Windows-Sicherheit** Ihre eindeutige Sicherheitsschlüssel-PIN ein, und wählen Sie dann **OK** aus.

    Daraufhin kehren Sie zum Dialogfeld **Ihre neue Anmeldemethode wird eingerichtet** zurück.

7. Klicken Sie auf **Weiter**.

8. Kehren Sie zur Seite **Sicherheitsinformationen** zurück, geben Sie einen Namen für Ihren neuen Sicherheitsschlüssel ein, den Sie später als solchen erkennen, und wählen Sie dann **Weiter** aus.

    ![Sicherheitsinfoseite, Benennen des Sicherheitsschlüssels](media/security-info/security-info-security-key-name.png)

    Ihr Sicherheitsschlüssel ist registriert und kann jetzt von Ihnen für die Anmeldung bei Ihrem Geschäfts-, Schul- oder Unikonto verwendet werden.

9. Wählen Sie **Fertig** aus, um das Dialogfeld **Sicherheitsschlüssel** zu schließen.

    Die Seite **Sicherheitsinformationen** wird mit Ihren Sicherheitsschlüsselinformationen aktualisiert.

    ![Sicherheitsinfoseite mit allen registrierten Methoden](media/security-info/security-info-security-key-configured.png)

## <a name="delete-a-security-key-from-your-security-info"></a>Löschen eines Sicherheitsschlüssels aus Ihren Sicherheitsinformationen

Falls Sie den Sicherheitsschlüssel nicht wiederfinden oder nicht mehr verwenden möchten, können Sie ihn aus Ihren Sicherheitsinformationen löschen. Der Sicherheitsschlüssel wird dann zwar nicht mehr mit Ihrem Geschäfts-, Schul- oder Unikonto verwendet, speichert jedoch weiterhin Ihre Daten und Anmeldeinformationen. Wenn Sie Ihre Daten und Anmeldeinformationen aus dem Sicherheitsschlüssel selbst löschen möchten, müssen Sie den im Abschnitt [Zurücksetzen Ihres Sicherheitsschlüssels](#reset-your-security-key) dieses Artikels beschriebenen Anweisungen folgen.

1. Wählen Sie den Link **Löschen** des Sicherheitsschlüssels aus, der entfernt werden soll.

2. Wählen Sie im Dialogfeld **Sicherheitsschlüssel löschen** die Option **OK** aus.

    Daraufhin wird Ihr Sicherheitsschlüssel gelöscht, und Sie können ihn nicht mehr zum Anmelden bei Ihrem Geschäfts-, Schul- oder Unikonto verwenden.

>[!Important]
>Falls Sie einen Sicherheitsschlüssel versehentlich gelöscht haben, können Sie ihn gemäß der Anleitung im Abschnitt [Registrieren Ihres Sicherheitsschlüssels](#register-your-security-key) dieses Artikels erneut registrieren.

## <a name="manage-your-security-key-settings-from-windows-settings"></a>Verwalten Ihrer Sicherheitsschlüsseleinstellungen in Windows-Einstellungen

Sie können Ihre Sicherheitsschlüsseleinstellungen in der App **Windows-Einstellungen** verwalten. Dies umfasst auch das Zurücksetzen des Sicherheitsschlüssels und das Erstellen einer neuen Sicherheitsschlüssel-PIN.

### <a name="reset-your-security-key"></a>Zurücksetzen Ihres Sicherheitsschlüssels

Wenn Sie alle Kontoinformationen löschen möchten, die auf Ihrem physischen Sicherheitsschlüssel gespeichert sind, müssen Sie den Schlüssel auf die Werkseinstellungen zurücksetzen. Beim Zurücksetzen Ihres Sicherheitsschlüssels werden alle Inhalte auf dem Schlüssel gelöscht, sodass Sie erneut starten können.

>[!IMPORTANT]
>Beim Zurücksetzen Ihres Sicherheitsschlüssels werden alle Inhalte auf dem Schlüssel gelöscht, wobei er auf die Werkseinstellungen zurückgesetzt wird.
>
> **Alle Daten und Anmeldeinformationen werden gelöscht.**

#### <a name="to-reset-your-security-key"></a>So setzen Sie Ihren Sicherheitsschlüssel zurück

1. Öffnen Sie die App Windows-Einstellungen, und wählen Sie **Konten** aus. Wählen Sie **Anmeldeoptionen** aus, wählen Sie **Sicherheitsschlüssel** aus, und wählen Sie dann **Verwalten** aus.

2. Schließen Sie Ihren Sicherheitsschlüssel an den USB-Anschluss an, oder halten Sie ihn an Ihr NFC-Lesegerät, um Ihre Identität zu bestätigen.

3. Folgen Sie den Anweisungen auf dem Bildschirm auf der Grundlage ihres spezifischen Sicherheitsschlüsselherstellers. Wenn Ihr Schlüsselhersteller in den Anweisungen auf dem Bildschirm nicht aufgeführt ist, besuchen Sie die Website des Herstellers, um weitere Informationen zu erhalten.

4. Wählen Sie **Schließen** aus, um den Bildschirm **Verwalten** zu schließen.

### <a name="create-a-new-security-key-pin"></a>Erstellen einer neuen Sicherheitsschlüssel-PIN

Sie können eine neue Sicherheitsschlüssel-PIN für Ihren Sicherheitsschlüssel erstellen.

#### <a name="to-create-a-new-security-key-pin"></a>So erstellen Sie eine neue Sicherheitsschlüssel-PIN

1. Öffnen Sie die App Windows-Einstellungen, und wählen Sie **Konten** aus. Wählen Sie **Anmeldeoptionen** aus, wählen Sie **Sicherheitsschlüssel** aus, und wählen Sie dann **Verwalten** aus.

2. Schließen Sie Ihren Sicherheitsschlüssel an den USB-Anschluss an, oder halten Sie ihn an Ihr NFC-Lesegerät, um Ihre Identität zu bestätigen.
3. Wählen Sie im Bereich **Sicherheitsschlüssel-PIN** die Option **Hinzufügen** aus, geben Sie Ihre neue Sicherheitsschlüssel-PIN ein, bestätigen Sie die PIN, und wählen Sie dann **OK** aus.

     Der Sicherheitsschlüssel wird mit der neuen Sicherheitsschlüssel-PIN zur Verwendung mit Ihrem Geschäfts-, Schul- oder Unikonto aktualisiert. Wenn Sie die PIN erneut ändern möchten, können Sie die Schaltfläche **Ändern** wählen.
4. Wählen Sie **Schließen** aus, um den Bildschirm **Verwalten** zu schließen.

## <a name="additional-security-info-methods"></a>Weitere Methoden für Sicherheitsinformationen

Um einen Sicherheitsschlüssel registrieren zu können, müssen Sie mindestens eine zusätzliche Methode für die Sicherheitsüberprüfung registriert haben. Weitere Informationen finden Sie im Abschnitt mit der [Übersicht](security-info-add-update-methods-overview.md). 

## <a name="next-steps"></a>Nächste Schritte

- Lesen Sie den Blogbeitrag [Microsoft Azure AD startet die öffentliche Vorschauversion von FIDO2-Sicherheitsschlüsseln, die kennwortlose Anmeldungen ermöglichen](https://www.onmsft.com/news/microsofts-azure-ad-begins-public-preview-of-fido2-security-keys-enabling-passwordless-logins) oder die Artikel [Wozu dient die Microsoft Authenticator-App?](user-help-auth-app-overview.md) und [Windows Hello: Lernen Sie die Gesichtserkennung unter Windows 10 kennen](https://www.microsoft.com/windows/windows-hello), um weitere Informationen zu kennwortlosen Authentifizierungsmethoden zu erhalten.

- Lesen Sie ausführlichere Informationen zu [Microsoft-kompatiblen Sicherheitsschlüsseln](https://docs.microsoft.com/windows/security/identity-protection/hello-for-business/microsoft-compatible-security-key).

- Setzen Sie Ihr Kennwort zurück, wenn Sie es verloren oder vergessen haben. Verwenden Sie dazu das [Portal für die Kennwortzurücksetzung](https://passwordreset.microsoftonline.com/), oder führen Sie die Schritte im Artikel [Reset your work or school password](active-directory-passwords-update-your-own-password.md) (Zurücksetzen des Kennworts eines Geschäfts-, Schul- oder Unikontos) aus.

- Der Artikel [Wenn Sie sich nicht bei Ihrem Microsoft-Konto anmelden können](https://support.microsoft.com/help/12429/microsoft-account-sign-in-cant) enthält Tipps zur Problembehandlung bei Anmeldeproblemen.
