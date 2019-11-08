---
title: Behandeln von Konnektivitätsproblemen
description: Erfahren Sie, wie Sie in Azure SQL-Datenbank Verbindungsprobleme beheben.
services: sql-database
ms.service: sql-database
ms.topic: troubleshooting
ms.custom: seo-lt-2019
author: v-miegge
ms.author: ramakoni
ms.reviewer: ''
ms.date: 09/27/2019
ms.openlocfilehash: 20988296b5eac7152c53abd6d238043288feacc8
ms.sourcegitcommit: ac56ef07d86328c40fed5b5792a6a02698926c2d
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 11/08/2019
ms.locfileid: "73807273"
---
# <a name="troubleshooting-connectivity-issues-with-microsoft-azure-sql-database"></a>Beheben von Verbindungsproblemen mit Microsoft Azure SQL-Datenbank

Wenn keine Verbindung mit Azure SQL-Datenbank hergestellt werden kann, erhalten Sie Fehlermeldungen. Diese Verbindungsprobleme können durch eine Neukonfiguration der Azure SQL-Datenbank-Instanz, Firewalleinstellungen, ein Verbindungstimeout oder fehlerhafte Anmeldeinformationen verursacht werden. Wenn außerdem die Obergrenze einiger Azure SQL-Datenbank-Ressourcen erreicht wird, können Sie keine Verbindung mit Azure SQL-Datenbank herstellen.

## <a name="error-40613-database--x--on-server--y--is-not-currently-available"></a>Fehler 40613: Die Datenbank „x“ auf dem Server „y“ ist zurzeit nicht verfügbar

**Detaillierter Fehler**

``40613: Database <DBname> on server < server name > is not currently available. Please retry the connection later. If the problem persists, contact customer support, and provide them the session tracing ID of '< Tracing ID >'.``

So lösen Sie dieses Problem:

1. Informationen zu bekannten Ausfällen finden Sie im [Microsoft Azure-Dienstdashboard](https://status.azure.com/status). 
2. Wenn es keine bekannten Ausfälle gibt, navigieren Sie zur [Website des Microsoft Azure-Supports](https://azure.microsoft.com/support/options), um eine Supportanfrage zu öffnen.

Weitere Informationen finden Sie unter [Behandlung von Fehlern des Typs „Die Datenbank auf dem Server ist zurzeit nicht verfügbar“](sql-database-troubleshoot-common-connection-issues.md#troubleshoot-transient-errors).

## <a name="a-network-related-or-instance-specific-error-occurred-while-establishing-a-connection-to-sql-server"></a>Netzwerkbezogener oder instanzspezifischer Fehler beim Herstellen einer Verbindung mit SQL Server

Das Problem tritt auf, wenn die Anwendung keine Verbindung mit dem Server herstellen kann.

Um dieses Problem zu beheben, führen Sie die unter [Schritte zum Beheben häufiger Verbindungsprobleme](#steps-to-fix-common-connection-issues) genannten Schritte (in der angegebenen Reihenfolge) aus.

## <a name="the-server-was-not-found-or-was-not-accessible-errors-26-40-10053"></a>Der Server wurde nicht gefunden, oder auf ihn kann nicht zugegriffen werden (Fehler 26, 40, 10053)

### <a name="error-26-error-locating-serverinstance-specified"></a>Fehler 26: Fehler beim Auffinden des angegebenen Servers bzw. der Instanz

**Detaillierter Fehler**

``System.Data.SqlClient.SqlException: A network-related or instance-specific error occurred while establishing a connection to SQL Server. The server was not found or was not accessible. Verify that the instance name is correct and that SQL Server is configured to allow remote connections.(provider: SQL Network Interfaces, error: 26 – Error Locating Server/Instance Specified)``

### <a name="error-40-could-not-open-a-connection-to-sql-server"></a>Fehler 40: Es konnte keine Verbindung mit SQL-Server geöffnet werden

**Detaillierter Fehler**

``A network-related or instance-specific error occurred while establishing a connection to SQL Server. The server was not found or was not accessible. Verify that the instance name is correct and that SQL Server is configured to allow remote connections. (provider: Named Pipes Provider, error: 40 - Could not open a connection to SQL Server)``

### <a name="error-10053-a-transport-level-error-has-occurred-when-receiving-results-from-the-server"></a>Fehler 10053: Beim Empfangen von Ergebnissen vom Server ist ein Fehler auf Übertragungsebene aufgetreten.

**Detaillierter Fehler**

``10053: A transport-level error has occurred when receiving results from the server. (Provider: TCP Provider, error: 0 - An established connection was aborted by the software in your host machine)``

Diese Fehler treten auf, weil die Anwendung keine Verbindung mit dem Server herstellen kann.

Um dieses Problem zu beheben, führen Sie die unter [Schritte zum Beheben häufiger Verbindungsprobleme](#steps-to-fix-common-connection-issues) genannten Schritte (in der angegebenen Reihenfolge) aus.

## <a name="cannot-connect-to-servername-due-to-firewall-issues"></a>Aufgrund von Firewallproblemen kann keine Verbindung mit <servername> hergestellt werden

### <a name="error-40615-cannot-connect-to--servername-"></a>Fehler 40615: Mit <Servername> kann keine Verbindung hergestellt werden

Um dieses Problem zu beheben, [konfigurieren Sie im Azure-Portal die Firewalleinstellungen für Azure SQL-Datenbank.](https://docs.microsoft.com/azure/sql-database/sql-database-firewall-configure)

### <a name="error-5-cannot-connect-to--servername-"></a>Fehler 5: Mit <Servername> kann keine Verbindung hergestellt werden

Stellen Sie für alle Firewalls zwischen Client und Internet sicher, dass Port 1433 für ausgehende Verbindungen geöffnet ist, um dieses Problem zu beheben.

Weitere Informationen finden Sie unter [Konfigurieren der Windows-Firewall für den SQL Server-Zugriff](https://docs.microsoft.com/azure/sql-database/sql-database-firewall-configure).

## <a name="unable-to-log-in-to-the-server-errors-18456-40531"></a>Die Anmeldung beim Server ist nicht möglich (Fehler 18456, 40531)

### <a name="login-failed-for-user--user-name-"></a>Fehler bei der Anmeldung für den Benutzer „<Benutzername>“

**Detaillierter Fehler**

``Login failed for user '<User name>'.This session has been assigned a tracing ID of '<Tracing ID>'. Provide this tracing ID to customer support when you need assistance. (Microsoft SQL Server, Error: 18456)``

Um dieses Problem zu beheben, wenden Sie sich an Ihren Dienstadministrator, um einen gültigen SQL Server-Benutzernamen und ein gültiges Kennwort anzugeben.

In der Regel kann der Dienstadministrator die Anmeldeinformationen mit den folgenden Schritten hinzufügen:

1. Anmelden beim Server mit SQL Server Management Studio (SSMS).
2. Mithilfe der folgenden SQL-Abfrage überprüfen, ob der Anmeldename deaktiviert ist:

   ```
   SELECT name, is_disabled FROM sys.sql_logins
   ```

3. Wenn der entsprechende Name deaktiviert ist, kann er mithilfe der folgenden Anweisung aktiviert werden: 

   ```
   Alter login <User name> enable
   ```

4. Wenn der Benutzername für die SQL Server-Anmeldung nicht vorhanden ist, muss er mithilfe dieser Schritte erstellt werden:

   1. Doppelklicken Sie in SSMS auf **Sicherheit**, um den Bereich zu erweitern. 
   2. Klicken Sie mit der rechten Maustaste auf **Anmeldungen**, und wählen Sie dann **Neue Anmeldung**. 
   3. Im generierten Skript mit Platzhaltern können Sie die folgende SQL-Abfrage bearbeiten und ausführen:
 
   ```
   CREATE LOGIN <SQL_login_name, sysname, login_name>
   WITH PASSWORD = ‘<password, sysname, Change_Password>’
   GO
   ```       
5. Doppelklicken Sie auf **Datenbank**. 
6. Wählen Sie die Datenbank aus, für die der Benutzer die Berechtigung erhalten soll.
7. Doppelklicken Sie auf **Sicherheit**. 
8. Klicken Sie mit der rechten Maustaste auf **Benutzer**, und wählen Sie dann **Neuer Benutzer**. 
9. Im generierten Skript mit Platzhaltern können Sie die folgende SQL-Abfrage bearbeiten und ausführen: 

   ```
   CREATE USER <user_name, sysname, user_name>          
   FOR LOGIN <login_name, sysname, login_name>
   WITH DEFAULT_SCHEMA = <default_schema, sysname, dbo>
   GO
   
   -- Add user to the database owner role

   EXEC sp_addrolemember N’db_owner’, N’<user_name, sysname, user_name>’
   GO
   ```
   
   > [!NOTE]
   > Sie können auch `sp_addrolemember` verwenden, um bestimmte Benutzer bestimmten Datenbankrollen zuzuordnen.

Weitere Informationen finden Sie unter [Steuern und Gewähren des Datenbankzugriffs für SQL-Datenbank und SQL Data Warehouse](https://docs.microsoft.com/azure/sql-database/sql-database-manage-logins).

## <a name="connection-timeout-expired-errors"></a>Verbindungstimeoutfehler

### <a name="systemdatasqlclientsqlexception-0x80131904-connection-timeout-expired"></a>System.Data.SqlClient.SqlException (0x80131904): Verbindungstimeout abgelaufen

**Detaillierter Fehler**

``System.Data.SqlClient.SqlException (0x80131904): Connection Timeout Expired. The timeout period elapsed while attempting to consume the pre-login handshake acknowledgement. This could be because the pre-login handshake failed or the server was unable to respond back in time. The duration spent while attempting to connect to this server was - [Pre-Login] initialization=3; handshake=29995;``

### <a name="systemdatasqlclientsqlexception-0x80131904-timeout-expired"></a>System.Data.SqlClient.SqlException (0x80131904): Timeout abgelaufen

**Detaillierter Fehler**

``System.Data.SqlClient.SqlException (0x80131904): Timeout expired. The timeout period elapsed prior to completion of the operation or the server is not responding.``

### <a name="systemdataentitycoreentityexception-the-underlying-provider-failed-on-open"></a>System.Data.Entity.Core.EntityException: Fehler des zugrunde liegenden Anbieters beim Öffnen

**Detaillierter Fehler**

``System.Data.Entity.Core.EntityException: The underlying provider failed on Open. -> System.Data.SqlClient.SqlException: Timeout expired. The timeout period elapsed prior to completion of the operation or the server is not responding. -> System.ComponentModel.Win32Exception: The wait operation timed out``

### <a name="cannot-connect-to--server-name-"></a>Mit <Servername> kann keine Verbindung hergestellt werden

**Detaillierter Fehler**

``Cannot connect to <server name>.ADDITIONAL INFORMATION:Connection Timeout Expired. The timeout period elapsed during the post-login phase. The connection could have timed out while waiting for server to complete the login process and respond; Or it could have timed out while attempting to create multiple active connections. The duration spent while attempting to connect to this server was - [Pre-Login] initialization=231; handshake=983; [Login] initialization=0; authentication=0; [Post-Login] complete=13000; (Microsoft SQL Server, Error: -2) For help, click: http://go.microsoft.com/fwlink?ProdName=Microsoft%20SQL%20Server&EvtSrc=MSSQLServer&EvtID=-2&LinkId=20476 The wait operation timed out``

Diese Ausnahmen können aufgrund von Verbindungs- oder Abfrageproblemen auftreten. Um zu bestätigen, dass dieser Fehler nicht durch Konnektivitätsprobleme verursacht wird, siehe [Bestätigen, dass ein Fehler von einem Konnektivitätsproblem verursacht wird](#confirm-whether-an-error-is-caused-by-a-connectivity-issue).

Verbindungstimeouts treten auf, weil die Anwendung keine Verbindung mit dem Server herstellen kann. Um dieses Problem zu beheben, führen Sie die unter [Schritte zum Beheben häufiger Verbindungsprobleme](#steps-to-fix-common-connection-issues) genannten Schritte (in der angegebenen Reihenfolge) aus.

## <a name="transient-errors-errors-40197-40545"></a>Vorübergehende Fehler (Fehler 40197, 40545)

### <a name="error-40197-the-service-has-encountered-an-error-processing-your-request-please-try-again-error-code--code-"></a>Fehler 40197: Dienstfehler beim Verarbeiten Ihrer Anforderung. Wiederholen Sie den Vorgang. Fehlercode <Code>

Dieses Problem tritt auf, weil bei einer Neukonfiguration bzw. einem Failover im Back-End ein vorübergehender Fehler aufgetreten ist.

Um dieses Problem zu beheben, warten Sie kurz, und wiederholen Sie den Vorgang dann. Eine Supportanfrage ist nur erforderlich, wenn das Problem weiter besteht.

Stellen Sie als bewährte Methode sicher, dass Wiederholungslogik vorhanden ist. Weitere Informationen zur Wiederholungslogik finden Sie unter [Behandlung vorübergehender Fehler und Verbindungsfehler mit SQL-Datenbank](https://docs.microsoft.com/azure/sql-database/sql-database-connectivity-issues).

## <a name="connection-terminated-because-of-a-system-defined-limit"></a>Verbindung wurde aufgrund eines vom System definierten Limits beendet

### <a name="error-10928-resource-id-d"></a>Fehler 10928: Ressourcen-ID: %d

**Detaillierter Fehler**

``10928: Resource ID: %d. The %s limit for the database is %d and has been reached. See http://go.microsoft.com/fwlink/?LinkId=267637 for assistance. The Resource ID value in error message indicates the resource for which limit has been reached. For sessions, Resource ID = 2.``

Verwenden Sie zum Beheben dieses Problems die folgenden Methoden:

* Überprüfen Sie, ob zeitintensive Abfragen ausgeführt werden.

  > [!NOTE]
  > Dies ist ein minimalistischer Ansatz, der das Problem möglicherweise nicht löst.

  1. Führen Sie die folgende SQL-Abfrage aus, um die Sicht [sys.dm_exec_requests](https://docs.microsoft.com/sql/relational-databases/system-dynamic-management-views/sys-dm-exec-requests-transact-sql) auf Sperranforderungen zu überprüfen:

             ```
             SELECT * FROM dm_exec_requests
             ```

  2. Bestimmen Sie den **Eingabepuffer** für „Blockiert andere Prozesse“.
  3. Optimieren Sie die Abfrage „Blockiert andere Prozesse“.

    Eine ausführliche Problembehandlung finden Sie unter [Wird meine Abfrage in der Cloud einwandfrei ausgeführt?](https://blogs.msdn.com/b/sqlblog/archive/2013/11/01/is-my-query-running-fine-in-the-cloud.aspx)

* Wenn die Datenbank trotz Berücksichtigung von Blockierungen und zeitintensiven Abfragen konsistent an ihre Grenzen stößt, sollten Sie ein Upgrade auf eine der neuen Vorschau-Editionen (z. B. [Standard- oder Premium-Edition](https://azure.microsoft.com/pricing/details/sql-database/)) in Betracht ziehen.

Weitere Informationen zu den Preisoptionen für Azure SQL-Datenbank finden Sie unter [Preise für Azure SQL-Datenbank](https://azure.microsoft.com/pricing/details/sql-database/single/).

Weitere Informationen zu dynamischen Verwaltungssichten finden Sie unter [Dynamische Systemverwaltungssichten](https://docs.microsoft.com/sql/relational-databases/system-dynamic-management-views/system-dynamic-management-views).

Weitere Informationen zu dieser Fehlermeldung finden Sie unter [SQL-Datenbank-Ressourcenlimits für den Azure SQL-Datenbank-Server](https://docs.microsoft.com/azure/sql-database/sql-database-resource-limits-database-server).

### <a name="error-10929-resource-id-1"></a>Fehler 10929: Ressourcen-ID: 1

**Detaillierter Fehler**

``10929: Resource ID: 1. The %s minimum guarantee is %d, maximum limit is %d and the current usage for the database is %d. However, the server is currently too busy to support requests greater than %d for this database. See http://go.microsoft.com/fwlink/?LinkId=267637 for assistance. Otherwise, please try again later.``

Weitere Informationen zu diesem Fehler finden Sie unter [Fehlermeldungen für SQL-Datenbank-Clientprogramme](https://docs.microsoft.com/azure/sql-database/sql-database-develop-error-messages).

### <a name="error-40501-the-service-is-currently-busy"></a>Fehler 40501: Der Dienst ist derzeit ausgelastet

**Detaillierter Fehler**

``40501: The service is currently busy. Retry the request after 10 seconds. Incident ID: %ls. Code: %d.``

Dies ist ein Engine-Einschränkungsfehler. Er weist darauf hin, dass Ressourcenlimits überschritten werden.

Weitere Informationen zu Ressourcenlimits finden Sie unter [Datenbankserver-Ressourcenlimits](https://docs.microsoft.com/azure/sql-database/sql-database-resource-limits-database-server).

### <a name="error-40544-the-database-has-reached-its-size-quota"></a>Fehler 40544: Das Größenkontingent der Datenbank wurde erreicht

**Detaillierter Fehler**

``40544: The database has reached its size quota. Partition or delete data, drop indexes, or consult the documentation for possible resolutions. Incident ID: <ID>. Code: <code>.``

Dieser Fehler tritt auf, wenn die Datenbank ihr Größenkontingent erreicht hat.

Mithilfe der folgenden Schritte können Sie das Problem umgehen oder zusätzliche Optionen nutzen:

1. Überprüfen Sie die aktuelle Größe der Datenbank, indem Sie das Dashboard im Azure-Portal verwenden.

   > [!NOTE]
   > Um festzustellen, welche Tabellen den meisten Speicherplatz belegen und somit potenzielle Kandidaten für die Bereinigung sind, führen Sie die folgende SQL-Abfrage aus:

   ```
   SELECT o.name,
    a.SUM(p.row_count) AS 'Row Count',
    b.SUM(p.reserved_page_count) * 8.0 / 1024 AS 'Table Size (MB)'
   FROM sys.objects o
   JOIN sys.dm_db_partition_stats p on p.object_id = o.object_id
   GROUP BY o.name
   ORDER BY [Table Size (MB)] DESC
   ```

2. Wenn die aktuelle Größe die für Ihre Edition maximal unterstützte Größe nicht überschreitet, können Sie mit ALTER DATABASE die MAXSIZE-Einstellung erhöhen. 
3. Wenn die Datenbank bereits die maximal unterstützte Größe für Ihre Edition überschreitet, können Sie einen oder mehrere der folgenden Schritte probieren:
   - Führen Sie normale Aktivitäten zur Bereinigung der Datenbank aus. Bereinigen Sie beispielsweise die unerwünschten Daten mithilfe von TRUNCATE/DELETE, oder verschieben Sie Daten mithilfe von SQL Server Integration Services (SSIS) oder des Hilfsprogramms zum Massenkopieren (Bulk Copy Program, bcp).
   - Partitionieren oder löschen Sie Daten, löschen Sie Indizes, oder informieren Sie sich in der Dokumentation über mögliche Lösungen.

   - Informationen zur Datenbankskalierung finden Sie unter [Skalieren der Ressourcen für einzelne Datenbanken](https://docs.microsoft.com/azure/sql-database/sql-database-single-database-scale) und [Skalieren der Ressourcen für Pools für elastische Datenbanken](https://docs.microsoft.com/azure/sql-database/sql-database-elastic-pool-scale).

### <a name="error-40549-session-is-terminated-because-you-have-a-long-running-transaction"></a>Fehler 40549: Sitzung wurde aufgrund einer zeitintensiven Transaktion beendet

**Detaillierter Fehler**

``40549: Session is terminated because you have a long-running transaction. Try shortening your transaction.``

Wenn diese Fehlermeldung wiederholt auftritt, führen Sie die folgenden Schritte aus, um das Problem zu beheben: 

1. Überprüfen Sie die Sicht sys.dm_exec_requests, um alle geöffneten Sitzungen mit einem hohen Wert in der Spalte total_elapsed_time anzuzeigen. Führen Sie diese Prüfung durch Ausführen des folgenden SQL-Skripts durch:

   ```
   SELECT * FROM dm_exec_requests
   ```
2. Ermitteln Sie den Eingabepuffer der zeitintensiven Abfrage. 
3. Optimieren Sie die Abfrage.

Sie sollten auch Batchverarbeitung für Ihre Abfragen in Erwägung ziehen. Weitere Informationen finden Sie unter [Gewusst wie: Verbessern der Leistung von SQL-Datenbankanwendungen mithilfe von Batchverarbeitung](https://docs.microsoft.com/azure/sql-database/sql-database-use-batching-to-improve-performance).

Eine ausführliche Problembehandlung finden Sie unter [Wird meine Abfrage in der Cloud einwandfrei ausgeführt?](https://blogs.msdn.com/b/sqlblog/archive/2013/11/01/is-my-query-running-fine-in-the-cloud.aspx)

### <a name="error-40551-the-session-has-been-terminated-because-of-excessive-tempdb-usage"></a>Fehler 40551: Die Sitzung wurde aufgrund übermäßiger TEMPDB-Auslastung beendet

**Detaillierter Fehler**

``40551: The session has been terminated because of excessive TEMPDB usage. Try modifying your query to reduce the temporary table space usage.``

Um dieses Problem zu umgehen, führen Sie die folgenden Schritte aus:

1. Ändern Sie die Abfragen, um die Verwendung von temporärem Tabellenspeicherplatz zu verringern. 
2. Löschen Sie temporäre Objekte, wenn sie nicht mehr benötigt werden. 
3. Kürzen Sie Tabellen, oder entfernen Sie nicht verwendete Tabellen.

### <a name="error-40552-the-session-has-been-terminated-because-of-excessive-transaction-log-space-usage"></a>Fehler 40552: Die Sitzung wurde aufgrund übermäßiger Belegung des Speicherplatzes für das Transaktionsprotokoll beendet

**Detaillierter Fehler**

``40552: The session has been terminated because of excessive transaction log space usage. Try modifying fewer rows in a single transaction.``

Probieren Sie die folgenden Methoden aus, um dieses Problem zu beheben:

* Das Problem kann aufgrund von Einfüge-, Aktualisierungs- oder Löschvorgängen auftreten. Versuchen Sie, die Anzahl der Zeilen zu verringern, die sofort ausgeführt werden, indem Sie Batchverarbeitung oder eine Aufteilung in mehrere kleinere Transaktionen implementieren.
* Das Problem kann aufgrund von Vorgängen zum Neuerstellen des Indexes auftreten. Um dieses Problem zu umgehen, stellen Sie sicher, dass die Anzahl der betroffenen Zeilen in der Tabelle multipliziert mit der durchschnittlichen Größe des aktualisierten Felds in Bytes + 80 kleiner als 2 Gigabyte (GB) ist.

  > [!NOTE]
  > Bei einer Indexneuerstellung muss die durchschnittliche Größe des aktualisierten Felds durch die durchschnittliche Indexgröße ersetzt werden.

### <a name="error-40553-the-session-has-been-terminated-because-of-excessive-memory-usage"></a>Fehler 40553: Die Sitzung wurde aufgrund übermäßiger Speicherauslastung beendet

**Detaillierter Fehler**

``40553 : The session has been terminated because of excessive memory usage. Try modifying your query to process fewer rows.``

Um dieses Problem zu umgehen, versuchen Sie, die Abfrage zu optimieren.

Eine ausführliche Problembehandlung finden Sie unter [Wird meine Abfrage in der Cloud einwandfrei ausgeführt?](https://blogs.msdn.com/b/sqlblog/archive/2013/11/01/is-my-query-running-fine-in-the-cloud.aspx)


### <a name="cannot-open-database-master-requested-by-the-login-the-login-failed"></a>Der von der Anmeldung angeforderte „Master“ der Datenbank kann nicht geöffnet werden. Fehler bei der Anmeldung.

Dieses Problem tritt auf, weil das Konto keine Zugriffsberechtigungen für die Masterdatenbank hat. SQL Server Management Studio (SSMS) versucht jedoch standardmäßig, eine Verbindung mit der Masterdatenbank herzustellen.

Gehen Sie folgendermaßen vor, um das Problem zu beheben:

1. Klicken Sie auf dem Anmeldebildschirm von SSMS auf **Optionen** und dann auf **Verbindungseigenschaften**. 
2. Geben Sie in das Feld **Verbindung mit Datenbank herstellen** den standardmäßigen Datenbanknamen des Benutzers als standardmäßige Anmeldedatenbank ein, und klicken Sie dann auf **Verbinden**.

   ![Verbindungseigenschaften](media/troubleshoot-connectivity-issues-microsoft-azure-sql-database/cannot-open-database-master.png)

## <a name="confirm-whether-an-error-is-caused-by-a-connectivity-issue"></a>Bestätigen, dass ein Fehler von einem Konnektivitätsproblem verursacht wird

Um zu bestätigen, dass ein Fehler durch ein Verbindungsproblem verursacht wird, überprüfen Sie die Stapelüberwachung für Frames, die Aufrufe zum Öffnen einer Verbindung wie die folgenden anzeigen (beachten Sie den Verweis auf die **SqlConnection**-Klasse):

```
System.Data.SqlClient.SqlConnection.TryOpen(TaskCompletionSource`1 retry)
 at System.Data.SqlClient.SqlConnection.Open()
 at AzureConnectionTest.Program.Main(String[] args)
ClientConnectionId:<Client connection ID>
```

Wenn die Ausnahme durch Abfrageprobleme ausgelöst wird, ähnelt der Aufrufstapel dem folgenden (beachten Sie den Verweis auf die **SqlCommand**-Klasse). [Optimieren Sie Ihre Abfragen](https://blogs.msdn.com/b/sqlblog/archive/2013/11/01/is-my-query-running-fine-in-the-cloud.aspx) in dieser Situation.

```
  at System.Data.SqlClient.SqlCommand.ExecuteReader()
  at AzureConnectionTest.Program.Main(String[] args)
  ClientConnectionId:<Client ID>
```
Weitere Anleitungen zum Optimieren der Leistung finden Sie in den folgenden Ressourcen:

* [Verwalten von Azure SQL-Indizes und -Statistiken](https://techcommunity.microsoft.com/t5/Azure-Database-Support-Blog/How-to-maintain-Azure-SQL-Indexes-and-Statistics/ba-p/368787)
* [Manuelles Optimieren der Abfrageleistung in Azure SQL-Datenbank](https://docs.microsoft.com/azure/sql-database/sql-database-performance-guidance)
* [Überwachen der Leistung von Azure SQL-Datenbank mit dynamischen Verwaltungssichten](https://docs.microsoft.com/azure/sql-database/sql-database-monitoring-with-dmvs)
* [Verwenden des Abfragespeichers in Azure SQL-Datenbank](https://docs.microsoft.com/azure/sql-database/sql-database-operate-query-store)


## <a name="steps-to-fix-common-connection-issues"></a>Schritte zum Beheben häufiger Verbindungsprobleme

1. Stellen Sie sicher, dass TCP/IP auf dem Anwendungsserver als Clientprotokoll aktiviert ist. Weitere Informationen finden Sie unter [Konfigurieren von Clientprotokollen](https://docs.microsoft.com/sql/database-engine/configure-windows/configure-client-protocols). Überprüfen Sie auf Anwendungsservern, auf denen SQL Server-Tools nicht installiert sind, ob TCP/IP aktiviert ist, indem Sie **cliconfg.exe** (Hilfsprogramm für das SQL Server-Clientnetzwerk) ausführen. 
2. Überprüfen Sie die Verbindungszeichenfolge der Anwendung auf ordnungsgemäße Konfiguration. Stellen Sie z. B. sicher, dass in der Verbindungszeichenfolge der richtige Port (1433) und der vollqualifizierte Servername angegeben sind.
Siehe [Abrufen von SQL Server-Verbindungsinformationen](https://docs.microsoft.com/azure/sql-database/sql-database-connect-query-ssms#get-sql-server-connection-information).
3. Versuchen Sie, den Wert für das Verbindungstimeout zu erhöhen. Wir empfehlen ein Verbindungstimeout von mindestens 30 Sekunden. 
4. Testen Sie die Konnektivität zwischen dem Anwendungsserver und der Azure SQL-Datenbank-Instanz, indem Sie [SQL Server Management Studio (SSMS) ](https://docs.microsoft.com/azure/sql-database/sql-database-connect-query-ssms), eine UDL-Datei, Ping oder Telnet verwenden. Weitere Informationen finden Sie unter [Problembehandlung von SQL Server-Verbindungsproblemen](https://support.microsoft.com/help/4009936/solving-connectivity-errors-to-sql-server) und [Diagnose bei Verbindungsproblemen](https://docs.microsoft.com/azure/sql-database/sql-database-connectivity-issues#diagnostics).

   > [!NOTE]
   > Als Problembehandlungsschritt können Sie die Konnektivität auch auf einem anderen Clientcomputer testen.

5. Stellen Sie als bewährte Methode sicher, dass die Wiederholungslogik vorhanden ist. Weitere Informationen zur Wiederholungslogik finden Sie unter [Arbeiten mit Verbindungsproblemen und vorübergehenden Fehlern bei SQL-Datenbank](https://docs.microsoft.com/azure/sql-database/sql-database-connectivity-issues).

Wenn Ihr Problem mit diesen Schritten nicht behoben werden kann, versuchen Sie, weitere Daten zu erfassen, und wenden Sie sich dann an den Support. Wenn es sich bei Ihrer Anwendung um einen Clouddienst handelt, aktivieren Sie die Protokollierung. Dieser Schritt gibt einen UTC-Zeitstempel des Fehlers zurück. Außerdem gibt SQL Azure die Ablaufverfolgungs-ID zurück. [Microsoft Customer Support Services](https://azure.microsoft.com/support/options/) kann diese Informationen verwenden. 

Weitere Informationen zum Aktivieren der Protokollierung finden Sie unter [Aktivieren der Diagnoseprotokollierung für Web-Apps in Azure App Service](https://azure.microsoft.com/documentation/articles/web-sites-enable-diagnostic-log/).

**Verwandte Dokumente**

* [Verbindungsarchitektur von Azure SQL](https://docs.microsoft.com/azure/sql-database/sql-database-connectivity-architecture)<br>
* [Netzwerkzugriffssteuerung für Azure SQL-Datenbank und Data Warehouse](https://docs.microsoft.com/azure/sql-database/sql-database-networkaccess-overview)
