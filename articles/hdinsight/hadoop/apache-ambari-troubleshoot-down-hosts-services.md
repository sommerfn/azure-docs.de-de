---
title: Die Apache Ambari-Benutzeroberfläche zeigt ausgefallene Hosts und Dienste in Azure HDInsight
description: Problembehandlung bei einer Apache Ambari-Benutzeroberfläche, wenn ausgefallene Hosts und Dienste in Azure HDInsight angezeigt werden
ms.service: hdinsight
ms.topic: troubleshooting
author: hrasheed-msft
ms.author: hrasheed
ms.date: 08/02/2019
ms.openlocfilehash: bc65387252fd425256684474f38ec5d783bb45e7
ms.sourcegitcommit: 97605f3e7ff9b6f74e81f327edd19aefe79135d2
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 09/06/2019
ms.locfileid: "70734851"
---
# <a name="scenario-apache-ambari-ui-shows-down-hosts-and-services-in-azure-hdinsight"></a>Szenario: Die Apache Ambari-Benutzeroberfläche zeigt ausgefallene Hosts und Dienste in Azure HDInsight

In diesem Artikel werden Schritte zur Problembehandlung und mögliche Lösungen für Probleme bei der Interaktion mit Azure HDInsight-Clustern beschrieben.

## <a name="issue"></a>Problem

Der Zugriff auf die Apache Ambari-Benutzeroberfläche ist möglich, aber die Benutzeroberfläche zeigt, dass fast alle Dienste ausgefallen sind, und alle Hosts zeigen, dass kein Heartbeat mehr empfangen wird.

## <a name="cause"></a>Ursache

In den meisten Szenarien tritt dieses Problem auf, wenn AmbariServer nicht auf dem aktiven Hauptknoten ausgeführt wird. Überprüfen Sie, welcher Hauptknoten der aktive Hauptknoten ist, und stellen Sie sicher, dass Ihr AmbariServer auf dem richtigen ausgeführt wird. Starten Sie AmbariServer nicht manuell, sondern legen Sie fest, dass der Failovercontrollerdienst für das Starten von AmbariServer auf dem richtigen Hauptknoten zuständig ist. Starten Sie den aktiven Hauptknoten neu, um ein Failover zu erzwingen.

Dieses Problem kann auch durch Netzwerkprobleme verursacht werden. Überprüfen Sie bei jedem Clusterknoten, ob Sie `headnodehost` pingen können. Es gibt eine seltene Situation, in der kein Clusterknoten eine Verbindung mit `headnodehost` herstellen kann:

```
$>telnet headnodehost 8440
... No route to host
```

## <a name="resolution"></a>Lösung

Normalerweise wird dieses Problem durch einen Neustart des aktiven Hauptknotens behoben. Falls nicht, wenden Sie sich an das Support-Team von HDInsight.

## <a name="next-steps"></a>Nächste Schritte

Wenn Ihr Problem nicht aufgeführt ist oder Sie es nicht lösen können, besuchen Sie einen der folgenden Kanäle, um weitere Unterstützung zu erhalten:

* Nutzen Sie den [Azure-Communitysupport](https://azure.microsoft.com/support/community/), um Antworten von Azure-Experten zu erhalten.

* Nutzen Sie [@AzureSupport](https://twitter.com/azuresupport) – das offizielle Microsoft Azure-Konto zur Verbesserung der Benutzerfreundlichkeit. Hierüber hat die Azure-Community Zugriff auf die richtigen Ressourcen: Antworten, Support und Experten.

* Sollten Sie weitere Unterstützung benötigen, senden Sie eine Supportanfrage über das [Azure-Portal](https://portal.azure.com/?#blade/Microsoft_Azure_Support/HelpAndSupportBlade/). Wählen Sie dazu auf der Menüleiste die Option **Support** aus, oder öffnen Sie den Hub **Hilfe und Support**. Ausführlichere Informationen hierzu finden Sie unter [Erstellen einer Azure-Supportanfrage](https://docs.microsoft.com/azure/azure-supportability/how-to-create-azure-support-request). Zugang zu Abonnementverwaltung und Abrechnungssupport ist in Ihrem Microsoft Azure-Abonnement enthalten. Technischer Support wird über einen [Azure-Supportplan](https://azure.microsoft.com/support/plans/) bereitgestellt.
