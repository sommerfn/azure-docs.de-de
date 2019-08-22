---
title: Kopieraktivität in Azure Data Factory | Microsoft-Dokumentation
description: Hier finden Sie Informationen über die Kopieraktivität in Azure Data Factory, die Sie zum Kopieren von Daten aus einem unterstützten Quelldatenspeicher in einen unterstützten Senkendatenspeicher verwenden können.
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
ms.openlocfilehash: 23ae7b5cfec26fb2483a3e4ac13a1220888d76ee
ms.sourcegitcommit: e42c778d38fd623f2ff8850bb6b1718cdb37309f
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 08/19/2019
ms.locfileid: "69614268"
---
# <a name="copy-activity-in-azure-data-factory"></a>Kopieraktivität in Azure Data Factory

## <a name="overview"></a>Übersicht

> [!div class="op_single_selector" title1="Wählen Sie die von Ihnen verwendete Version des Data Factory-Diensts aus:"]
> * [Version 1](v1/data-factory-data-movement-activities.md)
> * [Aktuelle Version](copy-activity-overview.md)

In Azure Data Factory können Sie die Kopieraktivität verwenden, um Daten zwischen lokalen Datenspeichern und Clouddatenspeichern zu kopieren. Nach dem Kopieren können die Daten mit anderen Aktivitäten weiter transformiert und analysiert werden. Sie können die Kopieraktivität auch zum Veröffentlichen von Transformations- und Analyseergebnissen für die Verwendung für Business Intelligence (BI) und in Anwendungen verwenden.

![Rolle der Kopieraktivität](media/copy-activity-overview/copy-activity.png)

Die Kopieraktivität wird in [Integration Runtime](concepts-integration-runtime.md) ausgeführt. Für ein anderes Datenkopierszenario kann eine andere Integration Runtime-Konfiguration genutzt werden:

* Beim Kopieren von Daten zwischen Datenspeichern, auf die beide der öffentliche Zugriff über beliebige IPs über das Internet möglich ist, kann die Kopieraktivität von **Azure Integration Runtime** unterstützt werden. Azure Integration Runtime ist sicher, zuverlässig, skalierbar und [global verfügbar](concepts-integration-runtime.md#integration-runtime-location).
* Beim Kopieren von Daten aus bzw. in lokale Datenspeicher oder Datenspeicher in einem Netzwerk mit Zugriffssteuerung (z.B. Azure Virtual Network) müssen Sie eine **selbstgehostete integrierte Runtime** einrichten, um das Kopieren der Daten zu ermöglichen.

Integration Runtime muss mit jedem Quell- und Senkendatenspeicher verknüpft werden. Erfahren Sie, wie die Kopieraktivität [bestimmt, welche IR verwendet werden muss](concepts-integration-runtime.md#determining-which-ir-to-use).

Die Kopieraktivität durchläuft die folgenden Phasen, um Daten aus einer Quelle in eine Senke zu kopieren. Folgendes gilt für den Dienst, auf dem die Kopieraktivität basiert:

1. Er liest Daten aus einem Quelldatenspeicher.
2. Er führt die Serialisierung/Deserialisierung, Komprimierung/Dekomprimierung, Spaltenzuordnung usw. durch. Diese Vorgänge erfolgen basierend auf den Konfigurationen von Eingabedataset, Ausgabedataset und Kopieraktivität.
3. Er schreibt Daten in den Senken-/Zieldatenspeicher.

![Kopieraktivität – Übersicht](media/copy-activity-overview/copy-activity-overview.png)

## <a name="supported-data-stores-and-formats"></a>Unterstützte Datenspeicher und Formate

[!INCLUDE [data-factory-v2-supported-data-stores](../../includes/data-factory-v2-supported-data-stores.md)]

### <a name="supported-file-formats"></a>Unterstützte Dateiformate

Sie können die Kopieraktivität verwenden, um **Dateien unverändert** zwischen zwei dateibasierten Datenspeichern zu kopieren. In diesem Fall werden die Daten effizient ohne Serialisierung/Deserialisierung kopiert.

Die Kopieraktivität unterstützt auch das Lesen von und Schreiben in Dateien in bestimmten Formaten: **Text, JSON, Avro, ORC und Parquet** sowie Komprimieren und Dekomprimieren von Dateien mit den folgenden Codecs: **GZip, Deflate, BZip2 und ZipDeflate**. Weitere Informationen finden Sie unter [Unterstützte Datei- und Komprimierungsformate](supported-file-formats-and-compression-codecs.md).

Sie können z.B. folgende Kopieraktivitäten ausführen:

* Kopieren von Daten in einer lokalen SQL Server-Instanz und Schreiben in Azure Data Lake Storage Gen2 im Parquet-Format
* Kopieren von Dateien im Textformat (CSV) aus dem lokalen Dateisystem und Schreiben in einen Azure-Blob im Avro-Format
* Kopieren von ZIP-Dateien aus dem lokalen Dateisystem und Dekomprimieren in Azure Data Lake Storage Gen2
* Kopieren von Daten im GZip-komprimierten Textformat (CSV) aus einem Azure-Blob und Schreiben in Azure SQL-Datenbank
* Und viele weitere Fälle, die Serialisierung/Deserialisierung oder Komprimierung/Dekomprimierung erfordern.

## <a name="supported-regions"></a>Unterstützte Regionen

Der Dienst, der die Kopieraktivität unterstützt, ist weltweit in den Regionen und Ländern verfügbar, die unter [Standorte der Azure Integration Runtime](concepts-integration-runtime.md#integration-runtime-location) aufgeführt sind. Die global verfügbare Topologie gewährleistet effiziente Datenverschiebungen, die regionsübergreifende Hops in der Regel vermeiden. Unter [Dienste nach Region](https://azure.microsoft.com/regions/#services) erfahren Sie, in welchen Regionen Data Factory und die Datenverschiebung verfügbar sind.

## <a name="configuration"></a>Konfiguration

Um die Kopieraktivität in Azure Data Factory zu verwenden, ist Folgendes erforderlich:

1. **Erstellen Sie verknüpfte Dienste für Quell- und Senkendatenspeicher.** Im Abschnitt „Eigenschaften des verknüpften Diensts“ im Connectorartikel finden Sie Informationen zur Konfiguration und zu unterstützten Eigenschaften. Sie finden die Liste der unterstützten Connectors im Abschnitt [Unterstützte Datenspeicher und Formate](#supported-data-stores-and-formats).
2. **Erstellen Sie Datasets für die Quelle und die Senke.** Im Abschnitt mit Dataseteigenschaften in den Artikeln zu Quell- und Senkenconnectors finden Sie Informationen zur Konfiguration und zu unterstützten Eigenschaften.
3. **Erstellen Sie eine Pipeline mit einer Kopieraktivität.** Der nächste Abschnitt enthält ein Beispiel.

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

### <a name="syntax-details"></a>Syntaxdetails

| Eigenschaft | BESCHREIBUNG | Erforderlich |
|:--- |:--- |:--- |
| type | Die type-Eigenschaft einer Kopieraktivität muss auf Folgendes festgelegt werden: **Copy** | Ja |
| inputs | Geben Sie das Dataset an, das Sie erstellt haben und das auf die Quelldaten verweist. Die Kopieraktivität unterstützt nur eine einzelne Eingabe. | Ja |
| outputs | Geben Sie das Dataset an, das Sie erstellt haben und das auf die Senkendaten verweist. Die Kopieraktivität unterstützt nur eine einzelne Ausgabe. | Ja |
| typeProperties | Eine Gruppe von Eigenschaften zum Konfigurieren der Kopieraktivität. | Ja |
| source | Geben Sie den Quelltyp für den Kopiervorgang und die zugehörigen Eigenschaften zum Abrufen von Daten an.<br/><br/>Informationen finden Sie im Abschnitt mit den Eigenschaften der Kopieraktivität im Connectorartikel, der unter [Unterstützte Datenspeicher und Formate](#supported-data-stores-and-formats) aufgeführt ist. | Ja |
| sink | Geben Sie den Senkentyp für den Kopiervorgang und die zugehörigen Eigenschaften zum Schreiben von Daten an.<br/><br/>Informationen finden Sie im Abschnitt mit den Eigenschaften der Kopieraktivität im Connectorartikel, der unter [Unterstützte Datenspeicher und Formate](#supported-data-stores-and-formats) aufgeführt ist. | Ja |
| translator | Geben Sie explizite Spaltenzuordnungen von der Quelle zur Senke an. Gilt, wenn das Standardverhalten für den Kopiervorgang Ihre Anforderungen nicht erfüllen kann.<br/><br/>Informationen finden Sie unter [Schema- und Datentypzuordnung](copy-activity-schema-and-type-mapping.md). | Nein |
| dataIntegrationUnits | Geben Sie die Leistungsfähigkeit von [Azure Integration Runtime](concepts-integration-runtime.md) an, um das Kopieren der Daten zu unterstützen. Ehemals Einheiten für Clouddatenverschiebungen. <br/><br/>Informationen finden Sie unter [Datenintegrationseinheiten](copy-activity-performance.md#data-integration-units). | Nein |
| parallelCopies | Geben Sie die Parallelität an, die die Kopieraktivität beim Lesen von Daten aus der Quelle und beim Schreiben von Daten in die Senke verwenden soll.<br/><br/>Informationen finden Sie unter [Parallele Kopie](copy-activity-performance.md#parallel-copy). | Nein |
| enableStaging<br/>stagingSettings | Wählen Sie aus, dass die vorläufigen Daten in einem Blob-Speicher bereitgestellt werden, anstatt Daten direkt aus der Quelle in die Senke zu kopieren.<br/><br/>Nützliche Szenarien und Konfigurationsdetails finden Sie unter [Gestaffeltes Kopieren](copy-activity-performance.md#staged-copy). | Nein |
| enableSkipIncompatibleRow<br/>redirectIncompatibleRowSettings| Wählen Sie aus, wie nicht kompatible Zeilen beim Kopieren von Daten aus der Quelle in die Senke behandelt werden sollen.<br/><br/>Informationen finden Sie unter [Fehlertoleranz](copy-activity-fault-tolerance.md). | Nein |

## <a name="monitoring"></a>Überwachung

Die Ausführung der Kopieraktivität kann auf der Azure Data Factory-Benutzeroberfläche unter „Erstellen und überwachen“ oder programmgesteuert überwacht werden.

### <a name="monitor-visually"></a>Visuelle Überwachung

Navigieren Sie zur visuellen Überwachung der Ausführung der Kopieraktivität zu Ihrer Data Factory und anschließend zu **Erstellen und überwachen** -> **Registerkarte „Überwachen“** . Dort wird eine Liste mit Pipelineausführungen und einem Link zum Anzeigen der Aktivitätsausführungen in der Spalte **Aktionen** angezeigt.

![Überwachen der Pipelineausführungen](./media/load-data-into-azure-data-lake-store/monitor-pipeline-runs.png)

Klicken Sie auf diese Option, um die Liste mit den Aktivitäten in dieser Pipelineausführung anzuzeigen. In der Spalte **Aktionen** stehen Links zur Eingabe, zur Ausgabe, zu ggf. aufgetretenen Fehlern und zu Details der Kopieraktivität zur Verfügung.

![Überwachung der Aktivitätsausführungen](./media/load-data-into-azure-data-lake-store/monitor-activity-runs.png)

Klicken Sie unter **Aktionen** auf den Link **Details**, um Details und Leistungsmerkmale für die Ausführung der Kopieraktivität anzuzeigen. Zu den angezeigten Informationen zählen neben Umfang/Zeilen/Dateien der Daten, die aus der Quelle in die Senke kopiert wurden, unter anderem der Durchsatz, die durchlaufenen Schritte und die jeweilige Dauer sowie die verwendeten Konfigurationen für Ihr Kopierszenario.

>[!TIP]
>In einigen Szenarien finden Sie oben auf der Seite zur Kopierüberwachung auch **Tipps zur Leistungsoptimierung**, in denen der identifizierte Engpass aufgeführt ist, und Sie erfahren, welche Änderungen vorgenommen werden müssen, um den Kopierdurchsatz zu erhöhen. Ein Beispiel mit Details finden Sie [hier](#performance-and-tuning).

**Beispiel: Kopieren aus Amazon S3 in Azure Data Lake Store**
![Überwachen von Details der Aktivitätsausführung](./media/copy-activity-overview/monitor-activity-run-details-adls.png)

**Beispiel: Gestaffeltes Kopieren aus Azure SQL-Datenbank in Azure SQL Data Warehouse**
![Überwachen von Details der Aktivitätsausführung](./media/copy-activity-overview/monitor-activity-run-details-sql-dw.png)

### <a name="monitor-programmatically"></a>Programmgesteuerte Überwachung

Ausführungsdetails der Kopieraktivität und Leistungsmerkmale werden auch im Ausgabeabschnitt im Ausführungsergebnis der Kopieraktivität zurückgegeben. Im Anschluss finden Sie eine umfassende Liste. Angezeigt werden jeweils nur Details, die für Ihr Kopierszenario relevant sind. Informationen zum Überwachen der Aktivitätsausführung finden Sie im [Abschnitt mit Schnellstartanleitungen für die Überwachung ](quickstart-create-data-factory-dot-net.md#monitor-a-pipeline-run).

| Eigenschaftenname  | BESCHREIBUNG | Unit |
|:--- |:--- |:--- |
| dataRead | Größe der Daten, die aus der Quelle gelesen werden | Int64-Wert in **Bytes** |
| dataWritten | Größe der Daten, die in die Senke geschrieben werden | Int64-Wert in **Bytes** |
| filesRead | Die Anzahl von Dateien, die beim Kopieren von Daten aus dem Dateispeicher kopiert werden. | Int64-Wert (ohne Einheit) |
| filesWritten | Die Anzahl von Dateien, die beim Kopieren von Daten in den Dateispeicher kopiert werden. | Int64-Wert (ohne Einheit) |
| sourcePeakConnections | Maximale Anzahl gleichzeitiger Verbindungen mit dem Quelldatenspeicher während der Ausführung der Kopieraktivität. | Int64-Wert (ohne Einheit) |
| sinkPeakConnections | Maximale Anzahl gleichzeitiger Verbindungen mit dem Zieldatenspeicher während der Ausführung der Kopieraktivität. | Int64-Wert (ohne Einheit) |
| rowsRead | Anzahl der Zeilen, die aus der Quelle gelesen werden (für eine Binärkopie nicht verfügbar) | Int64-Wert (ohne Einheit) |
| rowsCopied | Anzahl der Zeilen, die in die Senke kopiert werden (für eine Binärkopie nicht verfügbar) | Int64-Wert (ohne Einheit) |
| rowsSkipped | Anzahl der übersprungenen, nicht kompatiblen Zeilen. Sie können das Feature aktivieren, indem Sie „enableSkipIncompatibleRow“ auf „true“ festlegen. | Int64-Wert (ohne Einheit) |
| copyDuration | Dauer des Kopiervorgangs | Int32-Wert in Sekunden |
| throughput | Verhältnis, mit dem Daten übertragen werden | Gleitkommazahl in **KB/s** |
| sourcePeakConnections | Maximale Anzahl gleichzeitiger Verbindungen zum Quelldatenspeicher während des Kopiervorgangs | Int32-Wert |
| sinkPeakConnections| Maximale Anzahl gleichzeitiger Verbindungen zum Senkendatenspeicher während des Kopiervorgangs| Int32-Wert |
| sqlDwPolyBase | Wenn PolyBase beim Kopieren von Daten in SQL Data Warehouse verwendet wird. | Boolean |
| redshiftUnload | Wenn UNLOAD beim Kopieren von Daten aus Redshift verwendet wird. | Boolean |
| hdfsDistcp | Wenn DistCp beim Kopieren von Daten aus HDFS verwendet wird. | Boolean |
| effectiveIntegrationRuntime | Zeigt an, welche Integration Runtimes verwendet werden, um die Aktivitätsausführung zu unterstützen. Dabei wird folgendes Format verwendet: `<IR name> (<region if it's Azure IR>)`. | Text (Zeichenfolge) |
| usedDataIntegrationUnits | Die effektiven Datenintegrationseinheiten während des Kopiervorgangs. | Int32-Wert |
| usedParallelCopies | Die effektiven parallelen Kopien während des Kopiervorgangs. | Int32-Wert |
| redirectRowPath | Pfad zum Protokoll der übersprungenen, nicht kompatible Zeilen im Blobspeicher, den Sie unter „redirectIncompatibleRowSettings“ konfigurieren. Siehe das folgende Beispiel. | Text (Zeichenfolge) |
| executionDetails | Ausführlichere Informationen zu den Phasen, die die Kopieraktivität durchläuft, sowie zu den entsprechenden Schritten, zur Dauer, zu den verwenden Konfigurationen und Ähnlichem. Da sich der Abschnitt ändern kann, empfiehlt es sich nicht, ihn zu analysieren.<br/><br/>ADF meldet außerdem die genaue Dauer (in Sekunden) für die jeweiligen Schritte unter `detailedDurations`. Die Zeitangaben (für die Dauer) dieser Schritte sind exklusiv, und es werden nur die für die angegebene Kopieraktivität geltenden Zeiten angezeigt:<br/>- **Abfragedauer** (`queuingDuration`): Die bis zum tatsächlichen Start der Kopieraktivität in der Integration Runtime verstrichene Zeit. Wenn Sie eine selbstgehostete IR verwenden und dieser Wert sehr groß ist, sollten Sie die Kapazität und Nutzung der IR überprüfen und entsprechend Ihrer Workload zentral hoch- oder herunterskalieren. <br/>- **Dauer des Kopiervorbereitungsskripts** (`preCopyScriptDuration`): Die Zeit, die zwischen dem Starten der Kopieraktivität in der IR und dem Abschließen der Kopieraktivität für die Ausführung des Kopiervorbereitungsskripts im Senkendatenspeicher verstrichen ist. Wird beim Konfigurieren des Kopiervorbereitungsskripts angewandt. <br/>- **Zeit bis zum ersten Byte** (`timeToFirstByte`): Die verstrichene Zeit zwischen dem Ende des vorherigen Schritts und dem Empfangen des ersten Bytes aus dem Quelldatenspeicher durch die IR. Wird auf nicht dateibasierte Quellen angewandt. Wenn dieser Wert groß ist, empfiehlt es sich, die Abfrage bzw. den Server zu überprüfen und ggf. zu optimieren.<br/>- **Übertragungsdauer** (`transferDuration`): Die verstrichene Zeit zwischen dem Ende des vorherigen Schritts und dem Übertragen aller Daten aus der Quelle in die Senke durch die IR. | Array |
| perfRecommendation | Tipps zur Leistungsoptimierung beim Kopieren. Weitere Informationen finden Sie im Abschnitt [Leistung und Optimierung](#performance-and-tuning). | Array |

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

Unter [Schema- und Datentypzuordnung](copy-activity-schema-and-type-mapping.md) wird beschrieben, wie die Kopieraktivität die Quelldaten der Senke zugeordnet.

## <a name="fault-tolerance"></a>Fehlertoleranz

Standardmäßig beendet die Kopieraktivität das Kopieren von Daten und gibt einen Fehler zurück, wenn nicht kompatible Daten zwischen Quelle und Senke festgestellt werden. Sie können explizit konfigurieren, dass nicht kompatible Zeilen übersprungen und protokolliert werden, und nur die kompatiblen Daten kopieren, damit der Kopiervorgang erfolgreich ist. Weitere Details finden Sie unter [Copy Activity fault tolerance - skip incompatible rows](copy-activity-fault-tolerance.md) (Fehlertoleranz der Kopieraktivität – Überspringen inkompatibler Zeilen).

## <a name="performance-and-tuning"></a>Leistung und Optimierung

Der Artikel [Handbuch zur Leistung und Optimierung der Kopieraktivität](copy-activity-performance.md)beschreibt wichtige Faktoren, die sich auf die Leistung der Datenverschiebung (Kopieraktivität) in Azure Data Factory auswirken. Außerdem wird die beobachtete Leistung bei internen Tests aufgeführt und es werden verschiedene Methoden zur Optimierung der Leistung der Kopieraktivität erläutert.

In einigen Fällen werden bei der Ausführung einer Kopieraktivität in ADF direkt **Tipps zur Leistungsoptimierung** oben auf der [Seite zur Überwachung der Kopieraktivität](#monitor-visually) angezeigt, wie im folgenden Beispiel dargestellt. Es wird nicht nur der im jeweiligen Kopiervorgang identifizierte Engpass angezeigt, sondern Sie erfahren auch, welche Änderungen Sie zum Erhöhen des Durchsatzes durchführen müssen. Die Tipps zur Leistungsoptimierung enthalten derzeit Vorschläge wie die Verwendung von PolyBase beim Kopieren von Daten in das Azure SQL Data Warehouse, um die Anzahl der Azure Cosmos DB-sRU oder Azure SQL DB-DTUs zu erhöhen, wenn die Ressource auf der Datenspeicherseite der Engpass ist, um die unnötig bereitgestellte Kopie zu entfernen usw. Die Regeln für die Leistungsoptimierung werden auch schrittweise ergänzt werden.

**Beispiel: Kopieren in Azure SQL-Datenbank mit Tipps für die Leistungsoptimierung**

In diesem Beispiel bemerkt ADF während des Kopiervorgangs, dass die Senke von Azure SQL-DB eine hohe DTU-Auslastung erreicht, sodass die Schreibvorgänge verlangsamt werden. Daher wird vorgeschlagen, einen Tarif von Azure SQL-DB mit mehr DTU zu verwenden.

![Überwachung des Kopiervorgangs mit Tipps für die Leistungsoptimierung](./media/copy-activity-overview/copy-monitoring-with-performance-tuning-tips.png)

## <a name="incremental-copy"></a>Inkrementelles Kopieren
Data Factory unterstützt Szenarien für das inkrementelle Kopieren von Deltadaten aus einem Quelldatenspeicher in einen Senkendatenspeicher. Informationen finden Sie unter [Inkrementelles Laden von Daten aus einem Quelldatenspeicher in einen Zieldatenspeicher](tutorial-incremental-copy-overview.md).

## <a name="read-and-write-partitioned-data"></a>Lesen und Schreiben partitionierter Daten
In Version 1 unterstützte Azure Data Factory das Lesen oder Schreiben von partitionierten Daten mithilfe der Systemvariablen SliceStart/SliceEnd/WindowStart/WindowEnd. In der aktuellen Version können Sie dieses Verhalten mithilfe eines Pipelineparameters und der Anfangszeit/geplanten Zeit eines Triggers als Wert des Parameters erreichen. Weitere Informationen finden Sie unter [Lesen oder Schreiben partitionierter Daten](how-to-read-write-partitioned-data.md).

## <a name="next-steps"></a>Nächste Schritte
Weitere Informationen finden Sie in den folgenden Schnellstartanleitungen, Tutorials und Beispielen:

- [Kopieren von Daten von einem Speicherort an einen anderen Speicherort in der gleichen Azure Blob Storage-Instanz](quickstart-create-data-factory-dot-net.md)
- [Kopieren von Daten aus Azure Blob Storage in Azure SQL-Datenbank](tutorial-copy-data-dot-net.md)
- [Kopieren von Daten aus einer lokalen SQL Server-Instanz in Azure](tutorial-hybrid-copy-powershell.md)
