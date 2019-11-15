---
title: Handbuch zur Leistung und Skalierbarkeit der Copy-Aktivität in Azure Data Factory
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
ms.date: 10/24/2019
ms.author: jingwang
ms.openlocfilehash: 701eaad8d36b352e946ae8d74204876b41ecb53d
ms.sourcegitcommit: 609d4bdb0467fd0af40e14a86eb40b9d03669ea1
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 11/06/2019
ms.locfileid: "73678263"
---
# <a name="copy-activity-performance-and-scalability-guide"></a>Handbuch zur Leistung und Skalierbarkeit der Kopieraktivität
> [!div class="op_single_selector" title1="Wählen Sie die von Ihnen verwendete Version von Azure Data Factory aus:"]
> * [Version 1](v1/data-factory-copy-activity-performance.md)
> * [Aktuelle Version](copy-activity-performance.md)

Unabhängig davon, ob Sie eine umfangreiche Datenmigration aus Data Lake oder einem Data Warehouse auf Unternehmensniveau zu Azure durchführen möchten oder ob Sie umfassende Datenmengen aus verschiedenen Datenquellen für die Big Data-Analyse in Azure erfassen möchten, es ist unerlässlich, für die optimale Leistung und Skalierbarkeit zu sorgen.  Azure Data Factory bietet einen leistungsfähigen, robusten und kostengünstigen Mechanismus für die Erfassung von Daten im gewünschten Umfang. Dadurch ist es ideal geeignet für Datentechniker, die leistungsstarke und skalierbare Pipelines für die Datenerfassung erstellen möchten.

Nach dem Lesen dieses Artikels können Sie die folgenden Fragen beantworten:

- Welche Leistung und Skalierbarkeit kann ich mit ADF-Kopieraktivität für Datenmigrations- und Datenerfassungsszenarien erzielen?

- Welche Schritte sind durchzuführen, um die Leistung der ADF-Kopieraktivität zu optimieren?
- Welche Steuerelemente für die ADF-Leistungsoptimierung kann ich verwenden, um die Leistung für eine einzelne Kopieraktivitätsausführung zu optimieren?
- Welche anderen Faktoren außerhalb von ADF sind bei der Optimierung der Kopierleistung zu berücksichtigen?

> [!NOTE]
> Falls Sie mit dem grundlegenden Konzept der Kopieraktivität nicht vertraut sind, finden Sie weitere Informationen unter [Kopieraktivität – Übersicht](copy-activity-overview.md), bevor Sie sich mit diesem Artikel beschäftigen.

## <a name="copy-performance-and-scalability-achievable-using-adf"></a>Durch ADF erreichbare Kopierleistung und -skalierbarkeit

ADF verfügt über eine serverlose Architektur, die Parallelität auf unterschiedlichen Ebenen ermöglicht. Entwickler können dann Pipelines erstellen, um Ihre Netzwerkbandbreite vollständig zu nutzen – sowie IOPS und Bandbreite des Speichers, um den Durchsatz für die Datenverschiebung Ihrer Umgebung zu maximieren.  Das heißt, dass Sie den erzielbaren Durchsatz schätzen können, indem Sie den Mindestdurchsatz des Quelldatenspeichers und des Zieldatenspeichers sowie die Netzwerkbandbreite zwischen Quelle und Ziel messen.  In der folgenden Tabelle wird die Kopierdauer anhand der Datenmenge und des Bandbreitenlimits für Ihre Umgebung berechnet. 

| Datengröße/ <br/> bandwidth | 50 MBit/s    | 100 MBit/s  | 500 MBit/s  | 1 GBit/s   | 5 GBit/s   | 10 GBit/s  | 50 GBit/s   |
| --------------------------- | ---------- | --------- | --------- | -------- | -------- | -------- | --------- |
| **1 GB**                    | 2,7 min    | 1,4 min   | 0,3 min   | 0,1 min  | 0,03 min | 0,01 min | 0,0 min   |
| **10 GB**                   | 27,3 min   | 13,7 min  | 2,7 min   | 1,3 min  | 0,3 min  | 0,1 min  | 0,03 min  |
| **100 GB**                  | 4,6 h    | 2,3 h   | 0,5 h   | 0,2 h  | 0,05 h | 0,02 h | 0,0 h   |
| **1 TB**                    | 46,6 h   | 23,3 h  | 4,7 h   | 2,3 h  | 0,5 h  | 0,2 h  | 0,05 h  |
| **10 TB**                   | 19,4 Tage  | 9,7 Tage  | 1,9 Tage  | 0,9 Tage | 0,2 Tage | 0,1 Tage | 0,02 Tage |
| **100 TB**                  | 194,2 Tage | 97,1 Tage | 19,4 Tage | 9,7 Tage | 1,9 Tage | 1 Tag   | 0,2 Tage  |
| **1 PB**                    | 64,7 Monate    | 32,4 Monate   | 6,5 Monate    | 3,2 Monate   | 0,6 Monate   | 0,3 Monate   | 0,06 Monate   |
| **10 PB**                   | 647,3 Monate   | 323,6 Monate  | 64,7 Monate   | 31,6 Monate  | 6,5 Monate   | 3,2 Monate   | 0,6 Monate    |

Die ADF-Kopieraktivität kann auf verschiedenen Ebenen skaliert werden:

![Skalierung der ADF-Kopieraktivität](media/copy-activity-performance/adf-copy-scalability.png)

- Mit der ADF-Ablaufsteuerung können mehrere Kopieraktivitäten parallel gestartet werden, z. B. per [foreach-Schleife](control-flow-for-each-activity.md).
- Für eine Kopieraktivität können skalierbare Computeressourcen genutzt werden: Bei Verwendung der Azure Integration Runtime können Sie [bis zu 256 Datenintegrationseinheiten (DIUs)](#data-integration-units) serverlos für jede Kopieraktivität angeben. Wenn Sie die selbstgehostete Integration Runtime nutzen, können Sie den Computer manuell zentral hochskalieren oder das horizontale Hochskalieren auf mehrere Computer durchführen ([bis zu vier Knoten](create-self-hosted-integration-runtime.md#high-availability-and-scalability)). Bei einer einzelnen Kopieraktivität wird die Dateigruppe dann auf alle Knoten verteilt.
- Für eine einzelne Kopieraktivität werden mehrere Threads [parallel](#parallel-copy) genutzt, um für den Datenspeicher Lese- und Schreibvorgänge durchzuführen.

## <a name="performance-tuning-steps"></a>Schritte zur Optimierung der Leistung

Führen Sie diese Schritte aus, um die Leistung des Azure Data Factory-Diensts mit der Kopieraktivität zu verbessern.

1. **Wählen Sie ein Testdataset aus, und legen Sie eine Baseline fest.** Testen Sie Ihre Pipeline mit der Kopieraktivität in der Entwicklungsphase mit repräsentativen Beispieldaten. Das ausgewählte Dataset sollte die üblichen Datenmuster widerspiegeln (Ordnerstruktur, Dateimuster, Datenschema usw.) und groß genug sein, um die Kopierleistung bewerten zu können (also ob eine Kopieraktivität 10 Minuten oder länger dauert). Sammeln Sie Ausführungsdetails und Leistungsmerkmale, und folgen Sie dabei der [Überwachung der Kopieraktivität](copy-activity-overview.md#monitoring).

2. **So maximieren Sie die Leistung einer einzelnen Kopieraktivität**:

   Für den Anfang empfehlen wir, dass Sie zunächst die Leistung mit einer einzelnen Kopieraktivität maximieren.

   **Bei Ausführung der Kopieraktivität in einer Azure Integration Runtime:**

   Beginnen Sie mit den Standardwerten für [Datenintegrationseinheiten (DIU)](#data-integration-units) und [Paralleles Kopieren](#parallel-copy).  Führen Sie einen Leistungstestlauf aus, und notieren Sie sowohl die erzielte Leistung als auch die tatsächlichen Werte für DIUs und parallele Kopiervorgänge.  Einzelheiten zum Erfassen von Testlaufergebnissen und zu verwendeten Leistungseinstellungen finden Sie unter [Überwachen der Kopieraktivität](copy-activity-overview.md#monitoring).

   Führen Sie nun weitere Leistungstestdurchläufe durch, und verdoppeln Sie dabei jedes Mal den Wert für die DIU-Einstellung.  Wenn Sie der Ansicht sind, dass die mit der Standardeinstellung erzielte Leistung weit unter Ihren Erwartungen liegt, können Sie die DIU-Einstellung im darauffolgenden Testlauf auch stärker erhöhen.

   Die Kopieraktivität sollte beim Erhöhen des Werts der DIU-Einstellung nahezu vollkommen linear skaliert werden.  Wenn Sie durch die Verdoppelung des Wertes der DIU-Einstellung nicht den doppelten Durchsatz erhalten, kann dies zwei Ursachen haben:

   - Das ausgeführte spezifische Kopiermuster profitiert nicht vom Hinzufügen weiterer DIUs.  Trotz der Angabe eines größeren DIU-Werts blieben die tatsächlich genutzten DIUs unverändert, und daher erhalten Sie denselben Durchsatz wie zuvor.  In diesem Fall maximieren Sie den aggregierten Durchsatz durch paralleles Ausführen mehrerer Kopiervorgänge (siehe Schritt 3).
   - Durch das Hinzufügen weiterer DIUs (mehr Leistung), was eine höhere Geschwindigkeit beim Extrahieren, Übertragen und Laden von Daten bewirkt, wurde ein Engpass des Quelldatenspeichers, des Zieldatenspeichers oder des dazwischen liegenden Netzwerks Engpass erreicht und möglicherweise fand eine Drosselung statt.  Wenn dies der Fall ist, wenden Sie sich an Ihren Datenspeicheradministrator oder Netzwerkadministrator, um die Obergrenze heraufzusetzen, oder verringern Sie den Wert der DIU-Einstellung, bis keine Drosselung mehr auftritt.

   **Bei Ausführung der Kopieraktivität in einer selbstgehosteten Integration Runtime:**

   Wir empfehlen, dass Sie zum Hosten der Integration Runtime einen dedizierten Computer und nicht den Server verwenden, der den Datenspeicher hostet.

   Beginnen Sie mit den Standardwerten für [Paralleles Kopieren](#parallel-copy), und verwenden Sie einen einzelnen Knoten für die selbstgehostete Integration Runtime.  Führen Sie einen Leistungstestlauf aus, und notieren Sie die erzielte Leistung.

   Wenn Sie einen höheren Durchsatz erreichen möchten, können Sie die selbstgehostete Integration Runtime zentral hochskalieren oder horizontal hochskalieren:

   - Wenn die CPU und der verfügbare Arbeitsspeicher auf dem Knoten der selbstgehosteten Integration Runtime nicht vollständig genutzt werden, bei der Ausführung paralleler Aufträge jedoch das Limit erreicht wird, führen Sie eine zentrale Hochskalierung aus, indem Sie die Anzahl der parallelen Aufträge vergrößern, die auf einem Knoten ausgeführt werden können.  Anweisungen dazu finden Sie [hier](create-self-hosted-integration-runtime.md#scale-up).
   - Ist jedoch die CPU-Auslastung auf dem Knoten der selbstgehosteten IR hoch und der verfügbare Arbeitsspeicher gering, können Sie einen neuen Knoten hinzufügen, um die Last über mehrere Knoten horizontal zu skalieren.  Anweisungen dazu finden Sie [hier](create-self-hosted-integration-runtime.md#high-availability-and-scalability).

   Wenn Sie die Kapazität der selbstgehosteten Integration Runtime zentral oder horizontal hochskalieren, wiederholen Sie den Leistungstestlauf, um festzustellen, ob Sie einen zunehmend besseren Durchsatz erzielen.  Wenn sich der Durchsatz nicht mehr verbessert, hat höchstwahrscheinlich der Quelldatenspeicher, der Zieldatenspeicher oder das dazwischenliegende Netzwerk einen Engpass erreicht, und eine Drosselung setzt ein. Wenn dies der Fall ist, wenden Sie sich an Ihren Datenspeicheradministrator oder Netzwerkadministrator, um die Obergrenze heraufzusetzen. Alternativ können Sie auch zur vorherigen Skalierungseinstellung für die selbstgehostete Integration Runtime zurückkehren. 

3. **So maximieren Sie den aggregierten Durchsatz durch paralleles Ausführen mehrerer Kopiervorgänge:**

   Nun haben Sie die Leistung einer einzelnen Kopieraktivität maximiert. Wenn Sie die Durchsatzobergrenzen Ihrer Umgebung (Netzwerk, Quelldatenspeicher und Zieldatenspeicher) noch nicht erreicht haben, können Sie mit ADF-Flusssteuerungskonstrukten wie [Für jede Schleife](control-flow-for-each-activity.md) mehrere Kopieraktivitäten parallel ausführen.

4. **Tipps zur Leistungsoptimierung und Optimierungsfeatures.** In einigen Fällen wird bei der Ausführung einer Kopieraktivität in Azure Data Factory oben auf der [Seite zur Überwachung der Kopieraktivität](copy-activity-overview.md#monitor-visually) die Meldung „Tipps zur Leistungsoptimierung“ angezeigt, wie im folgenden Beispiel gezeigt. Die Meldung informiert Sie über den Engpass, der für die jeweilige Kopierausführung identifiziert wurde. Zudem enthält sie Ratschläge, was Sie ändern können, um die Kopierdurchsatz zu steigern. Die Tipps zur Leistungsoptimierung bieten derzeit Vorschläge wie:

   - Verwenden Sie PolyBase, wenn Sie Daten in Azure SQL Data Warehouse kopieren.
   - Erhöhen Sie die Azure Cosmos DB-Anforderungseinheiten oder Azure SQL-Datenbank-DTUs (Datenbankdurchsatzeinheiten), wenn die Ressource auf der Datenspeicherseite der Engpass ist.
   - Entfernen Sie das unnötige gestaffelte Kopieren.

   Die Regeln für die Leistungsoptimierung werden auch schrittweise ergänzt werden.

   **Beispiel: Kopieren in Azure SQL-Datenbank mit Tipps für die Leistungsoptimierung**

   In diesem Beispiel stellt Azure Data Factory während eines Kopiervorgangs fest, dass die Azure SQL-Senkendatenbank eine hohe DTU-Auslastung erreicht, was die Schreibvorgänge verlangsamt. Der Vorschlag ist, die Azure SQL-Datenbankebene mit mehr DTUs zu erweitern. 

   ![Überwachung des Kopiervorgangs mit Tipps für die Leistungsoptimierung](media/copy-activity-overview/copy-monitoring-with-performance-tuning-tips.png)

   Beachten Sie außerdem die folgenden Features für die Leistungsoptimierung:

   - [Parallele Kopie](#parallel-copy)
   - [Datenintegrationseinheiten](#data-integration-units)
   - [Gestaffeltes Kopieren](#staged-copy)
   - [Skalierbarkeit der selbstgehosteten Integration Runtime-Infrastruktur](concepts-integration-runtime.md#self-hosted-integration-runtime)

5. **Erweitern der Konfiguration auf das gesamte Dataset.** Wenn Sie mit den Ausführungsergebnissen und mit der Leistung zufrieden sind, können Sie die Definition und die Pipeline auf Ihr gesamtes Dataset erweitern.

## <a name="copy-performance-optimization-features"></a>Features für die Leistungsoptimierung von Kopiervorgängen

Azure Data Factory bietet die folgenden Funktionen zur Leistungsoptimierung:

- [Parallele Kopie](#parallel-copy)
- [Datenintegrationseinheiten](#data-integration-units)
- [Gestaffeltes Kopieren](#staged-copy)

### <a name="data-integration-units"></a>Datenintegrationseinheiten

Eine Datenintegrationseinheit ist eine Messgröße für die Leistungsfähigkeit (Kombination aus zugeteilten CPU-, Speicher- und Netzwerkressourcen) einer einzelnen Einheit in Azure Data Factory. Die Datenintegrationseinheit gilt nur für die [Azure-Integration Runtime](concepts-integration-runtime.md#azure-integration-runtime), aber nicht für [selbstgehostete Integration Runtimes](concepts-integration-runtime.md#self-hosted-integration-runtime).

Ihnen wird die **Anzahl der verwendeten DIUs \* Kopierdauer \* Einheitenpreis/DIU-Stunde** in Rechnung gestellt. Sehen Sie sich [hier](https://azure.microsoft.com/pricing/details/data-factory/data-pipeline/) die aktuellen Preise an. Je nach Abonnementtyp können lokale Währungen und separate Rabatte gelten.

Die zulässige Anzahl von DIUs für die Ausführung einer Kopieraktivität liegt zwischen **2 und 256**. Wenn diese Option nicht angegeben ist oder Sie „Auto“ in der Benutzeroberfläche auswählen, wendet die Data Factory die optimale DIU-Einstellung dynamisch auf Grundlage des Quell-/Senkenpaars und des Datenmusters an. In der folgenden Tabelle werden die Standard-DIUs aufgelistet, die in verschiedenen Kopierszenarien verwendet werden:

| Kopierszenario | Vom Dienst bestimmte Standard-DIUs |
|:--- |:--- |
| Kopieren von Daten zwischen dateibasierten Speichern | Zwischen 4 und 32, je nach Anzahl und Größe der Dateien |
| Kopieren von Daten nach Azure SQL-Datenbank oder Azure Cosmos DB |Zwischen 4 und 16, abhängig von der Dienstebene der Azure SQL-Senkendatenbank oder Cosmos DB (Anzahl von DTUs/RUs) |
| Alle anderen Kopierszenarien | 4 |

Sie können diese Standardeinstellung überschreiben, indem Sie wie folgt einen Wert für die **dataIntegrationUnits**-Eigenschaft angeben. Die *tatsächliche Anzahl von DIUs*, die der Kopiervorgang zur Laufzeit verwendet, entspricht maximal dem konfigurierten Wert. Dies ist abhängig von Ihrem Datenmuster.

Sie können die für die einzelnen Kopierausführungen verwendeten DIUs in der Ausgabe der Kopieraktivität sehen, wenn Sie eine Aktivitätsausführung überwachen. Weitere Informationen finden Sie unter [Überwachung der Kopieraktivität](copy-activity-overview.md#monitoring).

> [!NOTE]
> Ein höherer Wert als vier für DIUs gilt derzeit nur, wenn Sie mehrere Dateien aus Azure Blob/ADLS Gen1/ADLS Gen2/Amazon S3/Google Cloud Storage/Cloud-FTP/Cloud-SFTP oder aus einem relationalen Datenspeicher mit aktivierter Partitionsoption (einschließlich [Oracle](connector-oracle.md#oracle-as-source)/[Netezza](connector-netezza.md#netezza-as-source)/[Teradata](connector-teradata.md#teradata-as-source)) in einen beliebigen anderen Clouddatenspeicher kopieren.

**Beispiel:**

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

### <a name="parallel-copy"></a>Parallele Kopie

Mithilfe der **parallelCopies** -Eigenschaft können Sie die gewünschte Parallelität für die Kopieraktivität angeben. Diese Eigenschaft können Sie sich als die maximale Anzahl von Threads in einer Kopieraktivität vorstellen, die parallel aus Quelldatenspeichern lesen oder in Senkendatenspeicher schreiben können.

Für jede Kopieraktivitätsausführung ermittelt Azure Data Factory die Anzahl paralleler Kopien, die zum Kopieren von Daten aus dem Quelldatenspeicher sowie zum Kopieren von Daten in den Zieldatenspeicher verwendet werden. Die verwendete Standardanzahl paralleler Kopien hängt von der Art der verwendeten Quelle und Senke ab.

| Kopierszenario | Standardanzahl der vom Dienst ermittelten parallelen Kopien |
| --- | --- |
| Kopieren von Daten zwischen dateibasierten Speichern |Dies ist abhängig von der Größe der Dateien und der Anzahl DIUs, die zum Kopieren von Daten zwischen zwei Clouddatenspeichern verwendet werden, oder von der physischen Konfiguration des Computers der selbstgehosteten Integration Runtime-Infrastruktur. |
| Kopieren aus dem relationalen Datenspeicher mit aktivierter Partitionsoption (einschließlich [Oracle](connector-oracle.md#oracle-as-source), [Netezza](connector-netezza.md#netezza-as-source), [Teradata](connector-teradata.md#teradata-as-source), [SAP Table](connector-sap-table.md#sap-table-as-source) und [SAP Open Hub](connector-sap-business-warehouse-open-hub.md#sap-bw-open-hub-as-source))|4 |
| Kopieren von Daten aus einem beliebigen Quellspeicher im Azure-Tabellenspeicher |4 |
| Alle anderen Kopierszenarios |1 |

> [!TIP]
> Beim Kopieren von Daten zwischen dateibasierten Speichern bietet das Standardverhalten in der Regel den bestmöglichen Durchsatz. Das Standardverhalten wird automatisch basierend auf dem Muster Ihrer Quelldatei festgelegt.

Sie können den Standardwert jedoch überschreiben und einen Wert für die Eigenschaft **parallelCopies** angeben, um die Last für die Computer zu steuern, auf denen Ihre Datenspeicher gehostet werden, oder um die Kopierleistung zu optimieren. Der Wert muss eine ganze Zahl größer als oder gleich 1 sein. Zur Laufzeit verwendet die Kopieraktivität maximal den von Ihnen festgelegten Wert, um eine optimale Leistung zu erzielen.

**Beachten Sie Folgendes:**

- Beim Kopieren von Daten zwischen dateibasierten Speichern bestimmt **parallelCopies** die Parallelität auf Dateiebene. Die Segmentierung innerhalb einer einzelnen Datei erfolgt darunter automatisch und transparent. Dadurch wird die am besten geeignete Segmentgröße für einen angegebenen Quelldatenspeichertyp verwendet, um Daten parallel und orthogonal in **parallelCopies** zu laden. Die tatsächliche Anzahl paralleler Kopien, die der Datenverschiebungsdienst zur Laufzeit für den Kopiervorgang verwendet, entspricht maximal der Anzahl vorhandener Dateien. Bei Verwendung des Kopierverhaltens **mergeFile** kann die Kopieraktivität keine Parallelität auf Dateiebene nutzen.
- Beim Kopieren von Daten aus nicht dateibasierten Speichern (außer den Connectors für [Oracle](connector-oracle.md#oracle-as-source), [Netezza](connector-netezza.md#netezza-as-source), [Teradata](connector-teradata.md#teradata-as-source), [SAP Table](connector-sap-table.md#sap-table-as-source) und [SAP Open Hub](connector-sap-business-warehouse-open-hub.md#sap-bw-open-hub-as-source) als Quelle mit aktivierter Datenpartitionierung) in dateibasierte Speicher ignoriert der Datenverschiebungsdienst die **parallelCopies**-Eigenschaft. Die Parallelität wird in diesem Fall also nicht angewendet, auch wenn sie angegeben wurde.
- Die **parallelCopies**-Eigenschaft ist orthogonal zu **dataIntegrationUnits**. Die erste Eigenschaft wird für alle Datenintegrationseinheiten gezählt.
- Berücksichtigen Sie beim Angeben eines Werts für die **parallelCopies**-Eigenschaft die höhere Auslastung Ihrer Quell- und Senkendatenspeicher. Berücksichtigen Sie auch den Lastanstieg zur selbstgehosteten Integration Runtime, wenn die Kopieraktivität davon abhängt, z. B. bei Hybridkopiervorgängen. Dieser Lastanstieg tritt insbesondere auf, wenn Sie über mehrere Aktivitäten oder gleichzeitige Ausführungen der gleichen Aktivitäten verfügen, die für den gleichen Datenspeicher ausgeführt werden. Sollten Sie eine Überlastung des Datenspeichers oder der selbstgehosteten Integration Runtime-Infrastruktur feststellen, verringern Sie den Wert **parallelCopies**, um die Last zu reduzieren.

**Beispiel:**

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

### <a name="staged-copy"></a>gestaffeltem Kopieren

Beim Kopieren von Daten aus einem Quelldatenspeicher in einen Senkendatenspeicher können Sie ggf. Blob Storage als Stagingzwischenspeicher verwenden. Staging ist besonders in folgenden Fällen hilfreich:

- **Sie möchten Daten aus verschiedenen Datenspeichern über PolyBase in SQL Data Warehouse erfassen.** SQL Data Warehouse nutzt PolyBase als Mechanismus mit hohem Durchsatz, um große Datenmengen in SQL Data Warehouse zu laden. Die Quelldaten müssen sich in Blob Storage oder in Azure Data Lake Store befinden und einige andere Kriterien erfüllen. Wenn Sie Daten aus einem Blob Storage- oder Azure Data Lake Store-fremden Datenspeicher laden, können Sie das Kopieren von Daten über einen Staging-Blob-Zwischenspeicher aktivieren. In diesem Fall führt Azure Data Factory die erforderlichen Datentransformationen durch, um die Anforderungen von PolyBase zu erfüllen. Anschließend werden die Daten mithilfe von PolyBase auf effiziente Art und Weise in SQL Data Warehouse geladen. Weitere Informationen finden Sie unter [Use PolyBase to load data into Azure SQL Data Warehouse (Verwenden von PolyBase zum Laden von Daten in Azure SQL Data Warehouse)](connector-azure-sql-data-warehouse.md#use-polybase-to-load-data-into-azure-sql-data-warehouse).
- **Hybriddatenverschiebungen (also das Kopieren aus einem lokalen Datenspeicher in einen Clouddatenspeicher) können bei einer langsamen Netzwerkverbindung eine Weile dauern.** Zur Verbesserung der Leistung können Sie gestaffeltes Kopieren verwenden, um die Daten lokal zu komprimieren und dadurch die Verschiebung in den Stagingdatenspeicher in der Cloud zu beschleunigen. Anschließend können Sie die Daten im Stagingspeicher wieder dekomprimieren, bevor Sie sie in den Zieldatenspeicher laden.
- **Aufgrund der IT-Richtlinien des Unternehmens sollten Sie mit Ausnahme der Ports 80 und 443 keine weiteren Ports in der Firewall öffnen.** Ein Beispiel: Beim Kopieren von Daten aus einem lokalen Datenspeicher an eine Azure SQL-Datenbank- oder Azure SQL Data Warehouse-Senke muss die ausgehende TCP-Kommunikation über den Port 1433 sowohl für die Windows-Firewall als auch für die Unternehmensfirewall ermöglicht werden. In diesem Szenario kann das gestaffelte Kopieren die selbstgehostete Integration Runtime nutzen, um zunächst Daten per HTTP oder HTTPS über den Port 443 an eine Blob Storage-Staginginstanz kopieren. Anschließend können die Daten aus dem Blob Storage-Stagingspeicher in SQL-Datenbank oder in SQL Data Warehouse geladen werden. Dadurch muss der Port 1433 nicht geöffnet werden.

#### <a name="how-staged-copy-works"></a>Funktionsweise des gestaffelten Kopierens

Bei aktivierter Stagingfunktion werden die Daten zunächst aus dem Quelldatenspeicher in den (von Ihnen bereitgestellten) Blob-Stagingspeicher kopiert. Anschließend werden die Daten dann aus dem Stagingdatenspeicher in den Senkendatenspeicher kopiert. Dieser zweistufige Prozess wird automatisch von Azure Data Factory verwaltet. Nach Abschluss der Datenverschiebung bereinigt Azure Data Factory außerdem temporäre Daten im Stagingspeicher.

![gestaffeltem Kopieren](media/copy-activity-performance/staged-copy.png)

Wenn Sie die Datenverschiebung unter Verwendung des Stagingspeichers aktivieren, können Sie angeben, ob die Daten vor dem Verschieben aus dem Quelldatenspeicher in einen Zwischen- oder Stagingdatenspeicher komprimiert und vor dem Verschieben der Daten aus einem Zwischen- oder Stagingdatenspeicher in den Senkendatenspeicher wieder dekomprimiert werden sollen.

Derzeit können Sie keine Daten zwischen zwei Datenspeichern kopieren, die über verschiedene selbstgehostete IRs verbunden sind, weder mit noch ohne gestaffeltes Kopieren. Für ein solches Szenario können Sie zwei explizit verkettete Kopiervorgänge konfigurieren, um von der Quelle zum Stagingspeicher und aus dem Stagingspeicher zur Senke zu kopieren.

#### <a name="configuration"></a>Konfiguration

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

#### <a name="staged-copy-billing-impact"></a>Auswirkungen auf die Abrechnung von gestaffeltem Kopieren

Die Abrechnung basiert auf zwei Aspekten: Dauer des Kopiervorgangs und Art der Kopie.

* Wenn Sie einen Stagingspeicher während eines Cloudkopiervorgangs (d. h. Kopieren von Daten aus einem Clouddatenspeicher in einen anderen Clouddatenspeicher. Beide Phasen werden von Azure-Integration Runtime unterstützt) verwenden, gilt folgende Formel: [Gesamtkopierdauer für Schritt 1 und 2] [Einzelpreis für Cloudkopien].
* Wenn Sie Staging während eines Hybridkopiervorgangs (d. h. Kopieren von Daten aus einem lokalen Datenspeicher in einen Clouddatenspeicher. Eine Phase wird von der selbstgehosteten Integration Runtime-Infrastruktur unterstützt) verwenden, gilt folgende Formel: [Dauer des Hybridkopiervorgangs] [Einzelpreis für Hybridkopien] + [Dauer des Cloudkopiervorgangs] [Einzelpreis für Cloudkopien].

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
- [Verwenden von Azure Data Factory zum Migrieren von Daten aus einem Data Lake oder Data Warehouse zu Azure](data-migration-guidance-overview.md)
- [Migrieren von Daten aus Amazon S3 zu Azure Storage](data-migration-guidance-s3-azure-storage.md)
