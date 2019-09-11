---
title: Replizieren von Daten in Azure Database for MariaDB
description: In diesem Artikel wird die Datenreplikation für Azure Database for MariaDB beschrieben.
author: ajlam
ms.author: andrela
ms.service: mariadb
ms.topic: conceptual
ms.date: 07/11/2019
ms.openlocfilehash: 28c2c01e85120ec17e6f782fb0686a627d50d0d0
ms.sourcegitcommit: d200cd7f4de113291fbd57e573ada042a393e545
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 08/29/2019
ms.locfileid: "70136744"
---
# <a name="replicate-data-into-azure-database-for-mariadb"></a>Replizieren von Daten in Azure Database for MariaDB

Mithilfe der Datenreplikation können Sie Daten von einem MariaDB-Server, der lokal, auf virtuellen Computern oder von Datenbankdiensten ausgeführt wird, die von anderen Cloudanbietern gehostet werden, mit dem Azure Database for MariaDB-Dienst synchronisieren. Die Datenreplikation basiert auf der nativen MariaDB-Replikation, die wiederum auf der Position der binären Protokolldatei (binlog) basiert. Weitere Informationen zur binlog-Replikation finden Sie [Übersicht über die binlog Replikation](https://mariadb.com/kb/en/library/replication-overview/).

## <a name="when-to-use-data-in-replication"></a>Szenarien für die Verwendung der Datenreplikation für Azure Database for MySQL
Zu den wichtigsten Szenarien, bei denen die Datenreplikation für Azure Database for MySQL infrage kommt, zählen Folgende:

- **Synchronisierung von Hybriddaten:** Mit der Datenreplikation für Azure Database for MySQL können Sie Daten zwischen Ihren lokalen Servern und Azure Database for MariaDB synchron halten. Eine solche Synchronisierung ist für die Erstellung von Hybridanwendungen nützlich. Diese Methode ist optimal geeignet, wenn Sie über einen lokalen Datenbankserver verfügen, die Daten jedoch in eine Region verschieben möchten, die sich näher bei den Endbenutzern befindet.
- **Synchronisierung von Daten in verschiedenen Clouds:** Verwenden Sie bei komplexen Cloudlösungen die Datenreplikation für Azure Database for MariaDB, um Daten zwischen Azure Database for MySQL und unterschiedlichen Cloudanbietern zu synchronisieren, einschließlich virtueller Computer und Datenbankdienste, die in diesen Clouds gehostet werden.

## <a name="limitations-and-considerations"></a>Einschränkungen und Aspekte

### <a name="data-not-replicated"></a>Nicht replizierte Daten
Die [*Systemdatenbank „mysql“* ](https://mariadb.com/kb/en/library/the-mysql-database-tables/) auf dem Masterserver wird nicht repliziert. Änderungen an Konten und Berechtigungen auf dem Masterserver werden nicht repliziert. Wenn Sie ein Konto auf dem Masterserver erstellen und dieses Konto über Zugriff auf den Replikatserver verfügen muss, erstellen Sie dasselbe Konto manuell auf dem Replikatserver. Einen Überblick über die Tabellen, die in der Systemdatenbank enthalten sind, finden Sie in der [Dokumentation zu MariaDB](https://mariadb.com/kb/en/library/the-mysql-database-tables/).

### <a name="requirements"></a>Requirements (Anforderungen)
- Der Masterserver muss mindestens die MariaDB-Version 10.2 aufweisen.
- Die Versionen des Master- und des Replikatservers müssen identisch sein. Beispiel: Auf beiden muss MariaDB-Version 10.2 vorliegen.
- Jede Tabelle muss über einen Primärschlüssel verfügen.
- Der Masterserver sollte die InnoDB-Engine verwenden.
- Benutzer müssen über Berechtigungen zum Konfigurieren der binären Protokollierung und zum Erstellen neuer Benutzer auf dem Masterserver verfügen.
- Wenn für den Masterserver SSL aktiviert ist, vergewissern Sie sich, dass das für die Domäne bereitgestellte SSL-Zertifizierungsstellenzertifikat in die gespeicherte Prozedur `mariadb.az_replication_change_master` eingefügt wurde. Sehen Sie sich die folgenden [Beispiele](https://docs.microsoft.com/azure/mariadb/howto-data-in-replication#link-the-master-and-replica-servers-to-start-data-in-replication) und den Parameter `master_ssl_ca` an.
- Vergewissern Sie sich, dass die IP-Adresse des Masterservers den Firewallregeln des Azure Database for MariaDB-Replikatservers hinzugefügt wurde. Aktualisieren Sie Firewallregeln über das [Azure-Portal](https://docs.microsoft.com/azure/mariadb/howto-manage-firewall-portal) oder über die [Azure-Befehlszeilenschnittstelle](https://docs.microsoft.com/azure/mariadb/howto-manage-firewall-cli).
- Vergewissern Sie sich, dass für den Computer, der den Masterserver hostet, sowohl ein- als auch ausgehender Datenverkehr am Port 3306 zugelassen wird.
- Stellen Sie sicher, dass der Masterserver über eine **öffentliche IP-Adresse** verfügt oder dass das DNS öffentlich zugänglich ist.

### <a name="other"></a>Andere
- Die Datenreplikation wird nur in den Tarifen Universell und Arbeitsspeicheroptimiert unterstützt.

## <a name="next-steps"></a>Nächste Schritte
- Erfahren Sie, wie Sie [die Datenreplikation einrichten](howto-data-in-replication.md).
