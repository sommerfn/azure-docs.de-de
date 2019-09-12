---
title: Status von Datenbankmigrationsszenarios | Microsoft-Dokumentation
description: Informationen zum Status von Migrationsszenarien, die in Azure Database Migration Service unterstützt werden.
services: database-migration
author: HJToland3
ms.author: jtoland
manager: craigg
ms.reviewer: craigg
ms.service: dms
ms.workload: data-services
ms.custom: mvc
ms.topic: article
ms.date: 09/05/2019
ms.openlocfilehash: 9ea070ea86a4d0d5e7101e82450235a59c936d2d
ms.sourcegitcommit: aebe5a10fa828733bbfb95296d400f4bc579533c
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 09/05/2019
ms.locfileid: "70376406"
---
# <a name="status-of-migration-scenarios-supported-by-azure-database-migration-service"></a>Status von Migrationsszenarien, die in Azure Database Migration Service unterstützt werden

Azure Database Migration Service wurde zur Unterstützung verschiedener Migrationsszenarien (Quelle-Ziel-Paare) und sowohl für die Offline- (einmalig) als auch die Onlinemigration (fortlaufende Synchronisierung) konzipiert. Der in Azure Database Migration Service bereitgestellte Szenarioumfang wird im Lauf der Zeit erweitert. In regelmäßigen Abständen werden neue Szenarios hinzugefügt. In diesem Artikel werden die Migrationsszenarien, die derzeit in Azure Database Migration Service unterstützt werden, und der Status der einzelnen Szenarien (private Vorschau, öffentliche Vorschau oder allgemein verfügbar) definiert.

## <a name="offline-versus-online-migrations"></a>Offline- und Onlinemigrationen

Mit Azure Database Migration Service können Sie eine Offline- oder eine Onlinemigration durchführen. Bei Migrationen des Typs *Offline* beginnt die Ausfallzeit der Anwendung mit dem Start der Migration. Führen Sie eine Migration des Typs *Online* durch, um die Ausfallzeit auf die Zeit zu begrenzen, die bei Abschluss der Migration für die Umstellung auf die neue Umgebung erforderlich ist. Es empfiehlt sich, eine Offlinemigration zu testen, um zu ermitteln, ob die Ausfallzeit akzeptabel ist. Wenn dies nicht der Fall ist, sollten Sie eine Onlinemigration durchführen.

## <a name="migration-scenario-status"></a>Status von Migrationsszenarios

Der Status von Migrationsszenarien, die in Azure Database Migration Service unterstützt werden, kann sich im Lauf der Zeit ändern. Szenarien werden im Allgemeinen zuerst als **private Vorschau** veröffentlicht. Für die Teilnahme an der privaten Vorschau müssen Kunden über die [DMS Preview-Website](https://aka.ms/dms-preview) eine Nominierung einreichen. Nach der privaten Vorschau ändert sich der Szenariostatus in **Öffentliche Vorschau**. Benutzer von Azure Database Migration Service können die in der öffentlichen Vorschau verfügbaren Migrationsszenarien direkt über die Benutzeroberfläche testen. Eine Registrierung ist nicht erforderlich.  Allerdings sind Migrationsszenarien in der öffentlichen Vorschau möglicherweise nicht in allen Regionen verfügbar, und vor der endgültigen Veröffentlichung werden gegebenenfalls weitere Änderungen vorgenommen. Nach der öffentlichen Vorschau ändert sich der Szenariostatus in **Allgemeine Verfügbarkeit**. Die allgemeine Verfügbarkeit (General Availability, GA) ist der endgültige Veröffentlichungsstatus. Der Funktionsumfang ist vollständig und für alle Benutzer zugänglich.

## <a name="migration-scenario-support"></a>Unterstützung von Migrationsszenarios

In den folgenden Tabellen sind die Migrationsszenarien aufgeführt, die bei Verwendung von Azure Database Migration Service unterstützt werden.

> [!NOTE]
> Wenn ein Szenario, das im Folgenden als unterstützt aufgeführt wird, auf der Benutzeroberfläche aber nicht angezeigt wird, wenden Sie sich an den Alias für [Azure-Datenbankmigrationen](mailto:AskAzureDatabaseMigrations@service.microsoft.com), um weitere Informationen zu erhalten.

> [!IMPORTANT]
> Alle derzeit in Azure Database Migration Service in der privaten Vorschau unterstützten Szenarien finden Sie auf der [DMS Preview-Website](https://aka.ms/dms-preview).

### <a name="offline-one-time-migration-support"></a>Unterstützung der Offlinemigration (einmalig)

Die folgende Tabelle enthält die Azure Database Migration Service-Unterstützung für Offlinemigrationen.

| Ziel  | `Source` | Support | Status |
| ------------- | ------------- |:-------------:|:-------------:|
| **Azure SQL-Datenbank** | SQL Server | ✔ | Allgemein verfügbar |
|   | RDS SQL |  |  |
|   | Oracle |  |  |
| **Azure SQL-Datenbank MI** | SQL Server | ✔ | Allgemein verfügbar |
|   | RDS SQL |  |  |
|   | Oracle |  |   |
| **Virtueller Azure SQL-Computer** | SQL Server | ✔ | Allgemein verfügbar |
|   | Oracle |   |   |
| **Azure Cosmos DB** | MongoDB | ✔ | Allgemein verfügbar |
| **Azure-Datenbank für MySQL** | MySQL |   |   |
|   | RDS MySQL |   |   |
| **Azure-Datenbank für PostgreSQL** | PostgreSQL |  |
|  | RDS PostgreSQL |   |   |

### <a name="online-continuous-sync-migration-support"></a>Unterstützung der Onlinemigration (fortlaufende Synchronisierung)

Die folgende Tabelle enthält die Azure Database Migration Service-Unterstützung für Onlinemigrationen.

| Ziel  | `Source` | Support | Status |
| ------------- | ------------- |:-------------:|:-------------:|
| **Azure SQL-Datenbank** | SQL Server | ✔ | Allgemein verfügbar |
|   | RDS SQL | ✔ | Allgemein verfügbar |
|   | Oracle |  |  |
| **Azure SQL-Datenbank MI** | SQL Server | ✔ | Allgemein verfügbar |
|   | RDS SQL | ✔ | Allgemein verfügbar |
|   | Oracle | ✔ | Private Vorschau |
| **Virtueller Azure SQL-Computer** | SQL Server |   |   |
|   | Oracle  |  |  |
| **Azure Cosmos DB** | MongoDB | ✔ | Allgemein verfügbar |
| **Azure-Datenbank für MySQL** | MySQL | ✔ | Allgemein verfügbar |
|   | RDS MySQL | ✔ | Allgemein verfügbar |
| **Azure-Datenbank für PostgreSQL** | PostgreSQL | ✔ | Allgemein verfügbar |
|   | RDS PostgreSQL | ✔ | Allgemein verfügbar |
|   | Oracle | ✔ | Öffentliche Vorschau |

## <a name="next-steps"></a>Nächste Schritte

Eine Übersicht über Azure Database Migration Service und Informationen zur regionalen Verfügbarkeit finden Sie im Artikel [Was ist Azure Database Migration Service?](dms-overview.md).
