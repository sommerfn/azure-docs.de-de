---
title: Azure-Näherungsplatzierungsgruppen für optimale Netzwerklatenz mit SAP-Anwendungen | Microsoft Docs
description: Beschreibt SAP-Bereitstellungsszenarien mit Azure-Näherungsplatzierungsgruppen
services: virtual-machines-linux,virtual-machines-windows
documentationcenter: ''
author: msjuergent
manager: bburns
editor: ''
tags: azure-resource-manager
keywords: ''
ms.service: virtual-machines-linux
ms.devlang: NA
ms.topic: article
ms.tgt_pltfrm: vm-linux
ms.workload: infrastructure
ms.date: 07/15/2019
ms.author: juergent
ms.custom: H1Hack27Feb2017
ms.openlocfilehash: f97cce2d1a8b2de5634215629ddb997dc8f7196a
ms.sourcegitcommit: b2db98f55785ff920140f117bfc01f1177c7f7e2
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 07/16/2019
ms.locfileid: "68235168"
---
# <a name="azure-proximity-placement-groups-for-optimal-network-latency-with-sap-applications"></a>Azure-Näherungsplatzierungsgruppen für optimale Netzwerklatenz mit SAP-Anwendungen
SAP-Anwendungen, die auf der SAP NetWeaver- oder SAP S/4hana-Architektur basieren, sind von der Netzwerklatenz zwischen der SAP-Anwendungsebene und der SAP-Datenbankebene abhängig. Der Grund für die Sensibilität dieser Architekturen liegt darin, dass der Großteil der Geschäftslogik in der Anwendungsschicht ausgeführt wird. Als Ergebnis der Ausführung der Geschäftslogik stellt die SAP-Anwendungsschicht in hoher Frequenz Anfragen an die Datenbank. In den meisten Fällen ist die Art dieser Abfragen recht simpel. Sie können oft auf der Datenbankebene in weniger als 500 Mikrosekunden oder sogar noch weniger ausgeführt werden. Die Zeit, die im Netzwerk verbracht wird, um eine solche Anfrage von der Anwendungsebene an die Datenbankebene zu senden und das von der Datenbankebene zurückgeworfene Ergebnis zu empfangen, hat großen Einfluss auf die Zeit, die für die Ausführung von Geschäftsprozessen benötigt wird. Diese Sensibilität für Netzwerklatenzen ist der Grund dafür, dass in SAP-Bereitstellungsprojekten Zeit aufgewendet werden muss, um eine optimale Netzwerklatenz zu erreichen. Im [SAP-Hinweis 1100926 – Häufig gestellte Fragen: Netzwerkleistung](https://launchpad.support.sap.com/#/notes/1100926/E) hat SAP einige Richtlinien zum Klassifizieren der Netzwerklatenz veröffentlicht.

Auf der einen Seite wuchs in vielen Azure-Regionen die Zahl der Rechenzentren, auch ausgelöst durch die Einführung von Verfügbarkeitszonen. Auf der anderen Seite nutzten Kunden, insbesondere für High-End-SAP-Systeme, speziellere VM-Angebote aus der M-Serie oder HANA (große Instanzen). Arten von virtuellen Azure-Computer, die nicht in allen Rechenzentren in einer bestimmten Azure-Region vorhanden sind. Als Ergebnis dieser beiden Tendenzen erlebten die Kunden Fälle, in denen die Netzwerklatenz nicht im optimalen Bereich lag und in einigen Fällen zu einer suboptimalen Performance ihrer SAP-Systeme führte.

Um dieses Problem zu vermeiden, bietet Azure ein Konstrukt mit dem Namen [Azure-Näherungsplatzierungsgruppe](https://docs.microsoft.com/azure/virtual-machines/linux/co-location) an. Diese neue Funktionalität wurde bereits verwendet, um verschiedene unterschiedliche SAP-Systeme bereitzustellen. Lesen Sie den referenzierten Artikel über Einschränkungen der Platzierungsgruppen. Dieser Artikel behandelt die verschiedenen SAP-Szenarien, in denen Azure-Näherungsplatzierungsgruppen verwendet werden können oder sollten.

## <a name="what-are-proximity-placement-groups"></a>Was sind Näherungsplatzierungsgruppen? 
Eine Azure-Näherungsplatzierungsgruppe ist ein logisches Konstrukt, das in der Definitionsphase an eine Azure-Region und eine Azure-Ressourcengruppe gebunden ist. Während der Bereitstellung von VMS wird auf eine Azure-Näherungsplatzierungsgruppe verwiesen:

- Vom ersten bereitgestellten virtuellen Azure-Computer, der sich im Rechenzentrum befinden soll. Die erste virtuelle Maschine kann als Anker-VM betrachtet werden, die in einem Rechenzentrum bereitgestellt wird, das auf Azure-Allokationsalgorithmen basiert, die schließlich mit Benutzerdefinitionen für eine bestimmte Azure-Verfügbarkeitszone kombiniert werden.
- Von allen nachfolgenden VMs, die mit Bezug auf die Näherungsplatzierungsgruppe bereitgestellt werden, um alle nachfolgenden Azure-VMs im selben Rechenzentrum zu platzieren, in dem auch die erste virtuelle Maschine platziert wurde.

> [!NOTE]
> Wenn keine Host-Hardware bereitgestellt wird, die einen bestimmten VM-Typ im gleichen Rechenzentrum wie die erste VM betreiben könnte, wird die Bereitstellung des gewünschten VM-Typs nicht erfolgreich sein und mit einer Fehlermeldung enden. Dies können Fälle sein, in denen sich mehr ungewöhnliche VMs, wie virtuelle Maschinen mit GPUs oder HPC-VM-Typen, um z. B. eine M-Serie VM anordnen sollten, die als erster VM-Typ bereitgestellt wurde

Einer einzelnen [Azure-Ressourcengruppe](https://docs.microsoft.com/azure/azure-resource-manager/manage-resources-portal) können mehrere Näherungsplatzierungsgruppen zugewiesen werden. Eine Näherungsplatzierungsgruppe kann jedoch nur einer Azure-Ressourcengruppe zugeordnet werden.

Bei der Verwendung von Näherungsplatzierungsgruppen sollten Sie sich über Folgendes im Klaren sein:

- Wenn Sie die optimale Leistung für Ihr SAP-System anstreben und sich durch die Verwendung von Näherungsplatzierungsgruppen auf ein einziges Azure-Rechenzentrum für dieses System beschränken, können Sie möglicherweise nicht alle Arten von VM-Familien innerhalb einer solchen Näherungsplatzierungsgruppe kombinieren. Der Grund dafür ist, dass bestimmte Host-Hardware, die benötigt wird, um einen bestimmten VM-Typ auszuführen, möglicherweise nicht im Rechenzentrum vorhanden ist, in der Ihre Anker-VM der Platzierungsgruppe bereitgestellt wurde
- Während des Lebenszyklus eines solchen SAP-Systems können Sie gezwungen sein, das System in ein anderes Rechenzentrum zu verlagern. Eine solche Verschiebung könnte erzwungen werden, wenn Sie entschieden haben, dass Ihre horizontale Skalierung von HANA-DBMS-Schicht z. B. von vier Knoten auf 16 Knoten wechseln soll. Aber es gibt nicht mehr genug Kapazität, um zusätzliche 12 VMs des Typs zu erhalten, den Sie bereits im selben Rechenzentrum verwendet haben.
- Aufgrund der Außerbetriebnahme von Hardware kann es sein, dass Microsoft Kapazitäten für den/die VM-Typ(en), die Sie verwendet haben, in einem anderen Rechenzentrum anstelle des Rechenzentrums aufbaut, das Sie genutzt hatten. Ein solches Ereignis könnte bedeuten, dass Sie die gesamten VMs der Näherungsplatzierungsgruppe in ein anderes Rechenzentrum verschieben müssten.


## <a name="azure-proximity-placement-groups-with-sap-systems-using-azure-vms-exclusively"></a>Azure-Näherungsplatzierungsgruppen mit SAP-Systemen, die ausschließlich Azure VMs verwenden
Der überwiegende Teil der SAP NetWeaver- und S/4HANA-Systembereitstellungen auf Azure verwendet keine [HANA (großen Instanzen)](https://docs.microsoft.com/azure/virtual-machines/workloads/sap/hana-overview-architecture). Für die Bereitstellung solcher Systeme ist es wichtig, die optimale Performance zwischen der SAP-Anwendungsschicht und der DBMS-Schicht zu gewährleisten. Zu diesem Zweck würden Sie eine Azure-Näherungsplatzierungsgruppe exklusiv für dieses System definieren. In den meisten Kundenbereitstellungen haben sich die Kunden dafür entschieden, eine einzige [Azure-Ressourcengruppe](https://docs.microsoft.com/azure/azure-resource-manager/manage-resources-portal) für SAP-Systeme aufzubauen. In einem solchen Fall besteht eine 1:1-Beziehung zwischen z. B. den Ressourcengruppen des Produktions-ERP-Systems und seiner Näherungsplatzierungsgruppe. In einigen anderen Bereitstellungsfällen haben Kunden ihre Ressourcengruppen horizontal organisiert und alle Produktionssysteme in einer einzigen Ressourcengruppe zusammengefasst. In einem solchen Fall hätten Sie eine 1:n-Beziehung zwischen Ihrer Ressourcengruppe für produktive SAP-Systeme und mehreren Näherungsplatzierungsgruppen Ihrer Produktion SAP ERP, SAP BW usw. Eine Bündelung mehrerer oder sogar aller SAP-Produktions- oder Nicht-Produktionssysteme in einer einzigen Näherungsplatzierungsgruppe sollte vermieden werden. In Ausnahmefällen, in denen eine geringe Anzahl von SAP-Systemen oder ein SAP-System und einige umliegende Anwendungen eine latenzarme Netzwerkkommunikation benötigen, können Sie erwägen, diese Systeme in eine einzelne Näherungsplatzierungsgruppe zu verschieben. Der Grund für eine solche Empfehlung: Je mehr Systeme Sie in einer Näherungsplatzierungsgruppe gruppieren, desto höher ist die Wahrscheinlichkeit, …

- dass Sie einen VM-Typ benötigen, der nicht in dem spezifischen Rechenzentrum ausgeführt werden kann, in dem die Näherungsplatzierungsgruppe verankert ist.
- dass diese Ressourcen bestimmter ungewöhnlicher VMs wie der M-Serie nicht mehr ausgeführt werden konnten, da die Anforderungen steigen, wenn Sie im Laufe der Zeit zusätzliche Software zu einer bestehenden Näherungsplatzierungsgruppe hinzufügen.

Die ideale Nutzung würde folgendermaßen aussehen:

![Näherungsplatzierungsgruppem für alle Azure VMs](./media/sap-proximity-placement-scenarios/ppg-for-all-azure-vms.png)

In diesem Fall werden einzelne SAP-Systeme in jeweils einer Ressourcengruppe mit jeweils einer Näherungsplatzierungsgruppe zusammengefasst. Es besteht keine Abhängigkeit davon, ob Sie eine horizontale Skalierung von HANA oder DBMS-Skalierungskonfigurationen verwenden.


## <a name="azure-proximity-placement-groups-and-hana-large-instances"></a>Azure-Näherungsplatzierungsgruppen und HANA (große Instanzen)
Für die Fälle, in denen einige Ihrer SAP-Systeme auf [HANA (große Instanzen)](https://docs.microsoft.com/azure/virtual-machines/workloads/sap/hana-overview-architecture) als Anwendungsschicht angewiesen sind, können beträchtliche Verbesserungen in der Netzwerklatenz zwischen der HANA (große Instanzen) und Azure VMs erreicht werden, wenn HANA (große Instanzen)-Einheiten verwendet werden, die in [Revision 4-reihig oder gestempelt](https://docs.microsoft.com/azure/virtual-machines/workloads/sap/hana-network-architecture#networking-architecture-for-hana-large-instance) sind. Eine der Verbesserungen ist, dass HANA-Einheiten, als sie bereitgestellt wurden, bereits eine Näherungsplatzierungsgruppe erhalten haben. Sie können diese Näherungsplatzierungsgruppe verwenden, um Ihre VMs der Anwendungsschicht bereitzustellen. Infolgedessen werden diese VMs in demselben Rechenzentrum bereitgestellt, in dem sich auch Ihre HANA-Einheit befindet.

Um zu erkennen, ob Ihre HANA-Einheit in einem Stempel oder einer Zeile der Revision 4 bereitgestellt wird, lesen Sie den Artikel [Steuerung von HANA (große Instanzen) in Azure über das Azure-Portal](https://docs.microsoft.com/azure/virtual-machines/workloads/sap/hana-li-portal#look-at-attributes-of-single-hli-unit). In der Attributübersicht Ihrer HANA Large Instance Einheit finden Sie auch den Namen der Näherungsplatzierungsgruppe, wie sie zum Bereitstellungszeitpunkt für Ihre HANA Large Instance Einheit angelegt wurde. Der in der Übersicht der Attribute angezeigte Name ist der Name der Näherungsplatzierungsgruppe, in der Sie Ihre VMs der Anwendungsebene bereitstellen sollten.

Im Gegensatz zu SAP-Systemen, die nur virtuelle Azure-Computer verwenden, wird Ihnen bei der Verwendung von HANA (große Instanzen) die Entscheidung, wie viele [Ressourcengruppen von Azure](https://docs.microsoft.com/azure/azure-resource-manager/manage-resources-portal) verwendet werden sollen, bis zu einem gewissen Grad abgenommen. Alle HANA-Einheiten eines [HANA-Mandanten (große Instanzen)](https://docs.microsoft.com/azure/virtual-machines/workloads/sap/hana-know-terms) werden wie [hier](https://docs.microsoft.com/azure/virtual-machines/workloads/sap/hana-li-portal#display-of-hana-large-instance-units-in-the-azure-portal) beschrieben zu einer einzigen Azure-Ressourcengruppe zusammengefasst. Sofern Sie nicht eine Bereitstellung in verschiedenen Mandanten beabsichtigten, um z. B. Produktion und Nicht-Produktion oder bestimmte Systeme zu trennen, werden alle Ihre HANA-Einheiten in einem HANA-Mandanten eingesetzt, der wiederum eine 1:1-Beziehung zu einer Azure-Ressourcengruppe hat. Während alle einzelnen Einheiten eine separate Näherungsplatzierungsgruppe aufweisen. 

Infolgedessen wird die Gruppierung zwischen Azure Ressourcengruppen und Näherungsplatzierungsgruppen für einen einzelnen Mandanten folgendermaßen aussehen:

![Näherungsplatzierungsgruppen für alle Azure VMs](./media/sap-proximity-placement-scenarios/ppg-for-hana-large-instance-units.png)


## <a name="short-example-of-deploying-with-azure-proximity-placement-groups"></a>Kurzes Beispiel für die Bereitstellung mit Azure-Näherungsplatzierungsgruppen
Um zu demonstrieren, wie Sie Azure-Näherungsplatzierungsgruppen zum Bereitstellen Ihrer VM verwenden können, finden Sie hier eine Liste von PowerShell-Befehlen, die ihre Verwendung für eine erste kleine Übung mit Azure-Näherungsplatzierungsgruppe demonstrieren

Nachdem Sie sich bei Ihrer [Azure Cloud Shell](https://azure.microsoft.com/features/cloud-shell/) angemeldet haben, überprüfen Sie zunächst, ob Sie das richtige Azure-Abonnement haben, das Sie für die Bereitstellung mit dem Befehl verwenden möchten:

<pre><code>
Get-AzureRmContext
</code></pre>

Wenn Sie zu einem anderen Abonnement wechseln müssen, können Sie dies tun, indem Sie diesen Befehl ausführen:

<pre><code>
Set-AzureRmContext -Subscription "my PPG test subscription"
</code></pre>

Als dritten Schritt müssen Sie mit diesem Befehl eine neue Azure-Ressourcengruppe erstellen:

<pre><code>
New-AzResourceGroup -Name "myfirstppgexercise" -Location "westus2"
</code></pre>

Sie können die neue Näherungsplatzierungsgruppe jetzt anlegen mit:

<pre><code>
New-AzProximityPlacementGroup -ResourceGroupName "myfirstppgexercise" -Name "letsgetclose" -Location "westus2"
</code></pre>

Sie können nun mit dem Bereitstellen Ihrer ersten VM in dieser Näherungsplatzierungsgruppe mit folgendem Befehl beginnen:

<pre><code>
New-AzVm -ResourceGroupName "myfirstppgexercise" -Name "myppganchorvm" -Location "westus2" -OpenPorts 80,3389 -ProximityPlacementGroup "letsgetclose" -Size "Standard_DS11_v2"
</code></pre>

Mit dem obigen Befehl wird eine Windows-basierte VM bereitgestellt. Nachdem diese VM-Bereitstellung erfolgreich war, wird der Rechenzentrumsumfang der Näherungsplatzierungsgruppe innerhalb der Azure-Region definiert. Alle nachfolgenden VM-Bereitstellungen, die auf die Näherungsplatzierungsgruppe wie im letzten Befehl verweisen, werden im gleichen Azure-Rechenzentrum bereitgestellt, solange der VM-Typ auf Hardware gehostet werden kann, die in diesem Rechenzentrum installiert ist, und/oder ausreichend Kapazität für diesen VM-Typ verfügbar ist.

## <a name="combine-availability-sets-and-availability-zones-with-proximity-placement-groups"></a>Kombinieren Sie Verfügbarkeitsgruppen und Verfügbarkeitszonen mit Näherungsplatzierungsgruppen 
Bei der Verwendung von Verfügbarkeitszonen für SAP-System-Bereitstellungen ist einer der Nachteile die Tatsache, dass die SAP-Anwendungsschicht nicht über Verfügbarkeitsgruppen innerhalb der jeweiligen Zone gesteuert werden kann. Da Sie möchten, dass die SAP-Anwendungsschicht in den gleichen Zonen wie die DBMS-Schicht bereitgestellt wird und auf eine Verfügbarkeitszone und eine Verfügbarkeitsgruppe verwiesen wird, wenn die Bereitstellung einer einzelnen VM nicht unterstützt wird, waren Sie gezwungen, Ihre Anwendungsschicht durch Verweisen auf eine Zone bereitzustellen, und verlieren dadurch die Fähigkeit, sicherzustellen, dass die VMs der Anwendungsschicht auf verschiedene Update- und Fehlerdomänen verteilt werden. Mit Hilfe von Näherungsplatzierungsgruppen können Sie diese Einschränkung überwinden. Die Reihenfolge der Bereitstellung würde folgendermaßen aussehen:

- Erstellen einer Näherungsplatzierungsgruppe
- Stellen Sie Ihre „Anker-VM“ bereit, in der Regel den DBMS-Server, indem Sie auf eine bestimmte Azure-Verfügbarkeitszone verweisen
- Erstellen Sie eine Verfügbarkeitsgruppe, die auf die Näherungsplatzierungsgruppe verweist (siehe unten)
- Stellen Sie die VMs der Anwendungsschicht unter Bezugnahme auf die Verfügbarkeitsgruppe und die Näherungsplatzierungsgruppe bereit

Anstatt die erste VM wie oben gezeigt bereitzustellen, verweisen Sie beim Bereitstellen der VM auf eine Azure-Verfügbarkeitszone und die Näherungsplatzierungsgruppe:

<pre><code>
New-AzVm -ResourceGroupName "myfirstppgexercise" -Name "myppganchorvm" -Location "westus2" -OpenPorts 80,3389 -Zone "1" -ProximityPlacementGroup "letsgetclose" -Size "Standard_E16_v3"
</code></pre>

Bei erfolgreicher Bereitstellung dieses virtuellen Computers, der die Datenbankinstanz meines SAP-Systems in einer Azure-Verfügbarkeitszone hosten würde, ist der Umfang der Näherungsplatzierungsgruppe auf eines der Rechenzentren festgelegt, die die von Ihnen definierte Verfügbarkeitszone repräsentieren.

Wir gehen davon aus, dass Sie die zentralen Service-VMs auf die gleiche Weise wie die DBMs-VMs bereitstellen, indem Sie auf die gleiche(n) Zone(n) wie bei den DBMS-VMs und die gleichen Näherungsplatzierungsgruppen verweisen. Im nächsten Schritt müssen Sie die Verfügbarkeitsgruppen anlegen, die Sie für die Anwendungsschicht Ihres SAP-Systems verwenden möchten.
Die Näherungsplatzierungsgruppe muss definiert und angelegt werden. Der Befehl zum Erstellen der Verfügbarkeitsgruppe erfordert einen zusätzlichen Verweis auf die ID der Näherungsplatzierungsgruppe-ID (nicht den Namen). Sie können die ID der Näherungsplatzierungsgruppe erhalten mit:



<pre><code>
Get-AzProximityPlacementGroup -ResourceGroupName "myfirstppgexercise" -Name "letsgetclose"
</code></pre>

Wenn Sie die Verfügbarkeitsgruppe erstellen, müssen Sie zusätzliche Parameter berücksichtigen, wenn Sie verwaltete Datenträger (Standard, sofern nicht anders angegeben) und Näherungsplatzierungsgruppen verwenden:

<pre><code>
New-AzAvailabilitySet -ResourceGroupName "myfirstppgexercise" -Name "myppgavset" -Location "westus2" -ProximityPlacementGroupId "/subscriptions/my very long ppg id string" -sku "aligned" -PlatformUpdateDomainCount 3 -PlatformFaultDomainCount 2 
</code></pre>

Im Idealfall sollten Sie drei Fehlerdomänen verwenden. Die Anzahl der unterstützten Fehlerdomänen kann jedoch von Region zu Region variieren. In diesem Fall war die maximal mögliche Anzahl von Fehlerdomänen für die einzelnen Regionen zwei. Für die Bereitstellung Ihrer VMs auf der Anwendungsschicht müssen Sie einen Verweis auf den Namen Ihrer Verfügbarkeitsgruppen und den Namen der Näherungsplatzierungsgruppe hinzufügen, wie hier gezeigt:

<pre><code>
New-AzVm -ResourceGroupName "myfirstppgexercise" -Name "myppgavsetappvm" -Location "westus2" -OpenPorts 80,3389 -AvailabilitySetName "myppgavset" -ProximityPlacementGroup "letsgetclose" -Size "Standard_DS11_v2"
</code></pre>

Das Endergebnis dieser Sequenz ist eine DBMS-Schicht und zentrale Dienste Ihres SAP-Systems, die sich in einer/mehreren bestimmten Verfügbarkeitszone/n befindet/befinden, und eine SAP-Anwendungsschicht, die sich durch Verfügbarkeitsgruppen in denselben Azure-Rechenzentren befindet, in denen auch die DBMS-VM(s) bereitgestellt wurden.

> [!NOTE]
> Wenn Sie eine DBMS-VM in einer Zone und die zweite DBMS-VM in eine anderer Zone bereitstellen, um eine Hochverfügbarkeitskonfiguration zu erstellen, benötigen Sie für jede der Zonen unterschiedliche Näherungsplatzierungsgruppen. Dasselbe gilt für die Verfügbarkeitsgruppe, die Sie verwenden könnten

## <a name="get-an-existing-system-into-azure-proximity-placement-groups"></a>Ein bestehendes System in Azure-Näherungsplatzierungsgruppe einbinden
Da Sie bereits ein SAP-System bereitgestellt haben, sollten Sie die Netzwerklatenz einiger Ihrer kritischen Systeme optimieren und die Anwendungsschicht und die DBMS-Schicht im selben Rechenzentrum lokalisieren. In der Phase der öffentlichen Vorschau der Funktionalität der Näherungsplatzierungsgruppe ist ein Löschen der VMs und eine Neuerstellung der VMs erforderlich, um eine solche Verschiebung in Näherungsplatzierungsgruppen durchzuführen. In diesem Stadium der Funktionalität reicht es nicht aus, die VMs herunterzufahren, um diese heruntergefahrenen virtuellen Maschinen den Näherungsplatzierungsgruppen zuordnen zu können.


## <a name="next-steps"></a>Nächste Schritte
Lesen Sie die Dokumentation:

- [Prüfliste für die Planung und Bereitstellung von SAP-Workloads in Azure](https://docs.microsoft.com/azure/virtual-machines/workloads/sap/sap-deployment-checklist)
- [Vorschau: Bereitstellen von VMs für Näherungsplatzierungsgruppen mit Azure CLI](https://docs.microsoft.com/azure/virtual-machines/linux/proximity-placement-groups)
- [Vorschau: Bereitstellen von VMs für Näherungsplatzierungsgruppen mit PowerShell](https://docs.microsoft.com/azure/virtual-machines/windows/proximity-placement-groups)
- [Azure Virtual Machines – DBMS-Bereitstellung für SAP-Workload](https://docs.microsoft.com/azure/virtual-machines/workloads/sap/dbms_guide_general)

