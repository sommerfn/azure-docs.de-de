---
title: Verwenden von Azure Data Factory zum Migrieren von Daten von einem lokalen Hadoop-Cluster zu Azure Storage | Microsoft-Dokumentation
description: Verwenden Sie Azure Data Factory, um Daten von einem lokalen Hadoop-Cluster zu Azure Storage zu migrieren.
services: data-factory
documentationcenter: ''
author: dearandyxu
ms.author: yexu
ms.reviewer: ''
manager: ''
ms.service: data-factory
ms.workload: data-services
ms.tgt_pltfrm: na
ms.topic: conceptual
ms.date: 8/30/2019
ms.openlocfilehash: 1b26b22fa9cdf9f6671702ceb9640aa39a6ecf17
ms.sourcegitcommit: 8fea78b4521921af36e240c8a92f16159294e10a
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 09/02/2019
ms.locfileid: "70211597"
---
# <a name="use-azure-data-factory-to-migrate-data-from-on-premises-hadoop-cluster-to-azure-storage"></a>Verwenden von Azure Data Factory zum Migrieren von Daten von einem lokalen Hadoop-Cluster zu Azure Storage 

Azure Data Factory stellt einen leistungsstarken, stabilen und kostengünstigen Mechanismus bereit, um Daten bedarfsabhängig von lokalem HDFS zu Azure Blob Storage oder Azure Data Lake Storage Gen2 zu migrieren. Grundsätzlich bietet Azure Data Factory zwei Methoden, um Daten von lokalem HDFS zu Azure zu migrieren. Sie können diese basierend auf Ihrem Szenario auswählen. 

- DistCp-Modus von ADF. Statt die selbstgehostete Integration Runtime (IR) auszuführen, unterstützt ADF die Verwendung von [DistCP](https://hadoop.apache.org/docs/current3/hadoop-distcp/DistCp.html), um Dateien unverändert in Azure Blob (einschließlich [gestaffeltem Kopieren](https://docs.microsoft.com/azure/data-factory/copy-activity-performance#staged-copy)) oder Azure Data Lake Store zu kopieren, sodass Ihr Cluster optimal zum Einsatz kommt. Dies bietet einen besseren Kopierdurchsatz, insbesondere dann, wenn Sie über einen äußerst leistungsfähigen Cluster verfügen. Basierend auf Ihrer Konfiguration in Azure Data Factory erstellt die Kopieraktivität automatisch einen DistCp-Befehl, sendet diesen an Ihren Hadoop-Cluster und überwacht den Kopierstatus. Durch die Verwendung von ADF, das in DistCp integriert ist, können Kunden nicht nur den vorhandenen leistungsfähigen Cluster nutzen, um einen optimalen Kopierdurchsatz zu erzielen, sondern auch den Vorteil einer flexiblen Planung und einheitlichen Überwachung von ADF nutzen. Standardmäßig wird der DistCp-Modus von ADF zum Migrieren von Daten von einem lokalen Hadoop-Cluster zu Azure empfohlen.
- Systemeigener IR-Modus von ADF. In einigen Szenarien kann DistCp nicht verwendet werden (z.B. werden in einer VNET-Umgebung das private Peering von ExpressRoute und der VNET-Endpunkt von Azure Storage nicht vom DistCp-Tool unterstützt). Manchmal soll auch nicht der vorhandene Hadoop-Cluster als Engine zum Migrieren der Daten verwendet werden, da sich dadurch hohe Lasten für den Cluster ergeben und sich dies auf die Leistung vorhandener ETL-Aufträge auswirken kann. In diesem Fall können Sie die systemeigene Funktion von ADF verwenden, bei der die ADF-Integration Runtime (IR) die Engine zum Kopieren der Daten von lokalem HDFS nach Azure sein kann.

Dieser Artikel enthält für Datentechniker und Entwickler Informationen zu beiden Methoden für die folgenden Bereiche:
> [!div class="checklist"]
> * Leistung 
> * Resilienz beim Kopieren
> * Netzwerksicherheit
> * Allgemeine Lösungsarchitektur 
> * Bewährte Methoden für die Implementierung  

## <a name="performance"></a>Leistung

Im DistCp-Modus von ADF entspricht der Durchsatz einer unabhängigen Verwendung des DistCp-Tools, wobei die Kapazität Ihres vorhandenen Hadoop-Clusters genutzt wird. DistCp (Distributed Copy, verteiltes Kopieren) ist ein Tool, das für umfangreiche Kopiervorgänge innerhalb und zwischen Clustern verwendet wird. Es verwendet MapReduce, um die Verteilung, die Fehlerbehandlung und Wiederherstellung sowie die Berichterstellung zu handhaben. Das Tool erweitert eine Liste von Dateien und Verzeichnissen in eine Eingabe für Zuordnungsaufgaben, mit denen jeweils eine Partition von Dateien kopiert wird, die in der Quellliste angegeben sind. Durch die Verwendung von ADF mit Integration in DistCp können Sie Pipelines erstellen, um Ihre Netzwerkbandbreite sowie IOPS und Bandbreite des Speichers vollständig zu nutzen und so den Datenverschiebungsdurchsatz in Ihrer Umgebung zu maximieren.  

Im systemeigenen IR-Modus von ADF ist außerdem Parallelität auf unterschiedlichen Ebenen möglich, wodurch Ihre Netzwerkbandbreite sowie IOPS und Bandbreite des Speichers vollständig genutzt werden können, um den Datenverschiebungsdurchsatz zu maximieren. Dazu wird der selbstgehostete IR-Computer manuell zentral hochskaliert oder das horizontale Hochskalieren auf mehrere selbstgehostete IR-Computer durchgeführt.

- Eine einzelne Kopieraktivität kann skalierbare Computeressourcen nutzen. Bei der selbstgehosteten Integration Runtime können Sie den Computer manuell zentral hochskalieren oder das horizontale Hochskalieren auf mehrere Computer durchführen ([bis zu vier Knoten](https://docs.microsoft.com/azure/data-factory/create-self-hosted-integration-runtime#high-availability-and-scalability)). Bei einer einzelnen Kopieraktivität wird die Dateigruppe dann auf alle Knoten verteilt. 
- Für eine Kopieraktivität werden mehrere Threads genutzt, um für den Datenspeicher Lese- und Schreibvorgänge durchzuführen. 
- Mit der ADF-Ablaufsteuerung können mehrere Kopieraktivitäten parallel gestartet werden, z. B. per [foreach-Schleife](https://docs.microsoft.com/azure/data-factory/control-flow-for-each-activity). 

Weitere Informationen finden Sie im [Handbuch zur Leistung der Kopieraktivität](https://docs.microsoft.com/azure/data-factory/copy-activity-performance).

## <a name="resilience"></a>Resilienz

Im DistCp-Modus von ADF können Sie verschiedene DistCp-Befehlszeilenparameter nutzen (z.B. „-i“: Fehler ignorieren, „-update“: Daten schreiben, wenn Quelldatei und Zieldatei eine unterschiedliche Größe aufweisen), um unterschiedliche Resilienzstufen zu erzielen.

Im systemeigenen IR-Modus verfügt ADF bei Ausführung einer einzelnen Kopieraktivität über einen integrierten Wiederholungsmechanismus, damit vorübergehende Fehler in den Datenspeichern oder im zugrunde liegenden Netzwerk bis zu einem gewissen Grad gehandhabt werden können. Beim Durchführen von binären Kopiervorgängen von lokalem HDFS zu Blob Storage und von lokalem HDFS zu ADLS Gen2 werden von ADF automatisch Prüfpunkte in großem Umfang gesetzt. Wenn bei der Ausführung einer Kopieraktivität ein Fehler oder Timeout aufgetreten ist, wird der Vorgang bei einer nachfolgenden Wiederholung (Wiederholungsanzahl > 1) ab dem letzten Fehlerpunkt fortgesetzt und nicht wieder ab dem Anfang gestartet.

## <a name="network-security"></a>Netzwerksicherheit 

Standardmäßig werden Daten mit ADF über eine verschlüsselte Verbindung per HTTPS-Protokoll von lokalem HDFS zu Azure Blob Storage oder Azure Data Lake Storage Gen2 übertragen.  HTTPS ermöglicht die Datenverschlüsselung während der Übertragung und verhindert Abhör- und Man-in-the-Middle-Angriffe. 

Falls Sie nicht möchten, dass Daten über das öffentliche Internet übertragen werden, können Sie auch eine bessere Sicherheit erzielen, indem Sie Daten über einen privaten Peeringlink per Azure ExpressRoute übertragen. Unten im Abschnitt zur Lösungsarchitektur ist beschrieben, wie Sie dies erreichen.

## <a name="solution-architecture"></a>Lösungsarchitektur

Migrieren von Daten über das öffentliche Internet:

![solution-architecture-public-network](media/data-migration-guidance-hdfs-to-azure-storage/solution-architecture-public-network.png)

- Bei dieser Architektur werden Daten auf sichere Weise per HTTPS über das öffentliche Internet übertragen. 
- Der DistCp-Modus von ADF wird für die Verwendung in einer öffentlichen Netzwerkumgebung empfohlen. Dadurch können Sie nicht nur den vorhandenen leistungsfähigen Cluster nutzen, um einen optimalen Kopierdurchsatz zu erzielen, sondern auch den Vorteil einer flexiblen Planung und einheitlichen Überwachung von ADF nutzen.
- Sie müssen die selbstgehostete Integration Runtime von ADF auf einem Windows-Computer hinter der Unternehmensfirewall installieren, damit der DistCp-Befehl an Ihren Hadoop-Cluster gesendet und der Kopierstatus überwacht werden kann. Wenn dieser Computer nicht die Engine zum Verschieben von Daten ist (nur zur Steuerung), wirkt sich die Kapazität des Computers nicht auf den Durchsatz der Datenverschiebung aus.
- Die vorhandenen Parameter aus dem DistCp-Befehl werden unterstützt.


Migrieren von Daten über einen privaten Link: 

![solution-architecture-private-network](media/data-migration-guidance-hdfs-to-azure-storage/solution-architecture-private-network.png)

- Bei dieser Architektur wird die Datenmigration über einen privaten Peeringlink per Azure ExpressRoute durchgeführt, damit Daten niemals über das öffentliche Internet übertragen werden.
- Das DistCp-Tool bietet keine Unterstützung für das private Peering von ExpressRoute und den VNET-Endpunkt von Azure Storage. Daher wird empfohlen, die systemeigene Funktion von ADF über Integration Runtime zum Migrieren der Daten zu verwenden.
- Sie müssen die selbstgehostete Integration Runtime von ADF auf einer Windows-VM in Ihrem virtuellen Azure-Netzwerk installieren, um diese Architektur zu erhalten. Sie können Ihre VM manuell zentral hochskalieren oder das horizontale Hochskalieren auf mehrere VMs durchführen, um IOPS und Bandbreite für Netzwerk und Speicher vollständig zu nutzen.
- Die empfohlene Konfiguration, mit der für jede Azure-VM (mit installierter selbstgehosteter Integration Runtime von ADF) begonnen werden sollte, ist „Standard_D32s_v3“ mit 32 vCPUs und 128 GB Arbeitsspeicher. Sie können die Auslastung der CPU und des Arbeitsspeichers für die VM während der Datenmigration weiter überwachen. So können Sie ermitteln, ob Sie die VM weiter zentral hochskalieren müssen, um die Leistung zu verbessern, oder zentral herunterskalieren müssen, um Kosten zu sparen.
- Sie können auch horizontal hochskalieren, indem Sie bis zu vier VM-Knoten einer selbstgehosteten Integration Runtime zuordnen. Bei einem einzelnen Kopierauftrag, der für eine selbstgehostete IR ausgeführt wird, wird die Dateigruppe automatisch partitioniert, und alle VM-Knoten werden genutzt, um die Dateien parallel zu kopieren. Zur Sicherstellung von Hochverfügbarkeit ist es ratsam, mit zwei VM-Knoten zu beginnen, um bei der Datenmigration einen Single Point of Failure zu vermeiden.
- Mit dieser Architektur ist sowohl die Migration der Daten einer Anfangsmomentaufnahme als auch der Deltadaten möglich.


## <a name="implementation-best-practices"></a>Bewährte Methoden für die Implementierung 

### <a name="authentication-and-credential-management"></a>Authentifizierung und Verwaltung von Anmeldeinformationen 

- Zum Authentifizieren bei HDFS können Sie [entweder „Windows“ (Kerberos) oder „Anonym“](https://docs.microsoft.com/azure/data-factory/connector-hdfs#linked-service-properties) verwenden. 
- Für die Verbindungsherstellung mit Azure Blob Storage werden mehrere Authentifizierungstypen unterstützt.  Die Verwendung von [verwalteten Identitäten für Azure-Ressourcen](https://docs.microsoft.com/azure/data-factory/connector-azure-blob-storage#managed-identity) wird dringend empfohlen: Sie basieren auf einer automatisch verwalteten ADF-Identität in Azure AD und ermöglichen Ihnen die Konfiguration von Pipelines, ohne dass in der Definition des verknüpften Diensts Anmeldeinformationen angegeben werden müssen.  Alternativ können Sie die Authentifizierung für Azure Blob Storage auch per [Dienstprinzipal](https://docs.microsoft.com/azure/data-factory/connector-azure-blob-storage#service-principal-authentication), [Shared Access Signature](https://docs.microsoft.com/azure/data-factory/connector-azure-blob-storage#shared-access-signature-authentication) oder [Speicherkontoschlüssel](https://docs.microsoft.com/azure/data-factory/connector-azure-blob-storage#account-key-authentication) durchführen. 
- Auch für die Verbindungsherstellung mit Azure Data Lake Storage Gen2 werden mehrere Authentifizierungstypen unterstützt.  Die Verwendung von [verwalteten Identitäten für Azure-Ressourcen](https://docs.microsoft.com/azure/data-factory/connector-azure-data-lake-storage#managed-identity) wird dringend empfohlen. Es können aber auch ein [Dienstprinzipal](https://docs.microsoft.com/azure/data-factory/connector-azure-data-lake-storage#service-principal-authentication) oder ein [Speicherkontoschlüssel](https://docs.microsoft.com/azure/data-factory/connector-azure-data-lake-storage#account-key-authentication) verwendet werden. 
- Wenn Sie keine verwalteten Identitäten für Azure-Ressourcen nutzen, ist das [Speichern der Anmeldeinformationen in Azure Key Vault](https://docs.microsoft.com/azure/data-factory/store-credentials-in-key-vault) dringend zu empfehlen, um das zentrale Verwalten und Rotieren von Schlüsseln ohne Änderung der verknüpften ADF-Dienste zu vereinfachen.  Dies ist auch eine der [bewährten Methoden für CI/CD](https://docs.microsoft.com/azure/data-factory/continuous-integration-deployment#best-practices-for-cicd). 

### <a name="initial-snapshot-data-migration"></a>Migration der Daten der Anfangsmomentaufnahme 

Im DistCp-Modus von ADF können Sie eine Kopieraktivität erstellen, um den DistCp-Befehl mit unterschiedlichen Parametern zu übermitteln und so das anfängliche Datenmigrationsverhalten zu steuern. 

Im systemeigenen IR-Modus von ADF ist die Datenpartition besonders zu empfehlen, wenn mehr als 10 TB an Daten migriert werden. Zum Partitionieren der Daten verwenden Sie die Ordnernamen in HDFS. Anschließend kann jeder ADF-Kopierauftrag jeweils eine Ordnerpartition kopieren. Sie können mehrere ADF-Kopieraufträge gleichzeitig ausführen, um einen besseren Durchsatz zu erzielen.
Falls Kopieraufträge aufgrund eines vorübergehenden Problems mit dem Netzwerk oder dem Datenspeicher nicht erfolgreich sind, können Sie den fehlerhaften Kopierauftrag erneut ausführen, um diese spezifische Partition erneut aus HDFS zu laden. Alle anderen Kopieraufträge, bei denen andere Partitionen geladen werden, werden nicht beeinträchtigt.

### <a name="delta-data-migration"></a>Migration von Deltadaten 

Im DistCp-Modus von ADF können Sie DistCp-Befehlszeilenparameter nutzen („-update“: Daten schreiben, wenn Quelldatei und Zieldatei eine unterschiedliche Größe aufweisen), um eine Migration von Deltadaten zu erzielen.

Im systemeigenen IR-Modus von ADF ist die beste Möglichkeit zum Identifizieren von neuen oder geänderten Dateien aus HDFS die Verwendung der Namenskonvention mit Zeitpartitionierung. Wenn für Ihre Daten in HDFS die Zeitpartitionierung mit Zeitrauminformationen im Datei- oder Ordnernamen durchgeführt wurde (z.B. „/jjjj/mm/tt/file.csv), kann Ihre Pipeline leicht ermitteln, welche Dateien bzw. Ordner inkrementell kopiert werden müssen.
Wenn Ihre Daten in HDFS nicht über eine Zeitpartitionierung verfügen, kann ADF neue oder geänderte Dateien anhand des letzten Änderungsdatums (LastModifiedDate) identifizieren. Hierbei scannt ADF alle Dateien aus HDFS und kopiert nur die neuen und aktualisierten Dateien, deren Zeitstempel der letzten Änderung größer als ein bestimmter Wert ist. Beachten Sie Folgendes, wenn Sie in HDFS über eine große Zahl von Dateien verfügen: Der erste Scanvorgang für die Dateien kann unabhängig davon, wie viele Dateien die Filterbedingung erfüllen, sehr lange dauern. In diesem Fall sollten Sie die Daten zuerst partitionieren, indem Sie dieselbe Partition für die Migration der Anfangsmomentaufnahme verwenden, damit der Scanvorgang für die Dateien parallel erfolgen kann.

### <a name="estimating-price"></a>Schätzen des Preises 

Sehen Sie sich die folgende Pipeline für die Migration von Daten aus HDFS zu Azure Blob Storage an: 

![pricing-pipeline](media/data-migration-guidance-hdfs-to-azure-storage/pricing-pipeline.png)

Wir nehmen Folgendes an: 

- Das gesamte Datenvolumen beträgt 1 PB.
- Das Migrieren der Daten erfolgt mithilfe der zweiten Lösungsarchitektur (systemeigener IR-Modus von ADF).
- Die 1 PB sind in 1000 Partitionen unterteilt, und bei jedem Kopiervorgang wird eine Partition verschoben.
- Jede Kopieraktivität ist mit einer selbstgehosteten IR konfiguriert, die vier Computern zugeordnet ist, und erzielt einen Durchsatz von 500 MBit/s.
- Die ForEach-Parallelität ist auf 4 festgelegt, und der aggregierte Durchsatz beträgt 2 GBit/s.
- Insgesamt dauert es 146 Stunden, bis die Migration abgeschlossen ist.


Hier ist der geschätzte Preis basierend auf den obigen Annahmen angegeben: 

![pricing-table](media/data-migration-guidance-hdfs-to-azure-storage/pricing-table.png)

> [!NOTE]
> Dies ist ein hypothetisches Preisbeispiel.  Der tatsächliche Preis richtet sich nach dem tatsächlichen Durchsatz in Ihrer Umgebung.
> Der Preis für die Azure Windows-VM (mit installierter selbstgehosteter Integration Runtime) ist nicht enthalten.

### <a name="additional-references"></a>Zusätzliche Verweise 
- [HDFS-Connector](https://docs.microsoft.com/azure/data-factory/connector-hdfs)
- [Azure Blob Storage-Connector](https://docs.microsoft.com/azure/data-factory/connector-azure-blob-storage)
- [Azure Data Lake Storage Gen2-Connector](https://docs.microsoft.com/azure/data-factory/connector-azure-data-lake-storage)
- [Handbuch zur Leistung und Optimierung der Kopieraktivität](https://docs.microsoft.com/azure/data-factory/copy-activity-performance)
- [Erstellen und Konfigurieren einer selbstgehosteten Integration Runtime](https://docs.microsoft.com/azure/data-factory/create-self-hosted-integration-runtime)
- [Selbstgehostete Integration Runtime: Hochverfügbarkeit und Skalierbarkeit](https://docs.microsoft.com/azure/data-factory/create-self-hosted-integration-runtime#high-availability-and-scalability)
- [Sicherheitsüberlegungen für Datenverschiebung in Azure Data Factory](https://docs.microsoft.com/azure/data-factory/data-movement-security-considerations)
- [Speichern von Anmeldeinformationen in Azure Key Vault](https://docs.microsoft.com/azure/data-factory/store-credentials-in-key-vault)
- [Inkrementelles Kopieren neuer Dateien basierend auf dem zeitpartitionierten Dateinamen und mithilfe des Tools „Daten kopieren“](https://docs.microsoft.com/azure/data-factory/tutorial-incremental-copy-partitioned-file-name-copy-data-tool)
- [Inkrementelles Kopieren neuer und geänderter Dateien auf Basis von LastModifiedDate und mithilfe des Tools zum Kopieren von Daten](https://docs.microsoft.com/azure/data-factory/tutorial-incremental-copy-lastmodified-copy-data-tool)
- [Azure Data Factory: Datenpipelines – Preise](https://azure.microsoft.com/pricing/details/data-factory/data-pipeline/)

## <a name="next-steps"></a>Nächste Schritte

- [Kopieren von Dateien aus mehreren Containern mit Azure Data Factory](solution-template-copy-files-multiple-containers.md)