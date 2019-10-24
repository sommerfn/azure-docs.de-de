---
title: azcopy jobs clean | Microsoft-Dokumentation
description: Dieser Artikel enthält Referenzinformationen zum Befehl „azcopy jobs clean“.
author: normesta
ms.service: storage
ms.topic: reference
ms.date: 10/16/2019
ms.author: normesta
ms.subservice: common
ms.reviewer: zezha-msft
ms.openlocfilehash: 31529155ee44b2bcfad90e8634053403dfe8fc8c
ms.sourcegitcommit: 12de9c927bc63868168056c39ccaa16d44cdc646
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 10/17/2019
ms.locfileid: "72518190"
---
# <a name="azcopy-jobs-clean"></a>azcopy jobs clean

Entfernen aller Protokoll- und Plandateien für alle Aufträge

```
azcopy jobs clean [flags]
```

## <a name="examples"></a>Beispiele

```
  azcopy jobs clean --with-status=completed
```

## <a name="options"></a>Optionen

**-h, --help**                Hilfe zu „clean“

**--with-status** string   Entfernt nur die Aufträge mit dem jeweiligen Status. Verfügbare Werte: „Canceled“, „Completed“, „Failed“, „InProgress“, „All“ (Standardwert: „All“)

## <a name="options-inherited-from-parent-commands"></a>Von übergeordneten Befehlen geerbte Optionen

**--cap-mbps uint32**      Begrenzt die Übertragungsrate (in Megabits pro Sekunde). Der Schritt-für-Schritt-Durchsatz kann von der Obergrenze geringfügig abweichen. Wenn diese Option auf „null“ festgelegt oder weggelassen wird, ist der Durchsatz nicht begrenzt.

**--output-type** string   Das Format der Befehlsausgabe. Folgende Optionen sind verfügbar: „text“ und „json“. Der Standardwert lautet „text“. (Standardwert: „text“)

## <a name="see-also"></a>Weitere Informationen

- [azcopy jobs](storage-ref-azcopy-jobs.md)
