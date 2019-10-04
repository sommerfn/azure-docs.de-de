---
title: 'Konfigurieren des Zeitplans für das Patchen des Betriebssystems für Linux-basierte HDInsight-Cluster: Azure'
description: Erfahren Sie, wie der Zeitplan für das Patchen des Betriebssystems für Linux-basierte HDInsight-Cluster konfiguriert wird.
author: hrasheed-msft
ms.author: hrasheed
ms.reviewer: jasonh
ms.service: hdinsight
ms.custom: hdinsightactive
ms.topic: conceptual
ms.date: 07/01/2019
ms.openlocfilehash: 06111ec35a127cf17fdcc77ff717de7a4bc7299f
ms.sourcegitcommit: 8ef0a2ddaece5e7b2ac678a73b605b2073b76e88
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 09/17/2019
ms.locfileid: "71076859"
---
# <a name="configure-the-os-patching-schedule-for-linux-based-hdinsight-clusters"></a>Konfigurieren des Zeitplans für das Patchen des Betriebssystems für Linux-basierte HDInsight-Cluster 

> [!IMPORTANT]
> Ubuntu-Images stehen für die Erstellung neuer Azure HDInsight-Cluster innerhalb von drei Monaten nach der Veröffentlichung zur Verfügung. Seit Januar 2019 werden laufende Cluster nicht automatisch gepatcht. Kunden müssen Skriptaktionen oder andere Mechanismen verwenden, um einen laufenden Cluster zu patchen. Neu erstellte Cluster verfügen immer über die aktuellen verfügbaren Updates, einschließlich der neuesten Sicherheitspatches.

Gelegentlich müssen Sie virtuelle Computer (VMs) in einem HDInsight-Cluster neu starten, um wichtige Sicherheitspatches zu installieren.

Mithilfe der in diesem Artikel beschriebenen Skriptaktionen können Sie den Zeitplan für Patches des Betriebssystems in folgender Weise ändern:

1. Installieren aller Updates oder Installieren nur von Kernel- und Sicherheitsupdates oder nur von Kernelupdates.
2. Ausführen eines sofortigen Neustarts oder Planen eines Neustarts auf dem virtuellen Computer.

> [!NOTE]  
> Die in diesem Artikel beschriebenen Skriptaktionen funktionieren nur mit Linux-basierten HDInsight-Clustern, die nach dem 1. August 2016 erstellt wurden. Patches werden erst nach dem Neustart von VMs wirksam.
> Skriptaktionen wenden nicht automatisch Updates für alle zukünftigen Updatezyklen an. Führen Sie die Skripts immer dann aus, wenn neue Updates angewendet werden müssen, damit die Updates installiert werden, und starten Sie den virtuellen Computer dann neu.

## <a name="add-information-to-the-script"></a>Hinzufügen von Informationen zum Skript

Zur Verwendung eines Skripts benötigen Sie die folgenden Informationen:

- Den Speicherort des Skripts „install-updates-schedule-reboots“: https://hdiconfigactions.blob.core.windows.net/linuxospatchingrebootconfigv02/install-updates-schedule-reboots.sh.
    
   HDInsight verwendet diesen URI zum Suchen und Ausführen des Skripts auf allen VMs im Cluster. Dieses Skript bietet Optionen zum Installieren von Updates und zum Neustarten des virtuellen Computers.
  
- Den Speicherort des Skripts „schedule-reboots“: https://hdiconfigactions.blob.core.windows.net/linuxospatchingrebootconfigv02/schedule-reboots.sh.
    
   HDInsight verwendet diesen URI zum Suchen und Ausführen des Skripts auf allen VMs im Cluster. Dieses Skript startet den virtuellen Computer neu.
  
- Die Clusterknotentypen, auf die das Skript angewendet wird sind Hauptknoten, Arbeitsknoten und Zookeeper. Wenden Sie das Skript auf alle Knotentypen im Cluster an. Wenn das Skript nicht auf einen Knotentyp angewendet wird, werden die VMs für diesen Knotentyp nicht aktualisiert oder neu gestartet.

- Das Skript „install-updates-schedule-reboots“ akzeptiert zwei numerische Parameter:

    | Parameter | Definition |
    | --- | --- |
    | Nur Kernelupdates installieren/Alle Updates installieren/Nur Kernel- und Sicherheitsupdates installieren|0, 1 oder 2. Bei einem Wert von 0 werden nur Kernelupdates installiert. Bei einem Wert von 1 werden alle Updates installiert, und 2 installiert nur die Kernel- und Sicherheitsupdates. Wenn kein Parameter angegeben wird, ist der Standardwert 0. |
    | Kein Neustart/Geplanten Neustart aktivieren/Sofortigen Neustart aktivieren |0, 1 oder 2. Der Wert 0 deaktiviert den Neustart. Der Wert 1 aktiviert den Neustart des Zeitplans, und 2 aktiviert einen sofortigen Neustart. Wenn kein Parameter angegeben wird, ist der Standardwert 0. Der Benutzer muss den Eingabeparameter 1 in den Eingabeparameter 2 ändern. |
   
 - Das Skript „schedule-reboots“ akzeptiert einen numerischen Parameter:

    | Parameter | Definition |
    | --- | --- |
    | Geplanten Neustart aktivieren/Sofortigen Neustart aktivieren |1 oder 2. Der Wert 1 aktiviert den Neustart des Zeitplans (geplant in 12 bis 24 Stunden). Der Wert 2 ermöglicht einen sofortigen Neustart (in 5 Minuten). Wenn kein Parameter angegeben wird, ist der Standardwert 1. |  

> [!NOTE]
> Sie müssen ein Skript als permanent gespeichert kennzeichnen, nachdem Sie es auf einen vorhandenen Cluster angewendet haben. Andernfalls verwenden durch Skalierungsvorgänge erstellte neue Knoten den Standardpatchzeitplan. Wenn Sie das Skript im Rahmen der Clustererstellung anwenden, wird es automatisch persistent gespeichert.


## <a name="next-steps"></a>Nächste Schritte

Spezielle Schritte zur Verwendung der Skriptaktionen finden Sie in den folgenden Abschnitten unter [Anpassen von Linux-basierten HDInsight-Clustern mithilfe einer Skriptaktion](hdinsight-hadoop-customize-cluster-linux.md):

* [Verwenden einer Skriptaktion während der Clustererstellung](hdinsight-hadoop-customize-cluster-linux.md#use-a-script-action-during-cluster-creation)
* [Anwenden einer Skriptaktion auf einen ausgeführten Cluster](hdinsight-hadoop-customize-cluster-linux.md#apply-a-script-action-to-a-running-cluster)
