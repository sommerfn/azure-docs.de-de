---
title: Mapping Data Flows in Azure Data Factory | Microsoft-Dokumentation
description: Übersicht über Mapping Data Flows in Azure Data Factory
author: kromerm
ms.author: makromer
ms.reviewer: daperlov
ms.service: data-factory
ms.topic: conceptual
ms.date: 10/7/2019
ms.openlocfilehash: 8d0ad794caee8a06c8d403a981037d6560fb3f43
ms.sourcegitcommit: 11265f4ff9f8e727a0cbf2af20a8057f5923ccda
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 10/08/2019
ms.locfileid: "72030149"
---
# <a name="what-are-mapping-data-flows"></a>Was sind Mapping Data Flows?

Mapping Data Flows sind visuell entworfene Datentransformationen in Azure Data Factory. Mit Data Flows können Data Engineers grafische Datentransformationslogik entwickeln, ohne Code schreiben zu müssen. Die daraus resultierenden Datenflüsse werden als Aktivitäten in Azure Data Factory-Pipelines mit horizontal skalierten Spark-Clustern ausgeführt. Datenflussaktivitäten können über vorhandene Planungs-, Steuerungs-, Fluss- und Überwachungsfunktionen in Data Factory operationalisiert werden.

Mapping Data Flows bieten eine vollständig visuelle Darstellung, ohne dass Schreiben von Code erforderlich ist. Ihre Datenflüsse werden in Ihrem eigenen Ausführungscluster für die horizontal skalierte Datenverarbeitung ausgeführt. Azure Data Factory übernimmt die gesamte Codeübersetzung, Pfadoptimierung und Ausführung Ihrer Datenflussaufträge.

## <a name="getting-started"></a>Erste Schritte

Um einen Datenfluss zu erstellen, klicken Sie auf das Pluszeichen unter „Factory-Ressourcen“. 

![neuer Datenfluss](media/data-flow/newdataflow2.png "neuer Datenfluss")

Dadurch gelangen Sie zur Datenflusscanvas, auf der Sie Ihre Transformationslogik erstellen können. Klicken Sie auf das Feld „Quelle hinzufügen“, um mit dem Konfigurieren der Quelltransformation zu beginnen. Weitere Informationen finden Sie unter [Quelltransformation](data-flow-source.md).

## <a name="data-flow-canvas"></a>Datenflusscanvas

Die Datenflusscanvas ist in drei Bereiche unterteilt: die obere Leiste, das Diagramm und den Konfigurationsbereich. 

![Canvas](media/data-flow/canvas1.png "Canvas")

### <a name="graph"></a>Diagramm

Das Diagramm zeigt den Transformationsdatenstrom. Es zeigt die Herkunft der Quelldaten beim Fließen in eine oder mehrere Senken. Zum Hinzufügen einer neuen Quelle klicken Sie auf das Feld „Quelle hinzufügen“. Wenn Sie eine neue Transformation hinzufügen möchten, klicken Sie auf das Pluszeichen unten rechts in einer vorhandenen Transformation.

![Canvas](media/data-flow/canvas2.png "Canvas")

### <a name="configuration-panel"></a>Konfigurationsbereich

Im Konfigurationsbereich werden die für die aktuell ausgewählte Transformation spezifischen Einstellungen oder – falls keine Transformation ausgewählt ist – der Datenfluss angezeigt. In der allgemeinen Datenflusskonfiguration können Sie den Namen und die Beschreibung auf der Registerkarte **Allgemein** bearbeiten oder Parameter über die Registerkarte **Parameter** hinzufügen. Weitere Informationen finden Sie unter [Mapping Data Flow-Parameter](parameters-data-flow.md).

Jede Transformation verfügt über mindestens vier Registerkarten für die Konfiguration:

#### <a name="transformation-settings"></a>Transformationseinstellungen

Die erste Registerkarte im Konfigurationsbereich jeder Transformation enthält die Einstellungen, die für diese Transformation spezifisch sind. Weitere Informationen finden Sie auf der Dokumentationsseite für diese Transformation.

![Registerkarte „Quelleinstellungen“](media/data-flow/source1.png "Registerkarte „Quelleinstellungen“")

#### <a name="optimize"></a>Optimieren

Die Registerkarte _Optimieren_ enthält Einstellungen zum Konfigurieren von Partitionierungsschemas.

![Optimieren](media/data-flow/optimize1.png "Optimieren")

Die Standardeinstellung ist „Aktuelle Partitionierung verwenden“, durch die Azure Data Factory angewiesen wird, das native Partitionierungsschema der in Spark ausgeführten Datenflüsse zu verwenden. Diese Einstellung wird für die meisten Szenarien empfohlen.

Es gibt Fälle, in denen Sie möglicherweise die Partitionierung anpassen möchten. Wenn beispielsweise Ihre Transformationen in einer einzigen Datei im Lake ausgegeben werden sollen, wählen Sie „Einzelne Partition“ in einer Senkentransformation aus.

Ein weiterer Fall, in dem Sie die Partitionierungsschemas möglicherweise steuern möchten, ist die Optimierung der Leistung. Das Anpassen der Partitionierung bietet Kontrolle über die Verteilung Ihrer Daten auf Computeknoten und Datenstandortoptimierungen, die sowohl positive als auch negative Auswirkungen auf die gesamte Datenflussleistung haben können. Weitere Informationen finden Sie in der [Anleitung zur Leistung der Mapping Data Flow-Funktion](concepts-data-flow-performance.md).

Wenn Sie die Partitionierung für eine Transformation ändern möchten, klicken Sie auf die Registerkarte „Optimieren“, und aktivieren Sie das Optionsfeld „Partitionierung festlegen“. Daraufhin werden mehrere Optionen für die Partitionierung angezeigt. Die beste Methode der Partitionierung richtet sich jeweils nach den Datenmengen, Kandidatenschlüsseln, Nullwerten und der Kardinalität. Eine bewährte Methode besteht darin, mit der Standardpartitionierung zu beginnen und dann verschiedene Partitionierungsoptionen auszuprobieren. Sie können Tests anhand von Debugausführungen der Pipeline durchführen und die Ausführungszeit sowie die Verwendung der Partition in jeder Transformationsgruppierung in der Ansicht „Überwachung“ anzeigen. Weitere Informationen finden Sie unter [Überwachen von Datenflüssen](concepts-data-flow-monitoring.md).

Nachfolgend sind die verfügbaren Partitionierungsoptionen aufgeführt.

##### <a name="round-robin"></a>Round-Robin 

Roundrobin ist eine einfache Partition, die automatisch Daten gleichmäßig auf Partitionen verteilt. Verwenden Sie die Option „Roundrobin“, wenn Sie nicht über gute Kandidaten verfügen, um eine solide, intelligente Partitionierungsstrategie zu implementieren. Sie können die Anzahl der physischen Partitionen festlegen.

##### <a name="hash"></a>Hash

Azure Data Factory erzeugt einen Hash von Spalten, um einheitliche Partitionen zu erstellen, sodass Zeilen mit ähnlichen Werten in die gleiche Partition fallen. Testen Sie bei Verwendung der Option „Hash“ auf mögliche Partitionsungleichmäßigkeiten. Sie können die Anzahl der physischen Partitionen festlegen.

##### <a name="dynamic-range"></a>Dynamischer Bereich

Bei dieser Option werden dynamische Spark-Bereiche basierend auf den von Ihnen angegebenen Spalten oder Ausdrücken verwendet. Sie können die Anzahl der physischen Partitionen festlegen. 

##### <a name="fixed-range"></a>Fester Bereich

Erstellen Sie einen Ausdruck, der einen festen Bereich für Werte in Ihren partitionierten Datenspalten bereitstellt. Sie müssen über fundierte Kenntnisse Ihrer Daten verfügen, bevor Sie diese Option verwenden, um Partitionsungleichmäßigkeiten zu vermeiden. Die von Ihnen für den Ausdruck eingegebenen Werte werden als Teil einer Partitionsfunktion verwendet. Sie können die Anzahl der physischen Partitionen festlegen.

##### <a name="key"></a>Schlüssel

Wenn Sie eine genaue Kenntnis der Kardinalität Ihrer Daten haben, kann die Schlüsselpartitionierung eine gute Partitionsstrategie darstellen. Die Schlüsselpartitionierung erstellt Partitionen für jeden eindeutigen Wert in der Spalte. Sie können die Anzahl der Partitionen nicht festlegen, weil die Anzahl auf den eindeutigen Werten in den Daten basiert.

#### <a name="inspect"></a>Überprüfen

Die Registerkarte _Überprüfen_ bietet einen Einblick in die Metadaten des Datenstroms, den Sie transformieren. Sie können die Spaltenanzahl, geänderte Spalten, hinzugefügte Spalten, Datentypen, die Spaltensortierung und Spaltenverweise sehen. „Überprüfen“ ist eine schreibgeschützte Ansicht Ihrer Metadaten. Der Debugmodus muss nicht aktiviert sein, um die Metadaten im Bereich „Überprüfen“ anzeigen zu können.

![Überprüfen](media/data-flow/inspect1.png "Überprüfen")

Wenn Sie die Form Ihrer Daten durch Transformationen ändern, wird der Fluss der Metadatenänderungen im Bereich „Überprüfen“ angezeigt. Wenn in Ihrer Quelltransformation kein definiertes Schema vorhanden ist, werden im Bereich „Überprüfen“ keine Metadaten angezeigt. Fehlende Metadaten sind in Schemaabweichungsszenarien üblich.

#### <a name="data-preview"></a>Datenvorschau

Bei aktiviertem Debugmodus können Sie auf der Registerkarte _Datenvorschau_ eine interaktive Momentaufnahme der Daten bei jeder Transformation anzeigen. Weitere Informationen finden Sie unter [Datenvorschau im Debugmodus](concepts-data-flow-debug-mode.md#data-preview).

### <a name="top-bar"></a>Obere Leiste

Die obere Leiste enthält Aktionen, die sich auf den gesamten Datenfluss auswirken, z.B. das Speichern und Überprüfen. Sie können auch mithilfe der Schaltflächen **Diagramm anzeigen** und **Diagramm ausblenden** zwischen dem Diagramm- und Konfigurationsmodus wechseln.

![Diagramm ausblenden](media/data-flow/hideg.png "Diagramm ausblenden")

Wenn Sie das Diagramm ausblenden, können Sie über die Schaltflächen **Zurück** und **Weiter** seitwärts durch die Transformationsknoten navigieren.

![Navigieren](media/data-flow/showhide.png "Navigieren")

## <a name="next-steps"></a>Nächste Schritte

* Erfahren Sie, wie Sie eine [Quelltransformation](data-flow-source.md) erstellen.
* Erfahren Sie, wie Sie Ihre Datenflüsse im [Debugmodus](concepts-data-flow-debug-mode.md) erstellen.
