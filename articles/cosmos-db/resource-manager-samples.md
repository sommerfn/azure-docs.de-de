---
title: Azure Resource Manager-Vorlagen für Azure Cosmos DB
description: Verwenden Sie Azure Resource Manager-Vorlagen zum Erstellen und Konfigurieren von Azure Cosmos DB.
author: markjbrown
ms.service: cosmos-db
ms.topic: conceptual
ms.date: 10/31/2019
ms.author: mjbrown
ms.openlocfilehash: ecdfe89e899e0d416784ae32a0d66b335c09e2b6
ms.sourcegitcommit: f4d8f4e48c49bd3bc15ee7e5a77bee3164a5ae1b
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 11/04/2019
ms.locfileid: "73582805"
---
# <a name="azure-resource-manager-templates-for-azure-cosmos-db"></a>Azure Resource Manager-Vorlagen für Azure Cosmos DB

Die folgenden Tabellen enthalten Links zu Azure Resource Manager-Vorlagen für Azure Cosmos DB:

## <a name="sql-core-api"></a>SQL-API (Core-API)

|**Vorlage**|**Beschreibung**|
|---|---|
|[Erstellen eines Kontos, einer Datenbank und eines Containers in Azure Cosmos](manage-sql-with-resource-manager.md#create-resource) | Diese Vorlage erstellt ein SQL (Core) API-Konto in zwei Regionen mit zwei Containern mit gemeinsamem Datenbankdurchsatz und einem Container mit dediziertem Durchsatz. Der Durchsatz kann aktualisiert werden, indem die Vorlage mit dem aktualisierten Eigenschaftswert für den Durchsatz erneut übermittelt wird. |
|[Erstellen eines Azure Cosmos-Kontos, einer Datenbank und eines Containers mit einer gespeicherten Prozedur, einem Trigger und einer UDF](manage-sql-with-resource-manager.md#create-sproc) | Diese Vorlage erstellt ein SQL(Core) API-Konto in zwei Regionen mit einer gespeicherten Prozedur, einem Trigger und einer UDF für einen Container. |

## <a name="mongodb-api"></a>MongoDB-API

|**Vorlage**|**Beschreibung**|
|---| ---|
|[Erstellen eines Kontos, einer Datenbank und einer Sammlung in Azure Cosmos](manage-mongodb-with-resource-manager.md#create-resource) | Diese Vorlage erstellt mithilfe der Azure Cosmos DB-API für MongoDB ein Konto in zwei Regionen mit aktivierter Multimasterunterstützung. Das Azure Cosmos-Konto enthält zwei Container, die den Durchsatz auf Datenbankebene gemeinsam nutzen. |
|[Aktualisieren des Durchsatzes (RU/s) für eine Datenbank](manage-mongodb-with-resource-manager.md#database-ru-update) | Mit dieser Vorlage wird der Durchsatz für eine Datenbank in einem MongoDB-API-Konto aktualisiert. |
|[Aktualisieren des Durchsatzes (RU/s) für eine Sammlung](manage-mongodb-with-resource-manager.md#collection-ru-update) | Mit dieser Vorlage wird der Durchsatz für einen Container in einem MongoDB-API-Konto aktualisiert. |

## <a name="cassandra-api"></a>Cassandra-API

|**Vorlage**|**Beschreibung**|
|---| ---|
|[Erstellen eines Kontos, eines Keyspace und einer Tabelle in Azure Cosmos](manage-cassandra-with-resource-manager.md#create-resource) | Diese Vorlage erstellt ein Cassandra-API-Konto in zwei Regionen mit aktivierter Multimasterunterstützung. Das Azure Cosmos-Konto enthält zwei Tabellen, die den Durchsatz auf Schlüsselbereichsebene gemeinsam nutzen. |
|[Aktualisieren des Durchsatzes (RU/s) für einen Keyspace](manage-cassandra-with-resource-manager.md#keyspace-ru-update) | Mit dieser Vorlage wird der Durchsatz für einen Keyspace in einem Cassandra-API-Konto aktualisiert. |
|[Aktualisieren des Durchsatzes (RU/s) für eine Tabelle](manage-cassandra-with-resource-manager.md#table-ru-update) | Mit dieser Vorlage wird der Durchsatz für eine Tabelle in einem Cassandra-API-Konto aktualisiert. |

## <a name="gremlin-api"></a>Gremlin-API

|**Vorlage**|**Beschreibung**|
|---| ---|
|[Erstellen eines Kontos, einer Datenbank und eines Graphen in Azure Cosmos](manage-gremlin-with-resource-manager.md#create-resource) | Diese Vorlage erstellt ein Gremlin-API-Konto in zwei Regionen mit aktivierter Multimasterunterstützung. Das Azure Cosmos-Konto enthält zwei Graphen, die den Durchsatz auf Datenbankebene gemeinsam nutzen. |
|[Aktualisieren des Durchsatzes (RU/s) für eine Datenbank](manage-gremlin-with-resource-manager.md#database-ru-update) | Mit dieser Vorlage wird der Durchsatz für eine Datenbank in einem Gremlin-API-Konto aktualisiert. |
|[Aktualisieren des Durchsatzes (RU/s) für einen Graphen](manage-gremlin-with-resource-manager.md#graph-ru-update) | Mit dieser Vorlage wird der Durchsatz für einen Graphen in einem Gremlin-API-Konto aktualisiert. |

## <a name="table-api"></a>Tabelle-API

|**Vorlage**|**Beschreibung**|
|---| ---|
|[Erstellen eines Kontos und einer Tabelle in Azure Cosmos](manage-table-with-resource-manager.md#create-resource) | Diese Vorlage erstellt ein Tabellen-API-Konto in zwei Regionen mit aktivierter Multimasterunterstützung. Das Azure Cosmos-Konto enthält eine einzelne Tabelle. |
|[Aktualisieren des Durchsatzes (RU/s) für eine Tabelle](manage-table-with-resource-manager.md#table-ru-update) | Mit dieser Vorlage wird der Durchsatz für eine Tabelle in einem Tabellen-API-Konto aktualisiert. |

> [!TIP]
> Um bei Verwendung der Tabellen-API die gemeinsame Nutzung des Durchsatzes zu aktivieren, aktivieren Sie im Azure-Portal den Durchsatz auf Kontoebene.

Auf der Seite [ARM-Referenz für Azure Cosmos DB](/azure/templates/microsoft.documentdb/allversions) finden Sie die Referenzdokumentation.
