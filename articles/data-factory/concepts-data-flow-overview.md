---
title: Zuordnungsdatenflüsse in Azure Data Factory
description: Übersicht über Mapping Data Flows in Azure Data Factory
author: kromerm
ms.author: makromer
ms.reviewer: daperlov
ms.service: data-factory
ms.topic: conceptual
ms.date: 10/7/2019
ms.openlocfilehash: ed2502ffebbacf5e66e3e4738e2e88ce7fb8a562
ms.sourcegitcommit: 609d4bdb0467fd0af40e14a86eb40b9d03669ea1
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 11/06/2019
ms.locfileid: "73681560"
---
# <a name="what-are-mapping-data-flows"></a>Was sind Zuordnungsdatenflüsse?

Mapping Data Flows (Zuordnungsdatenflüsse) sind visuell entworfene Datentransformationen in Azure Data Factory. Mit Data Flows können Data Engineers grafische Datentransformationslogik entwickeln, ohne Code schreiben zu müssen. Die daraus resultierenden Datenflüsse werden als Aktivitäten in Azure Data Factory-Pipelines ausgeführt, für die horizontal hochskalierte Spark-Cluster verwendet werden. Datenflussaktivitäten können über vorhandene Planungs-, Steuerungs-, Fluss- und Überwachungsfunktionen in Data Factory operationalisiert werden.

Zuordnungsdatenflüsse bieten eine vollständig visuelle Darstellung, ohne dass das Schreiben von Code erforderlich ist. Ihre Datenflüsse werden in Ihrem eigenen Ausführungscluster für die Datenverarbeitung mit horizontaler Hochskalierung ausgeführt. Azure Data Factory übernimmt die gesamte Codeübersetzung, Pfadoptimierung und Ausführung Ihrer Datenflussaufträge.

## <a name="getting-started"></a>Erste Schritte

Wählen Sie zum Erstellen eines Datenflusses unter **Factory Resources** (Factory-Ressourcen) das Pluszeichen und dann die Option **Datenfluss** aus. 

![Neuer Datenfluss](media/data-flow/newdataflow2.png "neuer Datenfluss")

Dadurch gelangen Sie zur Datenflusscanvas, auf der Sie Ihre Transformationslogik erstellen können. Wählen Sie **Quelle hinzufügen** aus, um mit der Konfiguration Ihrer Quelltransformation zu beginnen. Weitere Informationen finden Sie im Artikel zur [Quelltransformation](data-flow-source.md).

## <a name="data-flow-canvas"></a>Datenflusscanvas

Die Datenflusscanvas ist in drei Bereiche unterteilt: die obere Leiste, das Diagramm und den Konfigurationsbereich. 

![Canvas](media/data-flow/canvas1.png "Canvas")

### <a name="graph"></a>Graph

Das Diagramm zeigt den Transformationsdatenstrom. Es zeigt die Herkunft der Quelldaten beim Fließen in eine oder mehrere Senken. Wählen Sie die Option **Quelle hinzufügen** aus, um eine neue Quelle hinzuzufügen. Wählen Sie zum Hinzufügen einer neuen Transformation unten rechts in einer vorhandenen Transformation das Pluszeichen aus.

![Canvas](media/data-flow/canvas2.png "Canvas")

### <a name="azure-integration-runtime-data-flow-properties"></a>Azure Integration Runtime: Datenflusseigenschaften

![Schaltfläche „Debuggen“](media/data-flow/debugbutton.png "Schaltfläche „Debuggen“")

Wenn Sie in ADF mit der Verwendung von Datenflüssen beginnen, ist es ratsam, oben in der Benutzeroberfläche des Browsers die Option „Debuggen“ für Datenflüsse zu aktivieren. Hiermit wird ein Azure Databricks-Cluster erstellt, der für das interaktive Debuggen, die Datenvorschau und die Ausführung von Debugvorgängen für die Pipeline verwendet wird. Sie können die Größe des verwendeten Clusters festlegen, indem Sie eine benutzerdefinierte [Azure Integration Runtime](concepts-integration-runtime.md)-Instanz auswählen. Die Debugsitzung bleibt nach Ihrer letzten Datenvorschau bzw. letzten Ausführung eines Debugvorgangs für die Pipeline bis zu 60 Minuten lang aktiv.

Wenn Sie Ihre Pipelines mit Datenflussaktivitäten operationalisieren, wird von ADF die Azure Integration Runtime-Instanz verwendet, die in der „Run On“-Eigenschaft der [Aktivität](control-flow-execute-data-flow-activity.md) zugeordnet ist.

Die Azure Integration Runtime-Standardinstanz ist ein einzelner kleiner Workerknotencluster mit vier Kernen, der Ihnen das Anzeigen einer Datenvorschau und das schnelle Ausführen von Debugpipelines zu geringen Kosten ermöglichen soll. Legen Sie eine höhere Azure IR-Konfiguration fest, wenn Sie Vorgänge für große Datasets durchführen.

Sie können ADF anweisen, einen Pool mit Clusterressourcen (VMs) vorzuhalten, indem Sie in den Datenflusseigenschaften der Azure IR-Instanz eine Gültigkeitsdauer angeben. Dies führt für nachfolgende Aktivitäten zu einer schnelleren Auftragsausführung.

#### <a name="azure-integration-runtime-and-data-flow-strategies"></a>Azure Integration Runtime und Datenflussstrategien

##### <a name="execute-data-flows-in-parallel"></a>Paralleles Ausführen von Datenflüssen

Wenn Sie Datenflüsse in einer Pipeline parallel ausführen, erstellt ADF separate Azure Databricks-Cluster für jede Ausführung einer Aktivität. Dies erfolgt basierend auf den Einstellungen in Ihrer Azure Integration Runtime-Instanz, die jeder Aktivität zugeordnet ist. Fügen Sie zum Entwerfen von parallelen Ausführungen in ADF-Pipelines Ihre Datenflussaktivitäten ohne Rangfolgeneinschränkung in der Benutzeroberfläche hinzu.

Diese Option der drei verfügbaren Optionen weist normalerweise die kürzeste Ausführungsdauer auf. Da jeder parallele Datenfluss gleichzeitig in separaten Clustern ausgeführt wird, ist die Sortierung der Ereignisse nicht deterministisch.

##### <a name="overload-single-data-flow"></a>Überladen eines einzelnen Datenflusses

Wenn Sie Ihre gesamte Logik in einem einzelnen Datenfluss anordnen, verwendet ADF für die gesamte Ausführung denselben Auftragsausführungskontext in einer einzelnen Spark-Clusterinstanz.

Bei dieser Option ist die Verfolgung und Problembehandlung unter Umständen schwieriger, weil Ihre Geschäftsregeln und die Geschäftslogik durcheinander geworfen werden. Zudem weist diese Option keine gute Wiederverwendbarkeit auf.

##### <a name="execute-data-flows-serially"></a>Serielles Ausführen von Datenflüssen

Wenn Sie Ihre Datenflussaktivitäten in der Pipeline seriell ausführen und für die Azure IR-Konfiguration eine Gültigkeitsdauer festgelegt haben, werden die Computeressourcen (VMs) von ADF wiederverwendet. Dies führt zu kürzeren Dauern bei den nachfolgenden Ausführungen. Sie erhalten weiterhin einen neuen Spark-Kontext für jede Ausführung.

Diese Option der drei verfügbaren Optionen weist normalerweise die längste Ausführungsdauer des Gesamtprozesses auf. Sie ermöglicht aber eine saubere Trennung der logischen Vorgänge in jedem Datenflussschritt.

### <a name="configuration-panel"></a>Konfigurationsbereich

Im Konfigurationsbereich werden die spezifischen Einstellungen für die derzeit ausgewählte Transformation angezeigt. Wenn keine Transformation ausgewählt ist, wird der Datenfluss angezeigt. In der allgemeinen Datenflusskonfiguration können Sie den Namen und die Beschreibung auf der Registerkarte **Allgemein** bearbeiten oder Parameter über die Registerkarte **Parameter** hinzufügen. Weitere Informationen finden Sie unter [Mapping Data Flow-Parameter](parameters-data-flow.md).

Jede Transformation verfügt über mindestens vier Registerkarten für die Konfiguration.

#### <a name="transformation-settings"></a>Transformationseinstellungen

Die erste Registerkarte im Konfigurationsbereich jeder Transformation enthält die Einstellungen, die für diese Transformation spezifisch sind. Weitere Informationen finden Sie auf der Dokumentationsseite für diese Transformation.

![Registerkarte „Quelleinstellungen“](media/data-flow/source1.png "Registerkarte „Quelleinstellungen“")

#### <a name="optimize"></a>Optimierung

Die Registerkarte **Optimieren** enthält Einstellungen zum Konfigurieren von Partitionierungsschemas.

![Optimieren](media/data-flow/optimize1.png "Optimierung")

Die Standardeinstellung ist **Aktuelle Partitionierung verwenden**, mit der Azure Data Factory angewiesen wird, das native Partitionierungsschema der in Spark ausgeführten Datenflüsse zu verwenden. Für die meisten Szenarien wird diese Einstellung empfohlen.

Es gibt Fälle, in denen es ratsam sein kann, die Partitionierung anzupassen. Wenn beispielsweise Ihre Transformationen in einer einzigen Datei im Lake ausgegeben werden sollen, wählen Sie in einer Senkentransformation die Option **Einzelne Partition** aus.

Ein weiterer Fall, in dem die Steuerung der Partitionierungsschemas ggf. ratsam ist, ist die Optimierung der Leistung. Das Anpassen der Partitionierung bietet Kontrolle über die Verteilung Ihrer Daten auf Computeknoten und Datenstandortoptimierungen, die sowohl positive als auch negative Auswirkungen auf die gesamte Datenflussleistung haben können. Weitere Informationen finden Sie in der [Anleitung zur Leistung der Mapping Data Flow-Funktion](concepts-data-flow-performance.md).

Wenn Sie die Partitionierung für eine Transformation ändern möchten, wählen Sie die Registerkarte **Optimieren** und das Optionsfeld **Partitionierung festlegen** aus. Daraufhin werden mehrere Optionen für die Partitionierung angezeigt. Die beste Methode der Partitionierung richtet sich jeweils nach den Datenmengen, Kandidatenschlüsseln, Nullwerten und der Kardinalität. 

Eine bewährte Methode besteht darin, mit der Standardpartitionierung zu beginnen und dann verschiedene Partitionierungsoptionen auszuprobieren. Sie können Tests anhand von Debugausführungen der Pipeline durchführen und die Ausführungszeit sowie die Verwendung der Partition in jeder Transformationsgruppierung in der Ansicht „Überwachung“ anzeigen. Weitere Informationen finden Sie unter [Überwachen von Datenflüssen](concepts-data-flow-monitoring.md).

Die folgenden Partitionierungsoptionen sind verfügbar.

##### <a name="round-robin"></a>Roundrobin 

„Roundrobin“ steht für eine einfache Partition, bei der Daten automatisch gleichmäßig auf Partitionen verteilt werden. Verwenden Sie die Option „Roundrobin“, wenn Sie nicht über gute Kandidaten verfügen, um eine solide, intelligente Partitionierungsstrategie zu implementieren. Sie können die Anzahl der physischen Partitionen festlegen.

##### <a name="hash"></a>Hash

Azure Data Factory erzeugt einen Hash von Spalten, um einheitliche Partitionen zu erstellen, sodass Zeilen mit ähnlichen Werten in die gleiche Partition fallen. Führen Sie bei Verwendung der Option „Hash“ einen Test auf mögliche Partitionsungleichmäßigkeiten durch. Sie können die Anzahl der physischen Partitionen festlegen.

##### <a name="dynamic-range"></a>Dynamischer Bereich

Bei dieser Option werden dynamische Spark-Bereiche basierend auf den von Ihnen angegebenen Spalten oder Ausdrücken verwendet. Sie können die Anzahl der physischen Partitionen festlegen. 

##### <a name="fixed-range"></a>Fester Bereich

Erstellen Sie einen Ausdruck, der einen festen Bereich für Werte in Ihren partitionierten Datenspalten bereitstellt. Sie sollten über fundiertes Wissen über Ihre Daten verfügen, bevor Sie diese Option verwenden, um Partitionsungleichmäßigkeiten zu vermeiden. Die von Ihnen für den Ausdruck eingegebenen Werte werden als Teil einer Partitionsfunktion verwendet. Sie können die Anzahl der physischen Partitionen festlegen.

##### <a name="key"></a>Schlüssel

Wenn Sie gut mit der Kardinalität Ihrer Daten vertraut sind, kann die Schlüsselpartitionierung eine gute Strategie darstellen. Die Schlüsselpartitionierung erstellt Partitionen für jeden eindeutigen Wert in der Spalte. Sie können die Anzahl der Partitionen nicht festlegen, weil die Anzahl auf den eindeutigen Werten in den Daten basiert.

#### <a name="inspect"></a>Überprüfen

Die Registerkarte **Überprüfen** bietet einen Einblick in die Metadaten des Datenstroms, den Sie transformieren. Sie können die Spaltenanzahl, geänderte Spalten, hinzugefügte Spalten, Datentypen, die Spaltensortierung und Spaltenverweise sehen. **Überprüfen** ist eine schreibgeschützte Ansicht Ihrer Metadaten. Der Debugmodus muss nicht aktiviert sein, um die Metadaten im Bereich **Überprüfen** anzeigen zu können.

![Überprüfen](media/data-flow/inspect1.png "Überprüfen")

Wenn Sie die Form Ihrer Daten durch Transformationen ändern, wird der Fluss der Metadatenänderungen im Bereich **Überprüfen** angezeigt. Falls in Ihrer Quelltransformation kein definiertes Schema vorhanden ist, werden im Bereich **Überprüfen** keine Metadaten angezeigt. Fehlende Metadaten kommen in Schemaabweichungsszenarien häufiger vor.

#### <a name="data-preview"></a>Datenvorschau

Bei aktiviertem Debugmodus können Sie auf der Registerkarte **Datenvorschau** eine interaktive Momentaufnahme der Daten bei jeder Transformation anzeigen. Weitere Informationen finden Sie unter [Datenvorschau im Debugmodus](concepts-data-flow-debug-mode.md#data-preview).

### <a name="top-bar"></a>Obere Leiste

Die obere Leiste enthält Aktionen, die sich auf den gesamten Datenfluss auswirken, z. B. das Speichern und Überprüfen. Sie können auch mithilfe der Schaltflächen **Diagramm anzeigen** und **Diagramm ausblenden** zwischen dem Diagramm- und Konfigurationsmodus wechseln.

![Diagramm ausblenden](media/data-flow/hideg.png "Diagramm ausblenden")

Wenn Sie das Diagramm ausblenden, können Sie über die Schaltflächen **Zurück** und **Weiter** seitwärts durch die Transformationsknoten blättern.

![Schaltflächen „Zurück“ und „Weiter“](media/data-flow/showhide.png "Schaltflächen „Zurück“ und „Weiter“")

## <a name="next-steps"></a>Nächste Schritte

* Informieren Sie sich über die Erstellung einer [Quelltransformation](data-flow-source.md).
* Informieren Sie sich darüber, wie Sie Ihre Datenflüsse im [Debugmodus](concepts-data-flow-debug-mode.md) erstellen.
