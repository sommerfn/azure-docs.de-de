---
title: Azure CLI-Beispiele für Azure Cosmos DB
description: Azure CLI-Beispiele – Erstellen und Verwalten von Konten, Datenbanken, Containern, Regionen und Firewalls von Azure Cosmos DB
author: markjbrown
ms.service: cosmos-db
ms.topic: sample
ms.date: 10/26/2018
ms.author: mjbrown
ms.reviewer: sngun
ms.openlocfilehash: d7ce5e2c12feeee770de8acfd5df81d340e0a7d0
ms.sourcegitcommit: e42c778d38fd623f2ff8850bb6b1718cdb37309f
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 08/19/2019
ms.locfileid: "69615572"
---
# <a name="azure-cli-samples-for-azure-cosmos-db"></a>Azure CLI-Beispiele für Azure Cosmos DB

Die folgende Tabelle enthält Links zu Azure CLI-Beispielskripts für Azure Cosmos DB. Referenzseiten für alle CLI-Befehle für Azure Cosmos DB sind in der [Referenz zur Azure CLI](/cli/azure/cosmosdb) verfügbar.

| |  |
|---|---|
|**Erstellen von Konten, Datenbanken und Containern für Azure Cosmos DB**||
| [Erstellen eines Azure Cosmos DB-Kontos mit der SQL-API](scripts/create-database-account-collections-cli.md?toc=%2fcli%2fazure%2ftoc.json)| Erstellen eines einzelnen Kontos, einer Datenbank und eines Containers für Azure Cosmos DB. |
| [Erstellen eines Azure Cosmos DB-Kontos mit der API für MongoDB von Cosmos DB](scripts/create-mongodb-database-account-cli.md?toc=%2fcli%2fazure%2ftoc.json) | Erstellen eines einzelnen Azure Cosmos DB-Kontos, einer Datenbank und einer Sammlung. |
| [Erstellen eines Azure Cosmos DB-Kontos mit der Gremlin-API](scripts/create-gremlin-database-account-cli.md?toc=%2fcli%2fazure%2ftoc.json) | Erstellen eines einzelnen Kontos, einer Datenbank und eines Graph für Azure Cosmos DB. |
| [Erstellen eines Azure Cosmos DB-Kontos mit der Cassandra-API](scripts/create-cassandra-database-account-cli.md?toc=%2fcli%2fazure%2ftoc.json) | Erstellen eines einzelnen Kontos und einer Datenbank für Azure Cosmos DB. |
| [Erstellen eines Azure Cosmos DB-Kontos mit der Tabellen-API](scripts/create-table-database-account-cli.md?toc=%2fcli%2fazure%2ftoc.json) | Erstellen eines einzelnen Kontos, einer Datenbank und einer Tabelle für Azure Cosmos DB. |
|**Skalieren von Azure Cosmos DB**||
| [Skalieren des Containerdurchsatzes](scripts/scale-collection-throughput-cli.md?toc=%2fcli%2fazure%2ftoc.json) | Ändern des in einem Container bereitgestellten Durchsatzes|
| [Replizieren des Datenbankkontos von Azure Cosmos in mehreren Regionen und Konfigurieren der Failoverprioritäten](scripts/scale-multiregion-cli.md?toc=%2fcli%2fazure%2ftoc.json)|Globales Replizieren von Kontodaten in verschiedenen Regionen mit einer festgelegten Failoverpriorität|
|**Schützen von Azure Cosmos DB**||
| [Abrufen von Kontoschlüsseln](scripts/secure-get-account-key-cli.md?toc=%2fcli%2fazure%2ftoc.json) | Abrufen der primären und sekundären Hauptschlüssel zum Schreiben und der sekundären schreibgeschützten Schlüssel für das Konto|
| [Abrufen der Verbindungszeichenfolge für das Cosmos-Konto, das mit der API für MongoDB von Azure Cosmos DB konfiguriert ist](scripts/secure-mongo-connection-string-cli.md?toc=%2fcli%2fazure%2ftoc.json) | Abrufen der Verbindungszeichenfolge zum Herstellen einer Verbindung zwischen der MongoDB-App und Ihrem Azure Cosmos DB-Konto.|
| [Erneutes Generieren von Kontoschlüsseln](scripts/secure-regenerate-key-cli.md?toc=%2fcli%2fazure%2ftoc.json)|Erneutes Generieren der Schlüssel für das Konto|
| [Erstellen einer Firewall](scripts/create-firewall-cli.md?toc=%2fcli%2fazure%2ftoc.json)| Erstellen einer Zugriffssteuerungsrichtlinie für eingehende IPs zur Beschränkung des Zugriffs auf das Konto über eine genehmigte Gruppe von Computern und/oder Clouddiensten|
|**Hochverfügbarkeit, Notfallwiederherstellung, Sicherung und Wiederherstellung**||
| [Konfigurieren der Failoverrichtlinie](scripts/ha-failover-policy-cli.md?toc=%2fcli%2fazure%2ftoc.json)|Festlegen der Failoverpriorität einzelner Regionen, in denen das Konto repliziert wird|
|**Herstellen einer Verbindung zwischen Azure Cosmos DB und Ressourcen**||
| [Herstellen einer Verbindung zwischen einer Web-App und Azure Cosmos DB](../app-service/scripts/cli-connect-to-documentdb.md?toc=%2fcli%2fazure%2ftoc.json)|Erstellen und Herstellen einer Verbindung zwischen einer Azure Cosmos-Datenbank und einer Azure-Web-App|
|||
