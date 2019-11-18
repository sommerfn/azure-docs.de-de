---
author: alkohli
ms.service: databox
ms.subservice: heavy
ms.topic: include
ms.date: 11/05/2019
ms.author: alkohli
ms.openlocfilehash: 16647b6a13e64073ab570d36a8a380d0e36bd855
ms.sourcegitcommit: 359930a9387dd3d15d39abd97ad2b8cb69b8c18b
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 11/06/2019
ms.locfileid: "73799844"
---
- Kopieren Sie Dateien nicht direkt in eine der vorab erstellten Freigaben. Sie müssen zunächst einen Ordner unter der Freigabe erstellen und dann Dateien in diesen Ordner kopieren.
- Ein Ordner unter *StorageAccount_BlockBlob* und *StorageAccount_PageBlob* ist ein Container. Container werden beispielsweise als *StorageAccount_BlockBlob/container* und *StorageAccount_PageBlob/container* erstellt.
- Jeder Ordner, der direkt unter *StorageAccount_AzureFiles* erstellt wird, wird in eine Azure-Dateifreigabe umgewandelt.
- Wenn Sie bereits über ein Azure-Objekt (z.B. Blob oder Datei) in der Cloud verfügen, das den gleichen Namen hat wie das Objekt, das kopiert wird, überschreibt Data Box die Datei in der Cloud.
- Jede Datei, die in die Freigaben *StorageAccount_BlockBlob* und *StorageAccount_PageBlob* geschrieben wird, wird als Blockblob bzw. Seitenblob hochgeladen.
- Azure Blob Storage unterstützt keine Verzeichnisse. Wenn Sie einen Ordner unter dem Ordner *StorageAccount_BlockBlob* erstellen, werden im Blobnamen virtuelle Ordner erstellt. Für Azure Files wird die tatsächliche Verzeichnisstruktur beibehalten.
- Eine unter den Ordnern *StorageAccount_BlockBlob* und *StorageAccount_PageBlob* erstellte leere Verzeichnishierarchie (ohne jegliche Dateien) wird nicht hochgeladen.
- Wenn beim Hochladen von Daten in Azure Fehler auftreten, wird im Zielspeicherkonto ein Fehlerprotokoll erstellt. Der Pfad zu diesem Fehlerprotokoll ist verfügbar, sobald das Hochladen abgeschlossen ist, und Sie können das Protokoll überprüfen, um Korrekturmaßnahmen zu ergreifen. Löschen Sie keine Daten aus der Quelle, ohne die hochgeladenen Daten zu überprüfen.
- Dateimetadaten und NTFS-Berechtigungen werden beim Hochladen der Daten in Azure Files nicht beibehalten. Beispielsweise wird das Attribut *Letzte Änderung* der Dateien beim Kopieren der Daten nicht beibehalten.
