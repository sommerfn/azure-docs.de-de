---
title: Sichere Web-App für PCI-DSS | Microsoft-Dokumentation
description: Diese Beispiel-App implementiert bewährte Sicherheitsmethoden, die Ihre Anwendung sowie den Sicherheitsstatus Ihres Unternehmens bei der Entwicklung in Azure verbessern.
keywords: na
services: security
documentationcenter: na
author: fehase
manager: rkarlin
editor: ''
ms.assetid: cd906856-f4f9-4ddc-9249-c998386f4085
ms.service: security
ms.devlang: na
ms.topic: conceptual
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 08/22/2019
ms.author: terrylan
ms.openlocfilehash: 4fe612db65d985be2f1f1c81d03c3ee735c03889
ms.sourcegitcommit: 007ee4ac1c64810632754d9db2277663a138f9c4
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 08/23/2019
ms.locfileid: "69992390"
---
# <a name="develop-a-secure-infrastructure-for-a-pci-app"></a>Entwickeln einer sicheren Infrastruktur für eine PCI-App

## <a name="overview"></a>Übersicht

Diese sichere Infrastruktur für eine PCI-App (Payment Card Industry) bietet einen Leitfaden für die Bereitstellung einer PaaS-Umgebung (Platform-as-a-Service), die für die Erfassung, Speicherung und Abfrage von Karteninhaberdaten geeignet ist. Diese Lösung automatisiert die Bereitstellung und Konfiguration von Azure-Ressourcen für eine allgemeine Referenzarchitektur. Sie zeigt Wege auf, wie Kunden spezifische Sicherheits- und Konformitätsanforderungen erfüllen können und dient als Grundlage für Kunden, um ihre eigenen Lösungen in Azure zu entwickeln und zu konfigurieren. Die Lösung implementiert einen Teil von Anforderungen, die mit PCI-DSS 3.2 (Payment Card Industry Data Security Standards) vergleichbar sind.

Dieses Beispiel stellt automatisch eine Referenzarchitektur für PaaS-Webanwendungen mit vorkonfigurierten Sicherheitskontrollen bereit, um Kunden bei der Einhaltung von mit PCI-DSS 3.2 vergleichbaren Anforderungen zu unterstützen. Die Lösung besteht aus Azure Resource Manager-Vorlagen und PowerShell-Skripts, die die Ressourcenbereitstellung und -konfiguration steuern.

Sie sollten die in diesem Artikel beschriebenen Schritte befolgen, um sicherzustellen, dass die Anwendungskomponenten ordnungsgemäß konfiguriert sind. Die Datenbank, die Azure App Service-Instanz, die Azure Key Vault-Instanz und die Azure Application Gateway-Instanz sind voneinander abhängig.

Die Bereitstellungsskripts richten die Infrastruktur ein. Nachdem Sie die Bereitstellungsskripts ausgeführt haben, müssen Sie einige manuelle Konfigurationen im Azure-Portal durchführen, um die Komponenten und Dienste zu verknüpfen.

Dieses Beispiel richtet sich an erfahrene Azure-Entwickler, die in der Einzelhandelsbranche tätig sind und eine Einzelhandels-App mit sicherer Azure-Infrastruktur erstellen möchten.

> [!NOTE]
> Diese Architektur soll Kunden als Grundlage für die Anpassung an ihre spezifischen Anforderungen dienen. Sie sollte nicht unverändert in einer Produktionsumgebung verwendet werden.

Sollen die PCI-DSS 3.2-Anforderungen vollständig erfüllt werden, genügt es nicht, eine Anwendung ohne Änderungen in dieser Umgebung bereitzustellen. Beachten Sie Folgendes:

- Diese Architektur bietet eine Grundlage, um Kunden bei der Verwendung von Azure in PCI-DSS 3.2-konformer Weise zu unterstützen.
- Der Kunde ist für die Durchführung einer angemessenen Sicherheits- und Konformitätsbewertung jeder Lösung verantwortlich, die mit dieser Architektur erstellt wurde, da die Anforderungen je nach den Besonderheiten der jeweiligen Implementierung des Kunden unterschiedlich sein können.

Bei der Entwicklung und Bereitstellung dieser App lernen Sie Folgendes:

- Erstellen einer Azure Key Vault-Instanz sowie Speichern und Abrufen von Geheimnissen
- Bereitstellen von Azure SQL-Datenbank, Einrichten sicherer Daten und Autorisieren des Zugriffs
- Bereitstellen der Azure-Web-App mit App Service-Umgebung (dedizierte, isolierte Umgebung mit Front-End-Firewallzugriff)
- Erstellen und Konfigurieren einer Azure Application Gateway-Instanz mit einer Firewall, die das [Top 10 OWASP-Regelwerk](https://coreruleset.org/) anwendet
- Aktivieren der Verschlüsselung von Daten während der Übertragung und im Ruhezustand mithilfe von Azure-Diensten
- Einrichten der Azure-Richtlinie und der Blaupausen zur Evaluierung der Compliance

Sobald Sie diese App entwickelt und bereitgestellt haben, haben Sie die folgende Beispiel-Web-App sowie die beschriebene Konfiguration und Sicherheitsmaßnahmen eingerichtet.

## <a name="architecture-diagram-and-components"></a>Architekturdiagramm und Komponenten
Bei der App handelt es sich um eine n-schichtige Anwendung mit drei Schichten. Die Front-End-, Back-End- und Datenbankebenen mit integrierten Überwachungs- und Geheimnisverwaltungskomponenten werden in der folgenden Abbildung veranschaulicht:

![App-Architektur](./media/secure-pci-web-app/architecture.png)

Die Architektur umfasst die folgenden Komponenten:

- [App Service-Umgebung v2:](https://docs.microsoft.com/azure/app-service/environment/intro/) Stellt die App Service-Umgebung und den Lastenausgleich für die Anwendungsarchitektur bereit.
- [Azure Application Gateway:](https://docs.microsoft.com/azure/application-gateway/) Stellt das Gateway und die Firewall für die Anwendungsarchitektur zur Verfügung.
- [Application Insights:](https://docs.microsoft.com/azure/azure-monitor/app/app-insights-overview) Stellt einen erweiterbaren APM-Dienst (Application Performance Management) auf mehreren Plattformen bereit.
- [Azure Key Vault](https://docs.microsoft.com/azure/key-vault/). Speichert und verschlüsselt die Geheimnisse der App und verwaltet die Erstellung von Zugriffsrichtlinien.
- [Azure Active Directory:](https://docs.microsoft.com/azure/active-directory/fundamentals/active-directory-whatis/) Stellt einen cloudbasierten Identitäts- und Zugriffsverwaltungsdienst sowie Anmeldefunktionen und Zugriffsressourcen bereit.
- [Azure DNS:](https://docs.microsoft.com/azure/dns/dns-overview) Stellt den Dienst zum Hosten der Domäne bereit.
- [Azure Load Balancer:](https://docs.microsoft.com/azure/load-balancer/load-balancer-overview/) Skaliert Ihre Anwendungen und bietet Hochverfügbarkeit für Ihre Dienste.
- [Azure Storage](https://docs.microsoft.com/azure/storage/common/storage-introduction/). Stellt eine Lösung für moderne Datenspeicherszenarien bereit.
- [Azure-Web-App](https://docs.microsoft.com/azure/app-service/overview/).  Stellt einen HTTP-basierten Dienst zum Hosten von Webanwendungen bereit.
- [Azure SQL-Datenbank:](https://docs.microsoft.com/azure/sql-database/sql-database-technical-overview/) Speichert die Daten der App sicher.
- [Azure Blueprints](https://docs.microsoft.com/azure/governance/blueprints/overview/). Stellt Spezifikationen bereit und sorgt für die Einhaltung bestimmter Standards und Anforderungen.
- [Azure Security Center](https://docs.microsoft.com/azure/security-center/)
- [Azure Policy](https://docs.microsoft.com/azure/governance/policy/overview). Überprüft Ihre Ressourcen auf Verstöße gegen zugewiesene Richtlinien.

## <a name="threat-model"></a>Bedrohungsmodell
Die Bedrohungsmodellierung ist der Prozess, bei dem potenzielle Sicherheitsbedrohungen für Ihr Unternehmen und Ihre Anwendung identifiziert werden und dann sichergestellt wird, dass geeignete vorbeugende Maßnahmen wirksam sind.

In diesem Beispiel wird das [Microsoft Threat Modeling Tool](https://docs.microsoft.com/azure/security/azure-security-threat-modeling-tool) zum Implementieren der Bedrohungsmodellierung für die sichere Beispiel-App verwendet. Mithilfe von Diagrammen der Komponenten und Datenflüsse können Sie Probleme und Bedrohungen frühzeitig im Entwicklungsprozess identifizieren. Das spart Ihnen später Zeit und Geld.

Das Bedrohungsmodell für die App sieht wie folgt aus:

![Bedrohungsmodell](./media/secure-pci-web-app/threat-model.png)

Im folgenden Screenshot werden einige Beispielbedrohungen und potenzielle Sicherheitsrisiken veranschaulicht, die das Threat Modeling Tool generiert. Das Bedrohungsmodell verschafft Ihnen einen Überblick über die freigestellte Angriffsfläche und fordert die Entwickler auf, sich Gedanken über die Problembehebung zu machen.

![Ausgabe des Bedrohungsmodells](./media/secure-web-app/threat-model-output.png)

Beispielsweise wird die SQL-Injektion in der vorangehenden Bedrohungsmodellausgabe mithilfe von Bereinigung der Benutzereingaben und mit gespeicherten Funktionen in Azure Database for PostgreSQL verhindert. Diese Risikominderung verhindert die willkürliche Ausführung von Abfragen, während Daten gelesen und geschrieben werden.

Entwickler verbessern die allgemeine Sicherheit des Systems, indem sie die Bedrohungen in der Ausgabe des Bedrohungsmodells verhindern.

## <a name="deployment"></a>Bereitstellung
### <a name="prerequisites"></a>Voraussetzungen
Sie müssen die folgenden Tools installieren, um die Anwendung ausführen zu können:

- Einen Code-Editor zum Ändern und Anzeigen des Anwendungscodes. [Visual Studio Code](https://code.visualstudio.com/) ist eine Open-Source-Option.
- Die [Azure CLI](https://docs.microsoft.com/cli/azure/install-azure-cli?view=azure-cli-latest&viewFallbackFrom=azure-cli-latest) auf Ihrem Entwicklungscomputer.
- [Git](https://git-scm.com/) muss auf Ihrem System installiert sein. Git wird verwendet, um den Quellcode lokal zu klonen.
- Sie benötigen auch [jq](https://stedolan.github.io/jq/). Dies ist ein benutzerfreundliches UNIX-Tool für das Abfragen von JSON.

Dieses Beispiel besteht aus JSON-Konfigurationsdateien und PowerShell-Skripts, die vom API-Dienst von Azure Resource Manager verwaltet werden, um Ressourcen innerhalb von Azure bereitzustellen. Ausführlichere Bereitstellungsanweisungen finden Sie [hier](https://github.com/Azure-Samples/pci-paas-webapp-ase-sqldb-appgateway-keyvault-oms-AzureRM).

#### <a name="quickstart"></a>Schnellstart

1.  Klonen oder laden Sie [dieses](https://github.com/Azure-Samples/pci-paas-webapp-ase-sqldb-appgateway-keyvault-oms-AzureRM.git) GitHub-Repository auf die lokale Arbeitsstation herunter.
2.  Überprüfen Sie „0-Setup-AdministrativeAccountAndPermission.md“, und führen Sie die angegebenen Befehle aus.
3.  Stellen Sie eine Testlösung mit Contoso-Beispieldaten oder eine Pilotlösung einer anfänglichen Produktionsumgebung bereit.

    Dieses Skript stellt die Azure-Ressourcen für eine Demonstration eines Web Store mithilfe von Contoso-Beispieldaten bereit.

In diesem Beispiel führen Sie das Bereitstellungsskript aus, das die Web-App in App Service bereitstellt, und erstellen die Ressourcen.

Es gibt viele Methoden für die Bereitstellung von Apps in Azure, einschließlich der folgenden:

- Azure-Ressourcen-Manager-Vorlagen
- PowerShell
- Azure-Befehlszeilenschnittstelle
- Azure-Portal
- Azure DevOps

## <a name="guidance-and-recommendations"></a>Anleitungen und Empfehlungen

### <a name="network"></a>Netzwerk
Die Architektur definiert ein privates virtuelles Netzwerk mit dem Adressraum 10.200.0.0/16.
 ![Virtual_Network](./media/secure-pci-web-app/virtual-network.png)

### <a name="network-security-groups"></a>Netzwerksicherheitsgruppen
Netzwerksicherheitsgruppen (https://docs.microsoft.com/azure/virtual-network/virtual-networks-nsg) enthalten Zugriffssteuerungslisten (Access Control Lists, ACLs), die den Datenverkehr in einem virtuellen Netzwerk zulassen oder ablehnen. Netzwerksicherheitsgruppen können verwendet werden, um Datenverkehr auf der Ebene eines Subnetzes oder eines einzelnen Computers zu schützen. Es gibt die folgenden Netzwerksicherheitsgruppen:

- 1 Netzwerksicherheitsgruppe für Application Gateway
- 1 Netzwerksicherheitsgruppe für die App Service-Umgebung
- 1 Netzwerksicherheitsgruppe für die Azure SQL-Datenbank
- 1 Netzwerksicherheitsgruppe für den Bastionhost

Jede der Netzwerksicherheitsgruppen verfügt über bestimmte offene Ports und Protokolle, damit die Lösung sicher und ordnungsgemäß ausgeführt werden kann. Darüber hinaus werden die folgenden Konfigurationen für jede Netzwerksicherheitsgruppe aktiviert:

- Diagnoseprotokolle und -ereignisse (https://docs.microsoft.com/azure/virtual-network/virtual-network-nsg-manage-log) sind aktiviert und werden in einem Speicherkonto gespeichert.
- Azure Monitor-Protokolle ist mit der Diagnose der Netzwerksicherheitsgruppe (https://github.com/krnese/AzureDeploy/blob/master/AzureMgmt/AzureMonitor/nsgWithDiagnostics.json) verbunden.

### <a name="nsg-config"></a>NSG-Konfiguration
Die NSG-Konfiguration für die App Service-Umgebung muss wie in der folgenden Abbildung konfiguriert werden:

 ![NSG_Config](./media/secure-pci-web-app/nsg-config.png)

Jedes Subnetz ist seiner entsprechenden Netzwerksicherheitsgruppe zugeordnet.

### <a name="config"></a>Konfiguration
Subnetze werden wie in der folgenden Abbildung konfiguriert:
 ![Konfiguration](./media/secure-pci-web-app/config.png)

### <a name="azure-dns"></a>Azure DNS
Das DNS (Domain Name System) ist für die Übersetzung (oder Auflösung) eines Website- oder Dienstnamens in die IP-Adresse verantwortlich. [Azure DNS](https://docs.microsoft.com/azure/dns/dns-overview) ist ein Hostingdienst für DNS-Domänen, der die Namensauflösung mithilfe der Azure-Infrastruktur durchführt. Durch das Hosten von Domänen in Azure können Benutzer DNS-Einträge mithilfe der gleichen Anmeldeinformationen, APIs, Tools und Abrechnungsschritte wie für die anderen Azure-Dienste verwalten. Azure DNS unterstützt auch private DNS-Domänen.

### <a name="protect-data"></a>Schützen von Daten
Zum Schutz von Daten in der Cloud müssen Sie die möglichen Zustände berücksichtigen, in denen Ihre Daten auftreten können. Außerdem sollten Sie die Steuerungsmöglichkeiten beachten, die für diesen Zustand verfügbar sind. Bewährte Methoden für Datensicherheit und Verschlüsselung in Azure beziehen sich auf die folgenden Zustände von Daten:

- Ruhende Daten: Dies umfasst alle Informationsspeicherobjekte, Container und Typen, die statisch auf physischen Medien existieren – ob auf Magnetspeichern oder optischen Datenträgern.
- Daten während der Übertragung: Wenn Daten zwischen Komponenten, Speicherorten oder Programmen übertragen werden, weisen sie den Zustand „während der Übertragung“ auf. Beispiele sind die Übertragung über das Netzwerk, über einen Service Bus (von dem lokalen Computer in die Cloud und umgekehrt, inklusive Hybridverbindungen wie ExpressRoute) oder während eines Eingabe-/Ausgabevorgangs.

### <a name="azure-storage"></a>Azure Storage
Um die Anforderungen für verschlüsselte ruhende Daten zu erfüllen, wird in [Azure Storage](https://azure.microsoft.com/services/storage/) überall Azure Key Vault verwendet, um die Kontrolle über Schlüssel zu behalten, die für Datenzugriffe und für die Datenverschlüsselung verwendet werden. Dadurch werden die Daten der Karteninhaber geschützt, um die Zusagen und Anforderungen der Organisation im Hinblick auf Sicherheit und Konformität gemäß der PCI-DSS 3.2 einzuhalten.

### <a name="azure-disk-encryption"></a>Azure Disk Encryption
Azure Disk Encryption nutzt das BitLocker-Feature von Windows, um eine Volumeverschlüsselung für Datenträger bereitzustellen. Die Lösung ist in Azure Key Vault integriert, um die Steuerung und Verwaltung der Datenträgerverschlüsselungsschlüssel zu erleichtern.

### <a name="azure-sql-database"></a>Azure SQL-Datenbank
In der Azure SQL-Datenbankinstanz werden die folgenden Datenbanksicherheitsmaßnahmen verwendet:

- Die [Active Directory-Authentifizierung und -Autorisierung](https://docs.microsoft.com/azure/sql-database/sql-database-aad-authentication) ermöglicht die zentrale Verwaltung der Identitäten von Datenbankbenutzern und anderen Microsoft-Diensten.
- Die [SQL-Datenbank-Überprüfung](https://docs.microsoft.com/azure/sql-database/sql-database-auditing-get-started/) verfolgt Datenbankereignisse nach und schreibt diese in ein Überwachungsprotokoll in einem Azure Storage-Konto.
- Azure SQL-Datenbank ist so konfiguriert, dass [transparente Datenverschlüsselung](https://docs.microsoft.com/sql/relational-databases/security/encryption/transparent-data-encryption-azure-sql) (Transparent Data Encryption) für die Echtzeitverschlüsselung und -entschlüsselung der Datenbank, der zugehörigen Sicherungen und der Transaktionsprotokolldateien verwendet wird, um ruhende Informationen zu schützen. Die transparente Datenverschlüsselung gewährleistet, dass gespeicherte Daten keinen unbefugten Zugriffen ausgesetzt sind.
- [Firewallregeln](https://docs.microsoft.com/azure/sql-database/sql-database-firewall-configure) verhindern sämtlichen Zugriff auf Datenbankserver, bis die richtigen Berechtigungen erteilt werden. Die Firewall gewährt den Datenbankzugriff auf der Grundlage der Ursprungs-IP-Adresse der jeweiligen Anforderung.
- Die [SQL-Bedrohungserkennung](https://docs.microsoft.com/azure/sql-database/sql-database-threat-detection-get-started) ermöglicht die Erkennung und Reaktion auf potenzielle Bedrohungen, sobald sie auftreten. Dazu werden Sicherheitswarnungen bei verdächtigen Datenbankaktivitäten, potenziellen Sicherheitslücken, Angriffen durch Einschleusung von SQL-Befehlen und anormalen Mustern beim Datenbankzugriff ausgegeben.
- [Encrypted-Spalten](https://docs.microsoft.com/azure/sql-database/sql-database-always-encrypted-azure-key-vault) stellen sicher, dass vertrauliche Daten im Datenbanksystem niemals im Klartextformat angezeigt werden. Nach dem Aktivieren der Datenverschlüsselung können nur Clientanwendungen oder Anwendungsserver, die Zugriff auf die Schlüssel haben, auf Klartextdaten zugreifen.
- Die [dynamische Datenmaskierung in SQL-Datenbank](https://docs.microsoft.com/azure/sql-database/sql-database-dynamic-data-masking-get-started) schränkt die Offenlegung vertraulicher Daten ein, indem diese Daten für nicht berechtigte Benutzer maskiert werden. Die dynamische Datenmaskierung kann automatisch potenziell vertrauliche Daten ermitteln und eine geeignete Maskierung vorschlagen. Dies hilft, den Zugriff auf die Daten zu identifizieren und so zu reduzieren, dass sie die Datenbank nicht durch unberechtigten Zugriff verlassen. Kunden sind dafür verantwortlich, die Einstellungen für die dynamische Datenmaskierung an ihr Datenbankschema anzupassen.

### <a name="identity-management"></a>Identitätsverwaltung
Die folgenden Technologien enthalten Funktionen zum Verwalten des Zugriffs auf Karteninhaberdaten in der Azure-Umgebung:

- Azure Active Directory ist der mehrinstanzenfähige cloudbasierte Verzeichnis- und Identitätsverwaltungsdienst von Microsoft. Alle Benutzer für diese Lösung werden in Azure Active Directory erstellt, so auch die Benutzer, die auf die Azure SQL-Datenbank zugreifen.
- Authentifizierung für die Anwendung erfolgt mithilfe von Azure Active Directory. Weitere Informationen finden Sie unter [Integrieren von Anwendungen in Azure Active Directory](https://docs.microsoft.com/azure/active-directory/manage-apps/plan-an-application-integration). Darüber hinaus wird für Verschlüsselung der Datenbankspalten Azure Active Directory verwendet, um die Anwendung in der Azure SQL-Datenbank zu authentifizieren. Weitere Informationen finden Sie unter [Eine Übersicht über die Sicherheitsfunktionen von Azure SQL-Datenbank](https://docs.microsoft.com/azure/sql-database/sql-database-security-overview).
- Die rollenbasierte Zugriffssteuerung in Azure ermöglicht Administratoren das Definieren präziser Zugriffsberechtigungen, um jeweils nur den Zugriff zu gewähren, den Benutzer für ihre Aufgaben benötigen. Anstatt jedem Benutzer uneingeschränkte Benutzerberechtigungen für Azure-Ressourcen zu gewähren, können Administratoren auch nur bestimmte Aktionen für den Zugriff auf Karteninhaberdaten erlauben. Der Abonnementzugriff ist auf den Abonnementadministrator beschränkt.
- Mit Azure Active Directory Privileged Identity Management können Kunden die Anzahl von Benutzern mit Zugriff auf bestimmte Informationen (etwa Karteninhaberdaten) einschränken. Administratoren können mit Azure Active Directory Privileged Identity Management privilegierte Identitäten und den damit verbundenen Zugriff auf Ressourcen erkennen, einschränken und überwachen. Diese Funktionalität kann auch verwendet werden, um einen bedarfsgesteuerten Just-in-Time-Administratorzugriff zu erzwingen.
- Azure Active Directory Identity Protection erkennt potenzielle Sicherheitsrisiken, die sich auf die Identitäten einer Organisation auswirken, konfiguriert automatisierte Reaktionen auf erkannte verdächtige Aktionen, die im Zusammenhang mit Identitäten der Organisation stehen, untersucht verdächtige Vorfälle und führt die entsprechenden Aktionen zu deren Behebung aus.

### <a name="secrets-management"></a>Verwaltung geheimer Schlüssel
Die Lösung verwendet Azure Key Vault für die Verwaltung von Schlüsseln und Geheimnissen. Azure Key Vault unterstützt Sie dabei, kryptografische Schlüssel und Geheimnisse zu schützen, die von Cloudanwendungen und -diensten verwendet werden. Die folgenden Funktionen von Azure Key Vault unterstützen Kunden beim Schützen von und Zugreifen auf diese Daten:

- Erweiterte Zugriffsrichtlinien werden auf Bedarfsbasis konfiguriert.
- Key Vault-Zugriffsrichtlinien werden mit minimalen erforderlichen Berechtigungen für Schlüssel und Geheimnisse definiert.
- Alle Schlüssel und Geheimnisse in Key Vault besitzen ein Ablaufdatum.
- Alle Schlüssel in Key Vault werden durch spezielle Hardwaresicherheitsmodule geschützt. Der Schlüsseltyp ist ein HSM-geschützter 2.048-Bit-RSA-Schlüssel.
- Mit Key Vault können Sie Schlüssel und Geheimnisse (beispielsweise Authentifizierungsschlüssel, Schlüssel für Speicherkonten, Datenverschlüsselungsschlüssel, PFX-Dateien und Kennwörter) verschlüsseln, indem Sie Schlüssel verwenden, die durch Hardwaresicherheitsmodule (HSMs) geschützt werden.
- Verwenden Sie die rollenbasierte Zugriffssteuerung, um Benutzern, Gruppen und Anwendungen Berechtigungen für einen bestimmten Bereich zu erteilen.
- Verwenden Sie Key Vault, um Ihre TLS-Zertifikate mit automatischer Verlängerung zu verwalten.
- Diagnoseprotokolle für Key Vault werden mit einer Aufbewahrungsdauer von mindestens 365 Tagen aktiviert.
- Zulässige kryptographische Vorgänge für Schlüssel sind auf die erforderlichen Vorgänge beschränkt.

### <a name="azure-security-center"></a>Azure Security Center
Mit Azure Security Center können Kunden zentral workloadübergreifende Sicherheitsrichtlinien anwenden und verwalten, die Angriffsfläche verringern sowie Angriffe erkennen und darauf reagieren. Darüber hinaus greift Azure Security Center auf die vorhandenen Konfigurationen der Azure-Dienste zu, um Empfehlungen für die Konfiguration und Verwaltung zu geben und zur Verbesserung der Sicherheit und zum Schutz Daten beizutragen.

Azure Security Center verwendet eine Vielzahl von Erkennungsfunktionen, um Kunden vor potenziellen Angriffen auf ihre Umgebungen zu warnen. Diese Warnungen enthalten wichtige Informationen zum Auslöser der Warnung, zu den möglicherweise betroffenen Ressourcen und zur Quelle des Angriffs. In Azure Security Center steht eine Reihe vordefinierter Sicherheitswarnungen zur Verfügung, die ausgelöst werden, wenn eine Bedrohung oder eine verdächtige Aktivität erkannt wird. Mithilfe benutzerdefinierter Warnungsregeln in Azure Security Center können Kunden neue Sicherheitswarnungen auf der Grundlage von Daten definieren, die bereits in ihrer Umgebung gesammelt werden.

Azure Security Center bietet priorisierte Sicherheitswarnungen und Incidents, sodass es für Kunden einfacher wird, potenzielle Sicherheitsprobleme zu erkennen und zu beheben. Für jede erkannte Bedrohung wird ein Bericht mit Bedrohungsinformationen generiert, der die zuständigen Teams bei der Untersuchung und Beseitigung von Bedrohungen unterstützt.

### <a name="azure-application-gateway"></a>Azure Application Gateway
Die Architektur verringert die Gefahr von Sicherheitsrisiken durch ein Azure Application Gateway mit einer konfigurierten Web Application Firewall und aktiviertem OWASP-Regelsatz. Weitere Funktionen:

- End-to-End-SSL
- Deaktivieren von TLS v1.0 und v1.1
- Aktivieren von TLS v1.2
- Web Application Firewall (Schutzmodus)
- Schutzmodus mit OWASP 3.0-Regelsatz
- Aktivieren der Diagnoseprotokollierung
- Benutzerdefinierte Integritätstests
- Azure Security Center und Azure Advisor bieten zusätzlichen Schutz und zusätzliche Benachrichtigungen. Azure Security Center stellt außerdem ein Reputationssystem bereit.

### <a name="logging-and-auditing"></a>Protokollierung und Überwachung
Azure-Dienste protokollieren umfassend die System- und Benutzeraktivitäten sowie die Systemintegrität:

- [Aktivitätsprotokolle](https://docs.microsoft.com/azure/monitoring-and-diagnostics/monitoring-overview-activity-logs) bieten Einblicke in Vorgänge, die für Ressourcen in einem Abonnement ausgeführt wurden. Aktivitätsprotokolle können dabei helfen, den Initiator eines Vorgangs, die Zeit des Auftretens und den Status zu bestimmen.
- [Diagnoseprotokolle](https://docs.microsoft.com/azure/monitoring-and-diagnostics/monitoring-overview-of-diagnostic-logs) umfassen sämtliche Protokolle, die von jeder Ressource ausgegeben werden. Zu diesen Protokollen gehören Windows-Ereignissystemprotokolle, Azure Storage-Protokolle, Key Vault-Überwachungsprotokolle sowie Application Gateway-Zugriffs- und -Firewallprotokolle. Alle Diagnoseprotokolle werden für die Archivierung in ein zentrales und verschlüsseltes Azure Storage-Konto geschrieben. Die Aufbewahrung kann vom Benutzer konfiguriert werden (bis zu 730 Tage), um den unternehmensspezifischen Aufbewahrungsanforderungen gerecht zu werden.

### <a name="azure-monitor-logs"></a>Azure Monitor-Protokolle
Diese Protokolle werden dann zur Verarbeitung, Speicherung und Dashboardanzeige in [Azure Monitor-Protokolle](https://azure.microsoft.com/services/log-analytics/) konsolidiert. Nachdem die Daten gesammelt wurden, werden sie in separaten Tabellen für die einzelnen Datentypen in Log Analytics-Arbeitsbereichen angeordnet, damit alle Daten unabhängig von der ursprünglichen Quelle zusammen analysiert werden können. Darüber hinaus ist das Azure Security Center in Azure Monitor-Protokolle integriert, sodass Kunden mit Kusto-Abfragen auf ihre Sicherheitsereignisdaten zugreifen und diese mit Daten aus anderen Diensten kombinieren können.

In dieser Architektur sind die folgenden Azure-[Überwachungslösungen](https://docs.microsoft.com/azure/log-analytics/log-analytics-add-solutions) enthalten:

- [Active Directory-Bewertung](https://docs.microsoft.com/azure/log-analytics/log-analytics-ad-assessment): Die Lösung „Active Directory-Integritätsüberprüfung“ bewertet die Risiken und die Integrität von Serverumgebungen in regelmäßigen Abständen und erstellt eine priorisierte Liste mit spezifischen Empfehlungen für die bereitgestellte Serverinfrastruktur.
- [SQL-Bewertung](https://docs.microsoft.com/azure/log-analytics/log-analytics-sql-assessment): Die Lösung „SQL-Integritätsüberprüfung“ bewertet die Risiken und die Integrität von Serverumgebungen in regelmäßigen Abständen und erstellt für die Kunden eine priorisierte Liste mit spezifischen Empfehlungen für die bereitgestellte Serverinfrastruktur.
- [Agent-Integritätsdiagnose](https://docs.microsoft.com/azure/operations-management-suite/oms-solution-agenthealth): Die Agent-Integritätsdiagnose meldet die Anzahl bereitgestellter Agents sowie deren geografische Verteilung. Außerdem meldet sie, wie viele Agents nicht mehr reagieren und wie viele Agents Betriebsdaten übermitteln.
- [Aktivitätsprotokollanalyse](https://docs.microsoft.com/azure/log-analytics/log-analytics-activity): Die Lösung „Aktivitätsprotokollanalyse“ hilft bei der Analyse der Azure-Aktivitätsprotokolle in allen Azure-Abonnements eines Kunden.

### <a name="azure-monitor"></a>Azure Monitor
[Azure Monitor](https://docs.microsoft.com/azure/monitoring-and-diagnostics/) bietet Überwachungsfunktionen für Organisationen und ermöglicht die Erstellung von Warnungen sowie die Archivierung von Daten (einschließlich Nachverfolgung von API-Aufrufen in ihren Azure-Ressourcen), um Benutzer beim Nachverfolgen der Leistung, beim Aufrechterhalten der Sicherheit und beim Identifizieren von Trends zu unterstützen.

### <a name="application-insights"></a>Application Insights
[Application Insights](https://docs.microsoft.com/azure/application-insights/app-insights-overview) ist ein erweiterbarer, für Webentwickler konzipierter Dienst zur Verwaltung der Anwendungsleistung auf mehreren Plattformen. Mit Application Insights werden Leistungsanomalien erkannt, und Kunden können die Lösung für die Liveüberwachung der Webanwendung nutzen. Sie verfügt über leistungsstarke Analysetools, mit denen Kunden Probleme diagnostizieren und nachvollziehen können, wie ihre App von den Benutzern verwendet wird. Der Dienst unterstützt Kunden bei der kontinuierlichen Verbesserung der Leistung und Benutzerfreundlichkeit.

### <a name="azure-key-vault"></a>Azure Key Vault
Erstellen Sie einen Tresor für die Schlüsselspeicherung der Organisation, und gewährleisten Sie die Verantwortlichkeit für operative Aufgaben:

- Folgende Daten werden in Key Vault gespeichert:

   - Application Insights-Schlüssel
   - Datenspeicher-Zugriffsschlüssel
   - Verbindungszeichenfolge
   - Datentabellenname
   - Benutzeranmeldeinformationen

- Erweiterte Zugriffsrichtlinien werden auf Bedarfsbasis konfiguriert.
- Key Vault-Zugriffsrichtlinien werden mit minimalen erforderlichen Berechtigungen für Schlüssel und Geheimnisse definiert.
- Alle Schlüssel und Geheimnisse in Key Vault besitzen ein Ablaufdatum.
- Alle Schlüssel in Key Vault sind durch HSM [Key Type = HSM Protected 2048-bit RSA Key] geschützt.
- Allen Benutzern/Identitäten werden mithilfe der rollenbasierten Zugriffssteuerung (Role Based Access Control, RBAC) die mindestens erforderlichen Berechtigungen erteilt.
- Anwendungen teilen sich nur dann einen Key Vault, wenn sie sich gegenseitig vertrauen und zur Laufzeit Zugriff auf dieselben Geheimnisse benötigen.
- Diagnoseprotokolle für Key Vault werden mit einer Aufbewahrungsdauer von mindestens 365 Tagen aktiviert.
- Zulässige kryptographische Vorgänge für Schlüssel sind auf die erforderlichen Vorgänge beschränkt.

### <a name="vpn-and-expressroute"></a>VPN und ExpressRoute
Zur Erzielung von sicheren Verbindungen mit Ressourcen, die als Teil dieser Referenzarchitektur einer PaaS-Webanwendung bereitgestellt werden, muss ein sicherer VPN-Tunnel oder eine [ExpressRoute-Verbindung](https://docs.microsoft.com/azure/expressroute/expressroute-introduction) konfiguriert werden. Durch die korrekte Einrichtung von VPN oder ExpressRoute können Kunden eine zusätzliche Sicherheitsebene für Daten während der Übertragung hinzufügen.

Mit der Implementierung eines sicheren VPN-Tunnels mit Azure kann eine virtuelle private Verbindung zwischen einem lokalen Netzwerk und einem virtuellen Azure-Netzwerk hergestellt werden. Diese Verbindung wird über das Internet hergestellt und ermöglicht Kunden das Übertragen von Informationen über einen „Tunnel“ innerhalb eines verschlüsselten Links zwischen dem Kundennetzwerk und Azure. Eine Site-to-Site-VPN-Verbindung ist eine sichere, ausgereifte Technologie, die seit Jahrzehnten von Unternehmen aller Größen eingesetzt wird. Bei dieser Option wird der IPsec-Tunnelmodus als Verschlüsselungsmechanismus verwendet.

Da Datenverkehr im VPN-Tunnel über ein Site-to-Site-VPN über das Internet übertragen wird, bietet Microsoft eine weitere Verbindungsoption an, die noch sicherer ist. Azure ExpressRoute ist ein dedizierter WAN-Link zwischen Azure und einem lokalen Standort oder einem Exchange-Hostinganbieter. Da ExpressRoute-Verbindungen nicht über das Internet verlaufen, bieten sie eine höhere Sicherheit, größere Zuverlässigkeit und schnellere Geschwindigkeit bei geringerer Latenz als herkömmliche Verbindungen über das Internet. Da es sich außerdem um eine direkte Verbindung vom Telekommunikationsanbieter des Kunden handelt, werden die Daten nicht über das Internet übertragen und dort nicht verfügbar gemacht.

Bewährte Methoden für das Implementieren eines sicheren hybriden Netzwerks, mit dem ein lokales Netzwerk auf Azure erweitert wird, finden Sie [hier](https://docs.microsoft.com/azure/architecture/reference-architectures/dmz/secure-vnet-hybrid).

## <a name="cost-considerations"></a>Kostenbetrachtung
Wenn Sie über kein Azure-Konto verfügen, können Sie ein kostenloses Konto erstellen. Rufen Sie zunächst die [Seite zur Erstellung eines kostenlosen Kontos](https://azure.microsoft.com/free/) auf, und sehen Sie sich an, was Sie mit einem kostenlosen Azure-Konto tun können und welche Produkte 12 Monate lang kostenlos genutzt werden können.

Sie müssen für einige Premium-Features zahlen, die Sie zur Bereitstellung der Ressourcen für die Beispiel-App mit Sicherheitsfeatures benötigen. Mit der Skalierung der App müssen die kostenlosen Tarife und Testversionen von Azure aktualisiert werden, um die Anwendungsanforderungen zu erfüllen. Dadurch können die Kosten steigen. Verwenden Sie den [Azure-Preisrechner](https://azure.microsoft.com/pricing/calculator/), um Ihre Kosten zu ermitteln.

## <a name="next-steps"></a>Nächste Schritte
Weitere Informationen zum Entwerfen, Entwickeln und Bereitstellen sicherer Anwendungen finden Sie in den folgenden Artikeln.

- [Entwurf](secure-design.md)
- [Entwickeln](secure-develop.md)
- [Bereitstellen](secure-deploy.md)
