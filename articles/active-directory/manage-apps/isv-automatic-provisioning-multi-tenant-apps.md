---
title: Aktivieren der automatischen Benutzerbereitstellung für mehrinstanzenfähige Anwendungen in Azure Active Directory
description: Anleitung für unabhängige Softwarehersteller zum Aktivieren der automatisierten Bereitstellung
services: active-directory
documentationcenter: azure
author: BarbaraSelden
manager: CelesteDG
ms.service: active-directory
ms.subservice: app-mgmt
ms.devlang: na
ms.topic: conceptual
ms.tgt_pltfrm: na
ms.workload: identity
ms.date: 07/23/2019
ms.author: baselden
ms.reviewer: zhchia
ms.collection: active-directory
ms.openlocfilehash: 119c46ac2d1d34d86a6bfb9f75384f262f89219b
ms.sourcegitcommit: 77bfc067c8cdc856f0ee4bfde9f84437c73a6141
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 10/16/2019
ms.locfileid: "72429450"
---
# <a name="enable-automatic-user-provisioning-for-your-multi-tenant-application"></a>Aktivieren der automatischen Benutzerbereitstellung für Ihre mehrinstanzenfähige Anwendung

Die automatische Benutzerbereitstellung ist der Vorgang der Automatisierung der Erstellung, Wartung und Entfernung von Benutzeridentitäten in Zielsystemen z. B. Ihren Software-as-a-Service-Anwendungen.

## <a name="why-enable-automatic-user-provisioning"></a>Warum muss die automatische Benutzerbereitstellung aktiviert werden?

Die Benutzerbereitstellung ist erforderlich für Anwendungen, in denen vor der ersten Anmeldung eines Benutzers ein Benutzerdatensatz vorhanden sein muss. Dies hat Vorteile für Sie als Dienstanbieter und Vorteile für Ihre Kunden.

### <a name="benefits-to-you-as-the-service-provider"></a>Vorteile für Sie als Dienstanbieter

* Erhöhte Sicherheit Ihrer Anwendung durch Verwendung von Microsoft Identity Platform

* Reduzierter tatsächlicher und wahrgenommener Aufwand für Kunden bei der Implementierung Ihrer Anwendung

* Verringerte Kosten bei der Integration mit mehreren Identitätsanbietern (IdPs) für die automatische Benutzerbereitstellung durch Verwendung der SCIM-basierten Bereitstellung (System for Cross-domain Identity Management)

* Verringerte Supportkosten durch Bereitstellen umfassender Protokolle zur Unterstützung der Kunden beim Beheben von Problemen bei der Benutzerbereitstellung

* Erhöhte Transparenz Ihrer Anwendung im [Azure AD-App-Katalog](https://azuremarketplace.microsoft.com/marketplace/apps)

* Priorisierte Auflistung auf der Seite für App-Tutorials

### <a name="benefits-to-your-customers"></a>Vorteile für Ihre Kunden

* Erhöhte Sicherheit durch automatisches Entfernen des Zugriffs auf Ihre Anwendung für Benutzer, die Rollen wechseln oder die Organisation verlassen

* Vereinfachte Benutzerverwaltung der Anwendung durch Vermeiden menschlicher Fehler oder sich wiederholender Aufgaben bei der manuellen Bereitstellung

* Verringerte Kosten für das Hosten und Verwalten benutzerdefiniert entwickelter Bereitstellungslösungen

## <a name="choose-a-provisioning-method"></a>Auswählen einer Bereitstellungsmethode

Azure AD umfasst mehrere Integrationspfade zum Aktivieren der automatischen Benutzerbereitstellung für Ihre Anwendung.

* Der [Azure AD-Bereitstellungsdienst](https://docs.microsoft.com/azure/active-directory/manage-apps/user-provisioning) verwaltet das Bereitstellen und Aufheben der Bereitstellung von Benutzern aus Azure AD in Ihrer Anwendung (ausgehende Bereitstellung) und aus Ihrer Anwendung in Azure AD (eingehende Bereitstellung). Der Dienst stellt eine Verbindung mit den in der Anwendung angegebenen Endpunkten der SCIM-Benutzerverwaltungs-API (System for Cross-domain Identity Management) her.

* Wenn Sie [Microsoft Graph](https://docs.microsoft.com/graph/) verwenden, verwaltet Ihre Anwendung die eingehende und ausgehende Bereitstellung von Benutzern und Gruppen aus Azure AD in Ihrer Anwendung durch Abfragen der Microsoft Graph-API.

* Die Security Assertion Markup Language-Just-In-Time-Benutzerbereitstellung (SAML JIT) kann aktiviert werden, wenn die Anwendung SAML für den Verbund verwendet. Dabei werden die im SAML-Token gesendeten Informationen zu Ansprüchen zum Bereitstellen von Benutzern verwendet.

Informationen zu den Integrationsoptionen für die Anwendung finden Sie in der allgemeinen Vergleichstabelle und in den nachfolgenden detaillierteren Abschnitten zu den einzelnen Optionen.

| Durch die automatische Bereitstellung aktivierte oder erweiterte Funktionen| Azure AD-Bereitstellungsdienst (SCIM 2.0)| Microsoft Graph-API (OData v4.0)| SAML JIT |
|---|---|---|---|
| Benutzer- und Gruppenverwaltung in Azure AD| √| √| Nur Benutzer |
| Verwalten von Benutzern und Gruppen, die mit der lokalen Active Directory-Instanz synchronisiert werden| √*| √*| Nur Benutzer* |
| Zugriff auf andere Daten neben Benutzern und Gruppen bei der Bereitstellung, Zugriff auf O365-Daten (Teams, SharePoint, E-Mail, Kalender, Dokumente usw.)| X+| √| X |
| Erstellen, Lesen und Aktualisieren von Benutzern basierend auf Geschäftsregeln| √| √| √ |
| Löschen von Benutzern basierend auf Geschäftsregeln| √| √| X |
| Verwalten der automatischen Benutzerbereitstellung für alle Anwendungen über das Azure-Portal| √| X| √ |
| Unterstützung mehrerer Identitätsanbieter| √| X| √ |
| Unterstützung von Gastkonten (B2B)| √| √| √ |
| Unterstützung von Nicht-Unternehmenskonten (B2C)| X| √| √ |

<sup>*</sup> Azure AD Connect muss eingerichtet werden, um Benutzer aus AD mit Azure AD zu synchronisieren.  
<sup>+</sup > Die Verwendung von SCIM für die Bereitstellung hindert Sie nicht daran, Ihre Anwendung zu anderen Zwecken in Microsoft Graph zu integrieren.

## <a name="azure-ad-provisioning-service-scim"></a>Azure AD-Bereitstellungsdienst (SCIM)

Der Azure AD-Bereitstellungsdienst nutzt [SCIM](https://aka.ms/SCIMOverview) – einen Branchenstandard für die Bereitstellung, der von vielen Identitätsanbietern (Identity Providers, IdPs) und Anwendungen (beispielsweise Slack, G Suite und Dropbox) unterstützt wird. Es wird empfohlen, den Azure AD-Bereitstellungsdienst zu verwenden, wenn Sie IdPs zusätzlich zu Azure AD unterstützen möchten, da jeder SCIM-konforme IdP eine Verbindung mit dem SCIM-Endpunkt herstellen kann. Wenn Sie einen einfachen /User-Endpunkt entwickeln, können Sie die Bereitstellung aktivieren, ohne eine eigene Synchronisierungsengine verwalten zu müssen. 

Weitere Informationen zur Verwendung von SCIM im Azure AD-Bereitstellungsdienst finden Sie hier: 

* [Bereitstellen mit SCIM: erste Schritte](https://aka.ms/SCIMOverview)

* [Automatisches Bereitstellen von Benutzern und Gruppen aus Azure Active Directory für Anwendungen mit SCIM (System for Cross-domain Identity Management)](https://docs.microsoft.com/azure/active-directory/manage-apps/use-scim-to-provision-users-and-groups)

* [Verstehen der Azure AD-SCIM-Implementierung](https://docs.microsoft.com/azure/active-directory/manage-apps/use-scim-to-provision-users-and-groups)

## <a name="microsoft-graph-for-provisioning"></a>Microsoft Graph für die Bereitstellung

Bei Verwendung von Microsoft Graph für die Bereitstellung haben Sie Zugriff auf alle in Graph verfügbaren umfassenden Benutzerdaten. Neben den Details von Benutzern und Gruppen können Sie auch weitere Informationen abrufen, z. B. die Rollen, Manager und direkten Mitarbeiter von Benutzern, Geräte im Besitz von Benutzern und registrierte Geräte sowie Hunderte anderer Datenelemente, die in [Microsoft Graph](https://docs.microsoft.com/graph/api/overview?view=graph-rest-1.0) verfügbar sind. 

Mehr als 15 Millionen Organisationen und 90 % der Fortune 500-Unternehmen, die Microsoft-Clouddienste wie Office 365, Microsoft Azure, Enterprise Mobility Suite oder Microsoft 365 abonniert haben, verwenden Azure AD. Mithilfe von Microsoft Graph können Sie Ihre Anwendung in administrative Workflows integrieren, z. B. Onboarding (und Kündigung) von Mitarbeitern, Profilwartung und vieles mehr. 

Weitere Informationen zur Verwendung von Microsoft Graph für die Bereitstellung:

* [Homepage von Microsoft Graph](https://developer.microsoft.com/graph)

* [Übersicht über Microsoft Graph](https://docs.microsoft.com/graph/overview)

* [Übersicht über die Microsoft Graph-Authentifizierung](https://docs.microsoft.com/graph/auth/)

* [Erste Schritte mit Microsoft Graph](https://developer.microsoft.com/graph/get-started)

## <a name="using-saml-jit-for-provisioning"></a>Verwenden von SAML JIT für die Bereitstellung

SAML JIT kann verwendet werden, wenn Sie Benutzer nur bei der ersten Anmeldung bei Ihrer Anwendung bereitstellen möchten und die Bereitstellung von Benutzern nicht automatisch aufgehoben werden muss. Zur Verwendung von SAML JIT muss ie Anwendung SAML 2.0 als Verbundprotokoll unterstützen.

SAML JIT verwendet die Informationen zu Ansprüchen im SAML-Token, um Benutzerinformationen in der Anwendung zu erstellen und zu aktualisieren. Kunden können diese erforderlichen Ansprüche in der Azure AD-Anwendung nach Bedarf konfigurieren. Manchmal muss die JIT-Bereitstellung in der Anwendung aktiviert werden, damit Kunden diese Funktion verwenden können. SAML JIT ist für das Erstellen und Aktualisieren von Benutzern nützlich, Sie können die Benutzer in der Anwendung jedoch nicht löschen oder deaktivieren.

## <a name="next-steps"></a>Nächste Schritte

* [Aktivieren des einmaligen Anmeldens für Ihre Anwendung](https://docs.microsoft.com/azure/active-directory/manage-apps/isv-sso-content)

* [Übermitteln Sie Ihre Liste der Anwendungen](https://microsoft.sharepoint.com/teams/apponboarding/Apps/SitePages/Default.aspx), und erstellen Sie gemeinsam mit Microsoft eine Dokumentation auf der Microsoft-Website.

* [Treten Sie dem Microsoft Partner Network (kostenlos) bei, und erstellen Sie Ihren Markteinführungsplan](https://partner.microsoft.com/en-us/explore/commercial).
