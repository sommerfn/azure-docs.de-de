---
title: Cosmos DB-Migrationsoptionen
description: In diesem Dokument werden die verschiedenen Optionen zum Migrieren Ihrer lokalen oder Clouddaten zu Azure Cosmos DB beschrieben.
author: bharathsreenivas
ms.service: cosmos-db
ms.topic: conceptual
ms.date: 10/23/2019
ms.author: bharathb
ms.openlocfilehash: 9111193bb441487b9e3c49bc9ee1a296d49f8a31
ms.sourcegitcommit: 7efb2a638153c22c93a5053c3c6db8b15d072949
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 10/24/2019
ms.locfileid: "72882388"
---
# <a name="options-to-migrate-your-on-premises-or-cloud-data-to-azure-cosmos-db"></a>Optionen zum Migrieren von lokalen oder Clouddaten zu Azure Cosmos DB

Sie können Daten aus verschiedenen Datenquellen in Azure Cosmos DB laden. Darüber hinaus unterstützt Azure Cosmos DB verschiedene APIs, die als Ziele genutzt werden können. Um Migrationspfade aus den verschiedenen Quellen zu den unterschiedlichen Azure Cosmos DB-APIs zu unterstützen, gibt es mehrere Lösungen, die eine spezielle Behandlung für jeden Migrationspfad bereitstellen. In diesem Dokument werden die verfügbaren Lösungen aufgeführt und ihre Vor- und Nachteile beschrieben.

## <a name="factors-affecting-the-choice-of-migration-tool"></a>Faktoren für die Auswahl des Migrationstools

Die folgenden Faktoren haben Einfluss auf die Auswahl des Migrationstools:
* **Online- oder Offlinemigration:** Viele Migrationstools bieten nur einen Pfad für eine einmalige Migration. Dies bedeutet, dass bei Anwendungen, die auf die Datenbank zugreifen, möglicherweise eine gewisse Ausfallzeit auftritt. Einige Migrationslösungen bieten eine Möglichkeit, eine Livemigration durchzuführen, bei der eine Replikationspipeline zwischen der Quelle und dem Ziel eingerichtet ist.

* **Datenquelle**: Die vorhandenen Daten können sich in verschiedenen Datenquellen wie Oracle DB2, DataStax Cassanda, Azure SQL Server, PostgreSQL usw. befinden. Außerdem können sich die Daten auch in einem vorhandenen Azure Cosmos DB-Konto befinden und das Ziel der Migration besteht darin, das Datenmodell zu ändern oder die Daten in einem Container mit einem anderen Partitionsschlüssel neu zu partitionieren.

* **Azure Cosmos DB-API:** Für die SQL-API in Azure Cosmos DB gibt es eine Reihe von Tools, die vom Azure Cosmos DB-Team für die unterschiedlichen Migrationsszenarien entwickelt wurden. Alle anderen APIs enthalten einen eigenen spezialisierten Satz von Tools, der von der Community entwickelt und verwaltet wird. Da Azure Cosmos DB diese APIs auf einer festen Protokollebene unterstützt, sollten diese Tools während der Migration von Daten zu Azure Cosmos DB ebenfalls unverändert funktionieren. Sie erfordern jedoch möglicherweise eine benutzerdefinierte Behandlung der Drosselung, da dieses Konzept für Azure Cosmos DB spezifisch ist.

* **Datengröße:** Die meisten Migrationstools funktionieren bei kleineren Datasets sehr gut. Wenn das Dataset jedoch einige Hundert Gigabyte überschreitet, ist die Auswahl an Migrationstools beschränkt. 

* **Erwartete Migrationsdauer:** Migrationsvorgänge können so konfiguriert werden, dass sie langsam und schrittweise ausgeführt werden und damit weniger Durchsatz beanspruchen, oder so, dass der gesamten Durchsatz des Ziel Azure Cosmos DB-Zielcontainers genutzt wird, um die Migration in kürzerer Zeit abzuschließen.

## <a name="azure-cosmos-db-sql-api"></a>SQL-API von Azure Cosmos DB
|**Migrationstyp**|**Lösung**|**Überlegungen**|
|---------|---------|---------|
|Offline|[Datenmigrationstool](https://docs.microsoft.com/azure/cosmos-db/import-data)|&bull; Einfache Einrichtung und Unterstützung mehrerer Quellen <br/>&bull; Nicht geeignet für große Datasets|
|Offline|[Azure Data Factory](https://docs.microsoft.com/azure/data-factory/connector-azure-cosmos-db)|&bull; Einfache Einrichtung und Unterstützung mehrerer Quellen <br/>&bull; Nutzt die Azure Cosmos DB-BulkExecutor-Bibliothek <br/>&bull; Geeignet für große Datasets <br/>&bull; Fehlende Prüfpunktausführung: Wenn während der Migration ein Problem auftritt, müssen Sie den gesamten Migrationsprozess neu starten.<br/>&bull; Fehlende Warteschlange für unzustellbare Nachrichten: Dies bedeutet, dass wenige fehlerhafte Dateien den gesamten Migrationsprozess unterbrechen können.|
|Offline|[Azure Cosmos DB-Spark-Connector](https://docs.microsoft.com/azure/cosmos-db/spark-connector)|&bull; Nutzt die Azure Cosmos DB-BulkExecutor-Bibliothek <br/>&bull; Geeignet für große Datasets <br/>&bull; Erfordert eine benutzerdefinierte Spark-Einrichtung <br/>&bull; Spark bietet keine Toleranz gegenüber Inkonsistenzen beim Schema. Dies kann während der Migration ein Problem darstellen. |
|Offline|[Benutzerdefiniertes Tool mit Cosmos DB-BulkExecutor-Bibliothek](https://docs.microsoft.com/azure/cosmos-db/migrate-cosmosdb-data)|&bull; Bietet Prüfpunktfunktionen für unzustellbare Nachrichten und damit eine höhere Resilienz bei der Migration <br/>&bull; Geeignet für sehr große Datasets (> 10 TB)  <br/>&bull; Erfordert die benutzerdefinierte Einrichtung dieses Tools, das als App Service ausgeführt wird |
|Online|[Cosmos DB-Funktionen + ChangeFeed-API](https://docs.microsoft.com/azure/cosmos-db/change-feed-functions)|&bull; Einfache Einrichtung <br/>&bull; Funktioniert nur, wenn die Quelle ein Azure Cosmos DB-Container ist <br/>&bull; Nicht geeignet für große Datasets <br/>&bull; Erfasst keine Löschvorgänge im Quellcontainer |
|Online|[Benutzerdefinierter Migrationsdienst mithilfe von ChangeFeed](https://aka.ms/CosmosDBMigrationSample)|&bull; Ermöglicht die Statusnachverfolgung <br/>&bull; Funktioniert nur, wenn die Quelle ein Azure Cosmos DB-Container ist <br/>&bull; Funktioniert auch für größere Datasets <br/>&bull; Erfordert die Einrichtung einer App Service-Instanz zum Hosten der Änderungsfeedverarbeitung <br/>&bull; Erfasst keine Löschvorgänge im Quellcontainer|
|Online|[Striim](https://docs.microsoft.com/azure/cosmos-db/cosmosdb-sql-api-migrate-data-striim)|&bull; Funktioniert mit einer Vielzahl von Quellen wie Oracle, DB2, SQL Server <br/>&bull; Einfaches Erstellen von ETL-Pipelines; einschließlich eines Dashboards für die Überwachung <br/>&bull; Unterstützt größere Datasets <br/>&bull; Da dies ein Drittanbietertool ist, muss es über den Marketplace erworben und in der Benutzerumgebung installiert werden.|

## <a name="azure-cosmos-db-mongo-api"></a>Mongo-API von Azure Cosmos DB
|**Migrationstyp**|**Lösung**|**Überlegungen**|
|---------|---------|---------|
|Offline|[Datenmigrationstool](https://docs.microsoft.com/azure/cosmos-db/import-data)|&bull; Einfache Einrichtung und Unterstützung mehrerer Quellen <br/>&bull; Nicht geeignet für große Datasets|
|Offline|[Azure Data Factory](https://docs.microsoft.com/azure/data-factory/connector-azure-cosmos-db)|&bull; Einfache Einrichtung und Unterstützung mehrerer Quellen <br/>&bull; Nutzt die Azure Cosmos DB-BulkExecutor-Bibliothek <br/>&bull; Geeignet für große Datasets <br/>&bull; Fehlende Prüfpunktausführung: Wenn während der Migration ein Problem auftritt, muss der gesamte Migrationsprozess neu gestartet werden.<br/>&bull; Fehlende Warteschlange für unzustellbare Nachrichten: Dies bedeutet, dass wenige fehlerhafte Dateien den gesamten Migrationsprozess unterbrechen können. <br/>&bull; Erfordert benutzerdefinierten Code, um den Lesedurchsatz für bestimmte Datenquellen zu erhöhen|
|Offline|[Vorhandene Mongo-Tools (mongodump, mongorestore, Studio3T)](https://azure.microsoft.com/resources/videos/using-mongodb-tools-with-azure-cosmos-db/)|&bull; Einfache Einrichtung und Integration <br/>&bull; Erfordert die benutzerdefinierte Behandlung von Drosselungen|
|Online|[Azure Database Migration Service](https://docs.microsoft.com/azure/dms/tutorial-mongodb-cosmos-db-online)|&bull; Nutzt die Azure Cosmos DB-BulkExecutor-Bibliothek <br/>&bull; Eignet sich für große Datasets und übernimmt die Replikation von Liveänderungen <br/>&bull; Funktioniert nur mit anderen MongoDB-Quellen|

## <a name="azure-cosmos-db-cassandra-api"></a>Cassandra-API für Azure Cosmos DB
|**Migrationstyp**|**Lösung**|**Überlegungen**|
|---------|---------|---------|
|Offline|[cqlsh-Befehl COPY](https://docs.microsoft.com/azure/cosmos-db/cassandra-import-data#migrate-data-using-cqlsh-copy-command)|&bull; Einfache Einrichtung <br/>&bull; Nicht geeignet für große Datasets <br/>&bull; Funktioniert nur, wenn die Quelle eine Cassandra-Tabelle ist|
|Offline|[Kopieren der Tabelle mit Spark](https://docs.microsoft.com/azure/cosmos-db/cassandra-import-data#migrate-data-using-spark) |&bull; Ermöglicht die Verwendung von Spark-Funktionen zum Parallelisieren von Transformation und Erfassung <br/>&bull; Erfordert die Konfiguration mit einer benutzerdefinierten Wiederholungsrichtlinie für die Behandlung der Drosselung|
|Online|[Striim (aus Oracle DB/Apache Cassandra)](https://docs.microsoft.com/azure/cosmos-db/cosmosdb-cassandra-api-migrate-data-striim)|&bull; Funktioniert mit einer Vielzahl von Quellen wie Oracle, DB2, SQL Server <br/>&bull; Einfaches Erstellen von ETL-Pipelines; einschließlich eines Dashboards für die Überwachung <br/>&bull; Unterstützt größere Datasets <br/>&bull; Da dies ein Drittanbietertool ist, muss es über den Marketplace erworben und in der Benutzerumgebung installiert werden.|
|Online|[Blitzz (aus Oracle DB/Apache Cassandra)](https://docs.microsoft.com/azure/cosmos-db/oracle-migrate-cosmos-db-blitzz)|<br/>&bull; Unterstützt größere Datasets <br/>&bull; Da dies ein Drittanbietertool ist, muss es über den Marketplace erworben und in der Benutzerumgebung installiert werden.|

## <a name="other-apis"></a>Andere APIs
Bei anderen APIs als SQL-API, Mongo-API und Cassandra-API werden in den Umgebungen der einzelnen APIs verschiedene Tools unterstützt. 

**Tabellen-API** 
* [Datenmigrationstool](https://docs.microsoft.com/azure/cosmos-db/table-import#data-migration-tool)
* [AzCopy](https://docs.microsoft.com/azure/cosmos-db/table-import#migrate-data-by-using-azcopy)

**Gremlin-API**
* [Graph-BulkExecutor-Bibliothek](https://docs.microsoft.com/azure/cosmos-db/bulk-executor-graph-dotnet)
* [Gremlin Spark](https://github.com/Azure/azure-cosmosdb-spark/blob/2.4/samples/graphframes/main.scala) 

## <a name="next-steps"></a>Nächste Schritte

* Erfahren Sie mehr darüber, indem Sie die Beispielanwendungen ausprobieren, die die BulkExecutor-Bibliothek in [.NET](bulk-executor-dot-net.md) und [Java](bulk-executor-java.md) nutzen. 
* Die BulkExecutor-Bibliothek ist in den Cosmos DB Spark-Connector integriert. Weitere Informationen finden Sie im Artikel [Spark-Connector für Azure Cosmos DB](spark-connector.md).  
* Wenden Sie sich an das Azure Cosmos DB-Team, indem Sie unter dem Problemtyp „General Advisory“ (Allgemeine Ratschläge) und dem Problemuntertyp „Large (TB+) migrations“ (Umfangreiche Migrationen (TB+)) ein Supportticket erstellen, um weitere Hilfe zu größeren Migrationen zu erhalten.
