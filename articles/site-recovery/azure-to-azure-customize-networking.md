---
title: Anpassen von Netzwerkkonfigurationen für eine Failover-VM | Microsoft-Dokumentation
description: Hier finden Sie eine Übersicht über das Anpassen von Netzwerkkonfigurationen für eine Failover-VM in der Replikation von virtuellen Azure-Computern mit Azure Site Recovery.
services: site-recovery
author: rajani-janaki-ram
manager: rochakm
ms.service: site-recovery
ms.topic: article
ms.date: 08/07/2019
ms.author: rajanaki
ms.openlocfilehash: 8038f7c909cfeaf15039afa7335dd6b0460a2622
ms.sourcegitcommit: 8b44498b922f7d7d34e4de7189b3ad5a9ba1488b
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 10/13/2019
ms.locfileid: "72293461"
---
# <a name="customize-networking-configurations-of-the-target-azure-vm"></a>Anpassen der Netzwerkkonfigurationen des virtuellen Azure-Zielcomputers

Dieser Artikel bietet eine Anleitung zum Anpassen der Netzwerkkonfigurationen auf Azure-Ziel-VMs für die Replikation und Wiederherstellung von Azure-VMs zwischen verschiedenen Regionen mithilfe von [Azure Site Recovery](site-recovery-overview.md).

## <a name="before-you-start"></a>Vorbereitung

Erfahren Sie, wie Site Recovery die Notfallwiederherstellung für [dieses Szenario](azure-to-azure-architecture.md) bereitstellt.

## <a name="supported-networking-resources"></a>Unterstützte Netzwerkressourcen

Beim Replizieren von virtuellen Azure-Computern können die folgenden Konfigurationen für Schlüsselressourcen für den virtuellen Failovercomputer bereitgestellt werden:

- [Interner Lastenausgleich](https://docs.microsoft.com/azure/load-balancer/load-balancer-standard-overview#what-is-standard-load-balancer)
- [Öffentliche IP-Adresse](https://docs.microsoft.com/azure/virtual-network/virtual-network-ip-addresses-overview-arm#public-ip-addresses)
- [Netzwerksicherheitsgruppe](https://docs.microsoft.com/azure/virtual-network/manage-network-security-group) für das Subnetz und für den Netzwerkadapter

 > [!IMPORTANT]
  > Diese Einstellungen werden derzeit nur beim Failovervorgang und nicht für das Testfailover unterstützt.

## <a name="prerequisites"></a>Voraussetzungen

- Sie sollten die Konfigurationen für die Wiederherstellung unbedingt im Voraus planen.
- Erstellen Sie die Netzwerkressourcen im Voraus. Geben Sie sie als Eingabe an, damit der Azure Site Recovery-Dienst diese Einstellungen beachten und sicherstellen kann, dass die Failover-VM diese Einstellungen einhält.

## <a name="customize-failover-networking-configurations"></a>Anpassen der Netzwerkkonfigurationen für Failover

1. Wechseln Sie zu **Replizierte Elemente**. 
2. Wählen Sie die gewünschte Azure-VM aus.
3. Wählen Sie **Compute und Netzwerk** und **Bearbeiten** aus. Sie werden feststellen, dass die Konfigurationseinstellungen des Netzwerkadapters die entsprechenden Ressourcen an der Quelle enthalten. 

     ![Anpassen der Netzwerkkonfigurationen für das Failover](media/azure-to-azure-customize-networking/edit-networking-properties.png)

4. Wählen Sie neben dem Netzwerkadapter, den Sie konfigurieren möchten, die Option **Bearbeiten** aus. Wählen Sie auf dem nächsten Blatt, das geöffnet wird, die entsprechenden vorab erstellten Ressourcen am Ziel aus.

    ![Bearbeiten der Konfiguration des Netzwerkadapters](media/azure-to-azure-customize-networking/nic-drilldown.png) 

5. Klicken Sie auf **OK**.

Site Recovery berücksichtigt diese Einstellungen nun und stellt sicher, dass die VM bei einem Failover über den entsprechenden Netzwerkadapter mit der ausgewählten Ressource verbunden ist.

## <a name="troubleshooting"></a>Problembehandlung

### <a name="unable-to-view-or-select-a-resource"></a>Eine Ressource kann nicht angezeigt oder ausgewählt werden.

Wenn Sie eine Netzwerkressource nicht auswählen oder anzeigen können, überprüfen Sie Folgendes:

- Das Zielfeld für eine Netzwerkressource ist nur aktiviert, wenn die Quell-VM eine entsprechende Eingabe enthielt. Dies basiert auf dem Prinzip, dass Sie bei einem Notfallwiederherstellungsszenario entweder die genau gleiche oder eine zentral herunterskalierte Version Ihrer Quelle benötigen.
- Für alle Netzwerkressourcen werden einige Filter in der Dropdownliste angewendet, um sicherzustellen, dass die Failover-VM sich selbst an die ausgewählte Ressource anfügen kann und die Failoverzuverlässigkeit beibehalten wird. Diese Filter basieren auf denselben Netzwerkbedingungen, die bei der Konfiguration der Quell-VM überprüft wurden.

Überprüfungen des internen Lastenausgleichs:

- Das Abonnement und die Region des Lastenausgleichs und der Ziel-VM sollten identisch sein.
- Das virtuelle Netzwerk, das dem internen Lastenausgleich zugeordnet ist, und das der Ziel-VM sollten identisch sein.
- Die SKU der öffentlichen IP-Adresse der Ziel-VM und die SKU des internen Lastenausgleichs sollten identisch sein.
- Wenn die Ziel-VM für die Platzierung in einer Verfügbarkeitszone konfiguriert ist, überprüfen Sie, ob das Lastenausgleichsmodul zonenredundant oder Teil einer Verfügbarkeitszone ist. (Lastenausgleichsmodule mit der Basic-SKU unterstützen keine Zonen und werden in diesem Fall nicht in der Dropdownliste angezeigt.)
- Stellen Sie sicher, dass der interne Lastenausgleich über einen vorab erstellten Back-End-Pool und eine Front-End-Konfiguration verfügt.


Öffentliche IP-Adresse:
    
- Das Abonnement und die Region der öffentlichen IP-Adresse und der Ziel-VM sollten identisch sein.
- Die SKU der öffentlichen IP-Adresse der Ziel-VM und die SKU des internen Lastenausgleichs sollten identisch sein.

Netzwerksicherheitsgruppe:
- Das Abonnement und die Region der Netzwerksicherheitsgruppe und der Ziel-VM sollten identisch sein.


> [!WARNING]
> Wenn die Ziel-VM einer Verfügbarkeitsgruppe zugeordnet ist, müssen Sie die öffentliche IP-Adresse und den internen Lastenausgleich der gleichen SKU wie die der öffentlichen IP-Adresse und des internen Lastenausgleichs des anderen virtuellen Computers in der Verfügbarkeitsgruppe zuordnen. Andernfalls ist das Failover möglicherweise nicht erfolgreich.
