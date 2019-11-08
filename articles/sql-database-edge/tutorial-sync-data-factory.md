---
title: Synchronisieren von Daten aus Azure SQL Database Edge unter Verwendung von Azure Data Factory | Microsoft-Dokumentation
description: Hier erfahren Sie, wie Sie Daten zwischen Azure SQL Database Edge und Azure Blob Storage synchronisieren.
keywords: SQL Database Edge, Synchronisieren von Daten aus SQL Database Edge, SQL Database Edge-Data Factory
services: sql-database-edge
ms.service: sql-database-edge
ms.topic: tutorial
author: SQLSourabh
ms.author: sourabha
ms.reviewer: sstein
ms.date: 11/04/2019
ms.openlocfilehash: 0e75da9516303bb4250b6847a4b381d07b3d7dad
ms.sourcegitcommit: c22327552d62f88aeaa321189f9b9a631525027c
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 11/04/2019
ms.locfileid: "73509204"
---
# <a name="tutorial-sync-data-from-sql-database-edge-to-azure-blob-storage-using-azure-data-factory"></a>Tutorial: Synchronisieren von Daten aus SQL Database Edge mit Azure Blob Storage unter Verwendung von Azure Data Factory

In diesem Tutorial verwenden Sie Azure Data Factory, um Daten aus einer Tabelle in einer Instanz von Azure SQL Database Edge inkrementell mit Azure Blob Storage zu synchronisieren.

## <a name="before-you-begin"></a>Voraussetzungen

Falls Sie in Ihrer Azure SQL Database Edge-Bereitstellung noch keine Datenbank oder Tabelle erstellt haben, verwenden Sie eine der folgenden Erstellungsmethoden:

* Stellen Sie mithilfe von [SQL Server Management Studio](/sql/ssms/download-sql-server-management-studio-ssms/) oder [Azure Data Studio](/sql/azure-data-studio/download/) eine Verbindung mit SQL Database Edge her, und führen Sie ein SQL-Skript zum Erstellen der Datenbank und der Tabelle aus.
* Erstellen Sie eine SQL-Datenbank und eine Tabelle mithilfe von [sqlcmd](/sql/tools/sqlcmd-utility/), indem Sie eine direkte Verbindung mit dem SQL Database Edge-Modul herstellen. Weitere Informationen finden Sie unter [Herstellen einer Verbindung mit der Datenbank-Engine mithilfe von „sqlcmd“](/sql/ssms/scripting/sqlcmd-connect-to-the-database-engine/).
* Verwenden Sie „SQLPackage.exe“, um eine DACPAC-Datei im SQL Database Edge-Container bereitzustellen. Dieser Vorgang kann automatisiert werden. Geben Sie hierzu im Rahmen der Konfiguration der gewünschten Moduleigenschaften den URI der SQLPackage-Datei an, oder stellen Sie direkt über das Clienttool „SqlPackage.exe“ eine DACPAC-Datei für SQL Database Edge bereit.

    Informationen zum Herunterladen von „sqlpackage“ finden Sie unter [Herunterladen und Installieren von „sqlpackage“](/sql/tools/sqlpackage-download/). Folgende Beispielbefehle für „SqlPackage.exe“ werden bereitgestellt. Weitere Informationen finden Sie in der Dokumentation zu „sqlpackage“.

    **Bereitstellen der DACPAC-Datei:**

    ```cmd
    sqlpackage /Action:Extract /SourceConnectionString:"Data Source=<Server_Name>,<port>;Initial Catalog=<DB_name>;User ID=<user>;Password=<password>" /TargetFile:<dacpac_file_name> 
    ```

    **Anwenden der DACPAC-Datei:**

    ```cmd
    sqlpackage /Action:Publish /Sourcefile:<dacpac_file_name> /TargetServerName:<Server_Name>,<port> /TargetDatabaseName:<DB_Name> /TargetUser:<user> /TargetPassword:<password>
    ```

## <a name="create-a-sql-table-and-procedure-to-store-and-update-the-watermark-levels"></a>Erstellen einer SQL-Tabelle und einer Prozedur zum Speichern und Aktualisieren der Grenzwertstufen

Die Grenzwerttabelle dient zum Speichern des letzten Zeitstempels, bis zu dem die Daten bereits mit dem Azure-Speicher synchronisiert wurden. Die gespeicherte T-SQL-Prozedur (Transact-SQL) dient dazu, die Grenzwerttabelle nach jeder Synchronisierung zu aktualisieren. 

Führen Sie für die SQL Database Edge-Instanz die folgenden Befehle aus:

```sql
    Create table [dbo].[watermarktable]
    (
    TableName varchar(255),
    WatermarkValue datetime,
    )
    GO

    CREATE PROCEDURE usp_write_watermark @timestamp datetime, @TableName varchar(50)  
    AS  
    BEGIN
    UPDATE [dbo].[watermarktable]
    SET [WatermarkValue] = @timestamp WHERE [TableName] = @TableName
    END
    Go
```

## <a name="create-a-data-factory-workflow"></a>Erstellen eines Data Factory-Workflows

In diesem Abschnitt erstellen Sie eine Azure Data Factory-Pipeline, um Daten aus einer Tabelle in Azure SQL Database Edge mit Azure Blob Storage zu synchronisieren.

### <a name="create-data-factory-using-the-data-factory-ui"></a>Erstellen einer Data Factory über die Data Factory-Benutzeroberfläche

Erstellen Sie eine Data Factory gemäß der Anleitung in [diesem Tutorial](../data-factory/quickstart-create-data-factory-portal.md#create-a-data-factory).

### <a name="create-a-data-factory-pipeline"></a>Erstellen einer Data Factory-Pipeline

1. Wählen Sie auf der Seite **Erste Schritte** der Data Factory-Benutzeroberfläche die Kachel **Pipeline erstellen** aus.

    ![Data Factory: Erstellen der Pipeline](media/tutorial-sync-data-factory/data-factory-get-started.png)

2. Geben Sie im Fenster **Eigenschaften** der Pipeline auf der Seite **Allgemein** den Namen **PeriodicSync** ein.

3. Fügen Sie die Aktivität **Suche** hinzu, um den alten Grenzwert abzurufen. Erweitern Sie in der **Aktivitätentoolbox** die Option **Allgemein**, und ziehen Sie die Aktivität **Suche** mittels Drag & Drop auf die Oberfläche des Pipeline-Designers. Ändern Sie den Namen der Aktivität in *OldWatermark*.

    ![Suche nach altem Grenzwert](media/tutorial-sync-data-factory/create-old-watermark-lookup.png)

4. Wechseln Sie zur Registerkarte **Einstellungen**, und wählen Sie unter **Source Dataset** (Quelldataset) die Option **+ Neu** aus. In diesem Schritt erstellen Sie ein Dataset für Daten in der Grenzwerttabelle. Diese Tabelle enthält den alten Grenzwert, der für den vorherigen Kopiervorgang verwendet wurde.

5. Wählen Sie im Fenster **Neues Dataset** die Option **Azure SQL Server** und anschließend **Weiter** aus.  

6. Geben Sie im Fenster **Eigenschaften festlegen** unter „Name“ den Namen *WatermarkDataset* ein.

7. Wählen Sie unter **Verknüpfter Dienst** die Option **Neu** aus, und gehen Sie anschließend wie folgt vor:

    1. Geben Sie unter **Name** die Zeichenfolge *SQLDBEdgeLinkedService* ein.

    2. Geben Sie unter **Servername** die Details Ihres SQL Database Edge-Servers ein.

    3. Geben Sie über die Dropdownliste **Datenbankname** den Namen Ihrer Datenbank an.

    4. Geben Sie unter **Benutzername** Ihren Benutzernamen und unter **Kennwort** Ihr Kennwort ein.

    5. Wählen Sie **Verbindung testen** aus, um die Verbindung mit der SQL Database Edge-Instanz zu testen.

    6. Klicken Sie auf **Erstellen**.

    ![Erstellen eines verknüpften Diensts](media/tutorial-sync-data-factory/create-linked-service.png)

    7. Wählen Sie **OK** aus.

8. Wählen Sie auf der Registerkarte **Einstellungen** die Option **Bearbeiten** aus.

9. Wählen Sie auf der Registerkarte **Verbindung** unter **Tabelle** die Option *[dbo].[watermarktable]* aus. Wählen Sie **Datenvorschau** aus, falls Sie eine Vorschau für die Daten in der Tabelle anzeigen möchten.

10. Wechseln Sie zum Pipeline-Editor, indem Sie oben die Registerkarte „Pipeline“ oder in der Strukturansicht auf der linken Seite den Namen der Pipeline auswählen. Vergewissern Sie sich im Eigenschaftenfenster für die Aktivität **Suche**, dass im Feld **Source Dataset** (Quelldataset) die Option **WatermarkDataset** ausgewählt ist.

11. Erweitern Sie in der **Aktivitätentoolbox** die Option **Allgemein**, und ziehen Sie eine weitere Aktivität vom Typ **Suche** mittels Drag & Drop auf die Oberfläche des Pipeline-Designers. Legen Sie dann den Namen im Eigenschaftenfenster auf der Registerkarte **Allgemein** auf **NewWatermark** fest. Mit dieser Lookup-Aktivität wird der neue Grenzwert aus der Tabelle mit den Quelldaten auf das Ziel kopiert.

12. Wechseln Sie im Eigenschaftenfenster für die zweite Aktivität vom Typ **Suche** zur Registerkarte **Einstellungen**, und wählen Sie **Neu** aus, um ein Dataset zu erstellen, das auf die Quelltabelle mit dem neuen Grenzwert verweist.

13. Wählen Sie im Fenster **Neues Dataset** die SQL Database Edge-Instanz und anschließend **Weiter** aus.

    1. Geben Sie im Fenster **Eigenschaften festlegen** unter **Name** den Namen **SourceDataset** ein. Wählen Sie unter „Verknüpfter Dienst“ die Option *SQLDBEdgeLinkedService* aus.

    2. Wählen Sie unter „Tabelle“ ***die zu synchronisierende Tabelle*** aus. Sie können auch eine Abfrage für dieses Dataset angeben, wie weiter unten in diesem Tutorial erläutert. Die Abfrage hat Vorrang vor der Tabelle, die Sie in diesem Schritt angeben.

    3. Klicken Sie auf **OK**.

14. Wechseln Sie zum Pipeline-Editor, indem Sie oben die Registerkarte „Pipeline“ oder in der Strukturansicht auf der linken Seite den Namen der Pipeline auswählen. Vergewissern Sie sich im Eigenschaftenfenster für die **Lookup**-Aktivität, dass im Feld **Source Dataset** (Quelldataset) die Option **SourceDataset** ausgewählt ist.

15. Wählen Sie im Feld **Abfrage verwenden** die Option **Abfrage** aus, und geben Sie die folgende Abfrage ein, nachdem Sie den Tabellennamen in der Abfrage aktualisiert haben: Sie wählen nur den maximalen Zeitstempelwert aus der Tabelle aus. Stellen Sie sicher, dass Sie auch **Nur erste Zeile** mit einem Häkchen versehen haben.

    ```sql
    select MAX(timestamp) as NewWatermarkvalue from [TableName]
    ```

    ![Auswählen der Abfrage](media/tutorial-sync-data-factory/select-query-data-factory.png)

16. Erweitern Sie in der **Aktivitätentoolbox** die Option **Move & Transform** (Verschieben und transformieren), ziehen Sie die Aktivität **Kopieren** mittels Drag & Drop aus der Aktivitätentoolbox, und legen Sie den Namen auf **IncrementalCopy** fest.

17. Verbinden Sie beide Aktivitäten vom Typ **Suche** mit der Aktivität **Kopieren**, indem Sie die **grüne Schaltfläche**, die den Aktivitäten vom Typ **Suche** zugeordnet ist, zur Aktivität **Kopieren** ziehen. Lassen Sie die Maustaste los, wenn die Rahmenfarbe der Kopieraktivität blau wird.

18. Wählen Sie die Aktivität **Kopieren** aus, und vergewissern Sie sich, dass die Eigenschaften für die Aktivität im Fenster **Eigenschaften** angezeigt werden.

19. Wechseln Sie im **Eigenschaftenfenster** zur Registerkarte **Quelle**, und führen Sie die folgenden Schritte aus:

    1. Wählen Sie im Feld **Source Dataset** (Quelldataset) die Option **SourceDataset**.

    2. Wählen Sie im Feld **Abfrage verwenden** die Option **Abfrage**.

    3. Geben Sie im Feld **Abfrage** die SQL-Abfrage ein. Beispielabfrage im Anschluss

    4. SQL-Abfrage:

    ```sql
    select * from TemperatureSensor where timestamp > '@{activity('OldWaterMark').output.firstRow.WatermarkValue}' and timestamp <= '@{activity('NewWaterMark').output.firstRow.NewWatermarkvalue}'
    ```

20. Wechseln Sie zur Registerkarte **Senke**, und wählen Sie für das Feld **Sink Dataset** (Senkendataset) die Option **+ Neu** aus.

21. In diesem Tutorial wird ein Senkendatenspeicher vom Typ **Azure Blob Storage** verwendet. Wählen Sie die Option **Azure Blob Storage** und anschließend im Fenster **Neues Dataset** die Option **Weiter** aus.

22. Wählen Sie im Fenster **Format auswählen** den Formattyp Ihrer Daten und anschließend **Weiter** aus.

23. Geben Sie im Fenster **Eigenschaften festlegen** unter „Name“ den Namen **SinkDataset** ein. Wählen Sie unter „Verknüpfter Dienst“ die Option **+ Neu** aus. In diesem Schritt erstellen Sie eine Verbindung (verknüpfter Dienst) mit Ihrem **Azure Blob Storage**.

24. Führen Sie im Fenster **New Linked Service (Azure Blob Storage)** (Neuer verknüpfter Dienst (Azure Blob Storage)) die folgenden Schritte aus:

    1. Geben Sie unter „Name“ die Zeichenfolge *AzureStorageLinkedService* ein.

    2. Wählen Sie unter **Speicherkontoname** Ihr Azure Storage-Konto mit Ihrem Azure-Abonnement aus.

    3. Testen Sie die **Verbindung**, und wählen Sie **Fertig stellen** aus.

25. Vergewissern Sie sich, dass im Fenster **Eigenschaften festlegen** unter **Verknüpfter Dienst** die Option *AzureStorageLinkedService* ausgewählt ist. Wählen Sie anschließend **Erstellen** und **OK** aus.

26. Wählen Sie auf der Registerkarte **Senke** die Option **Bearbeiten** aus.

27. Navigieren Sie zur Registerkarte **Verbindung** von *SinkDataset*, und führen Sie die folgenden Schritte aus:

    1. Geben Sie im Feld **Dateipfad** den Pfad *asdedatasync/incrementalcopy* ein. **asdedatasync** ist hierbei der Blobcontainername und **incrementalcopy** der Ordnername. Erstellen Sie den Container, wenn er noch nicht vorhanden ist, oder geben Sie den Namen eines bereits vorhandenen ein. Azure Data Factory erstellt den Ausgabeordner *incrementalcopy* automatisch, falls er nicht vorhanden ist. Sie können auch die Schaltfläche **Durchsuchen** für den **Dateipfad** verwenden, um zu einem Ordner in einem Blobcontainer zu navigieren.

    2. Wählen Sie für den Teil **Datei** des Felds **Dateipfad** die Option **Dynamischen Inhalt hinzufügen [ALT+P]** aus, und *geben Sie dann @CONCAT('Incremental-', pipeline().RunId, '.txt')* in das geöffnete Fenster ein. Klicken Sie dann auf **Fertig stellen**. Der Dateiname wird mit dem Ausdruck dynamisch generiert. Jede Pipelineausführung verfügt über eine eindeutige ID. Die Copy-Aktivität nutzt die Ausführungs-ID zum Generieren des Dateinamens.

28. Wechseln Sie zum Pipeline-Editor, indem Sie oben die Registerkarte **Pipeline** oder in der Strukturansicht auf der linken Seite den Namen der Pipeline auswählen.

29. Erweitern Sie in der Toolbox **Aktivitäten** die Option **Allgemein**, und ziehen Sie die **Stored Procedure**-Aktivität aus der Toolbox **Aktivitäten** auf die Oberfläche des Pipeline-Designers. **Verbinden** Sie die grüne Ausgabe (Erfolgreich) der **Copy**-Aktivität mit der **Stored Procedure**-Aktivität.

30. Wählen Sie im Pipeline-Designer die Option **Aktivität der gespeicherten Prozedur** aus, und ändern Sie den Namen in *SPtoUpdateWatermarkActivity*.

31. Wechseln Sie zur Registerkarte **SQL-Konto**, und wählen Sie unter **Verknüpfter Dienst** die Option *SQLDBEdgeLinkedService* aus.

32. Wechseln Sie zur Registerkarte **Gespeicherte Prozedur**, und führen Sie die folgenden Schritte aus:

    1. Wählen Sie unter **Name der gespeicherten Prozedur** die Option *[dbo].[usp_write_watermark]* aus.

    2. Wählen Sie zum Angeben von Werten für die Parameter der gespeicherten Prozedur die Option „Import parameter“ (Importparameter) aus, und geben Sie für die Parameter die folgenden Werte ein:

    |NAME|type|Wert|
    |-----|----|-----|
    |LastModifiedtime|Datetime|@{activity('NewWaterMark').output.firstRow.NewWatermarkvalue}|
    |TableName|Zeichenfolge|@{activity('OldWaterMark').output.firstRow.TableName}|

33. Wählen Sie zum Überprüfen der Pipelineeinstellungen auf der Symbolleiste die Option **Überprüfen** aus. Vergewissern Sie sich, dass keine Validierungsfehler vorliegen. Wählen Sie **>>** aus, um das Fenster **Pipeline Validation Report** (Pipelineüberprüfungsbericht) zu schließen.

34. Veröffentlichen Sie Entitäten (verknüpfte Dienste, Datasets und Pipelines) für den Azure Data Factory-Dienst, indem Sie die Schaltfläche **Alle veröffentlichen** wählen. Warten Sie, bis eine Meldung angezeigt wird, dass die Veröffentlichung erfolgreich war.

## <a name="trigger-a-pipeline-on-schedule"></a>Zeitplangesteuertes Auslösen einer Pipeline

1. Wählen Sie auf der Symbolleiste für die Pipeline nacheinander **Trigger hinzufügen** > **Neu/Bearbeiten** > **+ Neu** aus.

2. Nennen Sie Ihren Trigger *HourlySync*, wählen Sie unter **Typ** die Option „Zeitplan“ aus, und legen Sie **Wiederholung** auf stündlich fest.

3. Klicken Sie auf **OK**.

4. Wählen Sie **Alle veröffentlichen**.

5. Wählen Sie **Trigger Now** (Jetzt auslösen) aus.

6. Wechseln Sie im linken Bereich zur Registerkarte **Überwachen**. Sie können den Status der Pipelineausführung anzeigen, die vom manuellen Trigger ausgelöst wird. Wählen Sie die Schaltfläche **Aktualisieren** aus, um die Liste zu aktualisieren.

## <a name="next-steps"></a>Nächste Schritte

Die Azure Data Factory-Pipeline in diesem Tutorial kopiert stündlich Daten aus einer Tabelle in der SQL Database Edge-Instanz an einen Speicherort in Azure Blob Storage. Informationen zu weiteren Data Factory-Verwendungsszenarien finden Sie in [diesen Tutorials](../data-factory/tutorial-copy-data-portal.md).
