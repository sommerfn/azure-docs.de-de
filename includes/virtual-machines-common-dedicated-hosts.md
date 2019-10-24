---
title: include file
description: include file
services: virtual-machines
author: cynthn
ms.service: virtual-machines
ms.topic: include
ms.date: 07/26/2019
ms.author: cynthn
ms.custom: include file
ms.openlocfilehash: 31fdd85fdcc40b38738d33e2c0c13797db7b1d42
ms.sourcegitcommit: bb65043d5e49b8af94bba0e96c36796987f5a2be
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 10/16/2019
ms.locfileid: "72390541"
---
## <a name="benefits"></a>Vorteile 

Die Reservierung des gesamten Hosts bietet die folgenden Vorteile:

-   Hardwareisolation auf physischer Serverebene. Es werden keine anderen virtuellen Computer auf Ihren Hosts platziert. Dedizierte Hosts werden in denselben Rechenzentren bereitgestellt und nutzen dasselbe Netzwerk und dieselbe zugrunde liegende Speicherinfrastruktur wie andere, nicht isolierte Hosts.
-   Kontrolle über Wartungsereignisse, die von der Azure-Plattform initiiert werden. Der Großteil der Wartungsereignisse hat nur geringfügige oder gar keine Auswirkungen auf Ihre virtuellen Computer, es gibt jedoch einige empfindliche Workloads, bei denen jede Sekunde Pause Auswirkungen haben kann. Mit dedizierten Hosts können Sie sich für ein Wartungsfenster anmelden, um die Auswirkungen auf Ihren Dienst zu verringern.
-   Mit dem Azure-Hybridvorteil können Sie Ihre eigenen Lizenzen für Windows und SQL in Azure nutzen. Die Verwendung des Hybridvorteils bietet noch mehr Vorteile. Weitere Informationen finden Sie unter [Azure-Hybridvorteil](https://azure.microsoft.com/pricing/hybrid-benefit/).



## <a name="groups-hosts-and-vms"></a>Gruppen, Hosts und VMs  

![Ansicht der neuen Ressourcen für dedizierte Hosts.](./media/virtual-machines-common-dedicated-hosts/dedicated-hosts2.png)

Eine **Hostgruppe** ist eine Ressource, die eine Sammlung dedizierter Hosts darstellt. Sie erstellen eine Hostgruppe in einer Region und einer Verfügbarkeitszone und fügen ihr Hosts hinzu.

Ein **Host** ist eine Ressource, die einem physischen Server in einem Azure-Rechenzentrum zugeordnet ist. Der physische Server wird beim Erstellen des Hosts zugewiesen. Ein Host wird in einer Hostgruppe erstellt. Für jeden Host gilt eine SKU, die beschreibt, welche VM-Größen erstellt werden können. Auf jedem Host können mehrere VMs unterschiedlicher Größen gehostet werden, sofern sie aus derselben Größenserie stammen.

Wenn Sie eine VM in Azure erstellen, können Sie den dedizierten Host auswählen, der für die VM verwendet werden soll. Sie haben vollständige Kontrolle darüber, welche VMs auf Ihren Hosts platziert werden.


## <a name="high-availability-considerations"></a>Überlegungen zur Hochverfügbarkeit 

Für Hochverfügbarkeit sollten Sie mehrere VMs bereitstellen, die über mehrere Hosts verteilt sind (mindestens 2). Mit dedizierten Azure-Hosts können Sie Ihre Infrastruktur auf unterschiedliche Weise bereitstellen, um Ihre Fehlerisolationsgrenzen zu strukturieren.

### <a name="use-availability-zones-for-fault-isolation"></a>Verwenden von Verfügbarkeitszonen für die Fehlerisolation

Verfügbarkeitszonen sind eindeutige physische Standorte in einer Azure-Region. Jede Zone besteht aus mindestens einem Rechenzentrum, dessen Stromversorgung, Kühlung und Netzwerkbetrieb unabhängig funktionieren. Eine Hostgruppe wird in einer einzelnen Verfügbarkeitszone erstellt. Nach der Erstellung werden alle Hosts in dieser Zone platziert. Um zonenübergreifende Hochverfügbarkeit zu erreichen, müssen Sie mehrere Hostgruppen (eine pro Zone) erstellen und die Hosts entsprechend verteilen.

Wenn Sie einer Verfügbarkeitszone eine Hostgruppe zuweisen, müssen alle auf diesem Host erstellten VMs in derselben Zone erstellt werden.

### <a name="use-fault-domains-for-fault-isolation"></a>Verwenden von Fehlerdomänen für die Fehlerisolation

Ein Host kann in einer bestimmten Fehlerdomäne erstellt werden. Genau wie virtuelle Computer in einer Skalierungsgruppe oder Verfügbarkeitsgruppe werden Hosts in verschiedenen Fehlerdomänen in unterschiedlichen physischen Racks im Rechenzentrum platziert. Wenn Sie eine Hostgruppe erstellen, müssen Sie die Anzahl der Fehlerdomänen angeben. Beim Erstellen von Hosts in der Hostgruppe weisen Sie den einzelnen Hosts Fehlerdomänen zu. Für die VMs ist keine Zuweisung von Fehlerdomänen erforderlich.

Fehlerdomänen sind nicht das gleiche wie eine Näherungsplatzierung. Wenn dieselbe Fehlerdomäne für zwei Hosts verwendet wird, bedeutet das nicht, dass Sie sich nahe beieinander befinden.

Fehlerdomänen beziehen sich auf die Hostgruppe. Sie sollten nicht von Antiaffinität zwischen zwei Hostgruppen ausgehen (es sei denn, sie befinden sich in verschiedenen Verfügbarkeitszonen).

Bei virtuellen Computern, die auf Hosts mit unterschiedlichen Fehlerdomänen bereitgestellt werden, befinden sich die zugrunde liegenden Dienste für verwaltete Datenträger in mehreren Speicherstempeln, um den Schutz durch Fehlerisolation zu erhöhen.

### <a name="using-availability-zones-and-fault-domains"></a>Verwenden von Verfügbarkeitszonen und Fehlerdomänen

Sie können beide Funktionen kombinieren, um eine noch höhere Fehlerisolation zu erzielen. In diesem Fall geben Sie die Verfügbarkeitszone und die Anzahl der Fehlerdomänen für jede Hostgruppe an und weisen jedem Host in der Gruppe eine Fehlerdomäne und jeder VM eine Verfügbarkeitszone zu.

[Hier](https://github.com/Azure/azure-quickstart-templates/blob/master/201-vm-dedicated-hosts/README.md) finden Sie die Resource Manager-Beispielvorlage, die für maximale Resilienz in einer Region Zonen und Fehlerdomänen zum Verteilen der Hosts verwendet.

## <a name="maintenance-control"></a>Wartungssteuerung

Die Infrastruktur, die Ihre virtuellen Computer unterstützt, kann gelegentlich aktualisiert werden, um die Zuverlässigkeit, Leistung und Sicherheit zu verbessern und neue Features zu integrieren. Die Azure-Plattform versucht, die Auswirkungen der Plattformwartung möglichst gering zu halten, aber Kunden mit *wartungsempfindlichen* Workloads können nicht einmal wenige Sekunden tolerieren, in denen die VM zur Wartung eingefroren oder getrennt werden muss.

Die **Wartungssteuerung** bietet Kunden eine Option zum Überspringen regulärer Plattformupdates, die für ihre dedizierten Hosts geplant sind, und wendet sie dann zu einem ausgewählten Zeitpunkt innerhalb eines 35-tägigen gleitenden Fensters an.

> [!NOTE]
>  Die Wartungssteuerung befindet sich derzeit in einer begrenzten Vorschau und erfordert ein Onboarding. Bewerben Sie sich für diese Vorschau, indem Sie an einer [Nominierungsumfrage](https://forms.office.com/Pages/ResponsePage.aspx?id=v4j5cvGGr0GRqy180BHbR6lJf7DwiQxNmz51ksQvxV9UNUM3UllWUjBMTFZQUFhHUDI0VTBPQlJFNS4u) teilnehmen.

## <a name="capacity-considerations"></a>Überlegungen zur Kapazität

Nach der Bereitstellung eines dedizierten Hosts weist Azure ihn dem physischen Server zu. Dadurch wird sichergestellt, dass die Kapazität verfügbar ist, wenn Sie Ihren virtuellen Computer bereitstellen müssen. Azure nutzt die gesamte Kapazität in der Region (oder Zone), um einen physischen Server für Ihren Host auszuwählen. Das bedeutet auch, dass Kunden davon ausgehen können, ihren Bedarf an dedizierten Hosts nach Wunsch zu vergrößern, ohne auf mangelnden Speicherplatz im Cluster zu stoßen.

## <a name="quotas"></a>Kontingente

Es gibt eine Standardkontingentgrenze von 3.000 vCPUs für dedizierte Hosts pro Region. Die Anzahl der Hosts, die Sie bereitstellen können, ist jedoch auch durch das Kontingent für die für den Host verwendete VM-Größenfamilie beschränkt. Beispielsweise ist für ein Abonnement mit **nutzungsbasierter Bezahlung** in der Region „USA, Osten“ möglicherweise nur ein Kontingent von 10 vCPUs für die Größenserie Dsv3 verfügbar. In diesem Fall müssen Sie eine Kontingenterhöhung auf mindestens 64 vCPUs anfordern, bevor Sie einen dedizierten Host bereitstellen können. Wählen Sie in der rechten oberen Ecke die Schaltfläche **Erhöhung anfordern** aus, um bei Bedarf eine Anforderung einzureichen.

![Screenshot der Seite für Verbrauch und Kontingente im Portal](./media/virtual-machines-common-dedicated-hosts/quotas.png)

Weitere Informationen finden Sie unter [vCPU-Kontingente für virtuelle Computer](/azure/virtual-machines/windows/quotas).

Für kostenlose Testversionen und MSDN-Abonnements gilt kein Kontingent für dedizierte Azure-Hosts.

## <a name="pricing"></a>Preise

Benutzern wird die Anzahl dedizierter Hosts in Rechnung gestellt, unabhängig davon, wie viele VMs bereitgestellt sind. In Ihrem monatlichen Kontoauszug wird ein neuer abrechenbarer Ressourcentyp von Hosts aufgeführt. Die VMs auf einem dedizierten Host werden weiterhin in Ihrem Kontoauszug angezeigt, sie weisen jedoch einen Preis von 0 auf.

Der Hostpreis wird anhand der VM-Familie, dem Typ (Hardwaregröße) und der Region festgelegt. Ein Hostpreis ist relativ zur höchsten auf dem Host unterstützten VM-Größe.

Softwarelizenzierung, Speicher und Netzwerknutzung werden separat vom Host und den VMs abgerechnet. An diesen abrechenbaren Elementen finden keine Änderungen statt.

Weitere Informationen finden Sie unter [Azure Dedicated Host – Preise](https://aka.ms/ADHPricing).
 
## <a name="vm-families-and-hardware-generations"></a>VM-Familien und Hardwaregenerationen

Eine SKU ist für einen Host definiert und stellt die Größenserie und den Typ der VM dar. Sie können mehrere VMs unterschiedlicher Größe innerhalb eines einzelnen Hosts kombinieren, solange diese derselben Größenserie angehören. Der Typ ist die Hardwaregeneration, die derzeit in der Region verfügbar ist.

Die `types` der unterschiedlichen VM-Serien stammen von unterschiedlichen CPU-Anbietern und weisen andere CPU-Generationen und Anzahlen von Kernen auf.

Weitere Informationen finden Sie unter [Azure Dedicated Host – Preise](https://aka.ms/ADHPricing).

Während der Vorschauphase werden die folgenden Host-SKU-Typen unterstützt:  DSv3_Type1 und ESv3_Type1

 
## <a name="host-life-cycle"></a>Hostlebenszyklus


Azure überwacht und verwaltet den Integritätsstatus Ihrer Hosts. Die folgenden Zustände werden zurückgegeben, wenn Sie Ihren Host abfragen:

| Integritätsstatus   | BESCHREIBUNG       |
|----------|----------------|
| Host verfügbar     | Es liegen keine bekannten Probleme mit dem Host vor.   |
| Host wird untersucht  | Es liegen einige Probleme mit dem Host vor, die derzeit untersucht werden. Dies ist ein erforderlicher Übergangszustand, in dem Azure versucht, den Umfang und die Ursache des identifizierten Problems zu identifizieren. Auf dem Host ausgeführte virtuelle Computer können beeinträchtigt werden. |
| Ausstehende Aufhebung der Freigabe für den Host   | Azure kann den Host nicht wieder in einen fehlerfreien Status versetzen, und Sie werden aufgefordert, Ihre virtuellen Computer außerhalb dieses Hosts erneut bereitzustellen. Wenn `autoReplaceOnFailure` aktiviert ist, werden Ihre virtuellen Computer für eine fehlerfreie Hardware *wieder einsatzfähig gemacht*. Andernfalls wird der virtuelle Computer möglicherweise auf einem Host ausgeführt, der in Kürze ausfallen könnte.|
| Zuordnung für Host aufgehoben  | Alle virtuellen Computer wurden vom Host entfernt. Dieser Host wird Ihnen nicht mehr in Rechnung gestellt, da die Hardware aus der Rotation entfernt wurde.   |

