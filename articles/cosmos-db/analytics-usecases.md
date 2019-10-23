---
title: Anwendungsfälle für integrierte Analysen mit Azure Cosmos DB.
description: Erfahren Sie, wie Sie integrierte Analysen mit Azure Cosmos DB in verschiedenen Anwendungsfällen verwenden.
author: markjbrown
ms.author: mjbrown
ms.topic: conceptual
ms.service: cosmos-db
ms.date: 09/26/2019
ms.reviewer: sngun
ms.openlocfilehash: b9bac14ff47107a4b39f12989685e2e510569ba2
ms.sourcegitcommit: 8074f482fcd1f61442b3b8101f153adb52cf35c9
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 10/22/2019
ms.locfileid: "72757077"
---
# <a name="use-cases-for-built-in-analytics-with-azure-cosmos-db"></a>Anwendungsfälle für integrierte Analysen mit Azure Cosmos DB.

Die folgenden Anwendungsfälle können mithilfe der in Apache Spark integrierten Analysen in Azure Cosmos DB umgesetzt werden:

## <a name="htap-scenarios"></a>HTAP-Szenarien

Integrierte Analysen in Azure Cosmos DB können wie folgt verwendet werden:

* Erkennen von Betrugsversuchen während der Transaktionsverarbeitung
* Bereitstellen von Empfehlungen für Käufer beim Browsen in einem E-Commerce-Shop
* Benachrichtigen von Operatoren über den drohenden Ausfall einer wichtigen Fertigungsanlage
* Gewinnen schneller, ergebnisorientierter Erkenntnisse durch das direkte Einbetten von Echtzeitanalysen in operative Daten.

Azure Cosmos DB unterstützt Hybrid Transactional/Analytical Processing (HTAP)-Szenarien mithilfe des nativen, integrierten Apache Spark-Moduls. Azure Cosmos DB überwindet die operative und analytische Trennung herkömmlicher Systeme.

## <a name="globally-distributed-data-lake-without-requiring-any-etl"></a>Global verteilter Data Lake ohne ETL

Mit dem nativen, integrierten Apache Spark-Modul bietet Azure Cosmos DB eine schnelle, einfache und skalierbare Möglichkeit zur Erstellung eines global verteilten Data Lakes, der ein einzelnes Systemabbild bereitstellt. Dank der global verteilten Multimodelldaten entfallen Investitionen in teure ETL-Pipelines. Die bedarfsgerechte Skalierung revolutioniert die Art und Weise, wie Datenteams ihre Datasets analysieren.

## <a name="time-series-analytics-over-historic-data"></a>Zeitreihenanalyse für historische Daten

In einigen Fällen müssen Sie ggf. Fragen auf der Grundlage von Daten beantworten, die zu einem gegebenen Zeitpunkt über Ereignisse in der Vergangenheit vorlagen. Beispielsweise, um die CRM-Aktivitätsstatus-Anzahl für ein bestimmtes Datum in der Vergangenheit zu ermitteln. Wenn Sie den Bericht vor einer Woche ausgeführt haben, würde die Statusanzahl der Statusanzahl einzelner Aktivitäten zum damaligen Zeitpunkt entsprechen. Würden Sie den gleichen Bericht heute ausführen, erhielten Sie die Anzahl der Aktivitäten mit der aktuellen Statusanzahl. Diese könnte sich seit der letzten Woche geändert haben, da der Lebenszyklus von Anfang bis Ende durchlaufen wird. Folglich müssen Sie Momentaufnahmen in jeder Phase des Lebenszyklus eines Anwendungsfalls dokumentieren.

In herkömmlichen Data Warehouse-Szenarien werden Momentaufnahmen nicht unterstützt, da Data Warehouses nicht darauf ausgelegt sind und die Daten nur eine aktuelle Ansicht der Ereignisse widerspiegeln. Mit Azure Cosmos DB haben Benutzer die Möglichkeit, das Zeitreise-Konzept zu implementieren. Auf diese Weise lassen sich Daten retrospektiv abfragen und analysieren, um zu ermitteln, wie sich die Daten zu einem bestimmten Zeitpunkt in der Vergangenheit darstellten. Dies bedeutet, dass Benutzer problemlos sowohl die aktuelle als auch die historische Ansicht der Daten anzeigen und die Daten analysieren können.

## <a name="globally-distributed-machine-learning-and-ai"></a>Global verteiltes Machine Learning und KI

Wenn Unternehmen mit schnell wachsenden Datenmengen und einer immer größeren Vielfalt von Datentypen und Formaten konfrontiert sind, ist es nahezu unmöglich, tiefere und genauere Erkenntnisse aus Daten in Petabyte-Größe zu ziehen. Mit dem nativen, integrierten Apache Spark-Modul bietet Azure Cosmos DB eine global verteilte Analyseplattform, die eine umfassende Bibliothek mit Machine Learning-Algorithmen enthält. Sie können interaktive Jupyter Notebooks zum Erstellen und Trainieren von Modellen sowie Clusterverwaltungsfunktionen verwenden. Mithilfe dieser Funktionen können Sie feinabgestimmte und auto-elastische Spark-Cluster bedarfsgesteuert bereitstellen.

## <a name="deep-learning-on-multi-model-globally-distributed-data"></a>Deep Learning für global verteilte Multimodelldaten

Deep Learning ist eine ideale Möglichkeit, um Predictive Analytics Lösungen für Big Data bereitzustellen, während Daten immer umfangreicher und komplexer werden. Dank Deep Learning können Unternehmen das Potenzial unstrukturierter und teilweise strukturierter Daten nutzen, um Anwendungsfälle bereitzustellen, die Technologien wie KI, Verarbeitung natürlicher Sprache und viele mehr nutzen.

## <a name="reporting-integrating-with-power-apps-power-bi"></a>Berichterstellung (Integration in Power Apps, Power BI)

Power BI bietet interaktive Visualisierungen mit Self-Service-Business Intelligence-Funktionen und ermöglicht Endbenutzern die Erstellung eigener Berichte und Dashboards. Mit dem integrierten Spark-Connector können Sie Power BI Desktop mit Apache Spark-Clustern in Azure Cosmos DB verbinden. Dieser Connector unterstützt direkte Abfragen, um die Verarbeitung an Apache Spark in Azure Cosmos DB auszulagern. Dies ist eine hervorragende Lösung, wenn Sie über eine sehr große Datenmenge verfügen, die nicht in Power BI geladen werden soll, oder wenn Sie eine Analyse nahezu in Echtzeit durchführen möchten.

## <a name="iot-analytics-at-global-scale"></a>IoT-Analysen auf globaler Ebene

Netzwerksensoren generieren immer mehr Daten, die für unerreichte Transparenz in bisher undurchsichtigen Systemen und Prozessen sorgen. Der entscheidende Punkt besteht darin, umsetzbare Erkenntnisse aus dieser Masse an Informationen zu ziehen, unabhängig davon, wo auf der Welt sich IoT-Geräte befinden. Mit Azure Cosmos DB können IoT-Unternehmen Daten von Hochleistungssensoren und Zeitreihendaten in Echtzeit analysieren – und das überall auf der Welt. Damit können Sie die echten Vorteile einer vernetzten Welt nutzen, um die Kundenerfahrung und betriebliche Effizienz zu verbessern und neue Verkaufschancen zu erschließen.

## <a name="stream-processing-and-event-analytics"></a>Streamverarbeitung und Ereignisanalyse 

Von Protokolldateien bis hin zu Sensordaten müssen Unternehmen immer mehr Datenströme bewältigen. Diese Daten werden in einem permanenten Datenstrom empfangen, der häufig aus mehreren Quellen gleichzeitig gespeist wird. Obwohl es möglich ist, diese Datenströme auf einem Datenträger zu speichern und Daten retrospektiv zu analysieren, kann es manchmal sinnvoll oder wichtig sein, Daten direkt bei Eingang zu verarbeiten und entsprechend zu handeln. Datenströme im Zusammenhang mit Finanztransaktionen können z. B. in Echtzeit verarbeitet werden, um potenziell betrügerische Transaktionen zu identifizieren und einzudämmen.

## <a name="interactive-analytics"></a>Interaktive Analysen

Neben der Ausführung vordefinierter Abfragen, um beispielsweise statische Dashboards für Umsätze, Produktivität und Aktienkurse zu erstellen, können Sie Daten auch interaktiv untersuchen. Mithilfe interaktiver Analysen können Sie Fragen stellen, Ergebnisse anzeigen, ursprüngliche Fragestellungen basierend auf Antworten anpassen oder Ergebnisse genauer untersuchen. Das Apache Spark-Modul in Azure Cosmos DB unterstützt interaktive Abfragen, da es reaktionsschnell und anpassungsfähig ist.

## <a name="data-exploration-using-jupyter-notebooks"></a>Datenuntersuchung mithilfe von Jupyter Notebooks

Wenn Sie über ein neues Dataset verfügen, müssen Sie die Daten untersuchen, bevor Sie sich mit dem Ausführen von Modellen und Tests befassen. Anders ausgedrückt, müssen Sie eine explorative Datenanalyse durchführen. Durch Datenexploration lassen sich informiertere Entscheidungen treffen. Auf diese Weise können Sie detaillierte Erkenntnisse gewinnen: z. B. über Methoden, die für Ihre Daten geeignet sind, oder ob Daten bestimmte Modellierungsannahmen erfüllen, bereinigt, umstrukturiert usw. werden müssen. Mithilfe der nativen, integrierten Jupyter Notebooks und des Apache Stark-Moduls können Sie schnelle und effiziente Datenanalysen für Transaktions- und Analysedaten durchführen.

## <a name="next-steps"></a>Nächste Schritte

Informationen zu den ersten Schritten mit diesen Anwendungsfällen finden Sie in den folgenden Artikeln:

* [Integrierte Jupyter Notebooks in Azure Cosmos DB](cosmosdb-jupyter-notebooks.md)
* [Aktivieren von Notebooks für Azure Cosmos-Konten](enable-notebooks.md)
* [Erstellen eines Notebooks zum Analysieren und Visualisieren von Daten](create-notebook-visualize-data.md)