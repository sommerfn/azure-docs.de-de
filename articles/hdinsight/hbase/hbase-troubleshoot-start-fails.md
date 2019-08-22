---
title: Fehler beim Starten von Apache HBase Master in Azure HDInsight
description: Fehler beim Starten von Apache HBase Master (HMaster) in Azure HDInsight
ms.service: hdinsight
ms.topic: troubleshooting
author: hrasheed-msft
ms.author: hrasheed
ms.date: 08/06/2019
ms.openlocfilehash: 8368ebfca4cdd72c5c455a04e29b6c0cb44938ea
ms.sourcegitcommit: 13a289ba57cfae728831e6d38b7f82dae165e59d
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 08/09/2019
ms.locfileid: "68935453"
---
# <a name="apache-hbase-master-hmaster-fails-to-start-in-azure-hdinsight"></a>Fehler beim Starten von Apache HBase Master (HMaster) in Azure HDInsight

In diesem Artikel werden Schritte zur Problembehandlung und mögliche Lösungen für Probleme bei der Interaktion mit Azure HDInsight-Clustern beschrieben.

## <a name="scenario-atomic-renaming-failure"></a>Szenario: Fehler bei der atomischen Umbenennung

### <a name="issue"></a>Problem

Beim Start wurden unerwartete Dateien erkannt.

### <a name="cause"></a>Ursache

Im Rahmen des Startvorgangs führt HMaster verschiedene Initialisierungsschritte aus. Dabei werden unter anderem Daten aus dem Ablageordner (.tmp) in den Datenordner verschoben. Außerdem untersucht HMaster anhand des Ordners mit den Write-Ahead-Protokollen (Write Ahead Logs, WALs), ob nicht reagierende Regionsserver vorhanden sind. Für alle diese Ordner wird jeweils ein einfacher Befehl vom Typ `list` ausgeführt. Sollte in einem dieser Ordner eine unerwartete Datei gefunden werden, wird eine Ausnahme ausgelöst, und der Startvorgang ist nicht erfolgreich.

### <a name="resolution"></a>Lösung

Überprüfen Sie in einem solchen Fall anhand der Aufrufliste, auf welchen Ordner das Problem zurückzuführen ist. (Hierbei kann es sich beispielsweise um den Ordner mit den Write-Ahead-Protokollen oder um den TMP-Ordner handeln.) Suchen Sie anschließend über den Cloud-Explorer oder mithilfe von HDFS-Befehlen nach der problematischen Datei. Bei der problematischen Datei handelt es sich in der Regel um eine Datei vom Typ `*-renamePending.json` (eine Journaldatei zum Implementieren eines atomischen Umbenennungsvorgangs im WASB-Treiber). Fehler in dieser Implementierung können dazu führen, dass derartige Dateien nach einem Prozessabsturz weiterhin vorhanden sind. Erzwingen Sie die Löschung dieser Datei über den Cloud-Explorer. Darüber hinaus kann sich an dem Speicherort auch eine temporäre Datei vom Typ „$“ befinden. Die Datei ist im Cloud-Explorer nicht sichtbar und kann nur über den HDFS-Befehl `ls` angezeigt werden. Verwenden Sie zum Löschen dieser Datei den HDFS-Befehl `hdfs dfs -rm //\$\$\$.\$\$\$`.

Nachdem die problematische Datei entfernt wurde, sollte HMaster umgehend gestartet werden.

---

## <a name="scenario-no-server-address-listed"></a>Szenario: Keine Serveradresse aufgelistet

### <a name="issue"></a>Problem

Das HMaster-Protokoll enthält eine Fehlermeldung, die in etwa wie folgt aussieht: „Keine Serveradresse in „hbase:meta“ aufgelistet für Region xxx“.

### <a name="cause"></a>Ursache

HMaster konnte nach einem HBase-Neustart nicht initialisiert werden.

### <a name="resolution"></a>Lösung

1. Führen Sie über die HBase-Shell die folgenden Befehle aus (mit entsprechend angepassten Werten):

    ```
    scan 'hbase:meta'
    delete 'hbase:meta','hbase:backup <region name>','<column name>' 
    ```

1. Löschen Sie den Eintrag „hbase:namespace“, da der gleiche Fehler bei der Überprüfung der „hbase:namespace“-Tabelle gemeldet werden könnte.

1. Starten Sie den aktiven HMaster von der Ambari-Benutzeroberfläche aus neu, um HBase hochzufahren.

1. Führen Sie den folgenden Befehl in der HBase-Shell aus, um alle Tabellen hochzufahren, die offline sind:

    ```
    hbase hbck -ignorePreCheckPermission -fixAssignments
    ```

---

## <a name="scenario-javaioioexception-timedout"></a>Szenario: java.io.IOException: Timedout

### <a name="issue"></a>Problem

Für HMaster tritt ein Timeout mit einer schwerwiegenden Ausnahme wie `java.io.IOException: Timedout 300000ms waiting for namespace table to be assigned` auf.

### <a name="cause"></a>Ursache

Das Timeout ist ein bekannter HMaster-Fehler. Allgemeine Cluster-Startaufgaben können sehr lange dauern. HMaster wird heruntergefahren, wenn die Namespacetabelle noch nicht zugewiesen ist. Die langwierigen Starttasks treten auf, wenn eine große Menge an nicht geleerten Daten vorhanden und ein Zeitlimit von fünf Minuten nicht ausreichend ist.

### <a name="resolution"></a>Lösung

1. Navigieren Sie auf der Ambari-Benutzeroberfläche zu „HBase > Configs“ (HBase >Konfigurationen), und fügen Sie in der benutzerdefinierten Datei `hbase-site.xml` die folgende Einstellung hinzu:

    ```
    Key: hbase.master.namespace.init.timeout Value: 2400000  
    ```

1. Starten Sie die erforderlichen Dienste neu (hauptsächlich HMaster und möglicherweise andere HBase-Dienste).

---

## <a name="scenario-frequent-regionserver-restarts"></a>Szenario: Häufige Neustarts des Regionsservers

### <a name="issue"></a>Problem

Knoten werden regelmäßig neu gestartet. Die Regionsserverprotokolle enthalten Einträge wie:

```
2017-05-09 17:45:07,683 WARN  [JvmPauseMonitor] util.JvmPauseMonitor: Detected pause in JVM or host machine (eg GC): pause of approximately 31000ms
2017-05-09 17:45:07,683 WARN  [JvmPauseMonitor] util.JvmPauseMonitor: Detected pause in JVM or host machine (eg GC): pause of approximately 31000ms
2017-05-09 17:45:07,683 WARN  [JvmPauseMonitor] util.JvmPauseMonitor: Detected pause in JVM or host machine (eg GC): pause of approximately 31000ms
```

### <a name="cause"></a>Ursache

Lange Pause bei der JVM-Garbage Collection für den Regionsserver. Die Pause führt dazu, dass der Regionsserver nicht mehr reagiert und innerhalb des ZooKeeper-Sitzungstimeouts von 40 Sekunden keinen Heartbeat an HMaster senden kann. HMaster geht davon aus, dass der Regionsserver inaktiv ist, woraufhin der Regionsserver abgebrochen und neu gestartet wird.

### <a name="resolution"></a>Lösung

Ändern Sie das ZooKeeper-Sitzungstimeout. Hierzu muss sowohl die Einstellung `zookeeper.session.timeout` (hbase-site) als auch die ZooKeeper-Einstellung `maxSessionTimeout` (zoo.cfg) geändert werden.

1. Navigieren Sie auf der Ambari-Benutzeroberfläche zu **HBase > Configs > Settings** („HBase“ > „Konfigurationen“ > „Einstellungen“), und ändern Sie im Timeoutabschnitt den Wert des ZooKeeper-Sitzungstimeouts.

1. Navigieren Sie auf der Ambari-Benutzeroberfläche zu **Zookeeper > Configs > Custom** („ZooKeeper“ > „Konfigurationen“ > „Benutzerdefiniert“), und fügen Sie in „zoo.cfg“ die folgende Einstellung hinzu, bzw. ändern Sie sie. Achten Sie darauf, dass der Wert mit dem HBase-Wert für `zookeeper.session.timeout`übereinstimmt.

    ```
    Key: maxSessionTimeout Value: 120000  
    ```

1. Starten Sie die erforderlichen Dienste neu.

---

## <a name="scenario-log-splitting-failure"></a>Szenario: Fehler bei der Protokollaufteilung

### <a name="issue"></a>Problem

Fehler beim Starten von HMaster in einem HBase-Cluster.

### <a name="cause"></a>Ursache

Falsch konfigurierte HDFS- und HBase-Einstellungen für ein sekundäres Speicherkonto.

### <a name="resolution"></a>Lösung

Legen Sie „hbase.rootdir: wasb://@.blob.core.windows.net/hbase“ fest, und starten Sie die Dienste über Ambari neu.

---

## <a name="next-steps"></a>Nächste Schritte

Wenn Ihr Problem nicht aufgeführt ist oder Sie es nicht lösen können, besuchen Sie einen der folgenden Kanäle, um weitere Unterstützung zu erhalten:

* Erhalten Sie über den [Azure-Communitysupport](https://azure.microsoft.com/support/community/) Antworten von Azure-Experten.

* Nutzen Sie [@AzureSupport](https://twitter.com/azuresupport) – das offizielle Microsoft Azure-Konto zur Verbesserung der Benutzerfreundlichkeit. Hierüber hat die Azure-Community Zugriff auf die richtigen Ressourcen: Antworten, Support und Experten.

* Wenn Sie weitere Unterstützung benötigen, senden Sie eine Supportanfrage über das [Azure-Portal](https://portal.azure.com/?#blade/Microsoft_Azure_Support/HelpAndSupportBlade/). Wählen Sie dazu auf der Menüleiste die Option **Support** aus, oder öffnen Sie den Hub **Hilfe und Support**. Ausführlichere Informationen hierzu finden Sie unter [Erstellen einer Azure-Supportanfrage](https://docs.microsoft.com/azure/azure-supportability/how-to-create-azure-support-request). Zugang zu Abonnementverwaltung und Abrechnungssupport ist in Ihrem Microsoft Azure-Abonnement enthalten. Technischer Support wird über einen [Azure-Supportplan](https://azure.microsoft.com/support/plans/) bereitgestellt.
