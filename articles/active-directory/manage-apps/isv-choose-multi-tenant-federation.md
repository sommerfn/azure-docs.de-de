---
title: Auswählen des richtigen Verbundprotokolls für Ihre mehrinstanzenfähige Anwendung
description: Leitfaden für unabhängige Softwarehersteller zur Integration mit Azure Active Directory
services: active-directory
author: barbaraselden
manager: CelesteDG
ms.service: active-directory
ms.subservice: app-mgmt
ms.topic: conceptual
ms.workload: identity
ms.date: 05/22/2019
ms.author: baselden
ms.reviewer: jeeds
ms.collection: M365-identity-device-management
ms.openlocfilehash: ede458e7d4c1cb1a8d7e3f2e2c9df54d5925d6d8
ms.sourcegitcommit: 98ce5583e376943aaa9773bf8efe0b324a55e58c
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 10/30/2019
ms.locfileid: "73175969"
---
# <a name="choose-the-right-federation-protocol-for-your-multi-tenant-application"></a>Auswählen des richtigen Verbundprotokolls für Ihre mehrinstanzenfähige Anwendung

Wenn Sie eine Software als SaaS-Anwendung (Software-as-a-Service) entwickeln, müssen Sie das Verbundprotokoll auswählen, das ihren Anforderungen und den Anforderungen Ihrer Kunden am besten gerecht wird. Diese Entscheidung basiert auf Ihrer Entwicklungsplattform und auf Ihrer Entscheidung für eine Integration mit verfügbaren Daten im Office 365- und Azure AD-Ökosystem Ihrer Kunden.

Sehen Sie die komplette Liste der [verfügbaren Protokolle für SSO-Integrationen](what-is-single-sign-on.md) mit Azure Active Directory ein.
In der folgenden Tabelle werden folgende Protokolle verglichen: 
* Open Authentication 2.0 (OAuth 2.0)
* Open ID Connect (OIDC)
* Security Assertion Markup Language (SAML)
* Webdiensteverbund (WSFed)

| Funktion| OAuth/OIDC| SAML/WSFed |
| - |-|-|
| Webbasiertes einmaliges Anmelden| √| √ |
| Webbasiertes einmaliges Abmelden| √| √ |
| Mobile-basiertes einmaliges Anmelden| √| √* |
| Mobile-basiertes einmaliges Abmelden| √| √* |
| Richtlinien für den bedingten Zugriff für mobile Anwendungen| √| X |
| Nahtlose MFA-Erfahrung für mobile Anwendungen| √| X |
| Zugriff auf Microsoft Graph| √| X |

*Möglich, Microsoft stellt jedoch keine Beispiele und Anweisungen bereit.

## <a name="oauth-20-and-open-id-connect"></a>OAuth 2.0 und Open ID Connect

OAuth 2.0 ist ein [Branchenstandard](https://oauth.net/2/)-Protokoll für die Autorisierung. OIDC (OpenID Connect) ist eine [Branchenstandard](https://openid.net/connect/)-Authentifizierungsschicht für Identitäten, das auf dem Oath 2.0-Protokoll aufbaut.

### <a name="benefits"></a>Vorteile

Microsoft empfiehlt die Verwendung von OIDC/OAuth 2.0, da in diese Protokolle Authentifizierung und Autorisierung integriert sind. Bei SAML müssen Sie die Autorisierung zusätzlich implementieren.

Die inhärente Autorisierung in diesen Protokollen ermöglicht Ihrer Anwendung über die Microsoft Graph-API den Zugriff auf und die Integration mit umfassenden Benutzer- und Unternehmensdaten.

Die Verwendung von OAuth 2.0 und OIDC vereinfacht die Endbenutzererfahrung Ihrer Kunden, wenn Sie SSO für Ihre Anwendung einführen. Sie können auf einfache Weise die erforderlichen Berechtigungssätze definieren, die dann automatisch dem zustimmenden Administrator bzw. Endbenutzer präsentiert werden.

Zudem ermöglicht die Verwendung dieser Protokolle Ihren Kunden, mithilfe von Richtlinien für den bedingtem Zugriff und MFA-Richtlinien den Zugriff auf die Anwendungen zu steuern. Microsoft bietet Bibliotheken und [Codebeispiele für mehrere Technologieplattformen](https://github.com/AzureAD/microsoft-authentication-library-for-js/wiki/Samples), um Ihre Entwicklung zu unterstützen.  

### <a name="implementation"></a>Implementierung

Sie registrieren Ihre Anwendung bei Microsoft Identity, einem OAuth 2.0-Anbieter. Anschließend können Sie Ihre OAuth 2.0-basierte Anwendung bei einem beliebigen anderen Identitätsanbieter registrieren, mit dem Sie eine Integration wünschen. 

Informationen zum Registrieren Ihrer Anwendung und Implementieren dieser Protokolle für SSO in Web-Apps finden Sie unter [Autorisieren des Zugriffs auf Webanwendungen mit OpenID Connect und Azure Active Directory](../develop/sample-v2-code.md).  Informationen zum Implementieren dieser Protokolle für SSO in mobilen Apps finden Sie hier: 

* [Android](../develop/quickstart-v2-android.md)

* [iOS](../develop/quickstart-v2-ios.md)

* [Universelle Windows-Plattform](../develop/quickstart-v2-uwp.md)

## <a name="saml-20-and-wsfed"></a>SAML 2.0 und WSFed

SAML (Security Assertion Markup Language) wird normalerweise für Webanwendungen verwendet. Eine Übersicht finden Sie unter [Verwendung des SAML-Protokolls durch Azure AD](../develop/active-directory-saml-protocol-reference.md). 

Webdiensteverbund (WSFed) ist ein [Branchenstandard](https://docs.oasis-open.org/wsfed/federation/v1.2/ws-federation.html), der im Allgemeinen für mit der .NET-Plattform entwickelte Webanwendungen verwendet wird.

### <a name="benefits"></a>Vorteile

SAML 2.0 ist ein ausgereifter Standard, und die meisten Technologieplattformen unterstützen Open-Source-Bibliotheken für SAML 2.0. Sie können für Ihre Kunden eine Verwaltungsschnittstelle zum Konfigurieren von SAML-SSO bereitstellen. Sie können SAML-SSO für Microsoft Azure AD sowie beliebige andere Identitätsanbieter konfigurieren, die SAML 2 unterstützen.

### <a name="trade-offs"></a>Nachteile

Wenn Sie die Protokolle SAML 2.0 oder WSFed für mobile Anwendungen verwenden, ist die Leistung bestimmter Richtlinien für den bedingten Zugriff beeinträchtigt, u. a. gilt dies für die mehrstufige Authentifizierung (MFA). Für den Zugriff auf Microsoft Graph müssen Sie zudem die Autorisierung über OAuth 2.0 implementieren, um die erforderlichen Token zu generieren. 

### <a name="implementation"></a>Implementierung

Microsoft stellt keine Bibliotheken für die SAML-Implementierung bereit und empfiehlt keine bestimmten Bibliotheken. Es stehen viele Open Source-Bibliotheken zur Verfügung.

## <a name="sso-and-using-microsoft-graph-rest-api"></a>SSO und die Verwendung der Microsoft Graph-Rest-API 

Microsoft Graph ist die Data Fabric für das komplette Microsoft 365 (einschließlich von Office 365, Windows 10 sowie Enterprise Mobility and Security) und weitere Produkte wie Dynamics 365. Dies schließt die zentralen Schemas der Entitäten wie z. B. Benutzer, Gruppen, E-Mail, Kalender, Dateien usw. ein, welche die Produktivität der Benutzer fördern. Microsoft Graph bietet Entwicklern drei Schnittstellen: eine REST-basierte API, Microsoft Graph Data Connect und Connectors, mit denen Entwickler Microsoft Graph eigene Daten hinzufügen können.  

Durch Verwendung eines der obigen Protokolle für SSO kann ihre Anwendung auf die umfassenden Daten zugreifen, die über die Microsoft Graph-REST-API verfügbar gemacht werden. Dadurch können Ihre Kunden stärker von ihrer Investition in Microsoft 365 profitieren. Ihre Anwendung kann z. B. die Microsoft Graph-API für die Integration mit der Office 365-Instanz Ihrer Kunden aufrufen und die Microsoft Office- und SharePoint-Objekte der Benutzer in der Anwendung anzeigen. 

Wenn Sie Open ID Connect für die Authentifizierung verwenden, ist Ihre Entwicklungserfahrung nahtlos, denn Token, mit denen Microsoft Graph-APIs aufgerufen werden, werden mit OAuth2 (die Grundlage von Open ID Connect) abgerufen. Wenn Ihre Anwendung SAML oder WSFed verwendet, müssen Sie in der Anwendung zusätzlichen Code hinzufügen, damit OAuth2 für diese die erforderlichen Token zum Aufrufen von Microsoft Graph-APIs abruft. 

## <a name="next-steps"></a>Nächste Schritte

[Aktivieren von SSO für Ihre mehrinstanzenfähige Anwendung](isv-sso-content.md)

[Dokumentation für mehrinstanzenfähige Anwendung erstellen](isv-create-sso-documentation.md)
