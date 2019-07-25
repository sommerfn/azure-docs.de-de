---
title: Beispiel – Blaupause „NIST SP 800-53 R4“ – Steuerungszuordnung
description: Steuerungszuordnung des Blaupausenbeispiels „NIST SP 800-53 R4“ zu Azure Policy.
author: DCtheGeek
ms.author: dacoulte
ms.date: 06/24/2019
ms.topic: sample
ms.service: blueprints
manager: carmonm
ms.openlocfilehash: 251dbc396aea5694c4bdec45c194439c9476238b
ms.sourcegitcommit: 920ad23613a9504212aac2bfbd24a7c3de15d549
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 07/15/2019
ms.locfileid: "68226043"
---
# <a name="control-mapping-of-the-nist-sp-800-53-r4-blueprint-sample"></a>Steuerungszuordnung des Blaupausenbeispiels „NIST SP 800-53 R4“

In diesem Artikel wird erläutert, wie das Blaupausenbeispiel „NIST SP 800-53 R4“ von Azure Blueprints den NIST SP 800-53 R4-Steuerungen zugeordnet wird. Weitere Informationen zu den Steuerungen finden Sie unter [NIST SP 800-53](https://nvd.nist.gov/800-53).

Die folgenden Zuordnungen gelten für die Steuerungen unter **NIST SP 800-53 (Rev. 4)** . Über den rechten Navigationsbereich können Sie direkt zu einer bestimmten Steuerungszuordnung springen. Viele der zugeordneten Steuerungen werden mit einer [Azure Policy](../../../policy/overview.md)-Initiative implementiert. Zum Anzeigen der vollständigen Initiative öffnen Sie **Richtlinie** im Azure-Portal und wählen dann die Seite **Definitionen** aus. Suchen Sie anschließend die integrierte Richtlinieninitiative **\[Vorschauversion\]: NIST SP 800-53 R4-Steuerungen überwachen und spezifische VM-Erweiterungen zur Unterstützung von Überwachungsanforderungen bereitstellen**, und wählen Sie sie aus.

## <a name="ac-2-account-management"></a>AC-2 Kontoverwaltung

Diese Blaupause hilft Ihnen bei der Überprüfung von Konten, die u. U. nicht den Anforderungen Ihrer Organisation an die Kontoverwaltung entsprechen. Diese Blaupause weist fünf Azure Policy-Definitionen zu, die externe Konten mit Lese-, Schreib- und Besitzerberechtigungen für ein Abonnement und veraltete Konten prüfen. Durch die Überprüfung der durch diese Richtlinien überwachten Konten können Sie geeignete Maßnahmen ergreifen, um sicherzustellen, dass die Anforderungen an die Kontoverwaltung erfüllt werden.

- \[Vorschau\]: Veraltete Konten in einem Abonnement überwachen
- \[Vorschau\]: Veraltete Konten mit Besitzerberechtigungen für ein Abonnement überwachen
- \[Vorschau\]: Externe Konten mit Besitzerberechtigungen für ein Abonnement überwachen
- \[Vorschau\]: Externe Konten mit Leseberechtigungen für ein Abonnement überwachen
- \[Vorschau\]: Externe Konten mit Schreibberechtigungen für ein Abonnement überwachen

## <a name="ac-2-7-account-management--role-based-schemes"></a>AC-2 (7) Kontoverwaltung | Rollenbasierte Schemas

In Azure ist die [rollenbasierte Zugriffssteuerung ](../../../../role-based-access-control/overview.md) (RBAC) zur Verwaltung des Zugriffs auf Azure-Ressourcen implementiert. Über das Azure-Portal können Sie überprüfen, wer Zugriff auf Azure-Ressourcen und die zugehörigen Berechtigungen hat. Mit dieser Blaupause werden auch zwei [Azure Policy](../../../policy/overview.md)-Definitionen zugewiesen, um die Verwendung der Azure Active Directory-Authentifizierung für SQL Server-Instanzen und Service Fabric zu überwachen. Die Verwendung der Azure Active Directory-Authentifizierung ermöglicht eine vereinfachte Verwaltung von Berechtigungen und eine zentralisierte Identitätsverwaltung von Datenbankbenutzern und anderen Microsoft-Diensten.

- Bereitstellung eines Azure Active Directory-Administrators für SQL Server überwachen
- Verwendung von Azure Active Directory für Clientauthentifizierung in Service Fabric überwachen

## <a name="ac-2-12-account-management--account-monitoring--atypical-usage"></a>AC-2 (12) Kontoverwaltung | Kontoüberwachung/ungewöhnliche Nutzung

Mit Just-In-Time-Zugriff (JIT) auf virtuelle Computer wird eingehender Datenverkehr auf den virtuellen Azure-Computern gesperrt, um die Gefährdung durch Angriffe zu reduzieren und bei Bedarf einfachen Zugriff auf Verbindungen mit virtuellen Computern bereitzustellen. Alle JIT-Anforderungen des Zugriffs auf virtuelle Computer werden im Aktivitätsprotokoll protokolliert, sodass Sie atypische Nutzung überwachen können. Diese Blaupause weist eine [Azure Policy](../../../policy/overview.md)-Definition zu, die Ihnen hilft, virtuelle Computer zu überwachen, die Just-In-Time-Zugriff unterstützen können, aber noch nicht konfiguriert wurden.

- \[Vorschau\]: Möglichen Just-In-Time-Netzwerkzugriff in Azure Security Center überwachen

## <a name="ac-4-information-flow-enforcement"></a>AC-4 Erzwingung des Datenflusses

Ressourcenfreigabe zwischen verschiedenen Ursprüngen (Cross Origin Resource Sharing, CORS) kann das Anfordern von App Services-Ressourcen von einer externen Domäne ermöglichen. Microsoft empfiehlt, nur erforderlichen Domänen die Interaktion mit Ihrer API, Funktion und Webanwendungen zu ermöglichen. Diese Blaupause weist eine [Azure Policy](../../../policy/overview.md)-Definition zu, die Ihnen hilft, die Zugriffsbeschränkungen für CORS-Ressourcen in Azure Security Center zu überwachen.
Das Verstehen von CORS-Implementierungen kann Ihnen helfen, zu überprüfen, ob Informationsflusssteuerungen implementiert sind.

- \[Vorschau\]: CORS-Ressourcenzugriffsbeschränkungen für eine Webanwendung überwachen

## <a name="ac-5-separation-of-duties"></a>AC-5 Aufgabentrennung

Bei nur einem Azure-Abonnementbesitzer ist keine administrative Redundanz möglich. Im umgekehrten Fall können zu viele Azure-Abonnementbesitzer das Potenzial einer Sicherheitsverletzung durch ein kompromittiertes Besitzerkonto erhöhen. Mit dieser Blaupause können Sie eine angemessene Anzahl von Azure-Abonnementbesitzern beibehalten, indem zwei [Azure Policy](../../../policy/overview.md)-Definitionen zugewiesen werden, mit denen die Anzahl der Besitzer für Azure-Abonnements überwacht wird. Diese Blaupause weist auch vier Azure Policy-Definitionen zu, mit denen Sie die Mitgliedschaft in der Administratorengruppe auf virtuellen Windows-Computern steuern können. Über die Verwaltung von Berechtigungen für Abonnementbesitzer und Administratoren von virtuellen Computern können Sie eine entsprechende Aufgabentrennung implementieren.

- \[Vorschau\]: Höchstanzahl der Besitzer für ein Abonnement überwachen
- \[Vorschau\]: Mindestanzahl der Besitzer für ein Abonnement überwachen
- Überwachen, dass die Administratorengruppe innerhalb von virtuellen Windows-Computern die angegebenen Mitglieder ausschließt
- Überwachen, dass die Administratorengruppe innerhalb von virtuellen Windows-Computern die angegebenen Mitglieder einschließt
- Bereitstellen einer VM-Erweiterung zum Überwachen, dass die Administratorengruppe innerhalb von virtuellen Windows-Computern die angegebenen Mitglieder ausschließt
- Bereitstellen einer VM-Erweiterung zum Überwachen, dass die Administratorengruppe innerhalb von virtuellen Windows-Computern die angegebenen Mitglieder einschließt

## <a name="ac-6-7-least-privilege--review-of-user-privileges"></a>AC-6 (7) Ansatz der geringsten Rechte | Kontrolle von Benutzerberechtigungen

In Azure ist die [rollenbasierte Zugriffssteuerung ](../../../../role-based-access-control/overview.md) (RBAC) zur Verwaltung des Zugriffs auf Azure-Ressourcen implementiert. Über das Azure-Portal können Sie überprüfen, wer Zugriff auf Azure-Ressourcen und die zugehörigen Berechtigungen hat. Diese Blaupause weist sechs [Azure Policy](../../../policy/overview.md)-Definitionen zu, um Konten zu überwachen, die zur Überprüfung priorisiert werden sollten. Die Überprüfung dieser Kontoindikatoren kann Ihnen helfen, sicherzustellen, dass die Steuerungen mit den geringsten Rechten implementiert sind.

- \[Vorschau\]: Höchstanzahl der Besitzer für ein Abonnement überwachen
- \[Vorschau\]: Mindestanzahl der Besitzer für ein Abonnement überwachen
- Überwachen, dass die Administratorengruppe innerhalb von virtuellen Windows-Computern die angegebenen Mitglieder ausschließt
- Überwachen, dass die Administratorengruppe innerhalb von virtuellen Windows-Computern die angegebenen Mitglieder einschließt
- Bereitstellen einer VM-Erweiterung zum Überwachen, dass die Administratorengruppe innerhalb von virtuellen Windows-Computern die angegebenen Mitglieder ausschließt
- Bereitstellen einer VM-Erweiterung zum Überwachen, dass die Administratorengruppe innerhalb von virtuellen Windows-Computern die angegebenen Mitglieder einschließt

## <a name="ac-17-1-remote-access--automated-monitoring--control"></a>AC-17 (1) Remotezugriff | Automatisierte Überwachung/Steuerung

Diese Blaupause hilft Ihnen bei der Überwachung und Steuerung des Remotezugriffs, indem drei [Azure Policy](../../../policy/overview.md)-Definitionen zugewiesen werden. Diese überwachen, dass das Remote-Debugging für die Azure App Service-Anwendung deaktiviert ist. Zwei weitere Richtliniendefinitionen überwachen virtuelle Linux-Computer, die Remoteverbindungen von Konten ohne Kennwörter zulassen. Diese Blaupause weist auch eine Azure Policy-Definition zu, mit der Sie den uneingeschränkten Zugriff auf Speicherkonten überwachen können. Die Überwachung dieser Indikatoren kann Ihnen helfen, sicherzustellen, dass Remotezugriffsmethoden Ihrer Sicherheitsrichtlinie entsprechen.

- \[Vorschau\]: Remotedebugstatus für eine Funktions-App überwachen
- \[Vorschau\]: Remotedebugstatus für eine Webanwendung überwachen
- \[Vorschau\]: Remotedebugstatus für eine API-App überwachen
- \[Vorschau\]: Überwachen, dass Linux-VMs keine Remoteverbindungen über Konten ohne Kennwörter zulassen
- \[Vorschau\]: Bereitstellen einer VM-Erweiterung zur Überwachung, dass Linux-VMs keine Remoteverbindungen über Konten ohne Kennwörter zulassen
- Nicht eingeschränkten Netzwerkzugriff auf Speicherkonten überwachen

## <a name="au-3-2-content-of-audit-records--centralized-management-of-planned-audit-record-content"></a>AU-3 (2) Inhalt der Überwachungsdatensätze | Zentrale Verwaltung von Inhalten geplanter Überwachungsdatensätze

Von Azure Monitor erfasste Protokolldaten werden in einem Log Analytics-Arbeitsbereich gespeichert, um eine zentrale Konfiguration und Verwaltung zu ermöglichen. Mit dieser Blaupause können Sie sicherstellen, dass Ereignisse protokolliert werden, indem sieben [Azure Policy](../../../policy/overview.md)-Definitionen zugewiesen werden, die die Bereitstellung des Log Analytics-Agent auf virtuellen Azure-Computern überwachen und erzwingen.

- \[Vorschau\]: Bereitstellung des Log Analytics-Agents überwachen – VM-Image (Betriebssystem) nicht aufgelistet
- \[Vorschau\]: Bereitstellung des Log Analytics-Agents in VM-Skalierungsgruppen überwachen – VM-Image (Betriebssystem) nicht aufgelistet
- \[Vorschau\]: Überwachen des Log Analytics-Arbeitsbereichs für VM – Berichtskonflikt
- \[Vorschau\]: Log Analytics-Agent für Linux-VM-Skalierungsgruppen bereitstellen
- \[Vorschau\]: Bereitstellen des Log Analytics-Agents für Linux-VMs
- \[Vorschau\]: Log Analytics-Agent für Windows-VM-Skalierungsgruppen bereitstellen
- \[Vorschau\]: Bereitstellen des Log Analytics-Agents für Windows-VMs

## <a name="au-5-response-to-audit-processing-failures"></a>AU-5 Reaktion auf Verarbeitungsfehler bei Überwachungsinformationen

Diese Blaupause weist fünf [Azure Policy](../../../policy/overview.md)-Definitionen zu, die Überprüfungs- und Ereignisprotokollierungskonfigurationen überwachen. Die Überwachung dieser Konfigurationen kann einen Indikator für einen Ausfall oder eine Fehlkonfiguration eines Überprüfungssystems bereitstellen und Ihnen helfen, Korrekturmaßnahmen zu ergreifen.

- \[Vorschau\]: Nicht überwachte SQL-Datenbank in Azure Security Center überwachen
- Überwachen der Diagnoseeinstellung
- Überwachen von verwalteten SQL-Instanzen ohne Advanced Data Security
- Überwachungseinstellungen auf SQL Server-Ebene überwachen
- Überwachen von SQL-Servern ohne Advanced Data Security

## <a name="au-6-4-audit-review-analysis-and-reporting--central-review-and-analysis"></a>AU-6 (4) Prüfung, Analyse und Berichterstellung in Bezug auf die Überwachung | Zentrale Überprüfung und Analyse

Von Azure Monitor erfasste Protokolldaten werden in einem Log Analytics-Arbeitsbereich gespeichert, um eine zentrale Berichterstattung und Analyse zu ermöglichen. Mit dieser Blaupause können Sie sicherstellen, dass Ereignisse protokolliert werden, indem sieben [Azure Policy](../../../policy/overview.md)-Definitionen zugewiesen werden, die die Bereitstellung des Log Analytics-Agent auf virtuellen Azure-Computern überwachen und erzwingen.

- \[Vorschau\]: Bereitstellung des Log Analytics-Agents überwachen – VM-Image (Betriebssystem) nicht aufgelistet
- \[Vorschau\]: Bereitstellung des Log Analytics-Agents in VM-Skalierungsgruppen überwachen – VM-Image (Betriebssystem) nicht aufgelistet
- \[Vorschau\]: Überwachen des Log Analytics-Arbeitsbereichs für VM – Berichtskonflikt
- \[Vorschau\]: Log Analytics-Agent für Linux-VM-Skalierungsgruppen bereitstellen
- \[Vorschau\]: Bereitstellen des Log Analytics-Agents für Linux-VMs
- \[Vorschau\]: Log Analytics-Agent für Windows-VM-Skalierungsgruppen bereitstellen
- \[Vorschau\]: Bereitstellen des Log Analytics-Agents für Windows-VMs

## <a name="au-12-audit-generation"></a>AU-12 Generierung von Überwachungsdatensätzen

Mit dieser Blaupause können Sie sicherstellen, dass Systemereignisse protokolliert werden, indem Sie 15 [Azure Policy](../../../policy/overview.md)-Definitionen zuweisen, mit denen die Protokolleinstellungen für Azure-Ressourcen überwacht werden. Diese Richtliniendefinitionen überprüfen und erzwingen die Bereitstellung des Log Analytics-Agents auf virtuellen Azure-Computern und die Konfiguration von Überprüfungseinstellungen für andere Azure-Ressourcentypen. Diese Richtliniendefinitionen überprüfen auch die Konfiguration von Diagnoseprotokollen, um Einblick in Vorgänge zu erhalten, die innerhalb von Azure-Ressourcen ausgeführt werden. Darüber hinaus werden Überprüfung und Advanced Data Security auf SQL-Servern konfiguriert.

- \[Vorschau\]: Bereitstellung des Log Analytics-Agents überwachen – VM-Image (Betriebssystem) nicht aufgelistet
- \[Vorschau\]: Bereitstellung des Log Analytics-Agents in VM-Skalierungsgruppen überwachen – VM-Image (Betriebssystem) nicht aufgelistet
- \[Vorschau\]: Überwachen des Log Analytics-Arbeitsbereichs für VM – Berichtskonflikt
- \[Vorschau\]: Log Analytics-Agent für Linux-VM-Skalierungsgruppen bereitstellen
- \[Vorschau\]: Bereitstellen des Log Analytics-Agents für Linux-VMs
- \[Vorschau\]: Log Analytics-Agent für Windows-VM-Skalierungsgruppen bereitstellen
- \[Vorschau\]: Bereitstellen des Log Analytics-Agents für Windows-VMs
- \[Vorschau\]: Nicht überwachte SQL-Datenbank in Azure Security Center überwachen
- Anwenden von Diagnoseeinstellungen für Netzwerksicherheitsgruppen
- Überwachen der Diagnoseeinstellung
- Überwachen von verwalteten SQL-Instanzen ohne Advanced Data Security
- Überwachungseinstellungen auf SQL Server-Ebene überwachen
- Überwachen von SQL-Servern ohne Advanced Data Security
- Bereitstellen von Advanced Data Security auf SQL-Servern
- Bereitstellen von Auditing auf SQL-Servern

## <a name="cm-7-2-least-functionality--prevent-program-execution"></a>CM-7 (2) Mindestfunktionalität | Unterbinden der Programmausführung

Die adaptive Anwendungssteuerung im Azure Security Center ist eine intelligente, automatisierte End-to-End-Whitelistlösung für Anwendungen, die die Ausführung bestimmter Software auf Ihren virtuellen Computern blockieren oder verhindern kann. Anwendungssteuerung kann in einem Erzwingungsmodus ausgeführt werden, der die Ausführung nicht genehmigter Anwendungen verhindert. Diese Blaupause weist eine Azure Policy-Definition zu, mit der Sie virtuelle Computer überwachen können, für die eine Anwendungswhitelist empfohlen wird, aber noch nicht konfiguriert wurde.

- \[Vorschau\]: Mögliche App-Whitelist in Azure Security Center überwachen

## <a name="cm-7-5-least-functionality--authorized-software--whitelisting"></a>CM-7 (5) Mindestfunktionalität | Autorisierte Software und Whitelists

Die adaptive Anwendungssteuerung im Azure Security Center ist eine intelligente, automatisierte End-to-End-Whitelistlösung für Anwendungen, die die Ausführung bestimmter Software auf Ihren virtuellen Computern blockieren oder verhindern kann. Mit Anwendungssteuerung können Sie Listen von genehmigten Anwendungen für Ihre virtuellen Computer erstellen. Diese Blaupause weist eine [Azure Policy](../../../policy/overview.md)-Definition zu, mit der Sie virtuelle Computer überwachen können, für die eine Anwendungswhitelist empfohlen wird, aber noch nicht konfiguriert wurde.

- \[Vorschau\]: Mögliche App-Whitelist in Azure Security Center überwachen

## <a name="cm-11-user-installed-software"></a>CM-11 Von Benutzern installierte Software

Die adaptive Anwendungssteuerung im Azure Security Center ist eine intelligente, automatisierte End-to-End-Whitelistlösung für Anwendungen, die die Ausführung bestimmter Software auf Ihren virtuellen Computern blockieren oder verhindern kann. Mit Anwendungssteuerung können Sie die Einhaltung von Software-Einschränkungsrichtlinien durchsetzen und überwachen. Diese Blaupause weist eine [Azure Policy](../../../policy/overview.md)-Definition zu, mit der Sie virtuelle Computer überwachen können, für die eine Anwendungswhitelist empfohlen wird, aber noch nicht konfiguriert wurde.

- \[Vorschau\]: Mögliche App-Whitelist in Azure Security Center überwachen

## <a name="cp-7-alternate-processing-site"></a>CP-7 Alternativer Verarbeitungsstandort

Azure Site Recovery repliziert Workloads, die auf virtuellen Computern ausgeführt werden, von einem primären Standort an einen sekundären Standort. Bei einem Ausfall am primären Standort erfolgt ein Failover der Workload auf den sekundären Standort. Diese Blaupause weist eine [Azure Policy](../../../policy/overview.md)-Definition zu, die virtuelle Computer ohne konfigurierte Notfallwiederherstellungskonfiguration überwacht. Durch Überwachung dieses Indikators können Sie sicherstellen, dass die erforderlichen Notfallsteuerungen vorhanden sind.

- Überwachen von virtuellen Computern ohne konfigurierte Notfallwiederherstellung

## <a name="ia-2-1-identification-and-authentication-organizational-users--network-access-to-privileged-accounts"></a>IA-2 (1) Identifikation und Authentifizierung (Organisationsbenutzer) | Netzwerkzugriff auf privilegierte Konten

Mit dieser Blaupause können Sie den privilegierten Zugriff einschränken und steuern, indem zwei [Azure Policy](../../../policy/overview.md)-Definitionen zugewiesen werden, um Konten mit Besitzer- und/oder Schreibberechtigungen ohne aktivierte mehrstufige Authentifizierung zu überwachen. Über die mehrstufige Authentifizierung können Konten geschützt werden, auch wenn bestimmte Authentifizierungsinformationen kompromittiert sind. Durch die Überwachung von Konten ohne aktivierte mehrstufige Authentifizierung können Sie die Konten identifizieren, die möglicherweise eher kompromittiert werden.

- \[Vorschau\]: Konten mit Besitzerberechtigungen überwachen, für die MFA in einem Abonnement nicht aktiviert ist
- \[Vorschau\]: Konten mit Schreibberechtigungen überwachen, für die MFA in einem Abonnement nicht aktiviert ist

## <a name="ia-2-2-identification-and-authentication-organizational-users--network-access-to-non-privileged-accounts"></a>IA-2 (2) Identifikation und Authentifizierung (Organisationsbenutzer) | Netzwerkzugriff auf nicht privilegierte Konten

Mit dieser Blaupause können Sie den Zugriff einschränken und steuern, indem eine [Azure Policy](../../../policy/overview.md)-Definition zugewiesen wird, um Konten mit Leseberechtigungen ohne aktivierte mehrstufige Authentifizierung zu überwachen. Über die mehrstufige Authentifizierung können Konten geschützt werden, auch wenn bestimmte Authentifizierungsinformationen kompromittiert sind. Durch die Überwachung von Konten ohne aktivierte mehrstufige Authentifizierung können Sie die Konten identifizieren, die möglicherweise eher kompromittiert werden.

- \[Vorschau\]: Konten mit Leseberechtigungen überwachen, für die MFA in einem Abonnement nicht aktiviert ist

## <a name="ia-5-authenticator-management"></a>IA-5 Authentifikatorverwaltung

Diese Blaupause weist fünf [Azure Policy](../../../policy/overview.md)-Definitionen zu, die virtuelle Linux-Computer überwachen, die Remoteverbindungen von Konten ohne Kennwörter zulassen und/oder falsche Berechtigungen in der passwd-Datei haben. Diese Blaupause weist auch eine Richtliniendefinition zu, die die Konjugation des Kennwortverschlüsselungstyps für virtuelle Windows-Computer überwacht. Durch Überwachung dieser Indikatoren können Sie sicherstellen, dass die Systemauthentifikatoren die Identifizierung und Authentifizierungsrichtlinien Ihrer Organisation einhalten.

- \[Vorschau\]: Überwachen, dass Linux-VMs keine Konten ohne Kennwörter verwenden
- \[Vorschau\]: Bereitstellen einer VM-Erweiterung zur Überwachung, dass Linux-VMs keine Konten ohne Kennwörter verwenden
- \[Vorschau\]: Überwachen, dass bei Linux-VMs die passwd-Dateiberechtigungen auf „0644“ festgelegt sind
- \[Vorschau\]: Überwachen, dass Windows-VMs die Kennwörter mit umkehrbarer Verschlüsselung speichern
- \[Vorschau\]: Bereitstellen einer VM-Erweiterung zur Überwachung, dass bei Linux-VMs die passwd-Dateiberechtigungen auf „0644“ festgelegt sind

## <a name="ia-5-1-authenticator-management--password-based-authentication"></a>IA-5 (1) Authentifikatorverwaltung |Kennwortbasierte Authentifizierung

Mit dieser Blaupause können Sie sichere Kennwörter erzwingen, indem 12 [Azure Policy](../../../policy/overview.md)-Definitionen zugewiesen werden, mit denen virtuelle Windows-Computer überwacht werden, auf denen keine Mindestsicherheitsanforderungen oder andere Anforderungen für Kennwörter erzwungen werden. Aufgrund der Informationen zu virtuellen Computern, die gegen die Richtlinie zur Kennwortsicherheit verstoßen, können Sie Korrekturmaßnahmen ergreifen, um sicherzustellen, dass die Kennwörter für alle Benutzerkonten auf virtuellen Computern mit der Kennwortrichtlinie Ihrer Organisation konform sind.

- \[Vorschau\]: Überwachen, dass Windows-VMs die vorherigen 24 Kennwörter nicht wiederverwenden können
- \[Vorschau\]: Überwachen, dass das maximale Kennwortalter von 70 Tagen für Windows-VMs gilt
- \[Vorschau\]: Überwachen, dass das minimale Kennwortalter von 1 Tag für Windows-VMs gilt
- \[Vorschau\]: Überwachen, dass auf Windows-VMs die Einstellung für die Kennwortkomplexität aktiviert ist
- \[Vorschau\]: Überwachen, dass auf Windows-VMs eine Mindestkennwortlänge von 14 Zeichen festgelegt ist
- \[Vorschau\]: Überwachen, dass Windows-VMs die Kennwörter mit umkehrbarer Verschlüsselung speichern
- \[Vorschau\]: Bereitstellen einer VM-Erweiterung zur Überwachung, dass Windows-VMs die vorherigen 24 Kennwörter nicht wiederverwenden können
- \[Vorschau\]: Bereitstellen einer VM-Erweiterung zur Überwachung, dass das maximale Kennwortalter von 70 Tagen für Windows-VMs gilt
- \[Vorschau\]: Bereitstellen einer VM-Erweiterung zur Überwachung, dass das maximale Kennwortalter von 1 Tag für Windows-VMs gilt
- \[Vorschau\]: Bereitstellen einer VM-Erweiterung zur Überwachung, dass auf Windows-VMs die Einstellung für die Kennwortkomplexität aktiviert ist
- \[Vorschau\]: Bereitstellen einer VM-Erweiterung zur Überwachung, dass auf Windows-VMs eine Mindestkennwortlänge von 14 Zeichen festgelegt ist
- \[Vorschau\]: Bereitstellen einer VM-Erweiterung zur Überwachung, dass Windows-VMs die Kennwörter mit umkehrbarer Verschlüsselung speichern

## <a name="ra-5-vulnerability-scanning"></a>RA-5 Überprüfung auf Sicherheitsrisiken

Mit dieser Blaupause können Sie Sicherheitsrisiken im Informationssystem verwalten, indem vier [Azure Policy](../../../policy/overview.md)-Definitionen zugewiesen werden, die Sicherheitsrisiken des Betriebssystems, SQL-Sicherheitsrisiken und Sicherheitsrisiken von virtuellen Computern in Azure Security Center überwachen. Azure Security Center umfasst Funktionen zur Berichterstellung, über die Sie in Echtzeit Einblick in den Sicherheitsstatus von bereitgestellten Azure-Ressourcen erhalten. Diese Blaupause weist außerdem drei Richtliniendefinitionen zu, die Advanced Data Security auf SQL-Servern überprüfen und erzwingen. Erweiterte Datensicherheit umfasst Sicherheitsrisikobewertung und erweiterte Funktionen zum Schutz vor Bedrohungen, damit Sie Sicherheitsrisiken in Ihren bereitgestellten Ressourcen besser verstehen.

- Überwachen von verwalteten SQL-Instanzen ohne Advanced Data Security
- Überwachen von SQL-Servern ohne Advanced Data Security
- Bereitstellen von Advanced Data Security auf SQL-Servern
- \[Vorschau\]: Betriebssystem-Sicherheitsrisiken für Ihre VM-Skalierungsgruppen in Azure Security Center überwachen
- \[Vorschau\]: Betriebssystem-Sicherheitsrisiken in Azure Security Center überwachen
- \[Vorschau\]: Ergebnisse der SQL-Sicherheitsrisikobewertung in Azure Security Center überwachen
- \[Vorschau\]: VM-Sicherheitsrisiken in Azure Security Center überwachen

## <a name="sc-5-denial-of-service-protection"></a>SC-5 Schutz vor Denial-of-Service-Angriffen

Die DDoS-Standard-Tarif (Distributed Denial of Service) von Azure bietet mehr Features und Risikominderungsfunktionen als die Dienstebene „Basic“. Zu diesen zusätzlichen Features gehören die Integration von Azure Monitor und die Möglichkeit, Berichte zur Entschärfung nach einem Angriff zu überprüfen. Diese Blaupause weist eine [Azure Policy](../../../policy/overview.md)-Definition zu, die prüft, ob der DDoS-Standard-Tarif aktiviert ist. Das Verständnis des Funktionsunterschieds zwischen den Dienstebene kann Ihnen helfen, die beste Lösung für den Schutz vor Denial-of-Service-Angriffen für Ihre Azure-Umgebung auszuwählen.

- \[Vorschau\]: Aktivierung des Standard-Tarifs von DDoS Protection für ein virtuelles Netzwerk überwachen

## <a name="sc-7-boundary-protection"></a>SC-7 Schutz von Grenzen

Mit dieser Blaupause können Sie die Systemgrenze verwalten und steuern, indem eine [Azure Policy](../../../policy/overview.md)-Definition zugewiesen wird, die Netzwerksicherheitsgruppen mit wenig einschränkenden Regeln überwacht. Regeln, die zu wenig einschränkend sind, können einen unbeabsichtigten Netzwerkzugriff ermöglichen und sollten überprüft werden. Diese Blaupause weist auch eine Richtliniendefinition zu, die Empfehlungen für die Härtung von Netzwerksicherheitsgruppen in Azure Security Center überprüft. Azure Security Center analysiert Datenverkehrsmuster von virtuellen Computern mit Internetanbindung und gibt Empfehlungen für Netzwerksicherheitsgruppen-Regeln, um die potenzielle Angriffsfläche zu reduzieren.
Mit dieser Blaupause werden außerdem drei Richtliniendefinitionen zugewiesen, die nicht geschützte Endpunkte, Anwendungen und Speicherkonten überwachen. Endpunkte und Anwendungen, die nicht durch eine Firewall geschützt sind, sowie Speicherkonten mit uneingeschränktem Zugriff können einen unbeabsichtigten Zugriff auf Informationen im Informationssystem ermöglichen.

- \[Vorschau\]: Virtuelle Computer mit Internetzugriff auf Empfehlungen zum Absichern des NSG-Datenverkehrs überwachen
- \[Vorschau\]: Zu wenig einschränkenden Netzwerkzugriff in Azure Security Center überwachen
- \[Vorschau\]: Nicht geschützte Netzwerkendpunkte in Azure Security Center überwachen
- \[Vorschau\]: Nicht geschützte Webanwendung in Azure Security Center überwachen
- Nicht eingeschränkten Netzwerkzugriff auf Speicherkonten überwachen

## <a name="sc-7-3-boundary-protection--access-points"></a>SC-7 (3) Schutz von Grenzen | Zugriffspunkte

Mit Just-In-Time-Zugriff (JIT) auf virtuelle Computer wird eingehender Datenverkehr auf den virtuellen Azure-Computern gesperrt, um die Gefährdung durch Angriffe zu reduzieren und bei Bedarf einfachen Zugriff auf Verbindungen mit virtuellen Computern bereitzustellen. JIT-Zugriff auf virtuelle Computer hilft Ihnen, die Anzahl von externen Verbindungen mit Ihren Ressourcen in Azure zu begrenzen. Diese Blaupause weist eine [Azure Policy](../../../policy/overview.md)-Definition zu, die Ihnen hilft, virtuelle Computer zu überwachen, die Just-In-Time-Zugriff unterstützen können, aber noch nicht konfiguriert wurden.

- \[Vorschau\]: Möglichen Just-In-Time-Netzwerkzugriff in Azure Security Center überwachen

## <a name="sc-7-4-boundary-protection--external-telecommunications-services"></a>SC-7 (4) Schutz von Grenzen | Externe Telekommunikationsdienste

Mit Just-In-Time-Zugriff (JIT) auf virtuelle Computer wird eingehender Datenverkehr auf den virtuellen Azure-Computern gesperrt, um die Gefährdung durch Angriffe zu reduzieren und bei Bedarf einfachen Zugriff auf Verbindungen mit virtuellen Computern bereitzustellen. JIT-Zugriff auf virtuelle Computer hilft Ihnen bei der Verwaltung von Ausnahmen von Ihrer Datenverkehrsflussrichtlinie, indem die Zugriffsanforderungs- und Genehmigungsprozesse vereinfacht werden. Diese Blaupause weist eine [Azure Policy](../../../policy/overview.md)-Definition zu, die Ihnen hilft, virtuelle Computer zu überwachen, die Just-In-Time-Zugriff unterstützen können, aber noch nicht konfiguriert wurden.

- \[Vorschau\]: Möglichen Just-In-Time-Netzwerkzugriff in Azure Security Center überwachen

## <a name="sc-28-1-protection-of-information-at-rest--cryptographic-protection"></a>SC-28 (1) Schutz von ruhenden Informationen | Kryptografischer Schutz

Mit dieser Blaupause können Sie die Richtlinie zur Verwendung von kryptografischen Steuerungen zum Schutz von ruhenden Informationen erzwingen, indem 9 [Azure Policy](../../../policy/overview.md)-Definitionen zugewiesen werden, die spezifische kryptografische Steuerungen erzwingen und die Verwendung schwacher kryptografischer Einstellungen überwachen. Wenn Sie wissen, wo Ihre Azure-Ressourcen möglicherweise nicht optimale kryptografische Konfigurationen aufweisen, können Sie Korrekturmaßnahmen ergreifen, um sicherzustellen, dass die Ressourcen entsprechend Ihrer Richtlinie zur Informationssicherheit konfiguriert sind. Für die von dieser Blaupause zugewiesenen Richtlinien gilt insbesondere Folgendes: Sie erfordern eine Verschlüsselung für Data Lake-Speicherkonten, sie erfordern eine transparente Datenverschlüsselung für SQL-Datenbanken, sie überwachen die fehlende Verschlüsselung für SQL-Datenbanken, VM-Datenträger und Variablen von Automation-Konten.

- \[Vorschau\]: Nicht verschlüsselte SQL-Datenbank in Azure Security Center überwachen
- \[Vorschau\]: Nicht verschlüsselte VM-Datenträger in Azure Security Center überwachen
- Sichere Übertragung in Speicherkonten überwachen
- Überwachen von verwalteten SQL-Instanzen ohne Advanced Data Security
- Überwachen von SQL-Servern ohne Advanced Data Security
- Überwachen des Status der transparenten Datenverschlüsselung
- Bereitstellen von Advanced Data Security auf SQL-Servern
- Transparente SQL DB-Datenbankverschlüsselung bereitstellen
- Verschlüsselung für Data Lake Store Konten erzwingen

## <a name="si-2-flaw-remediation"></a>SI-2 Fehlerbehebung

Mit dieser Blaupause können Sie Fehler im Informationssystem verwalten, indem sechs [Azure Policy](../../../policy/overview.md)-Definitionen zugewiesen werden, die fehlende Systemupdates, Sicherheitsrisiken des Betriebssystems, SQL-Sicherheitsrisiken und Sicherheitsrisiken von virtuellen Computern in Azure Security Center überwachen. Azure Security Center umfasst Funktionen zur Berichterstellung, über die Sie in Echtzeit Einblick in den Sicherheitsstatus von bereitgestellten Azure-Ressourcen erhalten. Diese Blaupause weist auch eine Richtliniendefinition zu, die ein automatisches Upgrade des Betriebssystems für VM-Skalierungsgruppen sicherstellt.

- \[Vorschau\]: Fehlende Systemupdates für VM-Skalierungsgruppen in Azure Security Center überwachen
- \[Vorschau\]: Betriebssystem-Sicherheitsrisiken für Ihre VM-Skalierungsgruppen in Azure Security Center überwachen
- \[Vorschau\]: Fehlende Systemupdates in Azure Security Center überwachen
- \[Vorschau\]: Betriebssystem-Sicherheitsrisiken in Azure Security Center überwachen
- \[Vorschau\]: Ergebnisse der SQL-Sicherheitsrisikobewertung in Azure Security Center überwachen
- \[Vorschau\]: VM-Sicherheitsrisiken in Azure Security Center überwachen
- Automatisches Betriebssystemupgrades mit App-Integritätsüberprüfungen in VMSS erzwingen

## <a name="si-3-malicious-code-protection"></a>SI-3 Schutz vor schädlichem Code

Diese Blaupause hilft Ihnen bei der Verwaltung des Endpunktschutzes, einschließlich des Schutzes vor schädlichem Code, indem sie drei [Azure Policy](../../../policy/overview.md)-Definitionen zuweist, die virtuelle Computer in Azure Security Center auf fehlenden Endpunktschutz überprüfen und die Microsoft-Antimalware-Lösung auf virtuellen Windows-Computern erzwingen.

- \[Vorschau\]: Endpoint Protection-Lösung für VM-Skalierungsgruppen in Azure Security Center überwachen
- \[Vorschau\]: Fehlenden Endpoint Protection-Schutz in Azure Security Center überwachen
- Bereitstellung der standardmäßigen Microsoft IaaSAntimalware-Erweiterung für Windows Server

## <a name="si-3-1-malicious-code-protection--central-management"></a>SI-3 (1) Schutz vor schädlichem Code | Zentrale Verwaltung

Diese Blaupause hilft Ihnen bei der Verwaltung des Endpunktschutzes, einschließlich des Schutzes vor schädlichem Code, indem sie zwei [Azure Policy](../../../policy/overview.md)-Definitionen zuweist, die virtuelle Computer in Azure Security Center auf fehlenden Endpunktschutz überprüfen. Azure Security Center umfasst Funktionen zur zentralisierten Verwaltung und Berichterstellung, über die Sie in Echtzeit Einblick in den Sicherheitsstatus von bereitgestellten Azure-Ressourcen erhalten.

- \[Vorschau\]: Endpoint Protection-Lösung für VM-Skalierungsgruppen in Azure Security Center überwachen
- \[Vorschau\]: Fehlenden Endpoint Protection-Schutz in Azure Security Center überwachen

## <a name="si-4-information-system-monitoring"></a>SI-4 Überwachung des Informationssystems

Diese Blaupause hilft Ihnen bei der Überwachung Ihres Systems, indem sie die Protokollierung und Datensicherheit für alle Azure-Ressourcen überprüft und erzwingt. Insbesondere weisen die Richtlinien Überwachung zu und erzwingen die Bereitstellung des Log Analytics-Agent sowie erweiterte Sicherheitseinstellungen für SQL-Datenbanken, Speicherkonten und Netzwerkressourcen. Diese Funktionen helfen Ihnen beim Erkennen von anomalem Verhalten und Indikatoren für Angriffe, sodass Sie geeignete Maßnahmen ergreifen können.

- \[Vorschau\]: Bereitstellung des Log Analytics-Agents überwachen – VM-Image (Betriebssystem) nicht aufgelistet
- \[Vorschau\]: Bereitstellung des Log Analytics-Agents in VM-Skalierungsgruppen überwachen – VM-Image (Betriebssystem) nicht aufgelistet
- \[Vorschau\]: Überwachen des Log Analytics-Arbeitsbereichs für VM – Berichtskonflikt
- \[Vorschau\]: Log Analytics-Agent für Linux-VM-Skalierungsgruppen bereitstellen
- \[Vorschau\]: Bereitstellen des Log Analytics-Agents für Linux-VMs
- \[Vorschau\]: Log Analytics-Agent für Windows-VM-Skalierungsgruppen bereitstellen
- \[Vorschau\]: Bereitstellen des Log Analytics-Agents für Windows-VMs
- Überwachen von verwalteten SQL-Instanzen ohne Advanced Data Security
- Überwachen von SQL-Servern ohne Advanced Data Security
- Bereitstellen von Advanced Data Security auf SQL-Servern
- Bereitstellen von Advanced Threat Protection für Speicherkonten
- Bereitstellen von Auditing auf SQL-Servern
- Bereitstellen von Network Watcher beim Erstellen virtueller Netzwerke
- Bereitstellen von Bedrohungserkennung auf SQL-Servern

## <a name="si-4-18-information-system-monitoring--analyze-traffic--covert-exfiltration"></a>SI-4 (18) Überwachung des Informationssystems | Analyse des Datenverkehrs / verdeckte Ausschleusung

Advanced Threat Protection für Azure Storage erkennt ungewöhnliche und möglicherweise schädliche Versuchen, auf Speicherkonten zuzugreifen oder diese unbefugt zu nutzen. Zu den Schutzwarnungen gehören anomale Zugriffsmuster, anomale Extrahierungen/Uploads und verdächtige Speicheraktivitäten. Diese Indikatoren können Ihnen helfen, die verdeckte Exfiltration von Informationen zu erkennen.

- Bereitstellen von Advanced Threat Protection für Speicherkonten

## <a name="next-steps"></a>Nächste Schritte

Nachdem Sie sich nun die Steuerungszuordnung der Blaupause „NIST SP 800-53 R4“ angesehen haben, lesen Sie die folgenden Artikel, um eine Informationen zur Blaupause zu erhalten und zu erfahren, wie Sie dieses Beispiel bereitstellen:

> [!div class="nextstepaction"]
> [Übersicht über das Blaupausenbeispiel „NIST SP 800-53 R4“](./index.md)
> [Bereitstellungsschritte für das Blaupausenbeispiel „NIST SP 800-53 R4“](./deploy.md)

Weitere Artikel zu Blaupausen und ihrer Nutzung:

- Erfahren Sie mehr über den [Lebenszyklus von Blaupausen](../../concepts/lifecycle.md).
- Machen Sie sich mit der Verwendung [statischer und dynamischer Parameter](../../concepts/parameters.md) vertraut.
- Erfahren Sie, wie Sie die [Abfolge von Blaupausen](../../concepts/sequencing-order.md) anpassen können.
- Erfahren Sie, wie Sie [Ressourcen in Blaupausen sperren](../../concepts/resource-locking.md) können.
- Lernen Sie, wie Sie [vorhandene Zuweisungen aktualisieren](../../how-to/update-existing-assignments.md).
