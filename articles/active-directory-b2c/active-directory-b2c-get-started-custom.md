---
title: Erste Schritte mit benutzerdefinierten Richtlinien – Azure Active Directory B2C
description: Erste Schritte mit benutzerdefinierten Richtlinien in Azure Active Directory B2C.
services: active-directory-b2c
author: mmacy
manager: celestedg
ms.service: active-directory
ms.workload: identity
ms.topic: conceptual
ms.date: 07/16/2019
ms.author: marsma
ms.subservice: B2C
ms.openlocfilehash: 83269a5ae0d2e5fb7ae2651dbc27926c910a0e03
ms.sourcegitcommit: af58483a9c574a10edc546f2737939a93af87b73
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 07/17/2019
ms.locfileid: "68302486"
---
# <a name="get-started-with-custom-policies-in-azure-active-directory-b2c"></a>Erste Schritte für benutzerdefinierte Richtlinien in Azure Active Directory B2C

[!INCLUDE [active-directory-b2c-advanced-audience-warning](../../includes/active-directory-b2c-advanced-audience-warning.md)]

[Benutzerdefinierte Richtlinien](active-directory-b2c-overview-custom.md) sind Konfigurationsdateien, die das Verhalten Ihres Azure Active Directory B2C-Mandanten definieren. In diesem Artikel erfahren Sie, wie Sie eine benutzerdefinierte Richtlinie erstellen, die die Registrierung oder Anmeldung mit einem lokalen Konto über eine E-Mail-Adresse und ein Kennwort unterstützt. Außerdem bereiten Sie Ihre Umgebung für das Hinzufügen von Identitätsanbietern vor.

## <a name="prerequisites"></a>Voraussetzungen

- Wenn Sie noch nicht über einen [Azure AD B2C-Mandanten](tutorial-create-tenant.md) verfügen, erstellen Sie einen, der mit Ihrem Azure-Abonnement verknüpft ist.
- [Registrieren Sie Ihre Anwendung](tutorial-register-applications.md) in dem Mandanten, den Sie erstellt haben, damit er mit Azure AD B2C kommunizieren kann.

## <a name="add-signing-and-encryption-keys"></a>Hinzufügen von Signatur- und Verschlüsselungsschlüsseln

1. Melden Sie sich beim [Azure-Portal](https://portal.azure.com/) als globaler Administrator Ihres Azure AD B2C-Mandanten an.
2. Stellen Sie sicher, dass Sie das Verzeichnis verwenden, das Ihren Azure AD B2C-Mandanten enthält. Klicken Sie im oberen Menü auf **Verzeichnis- und Abonnementfilter**, und wählen Sie dann das Verzeichnis aus, das Ihren Mandanten enthält.
3. Klicken Sie links oben im Azure-Portal auf **Alle Dienste**, suchen Sie nach **Azure AD B2C**, und klicken Sie darauf.
4. Wählen Sie auf der Seite „Übersicht“ die Option **Framework für die Identitätsfunktion** aus.

### <a name="create-the-signing-key"></a>Erstellen des Signaturschlüssels

1. Klicken Sie erst auf **Richtlinienschlüssel** und anschließend auf **Hinzufügen**.
2. Klicken Sie unter **Optionen** auf `Generate`.
3. Geben Sie unter **Name** `TokenSigningKeyContainer` ein. Das Präfix `B2C_1A_` wird möglicherweise automatisch hinzugefügt.
4. Wählen Sie **RSA** als **Schlüsseltyp** aus.
5. Wählen Sie unter **Schlüsselverwendung** **Signatur** aus.
6. Klicken Sie auf **Create**.

### <a name="create-the-encryption-key"></a>Erstellen des Verschlüsselungsschlüssels

1. Klicken Sie erst auf **Richtlinienschlüssel** und anschließend auf **Hinzufügen**.
2. Klicken Sie unter **Optionen** auf `Generate`.
3. Geben Sie unter **Name** `TokenEncryptionKeyContainer` ein. Das Präfix `B2C_1A`_ wird möglicherweise automatisch hinzugefügt.
4. Wählen Sie **RSA** als **Schlüsseltyp** aus.
5. Wählen Sie für **Schlüsselverwendung** die Option **Verschlüsselung** aus.
6. Klicken Sie auf **Create**.

### <a name="create-the-facebook-key"></a>Erstellen des Facebook-Schlüssels

Wenn Sie bereits über ein [Facebook-Anwendungsgeheimnis](active-directory-b2c-setup-fb-app.md) verfügen, können Sie dieses Ihrem Mandanten als Richtlinienschlüssel hinzufügen. Andernfalls müssen Sie den Schlüssel mit einem Platzhalterwert erstellen, damit Ihre Richtlinien die Überprüfung bestehen.

1. Klicken Sie erst auf **Richtlinienschlüssel** und anschließend auf **Hinzufügen**.
2. Klicken Sie unter **Optionen** auf `Manual`.
3. Geben Sie unter **Name** `FacebookSecret` ein. Das Präfix `B2C_1A_` wird möglicherweise automatisch hinzugefügt.
4. Geben Sie unter **Geheimnis** Ihr Facebook-Geheimnis aus „developers.facebook.com“ oder `0` als Platzhalter ein. Bei diesem Wert handelt sich um das Geheimnis und nicht um die Anwendungs-ID.
5. Wählen Sie unter **Schlüsselverwendung** **Signatur** aus.
6. Klicken Sie auf **Create**.

## <a name="register-identity-experience-framework-applications"></a>Registrieren von Identity Experience Framework-Anwendungen

Azure AD B2C erfordert, dass Sie zwei Anwendungen registrieren, die zur Registrierung und Anmeldung von Benutzern verwendet werden. IdentityExperienceFramework (eine Web-App) und ProxyIdentityExperienceFramework (eine native App) mit delegierter Berechtigung von der IdentityExperienceFramework-App. Lokale Konten sind nur in Ihrem Mandanten vorhanden. Ihre Benutzer melden sich mit einer eindeutigen Kombination aus E-Mail-Adresse und Kennwort an, um auf Ihre bei Mandanten registrierten Anwendungen zuzugreifen.

### <a name="register-the-identityexperienceframework-application"></a>Registrieren der IdentityExperienceFramework-Anwendung

1. Wählen Sie links oben im Azure-Portal die Option **Alle Dienste** aus.
1. Geben Sie im Suchfeld `Azure Active Directory`ein.
1. Wählen Sie in den Suchergebnissen **Azure Active Directory** aus.
1. Wählen Sie unter **Verwalten** im linken Menü **App-Registrierungen (Legacy)** aus.
1. Wählen Sie **Registrierung einer neuen Anwendung** aus.
1. Geben Sie unter **Name** `IdentityExperienceFramework` ein.
1. Wählen Sie unter **Anwendungstyp** die Option **Web-App/API** aus.
1. Geben Sie `https://your-tenant-name.b2clogin.com/your-tenant-name.onmicrosoft.com` als **Anmelde-URL** ein, wobei `your-tenant-name` für den Domänennamen Ihres Azure AD B2C-Mandanten steht. Alle URLs sollten jetzt [b2clogin.com](b2clogin.md) verwenden.
1. Klicken Sie auf **Create**. Wenn der Bestellvorgang abgeschlossen ist, kopieren Sie die Anwendungs-ID und speichern Sie sie zur späteren Verwendung.

### <a name="register-the-proxyidentityexperienceframework-application"></a>Registrieren der ProxyIdentityExperienceFramework-Anwendung

1. Wählen Sie unter **App-Registrierungen (Legacy)** die Option **Registrierung einer neuen Anwendung** aus.
2. Geben Sie unter **Name** `ProxyIdentityExperienceFramework` ein.
3. Wählen Sie **Nativ** als **Anwendungstyp** aus.
4. Geben Sie `https://your-tenant-name.b2clogin.com/your-tenant-name.onmicrosoft.com` als **Umleitungs-URI** ein, wobei `your-tenant-name` für Ihren Azure AD B2C-Mandanten steht.
5. Klicken Sie auf **Create**. Wenn der Bestellvorgang abgeschlossen ist, kopieren Sie die Anwendungs-ID und speichern Sie sie zur späteren Verwendung.
6. Klicken Sie auf der Seite „Einstellungen“ zunächst auf **Erforderliche Berechtigungen** und dann auf **Hinzufügen**.
7. Wählen Sie **API auswählen** aus, suchen Sie nach **IdentityExperienceFramework**, wählen Sie diese API aus, und klicken Sie anschließend auf **Auswählen**.
9. Aktivieren Sie das Kontrollkästchen neben **Access IdentityExperienceFramework** (Auf IdentityExperienceFramework zugreifen), und klicken Sie erst auf **Auswählen** und dann auf **Done** (Fertig).
10. Wählen Sie **Berechtigungen erteilen** aus, und bestätigen Sie dies anschließend durch Auswahl von **Ja**.

## <a name="custom-policy-starter-pack"></a>Starter Pack für benutzerdefinierte Richtlinien

Benutzerdefinierte Richtlinien sind ein Satz von XML-Dateien, die Sie in den Azure AD B2C-Mandanten hochladen, um technische Profile und User Journeys zu definieren. Wir stellen Starter Packs mit mehreren vordefinierten Richtlinien bereit, damit Sie schnell loslegen können. Jedes dieser Starter Packs enthält die kleinste Anzahl von technischen Profilen sowie die zum Erreichen der beschriebenen Szenarien benötigten User Journeys:

- **LocalAccounts** ermöglicht die ausschließliche Nutzung von lokalen Konten.
- **SocialAccounts** ermöglicht die ausschließliche Nutzung von Konten sozialer Netzwerke (oder Verbundkonten).
- **SocialAndLocalAccounts** ermöglicht sowohl die Verwendung von lokalen Konten als auch von Konten für soziale Netzwerke.
- **SocialAndLocalAccountsWithMFA** ermöglicht lokale Optionen und Optionen für soziale Netzwerke sowie zur Multi-Factor Authentication.

Die einzelnen Starter Packs enthalten Folgendes:

- **Basisdatei**: Es sind einige Änderungen an der Basisdatei erforderlich. Beispiel: *TrustFrameworkBase.xml*
- **Erweiterungsdatei**: In dieser Datei werden die meisten Konfigurationsänderungen vorgenommen. Beispiel: *TrustFrameworkExtensions.xml*
- **Dateien der vertrauenden Seite**: aufgabenspezifische Dateien, die von Ihrer Anwendung aufgerufen werden. Beispiele: *SignUpOrSignin.xml*, *ProfileEdit.xml*, *PasswordReset.xml*

In diesem Artikel bearbeiten Sie die benutzerdefinierten XML-Richtliniendateien im Starter Pack **SocialAndLocalAccounts**. Wenn Sie einen XML-Editor benötigen, können Sie [Visual Studio Code](https://code.visualstudio.com/download), einen einfachen plattformübergreifenden Editor, ausprobieren.

### <a name="get-the-starter-pack"></a>Abrufen des Starter Packs

Holen Sie sich die Starter Packs für benutzerdefinierte Richtlinien-von GitHub, und aktualisieren Sie dann die XML-Dateien im Starter Pack „SocialAndLocalAccounts“ mit dem Namen Ihres Azure AD B2C-Mandanten.

1. [Laden Sie die ZIP-Datei herunter](https://github.com/Azure-Samples/active-directory-b2c-custom-policy-starterpack/archive/master.zip), oder klonen Sie das Repository:

    ```console
    git clone https://github.com/Azure-Samples/active-directory-b2c-custom-policy-starterpack
    ```

1. Ersetzen Sie in allen Dateien im Verzeichnis **SocialAndLocalAccounts** die Zeichenfolge `yourtenant` durch den Namen Ihres Azure AD B2C-Mandanten.

    Wenn z. B. der Name des B2C-Mandanten *contosotenant* lautet, werden alle Instanzen von `yourtenant.onmicrosoft.com` in `contosotenant.onmicrosoft.com` geändert.

### <a name="add-application-ids-to-the-custom-policy"></a>Hinzufügen der Anwendungs-IDs zur benutzerdefinierten Richtlinie

Fügen Sie die Anwendung-ID zur Erweiterungsdatei *TrustFrameworkExtensions.xml* hinzu.

1. Öffnen Sie `SocialAndLocalAccounts/` **`TrustFrameworkExtensions.xml`** , und suchen Sie das Element `<TechnicalProfile Id="login-NonInteractive">`.
1. Ersetzen Sie beide Instanzen von `IdentityExperienceFrameworkAppId` durch die Anwendungs-ID der IdentityExperienceFramework-Anwendung, die Sie zuvor erstellt haben.
1. Ersetzen Sie beide Instanzen von `ProxyIdentityExperienceFrameworkAppId` durch die Anwendungs-ID der ProxyIdentityExperienceFramework-Anwendung, die Sie zuvor erstellt haben.
1. Speichern Sie die Datei .

## <a name="upload-the-policies"></a>Hochladen der Richtlinien

1. Wählen Sie in Ihrem B2C-Mandanten im Azure-Portal das Menüelement **Identity Experience Framework** aus.
1. Wählen Sie **Benutzerdefinierte Richtlinie hochladen** aus.
1. Laden Sie die Richtliniendateien in dieser Reihenfolge hoch:
    1. *TrustFrameworkBase.xml*
    1. *TrustFrameworkExtensions.xml*
    1. *SignUpOrSignin.xml*
    1. *ProfileEdit.xml*
    1. *PasswordReset.xml*

Wenn Sie die Dateien hochladen, fügt Azure jeder Datei das Präfix `B2C_1A_` hinzu.

> [!TIP]
> Wenn Ihr XML-Editor Validierungen unterstützt, überprüfen Sie die Dateien anhand des XML-Schemas `TrustFrameworkPolicy_0.3.0.0.xsd` im Stammverzeichnis des Starter Packs. Durch die XML-Schemavalidierung werden vor dem Upload Fehler festgestellt.

## <a name="test-the-custom-policy"></a>Testen der benutzerdefinierten Richtlinie

1. Wählen Sie unter **Benutzerdefinierte Richtlinien** die Richtlinie **B2C_1A_signup_signin** aus.
1. Wählen Sie auf der Übersichtsseite der benutzerdefinierten Richtlinie unter **Anwendung auswählen** die Webanwendung namens *webapp1* aus, die Sie zuvor registriert haben.
1. Stellen Sie sicher, dass die **Antwort-URL** `https://jwt.ms` lautet.
1. Wählen Sie **Jetzt ausführen** aus.
1. Registrieren Sie sich mit einer E-Mail-Adresse.
1. Wählen Sie erneut **Jetzt ausführen** aus.
1. Melden Sie sich zur Bestätigung der richtigen Konfiguration mit demselben Konto an.

## <a name="add-facebook-as-an-identity-provider"></a>Hinzufügen von Facebook als Identitätsanbieter

1. Führen Sie die Schritte unter [Einrichten der Registrierung und Anmeldung mit einem Facebook-Konto mithilfe von Azure Active Directory B2C](active-directory-b2c-setup-fb-app.md) aus, um eine Facebook-Anwendung zu konfigurieren.
1. Ersetzen Sie in der Datei `SocialAndLocalAccounts/` **`TrustFrameworkExtensions.xml`** den Wert `client_id` durch die ID der Facebook-Anwendung:

   ```xml
   <TechnicalProfile Id="Facebook-OAUTH">
     <Metadata>
     <!--Replace the value of client_id in this technical profile with the Facebook app ID"-->
       <Item Key="client_id">00000000000000</Item>
   ```

1. Laden Sie die Datei *TrustFrameworkExtensions.xml* in Ihren Mandanten hoch.
1. Wählen Sie unter **Benutzerdefinierte Richtlinien** die Richtlinie **B2C_1A_signup_signin** aus.
1. Wählen Sie **Jetzt ausführen**, und wählen Sie „Facebook“ aus, um sich bei Facebook anzumelden und die benutzerdefinierte Richtlinie zu testen. Oder rufen Sie die Richtlinie direkt über Ihre registrierte Anwendung auf.

## <a name="next-steps"></a>Nächste Schritte

Fügen Sie als Nächstes Azure Active Directory (Azure AD) als Identitätsanbieter hinzu. Die in diesem Leitfaden für erste Schritte verwendete Basisdatei enthält bereits einige Inhalte, die Sie zum Hinzufügen anderer Identitätsanbieter, z. B. Azure AD, benötigen.

Weitere Informationen zur Einrichtung von Azure AD als Identitätsanbieter finden Sie unter [Einrichten der Anmeldung mit einem Azure Active Directory-Konto mithilfe benutzerdefinierter Richtlinien in Azure Active Directory B2C](active-directory-b2c-setup-aad-custom.md).
