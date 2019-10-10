---
title: 'Aggregattransformation in Mapping Data Flow: Azure Data Factory | Microsoft-Dokumentation'
description: Informationen zum bedarfsorientiertem Aggregieren von Daten in Azure Data Factory mithilfe der Mapping Data Flow-Aggregattransformation
author: kromerm
ms.author: makromer
ms.service: data-factory
ms.topic: conceptual
ms.date: 02/01/2019
ms.openlocfilehash: 0201cbdd05cd8aae4afb92b459bf58fb5ff6a142
ms.sourcegitcommit: 11265f4ff9f8e727a0cbf2af20a8057f5923ccda
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 10/08/2019
ms.locfileid: "72026977"
---
# <a name="aggregate-transformation-in-mapping-data-flow"></a>Aggregattransformation in Mapping Data Flow 



In der Transformation für das Aggregieren definieren Sie Aggregationen von Spalten in Ihren Datenströmen. Mithilfe des Ausdrucks-Generators können Sie verschiedene Arten von Aggregationen wie SUM, MIN, MAX und COUNT definieren, die anhand von bereits vorhandenen oder berechneten Spalten gruppiert werden können.

## <a name="group-by"></a>Gruppieren nach
Wählen Sie eine bereits vorhandene Spalte aus, oder erstellen Sie eine neue berechnete Spalte, um diese als GROUP BY-Klausel für Ihre Aggregation zu verwenden. Wählen Sie dafür einfach die gewünschte Spalte aus der entsprechenden Dropdownliste aus. Zeigen Sie mit der Maus auf die Klausel, und klicken Sie auf „Berechnete Spalte“, um eine neue berechnete Spalte zu erstellen. Dadurch öffnet sich [Data Flow Expression Builder](concepts-data-flow-expression-builder.md). Sobald Sie Ihre berechnete Spalte erstellt haben, geben Sie den Namen der Ausgabespalte in das Feld „Name as“ (Bennen als) ein. Wenn Sie eine zusätzliche GROUP BY-Klausel hinzufügen möchten, zeigen Sie mit der Maus auf eine bereits vorhandene Klausel, und klicken Sie auf das Symbol „+“.

![GROUP BY-Einstellungen für die Aggregattransformation](media/data-flow/agg.png "GROUP BY-Einstellungen für die Aggregattransformation")

> [!NOTE]
> Die Verwendung einer GROUP BY-Klausel in einer Aggregattransformation ist optional.

## <a name="aggregate-column"></a>Aggregatspalte 
Klicken Sie auf die Registerkarte „Aggregate“, um Aggregationsausdrücke zu erstellen. Sie können entweder eine bereits vorhandene Spalte auswählen, um den Wert mit der Aggregation zu überschreiben, oder ein neues Feld mit dem neuen Namen erstellen. Der Aggregationsausdruck wird in das Feld auf der rechten Seite neben dem Selektor für den Spaltennamen eingegeben. Sie können den Ausdruck bearbeiten, indem Sie auf das Textfeld klicken, um den Ausdrucks-Generator zu öffnen. Wenn Sie eine zusätzliche Aggregation hinzufügen möchten, zeigen Sie mit der Maus auf einen bereits vorhandenen Ausdruck, und klicken Sie auf das Symbol „+“, um eine neue Aggregationsspalte oder ein [Spaltenmuster](concepts-data-flow-column-pattern.md) zu erstellen.

![Aggregateinstellungen für die Aggregattransformation](media/data-flow/agg2.png "Aggregateinstellungen für die Aggregattransformation")

> [!NOTE]
> Jeder Aggregationsausdruck muss mindestens eine Aggregatfunktion enthalten.

> [!NOTE]
> Im Debugmodus kann der Ausdrucks-Generator keine Datenvorschau mit Aggregatfunktionen generieren. Wenn Sie die Datenvorschau für eine Aggregattransformation abrufen möchten, schließen Sie den Ausdrucks-Generator, und rufen Sie die Daten über die Registerkarte „Datenvorschau“ ab.

## <a name="reconnect-rows-and-columns"></a>Neuverbinden von Zeilen und Spalten
Aggregattransformationen entsprechen im Prinzip den SQL-Abfragen für die Aggregatauswahl. Spalten, die nicht in der GROUP BY-Klausel oder in den Aggregatfunktionen enthalten sind, fließen nicht in die Ausgabe der Aggregattransformation. Wenn andere Spalten vorhanden sind, die Sie in die Ausgabe der aggregierten Zeilen einschließen möchten, müssen Sie eine der folgenden Aktionen ausführen:

* Verwenden Sie eine Aggregatfunktion, um die zusätzliche Spalte einzuschließen, z. B. Last() oder First().
* Verknüpfen Sie die Spalten vor dem Aggregieren mit dem [Self-Join-Muster](https://mssqldude.wordpress.com/2018/12/20/adf-data-flows-self-join/) noch mal.

## <a name="next-steps"></a>Nächste Schritte

* Definieren Sie eine fensterbasierte Aggregation mithilfe der [Fenstertransformation](data-flow-window.md).
