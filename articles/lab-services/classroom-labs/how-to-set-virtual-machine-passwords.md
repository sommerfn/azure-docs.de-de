---
title: Festlegen von Kennwörtern für VMs in Azure Lab Services | Microsoft-Dokumentation
description: Erfahren Sie, wie Sie Kennwörter für virtuelle Computer (VMs) in Classroom-Labs mit Azure Lab Services festlegen und zurücksetzen.
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
ms.date: 10/12/2019
ms.author: spelluru
ms.openlocfilehash: 40cdd0adf7bf100e1dbca64dbba68db3bc59a4fe
ms.sourcegitcommit: 1d0b37e2e32aad35cc012ba36200389e65b75c21
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 10/15/2019
ms.locfileid: "72331399"
---
# <a name="set-up-and-manage-virtual-machine-pool"></a>Einrichten und Verwalten eines VM-Pools 
In diesem Artikel wird gezeigt, wie Sie Folgendes durchführen:

- Erhöhen der Anzahl virtueller Computer (VMs) im Lab
- Starten von allen virtuellen Computern oder von ausgewählten virtuellen Computern 
- Zurücksetzen von virtuellen Computern

## <a name="update-the-lab-capacity"></a>Aktualisieren der Labkapazität
Führen Sie die folgenden Schritte aus, um die Labkapazität (Anzahl der virtuellen Computer in einem Lab) zu erhöhen bzw. zu verringern:

1. Wählen Sie auf der Seite **VM-Pool** die Option **Lab capacity: &lt;number&gt; machines** (Labkapazität: <Anzahl> Computer) aus.
2. Geben Sie die gewünschte neue **Anzahl virtueller Computer** für das Lab ein. Diese Zahl muss größer oder gleich der Anzahl von Benutzern sein, die im Lab registriert sind. 
3. Wählen Sie anschließend **Speichern** aus. 

    ![Schaltfläche „Start all“ (Alle starten)](../media/how-to-set-virtual-machine-passwords/number-of-vms-in-lab.png)
4. Wenn Sie die Kapazität erhöht haben, sehen Sie, wie die virtuellen Computer erstellt werden. 

    ![Erstellung eines virtuellen Computers](../media/how-to-set-virtual-machine-passwords/vm-being-created.png)

## <a name="start-vms"></a>VMs starten

### <a name="start-ot-stop-all-vms"></a>Starten oder Beenden aller virtuellen Computer
1. Wechseln Sie zur Seite **VM-Pool**. 
2. Wählen Sie auf der Symbolleiste **Start all** (Alle starten) aus. 

    ![Schaltfläche „Start all“ (Alle starten)](../media/how-to-set-virtual-machine-passwords/start-all-vms-button.png)
3. Nachdem alle virtuellen Computer gestartet wurden, können Sie sie durch Auswählen der Schaltfläche **Stop all** (Alle beenden) auf der Symbolleiste beenden. 

    ![Schaltfläche „Stop all“ (Alle beenden)](../media/how-to-set-virtual-machine-passwords/stop-all-vms-button.png)

### <a name="start-selected-vms"></a>Starten von ausgewählten virtuellen Computern
Es gibt zwei Möglichkeiten, ausgewählte virtuelle Computer (einen oder mehrere) zu starten. Eine Möglichkeit besteht darin, die virtuellen Computer in der Liste auszuwählen und dann auf der Symbolleiste auf **Starten** zu klicken. Bei der zweiten Möglichkeit wählen Sie die virtuellen Computer in der Liste aus, und wählen in einer Zeile in der Spalte **Zustand** die Dropdownliste und dann **Starten** aus. 

![Starten von ausgewählten virtuellen Computern](../media/how-to-set-virtual-machine-passwords/start-selected-vms.png)

Auf ähnliche Weise können Sie virtuelle Computer über die Dropdownliste in der Spalte **Zustand** oder mit der Schaltfläche **Beenden** auf der Symbolleiste beenden. 

## <a name="reset-vms"></a>Zurücksetzen von virtuellen Computern
Wenn Sie virtuelle Computer zurücksetzen möchten, wählen Sie die entsprechenden Computer in der Liste aus, und klicken Sie dann auf der Symbolleiste auf **Zurücksetzen**. 

![Zurücksetzen von ausgewählten virtuellen Computern](../media/how-to-set-virtual-machine-passwords/reset-vm-button.png)

Wählen Sie im Dialogfeld **Reset virtual machine(s)** (Virtuelle(n) Computer zurücksetzen) die Option **Zurücksetzen** aus. 

![Dialogfeld zum Zurücksetzen virtueller Computer](../media/how-to-set-virtual-machine-passwords/reset-vms-dialog.png)



## <a name="set-password-for-vms"></a>Festlegen eines Kennworts für virtuelle Computer
Ein Labbesitzer (Kursleiter) kann die Kennwörter für virtuelle Computer zum Zeitpunkt der Erstellung des Labs (Assistent für Lab-Erstellung) oder nach dem Erstellen des Labs (auf der Seite **Vorlage**) festlegen und zurücksetzen. 

### <a name="set-password-at-the-time-of-lab-creation"></a>Festlegen des Kennworts zum Zeitpunkt der Lab-Erstellung
Ein Labbesitzer (Kursleiter) kann auf der Seite **Virtual machine credentials** (Anmeldeinformationen für virtuelle Computer) im Assistenten zur Lab-Erstellung ein Kennwort für die virtuellen Computer im Lab festlegen.

![Fenster „Neues Lab“](../media/tutorial-setup-classroom-lab/virtual-machine-credentials.png)

Durch das Aktivieren/Deaktivieren der Option **Use same password for all virtual machines** (Dasselbe Kennwort für alle virtuellen Computer verwenden) auf dieser Seite der Kursleiter festlegen, ob für alle virtuellen Computer im Lab dasselbe Kennwort verwendet werden soll oder ob die Kursteilnehmer eigene Kennwörter für ihre virtuellen Computer festlegen können. Standardmäßig ist diese Einstellung für alle Images mit den Betriebssystemen Windows und Linux mit Ausnahme von Ubuntu aktiviert. Wenn diese Einstellung deaktiviert ist, werden die Kursteilnehmer bei der ersten Verbindung mit der VM aufgefordert, ein Kennwort festzulegen. 

### <a name="reset-password-later"></a>Späteres Zurücksetzen des Kennworts

1. Wählen Sie auf der Seite **Vorlage** des Labs auf der Symbolleiste die Option **Kennwort zurücksetzen** aus. 

    ![Zurücksetzen des Kennworts im Menü auf der Startseite](../media/how-to-set-virtual-machine-passwords/reset-password-menu-dashboard.png)
1. Geben Sie im Dialogfeld **Kennwort zurücksetzen** ein Kennwort ein, und wählen Sie **Kennwort zurücksetzen** aus.
    
    ![Dialogfeld „Kennwort festlegen“](../media/how-to-set-virtual-machine-passwords/set-password.png)

## <a name="next-steps"></a>Nächste Schritte
Informationen zu anderen Verwendungsoptionen für Kursteilnehmer, die Sie (als Lab-Besitzer) konfigurieren können, finden Sie im folgenden Artikel: [Konfigurieren von Nutzungseinstellungen und Richtlinien](how-to-configure-student-usage.md).

Informationen dazu, wie Kursteilnehmer Kennwörter für ihre virtuellen Computer zurücksetzen können, finden Sie unter [Festlegen oder Zurücksetzen von Kennwörtern für virtuelle Computer in Classroom-Labs (Kursteilnehmer)](how-to-set-virtual-machine-passwords-student.md).