---
title: 'Kopieren und Einfügen bei virtuellen Computern: Azure Bastion | Microsoft-Dokumentation'
description: In diesem Artikel lernen Sie das Kopieren und Einfügen bei einer Azure-VM mit Bastion kennen.
services: bastion
author: cherylmc
ms.service: bastion
ms.topic: conceptual
ms.date: 10/15/2019
ms.author: cherylmc
ms.openlocfilehash: ed8d52a4932271020dfb2a010392b312fa38703b
ms.sourcegitcommit: c22327552d62f88aeaa321189f9b9a631525027c
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 11/04/2019
ms.locfileid: "73519354"
---
# <a name="copy-and-paste-to-a-virtual-machine-azure-bastion"></a>Kopieren und Einfügen bei virtuellen Computern: Azure Bastion

Dieser Artikel hilft Ihnen, Text mit Azure Bastion aus VMs zu kopieren und in VMs einzufügen. Bevor Sie mit einem virtuellen Computer arbeiten, stellen Sie sicher, dass Sie die Schritte zum [Erstellen eines Azure Bastion-Hosts (Vorschauversion)](bastion-create-host-portal.md) befolgt haben. Stellen Sie dann entweder mit [RDP](bastion-connect-vm-rdp.md) oder [SSH](bastion-connect-vm-ssh.md) eine Verbindung mit der VM her, mit der Sie arbeiten möchten.

Für Browser, die den erweiterten Zugriff auf die Zwischenablage-API unterstützen, können Sie Text zwischen Ihrem lokalen Gerät und der Remotesitzung genauso kopieren und einfügen wie zwischen Anwendungen und Ihrem lokalen Gerät. Für andere Browser können Sie die Zwischenablagenzugriffs-Toolpalette von Bastion verwenden.

   ![Zulassen der Zwischenablage](./media/bastion-vm-manage/allow.png)

Nur das Kopieren/Einfügen von Text wird unterstützt. Zum direkten Kopieren und Einfügen fordert Ihr Browser Sie möglicherweise auf, den Zugriff auf die Zwischenablage zuzulassen, wenn die Bastion-Sitzung initialisiert wird. **Erlauben** Sie den Zugriff der Webseite auf die Zwischenablage.

## <a name="to"></a>Kopieren in eine Remotesitzung

Führen Sie die folgenden Schritte aus, nachdem Sie im [Azure-Portal](https://portal.azure.com) eine Verbindung mit dem virtuellen Computer hergestellt haben:

1. Kopieren Sie den Text/Inhalt vom lokalen Gerät in die lokale Zwischenablage.
1. Starten Sie während der Remotesitzung die Zwischenablagenzugriffs-Toolpalette von Bastion durch Auswahl von zwei Pfeilen. Die Pfeile befinden sich in der Mitte der Sitzung links.

   ![Toolpalette](./media/bastion-vm-manage/left.png)

   ![Zwischenablage](./media/bastion-vm-manage/clipboard.png)
1. Normalerweise wird der kopierte Text automatisch in der Bastion-Palette zum Kopieren/Einfügen angezeigt. Wenn der Text nicht angezeigt wird, fügen Sie den Text in den Textbereich auf der Palette ein.
1. Sobald der Text im Textbereich ist, können Sie ihn in der Remotesitzung einfügen.

   ![Einfügen](./media/bastion-vm-manage/local.png)

## <a name="from"></a>Kopieren aus einer Remotesitzung

Führen Sie die folgenden Schritte aus, nachdem Sie im [Azure-Portal](https://portal.azure.com) eine Verbindung mit dem virtuellen Computer hergestellt haben:

1. Kopieren Sie den Text/Inhalt von der Remotesitzung in die Remotezwischenablage (mit STRG+C).

   ![Toolpalette](./media/bastion-vm-manage/remote.png)
1. Starten Sie während der Remotesitzung die Zwischenablagenzugriffs-Toolpalette von Bastion durch Auswahl von zwei Pfeilen. Die Pfeile befinden sich in der Mitte der Sitzung links.

   ![Zwischenablage](./media/bastion-vm-manage/clipboard2.png)
1. Normalerweise wird der kopierte Text automatisch in der Bastion-Palette zum Kopieren/Einfügen angezeigt. Wenn der Text nicht angezeigt wird, fügen Sie den Text in den Textbereich auf der Palette ein.
1. Sobald der Text im Textbereich ist, können Sie ihn auf dem lokalen Gerät einfügen.

   ![Einfügen](./media/bastion-vm-manage/local2.png)
 
## <a name="next-steps"></a>Nächste Schritte

Lesen Sie die [häufig gestellten Fragen zu Bastion](bastion-faq.md).