---
title: Azure-Hybridvorteil
description: Verwenden Sie vorhandene SQL Server-Lizenzen für SQL-Datenbank-Rabatte.
services: sql-database
ms.service: sql-database
ms.subservice: service
ms.topic: conceptual
author: stevestein
ms.author: sstein
ms.reviewer: sashan, moslake, carlrab
ms.date: 10/08/2019
ms.openlocfilehash: 17252f6544ee56647315dc44ace4db3e3abe9726
ms.sourcegitcommit: ac56ef07d86328c40fed5b5792a6a02698926c2d
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 11/08/2019
ms.locfileid: "73821872"
---
# <a name="azure-hybrid-benefit"></a>Azure-Hybridvorteil

Bei der bereitgestellten Computeebene des V-Kern-basierten Kaufmodells können Sie Ihre vorhandenen Lizenzen gegen Rabattpreise für SQL-Datenbank tauschen, indem Sie den [Azure-Hybridvorteil für SQL Server](https://azure.microsoft.com/pricing/hybrid-benefit/) verwenden. Mit diesem Azure-Vorteil können Sie bei Azure SQL-Datenbank bis zu 30 Prozent sparen, indem Sie Ihre lokalen SQL Server-Lizenzen mit Software Assurance verwenden.

![Preise](./media/sql-database-service-tiers/pricing.png)


## <a name="choose-a-license-model"></a>Auswählen eines Lizenzmodells

Mit dem Azure-Hybridvorteil können Sie wahlweise nur die zugrunde liegende Azure-Infrastruktur bezahlen und Ihre vorhandene SQL Server-Lizenz für die eigentliche SQL-Datenbank-Engine verwenden (Grundpreis für Computeleistung), oder Sie bezahlen für die zugrunde liegende Infrastruktur sowie für die SQL Server-Lizenz (Preismodell mit enthaltener Lizenz).

Sie können Ihr Lizenzierungsmodell im Azure-Portal oder mithilfe einer der folgenden APIs auswählen oder ändern:

- Festlegen oder Aktualisieren des Lizenztyps mithilfe von PowerShell:

  - [New-AzSqlDatabase](https://docs.microsoft.com/powershell/module/az.sql/new-azsqldatabase)
  - [Set-AzSqlDatabase](https://docs.microsoft.com/powershell/module/az.sql/set-azsqldatabase)
  - [New-AzSqlInstance](https://docs.microsoft.com/powershell/module/az.sql/new-azsqlinstance)
  - [Set-AzSqlInstance](https://docs.microsoft.com/powershell/module/az.sql/set-azsqlinstance)

- Festlegen oder Aktualisieren des Lizenztyps mithilfe der Azure-Befehlszeilenschnittstelle:

  - [az sql db create](https://docs.microsoft.com/cli/azure/sql/db#az-sql-db-create)
  - [az sql db update](https://docs.microsoft.com/cli/azure/sql/db#az-sql-db-update)
  - [az sql mi create](https://docs.microsoft.com/cli/azure/sql/mi#az-sql-mi-create)
  - [az sql mi update](https://docs.microsoft.com/cli/azure/sql/mi#az-sql-mi-update)

- Festlegen oder Aktualisieren des Lizenztyps mithilfe der REST-API:

  - [Datenbanken – Erstellen oder Aktualisieren](https://docs.microsoft.com/rest/api/sql/databases/createorupdate)
  - [Datenbanken – Aktualisieren](https://docs.microsoft.com/rest/api/sql/databases/update)
  - [Verwaltete Instanzen – Erstellen oder Aktualisieren](https://docs.microsoft.com/rest/api/sql/managedinstances/createorupdate)
  - [Verwaltete Instanzen – Aktualisieren](https://docs.microsoft.com/rest/api/sql/managedinstances/update)



## <a name="next-steps"></a>Nächste Schritte

- Informationen zur Auswahl zwischen den Bereitstellungsoptionen für Azure SQL-Datenbank finden Sie unter [Auswählen der richtigen Bereitstellungsoption in Azure SQL](sql-database-paas-vs-sql-server-iaas.md).
- Einen Vergleich der Funktionen von Azure SQL-Datenbank finden Sie unter [Azure SQL-Datenbank – Funktionen](sql-database-features.md).
