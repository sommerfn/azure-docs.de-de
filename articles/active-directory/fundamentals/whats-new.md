---
title: Neuerungen Versionshinweise – Azure Active Directory | Microsoft-Dokumentation
description: Hier finden Sie Informationen zu den Neuerungen in Azure Active Directory, z. B. aktuelle Versionshinweise, bekannte Probleme, Fehlerbehebungen, veraltete Funktionen und anstehende Änderungen.
services: active-directory
author: eross-msft
manager: daveba
featureFlags:
- clicktale
ms.assetid: 06a149f7-4aa1-4fb9-a8ec-ac2633b031fb
ms.service: active-directory
ms.subservice: fundamentals
ms.workload: identity
ms.topic: conceptual
ms.date: 05/23/2019
ms.author: lizross
ms.reviewer: dhanyahk
ms.custom: it-pro
ms.collection: M365-identity-device-management
ms.openlocfilehash: af215c80148010d526c951e7a5128d6e4622b1cd
ms.sourcegitcommit: c0419208061b2b5579f6e16f78d9d45513bb7bbc
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 07/08/2019
ms.locfileid: "67625567"
---
# <a name="whats-new-in-azure-active-directory"></a>Neuerungen in Azure Active Directory

>Lassen Sie sich über Updates auf dieser Seite benachrichtigen, indem Sie die URL `https://docs.microsoft.com/api/search/rss?search=%22release+notes+for+azure+AD%22&locale=en-us` kopieren und in Ihren ![RSS-Symbol](./media/whats-new/feed-icon-16x16.png)-Feedreader einfügen.

Es werden fortlaufend Verbesserungen an Azure AD vorgenommen. Damit Sie bezüglich der aktuellen Entwicklungen auf dem neuesten Stand bleiben, bietet dieser Artikel Informationen zu Folgendem:

- Neueste Versionen
- Bekannte Probleme
- Fehlerbehebungen
- Veraltete Funktionen
- Pläne für Änderungen

Besuchen Sie regelmäßig diese Seite. Diese wird monatlich aktualisiert. Wenn Sie nach Elementen suchen, die älter als sechs Monate sind, können Sie sie im [Archiv für Neuheiten in Azure Active Directory](whats-new-archive.md) finden.

---

## <a name="june-2019"></a>Juni 2019

### <a name="new-riskdetections-api-for-microsoft-graph-public-preview"></a>Neue riskDetection-API für Microsoft Graph (öffentliche Vorschau)

**Typ:** Neues Feature  
**Dienstkategorie:** Schutz der Identität (Identity Protection)  
**Produktfunktion:** Identitätssicherheit und -schutz

Wir freuen uns, Ihnen mitteilen zu können, dass die neue riskDetection-API für Microsoft Graph ab sofort in der öffentlichen Vorschau verfügbar ist. Sie können diese neue API verwenden, um eine Liste der Erkennungen von identitätsschutzbezogenen Benutzern und Anmelderisiken in Ihrem Unternehmen anzuzeigen. Sie können diese API auch verwenden, um Ihre Risikoerkennung effizienter abzufragen, einschließlich Details über die Erkennungstyp, Status, Level und vieles mehr.

Weitere Informationen finden Sie in der [Referenzdokumentation zur riskDetection-API](https://docs.microsoft.com/graph/api/resources/riskdetection?view=graph-rest-beta).

---

### <a name="new-federated-apps-available-in-azure-ad-app-gallery---june-2019"></a>Neue Verbund-Apps im Azure AD-App-Katalog verfügbar – Juni 2019

**Typ:** Neues Feature  
**Dienstkategorie:** Unternehmens-Apps  
**Produktfunktion:** Integration von Drittanbieterprodukten

Im Juni 2019 haben wir diese 22 neuen Apps mit Verbundunterstützung in unseren App-Katalog aufgenommen:

[Azure AD SAML Toolkit](https://docs.microsoft.com/azure/active-directory/saas-apps/saml-toolkit-tutorial), [Otsuka Shokai (大塚商会)](https://docs.microsoft.com/azure/active-directory/saas-apps/otsuka-shokai-tutorial), [ANAQUA](https://docs.microsoft.com/azure/active-directory/saas-apps/anaqua-tutorial), [Azure VPN Client](https://portal.azure.com/), [ExpenseIn](https://docs.microsoft.com/azure/active-directory/saas-apps/expensein-tutorial), [Helper Helper](https://docs.microsoft.com/azure/active-directory/saas-apps/helper-helper-tutorial), [Costpoint](https://docs.microsoft.com/azure/active-directory/saas-apps/costpoint-tutorial), [GlobalOne](https://docs.microsoft.com/azure/active-directory/saas-apps/globalone-tutorial), [Mercedes-Benz In-Car Office](https://me.secure.mercedes-benz.com/), [Skore](https://app.justskore.it/), [Oracle Cloud Infrastructure Console](https://docs.microsoft.com/azure/active-directory/saas-apps/oracle-cloud-tutorial), [CyberArk SAML Authentication](https://docs.microsoft.com/azure/active-directory/saas-apps/cyberark-saml-authentication-tutorial), [Scrible Edu](https://www.scrible.com/sign-in/#/create-account), [PandaDoc](https://docs.microsoft.com/azure/active-directory/saas-apps/pandadoc-tutorial), [Perceptyx](https://apexdata.azurewebsites.net/docs.microsoft.com/azure/active-directory/saas-apps/perceptyx-tutorial), [Proptimise OS](https://proptimise.co.uk/software/), [Vtiger CRM (SAML)](https://docs.microsoft.com/azure/active-directory/saas-apps/vtiger-crm-saml-tutorial), Oracle Access Manager for Oracle Retail Merchandising, Oracle Access Manager for Oracle E-Business Suite, Oracle IDCS for E-Business Suite, Oracle IDCS for PeopleSoft, Oracle IDCS for JD Edwards

Weitere Informationen zu den Apps finden Sie unter [SaaS-Anwendungsintegration mit Azure Active Directory](https://aka.ms/appstutorial). Weitere Informationen zum Listen Ihrer Anwendung im Azure AD-App-Katalog finden Sie unter [Listen Ihrer Anwendung im Azure Active Directory-Anwendungskatalog](https://aka.ms/azureadapprequest).

---

### <a name="automate-user-account-provisioning-for-these-newly-supported-saas-apps"></a>Automatisieren der Bereitstellung von Benutzerkonten für diese neu unterstützten SaaS-Apps

**Typ:** Neues Feature  
**Dienstkategorie:** Unternehmens-Apps  
**Produktfunktion:** Überwachung und Berichterstellung

Sie können jetzt das Erstellen, Aktualisieren und Löschen von Benutzerkonten für diese neu integrierten Apps automatisieren:

- [Zoom](https://docs.microsoft.com/azure/active-directory/saas-apps/zoom-provisioning-tutorial)

- [Envoy](https://docs.microsoft.com/azure/active-directory/saas-apps/envoy-provisioning-tutorial)

- [Proxyclick](https://docs.microsoft.com/azure/active-directory/saas-apps/proxyclick-provisioning-tutorial)

- [4me](https://docs.microsoft.com/azure/active-directory/saas-apps/4me-provisioning-tutorial)

Weitere Informationen dazu, wie Sie Ihre Organisation durch die automatisierte Bereitstellung von Benutzerkonten besser sichern können, finden Sie unter [Automatisieren der Bereitstellung und Bereitstellungsaufhebung von Benutzern für SaaS-Anwendungen mit Azure Active Directory](https://docs.microsoft.com/azure/active-directory/manage-apps/user-provisioning).

---

### <a name="view-the-real-time-progress-of-the-azure-ad-provisioning-service"></a>Anzeigen des Fortschritts des Azure AD-Bereitstellungsdiensts in Echtzeit

**Typ:** Geändertes Feature  
**Dienstkategorie:** App-Bereitstellung  
**Produktfunktion:** Identity Lifecycle Management

Wir haben die Azure AD-Bereitstellungsbenutzeroberfläche mit einer neuen Statusanzeige aktualisiert, die Ihnen anzeigt, wie weit Sie im Prozess der Benutzerbereitstellung fortgeschritten sind. Diese aktualisierte Benutzeroberfläche liefert auch Informationen über die Anzahl der Benutzer, die während des aktuellen Zyklus bereitgestellt wurden, sowie darüber, wie viele Benutzer bisher bereitgestellt wurden.

Weitere Informationen finden Sie unter [Ermitteln, wann ein bestimmter Benutzer auf eine Anwendung zugreifen kann](https://docs.microsoft.com/azure/active-directory/manage-apps/application-provisioning-when-will-provisioning-finish-specific-user).

---

### <a name="company-branding-now-appears-on-sign-out-and-error-screens"></a>Unternehmensbranding wird jetzt auf Abmelde- und Fehlerbildschirmen angezeigt.

**Typ:** Geändertes Feature  
**Dienstkategorie:** Authentifizierungen (Anmeldungen)  
**Produktfunktion:** Benutzerauthentifizierung

Wir haben Azure AD so aktualisiert, dass Ihr Unternehmensbranding nun auf den Abmelde- und Fehlerbildschirmen sowie auf der Anmeldeseite angezeigt wird. Sie müssen nichts tun, um diese Funktion zu aktivieren, Azure AD verwendet einfach die Ressourcen, die Sie bereits im Bereich **Unternehmensbranding** des Azure-Portals eingerichtet haben.

Weitere Informationen zur Einrichtung Ihres Unternehmensbrandings finden Sie unter [Hinzufügen von Branding zur Azure Active Directory-Anmeldeseite Ihrer Organisation](https://docs.microsoft.com/azure/active-directory/fundamentals/customize-branding).

---

### <a name="azure-multi-factor-authentication-mfa-server-is-no-longer-available-for-new-deployments"></a>Azure Multi-Factor Authentication-Server (MFA) ist nicht mehr für neue Bereitstellungen verfügbar

**Typ:** Veraltet  
**Dienstkategorie:** MFA  
**Produktfunktion:** Identitätssicherheit und -schutz

Ab dem 1. Juli 2019 bietet Microsoft keine MFA-Server mehr für neue Bereitstellungen an. Neue Kunden, die eine Multi-Factor Authentication in ihrer Organisation einrichten möchten, müssen jetzt die cloudbasierte Multi-Factor Authentication von Azure verwenden. Kunden, die MFA-Server vor dem 1. Juli aktiviert haben, werden keine Änderung feststellen. Sie können weiterhin die neueste Version herunterladen, zukünftige Updates erhalten und Aktivierungsdaten generieren.

Weitere Informationen finden Sie unter [Erste Schritte mit Azure Multi-Factor Authentication-Server](https://docs.microsoft.com/azure/active-directory/authentication/howto-mfaserver-deploy). Weitere Informationen zur cloudbasierten Azure Multi-Factor Authentication finden Sie unter [Planen einer cloudbasierten Azure Multi-Factor Authentication-Bereitstellung](https://docs.microsoft.com/azure/active-directory/authentication/howto-mfa-getstarted).

---

## <a name="may-2019"></a>Mai 2019

### <a name="service-change-future-support-for-only-tls-12-protocols-on-the-application-proxy-service"></a>Änderung des Diensts: Zukünftige Unterstützung nur für TLS 1.2-Protokolle auf dem Anwendungsproxy-Dienst

**Typ:** Plan für Änderung  
**Dienstkategorie:** Anwendungsproxy  
**Produktfunktion:** Zugriffssteuerung

Zur Bereitstellung erstklassiger Verschlüsselung für unsere Kunden beschränken wir den Zugriff ausschließlich auf TLS 1.2-Protokolle im Anwendungsproxydienst. Diese Änderung wird schrittweise für Kunden eingeführt, die bereits ausschließlich TLS 1.2-Protokolle verwenden, sodass Sie keine Änderungen feststellen sollten.

TLS 1.0 und TLS 1.1 sind am 31. August 2019 veraltet, aber wir werden noch zusätzliche erweiterte Benachrichtigungen senden, sodass Sie Zeit haben, sich auf die Änderung vorzubereiten. Stellen Sie zur Vorbereitung auf diese Änderung sicher, dass Ihre Client-Server- und Browser-Server-Kombinationen einschließlich aller Clients, mit denen Ihre Benutzer auf Apps zugreifen, die durch den Anwendungsproxy veröffentlicht werden, zur Verwendung des TLS 1.2-Protokolls aktualisiert werden, um die Verbindung mit dem Anwendungsproxy-Dienst zu verwalten. Weitere Informationen finden Sie unter [Hinzufügen einer lokalen Anwendung für den Remotezugriff über den Anwendungsproxy in Azure Active Directory](https://docs.microsoft.com/azure/active-directory/manage-apps/application-proxy-add-on-premises-application#before-you-begin).

---

### <a name="use-the-usage-and-insights-report-to-view-your-app-related-sign-in-data"></a>Verwenden des Nutzungs- und Erkenntnisberichts zum Anzeigen Ihrer app-bezogenen Anmeldungsdaten

**Typ:** Neues Feature  
**Dienstkategorie:** Unternehmens-Apps  
**Produktfunktion:** Überwachung und Berichterstellung

Nun können Sie mit dem Nutzungs- und Erkenntnisbericht im Bereich **Unternehmensanwendungen** des Azure-Portals eine anwendungsorientierte Ansicht Ihrer Anmeldungsdaten abrufen, einschließlich Informationen über:

- Am meisten verwendete Apps in Ihrer Organisation

- Apps mit den meisten fehlerhaften Anmeldungen

- Häufigste Anmeldefehler bei den einzelnen Apps

Weitere Informationen zu diesem Feature finden Sie unter [Bericht „Nutzung & Erkenntnisse“ im Azure Active Directory-Portal](https://docs.microsoft.com/azure/active-directory/reports-monitoring/concept-usage-insights-report).

---

### <a name="automate-your-user-provisioning-to-cloud-apps-using-azure-ad"></a>Automatisieren Ihrer Benutzerbereitstellung für Cloud-Apps mit Azure AD

**Typ:** Neues Feature  
**Dienstkategorie:** Unternehmens-Apps  
**Produktfunktion:** Überwachung und Berichterstellung

Befolgen Sie diese neuen Tutorials, um den Azure AD-Bereitstellungsdienst zu verwenden, um das Erstellen, Löschen und Aktualisieren von Benutzerkonten für die folgenden, cloudbasierten Apps zu automatisieren:

- [Comeet](https://docs.microsoft.com/azure/active-directory/saas-apps/comeet-recruiting-software-provisioning-tutorial)

- [DynamicSignal](https://docs.microsoft.com/azure/active-directory/saas-apps/dynamic-signal-provisioning-tutorial)

- [KeeperSecurity](https://docs.microsoft.com/azure/active-directory/saas-apps/keeper-password-manager-digitalvault-provisioning-tutorial)

Sie können auch dieses neue [Dropbox-Tutorial](https://docs.microsoft.com/azure/active-directory/saas-apps/dropboxforbusiness-provisioning-tutorial) befolgen, das Informationen zur Bereitstellung von Gruppenobjekten enthält.

Weitere Informationen dazu, wie Sie Ihre Organisation durch die automatisierte Bereitstellung von Benutzerkonten besser sichern können, finden Sie unter [Automatisieren der Bereitstellung und Bereitstellungsaufhebung von Benutzern für SaaS-Anwendungen mit Azure Active Directory](https://docs.microsoft.com/azure/active-directory/manage-apps/user-provisioning).

---

### <a name="identity-secure-score-is-now-available-in-azure-ad-general-availability"></a>Identity Secure Score ist jetzt in Azure AD verfügbar (allgemeine Verfügbarkeit)

**Typ:** Neues Feature  
**Dienstkategorie:** –  
**Produktfunktion:** Identitätssicherheit und -schutz

Sie können jetzt Ihren Identitätssicherheitsstatus mit dem Feature Identity Secure Score in Azure AD überwachen und verbessern. Das Feature Identity Secure Score nutzt ein einzelnes Dashboard, um Sie bei Folgendem zu unterstützen:

- Objektives Messen Ihres Identitätssicherheitsstatus mit einer Bewertung von 1 bis 223

- Planen von Verbesserungen Ihrer Identitätssicherheit

- Überprüfen des Erfolgs Ihrer Sicherheitsverbesserungen

Weitere Informationen zum Feature Identity Secure Score finden Sie unter [Was ist der Identity Secure Score in Azure Active Directory?](https://docs.microsoft.com/azure/active-directory/fundamentals/identity-secure-score).

---

### <a name="new-app-registrations-experience-is-now-available-general-availability"></a>Neue Benutzeroberfläche für App-Registrierungen ist jetzt verfügbar (allgemeine Verfügbarkeit)

**Typ:** Neues Feature  
**Dienstkategorie:** Authentifizierungen (Anmeldungen)  
**Produktfunktion:** Entwickleroberfläche

Die Benutzeroberfläche [App-Registrierungen](https://aka.ms/appregistrations) ist jetzt allgemein verfügbar. Diese neue Oberfläche umfasst alle wesentlichen Funktionen, die Ihnen aus dem Azure-Portal und dem Anwendungsregistrierungsportal vertraut sind, und verbessert sie folgendermaßen:

- **Bessere App-Verwaltung.** Anstatt Ihre Apps in verschiedenen Portalen anzuzeigen, können Sie jetzt alle Apps an einem Ort sehen.

- **Vereinfachte App-Registrierung.** Von der verbesserten Navigationserfahrung bis zur überarbeiteten Berechtigungsauswahlerfahrung können Sie Ihre Apps jetzt einfacher registrieren und verwalten.

- **Ausführlichere Informationen.** Sie finden weitere Informationen zu Ihrer App einschließlich Schnellstarthandbüchern und vielem mehr.

Weitere Informationen finden Sie unter [Microsoft Identity Platform (vormals Azure Active Directory für Entwickler)](https://docs.microsoft.com/azure/active-directory/develop/) und [App registrations experience is now generally available! (Benutzerfreundliche App-Registrierung ist jetzt allgemein verfügbar!)](https://developer.microsoft.com/identity/blogs/new-app-registrations-experience-is-now-generally-available/). Blogankündigung.

---

### <a name="new-capabilities-available-in-the-risky-users-api-for-identity-protection"></a>Neue Funktionen verfügbar in der riskyUser-API für Schutz der Identität (Identity Protection)

**Typ:** Neues Feature  
**Dienstkategorie:** Schutz der Identität (Identity Protection)  
**Produktfunktion:** Identitätssicherheit und -schutz

Wir freuen uns, ankündigen zu können, dass Sie jetzt die riskyUser-API verwenden können, um den Risikoverlauf von Benutzern abzurufen, riskante Benutzer zu schließen und Benutzer als gefährdet zu bestätigen. Mit dieser Änderung können Sie effizienter den Risikostatus Ihrer Benutzer aktualisieren und ihren Risikoverlauf verstehen.

Weitere Informationen finden Sie in der [Referenzdokumentation zur riskyUser-API](https://docs.microsoft.com/graph/api/resources/riskyuser?view=graph-rest-beta).

---

### <a name="new-federated-apps-available-in-azure-ad-app-gallery---may-2019"></a>Neue Verbund-Apps im Azure AD-App-Katalog verfügbar – Mai 2019

**Typ:** Neues Feature  
**Dienstkategorie:** Unternehmens-Apps  
**Produktfunktion:** Integration von Drittanbieterprodukten

Im Mai 2019 haben wir diese 21 neuen Apps mit Verbundunterstützung in unseren App-Katalog aufgenommen:

[Freedcamp](https://docs.microsoft.com/azure/active-directory/saas-apps/freedcamp-tutorial), [Real Links](https://docs.microsoft.com/azure/active-directory/saas-apps/real-links-tutorial), [Kianda](https://app.kianda.com/sso/OpenID/AzureAD/), [Simple Sign](https://docs.microsoft.com/azure/active-directory/saas-apps/simple-sign-tutorial), [Braze](https://docs.microsoft.com/azure/active-directory/saas-apps/braze-tutorial), [Displayr](https://docs.microsoft.com/azure/active-directory/saas-apps/displayr-tutorial), [Templafy](https://docs.microsoft.com/azure/active-directory/saas-apps/templafy-tutorial), [Marketo Sales Engage](https://toutapp.com/login), [ACLP](https://docs.microsoft.com/azure/active-directory/saas-apps/aclp-tutorial), [OutSystems](https://docs.microsoft.com/azure/active-directory/saas-apps/outsystems-tutorial), [Meta4 Global HR](https://docs.microsoft.com/azure/active-directory/saas-apps/meta4-global-hr-tutorial), [Quantum Workplace](https://docs.microsoft.com/azure/active-directory/saas-apps/quantum-workplace-tutorial), [Cobalt](https://docs.microsoft.com/azure/active-directory/saas-apps/cobalt-tutorial), [webMethods API Cloud](https://docs.microsoft.com/azure/active-directory/saas-apps/webmethods-integration-cloud-tutorial), [RedFlag](https://pocketstop.com/redflag/), [Whatfix](https://docs.microsoft.com/azure/active-directory/saas-apps/whatfix-tutorial), [Control](https://docs.microsoft.com/azure/active-directory/saas-apps/control-tutorial), [JOBHUB](https://docs.microsoft.com/azure/active-directory/saas-apps/jobhub-tutorial), [NEOGOV](https://docs.microsoft.com/azure/active-directory/saas-apps/neogov-tutorial), [Foodee](https://docs.microsoft.com/azure/active-directory/saas-apps/foodee-tutorial), [MyVR](https://docs.microsoft.com/azure/active-directory/saas-apps/myvr-tutorial)

Weitere Informationen zu den Apps finden Sie unter [SaaS-Anwendungsintegration mit Azure Active Directory](https://aka.ms/appstutorial). Weitere Informationen zum Listen Ihrer Anwendung im Azure AD-App-Katalog finden Sie unter [Listen Ihrer Anwendung im Azure Active Directory-Anwendungskatalog](https://aka.ms/azureadapprequest).

---

### <a name="improved-groups-creation-and-management-experiences-in-the-azure-ad-portal"></a>Verbesserte Gruppenerstellung und Verwaltungserfahrungen im Azure AD-Portal

**Typ:** Neues Feature  
**Dienstkategorie:** Gruppenverwaltung  
**Produktfunktion:** Zusammenarbeit

Wir haben die gruppenbezogenen Erfahrungen im Azure AD-Portal verbessert. Diese Verbesserungen ermöglichen Administratoren, Gruppenlisten und Mitgliederlisten besser zu verwalten und weitere Erstellungsoptionen bereitzustellen.

Die Verbesserungen umfassen:

- Einfaches Filtern von Mitgliedschaftstyp und Gruppentyp.

- Hinzufügen neuer Spalten, z.B. Quelle und E-Mail-Adresse.

- Möglichkeit zum Auswählen mehrerer Gruppen-, Mitglieder. und Besitzerlisten zum mühelosen Löschen.

- Möglichkeit zum Auswählen einer E-Mail-Adresse und Hinzufügen von Besitzern während der Gruppenerstellung.

Weitere Informationen dazu finden Sie in [Erstellen einer Basisgruppe und Hinzufügen von Mitgliedern mit Azure Active Directory](https://docs.microsoft.com/azure/active-directory/fundamentals/active-directory-groups-create-azure-portal).

---

### <a name="configure-a-naming-policy-for-office-365-groups-in-azure-ad-portal-general-availability"></a>Konfigurieren einer Benennungsrichtlinie für Office 365-Gruppen im Azure AD-Portal (allgemeine Verfügbarkeit)

**Typ:** Geändertes Feature  
**Dienstkategorie:** Gruppenverwaltung  
**Produktfunktion:** Zusammenarbeit

Administratoren können jetzt im Azure AD-Portal eine Benennungsrichtlinie für Office 365-Gruppen konfigurieren. Diese Änderung hilft, einheitliche Namenskonventionen für Office 365-Gruppen durchzusetzen, die von Benutzern in Ihrer Organisation erstellt oder bearbeitet werden.

Sie haben zwei Möglichkeiten, Benennungsrichtlinien für Office 365-Gruppen zu konfigurieren:

- Definieren Sie Präfixe oder Suffixe, die einem Gruppennamen automatisch hinzugefügt werden.

- Laden Sie eine benutzerdefinierte Liste blockierter Wörter hoch, die in Gruppennamen Ihrer Organisation nicht zulässig sind (z. B. „CEO, Gehalt, Personalabteilung“).

Weitere Informationen finden Sie unter [Erzwingen einer Benennungsrichtlinie für Office 365-Gruppen](https://docs.microsoft.com/azure/active-directory/users-groups-roles/groups-naming-policy).

---

### <a name="microsoft-graph-api-endpoints-are-now-available-for-azure-ad-activity-logs-general-availability"></a>Microsoft Graph-API-Endpunkte sind jetzt für Azure AD-Aktivitätsprotokolle verfügbar (allgemeine Verfügbarkeit)

**Typ:** Geändertes Feature  
**Dienstkategorie:** Berichterstellung  
**Produktfunktion:** Überwachung und Berichterstellung

Wir freuen uns, Ihnen die allgemeine Verfügbarkeit der Unterstützung von Microsoft Graph-API-Endpunkten für Azure AD-Aktivitätsprotokolle ankündigen zu können. Mit diesem Release können Sie nun Version 1.0 von Azure AD-Überwachungsprotokollen als auch die Anmeldeprotokolle-APIs verwenden.

Weitere Informationen hierzu finden Sie unter [Übersicht über die Überwachungsprotokoll-API in Azure AD](https://docs.microsoft.com/graph/api/resources/azure-ad-auditlog-overview?view=graph-rest-1.0).

---

### <a name="administrators-can-now-use-conditional-access-for-the-combined-registration-process-public-preview"></a>Administratoren können jetzt den bedingten Zugriff für den kombinierten Registrierungsprozess (öffentliche Vorschau) verwenden.

**Typ:** Neues Feature  
**Dienstkategorie:** Bedingter Zugriff  
**Produktfunktion:** Identitätssicherheit und -schutz  

Administratoren können jetzt Richtlinien für bedingten Zugriff für die Verwendung durch die kombinierte Registrierungsseite erstellen. Dies umfasst die Anwendung von Richtlinien, um die Registrierung zuzulassen, wenn:

- Benutzer sich in einem vertrauenswürdigen Netzwerk befinden.

- Benutzer ein geringes Anmelderisiko darstellen.

- Benutzer sich auf einem verwalteten Gerät befinden.

- Benutzer den Nutzungsbedingungen (Terms Of Use, TOU) der Organisation zustimmen.

Weitere Informationen zu bedingtem Zugriff und Zurücksetzen von Kennwörtern finden Sie in dem [Blogbeitrag über bedingten Zugriff für die kombinierte MFA- und Kennwortzurücksetzungsregistrierung in Azure AD](https://techcommunity.microsoft.com/t5/Azure-Active-Directory-Identity/Conditional-access-for-the-Azure-AD-combined-MFA-and-password/ba-p/566348). Weitere Informationen zu Richtlinien für bedingten Zugriff für den kombinierten Registrierungsprozess finden Sie unter [Richtlinien für bedingten Zugriff für die kombinierte Registrierung](https://docs.microsoft.com/azure/active-directory/authentication/howto-registration-mfa-sspr-combined#conditional-access-policies-for-combined-registration). Weitere Informationen zu den Azure AD-Nutzungsbedingungen finden Sie unter [Anzeige von Nutzungsbedingungen für Benutzer](https://docs.microsoft.com/azure/active-directory/conditional-access/terms-of-use).

---

## <a name="april-2019"></a>April 2019

### <a name="new-azure-ad-threat-intelligence-detection-is-now-available-in-refreshed-azure-ad-identity-protection"></a>Neue Azure AD Threat Intelligence-Erkennung ist jetzt in aktualisiertem Azure AD Identity Protection verfügbar

**Typ:** Neues Feature  
**Dienstkategorie:** Azure AD Identity Protection  
**Produktfunktion:** Identitätssicherheit und -schutz

Die Azure AD Threat Intelligence-Erkennung ist jetzt im aktualisiertem Azure AD Identity Protection verfügbar. Diese neue Funktionalität zeigt Benutzeraktivitäten an, die für einen spezifischen Benutzer unüblich oder mit bekannten Angriffsmustern konsistent sind, die auf der internen und externen Threat Intelligence von Microsoft basieren.

Weitere Informationen über die aktualisierte Version von Azure AD Identity Protection finden Sie im Blog [Four major Azure AD Identity Protection enhancements are now in public preview (Vier wichtige Verbesserungen von Azure AD Identity Protection sind jetzt in der öffentlichen Vorschau)](https://techcommunity.microsoft.com/t5/Azure-Active-Directory-Identity/Four-major-Azure-AD-Identity-Protection-enhancements-are-now-in/ba-p/326935) und im Artikel [Was ist Azure Active Directory Identity Protection (aktualisiert)?](https://docs.microsoft.com/azure/active-directory/identity-protection/overview-v2) . Weitere Informationen zur Azure AD Threat Intelligence-Erkennung finden Sie im Artikel [Azure AD Threat Intelligence](https://docs.microsoft.com/azure/active-directory/identity-protection/risk-events-reference#azure-ad-threat-intelligence).

---

### <a name="azure-ad-entitlement-management-is-now-available-public-preview"></a>Die Berechtigungsverwaltung von Azure AD ist jetzt öffentlich verfügbar (Public Preview)

**Typ:** Neues Feature  
**Dienstkategorie:** Identity Governance  
**Produktfunktion:** Identity Governance

Die Berechtigungsverwaltung von Azure AD ist jetzt in der Public Preview verfügbar und hilft Kunden, die Verwaltung von Zugriffspaketen zu delegieren. Dabei wird neben der Zugriffsdauer auch definiert, wie Mitarbeiter und Geschäftspartner Zugriff anfordern können und wer den Zugriff genehmigen muss. Mit Zugriffspaketen können Mitgliedschaften in Azure AD und Office 365-Gruppen, Rollenzuweisungen in Unternehmensanwendungen und Rollenzuweisungen für SharePoint Online-Sites verwaltet werden. Weitere Informationen zur Berechtigungsverwaltung finden Sie in der [Übersicht zur Berechtigungsverwaltung von Azure AD](https://docs.microsoft.com/azure/active-directory/governance/entitlement-management-overview). Weitere Informationen zu den vielseitigen Funktionen von Azure AD Identity Governance, einschließlich Privileged Identity Management, Zugriffsüberprüfungen und Nutzungsbedingungen, finden Sie unter [Was ist Azure AD Identity Governance?](../governance/identity-governance-overview.md).

---

### <a name="configure-a-naming-policy-for-office-365-groups-in-azure-ad-portal-public-preview"></a>Konfigurieren einer Benennungsrichtlinie für Office 365-Gruppen im Azure AD-Portal (öffentliche Vorschau)

**Typ:** Neues Feature  
**Dienstkategorie:** Gruppenverwaltung  
**Produktfunktion:** Zusammenarbeit

Administratoren können jetzt im Azure AD-Portal eine Benennungsrichtlinie für Office 365-Gruppen konfigurieren. Diese Änderung hilft, einheitliche Namenskonventionen für Office 365-Gruppen durchzusetzen, die von Benutzern in Ihrer Organisation erstellt oder bearbeitet werden.

Sie haben zwei Möglichkeiten, Benennungsrichtlinien für Office 365-Gruppen zu konfigurieren:

- Definieren Sie Präfixe oder Suffixe, die einem Gruppennamen automatisch hinzugefügt werden.

- Laden Sie eine benutzerdefinierte Liste blockierter Wörter hoch, die in Gruppennamen Ihrer Organisation nicht zulässig sind (z. B. „CEO, Gehalt, Personalabteilung“).

Weitere Informationen finden Sie unter [Erzwingen einer Benennungsrichtlinie für Office 365-Gruppen](https://docs.microsoft.com/azure/active-directory/users-groups-roles/groups-naming-policy).

---

### <a name="azure-ad-activity-logs-are-now-available-in-azure-monitor-general-availability"></a>Azure AD-Aktivitätsprotokolle jetzt in Azure Monitor verfügbar (Allgemeine Verfügbarkeit)

**Typ:** Neues Feature  
**Dienstkategorie:** Berichterstellung  
**Produktfunktion:** Überwachung und Berichterstellung

In Reaktion auf Ihr Feedback zu Visualisierungen anhand von Azure AD-Aktivitätsprotokollen haben wir ein neues Insights-Feature in Log Analytics eingeführt. Dieses Feature umfasst interaktive Vorlagen, so genannte „Arbeitsmappen“, die auf einfache Weise Einblicke in Ihre Azure AD-Ressourcen bieten. Die vorkonfigurierten Arbeitsmappen liefern Details zu Apps oder Benutzern und umfassen Folgendes:

- **Anmeldungen**: Enthält Details zu Apps und Benutzern, einschließlich Anmeldeort, verwendetes Betriebssystem oder Browserclient und -version sowie Anzahl erfolgreicher oder fehlerhafter Anmeldungen.

- **Legacyauthentifizierung und bedingter Zugriff.** Enthält Details zu Apps und Benutzern, die die Legacyauthentifizierung verwenden, einschließlich Multi-Factor Authentication, die durch Richtlinien für bedingten Zugriff ausgelöst wird, Apps mit Richtlinien für bedingten Zugriff usw.

- **Analyse von Anmeldefehlern**: Hiermit können Sie ermitteln, ob Anmeldefehler aufgrund von Benutzeraktionen, Richtlinienproblemen oder der Infrastruktur auftreten.

- **Benutzerdefinierte Berichte**: Sie können neue Arbeitsmappen erstellen oder vorhandene bearbeiten, um das Insights-Feature an die Anforderungen Ihrer Organisation anzupassen.

Weitere Informationen finden Sie unter [Verwenden von Azure Monitor-Arbeitsmappen für Azure Active Directory-Berichte](https://docs.microsoft.com/azure/active-directory/reports-monitoring/howto-use-azure-monitor-workbooks).

---

### <a name="new-federated-apps-available-in-azure-ad-app-gallery---april-2019"></a>Neue Verbund-Apps im Azure AD-App-Katalog verfügbar – April 2019

**Typ:** Neues Feature  
**Dienstkategorie:** Unternehmens-Apps  
**Produktfunktion:** Integration von Drittanbieterprodukten

Im April 2019 haben wir diese 21 neuen Apps mit Verbundunterstützung in den App-Katalog aufgenommen:

[SAP Fiori](https://docs.microsoft.com/azure/active-directory/saas-apps/sap-fiori-tutorial), [HRworks Single Sign-On](https://docs.microsoft.com/azure/active-directory/saas-apps/hrworks-single-sign-on-tutorial), [Percolate](https://docs.microsoft.com/azure/active-directory/saas-apps/percolate-tutorial), [MobiControl](https://docs.microsoft.com/azure/active-directory/saas-apps/mobicontrol-tutorial), [Citrix NetScaler](https://docs.microsoft.com/azure/active-directory/saas-apps/citrix-netscaler-tutorial), [Shibumi](https://docs.microsoft.com/azure/active-directory/saas-apps/shibumi-tutorial), [Benchling](https://docs.microsoft.com/azure/active-directory/saas-apps/benchling-tutorial), [MileIQ](https://mileiq.onelink.me/991934284/7e980085), [PageDNA](https://docs.microsoft.com/azure/active-directory/saas-apps/pagedna-tutorial), [EduBrite LMS](https://docs.microsoft.com/azure/active-directory/saas-apps/edubrite-lms-tutorial), [RStudio Connect](https://docs.microsoft.com/azure/active-directory/saas-apps/rstudio-connect-tutorial), [AMMS](https://docs.microsoft.com/azure/active-directory/saas-apps/amms-tutorial), [Mitel Connect](https://docs.microsoft.com/azure/active-directory/saas-apps/mitel-connect-tutorial), [Alibaba Cloud (Role-based SSO)](https://docs.microsoft.com/azure/active-directory/saas-apps/alibaba-cloud-service-role-based-sso-tutorial), [Certent Equity Management](https://docs.microsoft.com/azure/active-directory/saas-apps/certent-equity-management-tutorial), [Sectigo Certificate Manager](https://docs.microsoft.com/azure/active-directory/saas-apps/sectigo-certificate-manager-tutorial), [GreenOrbit](https://docs.microsoft.com/azure/active-directory/saas-apps/greenorbit-tutorial), [Workgrid](https://docs.microsoft.com/azure/active-directory/saas-apps/workgrid-tutorial), [monday.com](https://docs.microsoft.com/azure/active-directory/saas-apps/mondaycom-tutorial), [SurveyMonkey Enterprise](https://docs.microsoft.com/azure/active-directory/saas-apps/surveymonkey-enterprise-tutorial), [Indiggo](https://indiggolead.com/)

Weitere Informationen zu den Apps finden Sie unter [SaaS-Anwendungsintegration mit Azure Active Directory](https://aka.ms/appstutorial). Weitere Informationen zum Listen Ihrer Anwendung im Azure AD-App-Katalog finden Sie unter [Listen Ihrer Anwendung im Azure Active Directory-Anwendungskatalog](https://aka.ms/azureadapprequest).

---

### <a name="new-access-reviews-frequency-option-and-multiple-role-selection"></a>Neue Häufigkeitsoption und Auswahl mehrerer Rollen für Zugriffsüberprüfungen

**Typ:** Neues Feature  
**Dienstkategorie:** Zugriffsüberprüfungen  
**Produktfunktion:** Identity Governance

Die Neuerungen in den Azure AD-Zugriffsüberprüfungen bieten folgende Möglichkeiten:

- Sie können die Häufigkeit von Zugriffsüberprüfungen neben den bereits vorhandenen Optionen „Wöchentlich“, „Monatlich“, „Vierteljährlich“ und „Jährlich“ jetzt auch in **Halbjährlich** ändern.

- Sie können beim Erstellen einer einzelnen Zugriffsüberprüfung mehrere Azure AD- und Azure-Ressourcenrollen auswählen. In diesem Fall werden alle Rollen mit denselben Einstellungen eingerichtet, und alle Prüfer werden gleichzeitig benachrichtigt.

Weitere Informationen zum Erstellen einer Zugriffsüberprüfung finden Sie unter [Erstellen einer Zugriffsüberprüfung für Gruppen oder Anwendungen in Azure AD-Zugriffsüberprüfungen](https://docs.microsoft.com/azure/active-directory/governance/create-access-review).

---

### <a name="azure-ad-connect-email-alert-systems-are-transitioning-sending-new-email-sender-information-for-some-customers"></a>Azure AD Connect-E-Mail-Benachrichtigungssysteme werden umgestellt – für einige Kunden ändern sich die E-Mail-Absenderinformationen

**Typ:** Geändertes Feature  
**Dienstkategorie:** AD Sync  
**Produktfunktion:** Plattform

Unsere E-Mail-Benachrichtigungssysteme werden derzeit von Azure AD Connect umgestellt. Daher empfangen einige Kunden möglicherweise neue E-Mail-Absenderinformationen. Um dies zu beheben, müssen Sie der Erlaubnisliste Ihrer Organisation `azure-noreply@microsoft.com` hinzufügen, da Sie andernfalls keine wichtigen Benachrichtigungen von Ihren Office 365-, Azure- oder Synchronisierungsdiensten erhalten.

---

### <a name="upn-suffix-changes-are-now-successful-between-federated-domains-in-azure-ad-connect"></a>Änderungen am UPN-Suffix zwischen Verbunddomänen in Azure AD Connect jetzt erfolgreich durchführbar

**Typ:** Korrigiert  
**Dienstkategorie:** AD Sync  
**Produktfunktion:** Plattform

Sie können das UPN-Suffix eines Benutzers jetzt erfolgreich von einer Verbunddomäne in eine andere Verbunddomäne in Azure AD Connect ändern. Durch diesen Fix erhalten Sie während des Synchronisierungszyklus nicht mehr die FederatedDomainChangeError-Fehlermeldung oder eine Benachrichtigungs-E-Mail mit folgendem Wortlaut: „Dieses Objekt kann in Azure Active Directory nicht aktualisiert werden, da das Attribut [FederatedUser.UserPrincipalName] ungültig ist. Aktualisieren Sie den Wert in Ihren lokalen Verzeichnisdiensten“.

Weitere Informationen finden Sie unter [Beheben von Fehlern während der Synchronisierung](https://docs.microsoft.com/azure/active-directory/hybrid/tshoot-connect-sync-errors#federateddomainchangeerror).

---

### <a name="increased-security-using-the-app-protection-based-conditional-access-policy-in-azure-ad-public-preview"></a>Erhöhte Sicherheit mithilfe der Richtlinie für auf App-Schutz basierenden bedingten Zugriff in Azure AD (öffentliche Vorschau)

**Typ:** Neues Feature  
**Dienstkategorie:** Bedingter Zugriff  
**Produktfunktion:** Identitätssicherheit und -schutz

Auf App-Schutz basierender bedingter Zugriff steht jetzt über die Richtlinie **App-Schutz erforderlich** zur Verfügung. Diese neue Richtlinie hilft, die Sicherheit in Ihrer Organisation zu erhöhen, indem verhindert wird, dass:

- Benutzer ohne eine Microsoft Intune-Lizenz auf Apps zugreifen können.

- Benutzer eine Microsoft Intune-App-Schutzrichtlinie abrufen können.

- Benutzer ohne eine konfigurierte Microsoft Intune-App-Schutzrichtlinie auf Apps zugreifen können.

Weitere Informationen finden Sie unter [Vorschreiben einer App-Schutzrichtlinie für den Zugriff auf Cloud-Apps mithilfe des bedingten Zugriffs](https://docs.microsoft.com/azure/active-directory/conditional-access/app-protection-based-conditional-access).

---

### <a name="new-support-for-azure-ad-single-sign-on-and-conditional-access-in-microsoft-edge-public-preview"></a>Neue Unterstützung für einmaliges Anmelden und bedingten Zugriff mit Azure AD in Microsoft Edge (Öffentliche Vorschau)

**Typ:** Neues Feature  
**Dienstkategorie:** Bedingter Zugriff  
**Produktfunktion:** Identitätssicherheit und -schutz

Wir haben unsere Azure AD-Unterstützung für Microsoft Edge erweitert, einschließlich der Bereitstellung von neuer Unterstützung für einmaliges Anmelden und bedingten Zugriff mit Azure AD. Wenn Sie bisher Microsoft Intune Managed Browser verwendet haben, können Sie jetzt stattdessen Microsoft Edge verwenden.

Weitere Informationen zum Einrichten und Verwalten Ihrer Geräte und Apps mithilfe des bedingten Zugriffs finden Sie unter [Vorschreiben der Verwendung verwalteter Geräte für den Zugriff auf Cloud-Apps mithilfe des bedingten Zugriffs](https://docs.microsoft.com/azure/active-directory/conditional-access/require-managed-devices) und [Vorschreiben der Verwendung von genehmigten Client-Apps für den Zugriff auf Cloud-Apps mithilfe des bedingten Zugriffs](https://docs.microsoft.com/azure/active-directory/conditional-access/app-based-conditional-access). Weitere Informationen zum Verwalten des Zugriffs mithilfe von Microsoft Edge mit Microsoft Intune-Richtlinien finden Sie unter [Verwalten des Internetzugriffs mithilfe eines durch die Microsoft Intune-Richtlinie geschützten Browsers](https://docs.microsoft.com/intune/app-configuration-managed-browser).

---

## <a name="march-2019"></a>März 2019

### <a name="identity-experience-framework-and-custom-policy-support-in-azure-active-directory-b2c-is-now-available-ga"></a>Unterstützung von Identity Experience Framework und benutzerdefinierter Richtlinie ist jetzt in Azure Active Directory B2C verfügbar (GA)

**Typ:** Neues Feature  
**Dienstkategorie:** B2C – Verwaltung von Endbenutzeridentitäten  
**Produktfunktion:** B2B/B2C

Sie können jetzt in Azure AD B2C benutzerdefinierte Richtlinien inklusive der folgenden Aufgaben erstellen, die bedarfsorientiert und im Rahmen unserer Azure-SLA unterstützt werden:

- Erstellen und Hochladen benutzerdefinierter User Journeys für die Authentifizierung mit benutzerdefinierten Richtlinien.

- Beschreiben von User Journeys als Austauschvorgänge zwischen Anspruchsanbietern (Schritt für Schritt)

- Definieren der bedingten Verzweigung in User Journeys

- Transformieren und Zuordnen von Ansprüchen für die Verwendung in Echtzeitentscheidungen und -kommunikation.

- Verwenden REST-API-fähiger Dienste in Ihren benutzerdefinierten User Journeys für die Authentifizierung. Beispielsweise mit E-Mail-Anbietern, CRMs und proprietären Autorisierungssystemen.

- Einrichten von Verbünden mit Identitätsanbietern, die mit dem OpenIDConnect-Protokoll konform sind. Beispielsweise mit mehrinstanzenfähigem Azure AD, Anbietern von sozialen Konten oder Anbietern der zweistufigen Authentifizierung.

Weitere Informationen zum Erstellen benutzerdefinierter Richtlinien finden Sie unter [Entwicklerhinweise zu benutzerdefinierten Richtlinien in Azure Active Directory B2C](https://docs.microsoft.com/azure/active-directory-b2c/active-directory-b2c-developer-notes-custom), und lesen Sie den [Blogbeitrag von Alex Simons mit den Fallstudien](https://techcommunity.microsoft.com/t5/Azure-Active-Directory-Identity/Azure-AD-B2C-custom-policies-to-build-your-own-identity-journeys/ba-p/382791).

---

### <a name="new-federated-apps-available-in-azure-ad-app-gallery---march-2019"></a>Neue Verbund-Apps im Azure AD-App-Katalog verfügbar – März 2019

**Typ:** Neues Feature  
**Dienstkategorie:** Unternehmens-Apps  
**Produktfunktion:** Integration von Drittanbieterprodukten

Im März 2019 haben wir diese 14 neuen Apps mit Verbundunterstützung in den App-Katalog aufgenommen:

[ISEC7 Mobile Exchange Delegate](https://www.isec7.com/english/), [MediusFlow](https://office365.cloudapp.mediusflow.com/), [ePlatform](https://docs.microsoft.com/azure/active-directory/saas-apps/eplatform-tutorial), [Fulcrum](https://docs.microsoft.com/azure/active-directory/saas-apps/fulcrum-tutorial), [ExcelityGlobal](https://docs.microsoft.com/azure/active-directory/saas-apps/excelityglobal-tutorial), [Explanation-Based Auditing System](https://docs.microsoft.com/azure/active-directory/saas-apps/explanation-based-auditing-system-tutorial), [Lean](https://docs.microsoft.com/azure/active-directory/saas-apps/lean-tutorial), [Powerschool Performance Matters](https://docs.microsoft.com/azure/active-directory/saas-apps/powerschool-performance-matters-tutorial), [Cinode](https://cinode.com/), [Iris Intranet](https://docs.microsoft.com/azure/active-directory/saas-apps/iris-intranet-tutorial), [Empactis](https://docs.microsoft.com/azure/active-directory/saas-apps/empactis-tutorial), [SmartDraw](https://docs.microsoft.com/azure/active-directory/saas-apps/smartdraw-tutorial), [Confirmit Horizons](https://docs.microsoft.com/azure/active-directory/saas-apps/confirmit-horizons-tutorial), [TAS](https://docs.microsoft.com/azure/active-directory/saas-apps/tas-tutorial)

Weitere Informationen zu den Apps finden Sie unter [SaaS-Anwendungsintegration mit Azure Active Directory](https://aka.ms/appstutorial). Weitere Informationen zum Listen Ihrer Anwendung im Azure AD-App-Katalog finden Sie unter [Listen Ihrer Anwendung im Azure Active Directory-Anwendungskatalog](https://aka.ms/azureadapprequest).

---

### <a name="new-zscaler-and-atlassian-provisioning-connectors-in-the-azure-ad-gallery---march-2019"></a>Neue Zscaler- und Atlassian-Bereitstellungsconnectors im Azure AD-Katalog – März 2019

**Typ:** Neues Feature  
**Dienstkategorie:** App-Bereitstellung  
**Produktfunktion:** Integration von Drittanbieterprodukten

Automatisieren Sie das Erstellen, Aktualisieren und Löschen von Benutzerkonten für die folgenden Apps:

[Zscaler](https://aka.ms/ZscalerProvisioning), [Zscaler Beta](https://aka.ms/ZscalerBetaProvisioning), [Zscaler One](https://aka.ms/ZscalerOneProvisioning), [Zscaler Two](https://aka.ms/ZscalerTwoProvisioning), [Zscaler Three](https://aka.ms/ZscalerThreeProvisioning), [Zscaler ZSCloud](https://aka.ms/ZscalerZSCloudProvisioning), [Atlassian Cloud](https://aka.ms/atlassianCloudProvisioning)

Weitere Informationen dazu, wie Sie Ihre Organisation durch die automatisierte Bereitstellung von Benutzerkonten besser sichern können, finden Sie unter [Automatisieren der Bereitstellung und Bereitstellungsaufhebung von Benutzern für SaaS-Anwendungen mit Azure Active Directory](https://aka.ms/ProvisioningDocumentation).

---

### <a name="restore-and-manage-your-deleted-office-365-groups-in-the-azure-ad-portal"></a>Wiederherstellen und Verwalten Ihrer gelöschten Office 365-Gruppen im Azure AD-Portal

**Typ:** Neues Feature  
**Dienstkategorie:** Gruppenverwaltung  
**Produktfunktion:** Zusammenarbeit

Sie können Ihre gelöschten Office 365-Gruppen jetzt vom Azure AD-Portal aus anzeigen und verwalten. Diese Änderung erleichtert Ihnen zu sehen, welche Gruppen wiederhergestellt werden können, und ermöglicht Ihnen gleichzeitig, endgültig alle Gruppen zu löschen, die Ihre Organisation nicht mehr benötigt.

Weitere Informationen finden Sie unter [Anzeigen und Verwalten der wiederherstellbaren gelöschten Office 365-Gruppen](https://docs.microsoft.com/azure/active-directory/users-groups-roles/groups-restore-deleted#view-and-manage-the-deleted-office-365-groups-that-are-available-to-restore).

---

### <a name="single-sign-on-is-now-available-for-azure-ad-saml-secured-on-premises-apps-through-application-proxy-public-preview"></a>Einmaliges Anmelden steht jetzt für Azure AD-SAML-gesicherte lokale Apps über den Anwendungsproxy zur Verfügung (Public Preview).

**Typ:** Neues Feature  
**Dienstkategorie:** Anwendungsproxy  
**Produktfunktion:** Zugriffssteuerung

Sie können nun eine Benutzeroberfläche für einmaliges Anmelden (SSO) für lokale, SAML-authentifizierte Apps zusammen mit Remotezugriff auf diese Apps über den Anwendungsproxy bereitstellen. Weitere Informationen zum Einrichten von SAML-SSO mit Ihren lokalen Apps finden Sie unter [SAML-SSO (einmaliges Anmelden) für lokale Anwendungen mit dem Anwendungsproxy (Vorschauversion)](https://docs.microsoft.com/azure/active-directory/manage-apps/application-proxy-configure-single-sign-on-on-premises-apps).

---

### <a name="client-apps-in-request-loops-will-be-interrupted-to-improve-reliability-and-user-experience"></a>Client-Apps in Anforderungsschleifen werden unterbrochen, um Zuverlässigkeit und Benutzerfreundlichkeit zu verbessern.

**Typ:** Neues Feature  
**Dienstkategorie:** Authentifizierungen (Anmeldungen)  
**Produktfunktion:** Benutzerauthentifizierung

Client-Apps können in einem kurzen Zeitraum Hunderte der gleichen Anmeldeanforderungen falsch ausgeben. Alle diese Anforderungen, ob erfolgreich oder nicht, tragen zu einer schlechten Benutzererfahrung und erhöhten Workloads für den IDP bei, was zu einer höheren Latenz für alle Benutzer und einer geringeren Verfügbarkeit des IDP führt.

Dieses Update sendet eine `invalid_grant`-Fehlermeldung: `AADSTS50196: The server terminated an operation because it encountered a loop while processing a request` an Client-Apps, die mehrmals über den Rahmen des normalen Betriebs hinausgehend in einem kurzen Zeitraum doppelte Anforderungen ausgeben. Client-Apps, bei denen dieses Problem auftritt, sollten eine interaktive Eingabeaufforderung anzeigen, die den Benutzer zur erneuten Anmeldung auffordert. Weitere Informationen zu dieser Änderung und zur Korrektur Ihrer App bei Auftreten dieses Fehlers finden Sie unter [In der Schleife befindliche Clients werden unterbrochen](https://docs.microsoft.com/azure/active-directory/develop/reference-breaking-changes#looping-clients-will-be-interrupted).

---

### <a name="new-audit-logs-user-experience-now-available"></a>Neue Benutzeroberfläche für Überwachungsprotokolle jetzt verfügbar

**Typ:** Geändertes Feature  
**Dienstkategorie:** Berichterstellung  
**Produktfunktion:** Überwachung und Berichterstellung

Wir haben eine neue Azure AD-**Überwachungsprotokolle**-Seite erstellt, um die Lesbarkeit zu verbessern und die Art, in der Sie Ihre Informationen suchen. Um die neue **Überwachungsprotokolle**-Seite anzuzeigen, wählen Sie **Überwachungsprotokolle** im Abschnitt **Aktivität** von Azure AD aus.

![Neue Seite „Überwachungsprotokolle“ mit Beispielinformationen](media/whats-new/audit-logs-page.png)

Weitere Informationen über die neue Seite **Überwachungsprotokolle** finden Sie unter [Berichte zu Überwachungsaktivitäten im Azure Active Directory-Portal](https://docs.microsoft.com/azure/active-directory/reports-monitoring/concept-audit-logs#audit-logs).

---

### <a name="new-warnings-and-guidance-to-help-prevent-accidental-administrator-lockout-from-misconfigured-conditional-access-policies"></a>Neue Warnungen und Anleitungen, um das versehentliche Aussperren des Administrators aufgrund falsch konfigurierter Richtlinien für den bedingten Zugriff zu verhindern

**Typ:** Geändertes Feature  
**Dienstkategorie:** Bedingter Zugriff  
**Produktfunktion:** Identitätssicherheit und -schutz

Um zu verhindern, dass Administratoren sich versehentlich selbst von ihren eigenen Mandanten aus durch falsch konfigurierte Richtlinien für bedingten Zugriff aussperren, haben wir neue Warnungen und aktualisierte Anleitungen im Azure-Portal erstellt. Weitere Informationen zu den neuen Anleitungen finden Sie im Artikel [Was sind Dienstabhängigkeiten beim bedingten Azure Active Directory-Zugriff?](https://docs.microsoft.com/azure/active-directory/conditional-access/service-dependencies).

---

### <a name="improved-end-user-terms-of-use-experiences-on-mobile-devices"></a>Verbesserte Benutzeroberfläche zur Anzeige von Nutzungsbedingungen für Endbenutzer auf mobilen Geräten

**Typ:** Geändertes Feature  
**Dienstkategorie:** Nutzungsbedingungen  
**Produktfunktion:** Governance

Wir haben unsere vorhandene Benutzeroberfläche zur Anzeige von Nutzungsbedingungen aktualisiert, um Ihnen auf einem mobilen Gerät zu erleichtern, Nutzungsbedingungen zu überprüfen und ihnen zuzustimmen. Sie können jetzt vergrößern und verkleinern, zurückgehen, Informationen herunterladen und Links auswählen. Weitere Informationen zu den aktualisierten Nutzungsbedingungen finden Sie unter [Anzeige von Nutzungsbedingungen für Benutzer](https://docs.microsoft.com/azure/active-directory/conditional-access/terms-of-use#what-terms-of-use-looks-like-for-users).

---

### <a name="new-azure-ad-activity-logs-download-experience-available"></a>Neue Benutzeroberfläche zum Herunterladen von Azure AD-Aktivitätsprotokollen verfügbar

**Typ:** Geändertes Feature  
**Dienstkategorie:** Berichterstellung  
**Produktfunktion:** Überwachung und Berichterstellung

Sie können große Mengen von Aktivitätsprotokollen jetzt direkt über das Azure-Portal herunterladen. Dieses Update ermöglicht Ihnen Folgendes:

- Herunterladen von bis zu 250.000 Zeilen.

- Erhalten einer Benachrichtigung bei Abschluss des Downloads.

- Anpassen Ihres Dateinamens.

- Bestimmen Ihres Ausgabeformats, entweder JSON oder CSV.

Ausführlichere Informationen zu diesem Feature finden Sie unter [Schnellstart: Herunterladen eines Überwachungsberichts über das Azure-Portal](https://docs.microsoft.com/azure/active-directory/reports-monitoring/quickstart-download-audit-report).

---

### <a name="breaking-change-updates-to-condition-evaluation-by-exchange-activesync-eas"></a>Wichtige Änderung: Updates für die Bedingungsauswertung bei Exchange ActiveSync (EAS)

**Typ:** Plan für Änderung  
**Dienstkategorie:** Bedingter Zugriff  
**Produktfunktion:** Zugriffssteuerung

Wir aktualisieren momentan die Art, in der Exchange ActiveSync (EAS) die folgenden Bedingungen auswertet:

- Standort des Benutzers basierend auf Land, Region oder IP-Adresse

- Anmelderisiko

- Geräteplattform

Wenn Sie diese Bedingungen zuvor in Ihren Richtlinien für bedingten Zugriff verwendet haben, denken Sie daran, dass das Bedingungsverhalten sich ändern könnte. Wenn Sie z.B. zuvor die Benutzerstandortbedingung in einer Richtlinie verwendet haben, stellen Sie möglicherweise fest, dass die Richtlinie jetzt je nach Standort des Benutzers übersprungen wird.

---

## <a name="february-2019"></a>Februar 2019

### <a name="configurable-azure-ad-saml-token-encryption-public-preview"></a>Konfigurierbare Azure AD SAML-Tokenverschlüsselung (öffentliche Vorschau) 

**Typ:** Neues Feature  
**Dienstkategorie:** Unternehmens-Apps  
**Produktfunktion:** SSO

Sie können jetzt jede unterstützte SAML-App so konfigurieren, dass sie verschlüsselte SAML-Token erhält. Bei Konfiguration und Verwendung mit einer App verschlüsselt Azure AD die ausgegebenen SAML-Assertionen mit einem öffentlichen Schlüssel, der von einem in Azure AD gespeicherten Zertifikat bereitgestellt wird.

Weitere Informationen zum Konfigurieren Ihrer SAML-Tokenverschlüsselung finden Sie unter [Konfigurieren der Azure AD-SAML-Tokenverschlüsselung](https://docs.microsoft.com/azure/active-directory/manage-apps/howto-saml-token-encryption).

---

### <a name="create-an-access-review-for-groups-or-apps-using-azure-ad-access-reviews"></a>Erstellen einer Zugriffsüberprüfung für Gruppen oder Apps mit Azure AD-Zugriffsüberprüfungen

**Typ:** Neues Feature  
**Dienstkategorie:** Zugriffsüberprüfungen  
**Produktfunktion:** Governance

Sie können jetzt mehrere Gruppen oder Apps in eine einzige Azure AD-Zugriffsüberprüfung für die Gruppenmitgliedschaft oder App-Zuweisung einschließen. Zugriffsüberprüfungen mit mehreren Gruppen oder Apps werden mit denselben Einstellungen eingerichtet, und alle einbezogenen Prüfer werden gleichzeitig benachrichtigt.

Weitere Informationen zum Erstellen einer Zugriffsüberprüfung mithilfe von Azure AD-Zugriffsüberprüfungen finden Sie unter [Erstellen einer Zugriffsüberprüfung von Gruppenmitgliedern oder dem Anwendungszugriff mit Azure AD](https://docs.microsoft.com/azure/active-directory/governance/create-access-review).

---

### <a name="new-federated-apps-available-in-azure-ad-app-gallery---february-2019"></a>Neue Verbund-Apps im Azure AD-App-Katalog verfügbar – Februar 2019

**Typ:** Neues Feature  
**Dienstkategorie:** Unternehmens-Apps  
**Produktfunktion:** Integration von Drittanbieterprodukten
 
Im Februar 2019 haben wir diese 27 neuen Apps mit Verbundunterstützung in unseren App-Katalog aufgenommen:

[Euromonitor Passport](https://docs.microsoft.com/azure/active-directory/saas-apps/euromonitor-passport-tutorial), [MindTickle](https://docs.microsoft.com/azure/active-directory/saas-apps/mindtickle-tutorial), [FAT FINGER](https://seeforgetest-exxon.azurewebsites.net/Account/create?Length=7), [AirStack](https://docs.microsoft.com/azure/active-directory/saas-apps/airstack-tutorial), [Oracle Fusion ERP](https://docs.microsoft.com/azure/active-directory/saas-apps/oracle-fusion-erp-tutorial), [IDrive](https://docs.microsoft.com/azure/active-directory/saas-apps/idrive-tutorial), [Skyward Qmlativ](https://docs.microsoft.com/azure/active-directory/saas-apps/skyward-qmlativ-tutorial), [Brightidea](https://docs.microsoft.com/azure/active-directory/saas-apps/brightidea-tutorial), [AlertOps](https://docs.microsoft.com/azure/active-directory/saas-apps/alertops-tutorial), [Soloinsight-CloudGate SSO](https://docs.microsoft.com/azure/active-directory/saas-apps/soloinsight-cloudgate-sso-tutorial), Permission Click, [Brandfolder](https://docs.microsoft.com/azure/active-directory/saas-apps/brandfolder-tutorial), [StoregateSmartFile](https://docs.microsoft.com/azure/active-directory/saas-apps/smartfile-tutorial), [Pexip](https://docs.microsoft.com/azure/active-directory/saas-apps/pexip-tutorial), [Stormboard](https://docs.microsoft.com/azure/active-directory/saas-apps/stormboard-tutorial), [Seismic](https://docs.microsoft.com/azure/active-directory/saas-apps/seismic-tutorial), [Share A Dream](https://www.shareadream.org/how-it-works), [Bugsnag](https://docs.microsoft.com/azure/active-directory/saas-apps/bugsnag-tutorial), [webMethods Integration Cloud](https://docs.microsoft.com/azure/active-directory/saas-apps/webmethods-integration-cloud-tutorial), [Knowledge Anywhere LMS](https://docs.microsoft.com/azure/active-directory/saas-apps/knowledge-anywhere-lms-tutorial), [OU Campus](https://docs.microsoft.com/azure/active-directory/saas-apps/ou-campus-tutorial), [Periscope Data](https://docs.microsoft.com/azure/active-directory/saas-apps/periscope-data-tutorial), [Netop Portal](https://docs.microsoft.com/azure/active-directory/saas-apps/netop-portal-tutorial), [smartvid.io](https://docs.microsoft.com/azure/active-directory/saas-apps/smartvid.io-tutorial), [PureCloud by Genesys](https://docs.microsoft.com/azure/active-directory/saas-apps/purecloud-by-genesys-tutorial), [ClickUp Productivity Platform](https://docs.microsoft.com/azure/active-directory/saas-apps/clickup-productivity-platform-tutorial)

Weitere Informationen zu den Apps finden Sie unter [SaaS-Anwendungsintegration mit Azure Active Directory](https://aka.ms/appstutorial). Weitere Informationen zum Listen Ihrer Anwendung im Azure AD-App-Katalog finden Sie unter [Listen Ihrer Anwendung im Azure Active Directory-Anwendungskatalog](https://aka.ms/azureadapprequest).

---

### <a name="enhanced-combined-mfasspr-registration"></a>Erweiterte kombinierte MFA-/SSPR-Registrierung

**Typ:** Geändertes Feature  
**Dienstkategorie:** Self-Service-Kennwortzurücksetzung  
**Produktfunktion:** Benutzerauthentifizierung

Als Reaktion auf Kundenfeedback haben wir die Vorschauoberfläche für die kombinierte MFA-/SSPR-Registrierung verbessert, damit Ihre Benutzer ihre Sicherheitsinformationen für MFA und SSPR schneller registrieren können. 

**Gehen Sie wie folgt vor, um die verbesserte Oberfläche für Ihre Benutzer sofort zu aktivieren:**

1. Melden Sie sich als globaler Administrator oder Benutzeradministrator beim Azure-Portal an, und wechseln Sie zu **Azure Active Directory > Benutzereinstellungen > Einstellungen für Zugriffspanel-Vorschaufeatures verwalten**. 

2. Wählen Sie unter der Option **Benutzer können Vorschaufunktionen zum Registrieren und Verwalten von Sicherheitsinformationen verwenden (Aktualisierung)** aus, ob Sie die Features für eine **ausgewählte Gruppe von Benutzern** oder für **alle Benutzer** aktivieren möchten.

Im Laufe der nächsten Wochen entfernen wir die Möglichkeit, die alte Vorschauoberfläche für die kombinierte MFA-/SSPR-Registrierung für Mandanten zu aktivieren, für die sie noch nicht aktiviert wurde.

**Gehen Sie wie folgt vor, um festzustellen, ob das Steuerelement für Ihren Mandanten entfernt wird:**

1. Melden Sie sich als globaler Administrator oder Benutzeradministrator beim Azure-Portal an, und wechseln Sie zu **Azure Active Directory > Benutzereinstellungen > Einstellungen für Zugriffspanel-Vorschaufeatures verwalten**.  

2. Wenn die Option **Benutzer können Vorschaufunktionen zum Registrieren und Verwalten von Sicherheitsinformationen verwenden** auf **Kein** festgelegt ist, wird die Option von Ihrem Mandanten entfernt.

Unabhängig davon, ob Sie zuvor die alte Vorschauoberfläche für die kombinierte MFA-/SSPR-Registrierung für Benutzer aktiviert haben, wird die alte Benutzeroberfläche zu einem späteren Zeitpunkt deaktiviert. Aus diesem Grund raten wir Ihnen, so schnell wie möglich zur neuen erweiterten Oberfläche zu wechseln.

Weitere Informationen zur erweiterte Registrierungsoberfläche finden Sie unter [Cool enhancements to the Azure AD combined MFA and password reset registration experience](https://techcommunity.microsoft.com/t5/Azure-Active-Directory-Identity/Cool-enhancements-to-the-Azure-AD-combined-MFA-and-password/ba-p/354271) (Verbesserungen an der Oberfläche für die kombinierte Oberfläche für die MFA-Registrierung und Kennwortzurücksetzung).

---

### <a name="updated-policy-management-experience-for-user-flows"></a>Aktualisierte Richtlinienverwaltungsoberfläche für Benutzerflows

**Typ:** Geändertes Feature  
**Dienstkategorie:** B2C – Verwaltung von Endbenutzeridentitäten  
**Produktfunktion:** B2B/B2C

Wir haben den Vorgang zur Richtlinienerstellung und -verwaltung für Benutzerflows (zuvor als integrierte Richtlinien bezeichnet) vereinfacht. Diese neue Oberfläche gilt jetzt standardmäßig für all Ihre Azure AD-Mandanten.

Über die Symbole „Lächeln“ und „Stirnrunzeln“ im Bereich **Feedback senden** im oberen Bereich des Portals können Sie zusätzliches Feedback und Vorschläge angeben.

Weitere Informationen zur neuen Richtlinienverwaltungsoberfläche finden Sie im Blog [Azure AD B2C now has JavaScript customization and many more new features](https://techcommunity.microsoft.com/t5/Azure-Active-Directory-Identity/Azure-AD-B2C-now-has-JavaScript-customization-and-many-more-new/ba-p/353595) (Azure AAD B2C bietet jetzt JavaScript-Anpassung und viele weitere Features).

---

### <a name="choose-specific-page-element-versions-provided-by-azure-ad-b2c"></a>Auswahl bestimmter von Azure AD B2C bereitgestellter Seitenelementversionen

**Typ:** Neues Feature  
**Dienstkategorie:** B2C – Verwaltung von Endbenutzeridentitäten  
**Produktfunktion:** B2B/B2C

Jetzt können Sie eine bestimmte Version der von Azure AD B2C bereitgestellten Seitenelemente auswählen. Wenn Sie eine bestimmte Version auswählen, können Sie Ihre Updates testen, bevor sie auf einer Seite angezeigt werden, und Sie können vorhersagbares Verhalten erzielen. Darüber hinaus können Sie jetzt auswählen, dass bestimmte Seitenversionen erzwungen werden, um JavaScript-Anpassungen zuzulassen. Um dieses Feature zu aktivieren, wechseln Sie in Ihren Benutzerflows zur Seite **Eigenschaften**.

Weitere Informationen zur Auswahl bestimmter Versionen von Seitenelementen finden Sie im Blog [Azure AD B2C now has JavaScript customization and many more new features](https://techcommunity.microsoft.com/t5/Azure-Active-Directory-Identity/Azure-AD-B2C-now-has-JavaScript-customization-and-many-more-new/ba-p/353595) (Azure AAD B2C bietet jetzt JavaScript-Anpassung und viele weitere Features).

---

### <a name="configurable-end-user-password-requirements-for-b2c-ga"></a>Konfigurierbare Endbenutzerkennwortanforderungen für B2C (GA)

**Typ:** Neues Feature  
**Dienstkategorie:** B2C – Verwaltung von Endbenutzeridentitäten  
**Produktfunktion:** B2B/B2C

Jetzt können Sie die Kennwortkomplexität Ihrer Organisation für Ihre Endbenutzer einrichten, statt die native Azure AD-Kennwortrichtlinie verwenden zu müssen. Auf dem Blatt **Eigenschaften** Ihrer Benutzerflows (zuvor als integrierte Richtlinien bezeichnet) können Sie die Kennwortkomplexität **Einfach** oder **Stark** auswählen oder **benutzerdefinierte** Anforderungen erstellen.

Weitere Informationen zur Konfiguration von Anforderungen für die Kennwortkomplexität finden Sie unter [Konfigurieren der Komplexitätsanforderungen für Kennwörter in Azure Active Directory B2C](https://docs.microsoft.com/azure/active-directory-b2c/active-directory-b2c-reference-password-complexity).

---

### <a name="new-default-templates-for-custom-branded-authentication-experiences"></a>Neue Standardvorlagen für benutzerdefinierte mit der Marke versehene Authentifizierungsoberflächen

**Typ:** Neues Feature  
**Dienstkategorie:** B2C – Verwaltung von Endbenutzeridentitäten  
**Produktfunktion:** B2B/B2C

Mit unseren neuen Standardvorlagen auf dem Blatt **Seitenlayouts** Ihrer Benutzerflows (zuvor als integrierte Richtlinien bezeichnet) können Sie einen benutzerdefinierten mit Ihrer Marke versehenen Authentifizierungsvorgang für Ihre Benutzer erstellen.

Weitere Informationen zur Verwendung der Vorlagen finden Sie unter [Azure AD B2C now has JavaScript customization and many more new features](https://techcommunity.microsoft.com/t5/Azure-Active-Directory-Identity/Azure-AD-B2C-now-has-JavaScript-customization-and-many-more-new/ba-p/353595) (Azure AAD B2C bietet jetzt JavaScript-Anpassung und viele weitere Features).

---

## <a name="january-2019"></a>Januar 2019

### <a name="active-directory-b2b-collaboration-using-one-time-passcode-authentication-public-preview"></a>Active Directory B2B-Zusammenarbeit über Authentifizierung mit Einmalkennung (öffentliche Vorschau)

**Typ:** Neues Feature  
**Dienstkategorie:** B2B  
**Produktfunktion:** B2B/B2C

Für B2B-Gastbenutzer, die sich auf andere Weise (z.B. über Azure AD, ein Microsoft-Konto (MSA) oder den Verbund mit Google) nicht authentifizieren können, haben wir die Authentifizierung mit Einmalkennung eingeführt. Diese neue Authentifizierungsmethode bedeutet, dass Gastbenutzer kein neues Microsoft-Konto erstellen müssen. Stattdessen kann ein Gastbenutzer beim Einlösen einer Einladung oder beim Zugriff auf eine freigegebene Ressource einen temporären Code anfordern, der an eine E-Mail-Adresse gesendet wird. Mit diesem temporären Code kann der Gastbenutzer seine Anmeldung fortsetzen.

Weitere Informationen finden Sie unter [Authentifizierung mit Einmalkennung per E-Mail (Vorschauversion)](https://docs.microsoft.com/azure/active-directory/b2b/one-time-passcode) und im Blogartikel [Azure AD makes sharing and collaboration seamless for any user with any account](https://techcommunity.microsoft.com/t5/Azure-Active-Directory-Identity/Azure-AD-makes-sharing-and-collaboration-seamless-for-any-user/ba-p/325949) (Nahtlose Freigabe und Zusammenarbeit in Azure AD für alle Benutzer mit beliebigen Konten).

### <a name="new-azure-ad-application-proxy-cookie-settings"></a>Neue Cookieeinstellungen für den Azure AD-Anwendungsproxy

**Typ:** Neues Feature  
**Dienstkategorie:** Anwendungsproxy  
**Produktfunktion:** Zugriffssteuerung

Wir haben drei neue Cookieeinstellungen eingeführt, die für Ihre Apps verfügbar sind, die über den Anwendungsproxy veröffentlicht werden:

- **Nur-HTTP-Cookie verwenden.** Legt das **HTTPOnly**-Flag für Ihre Anwendungsproxyzugriffs- und Sitzungscookies fest. Das Aktivieren dieser Einstellung bietet zusätzliche Sicherheitsvorteile, z.B. verhindert es das Kopieren oder Ändern von Cookies durch clientseitige Skripts. Sie sollten dieses Flag wg. der zusätzlichen Vorteile aktivieren (wählen Sie **Ja** aus).

- **Sicheres Cookie verwenden.** Legt das **Secure**-Flag für Ihre Anwendungsproxyzugriffs- und Sitzungscookies fest. Das Aktivieren dieser Einstellung bietet zusätzliche Sicherheitsvorteile, indem sichergestellt wird, dass Cookies nur über sichere TLS-Kanäle wie z.B. HTTPS übertragen werden. Sie sollten dieses Flag wg. der zusätzlichen Vorteile aktivieren (wählen Sie **Ja** aus).

- **Beständiges Cookie verwenden.** Verhindert, dass Zugriffcookies ablaufen, wenn der Webbrowser geschlossen wird. Diese Cookies existieren für die Lebensdauer des Zugriffstokens. Allerdings werden die Cookies zurückgesetzt, wenn der Ablaufzeitpunkt erreicht ist, oder wenn der Benutzer das Cookie manuell löscht. Sie sollten die Standardeinstellung **Nein** beibehalten und die Einstellung nur für ältere Apps aktivieren, die keine Cookies zwischen Prozessen freigeben.

Weitere Informationen zu den neuen Cookies finden Sie unter [Cookieeinstellungen für den Zugriff auf lokale Anwendungen in Azure Active Directory](https://docs.microsoft.com/azure/active-directory/manage-apps/application-proxy-configure-cookie-settings).

---

### <a name="new-federated-apps-available-in-azure-ad-app-gallery---january-2019"></a>Neue Verbund-Apps im Azure AD-App-Katalog verfügbar – Januar 2019

**Typ:** Neues Feature  
**Dienstkategorie:** Unternehmens-Apps  
**Produktfunktion:** Integration von Drittanbieterprodukten
 
Im Juli 2019 haben wir diese 35 neuen Apps mit Verbundunterstützung in unseren App-Katalog aufgenommen:

[Firstbird](https://docs.microsoft.com/azure/active-directory/saas-apps/firstbird-tutorial), [Folloze](https://docs.microsoft.com/azure/active-directory/saas-apps/folloze-tutorial), [Talent Palette](https://docs.microsoft.com/azure/active-directory/saas-apps/talent-palette-tutorial), [Infor CloudSuite](https://docs.microsoft.com/azure/active-directory/saas-apps/infor-cloud-suite-tutorial), [Cisco Umbrella](https://docs.microsoft.com/azure/active-directory/saas-apps/cisco-umbrella-tutorial), [Zscaler Internet Access Administrator](https://docs.microsoft.com/azure/active-directory/saas-apps/zscaler-internet-access-administrator-tutorial), [Expiration Reminder](https://docs.microsoft.com/azure/active-directory/saas-apps/expiration-reminder-tutorial), [InstaVR Viewer](https://docs.microsoft.com/azure/active-directory/saas-apps/instavr-viewer-tutorial), [CorpTax](https://docs.microsoft.com/azure/active-directory/saas-apps/corptax-tutorial), [Verb](https://app.verb.net/login), [OpenLattice](https://openlattice.com/agora), [TheOrgWiki](https://www.theorgwiki.com/signup), [Pavaso Digital Close](https://docs.microsoft.com/azure/active-directory/saas-apps/pavaso-digital-close-tutorial), [GoodPractice Toolkit](https://docs.microsoft.com/azure/active-directory/saas-apps/goodpractice-toolkit-tutorial), [Cloud Service PICCO](https://docs.microsoft.com/azure/active-directory/saas-apps/cloud-service-picco-tutorial), [AuditBoard](https://docs.microsoft.com/azure/active-directory/saas-apps/auditboard-tutorial), [iProva](https://docs.microsoft.com/azure/active-directory/saas-apps/iprova-tutorial), [Workable](https://docs.microsoft.com/azure/active-directory/saas-apps/workable-tutorial), [CallPlease](https://webapp.callplease.com/create-account/create-account.html), [GTNexus SSO System](https://docs.microsoft.com/azure/active-directory/saas-apps/gtnexus-sso-module-tutorial), [CBRE ServiceInsight](https://docs.microsoft.com/azure/active-directory/saas-apps/cbre-serviceinsight-tutorial), [Deskradar](https://docs.microsoft.com/azure/active-directory/saas-apps/deskradar-tutorial), [Coralogixv](https://docs.microsoft.com/azure/active-directory/saas-apps/coralogix-tutorial), [Signagelive](https://docs.microsoft.com/azure/active-directory/saas-apps/signagelive-tutorial), [ARES for Enterprise](https://docs.microsoft.com/azure/active-directory/saas-apps/ares-for-enterprise-tutorial), [K2 for Office 365](https://www.k2.com/O365), [Xledger](https://www.xledger.net/), [iDiD Manager](https://docs.microsoft.com/azure/active-directory/saas-apps/idid-manager-tutorial), [HighGear](https://docs.microsoft.com/azure/active-directory/saas-apps/highgear-tutorial), [Visitly](https://docs.microsoft.com/azure/active-directory/saas-apps/visitly-tutorial), [Korn Ferry ALP](https://docs.microsoft.com/azure/active-directory/saas-apps/korn-ferry-alp-tutorial), [Acadia](https://docs.microsoft.com/azure/active-directory/saas-apps/acadia-tutorial), [Adoddle cSaas Platform](https://docs.microsoft.com/azure/active-directory/saas-apps/adoddle-csaas-platform-tutorial)<!-- , [CaféX Portal (Meetings)](https://docs.microsoft.com/azure/active-directory/saas-apps/cafexportal-meetings-tutorial), [MazeMap Link](https://docs.microsoft.com/azure/active-directory/saas-apps/mazemaplink-tutorial)-->  

Weitere Informationen zu den Apps finden Sie unter [SaaS-Anwendungsintegration mit Azure Active Directory](https://aka.ms/appstutorial). Weitere Informationen zum Listen Ihrer Anwendung im Azure AD-App-Katalog finden Sie unter [Listen Ihrer Anwendung im Azure Active Directory-Anwendungskatalog](https://aka.ms/azureadapprequest).

---

### <a name="new-azure-ad-identity-protection-enhancements-public-preview"></a>Neue Verbesserungen an Azure AD Identity Protection (Public Preview)

**Typ:** Geändertes Feature  
**Dienstkategorie:** Schutz der Identität (Identity Protection)  
**Produktfunktion:** Identitätssicherheit und -schutz

Wir freuen uns, bekanntgeben zu können, dass wir dem Public Preview-Angebot von Azure AD Identity Protection die folgenden Erweiterungen hinzugefügt haben:

- Eine aktualisierte und stärker integrierte Benutzeroberfläche

- Zusätzliche APIs

- Verbesserte Risikobewertung durch Machine Learning

- Produktweite Ausrichtung bezüglich riskanter Benutzer und riskanter Anmeldungen

Weitere Informationen über die Erweiterungen finden Sie unter [Was ist Azure Active Directory Identity Protection? (aktualisiert)](https://aka.ms/IdentityProtectionDocs), um mehr zu erfahren und Ihre Meinung mittels der Eingabeaufforderungen im Produkt mitzuteilen.

---

### <a name="new-app-lock-feature-for-the-microsoft-authenticator-app-on-ios-and-android-devices"></a>Neue App-Sperrfunktion für die Microsoft Authenticator-App auf iOS- und Android-Geräten

**Typ:** Neues Feature  
**Dienstkategorie:** Microsoft Authenticator-App  
**Produktfunktion:** Identitätssicherheit und -schutz

Um die Sicherheit Ihrer Einmalpasscodes, App-Informationen und App-Einstellungen zu erhöhen, können Sie in der Microsoft Authenticator-App das Feature „App-Sperre“ aktivieren. Das Aktivieren der App-Sperre bedeutet, dass Sie bei jedem Öffnen der Microsoft Authenticator-App aufgefordert werden, sich mit Ihrer PIN oder Ihren biometrischen Daten zu authentifizieren.

Weitere Informationen finden Sie unter [Microsoft Authenticator-App – häufig gestellte Fragen](https://docs.microsoft.com/azure/active-directory/user-help/microsoft-authenticator-app-faq).

---

### <a name="enhanced-azure-ad-privileged-identity-management-pim-export-capabilities"></a>Erweiterte Exportfunktionen für Azure AD Privileged Identity Management (PIM)

**Typ:** Neues Feature  
**Dienstkategorie:** Privileged Identity Management  
**Produktfunktion:** Privileged Identity Management

Privileged Identity Management-Administratoren (PIM) können jetzt alle aktiven und geeigneten Rollenzuweisungen für eine bestimmte Ressource exportieren, die Rollenzuweisungen für alle untergeordneten Ressourcen eingeschlossen. In der Vergangenheit war es für Administratoren nicht ganz einfach, eine vollständige Liste mit den Rollenzuweisungen für ein Abonnement zu erhalten, und die Rollenzuweisungen mussten für jede spezifische Ressource exportiert werden.

Weitere Informationen finden Sie unter [Anzeigen des Aktivitäts- und Überwachungsverlaufs für Azure-Ressourcenrollen in PIM](https://docs.microsoft.com/azure/active-directory/privileged-identity-management/azure-pim-resource-rbac).

---
