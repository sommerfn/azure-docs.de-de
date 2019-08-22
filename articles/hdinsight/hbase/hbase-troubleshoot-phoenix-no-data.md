---
title: HDP-Upgrade bewirkt das Fehlen von Daten in Apache Phoenix-Sichten in Azure HDInsight
description: HDP-Upgrade bewirkt das Fehlen von Daten in Apache Phoenix-Sichten in Azure HDInsight
ms.service: hdinsight
ms.topic: troubleshooting
author: hrasheed-msft
ms.author: hrasheed
ms.date: 08/08/2019
ms.openlocfilehash: e42ab97b4508a606781af76f8dd5927b1a14a8b3
ms.sourcegitcommit: 124c3112b94c951535e0be20a751150b79289594
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 08/10/2019
ms.locfileid: "68947925"
---
# <a name="scenario-hdp-upgrade-causes-no-data-in-apache-phoenix-views-in-azure-hdinsight"></a>Szenario: HDP-Upgrade bewirkt das Fehlen von Daten in Apache Phoenix-Sichten in Azure HDInsight

In diesem Artikel werden Schritte zur Problembehandlung und mögliche Lösungen für Probleme bei der Interaktion mit Azure HDInsight-Clustern beschrieben.

## <a name="issue"></a>Problem

Eine Apache Phoenix-Sicht enthält nach dem Upgrade von HDP 2.4 auf HDP 2.5 keine Daten.

## <a name="cause"></a>Ursache

Die Indextabelle für Sichten (alle Indizes für die Sicht werden in einer einzigen physischen Apache HBase-Tabelle gespeichert) wird beim Upgrade abgeschnitten.

## <a name="resolution"></a>Lösung

Löschen Sie alle Sichten nach dem Upgrade, und erstellen Sie sie neu.

## <a name="next-steps"></a>Nächste Schritte

Wenn Ihr Problem nicht aufgeführt ist oder Sie es nicht lösen können, besuchen Sie einen der folgenden Kanäle, um weitere Unterstützung zu erhalten:

* Erhalten Sie über den [Azure-Communitysupport](https://azure.microsoft.com/support/community/) Antworten von Azure-Experten.

* Herstellen einer Verbindung mit [@AzureSupport](https://twitter.com/azuresupport), dem offiziellen Microsoft Azure-Konto zum Verbessern der Kundenfreundlichkeit. Verbinden der Azure-Community mit den richtigen Ressourcen: Antworten, Support und Experten.

* Wenn Sie weitere Unterstützung benötigen, senden Sie eine Supportanfrage über das [Azure-Portal](https://portal.azure.com/?#blade/Microsoft_Azure_Support/HelpAndSupportBlade/). Wählen Sie dazu auf der Menüleiste die Option **Support** aus, oder öffnen Sie den Hub **Hilfe und Support**. Ausführlichere Informationen hierzu finden Sie unter [Erstellen einer Azure-Supportanfrage](https://docs.microsoft.com/azure/azure-supportability/how-to-create-azure-support-request). Zugang zu Abonnementverwaltung und Abrechnungssupport ist in Ihrem Microsoft Azure-Abonnement enthalten. Technischer Support wird über einen [Azure-Supportplan](https://azure.microsoft.com/support/plans/) bereitgestellt.
