---
title: Private Link
description: Übersicht über das Feature „Privater Endpunkt“
author: rohitnayakmsft
ms.author: rohitna
titleSuffix: Azure SQL Database and SQL Data Warehouse
ms.service: sql-database
ms.topic: overview
ms.reviewer: vanto
ms.date: 09/17/2019
ms.openlocfilehash: fcb89cbcadb5e101ab2b4bfd18d0b7b91c63c92a
ms.sourcegitcommit: ac56ef07d86328c40fed5b5792a6a02698926c2d
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 11/08/2019
ms.locfileid: "73821295"
---
# <a name="private-link-for-azure-sql-database-and-data-warehouse-preview"></a>Private Link für Azure SQL-Datenbank und Data Warehouse (Vorschauversion)

Private Link ermöglicht die Verbindungsherstellung mit verschiedenen PaaS-Diensten in Azure über einen **privaten Endpunkt**. Eine Liste der Dienste, die die Private Link-Funktion unterstützen, finden Sie in der [Private Link-Dokumentation](../private-link/index.yml). Ein privater Endpunkt ist eine private IP-Adresse in einem bestimmten [VNET](../virtual-network/virtual-networks-overview.md) und Subnetz. 

> [!IMPORTANT]
> Dieser Artikel gilt für den Azure SQL-Datenbankserver sowie für Datenbanken von SQL-Datenbank und SQL Data Warehouse, die auf dem Azure SQL-Datenbankserver erstellt werden. Der Einfachheit halber wird nur SQL-Datenbank verwendet, wenn sowohl SQL-Datenbank als auch SQL Data Warehouse gemeint sind. Dieser Artikel gilt *nicht* für Bereitstellungen vom Typ **Verwaltete Instanz** in Azure SQL-Datenbank.

## <a name="data-exfiltration-prevention"></a>Verhinderung der Datenexfiltration

Datenexfiltration in Azure SQL-Datenbank bedeutet, dass ein autorisierter Benutzer (etwa ein Datenbankadministrator) Daten aus einem System extrahieren und an einen anderen Ort oder in ein anderes System außerhalb der Organisation verschieben kann. So kann ein Benutzer beispielsweise Daten in ein Speicherkonto eines Dritten verschieben.

Stellen Sie sich ein Szenario vor, in dem ein Benutzer SQL Server Management Studio (SSMS) auf einem virtuellen Azure-Computer ausführt, von dem eine Verbindung mit SQL-Datenbank hergestellt wird. Die SQL-Datenbank-Instanz befindet sich im Rechenzentrum „USA, Westen“. Das folgende Beispiel zeigt, wie Sie den Zugriff mit öffentlichen Endpunkten in SQL-Datenbank unter Verwendung von Netzwerkzugriffssteuerungen einschränken.

1. Deaktivieren Sie den gesamten Datenverkehr von Azure-Diensten zur SQL-Datenbank-Instanz über den öffentlichen Endpunkt, indem Sie die Option zum Zulassen von Azure-Diensten auf **AUS** festlegen. Vergewissern Sie sich, dass durch die Firewallregeln auf Server- und Datenbankebene keine IP-Adressen zugelassen werden. Weitere Informationen finden Sie unter [Netzwerkzugriffssteuerung für Azure SQL-Datenbank und Data Warehouse](sql-database-networkaccess-overview.md).
1. Lassen Sie nur Datenverkehr für die SQL-Datenbank-Instanz mit der privaten IP-Adresse des virtuellen Computers zu. Weitere Informationen finden Sie in den Artikeln [Verwenden von VNET-Dienstendpunkten und -Regeln für Datenbankserver](sql-database-vnet-service-endpoint-rule-overview.md) und [IP-Firewallregeln für Azure SQL-Datenbank und Azure SQL Data Warehouse](sql-database-firewall-configure.md).
1. Beschränken Sie auf dem virtuellen Azure-Computer den Bereich der ausgehenden Verbindung mithilfe von [Netzwerksicherheitsgruppen (NSGs)](../virtual-network/manage-network-security-group.md) und Diensttags:
    - Geben Sie eine NSG-Regel an, um Datenverkehr für das Diensttag „SQL.WestUs“ zuzulassen. Dadurch ist nur eine Verbindung mit der SQL-Datenbank-Instanz in „USA, Westen“ möglich.
    - Geben Sie eine NSG-Regel (mit einer **höheren Priorität**) an, um Datenverkehr für das Diensttag „SQL“ zu verweigern. Dadurch werden Verbindungen mit der SQL-Datenbank-Instanz in allen Regionen verweigert.

Am Ende dieser Einrichtung kann der virtuelle Azure-Computer nur eine Verbindung mit SQL-Datenbank-Instanzen in der Region „USA, Westen“ herstellen. Die Konnektivität ist jedoch nicht auf eine einzelne SQL-Datenbank-Instanz beschränkt. Der virtuelle Computer kann weiterhin eine Verbindung mit beliebigen SQL-Datenbank-Instanzen in der Region „USA, Westen“ herstellen – also auch mit den Datenbanken, die nicht Teil des Abonnements sind. Wir haben den Bereich der Datenexfiltration im obigen Szenario auf eine bestimmte Region reduziert, sie aber nicht vollständig unterbunden.

Mit Private Link können Kunden jetzt Netzwerkzugriffssteuerungen wie NSGs einrichten, um den Zugriff auf den privaten Endpunkt einzuschränken. Einzelne Azure-PaaS-Ressourcen werden dann bestimmten privaten Endpunkten zugeordnet. Ein böswilliger Insider kann somit nur auf die zugeordnete PaaS-Ressource (etwa eine SQL-Datenbank-Instanz), aber nicht auf andere Ressourcen zugreifen. 

## <a name="on-premises-connectivity-over-private-peering"></a>Lokale Konnektivität über privates Peering

Wenn Kunden von lokalen Computern aus eine Verbindung mit dem öffentlichen Endpunkt herstellen, muss ihre IP-Adresse mithilfe einer [Firewallregel auf Serverebene](sql-database-server-level-firewall-rule.md) der IP-basierten Firewall hinzugefügt werden. Dieses Modell eignet sich zwar gut, um den Zugriff auf einzelne Computer für Entwicklungs- oder Testworkloads zuzulassen, in einer Produktionsumgebung gestaltet sich die Verwaltung jedoch schwierig.

Mit Private Link können Kunden standortübergreifenden Zugriff auf den privaten Endpunkt mittels [ExpressRoute](../expressroute/expressroute-introduction.md), privatem Peering oder VPN-Tunneling ermöglichen. Kunden haben dann die Möglichkeit, den gesamten Zugriff über den öffentlichen Endpunkt zu deaktivieren und nicht die IP-basierte Firewall zu verwenden, um IP-Adressen zuzulassen.

Mit Private Link können Kunden den standortübergreifenden Zugriff auf den privaten Endpunkt über privates ER-Peering (ExpressRoute) oder über einen VPN-Tunnel ermöglichen. Anschließend können sie den gesamten Zugriff über einen öffentlichen Endpunkt deaktivieren und nicht die IP-basierte Firewall verwenden.

## <a name="how-to-set-up-private-link-for-azure-sql-database"></a>Einrichten von Private Link für Azure SQL-Datenbank 

### <a name="creation-process"></a>Erstellungsprozess
Private Endpunkte können über das Portal, mithilfe von PowerShell oder über die Azure CLI erstellt werden:
- [Portal](../private-link/create-private-endpoint-portal.md)
- [PowerShell](../private-link/create-private-endpoint-powershell.md)
- [BEFEHLSZEILENSCHNITTSTELLE (CLI)](../private-link/create-private-endpoint-cli.md)

### <a name="approval-process"></a>Genehmigungsprozess
Nachdem der Netzwerkadministrator den privaten Endpunkt (PE) erstellt hat, kann der SQL-Administrator die private Endpunktverbindung (Private Endpoint Connection, PEC) mit SQL-Datenbank verwalten.

1. Navigieren Sie im Azure-Portal zu Ihrer SQL-Server-Ressource.

    - (1) Wählen Sie im linken Bereich die private Endpunktverbindung aus.
    - (2) Liste mit allen privaten Endpunktverbindungen (Private Endpoint Connections, PECs)
    - (3) Der entsprechende erstellte private Endpunkt (PE) ![Screenshot: Alle PECs][3]

1. Wählen Sie eine einzelne PEC aus der Liste aus.
![Screenshot: Ausgewählte PEC][6]

1. Der SQL-Administrator kann eine PEC genehmigen oder ablehnen und optional eine kurze Textantwort hinzufügen.
![Screenshot: PEC-Genehmigung][4]

1. Nach der Genehmigung oder Ablehnung wird der entsprechende Zustand zusammen mit dem Antworttext in der Liste angezeigt.
![Screenshot: Alle PECs nach der Genehmigung][5]

## <a name="use-cases-of-private-link-for-azure-sql-database"></a>Private Link-Anwendungsfälle für Azure SQL-Datenbank 

Clients können eine Verbindung mit dem privaten Endpunkt über das gleiche VNET, über das mittels Peering verbundene VNET in der gleichen Region oder regionsübergreifend über eine VNET-zu-VNET-Verbindung herstellen. Darüber hinaus können Clients von der lokalen Umgebung aus eine Verbindung über ExpressRoute, privates Peering oder VPN-Tunneling herstellen. Die gängigen Anwendungsfälle sind im folgenden Diagramm vereinfacht dargestellt:

 ![Diagramm: Konnektivitätsoptionen][1]

## <a name="test-connectivity-to-sql-database-from-an-azure-vm-in-same-virtual-network-vnet"></a>Testen der Konnektivität mit SQL-Datenbank über einen virtuellen Azure-Computer im gleichen virtuellen Netzwerk (VNET)

Für dieses Szenario wird davon ausgegangen, dass Sie einen virtuellen Azure-Computer unter Windows Server 2016 erstellt haben. 

1. [Starten Sie eine RDP-Sitzung (Remotedesktop), und stellen Sie eine Verbindung mit dem virtuellen Computer her.](../virtual-machines/windows/connect-logon.md#connect-to-the-virtual-machine) 
1. Anschließend können Sie mithilfe der folgenden Tools einige grundlegende Konnektivitätsprüfungen durchführen, um sicherzugehen, dass der virtuelle Computer die Verbindung mit SQL-Datenbank über den privaten Endpunkt herstellt:
    1. Telnet
    1. PsPing
    1. Nmap
    1. SQL Server Management Studio (SSMS)

### <a name="check-connectivity-using-telnet"></a>Überprüfen der Konnektivität mithilfe von Telnet

Der [Telnet-Client](https://docs.microsoft.com/previous-versions/windows/it-pro/windows-server-2008-R2-and-2008/cc754293%28v%3dws.10%29) ist ein Windows-Feature zum Testen der Konnektivität. Je nach Version des Windows-Betriebssystems muss dieses Feature ggf. explizit aktiviert werden. 

Öffnen Sie nach der Installation von Telnet ein Eingabeaufforderungsfenster. Führen Sie den Telnet-Befehl aus, und geben Sie dabei die IP-Adresse und den privaten Endpunkt der SQL-Datenbank-Instanz an:

```
>telnet 10.1.1.5 1433
```

Wenn Telnet erfolgreich eine Verbindung herstellt, wird im Befehlsfenster ein leerer Bildschirm angezeigt:

 ![Diagramm: Telnet][2]

### <a name="check-connectivity-using-psping"></a>Überprüfen der Konnektivität mithilfe von PsPing

[PsPing](/sysinternals/downloads/psping) kann wie folgt verwendet werden, um sich zu vergewissern, dass die private Endpunktverbindung (Private Endpoint Connection, PEC) auf Verbindungen am Port 1433 lauscht.

Führen Sie PsPing wie folgt aus, und geben Sie dabei den FQDN für Ihren SQL-Datenbank-Server und den Port 1433 an:

```
>psping.exe mysqldbsrvr.database.windows.net:1433

PsPing v2.10 - PsPing - ping, latency, bandwidth measurement utility
Copyright (C) 2012-2016 Mark Russinovich
Sysinternals - www.sysinternals.com

TCP connect to 10.6.1.4:1433:
5 iterations (warmup 1) ping test:
Connecting to 10.6.1.4:1433 (warmup): from 10.6.0.4:49953: 2.83ms
Connecting to 10.6.1.4:1433: from 10.6.0.4:49954: 1.26ms
Connecting to 10.6.1.4:1433: from 10.6.0.4:49955: 1.98ms
Connecting to 10.6.1.4:1433: from 10.6.0.4:49956: 1.43ms
Connecting to 10.6.1.4:1433: from 10.6.0.4:49958: 2.28ms
```

Die Ausgabe zeigt, dass PsPing die private IP-Adresse pingen konnte, die der PEC zugeordnet ist.

### <a name="check-connectivity-using-nmap"></a>Überprüfen der Konnektivität mithilfe von Nmap

Nmap (Network Mapper) ist ein kostenloses Open-Source-Tool für die Netzwerkermittlung und Sicherheitsüberwachung. Weitere Informationen sowie den Downloadlink finden Sie unter https://nmap.org. Mithilfe dieses Tools können Sie sich vergewissern, dass der private Endpunkt am Port 1433 auf Verbindungen lauscht.

Führen Sie Nmap wie folgt aus, und geben Sie dabei den Adressbereich des Subnetzes an, in dem der private Endpunkt gehostet wird:

```
>nmap -n -sP 10.1.1.0/24
...
...
Nmap scan report for 10.1.1.5
Host is up (0.00s latency).
Nmap done: 256 IP addresses (1 host up) scanned in 207.00 seconds
```

Das Ergebnis zeigt, dass eine einzelne IP-Adresse aktiv ist. Diese entspricht der IP-Adresse für den privaten Endpunkt.


### <a name="check-connectivity-using-sql-server-management-studio-ssms"></a>Überprüfen der Konnektivität mithilfe von SQL Server Management Studio (SSMS)

Der letzte Schritt besteht in der [Verwendung von SSMS zum Herstellen einer Verbindung mit der SQL-Datenbank-Instanz](sql-database-connect-query-ssms.md). Nachdem Sie mithilfe von SSMS eine Verbindung mit der SQL-Datenbank-Instanz hergestellt haben, führen Sie die folgende Abfrage aus, um sich zu vergewissern, dass für die Verbindung die private IP-Adresse des virtuellen Azure-Computers verwendet wird:

````
select client_net_address from sys.dm_exec_connections 
where session_id=@@SPID
````
> [!NOTE]
> In der Vorschauversion wird für Verbindungen mit dem privaten Endpunkt nur **Proxy** als [Verbindungsrichtlinie](sql-database-connectivity-architecture.md#connection-policy) unterstützt.


## <a name="connecting-from-an-azure-vm-in-peered-virtual-network-vnet"></a>Herstellen einer Verbindung über einen virtuellen Azure-Computer in einem virtuellen Netzwerk (VNET) mit Peering 

Konfigurieren Sie das [VNET-Peering](../virtual-network/tutorial-connect-virtual-networks-powershell.md), um über einen virtuellen Azure-Computer in einem VNET mit Peering eine Verbindung mit der SQL-Datenbank-Instanz herzustellen.

## <a name="connecting-from-an-azure-vm-in-vnet-to-vnet-environment"></a>Herstellen einer Verbindung über einen virtuellen Azure-Computer in einer VNET-zu-VNET-Umgebung

Konfigurieren Sie eine [VNET-zu-VNET-VPN-Gatewayverbindung](../vpn-gateway/vpn-gateway-howto-vnet-vnet-resource-manager-portal.md), um über einen virtuellen Azure-Computer in einer anderen Region oder in einem anderen Abonnement eine Verbindung mit einer SQL-Datenbank-Instanz herzustellen.

## <a name="connecting-from-an-on-premises-environment-over-vpn"></a>Herstellen einer VPN-Verbindung in einer lokalen Umgebung

Verwenden oder implementieren Sie eine der folgenden Optionen, um in einer lokalen Umgebung eine Verbindung mit der SQL-Datenbank-Instanz herzustellen:
- [Point-to-Site-Verbindung](../vpn-gateway/vpn-gateway-howto-point-to-site-rm-ps.md)
- [Site-to-Site-VPN-Verbindung](../vpn-gateway/vpn-gateway-create-site-to-site-rm-powershell.md)
- [ExpressRoute-Verbindung](../expressroute/expressroute-howto-linkvnet-portal-resource-manager.md)


## <a name="connecting-from-an-azure-sql-data-warehouse-to-azure-storage-using-polybase"></a>Herstellen einer Verbindung über eine Azure SQL Data Warehouse-Instanz mit Azure Storage unter Verwendung von Polybase

PolyBase wird häufig verwendet, um Daten aus Azure Storage-Konten in Azure SQL Data Warehouse zu laden. Wenn das Azure Storage-Konto, aus dem Sie Daten laden, den Zugriff auf einen Satz von VNET-Subnetzen über private Endpunkte, Dienstendpunkte oder IP-basierte Firewalls einschränkt, wird die Konnektivität zwischen PolyBase und dem Konto unterbrochen. Führen Sie die [hier](sql-database-vnet-service-endpoint-rule-overview.md#impact-of-using-vnet-service-endpoints-with-azure-storage) angegebenen Schritte aus, um PolyBase-Import- und -Exportszenarien zu ermöglichen, in denen Azure SQL Data Warehouse eine Verbindung mit Azure Storage (mit VNET-Schutz) herstellt. 



## <a name="next-steps"></a>Nächste Schritte

- Eine Übersicht über die Sicherheit von Azure SQL-Datenbank finden Sie unter [Sichern der SQL-Datenbank](sql-database-security-overview.md).
- Eine Übersicht über die Azure SQL-Datenbank-Konnektivität finden Sie unter [Verbindungsarchitektur von Azure SQL-Datenbank](sql-database-connectivity-architecture.md).

<!--Image references-->
[1]: ./media/sql-database-get-started-portal/pe-connect-overview.png
[2]: ./media/sql-database-get-started-portal/telnet-result.png
[3]: ./media/sql-database-get-started-portal/pec-list-before.png
[4]: ./media/sql-database-get-started-portal/pec-approve.png
[5]: ./media/sql-database-get-started-portal/pec-list-after.png
[6]: ./media/sql-database-get-started-portal/pec-select.png
