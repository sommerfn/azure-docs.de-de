---
title: Einrichten der Anmeldung für einen mehrinstanzenfähigen Azure AD-Identitätsanbieter mithilfe von benutzerdefinierten Richtlinien in Azure Active Directory B2C
description: Hinzufügen eines mehrinstanzenfähigen Azure AD-Identitätsanbieters mithilfe von benutzerdefinierten Richtlinien – Azure Active Directory B2C.
services: active-directory-b2c
author: mmacy
manager: celestedg
ms.service: active-directory
ms.workload: identity
ms.topic: conceptual
ms.date: 09/13/2019
ms.author: marsma
ms.subservice: B2C
ms.openlocfilehash: f254ebe599e64f4c48a839d9defd57e0899138a5
ms.sourcegitcommit: 8074f482fcd1f61442b3b8101f153adb52cf35c9
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 10/22/2019
ms.locfileid: "72755777"
---
# <a name="set-up-sign-in-for-multi-tenant-azure-active-directory-using-custom-policies-in-azure-active-directory-b2c"></a>Einrichten der Anmeldung für einen mehrinstanzenfähigen Azure Active Directory-Identitätsanbieter mithilfe von benutzerdefinierten Richtlinien in Azure Active Directory B2C

[!INCLUDE [active-directory-b2c-advanced-audience-warning](../../includes/active-directory-b2c-advanced-audience-warning.md)]

In diesem Artikel wird erläutert, wie Sie die Anmeldung für Benutzer des mehrinstanzenfähigen Endpunkts für Azure Active Directory (Azure AD) mithilfe [benutzerdefinierter Richtlinien](active-directory-b2c-overview-custom.md) in Azure AD B2C aktivieren. Dadurch können sich Benutzer aus mehreren Azure AD-Mandanten mit Azure AD B2C anmelden, ohne dass Sie einen Identitätsanbieter für jeden Mandanten konfigurieren müssen. Allerdings können sich Gastmitglieder in diesen Mandanten **nicht** anmelden. Dazu müssen Sie [jeden Mandanten einzeln konfigurieren](active-directory-b2c-setup-aad-custom.md).

## <a name="prerequisites"></a>Voraussetzungen

Führen Sie die unter [Erste Schritte mit benutzerdefinierten Richtlinien in Azure Active Directory B2C](active-directory-b2c-get-started-custom.md) beschriebenen Schritte aus.

## <a name="register-an-application"></a>Registrieren einer Anwendung

Um die Anmeldung für Benutzer von einer bestimmten Azure AD-Organisation zu aktivieren, müssen Sie eine Anwendung im Azure AD-Mandanten der Organisation registrieren.

1. Melden Sie sich beim [Azure-Portal](https://portal.azure.com) an.
1. Stellen Sie sicher, dass Sie das Verzeichnis verwenden, das Ihren Azure AD-Organisationsmandanten (z. B. „contoso.com“) enthält. Wählen Sie im oberen Menü den Filter **Verzeichnis und Abonnement** aus, und wählen Sie dann das Verzeichnis aus, das Ihren Mandanten enthält.
1. Klicken Sie links oben im Azure-Portal auf **Alle Dienste**, suchen Sie nach **App-Registrierungen**, und wählen Sie dann diese Option aus.
1. Wählen Sie **Neue Registrierung** aus.
1. Geben Sie einen **Namen** für Ihre Anwendung ein. Beispiel: `Azure AD B2C App`.
1. Wählen Sie für diese Anwendung die Option **Konten in einem beliebigen Organisationsverzeichnis** aus.
1. Übernehmen Sie den Wert **Web** als **Umleitungs-URI**, geben Sie die folgende URL in Kleinbuchstaben ein, und ersetzen Sie dabei `your-B2C-tenant-name` durch den Namen Ihres Azure AD B2C-Mandanten.

    ```
    https://your-B2C-tenant-name.b2clogin.com/your-B2C-tenant-name.onmicrosoft.com/oauth2/authresp
    ```

    Beispiel: `https://contoso.b2clogin.com/contoso.onmicrosoft.com/oauth2/authresp`.

1. Wählen Sie **Registrieren**. Notieren Sie sich die **Anwendungs-ID (Client)** zur Verwendung in einem späteren Schritt.
1. Wählen Sie **Zertifikate & Geheimnisse** und dann **Neuer geheimer Clientschlüssel** aus.
1. Geben Sie eine **Beschreibung** für das Geheimnis ein, wählen Sie ein Ablaufdatum aus, und wählen Sie dann **Hinzufügen** aus. Notieren Sie sich den **WERT** des Geheimnisses zur Verwendung in einem späteren Schritt.

## <a name="create-a-policy-key"></a>Erstellen eines Richtlinienschlüssels

Sie müssen den von Ihnen erstellten Anwendungsschlüssel in Ihrem Azure AD B2C-Mandanten speichern.

1. Stellen Sie sicher, dass Sie das Verzeichnis verwenden, das Ihren Azure AD B2C-Mandanten enthält. Wählen Sie im oberen Menü den Filter **Verzeichnis und Abonnement** aus, und wählen Sie dann das Verzeichnis aus, das Ihren Azure AD B2C-Mandanten enthält.
1. Wählen Sie links oben im Azure-Portal die Option **Alle Dienste** aus, suchen Sie nach **Azure AD B2C**, und wählen Sie dann diese Option aus.
1. Wählen Sie unter **Richtlinien** die Option **Identity Experience Framework** aus.
1. Wählen Sie **Richtlinienschlüssel** und dann **Hinzufügen** aus.
1. Klicken Sie unter **Optionen** auf `Manual`.
1. Geben Sie einen **Namen** für den Richtlinienschlüssel ein. Beispiel: `AADAppSecret`.  Das Präfix `B2C_1A_` wird dem Namen Ihres Schlüssels bei der Erstellung automatisch hinzugefügt. Im XML-Code im folgenden Abschnitt lautet der entsprechende Verweis darauf daher *B2C_1A_AADAppSecret*.
1. Geben Sie im Feld **Geheimnis** den zuvor notierten geheimen Clientschlüssel ein.
1. Wählen Sie für **Schlüsselverwendung** die Option `Signature` aus.
1. Klicken Sie auf **Erstellen**.

## <a name="add-a-claims-provider"></a>Hinzufügen eines Anspruchsanbieters

Wenn Sie möchten, dass sich Benutzer mithilfe von Azure AD anmelden, müssen Sie Azure AD als Anspruchsanbieter definieren, mit dem Azure AD B2C über einen Endpunkt kommunizieren kann. Der Endpunkt bietet eine Reihe von Ansprüchen, mit denen Azure AD B2C überprüft, ob ein bestimmter Benutzer authentifiziert wurde.

Sie können Azure AD als Anspruchsanbieter definieren, indem Sie Azure AD in der Erweiterungsdatei Ihrer Richtlinie dem Element **ClaimsProvider** hinzufügen.

1. Öffnen Sie die Datei *TrustFrameworkExtensions.xml*.
1. Suchen Sie nach dem Element **ClaimsProviders**. Falls das Element nicht vorhanden sein sollte, fügen Sie es unter dem Stammelement hinzu.
1. Fügen Sie ein neues **ClaimsProvider**-Element wie folgt hinzu:

    ```XML
    <ClaimsProvider>
      <Domain>commonaad</Domain>
      <DisplayName>Common AAD</DisplayName>
      <TechnicalProfiles>
        <TechnicalProfile Id="Common-AAD">
          <DisplayName>Multi-Tenant AAD</DisplayName>
          <Description>Login with your Contoso account</Description>
          <Protocol Name="OpenIdConnect"/>
          <Metadata>
            <Item Key="METADATA">https://login.microsoftonline.com/common/v2.0/.well-known/openid-configuration</Item>
            <!-- Update the Client ID below to the Application ID -->
            <Item Key="client_id">00000000-0000-0000-0000-000000000000</Item>
            <Item Key="response_types">code</Item>
            <Item Key="scope">openid profile</Item>
            <Item Key="response_mode">form_post</Item>
            <Item Key="HttpBinding">POST</Item>
            <Item Key="UsePolicyInRedirectUri">false</Item>
            <Item Key="DiscoverMetadataByTokenIssuer">true</Item>
            <!-- The key below allows you to specify each of the Azure AD tenants that can be used to sign in. Update the GUIDs below for each tenant. -->
            <Item Key="ValidTokenIssuerPrefixes">https://login.microsoftonline.com/00000000-0000-0000-0000-000000000000,https://login.microsoftonline.com/11111111-1111-1111-1111-111111111111</Item>
            <!-- The commented key below specifies that users from any tenant can sign-in. Uncomment if you would like anyone with an Azure AD account to be able to sign in. -->
            <!-- <Item Key="ValidTokenIssuerPrefixes">https://login.microsoftonline.com/</Item> -->
          </Metadata>
          <CryptographicKeys>
            <Key Id="client_secret" StorageReferenceId="B2C_1A_AADAppSecret"/>
          </CryptographicKeys>
          <OutputClaims>
            <OutputClaim ClaimTypeReferenceId="issuerUserId" PartnerClaimType="oid"/>
            <OutputClaim ClaimTypeReferenceId="tenantId" PartnerClaimType="tid"/>
            <OutputClaim ClaimTypeReferenceId="givenName" PartnerClaimType="given_name" />
            <OutputClaim ClaimTypeReferenceId="surName" PartnerClaimType="family_name" />
            <OutputClaim ClaimTypeReferenceId="displayName" PartnerClaimType="name" />
            <OutputClaim ClaimTypeReferenceId="authenticationSource" DefaultValue="socialIdpAuthentication" AlwaysUseDefaultValue="true" />
            <OutputClaim ClaimTypeReferenceId="identityProvider" PartnerClaimType="iss" />
          </OutputClaims>
          <OutputClaimsTransformations>
            <OutputClaimsTransformation ReferenceId="CreateRandomUPNUserName"/>
            <OutputClaimsTransformation ReferenceId="CreateUserPrincipalName"/>
            <OutputClaimsTransformation ReferenceId="CreateAlternativeSecurityId"/>
            <OutputClaimsTransformation ReferenceId="CreateSubjectClaimFromAlternativeSecurityId"/>
          </OutputClaimsTransformations>
          <UseTechnicalProfileForSessionManagement ReferenceId="SM-SocialLogin"/>
        </TechnicalProfile>
      </TechnicalProfiles>
    </ClaimsProvider>
    ```

1. Ändern Sie unter dem Element **ClaimsProvider** den Wert für **Domain** in einen eindeutigen Wert, der eine Unterscheidung von anderen Identitätsanbietern ermöglicht.
1. Aktualisieren Sie unter dem Element **TechnicalProfile** den Wert für **DisplayName**, z. B. `Contoso Employee`. Dieser Wert wird auf die Anmeldeschaltfläche auf der Anmeldeseite angezeigt.
1. Legen Sie **client_id** auf die Anwendungs-ID der mehrinstanzenfähigen Azure AD-Anwendung fest, die Sie zuvor registriert haben.
1. Aktualisieren Sie unter **CryptographicKeys** den Wert von **StorageReferenceId** auf den Namen des zuvor erstellten Richtlinienschlüssels. Beispiel: `B2C_1A_AADAppSecret`.

### <a name="restrict-access"></a>Beschränken des Zugriffs

> [!NOTE]
> Wenn `https://login.microsoftonline.com/` als Wert für **ValidTokenIssuerPrefixes** verwendet wird, können sich alle Azure AD-Benutzer bei Ihrer Anwendung anmelden.

Sie müssen die Liste der gültigen Tokenaussteller aktualisieren und den Zugriff auf eine bestimmte Liste von Azure AD-Mandanten beschränken, über die sich Benutzer anmelden können.

Zum Abrufen der Werte müssen Sie sich die OpenID Connect-Ermittlungsmetadaten für jeden einzelnen Azure AD-Mandanten ansehen, über den sich die Benutzer anmelden können sollen. Das Format der Metadaten-URL lautet in etwa `https://login.microsoftonline.com/your-tenant/v2.0/.well-known/openid-configuration`, wobei `your-tenant` der Name Ihres Azure AD-Mandanten ist. Beispiel:

`https://login.microsoftonline.com/fabrikam.onmicrosoft.com/v2.0/.well-known/openid-configuration`

Führen Sie die folgenden Schritte für jeden Azure AD-Mandanten aus, der für die Anmeldung verwendet werden soll:

1. Öffnen Sie Ihren Browser, und navigieren Sie zur OpenID Connect-Metadaten-URL für den Mandanten. Suchen Sie nach dem **issuer**-Objekt, und notieren Sie sich dessen Wert. Dies sollte in etwa wie folgt aussehen: `https://login.microsoftonline.com/00000000-0000-0000-0000-000000000000/`.
1. Kopieren Sie den Wert, und fügen Sie ihn für den Schlüssel **ValidTokenIssuerPrefixes** ein. Trennen Sie mehrere Aussteller durch ein Komma. Ein Beispiel mit zwei Ausstellern finden Sie im vorherigen `ClaimsProvider`-XML-Beispiel.

### <a name="upload-the-extension-file-for-verification"></a>Hochladen der Erweiterungsdatei zur Überprüfung

Nun haben Sie Ihre Richtlinie so konfiguriert, dass Azure AD B2C mit Ihren Azure AD-Verzeichnissen kommunizieren kann. Versuchen Sie, die Erweiterungsdatei Ihrer Richtlinie hochzuladen, um sich zu vergewissern, dass soweit keine Probleme vorliegen.

1. Wählen Sie in Ihrem Azure AD B2C-Mandanten auf der Seite **Benutzerdefinierte Richtlinien** die Option **Richtlinie hochladen** aus.
2. Aktivieren Sie **Richtlinie überschreiben, sofern vorhanden**, navigieren Sie dann zur Datei *TrustFrameworkExtensions.xml*, und wählen Sie die Datei aus.
3. Wählen Sie die Option **Hochladen**.

## <a name="register-the-claims-provider"></a>Registrieren des Anspruchsanbieters

An dieser Stelle wurde der Identitätsanbieter eingerichtet, ist jedoch auf keinem der Registrierungs-/Anmeldebildschirme vorhanden. Um ihn verfügbar zu machen, erstellen Sie ein Duplikat einer vorhandenen User Journey-Vorlage und ändern diese dann so, dass sie ebenfalls Azure AD als Identitätsanbieter aufweist.

1. Öffnen Sie die Datei *TrustFrameworkBase.xml* aus dem Starter Pack.
2. Suchen und kopieren Sie den gesamten Inhalt des **UserJourney**-Elements, das `Id="SignUpOrSignIn"` enthält.
3. Öffnen Sie die Datei *TrustFrameworkExtensions.xml*, und suchen Sie nach dem **UserJourneys**-Element. Wenn das Element nicht vorhanden ist, fügen Sie ein solches hinzu.
4. Fügen Sie den gesamten Inhalt des kopierten **UserJourney**-Element als untergeordnetes Element des **UserJourneys**-Elements ein.
5. Benennen Sie die ID der User Journey um. Beispiel: `SignUpSignInContoso`.

### <a name="display-the-button"></a>Anzeigen der Schaltfläche

Das **ClaimsProviderSelection**-Element entspricht einer Schaltfläche für einen Identitätsanbieter auf einem Registrierungs- oder Anmeldebildschirm. Wenn Sie ein **ClaimsProviderSelection**-Element für Azure AD hinzufügen, wird eine neue Schaltfläche angezeigt, wenn ein Benutzer zu der Seite gelangt.

1. Suchen Sie nach dem **OrchestrationStep**-Element, das `Order="1"` in der User Journey enthält, die Sie in der Datei *TrustFrameworkExtensions.xml* erstellt haben.
1. Fügen Sie unter **ClaimsProviderSelects** das folgende Element hinzu. Legen Sie den Wert von **TargetClaimsExchangeId** auf einen geeigneten Wert (z.B. auf `AzureADExchange`) fest:

    ```XML
    <ClaimsProviderSelection TargetClaimsExchangeId="AzureADExchange" />
    ```

### <a name="link-the-button-to-an-action"></a>Verknüpfen der Schaltfläche mit einer Aktion

Nachdem Sie eine Schaltfläche implementiert haben, müssen Sie sie mit einer Aktion verknüpfen. In diesem Fall soll bei der Aktion Azure AD B2C mit Azure AD kommunizieren, um ein Token zu empfangen. Verknüpfen Sie die Schaltfläche mit einer Aktion, indem Sie das technische Profil für Ihren Azure AD-Anspruchsanbieter verknüpfen.

1. Suchen Sie nach dem Element **OrchestrationStep**, das `Order="2"` in der User Journey enthält.
2. Fügen Sie das folgende **ClaimsExchange**-Element hinzu, um sicherzustellen, dass Sie für **Id** den gleichen Wert verwenden, den Sie für **TargetClaimsExchangeId** verwendet haben:

    ```XML
    <ClaimsExchange Id="AzureADExchange" TechnicalProfileReferenceId="Common-AAD" />
    ```

    Ändern Sie den Wert von **TechnicalProfileReferenceId** in die **Id** des technischen Profils, das Sie zuvor erstellt haben. Beispiel: `Common-AAD`.

3. Speichern Sie die Datei *TrustFrameworkExtensions.xml*, und laden Sie die Datei zur Überprüfung erneut hoch.

## <a name="create-an-azure-ad-b2c-application"></a>Erstellen einer Azure AD B2C-Anwendung

Die Kommunikation mit Azure AD B2C erfolgt über eine Anwendung, die Sie in Ihrem B2C-Mandanten registrieren. In diesem Abschnitt werden optionale Schritte aufgeführt, die Sie ausführen können, um eine Testanwendung zu erstellen, falls Sie dies noch nicht getan haben.

[!INCLUDE [active-directory-b2c-appreg-idp](../../includes/active-directory-b2c-appreg-idp.md)]

## <a name="update-and-test-the-relying-party-file"></a>Aktualisieren und Testen der Datei der vertrauenden Seite

Aktualisieren Sie die Datei der vertrauenden Seite (Relying Party, RP), mit der die User Journey initiiert wird, die Sie erstellt haben:

1. Erstellen Sie in Ihrem Arbeitsverzeichnis eine Kopie der Datei *SignUpOrSignIn.xml*, und benennen Sie sie um. Benennen Sie die Datei z. B. in *SignUpSignInContoso.xml* um.
1. Öffnen Sie die neue Datei, und aktualisieren Sie den Wert des Attributs **PolicyId** für **TrustFrameworkPolicy** mit einem eindeutigen Wert. Beispiel: `SignUpSignInContoso`.
1. Aktualisieren Sie den Wert von **PublicPolicyUri** mit dem URI für die Richtlinie. Beispiel: `http://contoso.com/B2C_1A_signup_signin_contoso`.
1. Aktualisieren Sie den Wert des Attributs **ReferenceId** im Element**DefaultUserJourney**,damit dieser der Kennung der User Journey entspricht, die Sie zuvor erstellt haben. Beispielsweise *SignUpSignInContoso*.
1. Speichern Sie die Änderungen, und laden Sie die Datei hoch.
1. Wählen Sie unter **Benutzerdefinierte Richtlinien** die neue Richtlinie in der Liste aus.
1. Wählen Sie in der Dropdownliste **Anwendung auswählen** die Azure AD B2C-Anwendung aus, die Sie zuvor erstellt haben. Zum Beispiel *testapp1*.
1. Kopieren Sie den **Endpunkt für sofortige Ausführung**, und öffnen Sie ihn in einem privaten Browserfenster, z. B. in Google Chrome im Inkognitomodus oder in Microsoft Edge in einem InPrivate-Fenster. Durch das Öffnen in einem privaten Browserfenster können Sie die vollständige User Journey testen, indem Sie keine aktuell zwischengespeicherten Azure AD-Anmeldeinformationen verwenden.
1. Wählen Sie die Schaltfläche für die Azure AD-Anmeldung (z. B. *Contoso-Mitarbeiter*) aus, und geben Sie dann die Anmeldeinformationen für einen Benutzer in einem Ihrer Azure AD-Organisationsmandanten ein. Sie werden aufgefordert, die Anwendung zu autorisieren und dann Informationen für Ihr Profil einzugeben.

Wenn der Anmeldevorgang erfolgreich verlaufen ist, wird der Browser an `https://jwt.ms` umgeleitet, wodurch der Inhalt des von Azure AD B2C zurückgegebenen Tokens angezeigt wird.

Wenn Sie die Funktion der mehrinstanzenfähigen Anmeldung testen möchten, führen Sie die letzten beiden Schritte unter Verwendung der Anmeldeinformationen für einen Benutzer aus, der in einem anderen Azure AD-Mandanten vorhanden ist.

## <a name="next-steps"></a>Nächste Schritte

Beim Arbeiten mit benutzerdefinierten Richtlinien benötigen Sie möglicherweise manchmal zusätzliche Informationen, wenn Sie Probleme mit einer Richtlinie während der Entwicklung beheben.

Um das Diagnostizieren von Problemen zu erleichtern, können Sie die Richtlinie vorübergehend in den „Entwicklermodus“ versetzen und Protokolle mit Azure Application Insights erfassen. Informationen hierzu finden Sie unter [Azure Active Directory B2C: mithilfe von Application Insights](active-directory-b2c-troubleshoot-custom.md).
