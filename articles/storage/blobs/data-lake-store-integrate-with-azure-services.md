---
title: Integrieren von Azure Data Lake Storage mit Azure-Diensten | Microsoft-Dokumentation
description: Hier erfahren Sie, welche Azure-Dienste mit Azure Data Lake Storage Gen2 integriert werden.
author: normesta
ms.subservice: data-lake-storage-gen2
ms.service: storage
ms.topic: conceptual
ms.date: 10/11/2019
ms.author: normesta
ms.openlocfilehash: e073c02f19a9add37e684a76839a620af0d07ec0
ms.sourcegitcommit: 8b44498b922f7d7d34e4de7189b3ad5a9ba1488b
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 10/13/2019
ms.locfileid: "72302015"
---
# <a name="integrate-azure-data-lake-storage-with-azure-services"></a>Integrieren von Azure Data Lake Storage mit Azure-Diensten

Mit Azure-Diensten können Sie Daten erfassen, Analysen durchführen und visuelle Darstellungen erstellen. Dieser Artikel enthält eine Liste mit unterstützten Azure-Diensten sowie Links zu hilfreichen Artikeln für die Verwendung dieser Dienste mit Azure Data Lake Storage Gen2.

## <a name="azure-services-that-support-azure-data-lake-storage-gen2"></a>Azure-Dienste, die Azure Data Lake Storage Gen2 unterstützen

Die folgende Tabelle enthält die Azure-Dienste, die Azure Data Lake Storage Gen2 unterstützen:

| Azure-Dienst |  Verwandte Artikel |
|---------------|-------------------|
|Azure Data Factory | [Laden von Daten in Azure Data Lake Storage Gen2 mit Azure Data Factory](https://docs.microsoft.com/azure/data-factory/load-azure-data-lake-storage-gen2?toc=%2fazure%2fstorage%2fblobs%2ftoc.json)|
|Azure Databricks | [Azure Data Lake Storage Gen2](https://docs.azuredatabricks.net/data/data-sources/azure/azure-datalake-gen2.html) <br> [Schnellstart: Analysieren von Daten in Azure Data Lake Storage Gen2 mit Azure Databricks](data-lake-storage-quickstart-create-databricks-account.md) <br>[Tutorial: Extrahieren, Transformieren und Laden von Daten mithilfe von Azure Databricks](https://docs.microsoft.com/azure/azure-databricks/databricks-extract-load-sql-data-warehouse) <br>[Tutorial: Zugreifen auf Daten vom Typ „Data Lake Storage Gen2“ mit Azure Databricks unter Verwendung von Spark](data-lake-storage-use-databricks-spark.md) |
|Azure Event Hubs Capture| [Erfassen von Ereignissen über Azure Event Hubs in Azure Blob Storage oder Azure Data Lake Storage](https://docs.microsoft.com/azure/event-hubs/event-hubs-capture-overview)|
|Azure Logic Apps | [Übersicht: Was ist Azure Logic Apps?](https://docs.microsoft.com/azure/logic-apps/logic-apps-overview)|
|Azure Search | [Suchen von Blob Storage mit Azure Search](https://docs.microsoft.com/azure/search/search-blob-storage-integration)|
|Azure Stream Analytics| [Blobspeicher und Azure Data Lake Gen2](https://docs.microsoft.com/azure/stream-analytics/stream-analytics-define-outputs#blob-storage-and-azure-data-lake-gen2) |
|DataBox|  [Verwenden von Azure Data Box zum Migrieren von Daten aus einem lokalen Hadoop Distributed File System-Speicher zu Azure Storage](data-lake-storage-migrate-on-premises-hdfs-cluster.md)|
|HDInsight | [Verwenden von Azure Data Lake Storage Gen2 mit Azure HDInsight-Clustern](https://docs.microsoft.com/azure/hdinsight/hdinsight-hadoop-use-data-lake-storage-gen2?toc=%2fazure%2fstorage%2fblobs%2ftoc.json)<br>[Verwenden der HDFS-CLI mit Data Lake Storage Gen2](data-lake-storage-use-hdfs-data-lake-storage.md) <br>[Tutorial: Extrahieren, Transformieren und Laden von Daten mithilfe von Apache Hive in Azure HDInsight](data-lake-storage-tutorial-extract-transform-load-hive.md) |
|IoT Hub | [Verwenden des IoT Hub-Nachrichtenroutings zum Senden von Gerät-zu-Cloud-Nachrichten an verschiedene Endpunkte](https://docs.microsoft.com/azure/iot-hub/iot-hub-devguide-messages-d2c)|
|Power BI|  [Analysieren von Daten in Azure Data Lake Storage Gen2 mit Power BI](data-lake-storage-use-power-bi.md) |
|SQL Data Warehouse | [Azure SQL Data Warehouse – PolyBase](https://docs.microsoft.com/azure/sql-database/sql-database-vnet-service-endpoint-rule-overview?toc=%2fazure%2fstorage%2fblobs%2ftoc.json#azure-sql-data-warehouse-polybase)|
|SQL Server Integration Services (SSIS) | [Azure Storage-Verbindungs-Manager](https://docs.microsoft.com/sql/integration-services/connection-manager/azure-storage-connection-manager?view=sql-server-2017)|

## <a name="next-steps"></a>Nächste Schritte

- Erfahren Sie mehr darüber, welche Tools Sie für die Datenverarbeitung in Ihrem Data Lake verwenden können: [Verwenden von Azure Data Lake Storage Gen2 für Big Data-Anforderungen](data-lake-storage-data-scenarios.md).

- Erfahren Sie mehr über Data Lake Storage Gen2 und über die ersten Schritte: [Einführung in Azure Data Lake Storage Gen2](data-lake-storage-introduction.md).