---
title: Konfigurieren des Zeitplans für das Patchen des Betriebssystems für Linux-basierte HDInsight-Cluster – Azure
description: Erfahren Sie, wie der Zeitplan für das Patchen des Betriebssystems für Linux-basierte HDInsight-Cluster konfiguriert wird.
author: hrasheed-msft
ms.author: hrasheed
ms.service: hdinsight
ms.custom: hdinsightactive
ms.topic: conceptual
ms.date: 07/01/2019
ms.openlocfilehash: a73866a8898042b546fa47d9c3d14ab4e58e9a12
ms.sourcegitcommit: 837dfd2c84a810c75b009d5813ecb67237aaf6b8
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 07/02/2019
ms.locfileid: "67503246"
---
# <a name="os-patching-for-hdinsight"></a>Patchen des Betriebssystems für HDInsight 

> [!IMPORTANT]
> Ubuntu-Images stehen für die Erstellung neuer HDInsight-Cluster innerhalb von drei Monaten nach der Veröffentlichung zur Verfügung. Seit Januar 2019 werden laufende Cluster **nicht** automatisch gepatcht. Kunden müssen Skriptaktionen oder andere Mechanismen verwenden, um einen laufenden Cluster zu patchen. Neu erstellte Cluster verfügen immer über die aktuellen verfügbaren Updates, einschließlich der neuesten Sicherheitspatches.

## <a name="how-to-configure-the-os-patching-schedule-for-linux-based-hdinsight-clusters"></a>Konfigurieren des Zeitplans für das Patchen des Betriebssystems für Linux-basierte HDInsight-Cluster
Die virtuellen Computer in einem HDInsight-Cluster müssen gelegentlich neu gestartet werden, damit wichtige Sicherheitspatches installiert werden können. 

Mithilfe der in diesem Artikel beschriebenen Skriptaktionen können Sie den Zeitplan für Patches des Betriebssystems in folgender Weise ändern:
1. Installieren Sie alle Updates, oder installieren Sie nur Kernel- und Sicherheitsupdates, oder installieren Sie nur Kernelupdates.
2. Führen Sie direkt einen Neustart durch, oder planen Sie einen Neustart auf dem virtuellen Computer.

> [!NOTE]  
> Diese Skriptaktionen funktionieren nur mit Linux-basierten HDInsight-Clustern, die nach dem 1. August 2016 erstellt wurden. Patches werden nur wirksam, wenn die virtuellen Computer neu gestartet werden. Diese Skripts wenden nicht automatisch Updates für alle zukünftigen Updatezyklen an. Führen Sie die Skripts immer dann aus, wenn neue Updates angewendet werden müssen, damit die Updates installiert werden und der virtuelle Computer neu gestartet wird.

## <a name="how-to-use-the-script"></a>Verwendung des Skripts 

Zur Verwendung des Skripts benötigen Sie die folgenden Informationen:
1. Den Speicherort des Skripts „install-updates-schedule-reboots“: https://hdiconfigactions.blob.core.windows.net/linuxospatchingrebootconfigv02/install-updates-schedule-reboots.sh.
    
   HDInsight verwendet diesen URI zum Suchen und Ausführen des Skripts auf allen virtuellen Computern im Cluster. Dieses Skript bietet Optionen zum Installieren von Updates und zum Neustarten des virtuellen Computers.
  
2. Den Speicherort des Skripts „schedule-reboots“: https://hdiconfigactions.blob.core.windows.net/linuxospatchingrebootconfigv02/schedule-reboots.sh.
    
   HDInsight verwendet diesen URI zum Suchen und Ausführen des Skripts auf allen virtuellen Computern im Cluster. Dieses Skript startet den virtuellen Computer neu.
  
3. Clusterknotentypen, auf die das Skript angewendet wird: Hauptknoten, Arbeitsknoten, Zookeeper. Dieses Skript muss auf alle Knotentypen im Cluster angewendet werden. Wenn es auf einen Knotentyp nicht angewendet wird, werden die virtuellen Computer für den betreffenden Knotentyp nicht aktualisiert oder neu gestartet.

4. Parameter: Das Skript „install-updates-schedule-reboots“ akzeptiert zwei numerische Parameter:

    | Parameter | Definition |
    | --- | --- |
    | Nur Kernelupdates installieren/Alle Updates installieren/Nur Kernel- und Sicherheitsupdates installieren |0 oder 1 oder 2. Ein Wert von 0 installiert nur Kernelupdates, während 1 alle Updates installiert und 2 nur Kernel- und Sicherheitsupdates installiert. Wenn kein Parameter angegeben wird, ist der Standardwert 0. |
    | Kein Neustart/Geplanten Neustart aktivieren/Sofortigen Neustart aktivieren |0 oder 1 oder 2. Ein Wert von 0 deaktiviert den Neustart, während 1 den geplanten und 2 den sofortigen Neustart aktiviert. Wenn kein Parameter angegeben wird, ist der Standardwert 0. Der Benutzer muss Parameter 1 in den Eingabeparameter 2 eingeben. |
   
 5. Parameter: Das Skript „schedule-reboots“ akzeptiert einen numerischen Parameter:

    | Parameter | Definition |
    | --- | --- |
    | Geplanten Neustart aktivieren/Sofortigen Neustart aktivieren |1 oder 2. Ein Wert von 1 ermöglicht einen geplanten Neustart (der Neustart ist in den nächsten 12 bis 24 Stunden geplant), während 2 einen sofortigen Neustart (in 5 Minuten) aktiviert. Wenn kein Parameter angegeben wird, ist der Standardwert 1. |  

> [!NOTE] 
> Sie müssen das Skript als permanent kennzeichnen, wenn Sie es auf einen vorhandenen Cluster anwenden. Andernfalls verwenden durch Skalierungsvorgänge erstellte neue Knoten den Standardpatchzeitplan.  Wenn Sie das Skript im Rahmen der Clustererstellung anwenden, ist es automatisch permanent.


## <a name="next-steps"></a>Nächste Schritte

Spezielle Schritte zur Verwendung der Skriptaktion finden Sie in den folgenden Abschnitten in [Anpassen von Linux-basierten HDInsight-Clustern mithilfe von Skriptaktionen](hdinsight-hadoop-customize-cluster-linux.md):

* [Verwenden einer Skriptaktion während der Clustererstellung](hdinsight-hadoop-customize-cluster-linux.md#use-a-script-action-during-cluster-creation)
* [Anwenden einer Skriptaktion auf einen ausgeführten Cluster](hdinsight-hadoop-customize-cluster-linux.md#apply-a-script-action-to-a-running-cluster)
