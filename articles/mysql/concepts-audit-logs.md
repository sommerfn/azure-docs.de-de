---
title: Überwachungsprotokolle für Azure Database for MySQL
description: Beschreibt die Überwachungsprotokolle, die in Azure Database for MySQL verfügbar sind, sowie die verfügbaren Parameter zum Aktivieren von Protokolliergraden.
author: ajlam
ms.author: andrela
ms.service: mysql
ms.topic: conceptual
ms.date: 06/26/2019
ms.openlocfilehash: 42881fcb12f29ec14bbdc0ec4942b2eef17c7312
ms.sourcegitcommit: 77bfc067c8cdc856f0ee4bfde9f84437c73a6141
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 10/16/2019
ms.locfileid: "72434408"
---
# <a name="audit-logs-in-azure-database-for-mysql"></a>Überwachungsprotokolle in Azure Database for MySQL

In Azure Database for MySQL ist das Überwachungsprotokoll für Benutzer verfügbar. Das Überwachungsprotokoll kann verwendet werden, um Aktivitäten auf Datenbankebene nachzuverfolgen, und es wird häufig für Konformitätszwecke genutzt.

> [!IMPORTANT]
> Diese Überwachungsprotokollfunktion ist derzeit in der Vorschauphase.

## <a name="configure-audit-logging"></a>Konfigurieren der Überwachungsprotokollierung

Das Überwachungsprotokoll ist standardmäßig deaktiviert. Um sie zu aktivieren, legen Sie `audit_log_enabled` auf ON fest.

Weitere Parameter, die Sie anpassen können:

- `audit_log_events`: steuert die zu protokollierenden Ereignisse. In der nachstehenden Tabelle finden Sie spezifische Überwachungsereignisse.
- `audit_log_include_users`: MySQL-Benutzer, die zur Protokollierung einbezogen werden sollen. Der Standardwert für diesen Parameter ist leer, wodurch alle Benutzer zur Protokollierung einbezogen werden. Dies hat eine höhere Priorität als `audit_log_exclude_users`. Die maximale Länge des Parameters ist 512 Zeichen.
> [!Note]
> `audit_log_include_users` hat eine höhere Priorität als `audit_log_exclude_users`. Beispiel: Wenn „audit_log_include_users“ = `demouser` und „audit_log_exclude_users“ = `demouser` lautet, werden die Protokolle überwacht, weil `audit_log_include_users` eine höhere Priorität hat.
- `audit_log_exclude_users`: MySQL-Benutzer, die aus der Protokollierung ausgeschlossen werden sollen. Die maximale Länge des Parameters ist 512 Zeichen.

> [!Note]
> Bei `sql_text` wird das Protokoll abgeschnitten, wenn es länger als 2.048 Zeichen ist.

| **Event** | **Beschreibung** |
|---|---|
| `CONNECTION` | : Initiierung der Verbindung (erfolgreich oder nicht erfolgreich) <br> : Erneute Authentifizierung des Benutzers mit anderem Benutzernamen/Kennwort während der Sitzung <br> : Beendigung der Verbindung |
| `DML_SELECT`| SELECT-Abfragen |
| `DML_NONSELECT` | INSERT/DELETE/UPDATE-Abfragen |
| `DML` | DML = DML_SELECT + DML_NONSELECT |
| `DDL` | Abfragen wie DROP DATABASE |
| `DCL` | Abfragen wie GRANT PERMISSION |
| `ADMIN` | Abfragen wie SHOW STATUS |
| `GENERAL` | Alle in „DML_SELECT“, „DML_NONSELECT“, „DML“, „DDL“, „DCL“ und „ADMIN“ |
| `TABLE_ACCESS` | - Nur für MySQL 5.7 verfügbar <br> - Leseanweisungen für Tabelle, z. B. SELECT oder INSERT INTO... SELECT <br> - Löschanweisungen für Tabelle, z. B. DELETE oder TRUNCATE TABLE <br> - Einfügeanweisungen für Tabelle, z. B. INSERT oder REPLACE <br> - Aktualisierungsanweisungen für Tabelle, z. B. UPDATE |

## <a name="access-audit-logs"></a>Zugreifen auf Überwachungsprotokolle

Überwachungsprotokolle sind in Azure Monitor-Diagnoseprotokolle integriert. Nachdem Sie Überwachungsprotokolle auf Ihrem MySQL-Server aktiviert haben, können Sie sie an Azure Monitor-Protokolle, Event Hubs oder Azure Storage senden. Weitere Informationen zum Aktivieren von Diagnoseprotokollen im Azure-Portal finden Sie im Artikel zum [Portal für Überwachungsprotokolle](howto-configure-audit-logs-portal.md#set-up-diagnostic-logs).

## <a name="diagnostic-logs-schemas"></a>Schemas für Diagnoseprotokolle

In den folgenden Abschnitten wird beschrieben, was von MySQL-Überwachungsprotokollen basierend auf dem Ereignistyp ausgegeben wird. Je nach Ausgabemethode können die enthaltenen Felder und ihre Reihenfolge variieren.

### <a name="connection"></a>Verbindung

| **Eigenschaft** | **Beschreibung** |
|---|---|
| `TenantId` | Ihre Mandanten-ID |
| `SourceSystem` | `Azure` |
| `TimeGenerated [UTC]` | Zeitstempel für den Aufzeichnungsbeginn des Protokolls in UTC |
| `Type` | Typ des Protokolls Immer `AzureDiagnostics` |
| `SubscriptionId` | GUID für das Abonnement, zu dem der Server gehört |
| `ResourceGroup` | Name der Ressourcengruppe, zu der der Server gehört |
| `ResourceProvider` | Name des Ressourcenanbieters Immer `MICROSOFT.DBFORMYSQL` |
| `ResourceType` | `Servers` |
| `ResourceId` | Ressourcen-URI |
| `Resource` | Name des Servers |
| `Category` | `MySqlAuditLogs` |
| `OperationName` | `LogEvent` |
| `LogicalServerName_s` | Name des Servers |
| `event_class_s` | `connection_log` |
| `event_subclass_s` | `CONNECT`, `DISCONNECT`, `CHANGE USER` (nur für MySQL 5.7 verfügbar) |
| `connection_id_d` | Von MySQL generierte eindeutige Verbindungs-ID |
| `host_s` | Leer |
| `ip_s` | IP-Adresse des Clients, der die Verbindung mit MySQL herstellt |
| `user_s` | Name des Benutzers, der die Abfrage ausführt |
| `db_s` | Name der Datenbank, mit der die Verbindung besteht |
| `\_ResourceId` | Ressourcen-URI |

### <a name="general"></a>Allgemein

Das unten angegebene Schema gilt für die Ereignistypen GENERAL, DML_SELECT, DML_NONSELECT, DML, DDL, DCL und ADMIN.

| **Eigenschaft** | **Beschreibung** |
|---|---|
| `TenantId` | Ihre Mandanten-ID |
| `SourceSystem` | `Azure` |
| `TimeGenerated [UTC]` | Zeitstempel für den Aufzeichnungsbeginn des Protokolls in UTC |
| `Type` | Typ des Protokolls Immer `AzureDiagnostics` |
| `SubscriptionId` | GUID für das Abonnement, zu dem der Server gehört |
| `ResourceGroup` | Name der Ressourcengruppe, zu der der Server gehört |
| `ResourceProvider` | Name des Ressourcenanbieters Immer `MICROSOFT.DBFORMYSQL` |
| `ResourceType` | `Servers` |
| `ResourceId` | Ressourcen-URI |
| `Resource` | Name des Servers |
| `Category` | `MySqlAuditLogs` |
| `OperationName` | `LogEvent` |
| `LogicalServerName_s` | Name des Servers |
| `event_class_s` | `general_log` |
| `event_subclass_s` | `LOG`, `ERROR`, `RESULT` (nur für MySQL 5.6 verfügbar) |
| `event_time` | Startzeit der Abfrage im UTC-Zeitstempel |
| `error_code_d` | Fehlercode bei fehlerhafter Abfrage. `0` bedeutet, dass kein Fehler vorliegt |
| `thread_id_d` | ID des Threads, der die Abfrage ausgeführt hat |
| `host_s` | Leer |
| `ip_s` | IP-Adresse des Clients, der die Verbindung mit MySQL herstellt |
| `user_s` | Name des Benutzers, der die Abfrage ausführt |
| `sql_text_s` | Vollständiger Abfragetext |
| `\_ResourceId` | Ressourcen-URI |

### <a name="table-access"></a>Tabellenzugriff

| **Eigenschaft** | **Beschreibung** |
|---|---|
| `TenantId` | Ihre Mandanten-ID |
| `SourceSystem` | `Azure` |
| `TimeGenerated [UTC]` | Zeitstempel für den Aufzeichnungsbeginn des Protokolls in UTC |
| `Type` | Typ des Protokolls Immer `AzureDiagnostics` |
| `SubscriptionId` | GUID für das Abonnement, zu dem der Server gehört |
| `ResourceGroup` | Name der Ressourcengruppe, zu der der Server gehört |
| `ResourceProvider` | Name des Ressourcenanbieters Immer `MICROSOFT.DBFORMYSQL` |
| `ResourceType` | `Servers` |
| `ResourceId` | Ressourcen-URI |
| `Resource` | Name des Servers |
| `Category` | `MySqlAuditLogs` |
| `OperationName` | `LogEvent` |
| `LogicalServerName_s` | Name des Servers |
| `event_class_s` | `table_access_log` |
| `event_subclass_s` | `READ`, `INSERT`, `UPDATE` oder `DELETE` |
| `connection_id_d` | Von MySQL generierte eindeutige Verbindungs-ID |
| `db_s` | Name der Datenbank, auf die zugegriffen wird |
| `table_s` | Name der Tabelle, auf die zugegriffen wird |
| `sql_text_s` | Vollständiger Abfragetext |
| `\_ResourceId` | Ressourcen-URI |

## <a name="next-steps"></a>Nächste Schritte

- [Konfigurieren von Überwachungsprotokollen im Azure-Portal](howto-configure-audit-logs-portal.md)