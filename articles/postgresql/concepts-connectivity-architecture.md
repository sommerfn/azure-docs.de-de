---
title: Verbindungsarchitektur in Azure Database for PostgreSQL
description: Hier wird die Verbindungsarchitektur für Ihren Azure Database for PostgreSQL-Server beschrieben.
author: kummanish
ms.author: manishku
ms.service: postgresql
ms.topic: conceptual
ms.date: 05/23/2019
ms.openlocfilehash: 5288c3025e523e767082f4ab7cfbb08805a74eaf
ms.sourcegitcommit: c22327552d62f88aeaa321189f9b9a631525027c
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 11/04/2019
ms.locfileid: "73500092"
---
# <a name="connectivity-architecture-in-azure-database-for-postgresql"></a>Verbindungsarchitektur in Azure Database for PostgreSQL
In diesem Artikel wird die Verbindungsarchitektur von Azure Database for PostgreSQL beschrieben, und Sie erfahren, wie Datenverkehr von Clients innerhalb und außerhalb von Azure an Ihre Azure Database for PostgreSQL-Datenbankinstanz weitergeleitet wird.

## <a name="connectivity-architecture"></a>Verbindungsarchitektur
Die Verbindung mit Ihrer Azure Database for PostgreSQL-Instanz wird über ein Gateway hergestellt, das eingehende Verbindungen an den physischen Ort weiterleitet, an dem sich Ihr Server in unseren Clustern befindet. Das folgende Diagramm veranschaulicht den Datenverkehrsfluss.

![Übersicht über die Verbindungsarchitektur](./media/concepts-connectivity-architecture/connectivity-architecture-overview-proxy.png)

Ein Client, der eine Verbindung mit der Datenbank herstellt, erhält eine mit dem Gateway verknüpfte Verbindungszeichenfolge. Dieses Gateway besitzt eine öffentliche IP-Adresse, die an Port 5432 lauscht. Innerhalb des Datenbankclusters wird der Datenverkehr an die entsprechende Azure Database for PostgreSQL-Instanz weitergeleitet. Für die Verbindungsherstellung mit Ihrem Server (etwa von Unternehmensnetzwerken aus) muss daher die clientseitige Firewall geöffnet werden, damit ausgehender Datenverkehr unsere Gateways erreichen kann. Weiter unten finden Sie eine vollständige, nach Region aufgeschlüsselte Liste der IP-Adressen, die von unseren Gateways verwendet werden.

## <a name="azure-database-for-postgresql-gateway-ip-addresses"></a>IP-Adressen des Azure Database for PostgreSQL-Gateways
Die folgende Tabelle enthält die primären und sekundären IP-Adressen des Azure Database for PostgreSQL-Gateways für alle Datenregionen. Die primäre IP-Adresse ist die aktuelle IP-Adresse des Gateways. Bei der zweiten IP-Adresse handelt es sich um eine Failover-IP-Adresse für den Fall, dass die primäre Adresse ausfällt. Kunden sollten wie bereits erwähnt ausgehenden Datenverkehr für beide IP-Adressen zulassen. Die zweite IP-Adresse lauscht an keinen Diensten, bis sie von Azure Database for PostgreSQL aktiviert wird, um Verbindungen zu akzeptieren.

| **Regionsname** | **Primäre IP-Adresse** | **Sekundäre IP-Adresse** |
|:----------------|:-------------|:------------------------|
| Australien (Osten) | 13.75.149.87 | 40.79.161.1 |
| Australien, Südosten | 191.239.192.109 | 13.73.109.251 |
| Brasilien Süd | 104.41.11.5 | |
| Kanada, Mitte | 40.85.224.249 | |
| Kanada, Osten | 40.86.226.166 | |
| USA (Mitte) | 23.99.160.139 | 13.67.215.62 |
| China, Osten 1 | 139.219.130.35 | |
| China, Osten 2 | 40.73.82.1 | |
| China, Norden 1 | 139.219.15.17 | |
| China, Norden 2 | 40.73.50.0 | |
| Asien, Osten | 191.234.2.139 | 52.175.33.150 |
| US, Osten 1 | 191.238.6.43 | 40.121.158.30 |
| USA (Ost) 2 | 191.239.224.107 | 40.79.84.180 * |
| Frankreich, Mitte | 40.79.137.0 | 40.79.129.1 |
| Deutschland, Mitte | 51.4.144.100 | |
| Indien, Mitte | 104.211.96.159 | |
| Indien, Süden | 104.211.224.146 | |
| Indien, Westen | 104.211.160.80 | |
| Japan, Osten | 191.237.240.43 | 13.78.61.196 |
| Japan, Westen | 191.238.68.11 | 104.214.148.156 |
| Korea, Mitte | 52.231.32.42 | |
| Korea, Süden | 52.231.200.86 |  |
| USA Nord Mitte | 23.98.55.75 | 23.96.178.199 |
| Nordeuropa | 191.235.193.75 | 40.113.93.91 |
| USA Süd Mitte | 23.98.162.75 | 13.66.62.124 |
| Südostasien | 23.100.117.95 | 104.43.15.0 |
| Südafrika, Norden | 102.133.152.0 | |
| Südafrika, Westen | 102.133.24.0 | |
| Vereinigte Arabische Emirate, Norden | 65.52.248.0 | |
| UK, Süden | 51.140.184.11 | |
| UK, Westen | 51.141.8.11| |
| Europa, Westen | 191.237.232.75 | 40.68.37.158 |
| USA, Westen 1 | 23.99.34.75 | 104.42.238.205 |
| USA, Westen 2 | 13.66.226.202 | |
||||

> [!NOTE]
> *USA, Osten 2* verfügt auch über eine tertiäre IP-Adresse von `52.167.104.0`.

## <a name="next-steps"></a>Nächste Schritte

* [Erstellen und Verwalten von Firewallregeln für Azure-Datenbank für PostgreSQL mithilfe des Azure-Portals](./howto-manage-firewall-using-portal.md)
* [Erstellen und Verwalten von Firewallregeln für Azure-Datenbank für PostgreSQL mithilfe der Azure CLI](./howto-manage-firewall-using-cli.md)
