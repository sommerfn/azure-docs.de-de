---
title: Kopieraktivität in Azure Data Factory
description: Erfahren Sie mehr über die Kopieraktivität in Azure Data Factory. Mit der dieser Aktivität können Sie Daten aus einem unterstützten Quelldatenspeicher in einen unterstützten Senkendatenspeicher kopieren.
services: data-factory
documentationcenter: ''
author: linda33wj
manager: craigg
ms.reviewer: douglasl
ms.service: data-factory
ms.workload: data-services
ms.tgt_pltfrm: na
ms.topic: conceptual
ms.date: 08/12/2019
ms.author: jingwang
ms.openlocfilehash: b88983b4941143e5323ee795908cb332bdd79817
ms.sourcegitcommit: 609d4bdb0467fd0af40e14a86eb40b9d03669ea1
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 11/06/2019
ms.locfileid: "73678420"
---
# <a name="copy-activity-in-azure-data-factory"></a>Kopieraktivität in Azure Data Factory

> [!div class="op_single_selector" title1="Wählen Sie die von Ihnen verwendete Data Factory-Version aus:"]
> * [Version 1](v1/data-factory-data-movement-activities.md)
> * [Aktuelle Version](copy-activity-overview.md)

In Azure Data Factory können Sie die Kopieraktivität verwenden, um Daten zwischen lokalen Datenspeichern und Clouddatenspeichern zu kopieren. Nach dem Kopieren können Sie andere Aktivitäten verwenden, um die Daten weiter zu transformieren und zu analysieren. Sie können die Kopieraktivität auch zum Veröffentlichen von Transformations- und Analyseergebnissen verwenden, um sie für Business Intelligence (BI) und Anwendungen zu nutzen.

![Die Aufgabe der Kopieraktivität](media/copy-activity-overview/copy-activity.png)

Die Kopieraktivität wird in einer [Integration Runtime](concepts-integration-runtime.md) ausgeführt. Sie können unterschiedliche Integration Runtime-Typen für unterschiedliche Datenkopierszenarien verwenden:

* Beim Kopieren von Daten zwischen zwei Datenspeichern, die über das Internet von einer beliebigen IP-Adresse aus öffentlich zugänglich sind, können Sie die Azure Integration Runtime für die Kopieraktivität verwenden. Diese Integration Runtime ist sicher, zuverlässig, skalierbar und [weltweit verfügbar](concepts-integration-runtime.md#integration-runtime-location).
* Beim Kopieren von Daten zwischen lokalen Datenspeichern oder Datenspeichern in einem Netzwerk mit Zugriffssteuerung (z. B. Azure Virtual Network) müssen Sie eine selbstgehostete Integration Runtime einrichten.

Eine Integration Runtime muss mit jedem Quell- und Senkendatenspeicher verknüpft werden. Informationen dazu, wie die zu verwendende Integration Runtime von der Kopieraktivität bestimmt wird, finden Sie unter [Bestimmen der zu verwendenden IR](concepts-integration-runtime.md#determining-which-ir-to-use).

Um Daten aus einer Quelle in eine Senke zu kopieren, führt der Dienst, der die Kopieraktivität ausführt, folgende Schritte durch:

1. Er liest Daten aus einem Quelldatenspeicher.
2. Er führt die Serialisierung/Deserialisierung, Komprimierung/Dekomprimierung, Spaltenzuordnung usw. durch. Diese Vorgänge erfolgen basierend auf den Konfigurationen von Eingabedataset, Ausgabedataset und Kopieraktivität.
3. Er schreibt Daten in den Senken-/Zieldatenspeicher.

![Kopieraktivität in Azure Data Factory](media/copy-activity-overview/copy-activity-overview.png)

## <a name="supported-data-stores-and-formats"></a>Unterstützte Datenspeicher und Formate

[!INCLUDE [data-factory-v2-supported-data-stores](../../includes/data-factory-v2-supported-data-stores.md)]

### <a name="supported-file-formats"></a>Unterstützte Dateiformate

Sie können die Kopieraktivität verwenden, um Dateien unverändert zwischen zwei dateibasierten Datenspeichern zu kopieren. In diesem Fall werden die Daten ohne jegliche Serialisierung oder Deserialisierung effizient kopiert.

Die Kopieraktivität unterstützt auch Lese- und Schreibvorgänge in Dateien mit folgenden Formaten:
- Text
- JSON
- Avro
- ORC
- Parquet

Die Kopieraktivität kann Dateien mit folgenden Codecs komprimieren und dekomprimieren: 
- GZip
- Deflate
- Bzip2
- ZipDeflate

Weitere Informationen finden Sie unter [Unterstützte Datei- und Komprimierungsformate](supported-file-formats-and-compression-codecs.md).

Sie können z. B. folgende Kopieraktivitäten ausführen:

* Kopieren von Daten aus einer lokalen SQL Server-Datenbank und Schreiben der Daten in Azure Data Lake Storage Gen2 im Parquet-Format
* Kopieren von Dateien im Textformat (CSV) aus einem lokalen Dateisystem und Schreiben in Azure Blob Storage im Avro-Format
* Kopieren von ZIP-Dateien aus einem lokalen Dateisystem, Dekomprimieren und Schreiben der Dateien in Azure Data Lake Storage Gen2
* Kopieren von Daten im Gzip-komprimierten Textformat (CSV) aus Azure Blob Storage und Schreiben in Azure SQL-Datenbank
* Viele weitere Aktivitäten, die eine Serialisierung/Deserialisierung oder Komprimierung/Dekomprimierung erfordern

## <a name="supported-regions"></a>Unterstützte Regionen

Der Dienst, der die Kopieraktivität unterstützt, ist weltweit in den Regionen und Ländern verfügbar, die unter [Standorte der Azure Integration Runtime](concepts-integration-runtime.md#integration-runtime-location) aufgeführt sind. Die global verfügbare Topologie gewährleistet effiziente Datenverschiebungen, die regionsübergreifende Hops in der Regel vermeiden. Unter [Produkte nach Region](https://azure.microsoft.com/regions/#services) können Sie die Verfügbarkeit von Data Factory und der Datenverschiebung in einer bestimmten Region überprüfen.

## <a name="configuration"></a>Konfiguration

Um die Kopieraktivität in Azure Data Factory zu verwenden, sind folgende Schritte erforderlich:

1. **Erstellen Sie verknüpfte Dienste für den Quell- und Senkendatenspeicher.** Im Abschnitt „Eigenschaften des verknüpften Diensts“ im Connectorartikel finden Sie Informationen zur Konfiguration und zu unterstützten Eigenschaften. Sie finden die Liste der unterstützten Connectors im Abschnitt [Unterstützte Datenspeicher und Formate](#supported-data-stores-and-formats) dieses Artikels.
2. **Erstellen Sie Datasets für die Quelle und die Senke.** In den Abschnitten zu Dataset-Eigenschaften in den Artikeln zu Quell- und Senkenconnectors finden Sie Informationen zur Konfiguration und zu unterstützten Eigenschaften.
3. **Erstellen Sie eine Pipeline mit der Kopieraktivität.** Der nächste Abschnitt enthält ein Beispiel.

### <a name="syntax"></a>Syntax

Die folgende Vorlage einer Kopieraktivität enthält eine vollständige Liste unterstützter Eigenschaften. Geben Sie diejenigen an, die zu Ihrem Szenario passen.

```json
"activities":[
    {
        "name": "CopyActivityTemplate",
        "type": "Copy",
        "inputs": [
            {
                "referenceName": "<source dataset name>",
                "type": "DatasetReference"
            }
        ],
        "outputs": [
            {
                "referenceName": "<sink dataset name>",
                "type": "DatasetReference"
            }
        ],
        "typeProperties": {
            "source": {
                "type": "<source type>",
                <properties>
            },
            "sink": {
                "type": "<sink type>"
                <properties>
            },
            "translator":
            {
                "type": "TabularTranslator",
                "columnMappings": "<column mapping>"
            },
            "dataIntegrationUnits": <number>,
            "parallelCopies": <number>,
            "enableStaging": true/false,
            "stagingSettings": {
                <properties>
            },
            "enableSkipIncompatibleRow": true/false,
            "redirectIncompatibleRowSettings": {
                <properties>
            }
        }
    }
]
```

#### <a name="syntax-details"></a>Syntaxdetails

| Eigenschaft | BESCHREIBUNG | Erforderlich? |
|:--- |:--- |:--- |
| type | Legen Sie für eine Kopieraktivität `Copy` fest. | Ja |
| inputs | Geben Sie das Dataset an, das Sie erstellt haben und das auf die Quelldaten verweist. Die Kopieraktivität unterstützt nur eine einzelne Eingabe. | Ja |
| outputs | Geben Sie das Dataset an, das Sie erstellt haben und das auf die Senkendaten verweist. Die Kopieraktivität unterstützt nur eine einzelne Ausgabe. | Ja |
| typeProperties | Geben Sie Eigenschaften zum Konfigurieren der Kopieraktivität an. | Ja |
| source | Geben Sie den Quelltyp für den Kopiervorgang und die zugehörigen Eigenschaften zum Abrufen von Daten an.<br/><br/>Weitere Informationen finden Sie im Abschnitt „Eigenschaften der Kopieraktivität“ im Connectorartikel unter [Unterstützte Datenspeicher und Formate](#supported-data-stores-and-formats). | Ja |
| sink | Geben Sie den Senkentyp für den Kopiervorgang und die zugehörigen Eigenschaften zum Schreiben von Daten an.<br/><br/>Weitere Informationen finden Sie im Abschnitt „Eigenschaften der Kopieraktivität“ im Connectorartikel unter [Unterstützte Datenspeicher und Formate](#supported-data-stores-and-formats). | Ja |
| translator | Geben Sie explizite Spaltenzuordnungen von der Quelle zur Senke an. Diese Eigenschaft wird angewendet, wenn das standardmäßige Kopierverhalten nicht Ihren Anforderungen entspricht.<br/><br/>Weitere Informationen finden Sie unter [Schemazuordnung in Kopieraktivität](copy-activity-schema-and-type-mapping.md). | Nein |
| dataIntegrationUnits | Geben Sie eine Maßeinheit an, die festlegt, wie viel Leistung der [Azure Integration Runtime](concepts-integration-runtime.md) für das Kopieren von Daten zur Verfügung steht. Diese Einheiten wurden früher als Einheiten für Clouddatenverschiebungen bezeichnet. <br/><br/>Weitere Informationen finden Sie unter [Datenintegrationseinheiten](copy-activity-performance.md#data-integration-units). | Nein |
| parallelCopies | Geben Sie die Parallelität an, die die Kopieraktivität beim Lesen von Daten aus der Quelle und beim Schreiben von Daten in die Senke verwenden soll.<br/><br/>Weitere Informationen finden Sie unter [Paralleles Kopieren](copy-activity-performance.md#parallel-copy). | Nein |
| enableStaging<br/>stagingSettings | Geben Sie an, ob die vorläufigen Daten im Blob Storage bereitgestellt werden, anstatt sie direkt aus der Quelle in die Senke zu kopieren.<br/><br/>Informationen zu nützlichen Szenarien und Konfigurationsdetails finden Sie unter [Gestaffeltes Kopieren](copy-activity-performance.md#staged-copy). | Nein |
| enableSkipIncompatibleRow<br/>redirectIncompatibleRowSettings| Wählen Sie aus, wie nicht kompatible Zeilen beim Kopieren von Daten aus der Quelle in die Senke behandelt werden sollen.<br/><br/>Weitere Informationen finden Sie unter [Fehlertoleranz](copy-activity-fault-tolerance.md). | Nein |

## <a name="monitoring"></a>Überwachung

Die Ausführung der Kopieraktivität kann auf der Azure Data Factory-Benutzeroberfläche unter **Erstellen und überwachen** oder programmgesteuert überwacht werden.

### <a name="monitor-visually"></a>Visuelle Überwachung

Um die Ausführung der Kopieraktivität visuell zu überwachen, wechseln Sie zu Ihrer Data Factory und dann zu **Erstellen und überwachen**. Auf der Registerkarte **Überwachen** wird eine Liste der Pipelineausführungen angezeigt. Dabei ist in der Spalte **Aktionen** eine Schaltfläche **Aktivitätsausführung anzeigen** enthalten:

![Überwachen der Pipelineausführungen](./media/load-data-into-azure-data-lake-store/monitor-pipeline-runs.png)

Wählen Sie **Aktivitätsausführungen anzeigen** aus, um die Liste der Aktivitäten in der Pipelineausführung anzuzeigen. In der Spalte **Aktionen** sehen Sie Links zur Eingabe, zur Ausgabe, zu möglichen Ausführungsfehlern und zu Details der Kopieraktivität:

![Überwachung der Aktivitätsausführungen](./media/load-data-into-azure-data-lake-store/monitor-activity-runs.png)

Wählen Sie in der Spalte **Aktionen** die Schaltfläche **Details** aus, um Ausführungsdetails und Leistungsmerkmale zur Kopieraktivität anzuzeigen. Zu den angezeigten Informationen zählen Umfang/Anzahl von Zeilen/Anzahl der aus der Quelle in die Senke kopierten Datendateien, Durchsatz, von der Kopieraktivität durchlaufene Schritte und deren Dauer sowie für das Kopierszenario verwendete Konfigurationen.

>[!TIP]
>In einigen Szenarien werden oben auf der Seite zur Überwachung des Kopiervorgangs auch **Tipps zur Leistungsoptimierung** angezeigt. Anhand der Tipps erfahren Sie, welche Engpässe identifiziert wurden und wie sich der Durchsatz des Kopiervorgangs steigern lässt. Ein Beispiel finden Sie im Abschnitt [Leistung und Optimierung](#performance-and-tuning) dieses Artikels.

**Beispiel: Kopieren aus Amazon S3 in Azure Data Lake Store**
![Überwachen von Details zur Aktivitätsausführung](./media/copy-activity-overview/monitor-activity-run-details-adls.png)

**Beispiel: Gestaffeltes Kopieren aus Azure SQL-Datenbank in Azure SQL Data Warehouse**
![Überwachen von Details zur Aktivitätsausführung](./media/copy-activity-overview/monitor-activity-run-details-sql-dw.png)

### <a name="monitor-programmatically"></a>Programmgesteuerte Überwachung

Ausführungsdetails und Leistungsmerkmale zur Kopieraktivität werden auch im Abschnitt **Ausführungsergebnis der Kopieraktivität** > **Ausgabe** zurückgegeben. Im Folgenden eine vollständige Liste der Eigenschaften, die zurückgegeben werden können. Es werden nur die Eigenschaften angezeigt, die auf Ihr Kopierszenario anwendbar sind. Informationen zum Überwachen von Aktivitätsausführungen finden Sie unter [Überwachen einer Pipelineausführung](quickstart-create-data-factory-dot-net.md#monitor-a-pipeline-run).

| Eigenschaftenname  | BESCHREIBUNG | Unit |
|:--- |:--- |:--- |
| dataRead | Die Menge der aus der Quelle gelesenen Daten. | Int64-Wert in Bytes |
| dataWritten | Die Menge der in die Senke geschriebenen Daten. | Int64-Wert in Bytes |
| filesRead | Die Anzahl der beim Kopiervorgang aus dem Dateispeicher kopierten Dateien. | Int64-Wert (ohne Einheit) |
| filesWritten | Die Anzahl der beim Kopiervorgang in den Dateispeicher kopierten Dateien. | Int64-Wert (ohne Einheit) |
| sourcePeakConnections | Die maximale Anzahl gleichzeitiger Verbindungen mit dem Quelldatenspeicher während der Ausführung der Kopieraktivität. | Int64-Wert (ohne Einheit) |
| sinkPeakConnections | Die maximale Anzahl gleichzeitiger Verbindungen mit dem Senkendatenspeicher während der Ausführung der Kopieraktivität. | Int64-Wert (ohne Einheit) |
| rowsRead | Die Anzahl der Zeilen, die aus der Quelle gelesen wurden (für Binärkopien nicht verfügbar). | Int64-Wert (ohne Einheit) |
| rowsCopied | Die Anzahl der Zeilen, die in die Senke kopiert wurden (für Binärkopien nicht verfügbar). | Int64-Wert (ohne Einheit) |
| rowsSkipped | Die Anzahl der übersprungenen, nicht kompatiblen Zeilen. Nicht kompatible Zeilen können übersprungen werden, indem Sie `enableSkipIncompatibleRow` auf „true“ festlegen. | Int64-Wert (ohne Einheit) |
| copyDuration | Die Dauer des Kopiervorgangs. | Int32-Wert in Sekunden |
| throughput | Die Datenübertragungsrate. | Gleitkommazahl in KB/s |
| sourcePeakConnections | Die maximale Anzahl gleichzeitiger Verbindungen mit dem Quelldatenspeicher während der Ausführung der Kopieraktivität. | Int32-Wert (ohne Einheit) |
| sinkPeakConnections| Die maximale Anzahl gleichzeitiger Verbindungen mit dem Senkendatenspeicher während der Ausführung der Kopieraktivität.| Int32-Wert (ohne Einheit) |
| sqlDwPolyBase | Gibt an, ob PolyBase beim Kopieren von Daten in SQL Data Warehouse verwendet wird. | Boolean |
| redshiftUnload | Gibt an, ob UNLOAD beim Kopieren von Daten aus Redshift verwendet wird. | Boolean |
| hdfsDistcp | Gibt an, ob DistCp beim Kopieren von Daten aus HDFS verwendet wird. | Boolean |
| effectiveIntegrationRuntime | Die Integration Runtimes (IR), die zur Unterstützung der Aktivitätsausführung verwendet werden, im Format `<IR name> (<region if it's Azure IR>)`. | Text (Zeichenfolge) |
| usedDataIntegrationUnits | Die effektiven Datenintegrationseinheiten während des Kopiervorgangs. | Int32-Wert |
| usedParallelCopies | Die effektiven parallelen Kopien während des Kopiervorgangs. | Int32-Wert |
| redirectRowPath | Der Pfad zum Protokoll der übersprungenen, nicht kompatiblen Zeilen im Blob Storage, den Sie in der Eigenschaft `redirectIncompatibleRowSettings` konfigurieren. Siehe [Fehlertoleranz](#fault-tolerance) weiter unten in diesem Artikel. | Text (Zeichenfolge) |
| executionDetails | Ausführlichere Informationen zu den Phasen, die die Kopieraktivität durchläuft, sowie zu den entsprechenden Schritten, zur Dauer, zu den Konfigurationen usw. Es wird davon abgeraten, diesen Abschnitt zu analysieren, da er sich möglicherweise ändert.<br/><br/>Unter `detailedDurations` wird von der Data Factory außerdem die genaue Dauer (in Sekunden) für die jeweiligen Phasen angegeben. Die Zeitangaben für die Dauer dieser Schritte sind exklusiv. Es wird nur die Dauer für die jeweils ausgeführte Kopieraktivität berücksichtigt:<br/>**Abfragedauer** (`queuingDuration`): Die bis zum tatsächlichen Start der Kopieraktivität in der Integration Runtime verstrichene Zeit. Wenn Sie eine selbstgehostete IR verwenden und dieser Wert hoch ist, sollten Sie die Kapazität und Nutzung der IR überprüfen und entsprechend Ihrer Workload zentral hochskalieren. <br/>**Dauer des Kopiervorbereitungsskripts** (`preCopyScriptDuration`): Die Zeit vom Start der Kopieraktivität in der IR bis zu dem Zeitpunkt, zu dem die Ausführung des Kopiervorbereitungsskripts im Senkendatenspeicher von der Kopieraktivität abgeschlossen wurde. Wird beim Konfigurieren des Kopiervorbereitungsskripts angewendet. <br/>**Zeit bis zum ersten Byte** (`timeToFirstByte`): Die Zeit vom Ende des vorherigen Schritts bis zu dem Zeitpunkt, zu dem das erste Byte aus dem Quelldatenspeicher von der IR empfangen wird. Wird auf nicht dateibasierte Quellen angewendet. Wenn dieser Wert hoch ist, sollte die Abfrage oder der Server überprüft und optimiert werden.<br/>**Übertragungsdauer** (`transferDuration`): Die Zeit vom Ende des vorherigen Schritts bis zu dem Zeitpunkt, zu dem alle Daten aus der Quelle von der IR in die Senke übertragen wurden. | Array |
| perfRecommendation | Tipps zur Leistungsoptimierung beim Kopieren. Weitere Informationen finden Sie unter [Leistung und Optimierung](#performance-and-tuning). | Array |

```json
"output": {
    "dataRead": 6198358,
    "dataWritten": 19169324,
    "filesRead": 1,
    "sourcePeakConnections": 1,
    "sinkPeakConnections": 2,
    "rowsRead": 39614,
    "rowsCopied": 39614,
    "copyDuration": 1325,
    "throughput": 4.568,
    "errors": [],
    "effectiveIntegrationRuntime": "DefaultIntegrationRuntime (West US)",
    "usedDataIntegrationUnits": 4,
    "usedParallelCopies": 1,
    "executionDetails": [
        {
            "source": {
                "type": "AzureBlobStorage"
            },
            "sink": {
                "type": "AzureSqlDatabase"
            },
            "status": "Succeeded",
            "start": "2019-08-06T01:01:36.7778286Z",
            "duration": 1325,
            "usedDataIntegrationUnits": 4,
            "usedParallelCopies": 1,
            "detailedDurations": {
                "queuingDuration": 2,
                "preCopyScriptDuration": 12,
                "transferDuration": 1311
            }
        }
    ],
    "perfRecommendation": [
        {
            "Tip": "Sink Azure SQL Database: The DTU utilization was high during the copy activity run. To achieve better performance, you are suggested to scale the database to a higher tier than the current 1600 DTUs.",
            "ReferUrl": "https://go.microsoft.com/fwlink/?linkid=2043368",
            "RuleName": "AzureDBTierUpgradePerfRecommendRule"
        }
    ]
}
```

## <a name="schema-and-data-type-mapping"></a>Schema- und Datentypzuordnung

Informationen dazu, wie die Quelldaten von der Kopieraktivität der Senke zugeordnet werden, finden Sie unter [Schema- und Datentypzuordnung](copy-activity-schema-and-type-mapping.md).

## <a name="fault-tolerance"></a>Fehlertoleranz

Wenn Datenzeilen in der Quelle und in der Senke nicht kompatibel sind, wird ein Fehler zurückgegeben, und der Kopiervorgang wird von der Kopieraktivität standardmäßig beendet. Damit der Kopiervorgang erfolgreich verläuft, können Sie die Kopieraktivität so konfigurieren, dass nicht kompatible Zeilen übersprungen und protokolliert werden und nur die kompatiblen Daten kopiert werden. Weitere Details finden Sie unter [Fehlertoleranz der Kopieraktivität](copy-activity-fault-tolerance.md).

## <a name="performance-and-tuning"></a>Leistung und Optimierung

Im [Handbuch zur Leistung und Optimierung der Kopieraktivität](copy-activity-performance.md) werden wichtige Faktoren beschrieben, die sich auf die Leistung der Kopieraktivität beim Verschieben von Daten in Azure Data Factory auswirken. Außerdem finden Sie dort in Tests ermittelte Leistungswerte und Möglichkeiten, die Leistung der Kopieraktivität zu optimieren.

In einigen Szenarien werden bei der Ausführung einer Kopieraktivität in Data Factory oben auf der **Seite zur Überwachung der Kopieraktivität** verschiedene [Tipps zur Leistungsoptimierung](#monitor-visually) angezeigt, wie im folgenden Beispiel zu sehen. Die Tipps geben Aufschluss über den Engpass, der für den jeweiligen Kopiervorgang ermittelt wurde. Außerdem erfahren Sie, wie Sie den Durchsatz des Kopiervorgangs steigern können. In den Tipps zur Leistungsoptimierung erhalten Sie beispielsweise die Empfehlung, beim Kopieren von Daten in Azure SQL Data Warehouse PolyBase zu verwenden, die Anzahl der Azure Cosmos DB-RUs oder der Azure SQL-Datenbank-DTUs zu erhöhen, wenn die Ressource auf der Datenspeicherseite den Engpass verursacht, oder unnötig bereitgestellte Kopie zu entfernen.

**Beispiel: Kopieren in Azure SQL-Datenbank mit einem Tipp zur Leistungsoptimierung**

In diesem Beispiel wird von Data Factory während eines Kopiervorgangs eine hohe DTU-Nutzung in der Microsoft Azure SQL-Datenbanksenke ermittelt. Dadurch werden Schreibvorgänge verlangsamt. Es wird empfohlen, die Anzahl der DTUs auf der Ebene der Azure SQL-Datenbank zu erhöhen:

![Überwachung des Kopiervorgangs mit Tipps für die Leistungsoptimierung](./media/copy-activity-overview/copy-monitoring-with-performance-tuning-tips.png)

## <a name="incremental-copy"></a>Inkrementelles Kopieren
Data Factory unterstützt das inkrementelle Kopieren von Deltadaten von einem Quelldatenspeicher in einen Senkendatenspeicher. Weitere Informationen finden Sie unter [Tutorial: Inkrementelles Kopieren von Daten](tutorial-incremental-copy-overview.md).

## <a name="next-steps"></a>Nächste Schritte
Weitere Informationen finden Sie in den folgenden Schnellstartanleitungen, Tutorials und Beispielen:

- [Kopieren von Daten zwischen Speicherorten unter demselben Azure Blob Storage-Konto](quickstart-create-data-factory-dot-net.md)
- [Kopieren von Daten aus Azure Blob Storage nach Azure SQL-Datenbank](tutorial-copy-data-dot-net.md)
- [Kopieren von Daten aus einer lokalen SQL Server-Datenbank nach Azure](tutorial-hybrid-copy-powershell.md)
