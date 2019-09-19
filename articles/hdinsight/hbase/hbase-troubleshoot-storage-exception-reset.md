---
title: Speicherausnahme nach dem Zurücksetzen der Verbindung in Azure HDInsight
description: Speicherausnahme nach dem Zurücksetzen der Verbindung in Azure HDInsight
ms.service: hdinsight
ms.topic: troubleshooting
author: hrasheed-msft
ms.author: hrasheed
ms.reviewer: jasonh
ms.date: 08/08/2019
ms.openlocfilehash: 52135391024eafdfea15afd6c05a5d13bf92a2c7
ms.sourcegitcommit: c79aa93d87d4db04ecc4e3eb68a75b349448cd17
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 09/18/2019
ms.locfileid: "71091585"
---
# <a name="scenario-storage-exception-after-connection-reset-in-azure-hdinsight"></a>Szenario: Speicherausnahme nach dem Zurücksetzen der Verbindung in Azure HDInsight

In diesem Artikel werden Schritte zur Problembehandlung und mögliche Lösungen für Probleme bei der Interaktion mit Azure HDInsight-Clustern beschrieben.

## <a name="issue"></a>Problem

Die neue Apache HBase-Tabelle kann nicht erstellt werden.

## <a name="cause"></a>Ursache

Beim Abschneiden einer Tabelle ist ein Speicherverbindungsproblem aufgetreten. Der Tabelleneintrag wurde in der HBase-Metadatentabelle gelöscht. Alles bis auf eine Blobdatei wurde gelöscht.

Obwohl kein Ordnerblob mit dem Namen `/hbase/data/default/ThatTable` im Speicher vorhanden ist, hat der WASB-Treiber die genannte Blobdatei erkannt und lässt die Erstellung des Blobs `/hbase/data/default/ThatTable` nicht zu, da er annimmt, dass die übergeordneten Ordner vorhanden sind. Daher tritt beim Erstellen der Tabelle ein Fehler auf.

## <a name="resolution"></a>Lösung

1. Starten Sie den aktiven HMaster über die Apache Ambari-Benutzeroberfläche neu. Dadurch wird einer der beiden Standby-HMaster aktiv, und der neue aktive HMaster lädt die Metadatentabellen-Informationen neu. Aus diesem Grund wird die Tabelle `already-deleted` nicht auf der HMaster-Benutzeroberfläche angezeigt.

1. Sie können die verwaiste Blobdatei mithilfe von Benutzeroberflächentools wie Cloud-Explorer oder durch Ausführen eines Befehls wie `hdfs dfs -ls /xxxxxx/yyyyy` suchen. Führen Sie `hdfs dfs -rmr /xxxxx/yyyy` aus, um dieses Blob zu löschen. Beispiel: `hdfs dfs -rmr /hbase/data/default/ThatTable/ThatFile`.

Nun können Sie in HBase eine neue Tabelle mit demselben Namen erstellen.

## <a name="next-steps"></a>Nächste Schritte

Wenn Ihr Problem nicht aufgeführt ist oder Sie es nicht lösen können, besuchen Sie einen der folgenden Kanäle, um weitere Unterstützung zu erhalten:

* Nutzen Sie den [Azure-Communitysupport](https://azure.microsoft.com/support/community/), um Antworten von Azure-Experten zu erhalten.

* Herstellen einer Verbindung mit [@AzureSupport](https://twitter.com/azuresupport), dem offiziellen Microsoft Azure-Konto zum Verbessern der Kundenfreundlichkeit. Verbinden der Azure-Community mit den richtigen Ressourcen: Antworten, Support und Experten.

* Sollten Sie weitere Unterstützung benötigen, senden Sie eine Supportanfrage über das [Azure-Portal](https://portal.azure.com/?#blade/Microsoft_Azure_Support/HelpAndSupportBlade/). Wählen Sie dazu auf der Menüleiste die Option **Support** aus, oder öffnen Sie den Hub **Hilfe und Support**. Ausführlichere Informationen hierzu finden Sie unter [Erstellen einer Azure-Supportanfrage](https://docs.microsoft.com/azure/azure-supportability/how-to-create-azure-support-request). Zugang zu Abonnementverwaltung und Abrechnungssupport ist in Ihrem Microsoft Azure-Abonnement enthalten. Technischer Support wird über einen [Azure-Supportplan](https://azure.microsoft.com/support/plans/) bereitgestellt.
