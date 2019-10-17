---
title: Authentifizierungsflows (Microsoft-Authentifizierungsbibliothek) | Azure
description: Erfahren Sie mehr über die von der Microsoft-Authentifizierungsbibliothek (MSAL) verwendeten Authentifizierungsflüsse/-zuweisungen.
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
ms.date: 10/16/2019
ms.author: twhitney
ms.reviewer: saeeda
ms.custom: aaddev
ms.collection: M365-identity-device-management
ms.openlocfilehash: d41e011fd58c20cbe6d2dc8d9029e645f8851bd9
ms.sourcegitcommit: 12de9c927bc63868168056c39ccaa16d44cdc646
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 10/17/2019
ms.locfileid: "72513036"
---
# <a name="authentication-flows"></a>Authentifizierungsflows

In diesem Artikel werden die verschiedenen Authentifizierungsflows beschrieben, die von der Microsoft-Authentifizierungsbibliothek (Microsoft Authentication Library, MSAL) bereitgestellt werden.  Diese Flows können in vielen verschiedenen Anwendungsszenarien verwendet werden.

| Flow | BESCHREIBUNG | Verwendet für|  
| ---- | ----------- | ------- | 
| [Interactive](#interactive) | Ruft in einem interaktiven Prozess das Token ab, durch das der Benutzer über einen Browser oder ein Popupfenster zur Eingabe von Anmeldeinformationen aufgefordert wird. | [Desktop-Apps](scenario-desktop-overview.md), [mobile Apps](scenario-mobile-overview.md) |
| [Implizite Gewährung](#implicit-grant) | Ermöglicht es der App, Token abzurufen, ohne dass die Anmeldeinformationen für den Back-End-Server ausgetauscht werden müssen. Dadurch kann die App den Benutzer anmelden, die Sitzung aufrechterhalten und Token für andere Web-APIs abrufen. All dies geschieht innerhalb des Client-JavaScript-Codes.| [Single-Page-Webanwendungen (SPA)](scenario-spa-overview.md) |
| [Autorisierungscode](#authorization-code) | Wird in Apps verwendet, die auf einem Gerät installiert sind, um auf geschützte Ressourcen wie Web-APIs zuzugreifen. Dadurch können Sie mobile und Desktop-Apps mit Anmeldefunktionen und API-Zugriff ausstatten. | [Desktop-Apps](scenario-desktop-overview.md), [mobile Apps](scenario-mobile-overview.md), [Web-Apps](scenario-web-app-call-api-overview.md) | 
| [OBO (On-Behalf-Of)](#on-behalf-of) | Eine Anwendung ruft eine Dienst- oder Web-API auf, die wiederum eine andere Dienst- oder Web-API aufrufen muss. Die Idee dabei ist, die delegierte Benutzeridentität und Berechtigungen über die Anforderungskette weiterzugeben. | [Web-APIs](scenario-web-api-call-api-overview.md) |
| [Clientanmeldeinformationen](#client-credentials) | Ermöglicht es, über die Identität einer Anwendung auf Ressourcen zugreifen, die im Web gehostet werden. Wird häufig für Interaktionen zwischen Servern verwendet, die ohne direkten Benutzereingriff im Hintergrund ausgeführt werden müssen. | [Daemon-Apps](scenario-daemon-overview.md) |
| [Gerätecode](#device-code) | Ermöglicht es Benutzern, sich bei Geräten mit Eingabeeinschränkung wie einem Smart-TV, IoT-Gerät oder Drucker anzumelden. | [Desktop-/mobile Apps](scenario-desktop-acquire-token.md#command-line-tool-without-web-browser) |
| [Integrierte Windows-Authentifizierung](scenario-desktop-acquire-token.md#integrated-windows-authentication) | Ermöglicht es Anwendungen auf Computern, die in eine Domäne oder in Azure Active Directory (Azure AD) eingebunden sind, ohne Eingriff des Benutzers über die Benutzeroberfläche automatisch ein Token abzurufen.| [Desktop-/mobile Apps](scenario-desktop-acquire-token.md#integrated-windows-authentication) |
| [Benutzername/Kennwort](scenario-desktop-acquire-token.md#username--password) | Ermöglicht es einer Anwendung, den Benutzer durch die direkte Verarbeitung seines Kennworts anzumelden. Von diesem Fluss wird abgeraten. | [Desktop-/mobile Apps](scenario-desktop-acquire-token.md#username--password) |

## <a name="how-each-flow-emits-tokens-and-codes"></a>Ausgabe von Token und Codes für die einzelnen Abläufe
 
Je nach Art Ihres Clients kann einer (oder auch mehrere) der Authentifizierungsabläufe verwendet werden, die von der Microsoft Identity Platform unterstützt werden.  Mit diesen Abläufen können verschiedene Token (ID-Token, Aktualisierungstoken, Zugriffstoken) und Autorisierungscodes erstellt werden, und es sind unterschiedliche Token erforderlich. Dieses Diagramm enthält eine Übersicht:
 
|Flow | Erforderlich | id_token | Zugriffstoken | Aktualisierungstoken | Autorisierungscode | 
|-----|----------|----------|--------------|---------------|--------------------|
|[Autorisierungscodeflow](v2-oauth2-auth-code-flow.md) | | x | x | x | x|  
|[Impliziter Flow](v2-oauth2-implicit-grant-flow.md) | | x        | x    |      |                    |
|[Hybrid-OIDC-Ablauf](v2-protocols-oidc.md#get-access-tokens)| | x  | |          |            x   |
|[Einlösung des Aktualisierungstokens](v2-oauth2-auth-code-flow.md#refresh-the-access-token) | Aktualisierungstoken | x | x | x| |
|[„Im Auftrag von“-Ablauf](v2-oauth2-on-behalf-of-flow.md) | Zugriffstoken| x| x| x| |
|[Gerätecodeflow](v2-oauth2-device-code.md) | | x| x| x| |
|[Clientanmeldeinformationen](v2-oauth2-client-creds-grant-flow.md) | | | x (nur App)| | |
 
Für Token, die im impliziten Modus ausgestellt werden, gilt eine Längenbeschränkung, weil sie per URL zurück an den Browser übergeben werden (`response_mode` ist hierbei `query` oder `fragment`).  Für einige Browser gilt eine Größenbeschränkung für die URL, die in die Browserleiste eingefügt werden kann. Es tritt ein Fehler auf, wenn die URL zu lang ist.  Diese Token verfügen daher nicht über Ansprüche der Art `groups` oder `wids`.

## <a name="interactive"></a>Interactive

MSAL bietet die Möglichkeit, die Anmeldeinformationen interaktiv vom Benutzer abzufragen und mit diesen ein Token abzurufen.

![Diagramm: interaktiver Vorgang](media/msal-authentication-flows/interactive.png)

In den folgenden Artikeln finden Sie weitere Informationen zur Verwendung von MSAL.NET für den interaktiven Abruf von Token für bestimmte Plattformen:
- [Xamarin Android](msal-net-xamarin-android-considerations.md)
- [Xamarin iOS](msal-net-xamarin-ios-considerations.md)
- [Universelle Windows-Plattform](msal-net-uwp-considerations.md)

Weitere Informationen zu interaktiven Aufrufen in MSAL.js finden Sie unter [Prompt behavior in MSAL.js interactive requests (Aufforderungsverhalten in interaktiven MSAL.js-Anforderungen)](msal-js-prompt-behavior.md).

## <a name="implicit-grant"></a>Implicit grant (Implizite Gewährung)

MSAL unterstützt den [OAuth 2-Fluss für implizite Genehmigungen](v2-oauth2-implicit-grant-flow.md), wodurch die App Token von Microsoft Identity Platform abrufen kann, ohne dass die Anmeldeinformationen für den Back-End-Server ausgetauscht werden müssen. Dadurch kann die App den Benutzer anmelden, die Sitzung aufrechterhalten und Token für andere Web-APIs abrufen. All dies geschieht innerhalb des Client-JavaScript-Codes.

![Diagramm: Fluss für implizite Genehmigung](media/msal-authentication-flows/implicit-grant.svg)

Viele moderne Webanwendungen werden als clientseitige Single-Page-Webanwendungen erstellt, die mithilfe von JavaScript- oder SPA-Frameworks wie Angular, Vue.js und React.js geschrieben werden. Diese Anwendungen werden in einem Webbrowser ausgeführt und haben andere Authentifizierungsmerkmale als herkömmliche serverseitige Webanwendungen. Microsoft Identity Platform ermöglicht Single-Page-Webanwendungen mithilfe des Flusses für implizite Genehmigungen das Anmelden von Benutzern und Abrufen von Token für den Zugriff auf Back-End-Dienste oder -Web-APIs. Durch den impliziten Fluss kann die Anwendung ID-Token abrufen, um den authentifizierten Benutzer darzustellen, sowie Zugriffstoken, die zum Aufrufen geschützter APIs erforderlich sind.

Dieser Authentifizierungsfluss umfasst keine Anwendungsszenarios, in denen plattformübergreifende JavaScript-Frameworks wie Electron und React-Native verwendet werden, da sie weitere Funktionen für die Interaktion mit den nativen Plattformen erfordern.

## <a name="authorization-code"></a>Authorization code (Autorisierungscode)

MSAL unterstützt die [OAuth 2-Zuweisung eines Autorisierungscodes](v2-oauth2-auth-code-flow.md). Diese Zuweisung wird in Apps verwendet, die auf einem Gerät installiert sind, um auf geschützte Ressourcen wie Web-APIs zuzugreifen. Dadurch können Sie mobile und Desktop-Apps mit Anmeldefunktionen und API-Zugriff ausstatten. 

Wenn sich Benutzer bei Webanwendungen (Websites) anmelden, erhält die Webanwendung einen Autorisierungscode.  Der Autorisierungscode wird eingelöst, um ein Token zum Aufrufen von Web-APIs abzurufen. In ASP.NET- und ASP.NET Core-Web-Apps besteht die einzige Aufgabe von `AcquireTokenByAuthorizationCode` darin, dem Tokencache ein Token hinzuzufügen. Das Token kann danach von der Anwendung verwendet werden (in der Regel in den Controllern, die nur ein Token für eine API mit `AcquireTokenSilent` abrufen).

![Diagramm: Autorisierungscodefluss](media/msal-authentication-flows/authorization-code.png)

Im obigen Diagramm führt die Anwendung folgende Vorgänge aus:

1. Fordert einen Autorisierungscode an, der gegen ein Zugriffstoken eingelöst wird.
2. Verwendet das Zugriffstoken zum Aufrufen einer Web-API.

### <a name="considerations"></a>Überlegungen

- Sie können den Autorisierungscode nur einmal zum Einlösen eines Tokens verwenden. Versuchen Sie nicht, ein Token mehrmals mit demselben Autorisierungscode abzurufen. Dies ist laut Protokollstandardspezifikation explizit untersagt. Wenn Sie den Code absichtlich mehrmals einlösen oder sich nicht bewusst sind, dass der Code ebenfalls von einem Framework für Sie eingelöst wird, erhalten Sie die folgende Fehlermeldung: `AADSTS70002: Error validating credentials. AADSTS54005: OAuth2 Authorization code was already redeemed, please retry with a new valid code or use an existing refresh token.`

- Beim Schreiben einer ASP.NET- oder ASP.NET Core-Anwendung kann dies geschehen, wenn Sie das Framework nicht darüber informieren, dass Sie den Autorisierungscode bereits eingelöst haben. Dazu müssen Sie die `context.HandleCodeRedemption()`-Methode des `AuthorizationCodeReceived`-Ereignishandlers aufrufen.

- Sie sollten das Zugriffstoken nicht mit ASP.NET freigeben, weil dadurch verhindert werden kann, dass die inkrementelle Zustimmung ordnungsgemäß abläuft. Weitere Informationen finden Sie unter [Problem Nr. 693](https://github.com/AzureAD/microsoft-authentication-library-for-dotnet/issues/693).

## <a name="on-behalf-of"></a>OBO (On-Behalf-Of)

MSAL unterstützt den [OAuth 2-On-Behalf-Of-Authentifizierungsflow](v2-oauth2-on-behalf-of-flow.md).  Dieser Fluss wird verwendet, wenn eine Anwendung eine Dienst- oder Web-API aufruft, die wiederum eine andere Dienst- oder Web-API aufrufen muss. Die Idee dabei ist, die delegierte Benutzeridentität und Berechtigungen über die Anforderungskette weiterzugeben. Damit der Dienst auf der mittleren Ebene Authentifizierungsanforderungen an den Downstreamdienst stellen kann, muss für den Benutzer ein Zugriffstoken der Microsoft Identity Platform gesichert werden.

![Diagramm: OBO-Fluss](media/msal-authentication-flows/on-behalf-of.png)

Im obigen Diagramm ist Folgendes zu sehen:

1. Die Anwendung ruft ein Zugriffstoken für die Web-API ab.
2. Ein Client (Webanwendung, Desktopanwendung, mobile Anwendung oder Single-Page-Webanwendung) ruft eine geschützte Web-API auf und fügt dabei das Zugriffstoken als Bearertoken in den Authentifizierungsheader der HTTP-Anforderung ein. Die Web-API authentifiziert den Benutzer.
3. Wenn der Client die Web-API aufruft, fordert diese ein weiteres Token im Namen des (On-Behalf-Of) Benutzers an.  
4. Die geschützte Web-API verwendet dieses Token, um eine Downstream-Web-API im Namen des Benutzers aufzurufen.  Die Web-API kann später auch Token für andere Downstream-APIs anfordern (allerdings immer noch im Namen desselben Benutzers).

## <a name="client-credentials"></a>Client credentials (Clientanmeldeinformationen)

MSAL unterstützt den [OAuth 2-Clientanmeldeinformations-Flow](v2-oauth2-client-creds-grant-flow.md). Mit diesem Flow können Sie über die Identität einer Anwendung auf Ressourcen zugreifen, die im Web gehostet werden. Diese Art der Gewährung wird häufig für Interaktionen zwischen Servern verwendet, die ohne Benutzereingriff im Hintergrund ausgeführt werden müssen. Diese Anwendungstypen werden oft als Daemons oder Dienstkonten bezeichnet. 

Beim Fluss zur Zuweisung von Clientanmeldeinformationen kann ein Webdienst (ein vertraulicher Client) seine eigenen Anmeldeinformationen zum Authentifizieren verwenden, wenn ein anderer Webdienst aufgerufen wird, anstatt die Identität eines Benutzers anzunehmen. In diesem Szenario ist der Client normalerweise ein Webdienst der mittleren Ebene, ein Daemondienst oder eine Website. Für ein höheres Maß an Sicherheit bietet die Microsoft Identity Platform auch die Möglichkeit, dass der aufrufende Dienst ein Zertifikat (statt eines gemeinsamen Geheimnisses) als Anmeldeinformationen verwendet.

> [!NOTE]
> Der Fluss für vertrauliche Clients ist auf mobilen Plattformen (UWP, Xamarin.iOS und Xamarin.Android) nicht verfügbar, da diese nur öffentliche Clientanwendungen unterstützen. Öffentliche Clientanwendungen sind nicht in der Lage, die Identität der Anwendung gegenüber dem Identitätsanbieter nachzuweisen. Sie können ein Zertifikat bereitstellen, um eine sichere Verbindung für Web-App- oder Web-API-Back-Ends zu erzielen.

MSAL.NET unterstützt zwei Arten von Clientanmeldeinformationen. Diese Clientanmeldeinformationen müssen in Azure AD registriert werden. Die Anmeldeinformationen werden an die Konstruktoren der vertraulichen Clientanwendung im Code übergeben.

### <a name="application-secrets"></a>Anwendungsgeheimnisse

![Diagramm: vertraulicher Client mit Kennwort](media/msal-authentication-flows/confidential-client-password.png)

Im obigen Diagramm führt die Anwendung folgende Vorgänge aus:

1. Sie ruft ein Token mithilfe der Anmeldeinformationen ab, die aus einem Anwendungsgeheimnis oder Kennwort bestehen.
2. Verwendet das Token für Anforderungen an die Ressource.

### <a name="certificates"></a>Zertifikate

![Diagramm: vertraulicher Client mit Zertifikat](media/msal-authentication-flows/confidential-client-certificate.png)

Im obigen Diagramm führt die Anwendung folgende Vorgänge aus:

1. Sie ruft ein Token mithilfe der Zertifikatanmeldeinformationen ab.
2. Verwendet das Token für Anforderungen an die Ressource.

Die Clientanmeldeinformationen müssen folgende Voraussetzungen erfüllen:
- Sie wurden in Azure AD registriert.
- Sie wurden bei der Erstellung der vertraulichen Clientanwendung im Code übergeben.

## <a name="device-code"></a>Gerätecode

MSAL unterstützt den [OAuth 2-Gerätecodefluss](v2-oauth2-device-code.md), der es Benutzern ermöglicht, sich bei Geräten mit Eingabeeinschränkung wie einem Smart-TV, einem IoT-Gerät oder einem Drucker anzumelden. Für die interaktive Authentifizierung mit Azure AD wird ein Webbrowser benötigt. Der Gerätecodefluss ermöglicht es dem Benutzer, ein anderes Gerät (z. B. einen anderen Computer oder ein anderes Mobiltelefon) zu verwenden, um sich interaktiv anzumelden, wenn das Gerät oder Betriebssystem keinen Webbrowser bereitstellt.

Mithilfe des Gerätecodeflusses ruft die Anwendung Token in einem zweistufigen Prozess ab, der speziell für diese Geräte oder Betriebssysteme entwickelt wurde. Beispiele sind Anwendungen auf IoT-Geräten oder Befehlszeilentools (CLI-Tools). 

![Diagramm: Gerätecodefluss](media/msal-authentication-flows/device-code.png)

Im obigen Diagramm ist Folgendes zu sehen:

1. Sobald eine Benutzerauthentifizierung erforderlich ist, stellt die App einen Code bereit und fordert den Benutzer auf, mit einem anderen Gerät (z. B. einem Smartphone mit Internetverbindung) eine URL (z. B. https://microsoft.com/devicelogin) ) aufzurufen. Der Benutzer wird anschließend aufgefordert, den Code einzugeben, und führt danach wie gewohnt den Authentifizierungsvorgang durch. Dabei sind ggf. auch Zustimmungsaufforderungen und eine mehrstufige Authentifizierung erforderlich.

2. Nach erfolgreicher Authentifizierung empfängt die Befehlszeilen-App die erforderlichen Token über einen Backchannel und führt damit die benötigten Web-API-Aufrufe aus.

### <a name="constraints"></a>Einschränkungen

- Der Gerätecodeflow ist nur in öffentlichen Clientanwendungen verfügbar.
- Die beim Erstellen der öffentlichen Clientanwendung übergebene Autorität muss eine der folgenden Voraussetzungen erfüllen:
  - Sie muss auf Mandanten beruhen (im Format `https://login.microsoftonline.com/{tenant}/`, wobei `{tenant}` entweder die GUID ist, die die Mandanten-ID darstellt, oder eine Domäne, die dem Mandanten zugeordnet ist).
  - Sie muss einem Geschäfts-, Schul- oder Unikonto entsprechen (`https://login.microsoftonline.com/organizations/`).
- Persönliche Microsoft-Konten werden vom Azure AD v2.0-Endpunkt noch nicht unterstützt (Mandanten vom Typ `/common` oder `/consumers` können nicht verwendet werden).

## <a name="integrated-windows-authentication"></a>Integrierte Windows-Authentifizierung

MSAL unterstützt die integrierte Windows-Authentifizierung (IWA) für Desktop- oder mobile Anwendungen auf Windows-Computern, die in eine Domäne oder in Azure AD eingebunden sind. Mit IWA können diese Anwendungen automatisch ein Token abrufen (ohne dass der Benutzer mit der Benutzeroberfläche interagieren muss). 

![Diagramm: integrierte Windows-Authentifizierung](media/msal-authentication-flows/integrated-windows-authentication.png)

Im obigen Diagramm führt die Anwendung folgende Vorgänge aus:

1. Sie ruft ein Token mithilfe der integrierten Windows-Authentifizierung ab.
2. Verwendet das Token für Anforderungen an die Ressource.

### <a name="constraints"></a>Einschränkungen

Die IWA unterstützt ausschließlich Verbundbenutzer, also Benutzer, die in Active Directory Domain Services erstellt und von Azure AD unterstützt werden. Direkt in Azure AD erstellte Benutzer ohne Active Directory Domain Services-Unterstützung (d. h. verwaltete Benutzer) können diesen Authentifizierungsfluss nicht verwenden. Diese Einschränkung wirkt sich nicht auf den [Benutzername/Kennwort-Fluss](#usernamepassword) aus.

IWA ist für Apps bestimmt, die für die .NET Framework- und .NET Core-Plattform und die universelle Windows-Plattform geschrieben wurden.

Die mehrstufige Authentifizierung (MFA) wird von der IWA nicht umgangen. Wenn eine solche Authentifizierung konfiguriert ist, kann die IWA im Fall einer MFA-Aufforderung fehlschlagen. Die MFA erfordert eine Interaktion mit dem Benutzer.

Sie haben keinen Einfluss darauf, wann der Identitätsanbieter eine MFA anfordert. Diese Einschränkung gilt allerdings nicht für den Mandantenadministrator. Die MFA ist üblicherweise erforderlich, wenn Sie sich beim Anmeldevorgang in einem anderen Land befinden oder sich mit einem Firmennetzwerk verbinden und dabei kein VPN verwenden. Gelegentlich ist allerdings selbst bei Nutzung eines VPN eine MFA notwendig. Azure AD greift auf KI zurück, um zu ermitteln, ob eine MFA erforderlich ist. Wenn die integrierte Windows-Authentifizierung zu einem Fehler führt, sollten Sie auf eine [interaktive Benutzeraufforderung](#Interactive) zurückgreifen.

Die beim Erstellen der öffentlichen Clientanwendung übergebene Autorität muss eine der folgenden Voraussetzungen erfüllen:
- Sie muss auf Mandanten beruhen (im Format `https://login.microsoftonline.com/{tenant}/`, wobei `tenant` entweder die GUID ist, die die Mandanten-ID darstellt, oder eine Domäne, die dem Mandanten zugeordnet ist).
- Sie muss einem Geschäfts-, Schul- oder Unikonto entsprechen (`https://login.microsoftonline.com/organizations/`). Persönliche Microsoft-Konten werden nicht unterstützt (Mandanten vom Typ `/common` oder `/consumers` können nicht verwendet werden).

Da die IWA ein automatischer Fluss ist, muss eine der folgenden Bedingungen zutreffen:
- Der Benutzer muss vorher in die Nutzung Ihrer Anwendung eingewilligt haben. 
- Der Mandantenadministrator muss vorher im Namen aller Benutzer in die Nutzung der Anwendung eingewilligt haben.

Dies bedeutet, dass eine der folgenden Aussagen zutrifft:
- Sie als Entwickler haben im Azure-Portal **Gewähren** für sich selbst ausgewählt.
- Ein Mandantenadministrator hat bei der Anwendungsregistrierung auf der Registerkarte **API-Berechtigungen** die Option **Grant/revoke admin consent for {tenant domain}** (Administratoreinwilligung für {Mandantendomäne} erteilen/widerrufen) ausgewählt (siehe [Add permissions to access web APIs (Hinzufügen von Zugriffsberechtigungen für Web-APIs)](quickstart-configure-app-access-web-apis.md#add-permissions-to-access-web-apis)).
- Sie haben Benutzern die Möglichkeit geboten, in die Nutzung der Anwendung einzuwilligen (siehe [Anfordern der Einwilligung einzelner Benutzer](v2-permissions-and-consent.md#requesting-individual-user-consent)).
- Sie haben dem Mandantenadministrator die Möglichkeit geboten, der Anwendung eine Einwilligung zu erteilen (siehe [Administratoreinwilligung](v2-permissions-and-consent.md#requesting-consent-for-an-entire-tenant)).

Der IWA-Flow ist für .NET-Desktop-, .NET Core- und UWP-Apps (universelle Windows-Plattform) aktiviert. Unter .NET Core ist nur eine Überladung mit dem Benutzernamen möglich. Die .NET Core-Plattform sendet den Benutzernamen nicht an das Betriebssystem.
  
Weitere Informationen zur Zustimmung finden Sie unter [Berechtigungen und Zustimmung für v2.0](v2-permissions-and-consent.md).

## <a name="usernamepassword"></a>Benutzername/Kennwort

MSAL unterstützt die [OAuth 2-Gewährung für Kennwortanmeldeinformationen des Ressourcenbesitzers](v2-oauth-ropc.md). So kann eine Anwendung Benutzer anmelden, indem sie ihr Kennwort direkt verarbeitet. Sie können in Ihrer Desktopanwendung den Benutzername/Kennwort-Flow verwenden, um ein Token automatisch abzurufen. Bei Verwendung der Anwendung ist keine Benutzeroberfläche erforderlich.

![Diagramm: Benutzername/Kennwort-Fluss](media/msal-authentication-flows/username-password.png)

Im obigen Diagramm führt die Anwendung folgende Vorgänge aus:

1. Ruft ein Token ab, indem der Benutzername und das Kennwort an den Identitätsanbieter gesendet werden.
2. Sie ruft eine Web-API mithilfe des Tokens auf.

> [!WARNING]
> Von diesem Fluss wird abgeraten, da er ein hohes Maß an Vertrauenswürdigkeit und eine Offenlegung vieler Benutzerinformationen erfordert.  Verwenden Sie diesen Flow nur, wenn kein anderer Flow verfügbar ist, der mehr Sicherheit bietet. Weitere Informationen finden Sie unter [What's the solution to the growing problem of passwords? (Wie sich das zunehmende Problem der Passwörter lösen lässt.)](https://news.microsoft.com/features/whats-solution-growing-problem-passwords-says-microsoft/). 

Der bevorzugte Fluss für das automatische Abrufen eines Tokens auf Computern in Windows-Domänen ist die [integrierte Windows-Authentifizierung](#integrated-windows-authentication). Alternativ können Sie auch den [Gerätecodefluss](#device-code) verwenden.

Dies ist gelegentlich in DevOps-Szenarios hilfreich. Wenn Sie jedoch Benutzernamen und Kennwörter in interaktiven Szenarios verwenden möchten, in denen Sie eine eigene Benutzeroberfläche bereitstellen, sollten Sie diesen Fluss vermeiden. Die Verwendung von Benutzernamen und Kennwörtern hat folgende Auswirkungen:
- Benutzer, die auf die mehrstufige Authentifizierung angewiesen sind, können sich nicht anmelden, da keine Interaktion stattfindet.
- Die Benutzer können einmaliges Anmelden nicht verwenden.

### <a name="constraints"></a>Einschränkungen

Neben den [Einschränkungen der integrierten Windows-Authentifizierung](#integrated-windows-authentication) gelten zusätzlich folgende Einschränkungen:

- Der Benutzername/Kennwort-Fluss ist nicht kompatibel mit dem bedingten Zugriff und der mehrstufigen Authentifizierung. Wenn also Ihre App in einem Azure AD-Mandanten ausgeführt wird, für den der Mandantenadministrator die mehrstufige Authentifizierung fordert, können Sie diesen Flow nicht nutzen. Dies ist aber in vielen Organisationen der Fall.
- Er funktioniert nur für Geschäfts-, Schul- oder Unikonten (nicht für Microsoft-Konten).
- Der Fluss ist für .NET-Desktop- und .NET Core-Plattformen, aber nicht für die Universelle Windows-Plattform verfügbar.

### <a name="azure-ad-b2c-specifics"></a>Spezifische Informationen zu Azure AD B2C

Weitere Informationen zur Verwendung von MSAL.NET und Azure AD B2C finden Sie unter [Verwenden von ROPC mit Azure AD B2C (MSAL.NET)](msal-net-aad-b2c-considerations.md#resource-owner-password-credentials-ropc-with-azure-ad-b2c).
