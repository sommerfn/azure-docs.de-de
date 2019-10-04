---
title: Herstellen einer Verbindung mit Ihren virtuellen Computern über einen Browser – Azure | Microsoft-Dokumentation
description: Erfahren Sie, wie Sie über einen Browser eine Verbindung mit Ihren virtuellen Computern herstellen.
services: devtest-lab,virtual-machines,lab-services
documentationcenter: na
author: tanmayeekamath
manager: femila
editor: ''
ms.service: lab-services
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 08/19/2019
ms.author: takamath
ms.openlocfilehash: 22d7afa889994068c3f906d0b9adbc1a74bc4839
ms.sourcegitcommit: 36e9cbd767b3f12d3524fadc2b50b281458122dc
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 08/20/2019
ms.locfileid: "69642014"
---
# <a name="connect-to-your-virtual-machines-through-a-browser"></a>Herstellen einer Verbindung mit Ihren virtuellen Computern über einen Browser 

DevTest Labs und [Azure Bastion](https://docs.microsoft.com/azure/bastion/) sind integriert, sodass Sie über einen Browser eine Verbindung mit Ihren virtuellen Computern herstellen können. Informationen zum Aktivieren dieses Features in DevTest Labs finden Sie unter [Aktivieren der Browserverbindung auf Lab-VMs](enable-browser-connection-lab-virtual-machines.md).

Nach dem Aktivieren der *Browserverbindung* können Benutzer des Labs über einen Browser auf virtuelle Computer zugreifen.  

> [!NOTE]
> Das Aktivieren der Browserverbindung auf Lab-VMs ist in der Vorschauphase.

## <a name="create-a-lab-virtual-machine"></a>Erstellen einer Lab-VM

Sie müssen zuerst die Lab-VM in einem VNet erstellen, auf dem Bastion konfiguriert ist. Sie können ein VNet während der Erstellung eines virtuellen Computers auswählen, indem Sie zur Registerkarte **Erweiterte Einstellungen** navigieren.

![Erstellen eines virtuellen Computers](./media/connect-virtual-machine-through-browser/create-virtual-machine.png)

## <a name="launch-virtual-machine-in-a-browser"></a>Starten des virtuellen Computers in einem Browser

Nachdem der virtuelle Computer erstellt wurde, können Sie ihn in einem Browser starten. Klicken Sie dazu auf die Schaltfläche *Browser verbinden*, und geben Sie Ihren Benutzernamen und Ihr Kennwort für die VM ein.  

![In einem Browser starten](./media/connect-virtual-machine-through-browser/browser-connect.png)

## <a name="next-steps"></a>Nächste Schritte

[Hinzufügen eines virtuellen Computers zu einem Lab in Azure DevTest Labs](devtest-lab-add-vm.md)