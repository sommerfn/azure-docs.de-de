---
title: Anmerkungen zu dieser Version von Azure HDInsight
description: Die neuesten Versionshinweise für Azure HDInsight. Rufen Sie Tipps zur Entwicklung und Details für Hadoop, Spark, R Server, Hive und mehr ab.
author: hrasheed-msft
ms.author: hrasheed
ms.reviewer: jasonh
ms.custom: hdinsightactive
ms.service: hdinsight
ms.topic: conceptual
ms.date: 08/08/2019
ms.openlocfilehash: 7c31520efd881e8e0b5ed309f62d273bac59c0e3
ms.sourcegitcommit: 124c3112b94c951535e0be20a751150b79289594
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 08/10/2019
ms.locfileid: "68945030"
---
# <a name="release-notes"></a>Versionshinweise

Dieser Artikel enthält Informationen zu den **neuesten** Versionsupdates für Azure HDInsight. Informationen zu neueren Versionen finden Sie unter [Versionshinweise zu HDInsight – Archiv](hdinsight-release-notes-archive.md).

> [!IMPORTANT]  
> Linux ist das einzige Betriebssystem, das unter HDInsight Version 3.4 oder höher verwendet wird. Weitere Informationen finden Sie im Artikel zur [HDInsight-Versionsverwaltung](hdinsight-component-versioning.md).

## <a name="summary"></a>Zusammenfassung

Azure HDInsight ist einer der beliebtesten Dienste unter Enterprise-Kunden für die Open Source-Analyseframeworks Apache Hadoop und Apache Spark in Azure.

## <a name="new-features"></a>Neue Funktionen

Weitere Informationen zu wichtigen Änderungen bei HDInsight 4.0. finden Sie unter [Neuerungen in HDI 4.0](../hdinsight/hdinsight-version-release.md).

## <a name="component-versions"></a>Komponentenversionen

Die offiziellen Apache-Versionen aller HDInsight 4.0-Komponenten werden unten aufgeführt. Die aufgelisteten Komponenten stellen Releases der neuesten stabilen Versionen dar, die verfügbar sind.

- Apache Ambari 2.7.1
- Apache Hadoop 3.1.1
- Apache HBase 2.0.0
- Apache Hive 3.1.0
- Apache Kafka 1.1.1, 2.1.0
- Apache Mahout 0.9.0 und höher
- Apache Oozie 4.2.0
- Apache Phoenix 4.7.0
- Apache Pig 0.16.0
- Apache Ranger 0.7.0
- Apache Slider 0.92.0
- Apache Spark 2.3.1, 2.4.0
- Apache Sqoop 1.4.7
- Apache TEZ 0.9.1
- Apache Zeppelin 0.8.0
- Apache ZooKeeper 3.4.6

Höhere Versionen von Apache-Komponenten werden gelegentlich zusätzlich zu den oben aufgeführten Versionen in der HDP-Distribution gebündelt. In diesem Fall werden diese neueren Versionen in der Technical Previews-Tabelle aufgeführt. Diese sollten die oben aufgeführten Versionen der Apache-Komponenten in einer Produktionsumgebung nicht ersetzen.

## <a name="apache-patch-information"></a>Apache-Patchinformationen

Weitere Informationen zu den in HDInsight 4.0 verfügbaren Patches finden Sie in der Patchauflistung zu den einzelnen Produkten in der Tabelle unten.

| Produktname | Patchinformationen |
|---|---|
| Ambari | [Ambari-Patchinformationen](https://docs.hortonworks.com/HDPDocuments/Ambari-2.7.1.0/bk_ambari-release-notes/content/ambari_relnotes-2.7.1.0-patch-information.html) |
| Hadoop | [Hadoop-Patchinformationen](https://docs.hortonworks.com/HDPDocuments/HDP3/HDP-3.0.1/release-notes/content/patch_hadoop.html) |
| hbase | [HBase-Patchinformationen](https://docs.hortonworks.com/HDPDocuments/HDP3/HDP-3.0.1/release-notes/content/patch_hbase.html) |
| Hive  | Dieses Release stellt Hive 3.1.0 ohne zusätzliche Apache-Patches bereit.  |
| Kafka | Dieses Release stellt Kafka 1.1.1 ohne zusätzliche Apache-Patches bereit. |
| Oozie | [Oozie-Patchinformationen](https://docs.hortonworks.com/HDPDocuments/HDP3/HDP-3.0.1/release-notes/content/patch_oozie.html) |
| Phoenix | [Phoenix-Patchinformationen](https://docs.hortonworks.com/HDPDocuments/HDP3/HDP-3.0.1/release-notes/content/patch_phoenix.html) |
| Pig | [Pig-Patchinformationen](https://docs.hortonworks.com/HDPDocuments/HDP3/HDP-3.0.1/release-notes/content/patch_pig.html) |
| Ranger | [Ranger-Patchinformationen](https://docs.hortonworks.com/HDPDocuments/HDP3/HDP-3.0.1/release-notes/content/patch_ranger.html) |
| Spark | [Spark-Patchinformationen](https://docs.hortonworks.com/HDPDocuments/HDP3/HDP-3.0.1/release-notes/content/patch_spark.html) |
| Sqoop | Dieses Release stellt Sqoop 1.4.7 ohne zusätzliche Apache-Patches bereit. |
| Tez | Dieses Release stellt Tez 0.9.1 ohne zusätzliche Apache-Patches bereit. |
| Zeppelin | Dieses Release stellt Zeppelin 0.8.0 ohne zusätzliche Apache-Patches bereit. |
| Zookeeper | [Zookeeper-Patchinformationen](https://docs.hortonworks.com/HDPDocuments/HDP3/HDP-3.0.1/release-notes/content/patch_zookeeper.html) |

## <a name="fixed-common-vulnerabilities-and-exposures"></a>Behobene Common Vulnerabilities and Exposures

Weitere Informationen zu den in dieser Version behobenen Sicherheitsproblemen finden Sie unter in Hortonworks [Behobene Common Vulnerabilities and Exposures für HDP 3.0.1](https://docs.hortonworks.com/HDPDocuments/HDP3/HDP-3.0.1/release-notes/content/cve.html).

## <a name="known-issues"></a>Bekannte Probleme

### <a name="replication-is-broken-for-secure-hbase-with-default-installation"></a>Die Replikation ist für Secure HBase in der Standardinstallation nicht funktional

Führen Sie für HDInsight 4.0 die folgenden Schritte aus:

1. Aktivieren Sie die Kommunikation zwischen Clustern.
1. Melden Sie sich beim aktiven Hauptknoten an.
1. Laden Sie mit dem folgenden Befehl ein Skript zum Aktivieren der Replikation herunter:

    ```
    sudo wget https://raw.githubusercontent.com/Azure/hbase-utils/master/replication/hdi_enable_replication.sh
    ```
1. Geben Sie den Befehl `sudo kinit <domainuser>` ein.
1. Geben Sie den folgenden Befehl ein, um das Skript auszuführen:

    ```
    sudo bash hdi_enable_replication.sh -m <hn0> -s <srclusterdns> -d <dstclusterdns> -sp <srcclusterpasswd> -dp <dstclusterpasswd> -copydata
    ```
Gehen Sie für HDInsight 3.6 wie folgt vor:

1. Melden Sie sich beim aktiven HMaster ZK an.
1. Laden Sie mit dem folgenden Befehl ein Skript zum Aktivieren der Replikation herunter:
    ```
    sudo wget https://raw.githubusercontent.com/Azure/hbase-utils/master/replication/hdi_enable_replication.sh
    ```
1. Geben Sie den Befehl `sudo kinit -k -t /etc/security/keytabs/hbase.service.keytab hbase/<FQDN>@<DOMAIN>` ein.
1. Geben Sie folgenden Befehl ein:

    ```bash
    sudo bash hdi_enable_replication.sh -s <srclusterdns> -d <dstclusterdns> -sp <srcclusterpasswd> -dp <dstclusterpasswd> -copydata
    ```

### <a name="phoenix-sqlline-stops-working-after-migrating-hbase-cluster-to-hdinsight-40"></a>Phoenix Sqlline funktioniert nach dem Migrieren des HBase-Clusters nach HDInsight 4.0 nicht mehr

Führen Sie folgende Schritte aus:

1. Löschen Sie die folgenden Phoenix-Tabellen:
    1. `SYSTEM.FUNCTION`
    1. `SYSTEM.SEQUENCE`
    1. `SYSTEM.STATS`
    1. `SYSTEM.MUTEX`
    1. `SYSTEM.CATALOG`
1. Wenn Sie keine der Tabellen löschen können, starten Sie HBase neu, um alle Verbindungen mit den Tabellen zu löschen.
1. Führen Sie `sqlline.py` erneut aus. Phoenix erstellt alle Tabellen, die in Schritt 1 gelöscht wurden, erneut.
1. Regenerieren Sie die Phoenix-Tabellen und -Ansichten für Ihre HBase-Daten.

### <a name="phoenix-sqlline-stops-working-after-replicating-hbase-phoenix-metadata-from-hdinsight-36-to-40"></a>Phoenix Sqlline funktioniert nach der Replikation von HBase Phoenix-Metadaten von HDInsight 3.6 nach 4.0 nicht mehr

Führen Sie folgende Schritte aus:

1. Wechseln Sie vor dem Ausführen der Replikation zum 4.0-Zielcluster, und führen Sie `sqlline.py` aus. Dieser Befehl generiert Phoenix-Tabellen wie `SYSTEM.MUTEX` und `SYSTEM.LOG`, die nur in Version 4.0 bestehen.
1. Löschen Sie die folgenden Tabellen:
    1. `SYSTEM.FUNCTION`
    1. `SYSTEM.SEQUENCE`
    1. `SYSTEM.STATS`
    1. `SYSTEM.CATALOG`
1. Starten Sie die HBase-Replikation

## <a name="deprecation"></a>Eingestellte Unterstützung

Die Apache-Dienste Storm und ML stehen in HDInsight 4.0 nicht zur Verfügung.
