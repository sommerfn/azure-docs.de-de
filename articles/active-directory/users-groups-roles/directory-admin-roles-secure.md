---
title: Bewährte Methoden für den sicheren Adminostratorzugriff – Azure Active Directory | Microsoft-Dokumentation
description: Stellen Sie sicher, dass Administratorzugriff und Administratorkonten Ihrer Organisation sicher sind. Für Systemarchitekten und IT-Profis, die Azure AD, Azure und Microsoft Online Services konfigurieren.
services: active-directory
keywords: ''
author: curtand
manager: mtillman
ms.author: curtand
ms.date: 03/18/2019
ms.topic: article
ms.service: active-directory
ms.workload: identity
ms.subservice: users-groups-roles
ms.custom: it-pro
ms.reviewer: martincoetzer; MarkMorow
ms.collection: M365-identity-device-management
ms.openlocfilehash: 7d3346c873f88654f75628863a01fda76449ce2d
ms.sourcegitcommit: 55f7fc8fe5f6d874d5e886cb014e2070f49f3b94
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 09/25/2019
ms.locfileid: "71259407"
---
# <a name="securing-privileged-access-for-hybrid-and-cloud-deployments-in-azure-ad"></a>Schützen des privilegierten Zugriffs für hybride und Cloudbereitstellungen in Azure AD

Die Sicherheit der meisten, wenn nicht aller Geschäftsressourcen in modernen Organisationen hängt von der Integrität der privilegierten Konten ab, die IT-Systeme verwalten. Böswillige Akteure einschließlich Cyberangreifern haben häufig Administratorkonten und andere Elemente des privilegierten Zugriffs im Visier, um zu versuchen, mit Angriffen, deren Ziel der Diebstahl von Anmeldeinformationen ist, schnell Zugriff auf sensible Daten und Systeme zu erhalten. Bei Clouddiensten sind Cloud-Dienstanbieter und Kunde gemeinsam verantwortlich für Prävention und Reaktion. Weitere Informationen zu den neuesten Bedrohungen von Endpunkten und Cloud finden Sie unter [Informieren Sie sich über die aktuellen Angriffe auf Endpunkte und auf die Cloud ](https://www.microsoft.com/security/operations/security-intelligence-report). Dieser Artikel kann Ihnen helfen, eine Roadmap zum Schließen der Lücken zwischen Ihren aktuellen Plänen und dem hier beschriebenen Leitfaden zu entwickeln.

> [!NOTE]
> Microsoft sieht sich in der Pflicht, den Ansprüchen von Vertrauen, Transparenz, Einhaltung von Standards und Einhaltung behördlicher Bestimmungen in höchstem Maße gerecht zu werden. Mehr darüber, wie das globale Incidentreaktionsteam von Microsoft die Auswirkungen von Angriffen auf Clouddienste abschwächt, und wie die Sicherheit in Businessprodukte und Clouddienste von Microsoft integriert ist, erfahren Sie unter [Sicherheit](https://www.microsoft.com/trustcenter/security), und mehr über Complianceziele von Microsoft unter [Compliance](https://www.microsoft.com/trustcenter/compliance).

<!--## Risk management, incident response, and recovery preparation

A cyber-attack, if successful, can shut down operations not just for a few hours, but in some cases for days or even weeks. The collateral damage, such as legal ramifications, information leaks, and media coverage, could potentially continue for years. To ensure effective company-wide risk containment, cybersecurity and IT pros must align their response and recovery processes. To reduce the risk of business disruption due to a cyber-attack, industry experts recommend you do the following:

* As part of your risk management operations, establish a crisis management team for your organization that is responsible for managing all types of business disruptions.

* Compare your current risk mitigations, incident response, and recovery plan with industry best practices for managing a business disruption before, during, and after a cyber-attack.

* Develop and implement a roadmap for closing the gaps between your current plans and the best practices described in this document.


## Securing privileged access for hybrid and cloud deployments

does the article really start here?-->
Für die meisten Organisationen hängt die Sicherheit der Geschäftsressourcen von der Integrität der privilegierten Konten ab, die IT-Systeme verwalten. Cyberangreifer konzentrieren sich auf privilegierten Zugriff auf Infrastruktursysteme (z.B. Active Directory und Azure Active Directory), um Zugriff auf sensible Daten einer Organisation zu erlangen. 

Herkömmliche Ansätze, die sich auf das Schützen der Ein- und Ausgangspunkte eines Netzwerks als primäre Sicherheitszone konzentrieren, sind aufgrund steigender Verwendung von SaaS-Apps und persönlichen Geräten im Internet weniger effektiv. In einem komplexen modernen Unternehmen bilden die Authentifizierungs- und Autorisierungssteuerelemente in der Identitätsschicht einer Organisation den natürlichen Ersatz für die Netzwerksicherheitszone.

Privilegierte Administratorkonten kontrollieren diese neue „Sicherheitszone“ wirksam. Der Schutz des privilegierten Zugriffs ist entscheidend – dabei spielt es keine Rolle, ob es sich um eine lokale oder Cloudumgebung bzw. hybride lokale und cloudgehostete Dienste handelt. Um Administratorzugriff gegen entschlossene Angreifer zu schützen, müssen Sie einen vollständigen und wohl überlegten Ansatz wählen, um die Systeme Ihrer Organisation vor Risiken zu bewahren. 

Schützen des privilegierten Zugriffs verlangt Änderungen an

* Prozessen, Verwaltungsmethoden und Wissensmanagement
* technischen Komponenten wie z.B. Maßnahmen zu Hostverteidigung, Kontenschutz und Identitätsverwaltung

Dieses Dokument konzentriert sich hauptsächlich auf das Erstellen einer Roadmap zum Schützen von Identitäten und Zugriff, die in Azure AD, Microsoft Azure, Office 365 und anderen Clouddiensten verwaltet oder gemeldet werden. Für Organisationen mit lokalen Administratorkonten beachten Sie den Leitfaden für lokalen und hybriden privilegierten Zugriff, der von Active Directory aus verwaltet wird, unter [Schützen des privilegierten Zugriffs](https://docs.microsoft.com/windows-server/identity/securing-privileged-access/securing-privileged-access). 

> [!NOTE] 
> Der Leitfaden in diesem Artikel bezieht sich in erster Linie auf Features von Azure Active Directory, die in den Azure Active Directory Premium-Plänen P1 und P2 enthalten sind. Azure Active Directory Premium P2 ist in der EMS E5-Suite und Microsoft 365 E5-Suite enthalten. In diesem Leitfaden wird davon ausgegangen, dass Ihre Organisation bereits Azure AD Premium P2-Lizenzen für Ihre Benutzer erworben hat. Wenn Sie nicht über diese Lizenzen verfügen, gilt möglicherweise ein Teil dieses Leitfadens nicht für Ihre Organisation. Darüber hinaus wird in diesem Artikel der Begriff globaler Administrator (oder globaler Admin) durchgängig als Synonym für „Unternehmensadministrator“ oder „Mandantenadministrator“ verwendet.

## <a name="develop-a-roadmap"></a>Entwickeln einer Roadmap 

Microsoft empfiehlt Ihnen, eine Roadmap zum Schützen des privilegierten Zugriffs gegenüber Cyberangreifern zu entwickeln und zu befolgen. Sie können Ihre Roadmap jederzeit den bestehenden Funktionen und spezifischen Anforderungen in Ihrer Organisation anpassen. Jede Phase der Roadmap sollte den Aufwand und Probleme erhöhen, die Angreifern beim Angriff auf den privilegierten Zugriff auf Ihre lokalen, Cloud- und Hybridressourcen entstehen. Microsoft empfiehlt die folgenden vier Roadmapphasen: Diese empfohlene Roadmap sieht zunächst die wirkungsvollsten und schnellsten Implementierungen auf der Grundlage der Erfahrung von Microsoft mit Cyberangriffsvorfällen und Reaktionsimplementierung vor. Die Zeitpläne für diese Roadmap sind ungefähre Werte.

![Phasen der Roadmap mit Zeitplänen](./media/directory-admin-roles-secure/roadmap-timeline.png)

* Phase 1 (24 bis 48 Stunden): Wichtige Dinge, die umgehend erledigt werden sollten

* Phase 2 (2 bis 4 Wochen): Gegenmaßnahmen für die gängigsten Angriffsstrategien

* Phase 3 (1 bis 3 Monate): Schaffung von Transparenz und uneingeschränkter Kontrolle über Administratoraktivitäten

* Phase 4 (sechs Monate und später): Fortsetzung des Aufbaus von Verteidigungsmaßnahmen zur weiteren Härtung Ihrer Sicherheitsplattform

Dieses Roadmapframework folgt dem Konzept, die Verwendung von Microsoft-Technologien zu maximieren, die Sie möglicherweise bereits bereitgestellt haben. Sie können auch die wichtigsten aktuellen und zukünftigen Sicherheitstechnologien nutzen und Sicherheitstools anderer Hersteller integrieren, die Sie bereits bereitgestellt haben oder bereitzustellen erwägen. 

## <a name="stage-1-critical-items-that-we-recommend-you-do-right-away"></a>Phase 1: Wichtige Dinge, die umgehend erledigt werden sollten

![Phase 1: Wichtige als Erstes zu erledigende Dinge](./media/directory-admin-roles-secure/stage-one.png)

Phase 1 der Roadmap konzentriert sich auf wichtige Aufgaben, die schnell und einfach zu implementieren sind. Sie sollten diese wenigen Elemente sofort innerhalb der ersten 24 bis 48 Stunden ausführen, um ein Basisniveau von geschütztem privilegiertem Zugriff sicherzustellen. Diese Phase der Roadmap für geschützten privilegierten Zugriff umfasst die folgenden Aktionen:

### <a name="general-preparation"></a>Allgemeine Vorbereitung

#### <a name="turn-on-azure-ad-privileged-identity-management"></a>Aktivieren von Azure AD Privileged Identity Management

Wenn Sie Azure AD Privileged Identity Management (PIM) nicht bereits aktiviert haben, holen Sie dies in Ihrem Mandanten für die Produktion nach. Nach der Aktivierung von Privileged Identity Management erhalten Sie Benachrichtigungs-E-Mails zu Rollenänderungen bei privilegiertem Zugriff. Diese Benachrichtigungen sind eine frühzeitige Warnung, wenn zusätzliche Benutzer hoch privilegierten Rollen in Ihrem Verzeichnis hinzugefügt werden.

Azure AD Privileged Identity Management ist in Azure AD Premium P2 oder EMS E5 enthalten. Diese Lösungen unterstützen Sie beim Schützen des Zugriffs auf Anwendungen und Ressourcen in lokaler Umgebung und Cloud. Wenn Sie noch nicht über Azure AD Premium P2 oder EMS E5 verfügen und weitere Features aus dieser Roadmap ausprobieren möchten, registrieren Sie sich für die [kostenlose 90-tägige Testversion von Enterprise Mobility + Security](https://www.microsoft.com/cloud-platform/enterprise-mobility-security-trial). Testen Sie mit diesen Testlizenzen Azure AD Privileged Identity Management und Azure AD Identity Protection, um die Aktivität mithilfe erweiterter Sicherheitsberichterstattung, Überwachung und Warnungen von Azure AD zu überwachen.

Nach dem Aktivieren von Azure AD Privileged Identity Management:

1. Melden Sie sich beim [Azure-Portal](https://portal.azure.com/) über ein Konto an, das als globaler Administrator Ihres Produktionsmandanten konfiguriert ist.

2. Um den Mandanten auszuwählen, in dem Sie Privileged Identity Management verwenden möchten, wählen Sie Ihren Benutzernamen in der oberen rechten Ecke des Azure-Portals aus.

3. Wählen Sie **Alle Dienste** aus, und filtern Sie die Liste nach **Azure AD Privileged Identity Management**.

4. Öffnen Sie Privileged Identity Management in der Liste **Alle Dienste**, und heften Sie Privileged Identity Management an Ihr Dashboard.

Der ersten Person, die Azure AD Privileged Identity Management in Ihrem Mandanten verwendet, werden automatisch die Rollen **Sicherheitsadministrator** und **Administrator für privilegierte Rollen** im Mandanten zugewiesen. Nur Administratoren für privilegierte Rollen können die Azure AD-Verzeichnisrollenzuweisungen von Benutzern verwalten. Darüber hinaus wird Ihnen nach dem Hinzufügen von Azure AD Privileged Identity Management der Sicherheits-Assistent angezeigt, der Sie durch die erste Ermittlung und Zuweisung führt. Sie können den Assistenten zu diesem Zeitpunkt ohne weitere Änderungen beenden. 

#### <a name="identify-and-categorize-accounts-that-are-in-highly-privileged-roles"></a>Identifizieren und Kategorisieren von Konten in stark privilegierten Rollen 

Zeigen Sie nach dem Aktivieren von Azure AD Privileged Identity Management die Benutzer in den Verzeichnisrollen globaler Administrator, privilegierter Rollenadministrator, Exchange Online-Administrator und SharePoint Online-Administrator an. Wenn Sie in Ihrem Mandanten nicht über Azure AD PIM verfügen, können Sie die [PowerShell-API](https://docs.microsoft.com/powershell/module/azuread/get-azureaddirectoryrolemember?view=azureadps-2.0) verwenden. Beginnen Sie mit der Rolle des globalen Administrators, da diese Rolle generisch ist: Ein Benutzer mit dieser Administratorrolle besitzt dieselben Berechtigungen für alle Clouddienste, die Ihre Organisation abonniert hat, unabhängig davon, ob ihm die Rolle im Microsoft 365 Admin Center, im Azure-Portal oder mithilfe des Azure AD-Moduls für Microsoft PowerShell zugewiesen wurde. 

Entfernen Sie alle Konten, die in diesen Rollen nicht mehr benötigt werden. Kategorisieren Sie anschließend die restlichen Konten, die Administratorrollen zugewiesen sind:

* Einzeln Benutzern mit Administratorrechten zugewiesen und kann auch zu nicht administrativen Zwecken (z.B. persönliche E-Mail) verwendet werden
* Einzeln Benutzern mit Administratorrechten zugewiesen und nur für Verwaltungszwecke vorgesehen
* Für mehrere Benutzer freigegeben
* Für Notfallzugriffs-Szenarios
* Für automatisierte Skripts
* Für externe Benutzer

#### <a name="define-at-least-two-emergency-access-accounts"></a>Definieren Sie mindestens zwei Notfallzugriffs-Konten 

Stellen Sie sicher, dass Sie nicht in eine Situation geraten, in der sie versehentlich von der Verwaltung Ihres Azure AD-Mandanten ausgesperrt werden, weil sie sich nicht anmelden oder nicht das Konto eines vorhandenen einzelnen Benutzers als Administrator aktivieren können. Wenn die Organisation z.B. mit einem lokalen Identitätsanbieter verbunden ist, könnte dieser Identitätsanbieter möglicherweise nicht verfügbar sein, sodass Benutzer sich nicht lokal anmelden können. Sie können die Auswirkungen eines versehentlichen Verlusts des Administratorzugriffs abmildern, indem Sie mindestens zwei Konten für den Notfallzugriff in Ihrem Mandanten konfigurieren.

Konten für den Notfallzugriff unterstützen Organisationen dabei, den privilegierten Zugriff innerhalb einer vorhandenen Azure Active Directory-Umgebung einzuschränken. Diese Konten verfügen über weitreichende Privilegien und werden keinen Einzelpersonen zugewiesen. Konten für den Notfallzugriff sind auf Notfallsituationen oder Szenarios beschränkt, in denen normale Administratorkonten nicht verwendet werden können. Organisationen müssen das Ziel der Kontrolle sicherstellen, und dass die Verwendung der Notfallkonten ausschließlich auf Zeiten beschränkt wird, in denen dies notwendig ist.

Bewerten Sie die Konten, die zugewiesen werden oder für die Rolle „globaler Administrator“ berechtigt sind. Wenn Ihnen keine ausschließlichen Cloudkonten mit der *.onmicrosoft.com-Domäne (vorgesehen für den Notfallzugriff) angezeigt werden, erstellen Sie sie. Weitere Informationen finden Sie unter [Verwalten von Administratorkonten für den Notfallzugriff in Azure AD](directory-emergency-access.md).

#### <a name="turn-on-multi-factor-authentication-and-register-all-other-highly-privileged-single-user-non-federated-admin-accounts"></a>Aktivieren der mehrstufigen Authentifizierung und Registrieren aller anderen nicht verbundenen Einzelbenutzer-Administratorkonten mit hohen Privilegien

Schreiben Sie bei der Anmeldung die mehrstufige Authentifizierung (Multi-Factor Authentication, MFA) von Azure für alle Benutzer vor, die dauerhaft einzelnen oder mehreren der Azure AD-Administratorrollen zugewiesen sind: globaler Administrator, privilegierter Rollenadministrator, Exchange Online-Administrator und SharePoint Online-Administrator. Nutzen Sie den Leitfaden zum Aktivieren [einer zweistufigen Überprüfung für einen Benutzer oder eine Gruppe](../authentication/howto-mfa-userstates.md), und stellen Sie sicher, dass alle Benutzer unter [https://aka.ms/mfasetup](https://aka.ms/mfasetup) registriert sind. Weitere Informationen finden Sie unter Schritt 2 und 3 des Handbuchs [Zugriffsschutz für Daten und Dienste in Office 365](https://support.office.com/article/Protect-access-to-data-and-services-in-Office-365-a6ef28a4-2447-4b43-aae2-f5af6d53c68e). 

## <a name="stage-2-mitigate-the-most-frequently-used-attack-techniques"></a>Phase 2: Gegenmaßnahmen für die gängigsten Angriffsstrategien

![Phase 2: Gegenmaßnahmen für gängige Angriffsstrategien](./media/directory-admin-roles-secure/stage-two.png)

Phase 2 der Roadmap konzentriert sich auf die Abwehr besonders häufig verwendeter Angriffstechniken zum Diebstahl und Missbrauch von Anmeldeinformationen und kann in etwa zwei bis vier Wochen implementiert werden. Diese Phase der Roadmap für geschützten privilegierten Zugriff umfasst die folgenden Aktionen.

### <a name="general-preparation"></a>Allgemeine Vorbereitung

#### <a name="conduct-an-inventory-of-services-owners-and-admins"></a>Durchführen einer Inventur der Dienste, Besitzer und Administratoren

Mit der Zunahme von Bring Your Own Device-Richtlinien (BYOD) und Work From Home-Richtlinien sowie wachsender Drahtloskonnektivität in Unternehmen ist es wichtig, dass Sie überwachen, wer eine Verbindung mit Ihrem Netzwerk herstellt. Eine effektive Sicherheitsüberwachung deckt häufig Geräte, Anwendungen und Programme in Ihrem Netzwerk auf, die nicht von der IT unterstützt werden und darum möglicherweise nicht sicher sind. Weitere Informationen finden Sie unter [Azure-Sicherheitsverwaltung und -Überwachung](../../security/fundamentals/management-monitoring-overview.md). Stellen Sie sicher, dass alle folgenden Aufgaben in Ihrem Inventurprozess enthalten sind. 

* Identifizieren Sie die Benutzer mit Administratorrollen und die Dienste, über die sie verwalten können.
* Finden Sie mit Azure AD PIM heraus, welche Benutzer in Ihrer Organisation Administratorzugriff auf Azure AD haben, einschließlich zusätzlicher Rollen zu den in Schritt 1 aufgelisteten.
* Neben den in Azure AD definierten Rollen enthält Office 365 eine Reihe von Administratorrollen, die Sie Benutzern in Ihrer Organisation zuweisen können. Jede Administratorrolle ist allgemeinen Geschäftsfunktionen zugeordnet und stattet Benutzer in Ihrer Organisation mit Berechtigungen für bestimmte Aufgaben im [Microsoft 365 Admin Center](https://admin.microsoft.com) aus. Finden Sie im Microsoft 365 Admin Center heraus, welche Benutzer in Ihrer Organisation Administratorzugriff auf Office 365 haben, einschließlich über Rollen, die nicht in Azure AD verwaltet werden. Weitere Informationen finden Sie unter [Informationen zu Administratorrollen von Office 365](https://support.office.com/article/About-Office-365-admin-roles-da585eea-f576-4f55-a1e0-87090b6aaa9d) und [Bewährte Methoden für die Sicherheit von Office 365](https://docs.microsoft.com/office365/servicedescriptions/office-365-platform-service-description/office-365-securitycompliance-center).
* Führen Sie die Inventur in anderen Diensten durch, von denen Ihre Organisation abhängig ist, z.B. Azure, Intune oder Dynamics 365.
* Stellen Sie sicher, dass Ihren Administratorkonten (für Verwaltungszwecke verwendete Konten, nicht einfach herkömmliche Benutzerkonten) gültige E-Mail-Adressen angefügt sind, und dass sie für Azure MFA registriert sind bzw. MFA lokal verwenden.
* Fragen Sie Benutzer nach der geschäftlichen Begründung für den Administratorzugriff.
* Entziehen Sie den Administratorzugriff Personen und Diensten, die ihn nicht benötigen.

#### <a name="identify-microsoft-accounts-in-administrative-roles-that-need-to-be-switched-to-work-or-school-accounts"></a>Identifizieren von Microsoft-Konten in Administratorrollen, die auf Arbeits- oder Schulkonten umgestellt werden müssen

In manchen Fällen verwenden die ersten globalen Administratoren für eine Organisation ihre vorhandenen Microsoft-Kontoanmeldeinformationen, wenn sie beginnen, Azure AD zu nutzen. Diese Microsoft-Konten sollten durch einzelne cloudbasierte oder synchronisierte Konten ersetzt werden. 

#### <a name="ensure-separate-user-accounts-and-mail-forwarding-for-global-administrator-accounts"></a>Sicherstellen von separaten Benutzerkonten und E-Mail-Weiterleitung für globale Administratorkonten 

Globale Administratorkonten sollten nicht über persönliche E-Mail-Adressen verfügen, da persönliche E-Mail-Konten regelmäßig das Phishingziel von Cyberangreifern sind. Um Internetrisiken (Phishingangriffe unbeabsichtigtes Webbrowsen) von Administratorrechten zu trennen, erstellen Sie dedizierte Konten für jeden Benutzer mit Administratorrechten. 

Sofern nicht bereits geschehen, erstellen Sie separate Konten für Benutzer zum Durchführen globaler Administratoraufgaben, um sicherzustellen, dass sie nicht versehentlich E-Mails öffnen oder Programme ausführen, die ihren Administratorkonten zugeordnet sind. Achten Sie darauf, dass die E-Mail dieser Konten an ein Arbeitspostfach weitergeleitet wird.  

#### <a name="ensure-the-passwords-of-administrative-accounts-have-recently-changed"></a>Sicherstellen, dass die Kennwörter von Administratorkonten kürzlich geändert wurden

Stellen Sie sicher, dass alle Benutzer sich mindestens einmal in den letzten 90 Tagen bei ihren administrativen Konten angemeldet und ihre Kennwörter geändert haben. Stellen Sie außerdem sicher, dass bei gemeinsam genutzten Konten, deren Kennwort mehreren Benutzern bekannt ist, die Kennwörter kürzlich geändert wurden.

#### <a name="turn-on-password-hash-synchronization"></a>Aktivieren der Kennworthashsynchronisierung

Die Kennworthashsynchronisierung ist ein Feature zum Synchronisieren von Hashes von Benutzerkennwörtern aus einer lokalen Active Directory-Instanz mit einer cloudbasierten Azure Active Directory-Instanz (Azure AD). Auch wenn Sie den Verbund mit Active Directory-Verbunddiensten (Active Directory Federation Services, AD FS) oder anderen Identitätsanbietern verwenden möchten, können Sie die Kennworthashsynchronisierung optional als eine Sicherung für den Fall einrichten, dass bei Ihrer lokalen Infrastruktur – z.B. AD- oder AD FS-Server – ein Fehler auftritt oder sie vorübergehend nicht verfügbar ist. Dies ermöglicht Benutzern, sich bei dem Dienst mit dem gleichen Kennwort anzumelden, das sie zur Anmeldung bei Ihrer lokalen AD-Instanz verwenden. Außerdem ermöglicht es Identity Protection das Erkennen gefährdeter Anmeldeinformationen durch Vergleichen ihrer Kennworthashes mit Kennwörtern, die bekanntermaßen gefährdet sind, wenn ein Benutzer dieselbe E-Mail-Adresse und dasselbe Kennwort bei anderen Diensten genutzt hat, die nicht mit Azure AD verbunden sind.  Weitere Informationen finden Sie unter [Implementieren der Kennworthashsynchronisierung mit der Azure AD Connect-Synchronisierung](../hybrid/how-to-connect-password-hash-synchronization.md).

#### <a name="require-multi-factor-authentication-mfa-for-users-in-all-privileged-roles-as-well-as-exposed-users"></a>Fordern der mehrstufigen Authentifizierung (Multi-Factor Authentication, MFA) sowohl für Benutzer in allen privilegierten Rollen als auch für Benutzer mit hoher Gefährdung

Azure AD empfiehlt, dass Sie die mehrstufige Authentifizierung (Multi-Factor Authentication, MFA) für alle Benutzer voraussetzen, einschließlich Administratoren und aller anderen Benutzer, die einen entscheidenden Einfluss haben würden, wenn ihr Konto gefährdet würde (z.B. kaufmännische Geschäftsführer). Auf diese Weise sinkt das Risiko eines Angriffs aufgrund eines offengelegten Kennworts.

Aktivieren Sie:

* [Richtlinien für MFA mit bedingtem Zugriff](../authentication/howto-mfa-getstarted.md) für alle Benutzer in Ihrer Organisation.

Wenn Sie Windows Hello for Business verwenden, kann die MFA-Anforderung durch Verwendung der Windows Hello-Anmeldebenutzeroberfläche erfüllt werden. Weitere Informationen finden Sie unter [Windows Hello](https://docs.microsoft.com/windows/uwp/security/microsoft-passport). 

#### <a name="configure-identity-protection"></a>Konfigurieren von Identity Protection 

Azure AD Identity Protection ist ein algorithmusbasiertes Überwachungs- und Berichterstattungstool, mit dem Sie potenzielle Sicherheitslücken erkennen können, die Auswirkungen auf die Identitäten Ihrer Organisation haben können. Sie können automatische Antworten auf diese erkannten verdächtigen Aktivitäten konfigurieren und entsprechende Maßnahmen zu deren Behebung ergreifen. Weitere Informationen finden Sie unter [Azure Active Directory Identity Protection](../active-directory-identityprotection.md).

#### <a name="obtain-your-office-365-secure-score-if-using-office-365"></a>Abrufen Ihres Office 365 Secure Score (bei Verwendung von Office 365)

Secure Score findet heraus, welche Office 365-Dienste Sie verwenden (z.B. OneDrive, SharePoint und Exchange), prüft dann Ihre Einstellungen und Aktivitäten und vergleicht diese mit einer von Microsoft aufgestellten Grundlinie. Das Ergebnis sagt aus, in welchem Maße Sie bewährte Sicherheitsmethoden befolgen. Jeder Benutzer, der über Administratorberechtigungen (globaler Administrator oder benutzerdefinierte Administratorrolle) für ein Office 365 Business Premium- oder Enterprise-Abonnement verfügt, kann unter [https://securescore.office.com](https://securescore.office.com/) auf Secure Score zugreifen.

#### <a name="review-the-office-365-security-and-compliance-guidance-if-using-office-365"></a>Leitfaden: Überprüfen der Office 365-Sicherheit und -Konformität (bei Verwendung von Office 365)

Die [Plan von Sicherheit und Compliance in Office 365](https://support.office.com/article/Plan-for-security-and-compliance-in-Office-365-dc4f704c-6fcc-4cab-9a02-95a824e4fb57) erklärt den Ansatz, mit dem ein Office 365-Kunde Office 365 konfigurieren und andere EMS-Funktionen nutzen sollte. Lesen Sie dann die Schritte 3 bis 6 zum [Zugriffsschutz für Daten und Dienste in Office 365](https://support.office.com/article/Protect-access-to-data-and-services-in-Office-365-a6ef28a4-2447-4b43-aae2-f5af6d53c68e) und das Handbuch zum [Überwachen von Sicherheit und Compliance in Office 365](https://support.office.com/article/Monitor-security-and-compliance-in-Office-365-b62f1722-fd39-44eb-8361-da61d21509b6).

#### <a name="configure-office-365-activity-monitoring-if-using-office-365"></a>Konfigurieren der Office 365-Aktivitätsüberwachung (bei Verwendung von Office 365)

Sie können überwachen, wie Benutzer in Ihrer Organisation Office 365-Dienste verwenden, sodass Sie Benutzer identifizieren können, die über ein Administratorkonto verfügen und möglicherweise keinen Office 365-Zugriff benötigen, weil sie sich nicht bei diesen Portalen anmelden. Weitere Informationen finden Sie unter [Aktivitätsberichte im Microsoft 365 Admin Center](https://support.office.com/article/Activity-Reports-in-the-Office-365-admin-center-0d6dfb17-8582-4172-a9a9-aed798150263).

#### <a name="establish-incidentemergency-response-plan-owners"></a>Einrichten von Notfallreaktionsplan-Besitzern

Eine wirksame Reaktion auf Notfälle ist eine komplexe Angelegenheit. Daher erfordert das Einrichten einer erfolgreichen Notfallreaktionsfunktion erhebliche Planung und Ressourcen. Es ist wichtig, dass Sie eine kontinuierliche Überwachung auf Cyberangriffe durchführen und Verfahren für die Priorisierung der Behandlung von Vorfällen festlegen. Effektive Methoden zum Sammeln, Analysieren und Berichten von Daten sind entscheidend für den Aufbau von Beziehungen und zum Einrichten einer Kommunikation mit anderen internen Gruppen und Planbesitzern. Weitere Informationen finden Sie unter [Microsoft Security Response Center](https://technet.microsoft.com/security/dn440717). 

#### <a name="secure-on-premises-privileged-administrative-accounts-if-not-already-done"></a>Schützen von lokalen privilegierten Administratorkonten, falls noch nicht geschehen

Wenn Ihr Azure Active Directory-Mandant mit einem lokalen Active Directory synchronisiert wird, orientieren Sie sich an dem Leitfaden in [Schützen des privilegierten Zugriffs](https://docs.microsoft.com/windows-server/identity/securing-privileged-access/securing-privileged-access): Phase 1. Dies umfasst das Erstellen separater Administratorkonten für Benutzer, die lokale Verwaltungsaufgaben durchführen, das Bereitstellen von Privileged Access Workstations für Active Directory-Administratoren und das Erstellen von eindeutigen Kennwörtern für lokale Administratoren für Arbeitsstationen und Server.

### <a name="additional-steps-for-organizations-managing-access-to-azure"></a>Zusätzliche Schritte für Organisationen, die den Zugriff auf Azure verwalten

#### <a name="complete-an-inventory-of-subscriptions"></a>Durchführen einer Abonnementinventur

Verwenden Sie Enterprise-Portal und Azure-Portal, um die Abonnements in Ihrer Organisation zu identifizieren, die Produktionsanwendungen hosten.

#### <a name="remove-microsoft-accounts-from-admin-roles"></a>Entfernen von Microsoft-Konten aus Administratorrollen

Microsoft-Konten aus anderen Programmen wie Xbox, Live und Outlook sollten nicht als Administratorkonten für organisatorische Abonnements verwendet werden. Entfernen Sie den Administratorstatus von allen Microsoft-Konten, und ersetzen Sie sie durch Azure Active Directory-Konten (z. B. chris@contoso.com), Geschäfts-, Schul- oder Unikonten.

#### <a name="monitor-azure-activity"></a>Überwachen der Azure-Aktivität

Das Azure-Aktivitätsprotokoll zeigt den Verlauf der Ereignisse auf Abonnementebene in Azure an. Darüber hinaus enthält es Informationen darüber, welche Ressourcen von welcher Person zu einem bestimmten Zeitpunkt erstellt, aktualisiert und gelöscht wurden, und wann diese Ereignisse auftraten. Weitere Informationen finden Sie unter [Überwachen und Empfangen von Benachrichtigungen zu wichtigen Aktionen im Azure-Abonnement](../../azure-monitor/platform/quick-audit-notify-action-subscription.md).

### <a name="additional-steps-for-organizations-managing-access-to-other-cloud-apps-via-azure-ad"></a>Zusätzliche Schritte für Organisationen, die den Zugriff auf andere Cloud-Apps über Azure AD verwalten

#### <a name="configure-conditional-access-policies"></a>Konfigurieren von Richtlinien für bedingten Zugriff

Bereiten Sie Richtlinien für bedingten Zugriff für lokale und Cloud-gehostete Anwendungen vor. Wenn Sie über mit dem Arbeitsbereich verknüpfte persönliche Geräte von Benutzern verfügen, erhalten Sie weitere Informationen unter [Einrichten des lokalen bedingten Zugriffs mithilfe der Azure Active Directory-Geräteregistrierung](../active-directory-device-registration-on-premises-setup.md).


## <a name="stage-3-build-visibility-and-take-full-control-of-admin-activity"></a>Phase 3: Schaffung von Transparenz und uneingeschränkter Kontrolle über Administratoraktivitäten

![Phase 3: Kontrolle über Administratoraktivitäten](./media/directory-admin-roles-secure/stage-three.png)

Phase 3 baut auf den abschwächenden Maßnahmen aus Phase 2 auf und soll innerhalb von ca. 1 bis 3 Monaten implementiert werden. Diese Phase der Roadmap für geschützten privilegierten Zugriff umfasst die folgenden Komponenten.

### <a name="general-preparation"></a>Allgemeine Vorbereitung

#### <a name="complete-an-access-review-of-users-in-administrator-roles"></a>Durchführen einer Zugriffsüberprüfung von Benutzern in Administratorrollen

Mehr Unternehmensbenutzer erhalten privilegierten Zugriff über Clouddienste, was zu einer zunehmend unverwalteten Plattform führen kann. Hierzu gehören Benutzer, die globale Administratoren von Office 365 werden, Azure-Abonnementadministratoren und Benutzer, die administrativen Zugriff auf virtuelle Computer oder über SaaS-Apps besitzen. Organisationen sollten stattdessen Wert darauf legen, dass alle Mitarbeiter, insbesondere Administratoren, alltägliche Geschäftstransaktionen als nicht privilegierte Benutzer abwickeln und nur bei Bedarf Administratorrechte haben. Da die Anzahl der Benutzer mit Administratorrollen seit der ersten Annahme zugenommen haben kann, führen Sie Zugriffsprüfungen durch, um jeden Benutzer sicher zu identifizieren, der zum Aktivieren von Administratorrechten berechtigt ist. 

Gehen Sie wie folgt vor:

* Bestimmen Sie, welche Benutzer Azure AD-Administratoren sind, aktivieren Sie bedarfsorientierten und Just-in-Time-Administratorzugriff sowie rollenbasierte Sicherheitssteuerungen.
* Konvertieren Sie Benutzer, für deren Berechtigung zum privilegierten Administratorzugriff es keinen triftigen Grund gibt, in eine andere Rolle (wenn keine geeignete Rolle vorhanden ist, entfernen Sie sie).

#### <a name="continue-rollout-of-stronger-authentication-for-all-users"></a>Fortsetzen der Einführung einer sichereren Authentifizierung für alle Benutzer 

Fordern Sie für C-Suite-Führungskräfte, Manager höherer Ebene, wichtiges IT- und Sicherheitspersonal und andere Benutzer mit hoher Gefährdung moderne, sichere Authentifizierung, z.B. Azure MFA oder Windows Hello. 

#### <a name="use-dedicated-workstations-for-administration-for-azure-ad"></a>Verwenden dedizierter Arbeitsstationen für die Azure AD-Verwaltung

Angreifer können versuchen, privilegierte Konten anzugreifen, um Zugriff auf Daten und Systeme eines Unternehmens zu erhalten, damit sie Integrität und Authentizität der Daten über schädlichen Code, der die Programmlogik ändert, oder das Ausspionieren von Administratoranmeldeinformationen zerstören können. Privileged Access Workstations (PAWs) bieten für sensible Aufgaben ein dediziertes Betriebssystem, das vor Internetangriffen und Bedrohungsüberträgern geschützt ist. Die Trennung solcher sensitiven Aufgaben und Konten von den täglich verwendeten Arbeitsstationen und Geräten bietet einen sehr starken Schutz vor Phishing-Angriffen, vor Sicherheitsrisiken für das Betriebssystem und die Anwendungen, verschiedenen Identitätswechselangriffen und dem Diebstahl von Anmeldeinformationen z.B. mithilfe von Keyloggern, Pass-the-Hash und Pass-the-Ticket. Durch die Bereitstellung von Privileged Access Workstations können Sie das Risiko reduzieren, dass Administratoren Administratoranmeldeinformationen außerhalb einer gefestigten Desktopumgebung eingeben. Weitere Informationen finden Sie unter [Privileged Access Workstations](https://docs.microsoft.com/windows-server/identity/securing-privileged-access/privileged-access-workstations).

#### <a name="review-national-institute-of-standards-and-technology-recommendations-for-handling-incidents"></a>Lesen der Empfehlungen des National Institute of Standards and Technology zur Abwicklung von Vorfällen 

Das National Institute of Standards and Technology (NIST) bietet Richtlinien für den Umgang mit Vorfällen, insbesondere für die Analyse vorfallbezogener Daten und Bestimmung der richtigen Reaktion auf jeden Vorfall. Weitere Informationen finden Sie unter [The (NIST) Computer Security Incident Handling Guide (SP 800-61, Revision 2)](https://nvlpubs.nist.gov/nistpubs/SpecialPublications/NIST.SP.800-61r2.pdf) (Handbuch zur Behandlung von Computersicherheitsvorfällen).

#### <a name="implement-privileged-identity-management-pim-for-jit-to-additional-administrative-roles"></a>Implementieren von Privileged Identity Management (PIM) für JIT in zusätzlichen Administratorrollen

Verwenden Sie für Azure Active Directory die Funktion [Azure AD Privileged Identity Management](../privileged-identity-management/pim-configure.md). Die zeitlich begrenzte Aktivierung von privilegierten Rollen ermöglicht Ihnen Folgendes:

* Aktivieren von Administratorrechten zur Ausführung einer bestimmten Aufgabe
* Erzwingen der mehrstufigen Authentifizierung während der Aktivierung
* Verwenden von Warnungen, um Administratoren über Out-of-band-Änderungen zu informieren
* Benutzern ermöglichen, bestimmte Berechtigungen für eine vorkonfigurierte Zeitspanne beizubehalten
* Sicherheitsadministratoren erlauben, alle privilegierten Identitäten zu ermitteln, Überwachungsberichte anzuzeigen und Zugriffsprüfungen zu erstellen, um jeden Benutzer zu identifizieren, der zum Aktivieren von Administratorrechten berechtigt ist

Wenn Sie bereits Azure AD Privileged Identity Management verwenden, passen Sie Zeitrahmen für zeitgebundene Berechtigungen nach Bedarf an (z.B. Wartungsfenster).

#### <a name="determine-exposure-to-password-based-sign-in-protocols-if-using-exchange-online"></a>Ermitteln der Gefährdung durch kennwortbasierte Anmeldeprotokolle (bei Verwendung von Exchange Online)

In der Vergangenheit setzten Protokolle voraus, dass Benutzername/Kennwort-Kombinationen in Geräten, E-Mail-Konten, Telefonen usw. eingebettet waren. Doch bei den heutigen Risiken von Cyberangriffen in der Cloud sollten Sie jeden Benutzer identifizieren, der der Organisation potenziell schweren Schaden zufügen könnte, wenn seine Anmeldeinformationen gefährdet wären, und ihm durch Implementieren von sicheren Authentifizierungsanforderungen und bedingtem Zugriff die Möglichkeit verwehren, sich mit Benutzername/Kennwort bei seiner E-Mail-Adresse anzumelden. Sie können [ältere Authentifizierungsmethoden mit dem bedingten Zugriff](https://docs.microsoft.com/azure/active-directory/conditional-access/block-legacy-authentication) blockieren. Überprüfen Sie die Details zum [Blockieren der Standardauthentifizierung](https://docs.microsoft.com/exchange/clients-and-mobile-in-exchange-online/disable-basic-authentication-in-exchange-online) über Exchange Online. 

#### <a name="complete-a-roles-review-assessment-for-office-365-roles-if-using-office-365"></a>Durchführen einer Rollenbewertung für Office 365-Rollen (bei Verwendung von Office 365)

Bewerten Sie, ob alle Administratorenbenutzer die richtigen Rollen besitzen (löschen Sie Rollen anhand dieser Bewertung, und weisen Sie sie neu zu).

#### <a name="review-the-security-incident-management-approach-used-in-office-365-and-compare-with-your-own-organization"></a>Sich vertraut machen mit dem in Office 365 verwendeten Ansatz zum Sicherheitsvorfallmanagement und Vergleichen mit Ihrer eigenen Organisation

Sie können diesen Bericht von [Sicherheitsvorfallmanagement in Microsoft Office 365](https://www.microsoft.com/download/details.aspx?id=54302) herunterladen.

#### <a name="continue-to-secure-on-premises-privileged-administrative-accounts"></a>Fortsetzen des Schützens von lokalen privilegierten Administratorkonten

Wenn Ihr Azure Active Directory-Mandant mit einem lokalen Active Directory verbunden ist, orientieren Sie sich an dem Leitfaden in [Schützen des privilegierten Zugriffs](https://docs.microsoft.com/windows-server/identity/securing-privileged-access/securing-privileged-access): Phase 2. Dies beinhaltet: Bereitstellen von Privileged Access Workstations für alle Administratoren, Fordern von MFA, Verwenden von „Minimale Administration“ für DC-Wartung, Verkleinern der Angriffsfläche von Domänen, Bereitstellen von ATA zur Angriffserkennung.

### <a name="additional-steps-for-organizations-managing-access-to-azure"></a>Zusätzliche Schritte für Organisationen, die den Zugriff auf Azure verwalten

#### <a name="establish-integrated-monitoring"></a>Einrichten der integrierten Überwachung

Das [Azure Security Center](../../security-center/security-center-intro.md) bietet integrierte Sicherheitsüberwachung und Richtlinienverwaltung für Ihre Azure-Abonnements, hilft bei der Erkennung von Bedrohungen, die andernfalls möglicherweise unbemerkt bleiben, und kann gemeinsam mit einem breiten Spektrum an Sicherheitslösungen verwendet werden.

#### <a name="inventory-your-privileged-accounts-within-hosted-virtual-machines"></a>Inventarisieren Ihrer privilegierten Konten auf gehosteten virtuellen Computern

In den meisten Fällen müssen Sie Benutzern keine uneingeschränkten Berechtigungen für Ihre gesamten Azure-Abonnements oder Ressourcen erteilen. Sie können mit Azure AD-Administratorrollen Aufgaben in Ihrer Organisation verteilen und Benutzern Zugriff nur in dem zur Ausführung bestimmter Aufträge erforderlichen Umfang gewähren. Gestatten Sie z.B. mit Azure AD-Administratorrollen einem Administrator nur die Verwaltung virtueller Computer in einem Abonnement, während ein anderer im gleichen Abonnement SQL-Datenbanken verwalten kann. Weitere Informationen finden Sie unter [Erste Schritte mit der rollenbasierten Zugriffssteuerung im Azure-Portal](../../role-based-access-control/overview.md).

#### <a name="implement-pim-for-azure-ad-administrator-roles"></a>Implementieren von PIM für Azure AD-Administratorrollen

Verwenden Sie Privileged Identity Management mit Azure AD-Administratorrollen zum Verwalten, Steuern und Überwachen des Zugriffs auf Azure-Ressourcen. Das Verwenden von PIM schützt privilegierte Konten vor Cyberangriffen durch Herabsetzen der Gefährdungszeit für Privilegien und Erweitern Ihres Einblicks in ihre Verwendung durch Berichte und Warnungen. Weitere Informationen finden Sie unter [Verwalten des Zugriffs auf Azure-Ressourcen mit Privileged Identity Management](../../role-based-access-control/pim-azure-resource.md).

#### <a name="use-azure-log-integrations-to-send-relevant-azure-logs-to-your-siem-systems"></a>Verwenden von Azure-Protokollintegrationen zum Senden von relevanten Azure-Protokollen an Ihre SIEM-Systeme 

Mit der Azure-Protokollintegration können Sie nicht aufbereitete Protokolle aus Ihren Azure-Ressourcen in die bestehenden SIEM-Systeme (Security Information and Event Management, Sicherheitsinformationen und Ereignisverwaltung) Ihrer Organisation integrieren. Die [Azure-Protokollintegration](../../security/fundamentals/azure-log-integration-overview.md) erfasst Windows-Ereignisse aus Protokollen der Windows-Ereignisanzeige und Azure-Ressourcen aus Azure-Aktivitätsprotokollen, Azure Security Center-Warnungen und Azure-Diagnoseprotokollen. 


### <a name="additional-steps-for-organizations-managing-access-to-other-cloud-apps-via-azure-ad"></a>Zusätzliche Schritte für Organisationen, die den Zugriff auf andere Cloud-Apps über Azure AD verwalten

#### <a name="implement-user-provisioning-for-connected-apps"></a>Implementieren der Benutzerbereitstellung für verbundene Apps

Azure AD ermöglicht Ihnen das automatisierte Erstellen, Warten und Entfernen von Benutzeridentitäten in Cloudanwendungen (SaaS) wie z.B. Dropbox, Salesforce, ServiceNow usw. Weitere Informationen finden Sie unter [Automatisieren der Bereitstellung und Bereitstellungsaufhebung von Benutzern für SaaS-Anwendungen mit Azure Active Directory](../manage-apps/user-provisioning.md).

#### <a name="integrate-information-protection"></a>Integrieren von Information Protection

Mit MCAS können Sie Dateien untersuchen und Richtlinien auf Grundlage von Klassifizierungsbezeichnungen von Azure Information Protection festlegen, sodass Sie umfangreicheren Einblick in Ihre Daten in der Cloud und sie besser unter Kontrolle haben. Überprüfen und klassifizieren Sie Dateien in der Cloud und wenden Sie Azure Information Protection-Bezeichnungen an. Weitere Informationen finden Sie unter [Integration mit Azure Information Protection](https://docs.microsoft.com/cloud-app-security/azip-integration).

#### <a name="configure-conditional-access"></a>Konfigurieren des bedingten Zugriffs

Konfigurieren Sie den bedingten Zugriff basierend auf einer Gruppen-, Standort- und Anwendungsvertraulichkeit für [SaaS-Apps](https://azure.microsoft.com/overview/what-is-saas/) und verbundene Azure AD-Apps. 

#### <a name="monitor-activity-in-connected-cloud-apps"></a>Überwachen der Aktivität in verbundenen Cloud-Apps

Um sicherzustellen, dass der Zugriff des Benutzers auch in verbundenen Anwendungen geschützt ist, sollten Sie [Microsoft Cloud App Security](https://docs.microsoft.com/cloud-app-security/what-is-cloud-app-security) nutzen. Dadurch wird zusätzlich zum Sichern Ihrer Administratorkonten der Zugriff des Unternehmens auf Cloud-Apps sichergestellt, indem Ihnen Folgendes ermöglicht wird:

* Erweitern des Einblicks in Cloud-Apps und der Kontrollmöglichkeiten
* Erstellen von Richtlinien für Zugriff, Aktivitäten und Datenfreigabe
* Automatisches Identifizieren von riskanten Aktivitäten, nicht normalem Verhalten und Bedrohungen
* Verhindern von Datenlecks
* Minimieren von Risiken, automatische Prävention und Durchsetzung von Richtlinien

Der SIEM-Agent von Cloud App Security integriert Cloud App Security in Ihren SIEM-Server, um zentralisierte Überwachung von Office 365-Warnungen und Aktivitäten zu ermöglichen. Er wird auf dem Server ausgeführt, bezieht Warnungen und Aktivitäten aus Cloud App Security und streamt sie an den SIEM-Server. Weitere Informationen finden Sie unter [SIEM-Integration](https://docs.microsoft.com/cloud-app-security/siem).

## <a name="stage-4-continue-building-defenses-to-a-more-proactive-security-posture"></a>Phase 4: Fortsetzung des Aufbaus von Verteidigungsmaßnahmen, um einen proaktiveren Sicherheitsstatus zu schaffen

![Phase 4: Schaffen eines proaktiven Sicherheitsstatus](./media/directory-admin-roles-secure/stage-four.png)

Phase 4 der Roadmap baut auf dem Einblick von Schritt 3 auf und sollte in sechs Monaten und später implementiert werden. Das Aufstellen einer Roadmap hilft Ihnen, starke Schutzmaßnahmen für den privilegierten Zugriff vor potenziellen Angriffen zu entwickeln, die zurzeit bekannt und verfügbar sind. Da Sicherheitsbedrohungen leider ständig weiterentwickelt und verschoben werden, sollten Sie die Sicherheit als einen laufenden Prozess betrachten, der sich darauf konzentriert, für Angreifer, die es auf Ihre Umgebung abgesehen haben, den Aufwand zu steigern und die Erfolgsrate zu verringern.

Schützen von privilegiertem Zugriff ist ein entscheidender Schritt zum Einrichten der Sicherheitsgarantien für Geschäftsressourcen in einem modernen Unternehmen, aber es ist nicht der einzige Teil eines vollständigen Sicherheitsprogramms, das Elemente wie Richtlinien, Vorgänge, Informationssicherheit, Server, Anwendungen, PCs, Geräte, Cloud-Fabric und andere Komponenten beinhaltet, die laufende Sicherheitsgarantien bieten. 

Zusätzlich zur Verwaltung Ihrer Konten für privilegierten Zugriff sollten Sie Folgendes fortlaufend überprüfen:

* Stellen Sie sicher, dass Administratoren ihre Routineaufgaben als Benutzer ohne Privilegien ausführen.
* Gewähren Sie privilegierten Zugriff nur bei Bedarf, und entziehen sie ihn danach (Just-in-Time).
* Sorgen Sie für eine kontinuierliche Überprüfungsaktivität im Zusammenhang mit privilegierten Konten.

Weitere Informationen zum Erstellen einer vollständigen Sicherheitsroadmap finden Sie unter [Ressourcen zur Cloud-IT-Architektur von Microsoft](https://docs.microsoft.com/office365/enterprise/microsoft-cloud-it-architecture-resources). Um weitere Informationen zum Einsatz von Microsoft-Diensten zur Unterstützung bei einem dieser Themen zu erhalten, wenden Sie sich an Ihren Microsoft-Vertriebsbeauftragten, oder besuchen Sie unsere Seite zum [Erstellen von wichtigen Schutzmaßnahmen vor Cyberangriffen zum Schutz Ihres Unternehmens](https://www.microsoft.com/en-us/microsoftservices/campaigns/cybersecurity-protection.aspx).

Diese letzte laufende Phase der Roadmap für geschützten privilegierten Zugriff umfasst die folgenden Komponenten.

### <a name="general-preparation"></a>Allgemeine Vorbereitung

#### <a name="review-admin-roles-in-azure-active-directory"></a>Überprüfen der Administratorrollen in Azure Active Directory 

Bestimmen Sie, ob die aktuellen integrierten Azure AD-Administratorrollen weiterhin auf dem neuesten Stand sind, und stellen Sie sicher, dass die Benutzer sich nur in den Rollen und Delegierungen befinden, die sie für die entsprechenden Berechtigungen benötigen. Mithilfe von Azure AD können Sie verschiedene Administratoren bestimmen, um unterschiedliche Funktionen zu erfüllen. Weitere Informationen finden Sie unter [Zuweisen von Administratorrollen in Azure Active Directory](directory-assign-admin-roles.md).

#### <a name="review-users-who-have-administration-of-azure-ad-joined-devices"></a>Überprüfen von Benutzern, die in Azure AD eingebundene Geräte verwalten

Weitere Informationen finden Sie unter [Konfigurieren von in Azure Active Directory eingebundenen Hybridgeräten](../device-management-hybrid-azuread-joined-devices-setup.md).

#### <a name="review-members-of-built-in-office-365-admin-roleshttpssupportofficecomarticleabout-office-365-admin-roles-da585eea-f576-4f55-a1e0-87090b6aaa9d"></a>Überprüfen der Mitglieder [integrierter Administratorrollen in Office 365](https://support.office.com/article/About-Office-365-admin-roles-da585eea-f576-4f55-a1e0-87090b6aaa9d)
Wenn Sie Office 365 verwenden.
‎
#### <a name="validate-incident-response-plan"></a>Validieren des Notfallreaktionsplans

Um Ihren Plan zu verbessern, empfiehlt Microsoft, dass Sie regelmäßig überprüfen, ob Ihr Plan wie erwartet funktioniert:

* Prüfen Sie Ihre vorhandene Roadmap auf Lücken
* Überarbeiten Sie basierend auf der Post-Mortem-Analyse vorhandene bewährte Methoden oder definieren Sie neue
* Stellen Sie sicher, dass Ihr aktualisierter Notfallreaktionsplan und Ihre bewährten Methoden in der gesamten Organisation verbreitet werden


### <a name="additional-steps-for-organizations-managing-access-to-azure"></a>Zusätzliche Schritte für Organisationen, die den Zugriff auf Azure verwalten 

Bestimmen Sie, ob Sie [den Besitz eines Azure-Abonnements auf ein anderes Konto übertragen müssen](../../billing/billing-subscription-transfer.md).
‎

## <a name="break-glass-what-to-do-in-an-emergency"></a>Was bei einem Notfall zu tun ist

![Konten für den Notfallzugriff](./media/directory-admin-roles-secure/emergency.jpeg)

1. Versorgen Sie Führungskräfte und Sicherheitsbeauftragte mit relevanten Informationen über den Vorfall.

2. Überprüfen Sie Ihr Angriffs-Playbook. 

3. Greifen Sie auf Ihre Kontobenutzername/Kennwort-Kombination für den Notfall zum Anmelden bei Azure AD zu. 

4. Bitten Sie Microsoft um Hilfe, indem Sie [eine Azure-Supportanfrage öffnen](../../azure-supportability/how-to-create-azure-support-request.md).

5. Werten Sie die [Azure AD-Anmeldeberichte aus](../reports-monitoring/overview-reports.md). Es gibt möglicherweise eine Verzögerung zwischen dem Auftreten eines Ereignisses und seiner Einbeziehung in den Bericht.

6. Wenn in Hybridumgebungen der Verbundserver und Ihr AD FS-Server nicht verfügbar sind, müssen Sie möglicherweise vorübergehend von der Verbundauthentifizierung zur Kennworthashsynchronisierung wechseln. Dadurch wird der Domänenverbund auf die verwaltete Authentifizierung zurückgeworfen, bis der AD FS-Server verfügbar ist.

7. Überwachen Sie E-Mail für privilegierte Konten.

8. Stellen Sie sicher, dass Sie Sicherungen der relevanten Protokolle für potenzielle forensische und rechtliche Untersuchungen speichern.

Weitere Informationen zur Behandlung von Sicherheitsvorfällen durch Microsoft Office 365 finden Sie unter [Sicherheitsvorfallmanagement in Microsoft Office 365](https://aka.ms/Office365SIM).

## <a name="faq-common-questions-we-receive-regarding-securing-privileged-access"></a>Häufig gestellte Fragen: allgemeine Fragen zum Schutz des privilegierten Zugriffs  

**F:** Wie gehe ich vor, wenn ich noch keine Komponenten für den sicheren Zugriff implementiert habe?

**Antwort:** Definieren Sie mindestens zwei Notfallkonten, weisen Sie Ihren privilegierten Administratorkonten MFA zu, und trennen Sie Benutzerkonten von globalen Administratorkonten.

**F:** Welches Problem muss nach einer Sicherheitsverletzung zuerst behandelt werden?

**Antwort:** Stellen Sie sicher, dass für Einzelpersonen mit hoher Gefährdung die sicherste Authentifizierung erforderlich ist.

**F:** Was geschieht, wenn unsere privilegierten Administratoren deaktiviert wurden?

**Antwort:** Erstellen Sie ein globales Administratorkonto, das immer auf dem neuesten Stand gehalten wird.

**F:** Was geschieht, wenn nur noch ein einzelner globaler Administrator vorhanden ist und dieser nicht erreicht werden kann? 

**Antwort:** Verwenden Sie eines Ihrer Notfallkonten, um umgehend privilegierten Zugriff zu erhalten.

**F:** Wie kann ich Administratoren innerhalb meiner Organisation schützen?

**Antwort:** Achten Sie darauf, dass Administratoren ihre täglichen Routineaufgaben stets als „unprivilegierte“ Standardbenutzer ausführen.

**F:** Welche Methoden haben sich für das Erstellen von Administratorkonten in Azure AD bewährt?

**Antwort:** Reservieren Sie privilegierten Zugriff für bestimmte Administratoraufgaben.

**F:** Welche Tools stehen zur Reduzierung des beständigen Administratorzugriffs zur Verfügung?

**Antwort:** Privileged Identity Management (PIM) und Azure AD-Administratorrollen.

**F:** Welche Position bezieht Microsoft hinsichtlich der Synchronisierung von Administratorkonten mit Azure AD?

**Antwort:** Administratorkonten der Ebene 0 (einschließlich Konten, Gruppen und anderer Ressourcen, die direkte oder indirekte administrative Kontrolle über AD-Gesamtstruktur, Domänen oder Domänencontroller und alle Ressourcen haben) werden nur für lokale AD-Konten verwendet und in der Regel nicht mit Azure AD für die Cloud synchronisiert.

**F:** Wie können wir verhindern, dass Administratoren im Portal willkürlich Administratorzugriff gewähren?

**Antwort:** Verwenden Sie nicht privilegierte Konten für alle Benutzer und die meisten Administratoren. Beginnen Sie mit der Entwicklung eines Bedarfs der Organisation, um festzustellen, welche Administratorkonten höchstens privilegiert sein sollten. Achten Sie auch auf neu erstellte Benutzer mit Administratorrechten.

## <a name="next-steps"></a>Nächste Schritte

* [Microsoft Trust Center für Produktsicherheit](https://www.microsoft.com/trustcenter/security) – Sicherheitsfeatures von Cloudprodukten und -diensten von Microsoft

* [Microsoft Trust Center – Complianceangebote](https://www.microsoft.com/trustcenter/compliance/complianceofferings) – Microsofts umfassende Reihe von Complianceangeboten für Clouddienste

* [Ausführen einer Risikobeurteilung und Compliancebewertung für Microsoft Cloud Services](https://www.microsoft.com/trustcenter/guidance/risk-assessment) – Verwalten von Sicherheits- und Complianceanforderungen für Clouddienste von Microsoft

### <a name="other-microsoft-online-services"></a>Andere Microsoft Online Services

* [Microsoft Intune-Sicherheit](https://www.microsoft.com/trustcenter/security/intune-security) – Intune bietet cloudbasierte Verwaltungsfunktionen für mobile Geräte, mobile Anwendungen und PCs.

* [Microsoft Dynamics 365](https://www.microsoft.com/trustcenter/security/dynamics365-security) – Dynamics 365 ist die cloudbasierte Lösung von Microsoft, die Funktionen für Kundenbeziehungsmanagement (Customer Relationship Management, CRM) und Enterprise Resource Planning (ERP) vereinheitlicht.
