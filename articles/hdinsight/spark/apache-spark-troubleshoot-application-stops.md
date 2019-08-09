---
title: Apache Spark-Streaminganwendung beendet die Verarbeitung von Daten nach der Ausführung für 24 Tage ohne bekannte Fehler in den Protokollen in Azure HDInsight
description: Eine Apache Spark-Streaminganwendung wird beendet, nachdem sie 24 Tage lang ausgeführt wurde. In den Protokolldateien werden keine Fehler angezeigt.
ms.service: hdinsight
ms.topic: troubleshooting
author: hrasheed-msft
ms.author: hrasheed
ms.date: 07/29/2019
ms.openlocfilehash: 002c45c514b5d8207a1aa70ec8e1c749677a239a
ms.sourcegitcommit: 08d3a5827065d04a2dc62371e605d4d89cf6564f
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 07/29/2019
ms.locfileid: "68620618"
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

* Erhalten Sie über den [Azure-Communitysupport](https://azure.microsoft.com/support/community/) Antworten von Azure-Experten.

* Setzen Sie sich mit [@AzureSupport](https://twitter.com/azuresupport) in Verbindung – dem offiziellen Microsoft Azure-Konto zur Verbesserung der Benutzerfreundlichkeit. Hierüber erhält die Azure-Community Kontakt mit den richtigen Ressourcen: Antworten, Support und Experten.

* Wenn Sie weitere Unterstützung benötigen, senden Sie eine Supportanfrage über das [Azure-Portal](https://portal.azure.com/?#blade/Microsoft_Azure_Support/HelpAndSupportBlade/). Wählen Sie dazu auf der Menüleiste die Option **Support** aus, oder öffnen Sie den Hub **Hilfe und Support**. Ausführliche Informationen hierzu finden Sie unter [Erstellen einer Azure-Supportanfrage](https://docs.microsoft.com/azure/azure-supportability/how-to-create-azure-support-request). Zugang zu Abonnementverwaltung und Abrechnungssupport ist in Ihrem Microsoft Azure-Abonnement enthalten. Technischer Support wird über einen [Azure-Supportplan](https://azure.microsoft.com/support/plans/) bereitgestellt.
