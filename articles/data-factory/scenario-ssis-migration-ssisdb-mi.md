---
title: SSIS-Migration mit einer verwalteten Azure SQL-Datenbank-Instanz als Datenbankworkloadziel
description: SSIS-Migration mit einer verwalteten Azure SQL-Datenbank-Instanz als Datenbankworkloadziel
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
ms.date: 9/12/2019
ms.openlocfilehash: 6ea9134085812b99d59ad64aa2c9ec1b2ff827d9
ms.sourcegitcommit: 609d4bdb0467fd0af40e14a86eb40b9d03669ea1
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 11/06/2019
ms.locfileid: "73684403"
---
# <a name="ssis-migration-with-azure-sql-database-managed-instance-as-the-database-workload-destination"></a>SSIS-Migration mit einer verwalteten Azure SQL-Datenbank-Instanz als Datenbankworkloadziel

Wenn Sie Datenbankworkloads von SQL Server lokal zu einer verwalteten Azure SQL-Datenbank-Instanz migrieren, sollten Sie mit [Azure Data Migration Service](https://docs.microsoft.com/azure/dms/dms-overview) (DMS) und den [Netzwerktopologien für die Migration verwalteter Azure SQL-Datenbank-Instanzen mithilfe von DMS vertraut sein](https://docs.microsoft.com/azure/dms/resource-network-topologies).

Dieser Artikel konzentriert sich auf die Migration von SSIS-Paketen (SQL Server Integration Service), die im SSIS-Katalog (SSISDB) gespeichert sind, und auf SQL Server-Agentaufträge, die die Ausführung von SSIS-Paketen planen.

## <a name="migrate-ssis-catalog-ssisdb"></a>Migrieren des SSIS-Katalogs (SSISDB)

Die SSISDB-Migration kann mithilfe von DMS erfolgen, wie im folgenden Artikel beschrieben: [Migrieren von SSIS-Paketen zu einer verwalteten Azure SQL-Datenbank-Instanz](https://docs.microsoft.com/azure/dms/how-to-migrate-ssis-packages-managed-instance)

## <a name="ssis-jobs-to-azure-sql-database-managed-instance-agent"></a>SSIS-Aufträge zu einem Agent einer verwalteten Azure SQL-Datenbank-Instanz

Die verwaltete Azure SQL-Datenbank-Instanz verfügt wie der lokale SQL Server-Agent über einen nativen, erstklassigen Planer.  Da ein Migrationstool für SSIS-Aufträge noch nicht verfügbar ist, müssen sie vom lokalen SQL Server-Agent in den vom Agent der verwalteten Azure SQL-Datenbank-Instanz mithilfe von Skripts bzw. manueller Kopie migriert werden.

## <a name="additional-resources"></a>Zusätzliche Ressourcen

- [Azure Data Factory](https://docs.microsoft.com/azure/data-factory/introduction)
- [Azure-SSIS Integration Runtime](https://docs.microsoft.com/azure/data-factory/create-azure-ssis-integration-runtime)
- [Azure Database Migration Service](https://docs.microsoft.com/azure/dms/dms-overview)
- [Netzwerktopologien für Migrationen vom Typ „Verwaltete Azure SQL-Datenbank-Instanz“ mithilfe von DMS](https://docs.microsoft.com/azure/dms/resource-network-topologies)
- [Migrieren von SSIS-Paketen zu einer verwalteten Azure SQL-Datenbank-Instanz](https://docs.microsoft.com/azure/dms/how-to-migrate-ssis-packages-managed-instance)

## <a name="next-steps"></a>Nächste Schritte

- [Herstellen einer Verbindung mit dem SSIS-Katalog (SSISDB) in Azure](https://docs.microsoft.com/sql/integration-services/lift-shift/ssis-azure-connect-to-catalog-database)
- [Ausführen von in Azure bereitgestellten SSIS-Paketen](https://docs.microsoft.com/sql/integration-services/lift-shift/ssis-azure-run-packages)
