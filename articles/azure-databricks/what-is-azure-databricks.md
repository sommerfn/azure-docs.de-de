---
title: Was ist Azure Databricks?
description: Hier erfahren Sie mehr über Azure Databricks und dazu, wie es Spark in Databricks in Azure integriert. Azure Databricks ist eine Apache Spark-basierte Analyseplattform, die für die Microsoft Azure-Clouddienstplattform optimiert ist.
services: azure-databricks
author: mamccrea
ms.reviewer: jasonh
ms.service: azure-databricks
ms.workload: big-data
ms.topic: overview
ms.date: 05/08/2019
ms.author: mamccrea
ms.custom: mvc
ms.openlocfilehash: 0864db682141b1d7f8cb0c9ff14c9897fb02fe1b
ms.sourcegitcommit: 300cd05584101affac1060c2863200f1ebda76b7
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 05/08/2019
ms.locfileid: "65412750"
---
# <a name="what-is-azure-databricks"></a>Was ist Azure Databricks?

Azure Databricks ist eine Apache Spark-basierte Analyseplattform, die für die Microsoft Azure-Clouddienstplattform optimiert ist. Databricks wurde zusammen mit den Gründern von Apache Spark entworfen und ist in Azure integriert, um Folgendes zu ermöglichen: Einrichtung mit nur einem Klick, optimierte Workflows und einen interaktiven Arbeitsbereich für die Zusammenarbeit von Datenspezialisten, Data Engineers und Business Analysts.

![Was ist Azure Databricks?](./media/what-is-azure-databricks/azure-databricks-overview.png "Was ist Azure Databricks?")

Azure Databricks ist ein schneller, einfacher und zusammenarbeitsorientierter Analysedienst auf Apache Spark-Basis. Für eine Big Data-Pipeline werden Rohdaten oder strukturierte Daten in Batches über Azure Data Factory in Azure erfasst oder mithilfe von Kafka, Event Hub oder IoT Hub nahezu in Echtzeit gestreamt. Diese Daten werden langfristig in einem Data Lake (Azure Blob Storage oder Azure Data Lake Storage) gespeichert. Mit Azure Databricks können Sie im Rahmen Ihres Analyseworkflows Daten aus mehreren Datenquellen (beispielsweise aus [Azure Blob Storage](../storage/blobs/storage-blobs-introduction.md), [Azure Data Lake Storage](../data-lake-store/index.md), [Azure Cosmos DB](../cosmos-db/index.yml) oder [Azure SQL Data Warehouse](../sql-data-warehouse/index.md)) lesen und mithilfe von Spark bahnbrechende Erkenntnisse gewinnen.

![Databricks-Pipeline](./media/what-is-azure-databricks/databricks-pipeline.png)

## <a name="apache-spark-based-analytics-platform"></a>Apache Spark-basierte Analyseplattform

Azure Databricks umfasst sämtliche Open Source-Technologien und -Funktionen für Apache Spark-Cluster. Spark in Azure Databricks enthält folgende Komponenten:

![Apache Spark in Azure Databricks](./media/what-is-azure-databricks/apache-spark-ecosystem-databricks.png "Apache Spark in Azure Databricks")

* **Spark SQL und DataFrames**: Spark SQL ist das Spark-Modul für die Verwendung strukturierter Daten. Ein DataFrame ist eine verteilte Sammlung von Daten, die in benannten Spalten organisiert sind. Hierbei handelt es sich um das gleiche Konzept wie bei einer Tabelle in einer relationalen Datenbank oder bei einem Datenrahmen in R/Python.

* **Streaming**: Echtzeitdatenverarbeitung und -analyse für analytische und interaktive Anwendungen. Bietet HDFS-, Flume- und Kafka-Integration.

* **MLib**: Machine Learning-Bibliothek mit gängigen Lernalgorithmen und Hilfsprogrammen – einschließlich Klassifizierung, Regression, Clustering, kombinierten Filtern und Reduktion der Anzahl von Dimensionen sowie zugrunde liegenden Optimierungsprimitiven.

* **GraphX**: Diagramme und Diagrammberechnung für ein breites Spektrum von Anwendungsfällen – von kognitiven Analysen bis hin zu Datenuntersuchungen.

* **Spark Core-API**: Bietet Unterstützung für R, SQL, Python, Scala und Java.

## <a name="apache-spark-in-azure-databricks"></a>Apache Spark in Azure Databricks

Azure Databricks basiert auf den Funktionen von Spark und bietet eine verwaltungsfreie Cloudplattform mit Folgendem:

- Vollständig verwaltete Spark-Cluster
- Interaktiver Arbeitsbereich zur Untersuchung und Visualisierung
- Plattform für Ihre bevorzugten Spark-basierten Anwendungen

### <a name="fully-managed-apache-spark-clusters-in-the-cloud"></a>Vollständig verwaltete Apache Spark-Cluster in der Cloud

Azure Databricks verfügt über eine sichere und zuverlässige Produktionsumgebung in der Cloud, die von Spark-Experten verwaltet und betreut wird. Ihre Möglichkeiten:

* Sekundenschnelle Clustererstellung
* Dynamische automatische Clusterskalierung (auch für serverlose Cluster) und teamübergreifende Clusterverwendung 
* Programmgesteuerte Verwendung von Clustern mithilfe der REST-APIs 
* Verwendung sicherer, Spark-basierter Datenintegrationsfunktionen zur Vereinheitlichung Ihrer Daten ohne Zentralisierung 
* Sofortiger Zugriff auf die neuesten Apache Spark-Features jeder neuen Version

### <a name="databricks-runtime"></a>Databricks-Laufzeit
Die Databricks-Laufzeit basiert auf Apache Spark und ist von Grund auf für die Azure-Cloud konzipiert. 

Mit der **serverlosen** Option können Sie Ihre Dateninfrastruktur dank vollständiger Abstrahierung der Infrastrukturkomplexität ohne spezielles Fachwissen einrichten und konfigurieren. Datenspezialisten können mit der serverlosen Option Daten schnell als Team durchlaufen.

Für Data Engineers, die sich für die Leistung von Produktionsaufträgen interessieren, bietet Azure Databricks ein Spark-Modul, das dank verschiedenster Optimierungen auf E/A- und Verarbeitungsebene (Databricks-E/A) schneller und performanter ist.

### <a name="workspace-for-collaboration"></a>Arbeitsbereich für die Zusammenarbeit

Über eine zusammenarbeitsorientierte und integrierte Umgebung optimiert Azure Databricks das Untersuchen von Daten, das Erstellen von Prototypen sowie das Ausführen datengesteuerter Anwendungen in Spark.

* Ermitteln Sie dank unkomplizierter Datenuntersuchung, wie Sie Daten verwenden.
* Dokumentieren Sie Ihre Fortschritte in Notizbüchern in R, Python, Scala oder SQL.
* Visualisieren Sie Daten mit wenigen Mausklicks, und verwenden Sie vertraute Tools wie Matplotlib, ggplot oder d3.
* Erstellen Sie mithilfe interaktiver Dashboards dynamische Berichte.
* Verwenden Sie Spark, und interagieren Sie gleichzeitig mit den Daten.

## <a name="enterprise-security"></a>Unternehmenssicherheit

Azure Databricks bietet Azure-Sicherheit für Unternehmen – einschließlich Azure Active Directory-Integration, rollenbasierter Zugriffssteuerung und SLAs zum Schutz Ihrer Daten und Ihres Unternehmens.

* Dank Azure Active Directory-Integration können Sie vollständige Azure-basierte Lösungen mit Azure Databricks ausführen.
* Die rollenbasierte Zugriffssteuerung von Azure Databricks ermöglicht die Verwendung präziser Benutzerberechtigungen für Notizbücher, Cluster, Aufträge und Daten.
* SLAs auf Unternehmensniveau. 

## <a name="integration-with-azure-services"></a>Integration in Azure-Dienste

Azure Databricks ist tief in Azure-Datenbanken und -Speicher integriert: SQL Data Warehouse, Cosmos DB, Data Lake Store und Blob Storage. 

## <a name="integration-with-power-bi"></a>Power BI-Integration
Dank umfangreicher Power BI-Integration können Sie mit Azure Databricks schnell und einfach wichtige Erkenntnisse gewinnen und weitergeben. Sie können aber auch andere BI-Tools wie etwa Tableau Software über JDBC-/ODBC-Clusterendpunkte verwenden.

## <a name="next-steps"></a>Nächste Schritte

* [Schnellstart: Ausführen eines Spark-Auftrags für Azure Databricks](quickstart-create-databricks-workspace-portal.md)
* [Clusters](https://docs.azuredatabricks.net/user-guide/clusters/index.html) (Cluster)
* [Notebooks](https://docs.azuredatabricks.net/user-guide/notebooks/index.html) (Notizbücher)
* [Jobs](https://docs.azuredatabricks.net/user-guide/jobs.html) (Aufträge)

 









