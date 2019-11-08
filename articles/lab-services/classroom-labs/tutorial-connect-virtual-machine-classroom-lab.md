---
title: Zugreifen auf ein Classroom-Lab in Azure Lab Services | Microsoft-Dokumentation
description: In diesem Tutorial greifen Sie in einem Classroom-Lab, das von einer Lehrkraft eingerichtet wurde, auf virtuelle Computer zu.
services: devtest-lab, lab-services, virtual-machines
documentationcenter: na
author: spelluru
manager: ''
editor: ''
ms.service: lab-services
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: tutorial
ms.custom: mvc
ms.date: 10/31/2019
ms.author: spelluru
ms.openlocfilehash: 30983d141f087a46e420f7ea457fba181956c28e
ms.sourcegitcommit: f4d8f4e48c49bd3bc15ee7e5a77bee3164a5ae1b
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 11/04/2019
ms.locfileid: "73577796"
---
# <a name="tutorial-access-a-classroom-lab-in-azure-lab-services"></a>Tutorial: Zugreifen auf ein Classroom-Lab in Azure Lab Services
In diesem Tutorial stellen Sie als Kursteilnehmer eine Verbindung mit einem virtuellen Computer in einem Classroom-Lab her. 

In diesem Tutorial führen Sie die folgenden Aktionen aus:

> [!div class="checklist"]
> * Registrieren beim Lab
> * Starten der VM
> * Herstellen der Verbindung zur VM

## <a name="register-to-the-lab"></a>Registrieren beim Lab

1. Navigieren Sie zur **Registrierungs-URL**, die Sie von der Lehrkraft erhalten haben. Die Registrierungs-URL muss nach Abschluss der Registrierung nicht mehr verwendet werden. Verwenden Sie stattdessen die folgende URL: [https://labs.azure.com](https://labs.azure.com). Internet Explorer 11 wird noch nicht unterstützt. 
1. Melden Sie sich mit Ihrem Geschäfts-, Schul- oder Unikonto beim Dienst an, um die Registrierung abzuschließen. 

    > [!NOTE]
    > Für die Verwendung von Azure Lab Services ist ein Microsoft-Konto erforderlich. Wenn Sie ein nicht von Microsoft stammendes Konto (beispielsweise ein Yahoo- oder Google-Konto) für die Anmeldung beim Portal verwenden möchten, befolgen Sie die Anweisungen zum Erstellen eines Microsoft-Kontos, das mit Ihrem Nicht-Microsoft-Konto verknüpft wird. Führen Sie anschließend die Schritte zum Abschließen des Registrierungsprozesses aus. 
1. Vergewissern Sie sich nach der Registrierung, dass Sie den virtuellen Computer für das Lab sehen, auf das Sie Zugriff haben. 
1. Warten Sie, bis der virtuelle Computer bereit ist. Beachten Sie auf der VM-Kachel die folgenden Felder:
    1. Im oberen Kachelbereich wird der **Name des Labs** angezeigt.
    1. Auf der rechten Seite wird das Symbol angezeigt, das für das **Betriebssystem** des virtuellen Computers steht. In diesem Beispiel ist es das Windows-Betriebssystem. 
    1. Die Statusanzeige auf der Kachel gibt Aufschluss darüber, wie viele Stunden der Ihnen zugewiesenen [Kontingentstunden](how-to-configure-student-usage.md#set-quotas-for-users) bereits verwendet wurden. Diese Zeit wurde Ihnen zusätzlich zu der geplanten Zeit für das Lab zugewiesen. 
    1. Im unteren Kachelbereich befinden sich Symbole und Schaltflächen, über die Sie den virtuellen Computer starten und beenden sowie eine Verbindung mit dem virtuellen Computer herstellen können. 
    1. Rechts von den Schaltflächen wird der Status des virtuellen Computers angezeigt. Vergewissern Sie sich, dass der Status des virtuellen Computers **Beendet** lautet. 

        ![Virtueller Computer im Status „Beendet“](../media/tutorial-connect-vm-in-classroom-lab/vm-in-stopped-state.png)

## <a name="start-the-vm"></a>Starten der VM
1. **Starten** Sie den virtuellen Computer, indem Sie die erste Schaltfläche auswählen, wie in der folgenden Abbildung gezeigt. Dieser Prozess nimmt einige Zeit in Anspruch.  

    ![Starten der VM](../media/tutorial-connect-vm-in-classroom-lab/start-vm.png)
4. Vergewissern Sie sich, dass der Status des virtuellen Computers **Wird ausgeführt** lautet. 

    ![Ausgeführter virtueller Computer](../media/tutorial-connect-vm-in-classroom-lab/vm-running.png)

    Beachten Sie, dass das Symbol der ersten Schaltfläche so geändert wurde, dass es nun einen Vorgang zum **Beenden** darstellt. Sie können diese Schaltfläche auswählen, um den virtuellen Computer zu beenden. 

## <a name="connect-to-the-vm"></a>Herstellen der Verbindung zur VM

1. Wählen Sie die zweite Schaltfläche aus wie in der folgenden Abbildung gezeigt, um eine **Verbindung** mit dem virtuellen Computer des Labs herzustellen. 

    ![Herstellen einer Verbindung mit dem virtuellen Computer](../media/tutorial-connect-vm-in-classroom-lab/connect-vm.png)
2. Führen Sie einen der folgenden Schritte aus: 
    1. Speichern Sie für virtuelle **Windows**-Computer die **RDP**-Datei auf der Festplatte. Öffnen Sie die RDP-Datei, um eine Verbindung mit dem virtuellen Computer herzustellen. Verwenden Sie die Kombination aus **Benutzername** und **Kennwort**, die Sie von Ihrer Lehrkraft erhalten haben, um sich bei dem Computer anzumelden. 
    3. Bei virtuellen **Linux**-Computern können Sie **SSH** oder **RDP** (sofern RDP aktiviert ist) für die Verbindungsherstellung verwenden. Weitere Informationen finden Sie unter [Enable and use remote desktop for Linux virtual machines in a lab in Azure Lab Services](how-to-enable-remote-desktop-linux.md) (Aktivieren und Verwenden von Remotedesktop für virtuelle Linux-Computer in einem Lab in Azure Lab Services). 

## <a name="next-steps"></a>Nächste Schritte
In diesem Tutorial haben Sie über einen Registrierungslink, den Sie von Ihrem Lehrer bzw. Dozenten erhalten haben, auf ein Classroom-Lab zugegriffen.

Als Besitzer des Labs möchten Sie die beim Lab registrierten Personen anzeigen und die Nutzung der VMs nachverfolgen. Im nächsten Tutorial erfahren Sie, wie Sie die Nutzung des Labs nachverfolgen:

> [!div class="nextstepaction"]
> [Tutorial: Nachverfolgen der Nutzung eines Labs in Azure Lab Services](tutorial-track-usage.md) 
