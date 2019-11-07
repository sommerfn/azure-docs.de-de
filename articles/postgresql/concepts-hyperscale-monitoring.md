---
title: Überwachung und Optimierung in Azure Database for PostgreSQL – Hyperscale (Citus)
description: In diesem Artikel werden die Überwachungs- und Optimierungsfunktionen in Azure Database for PostgreSQL – Hyperscale (Citus) beschrieben.
author: jonels-msft
ms.author: jonels
ms.service: postgresql
ms.topic: conceptual
ms.date: 11/04/2019
ms.openlocfilehash: 6c9bf7a427c0d3d26c3accd1d0d7098413916993
ms.sourcegitcommit: c22327552d62f88aeaa321189f9b9a631525027c
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 11/04/2019
ms.locfileid: "73510239"
---
# <a name="monitor-and-tune-azure-database-for-postgresql---hyperscale-citus"></a>Überwachung und Optimierung von Azure Database for PostgreSQL – Hyperscale (Citus)

Die Überwachung der Daten zu Ihren Servern unterstützt Sie bei der Problembehandlung und der Optimierung Ihrer Workloads. Hyperscale (Citus) bietet verschiedene Überwachungsoptionen, um Einblicke in das Verhalten von Knoten in einer Servergruppe zu gewähren.

## <a name="metrics"></a>metrics

Hyperscale (Citus) stellt Metriken für jeden Knoten in einer Servergruppe bereit. Die Metriken bieten Einblicke in das Verhalten der unterstützenden Ressourcen. Jede Metrik wird mit einer Frequenz von einer Minute ausgegeben und verfügt über einen Verlauf von bis zu 30 Tagen.

Zusätzlich zum Anzeigen von Diagrammen der Metriken können Sie Warnungen konfigurieren. Eine Schritt-für-Schritt-Anleitung finden Sie unter [Use the Azure portal to set up alerts on metrics for Azure Database for PostgreSQL](howto-hyperscale-alert-on-metric.md) (Verwenden des Azure-Portals zum Einrichten von Warnungen zu Metriken für Azure Database for PostgreSQL).  Darüber hinaus können weitere Aufgaben wie das Einrichten automatisierter Aktionen, das Ausführen erweiterter Analysen und das Archivieren des Verlaufs ausgeführt werden. Weitere Informationen finden Sie unter [Überblick über Metriken in Microsoft Azure](../monitoring-and-diagnostics/monitoring-overview-metrics.md).

### <a name="list-of-metrics"></a>Liste der Metriken

Diese Metriken sind für Hyperscale-Knoten (Citus) verfügbar:

|Metrik|Metrikanzeigename|Unit|BESCHREIBUNG|
|---|---|---|---|
|active_connections|Die aktiven Verbindungen.|Count|Die Anzahl aktiver Verbindungen mit dem Server|
|cpu_percent|CPU in Prozent|Percent|Die CPU-Auslastung in Prozent|
|iops|IOPS|Count|Weitere Informationen finden Sie unter [IOPS-Definition](../virtual-machines/linux/premium-storage-performance.md#iops) und [Hyperscale-Durchsatz](concepts-hyperscale-configuration-options.md).|
|memory_percent|Arbeitsspeicher in Prozent|Percent|Die Arbeitsspeicherauslastung in Prozent|
|network_bytes_ingress|Netzwerk eingehend|Byte|Eingehender Netzwerkdatenverkehr über aktive Verbindungen.|
|network_bytes_egress|Netzwerk ausgehend|Byte|Ausgehender Netzwerkdatenverkehr über aktive Verbindungen.|
|storage_percent|Speicher in Prozent|Percent|Der verwendete Speicher relativ zum Maximalwert des Servers (in Prozent)|
|storage_used|Verwendeter Speicher|Byte|Die Menge des verwendeten Speichers. Der vom Dienst verwendete Speicher kann die Datenbankdateien, Transaktionsprotokolle und Serverprotokolle umfassen.|

Azure liefert keine aggregierten Metriken für den Cluster als Ganzes, aber Metriken für mehrere Knoten können im selben Diagramm dargestellt werden.

## <a name="next-steps"></a>Nächste Schritte

- Anleitungen zum Erstellen einer Warnung zu einer Metrik finden Sie unter [Einrichten von Warnungen](howto-hyperscale-alert-on-metric.md).
