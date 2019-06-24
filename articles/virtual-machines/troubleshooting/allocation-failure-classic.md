---
title: Beheben von Azure-VM-Zuordnungsfehlern im klassischen Bereitstellungsmodell | Microsoft-Dokumentation
description: Beheben von Zuordnungsfehlern beim Erstellen, Neustarten oder Ändern der Größe eines klassischen virtuellen Computers in Azure
services: azure-service-management
documentationcenter: ''
author: genlin
manager: willchen
editor: ''
tags: top-support-issue,azure-resource-manager,azure-service-management
ms.assetid: bb939e23-77fc-4948-96f7-5037761c30e8
ms.service: virtual-machines
ms.topic: troubleshooting
ms.date: 11/01/2018
ms.author: genli
ms.openlocfilehash: 7cd7897e3a0b940bbc636b2fbc3dbbc13b7cf540
ms.sourcegitcommit: 41ca82b5f95d2e07b0c7f9025b912daf0ab21909
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 06/13/2019
ms.locfileid: "60505542"
---
# <a name="troubleshooting-steps-specific-to-allocation-failure-scenarios-in-the-classic-deployment-model"></a>Schritte zur Problembehandlung in Zuordnungsfehlerszenarien im klassischen Bereitstellungsmodell

Im Folgenden werden häufig vorkommende Zuordnungsszenarien aufgeführt, die bewirken, dass eine Zuordnungsanforderung „verknüpft“ wird. Die einzelnen Szenarien werden weiter unten in diesem Artikel genauer erläutert.

- Ändern der Größe eines virtuellen Computers oder Hinzufügen weiterer virtueller Computer oder Rolleninstanzen in einem vorhandenen Clouddienst
- Neustart teilweise beendeter (zuordnungsaufgehobener) virtueller Computer
- Neustart vollständig beendeter (zuordnungsaufgehobener) virtueller Computer
- Staging und Produktionsbereitstellungen (nur Platform as a Service)
- Affinitätsgruppe (Nähe von VMs und Diensten)
- Affinität – gruppenbasiertes virtuelles Netzwerk

Wenn Sie einen Zuordnungsfehler erhalten, überprüfen Sie, ob eines der aufgeführten Szenarien auf Ihren Fehler zutrifft. Verwenden Sie den von der Azure-Plattform zurückgegebenen Zuordnungsfehler, um das entsprechende Szenario zu identifizieren. Falls Ihre Anforderung verknüpft ist, versuchen Sie, einige Verknüpfungseinschränkungen zu beseitigen. Dadurch öffnen Sie Ihre Anforderung für mehr Cluster und erhöhen damit die Chancen für eine erfolgreiche Zuordnung.
Allgemein gilt: Wenn in der Fehlermeldung nicht angegeben wird, dass die angeforderte VM-Größe nicht unterstützt wird, können Sie den Vorgang immer zu einem späteren Zeitpunkt wiederholen. Denn möglicherweise wurden genügend Ressourcen im Cluster freigegeben, um Ihre Anforderung erfüllen zu können. Falls die angeforderte Größe des virtuellen Computers nicht unterstützt wird, versuchen Sie es mit einer anderen Größe. Andernfalls können Sie nur noch die Verknüpfungseinschränkung entfernen.

Zwei häufig auftretende Fehlerszenarien hängen mit der Affinitätsgruppe zusammen. In der Vergangenheit wurde eine Affinitätsgruppe verwendet, um Nähe zu VMs oder Dienstinstanzen zu schaffen oder um die Erstellung eines virtuellen Netzwerks zu ermöglichen. Seit der Einführung von regionalen virtuellen Netzwerks werden Affinitätsgruppen zum Erstellen eines virtuellen Netzwerks nicht mehr benötigt. Mit der Reduzierung der Netzwerklatenz in der Azure-Infrastruktur hat sich die Empfehlung, Affinitätsgruppen zur Herstellung von Nähe zwischen VMs und Diensten zu verwenden, geändert.

Im folgenden Diagramm ist die Taxonomie der Zuordnungsszenarien (mit Verknüpfung) dargestellt. 

![Verknüpfte Zuordnung, Taxonomie](./media/virtual-machines-common-allocation-failure/Allocation3.png)

## <a name="resize-a-vm-or-add-vms-or-role-instances-to-an-existing-cloud-service"></a>Ändern der Größe eines virtuellen Computers oder Hinzufügen weiterer virtueller Computer oder Rolleninstanzen in einem vorhandenen Clouddienst
**Fehler**

Upgrade_VMSizeNotSupported oder GeneralError

**Ursache der Verknüpfung mit dem Cluster**

Die Anforderung, die Größe eines virtuellen Computers zu ändern oder einen virtuellen Computer oder eine Rolleninstanz zu einem vorhandenen Clouddienst hinzuzufügen, muss für den Originalcluster, der den vorhandenen Clouddienst hostet, versucht werden. Die Erstellung eines neuen Clouddiensts ermöglicht der Azure-Plattform, einen anderen Cluster zu finden, der über freie Ressourcen verfügt oder die von Ihnen angeforderte Größe des virtuellen Computers unterstützt.

**Problemumgehung**

Wenn der Fehler „Upgrade_VMSizeNotSupported*“ lautet, versuchen Sie es mit einer anderen Größe des virtuellen Computers. Wenn das Verwenden einer anderen Größe des virtuellen Computers keine Option ist, dafür aber die Verwendung einer anderen virtuellen IP-Adresse (VIP), erstellen Sie einen neuen Clouddienst zum Hosten des neuen virtuellen Computers. Fügen Sie den neuen Clouddienst dem regionalen virtuellen Netzwerken hinzu, auf dem die vorhandenen virtuellen Computer ausgeführt werden. Falls Ihr vorhandener Clouddienst kein regionales virtuelles Netzwerk verwendet, können Sie trotzdem ein neues virtuelles Netzwerk für den neuen Clouddienst erstellen und dann [das vorhandene virtuelle Netzwerk mit dem neuen virtuellen Netzwerk verbinden](https://azure.microsoft.com/blog/vnet-to-vnet-connecting-virtual-networks-in-azure-across-different-regions/). Weitere Informationen zu regionalen virtuellen Netzwerken finden Sie [hier](https://azure.microsoft.com/blog/2014/05/14/regional-virtual-networks/).

Wenn der Fehler „GeneralError*“ lautet, wird der Typ der Ressource (z. B. eine bestimmte Größe des virtuellen Computers) vom Cluster wahrscheinlich unterstützt, allerdings verfügt der Cluster derzeit nicht über freie Ressourcen. Fügen Sie, ähnlich wie oben, die gewünschten Compute-Ressourcen hinzu, indem Sie einen neuen Clouddiensts erstellen (beachten Sie, dass der neue Clouddienst eine andere VIP verwenden muss) und verwenden Sie das regionale Virtual Network zum Verbinden Ihrer Clouddienste.

## <a name="restart-partially-stopped-deallocated-vms"></a>Neustart teilweise beendeter (zuordnungsaufgehobener) virtueller Computer
**Fehler**

GeneralError*

**Ursache der Verknüpfung mit dem Cluster**

Die Teilaufhebung der Zuordnung bedeutet, dass Sie mindestens einen, aber nicht alle virtuellen Computer innerhalb eines Clouddiensts beendet haben (Aufhebung der Zuordnung). Wenn Sie einen virtuellen Computer beenden (Aufhebung der Zuordnung), werden die zugeordneten Ressourcen freigegeben. Das Neustarten dieses beendeten (zuordnungsaufgehobenen) virtuellen Computers ist daher gleichbedeutend einer neue Zuordnungsanforderung. Das Neustarten virtueller Computer in einem Clouddienst, in dem die Zuordnung teilweise aufgehoben wurde, entspricht dem Hinzufügen von virtuellen Computern in vorhandene Clouddienste. Die Anforderung muss für den Originalcluster versucht werden, der den vorhandenen Clouddienst hostet. Die Erstellung eines neuen Clouddiensts ermöglicht der Azure-Plattform einen anderen Cluster zu finden, der über freie Ressourcen verfügt oder die von Ihnen angeforderte Größe des virtuellen Computers unterstützt.

**Problemumgehung**

Falls die Verwendung einer anderen VIP akzeptabel ist, löschen Sie die beendeten (zuordnungsaufgehobenen) virtuellen Computer (aber behalten Sie die zugeordneten Datenträger) und fügen Sie die virtuellen Computer über einen anderen Clouddienst wieder hinzu. Verwenden Sie ein regionales virtuelles Netzwerk, um die Verbindung für Ihre Clouddienste herzustellen:

* Wenn Ihr vorhandener Clouddienst das regionale virtuelle Netzwerk nutzt, fügen Sie den neuen Clouddienst einfach dem gleichen virtuellen Netzwerk hinzu.
* Falls Ihr vorhandener Clouddienst kein regionales virtuelles Netzwerk verwendet, können Sie ein neues virtuelles Netzwerk für den neuen Clouddienst erstellen und dann [das vorhandene virtuelle Netzwerk mit dem neuen virtuellen Netzwerk verbinden](https://azure.microsoft.com/blog/vnet-to-vnet-connecting-virtual-networks-in-azure-across-different-regions/). Weitere Informationen zu regionalen virtuellen Netzwerken finden Sie [hier](https://azure.microsoft.com/blog/2014/05/14/regional-virtual-networks/).

## <a name="restart-fully-stopped-deallocated-vms"></a>Neustart vollständig beendeter (zuordnungsaufgehobener) virtueller Computer
**Fehler**

GeneralError*

**Ursache der Verknüpfung mit dem Cluster**

Die vollständige Aufhebung der Zuordnung bedeutet, dass Sie alle virtuellen Computer für einen Clouddienst beendet (zuordnungsaufgehoben) haben. Die Zuordnungsanforderungen, den virtuellen Computer neu zu starten, müssen für den Originalcluster, der den vorhandenen Clouddienst hostet, versucht werden. Die Erstellung eines neuen Clouddiensts ermöglicht der Azure-Plattform, einen anderen Cluster zu finden, der über freie Ressourcen verfügt oder die von Ihnen angeforderte Größe des virtuellen Computers unterstützt.

**Problemumgehung**

Falls die Verwendung einer anderen VIP akzeptabel ist, löschen Sie die ursprünglichen, beendeten (zuordnungsaufgehobenen) virtuellen Computer (aber behalten Sie die zugeordneten Datenträger) und löschen Sie den entsprechenden Clouddienst (die zugeordneten Compute-Ressourcen wurden bereits freigegeben, als Sie die (zuordnungsaufgehoben) virtuellen Computer beendet haben). Erstellen Sie einen neuen Clouddienst, um die virtuellen Computer wieder hinzuzufügen.

## <a name="stagingproduction-deployments-platform-as-a-service-only"></a>Staging/Produktionsbereitstellungen (nur Platform-as-a-Service)
**Fehler**

New_General* oder New_VMSizeNotSupported\*

**Ursache der Verknüpfung mit dem Cluster**

Die Stagingbereitstellung und Produktionsbereitstellung eines Clouddiensts werden im selben Cluster gehostet. Wenn Sie die zweite Bereitstellung hinzufügen, wird versucht, die entsprechende Zuordnungsanforderung im demselben Cluster durchzuführen, von dem die erste Bereitstellung gehostet wird.

**Problemumgehung**

Löschen Sie die erste Bereitstellung und den ursprünglichen Clouddienst und stellen Sie den Clouddienst erneut bereit. Bei dieser Aktion kann die erste Bereitstellung in einem Cluster enden, der über genügend freie Ressourcen zur Aufnahme beider Bereitstellungen verfügt, oder in einem Cluster, der die von Ihnen angeforderten Größen des virtuellen Computers unterstützt.

## <a name="affinity-group-vmservice-proximity"></a>Affinitätsgruppe (Nähe von virtuellem Computer und Dienst)
**Fehler**

New_General* oder New_VMSizeNotSupported\*

**Ursache der Verknüpfung mit dem Cluster**

Jede Compute-Ressource, die einer Affinitätsgruppe zugewiesen ist, ist an ein Cluster gebunden. Es wird versucht, neue Anforderungen von Compute-Ressourcen in dieser Affinitätsgruppe im selben Cluster durchzuführen, in dem die vorhandenen Ressourcen gehostet werden. Dies gilt unabhängig davon, ob die neuen Ressourcen über einen neuen Clouddienst oder einen vorhandenen Clouddienst erstellt werden.

**Problemumgehung**

Falls dies nicht erforderlich ist, verwenden Sie keine Affinitätsgruppe oder gruppieren Sie Ihre Compute-Ressourcen in mehrere Affinitätsgruppen.

## <a name="affinity-group-based-virtual-network"></a>Auf Affinitätsgruppe basierendes virtuelles Netzwerk
**Fehler**

New_General* oder New_VMSizeNotSupported\*

**Ursache der Verknüpfung mit dem Cluster**

Bevor regionale virtuelle Netzwerke eingeführt wurden, mussten Sie einem Virtual Network eine Affinitätsgruppe zuordnen. Daher gelten für Compute-Ressourcen, die in eine Affinitätsgruppe eingefügt werden, die gleichen Einschränkungen wie oben im Abschnitt „Zuordnungsszenario: Affinitätsgruppe (Nähe von virtuellem Computer und Dienst)“ beschrieben. Die Compute-Ressourcen sind an ein Cluster gebunden.

**Problemumgehung**

Falls Sie keine Affinitätsgruppe benötigen, erstellen Sie ein neues regionales virtuelles Netzwerk für die neuen Ressourcen, die Sie hinzufügen, und [verbinden Sie Ihr vorhandenes virtuelles Netzwerk mit dem neuen virtuellen Netzwerk](https://azure.microsoft.com/blog/vnet-to-vnet-connecting-virtual-networks-in-azure-across-different-regions/). Weitere Informationen zu regionalen virtuellen Netzwerken finden Sie [hier](https://azure.microsoft.com/blog/2014/05/14/regional-virtual-networks/).

Alternativ dazu können Sie [Ihr auf einer Affinitätsgruppe basierendes virtuelles Netzwerk zu einem regionalen virtuellen Netzwerk migrieren](https://azure.microsoft.com/blog/2014/11/26/migrating-existing-services-to-regional-scope/)und dann die gewünschten Ressourcen wieder hinzufügen.


