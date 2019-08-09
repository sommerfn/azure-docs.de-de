---
title: Dem Azure HDInsight-Cluster können keine Knoten hinzugefügt werden.
description: Dem Azure HDInsight-Cluster können keine Knoten hinzugefügt werden.
ms.service: hdinsight
ms.topic: troubleshooting
author: hrasheed-msft
ms.author: hrasheed
ms.date: 07/31/2019
ms.openlocfilehash: ea36f8a6a8c1db031de8f6ce2b4e3a88d73907e7
ms.sourcegitcommit: 800f961318021ce920ecd423ff427e69cbe43a54
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 07/31/2019
ms.locfileid: "68700146"
---
# <a name="scenario-unable-to-add-nodes-to-azure-hdinsight-cluster"></a>Szenario: Dem Azure HDInsight-Cluster können keine Knoten hinzugefügt werden.

In diesem Artikel werden Schritte zur Problembehandlung und mögliche Lösungen für Probleme bei der Interaktion mit Azure HDInsight-Clustern beschrieben.

## <a name="issue"></a>Problem

Dem Azure HDInsight-Cluster können keine Knoten hinzugefügt werden.

## <a name="cause"></a>Ursache

Die Gründe können variieren.

## <a name="resolution"></a>Lösung

Berechnen Sie mithilfe des Features [Clustergröße](/hdinsight-scaling-best-practices.md) die Anzahl von für den Cluster erforderlichen zusätzlichen Kernen. Diese Anzahl basiert auf der Gesamtanzahl von Kernen in den neuen Workerknoten. Probieren Sie dann die folgenden Schritte aus:

* Überprüfen Sie, ob am Speicherort des Clusters Kerne verfügbar sind.

* Sehen Sie sich die Anzahl verfügbarer Kernen an anderen Speicherorten an. Ziehen Sie die Neuerstellung des Clusters an einem anderen Speicherort mit ausreichend verfügbaren Kernen in Betracht.

* Wenn Sie das Kernkontingent für einen bestimmten Speicherort erhöhen möchten, legen Sie ein Supportticket für eine Erhöhung des HDInsight-Kernkontingents an.

## <a name="next-steps"></a>Nächste Schritte

Wenn Ihr Problem nicht aufgeführt ist oder Sie es nicht lösen können, besuchen Sie einen der folgenden Kanäle, um weitere Unterstützung zu erhalten:

* Erhalten Sie über den [Azure-Communitysupport](https://azure.microsoft.com/support/community/) Antworten von Azure-Experten.

* Setzen Sie sich mit [@AzureSupport](https://twitter.com/azuresupport) in Verbindung – dem offiziellen Microsoft Azure-Konto zur Verbesserung der Benutzerfreundlichkeit. Hierüber erhält die Azure-Community Kontakt mit den richtigen Ressourcen: Antworten, Support und Experten.

* Wenn Sie weitere Unterstützung benötigen, senden Sie eine Supportanfrage über das [Azure-Portal](https://portal.azure.com/?#blade/Microsoft_Azure_Support/HelpAndSupportBlade/). Wählen Sie dazu auf der Menüleiste die Option **Support** aus, oder öffnen Sie den Hub **Hilfe und Support**. Ausführliche Informationen hierzu finden Sie unter [Erstellen einer Azure-Supportanfrage](https://docs.microsoft.com/azure/azure-supportability/how-to-create-azure-support-request). Zugang zu Abonnementverwaltung und Abrechnungssupport ist in Ihrem Microsoft Azure-Abonnement enthalten. Technischer Support wird über einen [Azure-Supportplan](https://azure.microsoft.com/support/plans/) bereitgestellt.
