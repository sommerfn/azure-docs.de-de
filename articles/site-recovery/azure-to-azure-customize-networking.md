---
title: Anpassen von Netzwerkkonfigurationen für Failover-VMs | Microsoft-Dokumentation
description: Übersicht über das Anpassen von Netzwerkkonfigurationen für Failover-VMs in der Replikation von virtuellen Azure-Computern mit Azure Site Recovery.
services: site-recovery
author: rajani-janaki-ram
manager: rochakm
ms.service: site-recovery
ms.topic: article
ms.date: 08/07/2019
ms.author: rajanaki
ms.openlocfilehash: 907a698a675a039dfdc852210adecb94c7bfab25
ms.sourcegitcommit: aa042d4341054f437f3190da7c8a718729eb675e
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 08/09/2019
ms.locfileid: "68886541"
---
# <a name="customize-networking-configurations-of-the-target-azure-vm"></a>Anpassen der Netzwerkkonfigurationen des virtuellen Azure-Zielcomputers

Dieser Artikel bietet eine Anleitung zum Anpassen der Netzwerkkonfigurationen auf Azure-Ziel-VMs für die Replikation und Wiederherstellung von Azure-VMs zwischen verschiedenen Regionen mithilfe von [Azure Site Recovery](site-recovery-overview.md).

## <a name="before-you-start"></a>Vorbereitung

Erfahren Sie, wie Site Recovery die Notfallwiederherstellung für [dieses Szenario](azure-to-azure-architecture.md) bereitstellt.

## <a name="support-networking-resources"></a>Unterstützung von Netzwerkressourcen

Für das Failover beim Replizieren von virtuellen Azure-Computern können die folgenden Konfigurationen für Schlüsselressourcen für den virtuellen Failovercomputer bereitgestellt werden.

- [Interner Load Balancer](https://docs.microsoft.com/azure/load-balancer/load-balancer-standard-overview#what-is-standard-load-balancer)
- [Öffentliche IP-Adresse](https://docs.microsoft.com/azure/virtual-network/virtual-network-ip-addresses-overview-arm#public-ip-addresses)
- [Netzwerksicherheitsgruppe](https://docs.microsoft.com/azure/virtual-network/manage-network-security-group) für das Subnetz und für die NIC

## <a name="pre-requisites"></a>Voraussetzungen

- Sie sollten die Konfigurationen für die Wiederherstellung unbedingt im Voraus planen.
- Sie müssen die Netzwerkressourcen im Voraus erstellen. Geben Sie sie als Eingabe an, damit der Azure Site Recovery-Dienst diese Einstellungen beachten und sicherstellen kann, dass die Failover-VM diese Einstellungen einhält.

## <a name="steps-to-customize-failover-networking-configurations"></a>Schritte zum Anpassen der Netzwerkkonfigurationen für Failover

1. Navigieren Sie zu **Replizierte Elemente**. 
2. Klicken Sie auf die gewünschte Azure-VM.
3. Klicken Sie auf **Compute und Netzwerk** und dann auf **Bearbeiten**. Sie werden feststellen, dass die Konfigurationseinstellungen des Netzwerkadapters die entsprechenden Ressourcen an der Quelle enthalten. 

     ![Anpassen](media/azure-to-azure-customize-networking/edit-networking-properties.png)

4. Klicken Sie neben dem Netzwerkadapter, den Sie konfigurieren möchten, auf **Bearbeiten**. Wählen Sie auf dem nächsten Blatt, das geöffnet wird, die entsprechenden vorab erstellten Ressourcen am Ziel aus.

    ![Drilldown in NIC](media/azure-to-azure-customize-networking/nic-drilldown.png) 

5. Klicken Sie auf **OK**.

Site Recovery berücksichtigt diese Einstellungen nun und stellt sicher, dass die VM bei einem Failover über den entsprechenden Netzwerkadapter mit der ausgewählten Ressource verbunden ist.

## <a name="troubleshooting"></a>Problembehandlung

### <a name="unable-to-view-or-select-a-resource"></a>Eine Ressource kann nicht angezeigt oder ausgewählt werden.

Wenn Sie eine Netzwerkressource nicht auswählen oder anzeigen können, überprüfen Sie Folgendes:

- Das Zielfeld für eine Netzwerkressource ist nur aktiviert, wenn die Quell-VM eine entsprechende Eingabe enthielt. Dies basiert auf dem Prinzip, dass Sie bei einem Notfallwiederherstellungsszenario entweder die genau gleiche oder eine zentral herunterskalierte Version Ihrer Quelle benötigen.
- Für alle entsprechenden Netzwerkressourcen werden einige Filter in der Dropdownliste angewandt, um sicherzustellen, dass die Failover-VM sich selbst an die ausgewählte Ressource anfügen kann und die Failoverzuverlässigkeit beibehalten wird. Diese Filter basieren auf denselben Netzwerkbedingungen, die bei der Konfiguration der Quell-VM überprüft wurden.

Überprüfungen des internen Lastenausgleichs:

1. Das Abonnement und die Region des Lastenausgleichsmoduls und der Ziel-VM sollten identisch sein.
2. Das virtuelle Netzwerk, das dem internen Lastenausgleichsmodul zugeordnet ist, und das der Ziel-VM sollten identisch sein.
3. Die SKU der öffentlichen IP-Adresse der Ziel-VM und die SKU des internen Lastenausgleichsmoduls sollten identisch sein.
4. Wenn die Ziel-VM für die Platzierung in einer Verfügbarkeitszone konfiguriert ist, überprüfen Sie, ob das Lastenausgleichsmodul zonenredundant oder Teil einer Verfügbarkeitszone ist. (Lastenausgleichsmodule mit der Basic-SKU unterstützen keine Zonen und werden in diesem Fall nicht in der Dropdownliste angezeigt.)
5. Stellen Sie sicher, dass der interne Lastenausgleich über einen vorab erstellten Back-End-Pool und eine Front-End-Konfiguration verfügt.


Öffentliche IP-Adresse:
    
1. Das Abonnement und die Region der öffentlichen IP-Adresse und der Ziel-VM sollten identisch sein.
2. Die SKU der öffentlichen IP-Adresse der Ziel-VM und die SKU des internen Lastenausgleichsmoduls sollten identisch sein.

Netzwerksicherheitsgruppe:
1. Das Abonnement und die Region der Netzwerksicherheitsgruppe und der Ziel-VM sollten identisch sein.


> [!WARNING]
> Wenn die Ziel-VM einer Verfügbarkeitsgruppe zugeordnet ist, müssen Sie die öffentliche IP-Adresse/den internen Lastenausgleich der gleichen SKU wie die der öffentlichen IP-Adresse/des internen Lastenausgleichs anderer virtueller Computer in der Verfügbarkeitsgruppe zuordnen. Andernfalls könnten Fehler beim Failover die Folge sein.
