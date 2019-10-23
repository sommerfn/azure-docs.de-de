---
title: 'Kostenanalyse und Budget: Azure Batch'
description: Es wird beschrieben, wie Sie eine Kostenanalyse abrufen und ein Budget für Ihre Batch-Workload festlegen können.
services: batch
author: laurenhughes
manager: gwallace
ms.service: batch
ms.topic: article
ms.tgt_pltfrm: vm-windows
ms.workload: big-compute
ms.date: 07/19/2019
ms.author: lahugh
ms.openlocfilehash: 6ccf530fe2164b3d9b1936648ffe9057c334efd6
ms.sourcegitcommit: e0e6663a2d6672a9d916d64d14d63633934d2952
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 10/21/2019
ms.locfileid: "70094200"
---
# <a name="cost-analysis-and-budgets-for-azure-batch"></a>Kostenanalyse und Budget für Azure Batch

Für Azure Batch selbst werden keine Gebühren berechnet, sondern nur für die zugrunde liegenden Computeressourcen und Softwarelizenzen, die zum Ausführen von Batch-Workloads verwendet werden. Generell fallen Kosten für virtuelle Computer (VMs) in einem Pool, für die Datenübertragung von der VM oder für in der Cloud gespeicherte Eingabe- oder Ausgabedaten an. Wir sehen uns nun einige wichtige Komponenten von Batch an, um zu verstehen, woher die Kosten stammen und wie ein Budget für einen Pool oder ein Konto festgelegt wird. Außerdem werden einige Verfahren zur Verbesserung der Kosteneffizienz von Batch-Workloads beschrieben.

## <a name="batch-resources"></a>Batch-Ressourcen

Virtuelle Computer sind die wichtigste Ressource, die für die Batch-Verarbeitung verwendet wird. Die Kosten für die Verwendung von VMs für Batch wird basierend auf dem Typ, der Menge und der Nutzungsdauer berechnet. Zu den VM-Abrechnungsoptionen gehören die [nutzungsbasierte Bezahlung](https://azure.microsoft.com/offers/ms-azr-0003p/) und die [Reservierung](../billing/billing-save-compute-costs-reservations.md) (Zahlung im Voraus). Beide Optionen für die Bezahlung haben bestimmte Vorteile, die von Ihrer Compute-Workload abhängen, und beide Zahlungsmodelle wirken sich anders auf Ihre Rechnung aus.

Bei der Bereitstellung von Anwendungen auf Batch-Knoten (VMs) mit [Anwendungspaketen](batch-application-packages.md) werden Ihnen die Azure Storage-Ressourcen in Rechnung gestellt, die von Ihren Anwendungspaketen verbraucht wurden. Darüber hinaus wird Ihnen die Speicherung von Eingabe- oder Ausgabedateien berechnet, z. B. Ressourcendateien und andere Protokolldaten. Im Allgemeinen liegen die Kosten für Speicherdaten in Verbindung mit Batch deutlich unter den Kosten für Computeressourcen. Jeder virtuelle Computer in einem Pool, der mit **VirtualMachineConfiguration** erstellt wurde, verfügt über einen zugeordneten Betriebssystemdatenträger, für den von Azure verwaltete Datenträger verwendet werden. Von Azure verwaltete Datenträger weisen zusätzliche Kosten auf, und auch für andere Datenträger-Leistungsstufen gelten noch andere Kosten.

Für Batch-Pools werden Netzwerkressourcen verwendet. Insbesondere werden für Pools vom Typ **VirtualMachineConfiguration** Standard-Lastenausgleichsmodule verwendet, für die statische IP-Adressen erforderlich sind. Die von Batch genutzten Lastenausgleichsmodule sind für Konten vom Typ **Benutzerabonnement** sichtbar, aber nicht für Konten vom Typ **Batch-Dienst**. Für Standard-Lastenausgleichsmodule fallen Gebühren für alle Daten an, die für Batch-Pool-VMs übergeben werden. Für einige Batch-APIs, mit denen Daten aus Poolknoten (z. B. Task/Knotendatei abrufen), Taskanwendungspaketen, Ressourcen-/Ausgabedateien und Containerimages abgerufen werden, fallen ebenfalls Gebühren an.

### <a name="additional-services"></a>Zusätzliche Dienste

Dienste ohne VMs und Speicher können in die Kosten für Ihr Batch-Konto integriert werden.

Andere Dienste, die häufig mit Batch verwendet werden, sind:

- Application Insights
- Data Factory
- Azure Monitor
- Virtual Network
- VMs mit Grafikanwendungen

Je nach den Diensten, die Sie mit Ihrer Batch-Lösung nutzen, fallen unter Umständen weitere Gebühren an. Mit dem [Preisrechner](https://azure.microsoft.com/pricing/calculator/) können Sie die Kosten für die einzelnen zusätzlichen Dienste ermitteln.

## <a name="cost-analysis-and-budget-for-a-pool"></a>Kostenanalyse und Budget für einen Pool

Mit dem Azure-Portal können Sie Budgets und Ausgabenwarnungen für Ihre Batch-Pools bzw. Ihr Batch-Konto erstellen. Budgets und Warnungen sind hilfreich, um Beteiligte über die Risiken von Budgetüberschreitungen zu informieren. Es ist möglich, dass es bei den Ausgabenwarnungen zu einer Verzögerung kommt und ein Budget leicht überschritten wird. In diesem Beispiel sehen wir uns die Kostenanalyse eines einzelnen Batch-Pools an.

1. Wählen Sie im Azure-Portal in der Navigationsleiste auf der linken Seite die Option **Kostenverwaltung + Abrechnung**.
1. Wählen Sie im Abschnitt **Meine Abonnements** Ihr Abonnement aus.
1. Navigieren Sie im Abschnitt **Kostenverwaltung** in der linken Navigationsleiste zu **Kostenanalyse**. Sie sehen die folgende Ansicht:
1. Wählen Sie **+ Filter hinzufügen**. Wählen Sie in der ersten Dropdownliste die Option **Ressource** ![Ressourcenfilter auswählen](./media/batch-budget/resource-filter.png).
1. Wählen Sie in der zweiten Dropdownliste den Batch-Pool aus. Nach dem Auswählen des Pools sieht die Kostenanalyse in etwa wie folgt aus:
    ![Kostenanalyse eines Pools](./media/batch-budget/pool-cost-analysis.png)

In der sich ergebenden Kostenanalyse werden die Kosten des Pools und die Ressourcen angezeigt, die zu diesen Kosten beitragen. In diesem Beispiel sind die im Pool verwendeten VMs die teuerste Ressource.

Wählen Sie zum Erstellen eines Budgets für den Pool die Option **Budget: Kein** und dann **Neues Budget erstellen >** . Verwenden Sie jetzt das Fenster, um ein Budget speziell für Ihren Pool zu konfigurieren.

Weitere Informationen zur Konfiguration eines Budgets finden Sie unter [Erstellen und Verwalten von Azure-Budgets](../cost-management/tutorial-acm-create-budgets.md).

> [!NOTE]
> Azure Batch basiert auf der Technologie von Azure Cloud Services und von Azure Virtual Machines. Wenn Sie sich für die **Cloud Services-Konfiguration** entscheiden, erfolgt die Abrechnung auf Grundlage der Preisstruktur, die auf der Seite mit den Cloud Services-Preisen angegeben ist. Wenn Sie sich für die **VM-Konfiguration** entscheiden, erfolgt die Abrechnung auf Grundlage der Preisstruktur unter „Virtuelle Computer – Preise“. Im Beispiel auf dieser Seite wird **Konfiguration des virtuellen Computers** verwendet.

## <a name="minimize-cost"></a>Verringern der Kosten

Die Verwendung mehrerer VMs und Azure-Dienste über längere Zeiträume kann teuer werden. Glücklicherweise gibt es Dienste, mit denen Sie Ihre Ausgaben verringern können, und es sind Strategien zur Steigerung der Effizienz Ihrer Workload verfügbar.

### <a name="low-priority-virtual-machines"></a>Virtuelle Computer mit niedriger Priorität

Virtuelle Computer mit niedriger Priorität führen zu einer Reduzierung der Kosten von Batch-Workloads, indem in Azure überschüssige Rechenkapazität genutzt wird. Wenn Sie in Ihren Pools VMs mit niedriger Priorität angeben, nutzt Batch diese überschüssige Kapazität zum Ausführen Ihrer Workload. Es können erhebliche Kosteneinsparungen erzielt werden, indem VMs mit niedriger Priorität anstelle von dedizierten VMs genutzt werden.

Weitere Informationen zum Einrichten von VMs mit niedriger Priorität für Ihre Workload finden Sie unter [Verwenden von VMs mit niedriger Priorität mit Batch](batch-low-pri-vms.md).

### <a name="virtual-machine-os-disk-type"></a>Typen von Betriebssystemdatenträgern für VMs

Es gibt mehrere [Typen von Betriebssystemdatenträgern für VMs](../virtual-machines/windows/disks-types.md). Die meisten VM-Serien verfügen über Größen, die sowohl Premium- als auch Standard-Speicher unterstützen. Wenn für einen Pool die VM-Größe „S“ ausgewählt wird, werden von Batch SSD Premium-Betriebssystemdatenträger konfiguriert. Bei Auswahl der VM-Größe „Nicht S“ wird der kostengünstigere Datenträgertyp „HDD Standard“ verwendet. Beispielsweise werden SSD Premium-Betriebssystemdatenträger für `Standard_D2s_v3` und HDD Standard-Betriebssystemdatenträger für `Standard_D2_v3` verwendet.

SSD Premium-Betriebssystemdatenträger sind teurer, verfügen aber auch über eine höhere Leistung. VMs mit Premium-Datenträgern können etwas schneller als VMs mit HDD Standard-Betriebssystemdatenträgern gestartet werden. Bei Batch wird der Betriebssystemdatenträger häufig nicht viel genutzt, da sich die Anwendungen und Taskdateien auf dem temporären SSD-Datenträger der VMs befinden. Daher ist es in vielen Fällen nicht erforderlich, die höheren Kosten für SSD Premium-Datenträger zu bezahlen, die bereitgestellt werden, wenn „S“ als VM-Größe angegeben wird.

### <a name="reserved-virtual-machine-instances"></a>Reservierte VM-Instanzen

Falls Sie Batch längere Zeit nutzen möchten, können Sie bei den Kosten für die VMs sparen, indem Sie [Azure-Reservierungen](../billing/billing-save-compute-costs-reservations.md) für Ihre Workloads verwenden. Reservierungsraten sind erheblich niedriger als die Raten bei der nutzungsbasierten Bezahlung. Für VM-Instanzen, die ohne Reservierung verwendet werden, wird die Rate für die nutzungsbasierte Bezahlung berechnet. Bei Erwerb einer Reservierung wird der Reservierungsrabatt angewendet, und Ihnen werden nicht mehr die Raten für die nutzungsbasierte Bezahlung berechnet.

### <a name="automatic-scaling"></a>Autoskalierung

Bei der [Autoskalierung](batch-automatic-scaling.md) wird die Anzahl von VMs in Ihrem Batch-Pool je nach dem Bedarf des aktuellen Auftrags dynamisch skaliert. Indem der Pool basierend auf der Lebensdauer eines Auftrags skaliert wird, wird mit der Autoskalierung sichergestellt, dass VMs zentral hochskaliert und nur verwendet werden, wenn ein Auftrag durchgeführt werden muss. Wenn der Auftrag abgeschlossen ist oder keine Aufträge vorhanden sind, werden die VMs automatisch zentral herunterskaliert, um Computeressourcen zu sparen. Die Skalierung ermöglicht es Ihnen, die Gesamtkosten Ihrer Batch-Lösung zu reduzieren, indem Sie nur die benötigten Ressourcen verwenden.

Weitere Informationen zur Autoskalierung finden Sie unter [Automatisches Skalieren von Computeknoten in einem Azure Batch-Pool](batch-automatic-scaling.md).

## <a name="next-steps"></a>Nächste Schritte

- Informieren Sie sich weiter über die [Batch-APIs und Tools](batch-apis-tools.md), die für die Erstellung und Überwachung von Batch-Lösungen verfügbar sind.  

- Informieren Sie sich über das [Verwenden von VMs mit niedriger Priorität mit Batch](batch-low-pri-vms.md).
