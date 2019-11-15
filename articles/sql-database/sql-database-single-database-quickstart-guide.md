---
title: 'Schnellstart: Einzeldatenbanken'
description: Hier finden Sie eine schnelle Einführung in die Verwendung von Einzeldatenbanken in Azure SQL-Datenbank.
services: sql-database
ms.service: sql-database
ms.subservice: single-database
ms.custom: ''
ms.devlang: ''
ms.topic: quickstart
author: jovanpop-msft
ms.author: jovanpop
ms.reviewer: carlr
ms.date: 07/29/2019
ms.openlocfilehash: 4332d57c8d5587a34ba3a593deb9bd635132b03d
ms.sourcegitcommit: ac56ef07d86328c40fed5b5792a6a02698926c2d
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 11/08/2019
ms.locfileid: "73820951"
---
# <a name="getting-started-with-single-databases-in-azure-sql-database"></a>Erste Schritte mit Einzeldatenbanken in Azure SQL-Datenbank

Eine [Einzeldatenbank](sql-database-single-index.yml) ist eine vollständig verwaltete PaaS-Datenbank als Dienst (Database-as-a-Service, DbaaS) und eine ideale Speicher-Engine für moderne cloudbasierte Anwendungen. In diesem Abschnitt erfahren Sie, wie Sie schnell eine Einzeldatenbank in SQL-Datenbank konfigurieren und erstellen.

## <a name="quickstart-overview"></a>Schnellstartübersicht

Dieser Abschnitt enthält eine Übersicht über die verfügbaren Artikel für den schnellen Einstieg in die Verwendung von Einzeldatenbanken. Die folgenden Schnellstartanleitungen unterstützen Sie beim schnellen Erstellen einer Einzeldatenbank, beim Konfigurieren einer Firewallregel für den Datenbankserver und beim anschließenden Importieren einer Datenbank in die neue Einzeldatenbank mithilfe einer Datei vom Typ `.bacpac`:

- [Quickstart: Create a single database in Azure SQL Database using the Azure portal](sql-database-single-database-get-started.md) (Schnellstart: Erstellen einer Einzeldatenbank in Azure SQL-Datenbank über das Azure-Portal)
- Nach der Erstellung der Datenbank müssen Sie [Ihre Datenbank durch Konfigurieren von Firewallregeln schützen](sql-database-server-level-firewall-rule.md).
- Falls Sie über eine vorhandene Datenbank in SQL Server verfügen, die Sie zu Azure migrieren möchten, sollten Sie den [Datenmigrations-Assistenten (DMA)](https://www.microsoft.com/download/details.aspx?id=53595) installieren. Dieser analysiert Ihre Datenbanken in SQL Server und ermittelt sämtliche Probleme, die die Migration zur Bereitstellungsoption für Einzeldatenbanken verhindern könnten. Wurden keine Probleme gefunden, können Sie Ihre Datenbank als Datei vom Typ `.bacpac` exportieren und [über das Azure-Portal oder mithilfe von „SqlPackage“ importieren](sql-database-import.md).

## <a name="automating-management-operations"></a>Automatisieren von Verwaltungsvorgängen

Sie können PowerShell oder die Azure-Befehlszeilenschnittstelle verwenden, um Ihre Datenbank zu erstellen, zu konfigurieren und zu skalieren.

- [Verwenden von PowerShell zum Erstellen einer einzelnen Azure SQL-Datenbank und Konfigurieren einer Firewallregel für einen SQL-Datenbankserver](scripts/sql-database-create-and-configure-database-powershell.md)
- [Verwenden der CLI zum Erstellen einer einzelnen Azure SQL-Datenbank und Konfigurieren einer Firewallregel](scripts/sql-database-create-and-configure-database-cli.md)
- [Überwachen und Skalieren einer einzelnen SQL­-Datenbank mit PowerShell](scripts/sql-database-monitor-and-scale-database-powershell.md)
- [Überwachen und Skalieren einer einzelnen SQL­-Datenbank über die Befehlszeilenschnittstelle](scripts/sql-database-monitor-and-scale-database-cli.md)

## <a name="migrating-to-a-single-database-with-minimal-downtime"></a>Migrieren zu einer Einzeldatenbank mit minimaler Downtime

In diesen Schnellstartanleitungen erfahren Sie, wie Sie Ihre Datenbank schnell erstellen oder unter Verwendung einer Datei vom Typ `.bacpac` in Azure importieren. Dateien vom Typ `.bacpac` und `.dacpac` dienen dazu, Datenbanken schnell zwischen verschiedenen Versionen von SQL Server und Bereitstellungsoptionen in Azure SQL-Datenbank zu migrieren oder Continuous Integration in Ihrer DevOps-Pipeline zu implementieren. Diese Methode ist jedoch nicht für die Migration Ihrer Produktionsdatenbanken mit minimaler Downtime geeignet, da Sie vorerst keine neuen Daten hinzufügen dürfen und warten müssen, bis der Export der Quelldatenbank in eine Datei vom Typ `.bacpac` und der anschließende Import in Azure SQL-Datenbank abgeschlossen ist. Diese Wartezeit führt zu Ausfallzeiten für Ihre Anwendung, insbesondere bei großen Datenbanken. Für die Verschiebung Ihrer Produktionsdatenbank benötigen Sie eine bessere Migrationsmethode mit möglichst geringer Downtime bei der Migration. Verwenden Sie dafür [Data Migration Service (DMS)](https://docs.microsoft.com/azure/dms/tutorial-sql-server-to-azure-sql?toc=/azure/sql-database/toc.json), um Ihre Datenbank mit minimaler Downtime zu migrieren. DMS pusht die in Ihrer Quelldatenbank vorgenommenen Änderungen inkrementell in die Einzeldatenbank, die wiederhergestellt wird. Dadurch können Sie Ihre Anwendung schnell und mit minimaler Downtime von der Quell- auf die Zieldatenbank umstellen.

## <a name="hands-on-learning-modules"></a>Praxisorientierte Lernmodule

In den folgenden Microsoft Learn-Modulen können Sie sich kostenlos über Azure SQL-Datenbank informieren.

- [Bereitstellen einer Azure SQL-Datenbank zum Speichern von Anwendungsdaten](https://docs.microsoft.com/learn/modules/provision-azure-sql-db/)
- [Entwickeln und Konfigurieren einer ASP.NET-Anwendung zum Abfragen einer Azure SQL-Datenbank-Instanz](https://docs.microsoft.com/learn/modules/develop-app-that-queries-azure-sql/)
- [Schützen von Azure SQL-Datenbank](https://docs.microsoft.com/learn/modules/secure-your-azure-sql-database/)

## <a name="next-steps"></a>Nächste Schritte

- Machen Sie sich mit einer [allgemeinen Liste unterstützter Features in Azure SQL-Datenbank](sql-database-features.md) vertraut.
- Informieren Sie sich darüber, wie Sie [Ihre Datenbank sicherer machen](sql-database-security-tutorial.md).
- Weiterführende Schrittanleitungen finden Sie unter [Verwenden eines Singleton in Azure SQL-Datenbank](sql-database-howto-single-database.md).
- Sehen Sie sich weitere Beispielskripts für [PowerShell](sql-database-powershell-samples.md) und die [Azure-Befehlszeilenschnittstelle](sql-database-cli-samples.md) an.
- Informieren Sie sich ausführlicher über die [Verwaltungs-API](sql-database-single-databases-manage.md), die Sie zum Konfigurieren Ihrer Datenbanken verwenden können.
- [Identifizieren der richtigen Azure SQL Datenbank-SKU/Verwaltete-Instanz-SKU für Ihre lokale Datenbank](/sql/dma/dma-sku-recommend-sql-db/).
