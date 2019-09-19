---
title: Updates und Komponentenupgrades in Azure Site Recovery
description: Bietet eine Übersicht über Azure Site Recovery-Dienstupdates und Komponentenupgrades.
author: rajani-janaki-ram
manager: rochakm
ms.service: site-recovery
ms.topic: conceptual
ms.date: 07/31/2019
ms.author: rajanaki
ms.openlocfilehash: 3489f7f812798504d0c57a265a04e57344105419
ms.sourcegitcommit: d70c74e11fa95f70077620b4613bb35d9bf78484
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 09/11/2019
ms.locfileid: "70910472"
---
# <a name="service-updates-in-site-recovery"></a>Dienstupdates in Azure Site Recovery

Dieser Artikel bietet eine Übersicht über [Azure Site Recovery](site-recovery-overview.md)-Updates und beschreibt, wie Sie ein Upgrade von Site Recovery-Komponenten durchführen.

Site Recovery veröffentlicht in regelmäßigen Abständen Dienstupdates. Updates umfassen neue Features, Supportverbesserungen, Komponentenupdates und Fehlerbehebungen. Um die neuesten Features und Korrekturen nutzen zu können, empfiehlt es sich, die aktuellsten Versionen der Site Recovery Komponenten auszuführen. 
 
 
## <a name="updates-support"></a>Support für Updates

### <a name="support-statement-for-azure-site-recovery"></a>Supporthinweis für Azure Site Recovery

Es wird empfohlen, immer auf die neuesten Komponentenversionen zu aktualisieren:

**Mit jeder neuen veröffentlichten Version N einer Azure Site Recovery-Komponente werden alle Versionen, die älter als N-4 sind, als „nicht mehr unterstützt“ betrachtet.** 

> [!IMPORTANT]
> Offizieller Support wird für das Upgrade von Version >N-4 auf Version N bereitgestellt. Wenn Sie z. B. N-6 verwenden, müssen Sie zunächst ein Upgrade auf N-4 ausführen und dann ein Upgrade auf N.


### <a name="links-to-currently-supported-update-rollups"></a>Links zu den derzeit unterstützten Updaterollups

 Schauen Sie sich das neueste Updaterollup (Version N) in [diesem Artikel](site-recovery-whats-new.md) an. Beachten Sie, dass Site Recovery nur für N-4-Versionen Support bereitstellt.



## <a name="component-expiry"></a>Ablauf der Komponente

Sie werden von Site Recovery über abgelaufene Komponenten (oder Komponenten, die demnächst ablaufen) per E-Mail benachrichtigt, (sofern Sie E-Mail-Benachrichtigungen abonniert haben). Alternativ sind diese Informationen auch im Tresordashboard im Portal verfügbar.

- Außerdem wird in der Infrastrukturansicht für Ihr Szenario im Portal eine **Update verfügbar**-Schaltfläche neben der Komponente angezeigt, wenn Updates verfügbar sind. Mit dieser Schaltfläche werden Sie zu einem Link zum Herunterladen der neuesten Komponentenversion weitergeleitet.
-  Wenn Sie Hyper-V-VMs replizieren, sind keine Dashboardbenachrichtigungen verfügbar. 

E-Mail-Benachrichtigungen werden wie folgt gesendet.

**Time** | **Frequency**
--- | ---
60 Tage vor Ablauf der Komponente | Einmal alle zwei Wochen
Nächste 53 Tage | Einmal pro Woche
Letzte 7 Tage | einmal täglich
Nach Ablauf | Einmal alle zwei Wochen


### <a name="upgrading-outside-official-support"></a>Aktualisierung außerhalb des offiziellen Supports

Wenn der Unterschied zwischen Ihrer Komponentenversion und der neuesten Releaseversion größer als vier ist, wird sie als „Nicht mehr unterstützt“ betrachtet. Aktualisieren Sie in diesem Fall wie folgt: 

1. Aktualisieren Sie die aktuell installierte Komponente auf die aktuell installierte Version plus vier. Wenn Ihre Version beispielsweise 9.16 ist, führen Sie ein Upgrade auf 9.20 aus.
2. Aktualisieren Sie dann auf die nächste kompatible Version. Führen Sie in unserem Beispiel also nach dem Upgrade von 9.16 auf 9.20 ein Upgrade auf 9.24 durch. 

Führen Sie den gleichen Prozess für alle relevanten Komponenten aus.

### <a name="support-for-latest-operating-systemskernels"></a>Unterstützung für die neuesten Betriebssysteme/Kernels

> [!NOTE]
> Wenn Sie ein Wartungsfenster geplant haben und dabei ein Neustart durchgeführt wird, empfiehlt es sich, zunächst ein Upgrade der Site Recovery-Komponenten durchzuführen und dann mit dem Rest der geplanten Aktivitäten im Wartungsfenster fortzufahren.

1. Bevor Sie Ihre Betriebssystem-/Kernelversionen aktualisieren, überprüfen Sie, ob die Zielversion von Site Recovery unterstützt wird. 

    - Support für [Azure-VM](azure-to-azure-support-matrix.md#replicated-machine-operating-systems).
    - Support von [VMware/physischem Server](vmware-physical-azure-support-matrix.md#replicated-machines).
    - Support von [Hyper-V](hyper-v-azure-support-matrix.md#replicated-vms).
2. Überprüfen Sie die [verfügbaren Updates](site-recovery-whats-new.md), um zu entscheiden, was Sie aktualisieren möchten.
3. Aktualisieren Sie auf die neueste Version von Site Recovery.
4. Führen Sie ein Upgrade des Betriebssystems/Kernels auf die erforderliche Version aus.
5. Führen Sie einen Neustart aus.


Durch diesen Vorgang wird sichergestellt, dass das Betriebssystem bzw. der Kernel des Computers auf die neueste Version aktualisiert wird und die neuesten Site Recovery-Änderungen, die zur Unterstützung der neuen Version erforderlich sind, auf dem Computer geladen werden.

## <a name="azure-vm-disaster-recovery-to-azure"></a>Notfallwiederherstellung von virtuellen Azure-Computern in Azure

In diesem Szenario wird dringend empfohlen, automatische Updates zu [aktivieren](azure-to-azure-autoupdate.md). Sie können das Verwalten von Updates durch Site Recovery folgendermaßen zulassen:

- Während des Prozesses zur Aktivierung der Replikation.
- Durch Festlegen der Erweiterungsupdateeinstellungen im Tresor.

Wenn Sie Updates manuell verwalten möchten, gehen Sie folgendermaßen vor:

1. Klicken Sie im Tresor unter **Replizierte Elemente** auf diese Benachrichtigung am oberen Rand des Bildschirms: 
    
    **Ein neues Site Recovery-Replikations-Agent-Update ist verfügbar. Klicken Sie, um es zu installieren ->**

4. Wählen Sie die VMs aus, auf die Sie das Update anwenden möchten, und klicken Sie dann auf **OK**.


## <a name="vmware-vmphysical-server-disaster-recovery-to-azure"></a>Notfallwiederherstellung für VMware-VM/physische Server auf Azure

1. Installieren Sie basierend auf Ihrer aktuellen Version und dem [Supporthinweis](#support-statement-for-azure-site-recovery) das Update zuerst auf dem lokalen Konfigurationsserver, indem Sie [diese Anweisungen](vmware-azure-deploy-configuration-server.md#upgrade-the-configuration-server) befolgen. 
2. Wenn Sie über Prozessserver für die horizontale Skalierung verfügen, aktualisieren Sie diese als Nächstes anhand [dieser Anweisungen](vmware-azure-manage-process-server.md#upgrade-a-process-server).
3. Informationen zum Aktualisieren des Mobilitäts-Agent auf jedem geschützten Computer finden Sie in [diesem](vmware-physical-manage-mobility-service.md#update-mobility-service-from-azure-portal) Artikel.

### <a name="reboot-after-mobility-service-upgrade"></a>Neustart nach dem Upgrade des Mobilitätsdiensts

Es wird empfohlen, nach jedem Mobilitätsdienstupgrade einen Neustart auszuführen, um sicherzustellen, dass alle aktuellen Änderungen auf dem Quellcomputer geladen werden.

Ein Neustart ist nicht zwingend erforderlich, es sei denn, der Abstand zwischen der Agent-Version während des letzten Neustarts und der aktuellen Version ist größer als vier.

Das Beispiel in der Tabelle verdeutlicht dies.

|**Agent-Version (letzter Neustart)** | **Upgrade auf** | **Neustart obligatorisch?**|
|---------|---------|---------|
|9.16 |  9.18 | Nicht obligatorisch|
|9.16 | 9.19 | Nicht obligatorisch|
| 9.16 | 9.20 | Nicht obligatorisch
 | 9.16 | 9.21 | Obligatorisch.<br/><br/> Aktualisieren Sie auf 9.20, und führen Sie dann vor dem Upgrade auf 9.21 einen Neustart aus.

## <a name="hyper-v-vm-disaster-recovery-to-azure"></a>Notfallwiederherstellung von virtuellen Hyper-V-Computern in Azure

### <a name="between-a-hyper-v-site-and-azure"></a>Zwischen einem Hyper-V-Standort und Azure

1. Laden Sie das Update für den Microsoft Azure Site Recovery-Anbieter herunter.
2. Installieren Sie den Anbieter auf allen Hyper-V-Servern, die in Site Recovery registriert sind. Wenn Sie einen Cluster verwenden, führen Sie ein Upgrade auf allen Cluster Knoten aus.


## <a name="between-an-on-premises-vmm-site-and-azure"></a>Zwischen einem lokalen VMM-Standort und Azure
1. Laden Sie das Update für den Microsoft Azure Site Recovery-Anbieter herunter.
2. Installieren Sie den Anbieter auf dem VMM-Server. Wenn VMM in einem Cluster bereitgestellt wird, installieren Sie den Anbieter auf allen Clusterknoten.
3. Installieren Sie den aktuellen Microsoft Azure Recovery Services-Agent auf allen Hyper-V-Hosts oder Clusterknoten.


## <a name="between-two-on-premises-vmm-sites"></a>Zwischen zwei lokalen VMM-Standorten
1. Laden Sie das neueste Update für den Microsoft Azure Site Recovery-Anbieter herunter.
2. Installieren Sie den neuesten Anbieter auf dem VMM-Server, der den sekundären Wiederherstellungsstandort verwaltet. Wenn VMM in einem Cluster bereitgestellt wird, installieren Sie den Anbieter auf allen Clusterknoten.
3. Nach dem Update des Wiederherstellungsstandorts installieren Sie den Anbieter auf dem VMM-Server, der den primären Standort verwaltet.

## <a name="next-steps"></a>Nächste Schritte

Folgen Sie unterer Seite mit den [Azure-Updates](https://azure.microsoft.com/updates/?product=site-recovery), um über neue Updates und Releases auf dem Laufenden zu bleiben.
