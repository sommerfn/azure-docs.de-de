---
title: Kit zur Einführung des bedingten Zugriffs – Azure Active Directory
description: Einführen des bedingten Azure AD-Zugriffs für den Zugriff auf Ressourcen
services: active-directory
ms.service: active-directory
ms.subservice: conditional-access
ms.topic: conceptual
ms.date: 06/24/2019
ms.author: joflore
author: MicrosoftGuyJFlo
manager: daveba
ms.reviewer: martinco
ms.collection: M365-identity-device-management
ms.openlocfilehash: c34f59c3e9f679adf8ae410f648cb7de6dba6447
ms.sourcegitcommit: 77bfc067c8cdc856f0ee4bfde9f84437c73a6141
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 10/16/2019
ms.locfileid: "72430043"
---
#  <a name="adopting-azure-ad-conditional-access"></a>Einführen des bedingten Azure AD-Zugriffs

In einer von Mobilgeräten und der Cloud geprägten Welt können Benutzer über verschiedenste Geräte und Anwendungen auf die Ressourcen Ihrer Organisation zugreifen. Daher reicht es nicht mehr aus, sich nur darauf zu konzentrieren, wer Zugriff auf Ihre Ressourcen hat. Sie können steuern, wer Zugriff hat, und ermitteln, wo sich der Benutzer befindet, welches Gerät verwendet wird und vieles mehr.

Für diese Steuerung können Sie mit dem **bedingten Azure Active Directory-Zugriff (AD)** die Bedingungen angeben, die jeder Benutzer für den Zugriff auf eine Anwendung erfüllen muss, z. B. mehrstufige Authentifizierung (MFA). Mit Richtlinien für bedingten Zugriff lässt sich steuern, wie autorisierte Benutzer (Benutzer, denen der Zugriff auf eine Cloud-App gewährt wurde) unter bestimmten Bedingungen auf Cloud-Apps zugreifen können. Weitere Informationen finden Sie unter [Was ist der bedingte Zugriff in Azure Active Directory?](overview.md).

## <a name="key-benefits"></a>Hauptvorteile

Die Verwendung des bedingten Azure AD-Zugriffs bietet folgende Hauptvorteile:

* **Steigerung der Produktivität:** Mithilfe von Richtlinien für bedingten Zugriff können Sie den Punkt festlegen, an dem Benutzer aufgefordert werden, MFA zu verwenden, an dem sie ein vertrauenswürdiges Gerät verwenden müssen oder an dem der Zugriff für Benutzer blockiert wird. Beispielsweise können Sie Richtlinien festlegen, dass Benutzer sich nur außerhalb des Unternehmensnetzwerks über MFA bei einer Anwendung anmelden müssen. Durch die Reduzierung der MFA-Anforderungen können Benutzer produktiver arbeiten, als wenn sie sich bei jeder Anmeldung mit MFA authentifizieren müssen. Darüber hinaus können Sie über den bedingten Azure AD-Zugriff Richtlinien pro Benutzer angeben und außerdem anwendungsspezifische Richtlinien erstellen.
* **Risikomanagement:** Durch Aktivieren von Richtlinien für bedingten Zugriff werden der Identitätsschutz in der Cloud, risikobasierte Zugriffssteuerungsfunktionen und die native mehrstufige Authentifizierung unterstützt. Die Kopplung von bedingtem Zugriff und Identitätsschutz ermöglicht Ihnen zu definieren, wann der Zugriff auf eine Anwendung blockiert oder begrenzt wird.
* **Erfüllung von Compliance und Governance:** Die Überwachung von Zugriffsanforderungen und -genehmigungen für die Anwendung sowie der Einblick in die gesamte Anwendungsnutzung ist mit Azure AD einfacher, da native Überwachungsprotokolle für jede für die Anwendung ausgeführte Zugriffsanforderung unterstützt werden. Die Überwachung beinhaltet die Identität der anfordernden Person, das Anforderungsdatum, die geschäftliche Begründung, den Genehmigungsstatus und die Identität der genehmigenden Person. Diese Daten sind auch über eine API verfügbar, über die der Import dieser Daten in ein SIEM-System (Security Incident and Event Monitoring) Ihrer Wahl ermöglicht wird.
* **Kostenmanagement:** Die Verlagerung von Zugriffsrichtlinien in Azure AD reduziert die Abhängigkeit von benutzerdefinierten oder lokalen Lösungen, z. B. Active Directory-Verbunddienste (AD FS) für bedingten Zugriff, und die Kosten für die Ausführung dieser Infrastruktur.

## <a name="customer-case-studies"></a>Fallstudien

Erfahren Sie, wie die meisten Organisationen mit dem bedingten Azure AD-Zugriff automatisierte Zugriffssteuerungsentscheidungen für den Zugriff auf Cloud-Apps basierend auf Bedingungen definieren und implementieren. Die folgenden Berichte zeigen, wie diese Kundenanforderungen erfüllt werden können.

* [**Wipro** steigert die mobile Produktivität mit Microsoft Cloud-Sicherheitstools zur Verbesserung der Kundenbindung](https://customers.microsoft.com/story/wipro-professional-services-enterprise-mobility-security) – Die Richtlinien für bedingten Zugriff in Azure AD haben es dem Unternehmen ermöglicht, Dokumente, Ressourcen und Anwendungen für vertrauenswürdige externe Entitäten freizugeben – die ihre eigenen Anmeldeinformationen verwenden können – und gleichzeitig die Kontrolle über die Unternehmensdaten zu behalten.
* [**Accenture** sichert seinen Umstieg auf die Cloud mit Microsoft Cloud App Security](https://customers.microsoft.com/story/accenture-professional-services-cloud-app-security) – Accenture evaluiert die Funktion [App-Steuerung für bedingten Zugriff](https://docs.microsoft.com/cloud-app-security/proxy-intro-aad) von Cloud App Security, bei der der bedingte Azure Active Directory-Zugriff verwendet wird, um den Zugriff auf Anwendungen basierend auf bestimmten Bedingungen zu steuern. Laut LePenske kann diese Funktion nützlich sein, um z. B. den schreibgeschützten Dateizugriff zu ermöglichen und gleichzeitig Downloads zu unterbinden.
* [**Aramex** delivery limited – Globales Logistik- und Transportunternehmen erstellt Büro mit Cloudkonnektivität mit einer Lösung zur Identitäts- und Zugriffsverwaltung](https://customers.microsoft.com/story/aramex-azure-active-directory-travel-transportation-united-arab-emirates-en) – Die Gewährleistung eines sicheren Zugriffs war bei den Remotemitarbeitern von Aramex besonders schwierig. Das Unternehmen nutzt jetzt den bedingten Zugriff, damit Remotemitarbeiter von außerhalb des Netzwerks auf die SaaS-Anwendungen zugreifen können. Über die Regel für bedingten Zugriff wird festgelegt, ob die mehrstufige Authentifizierung erzwungen wird, sodass nur autorisierten Personen der entsprechende Zugriff gewährt wird.

Weitere Informationen zu den Erfahrungen von Kunden und Partnern mit dem bedingten Azure AD-Zugriff finden Sie unter [Sehen Sie selbst, welche tollen Möglichkeiten Azure den Benutzern bietet](https://azure.microsoft.com/case-studies/?service=active-directory).

## <a name="announcements"></a>Ankündigungen

Es werden fortlaufend Verbesserungen an Azure AD vorgenommen. Wie Sie bezüglich der aktuellen Entwicklungen auf dem neuesten Stand bleiben, erfahren Sie unter [Neuerungen in Azure Active Directory](../fundamentals/whats-new.md).

Aktuelle Blogs der Tech-Community und der Microsoft Identity Division:

* 24. September 2018: [Bedingter Azure Active Directory-Zugriff in Azure Databricks](https://azure.microsoft.com/updates/azure-active-directory-conditional-access-in-azure-databricks/)
* 21. September 2018: [Benutzerdefinierte Steuerelemente für den bedingten Azure AD-Zugriff jetzt als Public Preview](https://azure.microsoft.com/updates/azure-ad-conditional-access-custom-controls-are-in-public-preview/)
* 21. September 2018: [Unterstützung durch den bedingten Azure AD-Zugriff für den beschränkten Zugriff mit Microsoft Cloud App Security jetzt verfügbar](https://azure.microsoft.com/updates/azure-ad-conditional-access-support-for-limited-access-with-microsoft-cloud-app-security-is-now-available/)
* 21. September 2018: [Bedingter Azure AD-Zugriff: Verwaltete Browserunterstützung für iOS- und Android-Plattformen ab sofort in der Vorschau](https://azure.microsoft.com/updates/azure-ad-conditional-access-managed-browser-support-for-ios-android-platforms-now-in-preview/)
* 21. September 2018: [Bedingter Azure AD-Zugriff für Ländercodes ab sofort als Public Preview](https://azure.microsoft.com/updates/azure-ad-conditional-access-for-country-codes-is-in-public-preview/)
* 21. September 2018: [Azure AD-Nutzungsbedingungen ab sofort verfügbar](https://azure.microsoft.com/updates/azure-ad-terms-of-use-now-available/)

## <a name="learning-resources"></a>Schulungsressourcen

Über die folgenden Links erhalten Sie eine Übersicht über die Funktionsweise des bedingten Azure AD-Zugriffs.

* [Was ist der bedingte Zugriff in Azure Active Directory?](overview.md)
* [Was sind Bedingungen beim bedingten Zugriff in Azure Active Directory?](conditions.md)
* [Was sind Standortbedingungen beim bedingten Zugriff in Azure Active Directory?](location-condition.md)
* [Was sind die Zugriffssteuerungen beim bedingten Zugriff mit Azure Active Directory?](controls.md)
* [Was ist das Was-wäre-wenn-Tool für den bedingten Azure Active Directory-Zugriff?](what-if-tool.md)
* [Best Practices für den bedingten Zugriff in Azure Active Directory](best-practices.md)

Außerdem finden Sie unter den folgenden Links Anleitungen zum Schutz des Zugriffs auf alle Dienste, die in Azure Active Directory integriert sind.

* [Konfigurationen für den Identitäts- und Gerätezugriff](https://docs.microsoft.com/microsoft-365/enterprise/microsoft-365-policies-configurations) – Erfahren Sie, wie Sie den sicheren Zugriff auf Clouddienste über Enterprise Mobility + Security-Produkte konfigurieren können, indem Sie eine empfohlene Umgebung und Konfiguration implementieren, einschließlich eines vorgeschriebenen Satzes von Richtlinien für bedingten Zugriff und zugehöriger Funktionen.
* [Referenz zu den Einstellungen für den bedingten Azure Active Directory-Zugriff](technical-reference.md) – Inhalt:
   * Welche Apps nutzen den bedingten Zugriff?
   * Für welche Dienste ist der bedingte Zugriff aktiviert?
* [Aktivieren von bedingtem Zugriff mit Azure Active Directory zum Absichern des Benutzerzugriffs](https://www.youtube.com/watch?v=eLAYBwjCGoA) – In diesem Video erfahren Sie, welche Rolle der bedingte Zugriff in anderen Workloads von Enterprise Mobility + Security spielt.

### <a name="training-videos"></a>Schulungsvideos

**Bedingter Zugriff in Enterprise Mobility + Security**
   > [!VIDEO https://www.youtube.com/embed/A7IrxAH87wc]

**Gerätebasierter bedingter Zugriff**
   > [!VIDEO https://www.youtube.com/embed/AdM0zYB-3WQ]

**Aktivieren von bedingtem Zugriff mit Azure Active Directory zum Absichern des Benutzerzugriffs**
   > [!VIDEO https://www.youtube.com/embed/eLAYBwjCGoA]

### <a name="online-courses"></a>Onlinekurse

Folgende Kurse zum bedingten Zugriff und weiteren Themen finden Sie unter [Pluralsight.com](https://www.pluralsight.com/):

* Pluralsight.com: [Design Identity Management in Microsoft Azure](https://www.pluralsight.com/courses/microsoft-azure-identity-management-design) (Entwerfen der Identitätsverwaltung in Microsoft Azure)
   * In diesem Kurs werden die wichtigsten Punkte erläutert, die Sie für die Entwicklung Ihrer Identitätsverwaltungslösung mit Azure AD kennen sollten. Der bedingte Azure AD-Zugriff wird im Modul „Using Roles and Access Control with Azure AD“ (Verwenden von Rollen und der Zugriffssteuerung in Azure AD) behandelt.

* Pluralsight.com: [Design Authentication for Microsoft Azure](https://www.pluralsight.com/courses/microsoft-azure-authentication-design) (Entwerfen der Authentifizierung für Microsoft Azure)
   * In diesem Kurs wird erläutert, wie Sie mit Azure AD alle Ihre Anforderungen an die Cloudauthentifizierung erfüllen können. Der bedingte Azure AD-Zugriff wird im Modul „Authentication Requirements for Different Scenarios“ (Authentifizierungsanforderungen für verschiedene Szenarien) behandelt.

* Pluralsight.com: [Design Authorization for Microsoft Azure](https://www.pluralsight.com/courses/microsoft-azure-authorization-design) (Entwerfen der Autorisierung für Microsoft Azure)
   * In diesem Kurs werden Autorisierungsoptionen beschrieben, die in Azure und Azure AD zur Verfügung stehen. Der bedingte Azure AD-Zugriff wird im Modul „Authorization with Azure Resource Manager and Azure AD“ (Autorisierung mit Azure Resource Manager und Azure AD) behandelt.

### <a name="books"></a>Bücher

* O'Reilly, [Implementing Azure Solutions – Second Edition](https://www.oreilly.com/library/view/implementing-azure-solutions/9781789343045/b7ead3db-eb1c-4ace-897e-86ee25ea86be.xhtml)
   * Machen Sie sich mit Azure-Diensten vertraut, und erfahren Sie, wie Sie sie in Ihrer Organisation implementieren. Der bedingte Azure AD-Zugriff wird im Kapitel [Deploying and Synchronizing Azure Active Directory](https://learning.oreilly.com/library/view/implementing-azure-solutions/9781789343045/02ca8bba-08cf-4691-a7d0-1b96e286e7ea.xhtml) (Bereitstellung und Synchronisation von Azure Active Directory) behandelt.

* Wiley, [Mastering Microsoft Azure Infrastructure Services](https://www.wiley.com/Mastering+Microsoft+Azure+Infrastructure+Services-p-9781119003298)
   * Hier finden Sie alles, was Sie zum Verstehen, Auswerten, Bereitstellen und Verwalten von Umgebungen benötigen, in denen Microsoft Azure verwendet wird.

## <a name="white-papers"></a>Whitepaper

* Veröffentlicht am 18. Dezember 2018: [Erstellen einer robusten Verwaltungsstrategie für die Zugriffssteuerung in Azure Active Directory](../authentication/concept-resilient-controls.md)
   * Dieses Dokument enthält Anleitungen zu Strategien, die eine Organisation übernehmen kann, um das Risiko der Sperrung während unvorhergesehener Störungen zu reduzieren.

* Veröffentlicht am 18. September 2018: [Ressourcen zum Migrieren von Anwendungen zu Azure Active Directory](../manage-apps/migration-resources.md)
   * Dieses Whitepaper enthält eine Liste der Ressourcen, die Ihnen beim Migrieren des Anwendungszugriffs und der Authentifizierung zu Azure Active Directory (Azure AD) helfen.

* Veröffentlicht am 12. Juli 2018: [Azure-Blaupause für Sicherheit und Compliance: Hosten von PaaS-Webanwendungen für UK OFFICIAL-Workloads](../../security/blueprints/ukofficial-paaswa-overview.md)
   * Azure-Vorlagen (Blueprints) umfassen Leitfäden und Automatisierungsvorlagen, mit denen cloudbasierte Architekturen bereitgestellt werden, um Lösungen für Szenarien zu ermöglichen, für die Akkreditierungs- oder Konformitätsanforderungen gelten.

## <a name="guidance-for-it-administrators"></a>Anleitungen für IT-Administratoren

Melden Sie sich beim [Azure-Portal](https://portal.azure.com/) als globaler Administrator, Sicherheitsadministrator oder Administrator für bedingten Zugriff an. Informationen finden Sie unter [Berechtigungen der Administratorrolle in Azure Active Directory](../users-groups-roles/directory-assign-admin-roles.md).

Verwenden Sie als IT-Administrator den [bedingten Azure AD-Zugriff](overview.md), um für Benutzer die Durchführung einer mehrstufigen Authentifizierung oder die Anmeldung über ein vertrauenswürdiges Netzwerk oder vertrauenswürdiges Gerät zu erzwingen.

Nachfolgend finden Sie nützliche Links, die Ihnen den Einstieg erleichtern:

* [Best Practices für den bedingten Zugriff in Azure Active Directory](best-practices.md)
* [Verwenden von Azure AD-Zugriffsüberprüfungen zum Verwalten von Benutzern, die aus Richtlinien für den bedingten Zugriff ausgeschlossen wurden](../governance/conditional-access-exclusion.md)
* [How To: Planen der Bereitstellung von bedingtem Zugriff in Azure Active Directory](plan-conditional-access.md)
* [Schnellstart: Anfordern der mehrstufigen Authentifizierung (Multi-Factor Authentication, MFA) für bestimmte Apps über den bedingten Zugriff von Azure Active Directory](app-based-mfa.md)
* [Schnellstart: Fordern der Annahme von Nutzungsbedingungen vor dem Zugreifen auf Cloud-Apps](require-tou.md)
* [Schnellstart: Blockieren des Zugriffs, wenn ein Sitzungsrisiko beim bedingten Azure Active Directory-Zugriff erkannt wird](app-sign-in-risk.md)
* [Häufig gestellte Fragen zum bedingten Zugriff mit Azure Active Directory](faqs.md)
   * Antworten auf weitere Fragen finden Sie außerdem im [MSDN-Forum](https://social.msdn.microsoft.com/Forums/home?forum=WindowsAzureAD&sort=relevancedesc&brandIgnore=True&searchTerm=password+reset+azure).
   * Wenn Sie keine Antwort auf ein Problem finden, stehen Ihnen unsere Supportteams immer für weitere Unterstützung zur Verfügung. Nutzen Sie dazu [Microsoft-Support kontaktieren](../authentication/active-directory-passwords-troubleshoot.md#contact-microsoft-support).

### <a name="tutorials"></a>Lernprogramme

* [**Schnellstart: Anfordern der mehrstufigen Authentifizierung (Multi-Factor Authentication, MFA) für bestimmte Apps über den bedingten Zugriff von Azure Active Directory**](app-based-mfa.md)
   * Dieser Schnellstart veranschaulicht das Konfigurieren einer Azure AD-Richtlinie für bedingten Zugriff, durch die für eine ausgewählte Cloud-App in Ihrer Umgebung die mehrstufige Authentifizierung erforderlich wird.

* [**Schnellstart: Fordern der Annahme von Nutzungsbedingungen vor dem Zugreifen auf Cloud-Apps**](require-tou.md)
   * Dieser Schnellstart veranschaulicht das Konfigurieren einer Azure AD-Richtlinie für bedingten Zugriff, durch die für eine ausgewählte Cloud-App in Ihrer Umgebung erforderlich ist, dass Nutzungsbedingungen akzeptiert werden.

* [**Schnellstart: Blockieren des Zugriffs, wenn ein Sitzungsrisiko beim bedingten Azure Active Directory-Zugriff erkannt wird**](app-sign-in-risk.md)
   * In diesem Schnellstart erfahren Sie, wie Sie eine Richtlinie für bedingten Zugriff konfigurieren, mit der eine Anmeldung blockiert wird, wenn eine konfigurierte Risikostufe für die Anmeldung erkannt wird.

* [Tutorial: **Migrieren einer klassischen Richtlinie, die mehrstufige Authentifizierung erfordert, im Azure-Portal**](policy-migration-mfa.md)
   * Dieses Tutorial zeigt, wie Sie eine klassische Richtlinie migrieren, die eine mehrstufige Authentifizierung (MFA) für eine Cloud-App erfordert.

## <a name="end-user-readiness-and-communication"></a>Endbenutzerbereitschaft und Kommunikation

Für den bedingten Zugriff werden andere Azure AD-Funktionen verwendet, die sich möglicherweise auf die Endbenutzererfahrung auswirken. Beispielsweise können Sie Azure Multi-Factor Authentication verwenden, um die strenge Authentifizierung für Benutzer zu aktivieren. In diesem Fall verwenden Sie die Vorlagen von Azure MFA für Endbenutzer.

## <a name="next-steps"></a>Nächste Schritte

* Beginnen Sie mit der Bereitstellung mit der [Dokumentation zum Planen der Bereitstellung von bedingtem Zugriff in Azure Active Directory](plan-conditional-access.md).
