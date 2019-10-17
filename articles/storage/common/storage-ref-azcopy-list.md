---
title: azcopy list | Microsoft-Dokumentation
description: Dieser Artikel enthält Referenzinformationen zum Befehl „azcopy list“.
author: normesta
ms.service: storage
ms.topic: reference
ms.date: 10/16/2019
ms.author: normesta
ms.subservice: common
ms.reviewer: zezha-msft
ms.openlocfilehash: b7348e3790166e1a1aecab422e571b8f2fc7cd5f
ms.sourcegitcommit: 12de9c927bc63868168056c39ccaa16d44cdc646
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 10/17/2019
ms.locfileid: "72513522"
---
# <a name="azcopy-list"></a>azcopy list

Listet die Entitäten in einer bestimmten Ressource auf.

## <a name="synopsis"></a>Zusammenfassung

In der aktuellen Version werden nur Blobcontainer unterstützt.

```azcopy
azcopy list [containerURL] [flags]
```

## <a name="examples"></a>Beispiele

```azcopy
azcopy list [containerURL]
```

## <a name="options"></a>Optionen

|Option|BESCHREIBUNG|
|--|--|
|-h, --help|Zeigt Hilfeinhalt zum Befehl „list“.|
|–machine-readable|Listet Dateigrößen in Byte auf.|
|–mega-units|Zeigt Einheiten in Reihenfolgen von 1000 (nicht 1024) an.|
|–running-tally|Zählt die Gesamtzahl von Dateien und deren Größen.|

## <a name="options-inherited-from-parent-commands"></a>Von übergeordneten Befehlen geerbte Optionen

|Option|BESCHREIBUNG|
|---|---|
|–cap-mbps uint32|Begrenzt die Übertragungsrate (in Megabit pro Sekunde). Der Schritt-für-Schritt-Durchsatz kann von der Obergrenze geringfügig abweichen. Wenn diese Option auf „null“ festgelegt oder weggelassen wird, ist der Durchsatz nicht begrenzt.|
|–output-type string|Format der Befehlsausgabe. Folgende Optionen sind verfügbar: „text“ und „json“. Der Standardwert lautet „text“.|

## <a name="see-also"></a>Weitere Informationen

- [azcopy](storage-ref-azcopy.md)
