---
title: azcopy | Microsoft-Dokumentation
description: Dieser Artikel enthält Referenzinformationen zum Befehl „azcopy“.
author: normesta
ms.service: storage
ms.topic: reference
ms.date: 10/16/2019
ms.author: normesta
ms.subservice: common
ms.reviewer: zezha-msft
ms.openlocfilehash: 984d0c570c6c0d5048d58377f113319157411244
ms.sourcegitcommit: 12de9c927bc63868168056c39ccaa16d44cdc646
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 10/17/2019
ms.locfileid: "72513387"
---
# <a name="azcopy"></a>azcopy

AzCopy ist ein Befehlszeilentool, mit dem Daten in und aus Azure Storage verschoben werden.

## <a name="synopsis"></a>Zusammenfassung

Das allgemeine Format der Befehle lautet: `azcopy [command] [arguments] --[flag-name]=[flag-value]`.

Informationen zum Melden von Problemen oder weitere Informationen zum Tool finden Sie unter [https://github.com/Azure/azure-storage-azcopy](https://github.com/Azure/azure-storage-azcopy).

## <a name="options"></a>Optionen

**--cap-mbps uint32**   Begrenzt die Übertragungsrate (in Megabits pro Sekunde). Der Schritt-für-Schritt-Durchsatz kann von der Obergrenze geringfügig abweichen. Wenn diese Option auf „null“ festgelegt oder weggelassen wird, ist der Durchsatz nicht begrenzt.

**-h, --help** Hilfe zu „azcopy“
      
**--output-type**  Das Format der Befehlsausgabe. Folgende Optionen sind verfügbar: „text“ und „json“. Der Standardwert lautet „text“. (Standardwert: „text“)

## <a name="see-also"></a>Weitere Informationen

- [Erste Schritte mit AzCopy](storage-use-azcopy-v10.md)
- [azcopy copy](storage-ref-azcopy-copy.md)
- [azcopy doc](storage-ref-azcopy-doc.md)
- [azcopy env](storage-ref-azcopy-env.md)
- [azcopy jobs](storage-ref-azcopy-jobs.md)
- [azcopy list](storage-ref-azcopy-list.md)
- [azcopy login](storage-ref-azcopy-login.md)
- [azcopy logout](storage-ref-azcopy-logout.md)
- [azcopy make](storage-ref-azcopy-make.md)
- [azcopy remove](storage-ref-azcopy-remove.md)
- [azcopy sync](storage-ref-azcopy-sync.md)
