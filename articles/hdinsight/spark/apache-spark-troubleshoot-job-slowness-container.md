---
title: Langsame Ausführung eines Apache Spark-Auftrags, wenn der Azure-Speichercontainer viele Dateien in Azure HDInsight enthält
description: Langsame Ausführung eines Apache Spark-Auftrags, wenn der Azure-Speichercontainer viele Dateien in Azure HDInsight enthält
ms.service: hdinsight
ms.topic: troubleshooting
author: hrasheed-msft
ms.author: hrasheed
ms.date: 08/21/2019
ms.openlocfilehash: 635b7adb8753b7e9490e8f14a0699c09297fdbbb
ms.sourcegitcommit: d3dced0ff3ba8e78d003060d9dafb56763184d69
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 08/22/2019
ms.locfileid: "69899094"
---
# <a name="scenario-apache-spark-job-run-slowly-when-the-azure-storage-container-contains-many-files-in-azure-hdinsight"></a>Szenario: Langsame Ausführung eines Apache Spark-Auftrags, wenn der Azure-Speichercontainer viele Dateien in Azure HDInsight enthält

Dieser Artikel beschreibt Schritte zur Problembehandlung sowie mögliche Lösungen für Probleme bei der Verwendung von Apache Spark-Komponenten in Azure HDInsight-Clustern.

## <a name="issue"></a>Problem

Wenn Sie einen HDInsight-Cluster ausführen, wird der Apache Spark-Auftrag, der in den Azure-Speichercontainer schreibt, langsam, wenn eine große Anzahl von Dateien/Unterordnern vorhanden ist. Schreibvorgänge in einem neuen Container dauern beispielsweise 20 Sekunden, in einem Container mit 200.000 Dateien dagegen etwa zwei Minuten.

## <a name="cause"></a>Ursache

Dies ist ein bekanntes Spark-Problem. Die Verlangsamung hängt mit den Vorgängen `ListBlob` und `GetBlobProperties` im Rahmen der Ausführung von Spark-Aufträgen zusammen.

Zur Nachverfolgung von Partitionen benötigt Spark einen Dateistatuscache (`FileStatusCache`) mit Informationen zur Verzeichnisstruktur. Anhand dieses Caches kann Spark die Pfade analysieren und die verfügbaren Partitionen berücksichtigen. Der Vorteil der Partitionsnachverfolgung besteht darin, dass Spark beim Lesen von Daten nur auf die erforderlichen Dateien zugreift. Um diese Informationen auf dem neuesten Stand zu halten, muss Spark beim Schreiben neuer Daten alle Dateien im Verzeichnis auflisten und diesen Cache aktualisieren.

In Spark 2.1 muss der Cache zwar nicht nach jedem Schreibvorgang aktualisiert werden, Spark überprüft jedoch, ob eine bereits vorhandene Partitionsspalte mit der in der aktuellen Schreibanforderung vorgeschlagenen Spalte übereinstimmt, was ebenfalls Auflistungsvorgänge zu Beginn jedes Schreibvorgangs zur Folge hat.

In Spark 2.2 wird dieses Leistungsproblem behoben, indem Daten im Anfügemodus geschrieben werden.

## <a name="resolution"></a>Lösung

Wenn Sie ein partitioniertes Dataset erstellen, ist es wichtig, ein Partitionierungsschema zu verwenden, das die Anzahl von Dateien einschränkt, die von Spark aufgelistet werden müssen, um den Dateistatuscache (`FileStatusCache`) zu aktualisieren.

Verschieben Sie bereits vorhandene Daten für jeden n-ten Microbatch mit „N % 100 == 0“ („100“ ist hier nur ein Beispiel) in ein anderes Verzeichnis, das von Spark geladen werden kann.

## <a name="next-steps"></a>Nächste Schritte

Wenn Ihr Problem nicht aufgeführt ist oder Sie es nicht lösen können, besuchen Sie einen der folgenden Kanäle, um weitere Unterstützung zu erhalten:

* Nutzen Sie den [Azure-Communitysupport](https://azure.microsoft.com/support/community/), um Antworten von Azure-Experten zu erhalten.

* Nutzen Sie [@AzureSupport](https://twitter.com/azuresupport) – das offizielle Microsoft Azure-Konto zur Verbesserung der Benutzerfreundlichkeit. Hierüber hat die Azure-Community Zugriff auf die richtigen Ressourcen: Antworten, Support und Experten.

* Sollten Sie weitere Unterstützung benötigen, senden Sie eine Supportanfrage über das [Azure-Portal](https://portal.azure.com/?#blade/Microsoft_Azure_Support/HelpAndSupportBlade/). Wählen Sie dazu auf der Menüleiste die Option **Support** aus, oder öffnen Sie den Hub **Hilfe und Support**. Ausführlichere Informationen hierzu finden Sie unter [Erstellen einer Azure-Supportanfrage](https://docs.microsoft.com/azure/azure-supportability/how-to-create-azure-support-request). Zugang zu Abonnementverwaltung und Abrechnungssupport ist in Ihrem Microsoft Azure-Abonnement enthalten. Technischer Support wird über einen [Azure-Supportplan](https://azure.microsoft.com/support/plans/) bereitgestellt.
