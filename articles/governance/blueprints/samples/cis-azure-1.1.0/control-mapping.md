---
title: 'Beispiel: Empfehlungszuordnung des Blaupausenbeispiels „CIS Microsoft Azure Foundations Benchmark“'
description: Empfehlungszuordnung des Blaupausenbeispiels „CIS Microsoft Azure Foundations Benchmark“ zu Azure Policy.
author: DCtheGeek
ms.author: dacoulte
ms.date: 08/09/2019
ms.topic: sample
ms.service: blueprints
manager: carmonm
ms.openlocfilehash: 118ff46ca3b640c6ec24ab85fd598f213229417a
ms.sourcegitcommit: dcf3e03ef228fcbdaf0c83ae1ec2ba996a4b1892
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 08/23/2019
ms.locfileid: "70014015"
---
# <a name="recommendation-mapping-of-the-cis-microsoft-azure-foundations-benchmark-blueprint-sample"></a>Empfehlungszuordnung des Blaupausenbeispiels „CIS Microsoft Azure Foundations Benchmark“

Im folgenden Artikel erfahren Sie, wie das Azure Blueprints-Blaupausenbeispiel „CIS Microsoft Azure Foundations Benchmark“ den Empfehlungen für „CIS Microsoft Azure Foundations Benchmark“ zugeordnet wird. Weitere Informationen zu den Empfehlungen finden Sie unter [CIS Microsoft Azure Foundations Benchmark](https://www.cisecurity.org/benchmark/azure/).

Die folgenden Zuordnungen gelten für die Empfehlungen für **CIS Microsoft Azure Foundations Benchmark v1.1.0**. Über den rechten Navigationsbereich können Sie direkt zu einer bestimmten Empfehlungszuordnung navigieren.
Viele der zugeordneten Empfehlungen werden mit einer [Azure Policy](../../../policy/overview.md)-Initiative implementiert. Zum Anzeigen der vollständigen Initiative öffnen Sie **Richtlinie** im Azure-Portal und wählen dann die Seite **Definitionen** aus. Suchen Sie anschließend die integrierte Richtlinieninitiative **\[Preview\] Audit CIS Microsoft Azure Foundations Benchmark v1.1.0 recommendations and deploy specific VM Extensions to support audit requirements** ([Vorschauversion] Empfehlungen für „CIS Microsoft Azure Foundations Benchmark v1.1.0“ überwachen und spezifische VM-Erweiterungen zur Unterstützung von Überwachungsanforderungen bereitstellen), und wählen Sie sie aus.

> [!NOTE]
> Die Vollversion des Blaupausenbeispiels wird in Kürze verfügbar. Die zugehörige Azure Policy-Initiative ist jetzt verfügbar.

## <a name="11-ensure-that-multi-factor-authentication-is-enabled-for-all-privileged-users"></a>1.1. Sicherstellen, dass die mehrstufige Authentifizierung für alle privilegierten Benutzer aktiviert ist

Diese Blaupause weist [Azure Policy](../../../policy/overview.md)-Definitionen zu, die dieser CIS-Empfehlung entsprechen.

- MFA sollte für Konten mit Besitzerberechtigungen in Ihrem Abonnement aktiviert sein.
- MFA sollte für Konten mit Schreibberechtigungen für Ihr Abonnement aktiviert werden.

## <a name="12-ensure-that-multi-factor-authentication-is-enabled-for-all-non-privileged-users"></a>1.2. Sicherstellen, dass die mehrstufige Authentifizierung für alle nicht privilegierten Benutzer aktiviert ist

Diese Blaupause weist eine [Azure Policy](../../../policy/overview.md)-Definition zu, die dieser CIS-Empfehlung entspricht.

- MFA sollte für Ihre Abonnementkonten mit Leseberechtigungen aktiviert sein

## <a name="13-ensure-that-there-are-no-guest-users"></a>1.3. Sicherstellen, dass keine Gastbenutzer vorhanden sind

Diese Blaupause weist [Azure Policy](../../../policy/overview.md)-Definitionen zu, die dieser CIS-Empfehlung entsprechen.

- Externe Konten mit Besitzerberechtigungen sollten aus Ihrem Abonnement entfernt werden.
- Externe Konten mit Leseberechtigungen sollten aus Ihrem Abonnement entfernt werden
- Externe Konten mit Schreibberechtigungen sollten aus Ihrem Abonnement entfernt werden.

## <a name="23-ensure-asc-default-policy-setting-monitor-system-updates-is-not-disabled"></a>2.3. Sicherstellen, dass die ASC-Standardrichtlinieneinstellung „Systemupdates überwachen“ nicht deaktiviert ist

Diese Blaupause weist eine [Azure Policy](../../../policy/overview.md)-Definition zu, die dieser CIS-Empfehlung entspricht.

- Systemupdates sollten auf Ihren Computern installiert sein.

## <a name="24-ensure-asc-default-policy-setting-monitor-os-vulnerabilities-is-not-disabled"></a>2.4. Sicherstellen, dass die ASC-Standardrichtlinieneinstellung „Betriebssystem-Sicherheitsrisiken überwachen“ nicht deaktiviert ist

Diese Blaupause weist eine [Azure Policy](../../../policy/overview.md)-Definition zu, die dieser CIS-Empfehlung entspricht.

- Sicherheitsrisiken in der Sicherheitskonfiguration für Ihre Computer sollten beseitigt werden.

## <a name="25-ensure-asc-default-policy-setting-monitor-endpoint-protection-is-not-disabled"></a>2.5. Sicherstellen, dass die ASC-Standardrichtlinieneinstellung „Endpoint Protection überwachen“ nicht deaktiviert ist

Diese Blaupause weist eine [Azure Policy](../../../policy/overview.md)-Definition zu, die dieser CIS-Empfehlung entspricht.

- Fehlenden Endpoint Protection-Schutz in Azure Security Center überwachen

## <a name="26-ensure-asc-default-policy-setting-monitor-disk-encryption-is-not-disabled"></a>2.6. Sicherstellen, dass die ASC-Standardrichtlinieneinstellung „Datenträgerverschlüsselung überwachen“ nicht deaktiviert ist

Diese Blaupause weist eine [Azure Policy](../../../policy/overview.md)-Definition zu, die dieser CIS-Empfehlung entspricht.

- Die Datenträgerverschlüsselung sollte auf virtuelle Computer angewendet werden.

## <a name="28-ensure-asc-default-policy-setting-monitor-web-application-firewall-is-not-disabled"></a>2.8. Sicherstellen, dass die ASC-Standardrichtlinieneinstellung „Web Application Firewall überwachen“ nicht deaktiviert ist

Diese Blaupause weist eine [Azure Policy](../../../policy/overview.md)-Definition zu, die dieser CIS-Empfehlung entspricht.

- Die NSG-Regeln für Webanwendungen in IaaS sollten verstärkt werden.

## <a name="210-ensure-asc-default-policy-setting-monitor-vulnerability-assessment-is-not-disabled"></a>2.10. Sicherstellen, dass die ASC-Standardrichtlinieneinstellung „Sicherheitsrisikobewertung überwachen“ nicht deaktiviert ist

Diese Blaupause weist eine [Azure Policy](../../../policy/overview.md)-Definition zu, die dieser CIS-Empfehlung entspricht.

- Sicherheitsrisiken sollten durch eine Lösung zur Sicherheitsrisikobewertung beseitigt werden.

## <a name="212-ensure-asc-default-policy-setting-monitor-jit-network-access-is-not-disabled"></a>2.12. Sicherstellen, dass die ASC-Standardrichtlinieneinstellung „JIT-Netzwerkzugriff überwachen“ nicht deaktiviert ist

Diese Blaupause weist eine [Azure Policy](../../../policy/overview.md)-Definition zu, die dieser CIS-Empfehlung entspricht.

- Die Just-In-Time-Netzwerkzugriffssteuerung sollte auf virtuelle Computer angewendet werden.

## <a name="215-ensure-asc-default-policy-setting-monitor-sql-encryption-is-not-disabled"></a>2.15. Sicherstellen, dass die ASC-Standardrichtlinieneinstellung „SQL-Verschlüsselung überwachen“ nicht deaktiviert ist

Diese Blaupause weist eine [Azure Policy](../../../policy/overview.md)-Definition zu, die dieser CIS-Empfehlung entspricht.

- Transparent Data Encryption für SQL-Datenbanken sollte aktiviert werden.

## <a name="31-ensure-that-secure-transfer-required-is-set-to-enabled"></a>3.1. Sicherstellen, dass „Sichere Übertragung erforderlich“ aktiviert ist

Diese Blaupause weist eine [Azure Policy](../../../policy/overview.md)-Definition zu, die dieser CIS-Empfehlung entspricht.

- Sichere Übertragung in Speicherkonten sollte aktiviert werden.

## <a name="37-ensure-default-network-access-rule-for-storage-accounts-is-set-to-deny"></a>3.7. Sicherstellen, dass die Standard-Netzwerkzugriffsregel für Speicherkonten auf „Verweigern“ festgelegt ist

Diese Blaupause weist eine [Azure Policy](../../../policy/overview.md)-Definition zu, die dieser CIS-Empfehlung entspricht.

- Nicht eingeschränkten Netzwerkzugriff auf Speicherkonten überwachen

## <a name="41-ensure-that-auditing-is-set-to-on"></a>4.1. Sicherstellen, dass „Überwachung“ auf „Ein“ festgelegt ist

Diese Blaupause weist eine [Azure Policy](../../../policy/overview.md)-Definition zu, die dieser CIS-Empfehlung entspricht.

- Überwachung muss für Advanced Data Security-Einstellungen für SQL Server aktiviert sein.

## <a name="42-ensure-that-auditactiongroups-in-auditing-policy-for-a-sql-server-is-set-properly"></a>4.2. Sicherstellen, dass „AuditActionGroups“ in der Überwachungsrichtlinie für eine SQL Server-Instanz ordnungsgemäß festgelegt ist

Diese Blaupause weist eine [Azure Policy](../../../policy/overview.md)-Definition zu, die dieser CIS-Empfehlung entspricht.

- In den SQL-Überwachungseinstellungen sollten Aktionsgruppen zur Erfassung kritischer Aktivitäten konfiguriert sein.

## <a name="43-ensure-that-auditing-retention-is-greater-than-90-days"></a>4.3. Sicherstellen, dass die Aufbewahrungsdauer für die Überwachung auf über 90 Tage festgelegt ist

Diese Blaupause weist eine [Azure Policy](../../../policy/overview.md)-Definition zu, die dieser CIS-Empfehlung entspricht.

- SQL Server-Instanzen sollten mit einer Überwachungsaufbewahrungsdauer von über 90 Tagen konfiguriert werden.

## <a name="44-ensure-that-advanced-data-security-on-a-sql-server-is-set-to-on"></a>4.4. Sicherstellen, dass „Advanced Data Security“ für eine SQL Server-Instanz auf „Ein“ festgelegt ist

Diese Blaupause weist eine [Azure Policy](../../../policy/overview.md)-Definition zu, die dieser CIS-Empfehlung entspricht.

- Advanced Data Security muss für Ihre SQL-Server aktiviert werden.

## <a name="45-ensure-that-threat-detection-types-is-set-to-all"></a>4.5. Sicherstellen, dass „Bedrohungstypen für Erkennung“ auf „Alle“ festgelegt ist

Diese Blaupause weist [Azure Policy](../../../policy/overview.md)-Definitionen zu, die dieser CIS-Empfehlung entsprechen.

- Advanced Threat Protection-Typen sollten in den Advanced Data Security-Einstellungen für SQL Server auf „Alle“ festgelegt werden.
- Advanced Threat Protection-Typen sollten in den Advanced Data Security-Einstellungen für verwaltete SQL-Instanzen auf „Alle“ festgelegt werden.

## <a name="46-ensure-that-send-alerts-to-is-set"></a>4.6. Sicherstellen, dass „Warnungen senden an“ festgelegt ist

Diese Blaupause weist eine [Azure Policy](../../../policy/overview.md)-Definition zu, die dieser CIS-Empfehlung entspricht.

- Advanced Data Security-Einstellungen für SQL Server sollten eine E-Mail-Adresse für den Empfang von Sicherheitswarnungen enthalten.

## <a name="47-ensure-that-email-service-and-co-administrators-is-enabled"></a>4.7. Sicherstellen, dass „E-Mail-Dienst und Co-Administratoren“ aktiviert ist

Diese Blaupause weist eine [Azure Policy](../../../policy/overview.md)-Definition zu, die dieser CIS-Empfehlung entspricht.

- Advanced Data Security-Einstellungen für verwaltete SQL-Instanzen sollten eine E-Mail-Adresse für den Empfang von Sicherheitswarnungen enthalten.

## <a name="48-ensure-that-azure-active-directory-admin-is-configured"></a>4.8. Sicherstellen, dass der Azure Active Directory-Administrator konfiguriert ist

Diese Blaupause weist eine [Azure Policy](../../../policy/overview.md)-Definition zu, die dieser CIS-Empfehlung entspricht.

- Ein Azure Active Directory-Administrator sollte für SQL-Server-Instanzen bereitgestellt werden

## <a name="49-ensure-that-data-encryption-is-set-to-on-on-a-sql-database"></a>4.9. Sicherstellen, dass „Datenverschlüsselung“ für eine SQL-Datenbank auf „Ein“ festgelegt ist

Diese Blaupause weist eine [Azure Policy](../../../policy/overview.md)-Definition zu, die dieser CIS-Empfehlung entspricht.

- Transparent Data Encryption für SQL-Datenbanken sollte aktiviert werden.

## <a name="410-ensure-sql-servers-tde-protector-is-encrypted-with-byok-use-your-own-key"></a>4.10. Sicherstellen, dass die TDE-Schutzvorrichtung der SQL Server-Instanz mit BYOK (also unter Verwendung eines eigenen Schlüssels) verschlüsselt ist

Diese Blaupause weist [Azure Policy](../../../policy/overview.md)-Definitionen zu, die dieser CIS-Empfehlung entsprechen.

- Die TDE-Schutzvorrichtung in SQL Server sollte mit Ihrem eigenen Schlüssel verschlüsselt werden.
- Die TDE-Schutzvorrichtung verwalteter SQL-Instanzen sollte mit Ihrem eigenen Schlüssel verschlüsselt werden.

## <a name="517-ensure-that-logging-for-azure-keyvault-is-enabled"></a>5.1.7. Sicherstellen, dass die Protokollierung für Azure Key Vault aktiviert ist

Diese Blaupause weist eine [Azure Policy](../../../policy/overview.md)-Definition zu, die dieser CIS-Empfehlung entspricht.

- Diagnoseprotokolle in Key Vault sollten aktiviert sein.

## <a name="71-ensure-that-os-disk-are-encrypted"></a>7.1. Sicherstellen, dass der Betriebssystem-Datenträger verschlüsselt ist

Diese Blaupause weist eine [Azure Policy](../../../policy/overview.md)-Definition zu, die dieser CIS-Empfehlung entspricht.

- Die Datenträgerverschlüsselung sollte auf virtuelle Computer angewendet werden.

## <a name="72-ensure-that-data-disks-are-encrypted"></a>7.2. Sicherstellen, dass reguläre Datenträger verschlüsselt sind

Diese Blaupause weist eine [Azure Policy](../../../policy/overview.md)-Definition zu, die dieser CIS-Empfehlung entspricht.

- Die Datenträgerverschlüsselung sollte auf virtuelle Computer angewendet werden.

## <a name="75-ensure-that-the-latest-os-patches-for-all-virtual-machines-are-applied"></a>7.5. Sicherstellen, dass die neuesten Betriebssystempatches für alle virtuellen Computer angewendet werden

Diese Blaupause weist eine [Azure Policy](../../../policy/overview.md)-Definition zu, die dieser CIS-Empfehlung entspricht.

- Systemupdates sollten auf Ihren Computern installiert sein.

## <a name="76-ensure-that-the-endpoint-protection-for-all-virtual-machines-is-installed"></a>7.6. Sicherstellen, dass Endpoint Protection für alle virtuellen Computer installiert ist

Diese Blaupause weist eine [Azure Policy](../../../policy/overview.md)-Definition zu, die dieser CIS-Empfehlung entspricht.

- Fehlenden Endpoint Protection-Schutz in Azure Security Center überwachen

## <a name="85-enable-role-based-access-control-rbac-within-azure-kubernetes-services"></a>8.5. Aktivieren der rollenbasierten Zugriffssteuerung (Role-Based Access Control, RBAC) in Azure Kubernetes Service

Diese Blaupause weist eine [Azure Policy](../../../policy/overview.md)-Definition zu, die dieser CIS-Empfehlung entspricht.

- \[Vorschau\]: Die rollenbasierte Zugriffssteuerung (Role-Based Access Control, RBAC) sollte für Kubernetes Service verwendet werden.

## <a name="92-ensure-web-app-redirects-all-http-traffic-to-https-in-azure-app-service"></a>9.2. Sicherstellen, dass die Web-App den gesamten HTTP-Datenverkehr in Azure App Service an HTTPS umleitet

Diese Blaupause weist eine [Azure Policy](../../../policy/overview.md)-Definition zu, die dieser CIS-Empfehlung entspricht.

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