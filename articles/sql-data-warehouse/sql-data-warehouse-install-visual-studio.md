---
title: Installieren von Visual Studio 2019 für SQL Data Warehouse | Microsoft-Dokumentation
description: Installieren von Visual Studio und SQL Server Data Tools (SSDT) für Azure SQL Data Warehouse
services: sql-data-warehouse
ms.custom: vs-azure
ms.workload: azure-vs
author: kevinvngo
manager: craigg
ms.service: sql-data-warehouse
ms.topic: conceptual
ms.subservice: development
ms.date: 10/17/2019
ms.author: kevin
ms.reviewer: igorstan
ms.openlocfilehash: 81c709e7705e16484438ab684a6b1591e5e624ba
ms.sourcegitcommit: ae461c90cada1231f496bf442ee0c4dcdb6396bc
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 10/17/2019
ms.locfileid: "72553517"
---
# <a name="getting-started-with-visual-studio-2019-for-sql-data-warehouse"></a>Erste Schritte mit Visual Studio 2019 für SQL Data Warehouse
SQL Server Data Tools (SSDT) von Visual Studio **2019** ist ein einzelnes Tool, mit dem Sie folgende Aufgaben ausführen können:

- Verbinden, Abfragen und Entwickeln von Anwendungen für SQL Data Warehouse 
- Nutzen eines Objekt-Explorers, um alle Objekte im Datenmodell visuell zu untersuchen, einschließlich Tabellen, Ansichten, gespeicherten Prozeduren usw.
- Generieren von T-SQL-DDL-Skripts (Data Definition Language) für die Objekte
- Entwickeln Ihres Data Warehouse mithilfe eines zustandsbasierten Ansatzes für SSDT-Datenbankprojekte
- Integrieren Ihres Datenbankprojekts in Quellcodeverwaltungssysteme wie Git mit Azure DevOps-Repositorys
- Einrichten von Pipelines für Continuous Integration und Continuous Deployment mit Automatisierungsservern wie Azure DevOps

> [!NOTE]
> Zurzeit befinden sich SSDT-Datenbankprojekte für Visual Studio in der Vorschauphase. Um regelmäßig Aktualisierungen zu dieser Funktion zu erhalten, stimmen Sie auf [UserVoice] ab.

## <a name="install-visual-studio-2019-preview"></a>Installieren der Vorschauversion von Visual Studio 2019
Informationen zum Herunterladen und Installieren von Visual Studio **16.3 und höher** finden Sie unter [Visual Studio 2019 herunterladen][]. Wählen Sie während der Installation die Workload „Datenspeicherung und -verarbeitung“ aus. Eine eigenständige SSDT-Installation ist in Visual Studio 2019 nicht mehr erforderlich.

## <a name="reporting-issues-with-ssdt-visual-studio-2019-preview"></a>Melden von Problemen mit SSDT Visual Studio 2019 (Vorschau)
Zum Melden von Problemen bei der Verwendung von SSDT mit SQL Data Warehouse senden Sie eine E-Mail an die folgende E-Mail-Verteilerliste: <sqldwssdtpreview@service.microsoft.com>

## <a name="next-steps"></a>Nächste Schritte
Da Sie jetzt die neueste Version von SSDT verwenden, sind Sie bereit für die [Verbindungsherstellung][connect] mit SQL Data Warehouse.

<!--Anchors-->

<!--Image references-->

<!--Articles-->
[connect]: ./sql-data-warehouse-query-visual-studio.md

<!--Other-->
[Visual Studio 2019 herunterladen]: https://visualstudio.microsoft.com/downloads/
[Installing Visual Studio]: https://msdn.microsoft.com/library/e2h7fzkw.aspx
[SSDT Download]: https://msdn.microsoft.com/library/mt204009.aspx
[UserVoice]: https://feedback.azure.com/forums/307516-sql-data-warehouse/suggestions/13313247-database-project-from-visual-studio-to-support-azu
