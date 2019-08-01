---
title: Gateway Migrationshinweis für Azure SQL Datenbank von Gen2 nach Gen3 | Microsoft Docs
description: Artikel informiert Benutzer über die Migration von Azure SQL Database Gateways IP-Adressen
services: sql-database
ms.service: sql-database
ms.subservice: development
ms.custom: ''
ms.topic: conceptual
author: rohitnayakmsft
ms.author: rohitna
ms.reviewer: vanto
ms.date: 07/01/2019
ms.openlocfilehash: 85691464684ff327c01a85bf357514f447564dd7
ms.sourcegitcommit: 7c4de3e22b8e9d71c579f31cbfcea9f22d43721a
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 07/26/2019
ms.locfileid: "68568114"
---
# <a name="azure-sql-database-traffic-migration-to-newer-gateways"></a>Azure SQL Database Traffic-Migration auf neuere Gateways

Da sich die Azure-Infrastruktur verbessert, wird Microsoft die Hardware regelmäßig aktualisieren, um sicherzustellen, dass wir das bestmögliche Kundenerlebnis bieten. In den kommenden Monaten planen wir, in einigen Regionen Gateways hinzuzufügen, die auf neueren Hardware-Generationen basieren, und Gateways stillzulegen, die auf älterer Hardware basieren.  

Kunden werden per E-Mail und im Azure-Portal rechtzeitig vor jeder Änderung der in jeder Region verfügbaren Gateways informiert. Die aktuellsten Informationen werden in der Tabelle der IP-Adressen des[ Azure SQL Datenbank-Gateways](sql-database-connectivity-architecture.md#azure-sql-database-gateway-ip-addresses) gespeichert.

## <a name="impact-of-this-change"></a>Einfluss dieser Änderung

Die erste Runde der Stilllegung von Gateway ist für den 1. September 2019 in den folgenden Regionen geplant:

- USA (Westen)
- Europa, Westen
- East US
- USA (Mitte)
- Südostasien
- USA Süd Mitte
- Nordeuropa
- USA Nord Mitte
- Japan, Westen
- Japan, Osten
- USA (Ost) 2
- Asien, Osten

Die außer Betrieb genommene IP-Adresse akzeptiert keinen Datenverkehr mehr und alle neuen Verbindungsversuche werden an eines der Gateways in der Region weitergeleitet.

Wo Sie die Auswirkungen dieser Änderung nicht sehen werden:

- Kunden, die die Umleitung als Verbindungsrichtlinie verwenden, werden keine Auswirkungen feststellen.
- Verbindungen zur SQL-Datenbank aus Azure heraus und über Service Tags werden nicht beeinträchtigt.
- Verbindungen zur SQL-Datenbank aus Azure heraus und über Service Tags werden nicht beeinträchtigt. Informationen zu unterstützten JDBC-Versionen finden Sie unter [Microsoft JDBC-Treiber für SQL Server herunterladen](/sql/connect/jdbc/download-microsoft-jdbc-driver-for-sql-server).

## <a name="what-to-do-you-do-if-youre-affected"></a>Was Sie tun müssen, wenn Sie betroffen sind

Wir empfehlen, dass Sie ausgehenden Datenverkehr zu IP-Adressen für alle [Azure SQL Database Gateway-IP-Adressen](sql-database-connectivity-architecture.md#azure-sql-database-gateway-ip-addresses) in der Region auf dem TCP-Port 1433 und dem Portbereich 11000-11999 in Ihrer Firewall-Vorrichtung zulassen. Weitere Informationen zu Portbereichen finden Sie unter [Verbindungsrichtlinie](sql-database-connectivity-architecture.md#connection-policy).

Verbindungen von Anwendungen, die den Microsoft JDBC Driver unter Version 4.0 verwenden, können die Zertifikatsprüfung nicht bestehen. Ältere Versionen von Microsoft JDBC basieren auf Common Name (CN) im Feld des Zertifikats. Die Minderung besteht darin, sicherzustellen, dass die Eigenschaft hostNameInCertificate auf *.database.windows.net gesetzt ist. Weitere Informationen zum Einstellen der Eigenschaft hostNameInCertificate finden Sie unter [Verbindung mit SSL-Verschlüsselung herstellen](/sql/connect/jdbc/connecting-with-ssl-encryption).

Wenn die oben genannten Minderungen nicht funktionieren, stellen Sie eine Supportanfrage für die SQL-Datenbank unter der folgenden URL: https://aka.ms/getazuresupport

## <a name="next-steps"></a>Nächste Schritte

- Erfahren Sie mehr über die [Azure SQL Connectivity Architecture](sql-database-connectivity-architecture.md)