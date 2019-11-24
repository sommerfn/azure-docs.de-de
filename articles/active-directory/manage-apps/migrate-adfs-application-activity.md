---
title: Verwenden des Aktivitätsberichts zum Verschieben von AD FS-Apps in Azure Active Directory | Microsoft-Dokumentation
description: Mit dem AD FS-Anwendungsaktivitätsbericht (Active Directory-Verbunddienste) können Sie Anwendungen schnell von AD FS zu Azure Active Directory (Azure AD) migrieren. Dieses Migrationstool für AD FS identifiziert die Kompatibilität mit Azure AD und bietet Anleitungen zur Migration.
services: active-directory
author: msmimart
manager: CelesteDG
ms.service: active-directory
ms.subservice: app-mgmt
ms.topic: conceptual
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.date: 10/30/2019
ms.author: mimart
ms.collection: M365-identity-device-management
ms.openlocfilehash: 10a267811b7e7ac8715b6823a24c2b3a1f0d430c
ms.sourcegitcommit: 98ce5583e376943aaa9773bf8efe0b324a55e58c
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 10/30/2019
ms.locfileid: "73180110"
---
# <a name="use-the-ad-fs-application-activity-report-preview-to-migrate-applications-to-azure-ad"></a>Verwenden Sie den AD FS-Anwendungsaktivitätsbericht (Vorschau), um Anwendungen zu Azure AD zu migrieren.

Viele Organisationen verwenden Active Directory-Verbunddienste (AD FS), um Cloudanwendungen einmaliges Anmelden bereitzustellen. Die Migration Ihrer AD FS Anwendungen zu Azure AD bietet bedeutende Vorteile für die Authentifizierung, insbesondere im Hinblick auf Kostenverwaltung, Risikomanagement, Produktivität, Compliance und Governance. Es kann jedoch zeitaufwändig sein, zu ermitteln, ob Anwendungen mit Azure AD kompatibel sind, und spezifische Migrationsschritte zu identifizieren.

Mit dem AD FS-Anwendungsaktivitätsbericht (Vorschau) im Azure-Portal können Sie schnell ermitteln, welche Ihrer Anwendungen zu Azure AD migriert werden können. Es werden alle AD FS-Anwendungen im Hinblick auf die Kompatibilität mit Azure AD bewertet und auf Probleme überprüft sowie Anleitungen zum Vorbereiten einzelner Anwendungen für die Migration bereitgestellt. Mit dem AD FS-Anwendungsaktivitätsbericht können Sie folgende Aktionen ausführen:

* **Ermitteln der AD FS-Anwendungen und Planen des Migrationsumfangs.** Der AD FS-Anwendungsaktivitätsbericht listet alle AD FS-Anwendungen in Ihrer Organisation auf und gibt deren Bereitschaft für die Migration zu Azure AD an.
* **Priorisieren von Anwendungen für die Migration.** Ermitteln Sie die Anzahl der verschiedenen Benutzer, die sich in den letzten 1, 7 oder 30 Tagen bei der Anwendung angemeldet haben, um die Wichtigkeit oder das Risiko der Migration der Anwendung zu ermitteln.
* **Ausführen von Migrationstests und Beheben von Problemen.** Der Berichterstellungsdienst führt automatisch Tests aus, um zu bestimmen, ob eine Anwendung für die Migration bereit ist. Die Ergebnisse werden im AD FS-Anwendungsaktivitätsbericht als Migrationsstatus angezeigt. Werden potenzielle Migrationsprobleme erkannt, erhalten Sie spezifische Anweisungen zu deren Behebung.

Die AD FS-Anwendungsaktivitätsdaten sind für Benutzer verfügbar, denen diese Administratorrollen zugewiesen sind: globaler Administrator, Benutzer mit Leseberechtigung für Berichte, Benutzer mit Leseberechtigung für Sicherheitsfunktionen, Anwendungsadministrator oder Cloudanwendungsadministrator.

## <a name="prerequisites"></a>Voraussetzungen

* Ihre Organisation muss AD FS aktuell für den Zugriff auf Anwendungen verwenden.
* Azure AD Connect Health muss in Ihrem Azure AD-Mandanten aktiviert sein.
   * [Weitere Informationen zu Azure AD Connect Health](https://docs.microsoft.com/azure/active-directory/hybrid/how-to-connect-health-adfs)
   * [Erste Schritte mit der Einrichtung von Azure AD Connect Health](https://docs.microsoft.com/azure/active-directory/hybrid/how-to-connect-health-agent-install)

## <a name="discover-ad-fs-applications-that-can-be-migrated"></a>Ermitteln von AD FS-Anwendungen, die migriert werden können 

Der AD FS-Anwendungsaktivitätsbericht ist im Azure-Portal unter Azure AD-Berichte zu **Nutzung und Erkenntnissen** verfügbar. Der AD FS-Anwendungsaktivitätsbericht analysiert jede AD FS-Anwendung, um zu bestimmen, ob sie unverändert migriert werden kann, oder ob eine zusätzliche Überprüfung erforderlich ist. 

1. Melden Sie sich beim [Azure-Portal](https://portal.azure.com) mit einer Administratorrolle an, die auf AD FS-Anwendungsaktivitätsdaten zugreifen kann (globaler Administrator, Benutzer mit Leseberechtigung für Berichte, Benutzer mit Leseberechtigung für Sicherheitsfunktionen, Anwendungsadministrator oder Cloudanwendungsadministrator).

2. Wählen Sie **Azure Active Directory** und dann **Unternehmensanwendungen** aus.

3. Wählen Sie unter **Aktivität** die Option **Nutzung und Erkenntnisse (Vorschau)** und dann **AD FS-Anwendungsaktivität** aus, um eine Liste aller AD FS-Anwendungen in Ihrer Organisation zu öffnen.

   ![AD FS-Anwendungsaktivität](media/migrate-adfs-application-activity/adfs-application-activity.png)

4. Zeigen Sie für jede Anwendung in der AD FS-Anwendungsaktivitätsliste den **Migrationsstatus** an:

   * **Bereit zum Migrieren** bedeutet, dass die AD FS-Anwendungskonfiguration in Azure AD vollständig unterstützt wird und unverändert migriert werden kann.

   * **Überprüfung erforderlich** bedeutet, dass einige Einstellungen der Anwendung zu Azure AD migriert werden können, Sie jedoch die Einstellungen, die nicht unverändert migriert werden können, überprüfen müssen.

   * **Zusätzliche Schritte erforderlich** bedeutet, dass Azure AD einige Einstellungen der Anwendung nicht unterstützt, sodass die Anwendung nicht im aktuellen Zustand migriert werden kann.

## <a name="evaluate-the-readiness-of-an-application-for-migration"></a>Auswerten der Bereitschaft einer Anwendung für die Migration 

1. Klicken Sie in AD FS-Anwendungsaktivitätsliste auf den Status in der Spalte **Migrationsstatus**, um Details zur Migration zu öffnen. Eine Zusammenfassung der bestandenen Konfigurationstests wird zusammen mit möglichen Migrationsproblemen angezeigt.

   ![Details zur Migration](media/migrate-adfs-application-activity/migration-details.png)

2. Klicken Sie auf eine Meldung, um zusätzliche Details zur Migrationsregel zu öffnen. Eine vollständige Liste der getesteten Eigenschaften finden Sie unten in der Tabelle [AD FS-Anwendungskonfigurationstests](#ad-fs-application-configuration-tests).

   ![Details zur Migrationsregel](media/migrate-adfs-application-activity/migration-rule-details.png)

### <a name="ad-fs-application-configuration-tests"></a>AD FS-Anwendungskonfigurationstests

In der folgenden Tabelle sind alle Konfigurationstests aufgeführt, die für AD FS-Anwendungen ausgeführt werden.

|Eigenschaft  |Status  |BESCHREIBUNG  |
|---------|---------|---------|
|Für „AdditionalAuthentication“ wurde mindestens eine nicht migrierbare Regel erkannt.       | Bestanden/Warnung          | Die vertrauende Seite in AD FS verwendet einen lokalen MFA-Anbieter. Für den Wechsel zu Azure AD wird empfohlen, auf Azure MFA oder die Integration benutzerdefinierter Steuerelemente mit einem MFA-Drittanbieter umzustellen. [Erfahren Sie mehr über Azure MFA](https://docs.microsoft.com/azure/active-directory/authentication/concept-mfa-howitworks).        |
|Für die vertrauende Seite ist „AdditionalWSFedEndpoint“ auf „true“ festgelegt.       | Erfolgreich/Fehler          | Die vertrauende Seite in AD FS unterstützt mehrere WS-Fed-Assertionsendpunkte. Azure AD unterstützt zurzeit nur einen (1). Wenn dies die Migration in Ihrem Szenario blockiert, [geben Sie uns Feedback](https://feedback.azure.com/forums/169401-azure-active-directory/suggestions/38695621-allow-multiple-ws-fed-assertion-endpoints).     |
|Für die vertrauende Seite ist „AllowedAuthenticationClassReferences“ festgelegt.       | Erfolgreich/Fehler          | Mit dieser Einstellung in AD FS können Sie angeben, ob die Anwendung so konfiguriert ist, dass nur bestimmte Authentifizierungstypen zugelassen werden. Diese Konfiguration wird von Azure AD aktuell nicht unterstützt. Wenn dies die Migration in Ihrem Szenario blockiert, [geben Sie uns Feedback](https://feedback.azure.com/forums/169401-azure-active-directory/suggestions/38695672-allow-in-azure-ad-to-specify-certain-authentication).          |
|„AlwaysRequireAuthentication“ ist eingerichtet.      | Erfolgreich/Fehler          | Gibt an, ob die Anwendung so konfiguriert ist, dass SSO-Cookies ignoriert werden und immer eine Authentifizierung angefordert wird. Diese Konfiguration wird von Azure AD aktuell nicht unterstützt.          |
|Für die vertrauende Seite ist „AutoUpdateEnabled“ auf „true“ festgelegt.       | Bestanden/Warnung          | Mit dieser Einstellung in AD FS können Sie angeben, ob AD FS für die automatische Aktualisierung der Anwendung basierend auf Änderungen innerhalb der Verbundmetadaten konfiguriert ist. Azure AD unterstützt dies derzeit nicht, sollte die Migration der Anwendung zu Azure AD jedoch nicht blockieren.           |
|Die vertrauende Seite verfügt über mehrere aktivierte ClaimsProviders-Elemente.       | Erfolgreich/Fehler          | Die vertrauende Seite ist so konfiguriert, dass Ansprüche von einem anderen Anspruchsanbieter als Active Directory abgeleitet werden.  Dies wird in Azure AD nicht unterstützt. Wenn dies die Migration in Ihrem Szenario blockiert, [geben Sie uns Feedback](https://feedback.azure.com/forums/169401-azure-active-directory/suggestions/38695717-allow-to-source-user-attributes-from-external-dire).          |
|„DelegationAuthorization“ ist gültig.      | Erfolgreich/Fehler          | Für die Anwendung sind benutzerdefinierte Delegationsautorisierungsregeln definiert. Diese Konfiguration wird von Azure AD aktuell nicht unterstützt. Wenn dies die Migration in Ihrem Szenario blockiert,  [geben Sie uns Feedback](https://feedback.azure.com/forums/169401-azure-active-directory/suggestions/38695747-allow-to-define-delegation-authorization-rules).          |
|Die vertrauende Seite verfügt über „ADFSRPImpersonationAuthorizationRules“.       | Bestanden/Warnung          | Für die Anwendung sind benutzerdefinierte Identitätswechsel-Autorisierungsregeln definiert. Azure AD unterstützt dies derzeit nicht, sollte die Migration der Anwendung zu Azure AD jedoch nicht blockieren.          |
|Für „IssuanceAuthorization“ wurde mindestens eine nicht migrierbare Regel erkannt.       | Bestanden/Warnung          | Für die Anwendung sind benutzerdefinierte 	Ausstellungsautorisierungsregeln in AD FS definiert. Azure AD unterstützt diese Funktion mit bedingtem Azure AD-Zugriff. [Erfahren Sie mehr über bedingten Zugriff](https://docs.microsoft.com/azure/active-directory/conditional-access/overview). In Azure AD können Sie zudem den Zugriff auf die Anwendung auf Benutzer oder Gruppen beschränken, die der Anwendung zugeordnet sind. [Erfahren Sie mehr über das Zuweisen von Benutzern und Gruppen für den Zugriff auf Anwendungen](https://docs.microsoft.com/azure/active-directory/manage-apps/methods-for-assigning-users-and-groups). Informationen zu den in AD FS konfigurierten unformatierten IssuanceAuthorization-Regeln finden Sie in der Tabelle [Anspruchsregeltests](#check-the-results-of-claim-rule-tests) weiter unten.           |
|Für „IssuanceTransform“ wurde mindestens eine nicht migrierbare Regel erkannt.       | Bestanden/Warnung          | Für die Anwendung sind benutzerdefinierte 	Ausstellungstransformationsregeln in AD FS definiert. Azure AD unterstützt das Anpassen der im Token ausgestellten Ansprüche. Weitere Informationen finden Sie unter [Anpassen von Ansprüchen im SAML-Token für Unternehmensanwendungen](https://docs.microsoft.com/azure/active-directory/develop/active-directory-saml-claims-customization).  Informationen zu den in AD FS konfigurierten unformatierten IssuanceTransformation-Regeln finden Sie in der Tabelle [Anspruchsregeltests](#check-the-results-of-claim-rule-tests) weiter unten.          |
|Für die vertrauende Seite ist „MonitoringEnabled“ auf „true“ festgelegt.       | Bestanden/Warnung          | Mit dieser Einstellung in AD FS können Sie angeben, ob AD FS zur Überwachung von Verbundmetadaten für die Anwendung konfiguriert ist. Dies wird in Azure AD derzeit nicht unterstützt, sollte die Migration der Anwendung zu Azure AD jedoch nicht blockieren.          |
|Auf der vertrauenden Seite wurde „NotBeforeSkew“ eingerichtet.      | Bestanden/Warnung          | AD FS lässt eine Zeitabweichung auf Grundlage der „NotBefore“- und „NotOnOrAfter“-Zeiten im SAML-Token zu. Azure AD unterstützt dies derzeit nicht, sollte die Migration der Anwendung zu Azure AD jedoch nicht blockieren.          |
|Für die vertrauende Seite ist „RequestMFAFromClaimsProviders“ auf „true“ festgelegt.       | Bestanden/Warnung          | Dies ist eine Einstellung in AD FS, mit der Sie angeben können, ob die Anwendung für einen anderen Anspruchsanbieter hartcodiert ist und MFA erfordert. Für den Wechsel zu Azure AD wird empfohlen, auf Azure MFA oder die Integration benutzerdefinierter Steuerelemente mit einem MFA-Drittanbieter umzustellen. [Weitere Informationen zu Azure MFA](https://docs.microsoft.com/azure/active-directory/authentication/concept-mfa-howitworks).          |
|Für die vertrauende Seite ist „SignedSamlRequestsRequired“ auf „true“ festgelegt.       | Erfolgreich/Fehler          | Die Anwendung wird in AD FS zur Überprüfung der Signatur in der SAML-Anforderung konfiguriert. Dies ist eine optionale Einstellung, die die Migration nicht blockieren sollte. Azure Active Directory verwendet die Antwort-URLs zum Überprüfen des Dienstanbieters.  Wenn dies die Migration in Ihrem Szenario blockiert, [geben Sie uns Feedback](https://feedback.azure.com/forums/169401-azure-active-directory/suggestions/13394589-saml-signature).          |
|TokenLifetimeCheckResult        | Erfolgreich/Fehler         | Die Anwendung ist für eine benutzerdefinierte Tokenlebensdauer konfiguriert. Der AD FS-Standardwert ist eine Stunde. Azure AD unterstützt diese Funktion mit bedingtem-Zugriff. Weitere Informationen finden Sie unter [Konfigurieren der Verwaltung von Authentifizierungssitzungen mit bedingtem Zugriff](https://docs.microsoft.com/azure/active-directory/conditional-access/howto-conditional-access-session-lifetime).          |
|Die vertrauende Seite ist so konfiguriert, dass Ansprüche verschlüsselt werden. Diese Konfiguration wird von Azure AD unterstützt.       | Pass          | Mit Azure AD können Sie das an die Anwendung gesendete Token verschlüsseln. Weitere Informationen finden Sie unter [Konfigurieren der Azure AD-SAML-Tokenverschlüsselung](https://docs.microsoft.com/azure/active-directory/manage-apps/howto-saml-token-encryption).          |
|Die vertrauende Seite ist so konfiguriert, dass „NameID“ im SAML-Token verschlüsselt wird.      | Erfolgreich/Fehler          | Die Anwendung ist so konfiguriert, dass der NameID-Anspruch im SAML-Token verschlüsselt wird. Mit Azure AD können Sie das gesamte an die Anwendung gesendete Token verschlüsseln. Die Verschlüsselung bestimmter Ansprüche wird noch nicht unterstützt. Weitere Informationen finden Sie unter [Konfigurieren der Azure AD-SAML-Tokenverschlüsselung](https://docs.microsoft.com/azure/active-directory/manage-apps/howto-saml-token-encryption).         |

## <a name="check-the-results-of-claim-rule-tests"></a>Prüfen der Ergebnisse von Anspruchsregeltests

Wenn Sie in AD FS eine Anspruchsregel für die Anwendung konfiguriert haben, bietet die Oberfläche eine detaillierte Analyse für alle Anspruchsregeln. Sie können erkennen, welche Anspruchsregeln nach Azure AD verschoben werden können und welche noch weiter geprüft werden müssen.

1. Klicken Sie in AD FS-Anwendungsaktivitätsliste auf den Status in der Spalte **Migrationsstatus**, um Details zur Migration zu öffnen. Eine Zusammenfassung der bestandenen Konfigurationstests wird zusammen mit möglichen Migrationsproblemen angezeigt.

2. Erweitern Sie auf der Seite **Details zur Migrationsregel** die Ergebnisse, um Details zu möglichen Migrationsproblemen anzuzeigen und weitere Anleitungen zu erhalten. Eine ausführliche Liste aller getesteten Anspruchsregeln finden Sie in der Tabelle [Prüfen der Ergebnisse von Anspruchsregeltests](#check-the-results-of-claim-rule-tests) weiter unten.

   Das folgende Beispiel zeigt Details zur Migrationsregel für die IssuanceTransform-Regel. Es werden die spezifischen Teile des Anspruchs aufgelistet, die überprüft und behandelt werden müssen, bevor Sie die Anwendung zu Azure AD migrieren können.

   ![Zusätzliche Anleitungen zu den Details zur Migrationsregel](media/migrate-adfs-application-activity/migration-rule-details-guidance.png)

### <a name="claim-rule-tests"></a>Anspruchsregeltests

In der folgenden Tabelle sind alle Anspruchsregeltests aufgeführt, die für AD FS-Anwendungen ausgeführt werden.

|Eigenschaft  |BESCHREIBUNG  |
|---------|---------|
|UNSUPPORTED_CONDITION_PARAMETER      | Die Bedingungsanweisung verwendet reguläre Ausdrücke, um auszuwerten, ob der Anspruch einem bestimmten Muster entspricht.  Um eine ähnliche Funktionalität in Azure AD zu erzielen, können Sie vordefinierte Transformationen wie z.B. IfEmpty(), StartWith() und Contains() verwenden. Weitere Informationen finden Sie unter [Anpassen von Ansprüchen im SAML-Token für Unternehmensanwendungen](https://docs.microsoft.com/azure/active-directory/develop/active-directory-saml-claims-customization).          |
|UNSUPPORTED_CONDITION_CLASS      | Die Bedingungsanweisung verfügt über mehrere Bedingungen, die vor dem Ausführen der Ausstellungsanweisung ausgewertet werden müssen. Azure AD unterstützt diese Funktionalität möglicherweise mit den Transformationsfunktionen des Anspruchs, in denen Sie mehrere Anspruchswerte auswerten können.  Weitere Informationen finden Sie unter [Anpassen von Ansprüchen im SAML-Token für Unternehmensanwendungen](https://docs.microsoft.com/azure/active-directory/develop/active-directory-saml-claims-customization).          |
|UNSUPPORTED_RULE_TYPE      | Die Anspruchsregel wurde nicht erkannt. Weitere Informationen zum Konfigurieren von Ansprüchen in Azure AD finden Sie unter [Anpassen von Ansprüchen im SAML-Token für Unternehmensanwendungen](https://docs.microsoft.com/azure/active-directory/develop/active-directory-saml-claims-customization).          |
|CONDITION_MATCHES_UNSUPPORTED_ISSUER      | Die Bedingungsanweisung verwendet einen Aussteller, der in Azure AD nicht unterstützt wird. Derzeit bindet Azure AD keine Ansprüche aus anderen Speichern als Active Directory oder Azure AD ein. Wenn dies die Migration von Anwendungen zu Azure AD blockiert, [geben Sie uns Feedback](https://feedback.azure.com/forums/169401-azure-active-directory/suggestions/38695717-allow-to-source-user-attributes-from-external-dire).         |
|UNSUPPORTED_CONDITION_FUNCTION      | Die Bedingungsanweisung verwendet eine Aggregatfunktion, um einen einzelnen Anspruch unabhängig von der Anzahl der Übereinstimmungen auszugeben oder hinzuzufügen.  In Azure AD können Sie das Attribut eines Benutzers z.B. über Funktionen wie IfEmpty(), StartWith() und Contains() auswerten, um zu entscheiden, welcher Wert für den Anspruch verwendet werden soll. Weitere Informationen finden Sie unter [Anpassen von Ansprüchen im SAML-Token für Unternehmensanwendungen](https://docs.microsoft.com/azure/active-directory/develop/active-directory-saml-claims-customization).          |
|RESTRICTED_CLAIM_ISSUED      | Die Bedingungsanweisung verwendet einen Anspruch, der in Azure AD eingeschränkt ist. Möglicherweise können Sie einen eingeschränkten Anspruch ausgeben, aber Sie können die Quelle nicht ändern und keine Transformation anwenden. Weitere Informationen finden Sie unter [Anpassen von in Token ausgegebenen Ansprüchen für eine bestimmte App in Azure AD](https://docs.microsoft.com/azure/active-directory/develop/active-directory-claims-mapping).          |
|EXTERNAL_ATTRIBUTE_STORE      | Die Ausstellungsanweisung verwendet einen anderen Attributspeicher als Active Directory. Derzeit bindet Azure AD keine Ansprüche aus anderen Speichern als Active Directory oder Azure AD ein. Wenn dies die Migration von Anwendungen zu Azure AD blockiert, [geben Sie uns Feedback](https://feedback.azure.com/forums/169401-azure-active-directory/suggestions/38695717-allow-to-source-user-attributes-from-external-dire).          |
|UNSUPPORTED_ISSUANCE_CLASS      | Die Ausstellungsanweisung verwendet ADD, um dem eingehenden Anspruchssatz Ansprüche hinzuzufügen. In Azure AD kann dies möglicherweise als mehrfache Anspruchstransformationen konfiguriert werden.  Weitere Informationen finden Sie unter [Anpassen von Ansprüchen im SAML-Token für Unternehmensanwendungen](https://docs.microsoft.com/azure/active-directory/develop/active-directory-claims-mapping).         |
|UNSUPPORTED_ISSUANCE_TRANSFORMATION      | Die Ausstellungsanweisung verwendet reguläre Ausdrücke, um den Wert des Anspruchs zu transformieren, der ausgegeben werden soll. Um eine ähnliche Funktionalität in Azure AD zu erzielen, können Sie vordefinierte Transformationen wie z.B. Extract(), Trim() und ToLower verwenden. Weitere Informationen finden Sie unter [Anpassen von Ansprüchen im SAML-Token für Unternehmensanwendungen](https://docs.microsoft.com/azure/active-directory/develop/active-directory-saml-claims-customization).          |


## <a name="next-steps"></a>Nächste Schritte

- [Verwalten von Anwendungen mit Azure Active Directory](what-is-application-management.md)
- [Verwalten des Zugriffs auf Apps](what-is-access-management.md)
- [Azure AD Connect und Verbund](../hybrid/how-to-connect-fed-whatis.md)
