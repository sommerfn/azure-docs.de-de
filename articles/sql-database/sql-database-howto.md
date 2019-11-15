---
title: Konfigurieren und Verwalten
description: Hier erfahren Sie, wie Sie Azure SQL-Datenbank konfigurieren und verwalten.
services: sql-database
ms.service: sql-database
ms.subservice: service
ms.custom: ''
ms.devlang: ''
ms.topic: conceptual
author: jovanpop-msft
ms.author: jovanpop
ms.reviewer: sstein
ms.date: 01/25/2019
ms.openlocfilehash: 5773ea37585bb5c63736aaa4432243dc1d41f7bc
ms.sourcegitcommit: ac56ef07d86328c40fed5b5792a6a02698926c2d
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 11/08/2019
ms.locfileid: "73811178"
---
# <a name="how-to-use-azure-sql-database"></a>Verwenden von Azure SQL-Datenbank

In diesem Abschnitt sind verschiedene Anleitungen, Skripts und Erläuterungen aufgeführt, die beim Verwalten und Konfigurieren von Azure SQL-Datenbank hilfreich sein können. Spezifische Anleitungen für [Einzeldatenbank](sql-database-howto-single-database.md) und [Verwaltete Instanz](sql-database-howto-managed-instance.md) stehen ebenfalls zur Verfügung.

## <a name="load-data"></a>Laden von Daten

- [Kopieren von Einzel- oder Pooldatenbanken innerhalb von Azure](sql-database-copy.md)
- [Importieren einer Datenbank über eine BACPAC-Datei](sql-database-import.md)
- [Exportieren einer Datenbank in eine BACPAC-Datei](sql-database-export.md)
- [Laden von Daten mit BCP](sql-database-load-from-csv-with-bcp.md)
- [Laden von Daten mit ADF](../data-factory/connector-azure-sql-database.md?toc=/azure/sql-database/toc.json)

### <a name="data-sync"></a>Datensynchronisierung

- [SQL-Datensynchronisierung](sql-database-sync-data.md)
- [Datensynchronisierungs-Agent](sql-database-data-sync-agent.md)
- [Replizieren von Schemaänderungen](sql-database-update-sync-schema.md)
- [Überwachen mit OMS](sql-database-sync-monitor-oms.md)
- [Bewährte Methoden für die Datensynchronisierung](sql-database-best-practices-data-sync.md)
- [Problembehandlung für die Datensynchronisierung](sql-database-troubleshoot-data-sync.md)

## <a name="monitoring-and-tuning"></a>Überwachung und Optimierung

- [Manuelles Optimieren](sql-database-performance-guidance.md)
- [Überwachen der Leistung mithilfe von DMVs](sql-database-monitoring-with-dmvs.md)
- [Überwachen der Leistung mit dem Abfragespeicher](sql-database-operate-query-store.md)
- [Behandeln von Leistungsproblemen mithilfe von Intelligent Insights](sql-database-intelligent-insights-troubleshoot-performance.md)
- [Verwenden des Intelligent Insights-Diagnoseprotokolls](sql-database-intelligent-insights-use-diagnostics-log.md)
- [Überwachen des In-Memory-OLTP-Speicherplatzes](sql-database-in-memory-oltp-monitoring.md)

### <a name="extended-events"></a>Erweiterte Ereignisse

- [Erweiterte Ereignisse](sql-database-xevent-db-diff-from-svr.md)
- [Speichern erweiterter Ereignisse in der Ereignisdatei](sql-database-xevent-code-event-file.md)
- [Speichern erweiterter Ereignisse im Ringpuffer](sql-database-xevent-code-ring-buffer.md)

## <a name="configure-features"></a>Konfigurieren von Features

- [Konfigurieren der Azure AD-Authentifizierung](sql-database-aad-authentication-configure.md)
- [Konfigurieren des bedingten Zugriffs](sql-database-conditional-access.md)
- [Mehrstufige AAD-Authentifizierung](sql-database-ssms-mfa-authentication.md)
- [Konfigurieren der mehrstufigen Authentifizierung](sql-database-ssms-mfa-authentication-configure.md)
- [Konfigurieren einer temporalen Aufbewahrungsrichtlinie](sql-database-temporal-tables-retention-policy.md)
- [Konfigurieren von TDE mit BYOK](transparent-data-encryption-byok-azure-sql-configure.md)
- [Rotieren von TDE-BYOK-Schlüsseln](transparent-data-encryption-byok-azure-sql-key-rotation.md)
- [Entfernen einer TDE-Schutzvorrichtung](transparent-data-encryption-byok-azure-sql-remove-tde-protector.md)
- [Konfigurieren von In-Memory OLTP](sql-database-in-memory-oltp-migration.md)
- [Konfigurieren von Azure Automation](sql-database-manage-automation.md)

## <a name="develop-applications"></a>Entwickeln von Anwendungen

- [Konnektivität](sql-database-libraries.md)
- [Verwenden des Spark-Connectors](sql-database-spark-connector.md)
- [Authentifizieren der App](sql-database-client-id-keys.md)
- [Fehlermeldungen](sql-database-develop-error-messages.md)
- [Verwenden der Batchverarbeitung für bessere Leistung](sql-database-use-batching-to-improve-performance.md)
- [Konnektivitätsleitfaden](sql-database-connectivity-issues.md)
- [DNS-Aliase](dns-alias-overview.md)
- [Einrichten eines DNS-Alias über PowerShell](dns-alias-powershell.md)
- [Ports – ADO.NET](sql-database-develop-direct-route-ports-adonet-v12.md)
- [C und C ++](sql-database-develop-cplusplus-simple.md)
- [Excel](sql-database-connect-excel.md)

## <a name="design-applications"></a>Entwerfen von Anwendungen

- [Entwurf für die Notfallwiederherstellung](sql-database-designing-cloud-solutions-for-disaster-recovery.md)
- [Entwurf für Pools für elastische Datenbanken](sql-database-disaster-recovery-strategies-for-applications-with-elastic-pool.md)
- [Entwurf für App-Upgrades](sql-database-manage-application-rolling-upgrade.md)

### <a name="design-multi-tenant-saas-applications"></a>Entwurf von SaaS-Anwendungen mit mehreren Mandanten

- [SaaS-Entwurfsmuster](saas-tenancy-app-design-patterns.md)
- [SaaS-Video Indexer](saas-tenancy-video-index-wingtip-brk3120-20171011.md)
- [SaaS-App-Sicherheit](saas-tenancy-elastic-tools-multi-tenant-row-level-security.md)

## <a name="next-steps"></a>Nächste Schritte

- Erfahren Sie mehr über [Anleitungen für verwaltete Instanzen](sql-database-howto-managed-instance.md).
- Erfahren Sie mehr über [Anleitungen für Einzeldatenbanken](sql-database-howto-single-database.md).
