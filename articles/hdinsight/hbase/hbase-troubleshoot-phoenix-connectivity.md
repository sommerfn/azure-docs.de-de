---
title: Konnektivitätsprobleme bei Apache Phoenix in Azure HDInsight
description: Konnektivitätsprobleme bei Apache Phoenix in Azure HDInsight
ms.service: hdinsight
ms.topic: troubleshooting
author: hrasheed-msft
ms.author: hrasheed
ms.date: 08/07/2019
ms.openlocfilehash: 641d622377bad7a1239efd526b93c6f0f0c08d4a
ms.sourcegitcommit: aa042d4341054f437f3190da7c8a718729eb675e
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 08/09/2019
ms.locfileid: "68886585"
---
# <a name="scenario-apache-phoenix-connectivity-issues-in-azure-hdinsight"></a>Szenario: Konnektivitätsprobleme bei Apache Phoenix in Azure HDInsight

In diesem Artikel werden Schritte zur Problembehandlung und mögliche Lösungen für Probleme bei der Interaktion mit Azure HDInsight-Clustern beschrieben.

## <a name="issue"></a>Problem

Verbindung von Apache Phoenix mit Apache HBase kann nicht hergestellt werden Die Gründe können variieren.

## <a name="cause-incorrect-ip"></a>Ursache: Falsche IP

Falsche IP-Adresse des aktiven ZooKeeper-Knotens.

### <a name="resolution"></a>Lösung

Die IP-Adresse des aktiven ZooKeeper-Knotens kann auf der Ambari-Benutzeroberfläche über die Links zu **HBase > Quick Links > ZK***  **(Active) > Zookeeper Info** ermittelt werden. Korrigieren Sie sie bei Bedarf.

---

## <a name="cause-systemcatalog-table-offline"></a>Ursache: Die Tabelle „SYSTEM.CATALOG“ ist offline.

Wenn Sie Befehle wie `!tables` ausführen, erhalten Sie eine Fehlermeldung ähnlich der folgenden:

```
Error while connecting to sqlline.py (Hbase - phoenix) Setting property: [isolation, TRANSACTION_READ_COMMITTED] issuing: !connect jdbc:phoenix:10.2.0.7 none none org.apache.phoenix.jdbc.PhoenixDriver Connecting to jdbc:phoenix:10.2.0.7 SLF4J: Class path contains multiple SLF4J bindings.
```

Wenn Sie Befehle wie `count 'SYSTEM.CATALOG'` ausführen, erhalten Sie eine Fehlermeldung ähnlich der folgenden:

```
ERROR: org.apache.hadoop.hbase.NotServingRegionException: Region SYSTEM.CATALOG,,1485464083256.c0568c94033870c517ed36c45da98129. is not online on 10.2.0.5,16020,1489466172189)
```

### <a name="resolution"></a>Lösung

Starten Sie den HMaster-Dienst auf allen ZooKeeper-Knoten von der Ambari-Benutzeroberfläche aus neu.

1. Wechseln Sie im zusammenfassenden Abschnitt von HBase zum Link **HBase > Active HBase-Master** (Aktiver HBase Master).

1. Starten Sie im Abschnitt **Components** den HBase Master-Dienst neu.

1. Wiederholen Sie die oben genannten Schritte für die verbleibenden **Standby HBase Master**-Dienste.

Es kann bis zu fünf Minuten dauern, bis der HBase Master-Dienst stabilisiert und die Wiederherstellung beendet ist. Sobald die Tabelle `SYSTEM.CATALOG` wieder in einem normalem Zustand ist, sollte das Konnektivitätsproblem mit Apache Phoenix automatisch gelöst werden.

## <a name="next-steps"></a>Nächste Schritte

Wenn Ihr Problem nicht aufgeführt ist oder Sie es nicht lösen können, besuchen Sie einen der folgenden Kanäle, um weitere Unterstützung zu erhalten:

* Erhalten Sie über den [Azure-Communitysupport](https://azure.microsoft.com/support/community/) Antworten von Azure-Experten.

* Herstellen einer Verbindung mit [@AzureSupport](https://twitter.com/azuresupport), dem offiziellen Microsoft Azure-Konto zum Verbessern der Kundenfreundlichkeit. Verbinden der Azure-Community mit den richtigen Ressourcen: Antworten, Support und Experten.

* Wenn Sie weitere Unterstützung benötigen, senden Sie eine Supportanfrage über das [Azure-Portal](https://portal.azure.com/?#blade/Microsoft_Azure_Support/HelpAndSupportBlade/). Wählen Sie dazu auf der Menüleiste die Option **Support** aus, oder öffnen Sie den Hub **Hilfe und Support**. Ausführlichere Informationen hierzu finden Sie unter [Erstellen einer Azure-Supportanfrage](https://docs.microsoft.com/azure/azure-supportability/how-to-create-azure-support-request). Zugang zu Abonnementverwaltung und Abrechnungssupport ist in Ihrem Microsoft Azure-Abonnement enthalten. Technischer Support wird über einen [Azure-Supportplan](https://azure.microsoft.com/support/plans/) bereitgestellt.
