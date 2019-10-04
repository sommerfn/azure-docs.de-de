---
title: Anmeldung beim Azure HDInsight-Cluster nicht möglich
description: Problembehandlung, wenn das Anmelden beim Apache Hadoop-Cluster in Azure HDInsight nicht möglich ist
ms.service: hdinsight
ms.topic: troubleshooting
author: hrasheed-msft
ms.author: hrasheed
ms.reviewer: jasonh
ms.date: 07/31/2019
ms.openlocfilehash: c4abc8a1b8d81f4ed0ef7e1c3b37c4e0e755c1b8
ms.sourcegitcommit: c79aa93d87d4db04ecc4e3eb68a75b349448cd17
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 09/18/2019
ms.locfileid: "71087262"
---
# <a name="scenario-unable-to-log-into-azure-hdinsight-cluster"></a>Szenario: Anmeldung beim Azure HDInsight-Cluster nicht möglich

In diesem Artikel werden Schritte zur Problembehandlung und mögliche Lösungen für Probleme bei der Interaktion mit Azure HDInsight-Clustern beschrieben.

## <a name="issue"></a>Problem

Anmeldung beim Azure HDInsight-Cluster nicht möglich.

## <a name="cause"></a>Ursache

Die Gründe können variieren. Verwenden Sie für die Anmeldung beim Cluster oder bei den App-Dashboards Ihre „Clusteranmeldedaten“ oder HTTP-Anmeldeinformationen. Bei einer Remoteverbindung verwenden Sie Ihre Secure Shell (SSH)- oder Remotedesktop-Anmeldeinformationen.

## <a name="resolution"></a>Lösung

Probieren Sie zum Beheben allgemeiner Probleme die folgenden Schritte aus:

* Versuchen Sie, das Clusterdashboard auf einer neuen Browserregisterkarte im Datenschutzmodus zu öffnen.

* Falls Sie sich nicht mehr an Ihre SSH-Anmeldeinformationen erinnern, können Sie die [Anmeldeinformationen auf der Ambari-Benutzeroberfläche zurücksetzen](../hdinsight-administer-use-portal-linux.md#change-passwords).

## <a name="next-steps"></a>Nächste Schritte

Wenn Ihr Problem nicht aufgeführt ist oder Sie es nicht lösen können, besuchen Sie einen der folgenden Kanäle, um weitere Unterstützung zu erhalten:

* Nutzen Sie den [Azure-Communitysupport](https://azure.microsoft.com/support/community/), um Antworten von Azure-Experten zu erhalten.

* Nutzen Sie [@AzureSupport](https://twitter.com/azuresupport) – das offizielle Microsoft Azure-Konto zur Verbesserung der Benutzerfreundlichkeit. Hierüber hat die Azure-Community Zugriff auf die richtigen Ressourcen: Antworten, Support und Experten.

* Sollten Sie weitere Unterstützung benötigen, senden Sie eine Supportanfrage über das [Azure-Portal](https://portal.azure.com/?#blade/Microsoft_Azure_Support/HelpAndSupportBlade/). Wählen Sie dazu auf der Menüleiste die Option **Support** aus, oder öffnen Sie den Hub **Hilfe und Support**. Ausführlichere Informationen hierzu finden Sie unter [Erstellen einer Azure-Supportanfrage](https://docs.microsoft.com/azure/azure-supportability/how-to-create-azure-support-request). Zugang zu Abonnementverwaltung und Abrechnungssupport ist in Ihrem Microsoft Azure-Abonnement enthalten. Technischer Support wird über einen [Azure-Supportplan](https://azure.microsoft.com/support/plans/) bereitgestellt.
