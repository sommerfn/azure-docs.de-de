---
title: 'Jupyter 404-Fehler: „Blockieren der ursprungsübergreifenden API“ – Azure HDInsight'
description: Fehler 404 ("Nicht gefunden") auf Jupyter-Server aufgrund von blockierender ursprungsübergreifender API in Azure HDInsight
ms.service: hdinsight
ms.topic: troubleshooting
author: hrasheed-msft
ms.author: hrasheed
ms.reviewer: jasonh
ms.date: 07/29/2019
ms.openlocfilehash: 7f8eb000533673d243258d569834869bbb2a1778
ms.sourcegitcommit: 3486e2d4eb02d06475f26fbdc321e8f5090a7fac
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 10/31/2019
ms.locfileid: "73241127"
---
# <a name="scenario-jupyter-server-404-not-found-error-due-to-blocking-cross-origin-api-in-azure-hdinsight"></a>Szenario: Fehler 404 ("Nicht gefunden") auf Jupyter-Server aufgrund von blockierender ursprungsübergreifender API in Azure HDInsight

Dieser Artikel beschreibt Schritte zur Problembehandlung und mögliche Lösungen für Probleme bei der Verwendung von Apache Spark-Komponenten in Azure HDInsight-Clustern.

## <a name="issue"></a>Problem

Wenn Sie auf den Jupyter-Dienst in HDInsight zugreifen, wird Ihnen die Fehlermeldung „Nicht gefunden“ angezeigt. Wenn Sie die Jupyter-Protokolle überprüfen, sehen Sie dort eine Ausgabe ähnlich der Folgenden:

```log
[W 2018-08-21 17:43:33.352 NotebookApp] 404 PUT /api/contents/PySpark/notebook.ipynb (10.16.0.144) 4504.03ms referer=https://pnhr01hdi-corpdir.msappproxy.net/jupyter/notebooks/PySpark/notebook.ipynb
Blocking Cross Origin API request.  
Origin: https://xxx.xxx.xxx, Host: hn0-pnhr01.j101qxjrl4zebmhb0vmhg044xe.ax.internal.cloudapp.net:8001
```

Möglicherweise finden Sie im Jupyter-Protokoll im Feld „Origin“ auch eine IP-Adresse.

## <a name="cause"></a>Ursache

Dieser Fehler kann verschiedene Ursachen haben:

- Sie haben Netzwerksicherheitsgruppen-Regeln (Network Security Group, NSG) konfiguriert, die den Zugriff auf den Cluster einschränken. Auch wenn Sie den Zugriff mithilfe von NSG-Regeln eingeschränkt haben, können Sie weiterhin direkt auf Apache Ambari und andere Dienste zugreifen, indem Sie die IP-Adresse anstelle des Clusternamens verwenden. Beim Zugriff auf Jupyter wird dennoch möglicherweise der Fehler 404 („Nicht gefunden“) angezeigt.

- Sie haben für Ihr HDInsight-Gateway einen benutzerdefinierten DNS-Namen verwendet, der vom Standardformat `xxx.azurehdinsight.net` abweicht.

## <a name="resolution"></a>Lösung

1. Ändern Sie die jupyter.py-Dateien an den folgenden beiden Stellen:

    ```bash
    /var/lib/ambari-server/resources/common-services/JUPYTER/1.0.0/package/scripts/jupyter.py
    /var/lib/ambari-agent/cache/common-services/JUPYTER/1.0.0/package/scripts/jupyter.py
    ```

1. Suchen Sie folgende Zeile: `NotebookApp.allow_origin='\"https://{2}.{3}\"'`. Ändern Sie diese zu `NotebookApp.allow_origin='\"*\"'`.

1. Starten Sie den Jupyter-Dienst über Ambari neu.

1. Wenn Sie `ps aux | grep jupyter` an der Eingabeaufforderung eingeben, sollte angezeigt werden, dass das Herstellen einer Verbindung über eine beliebige URL zulässig ist.

Dies ist weniger sicher als die vorherige Einstellung. Es wird jedoch davon ausgegangen, dass der Zugriff auf den Cluster eingeschränkt ist und dass das Herstellen einer Verbindung mit dem Cluster von außen zulässig ist, weil eine NSG eingerichtet ist.

## <a name="next-steps"></a>Nächste Schritte

Wenn Ihr Problem nicht aufgeführt ist oder Sie es nicht lösen können, besuchen Sie einen der folgenden Kanäle, um weitere Unterstützung zu erhalten:

* Nutzen Sie den [Azure-Communitysupport](https://azure.microsoft.com/support/community/), um Antworten von Azure-Experten zu erhalten.

* Nutzen Sie [@AzureSupport](https://twitter.com/azuresupport) – das offizielle Microsoft Azure-Konto zur Verbesserung der Benutzerfreundlichkeit. Hierüber hat die Azure-Community Zugriff auf die richtigen Ressourcen: Antworten, Support und Experten.

* Sollten Sie weitere Unterstützung benötigen, senden Sie eine Supportanfrage über das [Azure-Portal](https://portal.azure.com/?#blade/Microsoft_Azure_Support/HelpAndSupportBlade/). Wählen Sie dazu auf der Menüleiste die Option **Support** aus, oder öffnen Sie den Hub **Hilfe und Support**. Ausführlichere Informationen hierzu finden Sie unter [Erstellen einer Azure-Supportanfrage](https://docs.microsoft.com/azure/azure-supportability/how-to-create-azure-support-request). Zugang zu Abonnementverwaltung und Abrechnungssupport ist in Ihrem Microsoft Azure-Abonnement enthalten. Technischer Support wird über einen [Azure-Supportplan](https://azure.microsoft.com/support/plans/) bereitgestellt.
