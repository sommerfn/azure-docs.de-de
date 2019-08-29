---
title: Wiederherstellen von VMware-VMs mit Azure Backup Server
description: Verwenden Sie Azure Backup Server (MABS), um VMware-VMs wiederherzustellen, die auf einem VMware vCenter-/ESXi-Server ausgeführt werden.
author: dcurwin
manager: carmonm
ms.service: backup
ms.topic: conceptual
ms.date: 8/18/2019
ms.author: dacurwin
ms.openlocfilehash: fd851822a7068928a2f332c240bc33b70b1e16f6
ms.sourcegitcommit: 36e9cbd767b3f12d3524fadc2b50b281458122dc
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 08/20/2019
ms.locfileid: "69641926"
---
# <a name="restore-vmware-virtual-machines"></a>Wiederherstellen von virtuellen VMware-Computern

In diesem Artikel wird beschrieben, wie Sie Microsoft Azure Backup Server (MABS) zum Wiederherstellen von VMware-VM-Wiederherstellungspunkten verwenden. Eine Übersicht über die Verwendung von MABS zum Wiederherstellen von Daten finden Sie unter [Wiederherstellen von Daten von Azure Backup Server](https://docs.microsoft.com/azure/backup/backup-azure-alternate-dpm-server). In der MABS-Administratorkonsole haben Sie zwei Möglichkeiten, wiederherstellbare Daten zu ermitteln: Suchen und Durchsuchen. Beim Wiederherstellen von Daten kann es auch sein, dass Sie die Daten oder eine VM nicht an demselben Speicherort wiederherstellen möchten. Aus diesem Grund unterstützt MABS drei Wiederherstellungsoptionen für VMware-VM-Sicherungen:

• **Wiederherstellung am ursprünglichen Speicherort**: Verwenden Sie diese Wiederherstellungsoption, um eine geschützte VM an ihrem ursprünglichen Speicherort wiederherzustellen. Sie können einen virtuellen Computer nur dann am ursprünglichen Speicherort wiederherstellen, wenn seit der Durchführung des Sicherungsvorgangs keine Datenträger hinzugefügt oder gelöscht wurden. Wenn Datenträger hinzugefügt oder gelöscht wurden, müssen Sie einen anderen Ort für die Wiederherstellung verwenden.

• **Wiederherstellung an einem alternativen Speicherort**: Stellen Sie die VM an einem anderen Ort wieder her, falls die ursprüngliche VM fehlt oder Sie die ursprüngliche VM nicht beeinträchtigen möchten. Zum Wiederherstellen einer VM an einem anderen Speicherort müssen Sie den Speicherort eines ESXi-Hosts, Ressourcenpools, Ordners und des Speicherdatenspeichers und -pfads angeben. Zur Unterscheidung der wiederhergestellten VM von der ursprünglichen VM fügt MABS den Zusatz „-Recovered“ an den Namen der VM an.

• **Wiederherstellung einzelner Dateien an einem Speicherort**: Wenn es sich bei der geschützten VM um eine Windows Server-VM handelt, können einzelne Dateien/Ordner der VM mit dieser Option von MABS wiederhergestellt werden. Informationen zur Wiederherstellung einzelner Dateien finden Sie in den Schritten weiter unten in diesem Artikel.


## <a name="restore-a-recovery-point"></a>Wiederherstellen eines Wiederherstellungspunkts

1.  Klicken Sie in der MABS-Administratorkonsole auf die Ansicht „Wiederherstellung“.

2.  Suchen oder filtern Sie im Bereich „Durchsuchen“, um die wiederherzustellende VM zu ermitteln. Nachdem Sie eine VM oder einen Ordner ausgewählt haben, werden im entsprechenden Bereich die verfügbaren Wiederherstellungspunkte angezeigt. 

    ![Verfügbare Wiederherstellungspunkte](./media/restore-azure-backup-server-vmware/recovery-points.png)

3.  Verwenden Sie im Feld **Wiederherstellungspunkte für:** den Kalender und die Dropdownmenüs, um ein Datum auszuwählen, an dem ein Wiederherstellungspunkt erstellt wurde. Für fett formatierte Kalenderdaten sind Wiederherstellungspunkte verfügbar.

4.  Klicken Sie im Menüband des Tools auf **Wiederherstellen**, um den **Wiederherstellungs-Assistenten** zu öffnen. 

    ![Wiederherstellungs-Assistent, Wiederherstellungsauswahl überprüfen](./media/restore-azure-backup-server-vmware/recovery-wizard.png)

5.  Klicken Sie auf **Weiter**, um zum Bildschirm **Wiederherstellungsoptionen angeben**  zu wechseln.

6.  Klicken Sie auf dem Bildschirm **Wiederherstellungsoptionen angeben** auf **Ändern**, wenn Sie die Drosselung der Netzwerkbandbreite aktivieren möchten. Klicken Sie auf **Weiter**, falls die Netzwerkdrosselung deaktiviert bleiben soll. Für VMware-VMs sind auf diesem Bildschirm des Assistenten keine weiteren Optionen verfügbar. Wenn Sie sich für die Änderung einer Drosselung der Netzwerkbandbreite entscheiden, müssen Sie dies aktivieren, indem Sie im Dialogfeld „Drosselung“ die Option **Netzwerk-Bandbreiteneinschränkung aktivieren** auswählen. Konfigurieren Sie nach der Aktivierung die Optionen **Einstellungen** und **Arbeitszeitplan**.

7.  Wählen Sie auf dem Bildschirm **Wiederherstellungstyp auswählen** aus, ob die Wiederherstellung auf der ursprünglichen Instanz oder an einem anderen Speicherort durchgeführt werden soll, und klicken Sie auf **Weiter**.

     * Bei Auswahl von **In ursprünglicher Instanz wiederherstellen**  müssen Sie im Assistenten keine weiteren Angaben machen. Es werden die Daten für die ursprüngliche Instanz verwendet.

    * Wenn Sie **Als virtuellen Computer auf einem beliebigen Host wiederherstellen** auswählen, müssen Sie auf dem Bildschirm **Ziel angeben** die Informationen für **ESXi-Host, Ressourcenpool, Ordner** und **Pfad** angeben. 

      ![Auswählen des Wiederherstellungstyps](./media/restore-azure-backup-server-vmware/recovery-type.png)

8.    Überprüfen Sie Ihre Einstellungen auf dem Bildschirm **Zusammenfassung**, und klicken Sie auf **Wiederherstellen**, um den Wiederherstellungsprozess zu starten. Auf dem Bildschirm **Wiederherstellungsstatus** wird der Status des Wiederherstellungsvorgangs angezeigt.

## <a name="restore-an-individual-file-from-a-vm"></a>Wiederherstellen einer einzelnen Datei von einem virtuellen Computer

Sie können einzelne Dateien eines geschützten VM-Wiederherstellungspunkts wiederherstellen. Dieses Feature ist nur für Windows Server-VMs verfügbar. Die Wiederherstellung einzelner Dateien ähnelt dem Wiederherstellen der gesamten VM. Der Unterschied besteht aber darin, dass Sie das VMDK durchsuchen und die gewünschten Dateien ermitteln, bevor Sie den Wiederherstellungsprozess starten. Gehen Sie zum Wiederherstellen einer einzelnen Datei oder zum Auswählen von Dateien von einer Windows Server-VM wie folgt vor:

1.  Klicken Sie in der MABS-Administratorkonsole auf die Ansicht **Wiederherstellung**.

2.  Suchen oder filtern Sie im Bereich **Durchsuchen**, um die wiederherzustellende VM zu ermitteln. Nachdem Sie eine VM oder einen Ordner ausgewählt haben, werden im entsprechenden Bereich die verfügbaren Wiederherstellungspunkte angezeigt.

    ![Verfügbare Wiederherstellungspunkte](./media/restore-azure-backup-server-vmware/recovery-points.png)
 
3.  Verwenden Sie im Bereich **Wiederherstellungspunkte für:** den Kalender, um das Datum mit den gewünschten Wiederherstellungspunkten auszuwählen. Je nach Konfiguration der Sicherungsrichtlinie können Daten ggf. über mehr als einen Wiederherstellungspunkt verfügen. Nachdem Sie den Tag ausgewählt haben, an dem der Wiederherstellungspunkt erstellt wurde, sollten Sie sicherstellen, dass Sie die richtige **Wiederherstellungszeit** angegeben haben. Falls das ausgewählte Datum über mehrere Wiederherstellungspunkte verfügt, sollten Sie Ihren Wiederherstellungspunkt über das Dropdownmenü „Wiederherstellungszeit“ auswählen. Nach Auswahl des Wiederherstellungspunkts wird die Liste mit den wiederherstellbaren Elementen im Bereich **Pfad:** angezeigt.

4.  Ermitteln Sie die wiederherzustellenden Dateien, indem Sie im Bereich **Pfad** auf das Element in der Spalte **Wiederherstellbares Element** doppelklicken, um es zu öffnen. Wählen Sie die Dateien bzw. Ordner aus, die Sie wiederherstellen möchten. Wenn Sie mehrere Elemente auswählen möchten, können Sie beim Klicken auf die Elemente jeweils **STRG** drücken. Verwenden Sie den Bereich **Pfad**, um die Liste mit den Dateien oder Ordnern in der Spalte **Wiederherstellbares Element** zu durchsuchen. Bei der Option **Liste unten durchsuchen** werden die Unterordner nicht durchsucht. Doppelklicken Sie auf den Ordner, um die Unterordner zu durchsuchen. Verwenden Sie die Schaltfläche **Nach oben**, um von einem untergeordneten Ordner in den übergeordneten Ordner zu wechseln. Sie können mehrere Elemente (Dateien und Ordner) auswählen, aber diese müssen in demselben übergeordneten Ordner enthalten sein. Sie können mit demselben Wiederherstellungsauftrag nicht Elemente aus mehreren Ordnern wiederherstellen.

5.  Klicken Sie nach dem Auswählen der wiederherzustellenden Elemente im Menüband der Administratorkonsole auf **Wiederherstellen**, um den **Wiederherstellungs-Assistenten** zu öffnen. Im Wiederherstellungs-Assistenten werden auf dem Bildschirm **Wiederherstellungsauswahl überprüfen** die ausgewählten Elemente angezeigt, die wiederhergestellt werden sollen. 
    ![Wiederherstellungsauswahl überprüfen](./media/restore-azure-backup-server-vmware/review-recovery.png)

6.  Klicken Sie auf dem Bildschirm **Wiederherstellungsoptionen angeben** auf **Ändern**, wenn Sie die Drosselung der Netzwerkbandbreite aktivieren möchten. Klicken Sie auf **Weiter**, falls die Netzwerkdrosselung deaktiviert bleiben soll. Für VMware-VMs sind auf diesem Bildschirm des Assistenten keine weiteren Optionen verfügbar. Wenn Sie sich für die Änderung einer Drosselung der Netzwerkbandbreite entscheiden, müssen Sie dies aktivieren, indem Sie im Dialogfeld „Drosselung“ die Option **Netzwerk-Bandbreiteneinschränkung aktivieren** auswählen. Konfigurieren Sie nach der Aktivierung die Optionen **Einstellungen** und **Arbeitszeitplan**.
7.  Klicken Sie auf dem Bildschirm **Wiederherstellungstyp auswählen** auf **Weiter**. Sie können Ihre Dateien oder Ordner nur in einem Netzwerkordner wiederherstellen.
8.  Klicken Sie auf dem Bildschirm **Ziel angeben** auf **Durchsuchen**, um eine Netzwerkadresse für Ihre Dateien bzw. Ordner zu ermitteln. MABS erstellt einen Ordner, in den alle wiederhergestellten Elemente kopiert werden. Der Name des Ordners enthält das Präfix „MABS_day-month-year“. Wenn Sie einen Speicherort für die wiederhergestellten Dateien oder den Ordner auswählen, werden die Details für diesen Speicherort angegeben (Ziel, Zielpfad und verfügbarer Speicherplatz). 

       ![Angeben des Speicherorts zum Wiederherstellen von Dateien](./media/restore-azure-backup-server-vmware/specify-destination.png)

9.  Wählen Sie auf dem Bildschirm **Wiederherstellungsoptionen angeben** aus, welche Sicherheitseinstellung angewendet werden soll. Sie können sich für eine Änderung der Drosselung der Netzwerkbandbreite entscheiden, aber die Drosselung ist standardmäßig deaktiviert. Auch die Optionen **SAN-Wiederherstellung** und **Benachrichtigung** sind nicht aktiviert.
10. Überprüfen Sie Ihre Einstellungen auf dem Bildschirm **Zusammenfassung**, und klicken Sie auf **Wiederherstellen**, um den Wiederherstellungsprozess zu starten. Auf dem Bildschirm **Wiederherstellungsstatus** wird der Status des Wiederherstellungsvorgangs angezeigt.


## <a name="next-steps"></a>Nächste Schritte
Lesen Sie den [Leitfaden zur Problembehandlung für Azure Backup Server](./backup-azure-mabs-troubleshoot.md). Darin finden Sie Informationen zur Behandlung von Problemen bei der Verwendung von Azure Backup Server.

