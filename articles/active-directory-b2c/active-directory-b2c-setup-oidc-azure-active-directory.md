---
title: 'Einrichten der Anmeldung für eine Azure Active Directory-Organisation: Azure Active Directory B2C'
description: Einrichten der Anmeldung für eine bestimmte Azure Active Directory-Organisation in Azure Active Directory B2C.
services: active-directory-b2c
author: mmacy
manager: celestedg
ms.service: active-directory
ms.workload: identity
ms.topic: conceptual
ms.date: 08/08/2019
ms.author: marsma
ms.subservice: B2C
ms.custom: fasttrack-edit
ms.openlocfilehash: 0c2e368b9c12d8ab673e5b8808632501de448b9a
ms.sourcegitcommit: 8074f482fcd1f61442b3b8101f153adb52cf35c9
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 10/22/2019
ms.locfileid: "72755771"
---
# <a name="set-up-sign-in-for-a-specific-azure-active-directory-organization-in-azure-active-directory-b2c"></a>Einrichten der Anmeldung für eine bestimmte Azure Active Directory-Organisation in Azure Active Directory B2C

Wenn Sie Azure Active Directory (Azure AD) als [Identitätsanbieter](active-directory-b2c-reference-oauth-code.md) in Azure AD B2C verwenden möchten, müssen Sie eine Anwendung für seine Darstellung erstellen. In diesem Artikel erfahren Sie, wie Sie die Anmeldung für Benutzer einer bestimmten Azure AD-Organisation über einen Benutzerflow in Azure AD B2C aktivieren.

## <a name="create-an-azure-ad-app"></a>Erstellen einer Azure AD-App

Um die Anmeldung für Benutzer von einer bestimmten Azure AD-Organisation zu aktivieren, müssen Sie eine Anwendung im Azure AD-Mandanten der Organisation registrieren, der nicht dem Azure AD B2C-Mandanten entspricht.

1. Melden Sie sich beim [Azure-Portal](https://portal.azure.com) an.
2. Stellen Sie sicher, dass Sie das Verzeichnis verwenden, das Ihren Azure AD-Mandanten enthält. Wählen Sie im Hauptmenü den **Verzeichnis- und Abonnementfilter** aus, und wählen Sie das Verzeichnis aus, das Ihren Azure AD-Mandanten enthält. Dieser Mandant ist nicht mit Ihrem Azure AD B2C-Mandanten identisch.
3. Klicken Sie links oben im Azure-Portal auf **Alle Dienste**, suchen Sie nach **App-Registrierungen**, und wählen Sie dann diese Option aus.
4. Wählen Sie **Neue Registrierung** aus.
5. Geben Sie einen Namen für Ihre Anwendung ein. Beispiel: `Azure AD B2C App`.
6. Übernehmen Sie die Auswahl **Accounts in this organizational directory only** (Nur Konten in diesem Organisationsverzeichnis) für diese Anwendung.
7. Übernehmen Sie den Wert **Web** als **Umleitungs-URI**, geben Sie die folgende URL in Kleinbuchstaben ein, und ersetzen Sie dabei `your-B2C-tenant-name` durch den Namen Ihres Azure AD B2C-Mandanten. Beispiel: `https://fabrikam.b2clogin.com/fabrikam.onmicrosoft.com/oauth2/authresp`:

    ```
    https://your-B2C-tenant-name.b2clogin.com/your-B2C-tenant-name.onmicrosoft.com/oauth2/authresp
    ```

    Alle URLs sollten jetzt [b2clogin.com](b2clogin.md) verwenden.

8. Klicken Sie auf **Registrieren**. Kopieren Sie die **Anwendungs-ID (Client)** zur späteren Verwendung.
9. Wählen Sie **Zertifikate & Geheimnisse** im Anwendungsmenü und dann **Neuer geheimer Clientschlüssel** aus.
10. Geben Sie einen Namen für den geheimen Clientschlüssel ein. Beispiel: `Azure AD B2C App Secret`.
11. Wählen Sie den Ablaufzeitraum aus. Übernehmen Sie für diese Anwendung die Auswahl **In 1 Jahr** aus.
12. Klicken Sie auf **Hinzufügen**, und kopieren Sie den Wert des angezeigten neuen geheimen Clientschlüssels zur späteren Verwendung.

## <a name="configure-azure-ad-as-an-identity-provider"></a>Konfigurieren von Azure AD als Identitätsanbieter

1. Stellen Sie sicher, dass Sie das Verzeichnis verwenden, das den Azure AD B2C-Mandanten enthält. Wählen Sie im Hauptmenü den **Verzeichnis- und Abonnementfilter** aus, und wählen Sie das Verzeichnis aus, das Ihren Azure AD B2C-Mandanten enthält.
1. Wählen Sie links oben im Azure-Portal die Option **Alle Dienste** aus, suchen Sie nach **Azure AD B2C**, und wählen Sie dann diese Option aus.
1. Wählen Sie **Identitätsanbieter** und dann **Neuer OpenID Connect-Anbieter** aus.
1. Geben Sie einen **Namen** ein. Geben Sie beispielsweise *Contoso Azure AD* ein.
1. Geben Sie für **Metadaten-URL** die folgende URL ein, und ersetzen Sie dabei `your-AD-tenant-domain` durch den Domänennamen Ihres Azure AD-Mandanten.

    ```
    https://login.microsoftonline.com/your-AD-tenant-domain/.well-known/openid-configuration
    ```

    Beispiel: `https://login.microsoftonline.com/contoso.onmicrosoft.com/.well-known/openid-configuration`.

    Verwenden Sie **nicht** den Azure AD v2.0-Metadatenendpunkt (beispielsweise `https://login.microsoftonline.com/contoso.onmicrosoft.com/v2.0/.well-known/openid-configuration`). Andernfalls tritt bei der Anmeldung ein Fehler wie der folgende auf: `AADB2C: A claim with id 'UserId' was not found, which is required by ClaimsTransformation 'CreateAlternativeSecurityId' with id 'CreateAlternativeSecurityId' in policy 'B2C_1_SignUpOrIn' of tenant 'contoso.onmicrosoft.com'`.

1. Geben Sie für **Client-ID** die zuvor notierte Anwendungs-ID ein.
1. Geben Sie im Feld **Geheimer Clientschlüssel** den zuvor notierten geheimen Clientschlüssel ein.
1. Belassen Sie die Standardwerte für **Bereich**, **Antworttyp** und **Antwortmodus**.
1. (Optional) Geben Sie einen Wert für **Domänenhinweis** ein. Beispiel: *ContosoAD*. Dieser Wert wird zum Verweis auf diesen Identitätsanbieter mit *domain_hint* in der Anforderung verwendet.
1. Geben Sie unter **Identitätsanbieter für die Anspruchszuordnung** die folgenden Werte für die Anspruchszuordnung ein:

    * **Benutzer-ID**: *oid*
    * **Anzeigename**: *name*
    * **Vorname**: *given_name*
    * **Nachname**: *family_name*
    * **E-Mail**: *unique_name*

1. Wählen Sie **Speichern** aus.
