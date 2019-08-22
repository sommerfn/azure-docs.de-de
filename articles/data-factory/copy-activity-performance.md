---
title: Handbuch zur Leistung und Optimierung der Kopieraktivität in Azure Data Factory | Microsoft-Dokumentation
description: Hier erfahren Sie, welche Faktoren sich entscheidend auf die Leistung auswirken, wenn Sie Daten in Azure Data Factory mithilfe der Kopieraktivität verschieben.
services: data-factory
documentationcenter: ''
author: linda33wj
manager: craigg
ms.reviewer: douglasl
ms.service: data-factory
ms.workload: data-services
ms.tgt_pltfrm: na
ms.topic: conceptual
ms.date: 07/02/2019
ms.author: jingwang
ms.openlocfilehash: d8ce0a4f6bacdd1c8c858d474e6f3957a23c6357
ms.sourcegitcommit: 5d6c8231eba03b78277328619b027d6852d57520
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 08/13/2019
ms.locfileid: "68967362"
---
# <a name="copy-activity-performance-and-tuning-guide"></a>Handbuch zur Leistung und Optimierung der Kopieraktivität
> [!div class="op_single_selector" title1="Wählen Sie die von Ihnen verwendete Version von Azure Data Factory aus:"]
> * [Version 1](v1/data-factory-copy-activity-performance.md)
> * [Aktuelle Version](copy-activity-performance.md)


Die Azure Data Factory-Kopieraktivität bietet eine erstklassige, sichere und zuverlässige Lösung zum Laden von Daten mit hoher Leistung. Sie können damit Dutzende von Terabytes an Daten täglich zwischen einer großen Vielzahl von Cloud- und lokalen Datenspeichern kopieren. Schnelles Datenladen ist wesentlich, um sicherzustellen, dass Sie sich auf das „Big Data“-Kernproblem konzentrieren können: Erstellen erweiterter Analyselösungen und tiefe Einblicke in all diese Daten.

Azure bietet eine Reihe von Datenspeicher- und Data Warehouse-Lösungen auf Unternehmensniveau. Die Kopieraktivität bietet ein hochoptimiertes Datenladeverfahren, das einfach zu konfigurieren und einzurichten ist. Mit nur einer Kopieraktivität können Sie Daten in Folgendes laden:

* Azure SQL Data Warehouse mit 1,2 Gbit/s
* Azure Blob Storage mit 1,0 Gbit/s
* Azure Data Lake Store mit 1,0 Gbit/s

Dieser Artikel beschreibt Folgendes:

* [Leistungsreferenznummern](#performance-reference) für unterstützte Quellen- und Senkendatenspeicher, die Ihnen bei der Projektplanung helfen;
* Features, die den Kopierdurchsatz in verschiedenen Szenarien verbessern können, einschließlich [Datenintegrationseinheiten](#data-integration-units) (DIUs), [paralleler Kopien](#parallel-copy) und [gestaffeltem Kopieren](#staged-copy);
* [Leitfaden zur Leistungsoptimierung](#performance-tuning-steps) zum Optimieren der Leistung und der Schlüsselfaktoren, die die Kopierleistung beeinflussen können.

> [!NOTE]
> Falls Sie mit dem grundlegenden Konzept der Kopieraktivität nicht vertraut sind, finden Sie weitere Informationen unter [Kopieraktivität – Übersicht](copy-activity-overview.md), bevor Sie sich mit diesem Artikel beschäftigen.
>

## <a name="performance-reference"></a>Leistungsreferenz

Als Referenz ist in der folgenden Tabelle der Durchsatzwert beim Kopieren in MBit/s für die angegebenen Paare aus Quelle und Senke in einer einzelnen Kopieraktivitätsausführung basierend auf internen Tests aufgeführt. Zu Vergleichszwecken wird zudem veranschaulicht, wie unterschiedliche Einstellungen der [Datenintegrationseinheiten](#data-integration-units) oder der [Skalierbarkeit der selbstgehosteten Integration Runtime](concepts-integration-runtime.md#self-hosted-integration-runtime) (mehrere Knoten) zu einer verbesserten Kopierleistung beitragen können.

![Leistungsmatrix](./media/copy-activity-performance/CopyPerfRef.png)

> [!IMPORTANT]
> Wenn die Kopieraktivität in einer Azure-Integration Runtime ausgeführt wird, sind mindestens zwei Datenintegrationseinheiten (früher als Einheiten der Clouddatenverschiebungen bezeichnet) erforderlich. Wenn keine Angabe erfolgt, finden Sie unter [Datenintegrationseinheiten](#data-integration-units) Informationen zu den Standard-Datenintegrationseinheiten.

**Beachten Sie Folgendes:**

* Der Durchsatz wird mithilfe der folgenden Formel berechnet: [Größe der aus der Quelle gelesenen Daten]/[Ausführungsdauer der Kopieraktivität].
* Die Leistungsreferenzwerte in der Tabelle wurden mit einem [TPC-H](http://www.tpc.org/tpch/)-Dataset in einer einzelnen Kopieraktivitätsausführung gemessen. Testdateien für dateibasierte Speicher sind mehrere Dateien mit einer Größe von 10 GB.
* In Azure-Datenspeichern befinden sich Quelle und Senke in der gleichen Azure-Region.
* Bei hybriden Kopieraktivitäten zwischen lokalen und Clouddatenspeichern wurde jeder selbstgehostete Integration Runtime-Knoten auf einem Computer ausgeführt, bei dem es sich nicht um den Datenspeicher mit der folgenden Spezifikation gehandelt hat. Bei Ausführung einer einzelnen Aktivität wurde vom Kopiervorgang nur ein kleiner Teil der CPU, des Arbeitsspeichers und der Netzwerkbandbreite des Testcomputers in Anspruch genommen.
    <table>
    <tr>
        <td>CPU</td>
        <td>Intel Xeon E5-2660 v2 (32 Kerne, 2,20 GHz)</td>
    </tr>
    <tr>
        <td>Arbeitsspeicher</td>
        <td>128 GB</td>
    </tr>
    <tr>
        <td>Netzwerk</td>
        <td>Internetschnittstelle: 10 Gbit/s; Intranetschnittstelle: 40 GBit/s</td>
    </tr>
    </table>


> [!TIP]
> Mit weiteren DIUs können Sie einen höheren Durchsatz erzielen. Beispielsweise können Sie mit 100 DIUs Daten mit einer Rate von 1,0 GBit/s aus Azure Blob Storage nach Azure Data Lake Store kopieren. Weitere Informationen zu diesem Feature und dem unterstützten Szenario finden Sie im Abschnitt [Datenintegrationseinheiten](#data-integration-units). 

## <a name="data-integration-units"></a>Datenintegrationseinheiten

Eine Datenintegrationseinheit ist eine Messgröße für die Leistungsfähigkeit (Kombination aus zugeteilten CPU-, Speicher- und Netzwerkressourcen) einer einzelnen Einheit in Azure Data Factory. Die Datenintegrationseinheit gilt nur für die [Azure-Integration Runtime](concepts-integration-runtime.md#azure-integration-runtime), aber nicht für [selbstgehostete Integration Runtimes](concepts-integration-runtime.md#self-hosted-integration-runtime).

Zum Ausführen einer Kopieraktivität sind mindestens zwei DIUs erforderlich. Wenn keine Angabe erfolgt, finden Sie in der folgenden Tabelle die Standard-DIUs, die in verschiedenen Kopierszenarien verwendet werden:

| Kopierszenario | Vom Dienst bestimmte Standard-DIUs |
|:--- |:--- |
| Kopieren von Daten zwischen dateibasierten Speichern | Zwischen 4 und 32, je nach Anzahl und Größe der Dateien |
| Kopieren von Daten nach Azure SQL-Datenbank oder Azure Cosmos DB |Zwischen 4 und 16, abhängig von der Dienstebene der Azure SQL-Senkendatenbank oder Cosmos DB (Anzahl von DTUs/RUs) |
| Alle anderen Kopierszenarien | 4 |

Sie können diese Standardeinstellung überschreiben, indem Sie wie folgt einen Wert für die **dataIntegrationUnits**-Eigenschaft angeben. Als *zulässige Werte* für die **dataIntegrationUnits**-Eigenschaft können Werte bis 256 angegeben werden. Die *tatsächliche Anzahl von DIUs*, die der Kopiervorgang zur Laufzeit verwendet, entspricht maximal dem konfigurierten Wert. Dies ist abhängig von Ihrem Datenmuster. Informationen zum Umfang des möglichen Leistungsgewinns durch Konfigurieren weiterer Einheiten für eine bestimmte Kopierquelle und -senke finden Sie in der [Leistungsreferenz](#performance-reference).

Sie können die für die einzelnen Kopierausführungen verwendeten DIUs in der Ausgabe der Kopieraktivität sehen, wenn Sie eine Aktivitätsausführung überwachen. Weitere Informationen finden Sie unter [Überwachung der Kopieraktivität](copy-activity-overview.md#monitoring).

> [!NOTE]
> Ein höherer Wert als vier für DIUs gilt derzeit nur, wenn Sie mehrere Dateien aus Azure Storage, Azure Data Lake Storage, Amazon S3, Google Cloud Storage, Cloud-FTP oder Cloud-SFTP in einen beliebigen anderen Clouddatenspeicher kopieren.
>

**Beispiel**

```json
"activities":[
    {
        "name": "Sample copy activity",
        "type": "Copy",
        "inputs": [...],
        "outputs": [...],
        "typeProperties": {
            "source": {
                "type": "BlobSource",
            },
            "sink": {
                "type": "AzureDataLakeStoreSink"
            },
            "dataIntegrationUnits": 32
        }
    }
]
```

### <a name="data-integration-units-billing-impact"></a>Auswirkungen von Datenintegrationseinheiten auf die Abrechnung

Denken Sie daran, dass die Berechnung der Gebühren auf der Gesamtdauer des Kopiervorgangs basiert. Die Gesamtdauer, die Ihnen für die Datenverschiebungen in Rechnung gestellt wird, ist die Summe der DIU-übergreifenden Dauer. Wenn ein Kopierauftrag mit zwei Cloudeinheiten bislang eine Stunde gedauert hat und nun mit acht Cloudeinheiten 15 Minuten dauert, fällt die Gesamtrechnung in etwa gleich hoch aus.

## <a name="parallel-copy"></a>Parallele Kopie

Mithilfe der **parallelCopies** -Eigenschaft können Sie die gewünschte Parallelität für die Kopieraktivität angeben. Diese Eigenschaft können Sie sich als die maximale Anzahl von Threads in einer Kopieraktivität vorstellen, die parallel aus Quelldatenspeichern lesen oder in Senkendatenspeicher schreiben können.

Für jede Kopieraktivitätsausführung ermittelt Azure Data Factory die Anzahl paralleler Kopien, die zum Kopieren von Daten aus dem Quelldatenspeicher sowie zum Kopieren von Daten in den Zieldatenspeicher verwendet werden. Die verwendete Standardanzahl paralleler Kopien hängt von der Art der verwendeten Quelle und Senke ab.

| Kopierszenario | Standardanzahl der vom Dienst ermittelten parallelen Kopien |
| --- | --- |
| Kopieren von Daten zwischen dateibasierten Speichern |Dies ist abhängig von der Größe der Dateien und der Anzahl DIUs, die zum Kopieren von Daten zwischen zwei Clouddatenspeichern verwendet werden, oder von der physischen Konfiguration des Computers der selbstgehosteten Integration Runtime-Infrastruktur. |
| Kopieren von Daten aus einem beliebigen Quellspeicher im Azure-Tabellenspeicher |4 |
| Alle anderen Kopierszenarios |1 |

> [!TIP]
> Beim Kopieren von Daten zwischen dateibasierten Speichern bietet das Standardverhalten in der Regel den bestmöglichen Durchsatz. Das Standardverhalten wird automatisch basierend auf dem Muster Ihrer Quelldatei festgelegt.

Sie können den Standardwert jedoch überschreiben und einen Wert für die Eigenschaft **parallelCopies** angeben, um die Last für die Computer zu steuern, auf denen Ihre Datenspeicher gehostet werden, oder um die Kopierleistung zu optimieren. Der Wert muss eine ganze Zahl größer als oder gleich 1 sein. Zur Laufzeit verwendet die Kopieraktivität maximal den von Ihnen festgelegten Wert, um eine optimale Leistung zu erzielen.

```json
"activities":[
    {
        "name": "Sample copy activity",
        "type": "Copy",
        "inputs": [...],
        "outputs": [...],
        "typeProperties": {
            "source": {
                "type": "BlobSource",
            },
            "sink": {
                "type": "AzureDataLakeStoreSink"
            },
            "parallelCopies": 32
        }
    }
]
```

**Beachten Sie Folgendes:**

* Beim Kopieren von Daten zwischen dateibasierten Speichern bestimmt **parallelCopies** die Parallelität auf Dateiebene. Die Segmentierung innerhalb einer einzelnen Datei erfolgt darunter automatisch und transparent. Dadurch wird die am besten geeignete Segmentgröße für einen angegebenen Quelldatenspeichertyp verwendet, um Daten parallel und orthogonal in **parallelCopies** zu laden. Die tatsächliche Anzahl paralleler Kopien, die der Datenverschiebungsdienst zur Laufzeit für den Kopiervorgang verwendet, entspricht maximal der Anzahl vorhandener Dateien. Bei Verwendung des Kopierverhaltens **mergeFile** kann die Kopieraktivität keine Parallelität auf Dateiebene nutzen.
* Beim Kopieren von Daten aus nicht dateibasierten Speichern (außer den Connectors für [Oracle](connector-oracle.md#oracle-as-source), [Teradata](connector-teradata.md#teradata-as-source), [SAP-Tabelle](connector-sap-table.md#sap-table-as-source) und [SAP Open Hub](connector-sap-business-warehouse-open-hub.md#sap-bw-open-hub-as-source) als Quelle mit aktivierter Datenpartitionierung) in dateibasierte Speicher ignoriert der Datenverschiebungsdienst die **parallelCopies**-Eigenschaft. Die Parallelität wird in diesem Fall also nicht angewendet, auch wenn sie angegeben wurde.
* Die **parallelCopies**-Eigenschaft ist orthogonal zu **dataIntegrationUnits**. Die erste Eigenschaft wird für alle Datenintegrationseinheiten gezählt.
* Berücksichtigen Sie beim Angeben eines Werts für die **parallelCopies**-Eigenschaft die höhere Auslastung Ihrer Quell- und Senkendatenspeicher. Berücksichtigen Sie auch den Lastanstieg zur selbstgehosteten Integration Runtime, wenn die Kopieraktivität davon abhängt, z. B. bei Hybridkopiervorgängen. Dieser Lastanstieg tritt insbesondere auf, wenn Sie über mehrere Aktivitäten oder gleichzeitige Ausführungen der gleichen Aktivitäten verfügen, die für den gleichen Datenspeicher ausgeführt werden. Sollten Sie eine Überlastung des Datenspeichers oder der selbstgehosteten Integration Runtime-Infrastruktur feststellen, verringern Sie den Wert **parallelCopies**, um die Last zu reduzieren.

## <a name="staged-copy"></a>gestaffeltem Kopieren

Beim Kopieren von Daten aus einem Quelldatenspeicher in einen Senkendatenspeicher können Sie ggf. Blob Storage als Stagingzwischenspeicher verwenden. Staging ist besonders in folgenden Fällen hilfreich:

- **Sie möchten Daten aus verschiedenen Datenspeichern über PolyBase in SQL Data Warehouse erfassen.** SQL Data Warehouse nutzt PolyBase als Mechanismus mit hohem Durchsatz, um große Datenmengen in SQL Data Warehouse zu laden. Die Quelldaten müssen sich in Blob Storage oder in Azure Data Lake Store befinden und einige andere Kriterien erfüllen. Wenn Sie Daten aus einem Blob Storage- oder Azure Data Lake Store-fremden Datenspeicher laden, können Sie das Kopieren von Daten über einen Staging-Blob-Zwischenspeicher aktivieren. In diesem Fall führt Azure Data Factory die erforderlichen Datentransformationen durch, um die Anforderungen von PolyBase zu erfüllen. Anschließend werden die Daten mithilfe von PolyBase auf effiziente Art und Weise in SQL Data Warehouse geladen. Weitere Informationen finden Sie unter [Use PolyBase to load data into Azure SQL Data Warehouse (Verwenden von PolyBase zum Laden von Daten in Azure SQL Data Warehouse)](connector-azure-sql-data-warehouse.md#use-polybase-to-load-data-into-azure-sql-data-warehouse).
- **Hybriddatenverschiebungen (also das Kopieren aus einem lokalen Datenspeicher in einen Clouddatenspeicher) können bei einer langsamen Netzwerkverbindung eine Weile dauern.** Zur Verbesserung der Leistung können Sie gestaffeltes Kopieren verwenden, um die Daten lokal zu komprimieren und dadurch die Verschiebung in den Stagingdatenspeicher in der Cloud zu beschleunigen. Anschließend können Sie die Daten im Stagingspeicher wieder dekomprimieren, bevor Sie sie in den Zieldatenspeicher laden.
- **Aufgrund der IT-Richtlinien des Unternehmens sollten Sie mit Ausnahme der Ports 80 und 443 keine weiteren Ports in der Firewall öffnen.** Ein Beispiel: Beim Kopieren von Daten aus einem lokalen Datenspeicher an eine Azure SQL-Datenbank- oder Azure SQL Data Warehouse-Senke muss die ausgehende TCP-Kommunikation über den Port 1433 sowohl für die Windows-Firewall als auch für die Unternehmensfirewall ermöglicht werden. In diesem Szenario kann das gestaffelte Kopieren die selbstgehostete Integration Runtime nutzen, um zunächst Daten per HTTP oder HTTPS über den Port 443 an eine Blob Storage-Staginginstanz kopieren. Anschließend können die Daten aus dem Blob Storage-Stagingspeicher in SQL-Datenbank oder in SQL Data Warehouse geladen werden. Dadurch muss der Port 1433 nicht geöffnet werden.

### <a name="how-staged-copy-works"></a>Funktionsweise des gestaffelten Kopierens

Bei aktivierter Stagingfunktion werden die Daten zunächst aus dem Quelldatenspeicher in den (von Ihnen bereitgestellten) Blob-Stagingspeicher kopiert. Anschließend werden die Daten dann aus dem Stagingdatenspeicher in den Senkendatenspeicher kopiert. Dieser zweistufige Prozess wird automatisch von Azure Data Factory verwaltet. Nach Abschluss der Datenverschiebung bereinigt Azure Data Factory außerdem temporäre Daten im Stagingspeicher.

![gestaffeltem Kopieren](media/copy-activity-performance/staged-copy.png)

Wenn Sie die Datenverschiebung unter Verwendung des Stagingspeichers aktivieren, können Sie angeben, ob die Daten vor dem Verschieben aus dem Quelldatenspeicher in einen Zwischen- oder Stagingdatenspeicher komprimiert und vor dem Verschieben der Daten aus einem Zwischen- oder Stagingdatenspeicher in den Senkendatenspeicher wieder dekomprimiert werden sollen.

Derzeit können Sie keine Daten zwischen zwei Datenspeichern kopieren, die über verschiedene selbstgehostete IRs verbunden sind, weder mit noch ohne gestaffeltes Kopieren. Für ein solches Szenario können Sie zwei explizit verkettete Kopiervorgänge konfigurieren, um von der Quelle zum Stagingspeicher und aus dem Stagingspeicher zur Senke zu kopieren.

### <a name="configuration"></a>Konfiguration

Konfigurieren Sie für die Kopieraktivität die Einstellung **enableStaging**, um anzugeben, ob die Daten vor dem Laden in einen Zielspeicher in Blobspeicher bereitgestellt werden sollen. Wenn Sie **enableStaging** auf `TRUE` festlegen, müssen Sie zusätzliche Eigenschaften angeben. Diese sind in der folgenden Tabelle aufgeführt. Außerdem müssen Sie für das Staging einen verknüpften Azure Storage- oder Storage-SAS-Dienst erstellen, falls noch nicht vorhanden.

| Eigenschaft | Beschreibung | Standardwert | Erforderlich |
| --- | --- | --- | --- |
| enableStaging |Geben Sie an, ob Sie Daten über einen Stagingzwischenspeicher kopieren möchten. |False |Nein |
| linkedServiceName |Geben Sie den Namen eines verknüpften Diensts vom Typ [AzureStorage](connector-azure-blob-storage.md#linked-service-properties) an, der auf die Storage-Instanz verweist, die Sie als Stagingzwischenspeicher verwenden. <br/><br/> Storage kann nicht mit einer Shared Access Signature (SAS) verwendet werden, um Daten über PolyBase in SQL Data Warehouse zu laden. In allen anderen Szenarien ist dies hingegen problemlos möglich. |– |Ja, wenn **enableStaging** auf „TRUE“ festgelegt ist. |
| path |Geben Sie den gewünschten Blob Storage-Pfad für die bereitgestellten Daten an. Wenn Sie keinen Pfad angeben, erstellt der Dienst einen Container zum Speichern der temporären Daten. <br/><br/> Geben Sie nur dann einen Pfad an, wenn Sie Storage mit einer Shared Access Signature verwenden oder sich die temporären Daten an einem bestimmten Speicherort befinden müssen. |– |Nein |
| enableCompression |Gibt an, ob die Daten komprimiert werden sollen, bevor sie an das Ziel kopiert werden. Durch diese Einstellung wird die Menge der übertragenen Daten reduziert. |False |Nein |

>[!NOTE]
> Wenn Sie das gestaffelte Kopieren mit aktivierter Komprimierung verwenden, wird die Dienstprinzipal- oder MSI-Authentifizierung für das Staging eines verknüpften Blobdiensts nicht unterstützt.

Hier sehen Sie eine Beispieldefinition der Kopieraktivität mit den Eigenschaften aus der obigen Tabelle:

```json
"activities":[
    {
        "name": "Sample copy activity",
        "type": "Copy",
        "inputs": [...],
        "outputs": [...],
        "typeProperties": {
            "source": {
                "type": "SqlSource",
            },
            "sink": {
                "type": "SqlSink"
            },
            "enableStaging": true,
            "stagingSettings": {
                "linkedServiceName": {
                    "referenceName": "MyStagingBlob",
                    "type": "LinkedServiceReference"
                },
                "path": "stagingcontainer/path",
                "enableCompression": true
            }
        }
    }
]
```

### <a name="staged-copy-billing-impact"></a>Auswirkungen auf die Abrechnung von gestaffeltem Kopieren

Die Abrechnung basiert auf zwei Aspekten: Dauer des Kopiervorgangs und Art der Kopie.

* Wenn Sie einen Stagingspeicher während eines Cloudkopiervorgangs (d. h. Kopieren von Daten aus einem Clouddatenspeicher in einen anderen Clouddatenspeicher. Beide Phasen werden von Azure-Integration Runtime unterstützt) verwenden, gilt folgende Formel: [Gesamtkopierdauer für Schritt 1 und 2] [Einzelpreis für Cloudkopien].
* Wenn Sie Staging während eines Hybridkopiervorgangs (d. h. Kopieren von Daten aus einem lokalen Datenspeicher in einen Clouddatenspeicher. Eine Phase wird von der selbstgehosteten Integration Runtime-Infrastruktur unterstützt) verwenden, gilt folgende Formel: [Dauer des Hybridkopiervorgangs] [Einzelpreis für Hybridkopien] + [Dauer des Cloudkopiervorgangs] [Einzelpreis für Cloudkopien].

## <a name="performance-tuning-steps"></a>Schritte zur Optimierung der Leistung

Führen Sie diese Schritte aus, um die Leistung des Azure Data Factory-Diensts mit der Kopieraktivität zu verbessern.

1. **Einrichten einer Baseline**. Testen Sie Ihre Pipeline mit der Kopieraktivität in der Entwicklungsphase mit repräsentativen Beispieldaten. Sammeln Sie Ausführungsdetails und Leistungsmerkmale, und folgen Sie dabei der [Überwachung der Kopieraktivität](copy-activity-overview.md#monitoring).

2. **Analysieren und Optimieren der Leistung.** Sollte die Leistung in der Praxis nicht Ihren Erwartungen entsprechen, identifizieren Sie Leistungsengpässe. Anschließend können Sie die Leistung optimieren, um Engpässe zu beseitigen oder deren Auswirkungen zu minimieren.

    In einigen Fällen wird bei der Ausführung einer Kopieraktivität in Azure Data Factory oben auf der [Seite zur Überwachung der Kopieraktivität](copy-activity-overview.md#monitor-visually) die Meldung „Tipps zur Leistungsoptimierung“ angezeigt, wie im folgenden Beispiel gezeigt. Die Meldung informiert Sie über den Engpass, der für die jeweilige Kopierausführung identifiziert wurde. Zudem enthält sie Ratschläge, was Sie ändern können, um die Kopierdurchsatz zu steigern. Die Tipps zur Leistungsoptimierung bieten derzeit Vorschläge wie:

    - Verwenden Sie PolyBase, wenn Sie Daten in Azure SQL Data Warehouse kopieren.
    - Erhöhen Sie die Azure Cosmos DB-Anforderungseinheiten oder Azure SQL-Datenbank-DTUs (Datenbankdurchsatzeinheiten), wenn die Ressource auf der Datenspeicherseite der Engpass ist.
    - Entfernen Sie das unnötige gestaffelte Kopieren.

    Die Regeln für die Leistungsoptimierung werden auch schrittweise ergänzt werden.

    **Beispiel: Kopieren in Azure SQL-Datenbank mit Tipps für die Leistungsoptimierung**

    In diesem Beispiel stellt Azure Data Factory während eines Kopiervorgangs fest, dass die Azure SQL-Senkendatenbank eine hohe DTU-Auslastung erreicht, was die Schreibvorgänge verlangsamt. Der Vorschlag ist, die Azure SQL-Datenbankebene mit mehr DTUs zu erweitern. 

    ![Überwachung des Kopiervorgangs mit Tipps für die Leistungsoptimierung](./media/copy-activity-overview/copy-monitoring-with-performance-tuning-tips.png)

    Im Folgenden finden Sie einige allgemeine Aspekte. Eine vollständige Beschreibung der Leistungsdiagnose würde den Rahmen dieses Artikels sprengen.

   * Leistungsfeatures:
     * [Parallele Kopie](#parallel-copy)
     * [Datenintegrationseinheiten](#data-integration-units)
     * [Gestaffeltes Kopieren](#staged-copy)
     * [Skalierbarkeit der selbstgehosteten Integration Runtime-Infrastruktur](concepts-integration-runtime.md#self-hosted-integration-runtime)
   * [Selbstgehostete Integration Runtime](#considerations-for-self-hosted-integration-runtime).
   * [Quelle](#considerations-for-the-source)
   * [Senke](#considerations-for-the-sink)
   * [Serialisierung und Deserialisierung](#considerations-for-serialization-and-deserialization)
   * [Komprimierung](#considerations-for-compression)
   * [Spaltenzuordnung](#considerations-for-column-mapping)
   * [Weitere Überlegungen](#other-considerations)

3. **Erweitern der Konfiguration auf das gesamte Dataset.** Wenn Sie mit den Ausführungsergebnissen und mit der Leistung zufrieden sind, können Sie die Definition und die Pipeline auf Ihr gesamtes Dataset erweitern.

## <a name="considerations-for-self-hosted-integration-runtime"></a>Überlegungen zur selbstgehosteten Integration Runtime-Infrastruktur

Beachten Sie Folgendes, wenn Ihre Kopieraktivität in einer selbstgehosteten Integration Runtime-Infrastruktur ausgeführt wird:

**Setup**: Es wird empfohlen, Integration Runtime auf einem dedizierten Computer zu hosten. Weitere Informationen finden Sie unter [Integrationslaufzeit in Azure Data Factory](concepts-integration-runtime.md).

**Horizontales Skalieren:** In einer einzelnen, logischen, selbstgehosteten Integration Runtime-Infrastruktur mit einem oder mehreren Knoten können mehrere Kopieraktivitäten gleichzeitig parallel ausgeführt werden. Wenn Sie einen hohen Bedarf an Hybriddatenverschiebungen mit einer großen Anzahl an gleichzeitigen Ausführungen der Kopieraktivität oder mit einem umfangreichen Volumen an zu kopierenden Daten haben, sollten Sie [das horizontale Skalieren der selbstgehosteten Integration Runtime-Infrastruktur](create-self-hosted-integration-runtime.md#high-availability-and-scalability) erwägen, um für die Unterstützung des Kopierens weitere Ressourcen bereitzustellen.

## <a name="considerations-for-the-source"></a>Hinweise zur Quelle

### <a name="general"></a>Allgemein

Stellen Sie sicher, dass der zugrundeliegende Datenspeicher nicht durch andere darauf oder dafür ausgeführte Workloads überlastet wird.

Informationen zu Microsoft-Datenspeichern finden Sie in datenspeicherspezifischen [Themen zur Überwachung und Optimierung](#performance-reference). Hier können Sie sich über Leistungsmerkmale, die Minimierung von Antwortzeiten und die Maximierung des Durchsatzes informieren.

* Wenn Sie Daten aus einem Blob-Speicher in SQL Data Warehouse kopieren, können Sie die Leistung ggf. mithilfe von PolyBase steigern. Weitere Informationen finden Sie unter [Use PolyBase to load data into Azure SQL Data Warehouse (Verwenden von PolyBase zum Laden von Daten in Azure SQL Data Warehouse)](connector-azure-sql-data-warehouse.md#use-polybase-to-load-data-into-azure-sql-data-warehouse).
* Wenn Sie Daten aus HDFS in Azure Blob Storage oder Azure Data Lake Store kopieren, können Sie die Leistung ggf. mithilfe von DistCp steigern. Weitere Informationen finden Sie unter [Kopieren von Daten aus einem HDFS mithilfe von DistCp](connector-hdfs.md#use-distcp-to-copy-data-from-hdfs).
* Wenn Sie Daten aus Redshift in Azure SQL Data Warehouse, Azure Blob Storage oder Azure Data Lake Store kopieren, können Sie die Leistung ggf. mithilfe von UNLOAD steigern. Weitere Informationen finden Sie im Abschnitt [Verwenden von UNLOAD zum Kopieren von Daten aus Amazon Redshift](connector-amazon-redshift.md#use-unload-to-copy-data-from-amazon-redshift).

### <a name="file-based-data-stores"></a>Dateibasierte Datenspeicher

* **Durchschnittliche Größe und Anzahl von Dateien:** Die Kopieraktivität überträgt Daten als einzelne Dateien. Aufgrund der Bootstrap-Phase für die einzelnen Dateien ist der Gesamtdurchsatz bei gleicher zu verschiebender Datenmenge geringer, wenn die Daten nicht aus einer geringen Anzahl großer Dateien, sondern aus zahlreichen kleinen Dateien bestehen. Kombinieren Sie kleine Dateien möglichst zu größeren Dateien, um einen höheren Durchsatz zu erzielen.
* **Dateiformat und Komprimierung**: Weitere Methoden zur Verbesserung der Leistung finden Sie in den Abschnitten [Hinweise zur Serialisierung/Deserialisierung](#considerations-for-serialization-and-deserialization) und [Hinweise zur Komprimierung](#considerations-for-compression).

### <a name="relational-data-stores"></a>Relationale Datenspeicher

* **Datenmuster**: Ihr Tabellenschema hat Auswirkungen auf den Durchsatz beim Kopieren. Eine hohe Zeilengröße liefert zum Kopieren derselben Datenmenge eine bessere Leistung als eine niedrige. Der Grund ist, dass die Datenbank weniger Batches von Daten mit weniger Zeilen effizienter abrufen kann.
* **Abfrage oder gespeicherte Prozedur**: Optimieren Sie die Logik der Abfrage oder gespeicherten Prozedur, die Sie in der Quelle der Kopieraktivität angeben, um Daten effizienter abzurufen.

## <a name="considerations-for-the-sink"></a>Hinweise zur Senke

### <a name="general"></a>Allgemein

Stellen Sie sicher, dass der zugrundeliegende Datenspeicher nicht durch andere darauf oder dafür ausgeführte Workloads überlastet wird.

Informationen zu Microsoft-Datenspeichern finden Sie in datenspeicherspezifischen [Themen zur Überwachung und Optimierung](#performance-reference). Hier können Sie sich über Leistungsmerkmale, die Minimierung von Antwortzeiten und die Maximierung des Durchsatzes informieren.

* Wenn Sie Daten aus einem Datenspeicher in Azure SQL Data Warehouse kopieren, können Sie die Leistung ggf. mithilfe von PolyBase steigern. Weitere Informationen finden Sie unter [Use PolyBase to load data into Azure SQL Data Warehouse (Verwenden von PolyBase zum Laden von Daten in Azure SQL Data Warehouse)](connector-azure-sql-data-warehouse.md#use-polybase-to-load-data-into-azure-sql-data-warehouse).
* Wenn Sie Daten aus HDFS in Azure Blob Storage oder Azure Data Lake Store kopieren, können Sie die Leistung ggf. mithilfe von DistCp steigern. Weitere Informationen finden Sie unter [Kopieren von Daten aus einem HDFS mithilfe von DistCp](connector-hdfs.md#use-distcp-to-copy-data-from-hdfs).
* Wenn Sie Daten aus Redshift in Azure SQL Data Warehouse, Azure Blob Storage oder Azure Data Lake Store kopieren, können Sie die Leistung ggf. mithilfe von UNLOAD steigern. Weitere Informationen finden Sie im Abschnitt [Verwenden von UNLOAD zum Kopieren von Daten aus Amazon Redshift](connector-amazon-redshift.md#use-unload-to-copy-data-from-amazon-redshift).

### <a name="file-based-data-stores"></a>Dateibasierte Datenspeicher

* **Kopierverhalten:** Wenn Sie Daten aus einem anderen dateibasierten Datenspeicher kopieren, stehen über die **copyBehavior**-Eigenschaft drei Optionen für die Kopieraktivität zur Verfügung. Das Beibehalten oder Abflachen der Hierarchie verursacht nur einen geringen oder gar keinen Zusatzaufwand. Das Zusammenführen von Dateien ist hingegen mit einem gesteigerten Zusatzaufwand verbunden.
* **Dateiformat und Komprimierung**: Weitere Methoden zur Verbesserung der Leistung finden Sie in den Abschnitten [Hinweise zur Serialisierung/Deserialisierung](#considerations-for-serialization-and-deserialization) und [Hinweise zur Komprimierung](#considerations-for-compression).

### <a name="relational-data-stores"></a>Relationale Datenspeicher

* **Kopierverhalten und Auswirkung auf die Leistung**: Es gibt verschiedene Möglichkeiten zum Schreiben von Daten in eine SQL-Senke. Weitere Informationen finden Sie unter [Kopieren von Daten nach und aus Azure SQL-Datenbank](connector-azure-sql-database.md#best-practice-for-loading-data-into-azure-sql-database).

* **Datenmuster und Batchgröße**:
  * Ihr Tabellenschema hat Auswirkungen auf den Durchsatz beim Kopieren. Beim Kopieren der gleichen Datenmenge erzielen Sie mit großen Zeilen eine bessere Leistung als mit kleinen Zeilen, da die Datenbank eine geringere Anzahl von Datenbatches effizienter committen kann.
  * Die Kopieraktivität fügt Daten in einer Folge von Batches ein. Die Anzahl von Zeilen in einem Batch kann dabei mithilfe der **writeBatchSize** -Eigenschaft festgelegt werden. Wenn Ihre Daten kleine Zeilen enthalten, können Sie für die **writeBatchSize** -Eigenschaft einen höheren Wert festlegen, um den Batchaufwand zu verringern und den Durchsatz zu erhöhen. Bei umfangreichen Zeilen müssen Sie vorsichtig sein, wenn Sie den Wert von **writeBatchSize**erhöhen. Ein hoher Wert kann zu einer Datenbanküberlastung und dadurch zu Kopierfehlern führen.

### <a name="nosql-stores"></a>NoSQL-Speicher

* **Table Storage:**
  * **Partition**: Das Schreiben von Daten in überlappende Partitionen beeinträchtigt die Leistung erheblich. Sortieren Sie Ihre Daten anhand des Partitionsschlüssels, sodass sie effizient partitionsweise eingefügt werden. Sie können die Logik auch so anpassen, dass die Daten in eine einzelne Partition geschrieben werden.

## <a name="considerations-for-serialization-and-deserialization"></a>Hinweise zur Serialisierung und Deserialisierung

Serialisierung und Deserialisierung können auftreten, wenn Ihr Eingabe- oder Ausgabedataset eine Datei ist. Weitere Informationen zu den unterstützten Dateiformaten nach Kopieraktivität finden Sie unter [Unterstützte Datei- und Komprimierungsformate](supported-file-formats-and-compression-codecs.md).

**Kopierverhalten:**

* Beim Kopieren von Dateien zwischen dateibasierten Datenspeichern gilt Folgendes:
  * Wenn sowohl das Eingabe- als auch das Ausgabedataset die gleichen oder keine Dateiformateinstellungen besitzen, führt der Datenverschiebungsdienst einen *binären Kopiervorgang* ohne Serialisierung oder Deserialisierung durch. Dadurch wird ein höherer Durchsatz erreicht als in einem Szenario mit unterschiedlichen Dateiformateinstellungen für Quelle und Senke.
  * Wenn Eingabe- und Ausgabedataset im Textformat vorliegen und nur unterschiedlich codiert sind, konvertiert der Datenverschiebungsdienst nur die Codierung. Eine im Gegensatz zum binären Kopieren aufwändigere Serialisierung und Deserialisierung findet nicht statt.
  * Wenn Eingabe- und Ausgabedataset unterschiedliche Dateiformate oder Konfigurationen (etwa Trennzeichen) besitzen, deserialisiert und transformiert der Datenverschiebungsdienst die zu streamenden Quelldaten und serialisiert sie anschließend in das von Ihnen angegebene Ausgabeformat. Dies ist im Vergleich zu anderen Szenarien mit einem wesentlich höheren Aufwand verbunden.
* Wenn Sie Dateien in einen oder aus einem nicht dateibasierten Datenspeicher kopieren (etwa aus einem dateibasierten Speicher in einen relationalen Speicher), ist der Serialisierungs- oder Deserialisierungsschritt zwingend erforderlich. Dieser Schritt bedeutet einen erheblichen Mehraufwand.

**Dateiformat:** Das von Ihnen gewählte Dateiformat beeinträchtigt unter Umständen die Leistung beim Kopieren. Avro ist beispielsweise ein kompaktes binäres Format, das nicht nur Daten, sondern auch Metadaten speichert. Es wird umfassend im Hadoop-System für Verarbeitungs- und Abfragevorgänge unterstützt. Avro verursacht einen höheren Aufwand bei der Serialisierung und Deserialisierung, was im Vergleich zum Textformat zu einem niedrigeren Durchsatz führt. 

Berücksichtigen Sie bei der Entscheidung für ein Dateiformat den gesamten Verarbeitungsablauf. Beginnen Sie mit:

- In welchem Formular sind die Daten gespeichert: Quelldatenspeichern, oder sollen sie aus externen Systemen extrahiert werden?
- Das beste Format für die Speicherung, analytische Verarbeitung und Abfragen.
- In welchem Format sollen die Daten in Data Marts für Bericht- und Visualisierungstools exportiert werden?

Manchmal stellt sich heraus, dass ein angesichts der Lese- und Schreibleistung eigentlich suboptimales Dateiformat bei Berücksichtigung des gesamten analytischen Prozesses trotzdem gut geeignet ist.

## <a name="considerations-for-compression"></a>Hinweise zur Komprimierung

Wenn Ihr Eingabe- oder Ausgabedataset eine Datei ist, können Sie die Kopieraktivität so konfigurieren, dass beim Schreiben von Daten in das Ziel eine Komprimierung oder Dekomprimierung ausgeführt wird. Wenn Sie sich für eine Komprimierung entscheiden, gehen Sie einen Kompromiss zwischen Eingabe/Ausgabe (E/A) und CPU ein. Das Komprimieren der Daten verursacht zusätzliche Kosten für die Computeressourcen. Im Gegenzug verringern sich aber die Kosten für Netzwerk-E/A und Speicher. Abhängig von Ihren Daten ergibt sich dadurch möglicherweise eine Verbesserung des Gesamtdurchsatzes.

**Codec**: Jeder Komprimierungscodec hat seine Vorteile. BZIP2 bietet etwa den geringsten Durchsatz, im Gegenzug aber die beste Hive-Abfrageleistung, da sich dieser Typ für die Verarbeitung aufteilen lässt. GZIP ist die ausgewogenste Option und wird am häufigsten verwendet. Entscheiden Sie sich für den Codec, der am besten für Ihr End-to-End-Szenario geeignet ist.

**Ebene**: Für jeden Komprimierungscodec stehen zwei Optionen zur Verfügung: schnellste Komprimierung und optimale Komprimierung. Bei der schnellsten Komprimierung werden die Daten schnellstmöglich komprimiert, auch wenn die resultierende Datei nicht optimal komprimiert ist. Die optimale Komprimierung dauert länger, liefert aber die kleinstmögliche Datenmenge. Sie können beide Optionen testen, um die in Ihrem Fall beste Gesamtleistung zu ermitteln.

**Hinweis**: Verwenden Sie beim Kopieren umfangreicher Daten zwischen einem lokalen Datenspeicher und der Cloud ggf. das [gestaffelte Kopieren](#staged-copy) mit aktivierter Komprimierung. Die Verwendung von Zwischenspeichern ist hilfreich, wenn die Bandbreite Ihres Unternehmensnetzwerks und Ihrer Azure-Dienste die Einschränkung darstellt und Sie möchten, dass sowohl der Eingangsdatensatz als auch der Ausgangsdatensatz in unkomprimierter Form vorliegen.

## <a name="considerations-for-column-mapping"></a>Hinweise zur Spaltenzuordnung

Mit der **columnMappings**-Eigenschaft der Kopieraktivität können Eingabespalten ganz oder teilweise den Ausgabespalten zugeordnet werden. Nach dem Lesen der Daten aus der Quelle muss der Datenverschiebungsdienst eine Spaltenzuordnung für die Daten vornehmen, bevor sie in die Senke geschrieben werden. Dieser zusätzliche Verarbeitungsaufwand reduziert den Kopierdurchsatz.

Falls es sich bei Ihrem Quelldatenspeicher um einen abfragbaren Speicher (beispielsweise um einen relationalen Speicher wie die SQL-Datenbank oder SQL Server) oder um einen NoSQL-Speicher (etwa um Table Storage oder Azure Cosmos DB) handelt, empfiehlt es sich unter Umständen, die Logik für Filterung und Neuanordnung von Spalten in die **query**-Eigenschaft auszulagern, anstatt die Spaltenzuordnung zu verwenden. Dadurch erfolgt die Projektion, während der Datenverschiebungsdienst Daten aus dem Quelldatenspeicher liest, was deutlich effizienter ist.

Weitere Informationen finden Sie unter [Schemazuordnung in Kopieraktivität](copy-activity-schema-and-type-mapping.md).

## <a name="other-considerations"></a>Weitere Überlegungen

Wenn die zu kopierenden Daten sehr umfangreich sind, können Sie Ihre Geschäftslogik so anpassen, dass die Daten weiter partitioniert werden. Sie können eine häufigere Ausführung der Kopieraktivität planen, um die Datengröße für die einzelnen Kopieraktivitätsausführungen zu verringern.

Seien Sie vorsichtig bei der Anzahl von Datasets und Kopieraktivitäten, für die Azure Data Factory gleichzeitig eine Verbindung mit dem gleichen Datenspeicher herstellen muss. Eine hohe Anzahl gleichzeitiger Kopieraufträge kann zu einer Drosselung eines Datenspeichers sowie zu einer Beeinträchtigung der Leistung, zu internen Wiederholungsversuchen bei Kopieraufträgen und in einigen Fällen zu Fehlern bei der Ausführung führen.

## <a name="sample-scenario-copy-from-an-on-premises-sql-server-to-blob-storage"></a>Beispielszenario: Kopieren aus einer lokalen SQL Server-Instanz in Blob Storage

**Szenario:** Es wird eine Pipeline erstellt, um Daten aus einer lokalen SQL Server-Instanz im CSV-Format in Blob Storage zu kopieren. Zur Beschleunigung des Kopierauftrags sollen die CSV-Dateien im BZIP2-Format komprimiert werden.

**Test und Analyse**: Der Durchsatz der Kopieraktivität beträgt weniger als 2 MB/s und liegt damit deutlich unter der Leistungsbenchmark.

**Leistungsanalyse und -optimierung**: Zur Behebung des Leistungsproblems sehen wir uns zunächst einmal an, wie die Daten verarbeitet und verschoben werden.

- **Lesen der Daten**: Die Integration Runtime stellt eine Verbindung mit SQL Server her und sendet die Abfrage. SQL Server sendet daraufhin den Datenstrom über das Intranet an die Integration Runtime.
- **Serialisieren und Komprimieren der Daten**: Die Integration Runtime serialisiert den Datenstrom im CSV-Format und komprimiert die Daten zu einem BZIP2-Datenstrom.
- **Schreiben der Daten**: Die Integration Runtime lädt den BZIP2-Datenstrom über das Internet in Blob Storage hoch.

Wie Sie sehen, werden die Daten sequenziell mittels Streaming verarbeitet und verschoben: SQL Server -> LAN-> Gateway > WAN -> Blob Storage. Die Gesamtleistung wird durch den minimalen Durchsatz der gesamten Pipeline begrenzt.

![Datenfluss](./media/copy-activity-performance/case-study-pic-1.png)

Der Leistungsengpass kann auf folgende Faktoren zurückzuführen sein:

* **Quelle**: SQL Server selbst hat aufgrund einer hohen Auslastung einen geringen Durchsatz.
* **Selbstgehostete Integration Runtime**:
  * **LAN**: Integration Runtime ist weit vom SQL Server-Computer entfernt, und es wird eine Verbindung mit geringer Bandbreite verwendet.
  * **Integration Runtime**: Integration Runtime hat das Auslastungslimit erreicht, um die folgenden Vorgänge durchzuführen:
    * **Serialisierung**: Der Datenstrom lässt sich nur mit geringem Durchsatz in das CSV-Format serialisieren.
    * **Komprimierung**: Sie haben sich für einen langsamen Komprimierungscodec entschieden, z. B. für BZIP2, das 2,8 MB/s mit Core i7 entspricht.
  * **WAN**: Zwischen dem Unternehmensnetzwerk und Ihren Azure-Diensten steht nur eine geringe Bandbreite zur Verfügung. Beispiele: T1 = 1,544 KBit/s; T2 = 6,312 KBit/s.
* **Senke**: Der Durchsatz von Blob Storage ist gering. Dieses Szenario ist eher unwahrscheinlich, da die entsprechende Vereinbarung zum Servicelevel (SLA) mindestens 60 MBit/s garantiert.

In diesem Fall verlangsamt unter Umständen die BZIP2-Datenkomprimierung die gesamte Pipeline. Dieser Engpass lässt sich möglicherweise durch einen Wechsel zum GZIP-Komprimierungscodec beheben.

## <a name="references"></a>Referenzen

Hier finden Sie Referenzen zur Leistungsüberwachung und -optimierung für einige der unterstützten Datenspeicher:

* Azure Storage, einschließlich Blob Storage und Table Storage: [Skalierbarkeits- und Leistungsziele für Azure Storage](../storage/common/storage-scalability-targets.md) und [Checkliste zu Leistung und Skalierbarkeit von Microsoft Azure Storage](../storage/common/storage-performance-checklist.md)
* Azure SQL-Datenbank: Sie können [die Leistung überwachen](../sql-database/sql-database-single-database-monitor.md) und den prozentualen Anteil der Datenbanktransaktionseinheit (Database Transaction Unit, DTU) überprüfen.
* Azure SQL Data Warehouse: Die Funktion wird in DWUs (Data Warehouse-Einheiten) gemessen. Siehe [Verwalten von Computeleistung in Azure SQL Data Warehouse (Übersicht)](../sql-data-warehouse/sql-data-warehouse-manage-compute-overview.md).
* Azure Cosmos DB: [Leistungsebenen in Azure Cosmos DB](../cosmos-db/performance-levels.md)
* Lokaler SQL Server: [Überwachen und Optimieren der Leistung](https://msdn.microsoft.com/library/ms189081.aspx)
* Lokaler Dateiserver: [Leistungsoptimierung für Dateiserver](https://msdn.microsoft.com/library/dn567661.aspx)

## <a name="next-steps"></a>Nächste Schritte
Weitere Informationen finden Sie in den anderen Artikeln zur Kopieraktivität:

- [Kopieraktivität – Übersicht](copy-activity-overview.md)
- [Schemazuordnung der Kopieraktivität](copy-activity-schema-and-type-mapping.md)
- [Copy activity fault tolerance (Fehlertoleranz der Kopieraktivität)](copy-activity-fault-tolerance.md)
