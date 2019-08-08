---
title: VMware-Lösung von CloudSimple – Übersicht für virtuelle Azure-Computer
description: Informationen zu virtuellen CloudSimple-Computern und deren Vorteilen.
author: sharaths-cs
ms.author: dikamath
ms.date: 04/10/2019
ms.topic: article
ms.service: azure-vmware-cloudsimple
ms.reviewer: cynthn
manager: dikamath
ms.openlocfilehash: 0f4967bbd12107bf6a04cb80537d4425c75c5f46
ms.sourcegitcommit: c8a102b9f76f355556b03b62f3c79dc5e3bae305
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 08/06/2019
ms.locfileid: "68812496"
---
# <a name="cloudsimple-virtual-machines-overview"></a>Übersicht über virtuelle CloudSimple-Computer

Mit CloudSimple können Sie virtuelle VMware-Computer (VMware-VMs) über das Azure-Portal verwalten.  Ein Cluster oder ein Ressourcenpool aus Ihrem vSphere-Cluster wird über Azure verwaltet, indem er Ihrem Abonnement zugeordnet wird.  Virtuelle CloudSimple-Computer (CloudSimple-VMs) bieten Self-Service-Verwaltung von VMware-VMs über das Azure-Portal.  

Um eine CloudSimple-VM aus Azure zu erstellen, muss eine VM-Vorlage im vCenter Ihrer privaten Cloud vorhanden sein.  Die Vorlage wird verwendet, um das Betriebssystem und die Anwendungen anzupassen.  Die VM-Vorlage kann abgesichert werden, um die Sicherheitsrichtlinien für ein Unternehmen zu erfüllen.  Sie können mit der Vorlage virtuelle Computer erstellen und diese über das Azure-Portal mit einem Self-Service-Modell verwenden.

## <a name="benefits"></a>Vorteile

CloudSimple-VMs im Azure-Portal bieten einen Self-Service-Mechanismus, über den Benutzer virtuelle VMware-Computer erstellen und verwalten können.

* Erstellen einer CloudSimple-VM im vCenter Ihrer privaten Cloud
* Verwalten von VM-Eigenschaften
  * Hinzufügen/Entfernen von Datenträgern
  * Hinzufügen/Entfernen von Netzwerkadaptern (NICs)
* Ein-/Ausschaltvorgänge Ihrer CloudSimple-VM
  * Ein- und ausschalten
  * Zurücksetzen eines virtuellen Computers
* Löschen eines virtuellen Computers

## <a name="next-steps"></a>Nächste Schritte

* Erfahren Sie, wie Sie [VMware-VMs in Azure nutzen](quickstart-create-vmware-virtual-machine.md).
* Erfahren Sie, wie Sie [Ihre Azure-Abonnements zuordnen](https://docs.azure.cloudsimple.com/azure-subscription-mapping/).