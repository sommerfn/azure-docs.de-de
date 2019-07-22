---
title: Verwenden von T-SQL-Ansichten in Azure SQL Data Warehouse | Microsoft-Dokumentation
description: Tipps für die Verwendung von T-SQL-Sichten in Azure SQL Data Warehouse zum Entwickeln von Lösungen.
services: sql-data-warehouse
author: XiaoyuL-Preview
manager: craigg
ms.service: sql-data-warehouse
ms.topic: conceptual
ms.subservice: development
ms.date: 04/17/2018
ms.author: xiaoyul
ms.reviewer: igorstan
ms.openlocfilehash: e8d516cfd764f947bd2fe7fc25f6394c313c0d9a
ms.sourcegitcommit: ccb9a7b7da48473362266f20950af190ae88c09b
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 07/05/2019
ms.locfileid: "67595497"
---
# <a name="views-in-azure-sql-data-warehouse"></a>Sichten in Azure SQL Data Warehouse
Tipps für die Verwendung von T-SQL-Sichten in Azure SQL Data Warehouse zum Entwickeln von Lösungen. 

## <a name="why-use-views"></a>Gründe für die Verwendung von Sichten
Sichten können auf verschiedene Weisen zur Verbesserung der Qualität Ihrer Lösung verwendet werden.  Dieser Artikel enthält einige Beispiele dafür, wie Sie Ihre Lösung mit Sichten bereichern können, sowie Informationen zu den Einschränkungen, die zu berücksichtigen sind.


> [!IMPORTANT]
> Sehen Sie sich die Syntax für die neue materialisierte Sicht unter [CREATE MATERIALIZED VIEW AS SELECT](/sql/t-sql/statements/create-materialized-view-as-select-transact-sql?view=azure-sqldw-latest) an.  Weitere Informationen finden Sie in den [Versionshinweisen](/azure/sql-data-warehouse/release-notes-10-0-10106-0).
>


> [!NOTE]
> Die Syntax für CREATE VIEW wird in diesem Artikel nicht erörtert. Weitere Informationen finden Sie in der Dokumentation zu [CREATE VIEW](/sql/t-sql/statements/create-view-transact-sql).
> 

## <a name="architectural-abstraction"></a>Architekturabstraktion

Ein häufig verwendetes Anwendungsmuster ist das erneute Erstellen von Tabellen mit CREATE TABLE AS SELECT (CTAS) gefolgt von einem Muster zur Objektumbenennung beim Laden von Daten.

Im folgenden Beispiel werden einer Datumsdimension neue Datumsdatensätze hinzugefügt. Beachten Sie, wie eine neue Tabelle (DimDate_New) zuerst erstellt und dann umbenannt wird, um die ursprüngliche Version der Tabelle zu ersetzen.

```sql
CREATE TABLE dbo.DimDate_New
WITH (DISTRIBUTION = ROUND_ROBIN
, CLUSTERED INDEX (DateKey ASC)
)
AS
SELECT *
FROM   dbo.DimDate  AS prod
UNION ALL
SELECT *
FROM   dbo.DimDate_stg AS stg
;

RENAME OBJECT DimDate TO DimDate_Old;
RENAME OBJECT DimDate_New TO DimDate;

```

Dieser Ansatz kann aber auch dazu führen, dass Tabellen in der Sicht eines Benutzers ein- und ausgeblendet und Fehlermeldungen der Art „Tabelle nicht vorhanden“ angezeigt werden. Sichten können verwendet werden, um eine konsistente Darstellungsschicht für Benutzer bereitzustellen, während die zugrunde liegenden Objekte umbenannt werden. Wenn Zugriff auf Daten über Sichten bereitgestellt wird, sind Benutzer nicht auf die Sichtbarkeit der zugrunde liegenden Tabellen angewiesen. Diese Ebene bietet eine konsistente Benutzererfahrung und ermöglicht gleichzeitig die Weiterentwicklung des Datenmodells durch die Data Warehouse-Designer. Dank der Möglichkeit, die zugrunde liegenden Tabellen weiterentwickeln zu können, können Designer die Leistung beim Laden von Daten mit CTAS maximieren.   

## <a name="performance-optimization"></a>Leistungsoptimierung
Sichten können auch genutzt werden, um leistungsoptimierte Verknüpfungen zwischen Tabellen durchzusetzen. Beispielsweise kann eine Sicht einen redundanten Verteilungsschlüssel als Teil des Verknüpfungskriteriums enthalten, um die Datenverschiebung zu minimieren. Ein weiterer Vorteil einer Sicht kann beispielsweise sein, eine bestimmte Abfrage oder einen Verknüpfungshinweis zu erzwingen. Durch den Einsatz von Sichten auf diese Weise wird sichergestellt, dass Verknüpfungen immer optimal durchgeführt werden, und Benutzer müssen sich nicht mehr das richtige Konstrukt für ihre Verknüpfungen merken.

## <a name="limitations"></a>Einschränkungen
Sichten in SQL Data Warehouse werden ausschließlich als Metadaten gespeichert. Daher sind die folgenden Optionen nicht verfügbar:

* Es gibt keine Schemabindungsoption.
* Basistabellen können nicht über die Ansicht aktualisiert werden.
* Sichten können nicht für temporäre Tabellen erstellt werden.
* EXPAND/NOEXPAND-Hinweise werden nicht unterstützt.
* Es sind keine indizierten Sichten in SQL Data Warehouse verfügbar.

## <a name="next-steps"></a>Nächste Schritte
Weitere Hinweise zur Entwicklung finden Sie in der [Entwicklungsübersicht für SQL Data Warehouse](sql-data-warehouse-overview-develop.md).


