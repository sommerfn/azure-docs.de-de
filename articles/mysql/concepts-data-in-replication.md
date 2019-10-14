---
title: Replizieren von Daten in Azure Database for MySQL
description: Erfahren Sie mehr über das Verwenden der Datenreplikation zum Synchronisieren von Daten von einem externen Server mit dem Dienst Azure Database for MySQL.
author: ajlam
ms.author: andrela
ms.service: mysql
ms.topic: conceptual
ms.date: 09/13/2019
ms.openlocfilehash: b501a1f1ea54aff5617932dc5085d6d19f86976c
ms.sourcegitcommit: c2e7595a2966e84dc10afb9a22b74400c4b500ed
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 10/05/2019
ms.locfileid: "71970350"
---
# <a name="replicate-data-into-azure-database-for-mysql"></a>Replizieren von Daten in Azure Database for MySQL

Die Datenreplikation ermöglicht das Synchronisieren von Daten von einem externen MySQL-Server mit dem Dienst Azure Database for MySQL. Der externe Server kann lokal, in virtuellen Computern oder in einem Datenbankdienst vorhanden sein, der von anderen Cloudanbietern gehostet wird. Die Replikation eingehender Daten basiert auf der nativen MySQL-Replikation, die wiederum auf der Position der binären Protokolldatei (binlog) basiert. Weitere Informationen zur binlog-Replikation finden Sie unter [Binary Log File Position Based Replication Configuration Overview](https://dev.mysql.com/doc/refman/5.7/en/binlog-replication-configuration-overview.html) (Konfiguration der auf der Position der binären Protokolldatei basierenden Replikation – Übersicht). 

## <a name="when-to-use-data-in-replication"></a>Szenarien für die Verwendung der Datenreplikation für Azure Database for MySQL
Zu den wichtigsten Szenarien, bei denen die Datenreplikation für Azure Database for MySQL infrage kommt, zählen Folgende:

- **Synchronisierung von Hybriddaten:** Mit der Datenreplikation für Azure Database for MySQL können Sie Daten zwischen Ihren lokalen Servern und Azure Database for MySQL synchron halten. Eine solche Synchronisierung ist für die Erstellung von Hybridanwendungen nützlich. Diese Methode ist optimal geeignet, wenn Sie über einen lokalen Datenbankserver verfügen, die Daten jedoch in eine Region verschieben möchten, die sich näher bei den Endbenutzern befindet.
- **Synchronisierung von Daten in verschiedenen Clouds:** Verwenden Sie bei komplexen Cloudlösungen die Datenreplikation für Azure Database for MySQL, um Daten zwischen Azure Database for MySQL und unterschiedlichen Cloudanbietern zu synchronisieren, einschließlich virtueller Computer und Datenbankdienste, die in diesen Clouds gehostet werden.
 
Verwenden Sie bei Migrationsszenarien den [Azure Database Migration Service](https://azure.microsoft.com/services/database-migration/)(DMS).

## <a name="limitations-and-considerations"></a>Einschränkungen und Aspekte

### <a name="data-not-replicated"></a>Nicht replizierte Daten
Die [*mysql-Systemdatenbank*](https://dev.mysql.com/doc/refman/5.7/en/system-database.html) auf dem Masterserver wird nicht repliziert. Änderungen an Konten und Berechtigungen auf dem Masterserver werden nicht repliziert. Wenn Sie ein Konto auf dem Masterserver erstellen und dieses Konto über Zugriff auf den Replikatserver verfügen muss, erstellen Sie dasselbe Konto manuell auf dem Replikatserver. Einen Überblick über die Tabellen, die in der Systemdatenbank enthalten sind, finden Sie im [Leitfaden zu MySQL](https://dev.mysql.com/doc/refman/5.7/en/system-database.html).

### <a name="requirements"></a>Requirements (Anforderungen)
- Der Masterserver muss mindestens die MySQL-Version 5.6 aufweisen. 
- Die Versionen des Master- und des Replikatservers müssen identisch sein. Beispielsweise müssen beide die Version MySQL 5.6 oder beide MySQL 5.7 aufweisen.
- Jede Tabelle muss über einen Primärschlüssel verfügen.
- Der Masterserver sollte die MySQL InnoDB-Engine verwenden.
- Benutzer müssen über Berechtigungen zum Konfigurieren der binären Protokollierung und zum Erstellen neuer Benutzer auf dem Masterserver verfügen.
- Wenn für den Masterserver SSL aktiviert ist, vergewissern Sie sich, dass das für die Domäne bereitgestellte SSL-Zertifizierungsstellenzertifikat in die gespeicherte Prozedur `mysql.az_replication_change_master` eingefügt wurde. Sehen Sie sich die folgenden [Beispiele](https://docs.microsoft.com/azure/mysql/howto-data-in-replication#link-master-and-replica-servers-to-start-data-in-replication) und den Parameter `master_ssl_ca` an.
- Stellen Sie sicher, dass die IP-Adresse des Masterservers den Firewallregeln des Azure Database for MySQL-Replikatservers hinzugefügt wurde. Aktualisieren Sie Firewallregeln über das [Azure-Portal](https://docs.microsoft.com/azure/mysql/howto-manage-firewall-using-portal) oder über die [Azure-Befehlszeilenschnittstelle](https://docs.microsoft.com/azure/mysql/howto-manage-firewall-using-cli).
- Vergewissern Sie sich, dass für den Computer, der den Masterserver hostet, sowohl ein- als auch ausgehender Datenverkehr am Port 3306 zugelassen wird.
- Stellen Sie sicher, dass der Masterserver eine **öffentliche IP-Adresse** aufweist, das DNS öffentlich zugänglich ist oder ein vollqualifizierter Domänenname (FQDN) verfügbar ist.

### <a name="other"></a>Andere
- Die Datenreplikation wird nur in den Tarifen Universell und Arbeitsspeicheroptimiert unterstützt.
- IDs globaler Transaktionen (GTIDs) werden nicht unterstützt.

## <a name="next-steps"></a>Nächste Schritte
- Erfahren Sie, wie Sie [die Datenreplikation für Azure Database for MySQL einrichten](howto-data-in-replication.md).
- Erfahren Sie mehr über das [Replizieren in Azure mit Lesereplikaten](concepts-read-replicas.md).
- Erfahren Sie, wie Sie [Daten mit minimalen Ausfallzeiten mithilfe von DMS migrieren](howto-migrate-online.md) können.