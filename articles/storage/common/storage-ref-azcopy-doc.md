---
title: azcopy doc | Microsoft-Dokumentation
description: Dieser Artikel enthält Referenzinformationen zum Befehl „azcopy doc“.
author: normesta
ms.service: storage
ms.topic: reference
ms.date: 08/26/2019
ms.author: normesta
ms.subservice: common
ms.reviewer: zezha-msft
ms.openlocfilehash: d1f57ae40b47c1d910ba20ae8a8f19cdc6908d6b
ms.sourcegitcommit: 532335f703ac7f6e1d2cc1b155c69fc258816ede
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 08/30/2019
ms.locfileid: "70196836"
---
# <a name="azcopy-doc"></a>azcopy doc

Generiert Dokumentation zum Tool im Markdown-Format.

## <a name="synopsis"></a>Zusammenfassung

Generiert Dokumentation zum Tool im Markdown-Format und speichert sie am angegebenen Speicherort.

Die Dateien werden standardmäßig im aktuellen Verzeichnis in einem Ordner namens „doc“ gespeichert.

```azcopy
azcopy doc [flags]
```

## <a name="options"></a>Optionen

|Option|BESCHREIBUNG|
|--|--|
|-h, --help|Zeigt Hilfeinhalt zum Befehl „doc“.|

## <a name="options-inherited-from-parent-commands"></a>Von übergeordneten Befehlen geerbte Optionen

|Option|BESCHREIBUNG|
|---|---|
|–cap-mbps uint32|Begrenzt die Übertragungsrate, in Megabit pro Sekunde. Der Schritt-für-Schritt-Durchsatz kann von der Obergrenze geringfügig abweichen. Wenn diese Option auf „null“ festgelegt oder weggelassen wird, ist der Durchsatz nicht begrenzt.|
|–output-type string|Format der Befehlsausgabe. Folgende Optionen sind verfügbar: „text“, „json“. Der Standardwert lautet „text“.|

## <a name="see-also"></a>Weitere Informationen

- [azcopy](storage-ref-azcopy.md)
