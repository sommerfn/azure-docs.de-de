---
title: 'Fehler „watchdog: BUG: soft lockup - CPU“ in einem Azure HDInsight-Cluster'
description: 'Der Fehler „watchdog: BUG: soft lockup - CPU“ wird in Kernel-Syslog-Protokollen angezeigt.'
ms.service: hdinsight
ms.topic: troubleshooting
author: hrasheed-msft
ms.author: hrasheed
ms.date: 08/05/2019
ms.openlocfilehash: 9278c174d96cb6b1823c8dbfdcba197b7a3c05cc
ms.sourcegitcommit: 3073581d81253558f89ef560ffdf71db7e0b592b
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 08/06/2019
ms.locfileid: "68828870"
---
# <a name="scenario-watchdog-bug-soft-lockup---cpu-error-from-an-azure-hdinsight-cluster"></a>Szenario: Fehler „watchdog: BUG: soft lockup - CPU“ in einem Azure HDInsight-Cluster

In diesem Artikel werden Schritte zur Problembehandlung und mögliche Lösungen für Probleme bei der Interaktion mit Azure HDInsight-Clustern beschrieben.

## <a name="issue"></a>Problem

Die Kernel-Syslog-Protokolle enthalten die folgende Fehlermeldung: `watchdog: BUG: soft lockup - CPU`.

## <a name="cause"></a>Ursache

Ein [Fehler](https://bugzilla.kernel.org/show_bug.cgi?id=199437) im Linux-Kernel verursacht eine vorübergehende CPU-Blockierung.

## <a name="resolution"></a>Lösung

Wenden Sie einen Kernelpatch an. Das folgende Skript aktualisiert den Linux-Kernel und startet die Computer im Laufe von 24 Stunden zu verschiedenen Zeitpunkten neu. Führen Sie die Skriptaktion in zwei Batches aus. Der erste Batch wird für alle Knoten mit Ausnahme des Hauptknotens ausgeführt. Der zweite Batch wird für den Hauptknoten ausgeführt. Führen Sie die Batches nicht gleichzeitig für den Hauptknoten und die anderen Knoten aus.

1. Navigieren Sie im Azure-Portal zu Ihrem HDInsight-Cluster.

1. Rufen Sie die Skriptaktionen auf.

1. Wählen Sie **Neue übermitteln** aus, und geben Sie Folgendes ein:

    | Eigenschaft | Wert |
    | --- | --- |
    | Skripttyp | -Custom |
    | NAME |Fix for kernel soft lock issue |
    | Bash-Skript-URI |`https://raw.githubusercontent.com/hdinsight/hdinsight.github.io/master/ClusterCRUD/KernelSoftLockFix/scripts/KernelSoftLockIssue_FixAndReboot.sh` |
    | Knotentyp(en) |Worker, Zookeeper |
    | Parameter |– |

    Wählen Sie **Speichern Sie diese Skriptaktion...** aus, wenn Sie das Skript beim Hinzufügen neuer Knoten ausführen möchten.

1. Klicken Sie auf **Erstellen**.

1. Warten Sie, bis die Ausführung erfolgreich abgeschlossen wurde.

1. Führen Sie die Skriptaktion für den Hauptknoten aus, indem Sie Schritt 3 erneut ausführen. Geben Sie dieses Mal jedoch den Knotentyp „Hauptknoten“ an.

1. Warten Sie, bis die Ausführung erfolgreich abgeschlossen wurde.

## <a name="next-steps"></a>Nächste Schritte

Wenn Ihr Problem nicht aufgeführt ist oder Sie es nicht lösen können, besuchen Sie einen der folgenden Kanäle, um weitere Unterstützung zu erhalten:

* Erhalten Sie über den [Azure-Communitysupport](https://azure.microsoft.com/support/community/) Antworten von Azure-Experten.

* Nutzen Sie [@AzureSupport](https://twitter.com/azuresupport) – das offizielle Microsoft Azure-Konto zur Verbesserung der Benutzerfreundlichkeit. Hierüber hat die Azure-Community Zugriff auf die richtigen Ressourcen: Antworten, Support und Experten.

* Wenn Sie weitere Unterstützung benötigen, senden Sie eine Supportanfrage über das [Azure-Portal](https://portal.azure.com/?#blade/Microsoft_Azure_Support/HelpAndSupportBlade/). Wählen Sie dazu auf der Menüleiste die Option **Support** aus, oder öffnen Sie den Hub **Hilfe und Support**. Ausführliche Informationen hierzu finden Sie unter [Erstellen einer Azure-Supportanfrage](https://docs.microsoft.com/azure/azure-supportability/how-to-create-azure-support-request). Zugang zu Abonnementverwaltung und Abrechnungssupport ist in Ihrem Microsoft Azure-Abonnement enthalten. Technischer Support wird über einen [Azure-Supportplan](https://azure.microsoft.com/support/plans/) bereitgestellt.
