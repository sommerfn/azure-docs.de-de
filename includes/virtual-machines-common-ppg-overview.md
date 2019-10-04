---
title: include file
description: include file
services: virtual-machines
author: cynthn
ms.service: virtual-machines
ms.topic: include
ms.date: 06/28/2019
ms.author: cynthn
ms.custom: include file
ms.openlocfilehash: 9832ae6010f262322328ec22876a35c89e1bedca
ms.sourcegitcommit: fa45c2bcd1b32bc8dd54a5dc8bc206d2fe23d5fb
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 07/12/2019
ms.locfileid: "67850667"
---
Wenn Sie VMs in einer einzelnen Region anordnen, verringert sich der physische Abstand zwischen den Instanzen. Wenn Sie sie in einer einzelnen Verfügbarkeitszone anordnen, verringert sich ebenfalls ihr physischer Abstand. Wenn jedoch Ihre Ressourcen in Azure zunehmen, kann eine einzige Verfügbarkeitszone auch mehrere physische Rechenzentren umspannen. Dies kann zu Netzwerklatenzen führen, die sich auf die Anwendung auswirken. 

Um den Abstand zwischen den VMs so stark wie möglich zu verringern und somit die geringstmögliche Latenz zu erzielen, sollten Sie sie in einer Näherungsplatzierungsgruppe bereitstellen.

Eine Näherungsplatzierungsgruppe ist eine logische Gruppierung, mit der ein möglichst geringer Abstand zwischen Azure-Compute-Ressourcen sichergestellt wird. Näherungsplatzierungsgruppen sind für Workloads hilfreich, die eine geringe Latenz erfordern.


- Geringe Latenz zwischen eigenständigen VMs.
- Geringe Latenz zwischen VMs in einer einzelnen Verfügbarkeitsgruppe oder VM-Skalierungsgruppe. 
- Geringe Latenz zwischen eigenständigen VMs, VMs in mehreren Verfügbarkeitsgruppen oder mehreren Skalierungsgruppen. Sie können mehrere Computeressourcen in einer einzelnen Platzierungsgruppe zusammenfassen, um eine Multi-Tier-Anwendung zusammenzuführen. 
- Geringe Latenz zwischen mehreren Logikschichten, die unterschiedliche Hardwaretypen verwenden. Beispiel: Sie führen in einer einzelnen Näherungsplatzierungsgruppe das Back-End mithilfe der M-Serie in einer Verfügbarkeitsgruppe und das Front-End in einer Instanz der D-Serie in einer Skalierungsgruppe aus.

> [!IMPORTANT]
> Näherungsplatzierungsgruppen sind zurzeit als Public Preview verfügbar.
> Diese Vorschauversion wird ohne Vereinbarung zum Servicelevel bereitgestellt und ist nicht für Produktionsworkloads vorgesehen. Manche Features werden möglicherweise nicht unterstützt oder sind nur eingeschränkt verwendbar. Weitere Informationen finden Sie unter [Zusätzliche Nutzungsbestimmungen für Microsoft Azure-Vorschauen](https://azure.microsoft.com/support/legal/preview-supplemental-terms/).
>
> Näherungsplatzierungsgruppen sind in der Vorschauversion in den folgenden Regionen nicht verfügbar: **Japan, Osten**, **Australien, Osten** und **Indien, Mitte**.


## <a name="best-practices"></a>Bewährte Methoden 
- Verwenden Sie für die niedrigste Latenz Näherungsplatzierungsgruppen zusammen mit beschleunigtem Netzwerkbetrieb. Weitere Informationen finden Sie unter [Erstellen eines virtuellen Linux-Computers mit beschleunigtem Netzwerkbetrieb](https://docs.microsoft.com/azure/virtual-network/create-vm-accelerated-networking-cli?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json) oder [Erstellen eines virtuellen Windows-Computers mit beschleunigtem Netzwerkbetrieb](/azure/virtual-network/create-vm-accelerated-networking-powershell?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json).
- Stellen Sie alle VM-Größen in einer einzelnen Resource Manager-Vorlage bereit. Um die Verwendung von Hardware zu vermeiden, die nicht alle erforderlichen VM-SKUs und -Größen unterstützt, schließen Sie alle Logikschichten in eine einzelne Vorlage ein, damit diese alle gleichzeitig bereitgestellt werden.
- Wenn Sie mit PowerShell, der CLI oder dem SDK ein Bereitstellungsskript erstellen, erhalten Sie möglicherweise den Zuordnungsfehler `OverconstrainedAllocationRequest`. In diesem Fall sollten Sie alle vorhandenen VMs beenden bzw. ihre Zuordnung aufheben und die Sequenz im Bereitstellungsskript so ändern, dass es mit den fehlgeschlagenen VM-SKUs/-Größen beginnt. 
- Wenn Sie eine vorhandene Platzierungsgruppe wiederverwenden, aus der VMs gelöscht wurden, warten Sie, bis der Löschvorgang vollständig durchgeführt wurde, bevor Sie der Gruppe VMs hinzufügen.
- Wenn Latenz oberste Priorität hat, fügen Sie die VMs in eine Näherungsplatzierungsgruppe und die gesamte Lösung in eine Verfügbarkeitszone ein. Wenn jedoch Resilienz oberste Priorität hat, verteilen Sie die Instanzen auf mehrere Verfügbarkeitszonen (eine einzelne Näherungsplatzierungsgruppe kann nicht mehrere Zonen umfassen).