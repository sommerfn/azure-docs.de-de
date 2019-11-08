---
title: Kontrolle des Netzwerkzugriffs
description: Übersicht über die Netzwerkzugriffssteuerung für Azure SQL-Datenbank und Data Warehouse, um den Zugriff zu verwalten und eine einzelne oder eine Pooldatenbank zu konfigurieren.
services: sql-database
ms.service: sql-database
ms.subservice: security
titleSuffix: Azure SQL Database and SQL Data Warehouse
ms.custom: ''
ms.devlang: ''
ms.topic: conceptual
author: rohitnayakmsft
ms.author: rohitna
ms.reviewer: vanto
ms.date: 08/05/2019
ms.openlocfilehash: 16de1d9fcf86459b6bcadd9d8c372e436aad0915
ms.sourcegitcommit: ac56ef07d86328c40fed5b5792a6a02698926c2d
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 11/08/2019
ms.locfileid: "73802942"
---
# <a name="azure-sql-database-and-data-warehouse-network-access-controls"></a>Netzwerkzugriffssteuerung für Azure SQL-Datenbank und Data Warehouse

> [!NOTE]
> Dieser Artikel gilt für den Azure SQL-Datenbankserver sowie für Datenbanken von SQL-Datenbank und SQL Data Warehouse, die auf dem Azure SQL-Datenbankserver erstellt werden. Der Einfachheit halber wird nur SQL-Datenbank verwendet, wenn sowohl SQL-Datenbank als auch SQL Data Warehouse gemeint sind.

> [!IMPORTANT]
> Dieser Artikel gilt *nicht* für **verwaltete Azure SQL-Datenbank-Instanzen**. Weitere Informationen zur Netzwerkkonfiguration finden Sie unter [Herstellen einer Verbindung mit einer verwalteten Instanz](sql-database-managed-instance-connect-app.md).

Wenn Sie eine neue Azure SQL Server-Instanz im [Azure-Portal](sql-database-single-database-get-started.md) erstellen, ist das Ergebnis ein öffentlicher Endpunkt im Format *IhrServername.database.windows.net*. Der gesamte Zugriff auf den öffentlichen Endpunkt wird standardmäßig verweigert. Sie können dann die folgenden Netzwerkzugriffssteuerungen verwenden, um den Zugriff auf die SQL-Datenbank selektiv über den öffentlichen Endpunkt zuzulassen.
- Azure-Dienste zulassen: Bei einer Festlegung auf „Ein“ können andere Ressourcen innerhalb der Azure-Grenze (z. B. ein virtueller Azure-Computer) auf die SQL-Datenbank zugreifen.

- IP-Firewallregeln: Verwenden Sie dieses Feature, um Verbindungen von einer bestimmten IP-Adresse explizit zuzulassen (z. B. von lokalen Computern).

- Virtual Network-Firewallregeln: Verwenden Sie dieses Feature, um Datenverkehr von einem bestimmten virtuellen Netzwerk innerhalb der Azure-Grenze zuzulassen.

> [!VIDEO https://channel9.msdn.com/Shows/Data-Exposed/Data-Exposed--SQL-Database-Connectivity-Explained/player?WT.mc_id=dataexposed-c9-niner]

## <a name="allow-azure-services"></a>Azure-Dienste zulassen 
Diese Einstellung wird während der Erstellung einer neuen Azure SQL Server-Instanz im [Azure-Portal](sql-database-single-database-get-started.md) deaktiviert.

 ![Screenshot: Erstellen eines neuen Servers][1]

Sie können diese Einstellung auch über den Bereich „Firewall“ wie folgt ändern, nachdem die Azure SQL Server-Instanz erstellt wurde.
  
 ![Screenshot: Verwalten der Serverfirewall][2]

Wenn die Einstellung auf **Ein** festgelegt wird, erlaubt Azure SQL Server Azure die Kommunikation von allen Ressourcen innerhalb der Azure-Grenze, egal ob sie Teil Ihres Abonnements sind oder nicht.

In vielen Fällen lässt die Einstellung **Ein** mehr zu, als die meisten Kunden wünschen. Möglicherweise sollten sie diese Einstellung auf **Aus** festlegen und durch restriktivere IP-Firewallregeln oder Virtual Network-Firewallregeln ersetzen. Dies wirkt sich auf die folgenden Features aus:

### <a name="import-export-service"></a>Import/Export-Dienst

Der Azure SQL-Datenbank-Import-/Exportdienst wird auf virtuellen Computern in Azure ausgeführt. Diese virtuellen Computer befinden sich nicht in Ihrem VNET und erhalten daher beim Verbinden mit Ihrer Datenbank eine Azure-IP-Adresse. Beim Entfernen des Zugriffs **Azure-Diensten den Zugriff auf den Server erlauben** können diese virtuellen Computer nicht mehr auf Ihre Datenbanken zugreifen.
Sie können dieses Problem umgehen, indem Sie den BACPAC-Import oder -Export mithilfe der DACFx-API direkt im Code ausführen.

### <a name="sql-database-query-editor"></a>Abfrage-Editor für SQL-Datenbank

Der Abfrage-Editor für Azure SQL-Datenbank wird auf virtuellen Computern in Azure bereitgestellt. Diese virtuellen Computer befinden sich nicht in Ihrem VNET. Aus diesem Grund erhalten die virtuellen Computer beim Verbinden mit Ihrer Datenbank eine Azure-IP-Adresse. Beim Entfernen von **Azure-Diensten den Zugriff auf den Server erlauben** können diese virtuellen Computer nicht mehr auf Ihre Datenbanken zugreifen.

### <a name="table-auditing"></a>Tabellenüberwachung

Derzeit stehen Ihnen zwei Möglichkeiten zum Aktivieren der Überwachung für Ihre SQL-Datenbank-Instanz zur Verfügung. Die Tabellenüberwachung führt zu einem Fehler, nachdem Sie Dienstendpunkte in Ihrer Azure SQL Server-Instanz aktiviert haben. Um dieses Problem zu umgehen, wechseln Sie zur Blobüberwachung.

### <a name="impact-on-data-sync"></a>Auswirkungen auf die Datensynchronisierung

Azure SQL-Datenbank verfügt über ein Datensynchronisierungsfeature, das unter Verwendung von Azure-IP-Adressen eine Verbindung mit Ihren Datenbanken herstellt. Bei der Verwendung von Dienstendpunkten werden Sie wahrscheinlich den Zugriff **Allen Azure-Diensten Zugriff auf den Server erlauben** auf Ihrem SQL-Datenbank-Server deaktivieren und damit die Funktion zur Datensynchronisierung beenden.

## <a name="ip-firewall-rules"></a>IP-Firewallregeln
Die IP-basierte Firewall ist ein Feature von Azure SQL Server, das den gesamten Zugriff auf den Datenbankserver verhindert, bis Sie explizit [IP-Adressen der Clientcomputer hinzufügen](sql-database-server-level-firewall-rule.md).


## <a name="virtual-network-firewall-rules"></a>Virtual Network-Firewallregeln

Zusätzlich zu den IP-Regeln können Sie mit der Azure SQL Server-Firewall *Regeln für virtuelle Netzwerke* definieren.  
Weitere Informationen finden Sie unter [Virtual Network service endpoints and rules for Azure SQL Database](sql-database-vnet-service-endpoint-rule-overview.md) (VNET-Dienstendpunkte und -Regeln für Azure SQL-Datenbank) oder in diesem Video:

> [!VIDEO https://channel9.msdn.com/Shows/Data-Exposed/Data-Exposed--Demo--Vnet-Firewall-Rules-for-SQL-Database/player?WT.mc_id=dataexposed-c9-niner]

 ### <a name="azure-networking-terminology"></a>Azure-Netzwerkterminologie  
Beachten Sie die folgenden Azure-Netzwerkbegriffe beim Erkunden von Virtual Network-Firewallregeln.

**Virtuelles Netzwerk:** Sie können Ihrem Azure-Abonnement virtuelle Netzwerke zuordnen. 

**Subnetz:** Ein virtuelles Netzwerk enthält **Subnetze**. Ihre virtuellen Azure-Computer (VMs) sind Subnetzen zugewiesen. Ein Subnetz kann mehrere VMs oder andere Computeknoten enthalten. Computeknoten, die sich außerhalb Ihres virtuellen Netzwerks befinden, können nicht auf Ihr virtuelles Netzwerk zugreifen, es sei denn, Sie konfigurieren für sie den sicheren Zugriff.

**Virtual Network-Dienstendpunkt:** Ein [Virtual Network-Dienstendpunkt][vm-virtual-network-service-endpoints-overview-649d] ist ein Subnetz, dessen Eigenschaftswerte mindestens einen formalen Azure-Diensttypnamen enthalten. In diesem Artikel beschäftigen wir uns mit dem Typnamen **Microsoft.Sql**, der auf einen Azure-Dienst mit dem Namen „SQL-Datenbank“ verweist.

**VNET-Regel:** Eine VNET-Regel für Ihren SQL-Datenbank-Server ist ein Subnetz, das in der Zugriffssteuerungsliste des SQL-Datenbank-Servers aufgeführt ist. Um in die Zugriffssteuerungsliste für Ihre SQL-Datenbank-Instanz zu gelangen, muss das Subnetz den Typnamen **Microsoft.Sql** enthalten. Eine VNET-Regel weist Ihren Azure SQL-Datenbank-Server an, Nachrichten von jedem Knoten anzunehmen, der zum Subnetz gehört.


## <a name="ip-vs-virtual-network-firewall-rules"></a>IP-Firewallregeln im Vergleich zu Virtual Network-Firewallregeln

Die Firewall von Azure SQL Server ermöglicht Ihnen das Angeben von IP-Adressbereichen, aus denen Nachrichten an die SQL-Datenbank-Instanz gesendet werden dürfen. Dieser Ansatz eignet sich gut für statische IP-Adressen, die sich außerhalb des privaten Azure-Netzwerks befinden. Doch viele virtuelle Computer (VMs) innerhalb des privaten Azure-Netzwerks sind mit *dynamischen* IP-Adressen konfiguriert. Dynamische IP-Adressen können sich ändern, wenn der virtuelle Computer neu gestartet wird, und damit die IP-basierte Firewallregel ungültig machen. Es wäre töricht, eine dynamische IP-Adresse in einer Firewallregel in einer Produktionsumgebung anzugeben.

Sie können diese Einschränkung umgehen, indem Sie eine *statische* IP-Adresse für Ihre VM abrufen. Weitere Details finden Sie unter [Konfigurieren privater IP-Adressen für einen virtuellen Computer mithilfe des Azure-Portals][vm-configure-private-ip-addresses-for-a-virtual-machine-using-the-azure-portal-321w]. Die Verwendung statischer IP-Adressen kann jedoch schwierig zu verwalten sein, und ihre Skalierung ist aufwendig. 

Regeln für virtuelle Netzwerke stellen eine einfachere Alternative zum Einrichten und Verwalten des Zugriffs von einem bestimmten Subnetz mit Ihren virtuellen Computern dar.

> [!NOTE]
> Azure SQL-Datenbank wird noch nicht in einem Subnetz unterstützt. Wenn sich Ihr Azure SQL-Datenbank-Server auf einem Knoten in einem Subnetz in Ihrem virtuellen Netzwerk befände, könnten alle Knoten innerhalb des virtuellen Netzwerks mit Ihrer SQL-Datenbank-Instanz kommunizieren. In diesem Fall könnten Ihre virtuellen Computer mit der SQL-Datenbank-Instanz kommunizieren, ohne dass VNET-Regeln oder IP-Regeln erforderlich sind.

## <a name="next-steps"></a>Nächste Schritte

- Einen Schnellstart zum Erstellen einer IP-Firewallregel auf Serverebene finden Sie unter [Erstellen einer Azure SQL-Datenbank-Instanz](sql-database-single-database-get-started.md).

- Einen Schnellstart zum Erstellen einer VNET-Firewallregel auf Serverebene finden Sie unter [VNET-Dienstendpunkte und Regeln für Azure SQL-Datenbank](sql-database-vnet-service-endpoint-rule-overview.md).

- Hilfe beim Herstellen einer Verbindung mit einer Azure SQL-Datenbank-Instanz über Open-Source-Anwendungen oder Anwendungen von Drittanbietern finden Sie unter [Clientschnellstarts für SQL-Datenbank](https://msdn.microsoft.com/library/azure/ee336282.aspx).

- Informationen zu zusätzlichen Ports, die Sie ggf. öffnen müssen, finden Sie im Abschnitt **SQL-Datenbank: ,Außerhalb‘ im Vergleich zu ,Innerhalb‘** im Artikel [Andere Ports als 1433 für ADO.NET 4.5 und SQL-Datenbank](sql-database-develop-direct-route-ports-adonet-v12.md).

- Eine Übersicht über die Azure SQL-Datenbank-Konnektivität finden Sie unter [Verbindungsarchitektur von Azure SQL-Datenbank](sql-database-connectivity-architecture.md).

- Eine Übersicht über die Sicherheit von Azure SQL-Datenbank finden Sie unter [Sichern der SQL-Datenbank](sql-database-security-overview.md).

<!--Image references-->
[1]: ./media/sql-database-get-started-portal/new-server2.png
[2]: ./media/sql-database-get-started-portal/manage-server-firewall.png
