---
title: Behandeln von Problemen beim Kopieren von Daten in Azure Data Box Disk | Microsoft-Dokumentation
description: Beschreibt, wie Sie Probleme beim Kopieren von Daten in Azure Data Box Disk mithilfe von Protokollen beheben können.
services: databox
author: alkohli
ms.service: databox
ms.subservice: disk
ms.topic: article
ms.date: 06/13/2019
ms.author: alkohli
ms.openlocfilehash: 760f5c6c929aa082993683d7a466a71c6484289a
ms.sourcegitcommit: 72f1d1210980d2f75e490f879521bc73d76a17e1
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 06/14/2019
ms.locfileid: "67148134"
---
# <a name="troubleshoot-data-copy-issues-in-azure-data-box-disk"></a>Behandeln von Problemen beim Kopieren von Daten in Azure Data Box Disk

Dieser Artikel gilt für Microsoft Azure Data Box Disk. Er beschreibt das Beheben von Problemen, die beim Kopieren von Daten auf Datenträger auftreten können. Der Artikel behandelt auch Probleme beim Verwenden des Tools zum Aufteilen/Kopieren.


## <a name="data-copy-issues-when-using-a-linux-system"></a>Probleme beim Kopieren von Daten bei Verwendung eines Linux-Systems

In diesem Abschnitt werden einige der wichtigsten Probleme beschrieben, die bei Verwendung eines Linux-Clients zum Kopieren von Daten auf Datenträger auftreten können.

### <a name="issue-drive-getting-mounted-as-read-only"></a>Problem: Das Laufwerk wird schreibgeschützt eingebunden.
 
**Ursache** 

Dies kann auf ein fehlerhaftes Dateisystem zurückzuführen sein.

Das erneute Einbinden eines Laufwerks mit Lese-/Schreibzugriff funktioniert nicht mit Data Box Disk-Datenträgern. Dieses Szenario wird mit Laufwerken, die mit dislocker entschlüsselt werden, nicht unterstützt. Möglicherweise haben Sie das Gerät mithilfe des folgenden Befehls erfolgreich erneut eingebunden:

    `# mount -o remount, rw /mnt/DataBoxDisk/mountVol1`

Das erneute Einbinden war zwar erfolgreich, die Daten werden jedoch nicht beibehalten.

**Lösung**

Führen Sie die folgenden Schritte auf Ihrem Linux-System durch:

1. Installieren Sie das `ntfsprogs`-Paket für das Hilfsprogramm ntfsfix.
2. Heben Sie die Einbindung der Bereitstellungspunkte für das Laufwerk mit dem Tool zum Entsperren auf. Die Anzahl der Bereitstellungspunkte variiert bei Laufwerken.

    ```
    unmount /mnt/DataBoxDisk/mountVol1
    ```

3. Führen Sie `ntfsfix` im entsprechenden Pfad aus. Die hervorgehobene Anzahl sollte mit Schritt 2 übereinstimmen.

    ```
    ntfsfix /mnt/DataBoxDisk/bitlockerVol1/dislocker-file
    ```

4. Führen Sie den folgenden Befehl aus, um die Metadaten für den Ruhezustand zu entfernen, die möglicherweise Ursache des Bereitstellungsproblems sind.

    ```
    ntfs-3g -o remove_hiberfile /mnt/DataBoxDisk/bitlockerVol1/dislocker-file /mnt/DataBoxDisk/mountVol1
    ```

5. Heben Sie die Bereitstellung auf.

    ```
    ./DataBoxDiskUnlock_x86_64 /unmount
    ```

6. Führen Sie die Entsperrung und die Bereitstellung durch.
7. Testen Sie den Bereitstellungspunkt durch das Schreiben einer Datei.
8. Heben Sie die Bereitstellung auf und führen Sie sie anschließend erneut durch, um die Dateipersistenz zu überprüfen.
9. Fahren Sie mit der Datenkopie fort.
 
### <a name="issue-error-with-data-not-persisting-after-copy"></a>Problem: Fehler mit Daten, die nach dem Kopieren nicht beibehalten werden
 
**Ursache** 

Wenn Sie feststellen, dass nach dem Aufheben der Einbindung auf dem Laufwerk keine Daten mehr vorhanden sind (obwohl Daten darauf kopiert wurden), ist es möglich, dass Sie das Laufwerk mit Lese-/ Schreibzugriff eingebunden haben, nachdem es schreibgeschützt eingebunden wurde.

**Lösung**
 
Wenn dies der Fall ist, finden Sie weitere Informationen in der Lösung für Probleme durch [schreibgeschützt eingebundene Laufwerke](#issue-drive-getting-mounted-as-read-only).

Wenn dies nicht der Fall war, kopieren Sie die Protokolle aus dem Ordner, in dem sich das Data Box Disk-Tool zum Entsperren befindet, und [wenden Sie sich an den Microsoft-Support](data-box-disk-contact-microsoft-support.md).


## <a name="data-box-disk-split-copy-tool-errors"></a>Fehler des Data Box Disk-Tools zum Aufteilen/Kopieren

Die Probleme, die auftreten können, wenn Sie ein Tool zum Aufteilen/Kopieren für das Aufteilen der Daten auf mehrere Datenträger verwenden, sind in der folgenden Tabelle zusammengefasst.

|Fehlermeldung/Warnungen |Empfehlungen |
|---------|---------|
|[Info] Retrieving BitLocker password for volume: m <br>[Error] Exception caught while retrieving BitLocker key for volume m:<br> Sequence contains no elements. ([Information] BitLocker-Kennwort für folgendes Volume wird abgerufen: M.[Fehler] Ausnahme beim Abrufen des BitLocker-Schlüssels für das Volume M abgefangen:Die Sequenz enthält keine Elemente.)|Dieser Fehler tritt auf, wenn das Data Box Disk-Ziel offline ist. <br> Verwenden Sie das Tool `diskmgmt.msc`, um Datenträger online zu schalten.|
|[Error] Exception thrown: WMI operation failed:<br> Method=UnlockWithNumericalPassword, ReturnValue=2150694965, <br>Win32Message=The format of the recovery password provided is invalid. <br>BitLocker recovery passwords are 48 digits. <br>Verify that the recovery password is in the correct format and then try again.([Fehler] Eine Ausnahme wurde ausgelöst: Fehler beim WMI-Vorgang: Methode: UnlockWithNumericalPassword, Rückgabewert: 2150694965, Win32-Meldung: Das Format des angegebenen Wiederherstellungskennworts ist ungültig. BitLocker-Wiederherstellungskennwörter müssen 48-stellig sein. Vergewissern Sie sich, dass das Wiederherstellungskennwort das richtige Format hat, und wiederholen Sie den Vorgang.)|Entsperren Sie die Datenträger zunächst mit dem Data Box Disk-Tool zum Entsperren, und führen Sie den Befehl anschließend erneut aus. Weitere Informationen finden Sie unter: <li> [Entsperren von Datenträgern auf Windows-Client](data-box-disk-deploy-set-up.md#unlock-disks-on-windows-client) </li><li> [Entsperren von Datenträgern auf Linux-Client](data-box-disk-deploy-set-up.md#unlock-disks-on-linux-client) </li>|
|[Error] Exception thrown: A DriveManifest.xml file exists on the target drive. <br> This indicates the target drive may have been prepared with a different journal file. <br>To add more data to the same drive, use the previous journal file. To delete existing data and reuse target drive for a new import job, delete the *DriveManifest.xml* on the drive. Rerun this command with a new journal file. ([Fehler] Eine Ausnahme wurde ausgelöst: Auf dem Ziellaufwerk befindet sich eine Datei vom Typ „DriveManifest.xml“. Das Ziellaufwerk wurde möglicherweise mit einer anderen Journaldatei vorbereitet. Wenn Sie dem gleichen Laufwerk weitere Daten hinzufügen möchten, verwenden Sie die vorherige Journaldatei. Wenn Sie die vorhandenen Daten löschen und das Ziellaufwerk für einen neuen Importauftrag verwenden möchten, löschen Sie die Datei „DriveManifest.xml“ auf dem Laufwerk, und führen Sie diesen Befehl mit einer neuen Journaldatei aus.)| Dieser Fehler tritt auf, wenn Sie versuchen, die gleiche Gruppe von Laufwerken für mehrere Importsitzungen zu verwenden. <br> Verwenden Sie pro Aufteilungs-/Kopiersitzung immer nur eine Gruppe von Laufwerken.|
|[Error] Exception thrown: CopySessionId importdata-sept-test-1 refers to a previous copy session and cannot be reused for a new copy session. ([Fehler] Eine Ausnahme wurde ausgelöst: „CopySessionId importdata-sept-test-1“ bezieht sich auf eine vorherige Kopiersitzung und kann nicht für diese neue Kopiersitzung wiederverwendet werden.)|Dieser Fehler tritt auf, wenn Sie versuchen, den Auftragsnamen eines bereits erfolgreich abgeschlossenen Auftrags für einen neuen Auftrag zu verwenden.<br> Weisen Sie Ihrem neuen Auftrag einen eindeutigen Namen zu.|
|[Info] Destination file or directory name exceeds the NTFS length limit. ([Information] Der Name der Zieldatei oder des Zielverzeichnisses übersteigt die NTFS-Längenbeschränkung.) |Diese Meldung wird zurückgegeben, wenn die Zieldatei aufgrund eines langen Dateipfads umbenannt wurde.<br> Ändern Sie die Dispositionsoption in der Datei `config.json`, um dieses Verhalten zu steuern.|
|[Error] Exception thrown: Bad JSON escape sequence. ([Fehler] Eine Ausnahme wurde ausgelöst: fehlerhafte JSON-Escapesequenz.) |Diese Meldung wird zurückgegeben, wenn das Format von „config.json“ ungültig ist. <br> Überprüfen Sie die Datei `config.json` vor dem Speichern mithilfe von [JSONLint](https://jsonlint.com/).|


## <a name="next-steps"></a>Nächste Schritte

- Informieren Sie sich über das [Behandeln von Problemen mit dem Überprüfungstool](data-box-disk-troubleshoot.md).
