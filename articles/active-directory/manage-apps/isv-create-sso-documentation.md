---
title: Erstellen und Veröffentlichen einer Dokumentation zu einmaligem Anmelden (Single Sign-On, SSO) für Ihre Anwendung
description: Leitfaden für unabhängige Softwarehersteller zur Integration in Azure Active Directory
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
ms.openlocfilehash: 53221343ac606b6076cc9cc3cff6e0f96c1a3ac3
ms.sourcegitcommit: cf438e4b4e351b64fd0320bf17cc02489e61406a
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 07/08/2019
ms.locfileid: "67659452"
---
# <a name="create-and-publish-single-sign-on-documentation-for-your-application"></a>Erstellen und Veröffentlichen einer Dokumentation zu einmaligem Anmelden (Single Sign-On, SSO) für Ihre Anwendung   

## <a name="documentation-on-your-site"></a>Dokumentation auf Ihrer Website

Die einfache Einführung ist ein bedeutender Faktor bei Entscheidungen über Unternehmenssoftware. Eine übersichtliche und leicht verständliche Dokumentation unterstützt Ihre Kunden bei der Einführung und reduziert die Supportkosten. In Zusammenarbeit mit Tausenden von Softwareanbietern hat Microsoft erfahren, was funktioniert.

Die Dokumentation auf Ihrer Website sollte mindestens die folgenden Punkte enthalten.

* Einführung in die Funktionalität des einmaligen Anmeldens

  * Unterstützte Protokolle

  * Version und SKU

  * Liste der unterstützten Identitätsanbieter mit Dokumentationslinks

* Lizenzierungsinformationen zu Ihrer Anwendung

* Rollenbasierte Zugriffssteuerung zum Konfiguration von einmaligem Anmelden

* Schritte der SSO-Konfiguration

  * Konfigurationselemente der Benutzeroberfläche für SAML mit vom Anbieter erwarteten Werten

  * Informationen des Dienstanbieters, die an Identitätsanbieter weitergegeben werden sollen

* Bei OIDC/OAuth

  * Liste der Berechtigungen, die für die Übereinstimmung mit geschäftlichen Begründungen erforderlich sind

* Testschritte für Pilotbenutzer

* Problembehandlungsinformationen inklusive Fehlercodes und Meldungen

* Supportmechanismen für Kunden

## <a name="documentation-on-the-microsoft-site"></a>Dokumentation auf der Microsoft-Website

Wenn Sie Ihre Anwendung im Azure Active Directory-Anwendungskatalog auflisten, der Ihre Anwendung auch im Azure Marketplace veröffentlicht, generiert Microsoft eine Dokumentation für unsere gemeinsamen Kunden, die den schrittweisen Prozess erklärt. [Hier](https://aka.ms/appstutorial) sehen Sie ein Beispiel. Diese Dokumentation wird auf der Grundlage Ihrer Eingliederung in den Katalog erstellt und kann leicht aktualisiert werden, wenn Sie mit Ihrem GitHub-Konto Änderungen an Ihrer Anwendung vornehmen.

## <a name="next-steps"></a>Nächste Schritte

[Auflisten Ihrer Anwendung im Azure AD-Anwendungskatalog](https://microsoft.sharepoint.com/teams/apponboarding/Apps/SitePages/Default.aspx)