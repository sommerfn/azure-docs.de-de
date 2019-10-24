---
title: azcopy jobs show | Microsoft-Dokumentation
description: Dieser Artikel enthält Referenzinformationen zum Befehl „azcopy jobs show“.
author: normesta
ms.service: storage
ms.topic: reference
ms.date: 10/16/2019
ms.author: normesta
ms.subservice: common
ms.reviewer: zezha-msft
ms.openlocfilehash: 42b8bde9adb6980ff2c7004d43b02fc1fdc38363
ms.sourcegitcommit: 12de9c927bc63868168056c39ccaa16d44cdc646
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 10/17/2019
ms.locfileid: "72513454"
---
# <a name="azcopy-jobs-show"></a>azcopy jobs show

Zeigt detaillierte Informationen zur angegebenen Auftrags-ID an.

## <a name="synopsis"></a>Zusammenfassung

Wenn nur die Auftrags-ID ohne ein Flag angegeben wird, wird die Statuszusammenfassung des Auftrags zurückgegeben.

Die Byteanzahl und die Fertigstellung in Prozent, die beim Ausführen dieses Befehls angezeigt werden, beziehen sich nur auf die Dateien, die im Auftrag abgeschlossen werden. Teilweise abgeschlossene Dateien werden nicht berücksichtigt.

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
|–cap-mbps uint32|Begrenzt die Übertragungsrate (in Megabit pro Sekunde). Der Schritt-für-Schritt-Durchsatz kann von der Obergrenze geringfügig abweichen. Wenn diese Option auf „null“ festgelegt oder weggelassen wird, ist der Durchsatz nicht begrenzt.|
|–output-type string|Format der Befehlsausgabe. Folgende Optionen sind verfügbar: „text“ und „json“. Der Standardwert lautet „text“.|

## <a name="see-also"></a>Weitere Informationen

- [azcopy jobs](storage-ref-azcopy-jobs.md)
