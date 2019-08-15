---
title: 'Übersicht über Microsoft Identity Platform (v2.0): Azure'
description: 'Informationen zu Microsoft Identity Platform (v2.0): Endpunkt und Plattform'
services: active-directory
documentationcenter: dev-center-name
author: rwike77
manager: CelesteDG
editor: ''
ms.service: active-directory
ms.subservice: develop
ms.devlang: na
ms.topic: overview
ms.tgt_pltfrm: na
ms.workload: identity
ms.date: 05/08/2019
ms.author: ryanwi
ms.reviewer: agirling, saeeda, benv
ms.custom: aaddev, identityplatformtop40
ms.collection: M365-identity-device-management
ms.openlocfilehash: fbbcd854434bcc085f1b63fb864755dd0e928fc9
ms.sourcegitcommit: 670c38d85ef97bf236b45850fd4750e3b98c8899
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 08/08/2019
ms.locfileid: "68852161"
---
# <a name="microsoft-identity-platform-v20-overview"></a>Übersicht über Microsoft Identity Platform (v2.0)

Microsoft Identity Platform ist eine Weiterentwicklung der Azure AD-Entwicklerplattform (Azure Active Directory). Sie ermöglicht Entwicklern das Erstellen von Anwendungen, mit denen alle Microsoft-Identitäten angemeldet werden, sowie das Abrufen von Token zum Aufrufen von Microsoft-APIs, etwa Microsoft Graph oder von Entwicklern erstellte APIs. Microsoft Identity Platform umfasst folgende Komponenten:

- **Standardkonformer OAuth 2.0- und OpenID Connect-Authentifizierungsdienst**, der Entwicklern die Authentifizierung einer beliebigen Microsoft-Identität ermöglicht, einschließlich:
  - Geschäfts-, Schul- oder Unikonten (über Azure AD bereitgestellt)
  - Persönliche Microsoft-Konten (z. B. Skype, Xbox und Outlook.com)
  - Social Media-Konten oder lokale Konten (über Azure AD B2C)
- **Open-Source-Bibliotheken**: Microsoft Authentication Libraries (MSAL) und Unterstützung für andere standardkonforme Bibliotheken
- **Verwaltungsportal für Anwendungen**: Eine Registrierungs- und Konfigurationsumgebung, die zusammen mit allen Ihren anderen Azure-Verwaltungsfunktionen in das Azure-Portal integriert ist
- **Anwendungskonfigurations-API und PowerShell**: ermöglicht die programmgesteuerte Konfiguration Ihrer Anwendungen über die REST-API (Microsoft Graph und Azure Active Directory Graph 1.6) und PowerShell, damit Sie Ihre DevOps-Aufgaben automatisieren können.
- **Entwicklerinhalte**: Konzept- und Referenzdokumentation, Schnellstartbeispiele, Codebeispiele, Tutorials und Schrittanleitungen

Für Entwickler bietet Microsoft Identity Platform eine nahtlose Integration in Innovationen im Identitäts- und Sicherheitsbereich wie kennwortlose Authentifizierung, Step-up-Authentifizierung und bedingten Zugriff.  Sie müssen diese Funktionalität nicht selbst implementieren: In Microsoft Identity Platform integrierte Anwendungen nutzen solche Innovationen nativ.

Mit Microsoft Identity Platform müssen Sie Code nur einmal schreiben, um alle Benutzer zu erreichen. Sie können eine App einmalig erstellen und sie plattformübergreifend nutzen lassen, oder eine App erstellen, die sowohl als Client als auch als Ressourcenanwendung (API) fungiert.

## <a name="getting-started"></a>Erste Schritte

Die Verwendung von Identitäten muss nicht kompliziert sein. Wählen Sie ein auf Sie zutreffendes [Szenario](authentication-flows-app-scenarios.md) aus. In jedem Szenariopfad stehen ein Schnellstart sowie eine Übersichtsseite zur Verfügung, sodass Sie in wenigen Minuten einsatzbereit sind:

- [Erstellen einer Einzelseiten-App](scenario-spa-overview.md)
- [Erstellen einer Web-App, die Benutzer anmeldet](scenario-web-app-sign-user-overview.md)
- [Erstellen einer Web-App, die Web-APIs aufruft](scenario-web-app-call-api-overview.md)
- [Erstellen einer geschützten Web-API](scenario-protected-web-api-overview.md)
- [Erstellen einer Web-API, die Web-APIs aufruft](scenario-web-api-call-api-overview.md)
- [Erstellen einer Desktop-App](scenario-desktop-overview.md)
- [Erstellen einer Daemon-App](scenario-daemon-overview.md)
- [Erstellen einer mobilen App](scenario-mobile-overview.md)

Das folgende Diagramm zeigt gängige Authentifizierungs-App-Szenarien. Verwenden Sie es bei der Integration von Microsoft Identity Platform in Ihre App als Referenz.

[![Anwendungsszenarien in Microsoft Identity Platform](./media/v2-overview/application-scenarios-identity-platform.png)](./media/v2-overview/application-scenarios-identity-platform.svg#lightbox)

## <a name="next-steps"></a>Nächste Schritte

Wenn Sie mehr über die wichtigsten Authentifizierungskonzepte erfahren möchten, empfehlen wir Ihnen, mit diesen Themen zu beginnen:

- [Authentifizierungsflows und Anwendungsszenarien](authentication-flows-app-scenarios.md)
- [Authentifizierungsszenarien für Azure AD](authentication-scenarios.md)
- [Anwendungs- und Dienstprinzipale](app-objects-and-service-principals.md)
- [Zielgruppen](v2-supported-account-types.md)
- [Berechtigungen und Einwilligung](v2-permissions-and-consent.md)
- [ID-Token](id-tokens.md) und [Zugriffstoken](access-tokens.md)

Erstellen Sie eine Anwendung mit umfassenden Daten, die [Microsoft Graph](https://docs.microsoft.com/graph/overview) aufruft.

Wenn Sie zum Starten Ihrer App in einer **Produktionsumgebung** bereit sind, beachten Sie diese bewährten Methoden:

- [Aktivieren Sie die Protokollierung](msal-logging.md) in Ihrer Anwendung.
- Aktivieren Sie die Telemetrie in Ihrer Anwendung.
- Aktivieren Sie [Proxys und das Anpassen von HTTP-Clients](msal-net-provide-httpclient.md).
- Testen Sie Ihre Integration anhand der [Checkliste für die Integration von Microsoft Identity Platform](identity-platform-integration-checklist.md).

## <a name="learn-more"></a>Weitere Informationen

Wenn Sie die Erstellung einer kundenorientierten Anwendung planen, die Social Media-Identitäten und lokale Identitäten anmeldet, lesen Sie die [Azure AD B2C-Übersicht](https://docs.microsoft.com/azure/active-directory-b2c/tutorial-add-identity-providers).
