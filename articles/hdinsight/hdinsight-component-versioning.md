---
title: Apache Hadoop-Komponenten und -Versionen – Azure HDInsight
description: Lernen Sie die Apache Hadoop-Komponenten und -Versionen in Azure HDInsight kennen.
keywords: Hadoop-Versionen, Komponenten des Hadoop-Ökosystems, Hadoop-Komponenten, Hadoop-Version überprüfen
author: hrasheed-msft
ms.author: hrasheed
ms.reviewer: jasonh
ms.service: hdinsight
ms.custom: hdinsightactive,hdiseo17may2017
ms.topic: conceptual
ms.date: 11/13/2019
ms.openlocfilehash: a5632f4b7ca264f6fdeb88dc7f7cf843c73ddf54
ms.sourcegitcommit: 5a8c65d7420daee9667660d560be9d77fa93e9c9
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 11/15/2019
ms.locfileid: "74123404"
---
# <a name="what-are-the-apache-hadoop-components-and-versions-available-with-hdinsight"></a>Welche Apache Hadoop-Komponenten und -Versionen sind in HDInsight verfügbar?

Erfahren Sie mehr über die Komponenten und Versionen des [Apache Hadoop](https://hadoop.apache.org/)-Ökosystems in Microsoft Azure HDInsight sowie über das Enterprise Security Package. Darüber hinaus erfahren Sie, wie Sie Hadoop-Komponentenversionen in HDInsight überprüfen.

## <a name="apache-hadoop-components-available-with-different-hdinsight-versions"></a>Verfügbare Apache Hadoop-Komponenten in verschiedenen Versionen von HDInsight

Azure HDInsight unterstützt mehrere Hadoop-Clusterversionen, die jederzeit bereitgestellt werden können. Ab dem 4. April 2017 ist die von Azure HDInsight derzeit als Standardclusterversion verwendete Version die Version 3.6.

Die den HDInsight-Clusterversionen zugeordneten Komponentenversionen sind in der folgenden Tabelle aufgeführt: 

> [!NOTE]  
> Die Standardversion für den HDInsight-Dienst kann ohne vorherige Ankündigung geändert werden. Wenn eine Versionsabhängigkeit vorliegt, geben Sie die HDInsight-Version an, wenn Sie Ihre Cluster mit dem .NET SDK mit Azure PowerShell und der klassischen Azure-Befehlszeilenschnittstelle erstellen.

| Komponente | HDInsight 4.0 | HDInsight 3.6 (Standard) | HDInsight 3.5 | HDInsight 3.4 | HDInsight 3.3 | HDInsight 3.2 |
|---------------------------|---------------|-----------------------------|---------------|---------------|---------------|----------------------|
| Apache Hadoop und YARN | 3.1.1 | 2.7.3 | 2.7.3 | 2.7.1 | 2.7.1 | 2.6.0 |
| Apache Tez | 0.9.1 | 0.7.0 | 0.7.0 | 0.7.0 | 0.7.0 | 0.5.2 |
| Apache Pig | 0.16.0 | 0.16.0 | 0.16.0 | 0.15.0 | 0.15.0 | 0.14.0 |
| Apache Hive | 3.1.0 | 2.1.0, 1.2.1 | - | - | - | - |
| Apache Tez Hive2 | - | 0.8.4 | - | - | - | - |
| Apache Ranger | 1.1.0 | 0.7.0 | 0.6.0 | - | - | - |
| Apache HBase | 2.0.1 | 1.1.2 | 1.1.2 | 1.1.2 | 1.1.1 | 0.98.4 |
| Apache Sqoop | 1.4.7 | 1.4.6 | 1.4.6 | 1.4.6 | 1.4.6 | 1.4.5 |
| Apache Oozie | 4.3.1 | 4.2.0 | 4.2.0 | 4.2.0 | 4.2.0 | 4.1.0 |
| Apache Zookeeper | 3.4.6 | 3.4.6 | 3.4.6 | 3.4.6 | 3.4.6 | 3.4.6 |
| Apache Storm | - | 1.1.0 | 1.0.1 | 0.10.0 | 0.10.0 | 0.9.3 |
| Apache Mahout | - | 0.9.0+ | 0.9.0+ | 0.9.0+ | 0.9.0+ | 0.9.0 |
| Apache Phoenix | 5 | 4.7.0 | 4.7.0 | 4.4.0 | 4.4.0 | 4.2.0 |
| Apache Spark | 2.3.1, 2.4 | 2.3.0, 2.2.0, 2.1.0 | 1.6.2, 2.0 | 1.6.0 | 1.5.2 | 1.3.1 (nur für Windows) |
| Apache Livy | 0,5 | 0.4, 0.4, 0.3 | 0,3 | 0,3 | 0.2 | - |
| Apache Kafka | 1.1.1, 2.1 | 1.1, 1.0 * (Siehe Hinweis unten) | 0.10.0 | 0.9.0 | - | - |
| Apache Ambari | 2.7.0 | 2.6.0 | 2.4.0 | 2.2.1 | 2.1.0 | - |
| Apache Zeppelin | 0.8.0 | 0.7.0 | - | - | - | - |
| Mono | 4.2.1 | 4.2.1 | 4.2.1 | 3.2.8 | - | - |

> [!NOTE]
> Aufgrund von Überlegungen zur Systemleistung ist die Unterstützung für Kafka Version 0.10 im März 2019 abgelaufen.

## <a name="check-for-current-hadoop-component-version-information"></a>Überprüfen der Informationen zur aktuellen Hadoop-Komponentenversion

Die mit den HDInsight-Clusterversionen verknüpften Komponentenversionen des Hadoop-Ökosystems können sich bei Updates für HDInsight ändern. Um die Hadoop-Komponenten zu überprüfen und zu ermitteln, welche Versionen für einen Cluster verwendet werden, verwenden Sie die Ambari-REST-API. Mit dem Befehl **GetComponentInformation** rufen Sie Informationen zu Dienstkomponenten ab. Ausführliche Informationen finden Sie in der [Apache Ambari-Dokumentation](https://github.com/apache/ambari/blob/trunk/ambari-server/docs/api/v1/index.md).

### <a name="release-notes"></a>Versionshinweise

In den [HDInsight-Versionshinweisen](hdinsight-release-notes.md) finden Sie zusätzliche Anmerkungen zu den aktuellen Versionen von HDInsight.

## <a name="supported-hdinsight-versions"></a>Unterstützte HDInsight-Versionen

In den folgenden Tabellen sind die Versionen von HDInsight aufgeführt. Der Ablauf der Unterstützung und die Deaktivierungstermine werden ebenfalls bereitgestellt, wenn sie bekannt sind.

### <a name="available-versions"></a>Verfügbare Versionen

Die folgende Tabelle enthält die im Azure-Portal verfügbaren Versionen von HDInsight sowie andere Bereitstellungsmethoden wie z.B. PowerShell und .NET SDK.

| HDInsight-Version | Betriebssystem des virtuellen Computers | Herausgabedatum | Supportablaufdatum | Deaktivierungstermin | Hochverfügbarkeit |  Verfügbarkeit im Azure-Portal | 
| --- | --- | --- | --- | --- | --- | --- |
| HDInsight 4.0 |Ubuntu 16.0.4 LTS |24. September 2018 | | |Ja |Ja |
| HDInsight 3.6 |Ubuntu 16.0.4 LTS |4\. April 2017 | 30. Juni 2020 |31. Dezember 2020 |Ja |Ja |


> [!NOTE]  
> Nachdem die Unterstützung für eine Version abgelaufen ist, ist sie möglicherweise nicht mehr über das Microsoft Azure-Portal verfügbar. Clusterversionen stehen jedoch bis zum Zeitpunkt ihrer Deaktivierung weiterhin über den Parameter `Version` im Windows PowerShell-Befehl [New-AzHDInsightCluster](https://docs.microsoft.com/powershell/module/az.hdinsight/new-azhdinsightcluster) und das .NET SDK zur Verfügung.
>

### <a name="retired-versions"></a>Eingestellte Versionen

Die folgende Tabelle enthält die Versionen von HDInsight, die derzeit **nicht** im Azure-Portal verfügbar sind.

| HDInsight-Version | HDP-Version | Betriebssystem des virtuellen Computers | Herausgabedatum | Supportablaufdatum | Deaktivierungstermin | Hochverfügbarkeit |  Die Verfügbarkeit im Azure-Portal | 
| --- | --- | --- | --- | --- | --- | --- | --- |
| HDInsight 3.5 |HDP 2.5 |Ubuntu 16.0.4 LTS |30. September 2016 |5\. September 2017 |28. Juni 2018 |Ja |Nein |
| HDInsight 3.4 |HDP 2.4 |Ubuntu 14.0.4 LTS |29. März 2016 |29. Dezember 2016 |9\. Januar 2018 |Ja |Nein |
| HDInsight 3.3 |HDP 2.3 |Windows Server 2012 R2 |2\. Dezember 2015 |27. Juni 2016 |31. Juli 2018 |Ja |Nein |
| HDInsight 3.3 |HDP 2.3 |Ubuntu 14.0.4 LTS |2\. Dezember 2015 |27. Juni 2016 |31. Juli 2017 |Ja |Nein |
| HDInsight 3.2 |HDP 2.2 |Ubuntu 12.04 LTS oder Windows Server 2012 R2 |18. Februar 2015 |1\. März 2016 |1\. April 2017 |Ja |Nein |
| HDInsight 3.1 |HDP 2,1 |Windows Server 2012 R2 |24. Juni 2014 |18. Mai 2015 |30. Juni 2016 |Ja |Nein |
| HDInsight 3.0 |HDP 2,0 |Windows Server 2012 R2 |11. Februar 2014 |17. September 2014 |30. Juni 2015 |Ja |Nein |
| HDInsight 2.1 |HDP 1,3 |Windows Server 2012 R2 |28. Oktober 2013 |12. Mai 2014 |31. Mai 2015 |Ja |Nein |
| HDInsight 1.6 |HDP 1.1 | |28. Oktober 2013 |26. April 2014 |31. Mai 2015 |Nein |Nein |

> [!NOTE]  
> Hochverfügbare Cluster mit zwei Hauptknoten werden standardmäßig für HDInsight Version 2.1 und höher bereitgestellt. Für HDInsight Version 1.6-Cluster sind sie nicht verfügbar.

## <a name="enterprise-security-package-for-hdinsight"></a>Enterprise Security Package für HDInsight

Enterprise Security für HDInsight ist ein optionales Paket, das Sie während der Clustererstellung zu Ihrem HDInsight-Cluster hinzufügen können. Das Enterprise Security Package unterstützt Folgendes:

- Integration in Active Directory für die Authentifizierung

    Bisher konnten Sie HDInsight-Cluster ausschließlich mit einem lokalen Administratorbenutzer oder einem lokalen SSH-Benutzer erstellen. Der lokale Administratorbenutzer kann auf alle Dateien, Ordner, Tabellen und Spalten zugreifen.  Mit dem Enterprise Security Package können Sie die rollenbasierte Zugriffssteuerung aktivieren, indem Sie HDInsight-Cluster in Ihr eigenes Active Directory integrieren, zu dem das lokale Active Directory, Azure Active Directory Domain Services oder Active Directory auf dem virtuellen IaaS-Computer gehören. Der Domänenadministrator des Clusters kann Benutzern die Berechtigung erteilen, ihren eigenen Firmen- oder Domänenbenutzernamen und ihr eigenes Kennwort für den Zugriff auf den Cluster zu verwenden. 

    Weitere Informationen finden Sie unter

    - [Einführung in die Apache Hadoop-Sicherheit mit in die Domäne eingebundenen HDInsight-Clustern](./domain-joined/hdinsight-security-overview.md)
    - [Planen von in die Azure-Domäne eingebundenen Apache Hadoop-Clustern in HDInsight](./domain-joined/apache-domain-joined-architecture.md)
    - [Konfigurieren einer in die Domäne eingebundenen Sandboxumgebung](./domain-joined/apache-domain-joined-configure.md)
    - [Konfigurieren von in die Domäne eingebundenen HDInsight-Clustern mit Azure Active Directory Domain Services](./domain-joined/apache-domain-joined-configure-using-azure-adds.md)

- Autorisierung für Daten

  - Integration in Apache Ranger für die Autorisierung für Hive-, Spark SQL- und Yarn-Warteschlangen
  - Sie können die Zugriffssteuerung für Dateien und Ordner festlegen.

    Weitere Informationen finden Sie unter

  - [Konfigurieren von Apache Hive-Richtlinien in HDInsight mit Domänenverknüpfung](./domain-joined/apache-domain-joined-run-hive.md)

- Anzeigen der Überwachungsprotokolle zum Überwachen der Zugriffe und konfigurierten Richtlinien 

### <a name="supported-cluster-types"></a>Unterstützte Clustertypen

Derzeit wird das Enterprise Security Package nur durch die folgenden Clustertypen unterstützt:

- Hadoop (nur HDInsight 3.6)
- Spark
- Interactive Query

### <a name="support-for-azure-data-lake-storage"></a>Unterstützung für Azure Data Lake Storage

Das Enterprise-Sicherheitspaket unterstützt die Verwendung von Azure Data Lake Storage sowohl als Primärspeicher als auch als Zusatzspeicher.

### <a name="pricing-and-service-level-agreement"></a>Preise und Vereinbarung zum Servicelevel (SLA)

Informationen zu Preisen und SLA für das Enterprise Security Package finden Sie unter [HDInsight-Preise](https://azure.microsoft.com/pricing/details/hdinsight/).


## <a name="service-level-agreement-for-hdinsight-cluster-versions"></a>Die Vereinbarung zum Servicelevel (Service Level Agreement, SLA) für HDInsight-Clusterversionen

Die Vereinbarung zum Servicelevel (Service Level Agreement, SLA) ist als _Supportfenster_ definiert. Ein Supportfenster ist ein Zeitraum, in dem eine HDInsight-Clusterversion vom Microsoft-Kundendienst und -Support unterstützt wird. Wenn die Version ein verstrichenes _Supportablaufdatum_ hat, befindet sich das HDInsight-Cluster außerhalb des Supportfensters. Das Supportablaufdatum für eine bestimmte HDInsight-Version X (nachdem eine neuere X+1-Version verfügbar ist) wird berechnet als das spätere Datum von:  

* Formel 1: Addieren Sie 180 Tage zum Releasedatum von HDInsight-Clusterversion X.
* Formel 2: Addieren Sie 90 Tage zum Releasedatum von HDInsight-Clusterversion X+1 im Azure-Portal.

Das _Deaktivierungsdatum_ ist das Datum, nach dem die Clusterversion nicht mehr in HDInsight erstellt werden kann. Ab dem 31. Juli 2017 kann die Größe eines HDInsight-Clusters nicht mehr geändert werden. 

> [!NOTE]  
> Windows-basierte HDInsight-Cluster (einschließlich Version 2.1, 3.0, 3.1, 3.2 und 3.3) laufen unter der Azure-Gast-BS-Familie Version 4, die die 64-Bit-Version von Windows Server 2012 R2 verwendet. Die Azure-Gast-BS-Familie Version 4 unterstützt die .NET Framework-Versionen 4.0, 4.5, 4.5.1 und 4.5.2.

## <a name="hortonworks-release-notes-associated-with-hdinsight-versions"></a>Hortonworks-Versionshinweise im Zusammenhang mit HDInsight-Versionen

Dieser Abschnitt enthält Links zu den Anmerkungen zu dieser Version für Hortonworks Data Platform-Verteilungen und den Apache-Komponenten, die mit HDInsight verwendet werden.
* HDInsight-Clusterversion 4.0 verwendet eine Hadoop-Distribution, die auf [Hortonworks Data Platform 3.0](https://docs.hortonworks.com/HDPDocuments/HDP3/HDP-3.0.0/release-notes/content/relnotes.html) basiert.
* HDInsight-Clusterversion 3.6 verwendet eine Hadoop-Distribution, die auf [Hortonworks Data Platform 2.6](https://docs.hortonworks.com/HDPDocuments/HDP2/HDP-2.6.0/bk_release-notes/content/ch_relnotes.html) basiert.
* HDInsight-Clusterversion 3.5 verwendet eine Hadoop-Distribution, die auf [Hortonworks Data Platform 2.5](https://docs.hortonworks.com/HDPDocuments/HDP2/HDP-2.5.0/bk_release-notes/content/ch_relnotes_v250.html) basiert. HDInsight-Clusterversion 3.5 ist das _Standard_-Hadoop-Cluster, das im Azure-Portal erstellt wird.
* HDInsight-Clusterversion 3.4 verwendet eine Hadoop-Distribution, die auf [Hortonworks Data Platform 2.4](https://docs.hortonworks.com/HDPDocuments/HDP2/HDP-2.4.0/bk_HDP_RelNotes/content/ch_relnotes_v240.html)basiert.
* HDInsight-Clusterversion 3.3 verwendet eine Hadoop-Distribution, die auf [Hortonworks Data Platform 2.3](https://docs.hortonworks.com/HDPDocuments/HDP2/HDP-2.3.0/bk_HDP_RelNotes/content/ch_relnotes_v230.html)basiert.

  * Auf der Apache-Website finden Sie die [Anmerkungen zur Version von Apache Storm](https://storm.apache.org/2015/11/05/storm0100-released.html).
  * Auf der Apache-Website finden Sie die [Anmerkungen zur Version von Apache Hive](https://issues.apache.org/jira/secure/ReleaseNote.jspa?version=12332384&styleName=Text&projectId=12310843).
* HDInsight-Clusterversion 3.2 verwendet eine Hadoop-Distribution, die auf [Hortonworks Data Platform 2.2][hdp-2-2] basiert.

  * Versionshinweise für bestimmte Apache-Komponenten sind wie folgt verfügbar: [Hive 0.14](https://issues.apache.org/jira/secure/ReleaseNote.jspa?projectId=12310843&version=12326450), [Pig 0.14](https://issues.apache.org/jira/secure/ReleaseNote.jspa?projectId=12310730&version=12326954), [HBase 0.98.4](https://issues.apache.org/jira/secure/ReleaseNote.jspa?projectId=12310753&version=12326810), [Phoenix 4.2.0](https://issues.apache.org/jira/secure/ReleaseNote.jspa?projectId=12315120&version=12327581), [M/R 2.6](https://issues.apache.org/jira/secure/ReleaseNote.jspa?projectId=12310941&version=12327180), [HDFS 2.6](https://issues.apache.org/jira/secure/ReleaseNote.jspa?projectId=12310942&version=12327181), [YARN 2.6](https://issues.apache.org/jira/secure/ReleaseNote.jspa?projectId=12313722&version=12327197), [Common](https://issues.apache.org/jira/secure/ReleaseNote.jspa?projectId=12310240&version=12327179), [Tez 0.5.2](https://issues.apache.org/jira/secure/ReleaseNote.jspa?projectId=12314426&version=12328742), [Ambari 2.0](https://issues.apache.org/jira/secure/ReleaseNote.jspa?projectId=12312020&version=12327486), [Storm 0.9.3](https://issues.apache.org/jira/secure/ReleaseNote.jspa?projectId=12314820&version=12327112) und [Oozie 4.1.0](https://issues.apache.org/jira/secure/ReleaseNote.jspa?version=12324960&projectId=12311620).
* HDInsight-Clusterversion 3.1 verwendet eine Hadoop-Distribution, die auf [Hortonworks Data Platform 2.1.7][hdp-2-1-7] basiert. Vor dem 7. November 2014 erstellte HDInsight 3.1-Cluster basierten auf [Hortonworks Data Platform 2.1.1][hdp-2-1-1].
* HDInsight-Clusterversion 3.0 verwendet eine Hadoop-Distribution, die auf [Hortonworks Data Platform 2.0][hdp-2-0-8] basiert.
* HDInsight-Clusterversion 2.1 verwendet eine Hadoop-Distribution, die auf [Hortonworks Data Platform 1.3][hdp-1-3-0] basiert.
* HDInsight-Clusterversion 1.6 verwendet eine Hadoop-Distribution, die auf [Hortonworks Data Platform 1.1][hdp-1-1-0] basiert.

## <a name="default-node-configuration-and-virtual-machine-sizes-for-clusters"></a>Standardknotenkonfiguration und Standardgrößen von virtuellen Computern für Cluster

Weitere Informationen dazu, welche SKUs für virtuelle Computer für Ihren Cluster ausgewählt werden sollten, finden Sie in dem Artikel über [Details einer Azure HDInsight-Clusterkonfiguration](hdinsight-supported-node-configuration.md).

## <a name="next-steps"></a>Nächste Schritte
- [Einrichten von Clustern in HDInsight mit Apache Hadoop, Spark, Kafka usw.](hdinsight-hadoop-provision-linux-clusters.md)
- [Verwenden eines Windows-Computers mit Apache Hadoop in HDInsight](hdinsight-hadoop-windows-tools.md)

[hdp-2-2]: https://docs.hortonworks.com/HDPDocuments/HDP2/HDP-2.2.9/bk_HDP_RelNotes/content/ch_relnotes_v229.html

[hdp-2-1-7]: https://docs.hortonworks.com/HDPDocuments/HDP2/HDP-2.1.7-Win/bk_releasenotes_HDP-Win/content/ch_relnotes-HDP-2.1.7.html

[hdp-2-1-1]: https://docs.hortonworks.com/HDPDocuments/HDP2/HDP-2.1.1/bk_releasenotes_hdp_2.1/content/ch_relnotes-hdp-2.1.1.html

[hdp-2-0-8]: https://docs.hortonworks.com/HDPDocuments/HDP2/HDP-2.0.8.0/bk_releasenotes_hdp_2.0/content/ch_relnotes-hdp2.0.8.0.html

[hdp-1-3-0]: https://docs.hortonworks.com/HDPDocuments/HDP1/HDP-1.3.0/bk_releasenotes_hdp_1.x/content/ch_relnotes-hdp1.3.0_1.html

[hdp-1-1-0]: https://docs.hortonworks.com/HDPDocuments/HDP1/HDP-1.3.0/bk_releasenotes_hdp_1.x/content/ch_relnotes-hdp1.1.1.16_1.html
