---
title: Eine Apache Spark-Streaminganwendung wird nach 24 Tagen in Azure HDInsight beendet
description: Eine Apache Spark-Streaminganwendung wird beendet, nachdem sie 24 Tage lang ausgeführt wurde. In den Protokolldateien werden keine Fehler angezeigt.
ms.service: hdinsight
ms.topic: troubleshooting
author: hrasheed-msft
ms.author: hrasheed
ms.reviewer: jasonh
ms.date: 07/29/2019
ms.openlocfilehash: 946c54f6c1048c5f22b520333daf0da47740f2e4
ms.sourcegitcommit: c79aa93d87d4db04ecc4e3eb68a75b349448cd17
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 09/18/2019
ms.locfileid: "71087208"
---
# <a name="scenario-apache-spark-streaming-application-stops-after-executing-for-24-days-in-azure-hdinsight"></a>Szenario: Eine Apache Spark-Streaminganwendung wird beendet, nachdem sie 24 Tage lang in Azure HDInsight ausgeführt wurde.

In diesem Artikel werden Schritte zur Problembehandlung und mögliche Lösungen für Probleme bei der Verwendung von Apache Spark-Komponenten in Azure HDInsight-Clustern beschrieben.

## <a name="issue"></a>Problem

Eine Apache Spark-Streaminganwendung wird beendet, nachdem sie 24 Tage lang ausgeführt wurde. In den Protokolldateien werden keine Fehler angezeigt.

## <a name="cause"></a>Ursache

Der Wert von `livy.server.session.timeout` steuert, wie lange Apache Livy warten soll, bis eine Sitzung abgeschlossen wird. Wenn die Sitzungslänge den `session.timeout`-Wert erreicht hat, werden die Livy-Sitzung und die Anwendung automatisch beendet.

## <a name="resolution"></a>Lösung

Erhöhen Sie für zeitintensive Aufträge den Wert `livy.server.session.timeout` auf der Ambari-Benutzeroberfläche. Mithilfe der URL `https://<yourclustername>.azurehdinsight.net/#/main/services/LIVY/configs` können Sie über die Ambari-Benutzeroberfläche auf die Livy-Konfiguration zugreifen.

Ersetzen Sie `<yourclustername>` durch den Namen Ihres HDInsight-Clusters, wie er im Portal angezeigt wird.

## <a name="next-steps"></a>Nächste Schritte

Wenn Ihr Problem nicht aufgeführt ist oder Sie es nicht lösen können, besuchen Sie einen der folgenden Kanäle, um weitere Unterstützung zu erhalten:

* Nutzen Sie den [Azure-Communitysupport](https://azure.microsoft.com/support/community/), um Antworten von Azure-Experten zu erhalten.

* Nutzen Sie [@AzureSupport](https://twitter.com/azuresupport) – das offizielle Microsoft Azure-Konto zur Verbesserung der Benutzerfreundlichkeit. Hierüber hat die Azure-Community Zugriff auf die richtigen Ressourcen: Antworten, Support und Experten.

* Sollten Sie weitere Unterstützung benötigen, senden Sie eine Supportanfrage über das [Azure-Portal](https://portal.azure.com/?#blade/Microsoft_Azure_Support/HelpAndSupportBlade/). Wählen Sie dazu auf der Menüleiste die Option **Support** aus, oder öffnen Sie den Hub **Hilfe und Support**. Ausführlichere Informationen hierzu finden Sie unter [Erstellen einer Azure-Supportanfrage](https://docs.microsoft.com/azure/azure-supportability/how-to-create-azure-support-request). Zugang zu Abonnementverwaltung und Abrechnungssupport ist in Ihrem Microsoft Azure-Abonnement enthalten. Technischer Support wird über einen [Azure-Supportplan](https://azure.microsoft.com/support/plans/) bereitgestellt.
