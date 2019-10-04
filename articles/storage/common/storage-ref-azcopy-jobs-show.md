---
title: azcopy jobs show | Microsoft-Dokumentation
description: Dieser Artikel enthält Referenzinformationen zum Befehl „azcopy jobs show“.
author: normesta
ms.service: storage
ms.topic: reference
ms.date: 08/26/2019
ms.author: normesta
ms.subservice: common
ms.reviewer: zezha-msft
ms.openlocfilehash: d4369bd0c986ee20a0796436fea47509a711de4f
ms.sourcegitcommit: 532335f703ac7f6e1d2cc1b155c69fc258816ede
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 08/30/2019
ms.locfileid: "70196812"
---
# <a name="azcopy-jobs-show"></a>azcopy jobs show

Zeigt detaillierte Informationen zur angegebenen Auftrags-ID an.

## <a name="synopsis"></a>Zusammenfassung

Wenn nur die Auftrags-ID ohne ein Flag angegeben wird, wird die Statuszusammenfassung des Auftrags zurückgegeben.

Wenn das Flag `with-status` festgelegt wird, wird die Liste der Übertragungen im Auftrag mit dem angegebenen Wert angezeigt.

```azcopy
azcopy jobs show [jobID] [flags]
```

## <a name="options"></a>Optionen

|Option|BESCHREIBUNG|
|--|--|
|-h, --help|Zeigt Hilfeinhalt zum Befehl „show“.|
|–with-status string|Auflisten nur der Auftragsübertragungen mit diesem Status; verfügbare Werte: „Gestartet“, „Erfolgreich“, „Fehler“|

## <a name="options-inherited-from-parent-commands"></a>Von übergeordneten Befehlen geerbte Optionen

|Option|BESCHREIBUNG|
|---|---|
|–cap-mbps uint32|Begrenzt die Übertragungsrate, in Megabit pro Sekunde. Der Schritt-für-Schritt-Durchsatz kann von der Obergrenze geringfügig abweichen. Wenn diese Option auf „null“ festgelegt oder weggelassen wird, ist der Durchsatz nicht begrenzt.|
|–output-type string|Format der Befehlsausgabe. Folgende Optionen sind verfügbar: „text“, „json“. Der Standardwert lautet „text“.|

## <a name="see-also"></a>Weitere Informationen

- [azcopy jobs](storage-ref-azcopy-jobs.md)
