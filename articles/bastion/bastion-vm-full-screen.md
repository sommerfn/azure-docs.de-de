---
title: Ändern der Sitzungsansicht der VM in den Vollbildmodus in Azure Bastion | Microsoft-Dokumentation
description: In diesem Artikel erfahren Sie, wie Sie die Ansicht in den Vollbildmodus ändern.
services: bastion
author: cherylmc
ms.service: bastion
ms.topic: conceptual
ms.date: 06/03/2019
ms.author: cherylmc
ms.openlocfilehash: 6a063d6c8891133126924bfb934770f7818db71a
ms.sourcegitcommit: 156b313eec59ad1b5a820fabb4d0f16b602737fc
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 06/18/2019
ms.locfileid: "67191253"
---
# <a name="change-to-full-screen-view-for-a-vm-session-azure-bastion-preview"></a>Wechsel zur Vollbildansicht für eine VM-Sitzung: Azure Bastion (Vorschauversion)

Dieser Artikel hilft Ihnen, die Ansicht der VM in Ihrem Browser in den Vollbildmodus zu setzen und wieder zurück. Bevor Sie mit einem virtuellen Computer arbeiten, stellen Sie sicher, dass Sie die Schritte zum [Erstellen eines Azure Bastion-Hosts (Vorschauversion)](bastion-create-host-portal.md) befolgt haben. Stellen Sie dann entweder mit [RDP](bastion-connect-vm-rdp.md) oder [SSH](bastion-connect-vm-ssh.md) eine Verbindung mit der VM her, mit der Sie arbeiten möchten.

> [!IMPORTANT]
> Diese öffentliche Vorschauversion wird ohne Servicelevelvereinbarung bereitgestellt und sollte nicht für Produktionsworkloads verwendet werden. Unter Umständen werden bestimmte Features nicht unterstützt, verfügen über eingeschränkte Funktionen und sind nicht an allen Azure-Standorten verfügbar. Weitere Informationen finden Sie unter [Ergänzende Nutzungsbedingungen für Microsoft Azure-Vorschauversionen](https://azure.microsoft.com/support/legal/preview-supplemental-terms/).
>

## <a name="launch-the-clipboard-tool"></a>Starten des Zwischenablagetools

Starten Sie während der Remotesitzung die Zwischenablagenzugriffs-Toolpalette von Bastion durch Auswahl von zwei Pfeilen, die sich in der Mitte der Sitzung links befinden.

![Tools](./media/bastion-vm-manage/left.png)

## <a name="select-full-screen"></a>Auswählen des Vollbilds

Wählen Sie die **Vollbild**-Schaltfläche aus, um die Sitzung auf eine Vollbild-Benutzeroberfläche umzuschalten. Sobald Sie umschalten, wird die Sitzung mit Vollbild erneut initialisiert.

![Vollbild](./media/bastion-vm-manage/full-screen.png)
 
## <a name="next-steps"></a>Nächste Schritte

Lesen Sie die [häufig gestellten Fragen zu Bastion](bastion-faq.md).
Lernen Sie das [Kopieren und Einfügen](bastion-vm-copy-paste.md) in eine und aus einer Azure-VM kennen.