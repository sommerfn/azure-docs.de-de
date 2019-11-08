---
title: Herstellen einer Verbindung mit einer Windows-VM-Skalierungsgruppe mit Azure Bastion | Microsoft-Dokumentation
description: In diesem Artikel erfahren Sie, wie Sie eine Verbindung mit einer Azure-VM-Skalierungsgruppe mit Azure Bastion herstellen.
services: bastion
author: cherylmc
ms.service: bastion
ms.topic: conceptual
ms.date: 10/14/2019
ms.author: cherylmc
ms.openlocfilehash: 446784155cf6f72cfaa80523ed3913eacc7e5cfc
ms.sourcegitcommit: c22327552d62f88aeaa321189f9b9a631525027c
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 11/04/2019
ms.locfileid: "73510375"
---
# <a name="connect-to-a-virtual-machine-scale-set-using-azure-bastion"></a>Herstellen einer Verbindung mit einer VM-Skalierungsgruppe mit Azure Bastion

In diesem Artikel erfahren Sie, wie Sie mit Azure Bastion in einem virtuellen Azure-Netzwerk sicher und problemlos RDP auf Ihrer Windows-VM-Skalierungsgruppeninstanz durchführen können. Sie können direkt vom Azure-Portal aus eine Verbindung mit einer VM-Skalierungsgruppeninstanz herstellen. Wenn Sie Azure Bastion verwenden, benötigen VMs keinen Client, keinen Agent und auch keine zusätzliche Software. Weitere Informationen über Azure Bastion finden Sie in der [Übersicht](bastion-overview.md).

## <a name="before-you-begin"></a>Voraussetzungen

Stellen Sie sicher, dass Sie einen Azure Bastion-Host für das virtuelle Netzwerk eingerichtet haben, in dem sich die VM-Skalierungsgruppe befindet. Weitere Informationen finden Sie unter [Erstellen eines Azure Bastion-Hosts](bastion-create-host-portal.md). Sobald der Bastion-Dienst provisioniert und in Ihrem virtuellen Netzwerk bereitgestellt wurde, können Sie ihn verwenden, um mit jeder VM-Skalierungsgruppeninstanz in diesem virtuellen Netzwerk eine Verbindung herzustellen. Bastion setzt voraus, dass Sie RDP für die Verbindung mit einer Windows-VM-Skalierungsgruppe und SSH für die Verbindung mit Ihrer Linux-VM-Skalierungsgruppe verwenden. Informationen zur Verbindung mit einer Linux-VM finden Sie unter [Herstellen einer Verbindung mit einer VM – Linux](bastion-connect-vm-ssh.md).

## <a name="rdp"></a>Verbindung über RDP

1. Öffnen Sie das [Azure-Portal](https://portal.azure.com). Navigieren Sie zu der VM-Skalierungsgruppe, mit der Sie eine Verbindung herstellen möchten.

   ![Navigieren](./media/bastion-connect-vm-scale-set/1.png)
2. Navigieren Sie zu der VM-Skalierungsgruppeninstanz, mit der Sie eine Verbindung herstellen möchten, und wählen Sie **Verbinden** aus. Wenn Sie eine RDP-Verbindung verwenden, sollte die VM-Skalierungsgruppe eine Windows-VM-Skalierungsgruppe sein.

   ![VM-Skalierungsgruppe](./media/bastion-connect-vm-scale-set/2.png)
3. Nachdem Sie **Verbinden** ausgewählt haben, wird eine Seitenleiste mit drei Registerkarten angezeigt – „RDP“, „SSH“ und „Bastion“. Wählen Sie in der Seitenleiste die Registerkarte **Bastion** aus. Wenn Sie Bastion nicht für das virtuelle Netzwerk bereitgestellt haben, können Sie den Link auswählen, um Bastion zu konfigurieren. Konfigurationsanweisungen finden Sie unter [Konfigurieren von Bastion](bastion-create-host-portal.md).

   ![Registerkarte „Bastion“](./media/bastion-connect-vm-scale-set/3.png)
4. Geben Sie auf der Registerkarte „Bastion“ den Benutzernamen und das Kennwort für Ihre VM-Skalierungsgruppe ein, und wählen Sie dann **Verbinden** aus.

   ![Verbinden](./media/bastion-connect-vm-scale-set/4.png)
5. Die RDP-Verbindung zu diesem virtuellen Computer über Bastion wird direkt im Azure-Portal (über HTML5) über Port 443 und den Bastion-Dienst geöffnet.

## <a name="next-steps"></a>Nächste Schritte

Lesen Sie die [häufig gestellten Fragen zu Bastion](bastion-faq.md).