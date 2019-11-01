---
title: Was ist Azure Active Directory? – Azure Active Directory | Microsoft-Dokumentation
description: Hier finden Sie eine Übersicht und konzeptionelle Informationen zu Azure Active Directory, u. a. Terminologie, Informationen zu verfügbaren Lizenzen und eine Liste der zugehörigen Features mit Links zu weiteren Informationen.
services: active-directory
author: msaburnley
manager: daveba
ms.service: active-directory
ms.topic: overview
ms.date: 07/31/2019
ms.author: ajburnle
ms.custom: it-pro, seodec18, seo-update-azuread-jan
ms.collection: M365-identity-device-management
ms.openlocfilehash: 8e0e028be6b4a507ba97f9126c791ee77c764c70
ms.sourcegitcommit: 7efb2a638153c22c93a5053c3c6db8b15d072949
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 10/24/2019
ms.locfileid: "72880497"
---
# <a name="what-is-azure-active-directory"></a>Was ist Azure Active Directory?

Azure Active Directory (Azure AD) ist der cloudbasierte Identitäts- und Zugriffsverwaltungsdienst von Microsoft, mit dem sich Ihre Mitarbeiter anmelden und auf die folgenden Ressourcen zugreifen können:

- Externe Ressourcen (beispielsweise Microsoft Office 365, das Azure-Portal und tausende andere SaaS-Anwendungen)

- Interne Ressourcen (beispielsweise Apps im Netzwerk/Intranet Ihres Unternehmens oder selbst entwickelte Cloud-Apps Ihrer Organisation)

Anhand der verschiedenen Poster aus der Reihe [Microsoft-Cloud-Identität für Enterprise-Architekten](https://docs.microsoft.com/office365/enterprise/microsoft-cloud-it-architecture-resources#identity) können Sie sich mit den zentralen Identitätsdiensten in Azure, Azure AD und Office 365 vertraut machen.

## <a name="who-uses-azure-ad"></a>Azure AD-Zielgruppe

Azure AD ist für folgende Benutzer konzipiert:

- **IT-Administratoren.** Als IT-Administrator können Sie mit Azure AD den Zugriff auf Ihre Apps und App-Ressourcen steuern, um die Anforderungen Ihres Unternehmens zu erfüllen. So können Sie mit Azure AD beispielsweise beim Zugriff auf wichtige Organisationsressourcen eine mehrstufige Authentifizierung erzwingen. Darüber hinaus können Sie mit Azure AD die Benutzerbereitstellung zwischen Ihrem vorhandenen Windows Server-AD und Ihren Cloud-Apps (einschließlich Office 365) automatisieren. Azure AD bietet außerdem leistungsfähige Tools zum automatischen Schutz von Benutzeridentitäten und Anmeldeinformationen, um Ihre Anforderungen in puncto Zugriffssteuerung zu erfüllen. Registrieren Sie sich für eine [kostenlose 30-tägige Azure Active Directory Premium-Testversion](https://azure.microsoft.com/trial/get-started-active-directory/), um die Lösung kennenzulernen.

- **App-Entwickler:** Dank eines standardbasierten Ansatzes können Sie als App-Entwickler ihre App mithilfe von Azure AD mit einmaligem Anmelden (Single Sign-On, SSO) ausstatten und es Benutzern so ermöglichen, ihre bereits vorhandenen Anmeldeinformationen zu verwenden. Azure AD stellt außerdem APIs bereit, die Sie bei der Entwicklung personalisierter App-Umgebungen unter Verwendung vorhandener Organisationsdaten unterstützen. Registrieren Sie sich für eine [kostenlose 30-tägige Azure Active Directory Premium-Testversion](https://azure.microsoft.com/trial/get-started-active-directory/), um die Lösung kennenzulernen. Weitere Informationen finden Sie auch unter [Azure Active Directory für Entwickler](../develop/index.yml).

- **Abonnenten von Microsoft 365, Office 365, Azure oder Dynamics CRM Online:** Als Abonnent verwenden Sie bereits Azure AD. Jeder Mandant von Microsoft 365, Office 365, Azure oder Dynamics CRM Online ist automatisch auch ein Azure AD-Mandant. Sie können sofort mit der Verwaltung des Zugriffs auf Ihre integrierten Cloud-Apps beginnen.

## <a name="what-are-the-azure-ad-licenses"></a>Was sind Azure AD-Lizenzen?

Microsoft Online-Unternehmensdienste wie Office 365 und Microsoft Azure benötigen Azure AD für die Anmeldung sowie zum Schutz von Identitäten. Wenn Sie einen Microsoft Online-Unternehmensdienst abonnieren, erhalten Sie automatisch Azure AD und damit Zugriff auf alle kostenlosen Features.

Zur Erweiterung Ihrer Azure AD-Implementierung können Sie auch kostenpflichtige Funktionen hinzufügen, indem Sie auf Azure Active Directory-Lizenzen vom Typ „Premium P1“ oder „Premium P2“ upgraden. Die kostenpflichtigen Azure AD-Lizenzen ergänzen Ihr kostenloses Verzeichnis und bieten Self-Service-Funktionen, eine erweiterte Überwachung, Sicherheitsberichte sowie sicheren Zugriff für Ihre mobilen Benutzer.

>[!Note]
>Informationen zu den Preisoptionen für diese Lizenzen finden Sie unter [Azure Active Directory – Preise](https://azure.microsoft.com/pricing/details/active-directory/).
>
>Azure Active Directory Premium P1 und Premium P2 werden derzeit in China nicht unterstützt. Weitere Informationen zu den Preisen von Azure AD erhalten Sie im [Azure Active Directory-Forum](https://azure.microsoft.com/support/community/?product=active-directory).

- **Azure Active Directory Free:** Bietet Benutzer- und Gruppenverwaltung, lokale Verzeichnissynchronisierung, einfache Berichte, Self-Service-Kennwortänderung für Cloudbenutzer sowie einmaliges Anmelden für Azure, Office 365 und zahlreiche beliebte SaaS-Apps.

- **Azure Active Directory Premium P1:** P1 bietet zusätzlich zu den Features des Free-Tarifs Hybridbenutzerzugriff auf lokale und cloudbasierte Ressourcen. P1 unterstützt außerdem eine erweiterte Verwaltung – etwa durch dynamische Gruppen, Self-Service-Gruppenverwaltung, Microsoft Identity Manager (eine lokale Identitäts- und Zugriffsverwaltungssuite) und Cloudfunktionen für das Zurückschreiben, die die Self-Service-Kennwortzurücksetzung für Ihre lokalen Benutzer ermöglichen.

- **Azure Active Directory Premium P2:** P2 bietet zusätzlich zu den Features des Free- und des P1-Tarifs auch [Azure Active Directory Identity Protection](../identity-protection/overview-identity-protection.md), um risikobasierten bedingten Zugriff auf Ihre Apps und kritischen Unternehmensdaten zu ermöglichen, sowie [Privileged Identity Management](../privileged-identity-management/pim-getting-started.md), um Administratoren und deren Zugriff auf Ressourcen zu ermitteln, einzuschränken und zu überwachen und bei Bedarf Just-In-Time-Zugriff bereitzustellen.

- **Featurelizenzen mit nutzungsbasierter Bezahlung:** Es sind auch zusätzliche Featurelizenzen verfügbar (beispielsweise Azure Active Directory B2C). B2C kann Ihnen dabei helfen, Identitäts- und Zugriffsverwaltungslösungen für Ihre kundenorientierten Apps bereitzustellen. Weitere Informationen finden Sie in der [Dokumentation für Azure Active Directory B2C](../../active-directory-b2c/index.yml).

Weitere Informationen zum Zuordnen eines Azure-Abonnements zu Azure AD finden Sie unter [ Zuweisen oder Hinzufügen eines Azure-Abonnements zu Azure Active Directory](active-directory-how-subscriptions-associated-directory.md) und weitere Informationen zum Zuweisen von Lizenzen zu Ihren Benutzern unter [ Zuweisen oder Entfernen von Azure Active Directory-Lizenzen](license-users-groups.md).

## <a name="terminology"></a>Begriff

Zum besseren Verständnis von Azure AD und der dazugehörigen Dokumentation sollten Sie sich mit den folgenden Begriffen vertraut machen:

|Begriff oder Konzept|BESCHREIBUNG|
|---------------|-----------|
|Identity| Etwas, das authentifiziert werden kann. Eine Identität kann ein Benutzer mit Benutzername und Kennwort sein. Zu Identitäten gehören auch Anwendungen oder andere Server, die eine Authentifizierung durch geheime Schlüssel oder Zertifikate erfordern können.|
|Konto| Eine Identität, der Daten zugeordnet sind. Ohne Identität können Sie nicht über ein Konto verfügen.|
|Azure AD-Konto| Eine über Azure AD oder einen anderen Microsoft-Clouddienst (beispielsweise Office 365) erstellte Identität. Identitäten werden in Azure AD gespeichert und sind für die Clouddienstabonnements Ihrer Organisation zugänglich. Dieses Konto wird manchmal auch als Geschäfts-, Schul- oder Unikonto bezeichnet.|
|Azure-Abonnement| Dient zur Bezahlung für Azure-Clouddienste. Sie können über mehrere Abonnements verfügen, und die Abonnements sind mit einer Kreditkarte verknüpft.|
|Azure-Mandant| Eine dedizierte, vertrauenswürdige Instanz von Azure AD, die automatisch erstellt wird, wenn sich Ihre Organisation für ein Abonnement eines Microsoft-Clouddiensts wie Microsoft Azure, Microsoft Intune oder Office 365 registriert. Ein Azure-Mandant stellt eine einzelne Organisation dar.|
|Einzelner Mandant| Azure-Mandanten, die auf andere Dienste in einer dedizierten Umgebung zugreifen, werden als einzelne Mandanten betrachtet.|
|Mehrinstanzenfähig| Azure-Mandanten, die auf andere Dienste in einer gemeinsam genutzten Umgebung mit mehreren Organisationen zugreifen, werden als mehrinstanzenfähig betrachtet.|
|Azure AD-Verzeichnis|Jeder Azure-Mandant verfügt über ein dediziertes und vertrauenswürdiges Azure AD-Verzeichnis. Das Azure AD-Verzeichnis umfasst die Benutzer, Gruppen und Apps des Mandanten und dient zum Ausführen von Identitäts- und Zugriffsverwaltungsfunktionen für Mandantenressourcen.|
|Benutzerdefinierte Domäne|Jedes neue Azure AD-Verzeichnis verfügt über einen anfänglichen Domänennamen im Format „<Domänenname>.onmicrosoft.com“. Neben diesem anfänglichen Namen können Sie der Liste die Domänenamen Ihrer Organisation hinzufügen – einschließlich der Namen, die Sie für Ihre geschäftliche Tätigkeit nutzen und die von Benutzern für den Zugriff auf Ihre Organisationsressourcen verwendet werden. Durch Hinzufügen benutzerdefinierter Domänennamen können Sie Benutzernamen erstellen, mit denen Ihre Benutzer vertraut sind (beispielsweise alain@contoso.com).|
|Kontoadministrator|Diese klassische Abonnementadministratorrolle ist konzeptionell der Abrechnungsbesitzer eines Abonnements. Sie hat Zugriff auf das [Azure-Kontocenter](https://account.azure.com/Subscriptions) und ermöglicht die Verwaltung sämtlicher Abonnements in einem Konto. Weitere Informationen finden Sie unter [Administratorrollen für klassische Abonnements, Azure RBAC-Rollen und Azure AD-Administratorrollen](../../role-based-access-control/rbac-and-directory-admin-roles.md).|
|Dienstadministrator|Diese klassische Abonnementadministratorrolle ermöglicht die Verwaltung sämtlicher Azure-Ressourcen (einschließlich Zugriff). Sie hat den gleichen Zugriff wie ein Benutzer, dem für den Abonnementbereich die Rolle „Besitzer“ zugewiesen ist. Weitere Informationen finden Sie unter [Administratorrollen für klassische Abonnements, Azure RBAC-Rollen und Azure AD-Administratorrollen](../../role-based-access-control/rbac-and-directory-admin-roles.md).|
|Owner (Besitzer)|Diese Rolle hilft Ihnen bei der Verwaltung aller Azure-Ressourcen (einschließlich Zugriff). Sie basiert auf einem neueren Autorisierungssystem namens „rollenbasierte Zugriffssteuerung“ (Role-Based Access Control, RBAC), das eine präzise Zugriffsverwaltung für Azure-Ressourcen ermöglicht. Weitere Informationen finden Sie unter [Administratorrollen für klassische Abonnements, Azure RBAC-Rollen und Azure AD-Administratorrollen](../../role-based-access-control/rbac-and-directory-admin-roles.md).|
|Globaler Azure AD-Administrator|Diese Administratorrolle wird automatisch dem Ersteller des Azure AD-Mandanten zugewiesen. Globale Administratoren können sämtliche administrativen Funktionen für Azure AD sowie für jeden beliebigen Azure AD-Verbunddienst ausführen. Hierzu zählen etwa Exchange Online, SharePoint Online und Skype for Business Online. Es können mehrere globale Administratoren vorhanden sein, aber nur globale Administratoren können Benutzern Administratorrollen zuweisen (einschließlich der globalen Administratorrolle).<br><br>**Hinweis**<br>Diese Administratorrolle wird im Azure-Portal als globaler Administrator bezeichnet, in der Microsoft Graph-API und der Azure AD Graph-API sowie in Azure AD PowerShell heißt sie dagegen **Unternehmensadministrator**.<br><br>Weitere Informationen zu den unterschiedlichen Administratorrollen finden Sie unter [Berechtigungen der Administratorrolle in Azure Active Directory](../users-groups-roles/directory-assign-admin-roles.md).|
|Microsoft-Konto (auch MSA genannt)|Persönliche Microsoft-Konten, die Zugriff auf Ihre endbenutzerorientierten Microsoft-Produkte und -Clouddienste wie Outlook, OneDrive, MSN, Xbox LIVE oder Office 365 bieten. Ihr Microsoft-Konto wird im von Microsoft betriebenen Microsoft-Kontosystem für Endbenutzeridentitäten erstellt und gespeichert.|

## <a name="which-features-work-in-azure-ad"></a>Welche Features können in Azure AD verwendet werden?

Nachdem Sie Ihre Azure AD-Lizenz gewählt haben, erhalten Sie Zugriff auf einige oder alle der folgenden Features für Ihre Organisation:

|Category|BESCHREIBUNG|
|-------|-----------|
|Anwendungsverwaltung|Verwalten Sie Ihre cloudbasierten und lokalen Apps, und verwenden Sie den Anwendungsproxy, einmaliges Anmelden, das Portal „Meine Apps“ (auch „Zugriffsbereich“ genannt) sowie SaaS-Apps (Software as-a-Service). Weitere Informationen finden Sie unter [Bereitstellen von sicherem Remotezugriff auf lokale Anwendungen](../manage-apps/application-proxy.md) sowie in der [Dokumentation zur Anwendungsverwaltung](../manage-apps/index.yml).|
|Authentication|Verwalten Sie die Self-Service-Kennwortzurücksetzung von Azure Active Directory, die Multi-Factor Authentication, eine benutzerdefinierte Liste gesperrter Kennwörter und Smart Lockout. Weitere Informationen finden Sie in der [Dokumentation zur Azure AD-Authentifizierung](../authentication/index.yml).|
|Business-to-Business (B2B)|Verwalten Sie Ihre Gastbenutzer und externen Partner, ohne die Kontrolle über Ihre eigenen Unternehmensdaten aufzugeben. Weitere Informationen finden Sie in der [Dokumentation zu Azure Active Directory B2B](../b2b/index.yml).|
|Business-to-Consumer (B2C)|Steuern Sie die Registrierung, Anmeldung und Profilverwaltung der Benutzer, die Ihre App verwenden. Weitere Informationen finden Sie in der [Dokumentation für Azure Active Directory B2C](../../active-directory-b2c/index.yml).|
|Bedingter Zugriff|Verwalten Sie den Zugriff auf Ihre Cloud-Apps. Weitere Informationen finden Sie in der [Dokumentation zum bedingten Zugriff mit Azure AD](../conditional-access/index.yml).|
|Azure Active Directory für Entwickler|Erstellen Sie Apps, die alle Microsoft-Identitäten anmelden, und rufen Sie Token zum Aufrufen von Microsoft Graph, anderen Microsoft-APIs oder benutzerdefinierten APIs ab. Weitere Informationen finden Sie unter [Microsoft Identity Platform (Azure Active Directory für Entwickler)](../develop/index.yml).|
|Geräteverwaltung|Verwalten Sie den Zugriff auf Ihre Unternehmensdaten durch cloudbasierte oder lokale Geräte. Weitere Informationen finden Sie in der [Dokumentation zur Azure AD-Geräteverwaltung](../devices/index.yml).|
|Domänendienste|Binden Sie virtuelle Azure-Computer ganz ohne Domänencontroller in eine Domäne ein. Weitere Informationen finden Sie in der [Dokumentation zu Azure AD Domain Services](../../active-directory-domain-services/index.yml).|
|Unternehmensbenutzer|Verwalten Sie die Lizenzzuweisung sowie den Zugriff auf Apps, und richten Sie mithilfe von Gruppen und Administratorrollen Delegaten ein. Weitere Informationen finden Sie in der [Dokumentation zur Azure Active Directory-Benutzerverwaltung](../users-groups-roles/index.yml).|
|Hybrididentität|Verwenden Sie Azure Active Directory Connect und Connect Health, um eine einzelne Benutzeridentität für die Authentifizierung und Autorisierung gegenüber allen Ressourcen bereitzustellen – ganz gleich, ob es sich dabei um Cloudressourcen oder um lokale Ressourcen handelt. Weitere Informationen finden Sie in der [Dokumentation zur Hybrid-Identität](../hybrid/index.yml).|
|Identitätsgovernance|Verwalten Sie die Identität Ihrer Organisation über Zugriffssteuerungen für Mitarbeiter, Geschäftspartner, Anbieter, Dienste und Apps. Sie können auch Zugriffsüberprüfungen durchführen. Weitere Informationen finden Sie unter [Was ist Azure AD Identity Governance?](../governance/identity-governance-overview.md) sowie unter [Azure AD-Zugriffsüberprüfungen](../governance/access-reviews-overview.md).|
|Schutz der Identität (Identity Protection)|Erkennen Sie potenzielle Sicherheitsrisiken für die Identitäten Ihrer Organisation, konfigurieren Sie Richtlinien, um auf verdächtige Aktivitäten zu reagieren, und ergreifen Sie geeignete Gegenmaßnahmen. Weitere Informationen finden Sie unter [Azure AD Identity Protection](../identity-protection/index.yml).|
|Verwaltete Identitäten für Azure-Ressourcen|Stellt für Ihre Azure-Dienste eine automatisch verwaltete Identität in Azure AD bereit, die jeden von Azure AD unterstützten Authentifizierungsdienst authentifizieren kann (einschließlich Key Vault). Weitere Informationen finden Sie im unter [Was sind verwaltete Identitäten für Azure-Ressourcen?](../managed-identities-azure-resources/overview.md).|
|Privileged Identity Management (PIM)|Verwalten, steuern und überwachen Sie den Zugriff innerhalb Ihrer Organisation. Dieses Feature umfasst den Zugriff auf Ressourcen in Azure AD und Azure sowie auf andere Microsoft Online Services wie Office 365 oder Intune. Weitere Informationen finden Sie unter [Was ist Azure AD Privileged Identity Management?](../privileged-identity-management/index.yml).|
|Berichte und Überwachung|Gewinnen Sie Erkenntnisse zur Sicherheit und zu Verwendungsmustern in Ihrer Umgebung. Weitere Informationen finden Sie unter [Azure Active Directory-Berichte und -Überwachung](../reports-monitoring/index.yml).|

## <a name="next-steps"></a>Nächste Schritte

- [Gewusst wie: Registrieren für Azure Active Directory Premium](active-directory-get-started-premium.md)

- [Zuweisen oder Hinzufügen eines Azure-Abonnements zu Azure Active Directory](active-directory-how-subscriptions-associated-directory.md)

- [Schnellstart: Zugreifen auf Azure Active Directory zum Erstellen eines neuen Mandanten](active-directory-access-create-new-tenant.md)

- [Azure Active Directory Premium P2-Lizenzierungsfeature-Prüfliste](active-directory-deployment-checklist-p2.md)
