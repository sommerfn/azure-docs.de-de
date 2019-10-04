---
title: Behandeln von Problemen beim Entsperren des Azure Data Box Disk-Datenträgers | Microsoft-Dokumentation
description: Beschreibt, wie Sie Probleme mit dem Azure Data Box-Datenträger beheben können.
services: databox
author: alkohli
ms.service: databox
ms.subservice: disk
ms.topic: article
ms.date: 06/14/2019
ms.author: alkohli
ms.openlocfilehash: 02cbf64261bbfbf50561e1b7466b46b27b688e0a
ms.sourcegitcommit: 72f1d1210980d2f75e490f879521bc73d76a17e1
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 06/14/2019
ms.locfileid: "67148114"
---
# <a name="troubleshoot-disk-unlocking-issues-in-azure-data-box-disk"></a>Behandeln von Problemen beim Entsperren des Azure Data Box Disk-Datenträgers

Dieser Artikel gilt für Microsoft Azure Data Box Disk. Er beschreibt die Workflows zum Beheben von Problemen bei Verwendung des Tools zum Entsperren. 


<!--## Query activity logs

Use the activity logs to find who unlocked and accessed the disks. Your Data Box Disk arrive on your premises in a locked state. You can use the device credentials available in the Azure portal for your order to unlock them.  

To figure out who accessed the **Device credentials** blade, you can query the Activity logs.  Any action that involves accessing **Device details > Credentials** blade is logged into the activity logs as `ListCredentials` action.

![Query Activity logs](media/data-box-logs/query-activity-log-1.png)-->


## <a name="data-box-disk-unlock-tool-errors"></a>Fehler des Data Box Disk-Tools zum Entsperren


| Fehlermeldung/Toolverhalten      | Empfehlungen                                                                             |
|-------------------------------------------------------------------------------------------------------------------------------------|------------------------------------------------------------------------------------------------------|
| Die aktuelle .NET Framework-Version wird nicht unterstützt. Die unterstützten Versionen sind 4.5 oder höher.<br><br>Das Tool wird mit einer Meldung beendet.  | .NET 4.5 ist nicht installiert. Installieren Sie .NET 4.5 oder höher auf dem Hostcomputer, der das Tool zum Entsperren des Data Box-Datenträgers ausführt.                                                                            |
| Volumes konnten nicht entsperrt oder überprüft werden. Wenden Sie sich an den Microsoft Support.  <br><br>Das Tool entsperrt bzw. überprüft keine gesperrten Laufwerke. | Das Tool konnte keines der entsperrten Laufwerke mit dem angegebenen Hauptschlüssel entsperren. Erkundigen Sie sich beim Support von Microsoft nach den weiteren Schritten.                                                |
| Folgende Volumes sind entsperrt und überprüft. <br>Volumelaufwerkbuchstaben: E:<br>Es konnten keine Volumes mit den folgenden Hauptschlüsseln entsperrt werden: werwerqomnf, qwerwerqwdfda <br><br>Das Tool entsperrt einige Laufwerke und zeigt die erfolgreichen und fehlerhaften Laufwerkbuchstaben an.| Teilweise erfolgreich. Einige der Laufwerke konnten mit dem angegebenen Hauptschlüssel nicht entsperrt werden. Erkundigen Sie sich beim Support von Microsoft nach den weiteren Schritten. |
| Es wurden keine gesperrten Laufwerke gefunden. Stellen Sie sicher, dass der von Microsoft erhaltene Datenträger ordnungsgemäß verbunden ist und einen gesperrten Zustand aufweist.          | Das Tool findet keine gesperrten Laufwerke. Die Laufwerke sind bereits entsperrt, oder sie werden nicht erkannt. Stellen Sie sicher, dass die Laufwerke verbunden und gesperrt sind.                                                           |
| Schwerwiegender Fehler: Ungültiger Parameter<br>Parametername: invalid_arg<br>SYNTAX:<br>DataBoxDiskUnlock /PassKeys:<Durch_Semicolons_getrennte_Liste_der_Hauptschlüssel><br><br>Beispiel: DataBoxDiskUnlock /PassKeys:Hauptschlüssel1;Hauptschlüssel2;Hauptschlüssel3<br>Beispiel: DataBoxDiskUnlock /SystemCheck<br>Beispiel: DataBoxDiskUnlock /Help<br><br>/PassKeys:       Abrufen dieses Schlüssels aus dem Azure Data Box Disk-Auftrag. Der Hauptschlüssel entsperrt Ihre Datenträger.<br>/Help:           Diese Option bietet Hilfe und Beispiele für die Cmdlet-Verwendung.<br>/SystemCheck:    Diese Option überprüft, ob Ihr System die Anforderungen zum Ausführen des Tools erfüllt.<br><br>Drücken Sie zum Beenden eine beliebige Taste. | Ungültige Parametereingabe. Der einzigen zulässigen Parameter sind /SystemCheck, /PassKey und /Help.|


## <a name="unlock-issues-for-disks-when-using-a-windows-client"></a>Probleme beim Entsperren von Datenträgern bei Verwendung eines Windows-Clients

In diesem Abschnitt werden einige der wichtigsten Probleme beschrieben, die bei der Bereitstellung von Data Box Disk auftreten können, wenn ein Windows-Client zum Kopieren von Daten verwendet wird.

### <a name="issue-could-not-unlock-drive-from-bitlocker"></a>Problem: Laufwerk von BitLocker konnte nicht entsperrt werden.
 
**Ursache** 

Sie haben das Kennwort im BitLocker-Dialogfeld verwendet und versuchen, den Datenträger über das BitLocker-Dialogfeld zum Entsperren von Laufwerken zu entsperren. Dies funktioniert nicht.

**Lösung**

Um die Data Box Disk-Datenträger zu entsperren, müssen Sie das Data Box Disk-Entsperrtool verwenden und das Kennwort aus dem Azure-Portal angeben. Weitere Informationen finden Sie unter [Tutorial: Entpacken, Verbinden und Entsperren von Azure Data Box Disk](data-box-disk-deploy-set-up.md#connect-to-disks-and-get-the-passkey).
 
### <a name="issue-could-not-unlock-or-verify-some-volumes-contact-microsoft-support"></a>Problem: Einige Volumes konnten nicht entsperrt oder überprüft werden. Wenden Sie sich an den Microsoft Support.
 
**Ursache**

Möglicherweise ist der folgende Fehler im Fehlerprotokoll aufgeführt, und Sie können einige Volumes nicht entsperren oder überprüfen.

`Exception System.IO.FileNotFoundException: Could not load file or assembly 'Microsoft.Management.Infrastructure, Version=1.0.0.0, Culture=neutral, PublicKeyToken=31bf3856ad364e35' or one of its dependencies. The system cannot find the file specified.`
 
Dies deutet darauf hin, dass wahrscheinlich die geeignete Version von Windows PowerShell auf Ihrem Windows-Client fehlt.

**Lösung**

Sie können [Windows PowerShell v5.0](https://www.microsoft.com/download/details.aspx?id=54616) installieren und den Vorgang wiederholen.
 
Wenn es Ihnen weiterhin nicht möglich ist, die Volumes zu entsperren, kopieren Sie die Protokolle aus dem Ordner, in dem sich das Data Box Disk-Tool zum Entsperren befindet, und [wenden Sie sich an den Microsoft-Support](data-box-disk-contact-microsoft-support.md).

## <a name="next-steps"></a>Nächste Schritte

- Informieren Sie sich über das [Behandeln von Problemen bei der Überprüfung](data-box-disk-troubleshoot.md).
