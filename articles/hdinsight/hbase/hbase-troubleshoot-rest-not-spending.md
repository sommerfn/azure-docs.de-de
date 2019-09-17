---
title: Ausbleibende Reaktion von Apache HBase REST auf Anforderungen in Azure HDInsight
description: Beheben eines Problems bei ausbleibender Reaktion von Apache HBase REST auf Anforderungen in Azure HDInsight.
ms.service: hdinsight
ms.topic: troubleshooting
author: hrasheed-msft
ms.author: hrasheed
ms.date: 08/01/2019
ms.openlocfilehash: 5b1291e6996cb5db366128f829ca2cdf3cad0a9d
ms.sourcegitcommit: 97605f3e7ff9b6f74e81f327edd19aefe79135d2
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 09/06/2019
ms.locfileid: "70735315"
---
# <a name="scenario-apache-hbase-rest-not-responding-to-requests-in-azure-hdinsight"></a>Szenario: Ausbleibende Reaktion von Apache HBase REST auf Anforderungen in Azure HDInsight

In diesem Artikel werden Schritte zur Problembehandlung und mögliche Lösungen für Probleme bei der Interaktion mit Azure HDInsight-Clustern beschrieben.

## <a name="issue"></a>Problem

Der Apache HBase REST-Dienst reagiert nicht auf Anforderungen in Azure HDInsight.

## <a name="cause"></a>Ursache

Dieses Problem ist möglicherweise auf Socketverluste des Apache HBase REST-Diensts zurückzuführen, was insbesondere vorkommen kann, wenn der Dienst bereits sehr lange (beispielsweise über mehrere Monate) ausgeführt wird. Durch das Client-SDK wird ggf. eine Fehlermeldung wie die folgende angezeigt:

```
System.Net.WebException : Unable to connect to the remote server --->
System.Net.Sockets.SocketException : A connection attempt failed because the connected party did not properly respond after a period of time, or established connection failed because connected host has failed to respond 10.0.0.19:8090
```

## <a name="resolution"></a>Lösung

Stellen Sie eine SSH-Verbindung mit dem Host her, und starten Sie HBase REST anschließend mithilfe des folgenden Befehls neu. Alternativ kann der Dienst auch mithilfe von Skriptaktionen auf allen Workerknoten neu gestartet werden:

```bash
sudo service hdinsight-hbrest restart
```

Dieser Befehl beendet den HBase-Regionsserver auf dem gleichen Host. Sie können den HBase-Regionsserver entweder manuell über Ambari starten oder die automatische Neustartfunktion von Ambari verwenden, um den HBase-Regionsserver automatisch wiederherzustellen.

Sollte das Problem weiterhin bestehen, können Sie das folgende Abhilfeskript als CRON-Auftrag installieren, der alle fünf Minuten auf jedem Workerknoten ausgeführt wird. Dieses Abhilfeskript pingt den REST-Dienst und startet ihn neu, falls er nicht reagiert.

```bash
#!/bin/bash
nc localhost 8090 < /dev/null
if [ $? -ne 0 ]
    then
    echo "RESTServer is not responding. Restarting"
    sudo /usr/hdp/current/hbase-regionserver/bin/hbase-daemon.sh restart rest
fi
```

## <a name="next-steps"></a>Nächste Schritte

Wenn Ihr Problem nicht aufgeführt ist oder Sie es nicht lösen können, besuchen Sie einen der folgenden Kanäle, um weitere Unterstützung zu erhalten:

* Nutzen Sie den [Azure-Communitysupport](https://azure.microsoft.com/support/community/), um Antworten von Azure-Experten zu erhalten.

* Nutzen Sie [@AzureSupport](https://twitter.com/azuresupport) – das offizielle Microsoft Azure-Konto zur Verbesserung der Benutzerfreundlichkeit. Hierüber hat die Azure-Community Zugriff auf die richtigen Ressourcen: Antworten, Support und Experten.

* Sollten Sie weitere Unterstützung benötigen, senden Sie eine Supportanfrage über das [Azure-Portal](https://portal.azure.com/?#blade/Microsoft_Azure_Support/HelpAndSupportBlade/). Wählen Sie dazu auf der Menüleiste die Option **Support** aus, oder öffnen Sie den Hub **Hilfe und Support**. Ausführlichere Informationen hierzu finden Sie unter [Erstellen einer Azure-Supportanfrage](https://docs.microsoft.com/azure/azure-supportability/how-to-create-azure-support-request). Zugang zu Abonnementverwaltung und Abrechnungssupport ist in Ihrem Microsoft Azure-Abonnement enthalten. Technischer Support wird über einen [Azure-Supportplan](https://azure.microsoft.com/support/plans/) bereitgestellt.
