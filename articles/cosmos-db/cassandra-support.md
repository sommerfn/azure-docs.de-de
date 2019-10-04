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
ms.openlocfilehash: a6fc9f1a5c32fc9ffa1e1e6ebe525b72030fe803
ms.sourcegitcommit: 1289f956f897786090166982a8b66f708c9deea1
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 06/17/2019
ms.locfileid: "67155663"
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
  


## <a name="cassandra-query-language-limits"></a>Grenzwerte der Cassandra-Abfragesprache

Bei der Cassandra-API für Azure Cosmos DB gibt es keine Einschränkungen hinsichtlich der Größe von Daten, die in einer Tabelle gespeichert sind. Es können Hunderte von Terabytes oder Petabytes von Daten gespeichert werden. Gleichzeitig wird sichergestellt, dass die Grenzwerte für Partitionsschlüssel berücksichtigt werden. In ähnlicher Weise gibt es bei jedem Entitäts- oder Zeilenäquivalent keine Einschränkungen hinsichtlich der Anzahl von Spalten. Die Gesamtgröße der Entität sollte jedoch 2 MB nicht überschreiten.

## <a name="tools"></a>Tools 

Die Cassandra-API für Azure Cosmos DB ist eine verwaltete Dienstplattform. Sie erfordert keinen Verwaltungsaufwand oder Dienstprogramme wie Garbage Collector, Java Virtual Machine (JVM) und Nodetool zum Verwalten des Clusters. Sie unterstützt Tools wie cqlsh, das die binäre CQLv4-Kompatibilität nutzt. 

* Der Daten-Explorer des Azure-Portals, Metriken, Protokolldiagnose, PowerShell und Cli sind weitere unterstützte Mechanismen zur Verwaltung des Kontos.

## <a name="cql-shell"></a>CQL Shell  

Im CQLSH-Befehlszeilen-Hilfsprogramm ist Apache Cassandra 3.1.1 enthalten, und es arbeitet standardmäßig mit folgenden aktivierten Umgebungsvariablen:

[Fügen Sie dem cacerts-Speicher ein Baltimore-Stammzertifikat hinzu](https://docs.microsoft.com/java/azure/java-sdk-add-certificate-ca-store?view=azure-java-stable#to-add-a-root-certificate-to-the-cacerts-store), bevor Sie die folgenden Befehle ausführen. 

**Windows:** 

```bash
set SSL_VERSION=TLSv1_2 
SSL_CERTIFICATE=<path to Baltimore root ca cert>
set CQLSH_PORT=10350 
cqlsh <YOUR_ACCOUNT_NAME>.cassandra.cosmosdb.azure.com 10350 -u <YOUR_ACCOUNT_NAME> -p <YOUR_ACCOUNT_PASSWORD> --ssl 
```
**Unix/Linux/Mac:**

```bash
export SSL_VERSION=TLSv1_2 
export SSL_CERTFILE=<path to Baltimore root ca cert>
cqlsh <YOUR_ACCOUNT_NAME>.cassandra.cosmosdb.azure.com 10350 -u <YOUR_ACCOUNT_NAME> -p <YOUR_ACCOUNT_PASSWORD> --ssl 
```

## <a name="cql-commands"></a>CQL-Befehle

Azure Cosmos DB unterstützt die folgenden Datenbankbefehle für Cassandra-API-Konten.

* CREATE KEYSPACE 
* CREATE TABLE 
* ALTER TABLE 
* USE 
* INSERT 
* SELECT 
* UPDATE 
* BATCH – nur nicht protokollierte Befehle werden unterstützt. 
* DELETE

Alle CRUD-Vorgänge geben bei einer Ausführung über CQLV4-kompatibles SDK zusätzliche Informationen zu einem Fehler, verbrauchten Anforderungseinheiten und Aktivitäts-ID zurück. Die Befehle DELETE und UPDATE müssen unter Berücksichtigung von Ressourcenkontrolle verarbeitet werden, um eine Überlastung von bereitgestellten Ressourcen zu vermeiden. 
* Hinweis: Der Wert „gc_grace_seconds“ muss Null sein, wenn er angegeben wird.

```csharp
var tableInsertStatement = table.Insert(sampleEntity); 
var insertResult = await tableInsertStatement.ExecuteAsync(); 
 
foreach (string key in insertResult.Info.IncomingPayload) 
        { 
            byte[] valueInBytes = customPayload[key]; 
            double value = Encoding.UTF8.GetString(valueInBytes); 
            Console.WriteLine($“CustomPayload:  {key}: {value}”); 
        } 
```

## <a name="consistency-mapping"></a>Konsistenzzuordnung 

Die Cassandra-API für Azure Cosmos DB ermöglicht die Wahl der Konsistenz bei Lesevorgängen.  Die Konsistenzzuordnung finden Sie [hier[(https://docs.microsoft.com/azure/cosmos-db/consistency-levels-across-apis#cassandra-mapping).

## <a name="permission-and-role-management"></a>Berechtigungs- und Rollenverwaltung

Azure Cosmos DB unterstützt die rollenbasierte Zugriffssteuerung (RBAC) für die Bereitstellung, Rotation von Schlüsseln, Anzeige von Metriken sowie Lese-/Schreibkennwörter bzw. Lese-/Schreibschlüssel und Schreibschutzkennwörter/-schlüssel, die über das [Azure-Portal](https://portal.azure.com) abgerufen werden können. Azure Cosmos DB unterstützt noch keine Benutzer und Rollen für CRUD-Aktivitäten (Create, Read, Update, Delete; Erstellen, Lesen, Aktualisieren, Löschen). 

## <a name="planned-support"></a>Geplante Unterstützung 
* Der Regionsname wird derzeit im Befehl „create keyspace“ ignoriert. Die Verteilung von Daten wird über die zugrunde liegende Cosmos DB-Plattform implementiert und über das Portal oder über PowerShell für das Konto verfügbar gemacht. 





## <a name="next-steps"></a>Nächste Schritte

- Erste Schritte mit dem [Erstellen eines Cassandra-API-Kontos, einer Datenbank und einer Tabelle](create-cassandra-api-account-java.md) mithilfe einer Java-Anwendung

