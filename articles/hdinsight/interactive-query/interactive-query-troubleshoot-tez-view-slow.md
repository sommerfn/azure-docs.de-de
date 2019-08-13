---
title: Die Apache Ambari-Tez-Ansicht wird in Azure HDInsight langsam geladen
description: Die Apache Ambari-Tez-Ansicht wird möglicherweise langsam oder gar nicht geladen
ms.service: hdinsight
ms.topic: troubleshooting
author: hrasheed-msft
ms.author: hrasheed
ms.date: 07/30/2019
ms.openlocfilehash: 70e8ef1f36de30d659e09a15aa935f7a5b7693d6
ms.sourcegitcommit: bc3a153d79b7e398581d3bcfadbb7403551aa536
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 08/06/2019
ms.locfileid: "68841921"
---
# <a name="scenario-apache-ambari-tez-view-loads-slowly-in-azure-hdinsight"></a>Szenario: Die Apache Ambari-Tez-Ansicht wird in Azure HDInsight langsam geladen

In diesem Artikel werden Schritte zur Problembehandlung und mögliche Lösungen für Probleme bei der Verwendung von Interactive Query-Komponenten in Azure HDInsight-Clustern beschrieben.

## <a name="issue"></a>Problem

Die Apache Ambari-Tez-Ansicht wird möglicherweise langsam oder gar nicht geladen. Beim Laden der Ambari-Tez-Ansicht sehen Sie möglicherweise, dass Prozesse auf Hauptknoten nicht mehr reagieren.

## <a name="cause"></a>Ursache

Der Zugriff auf Yarn ATS-APIs kann manchmal zu einer schlechten Leistung bei vor dem Oktober 2017 erstellten Clustern führen, wenn auf dem Cluster eine große Anzahl von Hive-Aufträgen ausgeführt wird.

## <a name="resolution"></a>Lösung

Dieses Problem wurde im Oktober 2017 behoben. Wenn Sie Ihren Cluster neu erstellen, wird dieses Problem nicht mehr auftreten.

## <a name="next-steps"></a>Nächste Schritte

Wenn Ihr Problem nicht aufgeführt ist oder Sie es nicht lösen können, besuchen Sie einen der folgenden Kanäle, um weitere Unterstützung zu erhalten:

* Erhalten Sie über den [Azure-Communitysupport](https://azure.microsoft.com/support/community/) Antworten von Azure-Experten.

* Setzen Sie sich mit [@AzureSupport](https://twitter.com/azuresupport) in Verbindung – dem offiziellen Microsoft Azure-Konto zur Verbesserung der Benutzerfreundlichkeit. Hierüber erhält die Azure-Community Kontakt mit den richtigen Ressourcen: Antworten, Support und Experten.

* Wenn Sie weitere Unterstützung benötigen, senden Sie eine Supportanfrage über das [Azure-Portal](https://portal.azure.com/?#blade/Microsoft_Azure_Support/HelpAndSupportBlade/). Wählen Sie dazu auf der Menüleiste die Option **Support** aus, oder öffnen Sie den Hub **Hilfe und Support**. Ausführliche Informationen hierzu finden Sie unter [Erstellen einer Azure-Supportanfrage](https://docs.microsoft.com/azure/azure-supportability/how-to-create-azure-support-request). Zugang zu Abonnementverwaltung und Abrechnungssupport ist in Ihrem Microsoft Azure-Abonnement enthalten. Technischer Support wird über einen [Azure-Supportplan](https://azure.microsoft.com/support/plans/) bereitgestellt.
