---
title: Azure-Näherungsplatzierungsgruppen für optimale Netzwerklatenz bei SAP-Anwendungen | Microsoft-Dokumentation
description: Beschreibt SAP-Bereitstellungsszenarien mit Azure-Näherungsplatzierungsgruppen
services: virtual-machines-linux,virtual-machines-windows
documentationcenter: ''
author: msjuergent
manager: bburns
editor: ''
tags: azure-resource-manager
keywords: ''
ms.service: virtual-machines-linux
ms.topic: article
ms.tgt_pltfrm: vm-linux
ms.workload: infrastructure
ms.date: 10/01/2019
ms.author: juergent
ms.custom: H1Hack27Feb2017
ms.openlocfilehash: deffcb81a4f66783fedc89c3e21ea46b15ad1c64
ms.sourcegitcommit: a19f4b35a0123256e76f2789cd5083921ac73daf
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 10/02/2019
ms.locfileid: "71719999"
---
# <a name="azure-proximity-placement-groups-for-optimal-network-latency-with-sap-applications"></a>Azure-Näherungsplatzierungsgruppen für optimale Netzwerklatenz mit SAP-Anwendungen
SAP-Anwendungen, die auf der SAP NetWeaver- oder SAP S/4HANA-Architektur basieren, sind von der Netzwerklatenz zwischen der SAP-Anwendungsschicht und der SAP-Datenbankschicht abhängig. Diese Abhängigkeit ist das Ergebnis davon, dass der größte Teil der Geschäftslogik in der Anwendungsschicht ausgeführt wird. Da die SAP-Anwendungsschicht die Geschäftslogik ausführt, gibt sie mit einer hohen Frequenz Abfragen an die Datenbankschicht aus (tausende oder zehntausende von Abfragen pro Sekunde). In den meisten Fällen handelt es sich um einfache Abfragen. Sie können oft in 500 Mikrosekunden oder noch kürzerer Zeit in der Datenbankschicht ausgeführt werden.

Die Zeit, die im Netzwerk verbracht wird, um eine solche Abfrage von der Anwendungsschicht an die Datenbankschicht zu senden, und das zurückgegebene Ergebnis beeinflussen maßgeblich die Zeit, die zur Ausführung von Geschäftsprozessen benötigt wird. Diese Empfindlichkeit gegenüber Netzwerklatenzen ist der Grund, warum Sie eine optimale Netzwerklatenz in SAP-Bereitstellungsprojekten erreichen müssen. Weitere Informationen finden Sie im [SAP-Hinweis 1100926 – Häufig gestellte Fragen: Netzwerkleistung](https://launchpad.support.sap.com/#/notes/1100926/E). Dort finden Sie einige Richtlinien zum Klassifizieren der Netzwerklatenz.

In vielen Azure-Regionen ist die Zahl der Rechenzentren gestiegen. Diese Zunahme wurde auch durch die Einführung von Verfügbarkeitszonen ausgelöst. Gleichzeitig nutzen Kunden (insbesondere für High-End-SAP-Systeme) speziellere VM-SKUs aus der M-Serie oder HANA Large Instances. Diese Typen virtueller Azure-Computer sind in bestimmten Azure-Regionen nicht in allen Rechenzentren verfügbar. Aufgrund dieser beiden Tendenzen haben Kunden Netzwerklatenz bemerkt, die sich nicht im optimalen Bereich befindet. In einigen Fällen führt diese Latenz zu einer suboptimalen Leistung ihrer SAP-Systeme.

Um diese Probleme zu vermeiden, bietet Azure [Näherungsplatzierungsgruppen](https://docs.microsoft.com/azure/virtual-machines/linux/co-location). Diese neue Funktionalität wurde bereits zur Bereitstellung verschiedener SAP-Systeme verwendet. Einschränkungen für Näherungsplatzierungsgruppen finden Sie im Artikel, der zu Beginn dieses Absatzes erwähnt wurde. In diesem Artikel werden die SAP-Szenarien erläutert, in denen Azure-Näherungsplatzierungsgruppen verwendet werden können.

## <a name="what-are-proximity-placement-groups"></a>Was sind Näherungsplatzierungsgruppen? 
Eine Azure-Näherungsplatzierungsgruppe ist ein logisches Konstrukt. Wenn eine solche Gruppe definiert ist, ist sie an eine Azure-Region und eine Azure-Ressourcengruppe gebunden. Wenn VMs bereitgestellt werden, wird wie folgt auf eine Azure-Näherungsplatzierungsgruppe verwiesen:

- Von der ersten Azure-VM, die im Rechenzentrum bereitgestellt wird. Der erste virtuelle Computer kann als „Bereichs-VM“ betrachtet werden, die basierend auf Azure-Zuordnungsalgorithmen in einem Rechenzentrum bereitgestellt und schließlich mit Benutzerdefinitionen für eine bestimmte Azure-Verfügbarkeitszone kombiniert wird.
- Von allen nachfolgenden VMs, die auf die Näherungsplatzierungsgruppe verweisen, um alle nachfolgenden Azure-VMs im selben Rechenzentrum wie den ersten virtuellen Computer zu platzieren.

> [!NOTE]
> Wenn keine Hosthardware bereitgestellt wird, die einen bestimmten VM-Typ im Rechenzentrum ausführen kann, in dem die erste VM platziert wurde, ist die Bereitstellung des angeforderten VM-Typs nicht erfolgreich. Sie erhalten eine Fehlermeldung.

Einer einzelnen [Azure-Ressourcengruppe](https://docs.microsoft.com/azure/azure-resource-manager/manage-resources-portal) können mehrere Näherungsplatzierungsgruppen zugewiesen werden. Eine Näherungsplatzierungsgruppe kann jedoch nur einer Azure-Ressourcengruppe zugeordnet werden.

Beachten Sie bei der Verwendung von Näherungsplatzierungsgruppen die folgenden Überlegungen:

- Wenn Sie optimale Leistung für das SAP-System anstreben und sich durch die Verwendung von Näherungsplatzierungsgruppen auf ein einziges Azure-Rechenzentrum für dieses System beschränken, können Sie möglicherweise nicht alle Arten von VM-Produktfamilien innerhalb der Näherungsplatzierungsgruppe kombinieren. Diese Einschränkungen treten auf, weil die Hosthardware, die zum Ausführen eines bestimmten VM-Typs erforderlich ist, möglicherweise nicht im Rechenzentrum vorhanden ist, in dem die „Bereichs-VM“ der Näherungsplatzierungsgruppe bereitgestellt wurde.
- Während des Lebenszyklus eines solchen SAP-Systems könnten Sie gezwungen sein, das System in ein anderes Rechenzentrum zu verlagern. Diese Verlagerung kann erforderlich sein, wenn Sie entscheiden, dass Ihre horizontal skalierbare HANA-DBMS-Schicht z.B. von vier Knoten auf 16 Knoten erweitert werden soll, und nicht genügend Kapazität verfügbar ist, um zusätzliche 12 VMs des Typs zu erhalten, den Sie im Rechenzentrum verwendet haben.
- Aufgrund der Außerbetriebnahme von Hardware kann Microsoft Kapazitäten für einen VM-Typ bereitstellen, den Sie in einem anderen Rechenzentrum verwendet haben, anstatt für den Typ, den Sie ursprünglich verwendet haben. In diesem Szenario müssen Sie möglicherweise alle VMs der Näherungsplatzierungsgruppe in ein anderes Rechenzentrum verschieben.

## <a name="proximity-placement-groups-with-sap-systems-that-use-only-azure-vms"></a>Näherungsplatzierungsgruppen mit SAP-Systemen, die ausschließlich Azure-VMs verwenden
Die meisten SAP NetWeaver- und S/4HANA-Systembereitstellungen unter Azure verwenden keine [HANA Large Instances](https://docs.microsoft.com/azure/virtual-machines/workloads/sap/hana-overview-architecture). Für Bereitstellungen, die keine HANA Large Instances verwenden, ist es wichtig, eine optimale Leistung zwischen der SAP-Anwendungsschicht und der DBMS-Schicht zu gewährleisten. Zu diesem Zweck definieren Sie eine Azure-Näherungsplatzierungsgruppe exklusiv für dieses System.

In den meisten Kundenbereitstellungen erstellen Kunden eine einzige [Azure-Ressourcengruppe](https://docs.microsoft.com/azure/azure-resource-manager/manage-resources-portal) für SAP-Systeme. In diesem Fall besteht eine 1:1-Beziehung beispielsweise zwischen den Ressourcengruppen des ERP-Produktionssystems und seiner Näherungsplatzierungsgruppe. In anderen Fällen haben Kunden ihre Ressourcengruppen horizontal organisiert und alle Produktionssysteme in einer einzigen Ressourcengruppe zusammengefasst. In diesem Fall besteht eine 1:n-Beziehung zwischen der Ressourcengruppe mit SAP-Produktionssystemen und mehreren Näherungsplatzierungsgruppen beispielsweise für SAP ERP- und SAP BW-Produktionssysteme.

Eine Bündelung mehrerer SAP-Produktions- oder -Nicht-Produktionssysteme in einer einzigen Näherungsplatzierungsgruppe sollte vermieden werden. Wenn einige wenige SAP-Systeme oder ein SAP-System und einige zugehörige Anwendungen Netzwerkkommunikation mit niedriger Latenz erfordern, können Sie diese Systeme ggf. in eine einzelne Näherungsplatzierungsgruppe verschieben. Sie sollten Systembündelungen vermeiden: Je mehr Systeme Sie in einer Näherungsplatzierungsgruppe gruppieren, desto höher ist die Wahrscheinlichkeit für Folgendes:

- Sie benötigen einen VM-Typ, der nicht in dem spezifischen Rechenzentrum ausgeführt werden kann, für das die Näherungsplatzierungsgruppe ausgelegt wurde.
- Ressourcen für Nicht-Mainstream-VMs (z.B. VMs der M-Serie) können ggf. nicht bereitgestellt werden, wenn Sie sie benötigen, weil Sie im Lauf der Zeit einer Näherungsplatzierungsgruppe Software hinzufügen.

Die ideale Konfiguration sieht laut Beschreibung wie folgt aus:

![Näherungsplatzierungsgruppen nur mit Azure-VMs](./media/sap-proximity-placement-scenarios/ppg-for-all-azure-vms.png)

In diesem Fall werden einzelne SAP-Systeme in einer Ressourcengruppe mit jeweils einer Näherungsplatzierungsgruppe zusammengefasst. Dies ist unabhängig davon, ob Sie eine Konfiguration für eine horizontale HANA-Hochskalierung oder eine zentrale DBMS-Hochskalierung verwenden.

## <a name="proximity-placement-groups-and-hana-large-instances"></a>Näherungsplatzierungsgruppen und HANA Large Instances
Wenn einige Ihrer SAP-Systeme auf [HANA Large Instances](https://docs.microsoft.com/azure/virtual-machines/workloads/sap/hana-overview-architecture) als Anwendungsschicht angewiesen sind, können erhebliche Verbesserungen in der Netzwerklatenz zwischen HANA Large Instances und Azure-VMs erzielt werden, wenn HANA Large Instances-Einheiten verwendet werden, die in [Reihen oder Stempeln der Version 4](https://docs.microsoft.com/azure/virtual-machines/workloads/sap/hana-network-architecture#networking-architecture-for-hana-large-instance) bereitgestellt werden. Eine der Verbesserungen besteht darin, dass HANA Large Instances-Einheiten direkt bei der Bereitstellung eine Näherungsplatzierungsgruppe erhalten. Sie können diese Näherungsplatzierungsgruppe verwenden, um VMs der Anwendungsschicht bereitzustellen. Folglich werden diese VMs im selben Rechenzentrum bereitgestellt, in dem sich auch die HANA Large Instances-Einheit befindet.

Um zu erkennen, ob Ihre HANA Large Instances-Einheit in einem Stempel oder einer Reihe der Revision 4 bereitgestellt wird, lesen Sie den Artikel [Steuerung von HANA Large Instances in Azure über das Azure-Portal](https://docs.microsoft.com/azure/virtual-machines/workloads/sap/hana-li-portal#look-at-attributes-of-single-hli-unit). In der Attributübersicht Ihrer HANA Large Instances-Einheit können Sie auch den Namen der Näherungsplatzierungsgruppe ermitteln, da sie bei der Bereitstellung Ihrer HANA Large Instances-Einheit erstellt wurde. Der in der Attributübersicht angezeigte Name ist der Name der Näherungsplatzierungsgruppe, in der Sie VMs der Anwendungsschicht bereitstellen sollten.

Im Vergleich zu SAP-Systemen, die nur virtuelle Azure-Computer verwenden, haben Sie bei Verwendung von HANA Large Instances weniger Flexibilität bei der Entscheidung, wie viele [Azure-Ressourcengruppen](https://docs.microsoft.com/azure/azure-resource-manager/manage-resources-portal) Sie verwenden möchten. Alle HANA-Einheiten eines [HANA Large Instances-Mandanten](https://docs.microsoft.com/azure/virtual-machines/workloads/sap/hana-know-terms) werden wie in [diesem Artikel](https://docs.microsoft.com/azure/virtual-machines/workloads/sap/hana-li-portal#display-of-hana-large-instance-units-in-the-azure-portal) beschrieben in einer einzigen Ressourcengruppe zusammengefasst. Wenn Sie die Bereitstellung nicht auf verschiedene Mandanten aufteilen, um z.B. Produktions- und Nicht-Produktionssysteme oder andere Systeme zu trennen, werden alle Ihre HANA Large Instances-Einheiten in einem HANA Large Instances-Mandanten bereitgestellt. Dieser Mandant weist eine 1:1-Beziehung mit einer Ressourcengruppe auf. Für jede einzelne Einheit wird jedoch eine separate Näherungsplatzierungsgruppe definiert.

Infolgedessen sehen die Beziehungen zwischen Azure-Ressourcengruppen und Näherungsplatzierungsgruppen für einen einzelnen Mandanten wie hier gezeigt aus:

![Näherungsplatzierungsgruppen und HANA Large Instances](./media/sap-proximity-placement-scenarios/ppg-for-hana-large-instance-units.png)

## <a name="example-of-deployment-with-proximity-placement-groups"></a>Beispiel für die Bereitstellung mit Näherungsplatzierungsgruppen
Im Folgenden finden Sie einige PowerShell-Befehle, mit denen Sie Ihre VMs mit Azure-Näherungsplatzierungsgruppen bereitstellen können.

Nachdem Sie sich bei [Azure Cloud Shell](https://azure.microsoft.com/features/cloud-shell/) angemeldet haben, besteht der erste Schritt darin, zu überprüfen, ob Sie sich in dem Azure-Abonnement befinden, das Sie für die Bereitstellung verwenden möchten:

<pre><code>
Get-AzureRmContext
</code></pre>

Wenn Sie zu einem anderen Abonnement wechseln müssen, können Sie dazu den folgenden Befehl ausführen:

<pre><code>
Set-AzureRmContext -Subscription "my PPG test subscription"
</code></pre>

Erstellen Sie eine neue Azure-Ressourcengruppe, indem Sie diesen Befehl ausführen:

<pre><code>
New-AzResourceGroup -Name "myfirstppgexercise" -Location "westus2"
</code></pre>

Erstellen Sie die neue Näherungsplatzierungsgruppe, indem Sie den folgenden Befehl ausführen:

<pre><code>
New-AzProximityPlacementGroup -ResourceGroupName "myfirstppgexercise" -Name "letsgetclose" -Location "westus2"
</code></pre>

Stellen Sie Ihre erste VM mit einem Befehl wie dem folgenden in der Näherungsplatzierungsgruppe bereit:

<pre><code>
New-AzVm -ResourceGroupName "myfirstppgexercise" -Name "myppganchorvm" -Location "westus2" -OpenPorts 80,3389 -ProximityPlacementGroup "letsgetclose" -Size "Standard_DS11_v2"
</code></pre>

Mit dem vorhergehenden Befehl wird eine Windows-basierte VM bereitgestellt. Nachdem die VM-Bereitstellung erfolgreich war, wird die Rechenzentrumsreichweite der Näherungsplatzierungsgruppe innerhalb der Azure-Region definiert. Alle nachfolgenden VM-Bereitstellungen, die auf die Näherungsplatzierungsgruppe wie im vorherigen Befehl gezeigt verweisen, werden im selben Azure-Rechenzentrum bereitgestellt, solange der VM-Typ auf Hardware gehostet werden kann, die in diesem Rechenzentrum installiert ist, und ausreichend Kapazität für diesen VM-Typ verfügbar ist.

## <a name="combine-availability-sets-and-availability-zones-with-proximity-placement-groups"></a>Kombinieren von Verfügbarkeitsgruppen und Verfügbarkeitszonen mit Näherungsplatzierungsgruppen
Einer der Nachteile bei der Verwendung von Verfügbarkeitszonen für SAP-Systembereitstellungen besteht darin, dass Sie die SAP-Anwendungsschicht nicht mit Verfügbarkeitsgruppen innerhalb der jeweiligen Zone bereitstellen können. Sie möchten, dass die SAP-Anwendungsschicht in denselben Zonen wie die DBMS-Schicht bereitgestellt wird. Das Verweisen auf eine Verfügbarkeitszone und eine Verfügbarkeitsgruppe beim Bereitstellen einer einzelnen VM wird nicht unterstützt. Daher mussten Sie die Anwendungsschicht bisher durch Verweisen auf eine Zone bereitstellen. Sie waren dann nicht mehr in der Lage, sicherzustellen, dass die Anwendungsschicht-VMs auf verschiedene Update- und Fehlerdomänen verteilt wurden.

Durch die Verwendung von Näherungsplatzierungsgruppen können Sie diese Einschränkung umgehen. Dies ist die Bereitstellungssequenz:

- Erstellen Sie eine Näherungsplatzierungsgruppe.
- Stellen Sie Ihre „Anker-VM“ (in der Regel den DBMS-Server) bereit, indem Sie auf eine Verfügbarkeitszone verweisen.
- Erstellen Sie eine Verfügbarkeitsgruppe, die auf die Näherungsplatzierungsgruppe verweist. (Siehe den Befehl weiter unten in diesem Artikel.)
- Stellen Sie die VMs der Anwendungsschicht bereit, indem Sie auf die Verfügbarkeitsgruppe und die Näherungsplatzierungsgruppe verweisen.

Anstatt die erste VM wie im vorherigen Abschnitt gezeigt bereitzustellen, verweisen Sie beim Bereitstellen der VM auf eine Verfügbarkeitszone und die Näherungsplatzierungsgruppe:

<pre><code>
New-AzVm -ResourceGroupName "myfirstppgexercise" -Name "myppganchorvm" -Location "westus2" -OpenPorts 80,3389 -Zone "1" -ProximityPlacementGroup "letsgetclose" -Size "Standard_E16_v3"
</code></pre>

Eine erfolgreiche Bereitstellung dieses virtuellen Computers würde die Datenbankinstanz des SAP-Systems in einer Verfügbarkeitszone hosten. Der Bereich der Näherungsplatzierungsgruppe ist auf eines der Rechenzentren festgelegt, die die von Ihnen definierte Verfügbarkeitszone darstellen.

Angenommen, Sie stellen die VMs für die zentralen Dienste auf die gleiche Weise wie die DBMS VMs bereit und verweisen auf die gleichen Zonen und die gleichen Näherungsplatzierungsgruppen. Im nächsten Schritt müssen Sie die Verfügbarkeitsgruppen erstellen, die Sie für die Anwendungsschicht des SAP-Systems verwenden möchten.

Sie müssen die Näherungsplatzierungsgruppe definieren und erstellen. Der Befehl zum Erstellen der Verfügbarkeitsgruppe erfordert einen zusätzlichen Verweis auf die ID der Näherungsplatzierungsgruppe (nicht den Namen). Sie können die ID der Näherungsplatzierungsgruppe abrufen, indem Sie den folgenden Befehl verwenden:

<pre><code>
Get-AzProximityPlacementGroup -ResourceGroupName "myfirstppgexercise" -Name "letsgetclose"
</code></pre>

Beim Erstellen der Verfügbarkeitsgruppe müssen Sie zusätzliche Parameter berücksichtigen, wenn Sie verwaltete Datenträger (Standard, sofern nicht anders angegeben) und Näherungsplatzierungsgruppen verwenden:

<pre><code>
New-AzAvailabilitySet -ResourceGroupName "myfirstppgexercise" -Name "myppgavset" -Location "westus2" -ProximityPlacementGroupId "/subscriptions/my very long ppg id string" -sku "aligned" -PlatformUpdateDomainCount 3 -PlatformFaultDomainCount 2 
</code></pre>

Im Idealfall sollten Sie drei Fehlerdomänen verwenden. Die Anzahl der unterstützten Fehlerdomänen kann jedoch je nach Region unterschiedlich sein. In diesem Fall beträgt die maximal mögliche Anzahl von Fehlerdomänen für die einzelnen Regionen zwei Domänen. Für die Bereitstellung von Anwendungsschicht-VMs müssen Sie einen Verweis auf den Namen der Verfügbarkeitsgruppe und den Namen der Näherungsplatzierungsgruppe hinzufügen, wie hier gezeigt:

<pre><code>
New-AzVm -ResourceGroupName "myfirstppgexercise" -Name "myppgavsetappvm" -Location "westus2" -OpenPorts 80,3389 -AvailabilitySetName "myppgavset" -ProximityPlacementGroup "letsgetclose" -Size "Standard_DS11_v2"
</code></pre>

Dies ist das Ergebnis dieser Bereitstellung:
- Eine DBMS-Schicht und zentrale Dienste für Ihr SAP-System, die sich in einer bestimmten Verfügbarkeitszone oder mehreren Verfügbarkeitszonen befinden.
- Eine SAP-Anwendungsschicht, die sich in Verfügbarkeitsgruppen in denselben Azure-Rechenzentren wie die DBMS-VM(s) befindet.

> [!NOTE]
> Da Sie eine DBMS-VM in einer Zone und die zweite DBMS-VM in eine anderer Zone bereitstellen, um eine Hochverfügbarkeitskonfiguration zu erhalten, benötigen Sie für jede der Zonen unterschiedliche Näherungsplatzierungsgruppen. Das gleiche gilt für jede Verfügbarkeitsgruppe, die Sie verwenden.

## <a name="move-an-existing-system-into-proximity-placement-groups"></a>Verschieben eines vorhandenen Systems in Näherungsplatzierungsgruppen
Wenn Sie bereits SAP-Systeme bereitgestellt haben, sollten Sie die Netzwerklatenz einiger kritischer Systeme optimieren und die Anwendungsschicht und die DBMS-Schicht im selben Rechenzentrum anordnen. Während der öffentlichen Vorschau von Näherungsplatzierungsgruppen müssen Sie die VMs löschen und neu erstellen, um das System in Näherungsplatzierungsgruppen zu verschieben. Sie können die VMs zurzeit nicht einfach herunterfahren und sie dann Näherungsplatzierungsgruppen zuweisen.


## <a name="next-steps"></a>Nächste Schritte
Weitere Informationen finden Sie in der Dokumentation:

- [Prüfliste für die Planung und Bereitstellung von SAP-Workloads in Azure](https://docs.microsoft.com/azure/virtual-machines/workloads/sap/sap-deployment-checklist)
- [Vorschau: Bereitstellen von VMs für Näherungsplatzierungsgruppen mit Azure CLI](https://docs.microsoft.com/azure/virtual-machines/linux/proximity-placement-groups)
- [Vorschau: Bereitstellen von VMs für Näherungsplatzierungsgruppen mit PowerShell](https://docs.microsoft.com/azure/virtual-machines/windows/proximity-placement-groups)
- [Überlegungen zu Azure Virtual Machines – DBMS-Bereitstellung für SAP-Workloads](https://docs.microsoft.com/azure/virtual-machines/workloads/sap/dbms_guide_general)

