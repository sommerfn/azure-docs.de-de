---
title: Konfigurieren der Datenreplikation in Azure Database for MariaDB | Microsoft-Dokumentation
description: In diesem Artikel wird beschrieben, wie die Datenreplikation in Azure Database for MariaDB eingerichtet wird.
author: ajlam
ms.author: andrela
ms.service: mariadb
ms.topic: conceptual
ms.date: 09/24/2018
ms.openlocfilehash: 21e8a88cc6f03b4d54a6c5299b0b6be36cc32d6d
ms.sourcegitcommit: f56b267b11f23ac8f6284bb662b38c7a8336e99b
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 06/28/2019
ms.locfileid: "67444803"
---
# <a name="configure-data-in-replication-in-azure-database-for-mariadb"></a>Konfigurieren der Datenreplikation in Azure Database for MariaDB

In diesem Artikel erfahren Sie, wie Sie die Datenreplikation in Azure Database for MariaDB einrichten, indem Sie Master- und Replikatserver konfigurieren. In diesem Artikel wird davon ausgegangen, dass Sie über ein gewisses Maß an Erfahrung mit MariaDB-Servern und -Datenbanken verfügen.

Um ein Replikat im Azure Database for MariaDB-Dienst zu erstellen, synchronisiert die Datenreplikation Daten von einem lokalen MariaDB-Masterserver, virtuellen Computern (VMs) oder Cloud-Datenbankdiensten.

> [!NOTE]
> Wenn Ihr Masterserver die Version 10.2 oder höher hat, wird empfohlen, die Datenreplikation mithilfe der [globalen Transaktions-ID](https://mariadb.com/kb/en/library/gtid/) einzurichten.


## <a name="create-a-mariadb-server-to-use-as-a-replica"></a>Erstellen eines als Replikat zu verwendenden MariaDB-Servers

1. Erstellen Sie einen neuen Azure Database for MariaDB-Server (z.B. replica.mariadb.database.azure.com). Der Server ist bei der Datenreplikation der Replikatserver.

    Informationen zur Servererstellung finden Sie unter [Erstellen eines Azure Database for MariaDB-Servers über das Azure-Portal](quickstart-create-mariadb-server-database-using-azure-portal.md).

   > [!IMPORTANT]
   > Sie müssen den Azure Database for MariaDB-Server in den Tarifen „Universell“ oder „Arbeitsspeicheroptimiert“ erstellen.

2. Erstellen Sie identische Benutzerkonten und entsprechende Berechtigungen.
    
    Benutzerkonten werden nicht vom Masterserver auf den Replikatserver repliziert. Um Benutzern Zugriff auf den Replikatserver zu gewähren, müssen Sie alle Konten und die entsprechenden Berechtigungen für diesen neu erstellten Azure Database for MariaDB-Server manuell erstellen.

## <a name="configure-the-master-server"></a>Konfigurieren des Masterservers

Mit den folgenden Schritten wird der MariaDB-Server, der lokal, auf einer VM oder in einem Cloud-Datenbankdienst gehostet wird, für die Datenreplikation vorbereitet und konfiguriert. Der MariaDB-Server ist bei der Datenreplikation der Master.

1. Aktivieren Sie die binäre Protokollierung.
    
    Um zu prüfen, ob die binäre Protokollierung auf dem Master aktiviert wurde, führen Sie den folgenden Befehl aus:

   ```sql
   SHOW VARIABLES LIKE 'log_bin';
   ```

   Wenn die Variable [`log_bin`](https://mariadb.com/kb/en/library/replication-and-binary-log-server-system-variables/#log_bin) den Wert `ON` zurückgibt, ist die binäre Protokollierung auf Ihrem Server aktiviert.

   Wenn `log_bin` den Wert `OFF` zurückgibt, bearbeiten Sie die Datei **my.cnf**, damit `log_bin=ON` die binäre Protokollierung aktiviert. Starten Sie den Server neu, damit die Änderung wirksam wird.

2. Konfigurieren Sie Masterservereinstellungen.

    Der Parameter `lower_case_table_names` muss bei der Datenreplikation zwischen Master- und Replikatserver konsistent sein. Der Parameter `lower_case_table_names` ist in Azure Database for MariaDB standardmäßig auf `1` festgelegt.

   ```sql
   SET GLOBAL lower_case_table_names = 1;
   ```

3. Erstellen Sie eine neue Replikationsrolle, und richten Sie Berechtigungen ein.

   Erstellen Sie auf dem Masterserver ein Benutzerkonto, das mit Replikationsberechtigungen konfiguriert ist. Sie können ein Konto mit SQL-Befehlen oder MySQL Workbench erstellen. Wenn Sie eine Replikation mit SSL planen, müssen Sie dies bei der Erstellung des Benutzerkontos angeben.
   
   Wie Ihrem Masterserver Benutzerkonten hinzugefügt werden, erfahren Sie in der [MariaDB-Dokumentation](https://mariadb.com/kb/en/library/create-user/).

   Bei Verwenden der folgenden Befehle kann die neu erstellte Replikationsrolle nicht nur vom Computer, auf dem der Masterserver selbst gehostet wird, sondern von jedem Computer aus auf den Masterserver zugreifen. Geben Sie für diesen Zugriff im Befehl **syncuser\@'%'** an, um einen Benutzer zu erstellen.
   
   Weitere Informationen finden Sie in der MariaDB-Dokumentation unter [Specifying Account Names](https://mariadb.com/kb/en/library/create-user/#account-names) (Angeben von Kontonamen).

   **SQL-Befehl**

   - Replikation mit SSL

       Um für alle Benutzerverbindungen SSL zu erzwingen, geben Sie den folgenden Befehl zum Erstellen eines Benutzers ein:

       ```sql
       CREATE USER 'syncuser'@'%' IDENTIFIED BY 'yourpassword';
       GRANT REPLICATION SLAVE ON *.* TO ' syncuser'@'%' REQUIRE SSL;
       ```

   - Replikation ohne SSL

       Wenn SSL nicht für alle Benutzerverbindungen erforderlich ist, geben Sie den folgenden Befehl zum Erstellen eines Benutzers ein:
    
       ```sql
       CREATE USER 'syncuser'@'%' IDENTIFIED BY 'yourpassword';
       GRANT REPLICATION SLAVE ON *.* TO ' syncuser'@'%';
       ```

   **MySQL Workbench**

   Um die Replikationsrolle in MySQL Workbench zu erstellen, wählen Sie im Bereich **Management** (Verwaltung) **Users and Privileges** (Benutzer und Berechtigungen) aus. Klicken Sie dann auf **Add Account** (Konto hinzufügen).
 
   ![Benutzer und Berechtigungen](./media/howto-data-in-replication/users_privileges.png)

   Geben Sie in das Feld **Login Name** (Anmeldename) einen Benutzernamen ein.

   ![Benutzersynchronisierung](./media/howto-data-in-replication/syncuser.png)
 
   Wählen Sie den Bereich **Administratorrollen** (Administrative Roles) und dann in der Liste **Global Privileges** (Globale Berechtigungen) **Replication Slave** (Replikationsslave) aus. Klicken Sie dann auf **Apply** (Übernehmen), um die Replikationsrolle zu erstellen.

   ![Replikationsslave](./media/howto-data-in-replication/replicationslave.png)


4. Versetzen Sie den Masterserver in den schreibgeschützten Modus.

   Bevor Sie eine Datenbank sichern, muss der Server in den schreibgeschützten Modus versetzt werden. Im schreibgeschützten Modus kann der Masterserver keine Schreibtransaktionen verarbeiten. Um Beeinträchtigung des Geschäftsbetriebs zu vermeiden, planen Sie das Versetzen in den schreibgeschützten Modus außerhalb der Spitzenzeiten ein.

   ```sql
   FLUSH TABLES WITH READ LOCK;
   SET GLOBAL read_only = ON;
   ```

5. Rufen Sie den Namen und Offset der aktuellen binären Protokolldatei ab.

   Führen Sie den Befehl [`show master status`](https://mariadb.com/kb/en/library/show-master-status/) aus, um Name und Offset der aktuellen binären Protokolldatei zu ermitteln.
    
   ```sql
   show master status;
   ```
   Die Ergebnisse sollten in etwa wie in der folgenden Tabelle aussehen:
   
   ![Statusergebnisse des Masters](./media/howto-data-in-replication/masterstatus.png)

   Notieren Sie sich den Namen der Binärdatei, da dieser bei den nachfolgenden Schritten benötigt wird.
   
6. Rufen Sie (optional) die GTID-Position ab (die für die Replikation mit GTID benötigt wird).

   Führen Sie die Funktion [`BINLOG_GTID_POS`](https://mariadb.com/kb/en/library/binlog_gtid_pos/) aus, um die GTID-Position für den entsprechenden Dateinamen und Offset der binären Protokolldatei zu erhalten.
  
    ```sql
    select BINLOG_GTID_POS('<binlog file name>', <binlog offset>);
    ```
 

## <a name="dump-and-restore-the-master-server"></a>Sichern und Wiederherstellen des Masterservers

1. Sichern Sie alle Datenbanken auf dem Masterserver.

   Verwenden Sie „mysqldump“, um alle Datenbanken auf dem Masterserver zu sichern. Die MySQL- und Testbibliothek müssen nicht gesichert werden.

    Weitere Informationen finden Sie unter [Dump and restore](howto-migrate-dump-restore.md) (Sichern und Wiederherstellen).

2. Versetzen Sie den Masterserver in den Lese-/Schreibmodus.

   Nachdem die Datenbank gesichert wurde, versetzen Sie den MariaDB-Masterserver wieder in den Lese-/Schreibmodus.

   ```sql
   SET GLOBAL read_only = OFF;
   UNLOCK TABLES;
   ```

3. Stellen Sie die Sicherungsdatei auf dem neuen Server wieder her.

   Stellen Sie die Sicherungsdatei auf dem Server wieder her, der im Dienst Azure Database for MariaDB erstellt wurde. Informationen zum Wiederherstellen einer Sicherungsdatei auf einem MariaDB-Server finden Sie unter [Migrieren der MariaDB-Datenbank zu Azure Database for MariaDB durch Sicherungen und Wiederherstellungen](howto-migrate-dump-restore.md).

   Wenn es sich um eine große Sicherungsdatei handelt, laden Sie sie auf eine VM in der Region Ihres Replikatservers in Azure hoch. Stellen Sie sie von der VM aus auf dem Azure Database for MariaDB-Server wieder her.

## <a name="link-the-master-and-replica-servers-to-start-data-in-replication"></a>Verknüpfen des Master- und Replikatservers zum Starten der Datenreplikation

1. Bereiten Sie den Masterserver vor.

   Alle Datenreplikationsfunktionen erfolgen über gespeicherte Prozeduren. Alle Prozeduren finden Sie unter [Gespeicherte Prozeduren für Datenreplikationen in Azure Database for MySQL](reference-data-in-stored-procedures.md). Gespeicherte Prozeduren können in der MySQL-Shell oder MySQL Workbench ausgeführt werden.

   Um zwei Server zu verknüpfen und die Replikation zu starten, melden Sie sich beim Zielreplikatserver im Dienst Azure Database for MariaDB an. Legen Sie anschließend die externe Instanz als Masterserver fest, indem Sie die gespeicherte Prozedur `mysql.az_replication_change_master` oder `mysql.az_replication_change_master_with_gtid` auf dem Azure DB for MariaDB-Server verwenden.

   ```sql
   CALL mysql.az_replication_change_master('<master_host>', '<master_user>', '<master_password>', 3306, '<master_log_file>', <master_log_pos>, '<master_ssl_ca>');
   ```
   
   oder
   
   ```sql
   CALL mysql.az_replication_change_master_with_gtid('<master_host>', '<master_user>', '<master_password>', 3306, '<master_gtid_pos>', '<master_ssl_ca>');
   ```

   - master_host: Hostname des Masterservers
   - master_user: Benutzername des Masterservers
   - master_password: Kennwort des Masterservers
   - master_log_file: Name der binären Protokolldatei durch Ausführung von `show master status`
   - master_log_pos: Position des binären Protokolls durch Ausführung von `show master status`
   - master_gtid_pos: GTID-Position nach Ausführen von `select BINLOG_GTID_POS('<binlog file name>', <binlog offset>);`
   - master_ssl_ca: Der Kontext des Zertifizierungsstellenzertifikats. Wenn SSL nicht verwendet wird, übergeben Sie eine leere Zeichenfolge.*
    
    
    *Es wird empfohlen, den Parameter „master_ssl_ca“ als Variable zu übergeben. Weitere Informationen finden Sie in den folgenden Beispielen.

   **Beispiele**

   - Replikation mit SSL

       Erstellen Sie die Variable `@cert` durch Ausführen der folgenden Befehle:

       ```sql
       SET @cert = '-----BEGIN CERTIFICATE-----
       PLACE YOUR PUBLIC KEY CERTIFICATE’S CONTEXT HERE
       -----END CERTIFICATE-----'
       ```

       Eine Replikation mit SSL wird zwischen einem Masterserver, der in der Domäne companya.com gehostet wird, und einem Replikatserver eingerichtet, der in Azure Database for MariaDB gehostet wird. Diese gespeicherte Prozedur wird auf dem Replikat ausgeführt.
    
       ```sql
       CALL mysql.az_replication_change_master('master.companya.com', 'syncuser', 'P@ssword!', 3306, 'mariadb-bin.000016', 475, @cert);
       ```
   - Replikation ohne SSL

       Eine Replikation ohne SSL wird zwischen einem Masterserver, der in der Domäne companya.com gehostet wird, und einem Replikatserver eingerichtet, der in Azure Database for MariaDB gehostet wird. Diese gespeicherte Prozedur wird auf dem Replikat ausgeführt.

       ```sql
       CALL mysql.az_replication_change_master('master.companya.com', 'syncuser', 'P@ssword!', 3306, 'mariadb-bin.000016', 475, '');
       ```

2. Starten Sie die Replikation.

   Rufen Sie die gespeicherte Prozedur `mysql.az_replication_start` auf, um die Replikation zu starten.

   ```sql
   CALL mysql.az_replication_start;
   ```

3. Überprüfen Sie den Replikationsstatus.

   Rufen Sie den Befehl [`show slave status`](https://mariadb.com/kb/en/library/show-slave-status/) auf dem Replikatserver auf, um den Replikationsstatus anzuzeigen.
    
   ```sql
   show slave status;
   ```

   Wenn `yes` der Status von `Slave_IO_Running` und `Slave_SQL_Running` ist und `0` der Wert von `Seconds_Behind_Master` ist, funktioniert die Replikation ordnungsgemäß. `Seconds_Behind_Master` gibt an, wie stark das Replikat verzögert ist. Wenn der Wert nicht `0` ist, bedeutet dies, dass das Replikat momentan Updates verarbeitet.

4. Aktualisieren Sie die entsprechenden Servervariablen, um die Datenreplikation sicherer zu machen (nur für die Replikation ohne GTID erforderlich).
    
    Aufgrund einer nativen Replikationseinschränkung in MariaDB müssen Sie bei der Replikation ohne GTID-Szenario die Variablen [`sync_master_info`](https://mariadb.com/kb/en/library/replication-and-binary-log-system-variables/#sync_master_info) und [`sync_relay_log_info`](https://mariadb.com/kb/en/library/replication-and-binary-log-system-variables/#sync_relay_log_info) festlegen.

    Überprüfen Sie die Variablen `sync_master_info` und `sync_relay_log_info` Ihres Slaveservers, um sicherzustellen, dass die Datenreplikation stabil ist, und legen Sie die Variablen auf `1` fest.
    
## <a name="other-stored-procedures"></a>Andere gespeicherte Prozeduren

### <a name="stop-replication"></a>Beenden der Replikation

Um die Replikation zwischen dem Master- und Replikatserver zu beenden, verwenden Sie die folgende gespeicherte Prozedur:

```sql
CALL mysql.az_replication_stop;
```

### <a name="remove-the-replication-relationship"></a>Entfernen der Replikationsbeziehung

Um die Replikationsbeziehung zwischen Master- und Replikatserver zu entfernen, verwenden Sie die folgende gespeicherte Prozedur:

```sql
CALL mysql.az_replication_remove_master;
```

### <a name="skip-the-replication-error"></a>Überspringen des Replikationsfehlers

Um einen Replikationsfehler zu überspringen und die Replikation zuzulassen, verwenden Sie die folgende gespeicherte Prozedur:
    
```sql
CALL mysql.az_replication_skip_counter;
```

## <a name="next-steps"></a>Nächste Schritte
Informieren Sie sich über die [Datenreplikation](concepts-data-in-replication.md) für Azure Database for MariaDB.
