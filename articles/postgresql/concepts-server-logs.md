---
title: Serverprotokolle in Azure Database for PostgreSQL (Einzelserver)
description: Dieser Artikel beschreibt, wie Azure Database for PostgreSQL (Einzelserver) Protokolle für Abfragen und Fehler generiert und wie die Protokollaufbewahrung konfiguriert wird.
author: rachel-msft
ms.author: raagyema
ms.service: postgresql
ms.topic: conceptual
ms.date: 09/18/2019
ms.openlocfilehash: 17083029f2377037b99abfa3ce8371661eccb957
ms.sourcegitcommit: 11265f4ff9f8e727a0cbf2af20a8057f5923ccda
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 10/08/2019
ms.locfileid: "72029993"
---
# <a name="server-logs-in-azure-database-for-postgresql---single-server"></a>Serverprotokolle in Azure Database for PostgreSQL (Einzelserver)
Azure Database for PostgreSQL generiert Abfragen und Fehlerprotokolle. Diese Abfrage- und Fehlerprotokolle dienen zur Identifizierung, Behebung und Reparatur von Konfigurationsfehlern und suboptimaler Leistung. (Der Zugriff auf Transaktionsprotokolle ist nicht enthalten.) 

## <a name="configure-logging"></a>Konfigurieren der Protokollierung 
Sie können die Protokollierung auf dem Server mithilfe von Serverparametern für die Protokollierung konfigurieren. Auf jedem neuen Server sind **log_checkpoints** und **log_connections** standardmäßig aktiviert. Es gibt zusätzliche Parameter, die Sie entsprechend Ihren Protokollierungsanforderungen anpassen können: 

![Azure Database for PostgreSQL – Protokollierungsparameter](./media/concepts-server-logs/log-parameters.png)

Weitere Informationen zu diesen Parametern finden Sie in der Dokumentation zu PostgreSQL unter [Fehlerberichterstattung und -protokollierung](https://www.postgresql.org/docs/current/static/runtime-config-logging.html). Weitere Informationen dazu, wie Sie Azure Database for PostgreSQL-Parameter konfigurieren, finden Sie in der Dokumentation zum [Portal](howto-configure-server-parameters-using-portal.md) bzw. zur [Befehlszeilenschnittstelle](howto-configure-server-parameters-using-cli.md).

## <a name="access-server-logs-through-portal-or-cli"></a>Zugreifen auf Serverprotokolle über das Portal oder die Befehlszeilenschnittstelle
Wenn Sie Protokolle aktiviert haben, können Sie im Protokollspeicher von Azure Database for PostgreSQL über das [Azure-Portal](howto-configure-server-logs-in-portal.md), die [Azure-Befehlszeilenschnittstelle](howto-configure-server-logs-using-cli.md) und die Azure-REST-APIs auf sie zugreifen. Die Protokolldateien rotieren jede Stunde oder bei einer Größe von 100 MB, je nachdem, welcher Fall zuerst eintritt. Mithilfe des mit Ihrem Server verknüpften Parameters **log\_retention\_period** können Sie die Beibehaltungsdauer für diesen Protokollspeicher festlegen. Der Standardwert ist 3 Tage. Der Maximalwert beträgt 7 Tage. Ihrem Server muss genügend Speicher zugewiesen sein, damit die Protokolldateien gespeichert werden können. (Dieser retention-Parameter steuert nicht Azure-Diagnoseprotokolle.)


## <a name="diagnostic-logs"></a>Diagnoseprotokolle
Azure Database for PostgreSQL ist in Azure Monitor-Diagnoseprotokolle integriert. Nachdem Sie Protokolle auf Ihrem PostgreSQL-Server aktiviert haben, können Sie auswählen, dass sie an [Azure Monitor-Protokolle](../azure-monitor/log-query/log-query-overview.md), Event Hubs oder Azure Storage ausgegeben werden sollen. 

> [!IMPORTANT]
> Dieses Diagnosefeature für Serverprotokolle steht nur in den [Tarifen](concepts-pricing-tiers.md) „Universell“ und „Arbeitsspeicheroptimiert“ zur Verfügung.

So aktivieren Sie Diagnoseprotokolle über das Azure-Portal:

   1. Wechseln Sie im Portal im Navigationsmenü Ihres Postgres-Servers zu *Diagnoseeinstellungen*.
   2. Wählen Sie *Diagnoseeinstellung hinzufügen*  aus.
   3. Benennen Sie die Einstellung. 
   4. Wählen Sie Ihren bevorzugten Downstreamspeicherort aus (Speicherkonto, Event Hub, Log Analytics). 
   5. Wählen Sie die gewünschten Datentypen aus.
   6. Speichern Sie die Einstellungen.

In der folgenden Tabelle wird der Inhalt der einzelnen Protokolle beschrieben. Je nach dem ausgewählten Ausgabeendpunkt können die enthaltenen Felder und ihre Reihenfolge variieren. 

|**Feld** | **Beschreibung** |
|---|---|
| TenantId | Ihre Mandanten-ID |
| SourceSystem | `Azure` |
| TimeGenerated [UTC] | Zeitstempel für den Aufzeichnungsbeginn des Protokolls in UTC |
| type | Typ des Protokolls Immer `AzureDiagnostics` |
| SubscriptionId | GUID für das Abonnement, zu dem der Server gehört |
| ResourceGroup | Name der Ressourcengruppe, zu der der Server gehört |
| ResourceProvider | Name des Ressourcenanbieters Immer `MICROSOFT.DBFORPOSTGRESQL` |
| ResourceType | `Servers` |
| resourceId | Ressourcen-URI |
| Resource | Name des Servers |
| Category | `PostgreSQLLogs` |
| OperationName | `LogEvent` |
| errorLevel | Beispiel für die Protokollierungsstufe: LOG, ERROR, NOTICE |
| `Message` | Primäre Protokollmeldung | 
| Domain | Serverversion, Beispiel: postgres-10 |
| Detail | Sekundäre Protokollmeldung (falls zutreffend) |
| ColumnName | Name der Spalte (falls zutreffend) |
| SchemaName | Name des Schemas (falls zutreffend) |
| DatatypeName | Name des Datentyps (falls zutreffend) |
| LogicalServerName | Name des Servers | 
| _ResourceId | Ressourcen-URI |
| Präfix | Präfix der Protokollzeile |



## <a name="next-steps"></a>Nächste Schritte
- Informieren Sie sich über den Zugriff auf Protokolle über das [Azure-Portal](howto-configure-server-logs-in-portal.md) oder die [Azure-Befehlszeilenschnittstelle](howto-configure-server-logs-using-cli.md).
- Erfahren Sie mehr über die [Preise für Azure Monitor](https://azure.microsoft.com/pricing/details/monitor/).
