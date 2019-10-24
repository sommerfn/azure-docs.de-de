---
title: Leistungsoptimierung mit Zwischenspeichern von Resultsets | Microsoft-Dokumentation
description: Funktionsübersicht
services: sql-data-warehouse
author: XiaoyuMSFT
manager: craigg
ms.service: sql-data-warehouse
ms.topic: conceptual
ms.subservice: development
ms.date: 10/10/2019
ms.author: xiaoyul
ms.reviewer: nidejaco;
ms.openlocfilehash: f6323501fc0078677c4c0e2cd0e43a15583df29b
ms.sourcegitcommit: 12de9c927bc63868168056c39ccaa16d44cdc646
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 10/17/2019
ms.locfileid: "72513984"
---
# <a name="performance-tuning-with-result-set-caching"></a>Leistungsoptimierung mit Zwischenspeichern von Resultsets  
Wenn das Zwischenspeichern von Resultsets aktiviert ist, werden die Abfrageergebnisse von Azure SQL Data Warehouse in der Benutzerdatenbank automatisch zwischengespeichert, damit sie wiederholt verwendet werden können.  Dies ermöglicht es, dass nachfolgende Abfrageausführungen die Ergebnisse direkt aus dem permanenten Cache erhalten und so keine Neuberechnung erforderlich ist.   Durch das Zwischenspeichern von Resultsets wird die Abfrageleistung verbessert und die Nutzung von Computeressourcen verringert.  Außerdem belegen Abfragen, die das zwischengespeicherte Resultset verwenden, keine Parallelitätsslots und werden daher auf vorhandene Parallelitätslimits nicht angerechnet. Aus Sicherheitsgründen können Benutzer nur dann auf die zwischengespeicherten Ergebnisse zugreifen, wenn sie über dieselben Datenzugriffsberechtigungen wie die Benutzer verfügen, die die zwischengespeicherten Ergebnisse erstellen.  

## <a name="key-commands"></a>Schlüsselbefehle
[Aktivieren/Deaktivieren des Zwischenspeicherns von Resultsets für eine Benutzerdatenbank](https://docs.microsoft.com/sql/t-sql/statements/alter-database-transact-sql-set-options?view=azure-sqldw-latest)

[Aktivieren/Deaktivieren des Zwischenspeicherns von Resultsets für eine Sitzung](https://docs.microsoft.com/sql/t-sql/statements/set-result-set-caching-transact-sql?view=azure-sqldw-latest)

[Überprüfen der Größe des zwischengespeicherten Resultsets](https://docs.microsoft.com/sql/t-sql/database-console-commands/dbcc-showresultcachespaceused-transact-sql?view=azure-sqldw-latest)  

[Bereinigen des Caches](https://docs.microsoft.com/sql/t-sql/database-console-commands/dbcc-dropresultsetcache-transact-sql?view=azure-sqldw-latest)

## <a name="whats-not-cached"></a>Nicht zwischengespeicherte Elemente  

Sobald das Zwischenspeichern von Resultsets für eine Datenbank aktiviert wurde, werden die Ergebnisse für alle Abfragen so lange zwischengespeichert, bis der Cache voll ist – mit Ausnahme der folgenden Abfragen:
- Abfragen, die nicht deterministische Funktionen verwenden, z.B. „DateTime.Now()“
- Abfragen, die benutzerdefinierte Funktionen verwenden
- Abfragen, die Daten mit einer Zeilengröße von mehr als 64 KB zurückgeben

Abfragen mit umfangreichen Resultsets (z.B. > 1 Million Zeilen) können die Leistung während der ersten Ausführung beim Erstellen des Ergebniscaches möglicherweise verlangsamen.

Sicherheit auf Zeilenebene wird durch das Zwischenspeichern von Resultsets nicht unterstützt.  

## <a name="when-cached-results-are-used"></a>Wann zwischengespeicherte Ergebnisse verwendet werden

Das zwischengespeicherte Resultset wird für eine Abfrage erneut verwendet, wenn alle folgenden Anforderungen erfüllt sind:
- Der Benutzer, der die Abfrage ausführt, kann auf alle Tabellen zugreifen, auf die in der Abfrage verwiesen wird.
- Es gibt eine exakte Übereinstimmung zwischen der neuen Abfrage und der vorherigen Abfrage, die den Resultsetcache generiert hat.
- In den Tabellen, aus denen das zwischengespeicherte Resultset generiert wurde, gab es keine Daten- oder Schemaänderungen.

Führen Sie diesen Befehl aus, um zu überprüfen, ob eine Abfrage mit einem Ergebniscachetreffer oder -fehler ausgeführt wurde. Bei einem Cachetreffer gibt „result_cache_hit“ den Wert „1“ zurück.

```sql
SELECT request_id, command, result_cache_hit FROM sys.pdw_exec_requests 
WHERE request_id = <'Your_Query_Request_ID'>
```

## <a name="manage-cached-results"></a>Verwalten von zwischengespeicherten Ergebnissen 

Die maximale Größe des Resultsetcaches ist 1 TB pro Datenbank.  Wenn sich die zugrundeliegenden Abfragedaten ändern, werden die zwischengespeicherten Ergebnisse automatisch ungültig gemacht.  

Die Cacheentfernung wird von Azure SQL Data Warehouse automatisch nach diesem Zeitplan verwaltet: 
- Alle 48 Stunden, wenn das Resultset nicht verwendet oder ungültig gemacht wurde. 
- Wenn sich der Resultsetcache der maximalen Größe nähert.

Benutzer können den gesamten Resultsetcache mit einer der folgenden Optionen manuell leeren: 
- Deaktivieren des Resultsetcachefeatures für die Datenbank 
- Ausführen des Datenbankkonsolenbefehls (DBCC) DROPRESULTSETCACHE, während eine Verbindung zur Datenbank besteht

Durch das Anhalten einer Datenbank wird das zwischengespeicherte Resultset nicht geleert.  

## <a name="next-steps"></a>Nächste Schritte
Weitere Hinweise zur Entwicklung finden Sie in der [Entwicklungsübersicht für SQL Data Warehouse](sql-data-warehouse-overview-develop.md). 
