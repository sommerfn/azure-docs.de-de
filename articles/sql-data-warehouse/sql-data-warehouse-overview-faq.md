---
title: 'Azure Synapse Analytics (früher SQL DW): Häufig gestellte Fragen'
description: Dieser Artikel enthält häufig gestellte Fragen von Kunden und Entwicklern zu Azure Synapse Analytics (früher SQL DW).
services: sql-data-warehouse
author: mlee3gsd
manager: craigg
ms.service: sql-data-warehouse
ms.topic: conceptual
ms.subservice: design
ms.date: 11/04/2019
ms.author: martinle
ms.reviewer: igorstan
ms.openlocfilehash: f7b4f926bb9611d87c67276f754a6b596850b59d
ms.sourcegitcommit: 359930a9387dd3d15d39abd97ad2b8cb69b8c18b
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 11/06/2019
ms.locfileid: "73645593"
---
# <a name="azure-synapse-analytics-formerly-sql-dw-frequently-asked-questions"></a>Azure Synapse Analytics (früher SQL DW) – Häufig gestellte Fragen

## <a name="general"></a>Allgemein

F: Was ist Azure Synapse?

A. Azure Synapse ist ein grenzenloser Analysedienst, der Data Warehousing mit Big Data-Analysen kombiniert. Er ermöglicht flexible Datenabfragen nach Ihren Vorstellungen unter Verwendung von serverlosen On-Demand-Ressourcen oder bereitgestellten Ressourcen im gewünschten Umfang. Azure Synapse kombiniert diese beiden Welten in einer vereinheitlichten Oberfläche zur Erfassung, Vorbereitung, Verwaltung und Verarbeitung von Daten für sofortige BI- und Machine Learning-Anforderungen. Weitere Informationen finden Sie unter [Was ist Azure Synapse Analytics?](sql-data-warehouse-overview-what-is.md).

F: Was ist mit Azure SQL Data Warehouse geschehen?

A. Bei Azure Synapse handelt es sich um ein weiterentwickeltes Azure SQL Data Warehouse (SQL DW). Wir haben dasselbe branchenführende Data Warehouse auf ein ganz neues Leistungs-und Funktionsniveau gebracht. Sie können Ihre vorhandenen Data Warehouse Workloads in der Produktion heute mit Azure Synapse weiterhin ausführen und gleichzeitig von den neuen Funktionen (derzeit in der Vorschauversion) automatisch profitieren. Weitere Informationen finden Sie unter [Was ist Azure Synapse Analytics?](sql-data-warehouse-overview-what-is.md).

F: Was ist SQL Analytics?

A. SQL Analytics bezieht sich auf die Data Warehousing-Features für Unternehmen, die in Azure Synapse allgemein zur Verfügung stehen. Weitere Informationen finden Sie unter [Was ist Azure Synapse Analytics?](sql-data-warehouse-overview-what-is.md).

F: Wie beginne ich mit der Nutzung von Azure Synapse?

A. Sie können mit einem [kostenlosen Azure-Konto](https://azure.microsoft.com/free/sql-data-warehouse/) beginnen oder [den Vertrieb kontaktieren, um weitere Informationen zu erhalten](https://info.microsoft.com/ww-landing-azure-sql-data-warehouse-contactme.html). 

F: Was bietet Azure Synapse für die Datensicherheit?

A. Azure Synapse bietet verschiedene Lösungen für den Schutz von Daten, z.B. transparente Datenverschlüsselung (Transparent Data Encryption, TDE) und Überwachung. Weitere Informationen finden Sie unter [Sicherheit].

F: Wo finde ich Informationen dazu, welche rechtlichen oder geschäftsspezifischen Standards Azure Synapse erfüllt?

A. Auf der Seite [Microsoft-Compliance] finden Sie verschiedene nach Produkt sortierte Compliance-Angebote (beispielsweise für SOC und ISO). Wählen Sie zunächst einen Compliancetitel aus. Erweitern Sie anschließend rechts auf der Seite im Abschnitt mit den konformen Microsoft-Clouddiensten den Knoten „Azure“, um die richtlinienkonformen Azure Synapse-Dienste anzuzeigen.

F: Kann ich eine Verbindung mit Power BI herstellen?

A. Ja. Power BI unterstützt zwar direkte Abfragen mit Azure Synapse, ist für eine große Anzahl von Benutzern oder eine große Menge von Echtzeitdaten aber nicht vorgesehen. Um die Leistung von Power BI weiter zu steigern, wird die Nutzung von Power BI zusätzlich zu Azure Analysis Services oder Analysis Service IaaS empfohlen.

F: Welche Kapazitätsgrenzen gibt es für SQL Analytics?

A. Informationen finden Sie auf der Seite mit den [Kapazitätsgrenzen]. 

F: Warum erfordert Skalieren/Anhalten/Fortsetzen so viel Zeit?

A. Die Dauer der Computerverwaltungsvorgänge kann durch eine Vielzahl von Faktoren beeinflusst werden. Diese Vorgänge erfordern häufig viel Zeit, wenn Transaktionsrollback erfolgt. Wenn Skalierung oder Anhalten initiiert wird, werden alle eingehenden Sitzungen blockiert, und Abfragen werden ausgeglichen. Damit das System stabil bleibt, muss ein Rollback von Transaktionen erfolgen, bevor ein Vorgang beginnen kann. Je größer die Anzahl der Transaktionen und je größer ihre Protokollgröße, desto länger wird der Vorgang angehalten, um einen stabilen Zustand des Systems wiederherzustellen.

## <a name="user-support"></a>Unterstützung für Benutzer

F: Ich habe einen Funktionswunsch. Wie kann ich diesen übermitteln?

A. Wenn Sie einen Funktionswunsch haben, können Sie ihn über die Seite [UserVoice] einsenden.

F: Wo finde ich Informationen zu x?

A. Wenn Sie Hilfe bei der Entwicklung mit Azure Synapse benötigen, können Sie auf unserer Seite [Stack Overflow] Fragen stellen. 

F: Wie übermittle ich ein Supportticket?

A. [Supporttickets] können über das Azure-Portal erstellt werden.

## <a name="sql-languagefeature-support"></a>Unterstützung von SQL-Sprache/-Features 

F: Welche Datentypen werden unterstützt?

A. Lesen Sie dazu [Datentypen].

F: Welche Tabellenfunktionen werden unterstützt?

A. Viele Funktionen werden unterstützt. Die nicht unterstützten sind in [Nicht unterstützte Tabellenfunktionen] dokumentiert.

## <a name="tooling-and-administration"></a>Tools und Verwaltung

F: Werden Datenbankprojekte in Visual Studio unterstützt?

A. Wir unterstützen in Visual Studio derzeit keine Datenbankprojekte. Wenn Sie für die Einführung dieses Features stimmen möchten, besuchen Sie unsere User Voice-Seite für die [Anforderung von Features für Datenbankprojekte].

F: Unterstützt SQL Analytics Rest-APIs?

A. Ja. Die meisten REST-Funktionen, die bei SQL-Datenbank verwendet werden können, sind auch für SQL Analytics verfügbar. API-Informationen finden Sie auf den REST-Dokumentationsseiten oder auf [MSDN].


## <a name="loading"></a>Laden

F: Welche Clienttreiber werden unterstützt?

A. Treiberunterstützung für DW finden Sie auf der Seite [Verbindungszeichenfolgen].

F: Welche Dateiformate werden von PolyBase unterstützt?

A: Orc, RC, Parquet und flacher, durch Trennzeichen getrennter Text

F: Mit welchen Datenquellen kann ich mithilfe von PolyBase eine Verbindung herstellen? 

A: [Azure Data Lake Store] und [Azure-Speicherblobs]

F: Ist ein Berechnungs-Pushdown möglich, wenn ich eine Verbindung mit Azure Storage Blobs oder ADLS herstelle? 

A: Nein. PolyBase interagiert nur mit den Speicherkomponenten. 

F: Kann ich eine Verbindung mit HDI herstellen?

A: HDI kann entweder ADLS oder WASB als HDFS-Ebene verwenden. Wenn Sie eine der beiden Komponenten als HDFS-Ebene verwenden, können Sie diese Daten in SQL DW laden. Sie können jedoch keine Pushdown-Berechnung für die HDI-Instanz ausführen. 

## <a name="next-steps"></a>Nächste Schritte
Weitere Informationen zu Azure Synapse im Allgemeinen finden Sie auf unserer Seite mit der [Übersicht].


<!-- Article references -->
[UserVoice]: https://feedback.azure.com/forums/307516-sql-data-warehouse
[Verbindungszeichenfolgen]: ./sql-data-warehouse-connection-strings.md
[Stack Overflow]: https://stackoverflow.com/questions/tagged/azure-sqldw
[Supporttickets]: ./sql-data-warehouse-get-started-create-support-ticket.md
[Sicherheit]: ./sql-data-warehouse-overview-manage-security.md
[Microsoft-Compliance]: https://www.microsoft.com/en-us/trustcenter/compliance/complianceofferings
[Kapazitätsgrenzen]: ./sql-data-warehouse-service-capacity-limits.md
[Datentypen]: ./sql-data-warehouse-tables-data-types.md
[Nicht unterstützte Tabellenfunktionen]: ./sql-data-warehouse-tables-overview.md#unsupported-table-features
[Azure Data Lake Store]: ./sql-data-warehouse-load-from-azure-data-lake-store.md
[Azure-Speicherblobs]: ./sql-data-warehouse-load-from-azure-blob-storage-with-polybase.md
[Anforderung von Features für Datenbankprojekte]: https://feedback.azure.com/forums/307516-sql-data-warehouse/suggestions/13313247-database-project-from-visual-studio-to-support-azu
[MSDN]: https://msdn.microsoft.com/library/azure/mt163685.aspx
[Übersicht]: ./sql-data-warehouse-overview-faq.md
