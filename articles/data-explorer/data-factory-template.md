---
title: Verwenden der Azure Data Factory-Vorlage zum Massenkopieren aus der Datenbank in Azure Data Explorer
description: In diesem Thema lernen Sie das Verwenden einer Azure Data Factory-Vorlage zum Massenkopieren aus der Datenbank in Azure Data Explorer.
services: data-explorer
author: orspod
ms.author: orspodek
ms.reviewer: tzgitlin
ms.service: data-explorer
ms.topic: conceptual
ms.date: 09/08/2019
ms.openlocfilehash: 5a6aebd276ef8658da9ca763be7da5c38a9c772a
ms.sourcegitcommit: 23389df08a9f4cab1f3bb0f474c0e5ba31923f12
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 09/10/2019
ms.locfileid: "70873431"
---
# <a name="use-azure-data-factory-template-for-bulk-copy-from-database-to-azure-data-explorer"></a>Verwenden der Azure Data Factory-Vorlage zum Massenkopieren aus der Datenbank in Azure Data Explorer

Azure Data Explorer ist ein schneller, vollständig verwalteter Datenanalysedienst für Echtzeitanalysen großer Datenmengen, die aus vielfältigen Quellen wie Anwendungen, Websites und IoT-Geräten gestreamt werden. Azure Data Factory ist ein vollständig verwalteter, cloudbasierter Datenintegrationsdienst. Mit dem Azure Data Factory-Dienst können Sie Ihre Azure Data Explorer-Datenbank mit Daten aus dem vorhandenen System füllen und beim Erstellen von Analyselösungen Zeit sparen. 

[Azure Data Factory-Vorlagen](/azure/data-factory/solution-templates-introduction) sind vordefinierte Azure Data Factory-Pipelines, mit denen Sie schnell in Data Factory einsteigen können, und die die Entwicklungszeit für das Erstellen von Datenintegrationsprojekten verkürzen. Die Vorlage zum **Massenkopieren aus der Datenbank in Azure Data Explorer** wird mithilfe von **Lookup**- und **ForEach**-Aktivitäten erstellt. Sie können die Vorlage verwenden, um pro Datenbank oder Tabelle viele Pipelines zum schnelleren Kopieren von Daten zu erstellen. 

> [!IMPORTANT]
> * Verwenden Sie die Vorlage zum **Massenkopieren aus der Datenbank in Azure Data Explorer**, um große Datenmengen aus Datenbanken wie SQL Server und Google BigQuery in Azure Data Explorer zu kopieren. 
> * Verwenden Sie das [Kopiertool](data-factory-load-data.md), um einige Tabellen mit kleinen oder mittleren Datenmengen in Azure Data Explorer zu kopieren. 

## <a name="prerequisites"></a>Voraussetzungen

* Wenn Sie über kein Azure-Abonnement verfügen, können Sie ein [kostenloses Azure-Konto](https://azure.microsoft.com/free/) erstellen, bevor Sie beginnen.
* [Azure Data Explorer-Cluster und Datenbank](create-cluster-database-portal.md)
* [Erstellen einer Data Factory-Instanz](data-factory-load-data.md#create-a-data-factory)
* Quelle der Daten in der Datenbank

## <a name="create-controltabledataset"></a>Erstellen von ControlTableDataset

Das **ControlTableDataset** gibt an, welche Daten aus der Quelle in das Ziel in der Pipeline kopiert werden. Die Anzahl der Zeilen zeigt die Gesamtzahl der Pipelines an, die zum Kopieren der Daten erforderlich sind. Das **ControlTableDataset** muss als Teil der Quelldatenbank definiert werden.

Beispiel für SQL Server-Quelltabellenformat:
    
```sql   
CREATE TABLE control_table (
PartitionId int,
SourceQuery varchar(255),
ADXTableName varchar(255)
);
```
    
|Eigenschaft  |BESCHREIBUNG  | Beispiel
|---------|---------| ---------|
|PartitionId   |   Kopierauftrag | 1  |  
|SourceQuery   |   Abfrage, die angibt, welche Daten während der Pipelineruntime kopiert werden | <br>`select * from table where lastmodifiedtime  LastModifytime >= ''2015-01-01 00:00:00''>` </br>    
|ADXTableName  |  Name der Zieltabelle | MyAdxTable       |  

Wenn Ihr **ControlTableDataset** in einem anderen Format vorliegt, erstellen Sie ein vergleichbares **ControlTableDataset** für Ihr Format.

## <a name="use-bulk-copy-from-database-to-azure-data-explorer-template"></a>Verwenden der Vorlage zum Massenkopieren aus einer Datenbank in Azure Data Explorer

1. Wählen Sie auf der Seite **Fangen wir an!** die Kachel **Pipeline aus Vorlage erstellen** aus, oder wählen Sie das Stiftsymbol (Registerkarte **Autor**) auf der rechten Seite > **+**  > **Pipeline aus Vorlage** aus, um den Vorlagenkatalog zu öffnen.

    ![Erste Schritte mit Azure Data Factory](media/data-factory-template/adf-get-started.png)

1. Wählen Sie die Vorlage zum **Massenkopieren aus einer Datenbank in Azure Data Explorer** aus.
 
    ![Auswählen der Pipeline aus der Vorlage](media/data-factory-template/pipeline-from-template.png)

1.  Wählen Sie im Fenster **Massenkopieren aus einer Datenbank in Azure Data Explorer** in der Dropdownliste vorhandene Datasets aus. 

    * **ControlTableDataset**: Verknüpfter Dienst zum Steuern der Tabelle, der angibt, welche Daten aus der Quelle in das Ziel kopiert und wo sie im Ziel platziert werden. 
    * **SourceDataset**: mit Quelldatenbank verknüpfter Dienst. 
    * **AzureDataExplorerTable**: Azure Data Explorer-Tabelle. Wenn das Dataset nicht vorhanden ist, [erstellen Sie den mit Azure Data Explorer verknüpften Dienst](data-factory-load-data.md#create-the-azure-data-explorer-linked-service), um das Dataset hinzuzufügen.
    * Klicken Sie auf **Diese Vorlage verwenden**.

    ![Konfigurieren der Azure Data Explorer-Vorlage zum Massenkopieren](media/data-factory-template/configure-bulk-copy-adx-template.png)

1. Wählen Sie einen Bereich in der Canvas außerhalb der Aktivitäten aus, um auf die Vorlagenpipeline zuzugreifen. Wählen Sie **Parameter** aus, um die Parameter für die Tabelle einschließlich **Name** (Name der Steuerungstabelle) und **Standardwert** (Spaltennamen) einzugeben.

    ![Pipelineparameter](media/data-factory-template/pipeline-parameters.png)

1.  Wählen Sie die Lookup-Aktivität **GetPartitionList** aus, um die Standardeinstellungen anzuzeigen. Die Abfrage wird automatisch erstellt.
1.  Wählen Sie die Befehlsaktivität **ForEachPartition** und dann **Einstellungen** aus.
    * **Batchanzahl**: Wählen Sie eine Zahl von 1-50 aus. Diese Auswahl bestimmt die Anzahl der Pipelines, die parallel ausgeführt werden, bis die **ControlTableDataset**-Zeilenanzahl erreicht ist. 
    * Aktivieren Sie NICHT das Kontrollkästchen **Sequenziell**, damit die Pipelinebatches parallel ausgeführt werden.

    ![ForEachPartition-Einstellungen](media/data-factory-template/foreach-partition-settings.png)

    > [!TIP]
    > Die bewährte Vorgehensweise ist die parallele Ausführung vieler Pipelines, damit Daten schneller kopiert werden können. Partitionieren Sie die Daten in der Quelltabelle, und weisen Sie nach Datum und Tabelle eine Partition pro Pipeline zu, um die Effizienz zu erhöhen.

1. Wählen Sie **Alle überprüfen** aus, um die ADF-Pipeline zu überprüfen. Siehe **Pipelineüberprüfungsausgabe**.

    ![Überprüfen der Vorlagenpipelines](media/data-factory-template/validate-template-pipelines.png)

1. Wählen Sie bei Bedarf **Debuggen** aus, und dann **Trigger hinzufügen**, um die Pipeline auszuführen.

    ![Ausführen der Pipeline](media/data-factory-template/trigger-run-of-pipeline.png)    


Sie können jetzt die Vorlage zum **Massenkopieren aus der Datenbank in Azure Data Explorer** verwenden, um große Datenmengen effizient aus ihren Datenbanken und Tabellen zu kopieren.

## <a name="next-steps"></a>Nächste Schritte

* Informieren Sie sich über das [Kopieren von Daten in Azure Data Explorer mithilfe von Azure Data Factory](data-factory-load-data.md).

* Erfahren Sie mehr über den [Azure Data Explorer-Connector](/azure/data-factory/connector-azure-data-explorer) in Azure Data Factory.

* Erfahren Sie mehr über [Azure Data Explorer-Abfragen](/azure/data-explorer/web-query-data) zum Abfragen von Daten.






