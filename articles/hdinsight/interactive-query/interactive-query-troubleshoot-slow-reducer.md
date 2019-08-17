---
title: Der Reducer ist in Azure HDInsight langsam
description: Der Reducer ist in Azure HDInsight aufgrund möglicher Datenschiefe langsam
ms.service: hdinsight
ms.topic: troubleshooting
author: hrasheed-msft
ms.author: hrasheed
ms.date: 07/30/2019
ms.openlocfilehash: 5fe1b2c720bbc55e8e245f9592755e046e0b04a3
ms.sourcegitcommit: 5d6c8231eba03b78277328619b027d6852d57520
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 08/13/2019
ms.locfileid: "68968377"
---
# <a name="scenario-reducer-is-slow-in-azure-hdinsight"></a>Szenario: Der Reducer ist in Azure HDInsight langsam

In diesem Artikel werden Schritte zur Problembehandlung und mögliche Lösungen für Probleme bei der Verwendung von Interactive Query-Komponenten in Azure HDInsight-Clustern beschrieben.

## <a name="issue"></a>Problem

Wenn eine Abfrage wie `insert into table1 partition(a,b) select a,b,c from table2` ausgeführt wird, startet der Abfrageplan eine Reihe von Reducern, aber die Daten von jeder Partition gehen an einen einzelnen Reducer. Dies bewirkt, dass die Abfrage so langsam ist wie die Zeitspanne, die der Reducer der größten Partition benötigt.

## <a name="cause"></a>Ursache

Öffnen Sie [beeline](../hadoop/apache-hadoop-use-hive-beeline.md), und überprüfen Sie den Wert von `hive.optimize.sort.dynamic.partition`.

Der Wert dieser Variablen soll basierend auf der Art der Daten auf TRUE/FALSE festgelegt werden.

Wenn die Partitionen in der Eingabetabelle kleiner sind (z.B. weniger als 10), ebenso wie die Anzahl der Ausgabepartitionen, und die Variable auf `true` festgelegt ist, bewirkt dies, dass Daten global sortiert und mit einem einzigen Reducer pro Partition geschrieben werden. Auch wenn die Anzahl der verfügbaren Reducer größer ist, können einige Reducer aufgrund von Datenschiefe im Rückstand sein, und die maximale Parallelität kann nicht erreicht werden. Wenn der Wert in `false` geändert wird, können mehrere Reducer eine einzelne Partition verarbeiten, und es werden mehrere kleinere Dateien geschrieben, was zu einer schnelleren Einfügung führt. Dies wirkt sich jedoch möglicherweise auf weitere Abfragen aus, da kleinere Dateien vorhanden sind.

Der Wert `true` ist sinnvoll, wenn die Anzahl der Partitionen größer ist und keine Datenschiefe vorliegt. In solchen Fällen wird das Ergebnis der Zuordnungsphase so ausgegeben, dass jede Partition von einem einzigen Reducer verarbeitet wird, was zu einer besseren späteren Abfrageleistung führt.

## <a name="resolution"></a>Lösung

1. Versuchen Sie, die Daten neu zu partitionieren, sodass Sie in mehreren Partitionen normalisiert werden.

1. Wenn Lösung 1 nicht möglich ist, legen Sie den Wert der Konfiguration in der beeline-Sitzung auf FALSE fest, und wiederholen Sie die Abfrage. `set hive.optimize.sort.dynamic.partition=false`. Es wird nicht empfohlen, den Wert auf Clusterebene auf FALSE festzulegen. Der Wert `true` ist optimal und legt den Parameter basierend auf der Art der Daten und der Abfrage wie erforderlich fest.

## <a name="next-steps"></a>Nächste Schritte

Wenn Ihr Problem nicht aufgeführt ist oder Sie es nicht lösen können, besuchen Sie einen der folgenden Kanäle, um weitere Unterstützung zu erhalten:

* Erhalten Sie über den [Azure-Communitysupport](https://azure.microsoft.com/support/community/) Antworten von Azure-Experten.

* Setzen Sie sich mit [@AzureSupport](https://twitter.com/azuresupport) in Verbindung – dem offiziellen Microsoft Azure-Konto zur Verbesserung der Benutzerfreundlichkeit. Hierüber erhält die Azure-Community Kontakt mit den richtigen Ressourcen: Antworten, Support und Experten.

* Wenn Sie weitere Unterstützung benötigen, senden Sie eine Supportanfrage über das [Azure-Portal](https://portal.azure.com/?#blade/Microsoft_Azure_Support/HelpAndSupportBlade/). Wählen Sie dazu auf der Menüleiste die Option **Support** aus, oder öffnen Sie den Hub **Hilfe und Support**. Ausführliche Informationen hierzu finden Sie unter [Erstellen einer Azure-Supportanfrage](https://docs.microsoft.com/azure/azure-supportability/how-to-create-azure-support-request). Zugang zu Abonnementverwaltung und Abrechnungssupport ist in Ihrem Microsoft Azure-Abonnement enthalten. Technischer Support wird über einen [Azure-Supportplan](https://azure.microsoft.com/support/plans/) bereitgestellt.
