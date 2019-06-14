---
author: alkohli
ms.service: databox
ms.topic: include
ms.date: 02/04/2019
ms.author: alkohli
ms.openlocfilehash: 563849d3875ed0156d81770f58340633d90d515b
ms.sourcegitcommit: d4dfbc34a1f03488e1b7bc5e711a11b72c717ada
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 06/13/2019
ms.locfileid: "66161270"
---
Hier sind die Größen der Azure-Objekte, die geschrieben werden können. Stellen Sie sicher, dass alle Dateien, die hochgeladen werden, diesen Einschränkungen entsprechen.

| Azure-Objekttyp | Uploadlimit                                             |
|-------------------|-----------------------------------------------------------|
| Blockblob        | ~ 4,75 TB                                                 |
| Seitenblob         | 1 TB <br> Jede Datei, die im Seitenblobformat hochgeladen wird, muss einem ganzzahligen Vielfachen von 512 Bytes entsprechen. Andernfalls tritt beim Hochladen ein Fehler auf. <br> VHD- und VHDX-Dateien entsprechen einem ganzzahligen Vielfachen von 512 Bytes. |
| Azure Files         | 1 TB <br> Jede Datei, die im Seitenblobformat hochgeladen wird, muss einem ganzzahligen Vielfachen von 512 Bytes entsprechen. Andernfalls tritt beim Hochladen ein Fehler auf. <br> VHD- und VHDX-Dateien entsprechen einem ganzzahligen Vielfachen von 512 Bytes. |

> [!IMPORTANT]
> Die Erstellung von Dateien (unabhängig vom Speichertyp) ist bis zu einer Größe von 5 TB zulässig. Wenn Sie jedoch eine Datei erstellen, deren Größe das in der Tabelle oben definierte Uploadlimit überschreitet, wird die Datei nicht hochgeladen. Sie müssen die Datei dann manuell löschen, um den Speicherplatz freizugeben.