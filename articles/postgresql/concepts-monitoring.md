---
title: Überwachung und Optimierung in Azure Database for PostgreSQL – Einzelserver
description: In diesem Artikel werden die Überwachungs- und Optimierungsfunktionen in Azure Database for PostgreSQL (Einzelserver) beschrieben.
author: rachel-msft
ms.author: raagyema
ms.service: postgresql
ms.topic: conceptual
ms.date: 06/19/2019
ms.openlocfilehash: c69ffb30a37de8e6dc3e15aa1f7dcd6a9311d614
ms.sourcegitcommit: a52d48238d00161be5d1ed5d04132db4de43e076
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 06/20/2019
ms.locfileid: "67274301"
---
# <a name="monitor-and-tune-azure-database-for-postgresql---single-server"></a>Überwachung und Optimierung in Azure Database for PostgreSQL – Einzelserver
Die Überwachung der Daten zu Ihren Servern unterstützt Sie bei der Problembehandlung und der Optimierung Ihrer Workloads. Azure Database for PostgreSQL bietet verschiedene Überwachungsoptionen, um Einblicke in das Verhalten Ihres Servers zu gewähren.

## <a name="metrics"></a>metrics
Azure Database for PostgreSQL bietet verschiedene Metriken, die Einblicke in das Verhalten der Ressourcen gewähren, die dem PostgreSQL-Server zugrunde liegen. Jede Metrik wird mit einer Frequenz von einer Minute ausgegeben und verfügt über einen Verlauf von bis zu 30 Tagen. Sie können Warnungen für die Metriken konfigurieren. Eine Schritt-für-Schritt-Anleitung finden Sie unter [Use the Azure portal to set up alerts on metrics for Azure Database for PostgreSQL](howto-alert-on-metric.md) (Verwenden des Azure-Portals zum Einrichten von Warnungen zu Metriken für Azure Database for PostgreSQL). Darüber hinaus können weitere Aufgaben wie das Einrichten automatisierter Aktionen, das Durchführen erweiterter Analysen und das Archivieren des Verlaufs ausgeführt werden. Weitere Informationen finden Sie unter [Überblick über Metriken in Microsoft Azure](../monitoring-and-diagnostics/monitoring-overview-metrics.md).

### <a name="list-of-metrics"></a>Liste der Metriken
Die folgenden Metriken sind für Azure Database for PostgreSQL verfügbar:

|Metrik|Metrikanzeigename|Unit|BESCHREIBUNG|
|---|---|---|---|
|cpu_percent|CPU in Prozent|Percent|Die CPU-Auslastung in Prozent|
|memory_percent|Arbeitsspeicher in Prozent|Percent|Die Arbeitsspeicherauslastung in Prozent|
|io_consumption_percent|E/A in Prozent|Percent|Die E/A-Auslastung in Prozent|
|storage_percent|Speicher in Prozent|Percent|Der verwendete Speicher relativ zum Maximalwert des Servers (in Prozent)|
|storage_used|Verwendeter Speicher|Byte|Die Menge des verwendeten Speichers. Der vom Dienst verwendete Speicher kann die Datenbankdateien, Transaktionsprotokolle und Serverprotokolle umfassen.|
|storage_limit|Speicherbegrenzung|Byte|Der maximale Speicher für diesen Server|
|serverlog_storage_percent|Serverprotokollspeicher in Prozent|Percent|Der Prozentsatz des Serverprotokollspeichers, der aus dem maximalen Serverprotokollspeicher des Servers verwendet wird.|
|serverlog_storage_usage|Verwendeter Serverprotokollspeicher|Byte|Die Menge des verwendeten Serverprotokollspeichers.|
|serverlog_storage_limit|Begrenzung des Serverprotokollspeichers|Byte|Der maximale Serverprotokollspeicher für diesen Server.|
|active_connections|Die aktiven Verbindungen.|Count|Die Anzahl aktiver Verbindungen mit dem Server|
|connections_failed|Verbindungsfehler|Count|Die Anzahl von Verbindungsfehlern für den Server|
|network_bytes_egress|Netzwerk ausgehend|Byte|Ausgehender Netzwerkdatenverkehr über aktive Verbindungen.|
|network_bytes_ingress|Netzwerk eingehend|Byte|Eingehender Netzwerkdatenverkehr über aktive Verbindungen.|
|backup_storage_used|Verwendeter Sicherungsspeicher|Byte|Die Menge des verwendeten Sicherungsspeichers.|
|pg_replica_log_delay_in_bytes|Maximale Verzögerung zwischen Replikaten|Byte|Die Verzögerung in Bytes zwischen dem Masterserver und dem Replikat mit der größten Verzögerung. Diese Metrik steht nur auf dem Masterserver zur Verfügung.|
|pg_replica_log_delay_in_seconds|Replikatverzögerung|Sekunden|Die Zeit seit der letzten wiedergegebenen Transaktion. Diese Metrik steht nur für Replikatserver zur Verfügung.|

## <a name="server-logs"></a>Serverprotokolle
Sie können die Protokollierung auf Ihrem Server aktivieren. Diese Protokolle sind ebenfalls durch Azure-Diagnoseprotokolle in [Azure Monitor-Protokolle](../azure-monitor/log-query/log-query-overview.md), Event Hubs und im Speicherkonto verfügbar. Weitere Informationen zur Protokollierung finden Sie auf der Seite [Serverprotokolle](concepts-server-logs.md).

## <a name="query-store"></a>Abfragespeicher
Der [Abfragespeicher](concepts-query-store.md) dient dazu, die Abfrageleistung im Zeitablauf zu verfolgen, einschließlich Statistiken zur Abfrageausführungszeit und Warteereignissen. Das Feature speichert Informationen zur Leistung der Abfrageausführungszeit in einer Systemdatenbank namens **azure_sys** unter dem Schema „query_store“ persistent. Sie können die Sammlung und Speicherung von Daten über verschiedene Konfigurationsoptionen steuern.

## <a name="query-performance-insight"></a>Query Performance Insight
[Query Performance Insight](concepts-query-performance-insight.md) arbeitet mit dem Abfragespeicher zusammen, um Visualisierungen bereitzustellen, auf die über das Azure-Portal zugegriffen werden kann. Diese Diagramme ermöglichen es Ihnen, wichtige Abfragen zu identifizieren, die sich auf die Leistung auswirken. „Query Performance Insight“ ist im Abschnitt **Support und Problembehandlung** auf der Portalseite Ihres Azure Database for PostgreSQL-Servers verfügbar.

## <a name="performance-recommendations"></a>Leistungsempfehlungen
Das Feature [Leistungsempfehlungen](concepts-performance-recommendations.md) identifiziert Möglichkeiten zur Verbesserung der Workloadleistung. Unter „Leistungsempfehlungen“ erhalten Sie Empfehlungen zum Erstellen neuer Indizes, mit denen sich die Leistung Ihrer Workloads u. U. verbessern lässt. Um Indexempfehlungen zu generieren, berücksichtigt das Feature verschiedene Datenbankmerkmale einschließlich des Schemas und der Workload laut Abfragespeicher. Nach der Implementierung von Leistungsempfehlungen sollten Kunden die Leistung testen, um die Auswirkungen dieser Änderungen auszuwerten. 

## <a name="next-steps"></a>Nächste Schritte
- Anleitungen zum Erstellen einer Warnung zu einer Metrik finden Sie unter [Einrichten von Warnungen](howto-alert-on-metric.md).
- Weitere Informationen dazu, wie Sie mit dem Azure-Portal, der REST-API oder der CLI auf Metriken zugreifen bzw. diese exportieren, finden Sie unter [Überblick über Metriken in Microsoft Azure](../monitoring-and-diagnostics/monitoring-overview-metrics.md).
- Lesen Sie unseren Blog zu [Best Practices für die Überwachung Ihres Servers](https://azure.microsoft.com/blog/best-practices-for-alerting-on-metrics-with-azure-database-for-postgresql-monitoring/) (in englischer Sprache).
