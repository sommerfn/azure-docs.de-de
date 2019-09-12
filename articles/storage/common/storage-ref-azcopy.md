---
title: azcopy | Microsoft-Dokumentation
description: Dieser Artikel enthält Referenzinformationen zum Befehl „azcopy“.
author: normesta
ms.service: storage
ms.topic: reference
ms.date: 08/26/2019
ms.author: normesta
ms.subservice: common
ms.reviewer: zezha-msft
ms.openlocfilehash: 4da9206f4500941179d781a0fe2a57ad15d7393d
ms.sourcegitcommit: 532335f703ac7f6e1d2cc1b155c69fc258816ede
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 08/30/2019
ms.locfileid: "70196792"
---
# <a name="azcopy"></a>azcopy

AzCopy ist ein Befehlszeilentool, mit dem Daten in und aus Azure Storage verschoben werden.

## <a name="synopsis"></a>Zusammenfassung

Das allgemeine Format der Befehle lautet: `azcopy [command] [arguments] --[flag-name]=[flag-value]`.

Informationen zum Melden von Problemen oder weitere Informationen zum Tool finden Sie unter [https://github.com/Azure/azure-storage-azcopy](https://github.com/Azure/azure-storage-azcopy).

## <a name="options"></a>Optionen

|Option|BESCHREIBUNG|
|---|---|
|–cap-mbps uint32|Begrenzt die Übertragungsrate, in Megabit pro Sekunde. Der Schritt-für-Schritt-Durchsatz kann von der Obergrenze geringfügig abweichen. Wenn diese Option auf „null“ festgelegt oder weggelassen wird, ist der Durchsatz nicht begrenzt.|
|-h, --help|Zeigt den Hilfeinhalt zu „azcopy“.|
|–output-type string|Format der Befehlsausgabe. Folgende Optionen sind verfügbar: „text“, „json“. Der Standardwert lautet „text“.|

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
