---
title: Herstellen einer Verbindung mit einer Windows-VM mit Azure Bastion | Microsoft-Dokumentation
description: In diesem Artikel erfahren Sie, wie Sie eine Verbindung mit einem virtuellen Azure-Computer mit Azure Bastion herstellen.
services: bastion
author: cherylmc
ms.service: bastion
ms.topic: conceptual
ms.date: 10/15/2019
ms.author: cherylmc
ms.openlocfilehash: dc741007c7de8d8e24f9c0f9e4e0c03306d036a4
ms.sourcegitcommit: c22327552d62f88aeaa321189f9b9a631525027c
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 11/04/2019
ms.locfileid: "73498359"
---
# <a name="connect-to-a-windows-virtual-machine-using-azure-bastion"></a>Herstellen einer Verbindung mit einem virtuellen Windows-Computer mit Azure Bastion

In diesem Artikel erfahren Sie, wie Sie mit Azure Bastion sicher und problemlos RDP auf Ihren virtuellen Windows-Computern in einem virtuellen Azure-Netzwerk durchführen können. Sie können sich direkt vom Azure-Portal aus mit einer VM verbinden. Wenn Sie Azure Bastion verwenden, benötigen VMs keinen Client, keinen Agent und auch keine zusätzliche Software. Weitere Informationen über Azure Bastion finden Sie in der [Übersicht](bastion-overview.md).

## <a name="before-you-begin"></a>Voraussetzungen

Stellen Sie sicher, dass Sie einen Azure Bastion-Host für das virtuelle Netzwerk eingerichtet haben, in dem sich die VM befindet. Weitere Informationen finden Sie unter [Erstellen eines Azure Bastion-Hosts](bastion-create-host-portal.md). Sobald der Bastion-Dienst provisioniert und in Ihrem virtuellen Netzwerk bereitgestellt wurde, können Sie ihn verwenden, um sich mit jeder VM in diesem virtuellen Netzwerk zu verbinden. In Bastion wird vorausgesetzt, dass Sie RDP für die Verbindung mit virtuellen Windows-Computern und SSH für die Verbindung mit virtuellen Linux-Computern verwenden. Informationen zur Verbindung mit einer Linux-VM finden Sie unter [Herstellen einer Verbindung mit einer VM – Linux](bastion-connect-vm-ssh.md).

Zum Herstellen einer Verbindung sind die folgenden Rollen erforderlich:

* Rolle „Leser“ auf dem virtuellen Computer
* Rolle „Leser“ auf dem Netzwerkadapter mit privater IP-Adresse des virtuellen Computers
* Rolle „Leser“ für die Azure Bastion-Ressource

## <a name="rdp"></a>Verbindung über RDP

1. Öffnen Sie das [Azure-Portal](https://portal.azure.com). Navigieren Sie zu dem virtuellen Computer, mit dem Sie eine Verbindung herstellen möchten, und klicken Sie dann auf **Verbinden**. Die VM sollte ein virtueller Windows-Computer sein, wenn Sie eine RDP-Verbindung verwenden.

   ![VM-Verbindung](./media/bastion-connect-vm-rdp/connect.png)
1. Nachdem Sie auf Verbinden geklickt haben, erscheint eine Seitenleiste mit drei Registerkarten – RDP, SSH und Bastion. Wenn Bastion für das virtuelle Netzwerk bereitgestellt wurde, ist die Registerkarte Bastion standardmäßig aktiv. Wenn Sie Bastion nicht für das virtuelle Netzwerk bereitgestellt haben, können Sie auf den Link klicken, um Bastion zu konfigurieren. Konfigurationsanweisungen finden Sie unter [Konfigurieren von Bastion](bastion-create-host-portal.md).

   ![VM-Verbindung](./media/bastion-connect-vm-rdp/bastion.png)
1. Geben Sie auf der Registerkarte „Bastion“ den Benutzernamen und das Kennwort für Ihren virtuellen Computer ein, und klicken Sie dann auf **Verbinden**. Die RDP-Verbindung zu diesem virtuellen Computer über Bastion wird direkt im Azure-Portal (über HTML5) über Port 443 und den Bastion-Dienst geöffnet.

   ![VM-Verbindung](./media/bastion-connect-vm-rdp/443rdp.png)
 
## <a name="next-steps"></a>Nächste Schritte

Lesen Sie die [häufig gestellten Fragen zu Bastion](bastion-faq.md).