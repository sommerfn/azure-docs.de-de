---
title: Wiederherstellung von Speicherkontocontainern
description: Wiederherstellung von Speicherkontocontainern
author: v-miegge
ms.topic: troubleshooting
ms.author: kartup
ms.date: 10/30/2019
ms.service: storage
ms.openlocfilehash: f095bdfe7bbb5777a2ad2aabb3bda92d0974457d
ms.sourcegitcommit: 609d4bdb0467fd0af40e14a86eb40b9d03669ea1
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 11/06/2019
ms.locfileid: "73693799"
---
# <a name="storage-account-container-recovery"></a>Wiederherstellung von Speicherkontocontainern

Azure Storage bietet Datenresilienz durch automatisierte Replikate. Allerdings verhindert dies nicht die Beschädigung von Daten durch Anwendungscode oder Benutzer, ob versehentlich oder mit böser Absicht. Um Datengenauigkeit bei Anwendungs- oder Benutzerfehlern zu gewährleisten, sind weitergehende Methoden erforderlich – beispielsweise das Kopieren der Daten an einen sekundären Speicherort mit einem Überwachungsprotokoll.

## <a name="checking-azure-storage-account-type"></a>Überprüfen des Azure Storage-Kontotyps

1. Navigieren Sie zum [Azure-Portal](https://portal.azure.com/).

2. Suchen Sie nach Ihrem Speicherkonto.

3. Überprüfen Sie im Abschnitt **Übersicht** den Wert für **Replikation**.

   ![Image](media/storage-account-container-recovery/1.png)

4. Wenn der Replikationstyp **GRS/RA-GRS** ist, ist die Wiederherstellung des Kontocontainers ohne Garantie möglich. Bei allen anderen Replikationstypen ist dies nicht möglich.

5. Sammeln Sie die folgenden Informationen und reichen Sie eine Supportanfrage an den Microsoft-Support ein.

   * Speicherkontoname:
   * Containername:
   * Löschzeitpunkt:

   In der folgenden Tabelle finden Sie eine Übersicht über den Umfang der Speicherkontocontainerwiederherstellung, abhängig von der Replikationsstrategie.

   |Inhaltstyp|LRS|ZRS|GRS|RA - GRS| 
   |---|---|---|---|---|
   |Speichercontainer|Nein|Nein|Ja|Ja| 

   * Wir können versuchen, den Speicherkontocontainer wiederherzustellen, aber ohne jegliche Garantie. 

## <a name="things-not-to-do-for-recover-to-be-successful"></a>Aktionen, die für eine erfolgreiche Wiederherstellung zu vermeiden sind

* Erstellen Sie den Container nicht neu (mit dem gleichen Namen).  
* Wenn Sie den Container bereits neu erstellt haben, müssen Sie den Container löschen, bevor Sie eine Supportanfrage für die Wiederherstellung einreichen.

## <a name="steps-to-prevent-this-in-the-future"></a>Schritte, um dies in der Zukunft zu verhindern

1. Um dies in Zukunft zu vermeiden, wird die Verwendung der Funktion [Vorläufiges löschen](https://docs.microsoft.com/azure/storage/blobs/storage-blob-soft-delete) empfohlen.

2. Außerdem wird die Funktion [Momentaufnahme](https://docs.microsoft.com/rest/api/storageservices/Creating-a-Snapshot-of-a-Blob) empfohlen.
 
## <a name="next-steps"></a>Nächste Schritte

Nachstehend finden Sie zwei Beispielcodes für die Funktion:

  * [Erstellen und Verwalten einer Blobmomentaufnahme in .NET](https://docs.microsoft.com/azure/storage/storage-blob-snapshots)
  * [Verwenden von Blobmomentaufnahmen mit PowerShell](https://blogs.msdn.microsoft.com/cie/2016/05/17/using-blob-snapshots-with-powershell/)
  

