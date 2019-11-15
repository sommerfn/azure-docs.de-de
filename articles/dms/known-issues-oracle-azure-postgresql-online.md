---
title: Artikel zu bekannten Problemen/Einschränkungen bei Onlinemigrationen von Oracle zum Azure Database for PostgreSQL-Einzelserver | Microsoft-Dokumentation
description: Erfahren Sie mehr über bekannte Probleme/Einschränkungen bei Onlinemigrationen von Oracle zu Azure Database for PostgreSQL.
services: database-migration
author: HJToland3
ms.author: jtoland
manager: craigg
ms.reviewer: craigg
ms.service: dms
ms.workload: data-services
ms.custom: mvc
ms.topic: article
ms.date: 11/05/2019
ms.openlocfilehash: ecd2750926fd04c84d2d6b48f5fde06363dd15df
ms.sourcegitcommit: c62a68ed80289d0daada860b837c31625b0fa0f0
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 11/05/2019
ms.locfileid: "73606757"
---
# <a name="known-issuesmigration-limitations-with-online-migrations-from-oracle-to-azure-db-for-postgresql-single-server"></a>Bekannte Probleme/Einschränkungen bei Onlinemigrationen von Oracle zum Azure DB for PostgreSQL-Einzelserver

In den folgenden Abschnitten werden bekannte Probleme und Einschränkungen bei Onlinemigrationen von Oracle zum Azure Database for PostgreSQL-Einzelserver beschrieben.

## <a name="oracle-versions-supported-as-a-source-database"></a>Als Quelldatenbank unterstützte Oracle-Versionen

Azure Database Migration Service unterstützt Verbindungen mit:

- Oracle, Version 10g, 11g und 12c
- Oracle Enterprise, Standard, Express und Personal Edition.

Azure Database Migration Service unterstützt keine Verbindungen mit mehrinstanzenfähigen Containerdatenbanken (CDSBs).

## <a name="postgresql-versions-supported-as-a-target-database"></a>Als Zieldatenbank unterstützte PostgreSQL-Versionen

Azure Database Migration Service unterstützt Migrationen zu Azure Database for PostgreSQL-Einzelservern der Version 9.5, 9.6, 10 und 11. Aktuelle Informationen zur Versionsunterstützung für Azure Database for PostgreSQL-Einzelservern finden Sie im Artikel [Unterstützte PostgreSQL-Datenbankversionen](https://docs.microsoft.com/azure/postgresql/concepts-supported-versions).

## <a name="datatype-limitations"></a>Einschränkungen bei Datentypen

Die folgenden Datentypen werden **nicht** migriert:

- BFILE
- ROWID
- REF
- UROWID
- ANYDATA
- SDO_GEOMETRY
- Geschachtelte Tabellen
- Benutzerdefinierte Datentypen
- Notizen
- Virtuelle Spalten
- Materialisierte Sichten, die auf der ROWID-Spalte basieren

Außerdem werden leere BLOB-/CLOB-Spalten im Ziel NULL zugeordnet.

## <a name="lob-limitations"></a>LOB-Einschränkungen

- Wenn der LOB-Modus mit Größenbeschränkung aktiviert ist, werden leere LOBs in der Oracle-Quelle als NULL-Werte repliziert.
- Lange Objektnamen (über 30 Bytes) werden nicht unterstützt.
- Daten in LONG- und LONG RAW-Spalten dürfen maximal 64 K umfassen. Daten über 64 K werden abgeschnitten.
- Ausschließlich in Oracle 12 werden keine Änderungen an LOB-Spalten unterstützt (migriert).
- Aktualisierungen an XMLTYPE- und LOB-Spalten werden nicht unterstützt (migriert).

## <a name="known-issues-and-limitations"></a>Bekannte Probleme und Einschränkungen

- Kunden müssen SYSDBA verwenden, um eine Verbindung mit Oracle herzustellen.
- Datenänderungen durch Vorgänge auf Partitions-/Unterpartitionsebene (ADD, DROP, EXCHANGE und TRUNCATE) werden nicht migriert und können folgende Fehler verursachen:
  - Bei ADD-Vorgängen kann durch Aktualisierungen und Löschungen der hinzugefügten Daten die Warnung „0 Zeilen betroffen“ zurückgegeben werden.
  - Bei DROP- und TRUNCATE-Vorgängen können neu eingefügte Daten Fehler aufgrund von Duplikaten verursachen.
  - Bei EXCHANGE-Vorgängen können sowohl Fehler vom Typ „0 Zeilen betroffen“ als auch Duplikatfehler auftreten.
- Tabellen mit einem Apostroph im Namen können nicht repliziert werden.
