---
title: Herstellen einer Verbindung mit einer Windows-VM mit Azure Bastion | Microsoft-Dokumentation
description: In diesem Artikel erfahren Sie, wie Sie eine Verbindung mit einem virtuellen Azure-Computer mit Azure Bastion herstellen.
services: bastion
author: cherylmc
ms.service: bastion
ms.topic: conceptual
ms.date: 06/03/2019
ms.author: cherylmc
ms.openlocfilehash: 376b7042a513dd50647dc8f88bf1de70f65bb21c
ms.sourcegitcommit: f811238c0d732deb1f0892fe7a20a26c993bc4fc
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 06/29/2019
ms.locfileid: "67478400"
---
# <a name="connect-to-a-windows-virtual-machine-using-azure-bastion-preview"></a>Herstellen einer Verbindung mit einem virtuellen Windows-Computer mit Azure Bastion (Vorschauversion)

In diesem Artikel erfahren Sie, wie Sie mit Azure Bastion sicher und problemlos RDP auf Ihren virtuellen Windows-Computern in einem virtuellen Azure-Netzwerk durchführen können. Sie können sich direkt vom Azure-Portal aus mit einer VM verbinden. Wenn Sie Azure Bastion verwenden, benötigen VMs keinen Client, keinen Agent und auch keine zusätzliche Software. Weitere Informationen über Azure Bastion finden Sie in der [Übersicht](bastion-overview.md).

> [!IMPORTANT]
> Diese öffentliche Vorschauversion wird ohne Servicelevelvereinbarung bereitgestellt und sollte nicht für Produktionsworkloads verwendet werden. Unter Umständen werden bestimmte Features nicht unterstützt, verfügen über eingeschränkte Funktionen und sind nicht an allen Azure-Standorten verfügbar. Weitere Informationen finden Sie unter [Ergänzende Nutzungsbedingungen für Microsoft Azure-Vorschauversionen](https://azure.microsoft.com/support/legal/preview-supplemental-terms/).
>

## <a name="before-you-begin"></a>Voraussetzungen

Stellen Sie sicher, dass Sie einen Azure Bastion-Host für das virtuelle Netzwerk eingerichtet haben, in dem sich die VM befindet. Weitere Informationen finden Sie unter [Erstellen eines Azure Bastion-Hosts](bastion-create-host-portal.md). Sobald der Bastion-Dienst provisioniert und in Ihrem virtuellen Netzwerk bereitgestellt wurde, können Sie ihn verwenden, um sich mit jeder VM in diesem virtuellen Netzwerk zu verbinden. In dieser Vorschau geht Bastion davon aus, dass Sie RDP für die Verbindung mit einer Windows-VM und SSH für die Verbindung mit Ihren Linux-VMs verwenden. Informationen zur Verbindung mit einer Linux-VM finden Sie unter [Herstellen einer Verbindung mit einer VM – Linux](bastion-connect-vm-ssh.md).

Zum Herstellen einer Verbindung sind die folgenden Rollen erforderlich:

* Rolle „Leser“ auf dem virtuellen Computer
* Rolle „Leser“ auf dem Netzwerkadapter mit privater IP-Adresse des virtuellen Computers
* Rolle „Leser“ für die Azure Bastion-Ressource

## <a name="rdp"></a>Verbindung über RDP

1. Verwenden Sie [diesen Link](https://aka.ms/BastionHost), um die Vorschau-Portalseite für Azure Bastion zu öffnen. Navigieren Sie zu dem virtuellen Computer, mit dem Sie eine Verbindung herstellen möchten, und klicken Sie dann auf **Verbinden**. Die VM sollte ein virtueller Windows-Computer sein, wenn Sie eine RDP-Verbindung verwenden.

    ![VM-Verbindung](./media/bastion-connect-vm-rdp/connect.png)

1. Nachdem Sie auf Verbinden geklickt haben, erscheint eine Seitenleiste mit drei Registerkarten – RDP, SSH und Bastion. Wenn Bastion für das virtuelle Netzwerk bereitgestellt wurde, ist die Registerkarte Bastion standardmäßig aktiv. Wenn Sie Bastion nicht für das virtuelle Netzwerk bereitgestellt haben, können Sie auf den Link klicken, um Bastion zu konfigurieren. Konfigurationsanweisungen finden Sie unter [Konfigurieren von Bastion](bastion-create-host-portal.md). Wenn Sie die **Bastion** nicht aufgelistet sehen, haben Sie das Vorschauportal nicht geöffnet. Öffnen Sie das Portal mit diesem [Vorschaulink](https://aka.ms/BastionHost).

    ![VM-Verbindung](./media/bastion-connect-vm-rdp/bastion.png)

1. Geben Sie auf der Registerkarte „Bastion“ den Benutzernamen und das Kennwort für Ihren virtuellen Computer ein, und klicken Sie dann auf **Verbinden**. Die RDP-Verbindung zu diesem virtuellen Computer über Bastion wird direkt im Azure-Portal (über HTML5) über Port 443 und den Bastion-Dienst geöffnet.

    ![VM-Verbindung](./media/bastion-connect-vm-rdp/443rdp.png)
 
## <a name="next-steps"></a>Nächste Schritte

Lesen Sie die [Bastion FAQs](bastion-faq.md)
