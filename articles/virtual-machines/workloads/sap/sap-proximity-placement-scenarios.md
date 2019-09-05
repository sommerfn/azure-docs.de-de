---
title: Azure-Näherungsplatzierungsgruppen für optimale Netzwerklatenz bei SAP-Anwendungen | Microsoft-Dokumentation
description: Beschreibt SAP-Bereitstellungsszenarien bei Azure-Näherungsplatzierungsgruppen
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
ms.date: 07/15/2019
ms.author: juergent
ms.custom: H1Hack27Feb2017
ms.openlocfilehash: 02dcb7174dd9cb2926ef2fafda4b521b939ae68a
ms.sourcegitcommit: 44e85b95baf7dfb9e92fb38f03c2a1bc31765415
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 08/28/2019
ms.locfileid: "70077984"
---
# <a name="azure-proximity-placement-groups-for-optimal-network-latency-with-sap-applications"></a>Azure-Näherungsplatzierungsgruppen für optimale Netzwerklatenz mit SAP-Anwendungen
SAP-Anwendungen, die auf der SAP NetWeaver- oder SAP S/4HANA-Architektur basieren, sind von der Netzwerklatenz zwischen der SAP-Anwendungsschicht und der SAP-Datenbankschicht abhängig. Der Grund für die Sensibilität dieser Architekturen liegt darin, dass der Großteil der Geschäftslogik in der Anwendungsschicht ausgeführt wird. Aufgrund der Ausführung der Geschäftslogik sendet die SAP-Anwendungsschicht in hoher Frequenz Abfragen an die Datenbankschicht (Tausende oder sogar Zehntausende pro Sekunde). In den meisten Fällen handelt es sich um ziemlich einfache Abfragen. Sie können oft in weniger als 500 Mikrosekunden in der Datenbankschicht ausgeführt werden. Die Zeit, die im Netzwerk verbracht wird, um eine solche Abfrage von der Anwendungsschicht an die Datenbankschicht zu senden und das von der Datenbankschicht zurückgegebene Ergebnis beeinflussen maßgeblich die Zeit, die zur Ausführung von Geschäftsprozessen benötigt wird. Diese Sensibilität für Netzwerklatenzen ist der Grund dafür, dass SAP-Bereitstellungsprojekte Zeit benötigen, um eine optimale Netzwerklatenz zu erreichen. Im [SAP-Hinweis 1100926 – Häufig gestellte Fragen: Netzwerkleistung](https://launchpad.support.sap.com/#/notes/1100926/E) hat SAP einige Richtlinien zum Klassifizieren der Netzwerklatenz veröffentlicht.

Auf der einen Seite nahm die Zahl der Rechenzentren in vielen Azure-Regionen zu, was auch auf die Einführung von Verfügbarkeitszonen zurückzuführen ist. Auf der anderen Seite nutzten Kunden, insbesondere für High-End-SAP-Systeme, speziellere VM-Angebote aus der M-Serie oder HANA (große Instanzen). Typen virtueller Azure-Computer sind in bestimmten Azure-Regionen nicht in allen Rechenzentren verfügbar. Aufgrund dieser beiden Tendenzen erlebten Kunden Situationen, in denen die Netzwerklatenz nicht im optimalen Bereich lag und in einigen Fällen zu einer suboptimalen Leistung ihrer SAP-Systeme führte.

Um solche Problem zu vermeiden, bietet Azure ein Konstrukt mit dem Namen [Azure-Näherungsplatzierungsgruppe](https://docs.microsoft.com/azure/virtual-machines/linux/co-location). Diese neue Funktionalität wurde bereits zur Bereitstellung unterschiedlicher SAP-Systeme verwendet. Lesen Sie den Artikel über Einschränkungen bei Näherungsplatzierungsgruppen, auf den hier verwiesen wird. In diesem Artikel werden die verschiedenen SAP-Szenarien behandelt, in denen Azure-Näherungsplatzierungsgruppen verwendet werden können oder müssen.

## <a name="what-are-proximity-placement-groups"></a>Was sind Näherungsplatzierungsgruppen? 
Eine Azure-Näherungsplatzierungsgruppe ist ein logisches Konstrukt, das in der Definitionsphase an eine Azure-Region und eine Azure-Ressourcengruppe gebunden ist. Während der Bereitstellung von VMs wird wie folgt auf eine Azure-Näherungsplatzierungsgruppe verwiesen:

- Vom ersten, im Rechenzentrum bereitgestellten virtuellen Azure-Computer. Der erste virtuelle Computer kann als Anker-VM betrachtet werden, die basierend auf Azure-Zuteilungsalgorithmen in einem Rechenzentrum bereitgestellt und schließlich mit Benutzerdefinitionen für eine bestimmte Azure-Verfügbarkeitszone kombiniert werden.
- Von allen nachfolgenden VMs, die mit Verweis auf die Näherungsplatzierungsgruppe bereitgestellt werden, um alle nachfolgenden Azure-VMs im selben Rechenzentrum zu platzieren, in dem sich auch der erste virtuelle Computer befindet.

> [!NOTE]
> Wenn keine Hosthardware bereitgestellt wird, auf der ein bestimmter VM-Typ im selben Rechenzentrum wie die erste VM ausgeführt werden kann, wird die Bereitstellung des erforderlichen VM-Typs nicht erfolgreich sein und mit einer Fehlermeldung enden. Dies kann der Fall sein, wenn mehrere nicht standardmäßige VMs, wie virtuelle Computer mit GPUs oder HPC-VM-Typen, im Umkreis einer VM der M-Serie angeordnet werden sollen, die als erster VM-Typ bereitgestellt wurde.

Einer einzelnen [Azure-Ressourcengruppe](https://docs.microsoft.com/azure/azure-resource-manager/manage-resources-portal) können mehrere Näherungsplatzierungsgruppen zugewiesen werden. Eine Näherungsplatzierungsgruppe kann jedoch nur einer Azure-Ressourcengruppe zugeordnet werden.

Bei Verwendung von Näherungsplatzierungsgruppen ist auf Folgendes zu achten:

- Wenn Sie optimale Leistung für das SAP-System anstreben und sich durch die Verwendung von Näherungsplatzierungsgruppen auf ein einziges Azure-Rechenzentrum für dieses System beschränken, können Sie möglicherweise nicht alle Arten von VM-Familien innerhalb einer solchen Näherungsplatzierungsgruppe kombinieren. Der Grund dafür ist, dass bestimmte Hosthardware, die zur exklusiven Ausführung eines bestimmten VM-Typs benötigt wird, möglicherweise nicht im Rechenzentrum vorhanden ist, in dem die „Anker-VM“ der Platzierungsgruppe bereitgestellt wurde.
- Während des Lebenszyklus eines solchen SAP-Systems könnten Sie gezwungen sein, das System in ein anderes Rechenzentrum zu verlagern. Eine solche Verschiebung könnte zwingend erforderlich sein, wenn Sie sich für eine horizontale Skalierung der HANA-DBMS-Schicht von vier Knoten auf 16 Knoten entschieden haben. Aber es gibt nicht mehr genügend Kapazität für weitere 12 VMs desselben Typs, den Sie bereits im selben Rechenzentrum verwenden.
- Aufgrund der Außerbetriebsetzung von Hardware kann es sein, dass Microsoft Kapazitäten für die verwendeten VM-Typen in einem anderen Rechenzentrum und nicht im selben Rechenzentrum zur Verfügung stellt. Dies könnte bedeuten, dass Sie alle VMs der Näherungsplatzierungsgruppe in ein anderes Rechenzentrum verschieben müssen.


## <a name="azure-proximity-placement-groups-with-sap-systems-using-azure-vms-exclusively"></a>Azure-Näherungsplatzierungsgruppen mit SAP-Systemen, die ausschließlich Azure-VMs verwenden
Für die meisten SAP NetWeaver- und S/4HANA-Systembereitstellungen unter Azure wird kein [HANA (großen Instanzen)](https://docs.microsoft.com/azure/virtual-machines/workloads/sap/hana-overview-architecture) verwendet. Für die Bereitstellung solcher Systeme ist es wichtig, die optimale Leistung zwischen der SAP-Anwendungsschicht und der DBMS-Schicht zu gewährleisten. Zu diesem Zweck würden Sie eine Azure-Näherungsplatzierungsgruppe exklusiv für dieses System definieren. In den meisten Kundenbereitstellungen haben sich die Kunden dafür entschieden, eine einzige [Azure-Ressourcengruppe](https://docs.microsoft.com/azure/azure-resource-manager/manage-resources-portal) für SAP-Systeme bereitzustellen. In einem solchen Fall besteht eine 1:1-Beziehung zwischen z. B. den Ressourcengruppen des produktiven ERP-Systems und seiner Näherungsplatzierungsgruppe. In einigen anderen Bereitstellungsfällen haben Kunden ihre Ressourcengruppen horizontal organisiert und alle Produktionssysteme in einer einzigen Ressourcengruppe zusammengefasst. In einem solchen Fall würde eine 1:n-Beziehung zwischen der Ressourcengruppe mit produktiven SAP-Systemen und mehreren Näherungsplatzierungsgruppen des produktiven SAP ERP-, SAP BW-Systems usw. bestehen. Eine Bündelung mehrerer oder sogar aller produktiven oder nicht produktiven SAP-Systeme in einer einzigen Näherungsplatzierungsgruppe sollte vermieden werden. In Ausnahmefällen, in denen einige wenige SAP-Systeme oder ein SAP-System und einige umliegende Anwendungen eine Netzwerkkommunikation mit niedriger Latenz erfordern, können Sie diese Systeme ggf. in eine einzelne Näherungsplatzierungsgruppe verschieben. Der Grund für eine solche Empfehlung: Je mehr Systeme Sie in einer Näherungsplatzierungsgruppe gruppieren, desto höher ist die Wahrscheinlichkeit,

- dass Sie einen VM-Typ benötigen, der nicht in dem Rechenzentrum ausgeführt werden kann, in dem die Näherungsplatzierungsgruppe verankert ist.
- dass Ressourcen bestimmter nicht standardmäßiger VMs wie der M-Serie nicht mehr ausgeführt werden konnten, da die Anforderungen steigen, wenn Sie im Laufe der Zeit zusätzliche Software zu einer bestehenden Näherungsplatzierungsgruppe hinzufügen.

Die ideale Nutzung würde folgendermaßen aussehen:

![Näherungsplatzierungsgruppen für alle Azure-VMs](./media/sap-proximity-placement-scenarios/ppg-for-all-azure-vms.png)

In diesem Fall werden einzelne SAP-Systeme in einer Ressourcengruppe mit jeweils einer Näherungsplatzierungsgruppe zusammengefasst. Dies ist unabhängig davon, ob Sie eine Konfiguration für eine horizontale HANA-Skalierung oder eine horizontale DBMS-Skalierung verwenden.


## <a name="azure-proximity-placement-groups-and-hana-large-instances"></a>Azure-Näherungsplatzierungsgruppen und HANA (große Instanzen)
In Fällen, in denen einige Ihrer SAP-Systeme auf [HANA (große Instanzen)](https://docs.microsoft.com/azure/virtual-machines/workloads/sap/hana-overview-architecture) als Anwendungsschicht angewiesen sind, können beträchtliche Verbesserungen in der Netzwerklatenz zwischen HANA (große Instanzen) und Azure-VMs erzielt werden, wenn HANA-Einheiten (große Instanzen) verwendet werden, die in [Reihen oder Stempeln der Version 4](https://docs.microsoft.com/azure/virtual-machines/workloads/sap/hana-network-architecture#networking-architecture-for-hana-large-instance) bereitgestellt werden. Eine der Verbesserungen ist, dass HANA-Einheiten (große Instanzen) eine Näherungsplatzierungsgruppe direkt bei der Bereitstellung erhalten haben. Sie können diese Näherungsplatzierungsgruppe verwenden, um VMs der Anwendungsschicht bereitzustellen. Folglich werden diese VMs im selben Rechenzentrum bereitgestellt, in dem sich auch die HANA-Einheit (große Instanzen) befindet.

Um zu erkennen, ob Ihre HANA-Einheit in einem Stempel oder einer Reihe der Revision 4 bereitgestellt wird, lesen Sie den Artikel [Steuerung von HANA (große Instanzen) in Azure über das Azure-Portal](https://docs.microsoft.com/azure/virtual-machines/workloads/sap/hana-li-portal#look-at-attributes-of-single-hli-unit). In der Attributübersicht der HANA-Einheit (große Instanz) finden Sie auch den Namen der Näherungsplatzierungsgruppe, wie sie zum Bereitstellungszeitpunkt für die betreffende HANA-Einheit angelegt wurde. Der in der Attributübersicht angezeigte Name ist der Name der Näherungsplatzierungsgruppe, in der Sie VMs der Anwendungsschicht bereitstellen sollten.

Im Gegensatz zu SAP-Systemen, die nur virtuelle Azure-Computer verwenden, wird Ihnen bei der Verwendung von HANA (große Instanzen) die Entscheidung, wie viele [Azure-Ressourcengruppen](https://docs.microsoft.com/azure/azure-resource-manager/manage-resources-portal) verwendet werden sollen, bis zu einem gewissen Grad abgenommen. Alle HANA-Einheiten eines [HANA-Mandanten (große Instanzen)](https://docs.microsoft.com/azure/virtual-machines/workloads/sap/hana-know-terms) werden wie [hier](https://docs.microsoft.com/azure/virtual-machines/workloads/sap/hana-li-portal#display-of-hana-large-instance-units-in-the-azure-portal) beschrieben zu einer einzigen Azure-Ressourcengruppe zusammengefasst. Sofern Sie nicht eine Bereitstellung in verschiedenen Mandanten beabsichtigten, um z. B. Produktion und Nicht-Produktion oder bestimmte Systeme zu trennen, werden alle Ihre HANA-Einheiten in einem HANA-Mandanten bereitgestellt, der wiederum eine 1:1-Beziehung zu einer Azure-Ressourcengruppe hat. Demgegenüber weisen alle einzelnen Einheiten eine separate Näherungsplatzierungsgruppe auf. 

Folglich sieht die Gruppierung zwischen Azure Ressourcengruppen und Näherungsplatzierungsgruppen für einen einzelnen Mandanten wie folgt aus:

![Näherungsplatzierungsgruppen für alle Azure-VMs](./media/sap-proximity-placement-scenarios/ppg-for-hana-large-instance-units.png)


## <a name="short-example-of-deploying-with-azure-proximity-placement-groups"></a>Kurzes Beispiel für die Bereitstellung mit Azure-Näherungsplatzierungsgruppen
Um zu veranschaulichen, wie Sie Azure-Näherungsplatzierungsgruppen zum Bereitstellen von VM verwenden können, finden Sie hier eine Liste von PowerShell-Befehlen. Anhand eines Kurzbeispiels wird deren Nutzung für Azure-Näherungsplatzierungsgruppen demonstriert.

Nachdem Sie sich bei Ihrer [Azure Cloud Shell](https://azure.microsoft.com/features/cloud-shell/) angemeldet haben, überprüfen Sie zunächst, ob Sie das richtige Azure-Abonnement nutzen, das Sie für die Bereitstellung mit folgendem Befehl verwenden möchten:

<pre><code>
Get-AzureRmContext
</code></pre>

Wenn Sie zu einem anderen Abonnement wechseln müssen, können Sie dazu den folgenden Befehl ausführen:

<pre><code>
Set-AzureRmContext -Subscription "my PPG test subscription"
</code></pre>

Als dritten Schritt müssen Sie mit folgendem Befehl eine neue Azure-Ressourcengruppe erstellen:

<pre><code>
New-AzResourceGroup -Name "myfirstppgexercise" -Location "westus2"
</code></pre>

Sie können die neue Näherungsplatzierungsgruppe jetzt mit folgendem Befehl anlegen:

<pre><code>
New-AzProximityPlacementGroup -ResourceGroupName "myfirstppgexercise" -Name "letsgetclose" -Location "westus2"
</code></pre>

Anschließend können Sie mit dem Bereitstellen der ersten VM in der Näherungsplatzierungsgruppe beginnen. Verwenden Sie dazu etwa folgenden Befehl:

<pre><code>
New-AzVm -ResourceGroupName "myfirstppgexercise" -Name "myppganchorvm" -Location "westus2" -OpenPorts 80,3389 -ProximityPlacementGroup "letsgetclose" -Size "Standard_DS11_v2"
</code></pre>

Mit dem obigen Befehl wird eine Windows-basierte VM bereitgestellt. Nachdem die VM-Bereitstellung erfolgreich war, wird die Rechenzentrumsreichweite der Näherungsplatzierungsgruppe innerhalb der Azure-Region definiert. Alle nachfolgenden VM-Bereitstellungen, die auf die Näherungsplatzierungsgruppe wie im letzten Befehl verweisen, werden im selben Azure-Rechenzentrum bereitgestellt, solange der VM-Typ auf Hardware gehostet werden kann, die in diesem Rechenzentrum installiert ist, und/oder ausreichend Kapazität für diesen VM-Typ verfügbar ist.

## <a name="combine-availability-sets-and-availability-zones-with-proximity-placement-groups"></a>Kombinieren von Verfügbarkeitsgruppen und Verfügbarkeitszonen mit Näherungsplatzierungsgruppen 
Bei Verwendung von Verfügbarkeitszonen für SAP-Systembereitstellungen besteht einer der Nachteile darin, dass die SAP-Anwendungsschicht nicht über Verfügbarkeitsgruppen innerhalb der jeweiligen Zone gesteuert werden kann. Da Sie beabsichtigen, die SAP-Anwendungsschicht in den gleichen Zonen wie die DBMS-Schicht bereitzustellen und auf eine Verfügbarkeitszone und eine Verfügbarkeitsgruppe verwiesen wird, wenn die Bereitstellung einer einzelnen VM nicht unterstützt wird, waren Sie gezwungen, Ihre Anwendungsschicht durch Verweis auf eine Zone bereitzustellen. Dadurch verlieren Sie die Fähigkeit, die VMs der Anwendungsschicht auf verschiedene Update- und Fehlerdomänen verteilen zu können. Mithilfe von Näherungsplatzierungsgruppen können Sie diese Einschränkung überwinden. Die Reihenfolge der Bereitstellungen würde folgendermaßen aussehen:

- Erstellen Sie eine Näherungsplatzierungsgruppe.
- Stellen Sie Ihre „Anker-VM“ (in der Regel den DBMS-Server) bereit, indem Sie auf eine bestimmte Azure-Verfügbarkeitszone verweisen.
- Erstellen Sie eine Verfügbarkeitsgruppe, die auf die Näherungsplatzierungsgruppe verweist (siehe unten).
- Stellen Sie die VMs der Anwendungsschicht bereit, indem Sie auf die Verfügbarkeitsgruppe und die Näherungsplatzierungsgruppe verweisen.

Anstatt die erste VM wie oben gezeigt bereitzustellen, verweisen Sie beim Bereitstellen der VM auf eine Azure-Verfügbarkeitszone und die Näherungsplatzierungsgruppe:

<pre><code>
New-AzVm -ResourceGroupName "myfirstppgexercise" -Name "myppganchorvm" -Location "westus2" -OpenPorts 80,3389 -Zone "1" -ProximityPlacementGroup "letsgetclose" -Size "Standard_E16_v3"
</code></pre>

Bei erfolgreicher Bereitstellung des virtuellen Computers, der die Datenbankinstanz meines SAP-Systems in einer Azure-Verfügbarkeitszone hosten würde, ist der Umfang der Näherungsplatzierungsgruppe auf eines der Rechenzentren festgelegt, die die von Ihnen definierte Verfügbarkeitszone repräsentieren.

Wir gehen davon aus, dass Sie die zentralen Service-VMs auf die gleiche Weise wie die DBMs-VMs bereitstellen, indem Sie auf die gleiche(n) Zone(n) wie bei den DBMS-VMs und die gleichen Näherungsplatzierungsgruppen verweisen. Im nächsten Schritt müssen Sie die Verfügbarkeitsgruppe(n) anlegen, die Sie für die Anwendungsschicht des SAP-Systems verwenden möchten.
Die Näherungsplatzierungsgruppe muss definiert und angelegt werden. Der Befehl zum Erstellen der Verfügbarkeitsgruppe erfordert einen zusätzlichen Verweis auf die ID der Näherungsplatzierungsgruppe (nicht den Namen). Sie können die ID der Näherungsplatzierungsgruppe mit folgendem Befehl abrufen:



<pre><code>
Get-AzProximityPlacementGroup -ResourceGroupName "myfirstppgexercise" -Name "letsgetclose"
</code></pre>

Beim Erstellen der Verfügbarkeitsgruppe müssen Sie zusätzliche Parameter berücksichtigen, wenn Sie verwaltete Datenträger (Standard, sofern nicht anders angegeben) und Näherungsplatzierungsgruppen verwenden:

<pre><code>
New-AzAvailabilitySet -ResourceGroupName "myfirstppgexercise" -Name "myppgavset" -Location "westus2" -ProximityPlacementGroupId "/subscriptions/my very long ppg id string" -sku "aligned" -PlatformUpdateDomainCount 3 -PlatformFaultDomainCount 2 
</code></pre>

Im Idealfall sollten Sie drei Fehlerdomänen verwenden. Die Anzahl der unterstützten Fehlerdomänen kann jedoch je nach Region unterschiedlich sein. In diesem Fall lag die maximal mögliche Anzahl von Fehlerdomänen für die einzelnen Regionen bei zwei Domänen. Für die Bereitstellung von VMs auf der Anwendungsschicht müssen Sie einen Verweis auf den Namen der Verfügbarkeitsgruppe und den Namen der Näherungsplatzierungsgruppe hinzufügen, wie hier gezeigt:

<pre><code>
New-AzVm -ResourceGroupName "myfirstppgexercise" -Name "myppgavsetappvm" -Location "westus2" -OpenPorts 80,3389 -AvailabilitySetName "myppgavset" -ProximityPlacementGroup "letsgetclose" -Size "Standard_DS11_v2"
</code></pre>

Das Endergebnis dieser Sequenz sind eine DBMS-Schicht und zentrale Dienste des SAP-Systems, die sich in einer oder mehreren bestimmten Verfügbarkeitszonen befinden, und eine SAP-Anwendungsschicht, die durch Verfügbarkeitsgruppen in denselben Azure-Rechenzentren ermittelt wird, in denen auch die DBMS-VM(s) bereitgestellt wurden.

> [!NOTE]
> Wenn Sie eine DBMS-VM in einer Zone und die zweite DBMS-VM in eine anderer Zone bereitstellen, um eine Hochverfügbarkeitskonfiguration zu erhalten, benötigen Sie für jede der Zonen unterschiedliche Näherungsplatzierungsgruppen. Dasselbe gilt für die jeweils verwendete Verfügbarkeitsgruppe.

## <a name="get-an-existing-system-into-azure-proximity-placement-groups"></a>Einbinden eines bestehenden Systems in Azure-Näherungsplatzierungsgruppen
Da Sie bereits SAP-Systeme bereitgestellt haben, sollten Sie die Netzwerklatenz einiger kritischer Systeme optimieren und die Anwendungsschicht und die DBMS-Schicht im selben Rechenzentrum anordnen. In der Phase der öffentlichen Vorschau der Näherungsplatzierungsgruppen-Funktionalität müssen VMs gelöscht und neu erstellt werden, um eine solche Verschiebung in Näherungsplatzierungsgruppen durchzuführen. In diesem Stadium der Funktionalität reicht es nicht aus, die VMs herunterzufahren, um diese heruntergefahrenen virtuellen Computer den Näherungsplatzierungsgruppen zuordnen zu können.


## <a name="next-steps"></a>Nächste Schritte
Lesen Sie die Dokumentation:

- [Prüfliste für die Planung und Bereitstellung von SAP-Workloads in Azure](https://docs.microsoft.com/azure/virtual-machines/workloads/sap/sap-deployment-checklist)
- [Vorschau: Bereitstellen von VMs für Näherungsplatzierungsgruppen mit Azure CLI](https://docs.microsoft.com/azure/virtual-machines/linux/proximity-placement-groups)
- [Vorschau: Bereitstellen von VMs für Näherungsplatzierungsgruppen mit PowerShell](https://docs.microsoft.com/azure/virtual-machines/windows/proximity-placement-groups)
- [Azure Virtual Machines – DBMS-Bereitstellung für SAP-Workload](https://docs.microsoft.com/azure/virtual-machines/workloads/sap/dbms_guide_general)

