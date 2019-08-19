---
title: Timeouts mit dem Befehl „hbase hbck“ in Azure HDInsight
description: Timeoutproblem mit dem Befehl „hbase hbck“ beim Korrigieren von Regionszuweisungen
ms.service: hdinsight
ms.topic: troubleshooting
author: hrasheed-msft
ms.author: hrasheed
ms.date: 08/01/2019
ms.openlocfilehash: 4f3f1c22fa1dc05a66a8b6bf0179903a44cef9b6
ms.sourcegitcommit: c662440cf854139b72c998f854a0b9adcd7158bb
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 08/02/2019
ms.locfileid: "68737432"
---
# <a name="scenario-timeouts-with-hbase-hbck-command-in-azure-hdinsight"></a>Szenario: Timeouts mit dem Befehl „hbase hbck“ in Azure HDInsight

In diesem Artikel werden Schritte zur Problembehandlung und mögliche Lösungen für Probleme bei der Interaktion mit Azure HDInsight-Clustern beschrieben.

## <a name="issue"></a>Problem

Timeouts mit dem Befehl `hbase hbck` beim Korrigieren von Regionszuweisungen.

## <a name="cause"></a>Ursache

Die potenzielle Ursache hierfür ist möglicherweise, dass mehrere Regionen sich für einen langen Zeitraum im Status „im Übergang“ befinden. Diese Regionen werden auf der Apache HBase Master-Benutzeroberfläche als offline angezeigt. Aufgrund der hohen Anzahl von Regionen mit anstehendem Übergang ist unter Umständen ein HBase Master-Timeout aufgetreten, und HBase Master kann diese Regionen nicht wieder online schalten.

## <a name="resolution"></a>Lösung

1. Melden Sie sich über SSH beim HDInsight HBase-Cluster an.

1. Führen Sie den Befehl `hbase zkcli` aus, um eine Verbindung mit der ZooKeeper-Shell herzustellen.

1. Führen Sie den Befehl `rmr /hbase/regions-in-transition` oder `rmr /hbase-unsecure/regions-in-transition` aus.

1. Verwenden Sie den Befehl `exit`, um die Shell `hbase zkcli` zu beenden.

1. Öffnen Sie die Ambari-Benutzeroberfläche, und starten Sie den Active HBase Master-Dienst von Ambari aus neu.

1. Überwachen Sie auf der HBase Master-Benutzeroberfläche den Abschnitt mit den Regionen, für die ein Übergang vorgesehen ist, um sich zu vergewissern, dass keine Region hängen bleibt.

## <a name="next-steps"></a>Nächste Schritte

Wenn Ihr Problem nicht aufgeführt ist oder Sie es nicht lösen können, besuchen Sie einen der folgenden Kanäle, um weitere Unterstützung zu erhalten:

* Nutzen Sie den [Azure-Communitysupport](https://azure.microsoft.com/support/community/), um Antworten von Azure-Experten zu erhalten.

* Nutzen Sie [@AzureSupport](https://twitter.com/azuresupport) – das offizielle Microsoft Azure-Konto zur Verbesserung der Benutzerfreundlichkeit. Hierüber hat die Azure-Community Zugriff auf die richtigen Ressourcen: Antworten, Support und Experten.

* Sollten Sie weitere Unterstützung benötigen, senden Sie eine Supportanfrage über das [Azure-Portal](https://portal.azure.com/?#blade/Microsoft_Azure_Support/HelpAndSupportBlade/). Wählen Sie dazu auf der Menüleiste die Option **Support** aus, oder öffnen Sie den Hub **Hilfe und Support**. Ausführlichere Informationen hierzu finden Sie unter [Erstellen einer Azure-Supportanfrage](https://docs.microsoft.com/azure/azure-supportability/how-to-create-azure-support-request). Zugang zu Abonnementverwaltung und Abrechnungssupport ist in Ihrem Microsoft Azure-Abonnement enthalten. Technischer Support wird über einen [Azure-Supportplan](https://azure.microsoft.com/support/plans/) bereitgestellt.
