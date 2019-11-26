---
title: Verschieben von Azure-VMs in eine andere Region mit Azure Site Recovery
description: Verwenden Sie Azure Site Recovery, um virtuelle Azure-Computer aus einer Azure-Region in eine andere zu verschieben.
author: rajani-janaki-ram
ms.service: site-recovery
ms.topic: tutorial
ms.date: 01/28/2019
ms.author: rajanaki
ms.custom: MVC
ms.openlocfilehash: 05d0fcb9e689b8954bffaff402475762344c023d
ms.sourcegitcommit: a170b69b592e6e7e5cc816dabc0246f97897cb0c
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 11/14/2019
ms.locfileid: "74091942"
---
# <a name="moving-azure-vms-to-another-azure-region"></a>Verschieben von Azure-VMs in eine andere Azure-Region

Dieser Artikel enthält eine Übersicht über die Gründe und Schritte in Bezug auf das Verschieben von Azure-VMs in eine andere Azure-Region mit [Azure Site Recovery](site-recovery-overview.md). 


## <a name="reasons-to-move-azure-vms"></a>Gründe für das Verschieben virtueller Azure-Computer

Sie können virtuelle Computer aus folgenden Gründen verschieben:

- Sie haben bereits virtuelle Computer in einer Region bereitgestellt, und eine neue Region wurde hinzugefügt, die sich näher an den Endbenutzern Ihrer Anwendung oder Ihres Diensts befindet. In diesem Szenario können Sie Ihre virtuellen Computer unverändert in die neue Region verschieben, um die Wartezeit zu verringern. Die gleiche Vorgehensweise können Sie verwenden, wenn Sie Abonnements konsolidieren möchten oder Governance- oder Organisationsregeln eine Verschiebung erforderlich machen.
- Ihr virtueller Computer wurde als Einzelinstanz-VM oder als Teil einer Verfügbarkeitsgruppe bereitgestellt. Wenn Sie die Verfügbarkeits-SLAs erhöhen möchten, können Sie den virtuellen Computer in eine Verfügbarkeitszone verschieben.

## <a name="steps-to-move-azure-vms"></a>Schritte zum Verschieben virtueller Azure-Computer

Das Verschieben virtueller Computer umfasst folgende Schritte:

1. Vergewissern Sie sich, dass die Voraussetzungen erfüllt sind.
2. Bereiten Sie die virtuellen Quellcomputer vor.
3. Bereiten Sie die Zielregion vor.
4. Kopieren Sie die Daten in die Zielregion. Verwenden Sie die Azure Site Recovery-Replikationstechnologie, um die Daten des virtuellen Quellcomputers in die Zielregion zu kopieren.
5. Testen Sie die Konfiguration. Testen Sie nach Abschluss der Replikation die Konfiguration mithilfe eines Testfailovers auf ein Netzwerk, das nicht in die Produktion eingebunden ist.
6. Führen Sie die Verschiebung durch.
7. Verwerfen Sie die Ressourcen in der Quellregion.

> [!NOTE]
> Details zu diesen Schritten finden Sie in den folgenden Abschnitten.
> [!IMPORTANT]
> Azure Site Recovery unterstützt aktuell das Verschieben virtueller Computer zwischen Regionen, aber keine Verschiebung innerhalb einer Region.

## <a name="typical-architectures-for-a-multi-tier-deployment"></a>Typische Architekturen für eine Bereitstellung mit mehreren Ebenen

In diesem Abschnitt werden die gängigsten Bereitstellungsarchitekturen für eine Anwendung mit mehreren Ebenen in Azure erläutert. Als Beispiel dient eine Anwendung mit drei Ebenen und einer öffentlichen IP-Adresse. Die einzelnen Ebenen (Web, Anwendung und Datenbank) enthalten jeweils zwei virtuelle Computer und sind über Azure Load Balancer miteinander verbunden. Die Datenbankebene verfügt dank SQL Server Always On-Replikation zwischen den virtuellen Computern über Hochverfügbarkeit.

* **Einzelinstanz-VMs mit Bereitstellung über verschiedene Ebenen:** Jeder virtuelle Computer auf einer Ebene ist als Einzelinstanz-VM konfiguriert und über Load Balancer mit den anderen Ebenen verbunden. Diese Konfiguration lässt sich am einfachsten übernehmen.

     ![Bereitstellung von Einzelinstanz-VMs über verschiedene Ebenen](media/move-vm-overview/regular-deployment.png)

* **Virtuelle Computer auf den einzelnen Ebenen mit Bereitstellung über Verfügbarkeitsgruppen:** Jeder virtuelle Computer auf einer Ebene wird in einer Verfügbarkeitsgruppe konfiguriert. [Verfügbarkeitsgruppen](https://docs.microsoft.com/azure/virtual-machines/windows/tutorial-availability-sets) sorgen dafür, dass die von Ihnen in Azure bereitgestellten virtuellen Computer auf mehrere isolierte Hardwareknoten in einem Cluster verteilt werden. Dadurch wird sichergestellt, dass sich Hardware- oder Softwarefehler in Azure nur auf einen Teil Ihrer virtuellen Computer auswirken und die Lösung insgesamt verfügbar und betriebsbereit bleibt.

     ![VM-Bereitstellung über Verfügbarkeitsgruppen](media/move-vm-overview/avset.png)

* **Virtuelle Computer auf den einzelnen Ebenen mit Bereitstellung über Verfügbarkeitszonen:** Jeder virtuelle Computer auf einer Ebene wird über [Verfügbarkeitszonen](https://docs.microsoft.com/azure/availability-zones/az-overview) konfiguriert. Eine Verfügbarkeitszone in einer Azure-Region ist eine Kombination aus einer Fehlerdomäne und einer Updatedomäne. Wenn Sie z.B. drei oder mehr virtuelle Computer über drei Zonen verteilt in einer Azure-Region erstellen, werden Ihre virtuellen Computer effektiv auf drei Fehlerdomänen und drei Updatedomänen verteilt. Die Azure-Plattform erkennt diese Updatedomänen übergreifende Verteilung, um sicherzustellen, dass virtuelle Computer in unterschiedlichen Zonen nicht gleichzeitig aktualisiert werden.

     ![Bereitstellung über Verfügbarkeitszonen](media/move-vm-overview/zone.png)

## <a name="move-vms-as-is-to-a-target-region"></a>Unverändertes Verschieben virtueller Computer in eine Zielregion

In diesem Abschnitt wird anhand der zuvor erwähnten [Architekturen](#typical-architectures-for-a-multi-tier-deployment) erläutert, wie die Bereitstellungen nach der unveränderten Verschiebung in die Zielregion jeweils aussehen.

* **Einzelinstanz-VMs mit Bereitstellung über verschiedene Ebenen**

     ![Bereitstellung von Einzelinstanz-VMs über verschiedene Ebenen](media/move-vm-overview/single-zone.png)

* **Virtuelle Computer auf den einzelnen Ebenen mit Bereitstellung über Verfügbarkeitsgruppen**

     ![Regionsübergreifende Verfügbarkeitsgruppen](media/move-vm-overview/crossregionaset.png)

* **Virtuelle Computer auf den einzelnen Ebenen mit Bereitstellung über Verfügbarkeitszonen**

     ![Bereitstellung virtueller Computer über Verfügbarkeitszonen](media/move-vm-overview/azonecross.png)

## <a name="move-vms-to-increase-availability"></a>Verschieben virtueller Computer zur Erhöhung der Verfügbarkeit

* **Einzelinstanz-VMs mit Bereitstellung über verschiedene Ebenen**

     ![Bereitstellung von Einzelinstanz-VMs über verschiedene Ebenen](media/move-vm-overview/single-zone.png)

* **Virtuelle Computer auf den einzelnen Ebenen mit Bereitstellung über Verfügbarkeitsgruppen:** Sie können Ihre virtuellen Computer in einer Verfügbarkeitsgruppe in separaten Verfügbarkeitszonen konfigurieren, wenn Sie die Replikation für die virtuellen Computer über Azure Site Recovery aktivieren. Die SLA für die Verfügbarkeit hat nach Abschluss der Verschiebung einen Wert von 99,9 %.

     ![Bereitstellung virtueller Computer über Verfügbarkeitsgruppen und Verfügbarkeitszonen](media/move-vm-overview/aset-azone.png)

## <a name="next-steps"></a>Nächste Schritte

> [!div class="nextstepaction"]
> 
> * [Verschieben virtueller Azure-Computer in eine andere Region](azure-to-azure-tutorial-migrate.md)
> 
> * [Move Azure VMs into Availability Zones](move-azure-vms-avset-azone.md) (Verschieben virtueller Azure-Computer in Verfügbarkeitszonen)

