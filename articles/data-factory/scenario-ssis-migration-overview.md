---
title: Migrieren von lokalen SSIS-Workloads zu SSIS in Azure Data Factory
description: Migrieren Sie lokale SSIS-Workloads zu SSIS in ADF.
services: data-factory
documentationcenter: ''
author: chugugrace
ms.author: chugu
ms.reviewer: ''
manager: ''
ms.service: data-factory
ms.workload: data-services
ms.tgt_pltfrm: na
ms.topic: conceptual
ms.date: 9/3/2019
ms.openlocfilehash: e75e6bc78740ffb8aba0faa0ef95f4b13a8c56ef
ms.sourcegitcommit: 609d4bdb0467fd0af40e14a86eb40b9d03669ea1
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 11/06/2019
ms.locfileid: "73684408"
---
# <a name="migrate-on-premises-ssis-workloads-to-ssis-in-adf"></a>Migrieren von lokalen SSIS-Workloads zu SSIS in ADF

## <a name="overview"></a>Übersicht

Wenn Sie Ihre Datenbankworkloads von SQL Server lokal zu Azure-Datenbankdiensten migrieren (und zwar Azure SQL-Datenbank oder eine verwaltete Azure SQL-Datenbank-Instanz), müssen Ihre ETL-Workloads in SSIS (SQL Server Integration Services) als einer der primären Dienste mit Mehrwert ebenfalls migriert werden.

Azure-SSIS Integration Runtime (IR) in Azure Data Factory (ADF) unterstützt die Ausführung von SSIS-Paketen. Sobald Azure-SSIS IR bereitgestellt wurde, können Sie vertraute Tools wie SQL Server Data Tools (SSDT)/SQL Server Management Studio (SSMS) und Befehlszeilenprogramme wie „dtinstall/dtutil/dtexec“ verwenden, um Ihre Pakete in Azure bereitzustellen und auszuführen. Weitere Informationen finden Sie unter [Azure SSIS per Lift & Shift migrieren – Übersicht](https://docs.microsoft.com/sql/integration-services/lift-shift/ssis-azure-lift-shift-ssis-packages-overview).

In diesem Artikel wird der Migrationsprozess Ihrer ETL-Workloads von lokalen SSIS zu SSIS in ADF hervorgehoben. Der Migrationsprozess besteht aus zwei Phasen: **Bewertung** und **Migration**.

## <a name="assessment"></a>Bewertung

Um einen vollständigen Migrationsplan zu erstellen, hilft eine gründliche Bewertung, Probleme mit den SSIS-Quellpaketen zu identifizieren, die eine erfolgreiche Migration verhindern würden.

Der Datenmigrations-Assistent (DMA) ist ein zu diesem Zweck frei herunterladbares Tool, das lokal installiert und ausgeführt werden kann. Das DMA-Bewertungsprojekt vom Typ **Integrationsdienst** kann erstellt werden, um SSIS-Pakete in Batches zu bewerten und Kompatibilitätsprobleme zu identifizieren, die in den folgenden Kategorien dargestellt werden:

- Migrationsblockierungen: Dies sind Kompatibilitätsprobleme, die die Migration von Quellpaketen für die Ausführung in Azure-SSIS IR blockieren. DMA bietet Anleitungen, die Ihnen helfen, diese Probleme zu lösen.

- Informative Probleme: Dies sind teilweise unterstützte oder veraltete Features, die in Quellpaketen verwendet werden. DMA bietet zur Behebung eine umfassende Reihe von Empfehlungen, in Azure verfügbare alternative Ansätze, und Schritte zur Risikominimierung.

### <a name="four-storage-types-for-ssis-packages"></a>Vier Speichertypen für SSIS-Pakete

- SSIS-Katalog (SSISDB): Dies wurde mit SQL Server 2012 eingeführt und enthält eine Reihe von gespeicherten Prozeduren, Ansichten und Tabellenwertfunktionen, die für die Arbeit mit SSIS-Projekten/Paketen verwendet werden.
- Dateisystem:
- SQL Server-Systemdatenbank (MSDB).
- SSIS-Paketspeicher: Dabei handelt es sich um eine Paketverwaltungsebene über zwei Untertypen:
  - MSDB, eine Systemdatenbank in SQL Server, die zum Speichern von SSIS-Paketen verwendet wird.
  - Verwaltetes Dateisystem, bei dem es sich um einen bestimmten Ordner im SQL Server-Installationspfad handelt, in dem SSIS-Pakete gespeichert werden.

DMA unterstützt derzeit die Batchbewertung von Paketen, die im Speichertyp **Dateisystem** seit **DMA Version v4.5** gespeichert sind.

Rufen Sie [DMA](https://docs.microsoft.com/sql/dma/dma-overview) ab, und [führen Sie damit Ihre Paketbewertung durch](https://docs.microsoft.com/sql/dma/dma-assess-ssis).

## <a name="migration"></a>Migration

In Abhängigkeit von den [Speichertypen](#four-storage-types-for-ssis-packages) von SSIS-Quellpaketen und dem Migrationsziel der Datenbankworkloads können die Schritte zum Migrieren von **SSIS-Paketen** und **SQL Server-Agentaufträgen** variieren, die die Ausführung von SSIS-Paketen planen. Es gibt zwei Szenarien:

- [**Azure SQL-Datenbank – Verwaltete Instanz** als Datenbankworkloadziel](#azure-sql-database-managed-instance-as-database-workload-destination)
- [**Azure SQL-Datenbank** als Datenbankworkloadziel](#azure-sql-database-as-database-workload-destination)

### <a name="azure-sql-database-managed-instance-as-database-workload-destination"></a>**Azure SQL-Datenbank – Verwaltete Instanz** als Datenbankworkloadziel

| **Paketspeichertyp** |Batchmigration von SSIS-Paketen|Batchmigration von SSIS-Aufträgen|
|-|-|-|
|SSISDB|[Migrieren von **SSISDB**](scenario-ssis-migration-ssisdb-mi.md)|[Migrieren von SSIS-Aufträgen zu einem Agent einer verwalteten Azure SQL-Datenbank-Instanz](scenario-ssis-migration-ssisdb-mi.md#ssis-jobs-to-azure-sql-database-managed-instance-agent)|
|Dateisystem|Stellen Sie sie in Dateifreigaben/Azure Files über „dtinstall/dtutil/manual copy“ erneut bereit, oder bewahren Sie sie in Dateisystemen für den Zugriff über VNet/Selbstgehostete IR. Weitere Informationen finden Sie unter [dtutil-Hilfsprogramm](https://docs.microsoft.com/sql/integration-services/dtutil-utility).|Konvertieren Sie sie mithilfe von Skripts/SSMS/ADF-Portal in ADF-Pipelines/-Aktivitäten/-Trigger. Weitere Informationen finden Sie unter [SSMS-Zeitplanungsfunktion](https://docs.microsoft.com/sql/integration-services/lift-shift/ssis-azure-schedule-packages-ssms).|
|SQL Server (MSDB)|Exportieren Sie sie über SSMS/dtutil in Dateisysteme/Dateifreigaben/Azure Files. Weitere Informationen finden Sie unter [Exportieren von SSIS-Paketen](https://docs.microsoft.com/sql/integration-services/import-and-export-packages-ssis-service).|Konvertieren Sie sie mithilfe von Skripts/SSMS/ADF-Portal in ADF-Pipelines/-Aktivitäten/-Trigger. Weitere Informationen finden Sie unter [SSMS-Zeitplanungsfunktion](https://docs.microsoft.com/sql/integration-services/lift-shift/ssis-azure-schedule-packages-ssms).|
|Paketspeicher|Exportieren Sie sie über SSMS/dtutil in Dateisystme/Dateifreigaben/Azure Files, oder stellen Sie sie in Dateifreigaben/Azure Files über dtinstall/dtutil/manuelle Kopie erneut bereit, oder bewahren Sie sie in Dateisystemen für den Zugriff über VNet/Selbstgehostete IR. Weitere Informationen finden Sie unter „dtutil-Hilfsprogramm“. Weitere Informationen finden Sie unter [dtutil-Hilfsprogramm](https://docs.microsoft.com/sql/integration-services/dtutil-utility).|Konvertieren Sie sie mithilfe von Skripts/SSMS/ADF-Portal in ADF-Pipelines/-Aktivitäten/-Trigger. Weitere Informationen finden Sie unter [SSMS-Zeitplanungsfunktion](https://docs.microsoft.com/sql/integration-services/lift-shift/ssis-azure-schedule-packages-ssms).|

### <a name="azure-sql-database-as-database-workload-destination"></a>**Azure SQL-Datenbank** als Datenbankworkloadziel

| **Paketspeichertyp** |Batchmigration von SSIS-Paketen|Batchmigration von Aufträgen|
|-|-|-|
|SSISDB|Stellen Sie sie über SSDT/SSMS erneut in Azure-SSISDB bereit. Weitere Informationen finden Sie unter [Bereitstellen von SSIS-Paketen in Azure](https://docs.microsoft.com/sql/integration-services/lift-shift/ssis-azure-deploy-run-monitor-tutorial).|Konvertieren Sie sie mithilfe von Skripts/SSMS/ADF-Portal in ADF-Pipelines/-Aktivitäten/-Trigger. Weitere Informationen finden Sie unter [SSMS-Zeitplanungsfunktion](https://docs.microsoft.com/sql/integration-services/lift-shift/ssis-azure-schedule-packages-ssms).|
|Dateisystem|Stellen Sie sie in Dateifreigaben/Azure Files über „dtinstall/dtutil/manual copy“ erneut bereit, oder bewahren Sie sie in Dateisystemen für den Zugriff über VNet/Selbstgehostete IR. Weitere Informationen finden Sie unter [dtutil-Hilfsprogramm](https://docs.microsoft.com/sql/integration-services/dtutil-utility).|Konvertieren Sie sie mithilfe von Skripts/SSMS/ADF-Portal in ADF-Pipelines/-Aktivitäten/-Trigger. Weitere Informationen finden Sie unter [SSMS-Zeitplanungsfunktion](https://docs.microsoft.com/sql/integration-services/lift-shift/ssis-azure-schedule-packages-ssms).|
|SQL Server (MSDB)|Exportieren Sie sie über SSMS/dtutil in Dateisysteme/Dateifreigaben/Azure Files. Weitere Informationen finden Sie unter [Exportieren von SSIS-Paketen](https://docs.microsoft.com/sql/integration-services/import-and-export-packages-ssis-service).|Konvertieren Sie sie mithilfe von Skripts/SSMS/ADF-Portal in ADF-Pipelines/-Aktivitäten/-Trigger. Weitere Informationen finden Sie unter [SSMS-Zeitplanungsfunktion](https://docs.microsoft.com/sql/integration-services/lift-shift/ssis-azure-schedule-packages-ssms).|
|Paketspeicher|Exportieren Sie sie über SSMS/dtutil in Dateisystme/Dateifreigaben/Azure Files, oder stellen Sie sie in Dateifreigaben/Azure Files über dtinstall/dtutil/manuelle Kopie erneut bereit, oder bewahren Sie sie in Dateisystemen für den Zugriff über VNet/Selbstgehostete IR. Weitere Informationen finden Sie unter „dtutil-Hilfsprogramm“. Weitere Informationen finden Sie unter [dtutil-Hilfsprogramm](https://docs.microsoft.com/sql/integration-services/dtutil-utility).|Konvertieren Sie sie mithilfe von Skripts/SSMS/ADF-Portal in ADF-Pipelines/-Aktivitäten/-Trigger. Weitere Informationen finden Sie unter [SSMS-Zeitplanungsfunktion](https://docs.microsoft.com/sql/integration-services/lift-shift/ssis-azure-schedule-packages-ssms).|

## <a name="additional-resources"></a>Zusätzliche Ressourcen

- [Azure Data Factory](https://docs.microsoft.com/azure/data-factory/introduction)
- [Datenbankmigrations-Assistent](https://docs.microsoft.com/sql/dma/dma-overview)
- [Migrieren von SSIS-Workloads per Lift & Shift in die Cloud](https://docs.microsoft.com/sql/integration-services/lift-shift/ssis-azure-lift-shift-ssis-packages-overview?view=sql-server-2017)
- [Migrieren von SSIS-Paketen zu einer verwalteten Azure SQL-Datenbank-Instanz](https://docs.microsoft.com/azure/dms/how-to-migrate-ssis-packages-managed-instance)
- [Erneutes Bereitstellen von Paketen für Azure SQL-Datenbank](https://docs.microsoft.com/azure/dms/how-to-migrate-ssis-packages)

## <a name="next-steps"></a>Nächste Schritte

- [Überprüfen von in Azure bereitgestellten SSIS-Paketen](https://docs.microsoft.com/sql/integration-services/lift-shift/ssis-azure-validate-packages)
- [Ausführen von in Azure bereitgestellten SSIS-Paketen](https://docs.microsoft.com/sql/integration-services/lift-shift/ssis-azure-run-packages)
- [Überwachen von Azure-SSIS Integration Runtime](https://docs.microsoft.com/azure/data-factory/monitor-integration-runtime#azure-ssis-integration-runtime)
- [Planen der Ausführung von in Azure bereitgestellten SSIS-Paketen](https://docs.microsoft.com/sql/integration-services/lift-shift/ssis-azure-schedule-packages)
