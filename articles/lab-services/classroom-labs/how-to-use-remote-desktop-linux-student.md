---
title: Verwenden von Remotedesktop für Linux in Azure Lab Services | Microsoft-Dokumentation
description: Erfahren Sie, wie Sie Remotedesktop für virtuelle Linux-Computer in einem Lab in Azure Lab Services verwenden.
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
ms.openlocfilehash: 0f45af2730b05998fc82212c63778c89bb16b6ef
ms.sourcegitcommit: f4d8f4e48c49bd3bc15ee7e5a77bee3164a5ae1b
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 11/04/2019
ms.locfileid: "73585072"
---
# <a name="use-remote-desktop-for-linux-virtual-machines-in-a-classroom-lab-of-azure-lab-services"></a>Verwenden von Remotedesktop für virtuelle Linux-Computer in einem Classroom-Lab in Azure Lab Services
In diesem Artikel erfahren Sie, wie Kursteilnehmer mithilfe von RDP/SSH eine Verbindung mit einem virtuellen Linux-Computer in einem Lab herstellen können. 

Ein Kursleiter muss die Funktion für eine Remotedesktopverbindung aktivieren, bevor Kursteilnehmer eine Verbindung mit virtuellen Computern des Classroom-Labs herstellen können. Anweisungen dazu, wie ein Kursleiter die Funktion für eine Remotedesktopverbindung aktivieren kann, finden Sie unter [Aktivieren von Remotedesktop für virtuelle Linux-Computer](how-to-enable-remote-desktop-linux.md).

> [!IMPORTANT] 
> Beim Aktivieren der **Remotedesktopverbindung** wird auf Linux-Computern nur der **RDP**-Port geöffnet. Ein Kursleiter kann eine Verbindung mit dem Linux-Computer beim ersten Mal per SSH herstellen und dann RDP- und GUI-Pakete installieren, damit später eine Verbindung mit dem Linux-Computer per RDP hergestellt werden kann. 

## <a name="connect-to-the-student-vm"></a>Herstellen einer Verbindung mit dem virtuellen Computer eines Kursteilnehmers
Kursteilnehmer können per RDP eine Verbindung mit ihren virtuellen Linux-Computern herstellen, nachdem der Lab-Besitzer (Lehrkraft/Hochschullehrkraft) die Vorlage für virtuelle Computer mit darauf installierten RDP- und GUI-Paketen **veröffentlicht** hat. Gehen Sie wie folgt vor: 

1. Wenn sich ein Kursteilnehmer direkt am Labs-Portal (`https://labs.azure.com`) anmeldet oder einen Registrierungslink (`https://labs.azure.com/register/<registrationCode>`) verwendet, wird eine Kachel für jedes Lab angezeigt, auf das der Kursteilnehmer Zugriff hat. 
2. Schalten Sie auf der Kachel die Schaltfläche zum Starten des virtuellen Computers um, um den virtuellen Computer zu starten, wenn er sich im Status „Beendet“ befindet. 
3. Wählen Sie **Verbinden**aus. Es werden zwei Optionen für die Verbindungsherstellung mit dem virtuellen Computer angezeigt: **SSH** und **Remotedesktop**.

    ![Virtueller Computer eines Kursteilnehmers: Verbindungsoptionen](../media/how-to-enable-remote-desktop-linux/student-vm-connect-options.png)

## <a name="connect-using-ssh-or-rdp"></a>Herstellen einer SSH- oder RDP-Verbindung
Wenn Sie die Option **SSH** auswählen, wird das Dialogfeld **Eine Verbindung zu Ihrem virtuellen Computer herstellen** angezeigt:  

![SSH-Verbindungszeichenfolge](../media/how-to-enable-remote-desktop-linux/ssh-connection-string.png)

Wählen Sie neben dem Textfeld die Schaltfläche **Kopieren** aus, um den Wert in die Zwischenablage zu kopieren. Speichern Sie die SSH-Verbindungszeichenfolge. Verwenden Sie diese Verbindungszeichenfolge über ein SSH-Terminal (z. B. [Putty](https://www.putty.org/)), um eine Verbindung mit dem virtuellen Computer herzustellen.

Wenn Sie die Option **RDP** auswählen, wird eine RDP-Datei auf Ihren Computer heruntergeladen. Speichern Sie die Datei, und öffnen Sie sie, um eine Verbindung mit dem Computer herzustellen. 

## <a name="next-steps"></a>Nächste Schritte
Informationen dazu, wie die Funktion für eine Remotedesktopverbindung für virtuelle Linux-Computer in einem Classroom-Lab aktiviert werden kann, finden Sie unter [Aktivieren von Remotedesktop für virtuelle Linux-Computer](how-to-enable-remote-desktop-linux.md). 

