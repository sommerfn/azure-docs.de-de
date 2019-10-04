---
title: Timeout der Apache Hive-Ansicht beim Abrufen eines Abfrageergebnisses in Azure HDInsight
description: Timeout der Apache Hive-Ansicht beim Abrufen eines Abfrageergebnisses in Azure HDInsight
ms.service: hdinsight
ms.topic: troubleshooting
author: hrasheed-msft
ms.author: hrasheed
ms.reviewer: jasonh
ms.date: 07/30/2019
ms.openlocfilehash: 3e61103fceb668a9c8be381d1c4cc13efdc421f9
ms.sourcegitcommit: c79aa93d87d4db04ecc4e3eb68a75b349448cd17
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 09/18/2019
ms.locfileid: "71091254"
---
# <a name="scenario-apache-hive-view-times-out-when-fetching-a-query-result-in-azure-hdinsight"></a>Szenario: Timeout der Apache Hive-Ansicht beim Abrufen eines Abfrageergebnisses in Azure HDInsight

Dieser Artikel beschreibt Schritte zur Problembehandlung sowie mögliche Lösungen für Probleme bei der Verwendung von Interactive Query-Komponenten in Azure HDInsight-Clustern.

## <a name="issue"></a>Problem

Beim Ausführen bestimmter Abfragen über die Apache Hive-Ansicht tritt unter Umständen der folgende Fehler auf:

```
result fetch timed out
java.util.concurrent.TimeoutException: deadline passed
```

## <a name="cause"></a>Ursache

Der Standardtimeoutwert für die Hive-Ansicht eignet sich möglicherweise nicht für die von Ihnen ausgeführte Abfrage. Der angegebene Zeitraum ist zu kurz, um das Abfrageergebnis über die Hive-Ansicht abzurufen.

## <a name="resolution"></a>Lösung

Erhöhen Sie die Timeouts für die Apache Ambari Hive-Ansicht, indem Sie die folgenden Eigenschaften in `/etc/ambari-server/conf/ambari.properties` festlegen:

```
views.ambari.request.read.timeout.millis=300000
views.request.read.timeout.millis=300000
views.ambari.hive<HIVE_VIEW_INSTANCE_NAME>.result.fetch.timeout=300000
```

Der Wert von `HIVE_VIEW_INSTANCE_NAME` ist am Ende der URL für die Hive-Ansicht verfügbar.

## <a name="next-steps"></a>Nächste Schritte

Wenn Ihr Problem nicht aufgeführt ist oder Sie es nicht lösen können, besuchen Sie einen der folgenden Kanäle, um weitere Unterstützung zu erhalten:

* Nutzen Sie den [Azure-Communitysupport](https://azure.microsoft.com/support/community/), um Antworten von Azure-Experten zu erhalten.

* Nutzen Sie [@AzureSupport](https://twitter.com/azuresupport) – das offizielle Microsoft Azure-Konto zur Verbesserung der Benutzerfreundlichkeit. Hierüber hat die Azure-Community Zugriff auf die richtigen Ressourcen: Antworten, Support und Experten.

* Sollten Sie weitere Unterstützung benötigen, senden Sie eine Supportanfrage über das [Azure-Portal](https://portal.azure.com/?#blade/Microsoft_Azure_Support/HelpAndSupportBlade/). Wählen Sie dazu auf der Menüleiste die Option **Support** aus, oder öffnen Sie den Hub **Hilfe und Support**. Ausführlichere Informationen hierzu finden Sie unter [Erstellen einer Azure-Supportanfrage](https://docs.microsoft.com/azure/azure-supportability/how-to-create-azure-support-request). Zugang zu Abonnementverwaltung und Abrechnungssupport ist in Ihrem Microsoft Azure-Abonnement enthalten. Technischer Support wird über einen [Azure-Supportplan](https://azure.microsoft.com/support/plans/) bereitgestellt.
