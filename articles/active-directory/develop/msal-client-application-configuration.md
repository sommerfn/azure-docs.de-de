---
title: Konfigurieren von Clientanwendungen (Microsoft-Authentifizierungsbibliothek)
titleSuffix: Microsoft identity platform
description: Erfahren Sie mehr über Konfigurationsoptionen für öffentliche und vertrauliche Clientanwendungen in der Microsoft-Authentifizierungsbibliothek (MSAL).
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
ms.date: 09/27/2019
ms.author: twhitney
ms.reviewer: saeeda
ms.custom: aaddev
ms.collection: M365-identity-device-management
ms.openlocfilehash: 5d9a18f1ab5b86cbdb74a61a9520e89ecf920478
ms.sourcegitcommit: be8e2e0a3eb2ad49ed5b996461d4bff7cba8a837
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 10/23/2019
ms.locfileid: "72803190"
---
# <a name="application-configuration-options"></a>Anwendungskonfigurationsoptionen

In Ihrem Code initialisieren Sie eine neue öffentliche oder vertrauliche Clientanwendung (oder bei MSAL.js einen Benutzer-Agent) zum Authentifizieren und Abrufen von Token. Sie können eine Reihe von Konfigurationsoptionen festlegen, wenn Sie die Client-App in der Microsoft-Authentifizierungsbibliothek (Microsoft Authentication Library, MSAL) initialisieren. Diese Optionen lassen sich in zwei Gruppen einteilen:

- Registrierungsoptionen, einschließlich:
    - [Autorität](#authority) (bestehend aus der [Instanz](#cloud-instance) eines Identitätsanbieters und der [Zielgruppe](#application-audience) für die Anmeldung bei der App sowie darüber hinaus möglicherweise der Mandanten-ID).
    - [Client-ID](#client-id).
    - [Umleitungs-URI](#redirect-uri).
    - [Geheimer Clientschlüssel](#client-secret) (für vertrauliche Clientanwendungen).
- [Protokollierungsoptionen](#logging), einschließlich Protokollebene, Kontrolle über personenbezogene Daten und des Namens der Komponente, die die Bibliothek verwendet

## <a name="authority"></a>Authority

Die Autorität ist eine URL, die ein Verzeichnis angibt, aus dem die MSAL Token anfordern kann. Gängige Autoritäten sind folgende:

- https\://login.microsoftonline.com/\<Mandant\>/, wobei &lt;Mandant&gt; die Mandanten-ID des Azure AD-Mandanten (Azure Active Directory) oder eine Domäne ist, die diesem Azure AD-Mandanten zugeordnet ist. Diese wird nur für die Anmeldung von Benutzern einer bestimmten Organisation verwendet.
- https\://login.microsoftonline.com/common/. Hiermit werden Benutzer mit Geschäfts-, Schul- und Unikonten oder persönlichen Microsoft-Konten angemeldet.
- https\://login.microsoftonline.com/organizations/. Damit werden Benutzer mit Geschäfts-, Schul- oder Unikonten angemeldet.
- https\://login.microsoftonline.com/consumers/. Hiermit werden Benutzer angemeldet, die nur über ein persönliches Microsoft-Konto verfügen (früher als Windows Live ID-Konto bezeichnet).

Die Einstellung für die Autorität muss mit den Angaben im Anwendungsregistrierungsportal übereinstimmen.

Die Autoritäts-URL besteht aus der Instanz und der Zielgruppe.

Mögliche Autoritäten:
- Eine Azure AD-Cloudautorität.
- Eine Azure AD B2C-Autorität. Siehe [B2C-Besonderheiten](https://github.com/AzureAD/microsoft-authentication-library-for-dotnet/wiki/AAD-B2C-specifics).
- Eine AD FS-Autorität (Active Directory Federation Services, Active Directory-Verbunddienste). Weitere Informationen finden Sie unter [ADFS support](https://aka.ms/msal-net-adfs-support) (AD FS-Unterstützung).

Azure AD-Cloudautoritäten bestehen aus zwei Komponenten:
- Die *Instanz* eines Identitätsanbieters
- Die *Zielgruppe* für die Anmeldung bei der App

Die Instanz und die Zielgruppe können verkettet und als Autoritäts-URL angegeben werden. In MSAL.NET-Versionen vor MSAL 3.*x* mussten Sie die Autorität auf Basis der Zielcloud und der Anmeldezielgruppe selbst zusammenstellen.  Das folgende Diagramm zeigt, woraus sich die Autoritäts-URL zusammensetzt:

![Zusammensetzung der Autoritäts-URL](media/msal-client-application-configuration/authority.png)

## <a name="cloud-instance"></a>Cloudinstanz

Mit der *Instanz* geben Sie an, ob sich Benutzer aus der öffentlichen Azure-Cloud oder aus nationalen Clouds bei Ihrer App anmelden. Wenn Sie in Ihrem Code die MSAL verwenden, können Sie die Azure-Cloudinstanz mithilfe einer Enumeration oder durch Übergabe der URL an die [Instanz einer nationalen Cloud](authentication-national-cloud.md#azure-ad-authentication-endpoints) als `Instance`-Element (sofern bekannt) festlegen.

MSAL.NET löst eine explizite Ausnahme aus, wenn sowohl `Instance` als auch `AzureCloudInstance` angegeben werden.

Wenn Sie keine Instanz angeben, wird als Ziel Ihrer App eine Instanz der öffentlichen Azure-Cloud (die Instanz der URL `https://login.onmicrosoftonline.com`) angenommen.

## <a name="application-audience"></a>Anwendungszielgruppe

Die Zielgruppe für die Anmeldung richtet sich nach den geschäftlichen Anforderungen Ihrer App:
- Wenn Sie eine Branchenanwendung entwickeln, erstellen Sie wahrscheinlich eine Einzelinstanzanwendung, die nur in Ihrer Organisation verwendet wird. In diesem Fall müssen Sie die Organisation entweder anhand ihrer Mandanten-ID (der ID Ihrer Azure AD-Instanz) oder anhand eines Domänennamens angeben, der mit der Azure AD-Instanz verknüpft ist.
- Wenn Sie ein unabhängiger Softwareanbieter sind, möchten Sie wahrscheinlich, dass sich Benutzer mit ihren Geschäfts-, Schul- und Unikonten in einer beliebigen Organisation oder in mehreren Organisationen anmelden können (mehrinstanzenfähige App). Möglicherweise möchten Sie aber auch, dass Benutzer sich mit persönlichen Microsoft-Konten anmelden können.

### <a name="how-to-specify-the-audience-in-your-codeconfiguration"></a>Angeben der Zielgruppe im Code bzw. der Konfiguration

Wenn Sie die MSAL in Ihrem Code verwenden, geben Sie die Zielgruppe mit einem der folgenden Werte an:
- Die Zielgruppenenumeration der Azure AD-Autorität
- Die Mandanten-ID, bei der es sich um Folgendes handeln kann:
  - Eine GUID (die ID Ihrer Azure AD-Instanz) – für Einzelinstanzanwendungen
  - Ein Domänenname, der Ihrer Azure AD-Instanz zugeordnet ist (auch für Einzelinstanzanwendungen)
- Einer dieser Platzhalter als Mandanten-ID anstelle der Zielgruppenenumeration der Azure AD-Autorität:
    - `organizations` für mehrinstanzenfähige Anwendungen
    - `consumers` für Benutzeranmeldungen ausschließlich mit persönlichen Konten
    - `common` zum Anmelden von Benutzern mit Geschäfts-, Schul- und Unikonten oder persönlichen Microsoft-Konten

Die MSAL löst eine aussagekräftige Ausnahme aus, wenn Sie sowohl die Zielgruppe der Azure AD-Autorität als auch die Mandanten-ID angeben.

Wenn Sie keine Zielgruppe angeben, verwendet Ihre App Azure AD als Ziel und persönliche Microsoft-Konten als Zielgruppe. (Anders gesagt: Die App verhält sich so, als wäre `common` angegeben.)

### <a name="effective-audience"></a>Effektive Zielgruppe

Die effektive Zielgruppe für Ihre App ist (bei Überschneidungen) mindestens Zielgruppe, die Sie in Ihrer App festlegen, und die Zielgruppe, die in der App-Registrierung angegeben wurde. Tatsächlich können Sie auf der Portalseite [App-Registrierungen](https://aka.ms/appregistrations) die Zielgruppe (unterstützte Kontotypen) für die App angeben. Weitere Informationen finden Sie unter [Quickstart: Registrieren einer Anwendung bei der Microsoft Identity Platform](quickstart-register-app.md).

Um eine App so einzurichten, dass sich Benutzer nur mit persönlichen Microsoft-Konten anmelden können, müssen Sie beide folgenden Einstellungen konfigurieren (dies ist zurzeit die einzige Möglichkeit):
- Legen Sie die Zielgruppe für die App-Registrierung auf `Work and school accounts and personal accounts` fest.
- Legen Sie die Zielgruppe im Code bzw. der Konfiguration auf `AadAuthorityAudience.PersonalMicrosoftAccount` (oder `TenantID` ="consumers") fest.

## <a name="client-id"></a>Client-ID

Die Client-ID ist die eindeutige Anwendungs-ID, die Ihrer App bei der Registrierung von Azure AD zugewiesen wurde.

## <a name="redirect-uri"></a>Umleitungs-URI

Der Umleitungs-URI ist der URI, an den der Identitätsanbieter die Sicherheitstoken zurücksendet.

### <a name="redirect-uri-for-public-client-apps"></a>Umleitungs-URI für öffentliche Client-Apps

Wenn Sie eine öffentliche Client-App entwickeln und die MSAL verwenden, gilt Folgendes:
- Sie verwenden `.WithDefaultRedirectUri()` in Desktop- oder UWP-Anwendungen (MSAL.NET 4.1 und höher). Diese Methode legt die Umleitungs-URI-Eigenschaft der öffentlichen Clientanwendung auf den empfohlenen Standardumleitungs-URI für öffentliche Clientanwendungen fest. 

  Plattform  | Umleitungs-URI  
  ---------  | --------------
  Desktop-App (.NET FW) | `https://login.microsoftonline.com/common/oauth2/nativeclient` 
  UWP | Wert von `WebAuthenticationBroker.GetCurrentApplicationCallbackUri()`. Dies ermöglicht SSO mit dem Browser, indem der Wert auf das Ergebnis von WebAuthenticationBroker.GetCurrentApplicationCallbackUri() festgelegt wird, das Sie aufzeichnen müssen.
  .NET Core | `https://localhost`. Dies ermöglicht dem Benutzer, den Systembrowser für die interaktive Authentifizierung zu verwenden, da .NET Core derzeit keine Benutzeroberfläche für die eingebettete Webansicht hat.

- Sie müssen keinen Umleitungs-URI hinzufügen, wenn Sie eine Xamarin Android- und iOS-Anwendung erstellen, die keinen Broker unterstützt (der Umleitungs-URI wird für Xamarin Android und iOS automatisch auf `msal{ClientId}://auth` gesetzt).

- Sie müssen den Umleitungs-URI auf der Seite [App registrations](https://aka.ms/appregistrations) (App-Registrierungen) konfigurieren:

   ![Umleitungs-URI auf der Seite „App-Registrierungen“](media/msal-client-application-configuration/redirect-uri.png)

Sie können den Umleitungs-URI mit der `RedirectUri`-Eigenschaft überschreiben (beispielsweise dann, wenn Sie Broker verwenden). Im Folgenden finden Sie einige Beispiele für Umleitungs-URIs in diesem Szenario:

- `RedirectUriOnAndroid` = "msauth-5a434691-ccb2-4fd1-b97b-b64bcfbc03fc://com.microsoft.identity.client.sample";
- `RedirectUriOnIos` = $"msauth.{Bundle.ID}://auth";

Weitere Einzelheiten zu iOS finden Sie unter [Migrieren von iOS-Anwendungen mit Microsoft Authenticator von ADAL.NET zu MSAL.NET](msal-net-migration-ios-broker.md) und [Leveraging the broker on iOS](https://github.com/AzureAD/microsoft-authentication-library-for-dotnet/wiki/Leveraging-the-broker-on-iOS) (Verwenden des Brokers in iOS).
Weitere Einzelheiten zu Android finden Sie unter [Brokerauthentifizierung in Android](brokered-auth.md).

### <a name="redirect-uri-for-confidential-client-apps"></a>Umleitungs-URI für vertrauliche Client-Apps

Bei Web-Apps ist der Umleitungs-URI (oder Antwort-URI) der URI, den Azure AD verwendet, um das Token an die Anwendung zurückzusenden. Dies kann der URI der Web-App/Web-API sein, wenn die vertrauliche App diesem Typ entspricht. Der Umleitungs-URI muss in der App-Registrierung registriert werden. Diese Registrierung ist besonders wichtig, wenn Sie eine App bereitstellen, die Sie zuerst lokal getestet haben. In diesem Fall müssen Sie die Antwort-URL der bereitgestellten App im Anwendungsregistrierungsportal hinzufügen.

Für Daemon-Apps müssen Sie keinen Umleitungs-URI angeben.

## <a name="client-secret"></a>Geheimer Clientschlüssel

Diese Option gibt den geheimen Clientschlüssel für die vertrauliche Client-App an. Dieser geheime Schlüssel (das App-Kennwort) wird vom Anwendungsregistrierungsportal bereitgestellt oder während der App-Registrierung über PowerShell Azure AD, PowerShell AzureRM oder die Azure CLI an Azure AD übermittelt.

## <a name="logging"></a>Protokollierung

Die weiteren Konfigurationsoptionen aktivieren die Protokollierung und Problembehandlung. Informationen zur Verwendung finden Sie im Artikel [Protokollierung](msal-logging.md).

## <a name="next-steps"></a>Nächste Schritte

Erfahren Sie mehr über das [Instanziieren von Clientanwendungen mithilfe von MSAL.NET](msal-net-initializing-client-applications.md).
Erfahren Sie mehr über das [Instanziieren von Clientanwendungen mithilfe von MSAL.js](msal-js-initializing-client-applications.md).
