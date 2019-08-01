---
title: Stichprobe – UK OFFICIAL und UK NHS Blueprints – Zuordnungssteuerung
description: Zuordnungssteuerung der britischen OFFICIAL- und NHS-Blaupausenbeispiele.
services: blueprints
author: DCtheGeek
ms.author: dacoulte
ms.date: 06/26/2019
ms.topic: conceptual
ms.service: blueprints
manager: carmonm
ms.openlocfilehash: 945898105aab7261ee494a86aeff10337599feb3
ms.sourcegitcommit: 920ad23613a9504212aac2bfbd24a7c3de15d549
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 07/15/2019
ms.locfileid: "68226007"
---
# <a name="control-mapping-of-the-uk-official-and-uk-nhs-blueprint-samples"></a>Zuordnungssteuerung der britischen OFFICIAL- und NHS-Blaupausenbeispiele

Der folgende Artikel beschreibt, wie die Muster des UK OFFICIAL und UK NHS Blueprint mit den Kontrollen des UK OFFICIAL und UK NHS übereinstimmen. Weitere Informationen zu den Steuerungen finden Sie unter [UK OFFICIAL](https://www.gov.uk/government/publications/government-security-classifications).

Die folgende Zuordnung bezieht sich auf die Kontrollen von **UK OFFICIAL** und **UK NHS**. Über den rechten Navigationsbereich können Sie direkt zu einer bestimmten Steuerungszuordnung springen. Viele der zugeordneten Steuerungen werden mit einer [Azure Policy](../../../policy/overview.md)-Initiative implementiert. Zum Anzeigen der vollständigen Initiative öffnen Sie **Richtlinie** im Azure-Portal und wählen dann die Seite **Definitionen** aus. Dann finden und wählen Sie **\[Preview\] Audit UK OFFICIAL und UK NHS Kontrollen, und stellen Sie spezifische VM-Erweiterungen bereit, um die integrierte Richtlinieninitiative für Auditanforderungen** zu unterstützen.

## <a name="1-data-in-transit-protection"></a>1 Daten im Transportschutz

Die Blaupause hilft Ihnen, den sicheren Informationsfluss mit Azure-Diensten sicherzustellen, indem sie [Azure-Richtliniendefinitionen](../../../policy/overview.md) zuweist, die unsichere Verbindungen zu Speicherkonten und Redis Cache überprüfen.

- Nur sichere Verbindungen mit Ihrer Redis Cache-Instanz sollten aktiviert werden
- Sichere Übertragung in Speicherkonten sollte aktiviert werden.

## <a name="23-data-at-rest-protection"></a>2.3 Daten im Ruhezustand schützen

Die Blaupause hilft Ihnen, Ihre Richtlinien für die Verwendung von Kryptokontrollen durchzusetzen, indem sie [Azure-Richtliniendefinitionen](../../../policy/overview.md) zuweist, die bestimmte Kryptokontrollen durchsetzen und die Verwendung schwacher kryptographischer Einstellungen überprüfen.
Wenn Sie wissen, wo Ihre Azure-Ressourcen möglicherweise nicht optimale kryptografische Konfigurationen aufweisen, können Sie Korrekturmaßnahmen ergreifen, um sicherzustellen, dass die Ressourcen entsprechend Ihrer Richtlinie zur Informationssicherheit konfiguriert sind. Insbesondere erfordern die durch dieses Konzept zugewiesenen Richtlinien eine Verschlüsselung für Speicherkonten von Data Lake, eine transparente Datenverschlüsselung für SQL-Datenbanken, eine Überprüfung fehlender Verschlüsselung für Speicherkonten, SQL-Datenbanken, Festplatten virtueller Maschinen und Automatisierungskontenvariablen, eine Überprüfung unsicherer Verbindungen zu Speicherkonten und Redis Cache, eine Überprüfung schwacher Passwortverschlüsselung für virtuelle Maschinen und eine Überprüfung unverschlüsselter Kommunikation mit Service Fabric.

- Nicht verschlüsselte SQL-Datenbank in Azure Security Center überwachen
- Die Datenträgerverschlüsselung sollte auf virtuelle Computer angewendet werden.
- Automation-Kontovariablen sollten verschlüsselt werden.
- Sichere Übertragung in Speicherkonten sollte aktiviert werden.
- Service Fabric Cluster sollten die Eigenschaft ClusterProtectionLevel auf EncryptAndSign setzen
- Transparent Data Encryption für SQL-Datenbanken sollte aktiviert werden.
- SQL DB transparente Datenverschlüsselung bereitstellen
- Verschlüsselung für Data Lake Store Konten erforderlich
- Zulässige Standorte (wurde fest mit "UK SOUTH" und "UK WEST" kodiert)
- Zulässige Standorte für Ressourcengruppen (wurde fest mit "UK SOUTH" und "UK WEST" kodiert)

## <a name="52-vulnerability-management"></a>5.2 Schwachstellenmanagement

Diese Blaupause hilft Ihnen bei der Verwaltung von Schwachstellen im Informationssystem, indem sie [Azure-Richtliniendefinitionen](../../../policy/overview.md) zuweist, die den fehlenden Endpunktschutz, fehlende System-Updates, Betriebssystem-Schwachstellen, SQL-Schwachstellen und Schwachstellen in virtuellen Maschinen überwachen. Diese Erkenntnisse bieten Echtzeitinformationen zum Sicherheitsstatus Ihrer bereitgestellten Ressourcen und ermöglichen die Priorisierung von Aktionen zur Problembehebung.

- Fehlenden Endpoint Protection-Schutz in Azure Security Center überwachen
- Systemupdates sollten auf Ihren Computern installiert sein.
- Sicherheitsrisiken in der Sicherheitskonfiguration für Ihre Computer sollten beseitigt werden.
- Sicherheitsrisiken in SQL-Datenbanken sollten beseitigt werden.
- Sicherheitsrisiken sollten durch eine Lösung zur Sicherheitsrisikobewertung beseitigt werden.

## <a name="53-protective-monitoring"></a>5.3 Vorbeugende Überwachung

Diese Blaupause hilft Ihnen, Informationssysteme zu schützen, indem sie [Azure-Richtliniendefinitionen](../../../policy/overview.md) zuweist, die eine vorbeugende Überwachung von uneingeschränktem Zugriff, Whitelist-Aktivitäten und Bedrohungen ermöglichen.

- Nicht eingeschränkten Netzwerkzugriff auf Speicherkonten überwachen
- Die adaptive Anwendungssteuerung sollte auf virtuellen Computern aktiviert werden.
- Bereitstellen von Threat Protection auf SQL-Servern
- Bereitstellung der standardmäßigen Microsoft IaaS Anti-Malware-Erweiterung für Windows Server

## <a name="9-secure-user-management--10-identity-and-authentication"></a>9 Sichere Benutzerverwaltung / 10 Identität und Authentifizierung

Azure implementiert eine rollenbasierte Zugriffskontrolle (RBAC), die Ihnen hilft, zu verwalten, wer Zugriff auf Ressourcen in Azure hat. Über das Azure-Portal können Sie überprüfen, wer Zugriff auf Azure-Ressourcen und die zugehörigen Berechtigungen hat. Diese Blaupause hilft Ihnen, Zugriffsrechte einzuschränken und zu kontrollieren, indem sie [Azure-Richtliniendefinitionen](../../../policy/overview.md) zuweist, um externe Konten mit Eigentümer- und/oder Lese-/Schreibberechtigungen und Konten mit Eigentümer-, Lese- und/oder Schreibberechtigungen zu auditieren, für die keine Multifaktor-Authentifizierung aktiviert ist.

- MFA sollte für Konten mit Besitzerberechtigungen in Ihrem Abonnement aktiviert sein.
- MFA sollte für Konten mit Schreibrechten für Ihr Abonnement aktiviert werden
- MFA sollte für Ihre Abonnementkonten mit Leseberechtigungen aktiviert sein
- Externe Konten mit Besitzerberechtigungen sollten aus Ihrem Abonnement entfernt werden.
- Externe Konten mit Schreibberechtigungen sollten aus Ihrem Abonnement entfernt werden.
- Externe Konten mit Leseberechtigungen sollten aus Ihrem Abonnement entfernt werden

Diese Blaupause weist Azure-Richtliniendefinitionen zu, um die Verwendung der Azure Active Directory-Authentifizierung für SQL-Server und Service Fabric zu überprüfen. Die Verwendung der Azure Active Directory-Authentifizierung ermöglicht eine vereinfachte Verwaltung von Berechtigungen und eine zentralisierte Identitätsverwaltung von Datenbankbenutzern und anderen Microsoft-Diensten.

- Ein Azure Active Directory-Administrator sollte für SQL-Server bereitgestellt werden
- Service Fabric-Cluster sollten nur Azure Active Directory für die Clientauthentifizierung verwenden.

Diese Blaupause weist auch Azure-Richtliniendefinitionen zu Auditkonten zu, die zur Überprüfung priorisiert werden sollten, einschließlich abgeschriebener Konten und externer Konten. Bei Bedarf kann die Anmeldung für Konten blockiert werden (oder Konten können entfernt werden), wodurch die Zugriffsrechte für Azure-Ressourcen sofort entfernt werden. Diese Blaupause weist zwei Azure-Richtliniendefinitionen zur Überprüfung des abgeschriebenen Kontos zu, die bei der Entfernung berücksichtigt werden sollten.

- Veraltete Konten sollten aus Ihrem Abonnement entfernt werden.
- Veraltete Konten mit Besitzerberechtigungen sollten aus Ihrem Abonnement entfernt werden.
- Externe Konten mit Besitzerberechtigungen sollten aus Ihrem Abonnement entfernt werden.
- Externe Konten mit Schreibberechtigungen sollten aus Ihrem Abonnement entfernt werden.

Diese Blaupause weist auch eine Azure-Richtliniendefinition zu, die die Dateiberechtigungen für Passwörter von Linux-VM-Passwörtern überprüft, um zu benachrichtigen, wenn sie falsch eingestellt sind. Dieses Design ermöglicht es Ihnen, Korrekturmaßnahmen zu ergreifen, um sicherzustellen, dass die Authentifikatoren nicht beeinträchtigt werden.

- \[Vorschau\]: Festlegung der /etc/passwd-Dateiberechtigungen auf 0644 auf Linux-VMs überwachen

Diese Blaupause hilft Ihnen bei der Durchsetzung sicherer Passwörter, indem sie Azure-Richtliniendefinitionen zuweist, die Windows-VMs auditieren, die keine Mindeststärke und andere Passwortanforderungen erfüllen. Aufgrund der Informationen zu virtuellen Computern, die gegen die Richtlinie zur Kennwortsicherheit verstoßen, können Sie Korrekturmaßnahmen ergreifen, um sicherzustellen, dass die Kennwörter für alle Benutzerkonten auf virtuellen Computern mit der Richtlinie konform sind.

- \[Vorschau\]: Anforderungen zum Überwachen von Windows-VMs bereitstellen, auf denen nicht die Einstellung für die Kennwortkomplexität aktiviert ist
- \[Vorschau\]: Anforderungen zum Überwachen von Windows-VMs bereitstellen, die kein maximales Kennwortalter von 70 Tagen verwenden
- \[Vorschau\]: Anforderungen zum Überwachen von Windows-VMs bereitstellen, die kein Mindestkennwortalter von 1 Tag verwenden
- \[Vorschau\]: Anforderungen zum Überwachen von Windows-VMs bereitstellen, bei denen keine Mindestkennwortlänge von 14 Zeichen festgelegt ist
- \[Vorschau\]: Anforderungen zum Überwachen von Windows-VMs bereitstellen, die eine Wiederverwendung der vorherigen 24 Kennwörter zulassen
- \[Vorschau\]: Windows-VMs überwachen, auf denen nicht die Einstellung für die Kennwortkomplexität aktiviert ist
- \[Vorschau\]: Windows-VMs überwachen, die kein maximales Kennwortalter von 70 Tagen verwenden
- \[Vorschau\]: Windows-VMs überwachen, die kein Mindestkennwortalter von 1 Tag verwenden
- \[Vorschau\]: Windows-VMs überwachen, bei denen keine Mindestkennwortlänge von 14 Zeichen festgelegt ist
- \[Vorschau\]: Windows-VMs überwachen, die eine Wiederverwendung der vorherigen 24 Kennwörter zulassen

Diese Blaupause hilft Ihnen auch, den Zugriff auf Azure-Ressourcen zu steuern, indem sie Azure-Richtliniendefinitionen zuweist. Mit diesen Richtlinien wird die Verwendung von Ressourcentypen und Konfigurationen überwacht, die einen weniger restriktiven Zugriff auf Ressourcen ermöglichen. Durch Kenntnis der Ressourcen, die gegen diese Richtlinien verstoßen, können Sie Korrekturmaßnahmen ergreifen, um sicherzustellen, dass der Zugriff auf Azure-Ressourcen auf autorisierte Benutzer beschränkt ist.

- \[Vorschau\]: Anforderungen zum Überwachen von Linux-VMs bereitstellen, die Konten ohne Kennwörter verwenden
- \[Vorschau\]: Anforderungen zum Überwachen von Linux-VMs bereitstellen, die Remoteverbindungen über Konten ohne Kennwörter zulassen
- \[Vorschau\]: Linux-VMs überwachen, die Konten ohne Kennwörter verwenden
- \[Vorschau\]: Linux-VMs überwachen, die Remoteverbindungen über Konten ohne Kennwörter zulassen
- Speicherkonten sollten zu neuen Azure Resource Manager-Ressourcen migriert werden.
- Virtuelle Maschinen sollten auf neue Azure Resource Manager-Ressourcen migriert werden
- Virtuelle Computer überwachen, die keine verwalteten Datenträger verwenden

## <a name="11-external-interface-protection"></a>11 Schutz der externen Schnittstelle

Abgesehen von der Verwendung von mehr als 25 Richtlinien für eine angemessene sichere Benutzerverwaltung hilft Ihnen dieses Konzept, Service-Schnittstellen vor unbefugtem Zugriff zu schützen, indem es eine[ Azure-Richtliniendefinition](../../../policy/overview.md) zuweist, die uneingeschränkte Speicherkonten überwacht. Speicherkonten mit uneingeschränktem Zugriff können einen unbeabsichtigten Zugriff auf die im Informationssystem enthaltenen Informationen ermöglichen. Diese Blaupause weist auch eine Richtlinie zu, die adaptive Anwendungssteuerungen auf virtuellen Maschinen ermöglicht.

- Nicht eingeschränkten Netzwerkzugriff auf Speicherkonten überwachen
- Die adaptive Anwendungssteuerung sollte auf virtuellen Computern aktiviert werden.

## <a name="12-secure-service-administration"></a>12 Sichere Dienstverwaltung

Azure implementiert eine rollenbasierte Zugriffskontrolle (RBAC), die Ihnen hilft, zu verwalten, wer Zugriff auf Ressourcen in Azure hat. Über das Azure-Portal können Sie überprüfen, wer Zugriff auf Azure-Ressourcen und die zugehörigen Berechtigungen hat. Diese Blaupause hilft Ihnen, privilegierte Zugriffsrechte einzuschränken und zu kontrollieren, indem sie fünf [Azure-Richtliniendefinitionen](../../../policy/overview.md) zuweist, um externe Konten mit Eigentümer- und/oder Schreibberechtigungen und Konten mit Eigentümer- und/oder Schreibberechtigungen zu auditieren, für die keine mehrstufige Authentifizierung aktiviert ist.

Systeme, die für die Verwaltung eines Clouddiensts verwendet werden, verfügen über einen mit hohen Berechtigungen ausgestatteten Zugriff auf diesen Dienst. Ihre Gefährdung würde erhebliche Auswirkungen haben; dies schließt auch Mittel zum Umgehen der Sicherheitskontrollen und zum Stehlen oder Verändern großer Datenmengen ein. Die Methoden, die von den Administratoren des Dienstanbieters zur Verwaltung des Betriebsdienstes verwendet werden, sollten so konzipiert sein, dass sie jedes Risiko der Ausnutzung mindern, das die Sicherheit des Dienstes beeinträchtigen könnte. Wenn dieses Prinzip nicht implementiert ist, kann ein Angreifer über die Mittel verfügen, um Sicherheitskontrollen zu umgehen und große Datenmengen zu stehlen oder zu manipulieren.

- MFA sollte für Konten mit Besitzerberechtigungen in Ihrem Abonnement aktiviert sein.
- MFA sollte für Konten mit Schreibrechten für Ihr Abonnement aktiviert werden
- Externe Konten mit Besitzerberechtigungen sollten aus Ihrem Abonnement entfernt werden.
- Externe Konten mit Schreibberechtigungen sollten aus Ihrem Abonnement entfernt werden.

Diese Blaupause weist Azure-Richtliniendefinitionen zu, um die Verwendung der Azure Active Directory-Authentifizierung für SQL-Server und Service Fabric zu überprüfen. Die Verwendung der Azure Active Directory-Authentifizierung ermöglicht eine vereinfachte Verwaltung von Berechtigungen und eine zentralisierte Identitätsverwaltung von Datenbankbenutzern und anderen Microsoft-Diensten.

- Ein Azure Active Directory-Administrator sollte für SQL-Server bereitgestellt werden
- Service Fabric-Cluster sollten nur Azure Active Directory für die Clientauthentifizierung verwenden.

Mit dieser Blaupause werden vier Azure Richtliniendefinitionen zugewiesen, um Konten zu überwachen, die für die Überprüfung priorisiert sind, einschließlich veralteter Konten und externer Konten mit erhöhten Rechten. Bei Bedarf kann die Anmeldung für Konten blockiert werden (oder Konten können entfernt werden), wodurch die Zugriffsrechte für Azure-Ressourcen sofort entfernt werden. Diese Blaupause weist zwei Azure-Richtliniendefinitionen zur Überprüfung des abgeschriebenen Kontos zu, die bei der Entfernung berücksichtigt werden sollten.

- Veraltete Konten sollten aus Ihrem Abonnement entfernt werden.
- Veraltete Konten mit Besitzerberechtigungen sollten aus Ihrem Abonnement entfernt werden.
- Externe Konten mit Besitzerberechtigungen sollten aus Ihrem Abonnement entfernt werden.
- Externe Konten mit Schreibberechtigungen sollten aus Ihrem Abonnement entfernt werden.

Diese Blaupause weist auch eine Azure-Richtliniendefinition zu, die die Dateiberechtigungen für Passwörter von Linux-VM-Passwörtern überprüft, um zu benachrichtigen, wenn sie falsch eingestellt sind. Dieses Design ermöglicht es Ihnen, Korrekturmaßnahmen zu ergreifen, um sicherzustellen, dass die Authentifikatoren nicht beeinträchtigt werden.

- \[Vorschau\]: Festlegung der /etc/passwd-Dateiberechtigungen auf 0644 auf Linux-VMs überwachen

## <a name="13-audit-information-for-users"></a>13 Überwachungsinformationen für Benutzer

Diese Blaupause hilft Ihnen, sicherzustellen, dass Systemereignisse protokolliert werden, indem Sie [Azure-Richtliniendefinitionen](../../../policy/overview.md) zuweisen, die die Einstellungen des Audit-Protokolls auf Azure-Ressourcen überprüfen. Mit einer zugewiesenen Richtlinie wird zudem überwacht, dass virtuelle Computer keine Protokolle an einen angegebenen Log Analytics-Arbeitsbereich senden.

- Nicht überwachte SQL-Datenbank in Azure Security Center überwachen
- Überwachen der Diagnoseeinstellung
- Überwachungseinstellungen auf SQL Server-Ebene überwachen
- \[Vorschau\]: Bereitstellen des Log Analytics-Agents für Linux-VMs
- \[Vorschau\]: Bereitstellen des Log Analytics-Agents für Windows-VMs
- Bereitstellen von Network Watcher beim Erstellen virtueller Netzwerke

## <a name="next-steps"></a>Nächste Schritte

Nachdem Sie nun die Kontrollzuordnung der britischen OFFICIAL- und NHS-Blaupausen kennen, lesen Sie die folgenden Artikel, um mehr über die Übersicht und die Bereitstellung dieses Beispiels zu erfahren:

> [!div class="nextstepaction"]
> [UK OFFICIAL und UK NHS Blaupausen – Übersicht](./index.md)
> [ UK OFFICIAL und UK NHS Blaupausen – Bereitstellungsschritte](./deploy.md)

Weitere Artikel zu Blaupausen und ihrer Nutzung:

- Erfahren Sie mehr über den [Lebenszyklus von Blaupausen](../../concepts/lifecycle.md).
- Machen Sie sich mit der Verwendung [statischer und dynamischer Parameter](../../concepts/parameters.md) vertraut.
- Erfahren Sie, wie Sie die [Abfolge von Blaupausen](../../concepts/sequencing-order.md) anpassen können.
- Erfahren Sie, wie Sie [Ressourcen in Blaupausen sperren](../../concepts/resource-locking.md) können.
- Lernen Sie, wie Sie [vorhandene Zuweisungen aktualisieren](../../how-to/update-existing-assignments.md).
