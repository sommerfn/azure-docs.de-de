---
title: Verwaltete Azure SQL-Datenbank-Instanz – T-SQL-Unterschiede | Microsoft-Dokumentation
description: In diesem Artikel werden die T-SQL-Unterschiede zwischen einer verwalteten Instanz in Azure SQL-Datenbank und SQL Server beschrieben.
services: sql-database
ms.service: sql-database
ms.subservice: managed-instance
ms.devlang: ''
ms.topic: conceptual
author: jovanpop-msft
ms.author: jovanpop
ms.reviewer: sstein, carlrab, bonova
ms.date: 08/12/2019
ms.custom: seoapril2019
ms.openlocfilehash: 9a043d07004870c00c656b655d56a1526f8993d8
ms.sourcegitcommit: be344deef6b37661e2c496f75a6cf14f805d7381
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 10/07/2019
ms.locfileid: "72000494"
---
# <a name="managed-instance-t-sql-differences-limitations-and-known-issues"></a>Verwaltete Instanz, T-SQL-Unterschiede, Einschränkungen und bekannte Probleme

In diesem Artikel werden die Unterschiede in der Syntax und dem Verhalten zwischen einer verwalteten Azure SQL-Datenbank-Instanz und einer lokalen SQL Server-Datenbank-Engine zusammengefasst und erläutert. Die Bereitstellungsoption „Verwaltete Instanz“ bietet umfassende Kompatibilität mit einer lokalen SQL Server-Datenbank-Engine. Die meisten Features der SQL Server-Datenbank-Engine werden in verwalteten Instanzen unterstützt.

![Migration](./media/sql-database-managed-instance/migration.png)

Es gibt einige PaaS-Einschränkungen, die in der verwalteten Instanz eingeführt werden, und einige Verhaltensänderungen im Vergleich zu SQL Server. Die Unterschiede sind in die folgenden Kategorien unterteilt: <a name="Differences"></a>

- [Verfügbarkeit](#availability), einschließlich der Unterschiede bei [Always On](#always-on-availability) und [Sicherungen](#backup)
- [Sicherheit](#security), einschließlich der Unterschiede bei [Überwachung](#auditing), [Zertifikaten](#certificates), [Anmeldeinformationen](#credential), [Kryptografieanbietern](#cryptographic-providers), [Anmeldungen und Benutzern](#logins-and-users) sowie [Dienstschlüsseln und Diensthauptschlüsseln](#service-key-and-service-master-key)
- [Konfiguration](#configuration), einschließlich der Unterschiede bei [Pufferpoolerweiterung](#buffer-pool-extension), [Sortierung](#collation), [Kompatibilitätsgraden](#compatibility-levels),[Datenbankspiegelung](#database-mirroring), [Datenbankoptionen](#database-options), [SQL Server-Agent](#sql-server-agent) und [Tabellenoptionen](#tables)
- [Funktionen](#functionalities), einschließlich [BULK INSERT/OPENROWSET](#bulk-insert--openrowset), [CLR](#clr), [DBCC](#dbcc), [verteilter Transaktionen](#distributed-transactions), [erweiterter Ereignisse](#extended-events), [externer Bibliotheken](#external-libraries), [Filestream und Dateitabelle](#filestream-and-filetable), [semantischer Volltextsuche](#full-text-semantic-search), [Verbindungsserver](#linked-servers), [PolyBase](#polybase), [Replikation](#replication), [RESTORE](#restore-statement), [Service Broker](#service-broker) sowie [gespeicherter Prozeduren, Funktionen und Trigger](#stored-procedures-functions-and-triggers).
- [Umgebungseinstellungen](#Environment) wie VNets und Subnetzkonfigurationen.

Die meisten dieser Features sind architekturbezogene Einschränkungen und stellen Dienstfeatures dar.

Auf dieser Seite werden auch [temporäre bekannte Probleme](#Issues) erläutert, die in einer verwalteten Instanz erkannt wurden und in Zukunft behoben werden.

## <a name="availability"></a>Verfügbarkeit

### <a name="always-on-availability"></a>Always On

[Hochverfügbarkeit](sql-database-high-availability.md) ist in verwalteten Instanzen integriert und kann von Benutzern nicht gesteuert werden. Folgende Anweisungen werden nicht unterstützt:

- [CREATE ENDPOINT … FOR DATABASE_MIRRORING](https://docs.microsoft.com/sql/t-sql/statements/create-endpoint-transact-sql)
- [CREATE AVAILABILITY GROUP](https://docs.microsoft.com/sql/t-sql/statements/create-availability-group-transact-sql)
- [ALTER AVAILABILITY GROUP](https://docs.microsoft.com/sql/t-sql/statements/alter-availability-group-transact-sql)
- [DROP AVAILABILITY GROUP](https://docs.microsoft.com/sql/t-sql/statements/drop-availability-group-transact-sql)
- Die Klausel [SET HADR](https://docs.microsoft.com/sql/t-sql/statements/alter-database-transact-sql-set-hadr) der Anweisung [ALTER DATABASE](https://docs.microsoft.com/sql/t-sql/statements/alter-database-transact-sql)

### <a name="backup"></a>Backup

In verwalteten Instanzen werden automatische Sicherungen durchgeführt, sodass Benutzer vollständige `COPY_ONLY`-Sicherungen für Datenbanken erstellen können. Differenzielle, Protokoll- und Dateimomentaufnahme-Sicherungen werden nicht unterstützt.

- Bei verwalteten Instanzen können Sie eine Instanzdatenbank nur in einem Azure Blob Storage-Konto sichern:
  - Nur `BACKUP TO URL` wird unterstützt.
  - `FILE`, `TAPE` und Sicherungsmedien werden nicht unterstützt.
- Die meisten allgemeinen `WITH`-Optionen werden unterstützt.
  - `COPY_ONLY` ist obligatorisch.
  - `FILE_SNAPSHOT` wird nicht unterstützt.
  - Bandoptionen: `REWIND`, `NOREWIND`, `UNLOAD` und `NOUNLOAD` werden nicht unterstützt.
  - Protokollspezifische Optionen: `NORECOVERY`, `STANDBY` und `NO_TRUNCATE` werden nicht unterstützt.

Einschränkungen: 

- Bei verwalteten Instanzen können Sie eine Instanzdatenbank in einer Sicherung mit bis zu 32 Stripes sichern. Dies ist ausreichend für Datenbanken mit bis zu 4 TB, wenn die Sicherungskomprimierung verwendet wird.
- Sie können `BACKUP DATABASE ... WITH COPY_ONLY` nicht in einer Datenbank ausführen, die mit vom Dienst verwalteter Transparent Data Encryption (TDE) verschlüsselt ist. Die vom Dienst verwaltete TDE erzwingt die Verschlüsselung von Sicherungen mit einem internen TDE-Schlüssel. Der Schlüssel kann nicht exportiert werden, daher können Sie die Sicherung nicht wiederherstellen. Verwenden Sie automatische Sicherungen und die Point-in-Time-Wiederherstellung, oder verwenden Sie stattdessen [vom Kunden verwaltete Transparent Data Encryption – BYOK (Bring Your Own Key)](https://docs.microsoft.com/azure/sql-database/transparent-data-encryption-azure-sql#customer-managed-transparent-data-encryption---bring-your-own-key). Sie können die Verschlüsselung für die Datenbank auch deaktivieren.
- Die maximale Stripegröße für Sicherungen mit dem Befehl `BACKUP` in einer verwalteten Instanz beträgt 195 GB. Dies ist die maximale Blobgröße. Erhöhen Sie die Anzahl der Stripes im Sicherungsbefehl, um die einzelne Stripegröße zu verringern und diese Einschränkung einzuhalten.

    > [!TIP]
    > Um diese Einschränkung beim Sichern einer Datenbank von SQL Server in einer lokalen Umgebung oder auf einem virtuellen Computer zu umgehen, haben Sie folgende Möglichkeiten:
    >
    > - Führen Sie die Sicherung mit `DISK` anstatt mit `URL` aus.
    > - Laden Sie die Sicherungsdateien in Blob Storage hoch.
    > - Stellen Sie die Daten in der verwalteten Instanz wieder her.
    >
    > Der Befehl `Restore` in einer verwalteten Instanz unterstützt höhere Blobgrößen in den Sicherungsdateien, da für die Speicherung der hochgeladenen Sicherungsdateien ein anderer Blobtyp verwendet wird.

Informationen zu Sicherungen mithilfe von T-SQL finden Sie unter [BACKUP](https://docs.microsoft.com/sql/t-sql/statements/backup-transact-sql).

## <a name="security"></a>Sicherheit

### <a name="auditing"></a>Überwachung

Die wichtigsten Unterschiede zwischen der Überwachung in Datenbanken in Azure SQL-Datenbank und SQL Server:

- Bei der Bereitstellungsoption „Verwaltete Instanz“ in Azure SQL-Datenbank wird die Überwachung auf Serverebene ausgeführt. Die `.xel`-Protokolldateien werden in Azure Blob Storage gespeichert.
- Bei Bereitstellungen als Singleton und in Pools für elastische Datenbanken in Azure SQL-Datenbank erfolgt die Überwachung auf Datenbankebene.
- In einer SQL Server-Instanz auf lokalen oder virtuellen Computern wird die Überwachung auf Serverebene ausgeführt. Ereignisse werden in Dateisystem- oder Windows-Ereignisprotokollen gespeichert.
 
Die XEvent-Überwachung in einer verwalteten Instanz unterstützt Azure Blob Storage-Ziele. Dateiprotokolle und Windows-Protokolle werden nicht unterstützt.

Wichtigste Unterschiede in der Syntax von `CREATE AUDIT` zur Überwachung in Azure Blob Storage:

- Mit der neuen Syntax `TO URL` können Sie die URL des Azure Blob Storage-Containers angeben, in dem die `.xel`-Dateien gespeichert werden.
- Die Syntax `TO FILE` wird nicht unterstützt, weil verwaltete Instanzen nicht auf Windows-Dateifreigaben zugreifen können.

Weitere Informationen finden Sie unter 

- [CREATE SERVER AUDIT](https://docs.microsoft.com/sql/t-sql/statements/create-server-audit-transact-sql) 
- [ALTER SERVER AUDIT](https://docs.microsoft.com/sql/t-sql/statements/alter-server-audit-transact-sql)
- [Überwachung](https://docs.microsoft.com/sql/relational-databases/security/auditing/sql-server-audit-database-engine)

### <a name="certificates"></a>Zertifikate

Eine verwaltete Instanz kann nicht auf Dateifreigaben und Windows-Ordner zugreifen. Daher gelten folgende Einschränkungen:

- Die `CREATE FROM`/`BACKUP TO`-Datei wird für Zertifikate nicht unterstützt.
- Das `CREATE`/`BACKUP`-Zertifikat aus `FILE`/`ASSEMBLY` wird nicht unterstützt. Dateien mit privaten Schlüsseln können nicht verwendet werden. 

Siehe [CREATE CERTIFICATE](https://docs.microsoft.com/sql/t-sql/statements/create-certificate-transact-sql) und [BACKUP CERTIFICATE](https://docs.microsoft.com/sql/t-sql/statements/backup-certificate-transact-sql). 
 
**Problemumgehung**: Anstatt eine Sicherung des Zertifikats zu erstellen und die Sicherung wiederherzustellen, [rufen Sie den binären Inhalt des Zertifikats und den privaten Schlüssel ab, speichern Sie sie als SQL-Datei, und erstellen Sie Folgendes aus den Binärdaten](https://docs.microsoft.com/sql/t-sql/functions/certencoded-transact-sql#b-copying-a-certificate-to-another-database):

```sql
CREATE CERTIFICATE  
   FROM BINARY = asn_encoded_certificate
WITH PRIVATE KEY (<private_key_options>)
```

### <a name="credential"></a>Anmeldeinformation

Nur Azure Key Vault- und `SHARED ACCESS SIGNATURE`-Identitäten werden unterstützt. Windows-Benutzer werden nicht unterstützt.

Siehe [CREATE CREDENTIAL](https://docs.microsoft.com/sql/t-sql/statements/create-credential-transact-sql) und [ALTER CREDENTIAL](https://docs.microsoft.com/sql/t-sql/statements/alter-credential-transact-sql).

### <a name="cryptographic-providers"></a>Kryptografieanbieter

Eine verwaltete Instanz kann nicht auf Dateien zugreifen. Daher können keine Kryptografieanbieter erstellt werden:

- `CREATE CRYPTOGRAPHIC PROVIDER` wird nicht unterstützt. Siehe [CREATE CRYPTOGRAPHIC PROVIDER](https://docs.microsoft.com/sql/t-sql/statements/create-cryptographic-provider-transact-sql).
- `ALTER CRYPTOGRAPHIC PROVIDER` wird nicht unterstützt. Siehe [ALTER CRYPTOGRAPHIC PROVIDER](https://docs.microsoft.com/sql/t-sql/statements/alter-cryptographic-provider-transact-sql).

### <a name="logins-and-users"></a>Anmeldungen und Benutzer

- Mithilfe von `FROM CERTIFICATE`, `FROM ASYMMETRIC KEY` und `FROM SID` erstellte SQL-Anmeldungen werden unterstützt. Siehe [CREATE LOGIN](https://docs.microsoft.com/sql/t-sql/statements/create-login-transact-sql).
- Azure Active Directory-Dienstprinzipale (Anmeldungen), die mit der Syntax [CREATE LOGIN](https://docs.microsoft.com/sql/t-sql/statements/create-login-transact-sql?view=azuresqldb-mi-current) oder der Syntax [CREATE USER FROM LOGIN [Azure AD-Anmeldung]](https://docs.microsoft.com/sql/t-sql/statements/create-user-transact-sql?view=azuresqldb-mi-current) erstellt wurden, werden unterstützt (öffentliche Vorschau). Diese Anmeldungen werden auf Serverebene erstellt.

    Verwaltete Instanzen unterstützen Azure AD-Datenbankprinzipale mit der Syntax `CREATE USER [AADUser/AAD group] FROM EXTERNAL PROVIDER`. Dieses Feature wird auch als Azure AD-Benutzer für eigenständige Datenbanken bezeichnet.

- Windows-Anmeldungen, die mit der Syntax `CREATE LOGIN ... FROM WINDOWS` erstellt wurden, werden nicht unterstützt. Verwenden Sie Azure Active Directory-Anmeldungen und -Benutzer.
- Der Azure AD-Benutzer, der die Instanz erstellt hat, verfügt über [uneingeschränkte Administratorrechte](sql-database-manage-logins.md#unrestricted-administrative-accounts).
- Azure AD-Benutzer auf Datenbankebene ohne Administratorrechte können mit der Syntax `CREATE USER ... FROM EXTERNAL PROVIDER` erstellt werden. Siehe [CREATE USER ... FROM EXTERNAL PROVIDER](sql-database-manage-logins.md#non-administrator-users).
- Azure AD-Serverprinzipale (Anmeldungen) unterstützen SQL-Funktionen nur innerhalb einer verwalteten Instanz. Funktionen, die eine instanzübergreifende Interaktion erfordern – unabhängig davon, ob innerhalb desselben Azure AD-Mandanten oder in verschiedenen Mandanten –, werden für Azure AD-Benutzer nicht unterstützt. Beispiele für solche Funktionen:

  - SQL-Transaktionsreplikation
  - Linkserver

- Das Festlegen einer Azure AD-Anmeldung, die einer Azure AD-Gruppe zugeordnet ist, als Besitzer der Datenbank wird nicht unterstützt.
- Identitätswechsel von Azure AD-Prinzipalen auf Serverebene unter Verwendung anderer Azure AD-Prinzipale werden unterstützt, z.B. in der [EXECUTE AS](/sql/t-sql/statements/execute-as-transact-sql)-Klausel. Einschränkung für EXECUTE AS:

  - EXECUTE AS USER wird für Azure AD-Benutzer nicht unterstützt, wenn der Name sich vom Anmeldenamen unterscheidet. Beispiel: Der Benutzer wird über die Syntax CREATE USER [meinAADBenutzer] FROM LOGIN [john@contoso.com] erstellt, und es wird ein Identitätswechsel über EXEC AS USER = _meinAADBenutzer_ versucht. Wenn Sie einen Benutzer (**USER**) auf der Grundlage eines Azure AD-Serverprinzipals (Anmeldung) erstellen, müssen Sie als Benutzernamen den gleichen Anmeldenamen angeben wie in der Anmeldung (**LOGIN**).
  - Die folgenden Vorgänge für Azure AD-Prinzipale können nur von Prinzipalen (Anmeldungen) auf SQL Server-Ebene ausgeführt werden, die der Rolle `sysadmin` angehören:

    - EXECUTE AS USER
    - EXECUTE AS LOGIN

- Einschränkungen der öffentlichen Vorschau für Azure AD-Serverprinzipale (Anmeldungen):

  - Einschränkungen für Active Directory-Administratoren für verwaltete Instanzen:

    - Der Azure AD-Administrator, der zum Einrichten der verwalteten Instanz verwendet wurde, kann nicht zum Erstellen eines Azure AD-Serverprinzipals (Anmeldung) innerhalb der verwalteten Instanz verwendet werden. Sie müssen den ersten Azure AD-Serverprinzipal (Anmeldung) mit einem SQL Server-Konto erstellen, das die Rolle `sysadmin` besitzt. Diese temporäre Einschränkung wird aufgehoben, wenn Azure AD-Serverprinzipale (Anmeldungen) allgemein verfügbar sind. Wenn Sie versuchen, die Anmeldung mit einem Azure AD-Administratorkonto zu erstellen, wird der folgende Fehler angezeigt: `Msg 15247, Level 16, State 1, Line 1 User does not have permission to perform this action.`
      - Derzeit muss die erste Azure AD-Anmeldung in der Masterdatenbank unter Verwendung von [CREATE LOGIN](/sql/t-sql/statements/create-login-transact-sql?view=azuresqldb-mi-current) FROM EXTERNAL PROVIDER durch das SQL Server-Standardkonto (nicht Azure AD) erstellt werden, das die Rolle `sysadmin` besitzt. Wenn dieses Feature allgemein verfügbar ist, wird diese Einschränkung aufgehoben. Dann können Sie die erste Azure AD-Anmeldung mit dem Active Directory-Administrator für die verwaltete Instanz erstellen.
    - DacFx (exportieren/importieren) in Verbindung mit SQL Server Management Studio (SSMS) oder SqlPackage wird für Azure AD-Anmeldungen nicht unterstützt. Diese Einschränkung wird aufgehoben, wenn Azure AD-Serverprinzipale (Anmeldungen) allgemein verfügbar sind.
    - Verwenden von Azure AD-Serverprinzipalen (Anmeldungen) mit SQL Server Management Studio:

      - Das Erstellen von Azure AD-Anmeldungen per Skript, die eine beliebige authentifizierte Anmeldung verwenden, wird nicht unterstützt.
      - IntelliSense erkennt die Anweisung CREATE LOGIN FROM EXTERNAL PROVIDER nicht und zeigt eine rote Unterstreichung an.

- Nur die Prinzipalanmeldung auf Serverebene, die vom Bereitstellungsprozess der verwalteten Instanz, von Mitgliedern der Serverrollen, z.B. `securityadmin` oder `sysadmin`, oder von anderen Anmeldungen mit ALTER ANY LOGIN-Berechtigung auf Serverebene erstellt wurde, kann Azure AD-Serverprinzipale (Anmeldungen) in der Masterdatenbank für die verwaltete Instanz erstellen.
- Wenn es sich bei der Anmeldung um einen SQL-Prinzipal handelt, können nur Anmeldungen, die der Rolle `sysadmin` angehören, den Befehl „create“ verwenden, um Anmeldungen für ein Azure AD-Konto zu erstellen.
- Die Azure AD-Anmeldung muss Mitglied einer Azure AD-Instanz im selben Verzeichnis sein, das auch für die verwaltete Azure SQL-Datenbank-Instanz verwendet wird.
- Azure AD-Serverprinzipale (Anmeldungen) werden ab SQL Server Management Studio 18.0 Preview 5 im Objekt-Explorer angezeigt.
- Das Überlappen von Azure AD-Serverprinzipalen (Anmeldungen) mit einem Azure AD-Administratorkonto ist zulässig. Azure AD-Serverprinzipale (Anmeldungen) haben beim Auflösen des Prinzipals und beim Anwenden der Berechtigungen auf die verwaltete Instanz Vorrang vor Azure AD-Administratoren.
- Während der Authentifizierung wird die folgende Reihenfolge angewendet, um den authentifizierenden Prinzipal aufzulösen:

    1. Wenn das Azure AD-Konto direkt dem Azure AD-Serverprinzipal (Anmeldung) zugeordnet ist, der in „sys.server_principals“ als Typ „E“ vorhanden ist, wird der Zugriff gewährt, und die Berechtigungen des Azure AD-Serverprinzipals (Anmeldung) werden angewendet.
    2. Wenn das Azure AD-Konto Mitglied einer Azure AD-Gruppe ist, die dem Azure AD-Serverprinzipal (Anmeldung) zugeordnet ist – in „sys.server_principals“ als Typ „X“ vorhanden –, wird der Zugriff gewährt, und die Berechtigungen der Azure AD-Gruppenanmeldung werden angewendet.
    3. Wenn das Azure AD-Konto ein speziell im Portal konfigurierter Azure AD-Administrator für die verwaltete Instanz ist, die in den Systemansichten der verwalteten Instanz nicht vorhanden ist, werden spezielle feste Berechtigungen des Azure AD-Administrators für die verwaltete Instanz angewendet (Legacymodus).
    4. Wenn das Azure AD-Konto direkt einem Azure AD-Benutzer in einer Datenbank zugeordnet ist, die in „sys.database_principals“ als Typ „E“ vorhanden ist, wird der Zugriff gewährt, und die Berechtigungen des Azure AD-Datenbankbenutzers werden angewendet.
    5. Wenn das Azure AD-Konto Mitglied einer Azure AD-Gruppe ist, die einem Azure AD-Benutzer in einer Datenbank zugeordnet ist – in „sys.database_principals“ als Typ „X“ vorhanden –, wird der Zugriff gewährt, und die Berechtigungen der Azure AD-Gruppenanmeldung werden angewendet.
    6. Wenn eine Azure AD-Anmeldung einem Azure AD-Benutzerkonto oder einem Azure AD-Gruppenkonto zugeordnet ist, das zu dem authentifizierenden Benutzer aufgelöst werden kann, werden alle Berechtigungen dieser Azure AD-Anmeldung angewendet.

### <a name="service-key-and-service-master-key"></a>Dienstschlüssel und Diensthauptschlüssel

- Die [Sicherung des Hauptschlüssels](https://docs.microsoft.com/sql/t-sql/statements/backup-master-key-transact-sql) wird nicht unterstützt (wird durch den SQL-Datenbank-Dienst verwaltet).
- Die [Wiederherstellung des Hauptschlüssels](https://docs.microsoft.com/sql/t-sql/statements/restore-master-key-transact-sql) wird nicht unterstützt (wird durch den SQL-Datenbank-Dienst verwaltet).
- Die [Sicherung des Diensthauptschlüssels](https://docs.microsoft.com/sql/t-sql/statements/backup-service-master-key-transact-sql) wird nicht unterstützt (wird durch den SQL-Datenbank-Dienst verwaltet).
- Die [Wiederherstellung des Diensthauptschlüssels](https://docs.microsoft.com/sql/t-sql/statements/restore-service-master-key-transact-sql) wird nicht unterstützt (wird durch den SQL-Datenbank-Dienst verwaltet).

## <a name="configuration"></a>Konfiguration

### <a name="buffer-pool-extension"></a>Pufferpoolerweiterung

- Die [Pufferpoolerweiterung](https://docs.microsoft.com/sql/database-engine/configure-windows/buffer-pool-extension) wird nicht unterstützt.
- `ALTER SERVER CONFIGURATION SET BUFFER POOL EXTENSION` wird nicht unterstützt. Siehe [ALTER SERVER CONFIGURATION](https://docs.microsoft.com/sql/t-sql/statements/alter-server-configuration-transact-sql).

### <a name="collation"></a>Collation

Die standardmäßige Instanzsortierung ist `SQL_Latin1_General_CP1_CI_AS`, sie kann als Erstellungsparameter angegeben werden. Siehe [Sortierungen](https://docs.microsoft.com/sql/t-sql/statements/collations).

### <a name="compatibility-levels"></a>Kompatibilitätsgrade

- Folgende Kompatibilitätsgrade werden unterstützt: 100, 110, 120, 130, 140 und 150.
- Kompatibilitätsgrade unter 100 werden nicht unterstützt.
- Der standardmäßige Kompatibilitätsgrad für neue Datenbanken ist 140. Bei wiederhergestellten Datenbanken bleibt der Kompatibilitätsgrad unverändert, wenn er zuvor bei 100 und höher lag.

Siehe [ALTER DATABASE-Kompatibilitätsgrad](https://docs.microsoft.com/sql/t-sql/statements/alter-database-transact-sql-compatibility-level).

### <a name="database-mirroring"></a>Datenbankspiegelung

Die Datenbankspiegelung wird nicht unterstützt.

- Die Optionen `ALTER DATABASE SET PARTNER` und `SET WITNESS` werden nicht unterstützt.
- `CREATE ENDPOINT … FOR DATABASE_MIRRORING` wird nicht unterstützt.

Weitere Informationen finden Sie unter [ALTER DATABASE SET PARTNER und SET WITNESS](https://docs.microsoft.com/sql/t-sql/statements/alter-database-transact-sql-database-mirroring) und [CREATE ENDPOINT … FOR DATABASE_MIRRORING](https://docs.microsoft.com/sql/t-sql/statements/create-endpoint-transact-sql).

### <a name="database-options"></a>Datenbankoptionen

- Mehrere Protokolldateien werden nicht unterstützt.
- In-Memory-Objekte werden in der Dienstebene „Universell“ nicht unterstützt. 
- Es gilt ein Grenzwert von 280 Dateien pro universeller Instanz, d.h. maximal 280 Dateien pro Datenbank. Auf der Dienstebene „Universell“ zählen sowohl Datendateien als auch Protokolldateien für diesen Grenzwert. [Die Dienstebene „Unternehmenskritisch“ unterstützt 32.767 Dateien pro Datenbank](https://docs.microsoft.com/azure/sql-database/sql-database-managed-instance-resource-limits#service-tier-characteristics).
- Eine Datenbank darf keine Dateigruppen mit Filestreamdaten enthalten. Es kann keine Wiederherstellung durchgeführt werden, wenn die BAK-Datei `FILESTREAM`-Daten enthält. 
- Jede Datei wird in Azure Blob Storage gespeichert. E/A und Durchsatz pro Datei hängen von der Größe der jeweiligen Datei ab.

#### <a name="create-database-statement"></a>CREATE DATABASE-Anweisung

Für `CREATE DATABASE` gelten die folgenden Einschränkungen:

- Dateien und Dateigruppen können nicht definiert werden. 
- Die `CONTAINMENT`-Option wird nicht unterstützt. 
- `WITH`-Optionen werden nicht unterstützt. 
   > [!TIP]
   > Als Problemumgehung können Sie `ALTER DATABASE` nach `CREATE DATABASE` verwenden, um Datenbankoptionen zum Hinzufügen von Dateien oder Festlegen der Eigenständigkeit festzulegen. 

- Die `FOR ATTACH`-Option wird nicht unterstützt.
- Die `AS SNAPSHOT OF`-Option wird nicht unterstützt.

Weitere Informationen finden Sie unter [CREATE DATABASE](https://docs.microsoft.com/sql/t-sql/statements/create-database-sql-server-transact-sql).

#### <a name="alter-database-statement"></a>ALTER DATABASE-Anweisung

Einige Dateieigenschaften können nicht festgelegt oder geändert werden:

- Der Dateipfad kann nicht in der T-SQL-Anweisung `ALTER DATABASE ADD FILE (FILENAME='path')` angegeben werden. Entfernen Sie `FILENAME` aus dem Skript, da die verwaltete Instanz die Dateien automatisch speichert. 
- Der Dateiname kann nicht mithilfe der Anweisung `ALTER DATABASE` geändert werden.

Die folgenden Optionen werden standardmäßig festgelegt und können nicht geändert werden:

- `MULTI_USER`
- `ENABLE_BROKER ON`
- `AUTO_CLOSE OFF`

Die folgenden Optionen können nicht geändert werden:

- `AUTO_CLOSE`
- `AUTOMATIC_TUNING(CREATE_INDEX=ON|OFF)`
- `AUTOMATIC_TUNING(DROP_INDEX=ON|OFF)`
- `DISABLE_BROKER`
- `EMERGENCY`
- `ENABLE_BROKER`
- `FILESTREAM`
- `HADR`
- `NEW_BROKER`
- `OFFLINE`
- `PAGE_VERIFY`
- `PARTNER`
- `READ_ONLY`
- `RECOVERY BULK_LOGGED`
- `RECOVERY_SIMPLE`
- `REMOTE_DATA_ARCHIVE` 
- `RESTRICTED_USER`
- `SINGLE_USER`
- `WITNESS`

Weitere Informationen finden Sie unter [ALTER DATABASE](https://docs.microsoft.com/sql/t-sql/statements/alter-database-transact-sql-file-and-filegroup-options).

### <a name="sql-server-agent"></a>SQL Server-Agent

- Das Aktivieren und Deaktivieren von SQL Server-Agent wird derzeit in verwalteten Instanzen nicht unterstützt. Der SQL-Agent wird kontinuierlich ausgeführt.
- SQL Server-Agent-Einstellungen sind schreibgeschützt. Die Prozedur `sp_set_agent_properties` wird in einer verwalteten Instanz nicht unterstützt. 
- Aufträge
  - T-SQL-Auftragsschritte werden unterstützt.
  - Die folgenden Replikationsaufträge werden unterstützt:
    - Transaktionsprotokollleser
    - Momentaufnahme
    - Verteiler
  - SSIS-Auftragsschritte werden unterstützt.
  - Andere Arten von Auftragsschritten werden derzeit nicht unterstützt:
    - Der Auftragsschritt für die Mergereplikation wird nicht unterstützt. 
    - Der Warteschlangenleser wird nicht unterstützt. 
    - Die Befehlsshell wird noch nicht unterstützt.
  - Verwaltete Instanzen können nicht auf externe Ressourcen zugreifen – z.B. auf Netzwerkfreigaben über „robocopy“. 
  - SQL Server Analysis Services werden nicht unterstützt.
- Benachrichtigungen werden teilweise unterstützt.
- Die E-Mail-Benachrichtigung wird unterstützt. Dazu muss allerdings ein Datenbank-E-Mail-Profil konfiguriert werden. Der SQL Server-Agent kann nur ein Datenbank-E-Mail-Profil verwenden, für das der Name `AzureManagedInstance_dbmail_profile` angegeben werden muss. 
  - Pager wird nicht unterstützt.
  - NetSend wird nicht unterstützt.
  - Warnungen werden noch nicht unterstützt.
  - Proxys werden nicht unterstützt.
- EventLog wird nicht unterstützt.

Die folgenden SQL-Agent-Funktionen werden derzeit nicht unterstützt:

- Proxys
- Planen von Aufträgen für eine CPU im Leerlauf
- Aktivieren oder Deaktivieren eines Agents
- Alerts

Weitere Informationen zum SQL Server-Agent finden Sie unter [SQL Server-Agent](https://docs.microsoft.com/sql/ssms/agent/sql-server-agent).

### <a name="tables"></a>Tabellen

Die folgenden Tabellentypen werden nicht unterstützt:

- [FILESTREAM](https://docs.microsoft.com/sql/relational-databases/blob/filestream-sql-server)
- [FILETABLE](https://docs.microsoft.com/sql/relational-databases/blob/filetables-sql-server)
- [EXTERNAL TABLE](https://docs.microsoft.com/sql/t-sql/statements/create-external-table-transact-sql) (Polybase)
- [MEMORY_OPTIMIZED](https://docs.microsoft.com/sql/relational-databases/in-memory-oltp/introduction-to-memory-optimized-tables) (wird nur im universellen Tarif unterstützt)

Informationen zum Erstellen und Ändern von Tabellen finden Sie unter [CREATE TABLE](https://docs.microsoft.com/sql/t-sql/statements/create-table-transact-sql) und [ALTER TABLE](https://docs.microsoft.com/sql/t-sql/statements/alter-table-transact-sql).

## <a name="functionalities"></a>Funktionen

### <a name="bulk-insert--openrowset"></a>Bulk insert/OPENROWSET

Eine verwaltete Instanz kann nicht auf Dateifreigaben und Windows-Ordner zugreifen. Daher müssen die Dateien aus Azure Blob Storage importiert werden:

- `DATASOURCE` ist beim Importieren von Dateien aus Azure Blob Storage im Befehl `BULK INSERT` erforderlich. Siehe [BULK INSERT](https://docs.microsoft.com/sql/t-sql/statements/bulk-insert-transact-sql).
- `DATASOURCE` ist beim Lesen von Inhalten einer Datei aus Azure Blob Storage in der Funktion `OPENROWSET` erforderlich. Siehe [OPENROWSET](https://docs.microsoft.com/sql/t-sql/functions/openrowset-transact-sql).
- `OPENROWSET` kann zum Lesen von Daten aus anderen Azure SQL-Einzeldatenbanken, verwalteten Instanzen oder SQL Server-Instanzen verwendet werden. Andere Quellen wie Oracle-Datenbanken oder Excel-Dateien werden nicht unterstützt.

### <a name="clr"></a>CLR

Eine verwaltete Instanz kann nicht auf Dateifreigaben und Windows-Ordner zugreifen. Daher gelten folgende Einschränkungen:

- Nur `CREATE ASSEMBLY FROM BINARY` wird unterstützt. Siehe [CREATE ASSEMBLY FROM BINARY](https://docs.microsoft.com/sql/t-sql/statements/create-assembly-transact-sql). 
- `CREATE ASSEMBLY FROM FILE` wird nicht unterstützt. Siehe [CREATE ASSEMBLY FROM FILE](https://docs.microsoft.com/sql/t-sql/statements/create-assembly-transact-sql).
- `ALTER ASSEMBLY` kann nicht auf Dateien verweisen. Siehe [ALTER ASSEMBLY](https://docs.microsoft.com/sql/t-sql/statements/alter-assembly-transact-sql).

### <a name="database-mail-db_mail"></a>Datenbank-E-Mail – (db_mail)
 - `sp_send_dbmail` kann keine Anlagen mithilfe des Parameters @file_attachments senden. Aus dieser Prozedur kann auf das lokale Dateisystem und externe Freigaben oder Azure Blob Storage nicht zugegriffen werden.
 - Informieren Sie sich zu den bekannten Problemen im Zusammenhang mit dem Parameter `@query` und Authentifizierung.
 
### <a name="dbcc"></a>DBCC

Nicht dokumentierte DBCC-Anweisungen, die in SQL Server aktiviert sind, werden in verwalteten Instanzen nicht unterstützt.

- Es wird nur eine begrenzte Anzahl von globalen Ablaufverfolgungsflags unterstützt. `Trace flags` auf Sitzungsebene werden nicht unterstützt. Siehe [Ablaufverfolgungsflags](https://docs.microsoft.com/sql/t-sql/database-console-commands/dbcc-traceon-trace-flags-transact-sql).
- [DBCC TRACEOFF](https://docs.microsoft.com/sql/t-sql/database-console-commands/dbcc-traceoff-transact-sql) und [DBCC TRACEON](https://docs.microsoft.com/sql/t-sql/database-console-commands/dbcc-traceon-transact-sql) funktionieren mit einer begrenzten Anzahl globaler Ablaufverfolgungsflags.
- [DBCC CHECKDB](https://docs.microsoft.com/sql/t-sql/database-console-commands/dbcc-checkdb-transact-sql) mit den Optionen REPAIR_ALLOW_DATA_LOSS, REPAIR_FAST und REPAIR_REBUILD kann nicht verwendet werden, weil die Datenbank im Modus `SINGLE_USER` nicht festgelegt werden kann. Siehe [ALTER DATABASE-Unterschiede.](#alter-database-statement) Potenzielle Datenbankbeschädigungen werden vom Azure-Supportteam behandelt. Wenden Sie sich an den Azure-Support, wenn Sie bemerken, dass eine Datenbankbeschädigung vorliegt.

### <a name="distributed-transactions"></a>Verteilte Transaktionen

MS DTC und [elastische Transaktionen](sql-database-elastic-transactions-overview.md) werden in verwalteten Instanzen derzeit nicht unterstützt.

### <a name="extended-events"></a>Erweiterte Ereignisse

Einige Windows-spezifische Ziele für XEvents (Erweiterte Ereignisse) werden nicht unterstützt:

- Das Ziel `etw_classic_sync` wird nicht unterstützt. Speichern Sie `.xel`-Dateien in Azure Blob Storage. Siehe [etw_classic_sync-Ziel](https://docs.microsoft.com/sql/relational-databases/extended-events/targets-for-extended-events-in-sql-server#etw_classic_sync_target-target).
- Das Ziel `event_file` wird nicht unterstützt. Speichern Sie `.xel`-Dateien in Azure Blob Storage. Siehe [event_file-Ziel](https://docs.microsoft.com/sql/relational-databases/extended-events/targets-for-extended-events-in-sql-server#event_file-target).

### <a name="external-libraries"></a>Externe Bibliotheken

Datenbankinterne R- und externe Python-Bibliotheken werden noch nicht unterstützt. Siehe [SQL Server Machine Learning Services](https://docs.microsoft.com/sql/advanced-analytics/r/sql-server-r-services).

### <a name="filestream-and-filetable"></a>Filestream und Dateitabelle

- Filestreamdaten werden nicht unterstützt.
- Die Datenbank darf keine Dateigruppen mit `FILESTREAM`-Daten enthalten.
- `FILETABLE` wird nicht unterstützt.
- Tabellen dürfen keine `FILESTREAM`-Typen enthalten.
- Die folgenden Funktionen werden nicht unterstützt:
  - `GetPathLocator()`
  - `GET_FILESTREAM_TRANSACTION_CONTEXT()`
  - `PathName()`
  - `GetFileNamespacePat)`
  - `FileTableRootPath()`

Weitere Informationen finden Sie unter [FILESTREAM](https://docs.microsoft.com/sql/relational-databases/blob/filestream-sql-server) und [FileTables](https://docs.microsoft.com/sql/relational-databases/blob/filetables-sql-server).

### <a name="full-text-semantic-search"></a>Semantische Volltextsuche

Die [semantische Suche](https://docs.microsoft.com/sql/relational-databases/search/semantic-search-sql-server) wird nicht unterstützt.

### <a name="linked-servers"></a>Verknüpfte Server

Verbindungsserver in einer verwalteten Instanz unterstützen eine begrenzte Anzahl von Zielen:

- Unterstützte Ziele sind verwaltete Instanzen, einzelne Datenbanken und SQL Server-Instanzen. 
- Verbindungsserver unterstützen keine verteilten beschreibbaren Transaktionen (MS DTC).
- Nicht unterstützte Ziele sind Dateien, Analysis Services und andere RDBMS. Verwenden Sie einen nativen CSV-Import aus Azure Blob Storage mithilfe von `BULK INSERT` oder `OPENROWSET` als Alternative zum Dateiimport.

Vorgänge

- Instanzübergreifende Schreibtransaktionen werden nicht unterstützt.
- `sp_dropserver` wird zum Löschen eines Verbindungsservers unterstützt. Siehe [sp_dropserver](https://docs.microsoft.com/sql/relational-databases/system-stored-procedures/sp-dropserver-transact-sql).
- Die `OPENROWSET`-Funktion kann verwendet werden, um Abfragen nur auf SQL Server-Instanzen auszuführen. Diese Instanzen können verwaltet sein oder sich auf lokalen oder virtuellen Computern befinden. Siehe [OPENROWSET](https://docs.microsoft.com/sql/t-sql/functions/openrowset-transact-sql).
- Die `OPENDATASOURCE`-Funktion kann verwendet werden, um Abfragen nur auf SQL Server-Instanzen auszuführen. Diese Instanzen können verwaltet sein oder sich auf lokalen oder virtuellen Computern befinden. Als Anbieter werden nur die Werte `SQLNCLI`, `SQLNCLI11` und `SQLOLEDB` unterstützt. Ein Beispiel ist `SELECT * FROM OPENDATASOURCE('SQLNCLI', '...').AdventureWorks2012.HumanResources.Employee`. Siehe [OPENDATASOURCE](https://docs.microsoft.com/sql/t-sql/functions/opendatasource-transact-sql).
- Verbindungsserver können nicht zum Lesen von Dateien (Excel, CSV) aus den Netzwerkfreigaben verwendet werden. Versuchen Sie es mit [BULK INSERT](https://docs.microsoft.com/sql/t-sql/statements/bulk-insert-transact-sql#e-importing-data-from-a-csv-file) oder [OPENROWSET](https://docs.microsoft.com/sql/t-sql/functions/openrowset-transact-sql#g-accessing-data-from-a-csv-file-with-a-format-file), welche CSV-Dateien aus Azure Blob Storage lesen. Verfolgen Sie diese Anforderungen im [Feedback zu verwalteten Instanzen](https://feedback.azure.com/forums/915676-sql-managed-instance/suggestions/35657887-linked-server-to-non-sql-sources)|.

### <a name="polybase"></a>PolyBase

Externe Tabellen, die auf Dateien in HDFS oder Azure Blob Storage verweisen, werden nicht unterstützt. Informationen zu PolyBase finden Sie unter [PolyBase](https://docs.microsoft.com/sql/relational-databases/polybase/polybase-guide).

### <a name="replication"></a>Replikation

- Momentaufnahmen und bidirektionale Replikationstypen werden unterstützt. Mergereplikation, Peer-zu-Peer-Replikation und aktualisierbare Abonnements werden nicht unterstützt.
- Die [Transaktionsreplikation](sql-database-managed-instance-transactional-replication.md) ist mit einigen Einschränkungen für die öffentliche Vorschauversion der verwalteten Instanz verfügbar:
    - Alle Replikationsteilnehmertypen (Herausgeber, Verteiler, Pullabonnent und Pushabonnent) können auf der verwalteten Instanz platziert werden. Der Herausgeber und der Verteiler müssen dabei entweder beide in der Cloud oder beide lokal platziert sein.
    - Verwaltete Instanzen können mit der neuesten SQL Server-Version kommunizieren. Die unterstützten Versionen finden Sie [hier](sql-database-managed-instance-transactional-replication.md#supportability-matrix-for-instance-databases-and-on-premises-systems).
    - Für die Transaktionsreplikation gibt es einige [zusätzliche Netzwerkanforderungen](sql-database-managed-instance-transactional-replication.md#requirements).

Weitere Informationen zum Konfigurieren der Replikation finden Sie im [Replikationstutorial](replication-with-sql-database-managed-instance.md).


Wenn die Replikation für eine Datenbank in einer [Failovergruppe](sql-database-auto-failover-group.md) aktiviert ist, muss der Administrator der verwalteten Instanz alle Veröffentlichungen für die alte primäre Instanz bereinigen und nach einem Failover für die neue primäre Instanz erneut konfigurieren. Die folgenden Aktivitäten sind in diesem Szenario erforderlich:

1. Beenden aller Replikationsaufträge, die für die Datenbank ausgeführt werden, sofern vorhanden.
2. Löschen der Abonnementmetadaten vom Herausgeber, indem das folgende Skript für die Herausgeberdatenbank ausgeführt wird:

   ```sql
   EXEC sp_dropsubscription @publication='<name of publication>', @article='all',@subscriber='<name of subscriber>'
   ```             
 
1. Löschen von Abonnementmetadaten aus dem Abonnenten. Führen Sie das folgende Skript in der Abonnementdatenbank für die Abonnenteninstanz aus:

   ```sql
   EXEC sp_subscription_cleanup
      @publisher = N'<full DNS of publisher, e.g. example.ac2d23028af5.database.windows.net>', 
      @publisher_db = N'<publisher database>', 
      @publication = N'<name of publication>'; 
   ```                

1. Erzwingen Sie das Löschen aller Replikationsobjekte aus dem Herausgeber, indem Sie das folgende Skript in der veröffentlichten Datenbank ausführen:

   ```sql
   EXEC sp_removedbreplication
   ```

1. Erzwingen Sie das Löschen des alten Verteilers aus der ursprünglichen primären Instanz (bei einem Failback auf eine alte primäre Instanz, für die ein Verteiler verwendet wurde). Führen Sie das folgende Skript für die Masterdatenbank in der alten verwalteten Verteilerinstanz aus:

   ```sql
   EXEC sp_dropdistributor 1,1
   ```

### <a name="restore-statement"></a>RESTORE-Anweisung 

- Unterstützte Syntax:
  - `RESTORE DATABASE`
  - `RESTORE FILELISTONLY ONLY`
  - `RESTORE HEADER ONLY`
  - `RESTORE LABELONLY ONLY`
  - `RESTORE VERIFYONLY ONLY`
- Nicht unterstützte Syntax:
  - `RESTORE LOG ONLY`
  - `RESTORE REWINDONLY ONLY`
- Quelle: 
  - `FROM URL` (Azure Blob Storage) ist die einzige unterstützte Option.
  - `FROM DISK`/`TAPE`/Sicherungsmedium wird nicht unterstützt.
  - Sicherungssätze werden nicht unterstützt.
- `WITH`-Optionen wie `DIFFERENTIAL` oder `STATS` werden nicht unterstützt.
- `ASYNC RESTORE`: Die Wiederherstellung wird auch bei einer Unterbrechung der Clientverbindung fortgesetzt. Wenn die Verbindung ausfällt, können Sie sich in der Ansicht `sys.dm_operation_status` über den Status eines Wiederherstellungsvorgangs sowie über CREATE- und DROP-Vorgänge für die Datenbank informieren. Siehe [sys.dm_operation_status](https://docs.microsoft.com/sql/relational-databases/system-dynamic-management-views/sys-dm-operation-status-azure-sql-database). 

Die folgenden Datenbankoptionen werden festgelegt oder überschrieben und können später nicht geändert werden: 

- `NEW_BROKER`, wenn der Broker in der BAK-Datei nicht aktiviert ist. 
- `ENABLE_BROKER`, wenn der Broker in der BAK-Datei nicht aktiviert ist. 
- `AUTO_CLOSE=OFF`, wenn eine Datenbank in der BAK-Datei `AUTO_CLOSE=ON` enthält. 
- `RECOVERY FULL`, wenn eine Datenbank in der BAK-Datei den Wiederherstellungsmodus `SIMPLE` oder `BULK_LOGGED` enthält.
- Eine speicheroptimierte Dateigruppe wird hinzugefügt und erhält den Namen „XTP“, wenn sie in der BAK-Quelldatei nicht vorhanden war. 
- Alle vorhandenen speicheroptimierten Dateigruppen werden in „XTP“ umbenannt. 
- Die Optionen `SINGLE_USER` und `RESTRICTED_USER` werden zu `MULTI_USER` konvertiert.

Einschränkungen: 

- Sicherungen der beschädigten Datenbanken werden je nach Art der Beschädigung möglicherweise wiederhergestellt, automatisierte Sicherungen werden jedoch erst ausgeführt, nachdem die Beschädigung behoben wurde. Stellen Sie sicher, dass Sie `DBCC CHECKDB` für die Quellinstanz ausführen und für die Sicherung `WITH CHECKSUM` verwenden, um dieses Problem zu vermeiden.
- Die Wiederherstellung der `.BAK`-Datei einer Datenbank, die eine in diesem Dokument beschriebene Einschränkung enthält (z.B. `FILESTREAM`- oder `FILETABLE`-Objekte), kann in der verwalteten Instanz nicht wiederhergestellt werden.
- `.BAK`-Dateien mit mehreren Sicherungssätzen können nicht wiederhergestellt werden. 
- `.BAK`-Dateien mit mehreren Protokolldateien können nicht wiederhergestellt werden.
- Sicherungen, die Datenbanken von mehr als 8 TB, aktive In-Memory-OLTP-Objekte oder eine Reihe von Dateien enthalten, die das Limit von 280 Dateien pro Instanz überschreiten würden, können auf einer universellen Instanz nicht wiederhergestellt werden. 
- Sicherungen, die Datenbanken mit mehr als 4 TB oder In-Memory-OLTP-Objekte enthalten, deren Gesamtgröße größer ist als die unter [Ressourcenlimits](sql-database-managed-instance-resource-limits.md) beschriebene Größe, können auf der unternehmenskritischen Instanz nicht wiederhergestellt werden.
Weitere Informationen zu Anweisungen für Wiederherstellungen finden Sie unter [RESTORE-Anweisungen](https://docs.microsoft.com/sql/t-sql/statements/restore-statements-transact-sql).

 > [!IMPORTANT]
 > Die gleichen Einschränkungen gelten für den integrierten Vorgang der Point-in-Time-Wiederherstellung. So kann beispielsweise eine universelle Datenbank, die größer als 4 TB ist, auf einer unternehmenskritischen Instanz nicht wiederhergestellt werden. Eine unternehmenskritische Datenbank mit In-Memory-OLTP-Dateien oder mehr als 280 Dateien kann auf einer universellen Instanz nicht wiederhergestellt werden.

### <a name="service-broker"></a>Service Broker

Der instanzübergreifende Service Broker wird nicht unterstützt:

- `sys.routes`: Als Voraussetzung müssen Sie die Adresse aus „sys.routes“ auswählen. Die Adresse muss für jede Route auf LOCAL festgelegt sein. Siehe [sys.routes](https://docs.microsoft.com/sql/relational-databases/system-catalog-views/sys-routes-transact-sql).
- `CREATE ROUTE`: Sie können `CREATE ROUTE` nicht mit einer anderen `ADDRESS` als `LOCAL` verwenden. Siehe [CREATE ROUTE](https://docs.microsoft.com/sql/t-sql/statements/create-route-transact-sql).
- `ALTER ROUTE`: Sie können `ALTER ROUTE` nicht mit einer anderen `ADDRESS` als `LOCAL` verwenden. Siehe [ALTER ROUTE](https://docs.microsoft.com/sql/t-sql/statements/alter-route-transact-sql). 

### <a name="stored-procedures-functions-and-triggers"></a>Gespeicherte Prozeduren, Funktionen und Trigger

- `NATIVE_COMPILATION` wird in der Dienstebene „Universell“ nicht unterstützt.
- Die folgenden [sp_configure](https://docs.microsoft.com/sql/relational-databases/system-stored-procedures/sp-configure-transact-sql)-Optionen werden nicht unterstützt: 
  - `allow polybase export`
  - `allow updates`
  - `filestream_access_level`
  - `remote data archive`
  - `remote proc trans`
- `sp_execute_external_scripts` wird nicht unterstützt. Siehe [sp_execute_external_scripts](https://docs.microsoft.com/sql/relational-databases/system-stored-procedures/sp-execute-external-script-transact-sql#examples).
- `xp_cmdshell` wird nicht unterstützt. Siehe [xp_cmdshell](https://docs.microsoft.com/sql/relational-databases/system-stored-procedures/xp-cmdshell-transact-sql).
- `Extended stored procedures` werden nicht unterstützt. Hierzu gehören `sp_addextendedproc` und `sp_dropextendedproc`. Siehe [Erweiterte gespeicherte Prozeduren](https://docs.microsoft.com/sql/relational-databases/system-stored-procedures/general-extended-stored-procedures-transact-sql).
- `sp_attach_db`, `sp_attach_single_file_db` und `sp_detach_db` werden nicht unterstützt. Siehe [sp_attach_db](https://docs.microsoft.com/sql/relational-databases/system-stored-procedures/sp-attach-db-transact-sql), [sp_attach_single_file_db](https://docs.microsoft.com/sql/relational-databases/system-stored-procedures/sp-attach-single-file-db-transact-sql) und [sp_detach_db](https://docs.microsoft.com/sql/relational-databases/system-stored-procedures/sp-detach-db-transact-sql).

### <a name="system-functions-and-variables"></a>Systemfunktionen und Variablen

Die folgenden Variablen, Funktionen und Sichten geben abweichende Ergebnisse zurück:

- `SERVERPROPERTY('EngineEdition')` gibt den Wert 8 zurück. Durch diese Eigenschaft wird eine verwaltete Instanz eindeutig identifiziert. Siehe [SERVERPROPERTY](https://docs.microsoft.com/sql/t-sql/functions/serverproperty-transact-sql).
- `SERVERPROPERTY('InstanceName')` gibt NULL zurück, weil das für SQL Server bestehende Konzept der Instanz für verwaltete Instanzen nicht gilt. Siehe [SERVERPROPERTY('InstanceName')](https://docs.microsoft.com/sql/t-sql/functions/serverproperty-transact-sql).
- `@@SERVERNAME` gibt den vollständigen „verbindungsfähigen“ DNS-Namen zurück, z.B. „meine-verwaltete-instanz.wcus17662feb9ce98.database.windows.net“. Siehe [@@SERVERNAME](https://docs.microsoft.com/sql/t-sql/functions/servername-transact-sql). 
- `SYS.SERVERS` gibt den vollständigen „verbindungsfähigen“ DNS-Namen zurück, z.B. `myinstance.domain.database.windows.net` für die Eigenschaften „name“ und „data_source“. Siehe [SYS.SERVERS](https://docs.microsoft.com/sql/relational-databases/system-catalog-views/sys-servers-transact-sql).
- `@@SERVICENAME` gibt NULL zurück, weil das für SQL Server bestehende Konzept des Diensts für verwaltete Instanzen nicht gilt. Siehe [@@SERVICENAME](https://docs.microsoft.com/sql/t-sql/functions/servicename-transact-sql).
- `SUSER_ID` wird unterstützt. Gibt NULL zurück, wenn die Azure AD-Anmeldung in „sys.syslogins“ nicht vorhanden ist. Siehe [SUSER_ID](https://docs.microsoft.com/sql/t-sql/functions/suser-id-transact-sql). 
- `SUSER_SID` wird nicht unterstützt. Es werden falsche Daten zurückgegeben. Dies ist ein bekanntes vorübergehendes Problem. Siehe [SUSER_SID](https://docs.microsoft.com/sql/t-sql/functions/suser-sid-transact-sql). 

## <a name="Environment"></a>Umgebungseinschränkungen

### <a name="subnet"></a>Subnet
-  Sie können keine anderen Ressourcen (z. B. virtuelle Computer) in dem Subnetz platzieren, in dem Sie Ihre verwaltete Instanz bereitgestellt haben. Stellen Sie diese Ressourcen in einem anderen Subnetz bereit.
- Das Subnetz muss eine ausreichende Anzahl verfügbarer [IP-Adressen](sql-database-managed-instance-connectivity-architecture.md#network-requirements) aufweisen. Der Mindestwert ist 16, wir empfehlen jedoch, mindestens 32 IP-Adressen im Subnetz vorzusehen.
- [Dem Subnetz der verwalteten Instanz können keine Dienstendpunkte zugeordnet werden](sql-database-managed-instance-connectivity-architecture.md#network-requirements). Wenn Sie das virtuelle Netzwerk erstellen, überprüfen Sie, ob die Option „Dienstendpunkte“ auf „Deaktiviert“ festgelegt ist.
- Für die Anzahl von virtuellen Kernen und die Typen der Instanzen, die Sie in einer Region platzieren können, gibt es einige [Einschränkungen und Grenzwerte](sql-database-managed-instance-resource-limits.md#regional-resource-limitations).
- Es gibt eine Reihe von [Sicherheitsregeln, die auf das Subnetz angewendet werden müssen](sql-database-managed-instance-connectivity-architecture.md#network-requirements).

### <a name="vnet"></a>VNET
- VNet kann mithilfe des Ressourcenmodells bereitgestellt werden – das klassische Modell wird für VNets nicht unterstützt.
- Nachdem eine verwaltete Instanz erstellt wurde, wird das Verschieben dieser Instanz oder des VNET in eine andere Ressourcengruppe oder ein anderes Abonnement nicht unterstützt.
- Einige Dienste, wie App Service-Umgebungen, Logik-Apps und verwaltete Instanzen (die für Georeplikation, Transaktionsreplikation oder über Verbindungsserver verwendet werden) können nicht auf verwaltete Instanzen in anderen Regionen zugreifen, wenn ihre VNETs mithilfe von [globalem Peering](../virtual-network/virtual-networks-faq.md#what-are-the-constraints-related-to-global-vnet-peering-and-load-balancers) verbunden sind. Sie können sich mit diesen Ressourcen über ExpressRoute oder VNET-zu-VNET über VNet-Gateways verbinden.

### <a name="tempdb"></a>TEMPDB

Die maximale Dateigröße von `tempdb` darf in der Dienstebene „Universell“ 24 GB pro Kern nicht überschreiten. Die maximale Größe von `tempdb` ist in der Dienstebene „Unternehmenskritisch“ auf die Speichergröße der Instanz begrenzt. Die Größe der Protokolldatei `Tempdb` ist für die Ebenen „Universell“ und „Unternehmenskritisch“ auf 120 GB beschränkt. Einige Abfragen geben möglicherweise einen Fehler zurück, wenn für sie mehr als 24 GB pro Kern in `tempdb` erforderlich sind oder die erstellten Protokolldaten mehr als 120 GB benötigen.

### <a name="error-logs"></a>Fehlerprotokolle

Eine verwaltete Instanz stellt ausführliche Informationen in Fehlerprotokollen zur Verfügung. Es gibt viele interne Systemereignisse, die im Fehlerprotokoll protokolliert werden. Verwenden Sie zum Lesen von Fehlerprotokollen eine benutzerdefinierte Prozedur, die einige nicht relevante Einträge herausfiltert. Weitere Informationen finden Sie unter [Verwaltete Instanz – sp_readmierrorlog](https://blogs.msdn.microsoft.com/sqlcat/2018/05/04/azure-sql-db-managed-instance-sp_readmierrorlog/).

## <a name="Issues"></a> Bekannte Probleme

### <a name="wrong-error-returned-while-trying-to-remove-a-file-that-is-not-empty"></a>Zurückgegebener Fehler bei dem Versuch, eine nicht leere Datei zu entfernen

**Datum:** Oktober 2019

SQL Server/Verwaltete Instanz [erlaubt Benutzern nicht das Löschen von Dateien, die nicht leer sind](https://docs.microsoft.com/sql/relational-databases/databases/delete-data-or-log-files-from-a-database#Prerequisites). Wenn Sie versuchen, eine nicht leere Datendatei mithilfe der `ALTER DATABASE REMOVE FILE`-Anweisung zu entfernen, wird der Fehler `Msg 5042 – The file '<file_name>' cannot be removed because it is not empty` nicht sofort zurückgegeben. Die verwaltete Instanz versucht fortgesetzt, die Datei zu löschen, und der Vorgang schlägt nach 30 Minuten mit `Internal server error` fehl.

**Problemumgehung**: Entfernen Sie den Inhalt der Datei mit dem `DBCC SHRINKFILE (N'<file_name>', EMPTYFILE)`-Befehl. Wenn es sich um die einzige Datei in der Dateigruppe handelt, müssten Sie Daten aus der Tabelle oder Partition löschen, die dieser Dateigruppe zugeordnet ist, bevor Sie die Datei verkleinern, und diese Daten optional in eine andere Tabelle/Partition laden.

### <a name="change-service-tier-and-create-instance-operations-are-blocked-by-ongoing-database-restore"></a>Das Ändern der Dienstebene und Erstellen von Instanzvorgängen wird durch die laufende Datenbankwiederherstellung blockiert

**Datum:** September 2019

Durch die laufende `RESTORE`-Anweisung blockieren der Migrationsprozess des Datenmigrationsdiensts und die integrierte Point-in-Time-Wiederherstellung das Aktualisieren der Dienstebene oder eine Größenänderung bei der vorhandenen Instanz sowie das Erstellen neuer Instanzen so lange, bis der Wiederherstellungsvorgang abgeschlossen ist. Der Wiederherstellungsvorgang blockiert diese Vorgänge für die verwalteten Instanzen und Instanzenpools in demselben Subnetz, in dem der Wiederherstellungsvorgang ausgeführt wird. Die Instanzen in Instanzenpools sind davon nicht betroffen. Die Vorgänge des Erstellens oder Änderns von Dienstebenen führen nicht zu einem Fehler oder Timeout. Sie werden nach dem Abschluss oder einem Abbrechen des Wiederherstellungsvorgangs fortgesetzt.

**Problemumgehung**: Warten Sie, bis der Wiederherstellungsvorgang abgeschlossen ist, oder brechen Sie ihn ab, wenn der Vorgang des Erstellens oder Aktualisierens der Dienstebene höhere Priorität hat.

### <a name="missing-validations-in-restore-process"></a>Fehlende Überprüfungen im Wiederherstellungsprozess

**Datum:** September 2019

Die Anweisung `RESTORE` und die integrierte Point-in-Time-Wiederherstellung führen einige erforderliche Überprüfungen in der wiederhergestellten Datenbank nicht aus:
- Die Anweisung **DBCC CHECKDB** - `RESTORE` führt `DBCC CHECKDB` in der wiederhergestellten Datenbank nicht aus. Wenn eine ursprüngliche Datenbank beschädigt wird oder wenn die Sicherungsdatei während ihres Kopierens in Azure Blob Storage beschädigt wird, werden keine automatischen Sicherungen erstellt, und der Azure-Support kontaktiert den Kunden. 
- Der integrierte Vorgang der Point-in-Time-Wiederherstellung überprüft nicht, ob die automatisierte Sicherung aus der unternehmenskritischen Instanz die [In-Memory-OLTP-Objekte](sql-database-in-memory.md#in-memory-oltp) enthält. 

**Problemumgehung**: Stellen Sie sicher, dass Sie `DBCC CHECKDB` in der Quelldatenbank ausführen, bevor Sie eine Sicherung erstellen, und vermeiden Sie mithilfe der Option `WITH CHECKSUM` bei der Sicherung mögliche Beschädigungen, die auf einer verwalteten Instanz wiederhergestellt werden könnten. Stellen Sie sicher, dass Ihre Quelldatenbank keine [In-Memory-OLTP-Objekte](sql-database-in-memory.md#in-memory-oltp) enthält, wenn Sie sie auf der Dienstebene „Universell“ wiederherstellen.

### <a name="resource-governor-on-business-critical-service-tier-might-need-to-be-reconfigured-after-failover"></a>Resource Governor auf Dienstebene „Unternehmenskritisch“ muss möglicherweise nach einem Failover neu konfiguriert werden

**Datum:** September 2019

Die Funktion [Resource Governor](https://docs.microsoft.com/sql/relational-databases/resource-governor/resource-governor), die Ihnen ermöglicht, die der Benutzerworkload zugewiesenen Ressourcen einzuschränken, könnte eine andere Benutzerworkload nach einem Failover oder einer vom Benutzer initiierten Änderung der Dienstebene (z. B. Änderung der maximalen virtuellen Kerne oder maximalen Instanzspeichergröße) falsch klassifizieren.

**Problemumgehung**: Führen Sie `ALTER RESOURCE GOVERNOR RECONFIGURE` regelmäßig oder als Teil des SQL Agent-Auftrags aus, der den SQL-Task ausführt, wenn die Instanz gestartet wird, wenn Sie [Resource Governor](https://docs.microsoft.com/sql/relational-databases/resource-governor/resource-governor) verwenden.

### <a name="cannot-authenticate-to-external-mail-servers-using-secure-connection-ssl"></a>Authentifizierung bei externen E-Mail-Servern mit sicherer Verbindung (SSL) nicht möglich

**Datum:** August 2019

Datenbank-E-Mail, die [mit sicherer Verbindung (Secure Connection, SSL) konfiguriert wurde](https://docs.microsoft.com/sql/relational-databases/database-mail/configure-database-mail), kann sich bei einigen E-Mail-Servern außerhalb von Azure nicht authentifizieren. Dies ist ein Sicherheitskonfigurationsproblem, das bald behoben sein wird.

**Problemumgehung:** Entfernen Sie die sichere Verbindung (SSL) vorübergehend aus der Datenbank-E-Mail-Konfiguration, bis das Problem behoben wurde. 

### <a name="cross-database-service-broker-dialogs-must-be-re-initialized-after-service-tier-upgrade"></a>Datenbankübergreifende Service Broker-Dialoge müssen nach dem Upgrade der Dienstebene erneut initialisiert werden.

**Datum:** August 2019

Datenbankübergreifenden Service Broker-Dialoge stellen die Zustellung der Nachrichten an die Dienste in anderen Datenbanken nach Änderung der Dienstebene ein. Die Nachrichten sind **nicht verloren**, sondern befinden sich in der Absenderwarteschlange. Jegliche Änderung virtueller Kerne oder der Instanzspeichergröße in der verwalteten Instanz führt dazu, dass der Wert `service_broke_guid` in der Sicht [sys.databases](https://docs.microsoft.com/sql/relational-databases/system-catalog-views/sys-databases-transact-sql) für alle Datenbanken geändert wird. Ein `DIALOG`, der mit der Anweisung [BEGIN DIALOG](https://docs.microsoft.com/en-us/sql/t-sql/statements/begin-dialog-conversation-transact-sql) erstellt wurde und auf Service Broker in einer anderen Datenbank verweist, stellt die Zustellung von Nachrichten an den Zieldienst ein.

**Problemumgehung:** Beenden Sie alle Aktivitäten, die datenbankübergreifende Dialogkonversationen des Service Brokers verwenden, bevor Sie die Dienstebene aktualisieren, und initialisieren Sie sie danach neu. Wenn es noch Nachrichten gibt, die nach dem Ändern der Dienstebene nicht zugestellt werden, lesen Sie die Nachrichten aus der Quellwarteschlange, und senden Sie sie erneut an die Zielwarteschlange.

### <a name="impersonification-of-aad-login-types-is-not-supported"></a>Der Identitätswechsel für AAD-Anmeldetypen wird nicht unterstützt.

**Datum:** Juli 2019

Der Identitätswechsel mithilfe von `EXECUTE AS USER` oder `EXECUTE AS LOGIN` der folgenden AAD-Prinzipale wird nicht unterstützt:
-   AAD-Benutzer mit Alias. In diesem Fall wird der Fehler `15517` zurückgegeben.
- AAD-Anmeldungen und-Benutzer, die auf AAD-Anwendungen oder -Dienstprinzipalen basieren. In diesem Fall werden die Fehler `15517` und `15406` zurückgegeben.

### <a name="query-parameter-not-supported-in-sp_send_db_mail"></a>Der Parameter @query wird in sp_send_db_mail nicht unterstützt.

**Datum:** April 2019

Der `@query`-Parameter in der Prozedur [sp_send_db_mail](https://docs.microsoft.com/sql/relational-databases/system-stored-procedures/sp-send-dbmail-transact-sql) funktioniert nicht.

### <a name="transactional-replication-must-be-reconfigured-after-geo-failover"></a>Die Transaktionsreplikation muss nach einem geografischen Failover neu konfiguriert werden.

**Datum:** März 2019

Wenn die Transaktionsreplikation für eine Datenbank in einer Autofailover-Gruppe aktiviert ist, muss der Administrator der verwalteten Instanz alle Veröffentlichungen für die alte primäre Instanz bereinigen und nach einem Failover in eine andere Region für die neue primäre Instanz erneut konfigurieren. Weitere Informationen finden Sie unter [Replikation](#replication).

### <a name="aad-logins-and-users-are-not-supported-in-tools"></a>AAD-Anmeldungen und -Benutzer werden in Tools nicht unterstützt.

**Datum:** Januar 2019

SQL Server Management Studio und SQL Server Data Tools unterstützen Azure Active Directory-Anmeldungen und -Benutzer nicht vollständig.
- Die Verwendung von Azure AD-Serverprinzipalen (Anmeldungen) und Azure AD-Benutzern (öffentliche Vorschau) mit SQL Server Data Tools wird derzeit nicht unterstützt.
- Das Erstellen von Azure AD-Serverprinzipalen (Anmeldungen) und -Benutzern (öffentliche Vorschau) per Skript wird in SQL Server Management Studio nicht unterstützt.

### <a name="temporary-database-is-used-during-restore-operation"></a>Temporäre Datenbank wird während des RESTORE-Vorgangs verwendet

Wenn eine Datenbank auf einer verwalteten Instanz wiederhergestellt wird, erstellt der Wiederherstellungsdienst zunächst eine leere Datenbank mit dem gewünschten Namen, um den Namen auf der Instanz zuzuweisen. Nach einiger Zeit wird diese Datenbank gelöscht, und die Wiederherstellung der eigentlichen Datenbank wird gestartet. Die Datenbank, die sich im Status *Wird wiederhergestellt* befindet, weist temporär einen zufälligen GUID-Wert anstelle des Namens auf. Nachdem der Wiederherstellungsvorgang abgeschlossen ist, wird der temporäre Name in den in der `RESTORE`-Anweisung angegebenen gewünschten Namen geändert. In der Anfangsphase kann der Benutzer auf die leere Datenbank zugreifen und sogar Tabellen erstellen oder Daten in diese Datenbank laden. Diese temporäre Datenbank wird gelöscht, wenn der Wiederherstellungsdienst die zweite Phase startet.

**Problemumgehung**: Greifen Sie erst dann auf die Datenbank zu, die Sie wiederherstellen, wenn Sie feststellen, dass die Wiederherstellung abgeschlossen ist.

### <a name="tempdb-structure-and-content-is-re-created"></a>Struktur und Inhalt von TEMPDB werden neu erstellt.

Die Datenbank `tempdb` ist immer in 12 Datendateien aufgeteilt, und die Dateistruktur kann nicht geändert werden. Die maximale Größe pro Datei kann nicht geändert werden. Zudem können `tempdb` keine neuen Dateien hinzugefügt werden. `Tempdb` wird beim Start oder Failover einer Instanz immer als leere Datenbank neu erstellt. Änderungen an `tempdb` werden nicht beibehalten.

### <a name="exceeding-storage-space-with-small-database-files"></a>Überschreiten des Speicherplatzes mit kleinen Datenbankdateien

`CREATE DATABASE`-, `ALTER DATABASE ADD FILE`- und `RESTORE DATABASE`-Anweisungen schlagen möglicherweise fehl, weil die Instanz das Azure-Speicherlimit erreichen kann.

Jede verwaltete Instanz in der Dienstebene „Universell“ reserviert bis zu 35 TB Speicherplatz für Azure Premium-Datenträger. Jede Datenbankdatei wird auf einem separaten physischen Datenträger platziert. Mögliche Datenträgergrößen sind 128 GB, 256 GB, 512 GB, 1 TB oder 4 TB. Nicht verwendeter Speicherplatz auf dem Datenträger wird nicht berechnet, aber die Gesamtgröße der Azure Premium-Datenträger darf 35 TB nicht überschreiten. In einigen Fällen kann eine verwaltete Instanz, die nicht insgesamt 8 TB benötigt, aufgrund interner Fragmentierung das Azure-Limit von 35 TB überschreiten.

Ein Beispiel: In einer verwalteten Instanz der Dienstebene „Universell“ gibt es eine große Datei (1,2 TB), die auf einem 4-TB-Datenträger gespeichert ist. Es kann auch 248 Dateien mit einer Größe von jeweils 1 GB geben, die auf separaten 128-GB-Datenträgern gespeichert sind. In diesem Beispiel:

- Die Gesamtgröße des zugewiesenen Datenträgerspeichers beträgt 1 x 4 TB + 248 x 128 GB = 35 TB.
- Der reservierte Gesamtspeicherplatz für Datenbanken in der Instanz beträgt 1 x 1,2 TB + 248 x 1 GB = 1,4 TB.

Dieses Beispiel verdeutlicht, dass eine verwaltete Instanz unter bestimmten Umständen aufgrund einer spezifischen Verteilung von Dateien das für einen angefügten Azure Premium-Datenträger reservierte Limit von 35 TB erreichen kann, auch wenn Sie dies möglicherweise nicht erwarten.

In diesem Beispiel funktionieren vorhandene Datenbanken weiterhin und können ohne Probleme weiter wachsen, solange keine neuen Dateien hinzugefügt werden. Es könnten jedoch keine neuen Datenbanken erstellt oder wiederhergestellt werden, da für neue Datenträgerlaufwerke nicht genügend Speicherplatz vorhanden ist, selbst nicht dann, wenn die Gesamtgröße aller Datenbanken das Größenlimit der Instanz nicht überschreitet. Der Fehler, der in diesem Fall zurückgegeben wird, ist nicht klar.

Sie können mithilfe von Systemansichten [die Anzahl von verbleibenden Dateien identifizieren](https://medium.com/azure-sqldb-managed-instance/how-many-files-you-can-create-in-general-purpose-azure-sql-managed-instance-e1c7c32886c1). Wenn Sie dieses Limit erreichen, versuchen Sie, [einige der kleineren Dateien mithilfe der DBCC SHRINKFILE-Anweisung zu leeren und zu löschen](https://docs.microsoft.com/sql/t-sql/database-console-commands/dbcc-shrinkfile-transact-sql#d-emptying-a-file), oder wechseln Sie zur [Dienstebene „Unternehmenskritisch“, für die dieses Limit nicht gilt](https://docs.microsoft.com/azure/sql-database/sql-database-managed-instance-resource-limits#service-tier-characteristics).

### <a name="guid-values-shown-instead-of-database-names"></a>Anstelle von Datenbanknamen werden GUID-Werte gezeigt.

Mehrere Systemansichten, Leistungsindikatoren, Fehlermeldungen, XEvents und Fehlerprotokolleinträge zeigen GUID-Datenbankbezeichner anstelle der eigentlichen Datenbanknamen an. Verlassen Sie sich nicht auf diese GUIDs, da sie in Zukunft durch tatsächliche Datenbanknamen ersetzt werden.

### <a name="error-logs-arent-persisted"></a>Fehlerprotokolle werden nicht persistent gespeichert

Die Fehlerprotokolle in einer verwalteten Instanz werden nicht persistent gespeichert, und ihre Größe wird im Speicherlimit nicht berücksichtigt. Fehlerprotokolle werden im Falle eines Failovers möglicherweise automatisch gelöscht. Möglicherweise gibt es Lücken im Fehlerprotokollverlauf, weil die verwaltete Instanz auf mehreren virtuellen Computern mehrmals verschoben wurde.

### <a name="transaction-scope-on-two-databases-within-the-same-instance-isnt-supported"></a>Ein Transaktionsbereich in zwei Datenbanken in derselben Instanz wird nicht unterstützt

Die `TransactionScope`-Klasse in .NET funktioniert nicht, wenn zwei Abfragen an zwei Datenbanken in derselben Instanz im gleichen Transaktionsbereich gesendet werden:

```csharp
using (var scope = new TransactionScope())
{
    using (var conn1 = new SqlConnection("Server=quickstartbmi.neu15011648751ff.database.windows.net;Database=b;User ID=myuser;Password=mypassword;Encrypt=true"))
    {
        conn1.Open();
        SqlCommand cmd1 = conn1.CreateCommand();
        cmd1.CommandText = string.Format("insert into T1 values(1)");
        cmd1.ExecuteNonQuery();
    }

    using (var conn2 = new SqlConnection("Server=quickstartbmi.neu15011648751ff.database.windows.net;Database=b;User ID=myuser;Password=mypassword;Encrypt=true"))
    {
        conn2.Open();
        var cmd2 = conn2.CreateCommand();
        cmd2.CommandText = string.Format("insert into b.dbo.T2 values(2)");        cmd2.ExecuteNonQuery();
    }

    scope.Complete();
}

```

Obwohl dieser Code mit Daten innerhalb derselben Instanz funktioniert, erforderte er MS DTC.

**Problemumgehung:** Verwenden Sie [SqlConnection.ChangeDatabase(String)](https://docs.microsoft.com/dotnet/api/system.data.sqlclient.sqlconnection.changedatabase), um anstelle von zwei Verbindungen eine andere Datenbank im Verbindungskontext zu verwenden.

### <a name="clr-modules-and-linked-servers-sometimes-cant-reference-a-local-ip-address"></a>CLR-Module und Verbindungsserver können manchmal nicht auf eine lokale IP-Adresse verweisen

CLR-Module, die in einer verwalteten Instanz bereitgestellt werden, und Verbindungsserver oder verteilte Abfragen, die auf eine aktuelle Instanz verweisen, können die IP-Adresse der lokalen Instanz manchmal nicht auflösen. Dieser Fehler ist ein vorübergehendes Problem.

**Problemumgehung:** Verwenden Sie nach Möglichkeit Kontextverbindungen im CLR-Modul.

## <a name="next-steps"></a>Nächste Schritte

- Weitere Informationen zu verwalteten Instanzen finden Sie unter [Was ist eine verwaltete Instanz?](sql-database-managed-instance.md)
- Informationen zu den Funktionen und eine Vergleichsliste finden Sie unter [Funktionsvergleich: Azure SQL-Datenbank und SQL Server](sql-database-features.md).
- Einen Schnellstart zum Erstellen einer neuen verwalteten Instanz finden Sie unter [Erstellen einer verwalteten Instanz](sql-database-managed-instance-get-started.md).
