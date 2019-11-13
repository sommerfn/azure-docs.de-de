---
title: Überwachung in Azure Database for MariaDB
description: In diesem Artikel werden die Überwachungs- und Warnmetriken (CPU, Speicher, Verbindungsstatistiken und Ähnliches) für Azure Database for MariaDB beschrieben.
author: ajlam
ms.author: andrela
ms.service: mariadb
ms.topic: conceptual
ms.date: 11/04/2019
ms.openlocfilehash: 2f4346dfdb095e849adc65baf0fd31d25d03c4a7
ms.sourcegitcommit: c62a68ed80289d0daada860b837c31625b0fa0f0
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 11/05/2019
ms.locfileid: "73604067"
---
# <a name="monitoring-in-azure-database-for-mariadb"></a>Überwachung in Azure Database for MariaDB
Die Überwachung der Daten zu Ihren Servern unterstützt Sie bei der Problembehandlung und der Optimierung Ihrer Workloads. Azure Database for MariaDB bietet verschiedene Metriken, die Einblicke in das Verhalten Ihres Servers ermöglichen.

## <a name="metrics"></a>metrics
Alle Azure-Metriken werden im Minutentakt erfasst, und für jede Metrik steht ein Verlauf von 30 Tagen zur Verfügung. Sie können Warnungen für die Metriken konfigurieren. Darüber hinaus können weitere Aufgaben wie das Einrichten automatisierter Aktionen, das Durchführen erweiterter Analysen und das Archivieren des Verlaufs ausgeführt werden. Weitere Informationen finden Sie unter [Überblick über Metriken in Microsoft Azure](../monitoring-and-diagnostics/monitoring-overview-metrics.md).

Eine Schritt-für-Schritt-Anleitung finden Sie unter [Use the Azure portal to set up alerts on metrics for Azure Database for PostgreSQL](howto-alert-metric.md) (Verwenden des Azure-Portals zum Einrichten von Warnungen zu Metriken für Azure Database for PostgreSQL).

### <a name="list-of-metrics"></a>Liste der Metriken
Für Azure Database for MariaDB sind folgende Metriken verfügbar:

|Metrik|Metrikanzeigename|Unit|BESCHREIBUNG|
|---|---|---|---|
|cpu_percent|CPU in Prozent|Percent|Die CPU-Auslastung in Prozent|
|memory_percent|Arbeitsspeicher in Prozent|Percent|Die Arbeitsspeicherauslastung in Prozent|
|io_consumption_percent|E/A in Prozent|Percent|Die E/A-Auslastung in Prozent|
|storage_percent|Speicher in Prozent|Percent|Der verwendete Speicher relativ zum Maximalwert des Servers (in Prozent)|
|storage_used|Verwendeter Speicher|Byte|Die Menge des verwendeten Speichers. Der vom Dienst verwendete Speicher kann die Datenbankdateien, Transaktionsprotokolle und Serverprotokolle umfassen.|
|serverlog_storage_percent|Serverprotokollspeicher in Prozent|Percent|Der Prozentsatz des Serverprotokollspeichers, der aus dem maximalen Serverprotokollspeicher des Servers verwendet wird.|
|serverlog_storage_usage|Verwendeter Serverprotokollspeicher|Byte|Die Menge des verwendeten Serverprotokollspeichers.|
|serverlog_storage_limit|Begrenzung des Serverprotokollspeichers|Byte|Der maximale Serverprotokollspeicher für diesen Server.|
|storage_limit|Speicherbegrenzung|Byte|Der maximale Speicher für diesen Server|
|active_connections|Die aktiven Verbindungen.|Count|Die Anzahl aktiver Verbindungen mit dem Server|
|connections_failed|Verbindungsfehler|Count|Die Anzahl von Verbindungsfehlern für den Server|
|network_bytes_egress|Netzwerk ausgehend|Byte|Ausgehender Netzwerkdatenverkehr über aktive Verbindungen.|
|network_bytes_ingress|Netzwerk eingehend|Byte|Eingehender Netzwerkdatenverkehr über aktive Verbindungen.|

## <a name="server-logs"></a>Serverprotokolle

Sie können die Protokollierung von langsamen Abfragen auf Ihrem Server aktivieren. Diese Protokolle sind ebenfalls durch Azure-Diagnoseprotokolle in Azure Monitor-Protokolle, Event Hubs und im Speicherkonto verfügbar. Weitere Informationen zur Protokollierung finden Sie auf der Seite  [Serverprotokolle](concepts-server-logs.md).

## <a name="query-store"></a>Abfragespeicher

Der [Abfragespeicher](concepts-query-store.md) dient dazu, die Abfrageleistung im Zeitablauf zu verfolgen, einschließlich Statistiken zur Abfrageausführungszeit und Warteereignissen. Das Feature speichert Informationen zur Laufzeitleistung der Abfrage im **mysql**-Schema. Sie können die Sammlung und Speicherung von Daten über verschiedene Konfigurationsoptionen steuern.

## <a name="query-performance-insight"></a>Query Performance Insight

[Query Performance Insight](concepts-query-performance-insight.md) arbeitet mit dem Abfragespeicher zusammen, um Visualisierungen bereitzustellen, auf die über das Azure-Portal zugegriffen werden kann. Diese Diagramme ermöglichen es Ihnen, wichtige Abfragen zu identifizieren, die sich auf die Leistung auswirken. Query Performance Insight ist im Abschnitt **Intelligente Leistung** auf der Portalseite Ihres Azure Database for MariaDB-Servers verfügbar.

## <a name="performance-recommendations"></a>Leistungsempfehlungen

Das Feature [Leistungsempfehlungen](concepts-performance-recommendations.md) identifiziert Möglichkeiten zur Verbesserung der Workloadleistung. Unter „Leistungsempfehlungen“ erhalten Sie Empfehlungen zum Erstellen neuer Indizes, mit denen sich die Leistung Ihrer Workloads u. U. verbessern lässt. Um Indexempfehlungen zu generieren, berücksichtigt das Feature verschiedene Datenbankmerkmale einschließlich des Schemas und der Workload laut Abfragespeicher. Nach der Implementierung von Leistungsempfehlungen sollten Kunden die Leistung testen, um die Auswirkungen dieser Änderungen auszuwerten.

## <a name="next-steps"></a>Nächste Schritte

- Weitere Informationen dazu, wie Sie mit dem Azure-Portal, der REST-API oder der CLI auf Metriken zugreifen bzw. diese exportieren, finden Sie unter [Überblick über Metriken in Microsoft Azure](../monitoring-and-diagnostics/monitoring-overview-metrics.md).
  - Anleitungen zum Erstellen einer Warnung zu einer Metrik finden Sie unter [Einrichten von Warnungen](howto-alert-metric.md).
