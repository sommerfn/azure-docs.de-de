---
title: Apache Tez-Anwendung reagiert in Azure HDInsight nicht mehr
description: Apache Tez-Anwendung reagiert in Azure HDInsight nicht mehr
ms.service: hdinsight
ms.topic: troubleshooting
author: hrasheed-msft
ms.author: hrasheed
ms.date: 08/09/2019
ms.openlocfilehash: fa56667c039133700c100b3e01a56ad784d16a6c
ms.sourcegitcommit: 62bd5acd62418518d5991b73a16dca61d7430634
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 08/13/2019
ms.locfileid: "68976872"
---
# <a name="scenario-apache-tez-application-hangs-in-azure-hdinsight"></a>Szenario: Apache Tez-Anwendung reagiert in Azure HDInsight nicht mehr

In diesem Artikel werden Schritte zur Problembehandlung und mögliche Lösungen für Probleme bei der Interaktion mit Azure HDInsight-Clustern beschrieben.

## <a name="issue"></a>Problem

Nach dem Übermitteln eines Apache Hive-Auftrags wird in der Tez-Ansicht der Auftragsstatus „Wird ausgeführt“ angezeigt, es scheint jedoch kein Fortschritt mehr stattzufinden.

## <a name="cause"></a>Ursache

Es wurden zu viele Aufträge übermittelt, die Yarn-Warteschlange ist lang.

## <a name="resolution"></a>Lösung

Skalieren Sie den Cluster zentral hoch, oder warten Sie, bis die Yarn-Warteschlange abgenommen hat.

Standardmäßig steuert `yarn.scheduler.capacity.maximum-applications` die maximale Anzahl der ausgeführten und ausstehenden Anwendungen. Der Standardwert ist `10000`.

## <a name="next-steps"></a>Nächste Schritte

Wenn Ihr Problem nicht aufgeführt ist oder Sie es nicht lösen können, besuchen Sie einen der folgenden Kanäle, um weitere Unterstützung zu erhalten:

* Erhalten Sie über den [Azure-Communitysupport](https://azure.microsoft.com/support/community/) Antworten von Azure-Experten.

* Herstellen einer Verbindung mit [@AzureSupport](https://twitter.com/azuresupport), dem offiziellen Microsoft Azure-Konto zum Verbessern der Kundenfreundlichkeit. Verbinden der Azure-Community mit den richtigen Ressourcen: Antworten, Support und Experten.

* Wenn Sie weitere Unterstützung benötigen, senden Sie eine Supportanfrage über das [Azure-Portal](https://portal.azure.com/?#blade/Microsoft_Azure_Support/HelpAndSupportBlade/). Wählen Sie dazu auf der Menüleiste die Option **Support** aus, oder öffnen Sie den Hub **Hilfe und Support**. Ausführlichere Informationen hierzu finden Sie unter [Erstellen einer Azure-Supportanfrage](https://docs.microsoft.com/azure/azure-supportability/how-to-create-azure-support-request). Zugang zu Abonnementverwaltung und Abrechnungssupport ist in Ihrem Microsoft Azure-Abonnement enthalten. Technischer Support wird über einen [Azure-Supportplan](https://azure.microsoft.com/support/plans/) bereitgestellt.
