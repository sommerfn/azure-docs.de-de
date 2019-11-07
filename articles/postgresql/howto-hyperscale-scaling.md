---
title: Skalieren einer Azure Database for PostgreSQL – Hyperscale (Citus)-Servergruppe
description: Anpassen von Servergruppenspeicher-, Datenträger- und CPU-Ressourcen, um höhere Auslastungen zu bewältigen
author: jonels-msft
ms.author: jonels
ms.service: postgresql
ms.topic: conceptual
ms.date: 9/17/2019
ms.openlocfilehash: 6053ba37bf330f6b59e291dade822a5ca9de8c85
ms.sourcegitcommit: c22327552d62f88aeaa321189f9b9a631525027c
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 11/04/2019
ms.locfileid: "73492322"
---
# <a name="scale-a-hyperscale-citus-server-group"></a>Skalieren einer Hyperscale (Citus)-Servergruppe

Azure Database for PostgreSQL – Hyperscale (Citus) unterstützt Self-Service-Skalierung, um höhere Auslastungen zu bewältigen. Das Azure-Portal vereinfacht das Hinzufügen neuer Workerknoten und das Erhöhen der Kapazität vorhandener Knoten.

## <a name="add-worker-nodes"></a>Hinzufügen von Workerknoten

Navigieren Sie zum Hinzufügen von Knoten in der Servergruppe „Hyperscale (Citus)“ zur Registerkarte **Konfigurieren**.  Ziehen Sie den Schieberegler für **Anzahl von Workerknoten**, um den Wert zu ändern.

![Schieberegler für Ressourcen](./media/howto-hyperscale-scaling/01-sliders-workers.png)

Klicken Sie auf die Schaltfläche **Speichern**, damit der geänderte Wert wirksam wird.

> [!NOTE]
> Nachdem Sie die Anzahl der Workerknoten erhöht und gespeichert haben, kann sie mit dem Schieberegler nicht mehr verringert werden.

### <a name="rebalance-shards"></a>Ausgleichen von Shards

Um neu hinzugefügte Knoten optimal zu nutzen, müssen Sie die verteilte [Shard](concepts-hyperscale-distributed-data.md#shards)tabelle ausgleichen. Das bedeutet, dass einige Shards von vorhandenen Knoten auf neue Knoten verschoben werden. Vergewissern Sie sich zunächst, dass die Bereitstellung der neuen Worker erfolgreich abgeschlossen wurde. Starten Sie dann den Shardausgleich, indem Sie über „psql“ eine Verbindung mit dem Clusterkoordinatorknoten herstellen und Folgendes ausführen:

```sql
SELECT rebalance_table_shards('distributed_table_name');
```

Die `rebalance_table_shards`-Funktion gleicht alle Tabellen in der [Zusammenstellungs](concepts-hyperscale-colocation.md)gruppe der im Argument genannten Tabelle aus. Daher müssen Sie die Funktion nicht für jede verteilte Tabelle aufrufen, sondern einfach nur für eine repräsentative Tabelle aus jeder Zusammenstellungsgruppe.

## <a name="increase-vcores-or-storage-space"></a>Vergrößern von V-Kernen oder Speicherplatz

Zusätzlich zum Hinzufügen neuer Knoten können Sie die Funktionen vorhandener Knoten erweitern. Navigieren Sie zur Registerkarte **Konfigurieren** in Ihrer Hyperscale-Servergruppe (Citus), und ziehen Sie den Schieberegler für **V-Kerne** und **Speicher**, um diese Werte für alle Workerknoten zu ändern. Klicken Sie unbedingt auf **Speichern**, um die Änderungen zu übernehmen.

## <a name="next-steps"></a>Nächste Schritte

Erfahren Sie mehr über [Leistungsoptionen](concepts-hyperscale-configuration-options.md) für Servergruppen.
