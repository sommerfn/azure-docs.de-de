---
title: Fehler durch Apache Hive Zeppelin-Interpreter – Azure HDInsight
description: Der Apache Zeppelin Hive-JDBC-Interpreter zeigt auf die falsche URL in Azure HDInsight
ms.service: hdinsight
ms.topic: troubleshooting
author: hrasheed-msft
ms.author: hrasheed
ms.reviewer: jasonh
ms.date: 07/30/2019
ms.openlocfilehash: dd5a45cb885d800d9922e7c8967f20b63b92be60
ms.sourcegitcommit: c22327552d62f88aeaa321189f9b9a631525027c
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 11/04/2019
ms.locfileid: "73494166"
---
# <a name="scenario-apache-hive-zeppelin-interpreter-gives-a-zookeeper-error-in-azure-hdinsight"></a>Szenario: Apache Hive Zeppelin-Interpreter löst einen ZooKeeper-Fehler in Azure HDInsight aus

In diesem Artikel werden Schritte zur Problembehandlung und mögliche Lösungen für Probleme bei der Verwendung von Interactive Query-Komponenten in Azure HDInsight-Clustern beschrieben.

## <a name="issue"></a>Problem

In einem Apache Hive LLAP-Cluster gibt der Zeppelin-Interpreter beim Versuch, eine Abfrage auszuführen, die folgende Fehlermeldung aus:

```
java.sql.SQLException: org.apache.hive.jdbc.ZooKeeperHiveClientException: Unable to read HiveServer2 configs from ZooKeeper
```

## <a name="cause"></a>Ursache

Der Zeppelin Hive-JDBC-Interpreter zeigt auf die falsche URL.

## <a name="resolution"></a>Lösung

1. Navigieren Sie zur Hive-Komponentenzusammenfassung, und kopieren Sie die „Hive JDBC Url“ in die Zwischenablage.

1. Navigieren Sie zu `https://clustername.azurehdinsight.net/zeppelin/#/interpreter`.

1. Bearbeiten Sie die JDBC-Einstellungen: Aktualisieren Sie den Wert von hive.url in die in Schritt 1 kopierte Hive JDBC URL.

1. Speichern Sie die URL, und wiederholen Sie die Abfrage.

## <a name="next-steps"></a>Nächste Schritte

Wenn Ihr Problem nicht aufgeführt ist oder Sie es nicht lösen können, besuchen Sie einen der folgenden Kanäle, um weitere Unterstützung zu erhalten:

* Nutzen Sie den [Azure-Communitysupport](https://azure.microsoft.com/support/community/), um Antworten von Azure-Experten zu erhalten.

* Nutzen Sie [@AzureSupport](https://twitter.com/azuresupport) – das offizielle Microsoft Azure-Konto zur Verbesserung der Benutzerfreundlichkeit. Hierüber hat die Azure-Community Zugriff auf die richtigen Ressourcen: Antworten, Support und Experten.

* Sollten Sie weitere Unterstützung benötigen, senden Sie eine Supportanfrage über das [Azure-Portal](https://portal.azure.com/?#blade/Microsoft_Azure_Support/HelpAndSupportBlade/). Wählen Sie dazu auf der Menüleiste die Option **Support** aus, oder öffnen Sie den Hub **Hilfe und Support**. Ausführlichere Informationen hierzu finden Sie unter [Erstellen einer Azure-Supportanfrage](https://docs.microsoft.com/azure/azure-supportability/how-to-create-azure-support-request). Zugang zu Abonnementverwaltung und Abrechnungssupport ist in Ihrem Microsoft Azure-Abonnement enthalten. Technischer Support wird über einen [Azure-Supportplan](https://azure.microsoft.com/support/plans/) bereitgestellt.
