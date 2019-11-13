---
title: Apache Cassandra-Features und -Befehle, die von der Cassandra-API für Azure Cosmos DB unterstützt werden
description: Informationen zur Unterstützung der Apache Cassandra-Features in der Cassandra-API für Azure Cosmos DB
author: kanshiG
ms.author: govindk
ms.reviewer: sngun
ms.service: cosmos-db
ms.subservice: cosmosdb-cassandra
ms.topic: overview
ms.date: 09/24/2018
ms.openlocfilehash: 12df79696033e69abbf48f053c1a594be9409cda
ms.sourcegitcommit: bc7725874a1502aa4c069fc1804f1f249f4fa5f7
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 11/07/2019
ms.locfileid: "73721111"
---
# <a name="apache-cassandra-features-supported-by-azure-cosmos-db-cassandra-api"></a>Apache Cassandra-Features, die von der Cassandra-API für Azure Cosmos DB unterstützt werden 

Azure Cosmos DB ist ein global verteilter Datenbankdienst von Microsoft mit mehreren Modellen. Sie können mit der Cassandra-API für Azure Cosmos DB über Cassandra Open-Source-Client[treiber](https://cassandra.apache.org/doc/latest/getting_started/drivers.html?highlight=driver) des Typs „Cassandra-Abfragesprache v4 [Verbindungsprotokoll](https://github.com/apache/cassandra/blob/trunk/doc/native_protocol_v4.spec)-kompatibel“ kommunizieren. 

Durch Verwendung der Cassandra-API für Azure Cosmos DB können Sie die Vorteile der Apache Cassandra-APIs sowie die von Azure Cosmos DB gebotenen Unternehmensfunktionen nutzen. Die Unternehmensfunktionen umfassen [globale Verteilung](distribute-data-globally.md), [automatische Partitionierung mit horizontalem Hochskalieren](partition-data.md), Gewährleistung der Verfügbarkeit und Latenz, Verschlüsselung bei REST, Sicherungen und vieles mehr.

## <a name="cassandra-protocol"></a>Cassandra-Protokoll 

Die Cassandra-API für Azure Cosmos DB ist kompatibel mit CQL, Version **v4**. Die unterstützten CQL-Befehle, Tools, Einschränkungen und Ausnahmen sind nachstehend aufgeführt. Alle Clienttreiber, die diese Protokolle verstehen, sollten auch zur Cassandra-API für Azure Cosmos DB eine Verbindung herstellen können.

## <a name="cassandra-driver"></a>Cassandra-Treiber

Die folgenden Versionen von Cassandra-Treibern werden von der Cassandra-API für Azure Cosmos DB unterstützt:

* [Java 3.5+](https://github.com/datastax/java-driver)  
* [C# 3.5+](https://github.com/datastax/csharp-driver)  
* [Nodejs 3.5+](https://github.com/datastax/nodejs-driver)  
* [Python 3.15+](https://github.com/datastax/python-driver)  
* [C++ 2.9](https://github.com/datastax/cpp-driver)   
* [PHP 1.3](https://github.com/datastax/php-driver)  
* [Gocql](https://github.com/gocql/gocql)  
 
## <a name="cql-data-types"></a>CQL-Datentypen 

Die Cassandra-API für Azure Cosmos DB unterstützt die folgenden CQL-Datentypen:

* ascii  
* bigint  
* Blob  
* boolean  
* Zähler  
* date  
* decimal  
* double  
* float  
* frozen  
* inet  
* int  
* list  
* set  
* smallint  
* text  
* time  
* timestamp  
* timeuuid  
* tinyint  
* tuple  
* uuid  
* varchar  
* varint  
* tuples  
* udts  
* map  

## <a name="cql-functions"></a>CQL-Funktionen

Die Cassandra-API für Azure Cosmos DB unterstützt die folgenden CQL-Funktionen:

* Tokenverschlüsselung  
* Aggregatfunktionen
  * min, max, avg, count
* Blob-Konvertierungsfunktionen 
  * typeAsBlob(value)  
  * blobAsType(value)
* UUID- und timeuuid-Funktionen 
  * dateOf()  
  * now()  
  * minTimeuuid()  
  * unixTimestampOf()  
  * toDate(timeuuid)  
  * toTimestamp(timeuuid)  
  * toUnixTimestamp(timeuuid)  
  * toDate(timestamp)  
  * toUnixTimestamp(timestamp)  
  * toTimestamp(date)  
  * toUnixTimestamp(date) 
  


## <a name="cassandra-api-limits"></a>Einschränkungen für die Cassandra-API

Bei der Cassandra-API für Azure Cosmos DB gibt es keine Einschränkungen hinsichtlich der Größe von Daten, die in einer Tabelle gespeichert sind. Es können Hunderte von Terabytes oder Petabytes von Daten gespeichert werden. Gleichzeitig wird sichergestellt, dass die Grenzwerte für Partitionsschlüssel berücksichtigt werden. In ähnlicher Weise gibt es bei jedem Entitäts- oder Zeilenäquivalent keine Einschränkungen hinsichtlich der Anzahl von Spalten. Die Gesamtgröße der Entität sollte jedoch 2 MB nicht überschreiten. Der Daten für jeden Partitionsschlüssel dürfen in allen anderen APIs 10 GB nicht überschreiten.

## <a name="tools"></a>Tools 

Die Cassandra-API für Azure Cosmos DB ist eine verwaltete Dienstplattform. Sie erfordert keinen Verwaltungsaufwand oder Dienstprogramme wie Garbage Collector, Java Virtual Machine (JVM) und Nodetool zum Verwalten des Clusters. Sie unterstützt Tools wie cqlsh, das die binäre CQLv4-Kompatibilität nutzt. 

* Der Daten-Explorer des Azure-Portals, Metriken, Protokolldiagnose, PowerShell und CLI sind weitere unterstützte Mechanismen zur Verwaltung des Kontos.

## <a name="cql-shell"></a>CQL Shell  

Im CQLSH-Befehlszeilen-Hilfsprogramm ist Apache Cassandra 3.1.1 enthalten, das standardmäßig durch Festlegung einiger Umgebungsvariablen funktioniert.

**Windows:**

Wenn Sie Windows verwenden, empfiehlt es sich, das [Windows-Dateisystem für Linux](https://docs.microsoft.com/en-us/windows/wsl/install-win10#install-the-windows-subsystem-for-linux) zu aktivieren. Anschließend können Sie die folgenden Linux-Befehle verwenden.

**Unix/Linux/Mac:**

```bash
# Install default-jre and default-jdk
sudo apt install default-jre
sudo apt-get update
sudo apt install default-jdk

# Import the Baltimore CyberTrust root certificate:
curl https://cacert.omniroot.com/bc2025.crt > bc2025.crt
keytool -importcert -alias bc2025ca -file bc2025.crt

# Install the Cassandra libraries in order to get CQLSH:
echo "deb http://www.apache.org/dist/cassandra/debian 311x main" | sudo tee -a /etc/apt/sources.list.d/cassandra.sources.list
curl https://www.apache.org/dist/cassandra/KEYS | sudo apt-key add -
sudo apt-get update
sudo apt-get install cassandra

# Export the SSL variables:
export SSL_VERSION=TLSv1_2
export SSL_VALIDATE=false

# Connect to Azure Cosmos DB API for Cassandra:
cqlsh <YOUR_ACCOUNT_NAME>.cassandra.cosmosdb.azure.com 10350 -u <YOUR_ACCOUNT_NAME> -p <YOUR_ACCOUNT_PASSWORD> --ssl

```

## <a name="cql-commands"></a>CQL-Befehle

Azure Cosmos DB unterstützt die folgenden Datenbankbefehle für Cassandra-API-Konten.

* CREATE KEYSPACE (Die Replikationseinstellungen für diesen Befehl werden ignoriert.)
* CREATE TABLE 
* ALTER TABLE 
* USE 
* INSERT 
* SELECT 
* UPDATE 
* BATCH – nur nicht protokollierte Befehle werden unterstützt. 
* DELETE

Alle CRUD-Vorgänge geben bei Ausführung über ein CQL v4-kompatibles SDK zusätzliche Informationen zu Fehlern und beanspruchten Anforderungseinheiten zurück. Bei der Verarbeitung der Befehle DELETE und UPDATE sollte die Ressourcenkontrolle in Erwägung gezogen werden, um die effizienteste Verwendung des bereitgestellten Durchsatzes sicherzustellen.

* Hinweis: Der Wert „gc_grace_seconds“ muss Null sein, wenn er angegeben wird.

```csharp
var tableInsertStatement = table.Insert(sampleEntity); 
var insertResult = await tableInsertStatement.ExecuteAsync(); 
 
foreach (string key in insertResult.Info.IncomingPayload) 
        { 
            byte[] valueInBytes = customPayload[key]; 
            double value = Encoding.UTF8.GetString(valueInBytes); 
            Console.WriteLine($"CustomPayload:  {key}: {value}"); 
        } 
```

## <a name="consistency-mapping"></a>Konsistenzzuordnung 

Die Cassandra-API für Azure Cosmos DB ermöglicht die Wahl der Konsistenz bei Lesevorgängen.  Ausführliche Informationen zur Konsistenzzuordnung finden Sie [hier](consistency-levels-across-apis.md#cassandra-mapping).

## <a name="permission-and-role-management"></a>Berechtigungs- und Rollenverwaltung

Azure Cosmos DB unterstützt die rollenbasierte Zugriffssteuerung (RBAC) für die Bereitstellung, Rotation von Schlüsseln, Anzeige von Metriken sowie Lese-/Schreibkennwörter bzw. Lese-/Schreibschlüssel und Schreibschutzkennwörter/-schlüssel, die über das [Azure-Portal](https://portal.azure.com) abgerufen werden können. Azure Cosmos DB unterstützt keine Rollen für CRUD-Aktivitäten.

## <a name="keyspace-and-table-options"></a>Keyspace- und Tabellenoptionen

Die Optionen für Regionsname, Klasse, Replikationsfaktor und Rechenzentrum im Befehl „CREATE KEYSPACE“ werden derzeit ignoriert. Das System verwendet die Replikationsmethode der [globalen Datenverteilung](global-dist-under-the-hood.md) der zugrunde liegenden Azure Cosmos DB-Instanz, um die Regionen hinzuzufügen. Wenn die Daten regionsübergreifend vorhanden sein müssen, können Sie dies mithilfe von PowerShell, per CLI oder über das Portal auf der Kontoebene aktivieren. Weitere Informationen finden Sie unter [Hinzufügen/Entfernen von Regionen für Ihr Datenbankkonto](how-to-manage-database-account.md#addremove-regions-from-your-database-account). „Durable_writes“ kann nicht deaktiviert werden, da Azure Cosmos DB sicherstellt, dass jeder Schreibvorgang dauerhaft ist. In jeder Region repliziert Azure Cosmos DB die Daten innerhalb der gesamten Replikatgruppe, die sich aus vier Replikaten zusammensetzt. Diese [Konfiguration](global-dist-under-the-hood.md) der Replikatgruppe kann nicht geändert werden.
 
Beim Erstellen der Tabelle werden mit Ausnahme von „gc_grace_seconds“ alle Optionen ignoriert, und „gc_grace_seconds“ muss auf Null festgelegt werden.
Für den Keyspace und die Tabelle steht eine zusätzliche Option namens „cosmosdb_provisioned_throughput“ mit einem Mindestwert von 400 RU/s zur Verfügung. Der Keyspacedurchsatz ermöglicht die tabellenübergreifende gemeinsame Nutzung des Durchsatzes und ist hilfreich in Szenarien, in denen alle Tabellen nicht den bereitgestellten Durchsatz beanspruchen. Mit dem Befehl „ALTER TABLE“ kann der bereitgestellte Durchsatz regionsübergreifend geändert werden. 

```
CREATE  KEYSPACE  sampleks WITH REPLICATION = {  'class' : 'SimpleStrategy'}   AND cosmosdb_provisioned_throughput=2000;  

CREATE TABLE sampleks.t1(user_id int PRIMARY KEY, lastname text) WITH cosmosdb_provisioned_throughput=2000; 

ALTER TABLE gks1.t1 WITH cosmosdb_provisioned_throughput=10000 ;

```


## <a name="usage-of-cassandra-retry-connection-policy"></a>Verwendung der Cassandra-Richtlinie für die Verbindungswiederholung

Azure Cosmos DB ist ein ressourcengesteuertes System. Das bedeutet, dass pro Sekunde eine bestimmte Anzahl von Vorgängen ausgeführt werden kann – abhängig von den Anforderungseinheiten, die durch die Vorgänge beansprucht werden. Überschreitet eine Anwendung dieses Limit in einer Sekunde, wird die Anforderungsrate begrenzt, und Ausnahmen werden ausgelöst. Diese Ausnahmen werden von der Cassandra-API in Azure Cosmos DB in Überladungsfehler im nativen Cassandra-Protokoll übersetzt. Um sicherzustellen, dass Ihre Anwendung im Falle einer Ratenbegrenzung Anforderungen abfangen und wiederholen kann, werden die Erweiterungen für [Spark](https://mvnrepository.com/artifact/com.microsoft.azure.cosmosdb/azure-cosmos-cassandra-spark-helper) und [Java](https://github.com/Azure/azure-cosmos-cassandra-extensions) bereitgestellt. Wenn Sie über andere SDKs auf die Cassandra-API in Azure Cosmos DB zugreifen, müssen Sie eine Verbindungsrichtlinie erstellen, um für diese Ausnahmen eine Wiederholung durchzuführen.

## <a name="next-steps"></a>Nächste Schritte

- Erste Schritte mit dem [Erstellen eines Cassandra-API-Kontos, einer Datenbank und einer Tabelle](create-cassandra-api-account-java.md) mithilfe einer Java-Anwendung

