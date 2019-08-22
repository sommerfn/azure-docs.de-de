---
title: Artikel zu bekannten Problemen/Migrationseinschränkungen bei Onlinemigrationen zu Azure Database for MySQL | Microsoft-Dokumentation
description: Informationen zu bekannten Problemen/Migrationseinschränkungen bei Onlinemigrationen zu Azure Database for MySQL.
services: database-migration
author: HJToland3
ms.author: jtoland
manager: craigg
ms.reviewer: craigg
ms.service: dms
ms.workload: data-services
ms.custom: mvc
ms.topic: article
ms.date: 08/06/2019
ms.openlocfilehash: fc5565ab9e3be21b96ce5aa5a938cf22ec3caeb0
ms.sourcegitcommit: 670c38d85ef97bf236b45850fd4750e3b98c8899
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 08/08/2019
ms.locfileid: "68848483"
---
# <a name="known-issuesmigration-limitations-with-online-migrations-to-azure-db-for-mysql"></a>Bekannte Probleme/Migrationseinschränkungen bei Onlinemigrationen zu Azure DB for MySQL

In den folgenden Abschnitten werden bekannte Probleme und Einschränkungen in Bezug auf Onlinemigrationen von MySQL zu Azure Database for MySQL beschrieben.

## <a name="online-migration-configuration"></a>Konfiguration der Onlinemigration

- Die MySQL-Version des Quellservers muss 5.6.35, 5.7.18 oder höher sein.
- Azure Database for MySQL unterstützt Folgendes:
  - MySQL Community-Edition
  - InnoDB-Engine
- Migration zu derselben Version. Die Migration von MySQL 5.6 zu Azure Database for MySQL 5.7 wird nicht unterstützt.
- Aktivieren Sie die binäre Protokollierung in „my.ini“ (Windows) oder „my.cnf“ (Unix).
  - Legen Sie „Server_id“ auf eine beliebige Zahl größer oder gleich 1 fest, z.B. Server_id=1 (nur für MySQL 5.6).
  - Legen Sie „log-bin = \<Pfad>“ fest (nur für MySQL 5.6).
  - Legen Sie „binlog_format = row“ fest.
  - „Expire_logs_days = 5“ (empfohlen – nur für MySQL 5.6)
- Der Benutzer muss die Rolle „ReplicationAdmin“ besitzen.
- Die für die MySQL-Quelldatenbank definierten Sortierungen sind identisch mit den in der Azure Database for MySQL-Zielinstanz definierten Sortierungen.
- Das Schema muss zwischen der MySQL-Quelldatenbank und der Zieldatenbank in Azure Database for MySQL übereinstimmen.
- Das Schema in der Azure Database for MySQL-Zielinstanz darf keine Fremdschlüssel aufweisen. Verwenden Sie die folgende Abfrage, um Fremdschlüssel zu löschen:
    ```
    SET group_concat_max_len = 8192;
    SELECT SchemaName, GROUP_CONCAT(DropQuery SEPARATOR ';\n') as DropQuery, GROUP_CONCAT(AddQuery SEPARATOR ';\n') as AddQuery
    FROM
    (SELECT 
    KCU.REFERENCED_TABLE_SCHEMA as SchemaName, KCU.TABLE_NAME, KCU.COLUMN_NAME,
        CONCAT('ALTER TABLE ', KCU.TABLE_NAME, ' DROP FOREIGN KEY ', KCU.CONSTRAINT_NAME) AS DropQuery,
        CONCAT('ALTER TABLE ', KCU.TABLE_NAME, ' ADD CONSTRAINT ', KCU.CONSTRAINT_NAME, ' FOREIGN KEY (`', KCU.COLUMN_NAME, '`) REFERENCES `', KCU.REFERENCED_TABLE_NAME, '` (`', KCU.REFERENCED_COLUMN_NAME, '`) ON UPDATE ',RC.UPDATE_RULE, ' ON DELETE ',RC.DELETE_RULE) AS AddQuery
        FROM INFORMATION_SCHEMA.KEY_COLUMN_USAGE KCU, information_schema.REFERENTIAL_CONSTRAINTS RC
        WHERE
          KCU.CONSTRAINT_NAME = RC.CONSTRAINT_NAME
          AND KCU.REFERENCED_TABLE_SCHEMA = RC.UNIQUE_CONSTRAINT_SCHEMA
      AND KCU.REFERENCED_TABLE_SCHEMA = ['schema_name']) Queries
      GROUP BY SchemaName;
    ```

    Führen Sie „drop foreign key“ (zweite Spalte) im Abfrageergebnis aus.
- Das Schema in der Azure Database for MySQL-Zielinstanz darf keine Trigger aufweisen. So löschen Sie Trigger in der Zieldatenbank
    ```
    SELECT Concat('DROP TRIGGER ', Trigger_Name, ';') FROM  information_schema.TRIGGERS WHERE TRIGGER_SCHEMA = 'your_schema';
    ```

## <a name="datatype-limitations"></a>Einschränkungen bei Datentypen

- **Einschränkung**: Wenn die MySQL-Quelldatenbank einen JSON-Datentyp enthält, tritt bei der Migration während der fortlaufenden Synchronisierung ein Fehler auf.

    **Problemumgehung**: Ändern Sie den JSON-Datentyp in der MySQL-Quelldatenbank in „mediumtext“ oder „longtext“.

- **Einschränkung**: Wenn in Tabellen kein Primärschlüssel vorhanden ist, tritt bei der fortlaufenden Synchronisierung ein Fehler auf.

    **Problemumgehung**: Legen Sie vorübergehend einen Primärschlüssel für die Tabelle fest, damit die Migration fortgesetzt wird. Sie können den Primärschlüssel nach Abschluss der Datenmigration entfernen.

## <a name="lob-limitations"></a>LOB-Einschränkungen

LOB-Spalten (Large Object) sind Spalten, die groß werden können. Bei MySQL sind „Mediumtext“, „Longtext“, „Blob“, „Mediumblob“, „Longblob“ usw. einige der LOB-Datentypen.

- **Einschränkung**: Wenn LOB-Datentypen als Primärschlüssel verwendet werden, tritt bei der Migration ein Fehler auf.

    **Problemumgehung**: Ersetzen Sie den Primärschlüssel durch andere Datentypen oder Spalten, die keine LOB-Datentypen oder LOB-Spalten sind.

- **Einschränkung**: Wenn die Länge der LOB-Spalte (Large Object) 32 KB überschreitet, werden die Daten möglicherweise am Ziel abgeschnitten. Mithilfe der folgenden Abfrage können Sie die Länge der LOB-Spalte überprüfen:
    ```
    SELECT max(length(description)) as LEN from catalog;
    ```

    **Problemumgehung**: Wenden Sie sich bei einem LOB-Objekt, das größer als 32 KB ist, unter [Fragen zur Azure-Datenbankmigration](mailto:AskAzureDatabaseMigrations@service.microsoft.com) an das Entwicklerteam. 

## <a name="limitations-when-migrating-online-from-aws-rds-mysql"></a>Einschränkungen bei der Onlinemigration von AWS RDS MySQL

Beim Versuch der Onlinemigration von AWS RDS MySQL zu Azure Database for MySQL können die folgenden Fehler auftreten.

- **Fehler:** Datenbank „{0}“ verfügt auf dem Ziel über Fremdschlüssel. Beheben Sie den Fehler für das Ziel, und starten Sie eine neue Aktivität für die Datenmigration. Führen Sie das unten angegebene Skript auf dem Ziel aus, um die Fremdschlüssel aufzulisten.

  **Einschränkung**: Enthält das Schema Fremdschlüssel, tritt beim ersten Ladevorgang und bei der fortlaufenden Synchronisierung der Migration ein Fehler auf.
  **Problemumgehung**: Führen Sie das folgende Skript in MySQL Workbench aus, um das Skript zum Löschen des Fremdschlüssels und das Skript zum Hinzufügen des Fremdschlüssels zu extrahieren:

  ```
  SET group_concat_max_len = 8192; SELECT SchemaName, GROUP_CONCAT(DropQuery SEPARATOR ';\n') as DropQuery, GROUP_CONCAT(AddQuery SEPARATOR ';\n') as AddQuery FROM (SELECT KCU.REFERENCED_TABLE_SCHEMA as SchemaName, KCU.TABLE_NAME, KCU.COLUMN_NAME, CONCAT('ALTER TABLE ', KCU.TABLE_NAME, ' DROP FOREIGN KEY ', KCU.CONSTRAINT_NAME) AS DropQuery, CONCAT('ALTER TABLE ', KCU.TABLE_NAME, ' ADD CONSTRAINT ', KCU.CONSTRAINT_NAME, ' FOREIGN KEY (`', KCU.COLUMN_NAME, '`) REFERENCES `', KCU.REFERENCED_TABLE_NAME, '` (`', KCU.REFERENCED_COLUMN_NAME, '`) ON UPDATE ',RC.UPDATE_RULE, ' ON DELETE ',RC.DELETE_RULE) AS AddQuery FROM INFORMATION_SCHEMA.KEY_COLUMN_USAGE KCU, information_schema.REFERENTIAL_CONSTRAINTS RC WHERE KCU.CONSTRAINT_NAME = RC.CONSTRAINT_NAME AND KCU.REFERENCED_TABLE_SCHEMA = RC.UNIQUE_CONSTRAINT_SCHEMA AND KCU.REFERENCED_TABLE_SCHEMA = 'SchemaName') Queries GROUP BY SchemaName;
  ```

- **Fehler:** Die Datenbank „{0}“ ist auf dem Server nicht vorhanden. Für den angegebenen MySQL-Quellserver wird die Groß-/Kleinschreibung beachtet. Überprüfen Sie den Datenbanknamen.

  **Einschränkung**: Für Benutzer tritt dieser Fehler ggf. auf, wenn sie eine MySQL-Datenbank über die Befehlszeilenschnittstelle (CLI) zu Azure migrieren. Der Dienst konnte die Datenbank auf dem Quellserver nicht finden. Möglicherweise wurde ein falscher Datenbankname angegeben, oder die Datenbank ist auf dem angegebenen Server nicht vorhanden. Beachten Sie, dass bei Datenbanknamen die Groß-/Kleinschreibung berücksichtigt wird.

  **Problemumgehung**: Geben Sie den genauen Datenbanknamen an, und wiederholen Sie den Vorgang.

- **Fehler:** Die Datenbank „{database}“ enthält Tabellen mit demselben Namen. Azure Database for MySQL unterscheidet bei Tabellennamen keine Groß-/Kleinschreibung.

  **Einschränkung**: Dieser Fehler tritt auf, wenn in der Quelldatenbank zwei Tabellen den gleichen Namen haben. Azure Database for MySQL unterscheidet bei Tabellennamen keine Groß-/Kleinschreibung.

  **Problemumgehung**: Aktualisieren Sie die Tabellennamen, sodass sie eindeutig sind, und wiederholen Sie dann den Vorgang.

- **Fehler:** Die Zieldatenbank „{database}“ ist leer. Migrieren Sie das Schema.

  **Einschränkung**: Dieser Fehler tritt auf, wenn die Azure Database for MySQL-Zieldatenbank nicht über das erforderliche Schema verfügt. Die Schemamigration ist erforderlich, um Daten zu Ihrem Ziel migrieren zu können.

  **Problemumgehung**: [Migrieren Sie das Schema](https://docs.microsoft.com/azure/dms/tutorial-mysql-azure-mysql-online#migrate-the-sample-schema) von Ihrer Quell- zur Zieldatenbank.

## <a name="other-limitations"></a>Weitere Einschränkungen

- Kennwortzeichenfolgen mit öffnender und schließender geschweifter Klammer {  } am Anfang und Ende der Kennwortzeichenfolge werden nicht unterstützt. Diese Einschränkung gilt für Verbindungen mit der MySQL-Quelldatenbank sowie der Azure Database for MySQL-Zielinstanz.
- Die folgenden DDLs werden nicht unterstützt:
  - Alle DDLs von Partitionen
  - DROP TABLE
  - Umbenennen von Tabellen
- Die Verwendung der Anweisung *alter table <Tabellenname> add column <Spaltenname>* zum Hinzufügen von Spalten am Anfang oder in der Mitte einer Tabelle wird nicht unterstützt. Mit der Anweisung *alter table <Tabellenname> add column <Spaltenname>* wird eine Spalte am Ende der Tabelle hinzugefügt.
- Indizes, die nur für einen Teil der Spaltendaten erstellt werden, werden nicht unterstützt. Mit der folgenden Beispielanweisung wird ein Index mit nur einem Teil der Spaltendaten erstellt:

    ``` 
    CREATE INDEX partial_name ON customer (name(10));
    ```

- In DMS ist die Anzahl der in einer einzelnen Migrationsaktivität zu migrierenden Datenbanken auf vier Datenbanken beschränkt.
