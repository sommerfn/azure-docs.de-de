---
title: Erstellen einer Einzeldatenbank
description: Erstellen und Abfragen einer Einzeldatenbank in Azure SQL-Datenbank über das Azure-Portal, PowerShell und Azure CLI.
services: sql-database
ms.service: sql-database
ms.subservice: single-database
ms.custom: ''
ms.devlang: ''
ms.topic: quickstart
author: sachinpMSFT
ms.author: ninarn
ms.reviewer: carlrab, sstein
ms.date: 09/09/2019
ms.openlocfilehash: 482fc3f08fb986908efcab83a82338ced36ee689
ms.sourcegitcommit: ac56ef07d86328c40fed5b5792a6a02698926c2d
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 11/08/2019
ms.locfileid: "73821005"
---
# <a name="quickstart-create-a-single-database-in-azure-sql-database-using-the-azure-portal-powershell-and-azure-cli"></a>Schnellstart: Erstellen einer Einzeldatenbank in Azure SQL-Datenbank über das Azure-Portal, PowerShell und Azure CLI

Die Erstellung einer [Einzeldatenbank](sql-database-single-database.md) ist die schnellste und einfachste Bereitstellungsoption zum Erstellen einer Datenbank in Azure SQL-Datenbank. In dieser Schnellstartanleitung wird gezeigt, wie Sie im Azure-Portal eine Einzeldatenbank erstellen und anschließend abfragen.

Wenn Sie kein Azure-Abonnement besitzen, können Sie ein [kostenloses Konto erstellen](https://azure.microsoft.com/free/). 

Melden Sie sich zum Durchführen aller Schritte in diesem Schnellstart beim [Azure-Portal](https://portal.azure.com/) an.

## <a name="create-a-single-database"></a>Erstellen einer Einzeldatenbank

Eine Einzeldatenbank kann entweder auf der bereitgestellten Computeebene oder der serverlosen Computeebene erstellt werden.

- Einer Einzeldatenbank auf der bereitgestellten Computeebene wird mit einem von zwei [Kaufmodellen](sql-database-purchase-models.md) vorab eine festgelegte Menge an Computeressourcen zugeordnet, einschließlich CPU und Arbeitsspeicher.
- Eine Einzeldatenbank auf der serverlosen Computeebene verfügt über einen Bereich von Computeressourcen (einschließlich CPU und Arbeitsspeicher), die automatisch skaliert werden, und ist nur in [vCore-basierten Kaufmodellen](sql-database-service-tiers-vcore.md) verfügbar.

Wenn Sie eine Einzeldatenbank erstellen, legen Sie auch einen [SQL-Datenbank-Server](sql-database-servers.md) für ihre Verwaltung fest und platzieren ihn in einer [Azure-Ressourcengruppe](../azure-resource-manager/resource-group-overview.md) in einer bestimmten Region.

> [!NOTE]
> In dieser Schnellstartanleitung wird das [vCore-basierte Kaufmodell](sql-database-service-tiers-vcore.md) verwendet, das [DTU-basierte Kaufmodell](sql-database-service-tiers-DTU.md) ist jedoch ebenfalls verfügbar.

So erstellen Sie eine Einzeldatenbank mit den AdventureWorksLT-Beispieldaten:

[!INCLUDE [sql-database-create-single-database](includes/sql-database-create-single-database.md)]

## <a name="query-the-database"></a>Abfragen der Datenbank

Nachdem Sie die Datenbank erstellt haben, verwenden Sie das integrierte Abfragetool im Azure-Portal, um eine Verbindung mit der Datenbank herzustellen und die Daten abzufragen.

1. Wählen Sie auf der Seite **SQL-Datenbank** für Ihre Datenbank im Menü auf der linken Seite die Option **Abfrage-Editor (Vorschau)** aus.

   ![Anmelden beim Abfrage-Editor](./media/sql-database-get-started-portal/query-editor-login.png)

2. Geben Sie Ihre Anmeldeinformationen ein, und wählen Sie **OK**.
3. Geben Sie die folgende Abfrage im Bereich **Abfrage-Editor** ein.

   ```sql
   SELECT TOP 20 pc.Name as CategoryName, p.name as ProductName
   FROM SalesLT.ProductCategory pc
   JOIN SalesLT.Product p
   ON pc.productcategoryid = p.productcategoryid;
   ```

4. Klicken Sie auf **Ausführen**, und sehen Sie sich dann die Abfrageergebnisse im Bereich **Ergebnisse** an.

   ![Ergebnisse im Abfrage-Editor](./media/sql-database-get-started-portal/query-editor-results.png)

5. Schließen Sie die Seite **Abfrage-Editor**, und klicken Sie auf **OK**, um Ihre nicht gespeicherten Änderungen zu verwerfen, wenn Sie dazu aufgefordert werden.

## <a name="clean-up-resources"></a>Bereinigen von Ressourcen

Behalten Sie die Ressourcengruppe, den Datenbankserver und die Einzeldatenbank, wenn Sie mit [Nächste Schritte](#next-steps) fortfahren möchten. Die nächsten Schritte zeigen, wie Sie mithilfe verschiedener Methoden eine Verbindung mit Ihrer Datenbank herstellen und die Datenbank abfragen.

Wenn Sie diese Ressourcen nicht mehr benötigen, können Sie sie wie folgt löschen:

1. Wählen Sie im Azure-Portal im Menü auf der linken Seite die Option **Ressourcengruppen** und dann **myResourceGroup** aus.
2. Wählen Sie auf der Seite für die Ressourcengruppe die Option **Ressourcengruppe löschen** aus.
3. Geben Sie *myResourceGroup* in das Feld ein, und wählen Sie dann **Löschen** aus.

## <a name="next-steps"></a>Nächste Schritte

- Erstellen Sie eine Firewallregel auf Serverebene, um über lokale Tools oder Remotetools eine Verbindung mit der Einzeldatenbank herstellen zu können. Weitere Informationen finden Sie unter [Erstellen einer Firewallregel auf Serverebene](sql-database-server-level-firewall-rule.md).
- Nachdem Sie eine Firewallregel auf Serverebene erstellt haben, können Sie mit verschiedenen Tools und Programmiersprachen eine [Verbindung mit Ihrer Datenbank herstellen und Abfragen ausführen](sql-database-connect-query.md).
  - [Verbinden und Abfragen mit SQL Server Management Studio (SSMS)](sql-database-connect-query-ssms.md)
  - [Verbinden und Abfragen mit Azure Data Studio](https://docs.microsoft.com/sql/azure-data-studio/quickstart-sql-database?toc=/azure/sql-database/toc.json)
- Informationen zum Erstellen einer Einzeldatenbank auf der bereitgestellten Computeebene mit der Azure CLI finden Sie unter [Azure CLI-Beispiele für Azure SQL-Datenbank](sql-database-cli-samples.md).
- Informationen zum Erstellen einer Einzeldatenbank auf der bereitgestellten Computeebene mit Azure PowerShell finden Sie unter [Azure PowerShell-Beispiele für Azure SQL-Datenbank](sql-database-powershell-samples.md).
- Informationen zum Erstellen einer Einzeldatenbank auf der serverlosen Computeebene mit Azure PowerShell finden Sie unter [Erstellen einer serverlosen Datenbank](sql-database-serverless.md#create-new-database-in-serverless-compute-tier).
