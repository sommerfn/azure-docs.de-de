---
title: Durchführen eines Notfallwiederherstellungsverfahrens für virtuelle Azure-Computer mit Azure Site Recovery
description: Hier erfahren Sie, wie Sie für virtuelle Azure-Computer unter Verwendung von Azure Site Recovery ein Notfallwiederherstellungsverfahren in einer sekundären Region durchführen.
services: site-recovery
author: rayne-wiselman
manager: carmonm
ms.service: site-recovery
ms.topic: tutorial
ms.date: 11/14/2019
ms.author: raynew
ms.custom: mvc
ms.openlocfilehash: 817a220e36ac250b1d5a5aa90d0bddbfb155cc26
ms.sourcegitcommit: a170b69b592e6e7e5cc816dabc0246f97897cb0c
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 11/14/2019
ms.locfileid: "74091338"
---
# <a name="run-a-disaster-recovery-drill-to-a-secondary-region-for-azure-vms"></a>Durchführen eines Notfallwiederherstellungsverfahrens in einer sekundären Azure-Region für virtuelle Azure-Computer 

Der Dienst [Azure Site Recovery](site-recovery-overview.md) unterstützt Ihre Strategien für Geschäftskontinuität und Notfallwiederherstellung, indem die Verfügbarkeit Ihrer Geschäftsanwendungen bei geplanten und ungeplanten Ausfällen gewährleistet wird. Site Recovery verwaltet und koordiniert die Notfallwiederherstellung von lokalen Computern sowie virtuellen Azure-Computern (VMs), einschließlich Replikation, Failover und Wiederherstellung.

In diesem Tutorial wird erläutert, wie ein Notfallwiederherstellungsverfahren für eine Azure-VM aus einer Azure-Region in eine andere mit einem Testfailover durchgeführt wird. Bei dem Verfahren wird Ihre Replikationsstrategie ohne Datenverlust oder Ausfallzeiten überprüft. Dies hat keinen Einfluss auf Ihre Produktionsumgebung. In diesem Tutorial lernen Sie Folgendes:

> [!div class="checklist"]
> * Überprüfen der Voraussetzungen
> * Ausführen eines Testfailovers für eine einzelne VM

> [!NOTE]
> In diesem Tutorial erfahren Sie, wie Sie mit wenigen Schritten ein Notfallwiederherstellungsverfahren durchführen. Weitere Informationen zu den verschiedenen Aspekten im Zusammenhang mit der Durchführung eines Notfallwiederherstellungsverfahrens (beispielsweise zu Netzwerkaspekten, zur Automatisierung oder zur Problembehandlung) finden Sie in den Anleitungen für virtuelle Azure-Computer.

## <a name="prerequisites"></a>Voraussetzungen

- Bevor Sie ein Testfailover ausführen, empfehlen wir Ihnen, die VM-Eigenschaften zu überprüfen, um sicherzustellen, dass sie den Erwartungen entsprechen.  Die VM-Einstellungen finden Sie unter **Replizierte Elemente**. Das Blatt **Zusammenfassung** enthält Informationen zu den Einstellungen und zum Status der Computer.
- **Es wird empfohlen, ein separates Azure-VM-Netzwerk für das Testfailover zu nutzen**, und nicht das Standardnetzwerk, das bei der Aktivierung der Replikation eingerichtet wurde.
- Abhängig von Ihren Quellnetzwerkkonfigurationen für die einzelnen NICs können Sie vor der Durchführung des Notfallwiederherstellungsverfahrens in den Testfailovereinstellungen unter „Compute und Netzwerk“ optional **das Subnetz, die IP-Adresse, die öffentliche IP-Adresse, die Netzwerksicherheitsgruppe oder den internen Lastenausgleich** für die Anfügung an die jeweilige NIC angeben.


## <a name="run-a-test-failover"></a>Ausführen eines Testfailovers

1. Klicken Sie unter **Einstellungen** > **Replizierte Elemente** auf das VM-Symbol **+Testfailover**.

2. Wählen Sie unter **Testfailover** einen Wiederherstellungspunkt für das Failover aus:

    - **Aktuell**: Bei dieser Option werden alle Daten in Site Recovery verarbeitet. Sie bietet außerdem den niedrigsten RPO-Wert (Recovery Point Objective).
    - **Letzte Verarbeitung**: Führt ein Failover des virtuellen Computers auf den letzten Wiederherstellungspunkt aus, der von Site Recovery verarbeitet wurde. Der Zeitstempel wird angezeigt. Mit dieser Option wird keine Zeit für die Verarbeitung von Daten verwendet und die Recovery Time Objective (RTO) niedrig gehalten.
   - **Letzter anwendungskonsistenter Zeitpunkt**: Diese Option führt ein Failover aller virtuellen Computer auf den letzten App-konsistenten Wiederherstellungspunkt aus. Der Zeitstempel wird angezeigt.
   - **Benutzerdefiniert**: Dient zum Ausführen eines Failovers auf einen bestimmten Wiederherstellungspunkt. Die Option „Benutzerdefiniert“ ist nur verfügbar, wenn Sie ein Failover für einen einzelnen virtuellen Computer ausführen. Für Failover mit Wiederherstellungsplan steht die Option dagegen nicht zur Verfügung.

3. Wählen Sie das virtuelle Azure-Zielnetzwerk aus, mit dem Azure-VMs in der sekundären Region nach dem Failover verbunden werden.

    > [!NOTE]
    > Die Dropdownliste zum Auswählen des virtuellen Azure-Netzwerks wird nicht angezeigt, wenn die Testfailovereinstellungen für das replizierte Elemente vorkonfiguriert sind.

4. Um das Failover zu starten, klicken Sie auf **OK**. Klicken Sie zum Überwachen des Fortschritts auf die VM, um die Eigenschaften aufzurufen. Alternativ können Sie auf den Auftrag **Testfailover** unter dem Tresornamen und dann auf **Einstellungen** > **Aufträge** > **Site Recovery-Aufträge** klicken.
5. Nach Abschluss des Failovers wird der virtuelle Azure-Replikatcomputer im Azure-Portal unter **Virtuelle Computer** angezeigt. Stellen Sie sicher, dass die VM ausgeführt wird, die passende Größe hat und mit dem entsprechenden Netzwerk verbunden ist.
6. Um die VMs zu löschen, die beim Testfailover erstellt wurden, klicken Sie im replizierten Element oder im Wiederherstellungsplan auf **Testfailover bereinigen**. Erfassen und speichern Sie unter **Notizen** alle Beobachtungen im Zusammenhang mit dem Testfailover.

## <a name="next-steps"></a>Nächste Schritte

> [!div class="nextstepaction"]
> [Ausführen eines Produktionsfailovers](azure-to-azure-tutorial-failover-failback.md)
