---
title: Netzwerktopologien für Migrationen von verwalteten Azure SQL-Datenbank-Instanzen mithilfe von Azure Database Migration Service | Microsoft-Dokumentation
description: Erfahren Sie, wie Sie Quelle und Ziel für Azure Database Migration Service konfigurieren.
services: database-migration
author: HJToland3
ms.author: jtoland
manager: craigg
ms.reviewer: craigg
ms.service: dms
ms.workload: data-services
ms.custom: mvc
ms.topic: article
ms.date: 06/07/2019
ms.openlocfilehash: 74613599903f7cde606295a1e2d9eaaa0924cf50
ms.sourcegitcommit: d4dfbc34a1f03488e1b7bc5e711a11b72c717ada
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 06/13/2019
ms.locfileid: "66808422"
---
# <a name="network-topologies-for-azure-sql-db-managed-instance-migrations-using-azure-database-migration-service"></a>Netzwerktopologien für Migrationen von verwalteten Azure SQL-Datenbank-Instanzen mithilfe von Azure Database Migration Service

In diesem Artikel werden verschiedene Netzwerktopologien erläutert, die Azure Database Migration Service verwenden kann, um eine umfassende Migration von lokalen SQL Server-Instanzen zu einer verwalteten Azure SQL-Datenbank-Instanz zu ermöglichen.

## <a name="azure-sql-database-managed-instance-configured-for-hybrid-workloads"></a>Verwaltete Azure SQL-Datenbank-Instanz konfiguriert für Hybridworkloads 

Verwenden Sie diese Topologie, wenn die verwaltete Azure SQL-Datenbank-Instanz mit Ihrem lokalen Netzwerk verbunden ist. Dieser Ansatz nutzt das einfachste Netzwerkrouting bei maximalem Datendurchsatz während der Migration.

![Netzwerktopologie für Hybridworkloads](media/resource-network-topologies/hybrid-workloads.png)

**Anforderungen**

- In diesem Szenario werden die verwaltete Azure SQL-Datenbank-Instanz und die Azure Database Migration Service-Instanz im selben Azure-VNet erstellt, sie nutzen jedoch unterschiedliche Subnetze.  
- Das in diesem Szenario verwendete VNet ist über [ExpressRoute](https://docs.microsoft.com/azure/expressroute/expressroute-introduction) oder [VPN](https://docs.microsoft.com/azure/vpn-gateway/vpn-gateway-about-vpngateways) auch mit dem lokalen Netzwerk verbunden.

## <a name="azure-sql-database-managed-instance-isolated-from-the-on-premises-network"></a>Verwaltete Azure SQL-Datenbank-Instanz isoliert vom lokalen Netzwerk

Verwenden Sie diese Netzwerktopologie, wenn Ihre Umgebung mindestens eines der folgenden Szenarien erfordert:

- Die verwaltete Azure SQL-Datenbank-Instanz ist von der lokalen Konnektivität isoliert, aber die Azure Database Migration Service-Instanz ist mit dem lokalen Netzwerk verbunden.
- Es gelten RBAC-Richtlinien (rollenbasierte Zugriffssteuerung), und Sie müssen den Benutzerzugriff auf das Abonnement beschränken, unter dem die verwaltete Azure SQL-Datenbank-Instanz gehostet wird.
- Die für die verwaltete Azure SQL-Datenbank-Instanz und Azure Database Migration Service verwendeten VNets befinden sich in unterschiedlichen Abonnements.

![Netzwerktopologie für eine vom lokalen Netzwerk isolierte verwaltete Instanz](media/resource-network-topologies/mi-isolated-workload.png)

**Anforderungen**

- Das VNet, das Azure Database Migration Service für dieses Szenario verwendet, muss über ExpressRoute (https://docs.microsoft.com/azure/expressroute/expressroute-introduction) oder [VPN](https://docs.microsoft.com/azure/vpn-gateway/vpn-gateway-about-vpngateways) auch mit dem lokalen Netzwerk verbunden sein.
- Richten Sie ein [VNet-Netzwerkpeering](https://docs.microsoft.com/azure/virtual-network/virtual-network-peering-overview) zwischen dem VNet für die verwaltete Azure SQL-Datenbank-Instanz und Azure Database Migration Service ein.

## <a name="cloud-to-cloud-migrations-shared-vnet"></a>Cloud-zu-Cloud-Migrationen: Gemeinsam genutztes VNet

Verwenden Sie diese Topologie, wenn die SQL Server-Quelle auf einer Azure-VM gehostet wird und dasselbe VNet wie die verwaltete Azure SQL-Datenbank-Instanz und Azure Database Migration Service nutzt.

![Netzwerktopologie für Cloud-zu-Cloud-Migrationen mit einem gemeinsam genutzten VNet](media/resource-network-topologies/cloud-to-cloud.png)

**Anforderungen**

- Es bestehen keine weiteren Anforderungen.

## <a name="cloud-to-cloud-migrations-isolated-vnet"></a>Cloud-zu-Cloud-Migrationen: Isoliertes VNet

Verwenden Sie diese Netzwerktopologie, wenn Ihre Umgebung mindestens eines der folgenden Szenarien erfordert:

- Die verwaltete Azure SQL-Datenbank-Instanz wird in einem isolierten VNet bereitgestellt.
- Es gelten RBAC-Richtlinien (rollenbasierte Zugriffssteuerung), und Sie müssen den Benutzerzugriff auf das Abonnement beschränken, unter dem die verwaltete Azure SQL-Datenbank-Instanz gehostet wird.
- Die für die verwaltete Azure SQL-Datenbank-Instanz und Azure Database Migration Service verwendeten VNets befinden sich in unterschiedlichen Abonnements.

![Netzwerktopologie für Cloud-zu-Cloud-Migrationen mit einem isolierten VNet](media/resource-network-topologies/cloud-to-cloud-isolated.png)

**Anforderungen**

- Richten Sie ein [VNet-Netzwerkpeering](https://docs.microsoft.com/azure/virtual-network/virtual-network-peering-overview) zwischen dem VNet für die verwaltete Azure SQL-Datenbank-Instanz und Azure Database Migration Service ein.

## <a name="inbound-security-rules"></a>Eingangssicherheitsregeln

| **NAME**   | **PORT** | **PROTOKOLL** | **QUELLE** | **ZIEL** | **AKTION** |
|------------|----------|--------------|------------|-----------------|------------|
| DMS_subnet | Beliebig      | Beliebig          | DMS-SUBNETZ | Beliebig             | ZULASSEN      |

## <a name="outbound-security-rules"></a>Ausgangssicherheitsregeln

| **NAME**                  | **PORT**                                              | **PROTOKOLL** | **QUELLE** | **ZIEL**           | **AKTION** | **Grund für die Regel**                                                                                                                                                                              |
|---------------------------|-------------------------------------------------------|--------------|------------|---------------------------|------------|--------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| management                | 443, 9354                                              | TCP          | Beliebig        | Beliebig                       | ZULASSEN      | Kommunikation auf Verwaltungsebene über Service Bus und Azure Blob Storage. <br/>(Wenn Microsoft-Peering aktiviert ist, benötigen Sie diese Regel möglicherweise nicht.)                                                             |
| Diagnose               | 12000                                                 | TCP          | Beliebig        | Beliebig                       | ZULASSEN      | DMS verwendet diese Regel zum Sammeln von Diagnoseinformationen für die Problembehandlung.                                                                                                                      |
| SQL-Quellserver         | 1433 (oder TCP-IP-Port, an dem SQL Server lauscht) | TCP          | Beliebig        | Lokaler Adressraum | ZULASSEN      | SQL Server-Quellkonnektivität von DMS <br/>(Wenn Sie über Site-to-Site-Konnektivität verfügen, benötigen Sie diese Regel möglicherweise nicht.)                                                                                       |
| Benannte SQL Server-Instanz | 1434                                                  | UDP          | Beliebig        | Lokaler Adressraum | ZULASSEN      | Quellkonnektivität der benannten SQL Server-Instanz von DMS <br/>(Wenn Sie über Site-to-Site-Konnektivität verfügen, benötigen Sie diese Regel möglicherweise nicht.)                                                                        |
| SMB-Freigabe                 | 445                                                   | TCP          | Beliebig        | Lokaler Adressraum | ZULASSEN      | SMB-Netzwerkfreigabe, in der DMS Datenbank-Sicherungsdateien für Migrationen zu verwalteten Azure SQL-Datenbank-Instanzen und SQL Server-Instanzen auf virtuellen Azure-Computern speichert <br/>(Wenn Sie über Site-to-Site-Konnektivität verfügen, benötigen Sie diese Regel möglicherweise nicht.) |
| DMS_subnet                | Beliebig                                                   | Beliebig          | Beliebig        | DMS_Subnet                | ZULASSEN      |                                                                                                                                                                                                  |

## <a name="see-also"></a>Weitere Informationen

- [Migrieren von SQL Server zu einer verwalteten Azure SQL-Datenbank-Instanz](https://docs.microsoft.com/azure/dms/tutorial-sql-server-to-managed-instance)
- [Übersicht über die Voraussetzungen für die Verwendung von Azure Database Migration Service](https://docs.microsoft.com/azure/dms/pre-reqs)
- [Erstellen eines virtuellen Netzwerks im Azure-Portal](https://docs.microsoft.com/azure/virtual-network/quick-create-portal)

## <a name="next-steps"></a>Nächste Schritte

- Eine Übersicht über Azure Database Migration Service finden Sie im Artikel [Was ist Azure Database Migration Service?](dms-overview.md).
- Neueste Informationen zur regionalen Verfügbarkeit von Azure Database Migration Service finden Sie auf der Seite [Verfügbare Produkte nach Region](https://azure.microsoft.com/global-infrastructure/services/?products=database-migration).
