---
title: Archiv für Neuerungen in Azure Active Directory | Microsoft-Dokumentation
description: Die Versionshinweise zu Neuerungen im Abschnitt „Übersicht“ dieses Inhaltssatzes enthalten sechs Monate Aktivität. Nach 6 Monaten werden die Elemente aus den Hauptartikel entfernt und in diesem Archivartikel abgelegt.
services: active-directory
author: eross-msft
manager: daveba
ms.service: active-directory
ms.subservice: fundamentals
ms.workload: identity
ms.topic: conceptual
ms.date: 07/31/2019
ms.author: lizross
ms.reviewer: dhanyahk
ms.custom: it-pro, seo-update-azuread-jan
ms.collection: M365-identity-device-management
ms.openlocfilehash: a9aa129978201017c628da00e90190d93900d2de
ms.sourcegitcommit: 2d3740e2670ff193f3e031c1e22dcd9e072d3ad9
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 11/16/2019
ms.locfileid: "74131902"
---
# <a name="archive-for-whats-new-in-azure-active-directory"></a>Archiv für Neuerungen in Azure Active Directory

Der primäre Artikel [Neuerungen in Azure Active Directory – Anmerkungen zu dieser Version](whats-new.md) enthält die Aktualisierungen aus den letzten sechs Monaten, während dieser Artikel alle älteren Informationen enthält.

„Neuerungen in Azure Active Directory – Anmerkungen zu dieser Version“ bieten Ihnen Informationen über:

- Neueste Versionen
- Bekannte Probleme
- Fehlerbehebungen
- Veraltete Funktionen
- Pläne für Änderungen

---

## <a name="april-2019"></a>April 2019

### <a name="new-azure-ad-threat-intelligence-detection-is-now-available-as-part-of-azure-ad-identity-protection"></a>Neue Azure AD Threat Intelligence-Erkennung ist jetzt im Rahmen von Azure AD Identity Protection verfügbar

**Typ:** Neues Feature  
**Dienstkategorie:** Azure AD Identity Protection  
**Produktfunktion:** Identitätssicherheit und -schutz

Die Azure AD Threat Intelligence-Erkennung ist jetzt im Rahmen des aktualisierten Features „Azure AD Identity Protection“ verfügbar. Diese neue Funktionalität zeigt ungewöhnliche Benutzeraktivitäten für einen bestimmten Benutzer oder Aktivitäten an, die bekannten Angriffsmustern entsprechen (basierend auf internen und externen Threat Intelligence-Quellen von Microsoft).

Weitere Informationen über die aktualisierte Version von Azure AD Identity Protection finden Sie im Blog [Four major Azure AD Identity Protection enhancements are now in public preview (Vier wichtige Verbesserungen von Azure AD Identity Protection sind jetzt in der öffentlichen Vorschau)](https://techcommunity.microsoft.com/t5/Azure-Active-Directory-Identity/Four-major-Azure-AD-Identity-Protection-enhancements-are-now-in/ba-p/326935) und im Artikel [Was ist Azure Active Directory Identity Protection (aktualisiert)?](https://docs.microsoft.com/azure/active-directory/identity-protection/overview-v2) . Weitere Informationen zur Azure AD Threat Intelligence-Erkennung finden Sie im Artikel [Referenz für Risikoerkennungen in Azure Active Directory Identity Protection](https://docs.microsoft.com/azure/active-directory/identity-protection/concept-identity-protection-risks).

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

Weitere Informationen zu diesem Feature finden Sie unter [Schnellstart: Herunterladen eines Überwachungsberichts über das Azure-Portal](https://docs.microsoft.com/azure/active-directory/reports-monitoring/quickstart-download-audit-report).

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

## <a name="novemberdecember-2018"></a>November/Dezember 2018

### <a name="users-removed-from-synchronization-scope-no-longer-switch-to-cloud-only-accounts"></a>Aus dem Synchronisierungsbereich entfernte Benutzer werden nicht mehr auf reine Cloudkonten umgestellt

**Typ:** Korrigiert  
**Dienstkategorie:** Benutzerverwaltung  
**Produktfunktion:** Verzeichnis

>[!Important]
>Wir haben Ihre Verärgerung bezüglich dieser Fehlerbehebung zur Kenntnis genommen und verstanden. Aus diesem Grund haben wir die Änderung rückgängig gemacht, bis wir eine Fehlerbehebung anbieten können, die in Ihrer Organisation leichter implementiert werden kann.

Wir haben einen Fehler behoben, bei dem das DirSyncEnabled-Flag eines Benutzers fälschlicherweise in **False** geändert wurde, wenn das Objekt von Active Directory Domain Services (AD DS) aus dem Synchronisierungsbereich ausgeschlossen und dann im darauf folgenden Synchronisierungszyklus in Azure AD in den Papierkorb verschoben wurde. Wenn der Benutzer aus dem Synchronisierungsbereich ausgeschlossen und anschließend aus dem Azure AD-Papierkorb wiederhergestellt wird, bleibt das Benutzerkonto infolge dieser Korrektur wie erwartet von der lokalen AD-Instanz synchronisiert und kann nicht in der Cloud verwaltet werden, weil die Autoritätsquelle (Source of Authority, SoA) als lokale AD-Instanz erhalten bleibt.

Vor dieser Korrektur bestand ein Problem, wenn das DirSyncEnabled-Flag in „False“ geändert wurde. Es hat den falschen Eindruck vermittelt, dass diese Konten in reine Cloudobjekte umgewandelt wurden und die Konten in der Cloud verwaltet werden könnten. Die Konten behielten jedoch ihre SoA und alle synchronisierten-Eigenschaften (Schattenattribute) aus der lokalen AD-Instanz weiterhin als lokal bei. Dieser Zustand verursachte mehrere Probleme in Azure AD und anderen Cloudworkloads (wie Exchange Online), die davon ausgingen, diese Konten als von AD synchronisiert zu behandeln, die sich aber jetzt wie reine Cloudkonten verhielten.

Die einzige Möglichkeit, ein von AD synchronisiertes Konto tatsächlich in ein reines Cloudkonto umzuwandeln, besteht derzeit darin, DirSync auf Mandantenebene zu deaktivieren, wodurch ein Back-End-Vorgang zum Übertragen der SoA ausgelöst wird. Diese Art der Änderung der SoA erfordert (ist aber nicht beschränkt auf) das Bereinigen aller entsprechenden lokalen Attribute (wie LastDirSyncTime und Schattenattribute) und das Senden eines Signals an andere Cloudworkloads, das entsprechende Objekt ebenfalls in ein reines Cloudkonto umzuwandeln.

Durch diese Korrektur werden daher direkte Updates für das ImmutableID-Attribut eines von AD synchronisierten Benutzers verhindert, die in der Vergangenheit in einigen Szenarien erforderlich waren. Entwurfsbedingt soll die ImmutableID eines Objekts in Azure AD (wie der Name schon sagt) unveränderlich sein. Zur Unterstützung solcher Szenarien wurden neue Features in Azure AD Connect Health und in den Azure AD Connect-Synchronisierungsclient implementiert:

- **Umfangreiche ImmutableID-Updates für viele Benutzer in einem mehrstufigen Ansatz**
  
  Sie müssen beispielsweise eine langwierige gesamtstrukturübergreifende AD DS-Migration durchführen. Lösung: Verwenden Sie Azure AD Connect zum **Konfigurieren des Quellankers**, und kopieren Sie beim Migrieren des Benutzers die vorhandenen ImmutableID-Werte aus Azure AD in das „ms-DS-ConsistencyGuid“-Attribut der neuen Gesamtstruktur des lokalen AD DS-Benutzers. Weitere Informationen finden Sie unter [Verwendung von „ms-DS-ConsistencyGuid“ in „sourceAnchor“](/azure/active-directory/hybrid/plan-connect-design-concepts#using-ms-ds-consistencyguid-as-sourceanchor).

- **Umfangreiche ImmutableID-Updates für viele Benutzer in einem Durchgang**

  Ihnen unterläuft beispielsweise bei der Implementierung von Azure AD Connect ein Fehler, und nun müssen Sie das Attribut „SourceAnchor“ ändern. Lösung: Deaktivieren Sie DirSync auf Mandantenebene, und löschen Sie alle ungültigen ImmutableID-Werte. Weitere Informationen finden Sie unter [Deaktivieren der Verzeichnissynchronisierung für Office 365](/office365/enterprise/turn-off-directory-synchronization).

- **Erneutes Abgleichen (Zuordnen) eines lokalen Benutzers mit einem vorhandenen Benutzer in Azure AD** Beispiel: Ein in AD DS neu erstellter Benutzer generiert ein Duplikat in einem Azure AD-Konto statt es einem vorhandenen Azure AD-Konto (verwaisten Objekt) neu zuzuordnen. Lösung: Verwenden Sie Azure AD Connect Health im Azure-Portal, um den Quellanker/die ImmutableID neu zuzuordnen. Weitere Informationen finden Sie unter [Szenario: Verwaistes Objekt](/azure/active-directory/hybrid/how-to-connect-health-diagnose-sync-errors#orphaned-object-scenario).

### <a name="breaking-change-updates-to-the-audit-and-sign-in-logs-schema-through-azure-monitor"></a>Wichtige Änderung: Aktualisierungen des Überwachungs- und Anmeldeprotokollschemas über Azure Monitor

**Typ:** Geändertes Feature  
**Dienstkategorie:** Berichterstellung  
**Produktfunktion:** Überwachung und Berichterstellung

Wir veröffentlichen derzeit die Überwachungs- und Anmeldeprotokollstreams über Azure Monitor, sodass Sie die Protokolldateien nahtlos in Ihre SIEM-Tools oder in Log Analytics integrieren können. Basierend auf Ihrem Feedback und in Vorbereitung auf die Ankündigung der allgemeinen Verfügbarkeit dieses Features nehmen wir an unserem Schema die folgenden Änderungen vor. Diese Schemaänderungen und die zugehörigen Dokumentationsaktualisierungen erfolgen bis zur ersten Januarwoche.

#### <a name="new-fields-in-the-audit-schema"></a>Neue Felder im Überwachungsschema
Wir fügen ein neues Feld **Vorgangstyp** zum Angeben des für die Ressource ausgeführten Vorgangstyps hinzu. Beispiele hierfür sind **Hinzufügen**, **Aktualisieren** oder **Löschen**.

#### <a name="changed-fields-in-the-audit-schema"></a>Geänderte Felder im Überwachungsschema
Im Überwachungsschema werden die folgenden Felder geändert:

|Feldname|Änderung|Alte Werte|Neue Werte|
|----------|------------|----------|----------|
|Category|Das war das Feld **Dienstname**. Jetzt heißt das Feld **Überwachungskategorien**. **Dienstname** wurde in das Feld **loggedByService** umbenannt.|<ul><li>Kontobereitstellung</li><li>Kernverzeichnis</li><li>Self-Service-Kennwortzurücksetzung</li></ul>|<ul><li>Benutzerverwaltung</li><li>Gruppenverwaltung</li><li>App-Verwaltung</li></ul>|
|targetResources|Enthält **TargetResourceType** auf der obersten Ebene.|&nbsp;|<ul><li>Richtlinie</li><li>App</li><li>Benutzer</li><li>Group</li></ul>|
|loggedByService|Gibt den Namen des Diensts an, der das Überwachungsprotokoll generiert hat.|Null|<ul><li>Kontobereitstellung</li><li>Kernverzeichnis</li><li>Self-Service-Kennwortzurücksetzung</li></ul>|
|Ergebnis|Stellt das Ergebnis der Überwachungsprotokolle bereit. Früher wurde das als Auflistung angegeben, aber jetzt wird der tatsächliche Wert angezeigt.|<ul><li>0</li><li>1</li></ul>|<ul><li>Erfolgreich</li><li>Fehler</li></ul>|

#### <a name="changed-fields-in-the-sign-in-schema"></a>Geänderte Felder im Anmeldeschema
Im Anmeldeschema werden die folgenden Felder geändert:

|Feldname|Änderung|Alte Werte|Neue Werte|
|----------|------------|----------|----------|
|appliedConditionalAccessPolicies|Das war das Feld **conditionalaccessPolicies**. Jetzt heißt das Feld **appliedConditionalAccessPolicies**.|Keine Änderung|Keine Änderung|
|conditionalAccessStatus|Stellt das Ergebnis des Status der Richtlinie für bedingten Zugriff bei der Anmeldung bereit. Früher wurde das als Auflistung angegeben, aber jetzt wird der tatsächliche Wert angezeigt.|<ul><li>0</li><li>1</li><li>2</li><li>3</li></ul>|<ul><li>Erfolgreich</li><li>Fehler</li><li>Nicht angewendet</li><li>Deaktiviert</li></ul>|
|appliedConditionalAccessPolicies: Ergebnis|Stellt das Ergebnis des Status der einzelnen Richtlinien für bedingten Zugriff bei der Anmeldung bereit. Früher wurde das als Auflistung angegeben, aber jetzt wird der tatsächliche Wert angezeigt.|<ul><li>0</li><li>1</li><li>2</li><li>3</li></ul>|<ul><li>Erfolgreich</li><li>Fehler</li><li>Nicht angewendet</li><li>Deaktiviert</li></ul>|

Weitere Informationen zum Schema finden Sie unter [Interpretieren des Azure AD-Überwachungsprotokollschemas in Azure Monitor (Vorschauversion)](https://docs.microsoft.com/azure/active-directory/reports-monitoring/reference-azure-monitor-audit-log-schema).

---

### <a name="identity-protection-improvements-to-the-supervised-machine-learning-model-and-the-risk-score-engine"></a>Identity Protection-Verbesserungen am überwachten Machine Learning-Modell und am Risikobewertungsmodul

**Typ:** Geändertes Feature  
**Dienstkategorie:** Schutz der Identität (Identity Protection)  
**Produktfunktion:** Risikobewertungen

Verbesserungen am Identity Protection-bezogenen Benutzer- und Anmelderisikobewertungsmodul können zu einer besseren Genauigkeit und Abdeckung des Benutzerrisikos beitragen. Administratoren werden möglicherweise feststellen, dass die Benutzerrisikostufe nicht mehr direkt mit der Risikostufe bestimmter Erkennungen verknüpft ist und sich die Anzahl und Stufe von riskanten Anmeldeereignissen erhöht hat.

Risikoerkennungen werden jetzt vom überwachten Machine Learning-Modell ausgewertet, das das Benutzerrisiko mit zusätzlichen Features der Anmeldungen des Benutzers und einem Muster von Erkennungen berechnet. Auf Grundlage dieses Modells werden dem Administrator möglicherweise Benutzer mit hohen Risikobewertungen angezeigt, obwohl die mit diesem Benutzer verknüpften Erkennungen ein geringes oder mittleres Risiko darstellen. 

---

### <a name="administrators-can-reset-their-own-password-using-the-microsoft-authenticator-app-public-preview"></a>Administratoren können ihr eigenes Kennwort mithilfe der Microsoft Authenticator-App zurücksetzen (öffentliche Vorschau)

**Typ:** Geändertes Feature  
**Dienstkategorie:** Self-Service-Kennwortzurücksetzung  
**Produktfunktion:** Benutzerauthentifizierung

Azure AD-Administratoren können jetzt ihr eigenes Kennwort mithilfe von Benachrichtigungen der Microsoft Authenticator-App oder mit Code aus einer mobilen Authentifikator-App oder einem Hardwaretoken zurücksetzen. Administratoren können ihr eigenes Kennwort jetzt mit zwei der folgenden Methoden zurücksetzen:

- Benachrichtigung der Microsoft Authenticator-App

- Code aus einer anderen mobilen Authentifikator-App/aus einem Hardwaretoken

- Email

- Telefonanruf

- Textnachricht

Weitere Informationen zur Verwendung der Microsoft Authenticator-App zum Zurücksetzen von Kennwörtern finden Sie unter [Funktionsweise der Self-Service-Kennwortzurücksetzung in Azure AD – mobile App und SSPR (Vorschau)](https://docs.microsoft.com/azure/active-directory/authentication/concept-sspr-howitworks#mobile-app-and-sspr).

---

### <a name="new-azure-ad-cloud-device-administrator-role-public-preview"></a>Neue Azure AD-Rolle „Cloudgeräteadministrator“ (öffentliche Vorschau)

**Typ:** Neues Feature  
**Dienstkategorie:** Geräteregistrierung und -verwaltung  
**Produktfunktion:** Zugriffssteuerung

Administratoren können Benutzern die neue Rolle „Cloudgeräteadministrator“ zuweisen, damit sie Aufgaben eines Cloudgeräteadministrators ausführen können. Benutzer, denen die Rolle „Cloudgeräteadministrator“ zugewiesen ist, können Geräte in Azure AD aktivieren, deaktivieren und löschen sowie Windows 10-BitLocker-Schlüssel (falls vorhanden) im Azure-Portal lesen.

Weitere Informationen zu Rollen und Berechtigungen finden Sie unter [Zuweisen von Administratorrollen in Azure Active Directory](https://docs.microsoft.com/azure/active-directory/users-groups-roles/directory-assign-admin-roles).

---

### <a name="manage-your-devices-using-the-new-activity-timestamp-in-azure-ad-public-preview"></a>Verwalten Ihrer Geräte mit dem neuen Aktivitätszeitstempel in Azure AD (öffentliche Vorschau)

**Typ:** Neues Feature  
**Dienstkategorie:** Geräteregistrierung und -verwaltung  
**Produktfunktion:** Lebenszyklusverwaltung für Geräte

Wir wissen, dass Sie im Laufe der Zeit die Geräte Ihrer Organisation in Azure AD aktualisieren und außer Betrieb nehmen müssen, um zu vermeiden, dass Ihre Umgebung veraltete Geräte enthält. Um Sie bei diesem Prozess zu unterstützen, werden Ihre Geräte in Azure AD jetzt mit einem neuen Aktivitätszeitstempel aktualisiert, sodass Sie den Gerätelebenszyklus einfacher verwalten können.

Weitere Informationen zum Abrufen und Verwenden dieses Zeitstempels finden Sie unter [Anleitung: Verwalten der veralteten Geräte in Azure AD](https://docs.microsoft.com/azure/active-directory/devices/manage-stale-devices).

---

### <a name="administrators-can-require-users-to-accept-a-terms-of-use-on-each-device"></a>Administratoren können erzwingen, dass Benutzer den Nutzungsbedingungen auf jedem Gerät zustimmen müssen

**Typ:** Neues Feature  
**Dienstkategorie:** Nutzungsbedingungen  
**Produktfunktion:** Governance
 
Administratoren können jetzt die Option **Zustimmung der Benutzer auf jedem Gerät erforderlich** aktivieren, damit Ihre Benutzer Ihren Nutzungsbedingungen auf jedem Gerät zustimmen müssen, das sie in Ihrem Mandanten verwenden.

Weitere Informationen finden Sie unter [Nutzungsbedingungsfeature für Azure Active Directory im Abschnitt „Nutzungsbedingungen pro Gerät“](https://docs.microsoft.com/azure/active-directory/conditional-access/terms-of-use#per-device-terms-of-use).

---

### <a name="administrators-can-configure-a-terms-of-use-to-expire-based-on-a-recurring-schedule"></a>Administratoren können festlegen, dass die Einwilligungen für Nutzungsbedingungen basierend auf einer Zeitplanserie ablaufen

**Typ:** Neues Feature  
**Dienstkategorie:** Nutzungsbedingungen  
**Produktfunktion:** Governance
 

Administratoren können jetzt die Option **Ablauf für Einwilligungen** aktivieren, damit die Einwilligungen für Nutzungsbedingungen von allen Benutzern basierend auf der festgelegten Zeitplanserie ablaufen. Es kann ein jährlicher, halbjährlicher, vierteljährlicher oder monatlicher Zeitplan ausgewählt werden. Nach Ablauf der Nutzungsbedingungen müssen die Benutzer erneut zustimmen.

Weitere Informationen finden Sie unter [Nutzungsbedingungsfeature für Azure Active Directory im Abschnitt „Hinzufügen von Nutzungsbedingungen“](https://docs.microsoft.com/azure/active-directory/conditional-access/terms-of-use#add-terms-of-use).

---

### <a name="administrators-can-configure-a-terms-of-use-to-expire-based-on-each-users-schedule"></a>Administratoren können festlegen, dass die Einwilligungen für Nutzungsbedingungen basierend auf dem Zeitplan der einzelnen Benutzer ablaufen

**Typ:** Neues Feature  
**Dienstkategorie:** Nutzungsbedingungen  
**Produktfunktion:** Governance

Administratoren können jetzt einen Zeitraum angeben, in dem die Benutzer den Nutzungsbedingungen erneut zustimmen müssen. Administratoren können beispielsweise festlegen, dass die Benutzer den Nutzungsbedingungen alle 90 Tage erneut zustimmen müssen.

Weitere Informationen finden Sie unter [Nutzungsbedingungsfeature für Azure Active Directory im Abschnitt „Hinzufügen von Nutzungsbedingungen“](https://docs.microsoft.com/azure/active-directory/conditional-access/terms-of-use#add-terms-of-use).
 
---

### <a name="new-azure-ad-privileged-identity-management-pim-emails-for-azure-active-directory-roles"></a>Neue E-Mails von Azure AD Privileged Identity Management (PIM) für Azure Active Directory-Rollen

**Typ:** Neues Feature  
**Dienstkategorie:** Privileged Identity Management  
**Produktfunktion:** Privileged Identity Management
 
Kunden, die Azure AD Privileged Identity Management (PIM) verwenden, können jetzt eine wöchentliche Zusammenfassung per E-Mail mit den folgenden Informationen der letzten sieben Tage erhalten:

- Übersicht über die wichtigsten dauerhaften Zuweisungen berechtigter Rollen

- Anzahl von Benutzern, die Rollen aktivieren

- Anzahl von Benutzern, denen Rollen in PIM zugewiesen sind

- Anzahl von Benutzern, denen Rollen außerhalb von PIM zugewiesen sind

- Anzahl von Benutzern, denen eine Rolle in PIM dauerhaft zugewiesen wurde

Weitere Informationen zu PIM und den verfügbaren E-Mail-Benachrichtigungen finden Sie unter [E-Mail-Benachrichtigungen in PIM](https://docs.microsoft.com/azure/active-directory/privileged-identity-management/pim-email-notifications).

---

### <a name="group-based-licensing-is-now-generally-available"></a>Die gruppenbasierte Lizenzierung ist jetzt allgemein verfügbar

**Typ:** Geändertes Feature  
**Dienstkategorie:** Andere  
**Produktfunktion:** Verzeichnis

Die gruppenbasierte Lizenzierung befindet sich nicht mehr in der öffentlichen Vorschauphase und ist jetzt allgemein verfügbar. Im Rahmen dieser allgemeinen Version haben wir dieses Feature skalierbarer gestaltet und Optionen hinzugefügt, mit denen Sie Zuweisungen der gruppenbasierten Lizenzierung für einen einzelnen Benutzer erneut verarbeiten und die gruppenbasierte Lizenzierung für Office 365 E3-/A3-Lizenzen verwenden können.

Weitere Informationen zur gruppenbasierten Lizenzierung finden Sie unter [Was ist die gruppenbasierte Lizenzierung in Azure Active Directory?](https://docs.microsoft.com/azure/active-directory/fundamentals/active-directory-licensing-whatis-azure-portal)

---

### <a name="new-federated-apps-available-in-azure-ad-app-gallery---november-2018"></a>Neue Verbund-Apps im Azure AD-App-Katalog verfügbar – November 2018

**Typ:** Neues Feature  
**Dienstkategorie:** Unternehmens-Apps  
**Produktfunktion:** Integration von Drittanbieterprodukten
 
Im November 2018 haben wir diese 26 neuen Apps mit Verbundunterstützung in unseren App-Katalog aufgenommen:

[CoreStack](https://cloud.corestack.io/site/login), [HubSpot](https://docs.microsoft.com/azure/active-directory/saas-apps/HubSpot-tutorial), [GetThere](https://docs.microsoft.com/azure/active-directory/saas-apps/getthere-tutorial), [Gra-Pe](https://docs.microsoft.com/azure/active-directory/saas-apps/grape-tutorial), [eHour](https://getehour.com/try-now), [Consent2Go](https://docs.microsoft.com/azure/active-directory/saas-apps/Consent2Go-tutorial), [Appinux](https://docs.microsoft.com/azure/active-directory/saas-apps/appinux-tutorial), [DriveDollar](https://azuremarketplace.microsoft.com/marketplace/apps/savitas.drivedollar-azuread?tab=Overview), [Useall](https://docs.microsoft.com/azure/active-directory/saas-apps/useall-tutorial), [Infinite Campus](https://docs.microsoft.com/azure/active-directory/saas-apps/infinitecampus-tutorial), [Alaya](https://alayagood.com/en/demo/), [HeyBuddy](https://docs.microsoft.com/azure/active-directory/saas-apps/heybuddy-tutorial), [Wrike SAML](https://docs.microsoft.com/azure/active-directory/saas-apps/wrike-tutorial), [Drift](https://docs.microsoft.com/azure/active-directory/saas-apps/drift-tutorial), [Zenegy for Business Central 365](https://accounting.zenegy.com/), [Everbridge Member Portal](https://docs.microsoft.com/azure/active-directory/saas-apps/everbridge-tutorial), [IDEO](https://profile.ideo.com/users/sign_up), [Ivanti Service Manager (ISM)](https://docs.microsoft.com/azure/active-directory/saas-apps/ivanti-service-manager-tutorial), [Peakon](https://docs.microsoft.com/azure/active-directory/saas-apps/peakon-tutorial), [Allbound SSO](https://docs.microsoft.com/azure/active-directory/saas-apps/allbound-sso-tutorial), [Plex Apps – Classic Test](https://test.plexonline.com/signon), [Plex Apps – Classic](https://www.plexonline.com/signon), [Plex Apps – UX Test](https://test.cloud.plex.com/sso), [Plex Apps – UX](https://cloud.plex.com/sso), [Plex Apps – IAM](https://accounts.plex.com/), [CRAFTS – Childcare Records, Attendance, & Financial Tracking System](https://getcrafts.ca/craftsregistration) 

Weitere Informationen zu den Apps finden Sie unter [SaaS-Anwendungsintegration mit Azure Active Directory](https://aka.ms/appstutorial). Weitere Informationen zum Listen Ihrer Anwendung im Azure AD-App-Katalog finden Sie unter [Listen Ihrer Anwendung im Azure Active Directory-Anwendungskatalog](https://aka.ms/azureadapprequest).

---

## <a name="october-2018"></a>Oktober 2018

### <a name="azure-ad-logs-now-work-with-azure-log-analytics-public-preview"></a>Azure AD-Protokolle funktionieren jetzt mit Azure Log Analytics (öffentliche Vorschau)

**Typ:** Neues Feature  
**Dienstkategorie:** Berichterstellung  
**Produktfunktion:** Überwachung und Berichterstellung

Wir freuen uns, Ihnen mitteilen zu können, dass Sie Ihre Azure AD-Protokolle jetzt an Azure Log Analytics weiterleiten können. Dieses Feature – das auf der Wunschliste unserer Kunden ganz oben stand – ermöglicht einen noch besseren Zugang zu Analysefunktionen für Business, Betrieb und Sicherheit und bietet eine Methode für die Überwachung Ihrer Infrastruktur. Weitere Informationen finden Sie im Blog [Azure Active Directory Activity logs in Azure Log Analytics now available](https://techcommunity.microsoft.com/t5/Azure-Active-Directory-Identity/Azure-Active-Directory-Activity-logs-in-Azure-Log-Analytics-now/ba-p/274843) (Azure Active Directory-Aktivitätsprotokolle jetzt in Azure Log Analytics verfügbar).

---

### <a name="new-federated-apps-available-in-azure-ad-app-gallery---october-2018"></a>Neue Verbund-Apps im Azure AD-App-Katalog verfügbar – Oktober 2018

**Typ:** Neues Feature  
**Dienstkategorie:** Unternehmens-Apps  
**Produktfunktion:** Integration von Drittanbieterprodukten

Im Oktober 2018 haben wir diese 14 neuen Apps mit Verbundunterstützung in unseren App-Katalog aufgenommen:

[My Award Points](https://docs.microsoft.com/azure/active-directory/saas-apps/myawardpoints-tutorial), [Vibe HCM](https://docs.microsoft.com/azure/active-directory/saas-apps/vibehcm-tutorial), ambyint, [MyWorkDrive](https://docs.microsoft.com/azure/active-directory/saas-apps/myworkdrive-tutorial), [BorrowBox](https://docs.microsoft.com/azure/active-directory/saas-apps/borrowbox-tutorial), Dialpad, [ON24 Virtual Environment](https://docs.microsoft.com/azure/active-directory/saas-apps/on24-tutorial), [RingCentral](https://docs.microsoft.com/azure/active-directory/saas-apps/ringcentral-tutorial), [Zscaler Three](https://docs.microsoft.com/azure/active-directory/saas-apps/zscaler-three-tutorial), [Phraseanet](https://docs.microsoft.com/azure/active-directory/saas-apps/phraseanet-tutorial), [Appraisd](https://docs.microsoft.com/azure/active-directory/saas-apps/appraisd-tutorial), [Workspot Control](https://docs.microsoft.com/azure/active-directory/saas-apps/workspotcontrol-tutorial), [Shuccho Navi](https://docs.microsoft.com/azure/active-directory/saas-apps/shucchonavi-tutorial), [Glassfrog](https://docs.microsoft.com/azure/active-directory/saas-apps/glassfrog-tutorial)

Weitere Informationen zu den Apps finden Sie unter [SaaS-Anwendungsintegration mit Azure Active Directory](https://aka.ms/appstutorial). Weitere Informationen zum Listen Ihrer Anwendung im Azure AD-App-Katalog finden Sie unter [Listen Ihrer Anwendung im Azure Active Directory-Anwendungskatalog](https://aka.ms/azureadapprequest).

---

### <a name="azure-ad-domain-services-email-notifications"></a>Azure AD Domain Services – E-Mail-Benachrichtigungen

**Typ:** Neues Feature  
**Dienstkategorie:** Azure AD Domain Services  
**Produktfunktion:** Azure AD Domain Services

Azure AD Domain Services bietet Warnungen im Azure-Portal zu fehlerhaften Konfigurationen oder Problemen in Ihrer verwalteten Domäne. Diese Warnungen umfassen Schrittanleitungen, sodass Sie die Probleme beheben können, ohne sich an den Support wenden zu müssen.

Ab Oktober können Sie die Benachrichtigungseinstellungen für Ihre verwaltete Domäne anpassen, sodass beim Auftreten neuer Warnungen eine E-Mail an eine angegebene Gruppe von Personen gesendet wird. So müssen Sie nicht mehr dauernd das Portal auf Updates überprüfen.

Weitere Informationen finden Sie unter [Benachrichtigungseinstellungen in Azure AD Domain Services](https://docs.microsoft.com/azure/active-directory-domain-services/active-directory-ds-notifications).

---

### <a name="azure-ad-portal-supports-using-the-forcedelete-domain-api-to-delete-custom-domains"></a>Das Azure AD-Portal unterstützt die Verwendung der ForceDelete-Domänen-API zum Löschen von benutzerdefinierten Domänen 

**Typ:** Geändertes Feature  
**Dienstkategorie:** Verzeichnisverwaltung  
**Produktfunktion:** Verzeichnis

Wir freuen uns, Ihnen mitteilen zu können, dass Sie jetzt die ForceDelete-Domänen-API verwenden können, um Ihre benutzerdefinierten Domänennamen zu löschen. Über die API werden Verweise wie Benutzer, Gruppen und Apps mit Ihrem benutzerdefinierten Domänennamen (contoso.com) asynchron wieder in den standardmäßigen Domänennamen (contoso.onmicrosoft.com) umbenannt.

Dank dieser Änderung können Sie Ihren benutzerdefinierten Domänennamen schneller löschen, wenn dieser von Ihrem Unternehmen nicht mehr verwendet wird oder wenn Sie den Domänennamen für eine andere Azure AD-Instanz verwenden möchten.

Weitere Informationen finden Sie unter [Löschen eines benutzerdefinierten Domänennamens](https://docs.microsoft.com/azure/active-directory/users-groups-roles/domains-manage#delete-a-custom-domain-name).

---

## <a name="september-2018"></a>September 2018
 
### <a name="updated-administrator-role-permissions-for-dynamic-groups"></a>Aktualisierte Berechtigungen der Administratorrolle für dynamische Gruppen

**Typ:** Korrigiert  
**Dienstkategorie:** Gruppenverwaltung  
**Produktfunktion:** Zusammenarbeit

Wir haben ein Problem behoben, damit mit bestimmten Administratorrollen jetzt dynamische Mitgliedschaftsregeln erstellt und aktualisiert werden können, ohne dass der Benutzer der Besitzer der Gruppe sein müssen.

Die Rollen lauten:

- Globaler Administrator

- Intune-Administrator

- Benutzeradministrator

Weitere Informationen finden Sie unter [Erstellen einer dynamischen Gruppe und Überprüfen des Status](https://docs.microsoft.com/azure/active-directory/users-groups-roles/groups-create-rule).

---

### <a name="simplified-single-sign-on-sso-configuration-settings-for-some-third-party-apps"></a>Vereinfachte Konfigurationseinstellungen für einmaliges Anmelden (SSO) bei einigen Drittanbieter-Apps

**Typ:** Neues Feature  
**Dienstkategorie:** Unternehmens-Apps  
**Produktfunktion:** SSO

Uns ist bewusst, dass die Einrichtung des einmaligen Anmeldens (SSO) für SaaS-Apps (Software-as-a-Service) aufgrund der speziellen Konfiguration von Apps eine Herausforderung darstellen kann. Wir haben eine vereinfachte Benutzeroberfläche für die Konfiguration erstellt, um die SSO-Konfigurationseinstellungen für die folgenden Drittanbieter-SaaS-Apps automatisch einzufügen:

- Zendesk

- ArcGis Online

- Jamf Pro

Sie können diese 1-Klick-Oberfläche verwenden, indem Sie für die App zu **Azure-Portal** > **SSO-Konfiguration** navigieren. Weitere Informationen finden Sie unter [SaaS-Anwendungsintegration mit Azure Active Directory](https://docs.microsoft.com/azure/active-directory/saas-apps/tutorial-list).

---

### <a name="azure-active-directory---where-is-your-data-located-page"></a>Seite „Azure Active Directory – An welchem Ort befinden sich Ihre Daten?“

**Typ:** Neues Feature  
**Dienstkategorie:** Andere  
**Produktfunktion:** GoLocal

Wählen Sie auf der Seite **Azure Active Directory – An welchem Ort befinden sich Ihre Daten?** die Region Ihres Unternehmens aus, um anzuzeigen, in welchem Azure-Rechenzentrum sich Ihre ruhenden Azure AD-Daten für alle Azure AD-Dienste befinden. Sie können die Informationen nach bestimmten Azure AD-Diensten für die Region Ihres Unternehmens filtern.

Informationen zum Zugriff auf dieses Feature und weitere Informationen finden Sie unter [Azure Active Directory – An welchem Ort befinden sich Ihre Daten?](https://aka.ms/AADDataMap).

---

### <a name="new-deployment-plan-available-for-the-my-apps-access-panel"></a>Neuer Bereitstellungsplan verfügbar für Zugriffsbereich „Meine Apps“

**Typ:** Neues Feature  
**Dienstkategorie:** Meine Apps  
**Produktfunktion:** SSO

Erkunden Sie den neuen Bereitstellungsplan, der für den Zugriffsbereich „Meine Apps“ verfügbar ist (https://aka.ms/deploymentplans).
Im Zugriffsbereich „Meine Apps“ können Benutzer an einem zentralen Ort nach ihren Apps suchen und darauf zugreifen. In diesem Portal können Benutzer auch Self-Service-Funktionen verwenden, z.B. das Anfordern des Zugriffs auf Apps und Gruppen oder das Verwalten des Zugriffs auf diese Ressourcen im Namen von anderen Personen.

Weitere Informationen finden Sie unter [Was ist das MyApps-Portal?](https://docs.microsoft.com/azure/active-directory/user-help/active-directory-saas-access-panel-introduction).

---

### <a name="new-troubleshooting-and-support-tab-on-the-sign-ins-logs-page-of-the-azure-portal"></a>Neue Registerkarte „Problembehandlung und Support“ auf der Seite „Anmeldungen“ im Azure-Portal

**Typ:** Neues Feature  
**Dienstkategorie:** Berichterstellung  
**Produktfunktion:** Überwachung und Berichterstellung

Die neue Registerkarte **Problembehandlung und Support** auf der Seite **Anmeldungen** des Azure-Portals dient Administratoren und Engineers als Hilfe beim Behandeln von Problemen mit der Azure AD-Anmeldung. Diese neue Registerkarte enthält den Fehlercode, die Fehlermeldung und Empfehlungen zur Problemlösung (falls zutreffend), um die Behebung des Problems zu ermöglichen. Wenn das Problem nicht lösbar ist, können Sie auch eine neue Möglichkeit zum Erstellen eines Supporttickets nutzen, indem Sie die Option **In Zwischenablage kopieren** verwenden. Bei dieser Option werden die Felder **Anforderungs-ID** und **Datum (UTC)** für die Protokolldatei in Ihrem Supportticket ausgefüllt.  

![Anmeldeprotokolle mit der neuen Registerkarte](media/whats-new/troubleshooting-and-support.png)

---

### <a name="enhanced-support-for-custom-extension-properties-used-to-create-dynamic-membership-rules"></a>Verbesserte Unterstützung für benutzerdefinierte Erweiterungseigenschaften, die zum Erstellen von dynamischen Mitgliedschaftsregeln verwendet werden

**Typ:** Geändertes Feature  
**Dienstkategorie:** Gruppenverwaltung  
**Produktfunktion:** Zusammenarbeit

Mit diesem Update können Sie jetzt im Regel-Generator für dynamische Benutzergruppen auf den Link **Get custom extension properties** (Benutzerdefinierte Erweiterungseigenschaften abrufen) klicken, Ihre eindeutige App-ID eingeben und die vollständige Liste mit benutzerdefinierten Erweiterungseigenschaften erhalten, die Sie beim Erstellen einer dynamischen Mitgliedschaftsregeln für Benutzer verwenden. Diese Liste kann auch aktualisiert werden, um neue benutzerdefinierte Erweiterungseigenschaften für diese App zu erhalten.

Weitere Informationen zur Verwendung von benutzerdefinierten Erweiterungseigenschaften für dynamische Mitgliedschaftsregeln finden Sie unter [Erweiterungseigenschaften und benutzerdefinierte Erweiterungseigenschaften](https://docs.microsoft.com/azure/active-directory/users-groups-roles/groups-dynamic-membership#extension-properties-and-custom-extension-properties).

---

### <a name="new-approved-client-apps-for-azure-ad-app-based-conditional-access"></a>Neue genehmigte Client-Apps für den app-basierten bedingten Zugriff mit Azure AD

**Typ:** Plan für Änderung  
**Dienstkategorie:** Bedingter Zugriff  
**Produktfunktion:** Identitätssicherheit und -schutz

Die folgenden Apps wurden der Liste der [genehmigten Client-Apps](https://docs.microsoft.com/azure/active-directory/conditional-access/technical-reference#approved-client-app-requirement) hinzugefügt:

- Microsoft To-Do

- Microsoft Stream

Weitere Informationen finden Sie unter

- [App-basierter bedingter Zugriff mit Azure AD](https://docs.microsoft.com/azure/active-directory/conditional-access/app-based-conditional-access)

---

### <a name="new-support-for-self-service-password-reset-from-the-windows-7881-lock-screen"></a>Neue Unterstützung für Self-Service-Kennwortzurücksetzung über den Windows 7/8/8.1-Sperrbildschirm

**Typ:** Neues Feature  
**Dienstkategorie:** SSPR  
**Produktfunktion:** Benutzerauthentifizierung

Nachdem Sie dieses neue Feature eingerichtet haben, wird Ihren Benutzern ein Link zum Zurücksetzen Ihres Kennworts über den **Sperrbildschirm** eines Geräts mit Windows 7, Windows 8 oder Windows 8.1 angezeigt. Wenn Benutzer auf diesen Link klicken, werden sie durch den gleichen Ablauf zur Kennwortzurücksetzung wie im Webbrowser geleitet.

Weitere Informationen finden Sie unter [How to enable password reset from Windows 7, 8, and 8.1](https://aka.ms/ssprforwindows78) (Aktivieren der Kennwortzurücksetzung unter Windows 7, 8 und 8.1).

---

### <a name="change-notice-authorization-codes-will-no-longer-be-available-for-reuse"></a>Änderungshinweis: Autorisierungscodes können nicht mehr wiederverwendet werden. 

**Typ:** Plan für Änderung  
**Dienstkategorie:** Authentifizierungen (Anmeldungen)  
**Produktfunktion:** Benutzerauthentifizierung

Ab dem 15. November 2018 akzeptiert Azure AD bereits zuvor verwendete Authentifizierungscodes für Apps nicht mehr. Diese Sicherheitsänderung trägt dazu bei, Azure AD an die OAuth-Spezifikation anzupassen. Sie wird auf v1- und v2-Endpunkten erzwungen.

Wenn Ihre App Autorisierungscodes zum Abrufen von Token für mehrere Ressourcen wiederverwendet, sollten Sie den Code für das Abrufen eines Aktualisierungstokens verwenden. Verwenden Sie dieses Aktualisierungstoken anschließend, um die zusätzlichen Token für andere Ressourcen abzurufen. Autorisierungscodes können nur einmal verwendet werden, Aktualisierungstoken hingegen können mehrere Male und für mehrere Ressourcen verwendet werden. Für jede App, die einen Authentifizierungscode im OAuth-Codefluss erneut verwenden möchte, wird ein Fehler vom Typ „invalid_grant“ zurückgegeben.

Informationen hierzu und zu anderen protokollbezogenen Änderungen finden Sie in der [vollständigen Liste mit den Neuerungen bei der Authentifizierung](https://docs.microsoft.com/azure/active-directory/develop/reference-breaking-changes).

---

### <a name="new-federated-apps-available-in-azure-ad-app-gallery---september-2018"></a>Neue Verbund-Apps im Azure AD-App-Katalog verfügbar – September 2018

**Typ:** Neues Feature  
**Dienstkategorie:** Unternehmens-Apps  
**Produktfunktion:** Integration von Drittanbieterprodukten
 
Im September 2018 haben wir diese 16 neuen Apps mit Verbundunterstützung in unseren App-Katalog aufgenommen:

[Uberflip](https://docs.microsoft.com/azure/active-directory/saas-apps/uberflip-tutorial), [Comeet Recruiting Software](https://docs.microsoft.com/azure/active-directory/saas-apps/comeetrecruitingsoftware-tutorial), [Workteam](https://docs.microsoft.com/azure/active-directory/saas-apps/workteam-tutorial), [ArcGIS Enterprise](https://docs.microsoft.com/azure/active-directory/saas-apps/arcgisenterprise-tutorial), [Nuclino](https://docs.microsoft.com/azure/active-directory/saas-apps/nuclino-tutorial), [JDA Cloud](https://docs.microsoft.com/azure/active-directory/saas-apps/jdacloud-tutorial), [Snowflake](https://docs.microsoft.com/azure/active-directory/saas-apps/snowflake-tutorial), NavigoCloud, [Figma](https://docs.microsoft.com/azure/active-directory/saas-apps/figma-tutorial), join.me, [ZephyrSSO](https://docs.microsoft.com/azure/active-directory/saas-apps/zephyrsso-tutorial), [Silverback](https://docs.microsoft.com/azure/active-directory/saas-apps/silverback-tutorial), Riverbed Xirrus EasyPass, [Rackspace SSO](https://docs.microsoft.com/azure/active-directory/saas-apps/rackspacesso-tutorial), Enlyft SSO for Azure, SurveyMonkey, [Convene](https://docs.microsoft.com/azure/active-directory/saas-apps/convene-tutorial), [dmarcian](https://docs.microsoft.com/azure/active-directory/saas-apps/dmarcian-tutorial)

Weitere Informationen zu den Apps finden Sie unter [SaaS-Anwendungsintegration mit Azure Active Directory](https://aka.ms/appstutorial). Weitere Informationen zum Listen Ihrer Anwendung im Azure AD-App-Katalog finden Sie unter [Listen Ihrer Anwendung im Azure Active Directory-Anwendungskatalog](https://aka.ms/azureadapprequest).

---

### <a name="support-for-additional-claims-transformations-methods"></a>Unterstützung für zusätzliche Transformationsmethoden für Ansprüche

**Typ:** Neues Feature  
**Dienstkategorie:** Unternehmens-Apps  
**Produktfunktion:** SSO

Wir haben die neuen Transformationsmethoden „ToLower()“ und „ToUpper()“ eingeführt, die über die SAML-basierte Seite **SSO-Konfiguration** auf SAML-Token angewendet werden können.

Weitere Informationen finden Sie unter [Gewusst wie: Anpassen ausgestellter Ansprüche im SAML-Token für Unternehmensanwendungen in Azure AD](https://docs.microsoft.com/azure/active-directory/develop/active-directory-saml-claims-customization).

---

### <a name="updated-saml-based-app-configuration-ui-preview"></a>Aktualisierte SAML-basierte Benutzeroberfläche für die App-Konfiguration (Vorschauversion)

**Typ:** Geändertes Feature  
**Dienstkategorie:** Unternehmens-Apps  
**Produktfunktion:** SSO

Im Rahmen unserer aktualisierten SAML-basierten Benutzeroberfläche für die App-Konfiguration erhalten Sie Folgendes:

- Eine aktualisierte Benutzeroberfläche mit Schritt-für-Schritt-Anleitung für die Konfiguration Ihrer SAML-basierten Apps

- Bessere Informationen zu fehlenden oder fehlerhaften Teilen der Konfiguration

- Möglichkeit zum Hinzufügen von mehreren E-Mail-Adressen für Benachrichtigungen zum Zertifikatablauf

- Neue Transformationsmethoden „ToLower()“ und „ToUpper()“ für Ansprüche und mehr

- Möglichkeit zum Hochladen Ihres eigenen Tokensignaturzertifikats für Ihre Unternehmens-Apps

- Möglichkeit zum Festlegen des NameID-Formats für SAML-Apps und des NameID-Werts als Verzeichniserweiterungen

Klicken Sie zum Aktivieren dieser aktualisierten Ansicht oben auf der Seite **Einmaliges Anmelden** auf den Link **Neue Benutzeroberfläche ausprobieren**. Weitere Informationen finden Sie unter [Tutorial: Konfigurieren des SAML-basierten einmaligen Anmeldens für eine Anwendung mit Azure Active Directory](https://docs.microsoft.com/azure/active-directory/manage-apps/configure-single-sign-on-portal).

---

## <a name="august-2018"></a>August 2018

### <a name="changes-to-azure-active-directory-ip-address-ranges"></a>Änderungen an Azure Active Directory-IP-Adressbereichen

**Typ:** Plan für Änderung  
**Dienstkategorie:** Andere  
**Produktfunktion:** Plattform

Es werden größere IP-Bereiche in Azure AD eingeführt. Wenn Sie Azure AD-IP-Adressbereiche für Ihre Firewalls, Router oder Netzwerksicherheitsgruppen konfiguriert haben, müssen Sie diese daher aktualisieren. Wir führen dieses Update durch, sodass Sie die Konfiguration der IP-Bereiche für die Firewalls, Router oder Netzwerksicherheitsgruppen nicht erneut ändern müssen, wenn in Azure AD neue Endpunkte hinzugefügt werden. 

Der Netzwerkdatenverkehr wird in den nächsten zwei Monaten in diese neuen Bereiche verschoben. Um einen ununterbrochenen Dienst gewährleisten zu können, müssen Sie Ihren IP-Adressen vor dem 10. September 2018 die folgenden aktualisierten Werten hinzufügen:

- 20.190.128.0/18 

- 40.126.0.0/18 

Es wird dringend empfohlen, die alten IP-Adressbereiche erst zu entfernen, nachdem Ihr gesamter Netzwerkdatenverkehr in die neuen Bereiche verschoben wurde. Aktualisierungen zu dieser Verschiebung und Informationen dazu, wann die alten Bereiche entfernt werden können, finden Sie unter [URLs und IP-Adressbereiche von Office 365](https://support.office.com/article/Office-365-URLs-and-IP-address-ranges-8548a211-3fe7-47cb-abb1-355ea5aa88a2).

---

### <a name="change-notice-authorization-codes-will-no-longer-be-available-for-reuse"></a>Änderungshinweis: Autorisierungscodes können nicht mehr wiederverwendet werden. 

**Typ:** Plan für Änderung  
**Dienstkategorie:** Authentifizierungen (Anmeldungen)  
**Produktfunktion:** Benutzerauthentifizierung

Ab dem 15. November 2018 akzeptiert Azure AD bereits zuvor verwendete Authentifizierungscodes für Apps nicht mehr. Diese Sicherheitsänderung trägt dazu bei, Azure AD an die OAuth-Spezifikation anzupassen. Sie wird auf v1- und v2-Endpunkten erzwungen.

Wenn Ihre App Autorisierungscodes zum Abrufen von Token für mehrere Ressourcen wiederverwendet, sollten Sie den Code für das Abrufen eines Aktualisierungstokens verwenden. Verwenden Sie dieses Aktualisierungstoken anschließend, um die zusätzlichen Token für andere Ressourcen abzurufen. Autorisierungscodes können nur einmal verwendet werden, Aktualisierungstoken hingegen können mehrere Male und für mehrere Ressourcen verwendet werden. Für jede App, die einen Authentifizierungscode im OAuth-Codefluss erneut verwenden möchte, wird ein Fehler vom Typ „invalid_grant“ zurückgegeben.

Informationen hierzu und zu anderen protokollbezogenen Änderungen finden Sie in der [vollständigen Liste mit den Neuerungen bei der Authentifizierung](https://docs.microsoft.com/azure/active-directory/develop/reference-breaking-changes).
 
---

### <a name="converged-security-info-management-for-self-service-password-sspr-and-multi-factor-authentication-mfa"></a>Zusammengeführte Sicherheitsinformationenverwaltung für Self-Service-Kennwortzurücksetzung (SSPR) und Multi-Factor Authentication (MFA)

**Typ:** Neues Feature  
**Dienstkategorie:** SSPR  
**Produktfunktion:** Benutzerauthentifizierung
 
Dieses neue Feature hilft Benutzern bei der Verwaltung ihrer Sicherheitsinformationen (z.B. Telefonnummer, mobile App usw.) für SSPR und MFA an einem zentralen Ort und mit einer einheitlichen Benutzeroberfläche, wofür früher zwei verschiedenen Umgebungen notwendig waren.

Diese zusammengeführte Benutzeroberfläche funktioniert auch für Personen, die entweder SSPR oder MFA verwenden. Wenn darüber hinaus Ihre Organisation die MFA- oder SSPR-Registrierung nicht erzwingt, können sich Personen trotzdem mit beliebigen Verfahren unter Verwendung von MFA- oder SSPR-Sicherheitsinformationen anmelden, sofern diese von Ihrer Organisation im Meine Apps-Portal zugelassen wurden.

Dies ist eine abonnierbare Public Preview. Administratoren können die neue Oberfläche (falls gewünscht) für eine ausgewählte Gruppe von Benutzern oder alle Benutzer in einem Mandanten aktivieren. Weitere Informationen zur zusammengeführten Benutzeroberfläche finden Sie im Blogbeitrag [Converged experience](https://cloudblogs.microsoft.com/enterprisemobility/2018/08/06/mfa-and-sspr-updates-now-in-public-preview/) (Zusammengeführte Benutzeroberfläche).

---

### <a name="new-http-only-cookies-setting-in-azure-ad-application-proxy-apps"></a>Neue Einstellung „HTTP-Only Cookies“ (Nur HTTP-Cookies) in Azure AD-Anwendungsproxy-Apps

**Typ:** Neues Feature  
**Dienstkategorie:** Anwendungsproxy  
**Produktfunktion:** Zugriffssteuerung

Für Anwendungsproxy-Apps steht die neue Einstellung **HTTP-Only Cookies** (Nur HTTP-Cookies) zur Verfügung. Diese Einstellung bietet zusätzliche Sicherheit, da in HTTP-Antwortheader für den Zugriff auf den Anwendungsproxy und für Sitzungscookies das Flag „HTTPOnly“ eingefügt wird. Dieses beendet den Zugriff auf das Cookie von einem clientseitigen Skript und verhindert damit weitere Aktionen wie das Kopieren oder Ändern des Cookies. Auch wenn dieses Flag zuvor nicht verwendet wurde, wurden Ihre Cookies immer verschlüsselt und über eine SSL-Verbindung übertragen, um sie vor unerwünschten Änderungen zu schützen.

Diese Einstellung nicht mit Apps kompatibel, die ActiveX-Steuerelemente verwenden, z.B. Remotedesktop. In diesen Fällen wird das Deaktivieren dieser Einstellung empfohlen.

Weitere Informationen zur Einstellung „HTTP-Only Cookies“ (Nur HTTP-Cookies) finden Sie unter [Veröffentlichen von Anwendungen mit Azure AD-Anwendungsproxy](https://docs.microsoft.com/azure/active-directory/manage-apps/application-proxy-publish-azure-portal).

---

### <a name="privileged-identity-management-pim-for-azure-resources-supports-management-group-resource-types"></a>Privileged Identity Management (PIM) für Azure-Ressourcen unterstützt Verwaltungsgruppen-Ressourcentypen

**Typ:** Neues Feature  
**Dienstkategorie:** Privileged Identity Management  
**Produktfunktion:** Privileged Identity Management
 
Die Einstellungen für die Just-In-Time-Aktivierung und -Zuweisung können jetzt auf Verwaltungsgruppen-Ressourcentypen angewandt werden, wie dies bereits für Abonnements, Ressourcengruppen und Ressourcen (z.B. virtuelle Computer, App Services und mehr) möglich war. Darüber hinaus kann jede Person mit einer Rolle mit Administratorzugriff auf eine Verwaltungsgruppe diese Ressource in PIM ermitteln und verwalten.

Weitere Informationen zu PIM und Azure-Ressourcen finden Sie unter [Ermitteln und Verwalten von Azure-Ressourcen mit Privileged Identity Management](https://docs.microsoft.com/azure/active-directory/privileged-identity-management/pim-resource-roles-discover-resources).
 
---

### <a name="application-access-preview-provides-faster-access-to-the-azure-ad-portal"></a>Anwendungszugriff (Vorschau) mit schnellerem Zugriff auf das Azure AD-Portal

**Typ:** Neues Feature  
**Dienstkategorie:** Privileged Identity Management  
**Produktfunktion:** Privileged Identity Management
 
Wenn Sie heute eine Rolle mithilfe von PIM aktivieren, kann die Anwendung der Berechtigungen mehr als 10 Minuten dauern. Wenn Sie den Anwendungszugriff, der sich derzeit in der Public Preview befindet, verwenden, können Administratoren direkt auf das Azure AD-Portal zugreifen, nachdem die Aktivierungsanforderung abgeschlossen ist.

Derzeit unterstützt der Anwendungszugriff nur das Azure AD-Portal und Azure-Ressourcen. Weitere Informationen zu PIM und dem Anwendungszugriff finden Sie unter [Was ist Azure AD Privileged Identity Management?](https://docs.microsoft.com/azure/active-directory/privileged-identity-management/pim-configure).
 
---

### <a name="new-federated-apps-available-in-azure-ad-app-gallery---august-2018"></a>Neue Verbund-Apps im Azure AD-App-Katalog verfügbar – August 2018

**Typ:** Neues Feature  
**Dienstkategorie:** Unternehmens-Apps  
**Produktfunktion:** Integration von Drittanbieterprodukten
 
Im August 2018 haben wir diese 16 neuen Apps mit Verbundunterstützung in unseren App-Katalog aufgenommen:

[Hornbill](https://docs.microsoft.com/azure/active-directory/saas-apps/hornbill-tutorial), [Bridgeline Unbound](https://docs.microsoft.com/azure/active-directory/saas-apps/bridgelineunbound-tutorial), [Sauce Labs - Mobile and Web Testing](https://docs.microsoft.com/azure/active-directory/saas-apps/saucelabs-mobileandwebtesting-tutorial), [Meta Networks Connector](https://docs.microsoft.com/azure/active-directory/saas-apps/metanetworksconnector-tutorial), [Way We Do](https://docs.microsoft.com/azure/active-directory/saas-apps/waywedo-tutorial), [Spotinst](https://docs.microsoft.com/azure/active-directory/saas-apps/spotinst-tutorial), [ProMaster (by Inlogik)](https://docs.microsoft.com/azure/active-directory/saas-apps/promaster-tutorial), SchoolBooking, [4me](https://docs.microsoft.com/azure/active-directory/saas-apps/4me-tutorial), [Dossier](https://docs.microsoft.com/azure/active-directory/saas-apps/DOSSIER-tutorial), [N2F - Expense reports](https://docs.microsoft.com/azure/active-directory/saas-apps/n2f-expensereports-tutorial), [Comm100 Live Chat](https://docs.microsoft.com/azure/active-directory/saas-apps/comm100livechat-tutorial), [SafeConnect](https://docs.microsoft.com/azure/active-directory/saas-apps/safeconnect-tutorial), [ZenQMS](https://docs.microsoft.com/azure/active-directory/saas-apps/zenqms-tutorial), [eLuminate](https://docs.microsoft.com/azure/active-directory/saas-apps/eluminate-tutorial), [Dovetale](https://docs.microsoft.com/azure/active-directory/saas-apps/dovetale-tutorial).

Weitere Informationen zu den Apps finden Sie unter [SaaS-Anwendungsintegration mit Azure Active Directory](https://aka.ms/appstutorial). Weitere Informationen zum Listen Ihrer Anwendung im Azure AD-App-Katalog finden Sie unter [Listen Ihrer Anwendung im Azure Active Directory-Anwendungskatalog](https://aka.ms/azureadapprequest).

---

### <a name="native-tableau-support-is-now-available-in-azure-ad-application-proxy"></a>Native Tableau-Unterstützung jetzt im Azure AD-Anwendungsproxy verfügbar

**Typ:** Geändertes Feature  
**Dienstkategorie:** Anwendungsproxy  
**Produktfunktion:** Zugriffssteuerung

Mit unserem Update von OpenID Connect auf das OAuth 2.0-Codeberechtigungsprotokoll für unser Vorauthentifizierungsprotokoll müssen Sie keine zusätzliche Konfiguration zum Verwenden von Tableau mit dem Anwendungsproxy mehr vornehmen. Diese Protokolländerung hilft auch dem Anwendungsproxy bei der besseren Unterstützung moderner Apps mit Nur-HTTP-Umleitungen, die häufig in JavaScript- und HTML-Tags unterstützt werden.

Weitere Informationen zu unserer nativen Unterstützung für Tableau finden Sie unter [Azure AD Application Proxy now with native Tableau support](https://blogs.technet.microsoft.com/applicationproxyblog/2018/08/14/azure-ad-application-proxy-now-with-native-tableau-support) (Azure AD-Anwendungsproxy jetzt auch mit nativer Unterstützung für Tableau).

---

### <a name="new-support-to-add-google-as-an-identity-provider-for-b2b-guest-users-in-azure-active-directory-preview"></a>Neu: Unterstützung für das Hinzufügen von Google als Identitätsanbieter für B2B-Gastbenutzer in Azure Active Directory (Vorschau)

**Typ:** Neues Feature  
**Dienstkategorie:** B2B  
**Produktfunktion:** B2B/B2C

Durch das Einrichten eines Verbunds mit Google in Ihrer Organisation können Sie eingeladenen Gmail-Benutzern die Anmeldung bei Ihren freigegebenen Apps und Ressourcen mit ihren bestehenden Google-Konten ermöglichen, ohne dass sie ein persönliches Microsoft-Konto (MSA) oder Azure AD-Konto erstellen müssen.

Dies ist eine abonnierbare Public Preview. Weitere Informationen zum Google-Verbund finden Sie unter [Hinzufügen von Google als Identitätsanbieter für B2B-Gastbenutzer](https://docs.microsoft.com/azure/active-directory/b2b/google-federation).

---

## <a name="july-2018"></a>Juli 2018

### <a name="improvements-to-azure-active-directory-email-notifications"></a>Verbesserungen an Azure Active Directory-E-Mail-Benachrichtigungen

**Typ:** Geändertes Feature  
**Dienstkategorie:** Andere  
**Produktfunktion:** Identity Lifecycle Management
 
Azure Active Directory-E-Mails (Azure AD) weisen nun ein aktualisiertes Layout sowie Änderungen an der E-Mail-Adresse des Absenders und dem Anzeigenamen des Empfängers auf, wenn sie über die folgenden Dienste gesendet wurden:
 
- Azure AD-Zugriffsüberprüfungen
- Azure AD Connect Health 
- Azure AD Identity Protection 
- Azure AD Privileged Identity Management
- Benachrichtigungen zu ablaufenden Zertifikaten von Unternehmen-Apps
- Benachrichtigungen zum Bereitstellungsdienst von Unternehmen-Apps
 
Die E-Mail-Benachrichtigungen werden von folgender E-Mail-Adresse und folgendem Anzeigenamen gesendet:

- E-Mail-Adresse: azure-noreply@microsoft.com
- Anzeigename: Microsoft Azure
 
Ein Beispiel für einige der neuen E-Mail-Layouts und weitere Informationen finden Sie unter [E-Mail-Benachrichtigungen in Azure AD PIM](https://go.microsoft.com/fwlink/?linkid=2005832).

---

### <a name="azure-ad-activity-logs-are-now-available-through-azure-monitor"></a>Azure AD-Aktivitätsprotokolle sind jetzt über Azure Monitor verfügbar

**Typ:** Neues Feature  
**Dienstkategorie:** Berichterstellung  
**Produktfunktion:** Überwachung und Berichterstellung

Die Azure AD-Aktivitätsprotokolle stehen jetzt in der Public Preview für Azure Monitor (die gesamte Plattform umfassender Überwachungsdienst von Azure) zur Verfügung. Azure Monitor bietet langfristige Aufbewahrung und nahtlose Integration, zusätzlich zu diesen Verbesserungen:

- Langfristige Aufbewahrung durch Routing Ihrer Protokolldateien an Ihr eigenes Azure-Speicherkonto.

- Nahtlose SIEM-Integration, ohne dass Sie benutzerdefinierte Skripts schreiben oder verwalten müssen.

- Nahtlose Integration in Ihre eigenen benutzerdefinierten Lösungen, Analysetools oder Lösungen zur Verwaltung von Incidents.

Weitere Informationen zu diesen neuen Funktionen finden Sie in unserem Blog [Azure AD activity logs in Azure Monitor diagnostics is now in public preview (Azure AD-Aktivitätsprotokolle in der Azure Monitor-Diagnose stehen jetzt in der Public Preview zur Verfügung)](https://cloudblogs.microsoft.com/enterprisemobility/2018/07/26/azure-ad-activity-logs-in-azure-monitor-diagnostics-now-in-public-preview/) und unserer Dokumentation [Azure AD-Aktivitätsprotokolle in Azure Monitor (Vorschauversion)](https://docs.microsoft.com/azure/active-directory/reporting-azure-monitor-diagnostics-overview).

---

### <a name="conditional-access-information-added-to-the-azure-ad-sign-ins-report"></a>Informationen für den bedingten Zugriff werden dem Azure AD-Anmeldebericht hinzugefügt

**Typ:** Neues Feature  
**Dienstkategorie:** Berichterstellung  
**Produktfunktion:** Identitätssicherheit und -schutz
 
Dieses Update zeigt Ihnen, welche Richtlinien ausgewertet werden, wenn ein Benutzer sich mit dem Ergebnis der Richtlinie anmeldet. Darüber hinaus enthält der Bericht jetzt den Typ der vom Benutzer verwendeten Client-App, sodass Sie älteren Protokolldatenverkehr identifizieren können. Berichtseinträge können jetzt auch nach einer Korrelations-ID durchsucht werden, die in der an den Benutzer gerichteten Fehlermeldung enthalten ist und dazu verwendet werden kann, die entsprechende Anmeldungsanforderung zu identifizieren und dort ggf. eine Problembehandlung durchzuführen.

---

### <a name="view-legacy-authentications-through-sign-ins-activity-logs"></a>Anzeige von Legacyauthentifizierungen über Protokolle zur Anmeldeaktivität

**Typ:** Neues Feature  
**Dienstkategorie:** Berichterstellung  
**Produktfunktion:** Überwachung und Berichterstellung
 
Dank der Einführung des Felds **Client-App** in den Protokollen zur Anmeldeaktivität können Kunden jetzt Benutzer anzeigen, die Authentifizierungen einer Vorgängerversion nutzen. Kunden können auf diese Informationen zugreifen, indem sie die Microsoft Graph-API für Anmeldungen oder die Protokolle zur Anmeldeaktivität im Azure AD-Portal verwenden. Hier können Sie das Steuerelement **Client-App** nutzen, um nach Legacyauthentifizierungen zu filtern. Weitere Informationen finden Sie in der Dokumentation.

---

### <a name="new-federated-apps-available-in-azure-ad-app-gallery---july-2018"></a>Neue Verbund-Apps im Azure AD-App-Katalog verfügbar – Juli 2018

**Typ:** Neues Feature  
**Dienstkategorie:** Unternehmens-Apps  
**Produktfunktion:** Integration von Drittanbieterprodukten
 
Im Juli 2018 haben wir diese 16 neuen Apps mit Verbundunterstützung in unseren App-Katalog aufgenommen:

[Innovation Hub](https://docs.microsoft.com/azure/active-directory/saas-apps/innovationhub-tutorial), [Leapsome](https://docs.microsoft.com/azure/active-directory/saas-apps/leapsome-tutorial), [Certain Admin SSO](https://docs.microsoft.com/azure/active-directory/saas-apps/certainadminsso-tutorial), PSUC Staging, [iPass SmartConnect](https://docs.microsoft.com/azure/active-directory/saas-apps/ipasssmartconnect-tutorial), [Screencast-O-Matic](https://docs.microsoft.com/azure/active-directory/saas-apps/screencast-tutorial), PowerSchool Unified Classroom, [Eli Onboarding](https://docs.microsoft.com/azure/active-directory/saas-apps/elionboarding-tutorial), [Bomgar Remote Support](https://docs.microsoft.com/azure/active-directory/saas-apps/bomgarremotesupport-tutorial), [Nimblex](https://docs.microsoft.com/azure/active-directory/saas-apps/nimblex-tutorial), [Imagineer WebVision](https://docs.microsoft.com/azure/active-directory/saas-apps/imagineerwebvision-tutorial), [Insight4GRC](https://docs.microsoft.com/azure/active-directory/saas-apps/insight4grc-tutorial), [SecureW2 JoinNow Connector](https://docs.microsoft.com/azure/active-directory/saas-apps/securejoinnow-tutorial), [Kanbanize](https://review.docs.microsoft.com/azure/active-directory/saas-apps/kanbanize-tutorial), [SmartLPA](https://review.docs.microsoft.com/azure/active-directory/saas-apps/smartlpa-tutorial), [Skills Base](https://docs.microsoft.com/azure/active-directory/saas-apps/skillsbase-tutorial)

Weitere Informationen zu den Apps finden Sie unter [SaaS-Anwendungsintegration mit Azure Active Directory](https://aka.ms/appstutorial). Weitere Informationen zum Listen Ihrer Anwendung im Azure AD-App-Katalog finden Sie unter [Listen Ihrer Anwendung im Azure Active Directory-Anwendungskatalog](https://aka.ms/azureadapprequest).

---
 
### <a name="new-user-provisioning-saas-app-integrations---july-2018"></a>Neue Integrationen zur Benutzerbereitstellung in SaaS-Apps – Juli 2018

**Typ:** Neues Feature  
**Dienstkategorie:** App-Bereitstellung  
**Produktfunktion:** Integration von Drittanbieterprodukten
 
Azure AD ermöglicht Ihnen das automatisierte Erstellen, Warten und Entfernen von Benutzeridentitäten in SaaS-Anwendungen, z.B. Dropbox, Salesforce, ServiceNow und anderen. Für Juli 2018 haben wir für die folgenden Anwendungen im Azure AD-App-Katalog Unterstützung für die Benutzerbereitstellung hinzugefügt:

- [Cisco Webex](https://docs.microsoft.com/azure/active-directory/saas-apps/cisco-webex-provisioning-tutorial)

- [Bonusly](https://docs.microsoft.com/azure/active-directory/saas-apps/bonusly-provisioning-tutorial)

Eine Liste mit allen Anwendungen, für die die Benutzerbereitstellung im Azure AD-Katalog unterstützt wird, finden Sie unter [SaaS-Anwendungsintegration mit Azure Active Directory](https://aka.ms/appstutorial).

---

### <a name="connect-health-for-sync---an-easier-way-to-fix-orphaned-and-duplicate-attribute-sync-errors"></a>Connect Health für die Synchronisierung – eine einfachere Möglichkeit, um Synchronisierungsfehler in Verbindung mit verwaisten und doppelten Attributen zu beheben.

**Typ:** Neues Feature  
**Dienstkategorie:** AD Connect  
**Produktfunktion:** Überwachung und Berichterstellung
 
Azure AD Connect Health führt die Self-Service-Wartung ein, die Sie beim Hervorheben und Beheben von Synchronisierungsfehlern unterstützt. Dieses Feature kann für die Problembehandlung bei Synchronisierungsfehlern aufgrund doppelter Attribute und zur Behebung von in Azure AD verwaisten Objekten verwendet werden. Diese Diagnose hat folgende Vorteile:

- Eingrenzung von Synchronisierungsfehlern mit doppelten Attributen, Bereitstellung spezifischer Behebungsverfahren

- Anwendung einer Korrektur für dedizierte Azure AD-Szenarien, Beheben von Fehlern in einem einzigen Schritt

- Kein Upgrade- oder Konfigurationsaufwand zur Aktivierung und Verwendung dieses Features

Weitere Informationen finden Sie unter [Diagnose und Behebung von Synchronisierungsfehlern aufgrund doppelter Attribute](https://docs.microsoft.com/azure/active-directory/connect-health/active-directory-aadconnect-health-diagnose-sync-errors).

---

### <a name="visual-updates-to-the-azure-ad-and-msa-sign-in-experiences"></a>Visuelle Updates der Azure AD- und MSA-Anmeldebenutzeroberflächen

**Typ:** Geändertes Feature  
**Dienstkategorie:** Azure AD  
**Produktfunktion:** Benutzerauthentifizierung

Wir haben die Benutzeroberfläche für die Anmeldung bei Microsoft-Onlinediensten wie Office 365 und Azure aktualisiert. Durch diese Änderung sind die Bildschirme weniger überladen und übersichtlicher. Weitere Informationen zu dieser Änderung finden Sie im Blog [Upcoming improvements to the Azure AD sign-in experience (Bevorstehende Verbesserungen der Azure AD-Anmeldebenutzeroberfläche)](https://cloudblogs.microsoft.com/enterprisemobility/2018/04/04/upcoming-improvements-to-the-azure-ad-sign-in-experience/).

---

### <a name="new-release-of-azure-ad-connect---july-2018"></a>Neues Release von Azure AD Connect – Juli 2018

**Typ:** Geändertes Feature  
**Dienstkategorie:** App-Bereitstellung  
**Produktfunktion:** Identity Lifecycle Management

Das neueste Release von Azure AD Connect umfasst: 

- Programmfehlerbehebungen und Unterstützungsupdates 

- Allgemeine Verfügbarkeit der PingFederate-Integration

- Updates des aktuellen SQL 2012-Clients 

Weitere Informationen zu diesem Update finden Sie unter [Azure AD Connect: Verlauf der Versionsveröffentlichungen](https://docs.microsoft.com/azure/active-directory/connect/active-directory-aadconnect-version-history).

---

### <a name="updates-to-the-terms-of-use-end-user-ui"></a>Updates der Endbenutzeroberfläche der Nutzungsbedingungen

**Typ:** Geändertes Feature  
**Dienstkategorie:** Nutzungsbedingungen  
**Produktfunktion:** Governance

Wir aktualisieren die Zustimmungszeichenfolge in der Endbenutzeroberfläche der ToU.

**Aktueller Text.** Für den Zugriff auf die [Mandantenname]-Ressourcen müssen Sie die Nutzungsbedingungen lesen.<br>**Neuer Text.** Für den Zugriff auf die [Mandantenname]-Ressource müssen Sie die Nutzungsbedingungen lesen.

**Aktueller Text:** Wenn Sie „Akzeptieren“ auswählen, bedeutet dies, dass Sie allen oben aufgeführten Nutzungsbedingungen zustimmen.<br>**Neuer Text:** Klicken Sie auf „Akzeptieren“, um zu bestätigen, dass Sie die Nutzungsbedingungen gelesen und verstanden haben.

---
 
### <a name="pass-through-authentication-supports-legacy-protocols-and-applications"></a>Passthrough-Authentifizierung unterstützt ältere Protokolle und Anwendungen

**Typ:** Geändertes Feature  
**Dienstkategorie:** Authentifizierungen (Anmeldungen)  
**Produktfunktion:** Benutzerauthentifizierung
 
Passthrough-Authentifizierung unterstützt jetzt ältere Protokolle und Apps. Folgende Einschränkungen werden jetzt durch vollständige Unterstützung aufgehoben:

- Benutzeranmeldungen bei älteren Office-Clientanwendungen, Office 2010 und Office 2013, ohne moderne Authentifizierung.

- Zugriff auf Kalenderfreigabe und Frei-/Gebucht-Informationen in Exchange-Hybridumgebungen nur in Office 2010.

- Benutzeranmeldungen bei Skype for Business-Clientanwendungen ohne moderne Authentifizierung.

- Benutzeranmeldungen bei PowerShell Version 1.0.

- Das Apple Device Enrollment Program (Apple DEP. Programm zur Geräteregistrierung) mit dem iOS-Setup-Assistenten. 

---
 
### <a name="converged-security-info-management-for-self-service-password-reset-and-multi-factor-authentication"></a>Zusammengeführte Sicherheitsinformationenverwaltung für Self-Service-Kennwortzurücksetzung und Multi-Factor Authentication

**Typ:** Neues Feature  
**Dienstkategorie:** SSPR  
**Produktfunktion:** Benutzerauthentifizierung

Diese neue Funktion ermöglicht Benutzern die Verwaltung ihrer Sicherheitsinformationen (z.B. Telefonnummer, E-Mail-Adresse, mobile App usw.) für die Self-Service-Kennwortzurücksetzung (Self-Service Password Reset, SSPR) und Multi-Factor Authentication (MFA) in einer einzigen Benutzeroberfläche. Benutzer müssen nicht mehr die gleichen Sicherheitsinfos für SSPR und MFA in zwei unterschiedliche Benutzeroberflächen registrieren. Diese neue Benutzeroberfläche gilt auch für Benutzer, die entweder über SSPR oder MFA verfügen.

Wenn eine Organisation die MFA- oder SSPR-Registrierung nicht erzwingt, können Benutzer ihre Sicherheitsinformationen über das **Meine Apps**-Portal registrieren. Von dort aus können Benutzer mit beliebigen für MFA oder SSPR aktivierten Methoden registrieren. 

Dies ist eine abonnierbare Public Preview. Administratoren können die neue Oberfläche (falls gewünscht) für eine ausgewählte Gruppe von Benutzern oder alle Benutzer in einem Mandanten aktivieren.

---
 
### <a name="use-the-microsoft-authenticator-app-to-verify-your-identity-when-you-reset-your-password"></a>Verwenden Sie die Microsoft Authenticator-App, um Ihre Identität zu bestätigen, wenn Sie Ihr Kennwort zurücksetzen.

**Typ:** Geändertes Feature  
**Dienstkategorie:** SSPR  
**Produktfunktion:** Benutzerauthentifizierung

Mit diesem Feature können Benutzer, die keine Administratoren sind, ihre Identität bestätigen, während sie ein Kennwort mithilfe einer Benachrichtigung oder mittels Code aus der Microsoft Authenticator-App (oder einer beliebigen anderen Authenticator-App) zurücksetzen. Nachdem Administratoren diese Methode zur Self-Service-Kennwortzurücksetzung aktiviert haben, können Benutzer, die eine mobile App über „aka.ms/mfasetup“ oder „aka.ms/setupsecurityinfo“ registriert haben, ihre mobile App beim Zurücksetzen des Kennworts als Überprüfungsmethode verwenden.

Die Benachrichtigung über die mobile App kann nur als Teil einer Richtlinie aktiviert werden, die zwei Methoden zum Zurücksetzen Ihres Kennworts erfordert.

---

## <a name="june-2018"></a>Juni 2018

### <a name="change-notice-security-fix-to-the-delegated-authorization-flow-for-apps-using-azure-ad-activity-logs-api"></a>Änderungshinweis: Sicherheitsfix für den Flow für die delegierte Autorisierung für Apps, die die Azure AD-Aktivitätsprotokolle-API verwenden

**Typ:** Plan für Änderung  
**Dienstkategorie:** Berichterstellung  
**Produktfunktion:** Überwachung und Berichterstellung

Aufgrund unserer strengeren Durchsetzung von Sicherheitsmaßnahmen mussten wir die Berechtigungen für Apps ändern, die für den Zugriff auf [Azure AD-Aktivitätsprotokolle-APIs](https://aka.ms/aadreportsapi) einen Flow für die delegierte Autorisierung verwenden. Diese Änderung erfolgt am **26. Juni 2018**.

Wenn eine Ihrer Apps Azure AD-Aktivitätsprotokolle-APIs verwendet, gehen Sie folgendermaßen vor, um sicherzustellen, dass die App nach Durchführung der Änderung noch betriebsfähig ist.

**So aktualisieren Sie Ihre App-Berechtigungen**

1. Melden Sie sich beim Azure-Portal an. Wählen Sie **Azure Active Directory** und dann **App-Registrierungen** aus.
2. Wählen Sie die App aus, die die Azure AD-Aktivitätsprotokolle-API nutzt. Wählen Sie dann **Einstellungen**, danach **Erforderliche Berechtigungen** und schließlich die API **Microsoft Azure Active Directory** aus.
3. Aktivieren Sie im Bereich **Delegierte Berechtigungen** des Blatts **Zugriff aktivieren** das Kontrollkästchen neben **Verzeichnisdaten lesen**, und wählen Sie dann **Speichern** aus.
4. Wählen Sie **Berechtigungen erteilen** und dann **Ja** aus.
    
    >[!Note]
    >Sie müssen globaler Administrator sein, um Berechtigungen für die App zu erteilen.

Weitere Informationen finden Sie im Abschnitt [Erteilen von Berechtigungen](https://docs.microsoft.com/azure/active-directory/active-directory-reporting-api-prerequisites-azure-portal#grant-permissions) des Artikels „Voraussetzungen für den Zugriff auf die Azure Active Directory-Berichterstellungs-API“.

---

### <a name="configure-tls-settings-to-connect-to-azure-ad-services-for-pci-dss-compliance"></a>Konfigurieren von TLS-Einstellungen zum Herstellen einer Verbindung mit Azure AD-Diensten zum Zweck der PCI-DSS-Konformität

**Typ:** Neues Feature  
**Dienstkategorie:** –  
**Produktfunktion:** Plattform

Transport Layer Security (TLS) ist ein Protokoll, das Datenschutz und -integrität zwischen zwei kommunizierenden Anwendungen gewährleistet, und derzeit das am weitesten verbreitete Sicherheitsprotokoll.

Der [PCI Security Standards Council](https://www.pcisecuritystandards.org/) hat festgelegt, dass frühe Versionen von TLS und Secure Sockets Layer (SSL) deaktiviert werden müssen, um neue und sicherere Anwendungsprotokolle zu ermöglichen, wobei Konformität ab dem **30. Juni 2018** gegeben sein muss. Diese Änderung bedeutet, dass Sie TLS 1.0 deaktivieren müssen, wenn Sie eine Verbindung mit Azure AD-Diensten herstellen und PCI-DSS-Konformität benötigen. Mehrere TLS-Versionen sind verfügbar, aber TLS 1.2 ist die neueste für Azure Active Directory Services verfügbare Version. Es wird ausdrücklich empfohlen, für Client/Server- und Browser/Server-Kombinationen direkt zu TLS 1.2 zu wechseln.

Veraltete Browser unterstützen möglicherweise keine neueren TLS-Versionen wie z.B. TLS 1.2. Um festzustellen, welche Versionen von TLS von Ihrem Browser unterstützt werden, besuchen Sie die Website [Qualys SSL Labs](https://www.ssllabs.com/), und klicken Sie auf **Test your browser**. Sie sollten auf die neueste Version Ihres Webbrowsers aktualisieren und vorzugsweise nur TLS 1.2 aktivieren.

**Aktivieren von TLS 1.2 je nach Browser**

- **Microsoft Edge und Internet Explorer (beide werden mit Internet Explorer festgelegt)**

    1. Öffnen Sie Internet Explorer, und wählen Sie **Tools** > **Internetoptionen** > **Erweitert** aus.
    2. Wählen Sie im Bereich **Sicherheit** die Option **TLS 1.2 verwenden** aus, und klicken Sie dann auf **OK**.
    3. Schließen Sie alle Browserfenster, und starten Sie Internet Explorer neu. 

- **Google Chrome**

    1. Öffnen Sie Google Chrome. Geben Sie in die Adressleiste *chrome://settings/* ein, und drücken Sie die **EINGABETASTE**.
    2. Blenden Sie die Optionen unter **Erweitert** ein. Wechseln Sie zum Abschnitt **System**, und klicken Sie auf **Proxy-Einstellungen öffnen**.
    3. Wählen Sie im Feld **Interneteigenschaften** die Registerkarte **Erweitert** aus. Wechseln Sie zum Abschnitt **Sicherheit**, wählen Sie **TLS 1.2 verwenden** aus, und klicken Sie dann auf  **OK**.
    4. Schließen Sie alle Browserfenster, und starten Sie Google Chrome neu.

- **Mozilla Firefox**

    1. Öffnen Sie Firefox. Geben Sie in die Adressleiste *about:config* ein, und drücken Sie dann die **EINGABETASTE**.
    2. Suchen Sie nach dem Begriff *TLS*, und wählen Sie dann den Eintrag **security.tls.version.max** aus.
    3. Legen Sie den Wert auf **3** fest, um den Browser zu zwingen, Versionen bis TLS 1.2 zu verwenden, und klicken Sie dann auf **OK**.

        >[!NOTE]
        >Die Firefox-Version 60.0 unterstützt TLS 1.3, sodass Sie den Wert „security.tls.version.max“ auch auf **4** festlegen können.

    4. Schließen Sie alle Browserfenster, und starten Sie Mozilla Firefox neu.

---

### <a name="new-federated-apps-available-in-azure-ad-app-gallery---june-2018"></a>Neue Verbund-Apps im Azure AD-App-Katalog verfügbar – Juni 2018

**Typ:** Neues Feature  
**Dienstkategorie:** Unternehmens-Apps  
**Produktfunktion:** Integration von Drittanbieterprodukten
 
Im Juni 2018 haben wir diese 15 neuen Apps mit Verbundunterstützung in unseren App-Katalog aufgenommen:

[Skytap](https://docs.microsoft.com/azure/active-directory/active-directory-saas-skytap-tutorial), [Settling music](https://docs.microsoft.com/azure/active-directory/active-directory-saas-settlingmusic-tutorial), [Branchenspezifische App mit Unterstützung von SAML 1.1-Token](https://docs.microsoft.com/azure/active-directory/active-directory-saas-saml-tutorial), [Supermood](https://docs.microsoft.com/azure/active-directory/active-directory-saas-supermood-tutorial), [Autotask](https://docs.microsoft.com/azure/active-directory/active-directory-saas-autotaskendpointbackup-tutorial), [Endpoint Backup](https://docs.microsoft.com/azure/active-directory/active-directory-saas-autotaskendpointbackup-tutorial), [Skyhigh Networks](https://docs.microsoft.com/azure/active-directory/active-directory-saas-skyhighnetworks-tutorial), Smartway2, [TonicDM](https://docs.microsoft.com/azure/active-directory/active-directory-saas-tonicdm-tutorial), [Moconavi](https://docs.microsoft.com/azure/active-directory/active-directory-saas-moconavi-tutorial), [Zoho One](https://docs.microsoft.com/azure/active-directory/active-directory-saas-zohoone-tutorial), [lokales SharePoint](https://docs.microsoft.com/azure/active-directory/active-directory-saas-sharepoint-on-premises-tutorial), [ForeSee CX Suite](https://docs.microsoft.com/azure/active-directory/active-directory-saas-foreseecxsuite-tutorial), [Vidyard](https://docs.microsoft.com/azure/active-directory/active-directory-saas-vidyard-tutorial), [ChronicX](https://docs.microsoft.com/azure/active-directory/active-directory-saas-chronicx-tutorial)

Weitere Informationen zu den Apps finden Sie unter [SaaS-Anwendungsintegration mit Azure Active Directory](https://aka.ms/appstutorial). Weitere Informationen zum Listen Ihrer Anwendung im Azure AD-App-Katalog finden Sie unter [Listen Ihrer Anwendung im Azure Active Directory-Anwendungskatalog](https://docs.microsoft.com/azure/active-directory/develop/active-directory-app-gallery-listing). 

---

### <a name="azure-ad-password-protection-is-available-in-public-preview"></a>Kennwortschutz für Azure AD in Public Preview verfügbar

**Typ:** Neues Feature  
**Dienstkategorie:** Schutz der Identität (Identity Protection)  
**Produktfunktion:** Benutzerauthentifizierung

Nutzen Sie den Kennwortschutz für Azure AD, um leicht zu erratende Kennwörter aus Ihrer Umgebung zu verbannen. Das Verbannen dieser Kennwörter trägt dazu bei, das Risiko einer Gefährdung durch einen Kennwort-Spray-Angriff zu verringern.

Der Kennwortschutz für Azure AD hilft insbesondere bei folgenden Aufgaben:

- Schützen der Konten Ihrer Organisation in sowohl Azure AD als auch Windows Server Active Directory (AD). 
- Verhindern, dass Ihre Benutzer Kennwörter in einer Liste der mehr als 500 am häufigsten verwendeten Kennwörter und über 1 Mio. Zeichenersetzungsvarianten dieser Kennwörter verwenden.
- Verwalten des Kennwortschutzes für Azure AD an zentraler Stelle im Azure AD-Portal, sowohl für Azure AD als auch für lokales Windows Server AD.

Weitere Informationen zum Kennwortschutz für Azure AD finden Sie unter [Beseitigen falscher Kennwörter in Ihrer Organisation](https://aka.ms/aadpasswordprotectiondocs).

---

### <a name="new-all-guests-conditional-access-policy-template-created-during-terms-of-use-creation"></a>Neue Vorlage für eine Richtlinie für bedingten Zugriff namens „Alle Gäste“, die während der Erstellung der Nutzungsbedingungen erstellt wird

**Typ:** Neues Feature  
**Dienstkategorie:** Nutzungsbedingungen  
**Produktfunktion:** Governance

Während der Erstellung der Nutzungsbedingungen wird eine neue Vorlage für eine Richtlinie für bedingten Zugriff auch für „Alle Gäste“ und „Alle Apps“ erstellt. Diese neue Richtlinienvorlage wendet die neu erstellten Nutzungsbedingungen an und optimiert den Erstellungs- und Erzwingungsprozess für Gäste.

Weitere Informationen finden Sie unter [Nutzungsbedingungsfeature für Azure Active Directory](https://docs.microsoft.com/azure/active-directory/conditional-access/terms-of-use).

---

### <a name="new-custom-conditional-access-policy-template-created-during-terms-of-use-creation"></a>Neue „benutzerdefinierte“ Vorlage für eine Richtlinie für bedingten Zugriff, die während der Erstellung der Nutzungsbedingungen erstellt wird

**Typ:** Neues Feature  
**Dienstkategorie:** Nutzungsbedingungen  
**Produktfunktion:** Governance

Während der Erstellung der Nutzungsbedingungen wird auch eine neue „benutzerdefinierte“ Vorlage für eine Richtlinie für bedingten Zugriff erstellt. Mit dieser neuen Richtlinienvorlage können Sie die Nutzungsbedingungen erstellen und dann sofort zum Blatt für die Erstellung von Richtlinien mit bedingtem Zugriff wechseln, ohne manuell durch das Portal navigieren zu müssen.

Weitere Informationen finden Sie unter [Nutzungsbedingungsfeature für Azure Active Directory](https://docs.microsoft.com/azure/active-directory/conditional-access/terms-of-use).

---

### <a name="new-and-comprehensive-guidance-about-deploying-azure-multi-factor-authentication"></a>Neue und umfassende Anleitung zum Bereitstellen von Azure Multi-Factor Authentication

**Typ:** Neues Feature  
**Dienstkategorie:** Andere  
**Produktfunktion:** Identitätssicherheit und -schutz
 
Wir haben eine neue detaillierte Anleitung zum Bereitstellen von Azure Multi-Factor Authentication (MFA) in Ihrer Organisation veröffentlicht.

Die MFA-Bereitstellungsanleitung finden Sie auf GitHub im Repository [Identity Deployment Guides](https://aka.ms/DeploymentPlans). Um Feedback zu den Bereitstellungsanleitungen zu geben, verwenden Sie das Formular [Deployment Plan Feedback](https://aka.ms/deploymentplanfeedback). Bei Fragen zu den Bereitstellungsanleitungen wenden Sie sich unter [IDGitDeploy](mailto:idgitdeploy@microsoft.com) an uns.

---

### <a name="azure-ad-delegated-app-management-roles-are-in-public-preview"></a>Delegierte Azure AD-App-Verwaltungsrollen in Public Preview

**Typ:** Neues Feature  
**Dienstkategorie:** Unternehmens-Apps  
**Produktfunktion:** Zugriffssteuerung

Administratoren können nun App-Verwaltungsaufgaben delegieren, ohne die Rolle „Globaler Administrator“ zuzuweisen. Es gibt die folgenden neuen Rollen und Funktionen:

- **Neue Azure AD-Standardadministratorrollen:**

    - **Anwendungsadministrator.** Erteilt die Berechtigung, alle Aspekte aller Apps zu verwalten, einschließlich Registrierung, Einstellungen für einmaliges Anmelden (SSO), Zuweisung und Lizenzierung von Apps, Proxyeinstellungen für Apps und Zustimmung (mit Ausnahme von Azure AD-Ressourcen).

    - **Cloudanwendungsadministrator.** Erteilt alle Berechtigungen des Anwendungsadministrators außer für den App-Proxy, da dieser keinen lokalen Zugriff bietet.

    - **Anwendungsentwickler.** Erteilt die Berechtigung zum Erstellen von App-Registrierungen, auch wenn die Option **Benutzer die Registrierung von Apps erlauben** deaktiviert ist.

- **Besitz (wird vergleichbar mit dem Gruppenbesitzprozess je App-Registrierung und Unternehmens-App eingerichtet):**
 
    - **App-Registrierungsbesitzer.** Erteilt die Berechtigung, alle Aspekte der eigenen App-Registrierung zu verwalten, einschließlich App-Manifest und Hinzufügung weiterer Besitzer.

    - **Unternehmens-App-Besitzer.** Erteilt die Berechtigung, zahlreiche Aspekte eigener Unternehmens-Apps zu verwalten, einschließlich Einstellungen für einmaliges Anmelden (SSO), App-Zuweisungen und Zustimmung (mit Ausnahme von Azure AD-Ressourcen).

Weitere Informationen zur Public Preview finden Sie im Blogbeitrag [Azure AD delegated application management roles are in public preview! (Delegierte Azure AD-App-Verwaltungsrollen in Public Preview)](https://cloudblogs.microsoft.com/enterprisemobility/2018/06/13/hallelujah-azure-ad-delegated-application-management-roles-are-in-public-preview/) . Weitere Informationen zu Rollen und Berechtigungen finden Sie unter [Zuweisen von Administratorrollen in Azure Active Directory](https://docs.microsoft.com/azure/active-directory/active-directory-assign-admin-roles-azure-portal).

---

## <a name="may-2018"></a>Mai 2018

### <a name="expressroute-support-changes"></a>Änderungen der Unterstützung für ExpressRoute

**Typ:** Plan für Änderung  
**Dienstkategorie:** Authentifizierungen (Anmeldungen)  
**Produktfunktion:** Plattform  

„Software-as-a-Service“-Angebote wie Azure Active Directory (Azure AD) sind so konzipiert, dass sie direkt über das Internet – ohne ExpressRoute oder sonstige private VPN-Tunnel – am besten funktionieren. Aus diesem Grund beenden wir am **1. August 2018** die Unterstützung für Dienste für ExpressRoute für Azure AD, die öffentliches Azure-Peering und Azure-Communitys in Microsoft-Peering nutzen. Bei allen Diensten, die von dieser Änderung betroffen sind, ist möglicherweise die allmähliche Verschiebung des Azure AD-Datenverkehrs von ExpressRoute zum Internet spürbar.

Während wir unsere Unterstützung ändern, wissen wir allerdings auch, dass es noch Situationen gibt, in denen Sie möglicherweise eine dedizierte Gruppe von Leitungen für Ihren Authentifizierungsdatenverkehr verwenden müssen. Aus diesem Grund wird Azure AD weiterhin Einschränkungen des IP-Adressbereichs pro Mandant mithilfe von ExpressRoute und Diensten, die bereits im Microsoft-Peering enthalten sind, mit der Community „Other Office 365 Online services“ unterstützen. Wenn Ihre Dienste betroffen sind, Sie jedoch ExpressRoute benötigen, müssen Sie die folgenden Schritte ausführen:

- **Wenn Sie sich im öffentlichen Azure-Peering befinden.** Wechseln Sie zu Microsoft-Peering, und registrieren Sie sich für die Community **Other Office 365 Online services (12076:5100)** . Weitere Informationen über den Wechsel vom öffentlichen Azure-Peering zum Microsoft-Peering finden Sie im Artikel [Umstellen von öffentlichem Peering auf Microsoft-Peering](https://docs.microsoft.com/azure/expressroute/how-to-move-peering).

- **Wenn Sie sich im Microsoft-Peering befinden.** Registrieren Sie sich für die Community **Other Office 365 Online service (12076:5100)** . Weitere Informationen zu Routinganforderungen finden Sie im Artikel über ExpressRoute-Routinganforderungen im Abschnitt [Unterstützung für BGP-Communitys](https://docs.microsoft.com/azure/expressroute/expressroute-routing#bgp).

Wenn Sie weiterhin dedizierte Leitungen verwenden müssen, müssen Sie mit Ihrem Microsoft-Kontoteam erörtern, wie Sie die Autorisierung für die Nutzung der Community **Other Office 365 Online service (12076:5100)** erhalten. Das von MS Office verwaltete Prüfungsgremium überprüft, ob Sie diese Leitungen benötigen, und stellt sicher, dass Sie verstehen, welche technischen Auswirkungen damit verbunden sind, sie beizubehalten. Nicht autorisierte Abonnements, die versuchen, Weiterleitungsfilter für Office 365 zu erstellen, erhalten eine Fehlermeldung. 
 
---

### <a name="microsoft-graph-apis-for-administrative-scenarios-for-tou"></a>Microsoft Graph-APIs zum Verwalten der Nutzungsbedingungen

**Typ:** Neues Feature  
**Dienstkategorie:** Nutzungsbedingungen  
**Produktfunktion:** Entwickleroberfläche
 
Wir haben Microsoft Graph-APIs für die Verwaltung der Azure AD-Nutzungsbedingungen hinzugefügt. Sie können das Objekt mit den Nutzungsbedingungen erstellen, aktualisieren und löschen.

---

### <a name="add-azure-ad-multi-tenant-endpoint-as-an-identity-provider-in-azure-ad-b2c"></a>Möglichkeit zum Hinzufügen des mehrinstanzenfähigen Azure AD-Endpunkts als Identitätsanbieter in Azure AD B2C

**Typ:** Neues Feature  
**Dienstkategorie:** B2C – Verwaltung von Endbenutzeridentitäten  
**Produktfunktion:** B2B/B2C
 
Mit benutzerdefinierten Richtlinien können Sie jetzt den gemeinsamen Endpunkt für Azure AD als Identitätsanbieter in Azure AD B2C hinzufügen. Auf diese Weise können Sie einen zentralen Zugangspunkt (Single Point of Entry) für alle Azure AD-Benutzer verwenden, die sich an Ihren Anwendungen anmelden. Weitere Informationen finden Sie unter [Azure Active Directory B2C: Zulassen, dass Benutzer sich mithilfe von benutzerdefinierten Richtlinien bei einem mehrinstanzenfähigen Azure AD-Identitätsanbieter anmelden](https://docs.microsoft.com/azure/active-directory-b2c/active-directory-b2c-setup-commonaad-custom).

---

### <a name="use-internal-urls-to-access-apps-from-anywhere-with-our-my-apps-sign-in-extension-and-the-azure-ad-application-proxy"></a>Verwendung interner URLs für einen standortunabhängigen App-Zugriff – dank Erweiterung zur sicheren Anmeldung bei „Meine Apps“ und Azure AD-Anwendungsproxy

**Typ:** Neues Feature  
**Dienstkategorie:** Meine Apps  
**Produktfunktion:** SSO
 
Benutzer können über interne URLs jetzt auch dann auf Anwendungen zugreifen, wenn sie sich außerhalb Ihres Unternehmensnetzwerks befinden, indem sie die Erweiterung zur sicheren Anmeldung bei „Meine Apps“ verwenden. Dies funktioniert mit allen Anwendungen, die Sie per Azure AD-Anwendungsproxy veröffentlicht haben, und mit jedem Browser, für den die Browsererweiterung für den Zugriffsbereich installiert ist. Die Funktion für die URL-Umleitung wird automatisch aktiviert, nachdem sich ein Benutzer an der Erweiterung angemeldet hat. Die Erweiterung ist für [Microsoft Edge](https://go.microsoft.com/fwlink/?linkid=845176), [Chrome](https://go.microsoft.com/fwlink/?linkid=866367) und [Firefox](https://go.microsoft.com/fwlink/?linkid=866366) als Download verfügbar.

---
 
### <a name="azure-active-directory---data-in-europe-for-europe-customers"></a>Azure Active Directory – Datenspeicherung in Europa für europäische Kunden

**Typ:** Neues Feature  
**Dienstkategorie:** Andere  
**Produktfunktion:** GoLocal

Die Daten von Kunden in Europa müssen in Europa bleiben und dürfen nicht außerhalb von europäischen Rechenzentren repliziert werden. Dies ist erforderlich, um den Datenschutz zu gewährleisten und EU-Gesetze einzuhalten. [Dieser Artikel](https://go.microsoft.com/fwlink/?linkid=872328) enthält spezifische Details dazu, welche Identitätsinformationen in Europa gespeichert werden, und außerdem Details zu den Informationen, die außerhalb von europäischen Datencentern gespeichert werden. 

---
 
### <a name="new-user-provisioning-saas-app-integrations---may-2018"></a>Neue Integrationen zur Benutzerbereitstellung in SaaS-Apps – Mai 2018

**Typ:** Neues Feature  
**Dienstkategorie:** App-Bereitstellung  
**Produktfunktion:** Integration von Drittanbieterprodukten
 
Azure AD ermöglicht Ihnen das automatisierte Erstellen, Warten und Entfernen von Benutzeridentitäten in SaaS-Anwendungen, z.B. Dropbox, Salesforce, ServiceNow und anderen. Für Mai 2018 haben wir für die folgenden Anwendungen im Azure AD-App-Katalog Unterstützung für die Benutzerbereitstellung hinzugefügt:

- [BlueJeans](https://docs.microsoft.com/azure/active-directory/active-directory-saas-bluejeans-provisioning-tutorial)

- [Cornerstone OnDemand](https://docs.microsoft.com/azure/active-directory/active-directory-saas-cornerstone-ondemand-provisioning-tutorial)

- [Zendesk](https://docs.microsoft.com/azure/active-directory/active-directory-saas-zendesk-provisioning-tutorial)

Eine Liste mit allen Anwendungen, für die die Benutzerbereitstellung im Azure AD-Katalog unterstützt wird, finden Sie unter [https://aka.ms/appstutorial](https://aka.ms/appstutorial).

---
 
### <a name="azure-ad-access-reviews-of-groups-and-app-access-now-provides-recurring-reviews"></a>Möglichkeit zur regelmäßigen Durchführung von Azure AD-Zugriffsüberprüfungen für Gruppen- und App-Zugriff

**Typ:** Neues Feature  
**Dienstkategorie:** Zugriffsüberprüfungen  
**Produktfunktion:** Governance
 
Die Zugriffsüberprüfung von Gruppen und Apps ist für Azure AD Premium P2 jetzt allgemein verfügbar.  Administratoren können die Zugriffsüberprüfungen von Gruppenmitgliedschaften und Anwendungszuordnungen so konfigurieren, dass sie automatisch in bestimmten Intervallen durchgeführt werden, z.B. monatlich oder einmal pro Quartal.

---

### <a name="azure-ad-activity-logs-sign-ins-and-audit-are-now-available-through-ms-graph"></a>Azure AD-Aktivitätsprotokolle (Anmeldevorgänge und Überwachung) jetzt über MS Graph verfügbar

**Typ:** Neues Feature  
**Dienstkategorie:** Berichterstellung  
**Produktfunktion:** Überwachung und Berichterstellung
 
Azure AD-Aktivitätsprotokolle, die auch Anmeldungen und Überwachungsprotokolle umfassen, sind jetzt über MS Graph verfügbar. Wir haben zwei Endpunkte über MS Graph verfügbar gemacht, um auf diese Protokolle zuzugreifen. Sehen Sie sich zum Einstieg unsere [Dokumente](https://docs.microsoft.com/azure/active-directory/active-directory-reporting-api-getting-started-azure-portal) zum programmgesteuerten Zugriff auf Azure AD-Berichterstellungs-APIs an. 

---
 
### <a name="improvements-to-the-b2b-redemption-experience-and-leave-an-org"></a>Verbesserungen an der B2B-Oberfläche für die Einlösung und einfacheres Verlassen einer Organisation

**Typ:** Neues Feature  
**Dienstkategorie:** B2B  
**Produktfunktion:** B2B/B2C

**Just-in-Time-Einlösung:** Nachdem Sie eine Ressource für einen Gastbenutzer per B2B-API freigegeben haben, müssen Sie keine spezielle Einladungs-E-Mail mehr senden. In den meisten Fällen kann der Gastbenutzer auf die Ressource zugreifen, und der Einlösevorgang wird „just in time“ durchgeführt. Es entstehen keine Probleme aufgrund von verloren gegangenen E-Mails mehr. Sie müssen Ihren Gastbenutzern nicht mehr die folgende Frage stellen: „Haben Sie auf den vom System gesendeten Link für die Einlösung geklickt?“ Sobald SPO den Einladungs-Manager verwendet, können Anhänge aus der Cloud für alle Benutzer dieselbe kanonische URL verwenden – sowohl intern als auch extern und in jedem Stadium der Einlösung.

**Moderne Oberfläche für die Einlösung:** Es gibt keine Startseite mit geteiltem Bildschirm mehr. Benutzer sehen eine moderne Oberfläche für die Zustimmung, auf der die Datenschutzerklärung der einladenden Organisation angezeigt wird – genau wie bei Drittanbieter-Apps.

**Gastbenutzer können die Organisation verlassen:** Sobald die Beziehung eines Benutzers mit einer Organisation beendet ist, kann dieser die Organisation selbstständig verlassen. Anrufe beim Administrator der einladenden Organisation zum Entfernen des Benutzers entfallen, ebenso wie das Erstellen von Supporttickets.

---

### <a name="new-federated-apps-available-in-azure-ad-app-gallery---may-2018"></a>Neue Verbund-Apps im Azure AD-App-Katalog verfügbar – Mai 2018

**Typ:** Neues Feature  
**Dienstkategorie:** Unternehmens-Apps  
**Produktfunktion:** Integration von Drittanbieterprodukten
 
Im Mai 2018 haben wir diese 18 neuen Apps mit Verbundunterstützung in unseren App-Katalog aufgenommen:

[AwardSpring](https://docs.microsoft.com/azure/active-directory/active-directory-saas-awardspring-tutorial), Infogix Data3Sixty Govern, [Yodeck](https://docs.microsoft.com/azure/active-directory/active-directory-saas-infogix-tutorial), [Jamf Pro](https://docs.microsoft.com/azure/active-directory/active-directory-saas-jamfprosamlconnector-tutorial), [KnowledgeOwl](https://docs.microsoft.com/azure/active-directory/active-directory-saas-knowledgeowl-tutorial), [Envi MMIS](https://docs.microsoft.com/azure/active-directory/active-directory-saas-envimmis-tutorial), [LaunchDarkly](https://docs.microsoft.com/azure/active-directory/active-directory-saas-launchdarkly-tutorial), [Adobe Captivate Prime](https://docs.microsoft.com/azure/active-directory/active-directory-saas-adobecaptivateprime-tutorial), [Montage Online](https://docs.microsoft.com/azure/active-directory/active-directory-saas-montageonline-tutorial), [まなびポケット](https://docs.microsoft.com/azure/active-directory/active-directory-saas-manabipocket-tutorial), OpenReel, [Arc Publishing - SSO](https://docs.microsoft.com/azure/active-directory/active-directory-saas-arc-tutorial), [PlanGrid](https://docs.microsoft.com/azure/active-directory/active-directory-saas-plangrid-tutorial), [iWellnessNow](https://docs.microsoft.com/azure/active-directory/active-directory-saas-iwellnessnow-tutorial), [Proxyclick](https://docs.microsoft.com/azure/active-directory/active-directory-saas-proxyclick-tutorial), [Riskware](https://docs.microsoft.com/azure/active-directory/active-directory-saas-riskware-tutorial), [Flock](https://docs.microsoft.com/azure/active-directory/active-directory-saas-flock-tutorial), [Reviewsnap](https://docs.microsoft.com/azure/active-directory/active-directory-saas-reviewsnap-tutorial)

Weitere Informationen zu den Apps finden Sie unter [SaaS-Anwendungsintegration mit Azure Active Directory](https://aka.ms/appstutorial).

Weitere Informationen zum Listen Ihrer Anwendung im Azure AD-App-Katalog finden Sie unter [Listen Ihrer Anwendung im Azure Active Directory-Anwendungskatalog](https://docs.microsoft.com/azure/active-directory/develop/active-directory-app-gallery-listing).

---
 
### <a name="new-step-by-step-deployment-guides-for-azure-active-directory"></a>Neue detaillierte Bereitstellungsanleitungen für Azure Active Directory

**Typ:** Neues Feature  
**Dienstkategorie:** Andere  
**Produktfunktion:** Verzeichnis
 
Neue detaillierte Anleitungen für die Bereitstellung von Azure Active Directory (Azure AD), einschließlich Self-Service-Kennwortzurücksetzung, einmaliges Anmelden, bedingter Zugriff, App-Proxy, Benutzerbereitstellung, Active Directory-Verbunddienste (AD FS) zu Pass-Through-Authentifizierung und AD FS zu Kennworthashsynchronisierung.

Die Bereitstellungsanleitungen finden Sie auf GitHub im Repository [Identity Deployment Guides](https://aka.ms/DeploymentPlans). Um Feedback zu den Bereitstellungsanleitungen zu geben, verwenden Sie das Formular [Deployment Plan Feedback](https://aka.ms/deploymentplanfeedback). Bei Fragen zu den Bereitstellungsanleitungen wenden Sie sich unter [IDGitDeploy](mailto:idgitdeploy@microsoft.com) an uns.

---

### <a name="enterprise-applications-search---load-more-apps"></a>Suche nach Unternehmensanwendungen – Laden weiterer Apps

**Typ:** Neues Feature  
**Dienstkategorie:** Unternehmens-Apps  
**Produktfunktion:** SSO
 
Haben Sie Probleme, Ihre Anwendungen bzw. Dienstprinzipale zu finden? Wir haben eine Funktion hinzugefügt, um mehr Anwendungen in Ihre Liste der Unternehmensanwendungen zu laden. Standardmäßig werden 20 Anwendungen angezeigt. Sie können ab sofort auf **Weitere laden** klicken, um zusätzliche Anwendungen anzuzeigen. 

---
 
### <a name="the-may-release-of-aadconnect-contains-a-public-preview-of-the-integration-with-pingfederate-important-security-updates-many-bug-fixes-and-new-great-new-troubleshooting-tools"></a>Die im Mai veröffentlichte Version von AADConnect umfasst eine öffentliche Vorschau der PingFederate-Integration, wichtige Sicherheitsupdates, zahlreiche Fehlerkorrekturen und neue großartige Tools für die Problembehandlung. 

**Typ:** Geändertes Feature  
**Dienstkategorie:** AD Connect  
**Produktfunktion:** Identity Lifecycle Management
 
Die im Mai veröffentlichte Version von AADConnect umfasst eine öffentliche Vorschau der PingFederate-Integration, wichtige Sicherheitsupdates, zahlreiche Fehlerkorrekturen und neue großartige Tools für die Problembehandlung. Die Versionshinweise finden Sie [hier](https://docs.microsoft.com/azure/active-directory/connect/active-directory-aadconnect-version-history#118190).

---

### <a name="azure-ad-access-reviews-auto-apply"></a>Azure AD-Zugriffsüberprüfungen: automatische Anwendung

**Typ:** Geändertes Feature  
**Dienstkategorie:** Zugriffsüberprüfungen  
**Produktfunktion:** Governance

Zugriffsüberprüfungen von Gruppen und Apps sind für Azure AD Premium P2 jetzt allgemein verfügbar. Ein Administrator kann konfigurieren, dass die Änderungen des Prüfers automatisch auf diese Gruppe oder App angewendet werden, sobald die Zugriffsüberprüfung abgeschlossen wurde. Der Administrator kann auch festlegen, was mit dem fortgesetzten Zugriff des Benutzers geschieht, wenn Prüfer nicht antworten. Mögliche Optionen hierbei sind: Zugriff entfernen, Zugriff beibehalten oder Systemempfehlungen annehmen. 

---

### <a name="id-tokens-can-no-longer-be-returned-using-the-query-response_mode-for-new-apps"></a>Für neue Apps können ID-Token nicht mehr über den response_mode „query“ zurückgegeben werden. 

**Typ:** Geändertes Feature  
**Dienstkategorie:** Authentifizierungen (Anmeldungen)  
**Produktfunktion:** Benutzerauthentifizierung
 
Ab dem 25.04.2018 erstellte Apps sind nicht mehr in der Lage, ein **id_token** über den response_mode **query** anzufordern.  Damit entspricht Azure AD den OIDC-Spezifikationen und trägt dazu bei, die Angriffsfläche Ihrer Apps zu verringern.  Vor dem 25.04.2018 erstellte Apps werden nicht daran gehindert, den response_mode **query** mit dem response_typ **id_token** zu verwenden.  Der zurückgegebene Fehler beim Anfordern eines id_token aus AAD lautet: **AADSTS70007: „query“ ist beim Anfordern eines Tokens kein unterstützter Wert von „response_mode“** .

Die response_mode-Werte **fragment** und **form_post** funktionieren weiterhin. Vergewissern Sie sich beim Erstellen neuer Anwendungsobjekte (z.B. für die App-Proxyverwendung) vor dem Erstellen einer neuen Anwendung, dass einer dieser response_mode-Werte verwendet wird.  

---
 
## <a name="april-2018"></a>April 2018 

### <a name="azure-ad-b2c-access-token-are-ga"></a>Azure AD B2C-Zugriffstoken jetzt allgemein verfügbar

**Typ:** Neues Feature  
**Dienstkategorie:** B2C – Verwaltung von Endbenutzeridentitäten  
**Produktfunktion:** B2B/B2C 

Sie können nun mit Zugriffstoken auf Web-APIs zugreifen, die von Azure AD B2C geschützt werden. Das Feature wird von der Public Preview in die allgemeine Verfügbarkeit verlegt. Die Benutzeroberfläche zum Konfigurieren von Azure AD B2C-Anwendungen und Web-APIs wurde verbessert, und andere kleinere Verbesserungen wurden vorgenommen.
 
Weitere Informationen finden Sie unter [Azure AD B2C: Anfordern von Zugriffstoken](https://docs.microsoft.com/azure/active-directory-b2c/active-directory-b2c-access-tokens).

---

### <a name="test-single-sign-on-configuration-for-saml-based-applications"></a>Testen der SSO-Konfiguration für SAML-basierte Anwendungen

**Typ:** Neues Feature  
**Dienstkategorie:** Unternehmens-Apps  
**Produktfunktion:** SSO

Beim Konfigurieren von SAML-basierten SSO-Anwendungen können Sie die Integration auf der Konfigurationsseite testen. Wenn während des Anmeldens ein Fehler auftritt, können Sie den Fehler auf der Testbenutzeroberfläche bereitstellen, und Azure AD bietet Ihnen Lösungsschritte für das spezifische Problem an.

Weitere Informationen finden Sie unter

- [Konfigurieren des einmaligen Anmeldens für Anwendungen, die nicht im Azure Active Directory-Anwendungskatalog enthalten sind](https://docs.microsoft.com/azure/active-directory/active-directory-saas-custom-apps)
- [Debuggen des SAML-basierten einmaligen Anmeldens bei Anwendungen in Azure Active Directory](https://docs.microsoft.com/azure/active-directory/develop/active-directory-saml-debugging)

---
 
### <a name="azure-ad-terms-of-use-now-has-per-user-reporting"></a>Azure AD-Nutzungsbedingungen jetzt mit benutzerbasierter Berichterstellung

**Typ:** Neues Feature  
**Dienstkategorie:** Nutzungsbedingungen  
**Produktfunktion:** Compliance
 
Administratoren können jetzt bestimmte Nutzungsbedingungen auswählen und alle Benutzer anzeigen, die diesen Nutzungsbedingungen zugestimmt haben, sowie Datum/Uhrzeit der Zustimmung.

Weitere Informationen finden Sie unter [Nutzungsbedingungsfeature für Azure AD](https://docs.microsoft.com/azure/active-directory/conditional-access/terms-of-use).

---
 
### <a name="azure-ad-connect-health-risky-ip-for-ad-fs-extranet-lockout-protection"></a>Azure AD Connect Health: AD FS-Extranetsperrschutz für riskante IP-Adressen 

**Typ:** Neues Feature  
**Dienstkategorie:** Andere  
**Produktfunktion:** Überwachung und Berichterstellung

Connect Health unterstützt nun die Möglichkeit, IP-Adressen zu erkennen, die einen Schwellenwert stündlicher oder täglicher fehlerhafter Benutzername/Kennwort-Anmeldungen überschreiten. Dieses Feature bietet folgende Funktionen:

- Umfassender Bericht, der IP-Adressen und die Anzahl der fehlerhaften stündlichen/täglichen Anmeldungen auf der Basis eines anpassbaren Schwellenwerts anzeigt.
- E-Mail-basierte Warnungen, die anzeigen, wenn eine bestimmte IP-Adresse den Schwellenwert fehlerhafter stündlicher/täglicher Benutzername/Kennwort-Anmeldungen überschritten hat.
- Eine Downloadoption für eine detaillierte Datenanalyse

Weitere Informationen finden Sie unter [Bericht über riskante IP-Adressen (Public Preview)](https://aka.ms/aadchriskyip).

---
 
### <a name="easy-app-config-with-metadata-file-or-url"></a>Einfache App-Konfiguration mit Metadatendatei oder URL

**Typ:** Neues Feature  
**Dienstkategorie:** Unternehmens-Apps  
**Produktfunktion:** SSO

Auf der Enterprise-Anwendungsseite können Administratoren eine SAML-Metadatendatei zum Konfigurieren von SAML-basiertem Anmelden für AAD-Katalog- und Nicht-Katalog-Anwendung hochladen.

Darüber hinaus können Sie die Azure AD-Anwendungsverbundmetadaten-URL zum Konfigurieren von einmaligem Anmelden mit der entsprechenden Anwendung verwenden.

Weitere Informationen finden Sie unter [Konfigurieren des einmaligen Anmeldens für Anwendungen, die nicht im Azure Active Directory-Anwendungskatalog enthalten sind](https://docs.microsoft.com/azure/active-directory/active-directory-saas-custom-apps).

---

### <a name="azure-ad-terms-of-use-now-generally-available"></a>Azure AD-Nutzungsbedingungen jetzt allgemein verfügbar

**Typ:** Neues Feature  
**Dienstkategorie:** Nutzungsbedingungen  
**Produktfunktion:** Compliance
 

Azure AD-Nutzungsbedingungen wurden von „Public Preview“ in „Allgemein verfügbar“ geändert.

Weitere Informationen finden Sie unter [Nutzungsbedingungsfeature für Azure AD](https://docs.microsoft.com/azure/active-directory/conditional-access/terms-of-use).

---

### <a name="allow-or-block-invitations-to-b2b-users-from-specific-organizations"></a>Zulassen oder Blockieren von Einladungen für B2B-Benutzer von bestimmten Organisationen

**Typ:** Neues Feature  
**Dienstkategorie:** B2B  
**Produktfunktion:** B2B/B2C
 

Sie können jetzt angeben, für welche Partnerorganisationen Sie die Azure AD B2B-Zusammenarbeit und Freigaben einrichten möchten. Zu diesem Zweck können Sie wahlweise eine Liste der spezifischen Zulassungs- oder Verweigerungsdomänen erstellen. Wenn eine Domäne mit diesen Funktionen blockiert wird, können Mitarbeiter nicht mehr Einladungen an Personen in dieser Domäne senden.

So können Sie leichter den Zugriff auf Ihre Ressourcen steuern und dabei die Benutzerfreundlichkeit für berechtigte Benutzer garantieren.

Dieses B2B-Zusammenarbeitsfeature steht allen Kunden von Azure Active Directory zur Verfügung, und in Verbindung mit Azure AD Premium-Features wie bedingtem Zugriff und Identitätsschutz können Sie damit differenzierter steuern, wann und wie sich externe Geschäftsbenutzer anmelden und Zugriff erhalten.

Weitere Informationen finden Sie unter [Zulassen oder Blockieren von Einladungen für B2B-Benutzer von bestimmten Organisationen](https://docs.microsoft.com/azure/active-directory/active-directory-b2b-allow-deny-list).

---
 
### <a name="new-federated-apps-available-in-azure-ad-app-gallery"></a>Neue Verbund-Apps im Azure AD-App-Katalog verfügbar

**Typ:** Neues Feature  
**Dienstkategorie:** Unternehmens-Apps  
**Produktfunktion:** Integration von Drittanbieterprodukten

Im April 2018 haben wir diese 13 neuen Apps mit Verbundunterstützung in unseren App-Katalog aufgenommen:

Criterion HCM, [FiscalNote](https://docs.microsoft.com/azure/active-directory/active-directory-saas-fiscalnote-tutorial), [Secret Server (lokal)](https://docs.microsoft.com/azure/active-directory/active-directory-saas-secretserver-on-premises-tutorial), [Dynamic Signal](https://docs.microsoft.com/azure/active-directory/active-directory-saas-dynamicsignal-tutorial), [mindWireless](https://docs.microsoft.com/azure/active-directory/active-directory-saas-mindwireless-tutorial), [OrgChart Now](https://docs.microsoft.com/azure/active-directory/active-directory-saas-orgchartnow-tutorial), [Ziflow](https://docs.microsoft.com/azure/active-directory/active-directory-saas-ziflow-tutorial), [AppNeta Performance Monitor](https://docs.microsoft.com/azure/active-directory/active-directory-saas-appneta-tutorial), [Elium](https://docs.microsoft.com/azure/active-directory/active-directory-saas-elium-tutorial) , [Fluxx Labs](https://docs.microsoft.com/azure/active-directory/active-directory-saas-fluxxlabs-tutorial), [Cisco Cloud](https://docs.microsoft.com/azure/active-directory/active-directory-saas-ciscocloud-tutorial), Shelf, [SafetyNet](https://docs.microsoft.com/azure/active-directory/active-directory-saas-safetynet-tutorial)

Weitere Informationen zu den Apps finden Sie unter [SaaS-Anwendungsintegration mit Azure Active Directory](https://aka.ms/appstutorial).

Weitere Informationen zum Listen Ihrer Anwendung im Azure AD-App-Katalog finden Sie unter [Listen Ihrer Anwendung im Azure Active Directory-Anwendungskatalog](https://docs.microsoft.com/azure/active-directory/develop/active-directory-app-gallery-listing).

---
 
### <a name="grant-b2b-users-in-azure-ad-access-to-your-on-premises-applications-public-preview"></a>Gewähren des Zugriffs auf Ihre lokalen Anwendungen für B2B-Benutzer in Azure AD (Public Preview)

**Typ:** Neues Feature  
**Dienstkategorie:** B2B  
**Produktfunktion:** B2B/B2C

Wenn Ihr Unternehmen Funktionen der Azure Active Directory B2B-Zusammenarbeit (Azure AD) verwendet, um Gastbenutzer aus Partnerunternehmen zu Ihrem Azure AD einzuladen, können Sie für diese B2B-Benutzer jetzt Zugriff auf lokale Apps bereitstellen. Diese lokalen Apps können die SAML-basierte Authentifizierung oder die integrierte Windows-Authentifizierung (Integrated Windows Authentication, IWA) mit eingeschränkter Kerberos-Delegierung (Kerberos Constrained Delegation, KCD) verwenden.

Weitere Informationen finden Sie unter [Gewähren des Zugriffs auf Ihre lokalen Anwendungen für B2B-Benutzer in Azure AD](https://docs.microsoft.com/azure/active-directory/active-directory-b2b-hybrid-cloud-to-on-premises).

---
 
### <a name="get-sso-integration-tutorials-from-the-azure-marketplace"></a>Tutorials zur SSO-Integration im Azure Marketplace verfügbar

**Typ:** Geändertes Feature  
**Dienstkategorie:** Andere  
**Produktfunktion:** Integration von Drittanbieterprodukten

Wenn eine im [Azure-Marketplace](https://azuremarketplace.microsoft.com/marketplace/apps/category/azure-active-directory-apps?page=1) aufgelistete Anwendung einmaliges Anmelden auf SAML-Basis unterstützt, erhalten Sie beim Klicken auf **Jetzt anfordern** das der Anwendung zugeordnete Integrationstutorial. 

---

### <a name="faster-performance-of-azure-ad-automatic-user-provisioning-to-saas-applications"></a>Verbesserte Leistung bei der automatischen Azure AD-Benutzerbereitstellung in SaaS-Anwendungen

**Typ:** Geändertes Feature  
**Dienstkategorie:** App-Bereitstellung  
**Produktfunktion:** Integration von Drittanbieterprodukten
 
Bisher konnten Kunden, die die Connectors für Azure Active Directory-Benutzerbereitstellung für SaaS-Anwendungen nutzten (z.B. Salesforce, ServiceNow und Box) eine schwache Leistung feststellen, wenn ihre Azure AD-Mandanten über 100.000 kombinierte Benutzer und Gruppen enthielten, und sie legten mit Benutzer- und Gruppenzuweisungen fest, welche Benutzer bereitgestellt werden sollten.

Am 2. April 2018 wurden sehr bedeutende Verbesserungen der Leistung des Azure AD-Bereitstellungsdiensts wirksam, die die erforderliche Zeitspanne zur Ausführung der Erstsynchronisierung von Azure Active Directory mit SaaS-Zielanwendungen erheblich reduzieren.

In der Folge können viele Kunden, bei denen die Erstsynchronisierungen mit Apps mehrere Tage dauerten oder niemals abgeschlossen wurden, den Vorgang jetzt innerhalb Minuten oder Stunden abschließen.

Weitere Informationen finden Sie unter [Vorgänge während der Bereitstellung](https://docs.microsoft.com/azure/active-directory/active-directory-saas-app-provisioning#what-happens-during-provisioning).

---

### <a name="self-service-password-reset-from-windows-10-lock-screen-for-hybrid-azure-ad-joined-machines"></a>Self-Service-Kennwortzurücksetzung über Windows 10-Sperrbildschirm für hybride Computer mit Azure AD-Einbindung

**Typ:** Geändertes Feature  
**Dienstkategorie:** Self-Service-Kennwortzurücksetzung  
**Produktfunktion:** Benutzerauthentifizierung
 
Das Windows 10-SSPR-Feature wurde aktualisiert, sodass es die Unterstützung für hybride Computer mit Azure AD-Einbindung einschließt. Dieses in Windows 10 RS4 verfügbare Feature ermöglicht Benutzern, ihre Kennwörter vom Sperrbildschirm eines Windows 10-Computers aus zurückzusetzen. Benutzer, die für die Self-Service-Kennwortzurücksetzung zugelassen und registriert sind, können dieses Feature nutzen.

Weitere Informationen finden Sie unter [Azure AD-Kennwortzurücksetzung über den Anmeldebildschirm](https://docs.microsoft.com/azure/active-directory/authentication/tutorial-sspr-windows).

---

## <a name="march-2018"></a>März 2018
 
### <a name="certificate-expire-notification"></a>Benachrichtigung über Zertifikatablauf

**Typ:** Korrigiert  
**Dienstkategorie:** Unternehmens-Apps  
**Produktfunktion:** SSO
 
Azure AD sendet eine Benachrichtigung, wenn ein Zertifikat für eine Katalog- oder Nicht-Kataloganwendung kurz vor dem Ablauf steht. 

Einige Benutzer haben für Unternehmensanwendungen, die für SAML-basiertes einmaliges Anmelden konfiguriert waren, keine Benachrichtigungen erhalten. Dieses Problem wurde behoben. Azure AD sendet Benachrichtigungen für Zertifikate, die in 7, 30 und 60 Tagen ablaufen. Sie können dieses Ereignis in den Überwachungsprotokollen sehen. 

Weitere Informationen finden Sie unter

- [Verwalten von Zertifikaten für die einmalige Verbundanmeldung in Azure Active Directory](https://docs.microsoft.com/azure/active-directory/active-directory-sso-certs)
- [Berichte zu Überwachungsaktivitäten im Azure Active Directory-Portal](https://docs.microsoft.com/azure/active-directory/active-directory-reporting-activity-audit-logs)
 
---
 
### <a name="twitter-and-github-identity-providers-in-azure-ad-b2c"></a>Identitätsanbieter Twitter und GitHub in Azure AD B2C

**Typ:** Neues Feature  
**Dienstkategorie:** B2C – Verwaltung von Endbenutzeridentitäten  
**Produktfunktion:** B2B/B2C
 
Sie können in Azure AD B2C jetzt Twitter oder GitHub als Identitätsanbieter hinzufügen. Twitter stellt von der öffentlichen Vorschau auf allgemeine Verfügbarkeit um. GitHub wird als öffentliche Vorschau veröffentlicht.

Weitere Informationen finden Sie unter [Was ist die Azure AD B2B-Zusammenarbeit?](https://docs.microsoft.com/azure/active-directory/active-directory-b2b-what-is-azure-ad-b2b).
 
---

### <a name="restrict-browser-access-using-intune-managed-browser-with-azure-ad-application-based-conditional-access-for-ios-and-android"></a>Einschränken des Browserzugriffs durch Verwendung von Intune Managed Browser mit anwendungsbasiertem bedingtem Azure AD-Zugriff für iOS und Android

**Typ:** Neues Feature  
**Dienstkategorie:** Bedingter Zugriff  
**Produktfunktion:** Identitätssicherheit und -schutz
 
**Jetzt in der Public Preview!**

**Intune Managed Browser – SSO:** Ihre Mitarbeiter können einmaliges Anmelden übergreifend für native Clients (wie Microsoft Outlook) und Intune Managed Browser für alle mit Azure AD verbundenen Apps verwenden.

**Intune Managed Browser – Unterstützung von bedingtem Zugriff:** Mithilfe von Richtlinien für anwendungsbasierten bedingten Zugriff können Sie nun für Mitarbeiter die Verwendung von Intune Managed Browser erzwingen.

Weitere Informationen hierzu finden Sie in unserem [Blogbeitrag](https://cloudblogs.microsoft.com/enterprisemobility/2018/03/15/the-intune-managed-browser-now-supports-azure-ad-sso-and-conditional-access/).

Weitere Informationen finden Sie unter

- [Einrichten des app-basierten bedingten Zugriffs](https://docs.microsoft.com/azure/active-directory/conditional-access/app-based-conditional-access)

- [Verwalten des Internetzugriffs mittels Richtlinien für Managed Browser mit Microsoft Intune](https://aka.ms/managedbrowser)  

---
 
### <a name="app-proxy-cmdlets-in-powershell-ga-module"></a>App-Proxy-Cmdlets im allgemein verfügbaren Powershell-Modul

**Typ:** Neues Feature  
**Dienstkategorie:** Anwendungsproxy  
**Produktfunktion:** Zugriffssteuerung
 
Das allgemein verfügbare PowerShell-Modul bietet jetzt Unterstützung für Anwendungsproxy-Cmdlets! Dass erfordert, dass Sie bei PowerShell-Modulen auf dem neuesten Stand bleiben müssen. Wenn Sie mehr als ein Jahr in Rückstand geraten, funktionieren einige Module möglicherweise nicht mehr. 

Weitere Informationen finden Sie unter [AzureAD](https://docs.microsoft.com/powershell/module/Azuread/?view=azureadps-2.0).
 
---
 
### <a name="office-365-native-clients-are-supported-by-seamless-sso-using-a-non-interactive-protocol"></a>Native Office 365-Clients werden vom nahtlosen einmaligen Anmelden mithilfe eines nicht interaktiven Protokolls unterstützt

**Typ:** Neues Feature  
**Dienstkategorie:** Authentifizierungen (Anmeldungen)  
**Produktfunktion:** Benutzerauthentifizierung
 
Benutzer, die native Office 365-Clients (Version 16.0.8730.xxxx und höher) einsetzen, genießen mit nahtlosem einmaligem Anmelden eine Anmeldung ohne Benutzereingriff. Diese Unterstützung wird durch die Ergänzung von Azure AD durch ein nicht interaktives Protokoll (WS-Trust) bereitgestellt.

Weitere Informationen finden Sie unter [Wie funktioniert die Anmeldung auf einem nativen Client mit dem nahtlosen einmaligen Anmelden?](https://docs.microsoft.com/azure/active-directory/connect/active-directory-aadconnect-sso-how-it-works#how-does-sign-in-on-a-native-client-with-seamless-sso-work).
 
---

### <a name="users-get-a-silent-sign-on-experience-with-seamless-sso-if-an-application-sends-sign-in-requests-to-azure-ads-tenant-endpoints"></a>Benutzer kommen mit dem nahtlosen einmaligen Anmelden in den Genuss einer Anmeldung ohne Benutzereingriff, wenn eine Anwendung Anmeldeanforderungen an Azure AD-Mandantenendpunkte sendet

**Typ:** Neues Feature  
**Dienstkategorie:** Authentifizierungen (Anmeldungen)  
**Produktfunktion:** Benutzerauthentifizierung
 
Benutzer erhalten mit dem nahtlosen einmaligen Anmelden eine Anmeldung ohne Benutzereingriff, wenn eine Anwendung (z.B. `https://contoso.sharepoint.com`) Anmeldeanforderungen nicht mehr an den allgemeinen Azure AD-Endpunkt (`https://login.microsoftonline.com/common/<...>`) sendet, sondern an Mandantenendpunkte von Azure AD – d.h. `https://login.microsoftonline.com/contoso.com/<..>` oder `https://login.microsoftonline.com/<tenant_ID>/<..>`.

Weitere Informationen finden Sie unter [Nahtlose einmalige Anmeldung mit Azure Active Directory](https://docs.microsoft.com/azure/active-directory/connect/active-directory-aadconnect-sso). 

---
 
### <a name="need-to-add-only-one-azure-ad-url-instead-of-two-urls-previously-to-users-intranet-zone-settings-to-roll-out-seamless-sso"></a>Für das Rollout des nahtlosen einmaligen Anmeldens muss den Intranet-Zoneneinstellungen des Benutzers nur noch eine Azure AD-URL statt zwei wie bisher hinzugefügt werden

**Typ:** Neues Feature  
**Dienstkategorie:** Authentifizierungen (Anmeldungen)  
**Produktfunktion:** Benutzerauthentifizierung
 
Für das Rollout des nahtlosen einmaligen Anmeldens für Ihre Benutzer müssen Sie den Intranet-Zoneneinstellungen Ihrer Benutzer nur eine Azure AD-URL mithilfe einer Gruppenrichtlinie in Active Directory hinzufügen: `https://autologon.microsoftazuread-sso.com`. Bisher mussten Kunden zwei URLs hinzufügen.

Weitere Informationen finden Sie unter [Nahtlose einmalige Anmeldung mit Azure Active Directory](https://docs.microsoft.com/azure/active-directory/connect/active-directory-aadconnect-sso). 
 
---
 
### <a name="new-federated-apps-available-in-azure-ad-app-gallery"></a>Neue Verbund-Apps im Azure AD-App-Katalog verfügbar

**Typ:** Neues Feature  
**Dienstkategorie:** Unternehmens-Apps  
**Produktfunktion:** Integration von Drittanbieterprodukten

Im März 2018 haben wir diese 15 neuen Apps mit Verbundunterstützung in unseren App-Katalog aufgenommen:

[Boxcryptor](https://docs.microsoft.com/azure/active-directory/active-directory-saas-boxcryptor-tutorial), [CylancePROTECT](https://docs.microsoft.com/azure/active-directory/active-directory-saas-cylanceprotect-tutorial), Wrike, [SignalFx](https://docs.microsoft.com/azure/active-directory/active-directory-saas-signalfx-tutorial), Assistant von FirstAgenda, [YardiOne](https://docs.microsoft.com/azure/active-directory/active-directory-saas-yardione-tutorial), Vtiger CRM, inwink, [Amplitude](https://docs.microsoft.com/azure/active-directory/active-directory-saas-amplitude-tutorial), [Spacio](https://docs.microsoft.com/azure/active-directory/active-directory-saas-spacio-tutorial), [ContractWorks](https://docs.microsoft.com/azure/active-directory/active-directory-saas-contractworks-tutorial), [Bersin](https://docs.microsoft.com/azure/active-directory/active-directory-saas-bersin-tutorial), [Mercell](https://docs.microsoft.com/azure/active-directory/active-directory-saas-mercell-tutorial), [Trisotech Digital Enterprise Server](https://docs.microsoft.com/azure/active-directory/active-directory-saas-trisotechdigitalenterpriseserver-tutorial), [Qumu Cloud](https://docs.microsoft.com/azure/active-directory/active-directory-saas-qumucloud-tutorial).
 
Weitere Informationen zu den Apps finden Sie unter [SaaS-Anwendungsintegration mit Azure Active Directory](https://aka.ms/appstutorial).

Weitere Informationen zum Listen Ihrer Anwendung im Azure AD-App-Katalog finden Sie unter [Listen Ihrer Anwendung im Azure Active Directory-Anwendungskatalog](https://docs.microsoft.com/azure/active-directory/develop/active-directory-app-gallery-listing). 

---
 
### <a name="pim-for-azure-resources-is-generally-available"></a>PIM für Azure-Ressourcen ist allgemein verfügbar

**Typ:** Neues Feature  
**Dienstkategorie:** Privileged Identity Management  
**Produktfunktion:** Privileged Identity Management
 
Wenn Sie Azure AD Privileged Identity Management für Verzeichnisrollen verwenden, können Sie jetzt die PIM-Funktionen für zeitgesteuerten Zugriff und Zuweisungen für Azure-Ressourcenrollen verwenden, wie etwa Abonnements, Ressourcengruppen, virtuelle Computer und beliebige andere vom Azure Resource Manager unterstützte Ressourcen. Erzwingen der Multi-Factor Authentication bei der Just-In-Time-Aktivierung von Rollen und Planen von Aktivierungen in Abstimmung mit genehmigten Änderungsfenstern. Darüber hinaus wurden in dieser Version Verbesserungen hinzugefügt, die in der öffentlichen Vorschau nicht verfügbar waren, darunter eine aktualisierte Benutzeroberfläche, Genehmigungsworkflows und die Möglichkeit zur Verlängerung bald ablaufender Rollen und zur Erneuerung abgelaufener Rollen.

Weitere Informationen finden Sie unter [PIM für Azure-Ressourcen (Vorschauversion)](https://docs.microsoft.com/azure/active-directory/privileged-identity-management/azure-pim-resource-rbac).
 
---
 
### <a name="adding-optional-claims-to-your-apps-tokens-public-preview"></a>Hinzufügen von optionalen Ansprüchen zu App-Token (öffentliche Vorschau)

**Typ:** Neues Feature  
**Dienstkategorie:** Authentifizierungen (Anmeldungen)  
**Produktfunktion:** Benutzerauthentifizierung
 
Ihre Azure AD-App kann jetzt benutzerdefinierte oder optionale Ansprüche in JWTs oder SAML-Token anfordern.  Dabei handelt es sich um den Benutzer oder Mandanten betreffende Ansprüche, die aufgrund von Größen- oder Anwendbarkeitseinschränkungen nicht standardmäßig im Token enthalten sind.  Diese Funktion befindet sich aktuell in der öffentlichen Vorschau für Azure AD-Apps v1.0- und v2.0-Endpunkten.  Informationen zu den Ansprüchen, die hinzugefügt werden können, und zum Bearbeiten Ihres Anwendungsmanifests für ihre Anforderung finden Sie in der Dokumentation.  

Weitere Informationen finden Sie unter [Optionale Ansprüche in Azure AD](https://docs.microsoft.com/azure/active-directory/develop/active-directory-optional-claims) (Optionale Ansprüche in Azure AD).
 
---
 
### <a name="azure-ad-supports-pkce-for-more-secure-oauth-flows"></a>Azure AD unterstützt PKCE für sicherere OAuth-Flows

**Typ:** Neues Feature  
**Dienstkategorie:** Authentifizierungen (Anmeldungen)  
**Produktfunktion:** Benutzerauthentifizierung
 
Die Azure AD-Dokumentation wurde um Unterstützung für PKCE ergänzt, das eine sicherere Kommunikation beim Gewährungsflow des OAuth 2.0-Autorisierungscodes ermöglicht.  An v1.0- und v2.0-Endpunkten werden sowohl S256- als auch Klartext-Abfragecodes unterstützt. 

Weitere Informationen finden Sie unter [Anfordern eines Autorisierungscodes](https://docs.microsoft.com/azure/active-directory/develop/active-directory-v2-protocols-oauth-code#request-an-authorization-code). 
 
---
 
### <a name="support-for-provisioning-all-user-attribute-values-available-in-the-workday-get_workers-api"></a>Unterstützung der Bereitstellung aller in der Workday Get_Workers-API verfügbaren Benutzerattributwerte

**Typ:** Neues Feature  
**Dienstkategorie:** App-Bereitstellung  
**Produktfunktion:** Integration von Drittanbieterprodukten
 
Die öffentliche Vorschau der eingehenden Bereitstellung von Workday in Active Directory und Azure AD unterstützt jetzt die Funktion zum Extrahieren und Bereitstellen aller in der Workday Get_Workers-API verfügbaren Attributwerte. Dadurch wird jenseits der in der ursprünglichen Version des Workday-Connectors für eingehende Bereitstellung mitgelieferten Attribute Unterstützung für Hunderte weiterer Standard- und benutzerdefinierter Attribute hinzugefügt.

Weitere Informationen finden Sie unter [Anpassen der Liste der Workday-Benutzerattribute](https://docs.microsoft.com/azure/active-directory/active-directory-saas-workday-inbound-tutorial#customizing-the-list-of-workday-user-attributes)

---

### <a name="changing-group-membership-from-dynamic-to-static-and-vice-versa"></a>Ändern der Gruppenmitgliedschaft von dynamisch in statisch (und umgekehrt)

**Typ:** Neues Feature  
**Dienstkategorie:** Gruppenverwaltung  
**Produktfunktion:** Zusammenarbeit
 
Sie können ändern, wie die Mitgliedschaft in einer Gruppe verwaltet wird. Dies ist hilfreich, wenn Sie im System den gleichen Gruppennamen und die dazugehörige ID beibehalten möchten, damit alle vorhandenen Verweise auf die Gruppe weiterhin gültig sind. Wenn eine neue Gruppe erstellt wird, müssen diese Verweise aktualisiert werden.
Wir haben das Azure AD Admin Center um Unterstützung für diese Funktionalität erweitert. Kunden können jetzt vorhandene Gruppen von dynamischer Mitgliedschaft auf zugewiesene Mitgliedschaft umstellen und umgekehrt. Die vorhandenen PowerShell-Cmdlets stehen ebenfalls weiterhin zur Verfügung.

Weitere Informationen finden Sie unter [Regeln für eine dynamische Mitgliedschaft für Gruppen in Azure Active Directory](https://docs.microsoft.com/azure/active-directory/users-groups-roles/groups-dynamic-membership).

---

### <a name="improved-sign-out-behavior-with-seamless-sso"></a>Verbessertes Abmeldeverhalten bei nahtlosem einmaligem Anmelden

**Typ:** Geändertes Feature  
**Dienstkategorie:** Authentifizierungen (Anmeldungen)  
**Produktfunktion:** Benutzerauthentifizierung
 
Selbst wenn sie sich explizit bei einer durch Azure AD geschützten Anwendung abgemeldet hatten, wurden Benutzer bei einem erneuten Zugriffsversuch auf eine Azure AD-Anwendung aus dem Unternehmensnetzwerk von in der Domäne registrierten Geräten aus bisher mithilfe von nahtlosem einmaligem Anmelden automatisch wieder angemeldet. Mit dieser Änderung wird die Abmeldung unterstützt.  Dadurch können Benutzer für die erneute Anmeldung das gleiche oder ein anderes Azure AD-Konto auswählen, statt mithilfe von nahtlosem Anmelden automatisch angemeldet zu werden.

Weitere Informationen finden Sie unter [Nahtlose einmalige Anmeldung mit Azure Active Directory](https://docs.microsoft.com/azure/active-directory/connect/active-directory-aadconnect-sso).
 
---
 
### <a name="application-proxy-connector-version-154020-released"></a>Anwendungsproxyconnector Version 1.5.402.0 veröffentlicht

**Typ:** Geändertes Feature  
**Dienstkategorie:** Anwendungsproxy  
**Produktfunktion:** Identitätssicherheit und -schutz
 
Das Rollout dieser Connectorversion erfolgt nach und nach im Lauf des Novembers. Diese neue Connectorversion enthält die folgenden Änderungen:

- Der Connector legt jetzt Cookies auf Domänen- statt auf Unterdomänenebene fest. Dadurch wird ein reibungsloseres Verhalten beim einmaligen Anmelden sichergestellt, und redundante Authentifizierungsaufforderungen werden vermieden.
- Unterstützung für aufgeteilte Codierungsanforderungen
- Verbesserte Connector-Integritätsüberwachung 
- Verschiedene behobene Programmfehler und verbesserte Stabilität

Weitere Informationen finden Sie unter [Grundlegendes zu Azure AD-Anwendungsproxyconnectors](https://docs.microsoft.com/azure/active-directory/application-proxy-understand-connectors).
 
---

## <a name="february-2018"></a>Februar 2018
 
### <a name="improved-navigation-for-managing-users-and-groups"></a>Verbesserte Navigation zum Verwalten von Benutzern und Gruppen

**Typ:** Plan für Änderung  
**Dienstkategorie:** Verzeichnisverwaltung  
**Produktfunktion:** Verzeichnis

Die Navigationserfahrung zum Verwalten von Benutzern und Gruppen wurde optimiert. Sie können nun von der Verzeichnisübersicht aus direkt zur Liste aller Benutzer navigieren, um einfacher auf die Liste der gelöschten Benutzer zugreifen zu können. Sie können auch aus der Verzeichnisübersicht direkt zur Liste aller Gruppen navigieren, was den Zugriff auf die Einstellungen der Gruppenverwaltung erleichtert. Und von der Verzeichnisübersichtsseite aus können Sie außerdem nach einem Benutzer, einer Gruppe, einer Unternehmensanwendung oder einer App-Registrierung suchen. 

---

### <a name="availability-of-sign-ins-and-audit-reports-in-microsoft-azure-operated-by-21vianet-azure-china-21vianet"></a>Verfügbarkeit von Anmeldungen und Überwachungsberichten in Microsoft Azure, betrieben von 21Vianet (Azure China 21Vianet)

**Typ:** Neues Feature  
**Dienstkategorie:** Azure Stack  
**Produktfunktion:** Überwachung und Berichterstellung

Azure AD-Aktivitätsprotokollberichte sind nun in Microsoft Azure-Instanzen verfügbar, betrieben von 21Vianet (Azure China 21Vianet). Die folgenden Protokolle sind enthalten:

- **Aktivitätsprotokolle zu Anmeldevorgängen**: Umfasst alle Anmeldungsprotokolle, die Ihrem Mandanten zugeordnet sind.

- **Self-Service-Kennwortüberwachungsprotokolle**: Umfasst alle SSPR-Überwachungsprotokolle.

- **Verzeichnisverwaltungs-Überwachungsprotokolle**: Umfasst alle mit der Verzeichnisverwaltung zusammenhängenden Überwachungsprotokolle wie Benutzerverwaltung, App-Verwaltung und andere.

Mit diesen Protokollen können Sie Einblicke in den Zustand Ihrer Umgebung gewinnen. Die bereitgestellten Daten ermöglichen Ihnen Folgendes:

- Ermitteln, wie Ihre Apps und Dienste von Ihren Benutzern genutzt werden.

- Behandeln von Problemen, die Ihre Benutzer an der Erledigung ihrer Arbeit hindern.

Weitere Informationen zum Verwenden dieser Berichte finden Sie unter [Azure Active Directory-Berichterstellung](https://docs.microsoft.com/azure/active-directory/active-directory-reporting-azure-portal).

---

### <a name="use-report-reader-role-non-admin-role-to-view-azure-ad-activity-reports"></a>Verwenden der Rolle „Berichtsleser“ (Nicht-Administratorrolle) zum Anzeigen von Azure AD-Aktivitätsberichten

**Typ:** Neues Feature  
**Dienstkategorie:** Berichterstellung  
**Produktfunktion:** Überwachung und Berichterstellung

Als Teil des Kundenfeedbacks, Nicht-Administratorrollen den Zugriff auf die Azure AD-Aktivitätsprotokolle zu ermöglichen, haben wir eine Möglichkeit geschaffen, dass Benutzer, die die Rolle „Berichtsleser“ ausüben, auf die Anmelde- und Überwachungsaktivitäten im Azure-Portal sowie auf unsere Graph-APIs zugreifen können. 

Weitere Informationen zum Verwenden dieser Berichte finden Sie unter [Azure Active Directory-Berichterstellung](https://docs.microsoft.com/azure/active-directory/active-directory-reporting-azure-portal). 

---

### <a name="employeeid-claim-available-as-user-attribute-and-user-identifier"></a>EmployeeID-Anspruch als Benutzerattribut und Benutzer-ID verfügbar

**Typ:** Neues Feature  
**Dienstkategorie:** Unternehmens-Apps  
**Produktfunktion:** SSO
 
Sie können **EmployeeID** als Benutzerbezeichner und Benutzerattribut für Mitgliederbenutzer und B2B-Gäste in SAML-basierten Anmeldeanwendungen von der Benutzeroberfläche der Unternehmensanwendung aus konfigurieren.

Weitere Informationen finden Sie unter [Anpassen ausgestellter Ansprüche im SAML-Token für Unternehmensanwendungen in Azure Active Directory](https://docs.microsoft.com/azure/active-directory/develop/active-directory-saml-claims-customization).

---

### <a name="simplified-application-management-using-wildcards-in-azure-ad-application-proxy"></a>Vereinfachte Anwendungsverwaltung mithilfe von Platzhaltern im Azure AD-Anwendungsproxy

**Typ:** Neues Feature  
**Dienstkategorie:** Anwendungsproxy  
**Produktfunktion:** Benutzerauthentifizierung
 
Um die Anwendungsbereitstellung zu vereinfachen und Ihren Verwaltungsaufwand zu verringern, unterstützen wir nun die Möglichkeit, Anwendungen mithilfe von Platzhaltern zu veröffentlichen. Um eine Platzhalteranwendung zu veröffentlichen, können Sie dem Standardablauf bei der Veröffentlichung von Anwendungen folgen, aber einen Platzhalter in den internen und externen URLs verwenden.

Weitere Informationen finden Sie unter [Platzhalteranwendungen im Azure Active Directory-Anwendungsproxy](https://docs.microsoft.com/azure/active-directory/active-directory-application-proxy-wildcard)

---

### <a name="new-cmdlets-to-support-configuration-of-application-proxy"></a>Neue Cmdlets zur Unterstützung der Konfiguration des Anwendungsproxys

**Typ:** Neues Feature  
**Dienstkategorie:** Anwendungsproxy  
**Produktfunktion:** Plattform

Das neueste Release der Vorschau des Moduls AzureAD PowerShell Preview enthält neue Cmdlets, die es Kunden ermöglichen, Anwendungsproxyanwendungen mithilfe von PowerShell zu konfigurieren.

Die folgenden Cmdlets sind neu: 

- Get-AzureADApplicationProxyApplication
- Get-AzureADApplicationProxyApplicationConnectorGroup
- Get-AzureADApplicationProxyConnector
- Get-AzureADApplicationProxyConnectorGroup
- Get-AzureADApplicationProxyConnectorGroupMembers
- Get-AzureADApplicationProxyConnectorMemberOf
- New-AzureADApplicationProxyApplication
- New-AzureADApplicationProxyConnectorGroup
- Remove-AzureADApplicationProxyApplication
- Remove-AzureADApplicationProxyApplicationConnectorGroup
- Remove-AzureADApplicationProxyConnectorGroup
- Set-AzureADApplicationProxyApplication
- Set-AzureADApplicationProxyApplicationConnectorGroup
- Set-AzureADApplicationProxyApplicationCustomDomainCertificate
- Set-AzureADApplicationProxyApplicationSingleSignOn
- Set-AzureADApplicationProxyConnector
- Set-AzureADApplicationProxyConnectorGroup

---
 
### <a name="new-cmdlets-to-support-configuration-of-groups"></a>Neue Cmdlets zur Unterstützung der Konfiguration von Gruppen

**Typ:** Neues Feature  
**Dienstkategorie:** Anwendungsproxy  
**Produktfunktion:** Plattform

Das neueste Release des AzureAD PowerShell-Moduls enthält Cmdlets zum Verwalten von Gruppen in Azure AD. Diese Cmdlets waren zuvor im AzureADPreview-Modul verfügbar und werden nun dem AzureAD-Modul hinzugefügt.

Folgende Gruppen-Cmdlets werden nun für die allgemeine Verfügbarkeit freigegeben: 

- Get-AzureADMSGroup
- New-AzureADMSGroup
- Remove-AzureADMSGroup
- Set-AzureADMSGroup
- Get-AzureADMSGroupLifecyclePolicy
- New-AzureADMSGroupLifecyclePolicy
- Remove-AzureADMSGroupLifecyclePolicy
- Add-AzureADMSLifecyclePolicyGroup
- Remove-AzureADMSLifecyclePolicyGroup
- Reset-AzureADMSLifeCycleGroup   
- Get-AzureADMSLifecyclePolicyGroup

---
 
### <a name="a-new-release-of-azure-ad-connect-is-available"></a>Ein neues Release von Azure AD Connect ist verfügbar

**Typ:** Neues Feature  
**Dienstkategorie:** AD Sync  
**Produktfunktion:** Plattform
 
Azure AD Connect ist das bevorzugte Tool zur Synchronisierung von Daten zwischen Azure AD und lokalen Datenquellen, einschließlich Windows Server Active Directory und LDAP.

>[!Important]
>Bei diesem Build wurden Änderungen an Schemas und Synchronisierungsregeln vorgenommen. Der Azure AD Connect-Synchronisierungsdienst löst nach einem Upgrade die Schritte „Vollständiger Import“ und „Vollständige Synchronisierung“ aus. Informationen zum Ändern dieses Verhaltens finden Sie unter [Zurückstellen der vollständigen Synchronisierung nach dem Upgrade](https://docs.microsoft.com/azure/active-directory/connect/active-directory-aadconnect-upgrade-previous-version#how-to-defer-full-synchronization-after-upgrade).

Dieses Release enthält die folgenden Updates und Änderungen:

**Behobene Probleme**

- Es wurde ein Problem mit dem Zeitfenster für Hintergrundaufgaben auf der Seite „Partitionsfilterung“ behoben, das beim Wechsel auf die nächste Seite entstand.

- Es wurde ein Problem behoben, das während einer benutzerdefinierten ConfigDB-Aktion zu einer Zugriffsverletzung führte.

- Es wurde ein Problem mit der Wiederherstellung nach einem SQL-Verbindungstimeout behoben.

- Es wurde ein Problem behoben, bei dem Zertifikate mit SAN-Platzhaltern eine Voraussetzungsprüfung nicht bestanden.

- Es wurde ein Problem behoben, das zu einem Absturz von „miiserver.exe“ während eines AAD-Connectorexports führte.

- Es wurde ein Problem mit ungültigen Kennworteingabeversuchen behoben, die beim Ausführen des AAD Connect-Assistenten zum Ändern der Konfiguration auf dem Domänencontroller protokolliert wurden

**Neue Features und Verbesserungen**
 
- Anwendungstelemetrie: Administratoren können diese Datenklasse aktivieren/deaktivieren.

- Azure AD-Integritätsdaten: Administratoren müssen das Integritätsportal öffnen, um die Integritätseinstellungen zu steuern. Sobald die Dienstrichtlinie geändert wurde, wird sie von den Agents gelesen und erzwungen.

- Zusätzliche Aktionen zum Konfigurieren des Geräterückschreibens und eine Statusanzeige für die Seiteninitialisierung.

- Verbesserte allgemeine Diagnose mit HTML-Berichten und vollständiger Datenerfassung in einem ZIP-Text- bzw. HTML-Bericht.

- Die Zuverlässigkeit des automatischen Upgrades wurde verbessert, und es wurden zusätzliche Telemetriedaten hinzugefügt, um sicherzustellen, dass die Integrität des Servers ermittelt werden kann.

- Eingeschränkte Berechtigungen für privilegierte Konten im AD Connector-Konto. Bei Neuinstallationen schränkt der Assistent die Berechtigungen ein, über die privilegierte Konten im MSOL-Konto verfügen, nachdem das MSOL-Konto erstellt wurde. Die Änderungen betreffen Express-Installationen und benutzerdefinierte Installationen mit automatischer Kontoerstellung.

- Das Installationsprogramm wurde so geändert, dass bei einer Neuinstallation von AAD Connect keine SA-Berechtigung mehr erforderlich ist.

- Neues Hilfsprogramm, um Synchronisierungsprobleme für ein bestimmtes Objekt zu beheben. Zurzeit überprüft das Hilfsprogramm Folgendes:

    - Diskrepanz beim UserPrincipalName zwischen dem synchronisierten Benutzerobjekt und dem Benutzerkonto im Azure AD-Mandanten.
  
    - Ausschluss des Objekts von der Synchronisierung aufgrund der Domänenfilterung.
  
    - Ausschluss des Objekts von der Synchronisierung aufgrund der Filterung nach Organisationseinheit.

- Neues Hilfsprogramm, um den aktuellen Kennworthash zu synchronisieren, der im lokalen Active Directory-Verzeichnis für ein bestimmtes Benutzerkonto gespeichert ist. Das Hilfsprogramm erfordert keine Kennwortänderung. 

---
 
### <a name="applications-supporting-intune-app-protection-policies-added-for-use-with-azure-ad-application-based-conditional-access"></a>Anwendungen, die die Intune App-Schutzrichtlinien unterstützen, wurden für die Verwendung mit anwendungsbasiertem bedingten Azure AD-Zugriff hinzugefügt

**Typ:** Geändertes Feature  
**Dienstkategorie:** Bedingter Zugriff  
**Produktfunktion:** Identitätssicherheit und -schutz

Wir haben weitere Anwendungen hinzugefügt, die anwendungsbasierten bedingten Zugriff unterstützen. Jetzt können Sie Zugriff auf Office 365 und andere mit Azure AD verbundene Cloudanwendungen erhalten, indem Sie diese genehmigten Client-Apps verwenden.

Die folgenden Anwendungen werden bis Ende Februar hinzugefügt:

- Microsoft Power BI

- Microsoft Launcher

- Microsoft Invoicing

Weitere Informationen finden Sie unter

- [Genehmigte Client-App als Voraussetzung](https://docs.microsoft.com/azure/active-directory/active-directory-conditional-access-technical-reference#approved-client-app-requirement)
- [App-basierter bedingter Zugriff mit Azure AD](https://docs.microsoft.com/azure/active-directory/conditional-access/app-based-conditional-access)

---

### <a name="terms-of-use-update-to-mobile-experience"></a>Update der Nutzungsbedingungen für Mobilgeräte 

**Typ:** Geändertes Feature  
**Dienstkategorie:** Nutzungsbedingungen  
**Produktfunktion:** Compliance

Wenn die Nutzungsbedingungen angezeigt werden, können Sie nun auf Folgendes klicken: **Probleme mit der Anzeige? Klicken Sie hier**. Durch Klicken auf diesen Link werden die Nutzungsbedingungen nativ auf Ihrem Gerät geöffnet. Unabhängig vom Schriftgrad des Dokuments oder der Bildschirmgröße des Geräts können Sie das Dokument bei Bedarf zoomen und lesen. 

---
 
## <a name="january-2018"></a>Januar 2018
 
### <a name="new-federated-apps-available-in-azure-ad-app-gallery"></a>Neue Verbund-Apps im Azure AD-App-Katalog verfügbar 

**Typ:** Neues Feature  
**Dienstkategorie:** Unternehmens-Apps  
**Produktfunktion:** Integration von Drittanbieterprodukten

Im Januar 2018 wurden die folgenden neuen Apps mit Verbundunterstützung im App-Katalog hinzugefügt:

[IBM OpenPages](https://go.microsoft.com/fwlink/?linkid=864698), [OneTrust Privacy Management Software](https://go.microsoft.com/fwlink/?linkid=861660), [Dealpath](https://go.microsoft.com/fwlink/?linkid=863526), IriusRisk Federated Directory und [Fidelity NetBenefits](https://go.microsoft.com/fwlink/?linkid=864701).

Weitere Informationen zu den Apps finden Sie unter [SaaS-Anwendungsintegration mit Azure Active Directory](https://aka.ms/appstutorial).

Weitere Informationen zum Listen Ihrer Anwendung im Azure AD-App-Katalog finden Sie unter [Listen Ihrer Anwendung im Azure Active Directory-Anwendungskatalog](https://docs.microsoft.com/azure/active-directory/develop/active-directory-app-gallery-listing). 

---
 
### <a name="sign-in-with-additional-risk-detected"></a>Anmeldung mit erhöhtem Risiko erkannt

**Typ:** Neues Feature  
**Dienstkategorie:** Schutz der Identität (Identity Protection)  
**Produktfunktion:** Identitätssicherheit und -schutz

Die Erkenntnisse/Informationen, die Sie über eine erkannte Risikoerkennung erhalten, richten sich nach Ihrem Azure AD-Abonnement. Die Azure AD Premium P2-Edition bietet die ausführlichsten Informationen zu allen zugrunde liegenden erkannten Ereignissen.

Bei der Azure AD Premium P1-Edition werden Erkennungen, die nicht durch Ihre Lizenz abgedeckt sind, als Risikoerkennung „Anmeldung mit erhöhtem Risiko erkannt“ angezeigt.

Weitere Informationen finden Sie unter [Azure Active Directory-Risikoereignisse](https://docs.microsoft.com/azure/active-directory/active-directory-reporting-risk-events).
 
---

### <a name="hide-office-365-applications-from-end-users-access-panels"></a>Ausblenden von Office 365-Anwendungen in Zugriffsbereichen von Endbenutzern

**Typ:** Neues Feature  
**Dienstkategorie:** Meine Apps  
**Produktfunktion:** SSO

Durch eine neue Benutzereinstellung können Sie jetzt besser steuern, wie Office 365-Anwendungen in den Zugriffsbereichen Ihrer Benutzer angezeigt werden. Diese Option ist hilfreich, um die Anzahl der Apps in den Zugriffsbereichen eines Benutzers zu reduzieren, sofern nur Office-Apps im Office-Portal anzeigen werden sollen. Die Einstellung befindet sich in den **Benutzereinstellungen** und lautet **Benutzer können Office 365-Apps nur im Office 365-Portal anzeigen**.

Weitere Informationen finden Sie unter [Ausblenden einer Anwendung auf der Benutzeroberfläche in Azure Active Directory](https://docs.microsoft.com/azure/active-directory/active-directory-coreapps-hide-third-party-app).

---
 
### <a name="seamless-sign-into-apps-enabled-for-password-sso-directly-from-apps-url"></a>Nahtloses Anmelden bei Apps mit direkt über die App-URL aktiviertem Kennwort-SSO 

**Typ:** Neues Feature  
**Dienstkategorie:** Meine Apps  
**Produktfunktion:** SSO

Die Browsererweiterung „Meine Apps“ ist jetzt über ein nützliches Tool verfügbar, mit dem Sie die Funktion zum einmaligen Anmelden „Meine Apps“ als Verknüpfung in Ihrem Browser verwenden können. Nach der Installation wird dem Benutzer ein Waffelsymbol im Browser angezeigt, das Schnellzugriff auf Apps bietet. Benutzer können ab sofort von folgenden Vorteilen profitieren:

- Die Möglichkeit, sich direkt bei Apps per Kennwort-SSO über die Anmeldeseite der App anzumelden
- Starten einer App mithilfe der Schnellsuchfunktion
- Verknüpfungen für zuletzt verwendete Apps über die Erweiterung
- Die Erweiterung ist für Microsoft Edge, Chrome und Firefox verfügbar.
 
Weitere Informationen finden Sie unter [Erweiterung zur sicheren Anmeldung bei „Meine Apps“](../user-help/my-apps-portal-end-user-access.md#download-and-install-the-my-apps-secure-sign-in-extension).

---

### <a name="azure-ad-administration-experience-in-azure-classic-portal-has-been-retired"></a>Die Azure AD-Verwaltungsoberfläche im klassischen Azure-Portal wurde eingestellt

**Typ:** Veraltet   
**Dienstkategorie:** Azure AD  
**Produktfunktion:** Verzeichnis

Die Azure AD-Administratoroberfläche im klassischen Azure-Portal ist seit dem 8. Januar 2018 eingestellt. Diese Änderung fand im Zuge der Einstellung des klassischen Azure-Portals selbst statt. Für sämtliche portalbasierte Verwaltungsaufgaben von Azure AD müssen Sie künftig das [Azure AD Admin Center](https://aad.portal.azure.com) verwenden.
 
---

### <a name="the-phonefactor-web-portal-has-been-retired"></a>Das PhoneFactor-Webportal wurde eingestellt

**Typ:** Veraltet  
**Dienstkategorie:** Azure AD  
**Produktfunktion:** Verzeichnis
 
Am 8. Januar 2018 wurde das PhoneFactor-Webportal eingestellt. Die Funktionen des Portals, nämlich die Verwaltung des MFA-Servers, wurden zum Azure-Portal unter portal.azure.com migriert. 

Die MFA-Konfiguration befindet sich unter: **Azure Active Directory \> MFA Server**
 
---
 
### <a name="deprecate-azure-ad-reports"></a>Einstellung von Azure AD-Berichten

**Typ:** Veraltet  
**Dienstkategorie:** Berichterstellung  
**Produktfunktion:** Identity Lifecycle Management  


Im Zuge der allgemeinen Verfügbarkeit der neuen Azure Active Directory-Verwaltungskonsole und der neuen APIs, die ab sofort für Aktivitäts- und Sicherheitsberichte verfügbar sind, wurden die Berichts-APIs auf dem Endpunkt unter „/reports“ am 31. Dezember 2017 eingestellt.

**Was ist verfügbar?**

Im Rahmen der Umstellung auf die neue Verwaltungskonsole haben wir zwei neue APIs zum Abrufen von Azure AD-Aktivitätsprotokollen zur Verfügung gestellt. Die neuen APIs bieten umfangreichere Filter- und Sortierfunktionen sowie mehr Überprüfungs- und Anmeldeaktivitäten. Auf die Daten, die bisher in den Sicherheitsberichten verfügbar waren, kann jetzt über die API für Identity Protection-Risikoerkennungen in Microsoft Graph zugegriffen werden.

Weitere Informationen finden Sie unter

- [Erste Schritte mit der Berichterstellungs-API von Azure Active Directory](https://docs.microsoft.com/azure/active-directory/active-directory-reporting-api-getting-started-azure-portal)

- [Erste Schritte mit Azure Active Directory Identity Protection und Microsoft Graph](https://docs.microsoft.com/azure/active-directory/active-directory-identityprotection-graph-getting-started)

---

## <a name="december-2017"></a>Dezember 2017

### <a name="terms-of-use-in-the-access-panel"></a>Nutzungsbedingungen im Zugriffsbereich

**Typ:** Neues Feature  
**Dienstkategorie:** Nutzungsbedingungen  
**Produktfunktion:** Compliance
 
Ab sofort können Sie die von Ihnen zuvor akzeptierten Nutzungsbedingungen im Zugriffsbereich einsehen.

Folgen Sie diesen Schritten:

1. Navigieren Sie zum [MyApps-Portal](https://myapps.microsoft.com), und melden Sie sich an.

2. Wählen Sie oben rechts Ihren Namen und dann in der Liste **Profil** aus. 

3. Klicken Sie in Ihrem **Profil** auf **Nutzungsbedingungen lesen**. 

4. Nun können Sie sich die akzeptierten Nutzungsbedingungen ansehen. 

Weitere Informationen finden Sie unter [Nutzungsbedingungsfeature (Vorschauversion) für Azure Active Directory](https://docs.microsoft.com/azure/active-directory/conditional-access/terms-of-use).
 
---
 
### <a name="new-azure-ad-sign-in-experience"></a>Neuer Anmeldevorgang für Azure AD

**Typ:** Neues Feature  
**Dienstkategorie:** Azure AD  
**Produktfunktion:** Benutzerauthentifizierung
 
Die Benutzeroberfläche des Azure AD- und Microsoft-Kontoidentitätssystems wurden hinsichtlich eines konsistenten Aussehens und Verhaltens neu gestaltet. Darüber hinaus erfasst die Azure AD-Anmeldeseite zuerst den Benutzernamen, gefolgt von den Anmeldeinformationen auf einem zweiten Bildschirm.

Weitere Informationen finden Sie unter [The new Azure AD Signin Experience is now in Public Preview](https://cloudblogs.microsoft.com/enterprisemobility/2017/08/02/the-new-azure-ad-signin-experience-is-now-in-public-preview/) (Die neue Umgebung für die Azure AD-Anmeldung ist jetzt als Public Preview verfügbar).
 
---
 
### <a name="fewer-sign-in-prompts-a-new-keep-me-signed-in-experience-for-azure-ad-sign-in"></a>Weniger Anmeldeeingabeaufforderungen: Eine neue „Angemeldet bleiben“-Umgebung für die Azure AD-Anmeldung

**Typ:** Neues Feature  
**Dienstkategorie:** Azure AD  
**Produktfunktion:** Benutzerauthentifizierung
 
Das Kontrollkästchen **Angemeldet bleiben** auf der Azure AD-Anmeldeseite wurde durch eine neue Eingabeaufforderung ersetzt, die nach erfolgreicher Authentifizierung angezeigt wird. 

Wenn Sie für diese Eingabeaufforderung **Ja** auswählen, erhalten Sie vom Dienst ein beständiges Aktualisierungstoken. Dies ist das gleiche Verhalten wie beim Aktivieren des Kontrollkästchens **Angemeldet bleiben** in der alten Umgebung. Bei Verbundmandanten wird diese Eingabeaufforderung angezeigt, nachdem Sie sich erfolgreich bei dem Verbunddienst authentifiziert haben.

Weitere Informationen finden Sie unter [Weniger Anmeldeeingabeaufforderungen: Die neue „Angemeldet bleiben“-Umgebung für Azure AD befindet sich in der Vorschauphase](https://cloudblogs.microsoft.com/enterprisemobility/2017/09/19/fewer-login-prompts-the-new-keep-me-signed-in-experience-for-azure-ad-is-in-preview/). 

---

### <a name="add-configuration-to-require-the-terms-of-use-to-be-expanded-prior-to-accepting"></a>Hinzufügen einer Konfiguration, um vor dem Akzeptieren das Erweitern der Nutzungsbedingungen zu erfordern

**Typ:** Neues Feature  
**Dienstkategorie:** Nutzungsbedingungen  
**Produktfunktion:** Compliance
 
Eine Option für Administratoren wurde hinzugefügt, mit der Benutzer aufgefordert werden können, die Nutzungsbedingungen vor dem Akzeptieren zu erweitern.

Wählen Sie **Ein** oder **Aus** aus, um die Erweiterung der Nutzungsbedingungen durch den Benutzer zu erfordern. Bei der Einstellung **Ein** wird vorausgesetzt, dass sich Benutzer diese vor dem Akzeptieren der Nutzungsbedingungen ansehen.

Weitere Informationen finden Sie unter [Nutzungsbedingungsfeature (Vorschauversion) für Azure Active Directory](https://docs.microsoft.com/azure/active-directory/conditional-access/terms-of-use).
 
---

### <a name="scoped-activation-for-eligible-role-assignments"></a>Bereichsbezogene Aktivierung für berechtigte Rollenzuweisungen

**Typ:** Neues Feature  
**Dienstkategorie:** Privileged Identity Management  
**Produktfunktion:** Privileged Identity Management
 
Mit der bereichsbezogenen Aktivierung können Sie berechtigte Rollenzuweisungen für Azure-Ressourcen mit weniger Autonomie aktivieren, als die ursprünglichen Standardwerte für die Zuweisung vorsehen. Beispielsweise wird Ihnen „Besitzer“ für ein Abonnement in Ihrem Mandanten zugewiesen. Mit der bereichsbezogenen Aktivierung können Sie die Besitzerrolle für bis zu fünf innerhalb des Abonnements enthaltene Ressourcen aktivieren (z.B. für Ressourcengruppen und virtuelle Computer). Durch Festlegen des Gültigkeitsbereichs der Aktivierung kann die Wahrscheinlichkeit verringert werden, dass unerwünschte Änderungen an wichtigen Azure-Ressourcen vorgenommen werden.

Weitere Informationen finden Sie unter [Was ist Azure AD Privileged Identity Management?](https://docs.microsoft.com/azure/active-directory/active-directory-privileged-identity-management-configure).
 
---
 
### <a name="new-federated-apps-in-the-azure-ad-app-gallery"></a>Neue Verbund-Apps im Azure AD-App-Katalog

**Typ:** Neues Feature  
**Dienstkategorie:** Unternehmens-Apps  
**Produktfunktion:** Integration von Drittanbieterprodukten

Im Dezember 2017 haben wir diese neuen Apps mit Verbundunterstützung in unseren App-Katalog aufgenommen:

[Accredible](https://go.microsoft.com/fwlink/?linkid=863523), Adobe Experience Manager, [EFI Digital StoreFront](https://go.microsoft.com/fwlink/?linkid=861685), [Communifire](https://go.microsoft.com/fwlink/?linkid=861676) CybSafe, [FactSet](https://go.microsoft.com/fwlink/?linkid=863525), [IMAGE WORKS](https://go.microsoft.com/fwlink/?linkid=863517), [MOBI](https://go.microsoft.com/fwlink/?linkid=863521), [MobileIron Azure AD-Integration](https://go.microsoft.com/fwlink/?linkid=858027), [Reflektive](https://go.microsoft.com/fwlink/?linkid=863518), [SAML SSO for Bamboo by resolution GmbH](https://go.microsoft.com/fwlink/?linkid=863520), [SAML SSO for Bitbucket by resolution GmbH](https://go.microsoft.com/fwlink/?linkid=863519), [Vodeclic](https://go.microsoft.com/fwlink/?linkid=863522), WebHR, Zenegy Azure AD-Integration.

Weitere Informationen zu den Apps finden Sie unter [SaaS-Anwendungsintegration mit Azure Active Directory](https://aka.ms/appstutorial).

Weitere Informationen zum Listen Ihrer Anwendung im Azure AD-App-Katalog finden Sie unter [Listen Ihrer Anwendung im Azure Active Directory-Anwendungskatalog](https://docs.microsoft.com/azure/active-directory/develop/active-directory-app-gallery-listing). 
 
---

### <a name="approval-workflows-for-azure-ad-directory-roles"></a>Genehmigungsworkflows für Azure AD-Verzeichnisrollen

**Typ:** Geändertes Feature  
**Dienstkategorie:** Privileged Identity Management  
**Produktfunktion:** Privileged Identity Management
 
Ein Genehmigungsworkflow für Azure AD-Verzeichnisrollen ist allgemein verfügbar.

Mit dem Genehmigungsworkflow können Administratoren für privilegierte Rollen festlegen, dass berechtigte Rollenmitglieder die Rollenaktivierung anfordern müssen, bevor sie die privilegierte Rolle verwenden können. Es können Genehmigungsverantwortlichkeiten an verschiedene Benutzer und Gruppen delegiert werden. Berechtigte Rollenmitglieder erhalten eine Benachrichtigung, wenn die Genehmigung abgeschlossen und ihre Rolle aktiv ist.

---
 
### <a name="pass-through-authentication-skype-for-business-support"></a>Passthrough-Authentifizierung: Unterstützung von Skype for Business

**Typ:** Geändertes Feature  
**Dienstkategorie:** Authentifizierungen (Anmeldungen)  
**Produktfunktion:** Benutzerauthentifizierung

Die Pass-Through-Authentifizierung unterstützt jetzt Benutzeranmeldungen bei Skype for Business-Clientanwendungen, die die moderne Authentifizierung, einschließlich Online- und Hybridtopologien, unterstützen. 

Weitere Informationen finden Sie unter [Mit moderner Authentifizierung unterstützte Skype for Business-Topologien](https://technet.microsoft.com/library/mt803262.aspx).
 
---

### <a name="updates-to-azure-ad-privileged-identity-management-for-azure-rbac-preview"></a>Updates für die Azure RBAC in Azure AD Privileged Identity Management (Vorschauversion)

**Typ:** Geändertes Feature  
**Dienstkategorie:** Privileged Identity Management  
**Produktfunktion:** Privileged Identity Management
 
Mit der aktualisierten Public Preview von Azure AD Privileged Identity Management (PIM) für die rollenbasierte Zugriffssteuerung in Azure (RBAC) haben Sie nun folgende Möglichkeiten:

* Nicht mehr Verwaltung als nötig
* Erfordern der Genehmigung zum Aktivieren von Ressourcenrollen
* Planen einer zukünftigen Rollenaktivierung, die eine Genehmigung für Azure AD- und Azure RBAC-Rollen erfordert
 
Weitere Informationen finden Sie unter [PIM für Azure-Ressourcen (Vorschauversion)](https://docs.microsoft.com/azure/active-directory/privileged-identity-management/azure-pim-resource-rbac).

---
 
## <a name="november-2017"></a>November 2017
 
### <a name="access-control-service-retirement"></a>Einstellung des Access Control Service

**Typ:** Plan für Änderung  
**Dienstkategorie:** Access Control Service  
**Produktfunktion:** Access Control Service 

Der Azure Active Directory Access Control (auch als Access Control Service bezeichnet) wird Ende 2018 eingestellt. Weitere Informationen, u.a. ein ausführlicher Zeitplan und allgemeine Migrationsanweisungen, werden in den nächsten Wochen bereitgestellt. Auf dieser Seite können Sie uns Ihre Kommentare oder Fragen zum Access Control Service mitteilen. Ein Teammitglied wird diese dann beantworten.

---

### <a name="restrict-browser-access-to-the-intune-managed-browser"></a>Beschränkung des Browserzugriffs auf Intune Managed Browser 

**Typ:** Plan für Änderung  
**Dienstkategorie:** Bedingter Zugriff  
**Produktfunktion:** Identitätssicherheit und -schutz

Sie können den Browserzugriff auf Office 365 und weitere mit Azure AD verbundene Cloud-Apps mithilfe von Intune Managed Browser als genehmigte App einschränken. 

Ab sofort können Sie folgende Bedingung für den anwendungsbasierten bedingten Zugriff konfigurieren:

**Client-Apps:** Browser

**Welche Auswirkung hat die Änderung?**

Der Zugriff wird blockiert, wenn Sie diese Bedingung verwenden. Sobald die Vorschauversion verfügbar ist, ist für den gesamten Zugriff die Verwendung der Managed Browser-Anwendung erforderlich. 

Achten Sie in kommenden Blogbeiträgen und Anmerkungen zu dieser Version auf Informationen zu dieser Funktionalität. 

Weitere Informationen finden Sie unter [Bedingter Zugriff in Azure Active Directory](https://docs.microsoft.com/azure/active-directory/active-directory-conditional-access-azure-portal).
 
---

### <a name="new-approved-client-apps-for-azure-ad-app-based-conditional-access"></a>Neue genehmigte Client-Apps für den app-basierten bedingten Zugriff mit Azure AD

**Typ:** Plan für Änderung  
**Dienstkategorie:** Bedingter Zugriff  
**Produktfunktion:** Identitätssicherheit und -schutz

Die folgenden Apps wurden der Liste der [genehmigten Client-Apps](https://docs.microsoft.com/azure/active-directory/active-directory-conditional-access-technical-reference#approved-client-app-requirement) hinzugefügt:

- [Microsoft Kaizala](https://www.microsoft.com/garage/profiles/kaizala/)
- Microsoft StaffHub

Weitere Informationen finden Sie unter

- [Genehmigte Client-App als Voraussetzung](https://docs.microsoft.com/azure/active-directory/active-directory-conditional-access-technical-reference#approved-client-app-requirement)
- [App-basierter bedingter Zugriff mit Azure AD](https://docs.microsoft.com/azure/active-directory/conditional-access/app-based-conditional-access)

---

### <a name="terms-of-use-support-for-multiple-languages"></a>Unterstützung von Nutzungsbedingungen für mehrere Sprachen

**Typ:** Neues Feature    
**Dienstkategorie:** Nutzungsbedingungen  
**Produktfunktion:** Compliance

Administratoren können jetzt neue Nutzungsbedingungen erstellen, die mehrere PDF-Dokumente enthalten. Diese PDF-Dateien können mit einer zugehörigen Sprache gekennzeichnet werden. Benutzern wird dann die PDF-Datei angezeigt, die der Sprache in den Einstellungen entspricht. Falls keine Übereinstimmung vorliegt, wird die Standardsprache angezeigt.

---
 
### <a name="real-time-password-writeback-client-status"></a>Echtzeitclientstatus beim Kennwortrückschreiben

**Typ:** Neues Feature  
**Dienstkategorie:** Self-Service-Kennwortzurücksetzung  
**Produktfunktion:** Benutzerauthentifizierung

Nun können Sie den Status Ihres lokalen Clients für das Kennwortrückschreiben anzeigen. Diese Option steht im Abschnitt **Lokale Integration** auf der Seite [Kennwortzurücksetzung](https://portal.azure.com/#blade/Microsoft_AAD_IAM/ActiveDirectoryMenuBlade/PasswordReset) zur Verfügung. 

Wenn Verbindungsprobleme bei Ihrem lokalen Client für das Kennwortrückschreiben vorliegen, wird eine Fehlermeldung mit folgenden Informationen angezeigt:

- Informationen dazu, warum keine Verbindung mit dem lokalen Client für das Kennwortrückschreiben möglich ist
- Ein Link zur Dokumentation, die Sie bei der Problembehandlung unterstützt 

Weitere Informationen finden Sie unter [Lokale Integration](https://docs.microsoft.com/azure/active-directory/active-directory-passwords-how-it-works#on-premises-integration).

---

### <a name="azure-ad-app-based-conditional-access"></a>App-basierter bedingter Zugriff mit Azure AD 
 
**Typ:** Neues Feature  
**Dienstkategorie:** Azure AD  
**Produktfunktion:** Identitätssicherheit und -schutz

Sie können jetzt über den [App-basierten bedingten Zugriff mit Azure AD](https://docs.microsoft.com/azure/active-directory/conditional-access/app-based-conditional-access) den Zugriff auf Office 365 und weitere mit Azure AD verbundene Cloud-Apps auf [genehmigte Client-Apps](https://docs.microsoft.com/azure/active-directory/active-directory-conditional-access-technical-reference#approved-client-app-requirement) beschränken, die Intune-App-Schutzrichtlinien unterstützen. Intune-App-Schutzrichtlinien werden verwendet, um Unternehmensdaten in diesen Clientanwendungen zu konfigurieren und zu schützen.

Durch das Kombinieren von [app-basierten](https://docs.microsoft.com/azure/active-directory/conditional-access/app-based-conditional-access) und [gerätebasierten](https://docs.microsoft.com/azure/active-directory/active-directory-conditional-access-policy-connected-applications) Richtlinien für den bedingten Zugriff können Sie Daten für private Geräte und Unternehmensgeräte flexibel schützen.

Für den app-basierten bedingten Zugriff stehen ab sofort die folgenden Bedingungen und Steuerelemente zur Verfügung:

**Bedingung für unterstützte Plattform**

- iOS
- Android

**Client-Apps-Bedingung**

- Mobile Apps und Desktop-Apps

**Zugriffssteuerung**

- Genehmigte Client-App erforderlich

Weitere Informationen finden Sie unter [App-basierter bedingter Zugriff mit Azure Active Directory](https://docs.microsoft.com/azure/active-directory/conditional-access/app-based-conditional-access).
 
---

### <a name="manage-azure-ad-devices-in-the-azure-portal"></a>Verwalten von Azure AD-Geräten im Azure-Portal

**Typ:** Neues Feature  
**Dienstkategorie:** Geräteregistrierung und -verwaltung  
**Produktfunktion:** Identitätssicherheit und -schutz

Ab jetzt finden Sie alle mit Azure AD verbundenen Geräte sowie die gerätebezogenen Aktivitäten an einem Ort. Es steht eine neue Verwaltungsoberfläche zur Verfügung, mit der Sie all ihre Geräteidentitäten und -einstellungen im Azure-Portal verwalten können. Dieses Release ermöglicht Folgendes:

- Anzeigen all Ihrer Geräte, die für den bedingten Zugriff in Azure AD zur Verfügung stehen.
- Anzeigen von Eigenschaften, einschließlich in Hybrid-Azure AD eingebundenen Geräten
- Auffinden von BitLocker-Schlüsseln für Ihre in Azure AD eingebundenen Geräte, Verwalten Ihrer Geräte mit Intune und vieles mehr
- Verwalten von gerätebezogenen Azure AD-Einstellungen

Weitere Informationen finden Sie unter [Verwalten von Geräten mit dem Azure-Portal](https://docs.microsoft.com/azure/active-directory/device-management-azure-portal).

---

### <a name="support-for-macos-as-a-device-platform-for-azure-ad-conditional-access"></a>Unterstützung der macOS-Geräteplattform für den bedingten Azure AD-Zugriff 

**Typ:** Neues Feature    
**Dienstkategorie:** Bedingter Zugriff  
**Produktfunktion:** Identitätssicherheit und -schutz 

Ab sofort können Sie macOS als Geräteplattformbedingung in Ihre Richtlinie für den bedingten Azure AD-Zugriff einschließen (oder ausschließen). Durch das Hinzufügen von macOS als unterstützte Geräteplattform haben Sie folgende Möglichkeiten:

- **Registrieren und Verwalten von macOS-Geräten mit Intune**: Ähnlich wie für andere Plattformen (z.B. iOS und Android) steht für macOS eine Unternehmensportalanwendung bereit, um einheitliche Registrierungen durchzuführen. Sie können ein Gerät mithilfe der neuen Unternehmensportal-App für macOS bei Intune und Azure AD registrieren.
- **Sicherstellen, dass macOS-Geräte die in Intune definierten Konformitätsrichtlinien Ihrer Organisation einhalten**: In Intune im Azure-Portal können Sie nun Konformitätsrichtlinien für macOS-Geräte einrichten. 
- **Beschränken des Zugriffs auf Anwendungen in Azure AD auf ausschließlich konforme macOS-Geräte**: Für die Erstellung bedingter Zugriffsrichtlinien ist macOS als separate Geräteplattformoption festgelegt. Nun können Sie macOS-spezifische Richtlinien für den bedingten Zugriff für die festgelegte Zielanwendung in Azure erstellen.

Weitere Informationen finden Sie unter

- [Erstellen einer Gerätekonformitätsrichtlinie für macOS-Geräte mit Intune](https://aka.ms/macoscompliancepolicy)
- [Was ist der bedingte Zugriff in Azure Active Directory?](https://docs.microsoft.com/azure/active-directory/active-directory-conditional-access-azure-portal)
 
---

### <a name="network-policy-server-extension-for-azure-multi-factor-authentication"></a>Erweiterung „Netzwerkrichtlinienserver“ für Azure Multi-Factor Authentication 

**Typ:** Neues Feature    
**Dienstkategorie:**  Multi-Factor Authentication  
**Produktfunktion:** Benutzerauthentifizierung

Die Erweiterung „Netzwerkrichtlinienserver“ für Azure Multi-Factor Authentication fügt Ihrer Authentifizierungsinfrastruktur unter Verwendung Ihrer vorhandenen Server cloudbasierte Funktionen zur mehrstufigen Authentifizierung hinzu. Mit der Erweiterung „Netzwerkrichtlinienserver“ können Sie zu Ihrem bestehenden Authentifizierungsablauf eine Überprüfung per Telefonanruf, SMS oder Telefon-App hinzufügen. Dabei müssen keine neuen Server installiert, konfiguriert und verwaltet werden. 

Diese Erweiterung wurde für Organisationen entwickelt, die VPN-Verbindungen schützen möchten, ohne den Azure Multi-Factor Authentication-Server bereitzustellen. Die Erweiterung „Netzwerkrichtlinienserver“ fungiert als Adapter zwischen RADIUS und cloudbasierter Azure Multi-Factor Authentication, um eine zweite Stufe der Authentifizierung für Verbund- oder synchronisierte Benutzer bereitzustellen.

Weitere Informationen finden Sie unter [Integrieren Ihrer vorhandenen NPS-Infrastruktur in Azure Multi-Factor Authentication](https://docs.microsoft.com/azure/multi-factor-authentication/multi-factor-authentication-nps-extension).
 
---

### <a name="restore-or-permanently-remove-deleted-users"></a>Wiederherstellen oder dauerhaftes Entfernen gelöschter Benutzer

**Typ:** Neues Feature    
**Dienstkategorie:** Benutzerverwaltung  
**Produktfunktion:** Verzeichnis 

Im Azure AD Admin Center haben Sie jetzt folgende Möglichkeiten:

- Wiederherstellen eines gelöschten Benutzers 
- Dauerhaftes Löschen eines Benutzers

**So können Sie es ausprobieren**

1. Wählen Sie im Azure AD Admin Center im Abschnitt **Verwalten** die Option [Alle Benutzer](https://aad.portal.azure.com/#blade/Microsoft_AAD_IAM/UserManagementMenuBlade/All) aus. 

2. Wählen Sie in der Liste **Anzeigen** die Option **Kürzlich gelöschte Benutzer** aus. 

3. Wählen Sie mindestens einen kürzlich gelöschten Benutzer aus, und stellen Sie den Benutzer dann entweder wieder her, oder löschen Sie ihn dauerhaft.
 
---

### <a name="new-approved-client-apps-for-azure-ad-app-based-conditional-access"></a>Neue genehmigte Client-Apps für den app-basierten bedingten Zugriff mit Azure AD
 
**Typ:** Geändertes Feature  
**Dienstkategorie:** Bedingter Zugriff  
**Produktfunktion:** Identitätssicherheit und -schutz

Die folgenden Apps wurden der Liste der [genehmigten Client-Apps](https://docs.microsoft.com/azure/active-directory/active-directory-conditional-access-technical-reference#approved-client-app-requirement) hinzugefügt:

- Microsoft Planner
- Azure Information Protection 

Weitere Informationen finden Sie unter

- [Genehmigte Client-App als Voraussetzung](https://docs.microsoft.com/azure/active-directory/active-directory-conditional-access-technical-reference#approved-client-app-requirement)
- [App-basierter bedingter Zugriff mit Azure AD](https://docs.microsoft.com/azure/active-directory/conditional-access/app-based-conditional-access)

---

### <a name="use-or-between-controls-in-a-conditional-access-policy"></a>Einfügen von „OR“ zwischen Steuerelementen in einer Richtlinie für den bedingten Zugriff 

**Typ:** Geändertes Feature    
**Dienstkategorie:** Bedingter Zugriff  
**Produktfunktion:** Identitätssicherheit und -schutz
 
Ab sofort können Sie „OR“ (eines der ausgewählten Steuerelemente ist erforderlich) zwischen Steuerelementen für den bedingten Zugriff einfügen. Durch dieses Feature können Sie Richtlinien mit einem „OR“ zwischen Steuerelementen für den Zugriff erstellen. Beispielsweise können Sie mit diesem Feature eine Richtlinie erstellen, sodass der Benutzer sich entweder über die Multi-Factor Authentication anmelden „OR“ (oder) ein konformes Gerät verwenden muss.

Weitere Informationen finden Sie unter [Steuerelemente beim bedingten Zugriff mit Azure Active Directory](https://docs.microsoft.com/azure/active-directory/active-directory-conditional-access-controls).
 
---

### <a name="aggregation-of-real-time-risk-detections"></a>Aggregation von Echtzeitrisikoerkennungen

**Typ:** Geändertes Feature    
**Dienstkategorie:** Schutz der Identität (Identity Protection)  
**Produktfunktion:** Identitätssicherheit und -schutz

In Azure AD Identity Protection werden jetzt alle Echtzeitrisikoerkennungen, die von derselben IP-Adresse stammen, für einen vorgegebenen Tag für jeden Risikoerkennungstyp aggregiert. Durch diese Änderung werden weniger Risikoerkennungen ohne Änderung der Benutzersicherheit angezeigt.

Die zugrunde liegende Echtzeiterkennung erfolgt bei jeder Benutzeranmeldung. Wenn Sie eine Sicherheitsrichtlinie für das Anmelderisiko auf Multi-Factor Authentication oder eine Blockierung des Zugriffs festgelegt haben, wird diese bei jeder riskanten Anmeldung weiterhin ausgelöst.
 
---
 
## <a name="october-2017"></a>Oktober 2017

### <a name="deprecate-azure-ad-reports"></a>Einstellung von Azure AD-Berichten

**Typ:** Plan für Änderung  
**Dienstkategorie:** Berichterstellung  
**Produktfunktion:** Identity Lifecycle Management  

Azure-Portal bietet Ihnen Folgendes:

- Eine neue Azure AD-Verwaltungskonsole
- Neue APIs für Aktivitäts- und Sicherheitsberichte
 
Aufgrund dieser neuen Funktionen werden die Berichts-APIs auf dem Endpunkt unter „/reports“ am 10. Dezember 2017 eingestellt. 

---

### <a name="automatic-sign-in-field-detection"></a>Automatische Erkennung von Anmeldefeldern

**Typ:** Korrigiert   
**Dienstkategorie:** Meine Apps  
**Produktfunktion:** Einmaliges Anmelden  

Azure AD unterstützt die automatische Erkennung von Anmeldefeldern für Anwendungen, die ein HTML-Benutzernamen- und Kennwortfeld rendern. Diese Schritte sind unter [So erfassen Sie Anmeldefelder für eine Anwendung automatisch](https://docs.microsoft.com/azure/active-directory/manage-apps/configure-password-single-sign-on-non-gallery-applications-problems#manually-capture-sign-in-fields-for-an-app) dokumentiert. Sie finden diese Funktion, indem Sie im [Azure-Portal](https://aad.portal.azure.com) auf der Seite **Unternehmensanwendungen** eine *Nicht-Katalog*-Anwendung hinzufügen. Zusätzlich können Sie den Modus **Einmaliges Anmelden** in dieser neuen Anwendung auf **Kennwortbasiertes einmaliges Anmelden** festlegen, indem Sie eine Web-URL eingeben und die Seite anschließend speichern.
 
Aufgrund eines Dienstproblems war diese Funktion vorübergehend deaktiviert. Das Problem wurde behoben, und die automatische Erkennung des Anmeldefelds ist nun wieder verfügbar.

---

### <a name="new-multi-factor-authentication-features"></a>Neue Multi-Factor Authentication-Features

**Typ:** Neues Feature  
**Dienstkategorie:** Multi-Factor Authentication  
**Produktfunktion:** Identitätssicherheit und -schutz  

Die mehrstufige Authentifizierung (Multi-Factor Authentication, MFA) ist ein wesentlicher Bestandteil des Schutzes Ihrer Organisation. Um die Anmeldeinformationen anpassungsfähiger und das Benutzererlebnis nahtloser zu gestalten, wurden die folgenden Features hinzugefügt: 

- Die Captcha-Ergebnisse bei der mehrstufigen Authentifizierung sind unmittelbar in den Azure AD-Anmeldebericht integriert, einschließlich des programmgesteuerten Zugriffs auf MFA-Ergebnisse.
- Die MFA-Konfiguration wird stärker in die zentrale Azure AD-Konfigurationsumgebung im Azure-Portal integriert.

Mit dieser öffentlichen Vorschau werden MFA-Verwaltung und -Berichterstellung zu einem integrierten Bestandteil der zentralen Azure AD-Konfigurationsumgebung. Nun können Sie die MFA-Verwaltungsportalfunktionalität in der Azure AD-Oberfläche verwalten.

Weitere Informationen finden Sie in der [Referenz zur Berichterstellung für die mehrstufige Authentifizierung im Azure-Portal](https://docs.microsoft.com/azure/active-directory/active-directory-reporting-activity-sign-ins-mfa). 

---

### <a name="terms-of-use"></a>Nutzungsbedingungen

**Typ:** Neues Feature  
**Dienstkategorie:** Nutzungsbedingungen  
**Produktfunktion:** Compliance  

Mithilfe der Azure AD-Nutzungsbedingungen können Sie Benutzern Informationen wie relevante Haftungsausschlüsse zur Erfüllung gesetzlicher oder Complianceanforderungen zur Verfügung stellen.

Sie können Azure AD-Nutzungsbedingungen in den folgenden Szenarien verwenden:

- Allgemeine Nutzungsbedingungen für alle Benutzer in Ihrer Organisation
- Spezifische Nutzungsbedingungen basierend auf Benutzerattributen (z.B. Ärzte im Vergleich zu Krankenschwestern oder einheimische Mitarbeiter im Vergleich zu ausländischen Mitarbeitern) von dynamischen Gruppen
- Spezifische Nutzungsbedingungen für den Zugriff auf bedeutende Geschäftsanwendungen wie Salesforce

Weitere Informationen finden Sie im [Azure AD B2C](https://docs.microsoft.com/azure/active-directory/conditional-access/terms-of-use).

---

### <a name="enhancements-to-privileged-identity-management"></a>Verbesserungen an Privileged Identity Management

**Typ:** Neues Feature  
**Dienstkategorie:** Privileged Identity Management  
**Produktfunktion:** Privileged Identity Management  

Mit Azure AD Privileged Identity Management können Sie den Zugriff auf Azure-Ressourcen (Vorschauversion) für folgende Elemente innerhalb Ihrer Organisation verwalten, steuern und überwachen:

- Abonnements
- Ressourcengruppen
- Virtuelle Computer 

Alle Ressourcen im Azure-Portal, die die Azure RBAC-Funktion nutzen, können von sämtlichen Sicherheits- und Lebenszyklusverwaltungsfunktionen profitieren, die Azure AD Privileged Identity Management bereitstellt.

Weitere Informationen finden Sie unter [Privileged Identity Management für Azure-Ressourcen](https://docs.microsoft.com/azure/active-directory/privileged-identity-management/azure-pim-resource-rbac).

---

### <a name="access-reviews"></a>Zugriffsüberprüfungen

**Typ:** Neues Feature  
**Dienstkategorie:** Zugriffsüberprüfungen  
**Produktfunktion:** Compliance  

Organisationen können mithilfe von Zugriffsüberprüfungen (in der Vorschauversion) effizient Gruppenmitgliedschaften und den Zugriff auf Unternehmensanwendungen verwalten: 

- Sie können den Gastbenutzerzugriff erneut bestätigen, indem Sie den Zugriff auf Anwendungen und Gruppenmitgliedschaften im Rahmen einer Zugriffsüberprüfung überprüfen. Anhand der Erkenntnisse aus den Zugriffsüberprüfungen können Prüfer effizient entscheiden, ob Gäste weiterhin Zugriff haben sollen.
- Mithilfe von Zugriffsüberprüfungen können Sie den Anwendungszugriff von Mitarbeitern sowie deren Gruppenmitgliedschaften neu zertifizieren.

Die Kontrollen der Zugriffsüberprüfung können in unternehmensrelevanten Programmen zusammengefasst werden, um Überprüfungen für compliance- oder risikokritische Anwendungen nachzuverfolgen.

Weitere Informationen finden Sie unter [Azure AD-Zugriffsüberprüfungen](https://docs.microsoft.com/azure/active-directory/active-directory-azure-ad-controls-access-reviews-overview).

---

### <a name="hide-third-party-applications-from-my-apps-and-the-office-365-app-launcher"></a>Möglichkeit zum Ausblenden von Drittanbieteranwendungen in „Meine Apps“ und im Office 365-App-Startprogramm

**Typ:** Neues Feature  
**Dienstkategorie:** Meine Apps  
**Produktfunktion:** Einmaliges Anmelden  

Mit der neuen Eigenschaft **App ausblenden** können Sie nun Apps, die in Ihren Benutzerportalen angezeigt werden, besser verwalten. Sie können Apps ausblenden für den Fall, dass App-Kacheln für Back-End-Dienste angezeigt werden oder Kacheln doppelt vorhanden sind und die App-Startprogramme von Benutzern überladen. Die Umschaltfläche befindet sich im Abschnitt **Eigenschaften** der Drittanbieter-App und hat die Bezeichnung **Für Benutzer sichtbar?** . Sie können eine App auch programmgesteuert über PowerShell ausblenden. 

Weitere Informationen finden Sie unter [Ausblenden einer Anwendung auf der Benutzeroberfläche in Azure Active Directory](https://docs.microsoft.com/azure/active-directory/active-directory-coreapps-hide-third-party-app). 


**Was ist verfügbar?**

 Im Rahmen der Umstellung auf die neue Verwaltungskonsole stehen zwei neue APIs zum Abrufen von Azure AD-Aktivitätsprotokollen zur Verfügung. Die neuen APIs bieten umfangreichere Filter- und Sortierfunktionen sowie mehr Überprüfungs- und Anmeldeaktivitäten. Auf die Daten, die bisher in den Sicherheitsberichten verfügbar waren, kann jetzt über die API für Identity Protection-Risikoerkennungen in Microsoft Graph zugegriffen werden.


## <a name="september-2017"></a>September 2017

### <a name="hotfix-for-identity-manager"></a>Hotfix für Identity Manager

**Typ:** Geändertes Feature  
**Dienstkategorie:** Identity Manager  
**Produktfunktion:** Identity Lifecycle Management  

Ein Hotfixrollup-Paket (Build 4.4.1642.0) ist ab dem 25. September 2017 für Identity Manager 2016 Service Pack 1 verfügbar. Dieser Rolluppaket enthält Folgendes:

- Problembehebungen und Verbesserungen
- Ein kumulatives Update, das alle Identity Manager 2016 Service Pack 1-Updates bis Build 4.4.1459.0 für Identity Manager 2016 ersetzt 
- Identity Manager 2016 Build 4.4.1302.0 wird vorausgesetzt. 

Weitere Informationen finden Sie unter [Hotfixrollup-Paket (Build 4.4.1642.0) ist für Identity Manager 2016 Service Pack 1 verfügbar](https://support.microsoft.com/help/4021562). 

---
