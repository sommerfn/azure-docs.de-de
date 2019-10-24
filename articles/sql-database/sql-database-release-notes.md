---
title: Azure SQL-Datenbank – Versionshinweise | Microsoft-Dokumentation
description: Weitere Informationen zu den neuen Features und Verbesserungen in Azure SQL-Datenbank und in der zugehörigen Dokumentation
services: sql-database
author: stevestein
ms.service: sql-database
ms.subservice: service
ms.devlang: ''
ms.topic: conceptual
ms.date: 05/15/2019
ms.author: sstein
ms.openlocfilehash: f1450399dc027a6977f4c99507e2e15b301272c4
ms.sourcegitcommit: 961468fa0cfe650dc1bec87e032e648486f67651
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 10/10/2019
ms.locfileid: "72249387"
---
# <a name="sql-database-release-notes"></a>Versionshinweise zu SQL-Datenbank

In diesem Artikel werden die SQL-Datenbank-Features aufgeführt, die sich derzeit in der Public Preview-Phase befinden. Informationen zu Updates und Verbesserungen für SQL-Datenbank finden Sie unter [Azure-Updates](https://azure.microsoft.com/updates/?product=sql-database). Informationen zu Updates und Verbesserungen für andere Azure-Dienste finden Sie unter [Dienstupdates](https://azure.microsoft.com/updates).

## <a name="features-in-public-preview"></a>Features in der öffentlichen Vorschau

### <a name="single-databasetabsingle-database"></a>[Einzeldatenbank](#tab/single-database)

| Feature | Details |
| ---| --- |
| [Azure Private Link](https://azure.microsoft.com/updates/private-link-now-available-in-preview/)| Mit Private Link können Sie die Netzwerkarchitektur vereinfachen und die Verbindung zwischen Endpunkten in Azure schützen, indem die Daten im Azure-Netzwerk verbleiben und nicht im Internet verfügbar gemacht werden. Außerdem können Sie mit Private Link eigene Dienste in Azure erstellen und rendern. |
| Beschleunigte Datenbankwiederherstellung bei Singletons und Pools für elastische Datenbanken | Weitere Informationen finden Sie unter [Schnellere Datenbankwiederherstellung](sql-database-accelerated-database-recovery.md).|
|Geschätzte eindeutige Anzahl|Weitere Informationen finden Sie unter [Geschätzte Abfrageverarbeitung](https://docs.microsoft.com/sql/relational-databases/performance/intelligent-query-processing#approximate-query-processing).|
|Batchmodus für Rowstore (unter Kompatibilitätsgrad 150)|Weitere Informationen finden Sie unter [Batchmodus bei Rowstore](https://docs.microsoft.com/sql/relational-databases/performance/intelligent-query-processing#batch-mode-on-rowstore).|
| Datenermittlung und -klassifizierung  |Weitere Informationen finden Sie unter [Azure SQL-Datenbank und SQL Data Warehouse: Datenermittlung und -klassifizierung](sql-database-data-discovery-and-classification.md).|
| Aufträge für die elastische Datenbank | Weitere Informationen finden Sie unter [Erstellen, Konfigurieren und Verwalten von Aufträgen für die elastische Datenbank](elastic-jobs-overview.md). |
| Elastische Abfragen | Weitere Informationen finden Sie unter [Übersicht über elastische Abfragen](sql-database-elastic-query-overview.md). |
| Elastische Transaktionen | [Verteilte Transaktionen über Clouddatenbanken](sql-database-elastic-transactions-overview.md). |
|Feedback zur Speicherzuweisung (Zeilenmodus) (unter Kompatibilitätsgrad 150)|Weitere Informationen finden Sie unter [Feedback zur Speicherzuweisung im Zeilenmodus](https://docs.microsoft.com/sql/relational-databases/performance/intelligent-query-processing#row-mode-memory-grant-feedback).|
| Abfrage-Editor im Azure-Portal |Weitere Informationen finden Sie unter [Verwenden des SQL-Abfrage-Editors im Azure-Portal zum Verbinden und Abfragen von Daten](sql-database-connect-query-portal.md).|
| R-Dienste/maschinelles Lernen mit Singletons und Pools für elastische Datenbanken |Weitere Informationen finden Sie unter [Machine Learning Services in Azure SQL Database (Machine Learning Services in Azure SQL-Datenbank)](https://docs.microsoft.com/sql/advanced-analytics/what-s-new-in-sql-server-machine-learning-services?view=sql-server-2017#machine-learning-services-in-azure-sql-database).|
| Serverlose Computeebene | Weitere Informationen finden Sie unter [SQL-Datenbank – serverlos (Vorschau)](sql-database-serverless.md).|
|Azure SQL-Analyse|Weitere Informationen finden Sie unter [Azure SQL-Analyse](../azure-monitor/insights/azure-sql.md).|
|Verzögerte Kompilierung von Tabellenvariablen (unter Kompatibilitätsgrad 150)|Weitere Informationen finden Sie unter [Verzögerte Kompilierung von Tabellenvariablen](https://docs.microsoft.com/sql/relational-databases/performance/intelligent-query-processing#table-variable-deferred-compilation).|
| &nbsp; |

### <a name="managed-instancetabmanaged-instance"></a>[Verwaltete Instanz](#tab/managed-instance)

| Feature | Details |
| ---| --- |
| <a href="/azure/sql-database/sql-database-managed-instance-connectivity-architecture#service-aided-subnet-configuration-public-preview-in-east-us-and-west-us">Dienstgestützte Subnetzkonfiguration</a> | Eine sichere und praktische Möglichkeit zum Verwalten der Subnetzkonfiguration. |
| <a href="/azure/sql-database/sql-database-instance-pools">Instanzenpools</a> | Eine praktische und kostengünstige Möglichkeit, um kleinere SQL-Instanzen zur Cloud zu migrieren. |
| <a href="https://aka.ms/managed-instance-tde-byok">Transparent Data Encryption (TDE) mit Bring Your Own Key (BYOK)</a> |Weitere Informationen finden Sie unter [Azure SQL Transparent Data Encryption mithilfe von Schlüsseln, die vom Kunden in Azure Key Vault verwaltet werden: Bring Your Own Key-Unterstützung](transparent-data-encryption-byok-azure-sql.md).|
| <a href="https://aka.ms/managed-instance-aadlogins">Azure AD-Serverprinzipale auf Instanzebene (Anmeldungen)</a> | Erstellen Sie Anmeldungen auf Serverebene mithilfe der Anweisung <a href="https://docs.microsoft.com/sql/t-sql/statements/create-login-transact-sql?view=azuresqldb-mi-current">CREATE LOGIN FROM EXTERNAL PROVIDER</a>. |
| [Transaktionsreplikation](sql-database-managed-instance-transactional-replication.md) | Replizieren Sie die Änderungen aus Ihren Tabellen in andere Datenbanken, die in verwalteten Instanzen, Einzeldatenbanken oder SQL Server-Instanzen abgelegt wurden, oder aber aktualisieren Sie die Tabellen, wenn einige Zeilen in anderen verwalteten Instanzen oder einer SQL Server-Instanz geändert werden. Weitere Informationen finden Sie unter [Konfigurieren der Replikation in einer verwalteten Azure SQL-Datenbank-Instanzdatenbank](replication-with-sql-database-managed-instance.md). |
| Bedrohungserkennung |Weitere Informationen finden Sie unter [Konfigurieren der Bedrohungserkennung (Vorschau) für eine verwaltete Azure SQL-Datenbank-Instanz](sql-database-managed-instance-threat-detection.md).|
| Erneutes Erstellen gelöschter Datenbanken mit verwalteten Instanzen |Weitere Informationen finden Sie unter [Re-create dropped databases in Azure SQL Managed Instance (Erneutes Erstellen gelöschter Datenbanken in verwalteten Azure SQL-Datenbank-Instanzen)](https://medium.com/azure-sqldb-managed-instance/re-create-dropped-databases-in-azure-sql-managed-instance-dc369ed60266).|
| &nbsp; |

---

## <a name="new-features"></a>Neue Funktionen

### <a name="managed-instance-h2-2019-updates"></a>Verwaltete Instanz – H2 2019-Updates

- [Gruppen für automatisches Failover](https://azure.microsoft.com/updates/azure-sql-database-auto-failover-groups-feature-now-available-in-all-regions/) ermöglichen es Ihnen, alle Datenbanken aus der primären Instanz in eine sekundäre Instanz in einer anderen Region zu replizieren.
- Konfigurieren Sie das Verhalten Ihrer verwalteten Instanz mit [globalen Ablaufverfolgungsflags](https://azure.microsoft.com/updates/global-trace-flags-are-now-available-in-azure-sql-database-managed-instance/).

### <a name="managed-instance-h1-2019-updates"></a>Verwaltete Instanz – H1 2019-Updates

Die folgenden Funktionen sind im Bereitstellungsmodell für verwaltete Instanzen in H1 2019 aktiviert:
  - Unterstützung für Abonnements mit <a href="https://aka.ms/sql-mi-visual-studio-subscribers">monatlicher Azure-Gutschrift für Visual Studio-Abonnenten</a> und erhöhte [regionale Grenzwerte](sql-database-managed-instance-resource-limits.md#regional-resource-limitations).
  - Unterstützung für <a href="https://docs.microsoft.com/sharepoint/administration/deploy-azure-sql-managed-instance-with-sharepoint-servers-2016-2019"> SharePoint 2016 und SharePoint 2019 </a> und <a href="https://docs.microsoft.com/business-applications-release-notes/october18/dynamics365-business-central/support-for-azure-sql-database-managed-instance"> Dynamics 365 Business Central </a>
  - Erstellen Sie Instanzen mit <a href="https://aka.ms/managed-instance-collation">Sortierung auf Serverebene</a> und <a href="https://azure.microsoft.com/updates/managed-instance-time-zone-ga/">Zeitzone</a> für Ihre Auswahl.
  - Verwaltete Instanzen sind jetzt durch eine <a href="sql-database-managed-instance-management-endpoint-verify-built-in-firewall.md">integrierte Firewall</a> geschützt.
  - Konfigurieren Sie Instanzen für die Verwendung von [öffentlichen Endpunkten](sql-database-managed-instance-public-endpoint-configure.md), einer Verbindung zur [Proxy-Außerkraftsetzung](sql-database-connectivity-architecture.md#connection-policy), um eine bessere Netzwerkleistung zu erzielen, <a href="https://aka.ms/four-cores-sql-mi-update">4 virtuellen Kernen bei der Gen5-Hardwaregenerierung</a>, oder <a href="https://aka.ms/managed-instance-configurable-backup-retention">konfigurieren Sie die Beibehaltung der Sicherung bis zu 35 Tage</a> für eine Point-in-Time-Wiederherstellung. Die langfristige Beibehaltung der Sicherung (bis zu 10 Jahre) ist noch nicht aktiviert, sodass Sie als Alternative <a href="https://docs.microsoft.com/sql/relational-databases/backup-restore/copy-only-backups-sql-server">Kopiesicherungen</a> verwenden können.
  - Neue Funktionen ermöglichen Ihnen die <a href="https://medium.com/@jocapc/geo-restore-your-databases-on-azure-sql-instances-1451480e90fa">Geowiederherstellung Ihrer Datenbank in einem anderen Rechenzentrum mithilfe von PowerShell </a>, das [Umbenennen einer Datenbank](https://azure.microsoft.com/updates/azure-sql-database-managed-instance-database-rename-is-supported/) und das [Löschen eines virtuellen Clusters](sql-database-managed-instance-delete-virtual-cluster.md).
  - Die neue integrierte [Rolle „Mitwirkender für Instanzen“](https://docs.microsoft.com/azure/role-based-access-control/built-in-roles#sql-managed-instance-contributor) ermöglicht SoD-Einhaltung (Separation of Duty, Aufgabentrennung) von Sicherheitsprinzipien und Einhaltung von Unternehmensstandards.
  - Eine verwaltete Instanz ist in den folgenden Azure Government-Regionen für GA (US Gov Texas, US Gov Arizona) sowie in China, Norden 2, und China, Osten 2, verfügbar. Außerdem ist sie in den folgenden öffentlichen Regionen verfügbar: Australien, Mitte; Australien, Mitte 2; Brasilien, Süden; Frankreich, Süden; VAE, Mitte; VAE, Norden; Südafrika, Norden; Südafrika, Westen.

## <a name="fixed-known-issues"></a>Behobene bekannte Probleme

- **Aug 2019** –Eigenständige Datenbanken werden in einer verwalteten Instanz vollständig unterstützt.

## <a name="updates"></a>Aktualisierungen

Eine Liste mit Updates und Verbesserungen für SQL-Datenbank finden Sie unter [Azure-Updates](https://azure.microsoft.com/updates/?product=sql-database).

Informationen zu Updates und Verbesserungen für alle Azure-Dienste finden Sie unter [Dienstupdates](https://azure.microsoft.com/updates).

## <a name="contribute-to-content"></a>Mitwirkung am Inhalt

Informationen zur Mitwirkung an der Dokumentation für Azure SQL-Datenbank finden Sie unter [Leitfaden für Mitwirkende an der Microsoft-Dokumentation: Übersicht](https://docs.microsoft.com/contribute/).
