---
title: Fehler vom Typ „Berechtigung verweigert“ bei Apache Hive-Tabelle in Azure HDInsight
description: Fehler vom Typ „Berechtigung verweigert“ beim Erstellen einer Apache Hive-Tabelle in Azure HDInsight
ms.service: hdinsight
ms.topic: troubleshooting
author: hrasheed-msft
ms.author: hrasheed
ms.reviewer: jasonh
ms.date: 08/09/2019
ms.openlocfilehash: d9901132af992ea95a60773f404b1351386cfbcb
ms.sourcegitcommit: c22327552d62f88aeaa321189f9b9a631525027c
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 11/04/2019
ms.locfileid: "73494209"
---
# <a name="scenario-permission-denied-error-when-trying-to-create-an-apache-hive-table-in-azure-hdinsight"></a>Szenario: Fehler vom Typ „Berechtigung verweigert“ beim Erstellen einer Apache Hive-Tabelle in Azure HDInsight

Dieser Artikel beschreibt Schritte zur Problembehandlung sowie mögliche Lösungen für Probleme bei der Verwendung von Interactive Query-Komponenten in Azure HDInsight-Clustern.

## <a name="issue"></a>Problem

Beim Versuch, eine Tabelle zu erstellen, wird der folgende Fehler angezeigt:

```
java.sql.SQLException: Error while compiling statement: FAILED: HiveAccessControlException Permission denied: user [hdiuser] does not have [ALL] privilege on [wasbs://data@xxxxx.blob.core.windows.net/path/table]
```

Wenn Sie den folgenden HDFS-Speicherbefehl ausführen, wird eine ähnliche Fehlermeldung angezeigt:

```
hdfs dfs -mkdir wasbs://data@xxxxx.blob.core.windows.net/path/table
```

## <a name="cause"></a>Ursache

Ob eine Tabelle in Apache Hive erstellt werden kann, hängt von den Berechtigungen ab, die auf das Speicherkonto des Clusters angewendet wurden. Sind die Berechtigungen für das Clusterspeicherkonto nicht korrekt, können Sie keine Tabellen erstellen. Das bedeutet, dass möglicherweise trotz korrekter Ranger-Richtlinien für die Tabellenerstellung Fehler vom Typ „Berechtigung verweigert“ angezeigt werden.

## <a name="resolution"></a>Lösung

Dieses Problem ist auf unzureichende Berechtigungen für den verwendeten Speichercontainer zurückzuführen. Der Benutzer, der die Hive-Tabelle erstellt, muss über Lese-, Schreib- und Ausführungsberechtigungen für den Container verfügen. Weitere Informationen finden Sie unter [Best Practices for Hive Authorization Using Apache Ranger in HDP 2.2](https://hortonworks.com/blog/best-practices-for-hive-authorization-using-apache-ranger-in-hdp-2-2/) (Bewährte Methoden für die Hive-Autorisierung mit Apache Ranger in HDP 2.2).

## <a name="next-steps"></a>Nächste Schritte

Wenn Ihr Problem nicht aufgeführt ist oder Sie es nicht lösen können, besuchen Sie einen der folgenden Kanäle, um weitere Unterstützung zu erhalten:

* Nutzen Sie den [Azure-Communitysupport](https://azure.microsoft.com/support/community/), um Antworten von Azure-Experten zu erhalten.

* Nutzen Sie [@AzureSupport](https://twitter.com/azuresupport) – das offizielle Microsoft Azure-Konto zur Verbesserung der Benutzerfreundlichkeit. Hierüber hat die Azure-Community Zugriff auf die richtigen Ressourcen: Antworten, Support und Experten.

* Sollten Sie weitere Unterstützung benötigen, senden Sie eine Supportanfrage über das [Azure-Portal](https://portal.azure.com/?#blade/Microsoft_Azure_Support/HelpAndSupportBlade/). Wählen Sie dazu auf der Menüleiste die Option **Support** aus, oder öffnen Sie den Hub **Hilfe und Support**. Ausführlichere Informationen hierzu finden Sie unter [Erstellen einer Azure-Supportanfrage](https://docs.microsoft.com/azure/azure-supportability/how-to-create-azure-support-request). Zugang zu Abonnementverwaltung und Abrechnungssupport ist in Ihrem Microsoft Azure-Abonnement enthalten. Technischer Support wird über einen [Azure-Supportplan](https://azure.microsoft.com/support/plans/) bereitgestellt.
