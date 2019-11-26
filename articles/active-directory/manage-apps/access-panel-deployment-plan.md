---
title: Planen einer Azure Active Directory-Bereitstellung eines Zugriffsbereichs
description: Anleitung zum Bereitstellen des Azure Active Directory-Zugriffsbereichs
services: active-directory
author: barbaraselden
manager: CelesteDG
ms.assetid: ''
ms.service: active-directory
ms.subservice: app-mgmt
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 09/27/2019
ms.author: baselden
ms.reviewer: mimart
ms.collection: M365-identity-device-management
ms.openlocfilehash: 99c52f65bdec2b164cca86a6346d8865d210cf38
ms.sourcegitcommit: 98ce5583e376943aaa9773bf8efe0b324a55e58c
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 10/30/2019
ms.locfileid: "73176066"
---
# <a name="plan-an-azure-active-directory-access-panel-deployment"></a>Planen einer Azure Active Directory-Bereitstellung eines Zugriffsbereichs

Der Azure AD-Zugriffsbereich (Azure Active Directory) ist ein webbasiertes Portal, mit dem Sie die Supportkosten senken, die Produktivität und Sicherheit erhöhen und die Frustration der Benutzer verringern können. Das System verfügt über eine ausführliche Berichterstellung, bei der Ihr Zugriff auf das System nachverfolgt wird, sowie über eine Benachrichtigungsfunktion, mit der Administratoren über eine fehlerhafte oder missbräuchliche Nutzung informiert werden.

Der Azure AD-Zugriffsbereich bietet Ihnen folgende Möglichkeiten:

* Entdecken aller mit Azure AD verbundenen Ressourcen Ihres Unternehmens, z. B. Anwendungen, und Zugriff auf diese
* Anfordern von Zugriff auf neue Apps und Gruppen
* Verwalten des Zugriffs auf diese Ressourcen für andere Benutzer
* Verwalten der Self-Service-Kennwortzurücksetzung und der Einstellungen für Azure Multi-Factor Authentication
* Verwalten Ihrer Geräte

Administratoren können damit Folgendes verwalten:

* Vertragsbedingungen
* Organisationen
* Zugriffsüberprüfungen


## <a name="benefits-of-azure-ad-access-panel-integration"></a>Vorteile der Azure Active Directory-Zugriffsbereichsintegration

Der Azure AD-Zugriffsbereich bietet Unternehmen folgende Vorteile:

**Bietet eine intuitive Benutzeroberfläche**: Der Zugriffsbereich bietet eine einzige Plattform für alle mit einmaligem Anmelden (SSO) in Azure verbundenen Anwendungen. Sie verfügen über ein einheitliches Portal, um vorhandene Einstellungen und neue Funktionen, z. B. Gruppenverwaltung und Self-Service-Kennwortzurücksetzung, zu finden, sobald sie hinzugefügt wurden. Die intuitive Benutzeroberfläche ermöglicht es den Benutzern, schneller wieder an die Arbeit zu gehen und produktiver zu sein, während sie gleichzeitig ihre Frustration reduziert.

**Steigerung der Produktivität**: Für alle Benutzeranwendungen im Zugriffsbereich ist SSO aktiviert. Die Aktivierung von SSO zwischen Unternehmensanwendungen und Office 365 bietet eine optimierte Anmeldung, da hierdurch die Anzahl von zusätzlichen Anmeldeeingabeaufforderungen reduziert wird oder diese gänzlich beseitigt werden. Im Zugriffsbereich werden Self-Service und dynamische Mitgliedschaft verwendet, und die Gesamtsicherheit Ihres Identitätsverwaltungssystems wird verbessert. Dies stellt sicher, dass der Zugriff auf die Anwendungen von den richtigen Personen verwaltet wird. Der Zugriffsbereich dient als kohärente Landing Page, damit Sie schnell Ressourcen finden und Arbeitsaufgaben fortsetzen können.

**Verwaltung der Kosten**: Die Aktivierung des Zugriffsbereichs mit Azure AD kann die Desinvestition von lokalen Infrastrukturen unterstützen. Dadurch werden die Supportkosten gesenkt, da Sie über ein einheitliches Portal verfügen, um alle Ihre Anwendungen zu finden, den Zugriff auf Ressourcen anzufordern und Konten zu verwalten.

**Steigerung von Flexibilität und Sicherheit**: Über den Zugriffsbereich erhalten Sie die Sicherheit und Flexibilität, die eine Cloudplattform bietet. Administratoren können Einstellungen von Anwendungen und Ressourcen einfach ändern und neue Sicherheitsanforderungen erfüllen, ohne die Benutzer zu beeinträchtigen.

**Ermöglichen einer zuverlässigen Überwachung und Nutzungsnachverfolgung**: Durch die Überwachung und Nutzungsnachverfolgung für alle Benutzerfunktionen erfahren Sie, wann Benutzer ihre Ressourcen nutzen, und Sie können die Sicherheit bewerten.

### <a name="licensing-considerations"></a>Lizenzierungsaspekte

Der Zugriffsbereich ist kostenlos und erfordert auf der Ebene „Basic“ keine Lizenzen. Die Anzahl der Objekte in Ihrem Verzeichnis und die zusätzlichen Funktionen, die Sie bereitstellen möchten, können jedoch zusätzliche Lizenzen erfordern. Allgemeine Azure AD-Szenarien mit Lizenzanforderungen umfassen die folgenden Sicherheitsfeatures:

* [Azure Multi-Factor Authentication](https://docs.microsoft.com/azure/multi-factor-authentication/multi-factor-authentication-how-it-works)

* [Gruppenbasierte Mitgliedschaft](https://docs.microsoft.com/azure/active-directory/active-directory-manage-groups)

* [Self-Service-Kennwortzurücksetzung](https://docs.microsoft.com/azure/active-directory/authentication/quickstart-sspr)

* [Azure Active Directory Identity Protection](https://docs.microsoft.com/azure/active-directory/active-directory-identityprotection)

Weitere Informationen finden Sie im [Leitfaden zur Lizenzierung für Azure AD](https://azure.microsoft.com/pricing/details/active-directory/).

### <a name="prerequisites-for-deploying-the-azure-ad-access-panel"></a>Voraussetzungen für die Bereitstellung des Azure AD-Zugriffsbereichs

Bevor Sie mit diesem Projekt beginnen, müssen folgende erforderliche Schritte ausgeführt werden:

* [Integrieren von Anwendungs-SSO](https://docs.microsoft.com/azure/active-directory/manage-apps/plan-sso-deployment)

* [Verwalten der Benutzer- und Gruppeninfrastruktur von Azure AD](https://docs.microsoft.com/azure/active-directory/fundamentals/active-directory-manage-groups)

## <a name="plan-the-azure-ad-access-panel-deployment"></a>Planen der Bereitstellung des Azure AD-Zugriffsbereichs

In der folgenden Tabelle sind die wichtigsten Anwendungsfälle für eine Bereitstellung des Zugriffsbereichs aufgeführt:

| Bereich| BESCHREIBUNG |
| - | - |
| Access| Das Portal des Zugriffsbereichs ist von firmeneigenen und privaten Geräten im Unternehmensnetzwerk aus zugänglich. |
|Access | Das Portal des Zugriffsbereichs ist von firmeneigenen Geräten von außerhalb des Unternehmensnetzwerks aus zugänglich. |
| Überwachung| Die Nutzungsdaten werden mindestens alle 29 Tage in die Unternehmenssysteme heruntergeladen. |
| Governance| Der Lebenszyklus der Benutzerzuweisungen zu Anwendungen und Gruppen, die mit Azure AD verbunden sind, wird definiert und überwacht. |
| Sicherheit| Der Zugriff auf Ressourcen wird über die Zuweisung von Benutzern und Gruppen gesteuert. Nur autorisierte Benutzer können den Ressourcenzugriff verwalten. |
| Leistung| Die Zeitachsen für die Verteilung von Zugriffszuweisungen werden dokumentiert und überwacht. |
| Benutzererfahrung| Die Benutzer sind sich der Funktionen des Zugriffsbereichs und deren Verwendung bewusst.|
| Benutzererfahrung| Benutzer können ihren Zugriff auf Anwendungen und Gruppen verwalten.|
| Benutzererfahrung| Benutzer können ihre Konten verwalten. |
| Benutzererfahrung| Die Benutzer sind sich der Browserkompatibilität bewusst. |
| Support| Benutzer können Unterstützung bei Problemen mit dem Zugriffsbereich finden. |

### <a name="best-practices-for-deploying-the-azure-ad-access-panel"></a>Bewährte Methoden für die Bereitstellung des Azure AD-Zugriffsbereichs

Die Funktionalität des Zugriffsbereichs kann schrittweise aktiviert werden. Wir empfehlen die folgende Reihenfolge bei der Bereitstellung:

1. Meine Apps
   * App-Startfeld
   * Self-Service-App-Verwaltung
   * Microsoft Office 365-Integration

1. Self-Service-App-Ermittlung
   * Self-Service-Kennwortzurücksetzung
   * Einstellungen für die mehrstufige Authentifizierung
   * Geräteverwaltung
   * Nutzungsbedingungen
   * Verwalten von Organisationen

1. Eigene Gruppen
   * Self-Service-Gruppenverwaltung
1. Zugriffsüberprüfungen
   * Verwaltung der Zugriffsüberprüfung

Da mit „Meine Apps“ begonnen wird, erhalten Benutzer eine Einführung in das Portal als allgemeinen Ort für den Zugriff auf Ressourcen. Die Erweiterung um die Self-Service-Anwendungsermittlung baut auf der Erfahrung mit „Meine Apps“ auf. „Meine Gruppen“ und Zugriffsüberprüfungen bauen auf den Self-Service-Funktionen auf.

### <a name="plan-configurations-for-the-azure-ad-access-panel"></a>Planen von Konfigurationen für den Azure AD-Zugriffsbereich

In der folgenden Tabelle sind einige wichtige Zugriffsbereichskonfigurationen und die üblichen Werte aufgeführt, die Sie möglicherweise verwenden:

| Konfiguration| Typische Werte |
| - | - |
| Ermitteln der Pilotgruppen| Identifizieren Sie die zu verwendende Azure AD-Sicherheitsgruppe und stellen Sie sicher, dass alle Pilotmitglieder Teil der Gruppe sind. |
| Ermitteln Sie die für die Produktion zu aktivierende(n) Gruppe(n).| Identifizieren Sie die zu verwendenden Azure AD-Sicherheitsgruppen oder Azure AD-Gruppen, die mit Azure AD synchronisiert sind. Stellen Sie sicher, dass alle Pilotmitglieder Teil der Gruppe sind. |
| Benutzern die Verwendung von SSO für bestimmte Anwendungstypen erlauben| Verbund-SSO, OAuth, Kennwort-SSO, App-Proxy |
| Benutzern die Verwendung der Self-Service-Kennwortzurücksetzung erlauben | Ja |
| Benutzern die Verwendung der mehrstufigen Authentifizierung erlauben| Ja |
| Benutzern die Verwendung der Self-Service-Gruppenverwaltung für bestimmte Arten von Gruppen erlauben| Sicherheitsgruppen, Office 365-Gruppen |
| Benutzern die Verwendung der Self-Service-App-Verwaltung erlauben| Ja |
| Benutzern die Verwendung von Zugriffsüberprüfungen erlauben| Ja |

### <a name="plan-consent-strategy"></a>Planen von Einwilligungsstrategien

Benutzer oder Administratoren müssen den Nutzungsbedingungen und den Datenschutzrichtlinien jeder Anwendung zustimmen. Wenn möglich, sollten Sie nach Ihren Geschäftsregeln die Einwilligung des Administrators einholen, was den Benutzern ein besseres Erlebnis bietet.

Damit Sie die Einwilligung des Administrators nutzen können, müssen Sie ein globaler Administrator der Organisation sein, und die Anwendungen müssen eine der folgenden Anforderungen erfüllen:

* In Ihrer Organisation registriert

* In einem anderen Azure AD-Mandanten registriert, und mindestens ein Benutzer hat seine Einwilligung erteilt

Weitere Informationen finden Sie unter [Konfigurieren der Art der Benutzereinwilligung für eine Anwendung in Azure Active Directory](configure-user-consent.md).

### <a name="engage-the-right-stakeholders"></a>Einbeziehen der richtigen Beteiligten

Fehler in Technologieprojekten sind in der Regel auf nicht erfüllte Erwartungen auf den Gebieten Auswirkungen, Ergebnisse und Zuständigkeiten zurückzuführen. Um diese Fallstricke zu vermeiden, stellen Sie sicher, dass Sie die [richtigen Beteiligten hinzuziehen](../fundamentals/active-directory-deployment-plans.md) und dass die Rollen der Beteiligten im Projekt gut verstanden werden.

### <a name="plan-communications"></a>Planen der Benachrichtigungen

Kommunikation ist ein kritischer Faktor für den Erfolg jedes neuen Diensts. Informieren Sie die Benutzer proaktiv darüber, wie und wann sich ihre Erfahrungen ändern werden und wie sie bei Bedarf Unterstützung erhalten können.

Obwohl der Zugriffsbereich in der Regel keine Benutzerprobleme verursacht, ist es wichtig, sich darauf vorzubereiten. Erstellen Sie vor dem Start Leitfäden und eine Liste aller Ressourcen für Ihre Supportmitarbeiter.

#### <a name="communications-templates"></a>Kommunikationsvorlagen

Microsoft stellt [anpassbare Vorlagen für E-Mails und andere Kommunikationsformen](https://aka.ms/APTemplates) für den Zugriffsbereich zur Verfügung. Sie können diese Ressourcen für die Nutzung in anderen Kommunikationskanälen entsprechend Ihrer Unternehmenskultur anpassen.

## <a name="plan-your-sso-configuration"></a>Planen der SSO-Konfiguration

Wenn sich ein Benutzer bei einer Anwendung anmeldet, durchläuft er einen Authentifizierungsprozess und muss seine Identität belegen. Ohne SSO wird ein in der Anwendung gespeichertes Kennwort benötigt, und der Benutzer muss dieses Kennwort kennen. Mit SSO werden die Anmeldeinformationen der Benutzer an die Anwendung weitergegeben, sodass sie nicht für jede Anwendung erneut Kennwörter eingeben müssen.

Zum Starten von Anwendungen in „Meine Apps“ muss SSO aktiviert sein.

Azure AD unterstützt drei verschiedene Methoden für das [einmalige Anmelden bei Anwendungen](what-is-single-sign-on.md):

* **Einmalige Verbundanmeldung** 
    * Ermöglicht Anwendungen die Umleitung zu Azure AD für die Benutzerauthentifizierung, statt zur Eingabe eines Kennworts aufgefordert zu werden. 
    * Diese Methode wird für Anwendungen unterstützt, die Protokolle wie SAML 2.0, WS-Verbund oder OpenID Connect verwenden, und sie ist der umfassendste Modus bei der einmaligen Anmeldung.

* **Kennwortbasiertes einmaliges Anmelden** 
    * Ermöglicht die sichere Speicherung des Anwendungskennworts und dessen Wiedergabe mithilfe einer Webbrowsererweiterung oder einer mobilen App. 
    * Sie nutzt den von der Anwendung bereitgestellten vorhandenen Anmeldevorgang, die Kennwortverwaltung kann jedoch der Administrator übernehmen. Der Benutzer muss das Kennwort nicht kennen.

* **Vorhandenes einmaliges Anmelden** 
    * Ermöglicht es Azure AD, alle vorhandenen einmaligen Anmeldungen zu nutzen, die für die Anwendung konfiguriert wurden.
    * Ermöglicht die Verknüpfung dieser Anwendungen mit den Office 365- oder Azure AD-Zugriffsbereichsportalen. 
    * Ermöglicht eine zusätzliche Berichterstellung in Azure AD, wenn die Anwendungen dort gestartet werden. 
    * Umfasst die Verwendung von Azure Application Proxy und des verknüpften Modus für die einmalige Anmeldung.

Erfahren Sie hier, wie Sie den Modus einer Anwendung für einmaliges Anmelden konfigurieren: [Einmaliges Anmelden bei Anwendungen in Azure Active Directory](https://docs.microsoft.com/azure/active-directory/active-directory-appssoaccess-whatis)

Integrieren Sie für eine optimale Erfahrung mit der Seite „Meine Apps“ zunächst Cloudanwendungen, die für Verbund-SSO verfügbar sind. Verbund-SSO ermöglicht Benutzern eine einheitliche 1-Klick-Erfahrung über die verschiedenen App-Startoberflächen hinweg und ist hinsichtlich der Konfigurationssteuerung tendenziell stabiler.

Verwenden Sie Verbund-SSO mit Azure AD (OpenID Connect/SAML) anstelle von kennwortbasiertem SSO und ADFS, wenn eine Anwendung dies unterstützt.

Weitere Informationen zum Bereitstellen und Konfigurieren von SaaS-Anwendungen finden Sie im [SaaS-SSO-Bereitstellungsplan](https://aka.ms/deploymentplans/sso).

#### <a name="plan-to-deploy-the-my-apps-browser-extension"></a>Planen der Bereitstellung der Browsererweiterung „Meine Apps“

Wenn sich Benutzer bei kennwortbasierten SSO-Anwendungen anmelden, müssen sie die sichere Anmeldeerweiterung „Meine Apps“ installieren und verwenden. Die Erweiterung führt ein Skript aus, das das Kennwort in das Anmeldeformular der Anwendung überträgt. Die Benutzer werden beim ersten Start der kennwortbasierten SSO-Anwendung aufgefordert, die Erweiterung zu installieren. Weitere Informationen zur Erweiterung finden Sie in der Dokumentation [Installieren der Browsererweiterung für den Zugriffsbereich](https://docs.microsoft.com/azure/active-directory/application-access-panel-extension-problem-installing?/?WT.mc_id=DMC_AAD_Manage_Apps_Troubleshooting_Nav).

Wenn Sie kennwortbasierte SSO-Anwendungen integrieren müssen, sollten Sie einen Mechanismus definieren, um die Erweiterung skalierbar mit [unterstützten Browsern](https://docs.microsoft.com/azure/active-directory/active-directory-saas-access-panel-introduction) bereitzustellen. Beispiele für Optionen:

* [Gruppenrichtlinie für Internet Explorer](https://azure.microsoft.com/documentation/articles/active-directory-saas-ie-group-policy/)

* [System Center Configuration Manager (SCCM) für Internet Explorer](https://docs.microsoft.com/sccm/core/clients/deploy/deploy-clients-to-windows-computers)

* [Benutzergesteuerter Download und Konfiguration für Chrome, Firefox, Microsoft Edge oder Internet Explorer](https://docs.microsoft.com/azure/active-directory/active-directory-saas-access-panel-introduction)

Weitere Informationen: [Konfigurieren von einmaligem Anmelden mit Kennwort](https://docs.microsoft.com/azure/active-directory/application-config-sso-how-to-configure-password-sso-non-gallery)

Benutzer, die keine kennwortbasierten SSO-Anwendungen verwenden, profitieren ebenfalls von der Erweiterung. Zu diesen Vorteilen zählen die Möglichkeit, jede App über die Suchleiste zu starten, den Zugriff auf kürzlich verwendete Anwendungen zu finden und einen Link zur Seite „Meine Apps“ zu verwenden.

Folgendes wird dem Benutzer angezeigt, wenn er eine kennwortbasierte SSO-Anwendung zum ersten Mal startet:

![Screenshot des Installationsbildschirms der Browsererweiterung „Meine Apps“ ](media/access-panel-deployment-plan/ap-dp-sign-in.png)

#### <a name="plan-for-mobile-access"></a>Planen des mobilen Zugriffs

Für mobile Benutzer, die kennwortbasierte SSO-Anwendungen starten, ist ein mit Intune-Richtlinien geschützter Browser (Microsoft Edge oder Intune Managed Browser) erforderlich. Ein richtliniengeschützter Browser ermöglicht die Übertragung des für die Anwendung gespeicherten Kennworts. Microsoft Edge oder der verwaltete Browser bietet eine Reihe von Features zum Schutz von Webdaten. Sie können Microsoft Edge auch für Unternehmensszenarien auf iOS- und Android-Geräten verwenden. Microsoft Edge unterstützt dieselben Verwaltungsszenarien wie der Intune Managed Browser und erhöht die Benutzerfreundlichkeit. Weitere Informationen: [Verwalten des Webzugriffs mit einem durch Microsoft Intune-Richtlinien geschützten Browser](https://docs.microsoft.com/intune/app-configuration-managed-browser)

## <a name="plan-your-my-apps-deployment"></a>Planen der Bereitstellung von „Meine Apps“

Die Grundlage des Zugriffsbereichs ist das Anwendungsstartprogramm „My Apps“, auf das die Benutzer unter [https://myapps.microsoft.com](https://myapps.microsoft.com/) zugreifen können. Die „Meine Apps“-Seiten bieten den Benutzern einen zentralen Ort, an dem sie mit ihrer Arbeit beginnen und zu ihren erforderlichen Anwendungen gelangen können. Hier finden Benutzer eine Liste aller Anwendungen, auf die sie durch einmaliges Anmelden zugreifen können. 

![Screenshot des Bereichs „Apps“](media/access-panel-deployment-plan/ap-dp-app-launcher.png)

Dieselben Anwendungen werden im Office 365 App-Startfeld angezeigt, wenn Benutzer das Office 365-Portal verwenden.

Planen Sie die Reihenfolge, in der Sie dem „Meine Apps“-Startfeld Anwendungen hinzufügen, und entscheiden Sie, ob Sie deren Rollout schrittweise oder gleichzeitig durchführen möchten. Erstellen Sie dazu einen Anwendungsbestand, der die Art der Authentifizierung und alle vorhandenen SSO-Integrationen für die einzelnen Anwendungen enthält.

#### <a name="add-applications-to-the-my-apps-panel"></a>Hinzufügen von Anwendungen zum Bereich „Meine Apps“

Alle Azure AD SSO-fähigen Anwendungen können dem „Meine Apps“-Startprogramm hinzugefügt werden. Andere Anwendungen werden mithilfe der Option „Verknüpftes einmaliges Anmelden“ hinzugefügt. Sie können eine Anwendungskachel konfigurieren, die auf die URL Ihrer vorhandenen Webanwendung verweist. Mit „Verknüpftes einmaliges Anmelden“ können Sie beginnen, Benutzer zum „Meine Apps“-Portal zu leiten, ohne alle Anwendungen nach Azure AD SSO migrieren zu müssen. Sie können schrittweise zu Azure AD SSO-konfigurierten Anwendungen wechseln, ohne die Benutzerfreundlichkeit zu beeinträchtigen.

#### <a name="use-my-apps-workspaces-preview"></a>Verwenden von „Meine Apps“-Arbeitsbereichen (Vorschau)

Standardmäßig werden alle Anwendungen auf einer einzelnen Seite aufgelistet. Sie können jedoch mithilfe von Arbeitsbereichen verwandte Anwendungen gruppieren und sie auf einer separaten Registerkarte präsentieren, wodurch sie leichter auffindbar sind. So können Sie mit Arbeitsbereichen beispielsweise logische Gruppierungen von Anwendungen für bestimmte Positionen, Aufgaben, Projekte usw. erstellen. Weitere Informationen finden Sie unter [Verwenden von „Meine Apps“-Arbeitsbereichen zum Anpassen von Zugriffsbereichen für Benutzer (Vorschau)](access-panel-workspaces.md). 

#### <a name="plan-whether-to-use-my-apps-or-an-existing-portal"></a>Planen der Verwendung von „Meine Apps“ oder eines bestehenden Portals

Ihre Benutzer verfügen möglicherweise bereits über eine andere Anwendung oder ein anderes Portal als „Meine Apps“. In diesem Fall entscheiden Sie, ob Sie beide Portale unterstützen oder nur eines verwenden möchten.

Wenn ein vorhandenes Portal bereits als Startpunkt für Benutzer verwendet wird, können Sie die Funktionalität von „My Apps“ über „URLs für den Benutzerzugriff“ integrieren. URLs für den Benutzerzugriff fungieren als direkte Links zu den im Portal „Meine Apps“ verfügbaren Anwendungen. Diese URLs können in jede vorhandene Website eingebettet werden. Wenn ein Benutzer auf den Link klickt, wird die Anwendung über das Portal „Meine Apps“ gestartet.

Sie finden die Eigenschaft „URL für den Benutzerzugriff“ im Bereich **Eigenschaften** der Anwendung im Azure-Portal.

![Screenshot des Bereichs „Apps“](media/access-panel-deployment-plan/ap-dp-user-access-url.png)


## <a name="plan-self-service-application-discovery-and-access"></a>Planen der Self-Service-Anwendungsermittlung und des -zugriffs

Sobald ein Kernsatz von Anwendungen auf der Seite „Meine Apps“ eines Benutzers bereitgestellt wird, sollten Sie Features zur Verwaltung von Self-Service-Anwendungen aktivieren. Die Self-Service-App-Ermittlung ermöglicht Benutzern Folgendes:

* Suchen neuer Apps, die „Meine Apps“ hinzugefügt werden sollen. 
* Hinzufügen optionaler Apps, von denen die Benutzer vielleicht nicht wissen, dass sie sie während des Setups benötigen.

Für die ausdrückliche Genehmigung des Zugriffs auf Anwendungen stehen Genehmigungsworkflows zur Verfügung. Benutzer, die genehmigende Personen sind, erhalten Benachrichtigungen im Portal „Meine Apps“, wenn eine Anforderung für den Zugriff auf die Anwendung aussteht.

## <a name="plan-self-service-group-membership"></a>Planen der Self-Service-Gruppenmitgliedschaft 

Sie können Benutzern die Erstellung und Verwaltung ihrer eigenen Sicherheitsgruppen oder Office 365-Gruppen in Azure AD ermöglichen. Der Besitzer der Gruppe kann Mitgliedschaftsanforderungen genehmigen oder ablehnen sowie die Steuerung der Gruppenmitgliedschaft delegieren. Self-Service-Funktionen zur Gruppenverwaltung sind nicht für E-Mail-aktivierte Sicherheitsgruppen oder Verteilerlisten verfügbar.

Zur Planung der Self-Service-Gruppenmitgliedschaft bestimmen Sie, ob Sie allen Benutzern in Ihrem Unternehmen oder nur einer Teilmenge von Benutzern erlauben, Gruppen zu erstellen und zu verwalten. Wenn Sie es nur einer Teilmenge von Benutzern erlauben, müssen Sie eine Gruppe einrichten, zu der diese Personen hinzugefügt werden. Weitere Informationen zum Aktivieren dieser Szenarien finden Sie unter [Einrichten der Self-Service-Gruppenverwaltung in Azure Active Directory](https://docs.microsoft.com/azure/active-directory/users-groups-roles/groups-self-service-management).

## <a name="plan-reporting-and-auditing"></a>Planen der Berichterstellung und Überwachung

Azure AD stellt [Berichte mit technischen und geschäftlichen Einblicken](https://azure.microsoft.com/documentation/articles/active-directory-view-access-usage-reports/) zur Verfügung. Arbeiten Sie mit den Besitzern Ihrer geschäftlichen und technischen Anwendungen zusammen, um den Besitz dieser Berichte zu übernehmen und sie regelmäßig zu nutzen. Die folgende Tabelle enthält einige Beispiele für typische Berichtsszenarien.

|   | Risikomanagement| Steigerung der Produktivität| Governance und Einhaltung |
|  - |- | - | - |
| Berichtstypen|  Anwendungsberechtigungen und -nutzung| Kontobereitstellungsaktivität| Überprüfen, wer auf die Anwendungen zugreift |
| Mögliche Aktionen| Überwachen des Zugriffs, Widerrufen von Berechtigungen| Beheben von Bereitstellungsfehlern| Widerrufen des Zugriffs |

Azure AD speichert die meisten Überwachungsdaten für 30 Tage. Die Daten stehen über das Azure-Verwaltungsportal oder die API zur Verfügung, die Sie in Ihre Analysesysteme herunterladen können.

#### <a name="auditing"></a>Überwachung

Die Überwachungsprotokolle für den Anwendungszugriff sind 30 Tage lang verfügbar. Wenn die Sicherheitsüberwachung in Ihrem Unternehmen eine längere Aufbewahrungszeit erfordert, müssen die Protokolle in ein SIEM-Tool (Security Information Event and Management) wie Splunk oder ArcSight exportiert werden.

Dokumentieren Sie für Überwachungs-, Berichts- und Notfallwiederherstellungssicherungen die erforderliche Downloadhäufigkeit, das Zielsystem und wer für die Verwaltung der einzelnen Sicherungen verantwortlich ist. Möglicherweise benötigen Sie keine separaten Sicherungen für die Überwachung und Berichterstellung. Ihre Sicherung zur Notfallwiederherstellung sollte eine separate Entität sein.

## <a name="deploy-applications-to-users-my-apps-panel"></a>Bereitstellen von Anwendungen im Bereich „Meine Apps“ der Benutzer

Nachdem eine Anwendung für SSO konfiguriert wurde, wird den Gruppen der Zugriff zugewiesen. Benutzer in den zugewiesenen Gruppen haben Zugriff auf die Anwendung, und sie wird in „Meine Apps“ und im Office 365-Startfeld angezeigt.

Weitere Informationen finden Sie unter [Zuweisen von Benutzern und Gruppen zu einer Anwendung in Active Directory](methods-for-assigning-users-and-groups.md).

Wenn Sie während des Tests oder der Bereitstellung die Gruppen hinzufügen möchten, die Anwendungen aber noch nicht in „Meine Apps“ angezeigt werden sollen, finden Sie weitere Informationen unter [Ausblenden einer Anwendung auf der Benutzeroberfläche in Azure Active Directory](https://docs.microsoft.com/azure/active-directory/active-directory-coreapps-hide-third-party-app).

### <a name="deploy-microsoft-office-365-applications-to-my-apps"></a>Bereitstellen von Microsoft Office 365-Anwendungen für Meine Apps

Für Office 365-Anwendungen erhalten Benutzer eine Kopie von Office, die auf den ihnen zugewiesenen Lizenzen basiert. Eine Voraussetzung für den Zugriff auf Office-Anwendungen ist die Zuweisung von Benutzern zu den richtigen Lizenzen, die an die Office-Anwendungen gebunden sind. Wenn Sie einem Benutzer die Lizenz zuweisen, werden ihm automatisch die mit der Lizenz verbundenen Anwendungen auf der Seite „Meine Apps“ und im Office 365-App-Startfeld angezeigt.

Wenn Sie für Benutzer eine Reihe von Office-Anwendungen ausblenden möchten, gibt es eine Option zum Ausblenden von Anwendungen über das „Meine Apps“-Portal, während Sie dennoch den Zugriff über das Office 365-Portal gestatten. Diese Einstellungen finden Sie im Abschnitt „Benutzereinstellungen“ der Anwendung. Weitere Informationen: [Ausblenden einer Anwendung auf der Benutzeroberfläche in Azure Active Directory](https://docs.microsoft.com/azure/active-directory/active-directory-coreapps-hide-third-party-app)

![Screenshot zum Konfigurieren der Ausblendung von Anwendungen](media/access-panel-deployment-plan/ap-dp-o365-portal.png)

### <a name="deploy-application-self-service-capabilities"></a>Bereitstellen von Self-Service-Funktionen für Anwendungen

Der Zugriff auf Self-Service-Anwendungen ermöglicht es Benutzern, den Zugriff auf beliebige Anwendungen selbst zu entdecken und anzufordern. Die Benutzer erhalten die Möglichkeit, auf die erforderlichen Apps zuzugreifen, ohne jedes Mal eine IT-Gruppe zu durchlaufen, um den Zugriff anzufordern. Wenn ein Benutzer den Zugriff anfordert und entweder automatisch oder manuell von einem App-Besitzer genehmigt wird, wird er zu einer Gruppe im Back-End hinzugefügt. Die Berichterstellung ist dafür aktiviert, wer den Zugriff beantragt, genehmigt oder aufgehoben hat, und gibt Ihnen die Kontrolle über die Verwaltung der zugewiesenen Rollen.

Sie können die Genehmigung von Zugriffsanforderungen für Anwendungen an genehmigende Personen des Unternehmens delegieren. Die genehmigende Person des Unternehmens kann die Kennwörter für den App-Zugriff auf der Seite „Meine Apps“ der genehmigenden Person des Unternehmens festlegen.

Weitere Informationen: [Verwenden des Self-Service-Anwendungszugriffs](https://docs.microsoft.com/azure/active-directory/application-access-panel-self-service-applications-how-to)

![Screenshot zum Konfigurieren der Verwaltung von Self-Service-Anwendungen](media/access-panel-deployment-plan/ap-dp-salesforce.png)

## <a name="validate-your-deployment"></a>Überprüfen der Bereitstellung

Stellen Sie sicher, dass die Bereitstellung des Zugriffsbereichs sorgfältig getestet wurde und ein Rollbackplan vorhanden ist.

Die folgenden Tests sollten sowohl mit firmeneigenen als auch mit privaten Geräten durchgeführt werden. Diese Testfälle sollten auch Ihre geschäftlichen Anwendungsfälle widerspiegeln. Im Folgenden werden einige Fälle aufgeführt, die auf den Geschäftsanforderungsbeispielen in diesem Dokument und auf typischen technischen Szenarien basieren. Fügen Sie weitere, auf Ihre Bedürfnisse zugeschnittene Testfälle hinzu.

#### <a name="application-sso-access-test-case-examples"></a>Beispiele für Testfälle mit SSO-Zugriff auf die Anwendung:


| Geschäftsszenario| Erwartetes Ergebnis |
| - | -|
| Benutzer meldet sich beim „Meine Apps“-Portal an| Benutzer kann sich anmelden und seine Anwendungen anzeigen |
| Benutzer startet eine Verbund-SSO-Anwendung| Benutzer wird automatisch bei der Anwendung angemeldet |
| Benutzer öffnet zum ersten Mal eine Kennwort-SSO-Anwendung| Benutzer muss die Erweiterung „Meine Apps“ installieren |
| Benutzer startet eine Kennwort-SSO-Anwendung zu einem späteren Zeitpunkt erneut| Benutzer wird automatisch bei der Anwendung angemeldet |
| Benutzer startet eine App über das Office 365-Portal| Benutzer wird automatisch bei der Anwendung angemeldet |
| Benutzer startet eine App über den verwalteten Browser| Benutzer wird automatisch bei der Anwendung angemeldet |


#### <a name="application-self-service-capabilities-test-case-examples"></a>Beispiele für Testfälle mit Self-Service-Funktionen für Anwendungen


| Geschäftsszenario| Erwartetes Ergebnis |
| - | - |
| Benutzer kann die Mitgliedschaft für die Anwendung verwalten| Benutzer kann Mitglieder hinzufügen/entfernen, die Zugriff auf die App haben |
| Benutzer kann die Anwendung bearbeiten| Benutzer kann die Beschreibung und Anmeldeinformationen für Kennwort-SSO-Anwendungen bearbeiten |

### <a name="rollback-steps"></a>Rollbackschritte

Es ist wichtig, zu planen, was zu tun ist, wenn Ihre Bereitstellung nicht wie gewünscht verläuft. Wenn bei der SSO-Konfiguration während der Bereitstellung ein Fehler auftritt, müssen Sie verstehen, wie Sie [SSO-Probleme beheben](https://docs.microsoft.com/azure/active-directory/hybrid/tshoot-connect-sso) und die Auswirkungen auf Ihre Benutzer verringern können. In extremen Fällen müssen Sie möglicherweise für [SSO ein Rollback durchführen](../manage-apps/plan-sso-deployment.md).


## <a name="manage-your-implementation"></a>Verwalten Ihrer Implementierung

Sie sollten die am wenigsten privilegierte Rolle verwenden, um eine erforderliche Aufgabe innerhalb von Azure Active Directory auszuführen. [Überprüfen Sie die verschiedenen verfügbaren Rollen](https://docs.microsoft.com/azure/active-directory/active-directory-assign-admin-roles-azure-portal), und wählen Sie die richtige aus, um Ihre Anforderungen für jede Persona für diese Anwendung zu erfüllen. Einige Rollen müssen unter Umständen nur vorübergehend angewendet werden und können nach Abschluss der Bereitstellung wieder entfernt werden.

| Personas| Rollen| Azure AD-Rolle  |
| - | -| -|
| Helpdesk-Administrator| Support der Ebene 1| Keine |
| Identitätsadministrator| Konfigurieren und Debuggen, wenn sich Probleme auf Azure AD auswirken| Globaler Administrator |
| Anwendungsadministrator| Benutzernachweis in Anwendung, Konfiguration für Benutzer mit Berechtigungen| Keine |
| Infrastrukturadministratoren| Besitzer des Zertifikatrollovers| Globaler Administrator |
| Geschäftsbesitzer/Beteiligter| Benutzernachweis in Anwendung, Konfiguration für Benutzer mit Berechtigungen| Keine |

Sie können Ihre Rollen mithilfe von [Privileged Identity Management](https://docs.microsoft.com/azure/active-directory/active-directory-privileged-identity-management-configure) (PIM) verwalten, um zusätzliche Überwachung, Kontrolle und Zugriffsprüfungen für Benutzer mit Verzeichnisberechtigungen bereitzustellen.

### <a name="troubleshoot-access-panel-issues"></a>Beheben von Problemen mit dem Zugriffsbereich

Erstellen Sie Leitfäden zur Problembehandlung für Ihre Supportorganisation mit häufigen Szenarien, die in ihrer Auflösung auf die Microsoft-Dokumentation verweisen. Möglicherweise sollten Sie Leitfäden erstellen, die die Unterstützung auf die von Ihrem Unternehmen verwendeten Ebenen aufteilen.

Weitere Informationen finden Sie in den folgenden Leitfäden zur Problembehandlung:

[Anwendungen, die nicht angezeigt werden](https://docs.microsoft.com/azure/active-directory/manage-apps/access-panel-troubleshoot-application-not-appearing)

[Unerwartete Anwendungen werden angezeigt](https://docs.microsoft.com/azure/active-directory/manage-apps/access-panel-troubleshoot-unexpected-application)

[Der Benutzer kann sich nicht im Zugriffsbereich anmelden](https://docs.microsoft.com/azure/active-directory/manage-apps/access-panel-troubleshoot-web-sign-in-problem)

[Probleme beim Self-Service-Anwendungszugriff](https://docs.microsoft.com/azure/active-directory/manage-apps/access-panel-troubleshoot-self-service-access)

[Probleme mit der Browsererweiterung](https://docs.microsoft.com/azure/active-directory/manage-apps/manage-access-panel-browser-extension)

## <a name="next-steps"></a>Nächste Schritte

[Planen einer Bereitstellung von Azure Multi-Factor Authentication](https://aka.ms/deploymentplans/mfa)
