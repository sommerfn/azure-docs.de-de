---
title: 'Azure Data Factory-Mappingdatenfluss: Suchtransformation'
description: 'Azure Data Factory-Mappingdatenfluss: Suchtransformation'
author: kromerm
ms.author: makromer
ms.service: data-factory
ms.topic: conceptual
ms.date: 10/03/2019
ms.openlocfilehash: 01a1dba18e21a38695146560bbf85cf1a042ba02
ms.sourcegitcommit: b4f201a633775fee96c7e13e176946f6e0e5dd85
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 10/18/2019
ms.locfileid: "72596575"
---
# <a name="azure-data-factory-mapping-data-flow-lookup-transformation"></a>Azure Data Factory-Mappingdatenfluss: Suchtransformation

Verwenden Sie die Transformation für die Suche, um Ihrem Datenfluss Verweisdaten aus einer anderen Quelle hinzuzufügen. Für die Suchtransformation ist eine definierte Quelle erforderlich, die auf Ihre Verweistabelle verweist und mit Schlüsselfeldern übereinstimmt.

![Suchtransformation](media/data-flow/lookup1.png "Nachschlagen")

Wählen Sie die Schlüsselfelder aus, die zwischen den Feldern für den eingehenden Datenstrom und den Feldern der Referenzquelle abgeglichen werden sollen. Zuerst muss auf dem Datenfluss-Entwurfscanvas eine neue Quelle erstellt worden sein, die als rechte Seite für die Suche verwendet wird.

Werden Übereinstimmungen gefunden, werden Ihrem Datenfluss die resultierenden Zeilen und Spalten der Referenzquelle hinzugefügt. Sie können die relevanten Felder auswählen, die Sie in Ihre Senke am Ende des Datenflusses einschließen möchten. Verwenden Sie alternativ nach Ihrer Suche eine Auswahltransformation, um die Feldliste zu löschen und nur die gewünschten Felder aus beiden Streams beizubehalten.

Die Suchtransformation führt das Äquivalent eines linken äußeren Joins aus. Es werden also alle Zeilen aus Ihrer linken Quelle mit Übereinstimmungen von Ihrer rechten Seite kombiniert. Wenn bei der Suche mehrere übereinstimmende Werte vorhanden sind oder Sie den Suchausdruck anpassen möchten, empfiehlt es sich, zu einer Join-Transformation zu wechseln und ein Kreuzprodukt zu verwenden. Dadurch werden bei der Ausführung mögliche kartesische Produktfehler vermieden.

## <a name="match--no-match"></a>Übereinstimmung/Keine Übereinstimmung

Nach Ihrer Lookup-Transformation können Sie nachfolgende Transformationen verwenden, um die Ergebnisse der einzelnen übereinstimmenden Zeilen zu überprüfen, indem Sie die Ausdrucksfunktion `isMatch()` verwenden, um weitere Entscheidungen in Ihrer Logik zu treffen, je nachdem, ob der Lookup-Vorgang zu einer Übereinstimmung in einer Zeile führte.

![Suche: Muster](media/data-flow/lookup111.png "Suche: Muster")

Nach Verwendung der Suchtransformation können Sie eine Transformationsaufteilung vom Typ „Bedingtes Teilen“ für die Funktion ```isMatch()``` hinzufügen. Im obigen Beispiel durchlaufen übereinstimmende Zeilen den obersten Stream und nicht übereinstimmende Zeilen den Stream ```NoMatch```.

## <a name="first-or-last-value"></a>Erster oder letzter Wert

Sollte Ihre Suche mehrere Übereinstimmungen ergeben haben, können Sie die mehrfachen übereinstimmenden Zeilen verringern, indem Sie entweder den ersten oder den letzten Match auswählen. Hierzu können Sie im Anschluss an Ihre Suche eine Aggregattransformation verwenden.

In diesem Fall wird eine Aggregattransformation namens ```PickFirst``` verwendet, um den ersten Wert aus den Suchübereinstimmungen auszuwählen.

![Suche: Aggregat](media/data-flow/lookup333.png "Suche: Aggregat")

![Suche: Erste Übereinstimmung](media/data-flow/lookup444.png "Suche: Erste Übereinstimmung")

## <a name="optimizations"></a>Optimierungen

In Data Factory werden Datenflüsse in horizontal skalierten Spark-Umgebungen ausgeführt. Wenn Ihr Dataset in den Workerknotenspeicher eingepasst werden kann, können wir Ihre Lookup-Leistung optimieren.

![Broadcastjoin](media/data-flow/broadcast.png "Broadcastjoin")

### <a name="broadcast-join"></a>Broadcastjoin

Wählen Sie für den Broadcastjoin „Links“ und/oder „Rechts“ aus, um anzufordern, dass die Anwendungsdefinitionsdatei (Application Definition File, ADF) das gesamte Dataset mithilfe von Push von jeder der beiden Seiten der Lookup-Beziehung in den Speicher überträgt. Bei kleineren Datasets kann dies die Suchleistung erheblich verbessern.

### <a name="data-partitioning"></a>Datenpartitionierung

Sie können auch die Partitionierung Ihrer Daten angeben, indem Sie auf der Registerkarte „Optimize“ (Optimieren) der Lookup-Transformation „Set Partitioning“ (Partitionierung festlegen) auswählen, um Datasets zu erstellen, die besser in den Speicher pro Worker passen.

## <a name="next-steps"></a>Nächste Schritte

* [Join](data-flow-join.md)- und [Exists](data-flow-exists.md)-Transformationen erfüllen in ADF-Mappingdatenflüssen ähnliche Aufgaben. Sehen Sie sich als Nächstes diese Transformationen an.
* Verwenden Sie [Bedingtes Teilen](data-flow-conditional-split.md) mit ```isMatch()```, um Zeilen nach übereinstimmenden und nicht übereinstimmenden Werten aufzuteilen.
