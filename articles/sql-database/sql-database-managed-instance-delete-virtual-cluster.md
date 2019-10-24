---
title: Löschen eines Subnetzes nach Löschen einer verwalteten Azure SQL-Datenbank-Instanz | Microsoft-Dokumentation
description: Erfahren Sie, wie Sie ein virtuelles Azure-Netzwerk nach Löschen einer verwalteten Azure SQL-Datenbank-Instanz löschen.
services: sql-database
ms.service: sql-database
ms.custom: ''
ms.devlang: ''
ms.topic: conceptual
author: danimir
ms.author: danil
ms.reviewer: douglas, carlrab, sstein
ms.date: 06/26/2019
ms.openlocfilehash: 7ad09682275b5cc2311b792899a85c1c47eafc0d
ms.sourcegitcommit: 77bfc067c8cdc856f0ee4bfde9f84437c73a6141
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 10/16/2019
ms.locfileid: "72431290"
---
# <a name="delete-a-subnet-after-deleting-an-azure-sql-database-managed-instance"></a>Löschen eines Subnetzes nach Löschen einer verwalteten Azure SQL-Datenbank-Instanz

Dieser Artikel enthält Richtlinien zum manuellen Löschen eines Subnetzes nach Löschen der letzten darin enthaltenen verwalteten Azure SQL-Datenbank-Instanz.

Verwaltete Instanzen werden in [virtuellen Clustern](sql-database-managed-instance-connectivity-architecture.md#virtual-cluster-connectivity-architecture) bereitgestellt. Jeder virtuelle Cluster ist einem Subnetz zugeordnet. Der virtuelle Cluster bleibt nach dem Löschen der letzten Instanz noch 12 Stunden erhalten, damit Sie im gleichen Subnetz schneller verwaltete Instanzen erstellen können. Das Beibehalten eines leeren virtuellen Clusters ist kostenlos. Während dieses Zeitraums kann der mit dem Subnetz verbundene virtuelle Cluster nicht gelöscht werden.

Wenn Sie keine 12 Stunden warten, sondern den virtuellen Cluster sowie das Subnetz früher löschen möchten, können Sie dies manuell tun. Löschen Sie den virtuellen Cluster manuell über das Azure-Portal oder die API für virtuelle Cluster.

> [!IMPORTANT]
> - Damit der Löschvorgang erfolgreich ist, sollte der virtuelle Cluster keine verwalteten Instanzen enthalten. 
> - Das Löschen eines virtuellen Clusters ist ein zeitintensiver Vorgang mit einer Dauer von etwa 1,5 Stunden. (Aktuelle Angaben zur Löschdauer für virtuelle Cluster finden Sie unter [Verwaltungsvorgänge für verwaltete Instanzen](https://docs.microsoft.com/azure/sql-database/sql-database-managed-instance#managed-instance-management-operations).) Während dieser Zeit ist der virtuelle Cluster im Portal weiterhin sichtbar, bis der Vorgang abgeschlossen wurde.

## <a name="delete-virtual-cluster-from-the-azure-portal"></a>Löschen virtueller Cluster im Azure-Portal

Suchen Sie nach den Ressourcen eines virtuellen Clusters, um diesen über das Azure-Portal zu löschen.

![Screenshot: Azure-Portal mit hervorgehobenem Suchfeld](./media/sql-database-managed-instance-delete-virtual-cluster/virtual-clusters-search.png)

Nachdem Sie den virtuellen Cluster gefunden haben, den Sie löschen möchten, wählen Sie diese Ressource aus, und klicken Sie auf **Löschen**. Sie werden aufgefordert, das Löschen des virtuellen Clusters zu bestätigen.

![Screenshot: Dashboard „Virtuelle Cluster“ im Azure-Portal mit hervorgehobener Option „Löschen“](./media/sql-database-managed-instance-delete-virtual-cluster/virtual-clusters-delete.png)

In den Azure-Portalbenachrichtigungen wird eine Bestätigung dafür angezeigt, dass die Anforderung zum Löschen des virtuellen Clusters erfolgreich übermittelt wurde. Der eigentliche Löschvorgang dauert etwa 1,5 Stunden. Während dieser Zeit ist der virtuelle Cluster im Portal weiterhin sichtbar. Nach Abschluss des Vorgangs ist der virtuelle Cluster nicht mehr sichtbar, und das zugehörige Subnetz wird zur erneuten Verwendung freigegeben.

> [!TIP]
> Wenn in Ihrem virtuellen Cluster keine verwalteten Instanzen angezeigt werden und Sie den virtuellen Cluster nicht löschen können, vergewissern Sie sich, dass kein Instanzenbereitstellungsvorgang mehr läuft. Dies umfasst gestartete und abgebrochene Bereitstellungen, die noch nicht abgeschlossen sind. Der Grund: Diese Vorgänge verwenden weiterhin den virtuellen Cluster, was dazu führt, dass er nicht gelöscht werden kann. Auf der Registerkarte „Deployments“ (Bereitstellungen) der Ressourcengruppe, in der die Instanz bereitgestellt wurde, werden Bereitstellungen angezeigt, die noch nicht abgeschlossen sind. Warten Sie in diesem Fall, bis die Bereitstellung abgeschlossen ist, und löschen Sie dann die verwaltete Instanz und anschließend den virtuellen Cluster.

## <a name="delete-virtual-cluster-by-using-the-api"></a>Löschen eines virtuellen Clusters mithilfe der API

Verwenden Sie die in der [Methode zum Löschen virtueller Cluster](https://docs.microsoft.com/rest/api/sql/virtualclusters/delete) angegebenen URI-Parameter, um einen virtuellen Cluster mithilfe der API zu löschen.

## <a name="next-steps"></a>Nächste Schritte

- Eine Übersicht finden Sie unter [Was ist eine verwaltete Instanz?](sql-database-managed-instance.md).
- Machen Sie sich mit der [Verbindungsarchitektur der verwalteten Azure SQL-Datenbank-Instanz](sql-database-managed-instance-connectivity-architecture.md) vertraut.
- Unter [Ändern eines vorhandenen virtuellen Netzwerks für die verwaltete Instanz](sql-database-managed-instance-configure-vnet-subnet.md) erfahren Sie, wie Sie Änderungen vornehmen können.
- Ein Tutorial zum Erstellen eines virtuellen Netzwerks, Erstellen einer verwalteten Instanz und Wiederherstellen einer Datenbank aus einer Datenbanksicherung finden Sie unter [Erstellen einer verwalteten Azure SQL-Datenbank-Instanz](sql-database-managed-instance-get-started.md).
- Lesen Sie bei Problemen im Zusammenhang mit dem DNS den Artikel [Konfigurieren eines benutzerdefinierten DNS](sql-database-managed-instance-custom-dns.md).
