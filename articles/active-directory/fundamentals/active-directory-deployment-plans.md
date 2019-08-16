---
title: Bereitstellungspläne – Azure Active Directory | Microsoft-Dokumentation
description: Umfassende Anleitungen für die Bereitstellung vieler Azure Active Directory-Funktionen
services: active-directory
author: msaburnley
manager: daveba
ms.service: active-directory
ms.subservice: fundamentals
ms.workload: identity
ms.topic: conceptual
ms.date: 02/08/2019
ms.author: ajburnle
ms.custom: it-pro, seodec18
ms.collection: M365-identity-device-management
ms.openlocfilehash: a19c78efb7d81dcdcd3c221f58d4e4cc7be5ccd1
ms.sourcegitcommit: 85b3973b104111f536dc5eccf8026749084d8789
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 08/01/2019
ms.locfileid: "68722191"
---
# <a name="azure-active-directory-deployment-plans"></a>Azure Active Directory-Bereitstellungspläne
Sie suchen nach umfassenden Anleitungen für die Bereitstellung von Azure Active Directory-Funktionen (Azure AD)? In den folgenden Bereitstellungsplänen werden der Geschäftswert, Planungsüberlegungen, der Entwurf und Betriebsverfahren erläutert, die Sie zur erfolgreichen Bereitstellung einiger der gängigsten Azure AD-Funktionen benötigen. 

In den Dokumenten finden Sie E-Mail-Vorlagen, Systemarchitekturdiagramme, allgemeine Testfälle und vieles mehr. 

Wir freuen uns über Ihr Feedback zu den Dokumenten. Bitten nehmen Sie an dieser kurzen [Umfrage](https://aka.ms/deploymentplanfeedback) teil, und teilen Sie uns mit, wie hilfreich die Dokumente für Sie waren. 

## <a name="include-the-right-stakeholders"></a>Einbeziehen der richtigen Projektbeteiligten

Wenn Sie beginnen, eine neue Funktion für Ihre Bereitstellung zu planen, ist es wichtig, die wichtigsten Projektbeteiligten in Ihrer Organisation einzubeziehen. Wir empfehlen, die Person bzw. Personen zu identifizieren, die jede der folgenden Rollen erfüllen, und in Zusammenarbeit mit ihnen deren Beteiligung am Projekt zu bestimmen.  

Dazu zählen u.a. die folgenden Rollen: 

|Role |BESCHREIBUNG |
|-|-|
|Endbenutzer|Eine repräsentative Gruppe von Benutzern, für welche die Funktion implementiert wird. Erkundet häufig vorher die Änderungen in einem Pilotprogramm.
|IT-Supportmanager|Ein Vertreter der IT-Supportorganisation, der zu den Unterstützungsmöglichkeiten dieser Änderung aus einer Helpdesk-Perspektive Stellung nehmen kann.  
|Identitätsarchitekt oder globaler Azure-Administrator|Vertreter des Identitätsverwaltungsteams, der definieren muss, wie diese Änderung an die Kerninfrastruktur für die Identitätsverwaltung in Ihrer Organisation gekoppelt ist.|
|Geschäftlicher Anwendungsbesitzer |Der allgemeine geschäftliche Besitzer der betroffenen Anwendung(en), ggf. mit Zugriffsverwaltung.  Kann auch Aussagen zur Benutzerfreundlichkeit und Zweckmäßigkeit dieser Änderung aus Sicht eines Endbenutzers machen.
|Sicherheitsbesitzer|Ein Vertreter des Sicherheitsteams, der abzeichnen kann, dass der Plan die Sicherheitsanforderungen Ihrer Organisation erfüllt.|
|Compliance-Manager|Die Person in Ihrem Unternehmen, die für die Sicherstellung der Konformität mit Unternehmens-, Branchen- oder behördlichen Anforderungen verantwortlich ist.|

**Die Ebenen der Beteiligung und Verantwortlichkeiten können wie folgt definiert werden:**

- **R**esponsible – Verantwortlich für die Implementierung von Projektplan und Ergebnis 

- **A**ccountable – Rechenschaftspflichtig. Genehmigt Projektplan und Ergebnis 

- **C**onsulted – Konsultiert. Wird zu Projektplan und Ergebnis befragt 

- **I**nformed – Zu informieren. Wird über Projektplan und Ergebnis informiert
 
## <a name="deployment-plans"></a>Bereitstellungspläne



|Szenario |BESCHREIBUNG |
|-|-|
|[Multi-Factor Authentication](../authentication/howto-mfa-getstarted.md)|Azure Multi-Factor Authentication (MFA) ist die Lösung von Microsoft für die zweistufige Überprüfung. Mit vom Administrator genehmigten Authentifizierungsmethoden trägt Azure MFA zum Schutz Ihres Zugriffs auf Daten und Anwendungen bei, während gleichzeitig die Forderung nach einem einfachen Anmeldeprozess erfüllt wird.|
|[Bedingter Zugriff](https://aka.ms/deploymentplans/ca)|Mit dem bedingten Zugriff können Sie basierend auf bestimmten Bedingungen automatisierte Entscheidungen hinsichtlich der Zugriffssteuerung für den Zugriff auf Ihre Cloud-Apps implementieren.|
|[Self-Service-Kennwortzurücksetzung](https://aka.ms/deploymentplans/sspr)|Mit der Self-Service-Kennwortzurücksetzung können Ihre Benutzer jederzeit ihr Kennwort zurücksetzen, ohne dass ein Administrator eingreifen muss.|
|[Privileged Identity Management](https://aka.ms/deploymentplans/pim)|Azure AD Privileged Identity Management (PIM) dient zur Verwaltung von privilegierten Administratorrollen in Azure AD, Azure-Ressourcen und anderen Microsoft-Onlinediensten. PIM bietet Lösungen wie Just-In-Time-Zugriff, Workflows für die Anforderungsgenehmigung und vollständig integrierte Zugriffsüberprüfungen, damit Sie schädliche Aktivitäten von privilegierten Rollen in Echtzeit identifizieren, aufdecken und verhindern können.|
|[Einmaliges Anmelden](https://aka.ms/deploymentplans/sso)|Einmaliges Anmelden (Single Sign-On, SSO) ermöglicht Ihnen mit nur einer Anmeldung und einem einzigen Benutzerkonto den Zugriff auf sämtliche Apps und Ressourcen, die Sie für Ihre Geschäftsaktivitäten benötigen. Nach der Anmeldung können Sie von Microsoft Office zu Salesforce oder zu Box wechseln, ohne sich ein zweites Mal (beispielsweise durch Eingabe eines Kennworts) authentifizieren zu müssen.|
|[Nahtloses einmaliges Anmelden](https://aka.ms/SeamlessSSODPDownload)|Die nahtlose einmalige Anmeldung mit Azure Active Directory (nahtlose SSO mit Azure AD) meldet Benutzer automatisch auf ihren Unternehmensgeräten an, die mit dem Unternehmensnetzwerk verbunden sind. Wenn Sie dieses Feature aktivieren, müssen Benutzer zur Anmeldung bei Azure AD nicht ihr Kennwort und in der Regel nicht einmal ihren Benutzernamen eingeben. Diese Funktion ermöglicht Ihren Benutzern einen einfachen Zugriff auf Ihre cloudbasierten Anwendungen, ohne dass zusätzliche lokale Komponenten erforderlich sind.|
|[Zugriffsbereich](https://aka.ms/AccessPanelDPDownload)|Stellen Sie für Ihre Benutzer einen einfachen zentralen Ort zur Verfügung, über den diese alle Anwendungen ermitteln und darauf zugreifen können. Steigern Sie mit Self-Service-Funktionen, z.B. der Möglichkeit zum Anfordern des Zugriffs auf neue Apps und Gruppen oder zum Verwalten des Zugriffs auf diese Ressourcen im Namen von anderen Personen, die Produktivität Ihrer Mitarbeiter.|
|[Verteiltes Azure-Dateisystem (ADFS) für Kennworthashsynchronisierung](https://aka.ms/deploymentplans/adfs2phs)|Mit der Kennworthashsynchronisierung werden Hashes von Benutzerkennwörtern aus dem lokalen Active Directory mit Azure AD synchronisiert, sodass Azure AD Benutzer ohne Interaktion mit dem lokalen Active Directory authentifizieren kann.|
|[Verteiltes Azure-Dateisystem (ADFS) für Passthrough-Authentifizierung](https://aka.ms/deploymentplans/adfs2pta)|Mit der Azure AD-Passthrough-Authentifizierung können sich Benutzer mit denselben Kennwörtern sowohl bei lokalen als auch cloudbasierten Anwendungen anmelden. Diese Funktion stellt eine benutzerfreundlichere Oberfläche für Ihre Benutzer bereit, weil ein Kennwort wegfällt, und reduziert die Kosten des IT-Helpdesks, da Benutzer seltener vergessen, wie sie sich anmelden. Wenn Benutzer sich mithilfe von Azure AD anmelden, überprüft dieses Feature die Benutzerkennwörter direkt anhand Ihres lokalen Active Directory.|
|[Azure AD-Anwendungsproxy](https://aka.ms/deploymentplans/appproxy)|Heutzutage möchten Mitarbeiter an jedem Ort, zu jeder Zeit und mit jedem Gerät produktiv arbeiten können. Sie möchten auf ihren eigenen Geräten arbeiten, beispielsweise auf Tablets, Smartphones oder Laptops. Zudem erwarten Mitarbeiter, sowohl auf SaaS-Apps in der Cloud als auch auf lokale Unternehmens-Apps zugreifen zu können. Für die Bereitstellung des Zugriffs auf lokale Anwendungen wurden bisher virtuelle private Netzwerke (VPNs) oder Umkreisnetzwerke (demilitarisierte Zonen, DMZs) eingesetzt. Diese Lösungen sind aber nicht nur komplex und schwer zu schützen, sondern können außerdem nur mit hohem Kostenaufwand eingerichtet und verwaltet werden. Es gibt eine bessere Möglichkeit! - Azure AD-Anwendungsproxy|
|[Benutzerbereitstellung](https://aka.ms/UserProvisioningDPDownload)|Mit Azure AD können Sie die Erstellung, Wartung und Entfernung von Benutzeridentitäten in Cloudanwendungen (SaaS) wie Dropbox, Salesforce, ServiceNow usw. automatisieren.|
|[Workday-driven Inbound User Provisioning](https://aka.ms/WorkdayDeploymentPlan) (Workday-gesteuerte eingehende Benutzerbereitstellung)|Bei der Workday-gesteuerten eingehenden Benutzerbereitstellung für Active Directory wird die Grundlage für eine fortlaufende Identitätsgovernance geschaffen und die Qualität von Geschäftsprozessen verbessert, die auf autoritativen Identitätsdaten basieren. Mit diesem Feature können Sie den Identitätslebenszyklus von Mitarbeitern und Zeitarbeitern nahtlos verwalten, indem Sie Regeln konfigurieren, mit denen Mitarbeiterprozesse (z.B. Neueinstellung, Kündigung, Wechsel) IT-Bereitstellungsaktionen (z.B. Konten erstellen, aktivieren, deaktivieren, löschen) zugeordnet werden.|
|[Berichterstellung und Überwachung](https://aka.ms/deploymentplans/reporting)| Der Entwurf Ihrer Azure AD-Lösung für die Berichterstellung und Überwachung hängt von gesetzlichen, betrieblichen und sicherheitstechnischen Anforderungen sowie von vorhandenen Umgebungen und Prozessen ab. In diesem Artikel werden die unterschiedlichen Entwurfsoptionen vorgestellt. Zudem dient er als Richtlinie zur Ausarbeitung der geeigneten Bereitstellungsstrategie.|
