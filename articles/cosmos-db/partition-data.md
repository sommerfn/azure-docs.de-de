---
title: Partitionierung und horizontale Skalierung in Azure Cosmos DB
description: Erfahren Sie, wie die Partitionierung in Azure Cosmos DB funktioniert, wie Partitionierung und Partitionsschlüssel konfiguriert werden und wie Sie den richtigen Partitionsschlüssel für Ihre Anwendung wählen.
author: markjbrown
ms.author: mjbrown
ms.service: cosmos-db
ms.topic: conceptual
ms.date: 08/01/2019
ms.openlocfilehash: cbd171e10cc1a8b27de98d9d4d779f345ac5a3ed
ms.sourcegitcommit: 8074f482fcd1f61442b3b8101f153adb52cf35c9
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 10/22/2019
ms.locfileid: "72754906"
---
# <a name="partitioning-and-horizontal-scaling-in-azure-cosmos-db"></a>Partitionierung und horizontale Skalierung in Azure Cosmos DB

In diesem Artikel werden physische und logische Partitionen in Azure Cosmos DB erläutert. Außerdem werden bewährte Methoden für Skalierung und Partitionierung behandelt. 

## <a name="logical-partitions"></a>Logische Partitionen

Eine logische Partition besteht aus einer Gruppe von Elementen mit demselben Partitionsschlüssel. In einen Container, in dem beispielsweise alle Elemente die Eigenschaft `City` aufweisen, können Sie `City` als Partitionsschlüssel für den Container verwenden. Elementgruppen mit bestimmten Werten für `City`, z.B. `London`, `Paris` und `NYC`, bilden eindeutige logische Partitionen. Sie müssen sich keine Sorgen um das Löschen einer Partition machen, wenn die zugrunde liegenden Daten gelöscht werden.

In Azure Cosmos DB ist ein Container die grundlegende Einheit für die Skalierbarkeit. Daten, die dem Container hinzugefügt werden, und der Durchsatz, den Sie für den Container bereitstellen, werden automatisch (horizontal) auf eine Gruppe logischer Partitionen verteilt. Daten und Durchsatz werden basierend auf dem Partitionsschlüssel partitioniert, den Sie für den Azure Cosmos-Container angeben. Weitere Informationen finden Sie unter [Erstellen eines Azure Cosmos-Containers](how-to-create-container.md).

Eine logische Partition definiert auch den Bereich für Datenbanktransaktionen. Sie können Elemente in einer logischen Partition mithilfe einer [Transaktion mit Momentaufnahmeisolation](database-transactions-optimistic-concurrency.md) aktualisieren. Wenn einem Container neue Elemente hinzugefügt werden, werden neue logische Partitionen transparent vom System erstellt.

## <a name="physical-partitions"></a>Physische Partitionen

Ein Azure Cosmos-Container wird skaliert, indem Daten und Durchsatz auf eine große Anzahl logischer Partitionen verteilt werden. Intern werden eine oder mehrere logische Partitionen einer physischen Partition zugeordnet, die aus einer auch als [*Replikatgruppe*](global-dist-under-the-hood.md) bezeichneten Gruppe von Replikaten besteht. Jede Replikatgruppe hostet eine Instanz der Azure Cosmos-Datenbank-Engine. Eine Replikatgruppe macht die in der physischen Partition gespeicherten Daten dauerhaft, hochverfügbar und konsistent. Eine physische Partition unterstützt eine maximale Menge an Speicher und Anforderungseinheiten (Request Units, RUs). Jedes Replikat, aus dem die physische Partition besteht, erbt das Speicherkontingent der Partition. Alle Replikate einer physischen Partition unterstützen gemeinsam den Durchsatz, der der physischen Partition zugeordnet wurde. 

Die folgende Abbildung zeigt, wie logische Partitionen physischen Partitionen zugeordnet werden, die global verteilt sind:

![Abbildung zur Veranschaulichung der Azure Cosmos DB-Partitionierung](./media/partition-data/logical-partitions.png)

Der für einen Container bereitgestellte Durchsatz wird gleichmäßig auf physische Partitionen aufgeteilt. Ein Partitionsschlüsselentwurf, der die Durchsatzanforderungen nicht gleichmäßig verteilt, kann „heiße“ Partitionen verursachen. „Heiße“ Partitionen können zu einer Ratenbegrenzung und einer ineffizienten Nutzung des bereitgestellten Durchsatzes sowie zu höheren Kosten führen.

Im Gegensatz zu logischen Partitionen sind physische Partitionen eine interne Implementierung des Systems. Sie können Größe, Platzierung oder Anzahl physischer Partitionen nicht steuern. Auch die Zuordnung zwischen logischen Partitionen und physischen Partitionen lässt sich nicht steuern. Sie können allerdings die Anzahl der logischen Partitionen und die Verteilung der Daten, der Workload und des Durchsatzes steuern, indem Sie den [richtigen logischen Partitionsschlüssel auswählen](partitioning-overview.md#choose-partitionkey).

## <a name="next-steps"></a>Nächste Schritte

* Erfahren Sie mehr über das [Wählen eines Partitionsschlüssels](partitioning-overview.md#choose-partitionkey).
* Erfahren Sie mehr über den [bereitgestellten Durchsatz in Azure Cosmos DB](request-units.md).
* Erfahren Sie mehr über die [globale Verteilung in Azure Cosmos DB](distribute-data-globally.md).
* Erfahren Sie mehr über das [Bereitstellen von Durchsatz für einen Azure Cosmos-Container](how-to-provision-container-throughput.md).
* Erfahren Sie mehr über das [Bereitstellen des Durchsatzes für eine Datenbank in Azure Cosmos](how-to-provision-database-throughput.md).
