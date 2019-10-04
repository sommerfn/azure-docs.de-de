---
title: Aktivieren von SSO für Ihre mehrinstanzenfähige Anwendung
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
ms.openlocfilehash: 4c89a83ade6305579e700afb86f0b9e3aca2695e
ms.sourcegitcommit: 66237bcd9b08359a6cce8d671f846b0c93ee6a82
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 07/11/2019
ms.locfileid: "67795183"
---
# <a name="enable-single-sign-on-for-your-multi-tenant-application"></a>Aktivieren des einmaligen Anmeldens für Ihre mehrinstanzenfähige Anwendung  

Wenn Sie Ihre Anwendung per Kauf oder Abonnement für andere Unternehmen zur Nutzung anbieten, machen Sie die Anwendung für Kunden in deren eigenen Azure-Mandanten verfügbar. Sie erstellen also eine mehrinstanzenfähige Anwendung. Eine Übersicht über dieses Konzept finden Sie unter [Mehrinstanzenfähige Anwendungen in Azure](https://docs.microsoft.com/azure/dotnet-develop-multitenant-applications) und [Mandanten in Azure Active Directory](../develop/single-and-multi-tenant-apps.md).

## <a name="what-is-single-sign-on"></a>Worum handelt es sich beim einmaligen Anmelden?

Mit einmaligem Anmelden (Single Sign-On, SSO) ist für Sicherheit und Komfort gesorgt, wenn sich Benutzer mit Azure Active Directory (Azure AD) und anderen Identitäten bei Anwendungen anmelden. Wenn eine Anwendung SSO-fähig ist, müssen Benutzer beim Zugriff auf die betreffende Anwendung keine gesonderten Anmeldeinformationen eingeben. Eine umfassende Erläuterung zum einmaligen Anmelden finden Sie unter [Einmaliges Anmelden bei Anwendungen in Azure Active Directory](what-is-single-sign-on.md).

## <a name="why-enable-single-sign-on-in-your-application"></a>Warum sollten Sie das einmalige Anmelden in Ihrer Anwendung aktivieren?

Das Aktivieren von SSO in Ihrer mehrinstanzenfähigen Anwendung bringt viele Vorteile mit sich. Wenn Sie SSO für Ihre Anwendung aktivieren:

* Kann Ihre Anwendung im Azure Marketplace aufgeführt werden, wo sie von Millionen von Organisationen gefunden werden kann, die Azure Active Directory verwenden.
  * Können Kunden die Anwendung schnell mit Azure AD konfigurieren.

* Kann Ihre Anwendung im Office 365-App-Katalog, im Office 365-App-Startfeld und in Microsoft Search auf Office.com aufgefunden werden.

* Kann Ihre Anwendung mit der Microsoft Graph-REST-API für eine höhere Benutzerproduktivität auf die Daten zugreifen, die über Microsoft Graph verfügbar sind.

* Mindern Sie die Supportkosten, indem Sie die Benutzerfreundlichkeit verbessern.
  * Die gemeinsam mit dem Azure AD-Team erstellte anwendungsspezifische Dokumentation fördert die Akzeptanz unter unseren gemeinsamen Kunden.
  * Die IT-Administratoren müssen sich bei Aktivierung von 1-Klick-SSO nicht in die Konfiguration Ihrer Anwendung für die Nutzung in ihrer Organisation einarbeiten.

* Bieten Sie Ihren Kunden die Möglichkeit, die Authentifizierung und Autorisierung ihrer Mitarbeiter- und Gastidentitäten umfassend zu verwalten.

  * Die gesamte Verantwortung für Kontoverwaltung und Compliance wird dem Zuständigen für die betreffenden Identitäten auf Kundenseite übertragen.

  * Sie räumen die Möglichkeit ein, SSO für bestimmte Identitätsanbieter, Gruppen oder Benutzer zu aktivieren bzw. zu deaktivieren, um deren Geschäftsanforderungen gerecht zu werden.

* Verbessern Sie die Marktgängigkeit und Akzeptanz Ihres Produkts. Viele große Organisationen fordern (oder wünschen) eine nahtlose SSO-Erfahrung ihrer Mitarbeiter für alle Anwendungen. Die einfache Gestaltung von SSO ist unerlässlich.

* Mindern Sie potenzielle Probleme auf Endbenutzerseite, wodurch Sie die Nutzung durch Endbenutzer steigern und Ihre Erträge steigern können.

## <a name="how-to-enable-single-sign-on-in-your-published-application"></a>Aktivieren von SSO in Ihrer mehrinstanzenfähigen Anwendung

1. [Wählen Sie das richtige Verbundprotokoll für Ihre mehrinstanzenfähige Anwendung aus](isv-choose-multi-tenant-federation.md).
1. Implementieren von SSO in Ihrer Anwendung
   - Weitere Informationen finden Sie im [Leitfaden zu Authentifizierungsmustern](../develop/v2-app-types.md).
   - Die Protokolle OIDC und OAuth finden Sie unter [Azure Active Directory-Codebeispiele](../develop/sample-v2-code.md).
1. [Erstellen Sie den Azure-Mandanten](isv-tenant-multi-tenant-app.md), und testen Sie Ihre Anwendung.
1. [Erstellen und veröffentlichen Sie eine SSO-Dokumentation auf Ihrer Website](isv-create-sso-documentation.md).
1. [Übermitteln Sie Ihre Liste der Anwendungen](https://microsoft.sharepoint.com/teams/apponboarding/Apps/SitePages/Default.aspx), und erstellen Sie gemeinsam mit Microsoft eine Dokumentation auf der Microsoft-Website.
1. [Treten Sie dem Microsoft Partner Network (kostenlos) bei, und erstellen Sie Ihren Markteinführungsplan](https://partner.microsoft.com/en-us/explore/commercial#gtm).
