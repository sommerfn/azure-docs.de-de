---
title: 'Schnellstart: Skalieren von Computeressourcen – T-SQL '
description: Skalieren Sie Computeressourcen in Azure SQL Data Warehouse mithilfe von T-SQL und SQL Server Management Studio (SSMS). Skalieren Sie Computeressourcen zentral hoch, um eine bessere Leistung zu erzielen, oder skalieren Sie sie zentral herunter, um Kosten einzusparen.
services: sql-data-warehouse
author: Antvgski
manager: craigg
ms.service: sql-data-warehouse
ms.topic: quickstart
ms.subservice: implement
ms.date: 04/17/2018
ms.author: anvang
ms.reviewer: igorstan
ms.custom: seo-lt-2019
ms.openlocfilehash: 6729552262d7bea619948ddba406418b80cf69dc
ms.sourcegitcommit: 609d4bdb0467fd0af40e14a86eb40b9d03669ea1
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 11/06/2019
ms.locfileid: "73685943"
---
# <a name="quickstart-scale-compute-in-azure-sql-data-warehouse-using-t-sql"></a>Schnellstart: Skalieren von Computeressourcen in Azure SQL Data Warehouse mithilfe von T-SQL

Skalieren Sie Computeressourcen in Azure SQL Data Warehouse mithilfe von T-SQL und SQL Server Management Studio (SSMS). [Skalieren Sie Computeressourcen zentral hoch](sql-data-warehouse-manage-compute-overview.md), um eine bessere Leistung zu erzielen, oder skalieren Sie sie zentral herunter, um Kosten einzusparen. 

Wenn Sie kein Azure-Abonnement besitzen, können Sie ein [kostenloses Konto](https://azure.microsoft.com/free/) erstellen, bevor Sie beginnen.

## <a name="before-you-begin"></a>Voraussetzungen

Laden Sie die aktuelle Version von [SQL Server Management Studio](/sql/ssms/download-sql-server-management-studio-ssms) (SSMS) herunter, und installieren Sie sie.
 
## <a name="create-a-data-warehouse"></a>Erstellen eines Data Warehouse

Verwenden Sie den [Schnellstart: Erstellen und Abfragen einer Azure SQL Data Warehouse-Instanz im Azure-Portal](create-data-warehouse-portal.md), um ein Data Warehouse namens **mySampleDataWarehouse** zu erstellen. Schließen Sie den Schnellstart ab, um sicherzustellen, dass Sie über eine Firewallregel verfügen und mit Ihrem Data Warehouse in SQL Server Management Studio eine Verbindung herstellen können.

## <a name="connect-to-the-server-as-server-admin"></a>Herstellen einer Verbindung mit dem Server als Serveradministrator

In diesem Abschnitt wird [SQL Server Management Studio](/sql/ssms/download-sql-server-management-studio-ssms) zum Herstellen einer Verbindung mit Ihrem Azure SQL-Server verwendet.

1. Öffnen Sie SQL Server Management Studio.

2. Geben Sie im Dialogfeld **Mit Server verbinden** die folgenden Informationen ein:

   | Einstellung       | Empfohlener Wert | BESCHREIBUNG | 
   | ------------ | ------------------ | ------------------------------------------------- | 
   | Servertyp | Datenbank-Engine | Dieser Wert ist erforderlich. |
   | Servername | Der vollqualifizierte Servername | Beispiel: **mynewserver-20171113.database.windows.net**. |
   | Authentication | SQL Server-Authentifizierung | In diesem Tutorial ist die SQL-Authentifizierung der einzige konfigurierte Authentifizierungstyp. |
   | Anmeldung | Das Serveradministratorkonto | Hierbei handelt es sich um das Konto, das Sie bei der Servererstellung angegeben haben. |
   | Kennwort | Das Kennwort für das Serveradministratorkonto | Hierbei handelt es sich um das Kennwort, das Sie beim Erstellen des Servers angegeben haben. |

    ![Verbindung mit dem Server herstellen](media/load-data-from-azure-blob-storage-using-polybase/connect-to-server.png)

4. Klicken Sie auf **Verbinden**. Die Objekt-Explorer-Fenster wird in SSMS geöffnet. 

5. Erweitern Sie im Objekt-Explorer die Option **Datenbanken**. Erweitern Sie dann **mySampleDatabase**, um die Objekte in der neuen Datenbank anzuzeigen.

    ![Datenbankobjekte](media/create-data-warehouse-portal/connected.png) 

## <a name="view-service-objective"></a>Anzeigen des Dienstziels
Die Einstellung des Dienstziels enthält die Anzahl der Data Warehouse-Einheiten für das Data Warehouse. 

So zeigen Sie die aktuellen Data Warehouse-Einheiten für Ihr Data Warehouse an

1. Erweitern Sie unterhalb der Verbindung mit **mynewserver 20171113.database.windows.net** den Knoten **Systemdatenbanken**.
2. Klicken Sie mit der rechten Maustaste auf **master** und dann auf **Neue Abfrage**. Ein neues Abfragefenster wird geöffnet.
3. Führen Sie die folgende Abfrage aus, um eine Auswahl aus der dynamischen Verwaltungssicht „sys.database_service_objectives“ zu treffen. 

    ```sql
    SELECT
        db.name [Database]
    ,   ds.edition [Edition]
    ,   ds.service_objective [Service Objective]
    FROM
        sys.database_service_objectives ds
    JOIN
        sys.databases db ON ds.database_id = db.database_id
    WHERE 
        db.name = 'mySampleDataWarehouse'
    ```

4. Die folgenden Ergebnisse zeigen, dass für **mySampleDataWarehouse** ein Dienstziel von DW400 besteht. 

    ![Anzeigen der aktuellen DWUs](media/quickstart-scale-compute-tsql/view-current-dwu.png)


## <a name="scale-compute"></a>Skalieren von Computeressourcen
Sie können in SQL Data Warehouse Computeressourcen durch Anpassen der Data Warehouse-Einheiten (DWUs) erhöhen oder verringern. Mit dem [Schnellstart: Erstellen und Abfragen einer Azure SQL Data Warehouse-Instanz im Azure-Portal](create-data-warehouse-portal.md) wurde **mySampleDataWarehouse** erstellt und mit 400 DWUs initialisiert. In den folgenden Schritten werden die DWUs für **mySampleDataWarehouse** angepasst.

So ändern Sie Data Warehouse-Einheiten

1. Klicken Sie mit der rechten Maustaste auf **master** und dann auf **Neue Abfrage**.
2. Mit der T-SQL-Anweisung [ALTER DATABASE](/sql/t-sql/statements/alter-database-azure-sql-database) ändern Sie das Dienstziel. Führen Sie die folgende Abfrage aus, um das Dienstziel in DW300 zu ändern. 

    ```Sql
    ALTER DATABASE mySampleDataWarehouse
    MODIFY (SERVICE_OBJECTIVE = 'DW300c')
    ;
    ```

## <a name="monitor-scale-change-request"></a>Überwachen der Skalierungsänderungsanforderung
Wenn Sie den Fortschritt der vorherigen Änderungsanforderung anzeigen möchten, können Sie unter Verwendung der T-SQL-Syntax `WAITFORDELAY` die dynamische Verwaltungssicht (Dynamic Management View, DMV) „sys.dm_operation_status“ abfragen.

So fragen Sie den Status der Dienstobjektänderung ab:

1. Klicken Sie mit der rechten Maustaste auf **master** und dann auf **Neue Abfrage**.
2. Führen Sie die folgende Abfrage aus, um die DMV „sys.dm_operation_status“ abzufragen.

    ```sql
    WHILE 
    (
        SELECT TOP 1 state_desc
        FROM sys.dm_operation_status
        WHERE 
            1=1
            AND resource_type_desc = 'Database'
            AND major_resource_id = 'MySampleDataWarehouse'
            AND operation = 'ALTER DATABASE'
        ORDER BY
            start_time DESC
    ) = 'IN_PROGRESS'
    BEGIN
        RAISERROR('Scale operation in progress',0,0) WITH NOWAIT;
        WAITFOR DELAY '00:00:05';
    END
    PRINT 'Complete';
    ```
3. Die Ausgabe zeigt ein Protokoll für den Statusabruf.

    ![Vorgangsstatus](media/quickstart-scale-compute-tsql/polling-output.png)

## <a name="check-data-warehouse-state"></a>Überprüfen des Data Warehouse-Zustands

Mit einem angehaltenen Data Warehouse kann über T-SQL keine Verbindung hergestellt werden. Den aktuellen Zustand des Data Warehouse können Sie mithilfe eines PowerShell-Cmdlets anzeigen. Ein entsprechendes Beispiel finden Sie unter [Schnellstart: Skalieren von Computeressourcen in Azure SQL Data Warehouse mithilfe von PowerShell](quickstart-scale-compute-powershell.md#check-data-warehouse-state). 

## <a name="check-operation-status"></a>Überprüfen des Vorgangsstatus

Um Informationen über verschiedene Verwaltungsvorgänge für SQL Data Warehouse zurückzugeben, führen Sie die folgende Abfrage in der DMV [dm_operation_status](/sql/relational-databases/system-dynamic-management-views/sys-dm-operation-status-azure-sql-database) aus. Beispielsweise werden der Vorgang und der Status des Vorgangs („IN_PROGRESS“ oder „COMPLETED“) zurückgegeben.

```sql
SELECT *
FROM
    sys.dm_operation_status
WHERE
    resource_type_desc = 'Database'
AND 
    major_resource_id = 'MySampleDataWarehouse'
```


## <a name="next-steps"></a>Nächste Schritte
In diesem Artikel haben Sie gelernt, wie Sie Computeressourcen für Ihr Data Warehouse skalieren. Weitere Informationen zu Azure SQL Data Warehouse erhalten Sie im Tutorial zum Laden von Daten.

> [!div class="nextstepaction"]
>[Laden von Daten in SQL Data Warehouse](load-data-from-azure-blob-storage-using-polybase.md)
