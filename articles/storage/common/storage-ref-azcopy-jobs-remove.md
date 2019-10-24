---
title: azcopy jobs remove | Microsoft-Dokumentation
description: Dieser Artikel enthält Referenzinformationen zum Befehl „azcopy jobs remove“.
author: normesta
ms.service: storage
ms.topic: reference
ms.date: 10/16/2019
ms.author: normesta
ms.subservice: common
ms.reviewer: zezha-msft
ms.openlocfilehash: 82c399580322334e67c0c9c2b88d1edf6f175e0c
ms.sourcegitcommit: 12de9c927bc63868168056c39ccaa16d44cdc646
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 10/17/2019
ms.locfileid: "72518122"
---
# <a name="azcopy-jobs-remove"></a>azcopy jobs remove

Entfernt alle Dateien, die der angegebenen Auftrags-ID zugeordnet sind.

> [!NOTE] 
> Sie können den Speicherort von Protokoll- und Plandateien anpassen. Weitere Informationen finden Sie unter dem Befehl [azcopy env](storage-ref-azcopy-env.md).

```
azcopy jobs remove [jobID] [flags]
```

## <a name="examples"></a>Beispiele

```
  azcopy jobs rm e52247de-0323-b14d-4cc8-76e0be2e2d44
```

## <a name="options"></a>Optionen

**-h, --help**                Hilfe zu „remove“

## <a name="options-inherited-from-parent-commands"></a>Von übergeordneten Befehlen geerbte Optionen

**--cap-mbps uint32**      Begrenzt die Übertragungsrate (in Megabits pro Sekunde). Der Schritt-für-Schritt-Durchsatz kann von der Obergrenze geringfügig abweichen. Wenn diese Option auf „null“ festgelegt oder weggelassen wird, ist der Durchsatz nicht begrenzt.

**--output-type** string   Das Format der Befehlsausgabe. Folgende Optionen sind verfügbar: „text“ und „json“. Der Standardwert lautet „text“. (Standardwert: „text“)

## <a name="see-also"></a>Weitere Informationen

- [azcopy jobs](storage-ref-azcopy-jobs.md)
