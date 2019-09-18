---
title: Ausführen eines Failbacks von in einer sekundären Azure-Region replizierten virtuellen Azure-Computern zur Notfallwiederherstellung mit dem Azure Site Recovery-Dienst
description: Es wird beschrieben, wie Sie mit dem Azure Site Recovery-Dienst ein Failback für Azure-VMs durchführen.
author: rayne-wiselman
manager: carmonm
ms.service: site-recovery
ms.topic: tutorial
ms.date: 09/09/2019
ms.author: raynew
ms.custom: mvc
ms.openlocfilehash: c8be547790452774992b9226ca8010532263aaff
ms.sourcegitcommit: fa4852cca8644b14ce935674861363613cf4bfdf
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 09/09/2019
ms.locfileid: "70814520"
---
# <a name="fail-back-an-azure-vm-between-azure-regions"></a>Failback für Azure-VMs zwischen Azure-Regionen

Der Dienst [Azure Site Recovery](site-recovery-overview.md) verwaltet und orchestriert die Replikation, das Failover und das Failback von lokalen Computern sowie Azure-VMs und unterstützt dadurch Ihre Strategie zur Notfallwiederherstellung.

In diesem Tutorial wird beschrieben, wie Sie das Failback für eine einzelne Azure-VM durchführen. Nach dem Failover müssen Sie ein Failback auf die primäre Region ausführen, wenn diese verfügbar ist. In diesem Tutorial lernen Sie Folgendes:

> [!div class="checklist"]
> 
> * Ausführen eines Failbacks für den virtuellen Computer in der sekundären Region
> * Erneutes Schützen der primären VM zurück zur sekundären Region
> 
> [!NOTE]
> 
> In diesem Tutorial erfahren Sie, wie Sie mit minimalem Anpassungsaufwand für einige virtuelle Computer ein Failover auf eine Zielregion sowie ein Failback auf die Quellregion ausführen. Ausführlichere Anweisungen finden Sie in den [Anleitungsartikeln für virtuelle Azure-Computer](https://docs.microsoft.com/azure/virtual-machines/windows/).

## <a name="before-you-start"></a>Vorbereitung

* Vergewissern Sie sich, dass der Status des virtuellen Computers **Commit für Failover ausgeführt** lautet.
* Vergewissern Sie sich außerdem, dass die primäre Region verfügbar ist und dass Sie darin neue Ressourcen erstellen und auf diese zugreifen können.
* Stellen Sie sicher, dass die Option für das erneute Schützen aktiviert ist.

## <a name="fail-back-to-the-primary-region"></a>Ausführen eines Failbacks auf die primäre Region

Nachdem virtuelle Computer erneut geschützt wurden, können Sie bei Bedarf ein Failback zur primären Region ausführen.

1. Wählen Sie im Tresor **Replizierte Elemente** und dann den virtuellen Computer aus, der erneut geschützt wurde.

    ![Ausführen eines Failbacks auf die primäre Region](./media/site-recovery-azure-to-azure-failback/azure-to-azure-failback.png)

2. Wählen Sie unter **Replizierte Elemente** den virtuellen Computer und dann **Failover** aus.
3. Wählen Sie unter **Failover** einen Wiederherstellungspunkt für das Failover aus:
    - **Neuester (Standardeinstellung)** : Bei dieser Option werden alle Daten in Site Recovery verarbeitet. Sie bietet außerdem den niedrigsten RPO-Wert (Recovery Point Objective).
    - **Letzte Verarbeitung**: Bei dieser Option wird für den virtuellen Computer der Zustand des letzten Wiederherstellungspunkts wiederhergestellt, der von Site Recovery verarbeitet wurde.
    - **Benutzerdefiniert**: Bei dieser Option wird ein Failover auf einen bestimmten Wiederherstellungspunkt ausgeführt. Diese Option eignet sich für ein Testfailover.
4. Wählen Sie **Der Computer wird vor Beginn des Failovers heruntergefahren** aus, wenn Site Recovery vor dem Auslösen des Failovers versuchen soll, VMs in der Notfallwiederherstellungsregion herunterzufahren. Das Failover wird auch dann fortgesetzt, wenn das Herunterfahren nicht erfolgreich ist. 
5. Der Fortschritt des Failovers wird auf der Seite **Aufträge** angezeigt.
6. Überprüfen Sie den virtuellen Computer nach Abschluss des Failovers, indem Sie sich bei ihm anmelden. Der Wiederherstellungspunkt kann bei Bedarf geändert werden.
7. Wählen Sie nach der Überprüfung des Failovers **Commit** aus. Dadurch werden alle verfügbaren Wiederherstellungspunkte gelöscht. Die Option „Wiederherstellungspunkt ändern“ ist nicht mehr verfügbar.
8. Für den virtuellen Computer wird angezeigt, dass ein Failover und ein Failback ausgeführt wurden.

    ![VM in primärer und sekundärer Region](./media/site-recovery-azure-to-azure-failback/azure-to-azure-failback-vm-view.png)

## <a name="next-steps"></a>Nächste Schritte

Erfahren Sie mehr über den [erneuten Schutz](azure-to-azure-how-to-reprotect.md#what-happens-during-reprotection).
