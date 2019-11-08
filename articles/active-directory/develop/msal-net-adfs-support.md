---
title: AD FS-Unterstützung in der Microsoft Authentication Library für .NET
titleSuffix: Microsoft identity platform
description: Erfahren Sie mehr über die Unterstützung der Active Directory-Verbunddienste (AD FS) in der Microsoft Authentication Library für .NET (MSAL.NET).
services: active-directory
documentationcenter: dev-center-name
author: TylerMSFT
manager: CelesteDG
editor: ''
ms.service: active-directory
ms.subservice: develop
ms.devlang: na
ms.topic: conceptual
ms.tgt_pltfrm: na
ms.workload: identity
ms.date: 07/16/2019
ms.author: twhitney
ms.reviewer: saeeda
ms.custom: aaddev
ms.collection: M365-identity-device-management
ms.openlocfilehash: 273a8b06b3bd3bf299b47a1cc52fd4660353cec3
ms.sourcegitcommit: c22327552d62f88aeaa321189f9b9a631525027c
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 11/04/2019
ms.locfileid: "73473753"
---
# <a name="active-directory-federation-services-support-in-msalnet"></a>Unterstützung der Active Directory-Verbunddienste in MSAL.NET
Mithilfe der Active Directory-Verbunddienste (AD FS) in Windows Server können Sie den von Ihnen entwickelten Anwendungen die OpenID Connect- und OAuth 2.0-basierte Authentifizierung und Autorisierung hinzufügen. So können Benutzer von diesen Anwendungen direkt gegenüber AD FS authentifiziert werden. Weitere Informationen finden Sie in den [AD FS-Szenarien für Entwickler](/windows-server/identity/ad-fs/overview/ad-fs-openid-connect-oauth-flows-scenarios).

Die Microsoft Authentication Library für .NET (MSAL.NET) unterstützt zwei Szenarien für die Authentifizierung gegenüber AD FS:

- MSAL.NET kommuniziert mit Azure Active Directory, das sich selbst im *Verbund* mit AD FS befindet.
- MSAL.NET kommuniziert **direkt** mit einer ADFS-Autorität. Dies wird nur von AD FS 2019 und höher unterstützt. Eines der Szenarien, die dieses hervorheben, ist [Azure Stack](https://azure.microsoft.com/overview/azure-stack/)-Unterstützung.


## <a name="msal-connects-to-azure-ad-which-is-federated-with-ad-fs"></a>MSAL stellt eine Verbindung mit Azure AD her, das sich im Verbund mit AD FS befindet.
MSAL.NET unterstützt Verbindungen mit Azure AD, das die Anmeldung von verwalteten Benutzern (Benutzern, die in Azure AD verwaltet werden) oder von Verbundbenutzern (Benutzern, die von einem anderen Identitätsanbieter verwaltet werden, z. B. AD FS) ausführt. Die Tatsache, dass Benutzer in einem Verbund enthalten sind, ist MSAL.NET nicht bekannt. Daher kommuniziert MSAL.NET mit Azure AD.

Die in diesem Fall verwendete [Autorität](msal-client-application-configuration.md#authority) ist die übliche Autorität (Hostname der Autorität + „tenant“, „common“ oder „organizations“).

### <a name="acquiring-a-token-interactively"></a>Interaktives Abrufen eines Tokens
Wenn Sie die `AcquireTokenInteractive`-Methode aufrufen, verläuft die Benutzeranmeldung normalerweise wie folgt:

1. Der Benutzer gibt seine Konto-ID ein.
2. Azure AD blendet kurz die Meldung „Weiterleitung zur Anmeldeseite Ihrer Organisation“ ein.
3. Der Benutzer wird zur Anmeldeseite des Identitätsanbieters umgeleitet. Die Anmeldeseite ist in der Regel angepasst und weist das Logo der Organisation auf.

In diesem Verbundszenario werden die AD FS-Versionen AD FS v2, AD FS v3 (Windows Server 2012 R2) und AD FS v4 (AD FS 2016) unterstützt.

### <a name="acquiring-a-token-using-acquiretokenbyintegratedauthentication-or-acquiretokenbyusernamepassword"></a>Abrufen eines Tokens mit AcquireTokenByIntegratedAuthentication oder AcquireTokenByUsernamePassword
Beim Abrufen eines Tokens mit der `AcquireTokenByIntegratedAuthentication`-Methode oder `AcquireTokenByUsernamePassword`-Methode ruft MSAL.NET den zu kontaktierenden Identitätsanbieter anhand des Benutzernamens ab.  Nachdem der Identitätsanbieter kontaktiert wurde, empfängt MSAL.NET ein [SAML 1.1-Token](reference-saml-tokens.md).  Anschließend stellt MSAL.NET Azure AD das SAML-Token als Benutzerassertion zur Verfügung (ähnlich wie beim [On-Behalf-Of-Fluss](msal-authentication-flows.md#on-behalf-of)), um wiederum ein JSON Web Token zu erhalten.

## <a name="msal-connects-directly-to-ad-fs"></a>Direkte Verbindung zwischen MSAL und AD FS
MSAL.NET unterstützt Verbindungen mit AD FS 2019, was mit Open ID Connect kompatibel ist und PKCE sowie Bereiche versteht. Diese Unterstützung erfordert, dass ein Service Pack [KB 4490481](https://support.microsoft.com/en-us/help/4490481/windows-10-update-kb4490481) auf Windows Server angewendet wird. Bei einer direkten Verbindung mit AD FS lautet die Autorität, die Sie zum Erstellen Ihrer Anwendung verwenden möchten, ähnlich wie `https://mysite.contoso.com/adfs/`.

Derzeit gibt es keine Pläne, das Herstellen einer direkte Verbindung mit den Folgenden zu unterstützen:

- AD FS 16, da PKCE nicht unterstützt wird und weiterhin Ressourcen verwendet werden, kein Bereich
- AD FS v2, weil keine OIDC-Kompatibilität vorhanden ist.

 Falls Sie Unterstützung für Szenarien benötigen, die eine direkte Verbindung mit AD FS 2016 erfordern, verwenden Sie die neueste Version der [Azure Active Directory-Authentifizierungsbibliothek](active-directory-authentication-libraries.md#microsoft-supported-client-libraries). Wenn Sie Ihr lokales System auf AD FS 2019 aktualisiert haben, können Sie MSAL.NET verwenden.

## <a name="see-also"></a>Weitere Informationen

Informationen zum Verbundfall finden Sie unter [Konfigurieren des Verhaltens der Azure Active Directory-Anmeldung für eine Anwendung mit einer Richtlinie für die Startbereichsermittlung (Home Realm Discovery, HDR)](https://docs.microsoft.com/azure/active-directory/manage-apps/configure-authentication-for-federated-users-portal).
