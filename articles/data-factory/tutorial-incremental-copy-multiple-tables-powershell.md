---
title: Inkrementelles Kopieren von mehreren Tabellen mithilfe von Azure Data Factory | Microsoft-Dokumentation
description: In diesem Tutorial erstellen Sie eine Azure Data Factory-Pipeline, bei der Deltadaten inkrementell aus mehreren Tabellen einer lokalen SQL Server-Datenbank in eine Azure SQL-Datenbank kopiert werden.
services: data-factory
documentationcenter: ''
author: dearandyxu
manager: craigg
ms.reviewer: douglasl
ms.service: data-factory
ms.workload: data-services
ms.topic: tutorial
ms.date: 01/22/2018
ms.author: yexu
ms.openlocfilehash: 0c5b9a16a7b52239f1ef16d42e1b4be344863a04
ms.sourcegitcommit: d200cd7f4de113291fbd57e573ada042a393e545
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 08/29/2019
ms.locfileid: "70140624"
---
# <a name="incrementally-load-data-from-multiple-tables-in-sql-server-to-an-azure-sql-database"></a>Inkrementelles Laden aus mehreren SQL Server-Tabellen in eine Azure SQL-Datenbank
In diesem Tutorial erstellen Sie eine Azure Data Factory mit einer Pipeline, bei der Deltadaten aus mehreren Tabellen einer lokalen SQL Server-Instanz in eine Azure SQL-Datenbank geladen werden.    

In diesem Tutorial führen Sie die folgenden Schritte aus:

> [!div class="checklist"]
> * Vorbereiten von Quell- und Zieldatenspeichern
> * Erstellen einer Data Factory.
> * Erstellen Sie eine selbstgehostete Integration Runtime.
> * Installieren der Integration Runtime 
> * Erstellen Sie verknüpfte Dienste. 
> * Erstellen des Quell-, Senken-, Grenzwertdatasets
> * Erstellen, Ausführen und Überwachen einer Pipeline
> * Überprüfen Sie die Ergebnisse.
> * Hinzufügen oder Aktualisieren von Daten in Quelltabellen
> * Erneutes Ausführen und Überwachen der Pipeline
> * Überprüfen der Endergebnisse

## <a name="overview"></a>Übersicht
Hier sind die wesentlichen Schritte beim Erstellen dieser Lösung aufgeführt: 

1. **Select the watermark column (Wählen Sie die Grenzwert-Spalte aus)** .
    Wählen Sie für jede Tabelle im Quelldatenspeicher eine Spalte aus, die verwendet werden kann, um die neuen oder aktualisierten Datensätze für jede Ausführung zu identifizieren. Normalerweise steigen die Daten in dieser ausgewählten Spalte (z.B. Last_modify_time oder ID), wenn Zeilen erstellt oder aktualisiert werden. Der maximale Wert in dieser Spalte wird als Grenzwert verwendet.

1. **Prepare a data store to store the watermark value (Vorbereiten eines Datenspeichers zum Speichern des Grenzwerts)** .   
    In diesem Tutorial speichern Sie den Grenzwert in einer SQL-Datenbank.

1. **Erstellen Sie eine Pipeline mit den folgenden Aktivitäten:** 
    
    a. Erstellen Sie eine ForEach-Aktivität zum Durchlaufen einer Liste mit Namen von Quelltabellen, die als Parameter an die Pipeline übergeben wird. Für jede Quelltabelle werden die folgenden Aktivitäten aufgerufen, um den Deltaladevorgang für diese Tabelle durchzuführen:

    b. Erstellen Sie zwei Lookup-Aktivitäten. Verwenden Sie die erste Lookup-Aktivität, um den letzten Grenzwert abzurufen. Verwenden Sie die zweite Lookup-Aktivität, um den neuen Grenzwert abzurufen. Diese Grenzwerte werden an die Copy-Aktivität übergeben.

    c. Erstellen Sie eine Copy-Aktivität, die Zeilen aus dem Quelldatenspeicher kopiert, wobei der Wert der Grenzwertspalte größer als der alte Grenzwert und kleiner als der neue Grenzwert ist. Anschließend werden die Deltadaten aus dem Quelldatenspeicher als neue Datei in Azure Blob Storage kopiert.

    d. Erstellen Sie eine StoredProcedure-Aktivität, die den Grenzwert für die Pipeline aktualisiert, die nächstes Mal ausgeführt wird. 

    Allgemeines Lösungsdiagramm: 

    ![Lädt Daten inkrementell](media/tutorial-incremental-copy-multiple-tables-powershell/high-level-solution-diagram.png)


Wenn Sie kein Azure-Abonnement besitzen, können Sie ein [kostenloses Konto](https://azure.microsoft.com/free/) erstellen, bevor Sie beginnen.

## <a name="prerequisites"></a>Voraussetzungen
* **SQL Server**. In diesem Tutorial verwenden Sie eine lokale SQL Server-Datenbank als Quelldatenspeicher. 
* **Azure SQL-Datenbank**. Sie verwenden eine SQL-Datenbank als Senkendatenspeicher. Wenn Sie keine SQL-Datenbank besitzen, finden Sie Schritte zum Erstellen einer solchen Datenbank unter [Erstellen einer Azure SQL-Datenbank im Azure-Portal](../sql-database/sql-database-get-started-portal.md). 

### <a name="create-source-tables-in-your-sql-server-database"></a>Erstellen von Quelltabellen in Ihrer SQL Server-Datenbank

1. Öffnen Sie SQL Server Management Studio, und stellen Sie eine Verbindung mit Ihrer lokalen SQL Server-Datenbank her.

1. Klicken Sie im **Server-Explorer** mit der rechten Maustaste auf die Datenbank, und wählen Sie **Neue Abfrage**.

1. Führen Sie den folgenden SQL-Befehl für Ihre Datenbank aus, um Tabellen mit den Namen `customer_table` und `project_table` zu erstellen:

    ```sql
    create table customer_table
    (
        PersonID int,
        Name varchar(255),
        LastModifytime datetime
    );
    
    create table project_table
    (
        Project varchar(255),
        Creationtime datetime
    );
        
    INSERT INTO customer_table
    (PersonID, Name, LastModifytime)
    VALUES
    (1, 'John','9/1/2017 12:56:00 AM'),
    (2, 'Mike','9/2/2017 5:23:00 AM'),
    (3, 'Alice','9/3/2017 2:36:00 AM'),
    (4, 'Andy','9/4/2017 3:21:00 AM'),
    (5, 'Anny','9/5/2017 8:06:00 AM');
    
    INSERT INTO project_table
    (Project, Creationtime)
    VALUES
    ('project1','1/1/2015 0:00:00 AM'),
    ('project2','2/2/2016 1:23:00 AM'),
    ('project3','3/4/2017 5:16:00 AM');
    
    ```

### <a name="create-destination-tables-in-your-azure-sql-database"></a>Erstellen von Zieltabellen in Ihrer Azure SQL-Datenbank
1. Öffnen Sie SQL Server Management Studio, und stellen Sie eine Verbindung mit der SQL Server-Datenbank her.

1. Klicken Sie im **Server-Explorer** mit der rechten Maustaste auf die Datenbank, und wählen Sie **Neue Abfrage**.

1. Führen Sie den folgenden SQL-Befehl für Ihre SQL-Datenbank aus, um Tabellen mit den Namen `customer_table` und `project_table` zu erstellen:  
    
    ```sql
    create table customer_table
    (
        PersonID int,
        Name varchar(255),
        LastModifytime datetime
    );
    
    create table project_table
    (
        Project varchar(255),
        Creationtime datetime
    );

    ```

### <a name="create-another-table-in-the-azure-sql-database-to-store-the-high-watermark-value"></a>Erstellen einer weiteren Tabelle in der Azure SQL-Datenbank zum Speichern des hohen Grenzwerts
1. Führen Sie den folgenden SQL-Befehl für Ihre SQL-Datenbank aus, um eine Tabelle mit dem Namen `watermarktable` zum Speichern des Grenzwerts zu erstellen: 
    
    ```sql
    create table watermarktable
    (
    
        TableName varchar(255),
        WatermarkValue datetime,
    );
    ```
1. Fügen Sie die anfänglichen Grenzwerte für beide Quelltabellen in die Tabelle mit den Grenzwerten ein.

    ```sql

    INSERT INTO watermarktable
    VALUES 
    ('customer_table','1/1/2010 12:00:00 AM'),
    ('project_table','1/1/2010 12:00:00 AM');
    
    ```

### <a name="create-a-stored-procedure-in-the-azure-sql-database"></a>Erstellen Sie eine gespeicherte Prozedur in der Azure SQL-Datenbank 

Führen Sie den folgenden Befehl zum Erstellen einer gespeicherten Prozedur in Ihrer SQL-Datenbank aus. Mit dieser gespeicherten Prozedur wird der Grenzwert nach jeder Pipelineausführung aktualisiert. 

```sql
CREATE PROCEDURE usp_write_watermark @LastModifiedtime datetime, @TableName varchar(50)
AS

BEGIN

    UPDATE watermarktable
    SET [WatermarkValue] = @LastModifiedtime 
WHERE [TableName] = @TableName

END

```

### <a name="create-data-types-and-additional-stored-procedures-in-the-azure-sql-database"></a>Erstellen von Datentypen und zusätzlichen gespeicherten Prozeduren in der Azure SQL-Datenbank
Führen Sie die folgende Abfrage aus, um zwei gespeicherte Prozeduren und zwei Datentypen in der SQL-Datenbank zu erstellen. Sie werden zum Zusammenführen der Daten aus Quelltabellen in Zieltabellen verwendet. 

Um den Einstieg zu erleichtern, verwenden wir diese gespeicherten Prozeduren direkt. Dabei übergeben wir die Deltadaten mithilfe einer Tabellenvariablen und führen sie anschließend im Zielspeicher zusammen. In der Tabellenvariablen sollten maximal 100 Deltazeilen gespeichert werden.  

Falls Sie im Zielspeicher eine große Anzahl von Deltazeilen zusammenführen möchten, empfiehlt es sich, die Deltadaten zunächst mithilfe der Kopieraktivität in eine temporäre Stagingtabelle im Zielspeicher zu kopieren und anschließen eine eigene gespeicherte Prozedur ohne Tabellenvariable zu erstellen, um die Daten aus der Stagingtabelle in der finalen Tabelle zusammenzuführen. 


```sql
CREATE TYPE DataTypeforCustomerTable AS TABLE(
    PersonID int,
    Name varchar(255),
    LastModifytime datetime
);

GO

CREATE PROCEDURE usp_upsert_customer_table @customer_table DataTypeforCustomerTable READONLY
AS

BEGIN
  MERGE customer_table AS target
  USING @customer_table AS source
  ON (target.PersonID = source.PersonID)
  WHEN MATCHED THEN
      UPDATE SET Name = source.Name,LastModifytime = source.LastModifytime
  WHEN NOT MATCHED THEN
      INSERT (PersonID, Name, LastModifytime)
      VALUES (source.PersonID, source.Name, source.LastModifytime);
END

GO

CREATE TYPE DataTypeforProjectTable AS TABLE(
    Project varchar(255),
    Creationtime datetime
);

GO

CREATE PROCEDURE usp_upsert_project_table @project_table DataTypeforProjectTable READONLY
AS

BEGIN
  MERGE project_table AS target
  USING @project_table AS source
  ON (target.Project = source.Project)
  WHEN MATCHED THEN
      UPDATE SET Creationtime = source.Creationtime
  WHEN NOT MATCHED THEN
      INSERT (Project, Creationtime)
      VALUES (source.Project, source.Creationtime);
END

```

### <a name="azure-powershell"></a>Azure PowerShell
Installieren Sie die aktuellen Azure PowerShell-Module, indem Sie die Anweisungen unter [Installieren und Konfigurieren von Azure PowerShell](/powershell/azure/azurerm/install-azurerm-ps) befolgen.

## <a name="create-a-data-factory"></a>Erstellen einer Data Factory
1. Definieren Sie eine Variable für den Ressourcengruppennamen zur späteren Verwendung in PowerShell-Befehlen. Kopieren Sie den folgenden Befehlstext nach PowerShell, geben Sie einen Namen für die [Azure-Ressourcengruppe](../azure-resource-manager/resource-group-overview.md) in doppelten Anführungszeichen an, und führen Sie dann den Befehl aus. Ein Beispiel ist `"adfrg"`. 
   
     ```powershell
    $resourceGroupName = "ADFTutorialResourceGroup";
    ```

    Beachten Sie, dass die Ressourcengruppe ggf. nicht überschrieben werden soll, falls sie bereits vorhanden ist. Weisen Sie der Variablen `$resourceGroupName` einen anderen Wert zu, und führen Sie den Befehl erneut aus.

1. Definieren Sie eine Variable für den Speicherort der Data Factory. 

    ```powershell
    $location = "East US"
    ```
1. Führen Sie den folgenden Befehl aus, um die Azure-Ressourcengruppe zu erstellen: 

    ```powershell
    New-AzureRmResourceGroup $resourceGroupName $location
    ``` 
    Beachten Sie, dass die Ressourcengruppe ggf. nicht überschrieben werden soll, falls sie bereits vorhanden ist. Weisen Sie der Variablen `$resourceGroupName` einen anderen Wert zu, und führen Sie den Befehl erneut aus.

1. Definieren Sie eine Variable für den Namen der Data Factory. 

    > [!IMPORTANT]
    >  Aktualisieren Sie den Data Factory-Namen, damit er global eindeutig ist. Beispiel: ADFIncMultiCopyTutorialFactorySP1127. 

    ```powershell
    $dataFactoryName = "ADFIncMultiCopyTutorialFactory";
    ```
1. Führen Sie zum Erstellen der Data Factory das folgende **Set-AzureRmDataFactoryV2**-Cmdlet aus: 
    
    ```powershell       
    Set-AzureRmDataFactoryV2 -ResourceGroupName $resourceGroupName -Location $location -Name $dataFactoryName 
    ```

Beachten Sie folgende Punkte:

* Der Name der Data Factory muss global eindeutig sein. Wenn die folgende Fehlermeldung angezeigt wird, ändern Sie den Namen, und wiederholen Sie den Vorgang:

    ```
    The specified Data Factory name 'ADFIncMultiCopyTutorialFactory' is already in use. Data Factory names must be globally unique.
    ```
* Damit Sie Data Factory-Instanzen erstellen können, muss das Benutzerkonto, mit dem Sie sich bei Azure anmelden, ein Mitglied der Rolle „Mitwirkender“ oder „Besitzer“ oder ein Administrator des Azure-Abonnements sein.
* Eine Liste der Azure-Regionen, in denen Data Factory derzeit verfügbar ist, finden Sie, indem Sie die für Sie interessanten Regionen auf der folgenden Seite auswählen und dann **Analysen** erweitern, um **Data Factory** zu finden: [Verfügbare Produkte nach Region](https://azure.microsoft.com/global-infrastructure/services/). Die von der Data Factory verwendeten Datenspeicher (Azure Storage, SQL-Datenbank usw.) und Computedienste (Azure HDInsight usw.) können sich in anderen Regionen befinden.

[!INCLUDE [data-factory-create-install-integration-runtime](../../includes/data-factory-create-install-integration-runtime.md)]



## <a name="create-linked-services"></a>Erstellen von verknüpften Diensten
Um Ihre Datenspeicher und Compute Services mit der Data Factory zu verknüpfen, können Sie verknüpfte Dienste in einer Data Factory erstellen. In diesem Abschnitt erstellen Sie verknüpfte Dienste für Ihre lokale SQL Server-Datenbank und SQL-Datenbank. 

### <a name="create-the-sql-server-linked-service"></a>Erstellen des mit SQL Server verknüpften Diensts
In diesem Schritt verknüpfen Sie die lokale SQL Server-Datenbank mit der Data Factory.

1. Erstellen Sie im Ordner „C:\ADFTutorials\IncCopyMultiTableTutorial“ eine JSON-Datei mit dem Namen „SqlServerLinkedService.json“ mit folgendem Inhalt. Wählen Sie basierend auf der Authentifizierung, die Sie zum Herstellen einer Verbindung mit SQL Server verwenden, den richtigen Abschnitt aus. Erstellen Sie die lokalen Ordner, falls sie nicht bereits vorhanden sind. 

    > [!IMPORTANT]
    > Wählen Sie basierend auf der Authentifizierung, die Sie zum Herstellen einer Verbindung mit SQL Server verwenden, den richtigen Abschnitt aus.

    Kopieren Sie bei Verwendung der SQL-Authentifizierung die folgende JSON-Definition:

    ```json
    {
        "properties": {
            "type": "SqlServer",
            "typeProperties": {
                "connectionString": {
                    "type": "SecureString",
                    "value": "Server=<servername>;Database=<databasename>;User ID=<username>;Password=<password>;Timeout=60"
                }
            },
            "connectVia": {
                "type": "integrationRuntimeReference",
                "referenceName": "<integration runtime name>"
            }
        },
        "name": "SqlServerLinkedService"
    }
   ```    
    Kopieren Sie bei Verwendung der Windows-Authentifizierung die folgende JSON-Definition:

    ```json
    {
        "properties": {
            "type": "SqlServer",
            "typeProperties": {
                "connectionString": {
                    "type": "SecureString",
                    "value": "Server=<server>;Database=<database>;Integrated Security=True"
                },
                "userName": "<user> or <domain>\\<user>",
                "password": {
                    "type": "SecureString",
                    "value": "<password>"
                }
            },
            "connectVia": {
                "type": "integrationRuntimeReference",
                "referenceName": "<integration runtime name>"
            }
        },
        "name": "SqlServerLinkedService"
    }    
    ```
    > [!IMPORTANT]
    > - Wählen Sie basierend auf der Authentifizierung, die Sie zum Herstellen einer Verbindung mit SQL Server verwenden, den richtigen Abschnitt aus.
    > - Ersetzen Sie „&lt;integration runtime name>“ durch den Namen Ihrer Integration Runtime.
    > - Ersetzen Sie vor dem Speichern der Datei „&lt;servername>“, „&lt;databasename>“, „&lt;username>“ und „&lt;password>“ durch die Werte Ihrer SQL Server-Datenbank.
    > - Wenn Sie im Benutzerkonto- oder Servernamen einen Schrägstrich (`\`) verwenden müssen, verwenden Sie das Escapezeichen (`\`). Ein Beispiel ist `mydomain\\myuser`.

1. Wechseln Sie in PowerShell zum Ordner „C:\ADFTutorials\IncCopyMultiTableTutorial“.

1. Führen Sie das Cmdlet **Set-AzureRmDataFactoryV2LinkedService** aus, um den verknüpften Dienst „AzureStorageLinkedService“ zu erstellen. Im folgenden Beispiel übergeben Sie Werte für die *ResourceGroupName*- und *DataFactoryName*-Parameter: 

    ```powershell
    Set-AzureRmDataFactoryV2LinkedService -DataFactoryName $dataFactoryName -ResourceGroupName $resourceGroupName -Name "SqlServerLinkedService" -File ".\SqlServerLinkedService.json"
    ```

    Hier ist die Beispielausgabe:

    ```json
    LinkedServiceName : SqlServerLinkedService
    ResourceGroupName : ADFTutorialResourceGroup
    DataFactoryName   : ADFIncMultiCopyTutorialFactory1201
    Properties        : Microsoft.Azure.Management.DataFactory.Models.SqlServerLinkedService
    ```

### <a name="create-the-sql-database-linked-service"></a>Erstellen des mit der SQL-Datenbank verknüpften Diensts
1. Erstellen Sie im Ordner „C:\ADFTutorials\IncCopyMultiTableTutorial“ eine JSON-Datei mit dem Namen „AzureSQLDatabaseLinkedService.json“ mit folgendem Inhalt. (Erstellen Sie den Ordner „ADF“, wenn er noch nicht vorhanden ist.) Ersetzen Sie „&lt;server&gt;“, „&lt;database name&gt;“, „&lt;user id&gt;“ und „&lt;password&gt;“ durch den Namen Ihrer SQL Server-Datenbank, den Namen Ihrer Datenbank, die Benutzer-ID und das Kennwort, bevor Sie die Datei speichern. 

    ```json
    {
        "name": "AzureSQLDatabaseLinkedService",
        "properties": {
            "type": "AzureSqlDatabase",
            "typeProperties": {
                "connectionString": {
                    "value": "Server = tcp:<server>.database.windows.net,1433;Initial Catalog=<database name>; Persist Security Info=False; User ID=<user name>; Password=<password>; MultipleActiveResultSets = False; Encrypt = True; TrustServerCertificate = False; Connection Timeout = 30;",
                    "type": "SecureString"
                }
            }
        }
    }
    ```
1. Führen Sie in PowerShell das **Set-AzureRmDataFactoryV2LinkedService**-Cmdlet zum Erstellen des verknüpften Diensts „AzureSQLDatabaseLinkedService“ aus. 

    ```powershell
    Set-AzureRmDataFactoryV2LinkedService -DataFactoryName $dataFactoryName -ResourceGroupName $resourceGroupName -Name "AzureSQLDatabaseLinkedService" -File ".\AzureSQLDatabaseLinkedService.json"
    ```

    Hier ist die Beispielausgabe:

    ```json
    LinkedServiceName : AzureSQLDatabaseLinkedService
    ResourceGroupName : ADFTutorialResourceGroup
    DataFactoryName   : ADFIncMultiCopyTutorialFactory1201
    Properties        : Microsoft.Azure.Management.DataFactory.Models.AzureSqlDatabaseLinkedService
    ```

## <a name="create-datasets"></a>Erstellen von Datasets
In diesem Schritt erstellen Sie Datasets zur Darstellung der Datenquelle, des Datenziels und des Speicherorts des Grenzwerts.

### <a name="create-a-source-dataset"></a>Erstellen eines Quelldatasets

1. Erstellen Sie eine JSON-Datei mit dem Namen „SourceDataset.json“ im selben Ordner und dem folgenden Inhalt: 

    ```json
    {
        "name": "SourceDataset",
        "properties": {
            "type": "SqlServerTable",
            "typeProperties": {
                "tableName": "dummyName"
            },
            "linkedServiceName": {
                "referenceName": "SqlServerLinkedService",
                "type": "LinkedServiceReference"
            }
        }
    }
   
    ```

    Der Tabellenname ist ein Pseudoname. Für die Copy-Aktivität in der Pipeline wird eine SQL-Abfrage zum Laden der Daten anstelle der gesamten Tabelle verwendet.

1. Führen Sie nun das Cmdlet **Set-AzureRmDataFactoryV2Dataset** zum Erstellen des Datasets „SourceDataset“ aus.
    
    ```powershell
    Set-AzureRmDataFactoryV2Dataset -DataFactoryName $dataFactoryName -ResourceGroupName $resourceGroupName -Name "SourceDataset" -File ".\SourceDataset.json"
    ```

    Hier ist die Beispielausgabe des Cmdlets:
    
    ```json
    DatasetName       : SourceDataset
    ResourceGroupName : ADFTutorialResourceGroup
    DataFactoryName   : ADFIncMultiCopyTutorialFactory1201
    Structure         :
    Properties        : Microsoft.Azure.Management.DataFactory.Models.SqlServerTableDataset
    ```

### <a name="create-a-sink-dataset"></a>Erstellen Sie ein Senkendataset

1. Erstellen Sie eine JSON-Datei mit dem Namen „SinkDataset.json“ im selben Ordner und mit folgendem Inhalt. Das tableName-Element wird zur Laufzeit dynamisch von der Pipeline festgelegt. Die ForEach-Aktivität in der Pipeline durchläuft eine Liste mit Tabellennamen und übergibt den Tabellennamen bei jedem Durchlauf an dieses Dataset. 

    ```json
    {
        "name": "SinkDataset",
        "properties": {
            "type": "AzureSqlTable",
            "typeProperties": {
                "tableName": {
                    "value": "@{dataset().SinkTableName}",
                    "type": "Expression"
                }
            },
            "linkedServiceName": {
                "referenceName": "AzureSQLDatabaseLinkedService",
                "type": "LinkedServiceReference"
            },
            "parameters": {
                "SinkTableName": {
                    "type": "String"
                }
            }
        }
    }
    ```

1. Führen Sie nun das **Set-AzureRmDataFactoryV2Dataset**-Cmdlet zum Erstellen des Datasets „SinkDataset“ aus.
    
    ```powershell
    Set-AzureRmDataFactoryV2Dataset -DataFactoryName $dataFactoryName -ResourceGroupName $resourceGroupName -Name "SinkDataset" -File ".\SinkDataset.json"
    ```

    Hier ist die Beispielausgabe des Cmdlets:
    
    ```json
    DatasetName       : SinkDataset
    ResourceGroupName : ADFTutorialResourceGroup
    DataFactoryName   : ADFIncMultiCopyTutorialFactory1201
    Structure         :
    Properties        : Microsoft.Azure.Management.DataFactory.Models.AzureSqlTableDataset
    ```

### <a name="create-a-dataset-for-a-watermark"></a>Erstellen eines Datasets für einen Grenzwert
In diesem Schritt erstellen Sie ein Dataset zum Speichern eines hohen Grenzwerts. 

1. Erstellen Sie eine JSON-Datei mit dem Namen „WatermarkDataset.json“ im selben Ordner und dem folgenden Inhalt: 

    ```json
    {
        "name": " WatermarkDataset ",
        "properties": {
            "type": "AzureSqlTable",
            "typeProperties": {
                "tableName": "watermarktable"
            },
            "linkedServiceName": {
                "referenceName": "AzureSQLDatabaseLinkedService",
                "type": "LinkedServiceReference"
            }
        }
    }    
    ```
1. Führen Sie nun das **Set-AzureRmDataFactoryV2Dataset**-Cmdlet zum Erstellen des Datasets „WatermarkDataset“ aus.
    
    ```powershell
    Set-AzureRmDataFactoryV2Dataset -DataFactoryName $dataFactoryName -ResourceGroupName $resourceGroupName -Name "WatermarkDataset" -File ".\WatermarkDataset.json"
    ```

    Hier ist die Beispielausgabe des Cmdlets:
    
    ```json
    DatasetName       : WatermarkDataset
    ResourceGroupName : ADFTutorialResourceGroup
    DataFactoryName   : <data factory name>
    Structure         :
    Properties        : Microsoft.Azure.Management.DataFactory.Models.AzureSqlTableDataset    
    ```

## <a name="create-a-pipeline"></a>Erstellen einer Pipeline
Die Pipeline verwendet die Liste mit den Tabellennamen als Parameter. Die ForEach-Aktivität durchläuft die Liste mit den Tabellennamen und führt die folgenden Vorgänge aus: 

1. Verwenden Sie die Lookup-Aktivität, um den alten Grenzwert abzurufen (anfänglicher Wert oder der im letzten Durchlauf verwendete Wert).

1. Verwenden Sie die Lookup-Aktivität, um den neuen Grenzwert abzurufen (Höchstwert der Grenzwertspalte in der Quelltabelle).

1. Verwenden Sie die Copy-Aktivität, um Daten zwischen diesen beiden Grenzwerten aus der Quelldatenbank in die Zieldatenbank zu kopieren.

1. Verwenden Sie die StoredProcedure-Aktivität, um den alten Grenzwert zu aktualisieren, damit er im ersten Schritt des nächsten Durchlaufs verwendet werden kann. 

### <a name="create-the-pipeline"></a>Erstellen der Pipeline
1. Erstellen Sie in demselben Ordner die JSON-Datei „IncrementalCopyPipeline.json“ mit folgendem Inhalt: 

    ```json
    {
        "name": "IncrementalCopyPipeline",
        "properties": {
            "activities": [{
    
                "name": "IterateSQLTables",
                "type": "ForEach",
                "typeProperties": {
                    "isSequential": "false",
                    "items": {
                        "value": "@pipeline().parameters.tableList",
                        "type": "Expression"
                    },
    
                    "activities": [
                        {
                            "name": "LookupOldWaterMarkActivity",
                            "type": "Lookup",
                            "typeProperties": {
                                "source": {
                                    "type": "SqlSource",
                                    "sqlReaderQuery": "select * from watermarktable where TableName  =  '@{item().TABLE_NAME}'"
                                },
    
                                "dataset": {
                                    "referenceName": "WatermarkDataset",
                                    "type": "DatasetReference"
                                }
                            }
                        },
                        {
                            "name": "LookupNewWaterMarkActivity",
                            "type": "Lookup",
                            "typeProperties": {
                                "source": {
                                    "type": "SqlSource",
                                    "sqlReaderQuery": "select MAX(@{item().WaterMark_Column}) as NewWatermarkvalue from @{item().TABLE_NAME}"
                                },
    
                                "dataset": {
                                    "referenceName": "SourceDataset",
                                    "type": "DatasetReference"
                                }
                            }
                        },
    
                        {
                            "name": "IncrementalCopyActivity",
                            "type": "Copy",
                            "typeProperties": {
                                "source": {
                                    "type": "SqlSource",
                                    "sqlReaderQuery": "select * from @{item().TABLE_NAME} where @{item().WaterMark_Column} > '@{activity('LookupOldWaterMarkActivity').output.firstRow.WatermarkValue}' and @{item().WaterMark_Column} <= '@{activity('LookupNewWaterMarkActivity').output.firstRow.NewWatermarkvalue}'"
                                },
                                "sink": {
                                    "type": "SqlSink",
                                    "SqlWriterTableType": "@{item().TableType}",
                                    "SqlWriterStoredProcedureName": "@{item().StoredProcedureNameForMergeOperation}"
                                }
                            },
                            "dependsOn": [{
                                    "activity": "LookupNewWaterMarkActivity",
                                    "dependencyConditions": [
                                        "Succeeded"
                                    ]
                                },
                                {
                                    "activity": "LookupOldWaterMarkActivity",
                                    "dependencyConditions": [
                                        "Succeeded"
                                    ]
                                }
                            ],
    
                            "inputs": [{
                                "referenceName": "SourceDataset",
                                "type": "DatasetReference"
                            }],
                            "outputs": [{
                                "referenceName": "SinkDataset",
                                "type": "DatasetReference",
                                "parameters": {
                                    "SinkTableName": "@{item().TableType}"
                                }
                            }]
                        },
    
                        {
                            "name": "StoredProceduretoWriteWatermarkActivity",
                            "type": "SqlServerStoredProcedure",
                            "typeProperties": {
    
                                "storedProcedureName": "usp_write_watermark",
                                "storedProcedureParameters": {
                                    "LastModifiedtime": {
                                        "value": "@{activity('LookupNewWaterMarkActivity').output.firstRow.NewWatermarkvalue}",
                                        "type": "datetime"
                                    },
                                    "TableName": {
                                        "value": "@{activity('LookupOldWaterMarkActivity').output.firstRow.TableName}",
                                        "type": "String"
                                    }
                                }
                            },
    
                            "linkedServiceName": {
                                "referenceName": "AzureSQLDatabaseLinkedService",
                                "type": "LinkedServiceReference"
                            },
    
                            "dependsOn": [{
                                "activity": "IncrementalCopyActivity",
                                "dependencyConditions": [
                                    "Succeeded"
                                ]
                            }]
                        }
    
                    ]
    
                }
            }],
    
            "parameters": {
                "tableList": {
                    "type": "Object"
                }
            }
        }
    }
    ```
1. Führen Sie das Cmdlet **Set-AzureRmDataFactoryV2Pipeline** aus, um die Pipeline „IncrementalCopyPipeline“ zu erstellen.
    
   ```powershell
   Set-AzureRmDataFactoryV2Pipeline -DataFactoryName $dataFactoryName -ResourceGroupName $resourceGroupName -Name "IncrementalCopyPipeline" -File ".\IncrementalCopyPipeline.json"
   ``` 

   Hier ist die Beispielausgabe: 

   ```json
    PipelineName      : IncrementalCopyPipeline
    ResourceGroupName : ADFTutorialResourceGroup
    DataFactoryName   : ADFIncMultiCopyTutorialFactory1201
    Activities        : {IterateSQLTables}
    Parameters        : {[tableList, Microsoft.Azure.Management.DataFactory.Models.ParameterSpecification]}
   ```
 
## <a name="run-the-pipeline"></a>Führen Sie die Pipeline aus.

1. Erstellen Sie in demselben Ordner eine Parameterdatei namens „Parameters.json“ mit folgendem Inhalt:

    ```json
    {
        "tableList": 
        [
            {
                "TABLE_NAME": "customer_table",
                "WaterMark_Column": "LastModifytime",
                "TableType": "DataTypeforCustomerTable",
                "StoredProcedureNameForMergeOperation": "usp_upsert_customer_table"
            },
            {
                "TABLE_NAME": "project_table",
                "WaterMark_Column": "Creationtime",
                "TableType": "DataTypeforProjectTable",
                "StoredProcedureNameForMergeOperation": "usp_upsert_project_table"
            }
        ]
    }
    ```
1. Führen Sie die Pipeline „IncrementalCopyPipeline“ mithilfe des Cmdlets **Invoke-AzureRmDataFactoryV2Pipeline** aus. Ersetzen Sie Platzhalter mit Ihrem eigenen Ressourcengruppen- und Data Factory-Namen.

    ```powershell
    $RunId = Invoke-AzureRmDataFactoryV2Pipeline -PipelineName "IncrementalCopyPipeline" -ResourceGroup $resourceGroupName -dataFactoryName $dataFactoryName -ParameterFile ".\Parameters.json"        
    ``` 

## <a name="monitor-the-pipeline"></a>Überwachen der Pipeline

1. Melden Sie sich beim [Azure-Portal](https://portal.azure.com) an.

1. Klicken Sie auf **Alle Dienste**, führen Sie eine Suche mit dem Schlüsselwort *Data Factorys* durch, und wählen Sie **Data Factorys** aus. 

    ![Menü „Data Factorys“](media/tutorial-incremental-copy-multiple-tables-powershell/monitor-data-factories-menu-1.png)

1. Suchen Sie in der Liste mit den Data Factorys nach Ihrer Data Factory, und wählen Sie sie aus, um die Seite **Data Factory** zu öffnen. 

    ![Suchen nach Ihrer Data Factory](media/tutorial-incremental-copy-multiple-tables-powershell/monitor-search-data-factory-2.png)

1. Klicken Sie auf der Seite **Data Factory** auf die Kachel **Überwachung und Verwaltung**. 

    ![Kachel „Überwachung und Verwaltung“](media/tutorial-incremental-copy-multiple-tables-powershell/monitor-monitor-manage-tile-3.png)

1. Die **Anwendung für die Datenintegration** wird auf einer separaten Registerkarte geöffnet. Alle Pipelineausführungen mit dem dazugehörigen Status werden angezeigt. Beachten Sie, dass der Status der Pipelineausführung im folgenden Beispiel **Erfolgreich** lautet. Überprüfen Sie die an die Pipeline übergebenen Parameter, indem Sie in der Spalte **Parameter** auf den Link klicken. Wenn ein Fehler auftritt, wird in der Spalte **Fehler** ein Link angezeigt. Klicken Sie in der Spalte **Aktionen** auf den Link. 

    ![Pipelineausführungen](media/tutorial-incremental-copy-multiple-tables-powershell/monitor-pipeline-runs-4.png)    
1. Wenn Sie in der Spalte **Aktionen** auf den Link klicken, wird die folgende Seite mit allen Aktivitätsausführungen der Pipeline angezeigt: 

    ![Aktivitätsausführungen](media/tutorial-incremental-copy-multiple-tables-powershell/monitor-activity-runs-5.png)

1. Klicken Sie wie in der Abbildung dargestellt auf **Pipelines**, um zurück zur Ansicht **Pipelineausführungen** zu wechseln. 

## <a name="review-the-results"></a>Überprüfen der Ergebnisse
Führen Sie in SQL Server Management Studio die folgenden Abfragen für die SQL-Zieldatenbank aus, um sicherzustellen, dass die Daten aus den Quelltabellen in die Zieltabellen kopiert wurden: 

**Abfrage** 
```sql
select * from customer_table
```

**Ausgabe**
```
===========================================
PersonID    Name    LastModifytime
===========================================
1           John    2017-09-01 00:56:00.000
2           Mike    2017-09-02 05:23:00.000
3           Alice   2017-09-03 02:36:00.000
4           Andy    2017-09-04 03:21:00.000
5           Anny    2017-09-05 08:06:00.000
```

**Abfrage**

```sql
select * from project_table
```

**Ausgabe**

```
===================================
Project     Creationtime
===================================
project1    2015-01-01 00:00:00.000
project2    2016-02-02 01:23:00.000
project3    2017-03-04 05:16:00.000
```

**Abfrage**

```sql
select * from watermarktable
```

**Ausgabe**

```
======================================
TableName       WatermarkValue
======================================
customer_table  2017-09-05 08:06:00.000
project_table   2017-03-04 05:16:00.000
```

Beachten Sie, dass die Grenzwerte für beide Tabellen aktualisiert wurden. 

## <a name="add-more-data-to-the-source-tables"></a>Hinzufügen von weiteren Daten zu den Quelltabellen

Führen Sie die folgende Abfrage für die SQL Server-Quelldatenbank aus, um in der Kundentabelle (customer_table) eine vorhandene Zeile zu aktualisieren. Fügen Sie eine neue Zeile in die Projekttabelle (project_table) ein. 

```sql
UPDATE customer_table
SET [LastModifytime] = '2017-09-08T00:00:00Z', [name]='NewName' where [PersonID] = 3

INSERT INTO project_table
(Project, Creationtime)
VALUES
('NewProject','10/1/2017 0:00:00 AM');
``` 

## <a name="rerun-the-pipeline"></a>Erneutes Ausführen der Pipeline

1. Führen Sie die Pipeline jetzt erneut aus, indem Sie den folgenden PowerShell-Befehl verwenden:

    ```powershell
    $RunId = Invoke-AzureRmDataFactoryV2Pipeline -PipelineName "IncrementalCopyPipeline" -ResourceGroup $resourceGroupname -dataFactoryName $dataFactoryName -ParameterFile ".\Parameters.json"
    ```
1. Überwachen Sie die Pipelineausführungen, indem Sie die Anleitung im Abschnitt [Überwachen der Pipeline](#monitor-the-pipeline) befolgen. Da der Pipelinestatus **In Bearbeitung** lautet, wird unter **Aktionen** ein weiterer Aktionslink zum Abbrechen der Pipelineausführung angezeigt. 

    ![Pipelineausführungen „In Bearbeitung“](media/tutorial-incremental-copy-multiple-tables-powershell/monitor-pipeline-runs-6.png)

1. Klicken Sie auf **Aktualisieren**, um die Liste zu aktualisieren, bis die Pipelineausführung erfolgreich ist. 

    ![Aktualisieren der Pipelineausführungen](media/tutorial-incremental-copy-multiple-tables-powershell/monitor-pipeline-runs-succeded-7.png)

1. Klicken Sie optional unter **Aktionen** auf den Link **View Activity Runs** (Aktivitätsausführungen anzeigen), um alle Aktivitätsausführungen anzuzeigen, die dieser Pipelineausführung zugeordnet sind. 

## <a name="review-the-final-results"></a>Überprüfen der Endergebnisse
Führen Sie in SQL Server Management Studio die folgenden Abfragen für die Zieldatenbank aus, um sicherzustellen, dass die aktualisierten bzw. neuen Daten aus den Quelltabellen in die Zieltabellen kopiert wurden. 

**Abfrage** 
```sql
select * from customer_table
```

**Ausgabe**
```
===========================================
PersonID    Name    LastModifytime
===========================================
1           John    2017-09-01 00:56:00.000
2           Mike    2017-09-02 05:23:00.000
3           NewName 2017-09-08 00:00:00.000
4           Andy    2017-09-04 03:21:00.000
5           Anny    2017-09-05 08:06:00.000
```

Beachten Sie die neuen Werte in **Name** und **LastModifytime** für **PersonID** für Nummer 3. 

**Abfrage**

```sql
select * from project_table
```

**Ausgabe**

```
===================================
Project     Creationtime
===================================
project1    2015-01-01 00:00:00.000
project2    2016-02-02 01:23:00.000
project3    2017-03-04 05:16:00.000
NewProject  2017-10-01 00:00:00.000
```

Beachten Sie, dass der Projekttabelle (project_table) der Eintrag **NewProject** hinzugefügt wurde. 

**Abfrage**

```sql
select * from watermarktable
```

**Ausgabe**

```
======================================
TableName       WatermarkValue
======================================
customer_table  2017-09-08 00:00:00.000
project_table   2017-10-01 00:00:00.000
```

Beachten Sie, dass die Grenzwerte für beide Tabellen aktualisiert wurden.
     
## <a name="next-steps"></a>Nächste Schritte
In diesem Tutorial haben Sie die folgenden Schritte ausgeführt: 

> [!div class="checklist"]
> * Vorbereiten von Quell- und Zieldatenspeichern
> * Erstellen einer Data Factory.
> * Erstellen einer selbstgehosteten Integration Runtime (IR)
> * Installieren der Integration Runtime
> * Erstellen Sie verknüpfte Dienste. 
> * Erstellen des Quell-, Senken-, Grenzwertdatasets
> * Erstellen, Ausführen und Überwachen einer Pipeline
> * Überprüfen Sie die Ergebnisse.
> * Hinzufügen oder Aktualisieren von Daten in Quelltabellen
> * Erneutes Ausführen und Überwachen der Pipeline
> * Überprüfen der Endergebnisse

Fahren Sie mit dem nächsten Tutorial fort, um zu erfahren, wie Sie mithilfe eines Spark-Clusters in Azure Daten transformieren:

> [!div class="nextstepaction"]
>[Inkrementelles Laden von Daten aus Azure SQL-Datenbank in Azure Blob Storage mit Informationen der Änderungsnachverfolgung](tutorial-incremental-copy-change-tracking-feature-powershell.md)


