---
title: include file
description: include file
services: virtual-machines
author: cynthn
ms.service: virtual-machines
ms.topic: include
ms.date: 05/10/2019
ms.author: cynthn
ms.custom: include file
ms.openlocfilehash: 1582f61befb9b6d71adbfda9482175d67874ffb9
ms.sourcegitcommit: fa45c2bcd1b32bc8dd54a5dc8bc206d2fe23d5fb
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 07/12/2019
ms.locfileid: "67850651"
---
# <a name="availability-options-for-virtual-machines-in-azure"></a>Verfügbarkeitsoptionen für virtuelle Computer in Azure
Dieser Artikel enthält eine Übersicht über die Verfügbarkeitsfunktionen von virtuellen Azure-Computern (VMs).


## <a name="availability-sets"></a>Verfügbarkeitsgruppen
Eine Verfügbarkeitsgruppe ist eine logische Gruppierung von virtuellen Computern in einem Datencenter, über die Azure erkennen kann, wie Ihre Anwendung erstellt wurde, um für Redundanz und Verfügbarkeit zu sorgen. Es empfiehlt sich, mindestens zwei virtuelle Computer in einer Verfügbarkeitsgruppe zu erstellen, um eine Anwendung mit hoher Verfügbarkeit zu erhalten und die [Azure-SLA von 99,95 Prozent](https://azure.microsoft.com/support/legal/sla/virtual-machines/) zu erfüllen. Für die Verfügbarkeitsgruppe selbst entstehen keine Kosten. Sie bezahlen nur für die einzelnen VM-Instanzen, die Sie erstellen. Wenn ein einzelner virtuelle Computer [Azure Premium SSD](../articles/virtual-machines/windows/disks-types.md#premium-ssd) verwendet, gilt die Azure-SLA für ungeplante Wartungsereignisse.

Eine Verfügbarkeitsgruppe besteht aus zwei zusätzlichen Gruppierungen, die vor Hardwareausfällen schützen und die sichere Anwendung von Updates ermöglichen: Fehlerdomänen (FDs) und Updatedomänen (UDs). Erfahren Sie mehr über das Verwalten der Verfügbarkeit von [virtuellen Linux-Computern](../articles/virtual-machines/linux/manage-availability.md) oder [virtuellen Windows-Computern](../articles/virtual-machines/windows/manage-availability.md).

### <a name="fault-domains"></a>Fehlerdomänen
Eine Fehlerdomäne ist eine logische Gruppe von zugrunde liegender Hardware mit einer gemeinsamen Stromquelle und einem Netzwerkswitch, ähnlich einem Rack in einem lokalen Rechenzentrum. Wenn Sie VMs in einer Verfügbarkeitsgruppe erstellen, werden Ihre VMs von der Azure-Plattform automatisch auf diese Fehlerdomänen verteilt. Bei diesem Ansatz werden die Auswirkungen von potenziellen Hardwareausfällen, Netzwerkausfällen oder Stromausfällen beschränkt.

### <a name="update-domains"></a>Updatedomänen
Eine Updatedomäne ist eine logische Gruppe von zugrunde liegender Hardware, die zur gleichen Zeit gewartet oder neu gestartet werden kann. Wenn Sie VMs in einer Verfügbarkeitsgruppe erstellen, werden Ihre VMs von der Azure-Plattform automatisch auf diese Updatedomänen verteilt. Mit diesem Ansatz wird sichergestellt, dass mindestens eine Instanz Ihrer Anwendung immer ausgeführt wird, wenn für die Azure-Plattform die regelmäßige Wartung durchgeführt wird. Während einer geplanten Wartung werden die Updatedomänen unter Umständen nicht der Reihe nach neu gestartet, sondern es wird jeweils nur eine Updatedomäne neu gestartet.

![Verfügbarkeitsgruppen](./media/virtual-machines-common-manage-availability/ud-fd-configuration.png)

### <a name="managed-disk-fault-domains"></a>Fehlerdomänen für verwaltete Datenträger
Virtuelle Computer mit [Azure Managed Disks](../articles/virtual-machines/windows/faq-for-disks.md) werden bei Verwendung einer verwalteten Verfügbarkeitsgruppe auf Fehlerdomänen für verwaltete Datenträger ausgerichtet. Diese Ausrichtung stellt sicher, dass sich alle verwalteten, an einen virtuellen Computer angefügten Datenträger innerhalb der gleichen Fehlerdomäne für verwaltete Datenträger befinden. Nur virtuelle Computer mit verwalteten Datenträgern können in einer verwalteten Verfügbarkeitsgruppe erstellt werden. Die Anzahl der Fehlerdomänen für verwaltete Datenträger variiert je nach Region – pro Region sind entweder zwei oder drei Fehlerdomänen für verwaltete Datenträger vorhanden. Sie können sich weiter über diese Fehlerdomänen für verwaltete Datenträger für [Linux-VMs](../articles/virtual-machines/linux/manage-availability.md?#use-managed-disks-for-vms-in-an-availability-set) oder [Windows-VMs](../articles/virtual-machines/windows/manage-availability.md?#use-managed-disks-for-vms-in-an-availability-set) informieren.

![Verwaltete Verfügbarkeitsgruppe](./media/virtual-machines-common-manage-availability/md-fd-updated.png)

## <a name="availability-zones"></a>Verfügbarkeitszonen

Mit [Verfügbarkeitszonen](../articles/availability-zones/az-overview.md), bei denen es sich um eine Alternative zu Verfügbarkeitsgruppen handelt, erhalten Sie mehr Kontrolle beim Aufrechterhalten der Verfügbarkeit von Anwendungen und Daten auf Ihren virtuellen Computern. Eine Verfügbarkeitszone ist eine physisch getrennte Zone in einer Azure-Region. Pro unterstützter Azure-Region sind drei Verfügbarkeitszonen vorhanden. Jede Verfügbarkeitszone verfügt über eine eigene Stromquelle, ein Netzwerk und eine Kühlung. Indem Sie Ihre Lösungen für die Verwendung von replizierten VMs in Zonen gestalten, können Sie Ihre Apps und Daten vor dem Verlust eines Datencenters schützen. Wenn eine Zone kompromittiert ist, sind replizierte Apps und Daten sofort in einer anderen Zone verfügbar. 

![Verfügbarkeitszonen](./media/virtual-machines-common-regions-and-availability/three-zones-per-region.png)

Informieren Sie sich über das Bereitstellen einer [Windows](../articles/virtual-machines/windows/create-powershell-availability-zone.md)- oder [Linux](../articles/virtual-machines/linux/create-cli-availability-zone.md)-VM in einer Verfügbarkeitszone.


## <a name="next-steps"></a>Nächste Schritte
Sie können diese Verfügbarkeits- und Redundanzfunktionen jetzt nutzen, um Ihre Azure-Umgebung zu erstellen. Weitere Informationen zu bewährten Methoden finden Sie unter [Checkliste für die Verfügbarkeit](../articles/best-practices-availability-checklist.md).

