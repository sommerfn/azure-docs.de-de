---
title: Erstellen einer Näherungsplatzierungsgruppe über das Portal
description: Erfahren Sie, wie Sie eine Näherungsplatzierungsgruppe über das Azure-Portal erstellen.
services: virtual-machines
author: cynthn
manager: gwallace
ms.service: virtual-machines
ms.topic: article
ms.tgt_pltfrm: vm-linux
ms.workload: infrastructure-services
ms.date: 10/30/2019
ms.author: cynthn
ms.openlocfilehash: 8512d9b701242dc686d49bbe56e8a2059f14ee3d
ms.sourcegitcommit: 98ce5583e376943aaa9773bf8efe0b324a55e58c
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 10/30/2019
ms.locfileid: "73180022"
---
# <a name="create-a-proximity-placement-group-using-the-portal"></a>Erstellen einer Näherungsplatzierungsgruppe über das Portal

Um den Abstand zwischen den VMs so stark wie möglich zu verringern und somit die geringstmögliche Latenz zu erzielen, sollten Sie sie in einer [Näherungsplatzierungsgruppe](co-location.md#proximity-placement-groups) bereitstellen.

Eine Näherungsplatzierungsgruppe ist eine logische Gruppierung, mit der ein möglichst geringer Abstand zwischen Azure-Computeressourcen sichergestellt wird. Näherungsplatzierungsgruppen sind für Workloads hilfreich, die eine geringe Latenz erfordern.


## <a name="create-the-proximity-placement-group"></a>Erstellen der Näherungsplatzierungsgruppe

1. Geben Sie **Näherungsplatzierungsgruppe** in die Suche ein.
1. Wählen Sie unter **Dienste** in den Suchergebnissen **Näherungsplatzierungsgruppen** aus.
1. Wählen Sie auf der Seite **Näherungsplatzierungsgruppen** die Option **Hinzufügen** aus.
1. Stellen Sie sicher, dass auf der Registerkarte **Grundlagen** unter **Projektdetails** das richtige Abonnement ausgewählt ist.
1. Wählen Sie in **Ressourcengruppe** entweder die Option **Neu erstellen** aus, um eine neue Ressourcengruppe zu erstellen, oder wählen Sie eine vorhandene Ressourcengruppe aus der Dropdownliste aus.
1. Wählen Sie unter **Region** den Standort aus, an dem die Näherungsplatzierungsgruppe erstellt werden soll.
1. Geben Sie in **Name der Näherungsplatzierungsgruppe** einen Namen ein, und wählen Sie dann **Überprüfen und erstellen** aus.
1. Wählen Sie nach bestandenen Überprüfung die Option **Erstellen** aus, um die Näherungsplatzierungsgruppe zu erstellen.

    ![Screenshot: Erstellen einer Näherungsplatzierungsgruppe](./media/ppg/ppg.png)


## <a name="create-a-vm"></a>Erstellen einer VM

1. Während Sie einen virtuellen Computer im Portal erstellen, wechseln Sie zur Registerkarte **Erweitert**. 
1. Wählen Sie in der Auswahl **Näherungsplatzierungsgruppe** die richtige Platzierungsgruppe aus. 

    ![Screenshot des Abschnitts „Näherungsplatzierungsgruppe“ beim Erstellen eines neuen virtuellen Computers im Portal](./media/ppg/vm-ppg.png)

1. Wenn Sie alle anderen erforderlichen Optionen vorgenommen haben, wählen Sie **Überprüfen und erstellen** aus.
1. Wählen Sie nach der erfolgreichen Überprüfung **Erstellen** aus, um den virtuellen Computer in der Platzierungsgruppe bereitzustellen.




## <a name="next-steps"></a>Nächste Schritte

Sie können auch die [Azure PowerShell](proximity-placement-groups.md) zum Erstellen von Näherungsplatzierungsgruppen verwenden.

