---
title: 'Kopieren und Einfügen bei virtuellen Computern: Azure Bastion | Microsoft-Dokumentation'
description: In diesem Artikel lernen Sie das Kopieren und Einfügen bei einer Azure-VM mit Bastion kennen.
services: bastion
author: cherylmc
ms.service: bastion
ms.topic: conceptual
ms.date: 06/03/2019
ms.author: cherylmc
ms.openlocfilehash: 9d69d1a9fae258c9546a8c794fc0b0c68b952049
ms.sourcegitcommit: 156b313eec59ad1b5a820fabb4d0f16b602737fc
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 06/18/2019
ms.locfileid: "67191333"
---
# <a name="copy-and-paste-to-a-virtual-machine-azure-bastion-preview"></a>Kopieren und Einfügen bei virtuellen Computern: Azure Bastion (Vorschauversion)

Dieser Artikel hilft Ihnen, Text mit Azure Bastion aus VMs zu kopieren und in VMs einzufügen. Bevor Sie mit einem virtuellen Computer arbeiten, stellen Sie sicher, dass Sie die Schritte zum [Erstellen eines Azure Bastion-Hosts (Vorschauversion)](bastion-create-host-portal.md) befolgt haben. Stellen Sie dann entweder mit [RDP](bastion-connect-vm-rdp.md) oder [SSH](bastion-connect-vm-ssh.md) eine Verbindung mit der VM her, mit der Sie arbeiten möchten.

> [!IMPORTANT]
> Diese öffentliche Vorschauversion wird ohne Servicelevelvereinbarung bereitgestellt und sollte nicht für Produktionsworkloads verwendet werden. Unter Umständen werden bestimmte Features nicht unterstützt, verfügen über eingeschränkte Funktionen und sind nicht an allen Azure-Standorten verfügbar. Weitere Informationen finden Sie unter [Ergänzende Nutzungsbedingungen für Microsoft Azure-Vorschauversionen](https://azure.microsoft.com/support/legal/preview-supplemental-terms/).
>

Für Browser, die den erweiterten Zugriff auf die Zwischenablage-API unterstützen, können Sie Text zwischen Ihrem lokalen Gerät und der Remotesitzung genauso kopieren und einfügen wie zwischen Anwendungen und Ihrem lokalen Gerät. Für andere Browser können Sie die Zwischenablagenzugriffs-Toolpalette von Bastion verwenden.

  ![Zulassen der Zwischenablage](./media/bastion-vm-manage/allow.png)

Nur das Kopieren/Einfügen von Text wird unterstützt. Zum direkten Kopieren und Einfügen fordert Ihr Browser Sie möglicherweise auf, den Zugriff auf die Zwischenablage zuzulassen, wenn die Bastion-Sitzung initialisiert wird. **Erlauben** Sie den Zugriff der Webseite auf die Zwischenablage.

## <a name="to"></a>Kopieren in eine Remotesitzung

Nach dem Herstellen einer Verbindung mit dem virtuellen Computer mithilfe des [Azure-Portals](https://aka.ms/BastionHost) für die Bastion-Vorschau:

1. Kopieren Sie den Text/Inhalt vom lokalen Gerät in die lokale Zwischenablage.
1. Starten Sie während der Remotesitzung die Zwischenablagenzugriffs-Toolpalette von Bastion durch Auswahl von zwei Pfeilen. Die Pfeile befinden sich in der Mitte der Sitzung links.

    ![Toolpalette](./media/bastion-vm-manage/left.png)

    ![Zwischenablage](./media/bastion-vm-manage/clipboard.png)

1. Normalerweise wird der kopierte Text automatisch in der Bastion-Palette zum Kopieren/Einfügen angezeigt. Wenn der Text nicht angezeigt wird, fügen Sie den Text in den Textbereich auf der Palette ein.
1. Sobald der Text im Textbereich ist, können Sie ihn in der Remotesitzung einfügen.

    ![Einfügen](./media/bastion-vm-manage/local.png)

## <a name="from"></a>Kopieren aus einer Remotesitzung

Nach dem Herstellen einer Verbindung mit dem virtuellen Computer mithilfe des [Azure-Portals](https://aka.ms/BastionHost) für die Bastion-Vorschau:

1. Kopieren Sie den Text/Inhalt von der Remotesitzung in die Remotezwischenablage (mit STRG+C).

    ![Toolpalette](./media/bastion-vm-manage/remote.png)

1. Starten Sie während der Remotesitzung die Zwischenablagenzugriffs-Toolpalette von Bastion durch Auswahl von zwei Pfeilen. Die Pfeile befinden sich in der Mitte der Sitzung links.

    ![Zwischenablage](./media/bastion-vm-manage/clipboard2.png)

1. Normalerweise wird der kopierte Text automatisch in der Bastion-Palette zum Kopieren/Einfügen angezeigt. Wenn der Text nicht angezeigt wird, fügen Sie den Text in den Textbereich auf der Palette ein.
1. Sobald der Text im Textbereich ist, können Sie ihn auf dem lokalen Gerät einfügen.

    ![Einfügen](./media/bastion-vm-manage/local2.png)
 
## <a name="next-steps"></a>Nächste Schritte

Lesen Sie die [häufig gestellten Fragen zu Bastion](bastion-faq.md).