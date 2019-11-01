---
title: Blaupausenbeispiel „CIS Microsoft Azure Foundations Benchmark“ – Empfehlungszuordnung
description: Empfehlungszuordnung des Blaupausenbeispiels „CIS Microsoft Azure Foundations Benchmark“ zu Azure Policy.
author: DCtheGeek
ms.author: dacoulte
ms.date: 10/01/2019
ms.topic: sample
ms.service: blueprints
ms.openlocfilehash: 64af398ec4829a0336294b33fef102392b9a4099
ms.sourcegitcommit: 0b1a4101d575e28af0f0d161852b57d82c9b2a7e
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 10/30/2019
ms.locfileid: "73163879"
---
# <a name="recommendation-mapping-of-the-cis-microsoft-azure-foundations-benchmark-blueprint-sample"></a>Empfehlungszuordnung des Blaupausenbeispiels „CIS Microsoft Azure Foundations Benchmark“

Im folgenden Artikel erfahren Sie, wie das Azure Blueprints-Blaupausenbeispiel „CIS Microsoft Azure Foundations Benchmark“ den Empfehlungen für „CIS Microsoft Azure Foundations Benchmark“ zugeordnet wird. Weitere Informationen zu den Empfehlungen finden Sie unter [CIS Microsoft Azure Foundations Benchmark](https://www.cisecurity.org/benchmark/azure/).

Die folgenden Zuordnungen gelten für die Empfehlungen für **CIS Microsoft Azure Foundations Benchmark v1.1.0**. Über den rechten Navigationsbereich können Sie direkt zu einer bestimmten Empfehlungszuordnung navigieren.
Viele der zugeordneten Empfehlungen werden mit einer [Azure Policy](../../../policy/overview.md)-Initiative implementiert. Zum Anzeigen der vollständigen Initiative öffnen Sie **Richtlinie** im Azure-Portal und wählen dann die Seite **Definitionen** aus. Suchen Sie anschließend die integrierte Richtlinieninitiative **\[Preview\] Audit CIS Microsoft Azure Foundations Benchmark v1.1.0 recommendations and deploy specific VM Extensions to support audit requirements** ([Vorschauversion] Empfehlungen für „CIS Microsoft Azure Foundations Benchmark v1.1.0“ überwachen und spezifische VM-Erweiterungen zur Unterstützung von Überwachungsanforderungen bereitstellen), und wählen Sie sie aus.

> [!IMPORTANT]
> Jede Steuerung unten ist mindestens einer [Azure Policy](../../../policy/overview.md)-Definition zugeordnet. Diese Richtlinien können Ihnen bei der [Konformitätsbewertung](../../../policy/how-to/get-compliance-data.md) mit der Steuerung helfen. Es gibt jedoch oft keine 1:1- oder vollständige Übereinstimmung zwischen einer Steuerung und einer bzw. mehreren Richtlinien. Daher bezieht sich **Konform** in Azure Policy nur auf die Richtlinien selbst und gewährleistet nicht die vollständige Konformität mit allen Anforderungen einer Steuerung. Außerdem enthält der Kompatibilitätsstandard Steuerungen, die derzeit von keiner Azure Policy-Definition abgedeckt werden. Daher ist die Konformität in Azure Policy nur eine partielle Ansicht Ihres gesamten Konformitätsstatus. Die Zuordnungen zwischen Steuerungen und Azure Policy-Definitionen für dieses Konformitätsblaupausenbeispiel können sich im Laufe der Zeit ändern. Den Änderungsverlauf finden Sie im [GitHub-Commit-Verlauf](https://github.com/MicrosoftDocs/azure-docs/commits/master/articles/governance/blueprints/samples/cis-azure-1.1.0/control-mapping.md).

## <a name="11-ensure-that-multi-factor-authentication-is-enabled-for-all-privileged-users"></a>1.1. Sicherstellen, dass die mehrstufige Authentifizierung für alle privilegierten Benutzer aktiviert ist

Mit dieser Blaupause werden [Azure Policy](../../../policy/overview.md)-Definitionen zugewiesen, mit denen Sie die Überwachung durchführen können, wenn die mehrstufige Authentifizierung für privilegierte Azure Active Directory-Konten nicht aktiviert ist.

- MFA sollte für Konten mit Besitzerberechtigungen in Ihrem Abonnement aktiviert sein.
- MFA sollte für Konten mit Schreibberechtigungen für Ihr Abonnement aktiviert werden.

## <a name="12-ensure-that-multi-factor-authentication-is-enabled-for-all-non-privileged-users"></a>1.2. Sicherstellen, dass die mehrstufige Authentifizierung für alle nicht privilegierten Benutzer aktiviert ist

Mit dieser Blaupause wird eine [Azure Policy](../../../policy/overview.md)-Definition zugewiesen, mit denen Sie die Überwachung durchführen können, wenn die mehrstufige Authentifizierung für nicht privilegierte Azure Active Directory-Konten nicht aktiviert ist.

- MFA sollte für Ihre Abonnementkonten mit Leseberechtigungen aktiviert sein

## <a name="13-ensure-that-there-are-no-guest-users"></a>1.3. Sicherstellen, dass keine Gastbenutzer vorhanden sind

Mit dieser Blaupause werden [Azure Policy](../../../policy/overview.md)-Definitionen zugewiesen, mit denen Sie die Überwachung für Gastkonten durchführen können, die ggf. entfernt werden müssen.

- Externe Konten mit Besitzerberechtigungen sollten aus Ihrem Abonnement entfernt werden.
- Externe Konten mit Leseberechtigungen sollten aus Ihrem Abonnement entfernt werden
- Externe Konten mit Schreibberechtigungen sollten aus Ihrem Abonnement entfernt werden.

## <a name="21-ensure-that-standard-pricing-tier-is-selected"></a>2.1. Sicherstellen, dass der Tarif „Standard“ ausgewählt ist

Mit dieser Blaupause wird eine [Azure Policy](../../../policy/overview.md)-Definition zugewiesen, mit der Sie Netzwerke und virtuelle Computer überwachen können, für die der Standard-Tarif von Security Center nicht aktiviert ist.

 - Standard-Tarif von Security Center muss ausgewählt sein

## <a name="22-ensure-that-automatic-provisioning-of-monitoring-agent-is-set-to-on"></a>2.2. Sicherstellen, dass „Automatische Bereitstellung des Überwachungs-Agents“ auf „Ein“ festgelegt ist

Mit dieser Blaupause wird eine [Azure Policy](../../../policy/overview.md)-Definition zugewiesen, mit der Sie sicherstellen können, dass die automatische Bereitstellung des Log Analytics-Agents aktiviert ist.

- Für Ihr Abonnement muss die automatische Bereitstellung des Log Analytics-Überwachungs-Agents aktiviert sein

## <a name="23-ensure-asc-default-policy-setting-monitor-system-updates-is-not-disabled"></a>2.3. Sicherstellen, dass die ASC-Standardrichtlinieneinstellung „Systemupdates überwachen“ nicht deaktiviert ist

Mit dieser Blaupause wird eine [Azure Policy](../../../policy/overview.md)-Definition zugewiesen, mit der Sie sicherstellen können, dass auf virtuellen Computern Systemupdates installiert werden.

- Systemupdates sollten auf Ihren Computern installiert sein.

## <a name="24-ensure-asc-default-policy-setting-monitor-os-vulnerabilities-is-not-disabled"></a>2.4. Sicherstellen, dass die ASC-Standardrichtlinieneinstellung „Betriebssystem-Sicherheitsrisiken überwachen“ nicht deaktiviert ist

Mit dieser Blaupause wird eine [Azure Policy](../../../policy/overview.md)-Definition zugewiesen, mit der Sie eine Überwachung auf unbehandelte Sicherheitsrisiken von virtuellen Computern durchführen können.

- Sicherheitsrisiken in der Sicherheitskonfiguration für Ihre Computer sollten beseitigt werden.

## <a name="25-ensure-asc-default-policy-setting-monitor-endpoint-protection-is-not-disabled"></a>2.5. Sicherstellen, dass die ASC-Standardrichtlinieneinstellung „Endpoint Protection überwachen“ nicht deaktiviert ist

Mit dieser Blaupause wird eine [Azure Policy](../../../policy/overview.md)-Definition zugewiesen, mit der Sie sicherstellen können, dass der Endpunktschutz auf den virtuellen Computern aktiviert ist.

- Fehlenden Endpoint Protection-Schutz in Azure Security Center überwachen

## <a name="26-ensure-asc-default-policy-setting-monitor-disk-encryption-is-not-disabled"></a>2.6. Sicherstellen, dass die ASC-Standardrichtlinieneinstellung „Datenträgerverschlüsselung überwachen“ nicht deaktiviert ist

Mit dieser Blaupause wird eine [Azure Policy](../../../policy/overview.md)-Definition zugewiesen, mit der Sie sicherstellen können, dass die Datenträger von virtuellen Computern verschlüsselt sind.

- Die Datenträgerverschlüsselung sollte auf virtuelle Computer angewendet werden.

## <a name="27-ensure-asc-default-policy-setting-monitor-network-security-groups-is-not-disabled"></a>2.7. Sicherstellen, dass die ASC-Standardrichtlinieneinstellung „Netzwerksicherheitsgruppen überwachen“ nicht deaktiviert ist

Mit dieser Blaupause wird eine [Azure Policy](../../../policy/overview.md)-Definition zugewiesen, mit der Sie virtuelle Computer mit Internetzugriff schützen können.

- NSG-Regeln für VMs mit Internetzugriff sollten verstärken werden.

## <a name="28-ensure-asc-default-policy-setting-monitor-web-application-firewall-is-not-disabled"></a>2.8. Sicherstellen, dass die ASC-Standardrichtlinieneinstellung „Web Application Firewall überwachen“ nicht deaktiviert ist

Mit dieser Blaupause wird eine [Azure Policy](../../../policy/overview.md)-Definition zugewiesen, mit der Sie virtuelle Computer schützen können, auf denen Webanwendungen ausgeführt werden.

- Die NSG-Regeln für Webanwendungen in IaaS sollten verstärkt werden.

## <a name="29-ensure-asc-default-policy-setting-enable-next-generation-firewallngfw-monitoring-is-not-disabled"></a>2.9. Sicherstellen, dass die ASC-Standardrichtlinieneinstellung „Next Generation Firewall-Überwachung aktivieren“ nicht deaktiviert ist

Mit dieser Blaupause werden [Azure Policy](../../../policy/overview.md)-Definitionen zugewiesen, mit denen Sie Subnetze und virtuelle Computer vor Bedrohungen schützen können, indem Sie den Zugriff einschränken. Die Security Center-Richtlinie, auf die von dieser CIS Microsoft Azure Foundations Benchmark-Empfehlung verwiesen wird, wurde durch zwei neue Empfehlungen ersetzt. Die unten aufgeführten Richtlinien gelten für die neuen Empfehlungen.

- Subnetze sollten einer Netzwerksicherheitsgruppe zugeordnet werden
- Virtuellen Computern muss eine Netzwerksicherheitsgruppe zugeordnet sein

## <a name="210-ensure-asc-default-policy-setting-monitor-vulnerability-assessment-is-not-disabled"></a>2.10. Sicherstellen, dass die ASC-Standardrichtlinieneinstellung „Sicherheitsrisikobewertung überwachen“ nicht deaktiviert ist

Mit dieser Blaupause wird eine [Azure Policy](../../../policy/overview.md)-Definition zugewiesen, mit der Sie sicherstellen können, dass Sicherheitsrisiken erkannt und beseitigt werden.

- Sicherheitsrisiken sollten durch eine Lösung zur Sicherheitsrisikobewertung beseitigt werden.

## <a name="211-ensure-asc-default-policy-setting-monitor-storage-blob-encryption-is-not-disabled"></a>2.11. Sicherstellen, dass die ASC-Standardrichtlinieneinstellung „Speicherblobverschlüsselung überwachen“ nicht deaktiviert ist

Die Azure Storage-Verschlüsselung wird für alle neuen und vorhandenen Speicherkonten aktiviert und kann nicht deaktiviert werden. (Hierbei handelt es sich um eine Azure-Standardfunktion ohne Richtlinienzuweisung.)

## <a name="212-ensure-asc-default-policy-setting-monitor-jit-network-access-is-not-disabled"></a>2.12. Sicherstellen, dass die ASC-Standardrichtlinieneinstellung „JIT-Netzwerkzugriff überwachen“ nicht deaktiviert ist

Mit dieser Blaupause wird eine [Azure Policy](../../../policy/overview.md)-Definition zugewiesen, mit der Sie den Zugriff auf virtuelle Computer steuern können.

- Die Just-In-Time-Netzwerkzugriffssteuerung sollte auf virtuelle Computer angewendet werden.

## <a name="213-ensure-asc-default-policy-setting-monitor-adaptive-application-whitelisting-is-not-disabled"></a>2.13. Sicherstellen, dass die ASC-Standardrichtlinieneinstellung „Monitor Adaptive Application Whitelisting“ (Whitelist für adaptive Anwendungssteuerung überwachen) nicht deaktiviert ist

Mit dieser Blaupause wird eine [Azure Policy](../../../policy/overview.md)-Definition zugewiesen, mit der Sie sicherstellen können, dass die adaptive Anwendungssteuerung auf den virtuellen Computern aktiviert ist.

- Die adaptive Anwendungssteuerung sollte auf virtuellen Computern aktiviert werden.

## <a name="214-ensure-asc-default-policy-setting-monitor-sql-auditing-is-not-disabled"></a>2.14. Sicherstellen, dass die ASC-Standardrichtlinieneinstellung „SQL-Überwachung überwachen“ nicht deaktiviert ist

Mit dieser Blaupause wird eine [Azure Policy](../../../policy/overview.md)-Definition zugewiesen, mit der Sie sicherstellen können, dass die SQL Server-Überwachung aktiviert ist.

- Überwachung muss für Advanced Data Security-Einstellungen für SQL Server aktiviert sein.

## <a name="215-ensure-asc-default-policy-setting-monitor-sql-encryption-is-not-disabled"></a>2.15. Sicherstellen, dass die ASC-Standardrichtlinieneinstellung „SQL-Verschlüsselung überwachen“ nicht deaktiviert ist

Mit dieser Blaupause wird eine [Azure Policy](../../../policy/overview.md)-Definition zugewiesen, mit der Sie sicherstellen können, dass für SQL-Datenbanken Transparent Data Encryption aktiviert ist.

- Transparent Data Encryption für SQL-Datenbanken sollte aktiviert werden.

## <a name="216-ensure-that-security-contact-emails-is-set"></a>2.16. Sicherstellen, dass „E-Mail-Adressen für Sicherheitskontakt“ festgelegt ist

Mit dieser Blaupause wird eine [Azure Policy](../../../policy/overview.md)-Definition zugewiesen, mit der Sie sicherstellen können, dass die Sicherheitsbenachrichtigungen richtig aktiviert sind.

- Für Ihr Abonnement muss eine E-Mail-Adresse als Sicherheitskontakt angegeben sein

## <a name="217-ensure-that-security-contact-phone-number-is-set"></a>2.17. Sicherstellen, dass für den Sicherheitskontakt die „Telefonnummer“ festgelegt ist

Mit dieser Blaupause wird eine [Azure Policy](../../../policy/overview.md)-Definition zugewiesen, mit der Sie sicherstellen können, dass die Sicherheitsbenachrichtigungen richtig aktiviert sind.

- Für Ihr Abonnement muss eine Telefonnummer für den Sicherheitskontakt angegeben werden

## <a name="218-ensure-that-send-email-notification-for-high-severity-alerts-is-set-to-on"></a>2.18. Sicherstellen, dass „E-Mail-Benachrichtigung für Warnungen mit hohem Schweregrad senden“ auf „Ein“ festgelegt ist

Mit dieser Blaupause wird eine [Azure Policy](../../../policy/overview.md)-Definition zugewiesen, mit der Sie sicherstellen können, dass die Sicherheitsbenachrichtigungen richtig aktiviert sind.

- E-Mail-Benachrichtigung zu Warnungen mit hohem Schweregrad muss aktiviert sein

## <a name="219-ensure-that-send-email-also-to-subscription-owners-is-set-to-on"></a>2.19. Sicherstellen, dass „E-Mails auch an Abonnementbesitzer senden“ auf „Ein“ festgelegt ist

Mit dieser Blaupause wird eine [Azure Policy](../../../policy/overview.md)-Definition zugewiesen, mit der Sie sicherstellen können, dass die Sicherheitsbenachrichtigungen richtig aktiviert sind.

- E-Mail-Benachrichtigung des Abonnementbesitzers bei Warnungen mit hohem Schweregrad muss aktiviert sein

## <a name="31-ensure-that-secure-transfer-required-is-set-to-enabled"></a>3.1. Sicherstellen, dass „Sichere Übertragung erforderlich“ aktiviert ist

Mit dieser Blaupause wird eine [Azure Policy](../../../policy/overview.md)-Definition zugewiesen, mit der Sie Speicherkonten überwachen können, für die unsichere Verbindungen zulässig sind.

- Sichere Übertragung in Speicherkonten sollte aktiviert werden.

## <a name="37-ensure-default-network-access-rule-for-storage-accounts-is-set-to-deny"></a>3.7. Sicherstellen, dass die Standard-Netzwerkzugriffsregel für Speicherkonten auf „Verweigern“ festgelegt ist

Mit dieser Blaupause wird eine [Azure Policy](../../../policy/overview.md)-Definition zugewiesen, mit der Sie Speicherkonten überwachen können, für die uneingeschränkter Zugriff zulässig ist.

- Nicht eingeschränkten Netzwerkzugriff auf Speicherkonten überwachen

## <a name="38-ensure-trusted-microsoft-services-is-enabled-for-storage-account-access"></a>3.8. Sicherstellen, dass „Vertrauenswürdige Microsoft-Dienste“ für den Speicherkontozugriff aktiviert ist

Mit dieser Blaupause wird eine [Azure Policy](../../../policy/overview.md)-Definition zugewiesen, mit der Sie Speicherkonten überwachen können, für die der Zugriff über vertrauenswürdige Microsoft-Dienste nicht zulässig ist.

- Speicherkonten sollten Zugriff von vertrauenswürdigen Microsoft-Diensten zulassen

## <a name="41-ensure-that-auditing-is-set-to-on"></a>4.1. Sicherstellen, dass „Überwachung“ auf „Ein“ festgelegt ist

Mit dieser Blaupause wird eine [Azure Policy](../../../policy/overview.md)-Definition zugewiesen, mit der Sie sicherstellen können, dass die SQL Server-Überwachung aktiviert ist. 

- Überwachung muss für Advanced Data Security-Einstellungen für SQL Server aktiviert sein.

## <a name="42-ensure-that-auditactiongroups-in-auditing-policy-for-a-sql-server-is-set-properly"></a>4.2. Sicherstellen, dass „AuditActionGroups“ in der Überwachungsrichtlinie für eine SQL Server-Instanz ordnungsgemäß festgelegt ist

Mit dieser Blaupause wird eine [Azure Policy](../../../policy/overview.md)-Definition zugewiesen, mit der Sie sicherstellen können, dass die SQL Server-Überwachung richtig konfiguriert ist.

- In den SQL-Überwachungseinstellungen sollten Aktionsgruppen zur Erfassung kritischer Aktivitäten konfiguriert sein.

## <a name="43-ensure-that-auditing-retention-is-greater-than-90-days"></a>4.3. Sicherstellen, dass die Aufbewahrungsdauer für die Überwachung auf über 90 Tage festgelegt ist

Mit dieser Blaupause wird eine [Azure Policy](../../../policy/overview.md)-Definition zugewiesen, mit der Sie sicherstellen können, dass SQL Server-Protokolle mindestens 90 Tage lang aufbewahrt werden.

- SQL Server-Instanzen sollten mit einer Überwachungsaufbewahrungsdauer von über 90 Tagen konfiguriert werden.

## <a name="44-ensure-that-advanced-data-security-on-a-sql-server-is-set-to-on"></a>4.4. Sicherstellen, dass „Advanced Data Security“ für eine SQL Server-Instanz auf „Ein“ festgelegt ist

Mit dieser Blaupause wird eine [Azure Policy](../../../policy/overview.md)-Definition zugewiesen, mit der Sie sicherstellen können, dass auf Computern mit SQL Server und auf verwalteten SQL-Instanzen Advanced Data Security aktiviert ist.

- Advanced Data Security muss für Ihre verwalteten SQL-Instanzen aktiviert werden.
- Advanced Data Security muss für Ihre SQL-Server aktiviert werden.

## <a name="45-ensure-that-threat-detection-types-is-set-to-all"></a>4.5. Sicherstellen, dass „Bedrohungstypen für Erkennung“ auf „Alle“ festgelegt ist

Mit dieser Blaupause werden [Azure Policy](../../../policy/overview.md)-Definitionen zugewiesen, mit denen Sie sicherstellen können, dass Advanced Threat Protection auf Computern mit SQL Server und auf verwalteten SQL-Instanzen richtig konfiguriert ist.

- Advanced Threat Protection-Typen sollten in den Advanced Data Security-Einstellungen für SQL Server auf „Alle“ festgelegt werden.
- Advanced Threat Protection-Typen sollten in den Advanced Data Security-Einstellungen für verwaltete SQL-Instanzen auf „Alle“ festgelegt werden.

## <a name="46-ensure-that-send-alerts-to-is-set"></a>4.6. Sicherstellen, dass „Warnungen senden an“ festgelegt ist

Mit dieser Blaupause wird eine [Azure Policy](../../../policy/overview.md)-Definition zugewiesen, mit der Sie sicherstellen können, dass die Benachrichtigungen für Advanced Data Security richtig aktiviert sind.

- Advanced Data Security-Einstellungen für verwaltete SQL-Instanzen sollten eine E-Mail-Adresse für den Empfang von Sicherheitswarnungen enthalten.
- Advanced Data Security-Einstellungen für SQL Server sollten eine E-Mail-Adresse für den Empfang von Sicherheitswarnungen enthalten.

## <a name="47-ensure-that-email-service-and-co-administrators-is-enabled"></a>4.7. Sicherstellen, dass „E-Mail-Dienst und Co-Administratoren“ aktiviert ist

Mit dieser Blaupause wird eine [Azure Policy](../../../policy/overview.md)-Definition zugewiesen, mit der Sie sicherstellen können, dass die Benachrichtigungen für Advanced Data Security richtig aktiviert sind.

- In den Advanced Data Security-Einstellungen für die verwaltete SQL-Instanz müssen E-Mail-Benachrichtigungen an Administratoren und Abonnementbesitzer aktiviert sein
- E-Mail-Benachrichtigungen an Administratoren und Abonnementbesitzer sollten in den erweiterten Einstellungen für Datensicherheit in SQL Server aktiviert werden.

## <a name="48-ensure-that-azure-active-directory-admin-is-configured"></a>4.8. Sicherstellen, dass der Azure Active Directory-Administrator konfiguriert ist

Mit dieser Blaupause wird eine [Azure Policy](../../../policy/overview.md)-Definition zugewiesen, mit der Sie sicherstellen können, dass für Computer mit SQL Server ein Azure Active Directory-Administrator bereitgestellt wird.

- Ein Azure Active Directory-Administrator sollte für SQL-Server-Instanzen bereitgestellt werden

## <a name="49-ensure-that-data-encryption-is-set-to-on-on-a-sql-database"></a>4.9. Sicherstellen, dass „Datenverschlüsselung“ für eine SQL-Datenbank auf „Ein“ festgelegt ist

Mit dieser Blaupause wird eine [Azure Policy](../../../policy/overview.md)-Definition zugewiesen, mit der Sie sicherstellen können, dass für SQL-Datenbanken Transparent Data Encryption aktiviert ist.

- Transparent Data Encryption für SQL-Datenbanken sollte aktiviert werden.

## <a name="410-ensure-sql-servers-tde-protector-is-encrypted-with-byok-use-your-own-key"></a>4.10. Sicherstellen, dass die TDE-Schutzvorrichtung der SQL Server-Instanz mit BYOK (also unter Verwendung eines eigenen Schlüssels) verschlüsselt ist

Mit dieser Blaupause werden [Azure Policy](../../../policy/overview.md)-Definitionen zugewiesen, mit denen Sie sicherstellen können, dass die TDE-Schutzvorrichtung für Computer mit SQL Server und verwaltete SQL-Instanzen mit Ihrem eigenen Schlüssel verschlüsselt ist.

- Die TDE-Schutzvorrichtung verwalteter SQL-Instanzen sollte mit Ihrem eigenen Schlüssel verschlüsselt werden.
- Die TDE-Schutzvorrichtung in SQL Server sollte mit Ihrem eigenen Schlüssel verschlüsselt werden.

## <a name="411-ensure-enforce-ssl-connection-is-set-to-enabled-for-mysql-database-server"></a>4.11. Sicherstellen, dass „SSL-Verbindung erzwingen“ für den MySQL-Datenbankserver auf „AKTIVIERT“ festgelegt ist

Mit dieser Blaupause wird eine [Azure Policy](../../../policy/overview.md)-Definition zugewiesen, mit der Sie sicherstellen können, dass für MySQL-Datenbankserver SSL-Verbindungen erzwungen werden.

- Erzwingen einer SSL-Verbindung muss für MySQL-Datenbankserver aktiviert sein

## <a name="413-ensure-enforce-ssl-connection-is-set-to-enabled-for-postgresql-database-server"></a>4.13. Sicherstellen, dass „SSL-Verbindung erzwingen“ für den PostgreSQL-Datenbankserver auf „AKTIVIERT“ festgelegt ist

Mit dieser Blaupause wird eine [Azure Policy](../../../policy/overview.md)-Definition zugewiesen, mit der Sie sicherstellen können, dass für PostgreSQL-Datenbankserver SSL-Verbindungen erzwungen werden.

- Erzwingen einer SSL-Verbindung muss für PostgreSQL-Datenbankserver aktiviert sein

## <a name="417-ensure-server-parameter-connection_throttling-is-set-to-on-for-postgresql-database-server"></a>4.17 Sicherstellen, dass der Serverparameter „connection_throttling“ für den PostgreSQL-Datenbankserver auf „EIN“ festgelegt ist

Mit dieser Blaupause wird eine [Azure Policy](../../../policy/overview.md)-Definition zugewiesen, mit der Sie das Risiko von Brute-Force-Angriffen auf PostgreSQL-Datenbankserver reduzieren können.

- Die Verbindungsdrosselung muss für PostgreSQL-Datenbankserver aktiviert sein

## <a name="419-ensure-that-azure-active-directory-admin-is-configured"></a>4.19. Sicherstellen, dass der Azure Active Directory-Administrator konfiguriert ist

Mit dieser Blaupause wird eine [Azure Policy](../../../policy/overview.md)-Definition zugewiesen, mit der Sie sicherstellen können, dass für Computer mit SQL Server ein Azure Active Directory-Administrator bereitgestellt wird. CIS Microsoft Azure Foundations Benchmark enthält diese Empfehlung. Es handelt sich aber um ein Duplikat von [Empfehlung 4.8](#48-ensure-that-azure-active-directory-admin-is-configured).

- Ein Azure Active Directory-Administrator sollte für SQL-Server-Instanzen bereitgestellt werden

## <a name="511-ensure-that-a-log-profile-exists"></a>5.1.1. Sicherstellen, dass ein Protokollprofil vorhanden ist

Mit dieser Blaupause wird eine [Azure Policy](../../../policy/overview.md)-Definition zugewiesen, mit der Sie sicherstellen können, dass für alle Azure-Abonnements ein Protokollprofil vorhanden ist. 

- Azure-Abonnements benötigen ein Protokollprofil für das Aktivitätsprotokoll

## <a name="512-ensure-that-activity-log-retention-is-set-365-days-or-greater"></a>5.1.2. Sicherstellen, dass die Speicherdauer für Aktivitätsprotokolle auf mindestens 365 Tage festgelegt ist

Mit dieser Blaupause wird eine [Azure Policy](../../../policy/overview.md)-Definition zugewiesen, mit der Sie sicherstellen können, dass Aktivitätsprotokolle mindestens ein Jahr lang aufbewahrt werden.

- Das Aktivitätsprotokoll muss mindestens ein Jahr lang aufbewahrt werden

## <a name="513-ensure-audit-profile-captures-all-the-activities"></a>5.1.3. Sicherstellen, dass mit dem Überwachungsprofil alle Aktivitäten erfasst werden

Mit dieser Blaupause wird eine [Azure Policy](../../../policy/overview.md)-Definition zugewiesen, mit der Sie sicherstellen können, dass das Protokollprofil richtig konfiguriert ist.

- Das Azure Monitor-Protokollprofil muss Protokolle für die Kategorien „write“, „delete“ und „action“ erfassen

## <a name="514-ensure-the-log-profile-captures-activity-logs-for-all-regions-including-global"></a>5.1.4. Sicherstellen, dass mit dem Protokollprofil die Aktivitätsprotokolle für alle Regionen erfasst werden (einschließlich global)

Mit dieser Blaupause wird eine [Azure Policy](../../../policy/overview.md)-Definition zugewiesen, mit der Sie sicherstellen können, dass das Protokollprofil richtig konfiguriert ist.

- Azure Monitor muss Aktivitätsprotokolle aus allen Regionen erfassen

## <a name="517-ensure-that-logging-for-azure-keyvault-is-enabled"></a>5.1.7. Sicherstellen, dass die Protokollierung für Azure Key Vault aktiviert ist

Mit dieser Blaupause wird eine [Azure Policy](../../../policy/overview.md)-Definition zugewiesen, mit der Sie sicherstellen können, dass Diagnoseprotokolle für Schlüsseltresore aktiviert sind.

- Diagnoseprotokolle in Key Vault sollten aktiviert sein.

## <a name="65-ensure-that-network-watcher-is-enabled"></a>6.5. Sicherstellen, dass Network Watcher aktiviert ist

Mit dieser Blaupause wird eine [Azure Policy](../../../policy/overview.md)-Definition zugewiesen, mit der Sie sicherstellen können, dass Network Watcher für alle Regionen mit bereitgestellten Ressourcen aktiviert ist. Für diese Richtlinie wird ein Parameterarray benötigt, in dem alle zutreffenden Regionen angegeben sind. Der Standardwert in dieser Richtlinien-Initiativendefinition lautet „eastus“.

- Network Watcher muss aktiviert sein

## <a name="71-ensure-that-os-disk-are-encrypted"></a>7.1. Sicherstellen, dass der Betriebssystem-Datenträger verschlüsselt ist

Mit dieser Blaupause wird eine [Azure Policy](../../../policy/overview.md)-Definition zugewiesen, mit der Sie sicherstellen können, dass die Datenträgerverschlüsselung auf den virtuellen Computern aktiviert ist.

- Die Datenträgerverschlüsselung sollte auf virtuelle Computer angewendet werden.

## <a name="72-ensure-that-data-disks-are-encrypted"></a>7.2. Sicherstellen, dass reguläre Datenträger verschlüsselt sind

Mit dieser Blaupause wird eine [Azure Policy](../../../policy/overview.md)-Definition zugewiesen, mit der Sie sicherstellen können, dass die Datenträgerverschlüsselung auf den virtuellen Computern aktiviert ist.

- Die Datenträgerverschlüsselung sollte auf virtuelle Computer angewendet werden.

## <a name="73-ensure-that-unattached-disks-are-encrypted"></a>7.3. Sicherstellen, dass „nicht angefügte Datenträger“ verschlüsselt sind

Mit dieser Blaupause wird eine [Azure Policy](../../../policy/overview.md)-Definition zugewiesen, mit der Sie sicherstellen können, dass nicht angefügte Datenträger verschlüsselt sind.

- Nicht angefügte Datenträger müssen verschlüsselt werden

## <a name="74-ensure-that-only-approved-extensions-are-installed"></a>7.4. Sicherstellen, dass nur genehmigte Erweiterungen installiert werden

Mit dieser Blaupause wird eine [Azure Policy](../../../policy/overview.md)-Definition zugewiesen, mit der Sie sicherstellen können, dass nur genehmigte Erweiterungen für virtuelle Computer installiert werden. Für diese Richtlinie wird ein Parameterarray benötigt, in dem alle genehmigten Erweiterungen für virtuelle Computer angegeben sind. Diese Richtlinien-Initiativendefinition enthält die vorgeschlagenen Standardwerte, die von Kunden überprüft werden sollten. 

 - Es dürfen nur genehmigte VM-Erweiterungen installiert werden

## <a name="75-ensure-that-the-latest-os-patches-for-all-virtual-machines-are-applied"></a>7.5. Sicherstellen, dass die neuesten Betriebssystempatches für alle virtuellen Computer angewendet werden

Mit dieser Blaupause wird eine [Azure Policy](../../../policy/overview.md)-Definition zugewiesen, mit der Sie sicherstellen können, dass auf virtuellen Computern Systemupdates installiert werden.

- Systemupdates sollten auf Ihren Computern installiert sein.

## <a name="76-ensure-that-the-endpoint-protection-for-all-virtual-machines-is-installed"></a>7.6. Sicherstellen, dass Endpoint Protection für alle virtuellen Computer installiert ist

Mit dieser Blaupause wird eine [Azure Policy](../../../policy/overview.md)-Definition zugewiesen, mit der Sie sicherstellen können, dass der Endpunktschutz auf den virtuellen Computern aktiviert ist.

- Fehlenden Endpoint Protection-Schutz in Azure Security Center überwachen

## <a name="84-ensure-the-key-vault-is-recoverable"></a>8.4. Sicherstellen, dass der Schlüsseltresor wiederhergestellt werden kann

Mit dieser Blaupause wird eine [Azure Policy](../../../policy/overview.md)-Definition zugewiesen, mit der Sie sicherstellen können, dass Schlüsseltresorobjekte nach dem versehentlichen Löschen wiederhergestellt werden können.

- Key Vault-Objekte müssen wiederherstellbar sein

## <a name="85-enable-role-based-access-control-rbac-within-azure-kubernetes-services"></a>8.5. Aktivieren der rollenbasierten Zugriffssteuerung (Role-Based Access Control, RBAC) in Azure Kubernetes Service

Mit dieser Blaupause wird eine [Azure Policy](../../../policy/overview.md)-Definition zugewiesen, mit der Sie sicherstellen können, dass die rollenbasierte Zugriffssteuerung in Kubernetes-Dienstclustern zum Verwalten der Berechtigungen verwendet wird.

- \[Vorschau\]: Die rollenbasierte Zugriffssteuerung (Role-Based Access Control, RBAC) sollte für Kubernetes Service verwendet werden.

## <a name="92-ensure-web-app-redirects-all-http-traffic-to-https-in-azure-app-service"></a>9.2. Sicherstellen, dass die Web-App den gesamten HTTP-Datenverkehr in Azure App Service an HTTPS umleitet

Mit dieser Blaupause wird eine [Azure Policy](../../../policy/overview.md)-Definition zugewiesen, mit der Sie sicherstellen können, dass auf Webanwendungen nur über sichere Verbindungen zugegriffen werden kann.

- Zugriff auf Webanwendung nur über HTTPS gestatten

## <a name="next-steps"></a>Nächste Schritte

Nachdem Sie sich nun mit der Steuerungszuordnung der Blaupause „CIS Microsoft Azure Foundations Benchmark“ vertraut gemacht haben, lesen Sie den folgenden Artikel, um sich über die Blaupause zu informieren, oder weisen Sie die Initiative über das Azure-Portal unter „Azure Policy“ zu:

> [!div class="nextstepaction"]
> [Übersicht über die Blaupause „CIS Microsoft Azure Foundations Benchmark“](./index.md)
> [Azure-Portal](https://portal.azure.com/#blade/Microsoft_Azure_Policy/PolicyMenuBlade/Definitions)

Weitere Artikel zu Blaupausen und ihrer Nutzung:

- Erfahren Sie mehr über den [Lebenszyklus von Blaupausen](../../concepts/lifecycle.md).
- Machen Sie sich mit der Verwendung [statischer und dynamischer Parameter](../../concepts/parameters.md) vertraut.
- Erfahren Sie, wie Sie die [Abfolge von Blaupausen](../../concepts/sequencing-order.md) anpassen können.
- Erfahren Sie, wie Sie [Ressourcen in Blaupausen sperren](../../concepts/resource-locking.md) können.
- Lernen Sie, wie Sie [vorhandene Zuweisungen aktualisieren](../../how-to/update-existing-assignments.md).