---
title: include file
description: include file
services: storage
author: roygara
ms.service: storage
ms.topic: include
ms.date: 05/05/2019
ms.author: rogarana
ms.custom: include file
ms.openlocfilehash: c04875f92ee58c24567fbb4d73d8d01697810f99
ms.sourcegitcommit: 1d0b37e2e32aad35cc012ba36200389e65b75c21
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 10/15/2019
ms.locfileid: "72391643"
---
| Resource | Ziel | Harte Grenze |
|----------|--------------|------------|
| Speichersynchronisierungsdienste pro Region | 20 Speichersynchronisierungsdienste | Ja |
| Synchronisierungsgruppen pro Speichersynchronisierungsdienst | 100 Synchronisierungsgruppen | Ja |
| Registrierte Server pro Speichersynchronisierungsdienst | 99 Server | Ja |
| Cloudendpunkte pro Synchronisierungsgruppe | 1 Cloudendpunkt | Ja |
| Serverendpunkte pro Synchronisierungsgruppe | 50 Serverendpunkte | Nein |
| Serverendpunkte pro Server | 30 Serverendpunkte | Ja |
| Dateisystemobjekte (Verzeichnisse und Dateien) pro Synchronisierungsgruppe | 50 Millionen Objekte | Nein |
| Maximale Anzahl von Dateisystemobjekten (Verzeichnisse und Dateien) in einem Verzeichnis | 5 Millionen Objekte | Ja |
| Maximale Sicherheitsbeschreibung des Objekts (Verzeichnisse und Dateien) | 64 KiB | Ja |
| Dateigröße | 100 GB | Nein |
| Minimale Dateigröße für die Unterteilung einer Datei | 64 KiB | Ja |
| Gleichzeitige Synchronisierungssitzungen | Mindestens V4-Agent: Das Limit variiert basierend auf verfügbaren Systemressourcen. <BR> V3-Agent: Zwei aktive Synchronisierungssitzungen pro Prozessor oder maximal acht aktive Synchronisierungssitzungen pro Server. | Ja

> [!Note]  
> Ein Endpunkt für Azure-Dateisynchronisierung kann auf die Größe einer Azure-Dateifreigabe zentral hochskaliert werden. Wenn die maximale Größe der Azure-Dateifreigabe erreicht ist, kann keine Synchronisierung durchgeführt werden.
