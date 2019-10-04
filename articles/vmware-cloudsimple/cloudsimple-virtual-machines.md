---
title: Azure-VMware-Lösung von CloudSimple – Übersicht über virtuelle Computer
description: Informationen zu virtuellen CloudSimple-Computern und deren Vorteilen.
author: sharaths-cs
ms.author: dikamath
ms.date: 08/20/2019
ms.topic: article
ms.service: azure-vmware-cloudsimple
ms.reviewer: cynthn
manager: dikamath
ms.openlocfilehash: 213ab51dae20d281a1a0e0f8ea18f4bde888e64d
ms.sourcegitcommit: b3bad696c2b776d018d9f06b6e27bffaa3c0d9c3
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 08/21/2019
ms.locfileid: "69877900"
---
# <a name="cloudsimple-virtual-machines-overview"></a>Übersicht über virtuelle CloudSimple-Computer

CloudSimple ermöglicht Ihnen die Verwaltung von virtuellen VMware-Computern über das Azure-Portal.  Ein Cluster oder ein Ressourcenpool aus Ihrem vSphere-Cluster wird über Azure verwaltet, indem er Ihrem Abonnement zugeordnet wird.

Um eine CloudSimple-VM aus Azure zu erstellen, muss eine VM-Vorlage im vCenter Ihrer privaten Cloud vorhanden sein.  Die Vorlage wird verwendet, um das Betriebssystem und die Anwendungen anzupassen.  Die VM-Vorlage kann abgesichert werden, um die Sicherheitsrichtlinien für ein Unternehmen zu erfüllen.  Sie können mit der Vorlage virtuelle Computer erstellen und diese in einem Self-Service-Modell über das Azure-Portal verwenden.

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
* Erfahren Sie, wie Sie [Ihre Azure-Abonnements zuordnen](azure-subscription-mapping.md).
