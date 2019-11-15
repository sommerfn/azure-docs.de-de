---
title: Wiederherstellen von Dateien aus Azure auf einer Windows Server-Instanz
description: In diesem Tutorial erfahren Sie, wie Sie den MARS-Agent (Microsoft Azure Recovery Services-Agent) zum Wiederherstellen von Elementen aus Azure auf einem Windows-Server verwenden.
author: dcurwin
manager: carmonm
ms.service: backup
ms.topic: tutorial
ms.date: 02/14/2018
ms.author: dacurwin
ms.custom: mvc
ms.openlocfilehash: a0945c70b271d5213e8eb5b6adbd768ec390f40b
ms.sourcegitcommit: 827248fa609243839aac3ff01ff40200c8c46966
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 11/07/2019
ms.locfileid: "73746936"
---
# <a name="recover-files-from-azure-to-a-windows-server"></a>Wiederherstellen von Dateien aus Azure auf einer Windows Server-Instanz

Azure Backup ermöglicht die Wiederherstellung einzelner Elemente aus Sicherungen Ihrer Windows Server-Instanzen. Das Wiederherstellen einzelner Dateien ist nützlich, wenn Sie Dateien, die versehentlich gelöscht wurden, schnell wiederherstellen müssen. In diesem Tutorial erfahren Sie, wie Sie den Microsoft Azure Recovery Services-Agent (MARS) verwenden, um Elemente aus Sicherungen, die Sie zuvor in Azure erstellt haben, wiederherzustellen. In diesem Tutorial lernen Sie Folgendes:

> [!div class="checklist"]
>
> * Initiieren der Wiederherstellung einzelner Elemente
> * Wählen Sie einen Wiederherstellungspunkt
> * Wiederherstellen von Elementen aus einem Wiederherstellungspunkt

In diesem Tutorial wird davon ausgegangen, dass Sie bereits die Schritte zum [Sichern einer Windows Server-Instanz in Azure](backup-configure-vault.md) ausgeführt haben und über mindestens eine Sicherung Ihrer Windows Server-Dateien in Azure verfügen.

## <a name="initiate-recovery-of-individual-items"></a>Initiieren der Wiederherstellung einzelner Elemente

Zusammen mit dem Microsoft Azure Recovery Services-Agent (MARS) wird ein hilfreicher Assistent mit Benutzeroberfläche installiert: Microsoft Azure Backup. Der Microsoft Azure Backup-Assistent arbeitet mit dem Microsoft Azure Recovery Services-Agent (MARS) zusammen, um Sicherungsdaten aus Wiederherstellungspunkten, die in Azure gespeichert sind, abzurufen. Verwenden Sie den Microsoft Azure Backup-Assistenten zum Identifizieren der Dateien oder Ordner, die Sie unter Windows Server wiederherstellen möchten.

1. Öffnen Sie das Snap-In **Microsoft Azure Backup**. Den Agent finden Sie, indem Sie auf Ihrem Computer nach **Microsoft Azure Backup**suchen.

    ![Sicherung ausstehend](./media/tutorial-backup-restore-files-windows-server/mars.png)

2. Klicken Sie im Assistenten im Bereich **Aktionen** der Agent-Konsole auf **Daten wiederherstellen**, um den Assistenten **Daten wiederherstellen** zu starten.

    ![Sicherung ausstehend](./media/tutorial-backup-restore-files-windows-server/mars-recover-data.png)

3. Wählen Sie auf der Seite **Erste Schritte** die Option **Dieser Server (Servername)** aus, und klicken Sie auf **Weiter**.

4. Wählen Sie auf der Seite **Wiederherstellungsmodus auswählen** die Option **Einzelne Dateien und Ordner** aus, und klicken Sie dann auf **Weiter**, um den Auswahlprozess für den Wiederherstellungspunkt zu starten.

5. Wählen Sie auf der Seite **Volume und Datum auswählen** das Volume aus, das die wiederherzustellenden Dateien bzw. Ordner enthält, und klicken Sie dann auf **Bereitstellen**. Wählen Sie im Dropdownmenü ein Datum und einen Zeitpunkt für einen Wiederherstellungspunkt aus. Wenn Datumsangaben **fett** angezeigt werden, ist dies ein Hinweis darauf, dass mindestens ein Wiederherstellungszeitpunkt mit diesem Datum verfügbar ist.

    ![Sicherung ausstehend](./media/tutorial-backup-restore-files-windows-server/mars-select-date.png)

    Beim Klicken auf **Bereitstellen** macht Azure Backup den Wiederherstellungspunkt als Datenträger verfügbar. Suchen Sie die Dateien auf dem Datenträger, und stellen Sie sie wieder her.

## <a name="restore-items-from-a-recovery-point"></a>Wiederherstellen von Elementen aus einem Wiederherstellungspunkt

1. Klicken Sie nach dem Einbinden des Wiederherstellungsvolumes auf **Durchsuchen**, um Windows-Explorer zu öffnen und nach den Dateien und Ordnern für die Wiederherstellung zu suchen.

    ![Sicherung ausstehend](./media/tutorial-backup-restore-files-windows-server/mars-browse-recover.png)

    Sie können die Dateien direkt vom Wiederherstellungsvolume öffnen und überprüfen.

2. Kopieren Sie in Windows-Explorer die wiederherzustellenden Dateien bzw. Ordner, und fügen Sie sie am gewünschten Speicherort auf dem Server ein.

    ![Sicherung ausstehend](./media/tutorial-backup-restore-files-windows-server/mars-final.png)

3. Klicken Sie auf der Seite **Dateien suchen und wiederherstellen** im Assistenten **Daten wiederherstellen** auf **Bereitstellung aufheben**, wenn Sie die Wiederherstellung der Dateien bzw. Ordner abgeschlossen haben.

    ![Sicherung ausstehend](./media/tutorial-backup-restore-files-windows-server/unmount-and-confirm.png)

4. Klicken Sie auf **Ja**, um zu bestätigen, dass Sie die Einbindung des Volumes aufheben möchten.

    Nachdem die Einbindung der Momentaufnahme aufgehoben wurde, wird in der Agent-Konsole im Bereich **Aufträge** der Status **Auftrag abgeschlossen** angezeigt.

## <a name="next-steps"></a>Nächste Schritte

Dies schließt die Tutorials zum Sichern und Wiederherstellen von Windows Server-Daten in Azure ab. Informieren Sie sich über Azure Backup, und sehen Sie sich das PowerShell-Beispiel zum Sichern verschlüsselter virtueller Computer an.

> [!div class="nextstepaction"]
> [Sichern verschlüsselter virtueller Computer](./scripts/backup-powershell-sample-backup-encrypted-vm.md)
