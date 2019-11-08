---
title: Wiederherstellen einer vorhandenen Data Warehouse-Instanz
description: Anleitung zum Wiederherstellen einer vorhandenen Azure SQL Data Warehouse-Instanz.
services: sql-data-warehouse
author: anumjs
manager: craigg
ms.service: sql-data-warehouse
ms.topic: conceptual
ms.subservice: manage
ms.date: 08/29/2018
ms.author: anjangsh
ms.reviewer: igorstan
ms.custom: seo-lt-2019
ms.openlocfilehash: a73658510111df44c522d88ed5eceb7dcfa80d0d
ms.sourcegitcommit: 609d4bdb0467fd0af40e14a86eb40b9d03669ea1
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 11/06/2019
ms.locfileid: "73685535"
---
# <a name="restore-an-existing-azure-sql-data-warehouse"></a>Wiederherstellen einer vorhandenen Azure SQL Data Warehouse-Instanz

In diesem Artikel erfahren Sie, wie Sie eine vorhandene SQL Data Warehouse-Instanz mit Azure-Portal und PowerShell wiederherstellen:

[!INCLUDE [updated-for-az](../../includes/updated-for-az.md)]

**Überprüfen Sie Ihre DTU-Kapazität.** Jede SQL Data Warehouse-Instanz wird von einer SQL Server-Instanz gehostet (z.B. „myserver.database.windows.net“), die über ein Standard-DTU-Kontingent verfügt. Vergewissern Sie sich, dass die SQL Server-Instanz über ein ausreichendes DTU-Kontingent für die Datenbankwiederherstellung verfügt. Informationen zum Berechnen des DTU-Bedarfs bzw. zur Anforderung weiterer DTUs finden Sie unter [Anfordern einer DTU-Kontingentänderung][Request a DTU quota change].

## <a name="before-you-begin"></a>Voraussetzungen

1. Bevor Sie beginnen, müssen Sie [Azure PowerShell installieren][Install Azure PowerShell].
2. Sie benötigen einen vorhandenen Wiederherstellungspunkt. Wenn Sie eine neue Wiederherstellung erstellen möchten, lesen Sie [das Tutorial zum Erstellen eines neuen benutzerdefinierten Wiederherstellungspunkts][the tutorial to create a new user-defined restore point].

## <a name="restore-an-existing-data-warehouse-through-powershell"></a>Wiederherstellen einer vorhandenen Data Warehouse-Instanz mit PowerShell

Verwenden Sie das PowerShell-Cmdlet [Restore-AzSqlDatabase][Restore-AzSqlDatabase], um eine vorhandene Data Warehouse-Instanz auf der Grundlage eines Wiederherstellungspunkts wiederherzustellen.

1. Öffnen Sie PowerShell.

2. Stellen Sie eine Verbindung mit Ihrem Azure-Konto her, und listen Sie alle Abonnements auf, die Ihrem Konto zugeordnet sind.

3. Wählen Sie das Abonnement aus, in dem die wiederherzustellende Datenbank enthalten ist.

4. Listen Sie die Wiederherstellungspunkte für die Data Warehouse-Instanz auf.

5. Wählen Sie den gewünschten Wiederherstellungspunkt mit RestorePointCreationDate aus.

6. Stellen Sie die Data Warehouse-Instanz auf der Grundlage des gewünschten Wiederherstellungspunkts mit dem PowerShell-Cmdlet [Restore-AzSqlDatabase][Restore-AzSqlDatabase] wieder her.
        1. Um die SQL Data Warehouse-Instanz auf einem anderen logischen Server wiederherzustellen, stellen Sie sicher, dass Sie den Namen des anderen logischen Servers angeben.  Dieser logische Server kann sich auch in einer anderen Ressourcengruppe und Region befinden.
        2. Zum Wiederherstellen in einem anderen Abonnement verwenden Sie die Schaltfläche „Verschieben“, um den logischen Server in ein anderes Abonnement zu verschieben.

7. Überprüfen Sie, ob die wiederhergestellte Data Warehouse-Instanz online ist.

8. Nach Abschluss der Wiederherstellung können Sie Ihre wiederhergestellte Data Warehouse-Instanz konfigurieren. Befolgen Sie hierzu die Anleitung [Konfigurieren der Datenbank nach der Wiederherstellung][Configure your database after recovery].

```Powershell

$SubscriptionName="<YourSubscriptionName>"
$ResourceGroupName="<YourResourceGroupName>"
$ServerName="<YourServerNameWithoutURLSuffixSeeNote>"  # Without database.windows.net
#$TargetResourceGroupName="<YourTargetResourceGroupName>" # uncomment to restore to a different logical server.
#$TargetServerName="<YourtargetServerNameWithoutURLSuffixSeeNote>"  
$DatabaseName="<YourDatabaseName>"
$NewDatabaseName="<YourDatabaseName>"

Connect-AzAccount
Get-AzSubscription
Select-AzSubscription -SubscriptionName $SubscriptionName

# Or list all restore points
Get-AzSqlDatabaseRestorePoints -ResourceGroupName $ResourceGroupName -ServerName $ServerName -DatabaseName $DatabaseName

# Get the specific database to restore
$Database = Get-AzSqlDatabase -ResourceGroupName $ResourceGroupName -ServerName $ServerName -DatabaseName $DatabaseName

# Pick desired restore point using RestorePointCreationDate "xx/xx/xxxx xx:xx:xx xx"
$PointInTime="<RestorePointCreationDate>"  

# Restore database from a restore point
$RestoredDatabase = Restore-AzSqlDatabase –FromPointInTimeBackup –PointInTime $PointInTime -ResourceGroupName $Database.ResourceGroupName -ServerName $Database.ServerName -TargetDatabaseName $NewDatabaseName –ResourceId $Database.ResourceID

# Use the following command to restore to a different logical server
#$RestoredDatabase = Restore-AzSqlDatabase –FromPointInTimeBackup –PointInTime $PointInTime -ResourceGroupName $Database.ResourceTargetGroupName -ServerName $TargetServerName -TargetDatabaseName $NewDatabaseName –ResourceId $Database.ResourceID

# Verify the status of restored database
$RestoredDatabase.status

```

## <a name="restore-an-existing-data-warehouse-through-the-azure-portal"></a>Wiederherstellen einer vorhandenen Data Warehouse-Instanz mit dem Azure-Portal

1. Melden Sie sich beim [Azure-Portal][Azure portal] an.
2. Navigieren Sie zu der SQL Data Warehouse-Instanz, mit der Sie die Wiederherstellung durchführen möchten.
3. Wählen Sie oben im Blatt „Übersicht“ die Option **Wiederherstellen** aus.

    ![ Wiederherstellung – Übersicht](./media/sql-data-warehouse-restore-active-paused-dw/restoring-01.png)

4. Wählen Sie entweder die Option **Automatische Wiederherstellungspunkte** oder **Benutzerdefinierte Wiederherstellungspunkte** aus. Wenn die Data Warehouse-Instanz keine automatischen Wiederherstellungspunkte umfasst, warten Sie einige Stunden, oder erstellen Sie einen benutzerdefinierten Wiederherstellungspunkt, bevor Sie eine Wiederherstellung durchführen. Wählen Sie für „Benutzerdefinierte Wiederherstellungspunkte“ einen vorhandenen benutzerdefinierten Wiederherstellungspunkt aus, oder erstellen Sie einen neuen. Für **Server** können Sie einen logischen Server in einer anderen Ressourcengruppe und Region auswählen oder einen neuen erstellen. Nachdem Sie alle Parameter angegeben haben, klicken Sie auf **Überprüfen + wiederherstellen**.

    ![Automatische Wiederherstellungspunkte](./media/sql-data-warehouse-restore-active-paused-dw/restoring-11.png)

## <a name="next-steps"></a>Nächste Schritte
- [Wiederherstellen einer gelöschten Azure SQL Data Warehouse-Instanz][Restore a deleted data warehouse]
- [Geowiederherstellung einer Azure SQL Data Warehouse-Instanz][Restore from a geo-backup data warehouse]
 
<!--Image references-->

<!--Article references-->
[Azure SQL Database business continuity overview]: ../sql-database/sql-database-business-continuity.md
[Request a DTU quota change]: ./sql-data-warehouse-get-started-create-support-ticket.md
[Configure your database after recovery]: ../sql-database/sql-database-disaster-recovery.md#configure-your-database-after-recovery
[How to install and configure Azure PowerShell]: /powershell/azureps-cmdlets-docs
[Overview]: ./sql-data-warehouse-restore-database-overview.md
[Portal]: ./sql-data-warehouse-restore-database-portal.md
[PowerShell]: ./sql-data-warehouse-restore-database-powershell.md
[REST]: ./sql-data-warehouse-restore-database-rest-api.md
[Configure your database after recovery]: ../sql-database/sql-database-disaster-recovery.md#configure-your-database-after-recovery
[the tutorial to create a new user-defined restore point]:../sql-data-warehouse/sql-data-warehouse-restore-points.md
[Install Azure PowerShell]: https://docs.microsoft.com/powershell/azure/overview
[Restore an existing data warehouse]:./sql-data-warehouse-restore-active-paused-dw.md
[Restore a deleted data warehouse]:./sql-data-warehouse-restore-deleted-dw.md
[Restore from a geo-backup data warehouse]:./sql-data-warehouse-restore-from-geo-backup.md

<!--MSDN references-->
[Restore-AzSqlDatabase]: https://docs.microsoft.com/powershell/module/az.sql/restore-azsqldatabase

<!--Other Web references-->
[Azure Portal]: https://portal.azure.com/
