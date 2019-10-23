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
ms.date: 10/15/2019
ms.author: lizross
ms.reviewer: dhanyahk
ms.custom: it-pro
ms.collection: M365-identity-device-management
ms.openlocfilehash: 096c5f06c53f1e93a0c661f856ab112236c0ec60
ms.sourcegitcommit: 0576bcb894031eb9e7ddb919e241e2e3c42f291d
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 10/15/2019
ms.locfileid: "72373542"
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

## <a name="september-2019"></a>September 2019

### <a name="deprecation-of-the-power-bi-content-packs"></a>Einstellung der Power BI-Inhaltspakete

**Typ:** Veraltet  
**Dienstkategorie:** Berichterstellung  
**Produktfunktion:** Überwachung und Berichterstellung

Power BI hat damit begonnen, Inhaltspakete aus dem Store als veraltet zu kennzeichnen, und ab dem **1. Oktober 2019** wird das Azure AD Power BI-Inhaltspaket nicht mehr unterstützt. Kunden, die derzeit das Inhaltspaket verwenden, können dieses weiterhin nutzen, auch wenn die zugrunde liegenden APIs ältere Versionen sind. Als Alternative zu diesem Inhaltspaket können Sie Azure AD-Arbeitsmappen (erstellt in Log Analytics) verwenden, um Erkenntnisse aus Ihren Azure AD-bezogenen Diensten zu gewinnen.

Weitere Informationen zu Arbeitsmappen finden Sie unter [Verwenden von Azure Monitor-Arbeitsmappen für Azure Active Directory-Berichte](https://docs.microsoft.com/azure/active-directory/reports-monitoring/howto-use-azure-monitor-workbooks). Weitere Informationen zur Einstellung der Inhaltspakete finden Sie im Blogbeitrag mit der [Ankündigung der allgemeinen Verfügbarkeit von Power BI-Vorlagen-Apps](https://powerbi.microsoft.com/blog/announcing-power-bi-template-apps-general-availability/).

### <a name="my-profile-is-re-naming-and-integrating-with-the-microsoft-office-account-page"></a>„Mein Profil“ wird umbenannt und in die Kontoseite von Microsoft Office integriert

**Typ:** Plan für Änderung  
**Dienstkategorie:** Mein Profil/Konto  
**Produktfunktion:** Zusammenarbeit

Ab Oktober wird aus der Umgebung „Mein Profil“ die Umgebung „Mein Konto“. Im Rahmen dieser Änderung werden alle Vorkommen von **Mein Profil** in **Mein Konto** geändert. Zusätzlich zu der Namensänderung und einigen Entwurfsverbesserungen ist die aktualisierte Umgebung zusätzlich in die Kontoseite von Microsoft Office integriert. Insbesondere können Sie von der Seite **Kontoübersicht** auf Office-Installationen und-Abonnements sowie von der Seite **Datenschutz** auf Office-bezogene Kontakteinstellungen zugreifen.

Weitere Informationen zur Umgebung „Mein Profil“ (Vorschau) finden Sie unter [Übersicht über das Portal „Mein Profil“ (Vorschauversion)](https://docs.microsoft.com/azure/active-directory/user-help/myprofile-portal-overview).

---

### <a name="bulk-manage-groups-and-members-using-csv-files-in-the-azure-ad-portal-public-preview"></a>Massenverwaltung von Gruppen und Mitgliedern mithilfe von CSV-Dateien im Azure AD-Portal (Öffentliche Vorschau)

**Typ:** Neues Feature  
**Dienstkategorie:** Gruppenverwaltung  
**Produktfunktion:** Zusammenarbeit

Wir freuen uns, die Verfügbarkeit der Massenverwaltung von Gruppen im Azure AD-Portal in der öffentlichen Vorschau ankündigen zu können. Sie können jetzt mithilfe einer CSV-Datei und dem Azure AD-Portal Gruppen- und Mitgliederlisten verwalten. Dazu zählen folgende Aktionen:

- Hinzufügen oder Entfernen von Mitgliedern zu/aus einer Gruppe

- Herunterladen der Gruppenliste aus dem Verzeichnis

- Herunterladen der Gruppenmitgliederliste für eine bestimmte Gruppe

Weitere Informationen finden Sie unter [Massenhinzufügen von Mitgliedern](https://docs.microsoft.com/azure/active-directory/users-groups-roles/groups-bulk-import-members), [Massenentfernen von Mitgliedern ](https://docs.microsoft.com/azure/active-directory/users-groups-roles/groups-bulk-remove-members), [Massendownload von Mitgliederlisten](https://docs.microsoft.com/azure/active-directory/users-groups-roles/groups-bulk-download-members) und [Massendownload von Gruppenlisten](https://docs.microsoft.com/azure/active-directory/users-groups-roles/groups-bulk-download).

---

### <a name="dynamic-consent-is-now-supported-through-a-new-admin-consent-endpoint"></a>Die dynamische Zustimmung wird jetzt durch einen neuen Endpunkt für die Administratorzustimmung unterstützt

**Typ:** Neues Feature  
**Dienstkategorie:** Authentifizierungen (Anmeldungen)  
**Produktfunktion:** Benutzerauthentifizierung

Wir haben zur Unterstützung der dynamischen Zustimmung einen neuen Endpunkt für die Administratorzustimmung erstellt. Dies ist hilfreich für Apps, für die das Modell der dynamischen Zustimmung auf der Microsoft Identity Platform verwendet werden soll.

Weitere Informationen zum Verwenden des neuen Endpunkts finden Sie unter [Verwenden des Endpunkts für die Administratorzustimmung](https://docs.microsoft.com/azure/active-directory/develop/v2-admin-consent).

---

### <a name="new-azure-ad-global-reader-role"></a>Neue Azure AD-Rolle „Globaler Leser“

**Typ:** Neues Feature  
**Dienstkategorie:** RBAC  
**Produktfunktion:** Zugriffssteuerung

Ab dem 24. September 2019 führen wir eine neue Azure Active Directory (AD)-Rolle namens „Globaler Leser“ ein. Diese Einführung beginnt bei Produktions- und globalen Cloud-Kunden (Global Cloud Customers, GCC) und erreicht im Oktober den weltweiten Verbreitungsgrad.

Die Rolle „Globaler Leser“ ist das Gegenstück zu „Globaler Administrator“, hat jedoch eine Nur-Lese-Berechtigung. Benutzer in dieser Rolle können in Microsoft 365-Diensten Einstellungen und administrative Informationen lesen, aber keine Verwaltungsaktionen ausführen. Wir haben die Rolle „Globaler Leser“ erstellt, um die Anzahl der globalen Administratoren in Ihrer Organisation zu reduzieren. Da Konten für globale Administratoren leistungsstark und anfällig für Angriffe sind, empfehlen wir, weniger als fünf globale Administratoren einzurichten. Die Rolle „Globaler Leser“ wird für Planungen, Audits oder Untersuchungen empfohlen. Wir empfehlen auch, die Rolle „Globaler Leser“ mit anderen eingeschränkten Administratorrollen (z.B. Exchange-Administrator) zu kombinieren, um bei der Arbeit leichter auf die Rolle „Globaler Administrator“ verzichten zu können.

Die Rolle „Globaler Leser“ funktioniert mit dem neuen Microsoft 365 Admin Center, Exchange Admin Center, Teams Admin Center, Security Center, Compliance Center, Azure AD Admin Center und Device Management Admin Center.

>[!NOTE]
> Beim Start der öffentlichen Vorschauversion funktioniert die Rolle „Globaler Leser“ mit folgenden Diensten nicht: SharePoint, Privileged Access Management, Kunden-Lockbox, Vertraulichkeitsbezeichnungen, Teams Lifecycle, Teams Reporting & Call Analytics, Teams IP Phone Device Management und Teams-App-Katalog. All diese Dienste sollen jedoch in Zukunft mit dieser Rolle arbeiten.

Weitere Informationen finden Sie unter [Berechtigungen der Administratorrolle in Azure Active Directory](https://docs.microsoft.com/azure/active-directory/users-groups-roles/directory-assign-admin-roles).

---

### <a name="access-an-on-premises-report-server-from-your-power-bi-mobile-app-using-azure-active-directory-application-proxy"></a>Zugriff auf einen lokalen Berichtsserver von Ihrer Power BI Mobile-App mit dem Azure Active Directory-Anwendungsproxy

**Typ:** Neues Feature  
**Dienstkategorie:** Anwendungsproxy  
**Produktfunktion:** Zugriffssteuerung

Die neue Integration zwischen Power BI Mobile-App und Azure AD-Anwendungsproxy ermöglicht die sichere Anmeldung bei der Power BI Mobile-App und die Anzeige aller Berichte Ihrer Organisation, die auf dem lokalen Power BI-Berichtsserver gehostet werden.

Informationen zur Power BI Mobile-App finden Sie auf der [Power BI-Website](https://powerbi.microsoft.com/mobile/) Dort erfahren Sie auch, wo Sie die App herunterladen können. Weitere Informationen zum Einrichten der Power BI Mobile-App mit dem Azure AD-Anwendungsproxy finden Sie unter [Aktivieren des Remotezugriffs auf Power BI Mobile mit dem Azure AD-Anwendungsproxy](https://docs.microsoft.com/azure/active-directory/manage-apps/application-proxy-integrate-with-power-bi).

---

### <a name="new-version-of-the-azureadpreview-powershell-module-is-available"></a>Neue Version des AzureADPreview-Moduls in PowerShell verfügbar

**Typ:** Geändertes Feature  
**Dienstkategorie:** Andere  
**Produktfunktion:** Verzeichnis

Dem AzureADPreview-Modul wurden neue Cmdlets hinzugefügt, mit denen Sie benutzerdefinierte Rollen in Azure AD definieren und zuweisen können, darunter:

- `Add-AzureADMSFeatureRolloutPolicyDirectoryObject`
- `Get-AzureADMSFeatureRolloutPolicy`
- `New-AzureADMSFeatureRolloutPolicy`
- `Remove-AzureADMSFeatureRolloutPolicy`
- `Remove-AzureADMSFeatureRolloutPolicyDirectoryObject`
- `Set-AzureADMSFeatureRolloutPolicy`

---

### <a name="new-version-of-azure-ad-connect"></a>Neue Version von Azure AD Connect

**Typ:** Geändertes Feature  
**Dienstkategorie:** Andere  
**Produktfunktion:** Verzeichnis

Für Kunden mit automatischen Upgrade haben wir eine aktualisierte Version von Azure AD Connect veröffentlicht. Diese neue Version enthält verschiedene neue Features, Verbesserungen und Fehlerbehebungen. Weitere Informationen zu dieser neuen Version finden Sie unter [Azure AD Connect: Verlauf der Versionsveröffentlichungen](https://docs.microsoft.com/azure/active-directory/hybrid/reference-connect-version-history#14250).

---

### <a name="azure-multi-factor-authentication-mfa-server-version-802-is-now-available"></a>Azure Multi-Factor Authentication (MFA) Server, Version 8.0.2, ist jetzt verfügbar

**Typ:** Korrigiert  
**Dienstkategorie:** MFA  
**Produktfunktion:** Identitätssicherheit und -schutz

Bestehende Kunden, die MFA Server vor dem 1. Juli 2019 aktiviert haben, können jetzt die aktuelle Version von MFA Server (Version 8.0.2) herunterladen. In dieser neuen Version wurden folgende Verbesserungen vorgenommen:

- Ein Problem wurde behoben, sodass der Benutzer eine E-Mail erhält, wenn bei der Azure AD-Synchronisierung ein Benutzer von „Deaktiviert“ in „Aktiviert“ geändert wird.

- Ein Problem wurde behoben, sodass Kunden erfolgreich ein Upgrade ausführen und gleichzeitig die Tags-Funktionalität weiter nutzen können.

- Der Ländercode für den Kosovo (+383) wurde hinzugefügt.

- Eine einmalige Umgehung des Audit-Trails (Audit-Protokollierung) wurde zu MultiFactorAuthSvc.log hinzugefügt.

- Die Leistung des Web Service SDKs wurde verbessert.

- Kleinere Fehler wurden behoben.

Seit dem 1. Juli 2019 bietet Microsoft MFA Server nicht mehr für neue Bereitstellungen an. Neue Kunden, die eine mehrstufige Authentifizierung benötigen, sollten die cloudbasierte Azure Multi-Factor Authentication verwenden. Weitere Informationen finden Sie unter [Planen einer cloudbasierten Azure Multi-Factor Authentication-Bereitstellung](https://docs.microsoft.com/azure/active-directory/authentication/howto-mfa-getstarted).

---

## <a name="august-2019"></a>August 2019

### <a name="plan-for-change-deprecation-of-the-power-bi-content-packs"></a>Geplante Änderung: Einstellung der Power BI-Inhaltspakete

**Typ:** Plan für Änderung  
**Dienstkategorie:** Berichterstellung  
**Produktfunktion:** Überwachung und Berichterstellung

Ab dem 1. Oktober 2019 werden in Power BI alle Inhaltspakete, einschließlich des Azure AD-Power BI-Inhaltspakets, als veraltet markiert. Als Alternative zu diesem Inhaltspaket können Sie Azure AD-Arbeitsmappen verwenden, um Erkenntnisse aus Ihren Azure AD-bezogenen Diensten zu gewinnen. Es werden noch weitere Arbeitsmappen bereitgestellt, einschließlich Arbeitsmappen zu Richtlinien für den bedingten Zugriff im reinen Berichtsmodus, auf der App-Zustimmung basierender Erkenntnisse u. v. m.

Weitere Informationen zu Arbeitsmappen finden Sie unter [Verwenden von Azure Monitor-Arbeitsmappen für Azure Active Directory-Berichte](https://docs.microsoft.com/azure/active-directory/reports-monitoring/howto-use-azure-monitor-workbooks). Weitere Informationen zur Einstellung der Inhaltspakete finden Sie im Blogbeitrag mit der [Ankündigung der allgemeinen Verfügbarkeit von Power BI-Vorlagen-Apps](https://powerbi.microsoft.com/blog/announcing-power-bi-template-apps-general-availability/).

---

### <a name="enhanced-search-filtering-and-sorting-for-groups-is-available-in-the-azure-ad-portal-public-preview"></a>Erweiterte Suche, Filterung und Sortierung für Gruppen sind im Azure AD-Portal (Public Preview) verfügbar.

**Typ:** Neues Feature  
**Dienstkategorie:** Gruppenverwaltung  
**Produktfunktion:** Zusammenarbeit

Wir freuen uns, die Verfügbarkeit der erweiterten gruppenbezogenen Erfahrungen im Azure AD-Portal in der Public Preview ankündigen zu können. Diese Verbesserungen helfen Ihnen mit den folgenden Bereitstellungen, Gruppen und Mitgliederlisten besser zu verwalten:

- Erweiterte Suchfunktionen wie z. B. Suchen von Teilzeichenfolgen in Gruppenlisten.
- Erweiterte Filter- und Sortieroptionen für Mitglieds- und Besitzerlisten.
- Neue Suchfunktionen für Mitglieds- und Besitzerlisten.
- Genauere Gruppenanzahl für große Gruppen.

Weitere Informationen finden Sie unter [Verwalten von Gruppen im Azure-Portal](https://docs.microsoft.com/azure/active-directory/fundamentals/active-directory-groups-members-azure-portal?context=azure/active-directory/users-groups-roles/context/ugr-context).

---

### <a name="new-custom-roles-are-available-for-app-registration-management-public-preview"></a>Neue benutzerdefinierte Rollen für die App-Registrierungsverwaltung (Public Preview) verfügbar

**Typ:** Neues Feature  
**Dienstkategorie:** RBAC  
**Produktfunktion:** Zugriffssteuerung

Benutzerdefinierte Rollen (die mit einem P1- oder P2-Abonnement von Azure AD verfügbar sind) bieten nun einen differenzierteren Zugriff, indem Sie Rollendefinitionen mit bestimmten Berechtigungen erstellen und diese Rollen dann bestimmten Ressourcen zuweisen können. Derzeit erstellen Sie benutzerdefinierte Rollen mithilfe von Berechtigungen zum Verwalten von App-Registrierungen und weisen diese Rolle dann einer bestimmten App zu. Weitere Informationen zu benutzerdefinierten Rollen finden Sie unter [Zuweisen von Administratorrollen in Azure Active Directory (Vorschauversion)](https://docs.microsoft.com/azure/active-directory/users-groups-roles/roles-custom-overview).

Wenn Sie Unterstützung für zusätzliche Berechtigungen oder Ressourcen benötigen, die derzeit nicht verfügbar sind, können Sie Ihr Feedback auf [unserer Azure-Feedbackwebsite](https://feedback.azure.com/forums/169401-azure-active-directory?category_id=166032) übermitteln. Wir nutzen Ihre Anforderung, um unsere Updateroadmap zu ergänzen.

---

### <a name="new-provisioning-logs-can-help-you-monitor-and-troubleshoot-your-app-provisioning-deployment-public-preview"></a>Neue Bereitstellungsprotokolle helfen Ihnen bei der Überwachung und der Problembehandlung für die App-Bereitstellung (Public Preview).

**Typ:** Neues Feature  
**Dienstkategorie:** App-Bereitstellung  
**Produktfunktion:** Identity Lifecycle Management

Mit neuen Bereitstellungsprotokollen können Sie die Bereitstellung von Benutzern und Gruppen überwachen und eventuelle Probleme beheben. Diese neuen Protokolldateien enthalten Informationen zu Folgendem:

- Welche Gruppen wurden erfolgreich in [ServiceNow](https://docs.microsoft.com/azure/active-directory/saas-apps/servicenow-provisioning-tutorial) erstellt?
- Welche Rollen wurden aus [Amazon Web Services (AWS)](https://docs.microsoft.com/azure/active-directory/saas-apps/amazon-web-service-tutorial#configure-and-test-azure-ad-single-sign-on-for-amazon-web-services-aws) importiert?
- Welche Mitarbeiter wurden nicht aus [Workday](https://docs.microsoft.com/azure/active-directory/saas-apps/workday-inbound-tutorial) importiert?

Weitere Informationen finden Sie unter [Bereitstellungsberichte im Azure Active Directory-Portal (Vorschau)](https://docs.microsoft.com/azure/active-directory/reports-monitoring/concept-provisioning-logs).

---

### <a name="new-security-reports-for-all-azure-ad-administrators-general-availability"></a>Neue Sicherheitsberichte für alle Azure AD-Administratoren (allgemeine Verfügbarkeit)

**Typ:** Neues Feature  
**Dienstkategorie:** Schutz der Identität (Identity Protection)  
**Produktfunktion:** Identitätssicherheit und -schutz

Standardmäßig können bald alle Azure AD Administratoren in Azure AD auf moderne Sicherheitsberichte zugreifen. Bis Ende September können Sie das Banner oben in den modernen Sicherheitsberichten verwenden, um zurück zu den alten Berichten zu wechseln.

Die modernen Sicherheitsberichte bieten zusätzliche Funktionen gegenüber den älteren Versionen, einschließlich der folgenden:

- Erweiterte Filterung und Sortierung
- Massenaktionen, z.B. das Verwerfen des Benutzerrisikos
- Bestätigung kompromittierter oder sicherer Entitäten
- Risikostatus, darunter: „Gefährdet“, „Verworfen“, „Bereinigt“ und „Gefährdung bestätigt“
- Neue risikobezogene Erkennungen (verfügbar für Azure AD Premium-Abonnenten)

Weitere Informationen finden Sie unter [Riskante Benutzer](https://docs.microsoft.com/azure/active-directory/identity-protection/howto-investigate-risk#risky-users-report), [Riskante Anmeldungen](https://docs.microsoft.com/azure/active-directory/identity-protection/howto-investigate-risk#risky-sign-ins-report) und [Risikoerkennungen](https://docs.microsoft.com/azure/active-directory/identity-protection/howto-investigate-risk#risk-detections-report).

---

### <a name="user-assigned-managed-identity-is-available-for-virtual-machines-and-virtual-machine-scale-sets-general-availability"></a>Benutzerseitig zugewiesene verwaltete Identität für Virtual Machines und Virtual Machine Scale Sets verfügbar

**Typ:** Neues Feature  
**Dienstkategorie:** Verwaltete Identitäten für Azure-Ressourcen  
**Produktfunktion:** Entwickleroberfläche

Benutzerseitig zugewiesene verwaltete Identitäten sind nun für Virtual Machines und Virtual Machine Scale Sets verfügbar. Dabei kann Azure eine Identität in dem Azure AD-Mandanten erstellen, der vom verwendeten Abonnement als vertrauenswürdig eingestuft wird, und einer oder mehreren Azure-Dienstinstanzen zuweisen. Weitere Informationen zu benutzerseitig zugewiesenen verwalteten Identitäten finden Sie unter [Was sind verwaltete Identitäten für Azure-Ressourcen?](https://aka.ms/azuremanagedidentity).

---

### <a name="users-can-reset-their-passwords-using-a-mobile-app-or-hardware-token-general-availability"></a>Benutzer können Ihre Kennwörter mithilfe einer mobilen App oder eines Hardwaretokens zurücksetzen (allgemeine Verfügbarkeit)

**Typ:** Geändertes Feature  
**Dienstkategorie:** Self-Service-Kennwortzurücksetzung  
**Produktfunktion:** Benutzerauthentifizierung

Benutzer, die eine mobile App in Ihrer Organisation registriert haben, können jetzt ihr eigenes Kennwort zurücksetzen, indem sie eine Benachrichtigung von der Microsoft Authenticator-App bestätigen oder einen Code aus ihrer mobilen App oder ein Hardwaretoken eingeben.

Weitere Informationen finden Sie unter [Vorgehensweise: Self-Service-Kennwortzurücksetzung in Azure AD](https://aka.ms/authappsspr). Weitere Informationen zur Benutzeroberfläche finden Sie unter [Übersicht über die Kennwortzurücksetzung für Ihr Geschäfts-, Schul- oder Unikonto](https://docs.microsoft.com/azure/active-directory/user-help/user-help-password-reset-overview).

---

### <a name="adalnet-ignores-the-msalnet-shared-cache-for-on-behalf-of-scenarios"></a>ADAL.NET ignoriert den freigegebenen MSAL.NET-Cache für „Im Auftrag von“-Szenarien

**Typ:** Korrigiert  
**Dienstkategorie:** Authentifizierungen (Anmeldungen)  
**Produktfunktion:** Benutzerauthentifizierung

Ab Azure AD Authentication Library (ADAL.NET) Version 5.0.0-preview müssen App-Entwickler [einen Cache pro Konto für Web-Apps und Web-APIs serialisieren](https://github.com/AzureAD/azure-activedirectory-library-for-dotnet/wiki/Token-cache-serialization#custom-token-cache-serialization-in-web-applications--web-api). Andernfalls kann es in einigen Szenarien mit Verwendung des [„Im Auftrag von“-Ablaufs](https://docs.microsoft.com/azure/active-directory/develop/scenario-web-api-call-api-app-configuration#on-behalf-of-flow) und für einige spezifische Anwendungsfälle von `UserAssertion` zu Rechteerweiterungen kommen. Zur Vermeidung dieses Sicherheitsrisikos wird der freigegebene Cache in Verbindung mit der Microsoft-Authentifizierungsbibliothek für .NET (MSAL.NET) für „Im Auftrag von“-Szenarien von ADAL.NET jetzt ignoriert.

Weitere Informationen zu diesem Problem finden Sie unter [Active Directory-Authentifizierungsbibliothek: Sicherheitsrisiko in Verbindung mit der Rechteerweiterung](https://portal.msrc.microsoft.com/security-guidance/advisory/CVE-2019-1258).

---

### <a name="new-federated-apps-available-in-azure-ad-app-gallery---august-2019"></a>Neue Verbund-Apps im Azure AD-App-Katalog verfügbar – August 2019

**Typ:** Neues Feature  
**Dienstkategorie:** Unternehmens-Apps  
**Produktfunktion:** Integration von Drittanbieterprodukten

Im August 2019 haben wir diese 26 neuen Apps mit Verbundunterstützung in unseren App-Katalog aufgenommen:

[Civic Platform](https://docs.microsoft.com/azure/active-directory/saas-apps/civic-platform-tutorial), [Amazon Business](https://docs.microsoft.com/azure/active-directory/saas-apps/amazon-business-tutorial), [ProNovos Ops Manager](https://docs.microsoft.com/azure/active-directory/saas-apps/pronovos-ops-manager-tutorial), [Cognidox](https://docs.microsoft.com/azure/active-directory/saas-apps/cognidox-tutorial), [Viareport's Inativ Portal (Europa)](https://docs.microsoft.com/azure/active-directory/saas-apps/viareports-inativ-portal-europe-tutorial), [Azure Databricks](https://azure.microsoft.com/services/databricks), [Robin](https://docs.microsoft.com/azure/active-directory/saas-apps/robin-tutorial), [Academy Attendance](https://docs.microsoft.com/azure/active-directory/saas-apps/academy-attendance-tutorial), [Priority Matrix](https://sync.appfluence.com/pmwebng/), [Cousto MySpace](https://cousto.platformers.be/account/login), [Uploadcare](https://uploadcare.com/accounts/signup/), [Carbonite Endpoint Backup](https://docs.microsoft.com/azure/active-directory/saas-apps/carbonite-endpoint-backup-tutorial), [CPQSync von Cincom](https://docs.microsoft.com/azure/active-directory/saas-apps/cpqsync-by-cincom-tutorial), [Chargebee](https://docs.microsoft.com/azure/active-directory/saas-apps/chargebee-tutorial), [deliver.media™ Portal](https://portal.deliver.media), [Frontline Education](https://docs.microsoft.com/azure/active-directory/saas-apps/frontline-education-tutorial), [F5](https://www.f5.com/products/security/access-policy-manager), [stashcat AD connect](https://www.stashcat.com), [Blink](https://docs.microsoft.com/azure/active-directory/saas-apps/blink-tutorial), [Vocoli](https://docs.microsoft.com/azure/active-directory/saas-apps/vocoli-tutorial), [ProNovos Analytics](https://docs.microsoft.com/azure/active-directory/saas-apps/pronovos-analytics-tutorial), [Sigstr](https://docs.microsoft.com/azure/active-directory/saas-apps/sigstr-tutorial), [Darwinbox](https://docs.microsoft.com/azure/active-directory/saas-apps/darwinbox-tutorial), [Watch by Colors](https://docs.microsoft.com/azure/active-directory/saas-apps/watch-by-colors-tutorial), [Harness](https://docs.microsoft.com/azure/active-directory/saas-apps/harness-tutorial), [EAB Navigate Strategic Care](https://docs.microsoft.com/azure/active-directory/saas-apps/eab-navigate-strategic-care-tutorial)

Weitere Informationen zu den Apps finden Sie unter [SaaS-Anwendungsintegration mit Azure Active Directory](https://aka.ms/appstutorial). Weitere Informationen zum Listen Ihrer Anwendung im Azure AD-App-Katalog finden Sie unter [Listen Ihrer Anwendung im Azure Active Directory-Anwendungskatalog](https://aka.ms/azureadapprequest).

---

### <a name="new-versions-of-the-azuread-powershell-and-azureadpreview-powershell-modules-are-available"></a>Neue Versionen der PowerShell-Module AzureAD und AzureADPreview verfügbar

**Typ:** Geändertes Feature  
**Dienstkategorie:** Andere  
**Produktfunktion:** Verzeichnis

Es sind neue Updates der PowerShell-Module AzureAD und AzureADPreview verfügbar:

- Dem `Get-AzureADDirectoryRole`-Parameter im AzureAD-Modul wurde der neue Parameter `-Filter` hinzugefügt. Mit diesem Parameter können Sie nach den Verzeichnisrollen filtern, die vom Cmdlet zurückgegeben werden.
- Dem AzureADPreview-Modul wurden neue Cmdlets hinzugefügt, mit denen Sie benutzerdefinierte Rollen in Azure AD definieren und zuweisen können, darunter:

    - `Get-AzureADMSRoleAssignment`
    - `Get-AzureADMSRoleDefinition`
    - `New-AzureADMSRoleAssignment`
    - `New-AzureADMSRoleDefinition`
    - `Remove-AzureADMSRoleAssignment`
    - `Remove-AzureADMSRoleDefinition`
    - `Set-AzureADMSRoleDefinition`

---

### <a name="improvements-to-the-ui-of-the-dynamic-group-rule-builder-in-the-azure-portal"></a>Verbesserungen an der Benutzeroberfläche des Generators für dynamische Gruppenregeln im Azure-Portal

**Typ:** Geändertes Feature  
**Dienstkategorie:** Gruppenverwaltung  
**Produktfunktion:** Zusammenarbeit

Wir haben einige Verbesserungen an der Benutzeroberfläche des Generators für dynamische Gruppenregeln vorgenommen, der im Azure-Portal verfügbar ist und Ihnen beim Einrichten einer neuen Regel oder dem Ändern vorhandener Regeln hilft. Diese Designverbesserung ermöglicht Ihnen, Regeln mit bis zu fünf Ausdrücken (anstelle von einem) zu erstellen. Wir haben außerdem die Geräteeigenschaftenliste aktualisiert und dabei veraltete Geräteeigenschaften entfernt.

Weitere Informationen finden Sie unter [Verwalten von Regeln für dynamische Mitgliedschaft](https://docs.microsoft.com/azure/active-directory/users-groups-roles/groups-update-rule).

---

### <a name="new-microsoft-graph-app-permission-available-for-use-with-access-reviews"></a>Neue Microsoft Graph-App-Berechtigung für die Verwendung mit Zugriffsüberprüfungen verfügbar

**Typ:** Geändertes Feature  
**Dienstkategorie:** Zugriffsüberprüfungen  
**Produktfunktion:** Identity Governance

Wir haben die neue Microsoft Graph-App-Berechtigung `AccessReview.ReadWrite.Membership` eingeführt, die es Apps ermöglicht, automatisch Zugriffsüberprüfungen für Gruppenmitgliedschaften und App-Zuweisungen zu erstellen und abzurufen. Diese Berechtigung kann von geplanten Aufträgen oder als Teil der Automatisierung verwendet werden, ohne dass ein angemeldeter Benutzerkontext erforderlich ist.

Weitere Informationen finden Sie im Blogbeitrag [Example how to create Azure AD access reviews using Microsoft Graph app permissions with PowerShell](https://techcommunity.microsoft.com/t5/Azure-Active-Directory/Example-how-to-create-Azure-AD-access-reviews-using-Microsoft/m-p/807241) (Beispiel zum Erstellen von Azure AD-Zugriffsüberprüfungen mithilfe von Microsoft Graph-App-Berechtigungen mit PowerShell).

---

### <a name="azure-ad-activity-logs-are-now-available-for-government-cloud-instances-in-azure-monitor"></a>Azure AD-Aktivitätsprotokolle jetzt für Instanzen der Government-Cloud in Azure Monitor verfügbar

**Typ:** Geändertes Feature  
**Dienstkategorie:** Berichterstellung  
**Produktfunktion:** Überwachung und Berichterstellung

Wir freuen uns, bekannt geben zu können, dass Azure AD-Aktivitätsprotokolle jetzt in Azure Monitor für Instanzen der Government-Cloud verfügbar sind. Sie können jetzt Azure AD-Protokolle an Ihr Speicherkonto oder einen Event Hub senden, um sie in Ihre SIEM-Tools wie [Sumologic](https://docs.microsoft.com/azure/active-directory/reports-monitoring/howto-integrate-activity-logs-with-sumologic), [Splunk](https://docs.microsoft.com/azure/active-directory/reports-monitoring/howto-integrate-activity-logs-with-splunk) oder [ArcSight](https://docs.microsoft.com/azure/active-directory/reports-monitoring/howto-integrate-activity-logs-with-arcsight) zu integrieren. 

Weitere Informationen zum Einrichten von Azure Monitor finden Sie unter [Azure AD-Aktivitätsprotokolle in Azure Monitor](https://docs.microsoft.com/azure/active-directory/reports-monitoring/concept-activity-logs-azure-monitor#cost-considerations).

---

### <a name="update-your-users-to-the-new-enhanced-security-info-experience"></a>Aktualisieren Ihrer Benutzer auf die neue, erweiterte Benutzeroberfläche für Sicherheitsinformationen

**Typ:** Geändertes Feature  
**Dienstkategorie:**  Authentifizierungen (Anmeldungen)   
**Produktfunktion:** Benutzerauthentifizierung

Am 25. September 2019 werden wir die alte, nicht erweiterte Benutzeroberfläche für Sicherheitsinformationen zum Registrieren und Verwalten von Benutzersicherheitsinformationen einstellen und nur noch die neue, [erweiterte Version](https://techcommunity.microsoft.com/t5/Azure-Active-Directory-Identity/Cool-enhancements-to-the-Azure-AD-combined-MFA-and-password/ba-p/354271) aktivieren. Dies bedeutet, dass Ihre Benutzer die alte Benutzeroberfläche nicht mehr verwenden können.

Weitere Informationen zur erweiterten Benutzeroberfläche für Sicherheitsinformationen finden Sie in unserer [Administratordokumentation](https://aka.ms/securityinfodocs) und in unserer [Benutzerdokumentation](https://aka.ms/securityinfoguide).

#### <a name="to-turn-on-this-new-experience-you-must"></a>So aktivieren Sie diese neue Funktion

1. Melden Sie sich als globaler Administrator oder Benutzeradministrator beim Azure-Portal an.

2. Navigieren Sie zu  **Azure Active Directory > Benutzereinstellungen > Einstellungen für Zugriffspanel-Vorschaufeatures verwalten**.

3. Wählen Sie im Bereich **Benutzer können Vorschaufeatures zum Registrieren und Verwalten von Sicherheitsinformationen verwenden (erweitert)** die Option **Ausgewählt** aus, und wählen Sie dann entweder eine Gruppe von Benutzern aus oder **Alle**, um dieses Feature für alle Benutzer im Mandanten zu aktivieren.

4. Wählen Sie im Bereich „Benutzer können Vorschaufeatures zum Registrieren und Verwalten von Sicherheitsinformationen verwenden (erweitert)“ die Option **Keine** aus.

5. Speichern Sie die Einstellungen.

    Nach dem Speichern der Einstellungen haben Sie keinen Zugriff mehr auf die alte Benutzeroberfläche für Sicherheitsinformationen.

>[!Important]
>Wenn Sie diese Schritte nicht vor dem 25. September 2019 ausführen, wird für Ihren Azure Active Directory-Mandanten automatisch die erweiterte Benutzeroberfläche aktiviert. Wenn Sie Fragen haben, kontaktieren Sie uns unter registrationpreview@microsoft.com.

---

### <a name="authentication-requests-using-post-logins-will-be-more-strictly-validated"></a>Authentifizierungsanforderungen mit POST-Anmeldungen strenger validiert

**Typ:** Geändertes Feature  
**Dienstkategorie:** Authentifizierungen (Anmeldungen)  
**Produktfunktion:** Standards

Ab dem 2. September 2019 werden Authentifizierungsanforderungen, die die POST-Methode verwenden, genauer anhand der HTTP-Standards überprüft. Insbesondere werden Leerzeichen und doppelte Anführungszeichen (") nicht mehr aus den Anforderungsformularwerten entfernt. Es wird nicht erwartet, dass diese Änderungen vorhandene Clients unterbrechen. Durch diese Änderungen soll sichergestellt werden, dass an Azure AD gesendete Anforderungen jedes Mal zuverlässig verarbeitet werden.

Weitere Informationen finden Sie in den [Hinweisen zu Breaking Changes in Azure AD](https://docs.microsoft.com/azure/active-directory/develop/reference-breaking-changes#post-form-semantics-will-be-enforced-more-strictly---spaces-and-quotes-will-be-ignored).

---

## <a name="july-2019"></a>Juli 2019

### <a name="plan-for-change-application-proxy-service-update-to-support-only-tls-12"></a>Geplante Änderung: Aktualisierung des Anwendungsproxydiensts zur ausschließlichen Unterstützung von TLS 1.2

**Typ:** Plan für Änderung  
**Dienstkategorie:** Anwendungsproxy  
**Produktfunktion:** Zugriffssteuerung

Zur Bereitstellung einer bestmöglichen Verschlüsselung wird der Anwendungsproxydienst-Zugriff künftig ausschließlich auf TLS 1.2-Protokolle beschränkt. Diese Einschränkung wird zunächst für Kunden eingeführt, die bereits TLS 1.2-Protokolle verwenden, deshalb hat diese Änderung keine sichtbaren Auswirkungen. Die vollständige Einstellung der Protokolle TLS 1.0 und TLS 1.1 erfolgt am 31. August 2019. Kunden, die zurzeit noch TLS 1.0 und TLS 1.1 verwenden, werden rechtzeitig benachrichtigt, um sich auf diese Änderung vorzubereiten.

Um die Verbindung mit dem Anwendungsproxydienst während der Umstellung sicherzustellen, wird empfohlen, die Aktualisierung aller Client/Server- und Browser/Server-Kombinationen auf die Verwendung von TLS 1.2 sicherzustellen. Beziehen Sie bei dieser Aktualisierung auch alle Clientsysteme ein, die von Ihren Mitarbeitern für den Zugriff auf Apps verwendet werden, die über den Anwendungsproxydienst veröffentlicht werden.

Weitere Informationen finden Sie unter [Hinzufügen einer lokalen Anwendung für den Remotezugriff über den Anwendungsproxy in Azure Active Directory](https://docs.microsoft.com/azure/active-directory/manage-apps/application-proxy-add-on-premises-application).

---

### <a name="plan-for-change-design-updates-are-coming-for-the-application-gallery"></a>Geplante Änderung: Aktualisiertes Design für den Anwendungskatalog

**Typ:** Plan für Änderung  
**Dienstkategorie:** Unternehmens-Apps  
**Produktfunktion:** SSO

Für das Design des Bereichs **Aus Katalog hinzufügen** auf dem Blatt **Anwendung hinzufügen** sind Änderungen an der Benutzeroberfläche geplant. Durch diese Änderungen können Sie leichter nach Apps suchen, die eine automatische Bereitstellung, OpenID Connect, SAML (Security Assertion Markup Language) und Kennwort-SSO (Single Sign-On) unterstützen.

---

### <a name="plan-for-change-removal-of-the-mfa-server-ip-address-from-the-office-365-ip-address"></a>Geplante Änderung: Entfernung der MFA-Server-IP-Adresse aus der Office 365-IP-Adresse

**Typ:** Plan für Änderung  
**Dienstkategorie:** MFA  
**Produktfunktion:** Identitätssicherheit und -schutz

Die MFA-Server-IP-Adresse wird aus der [Office 365-IP-Adresse und dem URL-Webdienst](https://docs.microsoft.com/office365/enterprise/office-365-ip-web-service) entfernt. Wenn Sie sich aktuell auf diese Seiten stützen, um Ihre Firewalleinstellungen zu aktualisieren, müssen Sie sicherstellen, dass auch die Liste der IP-Adressen einbezogen wird, die im Abschnitt **Anforderungen an die Azure Multi-Factor Authentication-Server-Firewall** im Artikel [Erste Schritte mit Azure Multi-Factor Authentication-Server](https://docs.microsoft.com/azure/active-directory/authentication/howto-mfaserver-deploy#azure-multi-factor-authentication-server-firewall-requirements) dokumentiert ist.

---

### <a name="app-only-tokens-now-require-the-client-app-to-exist-in-the-resource-tenant"></a>Für reine App-Token muss die Client-App ab sofort im Ressourcenmandanten vorhanden sein

**Typ:** Korrigiert  
**Dienstkategorie:** Authentifizierungen (Anmeldungen)  
**Produktfunktion:** Benutzerauthentifizierung

Am 26. Juli 2019 wurde die Bereitstellung von reinen App-Token über die [Zuweisung von Clientanmeldeinformationen](https://docs.microsoft.com/azure/active-directory/develop/v1-oauth2-client-creds-grant-flow) geändert. Bisher konnten Apps Token abrufen, um andere Apps aufzurufen – unabhängig davon, ob die Client-App sich im Mandanten befand. Dieses Verhalten wurde aktualisiert, sodass Ressourcen mit einem einzelnen Mandanten (gelegentlich auch Web-APIs genannt) nur durch Client-Apps aufgerufen werden können, die im Ressourcenmandanten enthalten sind.

Wenn Ihre App nicht im Ressourcenmandanten enthalten ist, erhalten Sie diese Fehlermeldung: `The service principal named <app_name> was not found in the tenant named <tenant_name>. This can happen if the application has not been installed by the administrator of the tenant.`. Um das Problem zu beheben, müssen Sie den Client-App-Dienstprinzipal im Mandanten erstellen, entweder über den [Endpunkt für die Administratorzustimmung](https://docs.microsoft.com/azure/active-directory/develop/v2-permissions-and-consent#using-the-admin-consent-endpoint) oder [über PowerShell](https://docs.microsoft.com/azure/active-directory/develop/howto-authenticate-service-principal-powershell). So wird sichergestellt, dass Ihr Mandant der App die Berechtigung zum Betrieb innerhalb des Mandanten erteilt hat.

Weitere Informationen finden Sie unter [Neuerungen bei der Authentifizierung](https://docs.microsoft.com/azure/active-directory/develop/reference-breaking-changes#app-only-tokens-for-single-tenant-applications-are-only-issued-if-the-client-app-exists-in-the-resource-tenant).

> [!NOTE]
> Eine vorhandene Zustimmung zwischen Client und API ist nach wie vor nicht erforderlich. Apps müssen weiterhin eigene Autorisierungsüberprüfungen durchführen.

---

### <a name="new-passwordless-sign-in-to-azure-ad-using-fido2-security-keys"></a>Neue Anmeldung ohne Kennwort bei Azure AD über FIDO2-Sicherheitsschlüssel

**Typ:** Neues Feature  
**Dienstkategorie:** Authentifizierungen (Anmeldungen)  
**Produktfunktion:** Benutzerauthentifizierung

Azure AD-Kunden können ab sofort Richtlinien zum Verwalten von FIDO2-Sicherheitsschlüsseln für die Benutzer und Gruppen in ihrer Organisation festlegen. Endbenutzer können ihre Sicherheitsschlüssel außerdem selbst registrieren, die Schlüssel bei der Arbeit auf FIDO-fähigen Geräten zur Anmeldung bei ihren Microsoft-Konten für Websites verwenden sowie sich bei ihren in Azure AD eingebundenen Windows 10-Geräten anmelden.

Weitere Informationen für Administratoren finden Sie unter [Aktivieren des kennwortlosen Anmeldens für Azure AD (Vorschauversion)](/azure/active-directory/authentication/concept-authentication-passwordless), Informationen für Endbenutzer finden Sie unter [Einrichten der Sicherheitsinformationen zur Verwendung eines Sicherheitsschlüssels (Vorschau)](https://docs.microsoft.com/azure/active-directory/user-help/security-info-setup-security-key).

---

### <a name="new-federated-apps-available-in-azure-ad-app-gallery---july-2019"></a>Neue Verbund-Apps im Azure AD-App-Katalog verfügbar – Juli 2019

**Typ:** Neues Feature  
**Dienstkategorie:** Unternehmens-Apps  
**Produktfunktion:** Integration von Drittanbieterprodukten

Im Juli 2019 haben wir diese 18 neuen Apps mit Verbundunterstützung in unseren App-Katalog aufgenommen:

[Ungerboeck Software](https://docs.microsoft.com/azure/active-directory/saas-apps/ungerboeck-software-tutorial), [Bright Pattern Omnichannel Contact Center](https://docs.microsoft.com/azure/active-directory/saas-apps/bright-pattern-omnichannel-contact-center-tutorial), [Clever Nelly](https://docs.microsoft.com/azure/active-directory/saas-apps/clever-nelly-tutorial), [AcquireIO](https://docs.microsoft.com/azure/active-directory/saas-apps/acquireio-tutorial), [Looop](https://www.looop.co/schedule-a-demo/), [productboard](https://docs.microsoft.com/azure/active-directory/saas-apps/productboard-tutorial), [MS Azure SSO Access for Ethidex Compliance Office™](https://docs.microsoft.com/azure/active-directory/manage-apps/what-is-single-sign-on#password-based-sso), [Hype](https://docs.microsoft.com/azure/active-directory/saas-apps/hype-tutorial), [Abstract](https://docs.microsoft.com/azure/active-directory/saas-apps/abstract-tutorial), [Ascentis](https://docs.microsoft.com/azure/active-directory/saas-apps/ascentis-tutorial), [Flipsnack](https://www.flipsnack.com/accounts/sign-in-sso.html), [Wandera](https://docs.microsoft.com/azure/active-directory/saas-apps/wandera-tutorial), [TwineSocial](https://twinesocial.com/), [Kallidus](https://docs.microsoft.com/azure/active-directory/saas-apps/kallidus-tutorial), [HyperAnna](https://docs.microsoft.com/azure/active-directory/saas-apps/hyperanna-tutorial), [PharmID WasteWitness](https://www.pharmid.com/), [i2B Connect](https://www.i2b-online.com/sign-up-to-use-i2b-connect-here-sso-access/), [JFrog Artifactory](https://docs.microsoft.com/azure/active-directory/saas-apps/jfrog-artifactory-tutorial)

Weitere Informationen zu den Apps finden Sie unter [SaaS-Anwendungsintegration mit Azure Active Directory](https://aka.ms/appstutorial). Weitere Informationen zum Listen Ihrer Anwendung im Azure AD-App-Katalog finden Sie unter [Listen Ihrer Anwendung im Azure Active Directory-Anwendungskatalog](https://aka.ms/azureadapprequest).

---

### <a name="automate-user-account-provisioning-for-these-newly-supported-saas-apps"></a>Automatisieren der Bereitstellung von Benutzerkonten für diese neu unterstützten SaaS-Apps

**Typ:** Neues Feature  
**Dienstkategorie:** Unternehmens-Apps  
**Produktfunktion:** Überwachung und Berichterstellung

Sie können ab sofort das Erstellen, Aktualisieren und Löschen von Benutzerkonten für diese neu integrierten Apps automatisieren:

- [Dialpad](https://docs.microsoft.com/azure/active-directory/saas-apps/dialpad-provisioning-tutorial)

- [Federated Directory](https://docs.microsoft.com/azure/active-directory/saas-apps/federated-directory-provisioning-tutorial)

- [Figma](https://docs.microsoft.com/azure/active-directory/saas-apps/figma-provisioning-tutorial)

- [Leapsome](https://docs.microsoft.com/azure/active-directory/saas-apps/leapsome-provisioning-tutorial)

- [Peakon](https://docs.microsoft.com/azure/active-directory/saas-apps/peakon-provisioning-tutorial)

- [Smartsheet](https://docs.microsoft.com/azure/active-directory/saas-apps/smartsheet-provisioning-tutorial)

Weitere Informationen dazu, wie Sie Ihre Organisation durch die automatisierte Bereitstellung von Benutzerkonten besser sichern können, finden Sie unter [Automatisieren der Bereitstellung und Bereitstellungsaufhebung von Benutzern für SaaS-Anwendungen mit Azure Active Directory](https://docs.microsoft.com/azure/active-directory/manage-apps/user-provisioning).

---

### <a name="new-azure-ad-domain-services-service-tag-for-network-security-group"></a>Neues Azure AD Domain Services-Diensttag für Netzwerksicherheitsgruppe

**Typ:** Neues Feature  
**Dienstkategorie:** Azure AD Domain Services  
**Produktfunktion:** Azure AD Domain Services

Wenn Sie nicht länger lange Listen von IP-Adressen und Adressbereichen verwalten möchten, können Sie das neue **AzureActiveDirectoryDomainServices**-Netzwerkdiensttag in Ihrer Azure-Netzwerksicherheitsgruppe verwenden, um den eingehenden Datenverkehr im Subnetz Ihres virtuellen Azure AD Domain Services-Netzwerks zu schützen.

Weitere Informationen zu diesem neuen Diensttag finden Sie unter [Netzwerkaspekte für die Azure AD Domain Services](../../active-directory-domain-services/network-considerations.md#network-security-groups-and-required-ports).

---

### <a name="new-security-audits-for-azure-ad-domain-services-public-preview"></a>Neue Sicherheitsüberwachungen für Azure AD Domain Services (öffentliche Vorschau)

**Typ:** Neues Feature  
**Dienstkategorie:** Azure AD Domain Services  
**Produktfunktion:** Azure AD Domain Services

Wir freuen uns, die Veröffentlichung der Azure AD Domain Services-Sicherheitsüberwachung in der öffentlichen Vorschau bekanntzugeben. Die Sicherheitsüberwachung bietet Ihnen wertvollen Einblick in Ihre Authentifizierungsdienste, indem Sicherheitsüberwachungsereignisse über das Azure AD Domain Service-Portal an Zielressourcen wie Azure Storage, Azure Log Analytics-Arbeitsbereiche und Azure Event Hubs gestreamt werden.

Weitere Informationen finden Sie unter [Aktivieren von Sicherheitsüberwachungen in Azure AD Domain Services (Vorschauversion)](https://docs.microsoft.com/azure/active-directory-domain-services/security-audit-events).

---

### <a name="new-authentication-methods-usage--insights-public-preview"></a>Neue Berichte „Authentifizierungsmethoden: Nutzung und Erkenntnisse“ (öffentliche Vorschau)

**Typ:** Neues Feature  
**Dienstkategorie:** Self-Service-Kennwortzurücksetzung  
**Produktfunktion:** Überwachung und Berichterstellung

Die neuen Berichte „Authentifizierungsmethoden: Nutzung und Erkenntnisse“ ermöglichen ein besseres Verständnis der Registrierung und Verwendung von Features wie Azure Multi-Factor Authentication und Self-Service-Kennwortzurücksetzung innerhalb Ihrer Organisation. Beispielsweise erfahren Sie, wie viele Benutzer für jedes Feature registriert sind, wie häufig die Self-Service-Kennwortzurücksetzung zum Zurücksetzen von Kennwörtern verwendet wird und über welche Methode die Zurücksetzung erfolgt.

Weitere Informationen finden Sie unter [Verwendung und Erkenntnisse von Authentifizierungsmethoden (Vorschau)](https://docs.microsoft.com/azure/active-directory/authentication/howto-authentication-methods-usage-insights).

---

### <a name="new-security-reports-are-available-for-all-azure-ad-administrators-public-preview"></a>Neue Sicherheitsberichte für alle Azure AD-Administratoren (öffentliche Vorschau)

**Typ:** Neues Feature  
**Dienstkategorie:** Schutz der Identität (Identity Protection)  
**Produktfunktion:** Identitätssicherheit und -schutz

Alle Azure AD-Administratoren können nun das Banner im oberen Bereich vorhandener Sicherheitsberichte – beispielsweise den Bericht **Benutzer mit Risikomarkierung** – auswählen, um die neue Sicherheitsfunktionalität zu nutzen, wie in den Berichten **Riskante Benutzer** und **Riskante Anmeldungen** gezeigt. Im weiteren Verlauf werden alle Sicherheitsberichte von den alten Versionen auf die neuen Versionen umgestellt, wobei die neuen Berichte die folgenden zusätzlichen Funktionen bieten:

- Erweiterte Filterung und Sortierung

- Massenaktionen, z.B. das Verwerfen des Benutzerrisikos

- Bestätigung kompromittierter oder sicherer Entitäten

- Risikostatus, darunter: „Gefährdet“, „Verworfen“, „Bereinigt“ und „Gefährdung bestätigt“

Weitere Informationen finden Sie unter [Bericht „Riskante Benutzer“](https://docs.microsoft.com/azure/active-directory/identity-protection/howto-investigate-risk#risky-users-report) und [Bericht über riskante Anmeldungen](https://docs.microsoft.com/azure/active-directory/identity-protection/howto-investigate-risk#risky-sign-ins-report).

---

### <a name="new-security-audits-for-azure-ad-domain-services-public-preview"></a>Neue Sicherheitsüberwachungen für Azure AD Domain Services (öffentliche Vorschau)

**Typ:** Neues Feature  
**Dienstkategorie:** Azure AD Domain Services  
**Produktfunktion:** Azure AD Domain Services

Wir freuen uns, die Veröffentlichung der Azure AD Domain Services-Sicherheitsüberwachung in der öffentlichen Vorschau bekanntzugeben. Die Sicherheitsüberwachung bietet Ihnen wertvollen Einblick in Ihre Authentifizierungsdienste, indem Sicherheitsüberwachungsereignisse über das Azure AD Domain Service-Portal an Zielressourcen wie Azure Storage, Azure Log Analytics-Arbeitsbereiche und Azure Event Hubs gestreamt werden.

Weitere Informationen finden Sie unter [Aktivieren von Sicherheitsüberwachungen in Azure AD Domain Services (Vorschauversion)](https://docs.microsoft.com/azure/active-directory-domain-services/security-audit-events).

---

### <a name="new-b2b-direct-federation-using-samlws-fed-public-preview"></a>Neuer B2B-Direktverbund über SAML/WS-Verbund (öffentliche Vorschau)

**Typ:** Neues Feature  
**Dienstkategorie:** B2B  
**Produktfunktion:** B2B/B2C

Der Direktverbund erleichtert die Zusammenarbeit mit Partnern, die eine andere, von der IT-verwaltete Identitätslösung als Azure AD verwenden. Erreicht wird dies durch die Zusammenarbeit mit Identitätssystemen, die die SAML- oder WS-Verbundstandards unterstützen. Nachdem Sie eine Direktverbundbeziehung mit einem Partner eingerichtet haben, können neu eingeladene Gastbenutzer aus dieser Domäne unter Verwendung ihrer vorhandenen Organisationskonten mit Ihnen zusammenarbeiten. Dies ermöglicht eine nahtlosere Zusammenarbeit für Ihre Gastbenutzer.

Weitere Informationen finden Sie unter [Direkter Verbund mit AD FS und Drittanbietern für Gastbenutzer (Vorschau)](https://docs.microsoft.com/azure/active-directory/b2b/direct-federation).

---

### <a name="automate-user-account-provisioning-for-these-newly-supported-saas-apps"></a>Automatisieren der Bereitstellung von Benutzerkonten für diese neu unterstützten SaaS-Apps

**Typ:** Neues Feature  
**Dienstkategorie:** Unternehmens-Apps  
**Produktfunktion:** Überwachung und Berichterstellung

Sie können ab sofort das Erstellen, Aktualisieren und Löschen von Benutzerkonten für diese neu integrierten Apps automatisieren:

- [Dialpad](https://docs.microsoft.com/azure/active-directory/saas-apps/dialpad-provisioning-tutorial)

- [Federated Directory](https://docs.microsoft.com/azure/active-directory/saas-apps/federated-directory-provisioning-tutorial)

- [Figma](https://docs.microsoft.com/azure/active-directory/saas-apps/figma-provisioning-tutorial)

- [Leapsome](https://docs.microsoft.com/azure/active-directory/saas-apps/leapsome-provisioning-tutorial)

- [Peakon](https://docs.microsoft.com/azure/active-directory/saas-apps/peakon-provisioning-tutorial)

- [Smartsheet](https://docs.microsoft.com/azure/active-directory/saas-apps/smartsheet-provisioning-tutorial)

Weitere Informationen dazu, wie Sie Ihre Organisation durch die automatisierte Bereitstellung von Benutzerkonten besser schützen können, finden Sie unter [Automatisieren der Bereitstellung und Bereitstellungsaufhebung von Benutzern für SaaS-Anwendungen mit Azure Active Directory](https://docs.microsoft.com/azure/active-directory/manage-apps/user-provisioning).

---

### <a name="new-check-for-duplicate-group-names-in-the-azure-ad-portal"></a>Neue Überprüfung auf doppelte Gruppennamen im Azure AD-Portal

**Typ:** Neues Feature  
**Dienstkategorie:** Gruppenverwaltung  
**Produktfunktion:** Zusammenarbeit

Wenn Sie einen Gruppennamen über das Azure AD-Portal erstellen oder aktualisieren, wird ab sofort eine Überprüfung durchgeführt, um festzustellen, ob Sie einen vorhandenen Gruppennamen in Ihrer Ressource duplizieren. Wenn festgestellt wird, dass der Name bereits von einer anderen Gruppe verwendet wird, werden Sie aufgefordert, den Namen zu ändern.

Weitere Informationen finden Sie unter [Verwalten von Gruppen im Azure AD-Portal](https://docs.microsoft.com/azure/active-directory/fundamentals/active-directory-groups-create-azure-portal?context=azure/active-directory/users-groups-roles/context/ugr-context).

---

### <a name="azure-ad-now-supports-static-query-parameters-in-reply-redirect-uris"></a>Azure AD unterstützt ab sofort statische Abfrageparameter in Antwort-URIs (Umleitungs-URIs)

**Typ:** Neues Feature  
**Dienstkategorie:** Authentifizierungen (Anmeldungen)  
**Produktfunktion:** Benutzerauthentifizierung

Azure AD-Apps können jetzt Antwort-URIs mit statischen Abfrageparametern (z.B. `https://contoso.com/oauth2?idp=microsoft`) für OAuth 2.0-Anforderungen registrieren und verwenden. Der statische Abfrageparameter wird, ebenso wie jeder andere Bestandteil des Antwort-URI, einem Zeichenfolgenabgleich unterzogen. Wenn keine registrierte Zeichenfolge vorhanden ist, die dem URL-decodierten Umleitungs-URI entspricht, wird die Anforderung abgelehnt. Wird der Antwort-URI gefunden, wird die gesamte Zeichenfolge – einschließlich des statischen Abfrageparameters – zum Umleiten des Benutzers verwendet.

Dynamische Antwort-URIs sind weiterhin untersagt, weil sie ein Sicherheitsrisiko darstellen und nicht dazu verwendet werden können, statische Informationen über eine Authentifizierungsanforderung hinweg beizubehalten. Verwenden Sie zu diesem Zweck den `state`-Parameter.

Aktuell werden Abfrageparameter in den Bildschirmen für die App-Registrierung im Azure-Portal weiterhin blockiert. Sie können jedoch das App-Manifest manuell bearbeiten, um Abfrageparameter in Ihre App einzufügen und diese zu testen. Weitere Informationen finden Sie unter [Neuerungen bei der Authentifizierung](https://docs.microsoft.com/azure/active-directory/develop/reference-breaking-changes#redirect-uris-can-now-contain-query-string-parameters).

---

### <a name="activity-logs-ms-graph-apis-for-azure-ad-are-now-available-through-powershell-cmdlets"></a>Aktivitätsprotokolle (MS Graph-APIs) für Azure AD jetzt über PowerShell-Cmdlets verfügbar

**Typ:** Neues Feature  
**Dienstkategorie:** Berichterstellung  
**Produktfunktion:** Überwachung und Berichterstellung

Wir freuen uns, ankündigen zu können, dass Azure AD-Aktivitätsprotokolle (Überwachungsberichte und Berichte zu Anmeldungen) jetzt über das Azure AD-PowerShell-Modul zur Verfügung stehen. Bisher konnten Sie eigene Skripts über MS Graph-API-Endpunkte erstellen. Jetzt wurde diese Funktionalität auf PowerShell-Cmdlets ausgeweitet.

Weitere Informationen zur Verwendung dieser Cmdlets finden Sie unter [Azure AD PowerShell-Cmdlets für die Berichterstellung](https://docs.microsoft.com/azure/active-directory/reports-monitoring/reference-powershell-reporting).

---

### <a name="updated-filter-controls-for-audit-and-sign-in-logs-in-azure-ad"></a>Aktualisierte Filtersteuerelemente für Überwachungs- und Anmeldeprotokolle in Azure AD

**Typ:** Geändertes Feature  
**Dienstkategorie:** Berichterstellung  
**Produktfunktion:** Überwachung und Berichterstellung

Die Berichte zu Überwachungs- und Anmeldeprotokollen wurden aktualisiert, sodass Sie nun verschiedene Filter anwenden können, ohne diese als Spalten auf den Berichtsbildschirmen hinzufügen zu müssen. Zusätzlich können Sie ab sofort entscheiden, wie viele Filter Sie auf dem Bildschirm anzeigen möchten. All diese Updates greifen ineinander, sodass Ihre Berichte besser lesbar sind und genauer an Ihre Anforderungen angepasst werden können.

Weitere Informationen zu diesen Aktualisierungen finden Sie unter [Filtern von Überwachungsprotokollen](https://docs.microsoft.com/azure/active-directory/reports-monitoring/concept-audit-logs#filtering-audit-logs) und [Filtern von Anmeldeaktivitäten](https://docs.microsoft.com/azure/active-directory/reports-monitoring/concept-sign-ins#filter-sign-in-activities).

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

Sie können ab sofort das Erstellen, Aktualisieren und Löschen von Benutzerkonten für diese neu integrierten Apps automatisieren:

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

Wir haben Azure AD so aktualisiert, dass Ihr Unternehmensbranding nun auf den Abmelde- und Fehlerbildschirmen sowie auf der Anmeldeseite angezeigt wird. Sie müssen dieses Feature nicht explizit aktivieren. Azure AD verwendet einfach die Ressourcen, die Sie bereits im Bereich **Unternehmensbranding** des Azure-Portals eingerichtet haben.

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

### <a name="new-azure-ad-threat-intelligence-detection-is-now-available-as-part-of-azure-ad-identity-protection"></a>Neue Azure AD Threat Intelligence-Erkennung ist jetzt im Rahmen von Azure AD Identity Protection verfügbar

**Typ:** Neues Feature  
**Dienstkategorie:** Azure AD Identity Protection  
**Produktfunktion:** Identitätssicherheit und -schutz

Die Azure AD Threat Intelligence-Erkennung ist jetzt im Rahmen des aktualisierten Features „Azure AD Identity Protection“ verfügbar. Diese neue Funktionalität zeigt ungewöhnliche Benutzeraktivitäten für einen bestimmten Benutzer oder Aktivitäten an, die bekannten Angriffsmustern entsprechen (basierend auf internen und externen Threat Intelligence-Quellen von Microsoft).

Weitere Informationen über die aktualisierte Version von Azure AD Identity Protection finden Sie im Blog [Four major Azure AD Identity Protection enhancements are now in public preview (Vier wichtige Verbesserungen von Azure AD Identity Protection sind jetzt in der öffentlichen Vorschau)](https://techcommunity.microsoft.com/t5/Azure-Active-Directory-Identity/Four-major-Azure-AD-Identity-Protection-enhancements-are-now-in/ba-p/326935) und im Artikel [Was ist Azure Active Directory Identity Protection (aktualisiert)?](https://docs.microsoft.com/azure/active-directory/identity-protection/overview-v2) . Weitere Informationen zur Azure AD Threat Intelligence-Erkennung finden Sie im Artikel [Referenz für Risikoerkennungen in Azure Active Directory Identity Protection](https://docs.microsoft.com/azure/active-directory/identity-protection/risk-events-reference#azure-ad-threat-intelligence).

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
