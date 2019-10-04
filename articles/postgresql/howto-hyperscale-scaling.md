---
title: Skalieren einer Azure Database for PostgreSQL – Hyperscale (Citus)-Servergruppe
description: Anpassen von Servergruppenspeicher-, Datenträger- und CPU-Ressourcen, um höhere Auslastungen zu bewältigen
author: jonels-msft
ms.author: jonels
ms.service: postgresql
ms.topic: conceptual
ms.date: 9/17/2019
ms.openlocfilehash: a69b4988a5ee835381de986edaa5899b09aa9e4e
ms.sourcegitcommit: 55f7fc8fe5f6d874d5e886cb014e2070f49f3b94
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 09/25/2019
ms.locfileid: "71263017"
---
# <a name="scale-a-hyperscale-citus-server-group"></a>Skalieren einer Hyperscale (Citus)-Servergruppe

Azure Database for PostgreSQL – Hyperscale (Citus) unterstützt Self-Service-Skalierung, um höhere Auslastungen zu bewältigen. Neue Workerknoten lassen sich im Azure-Portal ganz einfach hinzufügen.

Wechseln Sie dazu in der Hyperscale (Citus)-Servergruppe zur Registerkarte **Konfigurieren**.
Ziehen Sie den Schieberegler für **Anzahl von Workerknoten**, um den Wert zu ändern.

![Schieberegler für Ressourcen](./media/howto-hyperscale-scaling/01-sliders-workers.png)

Klicken Sie auf die Schaltfläche „Speichern“, damit der geänderte Wert wirksam wird.

> [!NOTE]
> Nachdem Sie die Anzahl der Workerknoten erhöht und gespeichert haben, kann sie mit dem Schieberegler nicht mehr verringert werden.
>
> Diese Benutzeroberfläche bietet auch noch keine Möglichkeit, virtuelle Kerne und Speicher für den Koordinator oder für Worker anzupassen. Öffnen Sie ein Supportticket, wenn Computeressourcen für Koordinator- oder Workerknoten skaliert werden müssen.

Um neu hinzugefügte Knoten optimal zu nutzen, müssen Sie die verteilte [Shard](concepts-hyperscale-distributed-data.md#shards)tabelle ausgleichen. Das bedeutet, dass einige Shards von vorhandenen Knoten auf neue Knoten verschoben werden. Um den Shardausgleich zu starten, stellen Sie über „psql“ eine Verbindung mit dem Clusterkoordinatorknoten her und führen folgenden Befehl aus:

```sql
SELECT rebalance_table_shards('distributed_table_name');
```

Die `rebalance_table_shards`-Funktion gleicht alle Tabellen in der [Zusammenstellungs](concepts-hyperscale-colocation.md)gruppe der im Argument genannten Tabelle aus. Daher müssen Sie die Funktion nicht für jede verteilte Tabelle aufrufen, sondern einfach nur für eine repräsentative Tabelle aus jeder Zusammenstellungsgruppe.

## <a name="next-steps"></a>Nächste Schritte

Erfahren Sie mehr über [Leistungsoptionen](concepts-hyperscale-configuration-options.md) für Servergruppen.
