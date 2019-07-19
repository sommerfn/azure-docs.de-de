---
title: Überwachungsprotokolle für Azure Database for MariaDB
description: Informationen zu den Überwachungsprotokollen, die in Azure Database for MariaDB verfügbar sind, sowie die verfügbaren Parameter zum Aktivieren von Protokolliergraden.
author: ajlam
ms.author: andrela
ms.service: mariadb
ms.topic: conceptual
ms.date: 06/26/2019
ms.openlocfilehash: 13ea60c62283db35ce4bf9fde6c3b36ba7f88013
ms.sourcegitcommit: f56b267b11f23ac8f6284bb662b38c7a8336e99b
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 06/28/2019
ms.locfileid: "67439219"
---
# <a name="audit-logs-in-azure-database-for-mariadb"></a>Überwachungsprotokolle in Azure Database for MariaDB

In Azure Database for MariaDB ist das Überwachungsprotokoll für Benutzer verfügbar. Das Überwachungsprotokoll dient zum Nachverfolgen von Aktivitäten auf Datenbankebene und häufig zu Konformitätszwecken.

> [!IMPORTANT]
> Diese Überwachungsprotokollfunktion ist derzeit in der Vorschauphase.

## <a name="configure-audit-logging"></a>Konfigurieren der Überwachungsprotokollierung

Das Überwachungsprotokoll ist standardmäßig deaktiviert. Um sie zu aktivieren, legen Sie `audit_log_enabled` auf ON fest.

Weitere Parameter, die Sie anpassen können:

- `audit_log_events`: steuert die zu protokollierenden Ereignisse. In der nachstehenden Tabelle finden Sie spezifische Überwachungsereignisse.
- `audit_log_exclude_users`: MariaDB-Benutzer, die von der Protokollierung ausgeschlossen werden sollen. Maximal sind vier Benutzer zulässig. Die maximale Länge des Parameters ist 256 Zeichen.

| **Event** | **Beschreibung** |
|---|---|
| `CONNECTION` | : Initiierung der Verbindung (erfolgreich oder nicht erfolgreich) <br> : Erneute Authentifizierung des Benutzers mit anderem Benutzernamen/Kennwort während der Sitzung <br> : Beendigung der Verbindung |
| `DML_SELECT`| SELECT-Abfragen |
| `DML_NONSELECT` | INSERT/DELETE/UPDATE-Abfragen |
| `DML` | DML = DML_SELECT + DML_NONSELECT |
| `DDL` | Abfragen wie DROP DATABASE |
| `DCL` | Abfragen wie GRANT PERMISSION |
| `ADMIN` | Abfragen wie SHOW STATUS |
| `GENERAL` | Alle in DML_SELECT, DML_NONSELECT, DML, DDL, DCL und ADMIN |

## <a name="access-audit-logs"></a>Zugreifen auf Überwachungsprotokolle

Überwachungsprotokolle sind in Azure Monitor-Diagnoseprotokolle integriert. Nachdem Sie auf Ihrem MariaDB-Server Überwachungsprotokolle aktiviert haben, können Sie sie an Azure Monitor-Protokolle, Event Hubs oder Azure Storage senden. Weitere Informationen zum Aktivieren von Diagnoseprotokollen im Azure-Portal finden Sie im Artikel zum [Portal für Überwachungsprotokolle](howto-configure-audit-logs-portal.md#set-up-diagnostic-logs).

## <a name="diagnostic-logs-schemas"></a>Schemas für Diagnoseprotokolle

In den folgenden Abschnitten wird beschrieben, was in MariaDB-Überwachungsprotokollen basierend auf dem Ereignistyp ausgegeben wird. Je nach Ausgabemethode können die enthaltenen Felder und ihre Reihenfolge variieren.

### <a name="connection"></a>Verbindung

| **Eigenschaft** | **Beschreibung** |
|---|---|
| `TenantId` | Ihre Mandanten-ID |
| `SourceSystem` | `Azure` |
| `TimeGenerated [UTC]` | Zeitstempel für den Aufzeichnungsbeginn des Protokolls in UTC |
| `Type` | Typ des Protokolls Immer `AzureDiagnostics` |
| `SubscriptionId` | GUID für das Abonnement, zu dem der Server gehört |
| `ResourceGroup` | Name der Ressourcengruppe, zu der der Server gehört |
| `ResourceProvider` | Name des Ressourcenanbieters Immer `MICROSOFT.DBFORMARIADB` |
| `ResourceType` | `Servers` |
| `ResourceId` | Ressourcen-URI |
| `Resource` | Name des Servers |
| `Category` | `MySqlAuditLogs` |
| `OperationName` | `LogEvent` |
| `event_class_s` | `connection_log` |
| `event_subclass_s` | `CONNECT`, `DISCONNECT` |
| `connection_id_d` | Von MariaDB generierte eindeutige Verbindungs-ID |
| `host_s` | Leer |
| `ip_s` | IP-Adresse des Clients, der die Verbindung mit MariaDB herstellt |
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
| `ResourceProvider` | Name des Ressourcenanbieters Immer `MICROSOFT.DBFORMARIADB` |
| `ResourceType` | `Servers` |
| `ResourceId` | Ressourcen-URI |
| `Resource` | Name des Servers |
| `Category` | `MySqlAuditLogs` |
| `OperationName` | `LogEvent` |
| `LogicalServerName_s` | Name des Servers |
| `event_class_s` | `general_log` |
| `event_subclass_s` | `LOG`, `ERROR`, `RESULT` |
| `event_time` | Sekunden des Abfragestarts im UNIX-Zeitstempel |
| `error_code_d` | Fehlercode bei fehlerhafter Abfrage. `0` bedeutet, dass kein Fehler vorliegt |
| `thread_id_d` | ID des Threads, der die Abfrage ausgeführt hat |
| `host_s` | Leer |
| `ip_s` | IP-Adresse des Clients, der die Verbindung mit MariaDB herstellt |
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
| `ResourceProvider` | Name des Ressourcenanbieters Immer `MICROSOFT.DBFORMARIADB` |
| `ResourceType` | `Servers` |
| `ResourceId` | Ressourcen-URI |
| `Resource` | Name des Servers |
| `Category` | `MySqlAuditLogs` |
| `OperationName` | `LogEvent` |
| `LogicalServerName_s` | Name des Servers |
| `event_class_s` | `table_access_log` |
| `event_subclass_s` | `READ`, `INSERT`, `UPDATE` oder `DELETE` |
| `connection_id_d` | Von MariaDB generierte eindeutige Verbindungs-ID |
| `db_s` | Name der Datenbank, auf die zugegriffen wird |
| `table_s` | Name der Tabelle, auf die zugegriffen wird |
| `sql_text_s` | Vollständiger Abfragetext |
| `\_ResourceId` | Ressourcen-URI |

## <a name="next-steps"></a>Nächste Schritte

- [Konfigurieren von Überwachungsprotokollen im Azure-Portal](howto-configure-audit-logs-portal.md)