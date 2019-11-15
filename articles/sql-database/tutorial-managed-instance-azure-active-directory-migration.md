---
title: Migrieren Sie SQL Server-Windows-Benutzern und -Gruppen in eine verwaltete Instanz mit T-SQL
description: Hier erfahren Sie, wie Sie lokale Windows-Benutzer und -Gruppen in SQL Server zu einer verwalteten Instanz migrieren.
services: sql-database
ms.service: sql-database
ms.subservice: security
ms.custom: seo-lt-2019
ms.topic: tutorial
author: GitHubMirek
ms.author: mireks
ms.reviewer: vanto
ms.date: 10/30/2019
ms.openlocfilehash: 3ed4e4b1d37a9705378281ca74b53a6b60713d97
ms.sourcegitcommit: ac56ef07d86328c40fed5b5792a6a02698926c2d
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 11/08/2019
ms.locfileid: "73807176"
---
# <a name="tutorial-migrate-sql-server-on-premises-windows-users-and-groups-to-azure-sql-database-managed-instance-using-t-sql-ddl-syntax"></a>Tutorial: Migrieren lokaler Windows-Benutzer und -Gruppen in SQL Server zu einer verwalteten Azure SQL-Datenbank-Instanz unter Verwendung der T-SQL-DDL-Syntax

> [!NOTE]
> Die Syntax, die in diesem Artikel zum Migrieren von Benutzern und Gruppen zu einer verwalteten Instanz verwendet wird, befindet sich in der **Public Preview-Phase**.

In diesem Artikel erfahren Sie, wie Sie Ihre lokalen Windows-Benutzer und -Gruppen in SQL Server unter Verwendung der T-SQL-Syntax zu einer vorhandenen verwalteten Azure SQL-Datenbank-Instanz migrieren.

In diesem Tutorial lernen Sie Folgendes:

> [!div class="checklist"]
> - Erstellen von Anmeldungen für SQL Server
> - Erstellen einer Testdatenbank für die Migration
> - Erstellen von Anmeldungen, Benutzern und Rollen
> - Sichern und Wiederherstellen Ihrer Datenbank in einer verwalteten Instanz (managed instance, MI)
> - Manuelles Migrieren von Benutzern zur MI mithilfe der ALTER USER-Syntax
> - Testen der Authentifizierung mit den neuen zugeordneten Benutzern

## <a name="prerequisites"></a>Voraussetzungen

Für dieses Tutorials ist Folgendes erforderlich:

- Die Windows-Domäne muss sich in einem Verbund mit Azure Active Directory (Azure AD) befinden.
- Zugriff auf Active Directory für die Erstellung von Benutzern/Gruppen
- Eine SQL Server-Instanz in Ihrer lokalen Umgebung
- Eine verwaltete Instanz. Weitere Informationen finden Sie unter [Schnellstart: Erstellen einer verwalteten Azure SQL-Datenbank-Instanz](sql-database-managed-instance-get-started.md).
  - Ein `sysadmin` in der verwalteten Instanz muss verwendet werden, um Azure AD-Anmeldungen zu erstellen.
- [Erstellen Sie einen Azure AD-Administrator für die verwaltete Instanz](sql-database-aad-authentication-configure.md#provision-an-azure-active-directory-administrator-for-your-managed-instance).
- Erfolgreiche Verbindungsherstellung mit der verwalteten Instanz in Ihrem Netzwerk. Weitere Informationen finden Sie in den folgenden Artikeln: 
    - [Herstellen einer Verbindung zwischen einer Anwendung und einer verwalteten Azure SQL-Datenbank-Instanz](sql-database-managed-instance-connect-app.md)
    - [Schnellstart: Konfigurieren einer Point-to-Site-Verbindung von einem lokalen Computer mit einer verwalteten Azure SQL-Datenbank-Instanz](sql-database-managed-instance-configure-p2s.md)
    - [Konfigurieren des öffentlichen Endpunkts in der verwalteten Azure SQL-Datenbank-Instanz](sql-database-managed-instance-public-endpoint-configure.md)

## <a name="t-sql-ddl-syntax"></a>T-SQL-DDL-Syntax

In diesem Abschnitt finden Sie die T-SQL-DDL-Syntax für die Migration von Windows-Benutzern und -Gruppen in SQL Server zu einer verwalteten Instanz mit Azure AD-Authentifizierung.

```sql
-- For individual Windows users with logins 
ALTER USER [domainName\userName] WITH LOGIN = [loginName@domainName.com]; 

--For individual groups with logins 
ALTER USER [domainName\groupName] WITH LOGIN=[groupName] 
```

## <a name="arguments"></a>Argumente

_domainName_</br>
Gibt den Domänennamen des Benutzers an.

_userName_</br>
Gibt den Namen des in der Datenbank identifizierten Benutzers an.

_= loginName\@domainName.com_</br>
Ordnet einen Benutzer der Azure AD-Anmeldung zu.

_groupName_</br>
Gibt den Namen der in der Datenbank identifizierten Gruppe an.

## <a name="part-1-create-logins-for-sql-server-on-premises-users-and-groups"></a>Teil 1: Erstellen von Anmeldungen für lokale Benutzer und Gruppen in SQL Server

> [!IMPORTANT]
> Die folgende Syntax erstellt einen Benutzer und eine Gruppenanmeldung in Ihrer SQL Server-Instanz. Der Benutzer und die Gruppe müssen vor dem Ausführen der folgenden Syntax bereits in Active Directory (AD) vorhanden sein. </br> </br>
> Benutzer: „testUser1“, „testGroupUser“ </br>
> Gruppe: „migration“; „testGroupUser“ muss der Migrationsgruppe in AD angehören.

Im folgenden Beispiel in SQL Server eine Anmeldung für ein Konto namens _testUser1_ unter der Domäne _aadsqlmi_ erstellt. 

```sql
-- Sign into SQL Server as a sysadmin or a user that can create logins and databases
 
use master;  
go

-- Create Windows login
create login [aadsqlmi\testUser1] from windows;   
go; 

/** Create a Windows group login which contains one user [aadsqlmi\testGroupUser]. 
testGroupUser will need to be added to the migration group in Active Directory
**/
create login [aadsqlmi\migration] from windows;    
go; 


-- Check logins were created
select * from sys.server_principals; 
go; 
```

Erstellen Sie eine Datenbank für diesen Test:

```sql
-- Create a database called [migration]
create database migration 
go
```

## <a name="part-2-create-windows-users-and-groups-then-add-roles-and-permissions"></a>Teil 2: Erstellen von Windows-Benutzern und -Gruppen und Hinzufügen von Rollen und Berechtigungen

Verwenden Sie die folgende Syntax, um den Testbenutzer zu erstellen:

```sql
use migration;  
go 
 
-- Create Windows user [aadsqlmi\testUser1] with login 
create user [aadsqlmi\testUser1] from login [aadsqlmi\testUser1]; 
go 
```

Überprüfen Sie die Benutzerberechtigungen:

```sql
-- Check the user in the Metadata 
select * from sys.database_principals; 
go 
 
-- Display the permissions – should only have CONNECT permissions
select user_name(grantee_principal_id), * from sys.database_permissions; 
go
```

Erstellen Sie eine Rolle, und weisen Sie ihr Ihren Testbenutzer zu:

```sql 
-- Create a role with some permissions and assign the user to the role
create role UserMigrationRole; 
go

grant CONNECT, SELECT, View DATABASE STATE, VIEW DEFINITION to UserMigrationRole; 
go 

alter role UserMigrationRole add member [aadsqlmi\testUser1]; 
go 
``` 

Verwenden Sie die folgende Abfrage, um Benutzernamen anzuzeigen, die einer bestimmten Rolle zugewiesen sind:

```sql
-- Display user name assigned to a specific role 
SELECT DP1.name AS DatabaseRoleName,    
   isnull (DP2.name, 'No members') AS DatabaseUserName    
 FROM sys.database_role_members AS DRM   
 RIGHT OUTER JOIN sys.database_principals AS DP1   
   ON DRM.role_principal_id = DP1.principal_id   
 LEFT OUTER JOIN sys.database_principals AS DP2   
   ON DRM.member_principal_id = DP2.principal_id   
WHERE DP1.type = 'R' 
ORDER BY DP1.name; 
```

Verwenden Sie die folgende Syntax, um eine Gruppe zu erstellen. Fügen Sie die Gruppe anschließend der Rolle `db_owner` hinzu:

```sql
-- Create Windows group
create user [aadsqlmi\migration] from login [aadsqlmi\migration]; 
go

-- ADD 'db_owner' role to this group
sp_addrolemember 'db_owner', 'aadsqlmi\migration'; 
go 

--Check the db_owner role for 'aadsqlmi\migration' group 
select is_rolemember('db_owner', 'aadsqlmi\migration')   
go 
-- Output  ( 1 means YES) 
```

Erstellen Sie eine Testtabelle, und fügen Sie mithilfe der folgenden Syntax einige Daten hinzu:

```sql
-- Create a table and add data 
create table test ( a int, b int); 
go 

insert into test values (1,10) 
go 

-- Check the table values
select * from test; 
go
```

## <a name="part-3-backup-and-restore-the-individual-user-database-to-managed-instance"></a>Teil 3: Sichern und Wiederherstellen der individuellen Benutzerdatenbank in einer verwalteten Instanz

Erstellen wie im Artikel [Kopieren von Datenbanken durch Sichern und Wiederherstellen](/sql/relational-databases/databases/copy-databases-with-backup-and-restore) beschrieben eine Sicherung der Migrationsdatenbank, oder verwenden Sie die folgende Syntax:

```sql
use master; 
go 
backup database migration to disk = 'C:\Migration\migration.bak'; 
go
```

Gehen Sie wie unter [Schnellstart: Wiederherstellen einer Datenbank in einer verwalteten Instanz](sql-database-managed-instance-get-started-restore.md) beschrieben vor.

## <a name="part-4-migrate-users-to-managed-instance"></a>Teil 4: Migrieren von Benutzern zu einer verwalteten Instanz

> [!NOTE]
> Der Azure AD-Administrator für verwaltete Instanzfunktionen nach der Erstellung hat sich geändert. Weitere Informationen finden Sie unter [Neue Azure AD-Administratorfunktionen für verwaltete Instanzen](sql-database-aad-authentication-configure.md#new-azure-ad-admin-functionality-for-mi).

Führen Sie den Befehl „ALTER USER“ aus, um die Migration zu einer verwalteten Instanz abzuschließen.

1. Melden Sie sich mit Ihrem Azure AD-Administratorkonto für die verwaltete Instanz bei Ihrer verwalteten Instanz an. Erstellen Sie anschließend mithilfe der folgenden Syntax Ihre Azure AD-Anmeldung in der verwalteten Instanz. Weitere Informationen finden Sie unter [Tutorial: Sicherheit für verwaltete Instanzen in Azure SQL-Datenbank durch Azure AD-Serverprinzipale (Anmeldungen)](sql-database-managed-instance-aad-security-tutorial.md).

    ```sql
    use master 
    go 
    
    -- Create login for AAD user [testUser1@aadsqlmi.net] 
    create login [testUser1@aadsqlmi.net] from external provider 
    go
    
    -- Create login for the Azure AD group [migration]. This group contains one user [testGroupUser@aadsqlmi.net] 
    create login [migration] from external provider 
    go 
     
    --Check the two new logins 
    select * from sys.server_principals 
    go
    ```

1. Überprüfen Sie, ob Ihre Migration über die korrekte Datenbank und Tabelle sowie über die korrekten Prinzipale verfügt:

    ```sql
    -- Switch to the database migration that is already restored for MI 
    use migration;  
    go 
     
    --Check if the restored table test exist and contain a row 
    select * from test; 
    go 
     
    -- Check that the SQL on-premise Windows user/group exists  
    select * from sys.database_principals; 
    go 
    -- the old user aadsqlmi\testUser1 should be there 
    -- the old group aadsqlmi\migration should be there
    ```

1. Ordnen Sie den lokalen Benutzer mithilfe der ALTER USER-Syntax der Azure AD-Anmeldung zu:

    ```sql
    /** Execute the ALTER USER command to alter the Windows user [aadsqlmi\testUser1]
    to map to the Azure AD user testUser1@aadsqlmi.net
    **/
    alter user [aadsqlmi\testUser1] with login = [testUser1@aadsqlmi.net]; 
    go

    -- Check the principal
    select * from sys.database_principals; 
    go 
    -- New user testUser1@aadsqlmi.net should be there instead 
     
    --Check new user permissions  - should only have CONNECT permissions
    select user_name(grantee_principal_id), * from sys.database_permissions; 
    go

    -- Check a specific role   
    -- Display Db user name assigned to a specific role 
    SELECT DP1.name AS DatabaseRoleName,    
    isnull (DP2.name, 'No members') AS DatabaseUserName    
    FROM sys.database_role_members AS DRM   
    RIGHT OUTER JOIN sys.database_principals AS DP1   
    ON DRM.role_principal_id = DP1.principal_id   
    LEFT OUTER JOIN sys.database_principals AS DP2   
    ON DRM.member_principal_id = DP2.principal_id   
    WHERE DP1.type = 'R' 
    ORDER BY DP1.name;
    ```

1. Ordnen Sie die lokale Gruppe mithilfe der ALTER USER-Syntax der Azure AD Anmeldung zu:

    ```sql
    /** Execute ALTER USER command to alter the Windows group [aadsqlmi\migration]
    to the Azure AD group login [migration]
    **/
    alter user [aadsqlmi\migration] with login = [migration]; 
    -- old group migration is changed to Azure AD migration group
    go

    -- Check the principal
    select * from sys.database_principals; 
    go 
     
    --Check the group permission - should only have CONNECT permissions
    select user_name(grantee_principal_id), * from sys.database_permissions; 
    go 
     
    --Check the db_owner role for 'aadsqlmi\migration' user 
    select is_rolemember('db_owner', 'migration')   
    go 
    -- Output 1 means 'YES'
    ```

## <a name="part-5-testing-azure-ad-user-or-group-authentication"></a>Teil 5: Testen der Authentifizierung für Azure AD-Benutzer oder -Gruppen

Testen Sie die Authentifizierung bei der verwalteten Instanz mithilfe des Benutzers, den Sie zuvor unter Verwendung der ALTER USER-Syntax der Azure AD-Anmeldung zugeordnet haben.
 
1. Melden Sie sich mit Ihrem MI-Abonnement als `aadsqlmi\testUser1` bei dem virtuellen Verbundcomputer an.
1. Melden Sie sich unter Verwendung von SQL Server Management Studio (SSMS) bei Ihrer verwalteten Instanz an. Verwenden Sie dabei die integrierte **Active Directory-Authentifizierung**, und stellen Sie eine Verbindung mit der Datenbank `migration` her.
    1. Sie können sich auch mit den Anmeldeinformationen testUser1@aadsqlmi.net und der SSMS-Option **Active Directory: universell mit MFA-Unterstützung** anmelden. In diesem Fall steht allerdings das einmalige Anmelden nicht zur Verfügung, und Sie müssen ein Kennwort eingeben. Für die Anmeldung bei Ihrer verwalteten Instanz muss kein virtueller Verbundcomputer verwendet werden.
1. Im Rahmen des Rollenmitglieds **SELECT** können Sie Inhalte aus der Tabelle `test` auswählen:

    ```sql
    Select * from test  --  and see one row (1,10)
    ```


Testen Sie die Authentifizierung bei einer verwalteten Instanz mithilfe eines Mitglieds einer Windows-Gruppe (`migration`). Der Benutzer `aadsqlmi\testGroupUser` muss der Gruppe `migration` vor der Migration hinzugefügt worden sein.

1. Melden Sie sich mit Ihrem MI-Abonnement als `aadsqlmi\testGroupUser` bei dem virtuellen Verbundcomputer an. 
1. Stellen Sie unter Verwendung von SSMS mit integrierter **Active Directory-Authentifizierung eine Verbindung mit dem MI-Server und der Datenbank** `migration` her.
    1. Sie können sich auch mit den Anmeldeinformationen testGroupUser@aadsqlmi.net und der SSMS-Option **Active Directory: universell mit MFA-Unterstützung** anmelden. In diesem Fall steht allerdings das einmalige Anmelden nicht zur Verfügung, und Sie müssen ein Kennwort eingeben. Für die Anmeldung bei Ihrer verwalteten Instanz muss kein virtueller Verbundcomputer verwendet werden. 
1. Im Rahmen der Rolle `db_owner` können Sie eine neue Tabelle erstellen.

    ```sql
    -- Create table named 'new' with a default schema
    Create table dbo.new ( a int, b int)
    ```
                             
> [!NOTE] 
> Aufgrund eines bekannten entwurfsbedingten Problems im Zusammenhang mit Azure SQL-Datenbank tritt der folgende Fehler auf, wenn eine Tabellenerstellungsanweisung als Mitglied einer Gruppe ausgeführt wird: </br> </br>
> `Msg 2760, Level 16, State 1, Line 4 
The specified schema name "testGroupUser@aadsqlmi.net" either does not exist or you do not have permission to use it.` </br> </br>
> Erstellen Sie zur Umgehung dieses Problems eine Tabelle mit einem vorhandenen Schema (im obigen Fall: <dbo.new>).

## <a name="next-steps"></a>Nächste Schritte

- [Tutorial: Migrieren von SQL Server zu einer verwalteten Azure SQL-Datenbank-Instanz mithilfe von DMS (offline)](../dms/tutorial-sql-server-to-managed-instance.md?toc=/azure/sql-database/toc.json)