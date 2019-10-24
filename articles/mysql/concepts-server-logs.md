---
title: Serverprotokolle für Azure Database for MySQL
description: Beschreibt die Protokolle für langsame Abfragen, die in Azure Database for MySQL verfügbar sind, sowie die verfügbaren Parameter zum Aktivieren verschiedener Protokolliergrade.
author: ajlam
ms.author: andrela
ms.service: mysql
ms.topic: conceptual
ms.date: 05/29/2019
ms.openlocfilehash: 90f3e80c92cd4409a77d4661462ae027c535eaf7
ms.sourcegitcommit: 77bfc067c8cdc856f0ee4bfde9f84437c73a6141
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 10/16/2019
ms.locfileid: "72434285"
---
# <a name="slow-query-logs-in-azure-database-for-mysql"></a>Protokolle für langsame Abfragen in Azure Database for MySQL
In Azure Database for MySQL ist das Protokoll für langsame Abfragen für Benutzer verfügbar. Der Zugriff auf das Transaktionsprotokoll wird jedoch nicht unterstützt. Das Protokoll für langsame Abfragen kann verwendet werden, um Leistungsengpässe für die Problembehandlung zu erkennen.

Weitere Informationen zum MySQL-Protokoll für langsame Abfragen finden Sie im [Abschnitt zu langsamen Abfragen](https://dev.mysql.com/doc/refman/5.7/en/slow-query-log.html) im MySQL-Referenzhandbuch.

## <a name="access-slow-query-logs"></a>Zugreifen auf Protokolle für langsame Abfragen
Sie können Protokolle für langsame Abfragen von Azure Database for MySQL über das Azure-Portal und die Azure CLI auflisten und herunterladen.

Wählen Sie im Azure-Portal Ihren Azure Database for MySQL-Server aus. Wählen Sie unter der Überschrift **Überwachung** die Seite **Serverprotokolle** aus.

Weitere Informationen zur Azure CLI finden Sie unter [Konfigurieren der und Zugreifen auf die Serverprotokolle mithilfe der Azure CLI](howto-configure-server-logs-in-cli.md).

## <a name="log-retention"></a>Protokollaufbewahrung
Protokolle sind für bis zu sieben Tage nach ihrer Erstellung verfügbar. Wenn die Gesamtgröße der verfügbaren Protokolle 7 GB überschreitet, werden die ältesten Dateien gelöscht, bis Speicherplatz verfügbar ist. 

Die Protokolle werden alle 24 Stunden oder bei Erreichen einer Größe von 7 GB rotiert, je nachdem, welches Ereignis früher eintritt.

## <a name="configure-slow-query-logging"></a>Konfigurieren der Protokollierung für langsame Abfragen 
Das Protokoll für langsame Abfragen ist standardmäßig deaktiviert. Legen Sie „slow_query_log“ auf „ON“ fest, um dieses Feature zu aktivieren.

Weitere Parameter, die Sie anpassen können:

- **long_query_time**: Die Abfrage wird protokolliert, wenn sie länger als „long_query_time“ (in Sekunden) dauert. Der Standardwert ist 10 Sekunden.
- **log_slow_admin_statements**: Wenn „ON“, sind administrative Anweisungen wie ALTER_TABLE und ANALYZE_TABLE in den Anweisungen enthalten, die in „slow_query_log“ geschrieben werden.
- **log_queries_not_using_indexes**: Bestimmt, ob Abfragen, die keine Indizes verwenden, in „slow_query_log“ protokolliert werden.
- **log_throttle_queries_not_using_indexes**: Dieser Parameter begrenzt die Anzahl der Nichtindexabfragen, die in das Protokoll für langsame Abfragen geschrieben werden können. Dieser Parameter wird wirksam, wenn „log_queries_not_using_indexes“ auf „ON“ festgelegt ist.

> [!Note]
> Bei `sql_text` wird das Protokoll abgeschnitten, wenn es länger als 2.048 Zeichen ist.

Vollständige Beschreibungen der Parameter des Protokolls für langsame Abfragen finden Sie in der [MySQL-Dokumentation zum Protokoll für langsame Abfragen](https://dev.mysql.com/doc/refman/5.7/en/slow-query-log.html).

## <a name="diagnostic-logs"></a>Diagnoseprotokolle
Azure Database for MySQL ist in Azure Monitor-Diagnoseprotokolle integriert. Nachdem Sie die Protokolle zu langsamen Abfragen auf Ihrem MySQL-Server aktiviert haben, können Sie diese an Azure Monitor-Protokolle, Event Hubs oder Azure Storage ausgeben. Weitere Informationen zum Aktivieren von Diagnoseprotokollen finden Sie im Gewusst-wie-Abschnitt der [Dokumentation zu Diagnoseprotokollen](../azure-monitor/platform/resource-logs-overview.md).

> [!IMPORTANT]
> Dieses Diagnosefeature für Serverprotokolle steht nur in den [Tarifen](concepts-pricing-tiers.md) „Universell“ und „Arbeitsspeicheroptimiert“ zur Verfügung.

In der folgenden Tabelle wird der Inhalt der einzelnen Protokolle beschrieben. Je nach Ausgabemethode können die enthaltenen Felder und ihre Reihenfolge variieren.

| **Eigenschaft** | **Beschreibung** |
|---|---|
| `TenantId` | Ihre Mandanten-ID |
| `SourceSystem` | `Azure` |
| `TimeGenerated` [UTC] | Zeitstempel für den Aufzeichnungsbeginn des Protokolls in UTC |
| `Type` | Typ des Protokolls Immer `AzureDiagnostics` |
| `SubscriptionId` | GUID für das Abonnement, zu dem der Server gehört |
| `ResourceGroup` | Name der Ressourcengruppe, zu der der Server gehört |
| `ResourceProvider` | Name des Ressourcenanbieters Immer `MICROSOFT.DBFORMYSQL` |
| `ResourceType` | `Servers` |
| `ResourceId` | Ressourcen-URI |
| `Resource` | Name des Servers |
| `Category` | `MySqlSlowLogs` |
| `OperationName` | `LogEvent` |
| `Logical_server_name_s` | Name des Servers |
| `start_time_t` [UTC] | Uhrzeit, zu der die Abfrage begann |
| `query_time_s` | Gesamtzeit (in Sekunden), die die Ausführung der Abfrage gedauert hat |
| `lock_time_s` | Gesamtzeit in Sekunden, während der die Abfrage gesperrt war |
| `user_host_s` | Username |
| `rows_sent_s` | Anzahl gesendeter Zeilen |
| `rows_examined_s` | Anzahl untersuchter Zeilen |
| `last_insert_id_s` | [last_insert_id](https://dev.mysql.com/doc/refman/8.0/en/information-functions.html#function_last-insert-id) |
| `insert_id_s` | Einfüge-ID |
| `sql_text_s` | Vollständige Abfrage |
| `server_id_s` | Die ID des Servers |
| `thread_id_s` | Thread-ID |
| `\_ResourceId` | Ressourcen-URI |

## <a name="next-steps"></a>Nächste Schritte
- [Konfigurieren der und Zugreifen auf die Serverprotokolle mithilfe der Azure CLI](howto-configure-server-logs-in-cli.md).
