---
title: Problembehandlung bei HBase mit Azure HDInsight
description: Hier erhalten Sie Antworten auf häufig gestellte Fragen zum Arbeiten mit HBase und Azure HDInsight.
ms.service: hdinsight
author: hrasheed-msft
ms.author: hrasheed
ms.custom: hdinsightactive, seodec18
ms.topic: troubleshooting
ms.date: 08/16/2019
ms.openlocfilehash: cf44c27f51bf6312ef546b424646833f69ba0283
ms.sourcegitcommit: 36e9cbd767b3f12d3524fadc2b50b281458122dc
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 08/20/2019
ms.locfileid: "69638417"
---
# <a name="troubleshoot-apache-hbase-by-using-azure-hdinsight"></a>Problembehandlung bei Apache HBase mit Azure HDInsight

Lernen Sie die wichtigsten Probleme und ihre Lösungen bei der Arbeit mit Apache HBase-Nutzlasten in Apache Ambari kennen.

## <a name="how-do-i-fix-jdbc-or-sqlline-connectivity-issues-with-apache-phoenix"></a>Wie behebe ich JDBC- oder SQLLine-Konnektivitätsprobleme mit Apache Phoenix?

### <a name="resolution-steps"></a>Lösungsschritte

Um eine Verbindung mit Apache Phoenix herzustellen, müssen Sie die IP-Adresse eines aktiven Apache ZooKeeper-Knotens angeben. Stellen Sie sicher, dass der ZooKeeper-Dienst, mit dem „sqlline.py“ eine Verbindung herstellen soll, ausgeführt wird.
1. Melden Sie sich mithilfe von SSH beim HDInsight-Cluster an.
2. Geben Sie den folgenden Befehl ein:
                
   ```apache
           "/usr/hdp/current/phoenix-client/bin/sqlline.py <IP of machine where Active Zookeeper is running"
   ```

   > [!Note] 
   > Sie können die IP-Adresse des aktiven ZooKeeper-Knotens aus der Ambari-Benutzeroberfläche abrufen. Wechseln Sie zu **HBase** > **Quick Links** > **ZK\* (Active)**  > **ZooKeeper Info**.

3. Wenn „sqlline.py“ versucht, eine Verbindung mit Phoenix herzustellen und kein Timeout auftritt, führen Sie den folgenden Befehl aus, um die Verfügbarkeit und Integrität von Phoenix zu überprüfen:

   ```apache
           !tables
           !quit
   ```      
4. Wenn dieser Befehl erfolgreich ausgeführt wird, liegt kein Problem vor. Die vom Benutzer angegebene IP-Adresse ist möglicherweise falsch. Wenn der Befehl jedoch über einen längeren Zeitraum pausiert und anschließend der folgende Fehler ausgegeben wird, fahren Sie mit Schritt 5 fort.

   ```apache
           Error while connecting to sqlline.py (Hbase - phoenix) Setting property: [isolation, TRANSACTION_READ_COMMITTED] issuing: !connect jdbc:phoenix:10.2.0.7 none none org.apache.phoenix.jdbc.PhoenixDriver Connecting to jdbc:phoenix:10.2.0.7 SLF4J: Class path contains multiple SLF4J bindings. 
   ```

5. Führen Sie die folgenden Befehle vom Hauptknoten (hn0) aus, um den Zustand der Phoenix-Tabelle SYSTEM.CATALOG zu diagnostizieren:

   ```apache
            hbase shell
                
           count 'SYSTEM.CATALOG'
   ```

   Der Befehl sollte eine Fehlermeldung ähnlich der Folgenden zurückgeben: 

   ```apache
           ERROR: org.apache.hadoop.hbase.NotServingRegionException: Region SYSTEM.CATALOG,,1485464083256.c0568c94033870c517ed36c45da98129. is not online on 10.2.0.5,16020,1489466172189) 
   ```
6. Führen Sie auf der Apache Ambari-Benutzeroberfläche die folgenden Schritte aus, um den HMaster-Dienst auf allen ZooKeeper-Knoten neu zu starten:

    1. Wechseln Sie im Abschnitt **Summary** von HBase zu **HBase** > **Active HBase Master**. 
    2. Starten Sie im Abschnitt **Components** den HBase-Master-Dienst neu.
    3. Wiederholen Sie diese Schritte für die verbleibenden **Standby HBase Master**-Dienste. 

Es kann bis zu fünf Minuten dauern, bis der HBase-Master-Dienst stabilisiert und der Wiederherstellungsvorgang abgeschlossen ist. Warten Sie einige Minuten, und führen Sie die sqlline.py-Befehle erneut aus, um sich zu vergewissern, dass die Tabelle SYSTEM.CATALOG aufgerufen wurde und abgefragt werden kann. 

Sobald sich die Tabelle „SYSTEM.CATALOG“ wieder im normalen Zustand befindet, sollte das Problem der Konnektivität mit Phoenix automatisch gelöst werden.

## <a name="what-causes-a-restart-failure-on-a-region-server"></a>Was verursacht einen Neustartfehler auf einem Regionsserver?

### <a name="issue"></a>Problem

Ein Neustartfehler auf einem Regionsserver kann durch Befolgen der folgenden bewährten Methoden vermieden werden. Es wird empfohlen, hohe Workload-Aktivitäten zu unterbrechen, wenn Sie einen Neustart von HBase-Regionsservern planen. Wenn die Anwendung während des Herunterfahrens weiterhin versucht, Verbindungen mit Regionsservern herzustellen, wird der Regionsserver-Neustart um einige Minuten verlangsamt. Zudem empfiehlt es sich, zuvor alle Tabellen zu leeren. Eine Referenz zum Leeren von Tabellen finden Sie unter [HDInsight HBase: Verbessern der Neustartzeit für den Apache HBase-Cluster durch Leeren der Tabellen](https://web.archive.org/web/20190112153155/https://blogs.msdn.microsoft.com/azuredatalake/2016/09/19/hdinsight-hbase-how-to-improve-hbase-cluster-restart-time-by-flushing-tables/).

Wenn Sie den Neustartvorgang auf HBase-Regionsservern über die Apache Ambari-Benutzeroberfläche initiieren, stellen Sie sofort fest, dass die Regionsserver heruntergefahren wurden, aber nicht sofort neu gestartet werden. 

Die folgenden Vorgänge laufen im Hintergrund ab: 

1. Der Ambari-Agent sendet eine Beendigungsanforderung an den Regionsserver.
2. Der Ambari-Agent wartet 30 Sekunden auf das ordnungsgemäße Herunterfahren des Regionsservers. 
3. Wenn Ihre Anwendung weiterhin eine Verbindung mit dem Regionsserver unterhält, wird der Server nicht sofort heruntergefahren. Das Zeitlimit von 30 Sekunden läuft ab, bevor der Server heruntergefahren wird. 
4. Nach 30 Sekunden sendet der Ambari-Agent einen Befehl zum Erzwingen des Beendens (`kill -9`) an den Regionsserver. Dies wird im Ambari-Agent-Protokoll angezeigt (im Verzeichnis „/var/log/“ des jeweiligen Workerknotens):

   ```apache
           2017-03-21 13:22:09,171 - Execute['/usr/hdp/current/hbase-regionserver/bin/hbase-daemon.sh --config /usr/hdp/current/hbase-regionserver/conf stop regionserver'] {'only_if': 'ambari-sudo.sh  -H -E t
           est -f /var/run/hbase/hbase-hbase-regionserver.pid && ps -p `ambari-sudo.sh  -H -E cat /var/run/hbase/hbase-hbase-regionserver.pid` >/dev/null 2>&1', 'on_timeout': '! ( ambari-sudo.sh  -H -E test -
           f /var/run/hbase/hbase-hbase-regionserver.pid && ps -p `ambari-sudo.sh  -H -E cat /var/run/hbase/hbase-hbase-regionserver.pid` >/dev/null 2>&1 ) || ambari-sudo.sh -H -E kill -9 `ambari-sudo.sh  -H 
           -E cat /var/run/hbase/hbase-hbase-regionserver.pid`', 'timeout': 30, 'user': 'hbase'}
           2017-03-21 13:22:40,268 - Executing '! ( ambari-sudo.sh  -H -E test -f /var/run/hbase/hbase-hbase-regionserver.pid && ps -p `ambari-sudo.sh  -H -E cat /var/run/hbase/hbase-hbase-regionserver.pid` >
           /dev/null 2>&1 ) || ambari-sudo.sh -H -E kill -9 `ambari-sudo.sh  -H -E cat /var/run/hbase/hbase-hbase-regionserver.pid`'. Reason: Execution of 'ambari-sudo.sh su hbase -l -s /bin/bash -c 'export  
           PATH=/usr/local/sbin:/usr/local/bin:/usr/sbin:/usr/bin:/sbin:/bin:/usr/games:/usr/local/games:/var/lib/ambari-agent ; /usr/hdp/current/hbase-regionserver/bin/hbase-daemon.sh --config /usr/hdp/curre
           nt/hbase-regionserver/conf stop regionserver was killed due timeout after 30 seconds
           2017-03-21 13:22:40,285 - File['/var/run/hbase/hbase-hbase-regionserver.pid'] {'action': ['delete']}
           2017-03-21 13:22:40,285 - Deleting File['/var/run/hbase/hbase-hbase-regionserver.pid']
   ```
   Aufgrund dieses abrupten Herunterfahrens wird der Port, der dem Prozess zugeordnet ist, möglicherweise nicht freigegeben, obwohl der Regionsserverprozess beendet wird. Dieser Umstand kann beim Starten des Servers zu einer AddressBindException führen, wie in den folgenden Protokollen ersichtlich ist. Dies können Sie in „region-server.log“ im Verzeichnis „/var/log/hbase“ der Workerknoten überprüfen, auf denen beim Starten des Regionsservers ein Fehler auftritt. 

   ```apache

   2017-03-21 13:25:47,061 ERROR [main] regionserver.HRegionServerCommandLine: Region server exiting
   java.lang.RuntimeException: Failed construction of Regionserver: class org.apache.hadoop.hbase.regionserver.HRegionServer
   at org.apache.hadoop.hbase.regionserver.HRegionServer.constructRegionServer(HRegionServer.java:2636)
   at org.apache.hadoop.hbase.regionserver.HRegionServerCommandLine.start(HRegionServerCommandLine.java:64)
   at org.apache.hadoop.hbase.regionserver.HRegionServerCommandLine.run(HRegionServerCommandLine.java:87)
   at org.apache.hadoop.util.ToolRunner.run(ToolRunner.java:76)
   at org.apache.hadoop.hbase.util.ServerCommandLine.doMain(ServerCommandLine.java:126)
   at org.apache.hadoop.hbase.regionserver.HRegionServer.main(HRegionServer.java:2651)
        
   Caused by: java.lang.reflect.InvocationTargetException
   at sun.reflect.NativeConstructorAccessorImpl.newInstance0(Native Method)
   at sun.reflect.NativeConstructorAccessorImpl.newInstance(NativeConstructorAccessorImpl.java:57)
   at sun.reflect.DelegatingConstructorAccessorImpl.newInstance(DelegatingConstructorAccessorImpl.java:45)
   at java.lang.reflect.Constructor.newInstance(Constructor.java:526)
   at org.apache.hadoop.hbase.regionserver.HRegionServer.constructRegionServer(HRegionServer.java:2634)
   ... 5 more
        
   Caused by: java.net.BindException: Problem binding to /10.2.0.4:16020 : Address already in use
   at org.apache.hadoop.hbase.ipc.RpcServer.bind(RpcServer.java:2497)
   at org.apache.hadoop.hbase.ipc.RpcServer$Listener.<init>(RpcServer.java:580)
   at org.apache.hadoop.hbase.ipc.RpcServer.<init>(RpcServer.java:1982)
   at org.apache.hadoop.hbase.regionserver.RSRpcServices.<init>(RSRpcServices.java:863)
   at org.apache.hadoop.hbase.regionserver.HRegionServer.createRpcServices(HRegionServer.java:632)
   at org.apache.hadoop.hbase.regionserver.HRegionServer.<init>(HRegionServer.java:532)
   ... 10 more
        
   Caused by: java.net.BindException: Address already in use
   at sun.nio.ch.Net.bind0(Native Method)
   at sun.nio.ch.Net.bind(Net.java:463)
   at sun.nio.ch.Net.bind(Net.java:455)
   at sun.nio.ch.ServerSocketChannelImpl.bind(ServerSocketChannelImpl.java:223)
   at sun.nio.ch.ServerSocketAdaptor.bind(ServerSocketAdaptor.java:74)
   at org.apache.hadoop.hbase.ipc.RpcServer.bind(RpcServer.java:2495)
   ... 15 more
   ```

### <a name="resolution-steps"></a>Lösungsschritte

1. Versuchen Sie, die Last auf den HBase-Regionsservern zu reduzieren, bevor Sie einen Neustart initiieren. 
2. Sie können auch versuchen (wenn Schritt 1 nicht erfolgreich war), die Regionsserver auf den Workerknoten mithilfe der folgenden Befehle manuell neu zu starten:

   ```apache
   sudo su - hbase -c "/usr/hdp/current/hbase-regionserver/bin/hbase-daemon.sh stop regionserver"
   sudo su - hbase -c "/usr/hdp/current/hbase-regionserver/bin/hbase-daemon.sh start regionserver"   
   ```

## <a name="next-steps"></a>Nächste Schritte

Wenn Ihr Problem nicht aufgeführt ist oder Sie es nicht lösen können, besuchen Sie einen der folgenden Kanäle, um weitere Unterstützung zu erhalten:

* Nutzen Sie den [Azure-Communitysupport](https://azure.microsoft.com/support/community/), um Antworten von Azure-Experten zu erhalten.

* Herstellen einer Verbindung mit [@AzureSupport](https://twitter.com/azuresupport), dem offiziellen Microsoft Azure-Konto zum Verbessern der Kundenfreundlichkeit. Verbinden der Azure-Community mit den richtigen Ressourcen: Antworten, Support und Experten.

* Sollten Sie weitere Unterstützung benötigen, senden Sie eine Supportanfrage über das [Azure-Portal](https://portal.azure.com/?#blade/Microsoft_Azure_Support/HelpAndSupportBlade/). Wählen Sie dazu auf der Menüleiste die Option **Support** aus, oder öffnen Sie den Hub **Hilfe und Support**. Ausführlichere Informationen hierzu finden Sie unter [Erstellen einer Azure-Supportanfrage](https://docs.microsoft.com/azure/azure-supportability/how-to-create-azure-support-request). Zugang zu Abonnementverwaltung und Abrechnungssupport ist in Ihrem Microsoft Azure-Abonnement enthalten. Technischer Support wird über einen [Azure-Supportplan](https://azure.microsoft.com/support/plans/) bereitgestellt.
