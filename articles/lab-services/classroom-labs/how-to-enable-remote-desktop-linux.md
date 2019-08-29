---
title: Aktivieren von Remotedesktop für Linux in Azure Lab Services | Microsoft-Dokumentation
description: Es wird beschrieben, wie Sie Remotedesktop für virtuelle Linux-Computer in einem Lab in Azure Lab Services aktivieren.
services: lab-services
documentationcenter: na
author: spelluru
manager: ''
editor: ''
ms.service: lab-services
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 08/20/2019
ms.author: spelluru
ms.openlocfilehash: 3d08105e78274300eb7ee0a8c0ad146a737d0ffa
ms.sourcegitcommit: bb8e9f22db4b6f848c7db0ebdfc10e547779cccc
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 08/20/2019
ms.locfileid: "69644953"
---
# <a name="enable-remote-desktop-for-linux-virtual-machines-in-a-lab-in-azure-lab-services"></a>Aktivieren von Remotedesktop für virtuelle Linux-Computer in einem Lab in Azure Lab Services
In diesem Artikel wird gezeigt, wie Sie Folgendes durchführen:

- Aktivieren von Remotedesktop für einen virtuellen Linux-Computer
- Herstellen einer Verbindung mit der Vorlage für virtuelle Computer per Remotedesktopverbindung (RDP) für Lehrkräfte

## <a name="enable-remote-desktop-for-linux-vm"></a>Aktivieren von Remotedesktop für einen virtuellen Linux-Computer
Während der Lab-Erstellung können Lehrkräfte die **Remotedesktopverbindung** für **Linux**-Images aktivieren. Die Option **Remotedesktop aktivieren** wird angezeigt, wenn für die Vorlage ein Linux-Image ausgewählt wurde. Wenn diese Option aktiviert ist, können Lehrkräfte eine Verbindung mit der Vorlage für virtuelle Computer und mit virtuellen Computern für Kursteilnehmer per RDP (Remotedesktopverbindung) herstellen. 

![Aktivieren der Remotedesktopverbindung für ein Linux-Image](../media/how-to-enable-remote-desktop-linux/enable-rdp-option.png)

Wählen Sie im Meldungsfeld **Enabling Remote Desktop Connection** (Aktivieren der Remotedesktopverbindung) die Option **Continue with Remote Desktop** (Weiter mit Remotedesktop) aus. 

![Aktivieren der Remotedesktopverbindung für ein Linux-Image](../media/how-to-enable-remote-desktop-linux/enabling-remote-desktop-connection-dialog.png)

> [!IMPORTANT] 
> Beim Aktivieren der **Remotedesktopverbindung** wird auf Linux-Computern nur der **RDP**-Port geöffnet. Sie als Lehrkraft stellen eine Verbindung mit dem Linux-Computer beim ersten Mal per SSH her und installieren RDP- und GUI-Pakete, damit Sie später RDP nutzen können, um eine Verbindung mit dem Linux-Computer herzustellen. Anschließend **veröffentlichen** Sie das Image, damit die Kursteilnehmer per RDP eine Verbindung mit den virtuellen Linux-Computern herstellen können, die für sie bestimmt sind. 

## <a name="supported-operating-systems"></a>Unterstützte Betriebssysteme
Derzeit wird die Remotedesktopverbindung für die folgenden Betriebssysteme unterstützt:

- openSUSE Leap 42.3
- 7\.5 (CentOS-basiert)
- Debian 9 „Stretch“
- Ubuntu Server 16.04 LTS

## <a name="teachers-connecting-to-the-template-vm-using-rdp"></a>Herstellen einer Verbindung mit der Vorlage für virtuelle Computer per RDP durch Lehrkräfte
Lehrkräfte müssen zunächst per SSH eine Verbindung mit der Vorlage für virtuelle Computer herstellen und darauf RDP- und GUI-Pakete installieren. Anschließend können die Lehrkräfte die folgenden Schritte ausführen, um per RDP eine Verbindung mit den virtuellen Linux-Computern herzustellen: 

Die Option **Remotedesktop** zum Herstellen einer Verbindung mit der Vorlage für virtuelle Computer wird bei der Erstellung des Labs angezeigt. 

![Herstellen einer Verbindung mit der Vorlage per RDP während der Erstellung](../media/how-to-enable-remote-desktop-linux/connect-at-creation.png)

Die Option **Remotedesktop** wird auf der Startseite des Labs angezeigt, nachdem das Lab erstellt und die Vorlage für virtuelle Computer gestartet wurde. Starten Sie die Vorlage für virtuelle Computer, falls sie nicht bereits gestartet wurde. 

![Herstellen einer Verbindung mit der Vorlage per RDP nach der Lab-Erstellung](../media/how-to-enable-remote-desktop-linux/rdp-after-lab-creation.png) 

Weitere Informationen zum Herstellen einer SSH- oder RDP-Verbindung mit dem virtuellen Computer finden Sie unter [Connect using SSH or RDP]((#connect-using-ssh-or-rdp) (Herstellen einer SSH- oder RDP-Verbindung). 

## <a name="teachers-connecting-to-a-student-vm-using-rdp"></a>Herstellen einer Verbindung mit einem virtuellen Computer für Kursteilnehmer per RDP
Eine Lehrkraft/Hochschullehrkraft kann eine Verbindung mit einem virtuellen Computer für Kursteilnehmer herstellen, indem er zur Ansicht **Virtuelle Computer** wechselt und das Symbol **Verbinden** auswählt. Vorher müssen Lehrkräfte das Vorlagenimage mit darauf installierten RDP- und GUI-Paketen **veröffentlichen**. 

![Herstellen einer Verbindung mit dem virtuellen Computer für Kursteilnehmer durch Lehrkräfte](../media/how-to-enable-remote-desktop-linux/teacher-connect-to-student-vm.png)

Weitere Informationen zum Herstellen einer SSH- oder RDP-Verbindung mit dem virtuellen Computer finden Sie unter [Connect using SSH or RDP]((#connect-using-ssh-or-rdp) (Herstellen einer SSH- oder RDP-Verbindung). 

## <a name="connect-using-ssh-or-rdp"></a>Herstellen einer SSH- oder RDP-Verbindung
Wenn Sie die Option **SSH** auswählen, wird das Dialogfeld **Eine Verbindung zu Ihrem virtuellen Computer herstellen** angezeigt:  

![SSH-Verbindungszeichenfolge](../media/how-to-enable-remote-desktop-linux/ssh-connection-string.png)

Wählen Sie neben dem Textfeld die Schaltfläche **Kopieren** aus, um den Wert in die Zwischenablage zu kopieren. Speichern Sie die SSH-Verbindungszeichenfolge. Verwenden Sie diese Verbindungszeichenfolge über ein SSH-Terminal (z. B. [Putty](https://www.putty.org/)), um eine Verbindung mit dem virtuellen Computer herzustellen.

Wenn Sie die Option **RDP** auswählen, wird eine RDP-Datei auf Ihren Computer heruntergeladen. Speichern Sie die Datei, und öffnen Sie sie, um eine Verbindung mit dem Computer herzustellen. 

## <a name="next-steps"></a>Nächste Schritte
Nachdem ein Dozent das Feature „Remotedesktopverbindung“ aktiviert hat, können Kursteilnehmer über RDP/SSH eine Verbindung mit ihren VMs herstellen. Weitere Informationen finden Sie unter [Verwenden von Remotedesktop für virtuelle Linux-Computer in einem Classroom-Lab in Azure Lab Services](how-to-use-remote-desktop-linux-student.md). 