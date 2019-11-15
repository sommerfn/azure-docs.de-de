---
title: Verschieben von Ressourcen in eine andere Region
description: Erfahren Sie, wie Sie Ihre Azure SQL-Datenbank, Ihren Pool für elastische Azure SQL-Datenbanken oder eine verwaltete Azure SQL-Instanz in eine andere Region verschieben.
services: sql-database
ms.service: sql-database
ms.subservice: data-movement
ms.custom: ''
ms.devlang: ''
ms.topic: conceptual
author: MashaMSFT
ms.author: mathoma
ms.reviewer: carlrab
ms.date: 06/25/2019
ms.openlocfilehash: 851ef49a5c066f12a95baa54daf5e267cb4278c5
ms.sourcegitcommit: ac56ef07d86328c40fed5b5792a6a02698926c2d
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 11/08/2019
ms.locfileid: "73821436"
---
# <a name="how-to-move-azure-sql-resources-to-another-region"></a>Verschieben von Azure SQL-Ressourcen in eine andere Region

In diesem Artikel finden Sie einen allgemeinen Workflow zum Verschieben einer Azure SQL-Einzeldatenbank, einen Pool für elastische Azure SQL-Datenbanken oder eine verwaltete Azure SQL-Instanz in eine andere Region verschieben. 

## <a name="overview"></a>Übersicht

Es gibt verschiedene Szenarien, in denen Sie Ihre vorhandenen virtuellen Azure-Ressourcen aus einer Region in eine andere verschieben möchten. Beispielsweise erweitern Sie Ihr Unternehmen in eine neue Region und wollen es für den neuen Kundenstamm optimieren. Oder Sie müssen die Vorgänge aus Compliancegründen in eine andere Region verlegen. Oder Azure hat eine brandneue Region freigegeben, die eine bessere Nähe bietet und die Kundenzufriedenheit verbessert.  

Dieser Artikel bietet einen allgemeinen Workflow zum Verschieben von Ressourcen in eine andere Region. Der Workflow umfasst folgende Schritte: 

- Überprüfen der Voraussetzungen für das Verschieben 
- Vorbereiten auf das Verschieben der Ressourcen im Gültigkeitsbereich
- Überwachen des Vorbereitungsprozesses
- Testen des Verschiebungsprozesses
- Initiieren der tatsächlichen Verschiebung 
- Entfernen der Ressourcen aus der Quellregion 


> [!NOTE]
> Dieser Artikel gilt für Migrationen innerhalb der öffentlichen Azure-Cloud oder innerhalb derselben Sovereign Cloud. 


[!INCLUDE [updated-for-az](../../includes/updated-for-az.md)]

## <a name="move-single-database"></a>Verschieben einer Einzeldatenbank

### <a name="verify-prerequisites"></a>Überprüfen der Voraussetzungen 

1. Erstellen Sie einen logischen Zielserver für jeden Quellserver. 
1. Konfigurieren Sie die Firewall mit den richtigen Ausnahmen mithilfe von [PowerShell](scripts/sql-database-create-and-configure-database-powershell.md).  
1. Konfigurieren Sie die logischen Server mit den richtigen Anmeldedaten. Wenn Sie nicht der Abonnement- oder SQL Server-Administrator sind, wenden Sie sich an den Administrator, damit dieser Ihnen die erforderlichen Berechtigungen zuweist. Weitere Informationen finden Sie unter [Verwalten der Sicherheit der Azure SQL-Datenbank nach der Notfallwiederherstellung](sql-database-geo-replication-security-config.md). 
1. Wenn Ihre Datenbanken mit TDE verschlüsselt sind und Sie Ihren eigenen Schlüssel im Azure Key Vault verwenden, stellen Sie sicher, dass das richtige Verschlüsselungsmaterial in den Zielregionen bereitgestellt wird. Weitere Informationen finden Sie unter [Azure SQL Transparent Data Encryption mithilfe von Schlüsseln, die vom Kunden in Azure Key Vault verwaltet werden](transparent-data-encryption-byok-azure-sql.md).
1. Wenn die Überwachung auf Datenbankebene aktiviert ist, deaktivieren Sie sie, und aktivieren Sie stattdessen die Überwachung auf Serverebene. Nach dem Failover ist für die Überwachung auf Datenbankebene überregionaler Datenverkehr erforderlich. Dies ist nach der Verschiebung jedoch nicht gewünscht oder nicht mehr möglich. 
1. Stellen Sie für Überwachung auf Serverebene Folgendes sicher:
   - Der Speichercontainer, Log Analytics oder der Event Hub mit den vorhandenen Überwachungsprotokollen wird in die Zielregion verschoben. 
   - Die Überwachung ist auf dem Zielserver konfiguriert. Weitere Informationen finden Sie unter [Erste Schritte mit der SQL-Datenbanküberwachung](sql-database-auditing.md). 
1. Wenn für Ihre Instanz eine Richtlinie für die Langzeitaufbewahrung (LTR) vorhanden ist, bleiben die vorhandenen LTR-Sicherungen dem aktuellen Server zugeordnet. Da ein anderer Zielserver verwendet wird, können Sie über den Quellserver auf die älteren LTR-Sicherungen in der Quellregion zugreifen, auch wenn der Server gelöscht wird. 

  > [!NOTE]
  > Dies reicht für das Verschieben zwischen der Sovereign Cloud und einer öffentlichen Region nicht aus. Für eine solche Migration müssen die LTR-Sicherungen auf den Zielserver verschoben werden. Dies wird derzeit nicht unterstützt. 

### <a name="prepare-resources"></a>Vorbereiten der Ressourcen

1. Erstellen Sie eine [Failovergruppe](sql-database-single-database-failover-group-tutorial.md#2---create-the-failover-group) zwischen dem logischen Server der Quelle und dem logischen Server des Ziels.  
1. Fügen Sie die Datenbanken hinzu, die Sie in die Failovergruppe verschieben möchten. 
    - Die Replikation aller hinzugefügten Datenbanken wird automatisch initiiert. Weitere Informationen finden Sie unter [Bewährte Methoden für die Verwendung von Failovergruppen für Einzeldatenbanken](sql-database-auto-failover-group.md#best-practices-of-using-failover-groups-with-single-databases-and-elastic-pools). 
 
### <a name="monitor-the-preparation-process"></a>Überwachen des Vorbereitungsprozesses

Sie können in regelmäßigen Abständen [Get-AzSqlDatabaseFailoverGroup](/powershell/module/az.sql/get-azsqldatabasefailovergroup) aufrufen, um die Replikation Ihrer Datenbanken von der Quelle zum Ziel zu überwachen. Das Ausgabeprojekt von `Get-AzSqlDatabaseFailoverGroup` umfasst eine Eigenschaft für den **ReplicationState**: 
   - **ReplicationState = 2** (CATCH_UP) gibt an, dass die Datenbank synchronisiert ist und das Failover sicher ausgeführt werden kann. 
   - **ReplicationState = 0** (SEEDING) gibt an, dass das Seeding für die Datenbank noch nicht abgeschlossen ist und bei einem Failover ein Fehler auftreten würde. 

### <a name="test-synchronization"></a>Testen der Synchronisierung

Sobald **ReplicationState** `2` ist, verbinden Sie sich mit der jeweiligen Datenbank oder einer Teilmenge von Datenbanken unter Verwendung des sekundären Endpunkts `<fog-name>.secondary.database.windows.net`, und führen Sie jede Abfrage an den Datenbanken durch, um die Konnektivität, die richtige Sicherheitskonfiguration und die Datenreplikation sicherzustellen. 

### <a name="initiate-the-move"></a>Initiieren der Verschiebung

1. Stellen Sie über den sekundären Endpunkt `<fog-name>.secondary.database.windows.net` eine Verbindung zum Zielserver her.
1. Verwenden Sie [Switch-AzSqlDatabaseFailoverGroup](/powershell/module/az.sql/switch-azsqldatabasefailovergroup), um die sekundäre verwaltete Instanz auf die primäre mit vollständiger Synchronisation umzuschalten. Dieser Vorgang wird entweder erfolgreich ausgeführt, oder es wird ein Rollback ausgeführt. 
1. Vergewissern Sie sich, dass der Befehl erfolgreich abgeschlossen wurde, indem Sie `nslook up <fog-name>.secondary.database.windows.net` verwenden, um sicherzustellen, dass der DNS CNAME-Eintrag auf die IP-Adresse der Zielregion verweist. Wenn der Switch-Befehl fehlschlägt, wird CNAME nicht aktualisiert. 

### <a name="remove-the-source-databases"></a>Entfernen von Quelldatenbanken

Nachdem der Verschiebe Vorgang abgeschlossen ist, entfernen Sie die Ressourcen in der Quellregion, um unnötige Gebühren zu vermeiden. 

1. Löschen Sie die Failovergruppe mit [Remove-AzSqlDatabaseFailoverGroup](/powershell/module/az.sql/remove-azsqldatabasefailovergroup). 
1. Löschen Sie jede Quelldatenbank mit [Remove-AzSqlDatabase](/powershell/module/az.sql/remove-azsqldatabase) für jede der Datenbanken auf dem Quellserver. Dadurch werden die Links für die Georeplikation automatisch beendet. 
1. Löschen Sie den Quellserver mit [Remove-AzSqlServer](/powershell/module/az.sql/remove-azsqlserver). 
1. Entfernen Sie den Schlüsselspeicher, den Überwachungsspeichercontainer, den Event Hub, die AAD-Instanz und andere abhängige Ressourcen, damit Ihnen diese nicht mehr in Rechnung gestellt werden. 

## <a name="move-elastic-pools"></a>Verschieben der Pools für elastische Datenbanken

### <a name="verify-prerequisites"></a>Überprüfen der Voraussetzungen 

1. Erstellen Sie einen logischen Zielserver für jeden Quellserver. 
1. Konfigurieren Sie die Firewall mit den richtigen Ausnahmen mithilfe von [PowerShell](scripts/sql-database-create-and-configure-database-powershell.md). 
1. Konfigurieren Sie die logischen Server mit den richtigen Anmeldedaten. Wenn Sie nicht der Abonnement- oder SQL Server-Administrator sind, wenden Sie sich an den Administrator, damit dieser Ihnen die erforderlichen Berechtigungen zuweist. Weitere Informationen finden Sie unter [Verwalten der Sicherheit der Azure SQL-Datenbank nach der Notfallwiederherstellung](sql-database-geo-replication-security-config.md). 
1. Wenn Ihre Datenbanken mit TDE verschlüsselt sind und Sie Ihren eigenen Schlüssel im Azure Key Vault verwenden, stellen Sie sicher, dass das richtige Verschlüsselungsmaterial in der Zielregion bereitgestellt wird.
1. Erstellen Sie für jeden Quellpool für elastische Datenbanken einen Zielpool, und stellen Sie sicher, dass der Pool in der gleichen Serviceebene, mit dem gleichen Namen und der gleichen Größe angelegt wird. 
1. Wenn die Überwachung auf Datenbankebene aktiviert ist, deaktivieren Sie sie, und aktivieren Sie stattdessen die Überwachung auf Serverebene. Nach dem Failover ist für die Überwachung auf Datenbankebene überregionaler Datenverkehr erforderlich. Dies ist nach der Verschiebung jedoch nicht gewünscht oder nicht mehr möglich. 
1. Stellen Sie für Überwachung auf Serverebene Folgendes sicher:
    - Der Speichercontainer, Log Analytics oder der Event Hub mit den vorhandenen Überwachungsprotokollen wird in die Zielregion verschoben.
    - Die Überwachungskonfiguration ist auf dem Zielserver konfiguriert. Weitere Informationen finden Sie unter [SQL-Datenbanküberwachung](sql-database-auditing.md).
1. Wenn für Ihre Instanz eine Richtlinie für die Langzeitaufbewahrung (LTR) vorhanden ist, bleiben die vorhandenen LTR-Sicherungen dem aktuellen Server zugeordnet. Da ein anderer Zielserver verwendet wird, können Sie über den Quellserver auf die älteren LTR-Sicherungen in der Quellregion zugreifen, auch wenn der Server gelöscht wird. 

  > [!NOTE]
  > Dies reicht für das Verschieben zwischen der Sovereign Cloud und einer öffentlichen Region nicht aus. Für eine solche Migration müssen die LTR-Sicherungen auf den Zielserver verschoben werden. Dies wird derzeit nicht unterstützt. 

### <a name="prepare-to-move"></a>Vorbereiten der Verschiebung
 
1.  Erstellen Sie eine separate [Failovergruppe](sql-database-elastic-pool-failover-group-tutorial.md#3---create-the-failover-group) zwischen den Pools für elastische Datenbanken auf dem logischen Quellserver und dem jeweiligen Gegenstück auf dem Zielserver. 
1.  Fügen Sie alle Datenbanken im Pool zur Failovergruppe hinzu. 
    - Die Replikation der hinzugefügten Datenbanken wird automatisch initiiert. Weitere Informationen finden Sie unter [Bewährte Methoden für die Verwendung von Failovergruppen für Pools für elastische Datenbanken](sql-database-auto-failover-group.md#best-practices-of-using-failover-groups-with-single-databases-and-elastic-pools). 

  > [!NOTE]
  > Es ist zwar möglich, eine Failovergruppe zu erstellen, die mehrere Pools für elastische Datenbanken umfasst, aber es wird dringend empfohlen, für jeden Pool eine eigene Failovergruppe zu erstellen. Wenn Sie eine große Anzahl von Datenbanken über mehrere Pools für elastische Datenbanken verteilt haben, die Sie verschieben müssen, können Sie die Vorbereitungsschritte parallel ausführen und dann den Verschiebungsschritt parallel starten. Dieser Prozess kann besser skaliert werden und nimmt weniger Zeit in Anspruch als mehrere Pools für elastische Datenbanken in derselben Failovergruppe. 

### <a name="monitor-the-preparation-process"></a>Überwachen des Vorbereitungsprozesses

Sie können in regelmäßigen Abständen [Get-AzSqlDatabaseFailoverGroup](/powershell/module/az.sql/get-azsqldatabasefailovergroup) aufrufen, um die Replikation Ihrer Datenbanken von der Quelle zum Ziel zu überwachen. Das Ausgabeprojekt von `Get-AzSqlDatabaseFailoverGroup` umfasst eine Eigenschaft für den **ReplicationState**: 
   - **ReplicationState = 2** (CATCH_UP) gibt an, dass die Datenbank synchronisiert ist und das Failover sicher ausgeführt werden kann. 
   - **ReplicationState = 0** (SEEDING) gibt an, dass das Seeding für die Datenbank noch nicht abgeschlossen ist und bei einem Failover ein Fehler auftreten würde. 

### <a name="test-synchronization"></a>Testen der Synchronisierung
 
Sobald **ReplicationState** `2` ist, verbinden Sie sich mit der jeweiligen Datenbank oder einer Teilmenge von Datenbanken unter Verwendung des sekundären Endpunkts `<fog-name>.secondary.database.windows.net`, und führen Sie jede Abfrage an den Datenbanken durch, um die Konnektivität, die richtige Sicherheitskonfiguration und die Datenreplikation sicherzustellen. 

### <a name="initiate-the-move"></a>Initiieren der Verschiebung
 
1. Stellen Sie über den sekundären Endpunkt `<fog-name>.secondary.database.windows.net` eine Verbindung zum Zielserver her.
1. Verwenden Sie [Switch-AzSqlDatabaseFailoverGroup](/powershell/module/az.sql/switch-azsqldatabasefailovergroup), um die sekundäre verwaltete Instanz auf die primäre mit vollständiger Synchronisation umzuschalten. Dieser Vorgang wird entweder erfolgreich ausgeführt, oder es wird ein Rollback ausgeführt. 
1. Vergewissern Sie sich, dass der Befehl erfolgreich abgeschlossen wurde, indem Sie `nslook up <fog-name>.secondary.database.windows.net` verwenden, um sicherzustellen, dass der DNS CNAME-Eintrag auf die IP-Adresse der Zielregion verweist. Wenn der Switch-Befehl fehlschlägt, wird CNAME nicht aktualisiert. 

### <a name="remove-the-source-elastic-pools"></a>Entfernen der Quellpools für elastische Datenbanken
 
Nachdem der Verschiebe Vorgang abgeschlossen ist, entfernen Sie die Ressourcen in der Quellregion, um unnötige Gebühren zu vermeiden. 

1. Löschen Sie die Failovergruppe mit [Remove-AzSqlDatabaseFailoverGroup](/powershell/module/az.sql/remove-azsqldatabasefailovergroup).
1. Löschen Sie jeden Quellpool für elastische Datenbanken auf dem Quellserver mit [Remove-AzSqlElasticPool](/powershell/module/az.sql/remove-azsqlelasticpool). 
1. Löschen Sie den Quellserver mit [Remove-AzSqlServer](/powershell/module/az.sql/remove-azsqlserver). 
1. Entfernen Sie den Schlüsselspeicher, den Überwachungsspeichercontainer, den Event Hub, die AAD-Instanz und andere abhängige Ressourcen, damit Ihnen diese nicht mehr in Rechnung gestellt werden. 

## <a name="move-managed-instance"></a>Verschieben einer verwalteten Instanz

### <a name="verify-prerequisites"></a>Überprüfen der Voraussetzungen
 
1. Erstellen Sie für jede verwaltete Quellinstanz eine verwaltete Zielinstanz gleicher Größe in der Zielregion.  
1. Konfigurieren Sie das Netzwerk für eine verwaltete Instanz. Weitere Informationen finden Sie unter [Netzwerkkonfiguration](sql-database-howto-managed-instance.md#network-configuration).
1. Konfigurieren Sie die Ziel-Masterdatenbank mit den richtigen Anmeldedaten. Wenn Sie nicht der Abonnement- oder SQL Server-Administrator sind, wenden Sie sich an den Administrator, damit dieser Ihnen die erforderlichen Berechtigungen zuweist. 
1. Wenn Ihre Datenbanken mit TDE verschlüsselt sind und Sie Ihren eigenen Schlüssel im Azure Key Vault verwenden, stellen Sie sicher, dass der AKV mit identischen Verschlüsselungsschlüsseln sowohl in der Quell- als auch in der Zielregion vorhanden ist. Weitere Informationen finden Sie unter [TDE mit vom Kunden definierten, verwalteten Schlüsseln in Azure Key Vault](transparent-data-encryption-byok-azure-sql.md).
1. Wenn für die Instanz die Überwachung aktiviert ist, stellen Sie Folgendes sicher:
    - Der Speichercontainer oder der Event Hub mit den vorhandenen Protokollen wird in die Zielregion verschoben. 
    - Die Überwachung ist auf der Zielinstanz konfiguriert. Weitere Informationen finden Sie unter [Überwachen mit einer verwalteten Instanz](sql-database-managed-instance-auditing.md).
1. Wenn für Ihre Instanz eine Richtlinie für die Langzeitaufbewahrung (LTR) vorhanden ist, bleiben die vorhandenen LTR-Sicherungen dem aktuellen Server zugeordnet. Da ein anderer Zielserver verwendet wird, können Sie über den Quellserver auf die älteren LTR-Sicherungen in der Quellregion zugreifen, auch wenn der Server gelöscht wird. 

  > [!NOTE]
  > Dies reicht für das Verschieben zwischen der Sovereign Cloud und einer öffentlichen Region nicht aus. Für eine solche Migration müssen die LTR-Sicherungen auf den Zielserver verschoben werden. Dies wird derzeit nicht unterstützt. 

### <a name="prepare-resources"></a>Vorbereiten der Ressourcen

Erstellen Sie eine Failovergruppe zwischen jeder Quellinstanz und der entsprechenden Zielinstanz.
    - Die Replikation aller Datenbanken auf den Instanzen wird automatisch initiiert. Weitere Informationen finden Sie unter [Autofailover-Gruppen](sql-database-auto-failover-group.md).

 
### <a name="monitor-the-preparation-process"></a>Überwachen des Vorbereitungsprozesses

Sie können in regelmäßigen Abständen [Get-AzSqlDatabaseFailoverGroup](/powershell/module/az.sql/get-azsqldatabasefailovergroup?view=azps-2.3.2) aufrufen, um die Replikation Ihrer Datenbanken von der Quelle zum Ziel zu überwachen. Das Ausgabeprojekt von `Get-AzSqlDatabaseFailoverGroup` umfasst eine Eigenschaft für den **ReplicationState**: 
   - **ReplicationState = 2** (CATCH_UP) gibt an, dass die Datenbank synchronisiert ist und das Failover sicher ausgeführt werden kann. 
   - **ReplicationState = 0** (SEEDING) gibt an, dass das Seeding für die Datenbank noch nicht abgeschlossen ist und bei einem Failover ein Fehler auftreten würde. 

### <a name="test-synchronization"></a>Testen der Synchronisierung

Sobald **ReplicationState** `2` ist, verbinden Sie sich mit der jeweiligen Datenbank oder einer Teilmenge von Datenbanken unter Verwendung des sekundären Endpunkts `<fog-name>.secondary.database.windows.net`, und führen Sie jede Abfrage an den Datenbanken durch, um die Konnektivität, die richtige Sicherheitskonfiguration und die Datenreplikation sicherzustellen. 

### <a name="initiate-the-move"></a>Initiieren der Verschiebung 

1. Stellen Sie über den sekundären Endpunkt `<fog-name>.secondary.database.windows.net` eine Verbindung zum Zielserver her.
1. Verwenden Sie [Switch-AzSqlDatabaseFailoverGroup](/powershell/module/az.sql/switch-azsqldatabasefailovergroup?view=azps-2.3.2), um die sekundäre verwaltete Instanz auf die primäre mit vollständiger Synchronisation umzuschalten. Dieser Vorgang wird entweder erfolgreich ausgeführt, oder es wird ein Rollback ausgeführt. 
1. Vergewissern Sie sich, dass der Befehl erfolgreich abgeschlossen wurde, indem Sie `nslook up <fog-name>.secondary.database.windows.net` verwenden, um sicherzustellen, dass der DNS CNAME-Eintrag auf die IP-Adresse der Zielregion verweist. Wenn der Switch-Befehl fehlschlägt, wird CNAME nicht aktualisiert. 

### <a name="remove-the-source-managed-instances"></a>Entfernen der verwalteten Quellinstanzen
Nachdem der Verschiebe Vorgang abgeschlossen ist, entfernen Sie die Ressourcen in der Quellregion, um unnötige Gebühren zu vermeiden. 

1. Löschen Sie die Failovergruppe mit [Remove-AzSqlDatabaseFailoverGroup](/powershell/module/az.sql/remove-azsqldatabasefailovergroup). Dadurch wird die Konfiguration der Failovergruppe gelöscht und die Links für die Georeplikation zwischen den beiden Instanzen beendet. 
1. Löschen Sie die verwaltete Quellinstanz mithilfe von [Remove-AzSqlInstance](/powershell/module/az.sql/remove-azsqlinstance). 
1. Entfernen Sie alle zusätzlichen Ressourcen in der Ressourcengruppe, wie beispielsweise den virtuellen Cluster, das virtuelle Netzwerk und die Sicherheitsgruppe. 

## <a name="next-steps"></a>Nächste Schritte 

[Verwalten](sql-database-manage-after-migration.md) Sie Ihre Azure SQL-Datenbank, nachdem Sie migriert wurde. 

