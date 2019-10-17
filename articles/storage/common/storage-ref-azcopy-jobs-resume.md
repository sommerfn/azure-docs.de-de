---
title: azcopy jobs resume | Microsoft-Dokumentation
description: Dieser Artikel enthält Referenzinformationen zum Befehl „azcopy jobs resume“.
author: normesta
ms.service: storage
ms.topic: reference
ms.date: 10/16/2019
ms.author: normesta
ms.subservice: common
ms.reviewer: zezha-msft
ms.openlocfilehash: 9e8dbbd40259c7a71f252d0d6e93dd6f135973de
ms.sourcegitcommit: 12de9c927bc63868168056c39ccaa16d44cdc646
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 10/17/2019
ms.locfileid: "72513544"
---
# <a name="azcopy-jobs-resume"></a>azcopy jobs resume

Setzt den vorhandenen Auftrag mit der angegebenen Auftrags-ID fort.

## <a name="synopsis"></a>Zusammenfassung

```azcopy
azcopy jobs resume [jobID] [flags]
```

## <a name="options"></a>Optionen

|Option|BESCHREIBUNG|
|--|--|
|–destination-sas string|Ziel-SAS des Ziels für die angegebene JobId.|
|–exclude string|Filter: Schließen Sie diese fehlgeschlagene(n) Übertragung(en) bei der Fortsetzung des Auftrags aus. Dateien sollten durch „;“ getrennt werden.|
|-h, --help|Zeigt Hilfeinhalt zum Befehl „resume“.|
|–include string|Filter: Schließen Sie nur diese fehlgeschlagene(n) Übertragung(en) bei der Fortsetzung des Auftrags ein. Dateien sollten durch „;“ getrennt werden.|
|–source-sas string |Quell-SAS der Quelle für die angegebene JobId.|

## <a name="options-inherited-from-parent-commands"></a>Von übergeordneten Befehlen geerbte Optionen

|Option|BESCHREIBUNG|
|---|---|
|–cap-mbps uint32|Begrenzt die Übertragungsrate (in Megabit pro Sekunde). Der Schritt-für-Schritt-Durchsatz kann von der Obergrenze geringfügig abweichen. Wenn diese Option auf „null“ festgelegt oder weggelassen wird, ist der Durchsatz nicht begrenzt.|
|–output-type string|Format der Befehlsausgabe. Folgende Optionen sind verfügbar: „text“ und „json“. Der Standardwert lautet „text“.|

## <a name="see-also"></a>Weitere Informationen

- [azcopy jobs](storage-ref-azcopy-jobs.md)
