---
title: Workloadverwaltung
description: Leitfaden zum Implementieren der Workloadverwaltung in Azure SQL Data Warehouse.
services: sql-data-warehouse
author: ronortloff
manager: craigg
ms.service: sql-data-warehouse
ms.topic: conceptual
ms.subservice: workload-management
ms.date: 10/30/2019
ms.author: rortloff
ms.reviewer: jrasnick
ms.custom: seo-lt-2019
ms.openlocfilehash: 2563ea4ab498c11c846cfe79f0e668f7d491c2e7
ms.sourcegitcommit: 609d4bdb0467fd0af40e14a86eb40b9d03669ea1
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 11/06/2019
ms.locfileid: "73692358"
---
# <a name="what-is-workload-management"></a>Worum geht es bei der Workloadverwaltung?

Das Ausführen gemischter Workloads kann bei ausgelasteten Systemen zu Ressourcenproblemen führen.  Lösungsarchitekten suchen nach Möglichkeiten zum Trennen klassischer Data Warehousing-Aktivitäten (z.B. Laden, Transformieren und Abfragen von Daten), um sicherzustellen, dass genügend Ressourcen zum Einhalten von SLAs vorhanden sind.  

Die Isolation physischer Server kann zu Bereichen in einer Infrastruktur führen, die nicht ausgelastet, überlastet oder in einem Zustand sind, in dem Caches ständig auf das Starten und Beenden von Hardware vorbereitet werden.  Mit einem erfolgreichen Verwaltungsschema für Workloads werden Ressourcen effektiv verwaltet, eine hochgradig effiziente Ressourcennutzung sichergestellt und die Rendite (ROI) maximiert.

Eine Data Warehouse-Workload bezieht sich auf alle Vorgänge, die in Bezug auf ein Data Warehouse ablaufen müssen. Die Tiefe und Breite dieser Komponenten hängt vom Reifegrad des Data Warehouse ab.  Die Data Warehouse-Workload umfasst Folgendes: 
- Den gesamte Prozess des Ladens von Daten in das Warehouse 
- Ausführen von Data Warehouse-Analyse und Berichterstellung
- Verwalten von Daten im Data Warehouse 
- Exportieren von Daten aus dem Data Warehouse

Die Leistungskapazität eines Data Warehouse richtet sich nach den [Data Warehouse-Einheiten](what-is-a-data-warehouse-unit-dwu-cdwu.md).
- Zum Anzeigen der zugewiesenen Ressourcen für alle Leistungsprofile lesen Sie die Informationen unter [Speicher- und Parallelitätsgrenzwerte]memory-concurrency-limits.md).
- Für die Anpassung der Kapazität können Sie [zentral hoch- oder herunterskalieren](quickstart-scale-compute-portal.md).


## <a name="workload-management-concepts"></a>Workloadverwaltungskonzepte
In der Vergangenheit haben Sie die Abfrageleistung von SQL Data Warehouse über [Ressourcenklassen](resource-classes-for-workload-management.md) verwaltet.  Mithilfe von Ressourcenklassen konnte einer Abfrage Arbeitsspeicher basierend auf der Rollenmitgliedschaft zugewiesen werden.  Die größte Herausforderung bei Ressourcenklassen war, dass nach der Konfiguration keine Governance oder Fähigkeit zum Steuern der Workload bestand.  

Wenn Sie z.B. eine Ad-hoc-Benutzerrollenmitgliedschaft für „smallrc“ gewährten, konnte dieser Benutzer 100 % des Arbeitsspeichers im System belegen.  Bei Ressourcenklassen gibt es keine Möglichkeit, Ressourcen zu reservieren und sicherzustellen, dass Ressourcen für kritische Workloads verfügbar sind.

Die Workloadverwaltung in SQL Data Warehouse umfasst drei grundlegende Konzepte: [Workloadklassifizierung](sql-data-warehouse-workload-classification.md), [Workloadpriorität](sql-data-warehouse-workload-importance.md) und [Workloadisolation](sql-data-warehouse-workload-isolation.md).  Mit diesen Funktionen können Sie besser steuern, wie Systemressourcen von der Workload genutzt werden.

Die Workloadklassifizierung ist das Konzept, bei dem eine Anforderung einer Arbeitsauslastungsgruppe zugewiesen wird und Prioritätsstufen festgelegt werden.  Bisher erfolgte diese Zuweisung über die Rollenmitgliedschaft mithilfe von [sp_addrolemember](https://docs.microsoft.com/azure/sql-data-warehouse/resource-classes-for-workload-management#change-a-users-resource-class).  Dies ist nun über [CREATE WORKLOAD CLASSIFER](https://docs.microsoft.com/sql/t-sql/statements/create-workload-classifier-transact-sql) möglich.  Die Klassifizierungsfunktion bietet einen umfassenderen Satz von Optionen, wie z.B. Bezeichnung, Sitzung und Zeit zum Klassifizieren von Anforderungen.

Die Workloadpriorität wirkt sich auf die Reihenfolge aus, in der eine Anforderung Zugriff auf Ressourcen erhält.  Auf einem ausgelasteten System hat eine Anforderung mit höherer Priorität zuerst Zugriff auf Ressourcen.  Durch die Priorität kann auch der geordnete Zugriff auf Sperren sichergestellt werden. 

Mit der Workloadisolation werden Ressourcen für eine Arbeitsauslastungsgruppe reserviert.  Ressourcen, die in einer Arbeitsauslastungsgruppe reserviert sind, werden ausschließlich für diese Arbeitsauslastungsgruppe bereitgehalten, um die Ausführung zu gewährleisten.  Mithilfe von Arbeitsauslastungsgruppen können Sie auch die Menge der Ressourcen definieren, die pro Anforderung zugewiesen werden, ähnlich wie bei Ressourcenklassen.  Arbeitsauslastungsgruppen bieten Ihnen die Möglichkeit, die Menge an Ressourcen zu reservieren oder zu begrenzen, die von einer Gruppe von Anforderungen belegt werden können.  Außerdem stellen Arbeitsauslastungsgruppen einen Mechanismus zum Anwenden von Regeln (z.B. Abfragetimeout) auf Anforderungen dar.  


## <a name="next-steps"></a>Nächste Schritte

- Weitere Informationen zur Workloadklassifizierung finden Sie unter [Workloadklassifizierung](sql-data-warehouse-workload-classification.md).  
- Weitere Informationen zur Workloadisolation finden Sie unter [Workloadisolation](sql-data-warehouse-workload-isolation.md).  
- Weitere Informationen zur Workloadpriorität finden Sie unter [Workloadpriorität](sql-data-warehouse-workload-importance.md).  