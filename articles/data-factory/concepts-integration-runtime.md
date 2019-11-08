---
title: Integrationslaufzeit in Azure Data Factory
description: Enthält Informationen zur Integrationslaufzeit in Azure Data Factory.
services: data-factory
documentationcenter: ''
author: nabhishek
manager: craigg
ms.reviewer: douglasl
ms.service: data-factory
ms.workload: data-services
ms.tgt_pltfrm: na
ms.topic: conceptual
ms.date: 05/31/2019
ms.author: abnarain
ms.openlocfilehash: 7475d660fd2fa64ce69e7c23e0bad518fffddd26
ms.sourcegitcommit: 609d4bdb0467fd0af40e14a86eb40b9d03669ea1
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 11/06/2019
ms.locfileid: "73681509"
---
# <a name="integration-runtime-in-azure-data-factory"></a>Integrationslaufzeit in Azure Data Factory
Bei der Integrationslaufzeit (Integration Runtime, IR) handelt es sich um die Computeinfrastruktur, mit der Azure Data Factory die folgenden Datenintegrationsfunktionen für verschiedene Netzwerkumgebungen bereitstellt:

- **Datenfluss**: Ausführen eines [Datenflusses](concepts-data-flow-overview.md) in einer verwalteten Azure-Computeumgebung.  
- **Datenverschiebung:** Dient zum Kopieren von Daten in Datenspeichern im öffentlichen Netzwerk und Datenspeichern im privaten Netzwerk (lokales oder virtuelles privates Netzwerk). Es wird Unterstützung für integrierte Connectors, Formatkonvertierung, Spaltenzuordnung und eine leistungsstarke und skalierbare Datenübertragung bereitgestellt.
- **Aktivitätsverteilung:**  Dient zum Verteilen und Überwachen von Transformationsaktivitäten, die in vielen verschiedenen Computediensten wie Azure Databricks, Azure HDInsight, Azure Machine Learning, Azure SQL-Datenbank, SQL Server und vielen weiteren ausgeführt werden.
- **SSIS-Paketausführung:** Dient zum nativen Ausführen von SSIS-Paketen (SQL Server Integration Services) in einer verwalteten Azure-Computeumgebung.

In Data Factory wird mit einer Aktivität eine durchzuführende Aktion definiert. Mit einem verknüpften Dienst wird ein Zieldatenspeicher oder ein Computedienst definiert. Eine Integrationslaufzeit stellt die Brücke zwischen der Aktivität und verknüpften Diensten dar.  Vom verknüpften Dienst oder der Aktivität wird darauf verwiesen, und sie stellt die Computeumgebung bereit, in der die Aktivität entweder ausgeführt wird oder aus der sie verteilt wird. Auf diese Weise kann die Aktivität in der Region durchgeführt werden, die dem Zieldatenspeicher bzw. dem Computedienst am nächsten liegt, und es kann die höchste Leistung erzielt werden, während gleichzeitig die Anforderungen an die Sicherheit und Konformität erfüllt werden.

## <a name="integration-runtime-types"></a>Integrationslaufzeit-Typen
Data Factory verfügt über drei Integrationslaufzeit-Typen. Sie sollten den Typ wählen, der die Anforderungen an die gewünschten Datenintegrationsfunktionen und die Netzwerkumgebung am besten erfüllt.  Diese drei Typen lauten:

- Azure
- Selbstgehostet
- Azure-SSIS

In der folgenden Tabelle sind die Funktionen und die Netzwerkunterstützung für die einzelnen Integrationslaufzeit-Typen beschrieben:

IR-Typ | Öffentliches Netzwerk | Privates Netzwerk
------- | -------------- | ---------------
Azure | Datenfluss<br/>Datenverschiebung<br/>Aktivitätsverteilung | &nbsp;
Selbstgehostet | Datenverschiebung<br/>Aktivitätsverteilung | Datenverschiebung<br/>Aktivitätsverteilung
Azure-SSIS | SSIS-Paketausführung | SSIS-Paketausführung

Im folgenden Diagramm ist dargestellt, wie die unterschiedlichen Integrationslaufzeiten als Kombination verwendet werden können, um umfassende Datenintegrationsfunktionen und Netzwerkunterstützung zu erzielen:

![Verschiedene Integrationslaufzeit-Typen](media/concepts-integration-runtime/different-integration-runtimes.png)

## <a name="azure-integration-runtime"></a>Azure-Integrationslaufzeit
Die Azure-Integrationslaufzeit ermöglicht Folgendes:

- Ausführen von Datenflüssen in Azure 
- Ausführen von Kopieraktivitäten zwischen Clouddatenspeichern
- Bereitstellen der folgenden Transformationsaktivitäten im öffentlichen Netzwerk: Databricks Notebook-/ Jar-/ Python-Aktivität, HDInsight Hive-Aktivität, HDInsight Pig-Aktivität, HDInsight MapReduce-Aktivität, HDInsight Spark-Aktivität, HDInsight-Streamingaktivität, Machine Learning-Batchausführungsaktivität, Machine Learning-Ressourcenaktualisierungsaktivitäten, Aktivität „Gespeicherte Prozedur“, U-SQL-Aktivität für Data Lake Analytics, benutzerdefinierte .NET-Aktivität, Webaktivität, Lookup-Aktivität und Aktivität „Metadaten abrufen“.

### <a name="azure-ir-network-environment"></a>Azure-Integrationslaufzeit: Netzwerkumgebung
Azure Integration Runtime unterstützt die Verbindungsherstellung mit Datenspeichern und Computediensten mit öffentlich zugänglichen Endpunkten. Verwenden Sie eine selbstgehostete Integrationslaufzeit für die virtuelle Azure-Netzwerkumgebung.

### <a name="azure-ir-compute-resource-and-scaling"></a>Azure-Integrationslaufzeit: Computeressource und Skalierung
Die Azure-Integrationslaufzeit stellt in Azure eine vollständig verwaltete, serverlose Computeressource bereit.  Sie müssen sich keine Gedanken um die Infrastrukturbereitstellung, die Softwareinstallation, das Patchen oder die Kapazitätsskalierung machen.  Darüber hinaus zahlen Sie nur für die tatsächliche Nutzungsdauer.

Mit der Azure-Integrationslaufzeit wird die native Computeressource zum sicheren, zuverlässigen und leistungsstarken Verschieben von Daten zwischen Clouddatenspeichern bereitgestellt.  Sie können festlegen, wie viele Datenintegrationseinheiten für die Kopieraktivität verwendet werden. Die Computegröße der Azure IR wird entsprechend elastisch zentral hochskaliert, ohne dass Sie die Größe der Azure-Integrationslaufzeit explizit anpassen müssen. 

Die Aktivitätsverteilung ist ein einfacher Vorgang zum Leiten der Aktivität an den Zielcomputedienst, sodass es nicht erforderlich ist, die Computegröße für dieses Szenario zentral hochzuskalieren.

Informationen zur Erstellung und Konfiguration einer Azure IR finden Sie in den Anleitungen unter „Gewusst wie: Erstellen und Konfigurieren der Azure IR“. 

> [!NOTE] 
> Azure Integration Runtime verfügt über Eigenschaften, die mit der Datenflussruntime in Zusammenhang stehen, die die zugrunde liegende Computeinfrastruktur definiert, auf der die Datenflüsse ausgeführt würden. 

## <a name="self-hosted-integration-runtime"></a>Selbstgehostete Integrationslaufzeit
Eine selbstgehostete Integrationslaufzeit ermöglicht Folgendes:

- Ausführen einer Kopieraktivität zwischen einem Clouddatenspeicher und einem Datenspeicher im privaten Netzwerk.
- Bereitstellen der folgenden Transformationsaktivitäten für Computeressourcen im lokalen Netzwerk oder im virtuellen Azure-Netzwerk: HDInsight Hive-Aktivität (Bring Your Own Cluster, BYOC), HDInsight Pig-Aktivität (BYOC), HDInsight MapReduce-Aktivität (BYOC), HDInsight Spark-Aktivität (BYOC), HDInsight-Streamingaktivität (BYOC), Machine Learning-Batchausführungsaktivität, Machine Learning-Ressourcenaktualisierungsaktivitäten, Aktivität „Gespeicherte Prozedur“, U-SQL-Aktivität für Data Lake Analytics, benutzerdefinierte Aktivität (wird in Azure Batch ausgeführt), Lookup-Aktivität und Aktivität „Metadaten abrufen“.

> [!NOTE] 
> Verwenden Sie die selbstgehostete Integrationslaufzeit zum Unterstützen von Datenspeichern, für die „Bring-your-own Driver“ erforderlich ist, z.B. SAP Hana, MySQL usw.  Weitere Informationen finden Sie unter [Unterstützte Datenspeicher](copy-activity-overview.md#supported-data-stores-and-formats).

### <a name="self-hosted-ir-network-environment"></a>Selbstgehostete Integrationslaufzeit: Netzwerkumgebung
Wenn Sie die Datenintegration auf sichere Weise in einer privaten Netzwerkumgebung durchführen möchten, die nicht über eine direkte Sichtlinie aus der öffentlichen Cloudumgebung verfügt, können Sie eine selbstgehostete IR in der lokalen Umgebung hinter Ihrer Unternehmensfirewall oder in einem virtuellen privaten Netzwerk installieren.  Die selbstgehostete Integrationslaufzeit stellt nur ausgehende HTTP-basierte Verbindungen in das offene Internet her.

### <a name="self-hosted-ir-compute-resource-and-scaling"></a>Selbstgehostete Integrationslaufzeit: Computeressource und Skalierung
Die selbstgehostete Integrationslaufzeit muss auf einem lokalen Computer oder einem virtuellen Computer in einem privaten Netzwerk installiert werden. Derzeit wird nur das Ausführen der selbstgehosteten Integrationslaufzeit auf einem Windows-Betriebssystem unterstützt.  

Zur Erzielung von Hochverfügbarkeit und Skalierbarkeit können Sie die selbstgehostete Integrationslaufzeit horizontal hochskalieren, indem Sie die logische Instanz mehreren lokalen Computern im Aktiv-Aktiv-Modus zuordnen.  Ausführlichere Informationen finden Sie in den Anleitungen im Artikel [Erstellen und Konfigurieren einer selbstgehosteten Integration Runtime](create-self-hosted-integration-runtime.md).

## <a name="azure-ssis-integration-runtime"></a>Azure-SSIS-Integrationslaufzeit
Für die Durchführung von Lift & Shift-Vorgängen für vorhandene SSIS-Workloads können Sie eine Azure-SSIS-Integrationslaufzeit erstellen, um SSIS-Pakete nativ auszuführen.

### <a name="azure-ssis-ir-network-environment"></a>Azure-SSIS-Integrationslaufzeit: Netzwerkumgebung
Die Azure-SSIS--Integrationslaufzeit kann entweder im öffentlichen oder im privaten Netzwerk bereitgestellt werden.  Der lokale Datenzugriff wird unterstützt, indem Azure-SSIS-IR mit einem virtuellen Netzwerk verknüpft wird, für das eine Verbindung mit Ihrem lokalen Netzwerk besteht.  

### <a name="azure-ssis-ir-compute-resource-and-scaling"></a>Azure-SSIS-Integrationslaufzeit: Computeressource und Skalierung
Die Azure-SSIS-Integrationslaufzeit ist ein vollständig verwalteter Cluster mit Azure-VMs, die speziell für die Ausführung von SSIS-Paketen bestimmt sind. Sie können Ihren eigenen Server mit Azure SQL-Datenbank oder mit einer verwalteten Instanz bereitstellen, um den Katalog mit SSIS-Projekten/-Paketen (SSISDB) zu hosten, der daran angefügt werden soll. Sie können die Computeleistung zentral hochskalieren, indem Sie die Knotengröße angeben und horizontal hochskalieren. Geben Sie hierzu die Anzahl von Knoten im Cluster an. Sie können die Kosten für die Ausführung Ihrer Azure-SSIS-Integrationslaufzeit verwalten, indem Sie sie je nach Bedarf anhalten und starten.

Weitere Informationen finden Sie in den Anleitungen unter „Gewusst wie: Erstellen und Konfigurieren von Azure-SSIS-Integrationslaufzeit“.  Nach der Erstellung können Sie Ihre vorhandenen SSIS-Pakete mit nur wenigen oder auch ganz ohne Änderungen bereitstellen und verwalten, indem Sie vertraute Tools wie SQL Server Data Tools (SSDT) und SQL Server Management Studio (SSMS) verwenden – genauso wie bei der lokalen Nutzung von SSIS.

Weitere Informationen zur Azure-SSIS-Laufzeit finden Sie in den folgenden Artikeln: 

- [Tutorial: Bereitstellen von SSIS-Paketen in Azure](tutorial-create-azure-ssis-runtime-portal.md): Dieser Artikel enthält schrittweise Anweisungen zum Erstellen einer Azure-SSIS-Integrationslaufzeit und verwendet eine Azure SQL-Datenbank zum Hosten des SSIS-Katalogs. 
- [Erstellen der Azure-SSIS Integration Runtime in Azure Data Factory](create-azure-ssis-integration-runtime.md): In diesem Artikel wird das Tutorial vertieft, und er enthält Anweisungen zur Verwendung einer verwalteten Azure SQL-Datenbank-Instanz und zum Verknüpfen der IR mit einem virtuellen Netzwerk. 
- [Überwachen einer Azure-SSIS-Integrationslaufzeit](monitor-integration-runtime.md#azure-ssis-integration-runtime): In diesem Artikel wird das Abrufen von Informationen zu einer Azure-SSIS-Integrationslaufzeit veranschaulicht, und er enthält Beschreibungen der Status in den zurückgegebenen Informationen. 
- [Verwalten einer Azure-SSIS-Integrationslaufzeit](manage-azure-ssis-integration-runtime.md): In diesem Artikel wird beschrieben, wie Sie eine Azure-SSIS-Integrationslaufzeit beenden, starten oder entfernen. Es wird zudem gezeigt, wie Sie Ihre Azure-SSIS-Integrationslaufzeit horizontal hochskalieren, indem Sie der Integrationslaufzeit weitere Knoten hinzufügen. 
- [Verknüpfen einer Azure-SSIS-Integration Runtime mit einem virtuellen Netzwerk:](join-azure-ssis-integration-runtime-virtual-network.md). Dieser Artikel enthält grundlegende Informationen zum Verknüpfen einer Azure-SSIS-IR mit einem virtuellen Azure-Netzwerk. Darüber hinaus enthält er Schritte zur Verwendung des Azure-Portals zum Konfigurieren des virtuellen Netzwerks, damit die Azure-SSIS-IR dem virtuellen Netzwerk beitreten kann. 

## <a name="integration-runtime-location"></a>Ort der Integrationslaufzeit
Am Data Factory-Standort werden die Metadaten der Data Factory gespeichert, und von diesem Standort wird auch die Auslösung der Pipeline initiiert. Eine Data Factory kann währenddessen auf Datenspeicher und Compute Services in anderen Azure-Regionen zugreifen, um Daten zwischen Datenspeichern zu verschieben oder Daten mithilfe von Computediensten zu verarbeiten. Dieses Verhalten wird durch die [global verfügbare Integrationslaufzeit](https://azure.microsoft.com/global-infrastructure/services/) realisiert, um für Datenkonformität, Effizienz und geringere Kosten für ausgehenden Netzwerkdatenverkehr zu sorgen.

Mit dem Integrationslaufzeit-Standort wird der Standort der Back-End-Computeinstanz definiert. Somit ist dies auch der Standort, an dem die Datenverschiebung, Aktivitätsverteilung und SSIS-Paketausführung durchgeführt werden. Der Integrationslaufzeit-Standort kann sich vom Standort der Data Factory unterscheiden, zu der er gehört. 

### <a name="azure-ir-location"></a>Azure-Integrationslaufzeit: Standort
Wenn Sie für eine Azure-Integrationslaufzeit einen bestimmten Standort festlegen, werden Datenverschiebungen und Aktivitätsverteilungen in der entsprechenden Region durchgeführt. 

Bei Verwendung der **Azure-Integrationslaufzeit mit automatischer Auflösung** (Standardeinstellung) gilt Folgendes: 

- Bei Kopieraktivitäten versucht ADF, Ihre Senke und Ihren Quelldatenspeicher automatisch zu erkennen und den bestmöglichen Ort zu wählen – entweder in der gleichen Region (sofern verfügbar) oder in der nächstgelegenen Region im gleichen geografischen Gebiet. Ist keine Erkennung möglich, wird alternativ die Data Factory-Region verwendet.

- ADF verwendet die IR in der Data Factory-Region für die Ausführung der Aktivitäten Lookup/GetMetadata/Delete (auch als Pipelineaktivitäten bezeichnet), die Bereitstellung von Transformationsaktivitäten (auch als externe Aktivitäten bezeichnet) und die Erstellung von Vorgängen (Verbindung testen, Ordner- und Tabellenliste durchsuchen, Daten als Vorschau anzeigen).

- Für den Datenfluss verwendet ADF die IR in der Data Factory-Region. 

  > [!TIP] 
  > Eine bewährte Methode wäre, sicherzustellen, dass der Datenfluss (sofern möglich) in der gleichen Region wie Ihre entsprechenden Datenspeicher ausgeführt wird. Sie können dies entweder durch die automatische Auflösung der Azure IR-Instanz (falls der Datenspeicherort mit dem Data Factory-Standort identisch ist) oder das Erstellen einer neuen Azure-IR-Instanz, die sich in der gleichen Region wie Ihre Datenspeicher befindet, und anschließendes Ausführen des Datenflusses erreichen. 

In der Überwachungsansicht für Pipelineaktivitäten (auf der Benutzeroberfläche) oder über die Nutzlast für die Aktivitätsüberwachung können Sie überwachen, welcher Integrationslaufzeit-Standort bei der Aktivitätsausführung verwendet wird.

>[!TIP]
>Wenn Ihre Daten strengen Complianceanforderungen unterliegen und ein bestimmtes geografisches Gebiet nicht verlassen dürfen, können Sie explizit eine Azure-Integrationslaufzeit in einer bestimmten Region erstellen und den verknüpften Dienst mithilfe der ConnectVia-Eigenschaft auf diese Integrationslaufzeit verweisen. Ein Beispiel: Angenommen, Sie möchten Daten aus einem Blob in der Region „Vereinigtes Königreich, Süden“ in SQL Data Warehouse in der Region „Vereinigtes Königreich, Süden“ kopieren und dabei sicherstellen, dass die Daten das Vereinigte Königreich nicht verlassen. In diesem Fall können Sie eine Azure-Integrationslaufzeit in der Region „Vereinigtes Königreich, Süden“ erstellen und beide verknüpften Dienste mit dieser Integrationslaufzeit verknüpfen.

### <a name="self-hosted-ir-location"></a>Selbstgehostete Integrationslaufzeit: Standort
Die selbstgehostete Integrationslaufzeit wird logisch unter der Data Factory registriert, und die Computekomponente, mit der die Funktionen unterstützt werden, wird von Ihnen bereitgestellt. Aus diesem Grund ist für die selbstgehostete Integrationslaufzeit keine explizite Standorteigenschaft vorhanden. 

Bei Verwendung zum Durchführen der Datenverschiebung extrahiert die selbstgehostete Integrationslaufzeit Daten aus der Quelle und schreibt sie an das Ziel.

### <a name="azure-ssis-ir-location"></a>Azure-SSIS-Integrationslaufzeit: Standort
Die Auswahl des richtigen Standorts für Ihre Azure-SSIS-Integrationslaufzeit ist entscheidend, um für Ihre ETL-Workflows (Extrahieren-Transformieren-Laden) eine hohe Leistung zu erzielen.

- Der Standort von Azure-SSIS IR muss nicht dem Standort Ihrer Data Factory entsprechen. Es sollte aber derselbe Standort wie für Ihren eigenen Server mit Azure SQL-Datenbank bzw. einer verwalteten Instanz sein, auf dem die SSISDB gehostet werden soll. Ihre Azure-SSIS-Integrationslaufzeit kann dann leicht auf SSISDB zugreifen, ohne dass es zwischen unterschiedlichen Standorten zu übermäßig viel Datenverkehr kommt.
- Wenn Sie nicht über einen Server mit Azure SQL-Datenbank bzw. mit einer verwalteten Instanz zum Hosten der SSISDB verfügen, aber über lokale Datenquellen bzw. -ziele, sollten Sie wie folgt vorgehen: Erstellen Sie einen neuen Server mit Azure SQL-Datenbank bzw. mit einer verwalteten Instanz am Standort eines virtuellen Netzwerks, das mit Ihrem lokalen Netzwerk verbunden ist.  Auf diese Weise können Sie am gleichen Ort Azure-SSIS IR mit dem neuen Server mit Azure SQL-Datenbank bzw. mit einer verwalteten Instanz erstellen und den Beitritt zu diesem virtuellen Netzwerk durchführen. Dies führt effektiv zu einer Verringerung der Datenverschiebungen zwischen unterschiedlichen Standorten.
- Wenn der Standort Ihres vorhandenen Servers mit Azure SQL-Datenbank bzw. mit einer verwalteten Instanz, auf dem die SSISDB gehostet wird, nicht dem Standort eines virtuellen Netzwerks entspricht, das mit Ihrem lokalen Netzwerk verbunden ist, sollten Sie wie folgt vorgehen: Erstellen Sie zuerst Azure-SSIS IR, indem Sie einen vorhandenen Server mit Azure SQL-Datenbank bzw. mit einer verwalteten Instanz verwenden und den Beitritt zu einem anderen virtuellen Netzwerk am gleichen Ort durchführen, und konfigurieren Sie dann eine VNET-zu-VNET-Verbindung zwischen unterschiedlichen Standorten.

Im folgenden Diagramm sind die Standorteinstellungen von Data Factory und die dazugehörigen Integrationslaufzeiten dargestellt:

![Ort der Integrationslaufzeit](media/concepts-integration-runtime/integration-runtime-location.png)

## <a name="determining-which-ir-to-use"></a>Ermitteln der richtigen Integrationslaufzeit

### <a name="copy-activity"></a>Copy-Aktivität

Für die Kopieraktivität sind hierbei verknüpfte Quellen- und Senkendienste zum Definieren der Datenflussrichtung erforderlich. Anhand der folgenden Logik wird ermittelt, welche Integrationslaufzeit-Instanz zum Durchführen des Kopiervorgangs verwendet wird: 

- **Kopieren zwischen zwei Clouddatenquellen**: Wenn sowohl der verknüpfte Quelldienst als auch der verknüpfte Senkendienst die Azure-Integrationslaufzeit nutzt, verwendet ADF die regionale Azure-Integrationslaufzeit (sofern angegeben) oder bestimmt automatisch einen Ort der Azure-Integrationslaufzeit, sofern Sie wie unter [Ort der Integrationslaufzeit](#integration-runtime-location) beschrieben die Integrationslaufzeit mit automatischer Auflösung (Standardeinstellung) ausgewählt haben.
- **Kopieren zwischen einer Clouddatenquelle und einer Datenquelle im privaten Netzwerk**: Wenn entweder der verknüpfte Quellen- oder Senkendienst auf eine selbstgehostete Integrationslaufzeit zeigt, wird die Kopieraktivität unter dieser selbstgehosteten Integrationslaufzeit ausgeführt.
- **Kopieren zwischen zwei Datenquellen im privaten Netzwerk**: Sowohl der verknüpfte Quelldienst als auch der verknüpfte Senkendienst muss auf die gleiche Integrationslaufzeit verweisen, und diese Integrationslaufzeit wird zum Ausführen der Kopieraktivität verwendet.

### <a name="lookup-and-getmetadata-activity"></a>Lookup-/GetMetadata-Aktivität

Die Lookup-/GetMetadata-Aktivität wird für die Integrationslaufzeit ausgeführt, die dem verknüpften Datenspeicherdienst zugeordnet ist.

### <a name="transformation-activity"></a>Transformationsaktivität

Jede Transformationsaktivität verfügt über einen verknüpften Zielcomputedienst, der auf eine Integrationslaufzeit verweist. Über diese Instanz der Integrationslaufzeit wird die Transformationsaktivität bereitgestellt.

### <a name="data-flow-activity"></a>Datenflussaktivität

Die Datenflussaktivität wird auf der ihr zugeordneten Integration Runtime ausgeführt. 

## <a name="next-steps"></a>Nächste Schritte
Entsprechende Informationen finden Sie in den folgenden Artikeln:

- [Erstellen einer Azure Integration Runtime-Instanz](create-azure-integration-runtime.md)
- [Create self-hosted integration runtime](create-self-hosted-integration-runtime.md) (Erstellen einer selbstgehosteten Integrationslaufzeit)
- [Erstellen einer Azure-SSIS Integration Runtime in Azure Data Factory | Microsoft-Dokumentation](create-azure-ssis-integration-runtime.md). In diesem Artikel wird das Tutorial vertieft, und er enthält Anweisungen zur Verwendung einer verwalteten Azure SQL-Datenbank-Instanz und zum Verknüpfen der IR mit einem virtuellen Netzwerk. 
