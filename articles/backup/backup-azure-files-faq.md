---
title: Häufig gestellte Fragen zum Sichern von Azure Files
description: Dieser Artikel enthält ausführliche Informationen dazu, wie Sie Ihre Azure-Dateifreigaben schützen.
author: dcurwin
ms.author: dacurwin
ms.date: 01/31/2019
ms.topic: tutorial
ms.service: backup
manager: carmonm
ms.openlocfilehash: e3f94badb3af2e3f26a857f27f0b68a78338db92
ms.sourcegitcommit: c72ddb56b5657b2adeb3c4608c3d4c56e3421f2c
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 07/24/2019
ms.locfileid: "68466309"
---
# <a name="questions-about-backing-up-azure-files"></a>Fragen zum Sichern von Azure Files
In diesem Artikel werden allgemeine Fragen zum Sichern von Azure Files beantwortet. Einige Antworten enthalten Links zu Artikeln mit umfassenderen Informationen. Außerdem können Sie Fragen zum Azure Backup-Dienst im [Diskussionsforum](https://social.msdn.microsoft.com/forums/azure/home?forum=windowsazureonlinebackup)stellen.

Verwenden Sie die Links auf der rechten Seite unter **In diesem Artikel**, um sich einen Überblick über die Abschnitte dieses Artikels zu verschaffen.

## <a name="configuring-the-backup-job-for-azure-files"></a>Konfigurieren des Sicherungsauftrags für Azure Files

### <a name="why-cant-i-see-some-of-my-storage-accounts-i-want-to-protect-that-contain-valid-azure-file-shares-br"></a>Warum werden einige Speicherkonten nicht angezeigt, die geschützt werden sollen und gültige Azure-Dateifreigaben enthalten? <br/>
Während der Vorschauphase unterstützt die Sicherung für Azure-Dateifreigaben nicht alle Arten von Speicherkonten. Die Liste mit den unterstützten Speicherkonten finden Sie [hier](troubleshoot-azure-files.md#limitations-for-azure-file-share-backup-during-preview). Das gesuchte Speicherkonto kann auch bereits geschützt oder in einem anderen Tresor registriert sein. [Heben Sie die Registrierung beim Tresor auf](troubleshoot-azure-files.md#configuring-backup), um das Speicherkonto in anderen Tresoren für den Schutz zu ermitteln.

### <a name="why-cant-i-see-some-of-my-azure-file-shares-in-the-storage-account-when-im-trying-to-configure-backup-br"></a>Warum werden beim Konfigurieren der Sicherung einige Azure-Dateifreigaben im Speicherkonto nicht angezeigt? <br/>
Überprüfen Sie, ob die Azure-Dateifreigabe bereits in demselben Recovery Services-Tresor geschützt wird oder vor Kurzem gelöscht wurde.

### <a name="can-i-protect-file-shares-connected-to-a-sync-group-in-azure-files-sync-br"></a>Kann ich Dateifreigaben schützen, die mit einer Synchronisierungsgruppe in Azure Files Sync verbunden sind? <br/>
Ja. Die Schutzfunktion für Azure-Dateifreigaben, die mit Synchronisierungsgruppen verbunden sind, ist aktiviert und Teil einer öffentlichen Vorschauversion.

### <a name="when-trying-to-back-up-file-shares-i-clicked-on-a-storage-account-for-discovering-the-file-shares-in-it-however-i-did-not-protect-them-how-do-i-protect-these-file-shares-with-any-other-vault"></a>Beim Sichern von Dateifreigaben habe ich auf ein Speicherkonto geklickt, um die darin enthaltenen Dateifreigaben zu ermitteln. Ich habe sie jedoch nicht geschützt. Wie schütze ich diese Dateifreigaben mit einem anderen Tresor?
Wenn Sie beim Sichern ein Speicherkonto auswählen, um die darin enthaltenen Dateifreigaben zu ermitteln, wird das Speicherkonto bei dem Tresor registriert, über den der Vorgang ausgeführt wird. Falls Sie die Dateifreigaben bei einem anderen Tresor registrieren möchten, [heben Sie die Registrierung des ausgewählten Speicherkontos bei diesem Tresor auf](troubleshoot-azure-files.md#configuring-backup).

### <a name="can-i-change-the-vault-to-which-i-backup-my-file-shares"></a>Kann ich den Tresor ändern, in dem ich meine Dateifreigaben sichere?
Ja. Sie müssen jedoch im verknüpften Tresor den [Schutz beenden](backup-azure-files.md#stop-protecting-an-azure-file-share), [die Registrierung dieses Speicherkontos aufheben](troubleshoot-azure-files.md#configuring-backup) und es dann über einen anderen Tresor schützen.

### <a name="in-which-geos-can-i-back-up-azure-file-shares-br"></a>An welchen geografischen Standorten kann ich Azure-Dateifreigaben sichern? <br/>
Die Sicherung für Azure-Dateifreigaben befindet sich derzeit in der Vorschauphase und ist nur an folgenden geografischen Standorten verfügbar:
- Australien, Osten (AE)
- Australien, Südosten (ASE)
- Brasilien, Süden (BRS)
- Kanada, Mitte (CNC)
- Kanada, Osten (CE)
- USA, Mitte (CUS)
- Asien, Osten (EA)
- USA, Osten (EUS)
- USA, Osten 2 (EUS2)
- Japan, Osten (JPE)
- Japan, Westen (JPW)
- Indien, Mitte (INC)
- Indien, Süden (INS)
- Südkorea, Mitte (KRC)
- Korea, Süden (KRS)
- USA, Norden-Mitte (NCUS)
- Europa, Norden (NE)
- USA, Süden-Mitte (SCUS)
- Asien, Südosten (SEA)
- Vereinigtes Königreich, Süden (UKS)
- Vereinigtes Königreich, Westen (UKW)
- Europa, Westen (WE)
- USA, Westen (WUS)
- USA, Westen-Mitte (WCUS)
- USA, Westen 2 (WUS 2)
- US Gov Arizona (UGA)
- US Gov Texas (UGT)
- US Gov Virginia (UGV)

Schreiben Sie an [AskAzureBackupTeam@microsoft.com](email:askazurebackupteam@microsoft.com), wenn Sie die Version an einem bestimmten geografischen Standort verwenden müssen, der oben nicht aufgeführt ist.

### <a name="how-many-azure-file-shares-can-i-protect-in-a-vaultbr"></a>Wie viele Azure-Dateifreigaben kann ich in einem Tresor schützen?<br/>
Während der Vorschauphase können Sie Azure-Dateifreigaben aus bis zu 50 Speicherkonten pro Tresor schützen. Sie können auch bis zu 200 Azure-Dateifreigaben in einem einzelnen Tresor schützen.

### <a name="can-i-protect-two-different-file-shares-from-the-same-storage-account-to-different-vaults"></a>Kann ich zwei verschiedene Dateifreigaben im gleichen Speicherkonto in verschiedenen Tresoren schützen?
Nein. Alle Dateifreigaben in einem Speicherkonto können nur durch denselben Tresor geschützt werden.

## <a name="backup"></a>Backup

### <a name="how-many-on-demand-backups-can-i-take-per-file-share-br"></a>Wie viele bedarfsgesteuerte Sicherungen kann ich pro Dateifreigabe erstellen? <br/>
Für eine Dateifreigabe können jeweils bis zu 200 Momentaufnahmen vorhanden sein. In die Berechnung dieses Grenzwerts werden auch Momentaufnahmen einbezogen, die mit Azure Backup erstellt werden. Dies wird durch Ihre Richtlinie definiert. Falls für Ihre Sicherungen nach dem Erreichen des Grenzwerts Fehler auftreten, sollten Sie bedarfsgesteuerte Wiederherstellungspunkte löschen, damit die Erstellung von Sicherungen wieder erfolgreich ist.

### <a name="after-enabling-virtual-networks-on-my-storage-account-the-backup-of-file-shares-in-the-account-started-failing-why"></a>Nach dem Aktivieren von virtuellen Netzwerken in meinem Speicherkonto ist beim Sichern der Dateifreigaben im Konto ein Fehler aufgetreten. Warum?
Für die Sicherung für Azure-Dateifreigaben werden keine Speicherkonten unterstützt, für die virtuelle Netzwerke aktiviert sind. Deaktivieren Sie virtuelle Netzwerke in Speicherkonten, um erfolgreiche Sicherungen zu ermöglichen.

## <a name="restore"></a>Restore

### <a name="can-i-recover-from-a-deleted-azure-file-share-br"></a>Kann die Wiederherstellung auf der Grundlage einer gelöschten Azure-Dateifreigabe erfolgen? <br/>
Beim Löschen einer Azure-Dateifreigabe wird die Liste mit den Sicherungen angezeigt, die gelöscht werden, und Sie werden zum Bestätigen aufgefordert. Eine gelöschte Azure-Dateifreigabe kann nicht wiederhergestellt werden.

### <a name="can-i-restore-from-backups-if-i-stopped-protection-on-an-azure-file-share-br"></a>Kann ich Sicherungen wiederherstellen, wenn ich den Schutz für eine Azure-Dateifreigabe beendet habe? <br/>
Ja. Wenn Sie beim Beenden des Schutzes die Option **Sicherungsdaten beibehalten** ausgewählt haben, können Sie alle vorhandenen Wiederherstellungspunkte wiederherstellen.

### <a name="what-happens-if-i-cancel-an-ongoing-restore-job"></a>Was geschieht, wenn ich einen laufenden Wiederherstellungsauftrag abbreche?
Wenn ein laufender Wiederherstellungsauftrag abgebrochen wird, wird der Wiederherstellungsvorgang beendet, und alle vor dem Abbruch wiederhergestellten Dateien verbleiben ohne Rollbacks am konfigurierten Ziel (ursprünglicher oder alternativer Speicherort). 


## <a name="manage-backup"></a>Verwalten von Sicherungen

### <a name="can-i-use-powershell-to-configuremanagerestore-backups-of-azure-file-shares-br"></a>Kann ich PowerShell zum Konfigurieren/Verwalten/Wiederherstellen von Sicherungen von Azure-Dateifreigaben verwenden? <br/>
Ja. Informationen hierzu finden Sie in der ausführlichen Dokumentation unter [diesem Link](backup-azure-afs-automation.md).

### <a name="can-i-access-the-snapshots-taken-by-azure-backups-and-mount-it-br"></a>Kann ich auf die von Azure Backup erstellten Momentaufnahmen zugreifen und sie einbinden? <br/>
Auf alle von Azure Backup erstellten Momentaufnahmen kann im Portal, über PowerShell oder mithilfe der CLI über die Funktion zum Anzeigen von Momentaufnahmen zugegriffen werden. Weitere Informationen zu Azure Files-Freigabemomentaufnahmen finden Sie unter [Übersicht über Freigabemomentaufnahmen für Azure Files (Vorschauversion)](../storage/files/storage-snapshots-files.md).

### <a name="what-is-the-maximum-retention-i-can-configure-for-backups-br"></a>Was ist die maximale Aufbewahrungsdauer, die ich für Sicherungen konfigurieren kann? <br/>
Bei der Sicherung von Azure-Dateifreigaben können Sie Richtlinien mit einem Aufbewahrungszeitraum von bis zu 180 Tagen konfigurieren. Mit der [Option für die bedarfsgesteuerte Sicherung in PowerShell](backup-azure-afs-automation.md#trigger-an-on-demand-backup) können Sie einen Wiederherstellungspunkt jedoch sogar bis zu zehn Jahre aufbewahren.

### <a name="what-happens-when-i-change-the-backup-policy-for-an-azure-file-share-br"></a>Was passiert, wenn ich die Sicherungsrichtlinie für eine Azure-Dateifreigabe ändere? <br/>
Wenn eine neue Richtlinie auf Dateifreigaben angewendet wird, werden der Zeitplan und die Aufbewahrung der neuen Richtlinie beachtet. Bei einer Ausweitung der Aufbewahrung werden bereits vorhandene Wiederherstellungspunkte markiert, um sie gemäß der neuen Richtlinie aufzubewahren. Bei einer Verkürzung der Aufbewahrung werden sie im Rahmen der nächsten Bereinigung gelöscht.

## <a name="see-also"></a>Weitere Informationen
Diese Informationen beziehen sich nur auf das Sichern von Azure Files. Wenn Sie weitere Informationen zu anderen Bereichen von Azure Backup benötigen, sehen Sie sich die folgenden Artikel mit häufig gestellten Fragen zu Backup an:
-  [Häufig gestellte Fragen zum Recovery Services-Tresor](backup-azure-backup-faq.md)
-  [Häufig gestellte Fragen zur Azure-VM-Sicherung](backup-azure-vm-backup-faq.md)
-  [Häufig gestellte Fragen zur Sicherung von Dateien/Ordnern mit dem Azure Backup-Agent](backup-azure-file-folder-backup-faq.md)