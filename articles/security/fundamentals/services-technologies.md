---
title: Sicherheitsdienste und -technologien in Azure | Microsoft Docs
description: Der Artikel enthält eine Aufstellung von Sicherheitsdiensten und -technologien in Azure.
services: security
documentationcenter: na
author: barclayn
manager: barbkess
editor: TomSh
ms.assetid: a5a7f60a-97e2-49b4-a8c5-7c010ff27ef8
ms.service: security
ms.devlang: na
ms.topic: conceptual
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 1/29/2019
ms.author: barclayn
ms.openlocfilehash: 621e466b286c155459211c35990811f9e018e5fe
ms.sourcegitcommit: 6cff17b02b65388ac90ef3757bf04c6d8ed3db03
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 07/29/2019
ms.locfileid: "68610745"
---
# <a name="security-services-and-technologies-available-on-azure"></a>Bei Azure verfügbare Sicherheitsdienste und -technologien

In unseren Diskussionen mit bestehenden und zukünftigen Azure-Kunden werden wir häufig gefragt: „Haben Sie eine Liste aller sicherheitsbezogenen Dienste und Technologien, die Azure zu bieten hat?“

Bei der Auswertung der Optionen für Clouddienstanbieter ist es hilfreich, über diese Informationen zu verfügen. Daher haben wir diese Liste bereitgestellt, um Ihnen den Einstieg zu erleichtern.

Im Laufe der Zeit wird sich diese Liste – ebenso wie Azure – ändern und wachsen. Besuchen Sie diese Seite regelmäßig, um stets über den neuesten Stand unserer Sicherheitsdienste und -technologien informiert zu sein.

## <a name="general-azure-security"></a>Allgemeine Azure-Sicherheit
|Dienst|BESCHREIBUNG|
|--------|--------|
|[Azure&nbsp;Security&nbsp;Center](/azure/security-center/security-center-intro)| Eine Cloudlösung für Workloadschutz, die einheitliche Funktionen für die Sicherheitsverwaltung und den erweiterten Schutz vor Bedrohungen für Hybrid Cloud-Workloads bietet|
|[Azure Key Vault](/azure/key-vault/key-vault-overview)| Ein sicherer Kennwortspeicher für Kennwörter, Verbindungszeichenfolgen und andere Informationen, die Sie benötigen, damit Ihre Apps weiterhin funktionieren |
|[Azure Monitor-Protokolle](/azure/log-analytics/log-analytics-overview)|Ein Überwachungsdienst, der Telemetrie- und andere Daten erfasst und eine Abfragesprache sowie ein Analysemodul bietet, mit denen Sie Einblicke in die Abläufe Ihrer Apps und Ressourcen erhalten. Er kann alleine oder in Verbindung mit anderen Diensten wie Security Center verwendet werden. |
|[Dokumentation zu Azure Dev/Test Lab](/azure/lab-services/devtest-lab-overview)|Ein Dienst, der Entwicklern und Testern dabei hilft, Umgebungen in Azure schnell zu erstellen und dabei unnötigen Aufwand zu minimieren und die Kosten unter Kontrolle zu halten  |

<!---|[Azure&nbsp;Disk&nbsp;Encryption](/azure/azure-security-disk-encryption-overview)| THIS WILL GO TO THE NEW OVERVIEW TOPIC MEGHAN STEWART IS WRITING|--->

## <a name="storage-security"></a>Speichersicherheit
|Dienst|BESCHREIBUNG|
|------|--------|
| [Azure-Speicherdienstverschlüsselung&nbsp;&nbsp;&nbsp;](/azure/storage/common/storage-service-encryption)|Ein Sicherheitsfeature, das Ihre Daten in Azure Storage automatisch verschlüsselt   |
|[Verschlüsselte Hybridspeicherung mit StorSimple](/azure/storsimple/storsimple-ova-overview)| Eine integrierte Speicherlösung, die Speicheraufgaben zwischen lokalen Geräten und Azure-Cloudspeicher verwaltet|
|[Clientseitige Verschlüsselung in Azure](/azure/storage/common/storage-client-side-encryption)| Eine clientseitige Verschlüsselungslösung, die Daten in Clientanwendungen vor dem Hochladen in Azure Storage verschlüsselt. Außerdem werden die Daten beim Herunterladen entschlüsselt |
| [Azure Storage – Shared Access-Signaturen](/azure/storage/common/storage-dotnet-shared-access-signature-part-1)|Shared Access Signatures bieten delegierten Zugriff auf Ressourcen in Ihrem Speicherkonto.  |
|[Azure-Speicherkontenschlüssel](/azure/storage/common/storage-create-storage-account)| Eine Zugriffssteuerungsmethode für Azure Storage, die für die Authentifizierung beim Zugreifen auf das Speicherkonto verwendet wird |
|[Azure-Dateifreigaben mit SMB 3.0-Verschlüsselung](/azure/storage/files/storage-files-introduction)|Eine Netzwerksicherheitstechnologie, die automatische Netzwerkverschlüsselung für das SMB-Dateifreigabeprotokoll (Server Message Block) ermöglicht |
|[Azure-Speicheranalyse](/rest/api/storageservices/Storage-Analytics)| Eine Protokollierungs- und Metrikgenerierungstechnologie für die Daten in Ihrem Speicherkonto |

<!------>

## <a name="database-security"></a>Datenbanksicherheit
|Dienst|BESCHREIBUNG|
|------|--------|
| [Azure&nbsp;SQL&nbsp;Firewall](/azure/sql-database/sql-database-firewall-configure)|Ein Netzwerk-Zugriffssteuerungsfeature, das Schutz vor netzwerkbasierten Angriffen auf die Datenbank bietet |
|[Azure&nbsp;SQL-Zellebenenverschlüsselung&nbsp;&nbsp;](https://blogs.msdn.microsoft.com/sqlsecurity/2015/05/12/recommendations-for-using-cell-level-encryption-in-azure-sql-database/)| Eine Datenbanksicherheitstechnologie, die Verschlüsselung auf granularer Ebene bereitstellt  |
| [Azure&nbsp;SQL-Verbindungsverschlüsselung&nbsp;](/azure/sql-database/sql-database-control-access)|Zur Gewährleistung der Sicherheit steuert SQL-Datenbank den Zugriff mit Firewallregeln, die die Konnektivität nach IP-Adresse einschränken, Authentifizierungsmechanismen, die die Identitätsbestätigung durch Benutzer erfordern, und Autorisierungsmechanismen, die bestimmte Aktionen und Daten für Benutzer eingrenzen. |
| [Azure SQL – Always Encrypted](/sql/relational-databases/security/encryption/always-encrypted-database-engine?view=sql-server-2017)|Schützt vertrauliche Daten wie Kreditkartennummern oder nationale Identifikationsnummern (etwa Sozialversicherungsnummern), die in Azure SQL-Datenbank oder SQL Server-Datenbanken gespeichert sind  |
| [Azure&nbsp;SQL&nbsp;Transparent Data Encryption](/sql/relational-databases/security/encryption/transparent-data-encryption-azure-sql?view=azuresqldb-current)| Ein Datenbanksicherheitsfeature, das den Speicher einer gesamten Datenbank verschlüsselt |
| [Überwachung von Azure SQL-Datenbank](/azure/sql-database/sql-database-auditing)|Ein Datenbanküberwachungsfeature, das Datenbankereignisse verfolgt und diese in ein Überwachungsprotokoll in Ihrem Azure Storage-Konto schreibt  |


## <a name="identity-and-access-management"></a>Identitäts- und Zugriffsverwaltung
|Dienst|BESCHREIBUNG|
|------|--------|
| [Azure&nbsp;–&nbsp;Rollenbasierte&nbsp;Zugriffssteuerung](/azure/active-directory/role-based-access-control-configure)|Ein Zugriffssteuerungsfeature, mit dem Benutzer nur auf die Ressourcen zugreifen können, die sie anhand ihrer Rollen innerhalb der Organisation benötigen  |
| [Azure Active Directory](/azure/active-directory/fundamentals/active-directory-whatis)|Ein cloudbasiertes Authentifizierungsrepository, das ein cloudbasiertes Verzeichnis mit mehreren Mandanten sowie Verwaltungsdienste für mehrere Identitäten in Azure unterstützt  |
| [Azure Active Directory B2C](/azure/active-directory-b2c/active-directory-b2c-overview)|Ein Identitätsverwaltungsdienst, mit dem Sie die Registrierung und Anmeldung von Kunden und deren Verwaltung ihrer Profile bei der Verwendung von Azure-Anwendungen steuern können   |
| [Azure Active Directory-Domänendienste](/azure/active-directory-domain-services/overview)| Eine cloudbasierte und verwaltete Version von Active Directory Domain Services |
| [Azure Multi-Factor Authentication](/azure/active-directory/authentication/multi-factor-authentication)| Ein Sicherheitsmechanismus, der verschiedene Formen der Authentifizierung und Überprüfung anwendet, bevor der Zugriff auf abgesicherte Informationen zugelassen wird |

## <a name="backup-and-disaster-recovery"></a>Backup und Notfallwiederherstellung
|Dienst|BESCHREIBUNG|
|------|--------|
| [Azure&nbsp;Backup](/azure/backup/backup-introduction-to-azure-backup)| Ein Azure-basierter Dienst zum Sichern und Wiederherstellen von Daten in der Azure-Cloud |
| [Azure&nbsp;Site&nbsp;Recovery](/azure/site-recovery/site-recovery-overview)|Ein Onlinedienste, der Workloads, die auf physischen und virtuellen Computern ausgeführt werden, von einem primären Standort an einen sekundären Standort repliziert und somit eine Wiederherstellung der Dienste nach einem Ausfall ermöglicht |

## <a name="networking"></a>Netzwerk
|Dienst|BESCHREIBUNG|
|------|--------|
| [Netzwerksicherheitsgruppen&nbsp;&nbsp;](/azure/virtual-network/virtual-networks-nsg)| Ein netzwerkbasiertes Zugriffssteuerungsfeature, das Entscheidungen über das Zulassen oder Verweigern mithilfe eines 5-Tupels trifft  |
| [Azure VPN Gateway](/azure/vpn-gateway/vpn-gateway-about-vpngateways)| Ein als VPN-Endpunkt verwendetes Netzwerkgerät, das den standortübergreifenden Zugriff auf Azure Virtual Networks ermöglicht  |
| [Azure Application Gateway](/azure/application-gateway/application-gateway-introduction)|Ein erweiterter Webanwendungs-Lastenausgleich, der Datenverkehr anhand der URL weiterleiten und SSL-Abladung ausführen kann |
|[Web Application Firewall](/azure/frontdoor/waf-overview) (WAF)|Ein Feature von Application Gateway, das zentralisierten Schutz Ihrer Webanwendungen vor allgemeinen Exploits und Sicherheitsrisiken bietet|
| [Azure-Lastenausgleich](/azure/load-balancer/load-balancer-overview)|Ein TCP/UDP-Anwendungslastenausgleich für das Netzwerk |
| [Azure ExpressRoute](/azure/expressroute/expressroute-introduction)| Ein dedizierter WAN-Link zwischen einem lokalen Netzwerk und Azure Virtual Networks |
| [Azure Traffic Manager](/azure/traffic-manager/traffic-manager-overview)| Ein globaler DNS-Lastenausgleich|
| [Azure Anwendungsproxy](/azure/active-directory/active-directory-application-proxy-get-started)| Ein Authentifizierungs-Front-End für den sicheren Remotezugriff auf lokal gehostete Webanwendungen |
|[Azure Firewall](/azure/firewall/overview)|Ein verwalteter, cloudbasierter Netzwerksicherheitsdienst, der Ihre Azure Virtual Network-Ressourcen schützt.|
|[Azure DDoS Protection](/azure/virtual-network/ddos-protection-overview)|Schützt in Kombination mit bewährten Anwendungsentwurfsmethoden vor DDoS-Angriffen.|
|[Virtual Network-Dienstendpunkte](/azure/virtual-network/virtual-network-service-endpoints-overview)|Erweitern den privaten Adressraum Ihres virtuellen Netzwerks und die Identität Ihres VNET über eine direkte Verbindung auf die Azure-Dienste.|