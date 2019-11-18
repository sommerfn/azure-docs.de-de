---
title: Verwenden von Bezeichnungen zum Instrumentieren von Abfragen
description: Tipps zum Verwenden von Bezeichnungen zum Instrumentieren von Abfragen in Azure SQL Data Warehouse für die Entwicklung von Lösungen
services: sql-data-warehouse
author: XiaoyuMSFT
manager: craigg
ms.service: sql-data-warehouse
ms.topic: conceptual
ms.subservice: query
ms.date: 04/17/2018
ms.author: xiaoyul
ms.reviewer: igorstan
ms.custom: seo-lt-2019
ms.openlocfilehash: 4ca0b3564418aafe774158057bc3efd541f71f66
ms.sourcegitcommit: 609d4bdb0467fd0af40e14a86eb40b9d03669ea1
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 11/06/2019
ms.locfileid: "73692822"
---
# <a name="using-labels-to-instrument-queries-in-azure-sql-data-warehouse"></a>Verwenden von Bezeichnungen zum Instrumentieren von Abfragen in Azure SQL Data Warehouse
Tipps zum Verwenden von Bezeichnungen zum Instrumentieren von Abfragen in Azure SQL Data Warehouse für die Entwicklung von Lösungen


## <a name="what-are-labels"></a>Was sind Bezeichnungen?
In SQL Data Warehouse wird das Konzept der „Abfragebezeichnungen“ unterstützt. Bevor wir darauf näher eingehen, sehen wir uns ein Beispiel an:

```sql
SELECT *
FROM sys.tables
OPTION (LABEL = 'My Query Label')
;
```

Mit dieser letzten Zeile wird die Abfrage mit der Zeichenfolge „My Query Label“ versehen. Dieses Tag ist besonders hilfreich, da die Bezeichnung über die DMVs abgefragt werden kann. Abfragen für Bezeichnungen stellen einen Mechanismus dar, um fehlerhafte Abfragen zu suchen und den Fortschritt über eine ELT-Ausführung zu ermitteln.

Eine gute Namenskonvention ist wirklich hilfreich. Beispielsweise ist eine Bezeichnung, die mit „PROJEKT“, „PROZEDUR“, „ANWEISUNG“ oder „KOMMENTAR“ beginnt, hilfreich, um die Abfrage im Code der Quellcodeverwaltung eindeutig identifizieren zu können.

Die folgende Abfrage verwendet zum Suchen nach Bezeichnungen eine dynamische Verwaltungssicht.

```sql
SELECT  *
FROM    sys.dm_pdw_exec_requests r
WHERE   r.[label] = 'My Query Label'
;
```

> [!NOTE]
> Es ist wichtig, dass Sie das Wort „label“ bei Abfragen in eckige Klammern oder doppelte Anführungszeichen setzen. Das Wort „label“ ist ein reserviertes Wort und verursacht einen Fehler, wenn es nicht begrenzt wird. 
> 
> 

## <a name="next-steps"></a>Nächste Schritte
Weitere Hinweise zur Entwicklung finden Sie in der [Entwicklungsübersicht](sql-data-warehouse-overview-develop.md).


