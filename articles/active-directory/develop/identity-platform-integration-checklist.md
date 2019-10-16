---
title: Bewährte Methoden für Microsoft Identity Platform | Azure
description: Hier finden Sie bewährten Methoden, Empfehlungen und häufige Fehler im Zusammenhang mit der Microsoft Identity Platform-Integration.
services: active-directory
documentationcenter: ''
author: rwike77
manager: CelesteDG
editor: ''
ms.service: active-directory
ms.subservice: develop
ms.devlang: na
ms.topic: conceptual
ms.tgt_pltfrm: na
ms.workload: identity
ms.date: 09/11/2019
ms.author: ryanwi
ms.reviewer: lenalepa, sureshja, jesakowi
ms.custom: aaddev, identityplatformtop40
ms.openlocfilehash: f7e9b738a55248678a207f0b298ef65e6c2761a4
ms.sourcegitcommit: 824e3d971490b0272e06f2b8b3fe98bbf7bfcb7f
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 10/10/2019
ms.locfileid: "72240148"
---
# <a name="microsoft-identity-platform-best-practices-and-recommendations"></a>Bewährte Methoden und Empfehlungen für Microsoft Identity Platform

Dieser Artikel enthält bewährte Methoden, Empfehlungen und häufige Fehler im Zusammenhang mit der Microsoft Identity Platform-Integration.  Die Prüfliste unterstützt Sie bei einer hochwertigen und sicheren Integration. Überprüfen Sie diese Liste in regelmäßigen Abständen, um die Qualität und Sicherheit der Identity Platform-Integration Ihrer App zu gewährleisten. Die Checkliste ist nicht auf die Überprüfung der gesamten Anwendung ausgerichtet. Der Inhalt der Checkliste kann jederzeit geändert werden, während wir Verbesserungen an der Plattform vornehmen.

Machen Sie sich zum Einstieg anhand der [Dokumentation zu Microsoft Identity Platform](index.yml) unter anderem mit Authentifizierungsgrundlagen und mit Anwendungsszenarien in Microsoft Identity Platform vertraut.

Stellen Sie anhand der folgenden Checkliste sicher, dass Ihre Anwendung effektiv in [Microsoft Identity Platform](https://docs.microsoft.com/azure/active-directory/develop/) integriert wird.

## <a name="basics"></a>Grundlagen

|   |   |
|---|---|
| ![Kontrollkästchen](./media/active-directory-integration-checklist/checkbox-two.svg) | Beschäftigen Sie sich intensiv mit den [Richtlinien für die Microsoft-Plattform](https://go.microsoft.com/fwlink/?linkid=2090497&clcid=0x409). Stellen Sie sicher, dass Ihre Anwendung den dargelegten Bedingungen entspricht, weil diese für den Schutz der Benutzer und der Plattform entwickelt wurden. |

## <a name="ownership"></a>Besitz

|   |   |
|---|---|
| ![Kontrollkästchen](./media/active-directory-integration-checklist/checkbox-two.svg) | Stellen Sie sicher, dass die Informationen, die dem zur Registrierung und Verwaltung von Apps verwendeten Konto zugeordnet sind, auf dem neuesten Stand sind. |

## <a name="branding"></a>Branding

|   |   |
|---|---|
| ![Kontrollkästchen](./media/active-directory-integration-checklist/checkbox-two.svg) | Richten Sie sich nach den [Brandingrichtlinien für Anwendungen](howto-add-branding-in-azure-ad-apps.md). |
| ![Kontrollkästchen](./media/active-directory-integration-checklist/checkbox-two.svg) | Geben Sie einen aussagekräftigen Namen und ein Logo für Ihre Anwendung ein. Diese Informationen werden in der [Zustimmungsaufforderung Ihrer Anwendung](application-consent-experience.md) angezeigt. Stellen Sie sicher, dass Name und Logo für Ihr Unternehmen/Produkt repräsentativ sind, damit Benutzer fundierte Entscheidungen treffen können. Stellen Sie sicher, dass Sie keine Markenrechte verletzen. |

## <a name="privacy"></a>Datenschutz

|   |   |
|---|---|
| ![Kontrollkästchen](./media/active-directory-integration-checklist/checkbox-two.svg) | Geben Sie Links zu den Nutzungsbedingungen und Datenschutzbestimmungen Ihrer App an. |

## <a name="security"></a>Sicherheit

|   |   |
|---|---|
| ![Kontrollkästchen](./media/active-directory-integration-checklist/checkbox-two.svg) | Verwalten Sie Ihre Umleitungs-URIs: <ul><li>Verwalten Sie den Besitz all Ihrer Umleitungs-URIs, und halten Sie die zugehörigen DNS-Einträge auf dem neuesten Stand.</li><li>Verwenden Sie keine Platzhalter (*) in Ihren URIs.</li><li>Stellen Sie für Web-Apps sicher, dass alle URIs sicher und verschlüsselt sind (z.B. mithilfe von HTTPS-Schemas).</li><li>Verwenden Sie für öffentliche Clients ggf. plattformspezifische Umleitungs-URIs (vor allem für iOS und Android). Verwenden Sie andernfalls Umleitungs-URIs mit hohem Zufallsfaktor, um Konflikte beim Rückruf an Ihre App zu vermeiden.</li><li>Wenn Ihre App von einem isolierten Web-Agent aus verwendet wird, können Sie https://login.microsoftonline.com/common/oauth2/nativeclient verwenden.</li><li>Überprüfen und kürzen Sie in regelmäßigen Abständen alle nicht verwendeten oder nicht benötigten Umleitungs-URIs.</li></ul> |
| ![Kontrollkästchen](./media/active-directory-integration-checklist/checkbox-two.svg) | Wenn Ihre App in einem Verzeichnis registriert ist, minimieren Sie die Liste der Besitzer von App-Registrierungen, und überwachen Sie sie manuell. |
| ![Kontrollkästchen](./media/active-directory-integration-checklist/checkbox-two.svg) | Aktivieren Sie die Unterstützung für den [Flow zur impliziten OAuth2-Genehmigung](v2-oauth2-implicit-grant-flow.md) nur dann, wenn dies explizit erforderlich ist. Informationen zu einem gültigen Szenario finden Sie [hier](v1-oauth2-implicit-grant-flow.md#suitable-scenarios-for-the-oauth2-implicit-grant). |
| ![Kontrollkästchen](./media/active-directory-integration-checklist/checkbox-two.svg) | Gehen Sie über Benutzername/Kennwort hinaus. Verwenden Sie keinen [Flow für Ressourcenbesitzer-Kennwortanmeldeinformationen (ROPC)](v2-oauth-ropc.md), in dem die Kennwörter von Benutzern direkt verarbeitet werden. Dieser Flow erfordert ein hohes Maß an Vertrauen und die Freigabe von Benutzeranmeldeinformationen. Er darf nur dann verwendet werden, wenn keine anderen, sichereren Flows genutzt werden können. Bei einigen Szenarien (z.B. DevOps) ist zu beachten, dass dieser Flow zwar weiterhin erforderlich ist, seine Verwendung jedoch Einschränkungen für Ihre Anwendung bedeutet.  Weitere moderne Ansätze finden Sie unter [Authentifizierungsflows und Anwendungsszenarien](authentication-flows-app-scenarios.md).|
| ![Kontrollkästchen](./media/active-directory-integration-checklist/checkbox-two.svg) | Schützen und verwalten Sie Ihre vertraulichen App-Anmeldeinformationen für Web-Apps, Web-APIs und Daemon-Apps. Verwenden Sie [Zertifikatanmeldeinformationen](active-directory-certificate-credentials.md), keine Kennwortanmeldeinformationen (Clientgeheimnisse). Wenn Sie eine Kennwortanmeldeinformation verwenden müssen, legen Sie diese nicht manuell fest. Speichern Sie Anmeldeinformationen nicht im Code oder in der Konfiguration, und lassen Sie sie niemals von Personen bearbeiten. Verwenden Sie möglichst [verwaltete Identitäten für Azure-Ressourcen](https://docs.microsoft.com/azure/active-directory/managed-identities-azure-resources/overview) oder [Azure Key Vault](https://docs.microsoft.com/azure/key-vault/key-vault-whatis) zum Speichern und regelmäßigen Rotieren Ihrer Anmeldeinformationen. |
| ![Kontrollkästchen](./media/active-directory-integration-checklist/checkbox-two.svg) | Stellen Sie sicher, dass Ihre Anwendung den Ansatz der geringsten Rechte verfolgt. Fordern Sie nur Berechtigungen an, die für Ihre Anwendung unbedingt erforderlich sind, und nur dann, wenn sie benötigt werden. Informieren Sie sich über die verschiedenen [Berechtigungstypen](v1-permissions-and-consent.md#types-of-permissions). Anwendungsberechtigungen sollten nur bei Bedarf verwendet werden. Verwenden Sie nach Möglichkeit delegierte Berechtigungen. Eine vollständige Liste der Microsoft Graph-Berechtigungen finden Sie in dieser [Berechtigungsreferenz](https://docs.microsoft.com/graph/permissions-reference). |
| ![Kontrollkästchen](./media/active-directory-integration-checklist/checkbox-two.svg) | Wenn Sie eine API über Microsoft Identity Platform schützen, erwägen Sie sorgfältig, welche Berechtigungen darin verfügbar gemacht werden sollen. Überlegen Sie sich die richtige Granularität für Ihre Lösung und entscheiden Sie, welche Berechtigungen die Einwilligung eines Administrators erfordern. Prüfen Sie auf erwartete Berechtigungen in den eingehenden Token, bevor Sie Autorisierungsentscheidungen treffen. |

## <a name="implementation"></a>Implementierung

|   |   |
|---|---|
| ![Kontrollkästchen](./media/active-directory-integration-checklist/checkbox-two.svg) | Verwenden Sie moderne Authentifizierungslösungen (OAuth 2.0, [OpenID Connect](v2-protocols-oidc.md)) zur sicheren Benutzeranmeldung. |
| ![Kontrollkästchen](./media/active-directory-integration-checklist/checkbox-two.svg) |  Programmieren Sie nicht direkt für Protokolle wie OAuth 2.0 und Open ID. Nutzen Sie stattdessen die [Microsoft-Authentifizierungsbibliothek (Microsoft Authentication Library, MSAL)](msal-overview.md). Die MSAL-Bibliotheken schließen Sicherheitsprotokolle sicher in eine benutzerfreundliche Bibliothek ein und bieten integrierte Unterstützung für Szenarien mit [bedingtem Zugriff](/azure/active-directory/conditional-access/overview), geräteweites [einmaliges Anmelden (Single Sign-On, SSO)](/azure/active-directory/manage-apps/what-is-single-sign-on) sowie integrierte Unterstützung für die Tokenzwischenspeicherung. Weitere Informationen finden Sie in der Liste mit den von Microsoft unterstützten [Clientbibliotheken](reference-v2-libraries.md#microsoft-supported-client-libraries) und [Middlewarebibliotheken](reference-v2-libraries.md#microsoft-supported-server-middleware-libraries) sowie in der Liste mit den [kompatiblen Drittanbieter-Clientbibliotheken](reference-v2-libraries.md#compatible-client-libraries).<br/><br/>Wenn Sie Code für die Authentifizierungsprotokolle schreiben müssen, verwenden Sie eine Methodik wie [Microsoft SDL](https://www.microsoft.com/sdl/default.aspx). Achten Sie auf die Sicherheitsüberlegungen in den Standardspezifikationen für jedes Protokoll.|
| ![Kontrollkästchen](./media/active-directory-integration-checklist/checkbox-two.svg) |  Migrieren Sie bereits vorhandene Apps von der [Azure Active Directory-Authentifizierungsbibliothek (Active Directory Authentication Library, ADAL)](active-directory-authentication-libraries.md) zur [Microsoft-Authentifizierungsbibliothek](msal-overview.md). MSAL ist die neueste Identity Platform-Lösung von Microsoft und ADAL vorzuziehen. Sie steht für .NET und JavaScript zur Verfügung und befindet sich für Android, iOS, Python und Java momentan in der Public Preview-Phase. Ausführlichere Informationen zur App-Migration finden Sie in den Artikeln für [ADAL.NET](msal-net-migration.md), [ADAL.js](msal-compare-msal-js-and-adal-js.md) und [ADAL.NET/iOS-Broker](msal-net-migration-ios-broker.md).|
| ![Kontrollkästchen](./media/active-directory-integration-checklist/checkbox-two.svg) |  Konfigurieren Sie für mobile Apps jede Plattform unter Verwendung der Anwendungsregistrierung. Für Ihre App muss ein Broker-Umleitungs-URI konfiguriert werden, damit sie Microsoft Authenticator oder das Microsoft-Unternehmensportal für einmaliges Anmelden nutzen kann. Dadurch kann Microsoft nach der Authentifizierung die Kontrolle an Ihre Anwendung zurückgeben. Beim Konfigurieren der einzelnen Plattformen führt Sie die App-Registrierung durch den Prozess. Verwenden Sie die Schnellstartanleitung, um ein Beispiel herunterzuladen. Verwenden Sie unter iOS nach Möglichkeit Broker und System WebView.|
| ![Kontrollkästchen](./media/active-directory-integration-checklist/checkbox-two.svg) |  Halten Sie in Web-Apps oder Web-APIs jeweils einen Tokencache pro Konto bereit.  Bei Web-Apps sollte der Tokencache durch die Konto-ID mit einem Schlüssel versehen werden.  Bei Web-APIs sollte das Konto durch den Hash des Tokens, das zum Aufrufen der API verwendet wird, mit einem Schlüssel versehen werden. MSAL.NET bietet eine benutzerdefinierte Tokencache-Serialisierung auf der .NET Framework- und der .NET Core-Unterplattform. Aus Sicherheits- und Leistungsgründen wird empfohlen, jeweils einen Cache pro Benutzer zu serialisieren. Weitere Informationen finden Sie unter [Tokencache für eine Web-App (vertrauliche Clientanwendung)](msal-net-token-cache-serialization.md#token-cache-for-a-web-app-confidential-client-application).|
| ![Kontrollkästchen](./media/active-directory-integration-checklist/checkbox-two.svg) | Wenn die von Ihrer App benötigten Daten über [Microsoft Graph](https://developer.microsoft.com/graph) verfügbar sind, fordern Sie Berechtigungen für diese Daten über den Microsoft Graph-Endpunkt statt über die einzelne API an. |
| ![Kontrollkästchen](./media/active-directory-integration-checklist/checkbox-two.svg) |Betrachten Sie nicht den Wert des Zugriffstoken, und versuchen Sie nicht, ihn als Client zu analysieren.  Werte und Formate können ohne Vorwarnung geändert oder sogar verschlüsselt werden. Verwenden Sie daher immer den id_token-Wert, wenn der Client Informationen zum Benutzer erhalten soll, oder rufen Sie Microsoft Graph auf.  Nur Web-APIs sollten Zugriffstoken analysieren (da über sie das Format definiert wird und die Verschlüsselungsschlüssel festgelegt werden). |

## <a name="end-user-experience"></a>Endbenutzererfahrung

|   |   |
|---|---|
| ![Kontrollkästchen](./media/active-directory-integration-checklist/checkbox-two.svg) | [Beschäftigen Sie sich mit der Einwilligungsoberfläche](application-consent-experience.md), und konfigurieren Sie die Bestandteile der App-Einwilligungsaufforderung so, dass Endbenutzer und Administratoren genügend Informationen erhalten, um über die Vertrauenswürdigkeit Ihrer App zu entscheiden. |
| ![Kontrollkästchen](./media/active-directory-integration-checklist/checkbox-two.svg) | Minimieren Sie, wie häufig ein Benutzer bei Verwendung Ihrer App Anmeldeinformationen eingeben muss, indem vor interaktiven Flows versucht wird, die automatische Authentifizierung (automatischer Tokenabruf) durchzuführen. |
| ![Kontrollkästchen](./media/active-directory-integration-checklist/checkbox-two.svg) | Verwenden Sie nicht „prompt=consent“ für jede Anmeldung. Verwenden Sie „prompt=consent“ nur dann, wenn Sie festgestellt haben, dass für die zusätzlichen Berechtigungen eine Einwilligung erforderlich ist (z.B., wenn Sie erforderliche Berechtigungen Ihrer App geändert haben). |
| ![Kontrollkästchen](./media/active-directory-integration-checklist/checkbox-two.svg) | Reichern Sie Ihre Anwendung ggf. mit Benutzerdaten an. Eine einfache Möglichkeit dazu besteht in der Verwendung der [Microsoft Graph-API](https://developer.microsoft.com/graph). Das Tool [Graph-Explorer](https://developer.microsoft.com/graph/graph-explorer) kann Sie bei den ersten Schritten unterstützen. |
| ![Kontrollkästchen](./media/active-directory-integration-checklist/checkbox-two.svg) | Registrieren Sie den vollständigen Satz an Berechtigungen, die für Ihre App erforderlich sind, damit Administratoren ihre Einwilligung für ihren Mandanten ganz leicht erteilen können. Verwenden Sie die [inkrementelle Einwilligung](azure-ad-endpoint-comparison.md#incremental-and-dynamic-consent) zur Laufzeit. So können Benutzer nachvollziehen, warum Ihre App Berechtigungen anfordert, die Besorgnis oder Verwirrung hervorrufen können, wenn sie gleich beim ersten Start angefordert werden. |
| ![Kontrollkästchen](./media/active-directory-integration-checklist/checkbox-two.svg) | Implementieren Sie eine [saubere Oberfläche für einmaliges Abmelden](https://github.com/Azure-Samples/active-directory-aspnetcore-webapp-openidconnect-v2/tree/master/1-WebApp-OIDC/1-6-SignOut). Dies ist eine Datenschutz- und Sicherheitsanforderung, die eine gute Benutzeroberfläche ausmacht. |

## <a name="testing"></a>Testen

|   |   |
|---|---|
| ![Kontrollkästchen](./media/active-directory-integration-checklist/checkbox-two.svg) | Testen Sie auf [Richtlinien für bedingten Zugriff](https://github.com/Azure-Samples/active-directory-aspnetcore-webapp-openidconnect-v2/tree/master/1-WebApp-OIDC/1-6-SignOut), die die Verwendung Ihrer Anwendung verhindern können. |
| ![Kontrollkästchen](./media/active-directory-integration-checklist/checkbox-two.svg) | Testen Sie Ihre Anwendung mit allen möglichen Konten, die Sie unterstützen möchten (z.B. Geschäfts-, Schul- oder Unikonten, persönliche Microsoft-Konten, Kinderkonten und unabhängige Konten). |

## <a name="additional-resources"></a>Zusätzliche Ressourcen

Hier erhalten Sie detaillierte Informationen zu v2.0:

* [Microsoft Identity Platform (Übersicht über v2.0)](v2-overview.md)
* [Referenz zu Microsoft Identity Platform-Protokollen](active-directory-v2-protocols.md)
* [Zugriffstokenreferenz](access-tokens.md)
* [ID-Tokenreferenz](id-tokens.md)
* [Referenz zu Authentifizierungsbibliotheken](reference-v2-libraries.md)
* [Berechtigungen und Zustimmung im Microsoft Identity Platform-Endpunkt](v2-permissions-and-consent.md)
* [Microsoft Graph-API](https://developer.microsoft.com/graph)
