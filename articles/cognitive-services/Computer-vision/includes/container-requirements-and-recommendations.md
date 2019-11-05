---
title: Containeranforderungen und -empfehlungen
titleSuffix: Azure Cognitive Services
services: cognitive-services
author: IEvangelist
manager: nitinme
ms.service: cognitive-services
ms.topic: include
ms.date: 09/20/2019
ms.author: dapine
ms.openlocfilehash: 2fa97a3a18741f506e10e5e4d61da286db3f744c
ms.sourcegitcommit: c22327552d62f88aeaa321189f9b9a631525027c
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 11/04/2019
ms.locfileid: "73481761"
---
> [!NOTE]
> Die Anforderungen und Empfehlungen basieren auf Benchmarks mit einer einzelnen Anforderung pro Sekunde, wobei ein 8 MB großes Bild eines gescannten Geschäftsbriefs mit 29 Zeilen und insgesamt 803 Zeichen verwendet wird.

In der folgenden Tabelle werden die minimale und empfohlene Zuordnung von Ressourcen für jeden Container für das Lesen beschrieben.

| Container | Minimum | Empfohlen |TPS<br>(Minimum, Maximum)|
|-----------|---------|-------------|--|
| Lesen | Ein Kern, 8 GB Arbeitsspeicher, 0,24 TPS | Acht Kerne, 16 GB Arbeitsspeicher, 1,17 TPS | 0,24, 1,17 |

* Jeder Kern muss eine Geschwindigkeit von mindestens 2,6 GHz aufweisen.
* TPS: Transaktionen pro Sekunde

Kern und Arbeitsspeicher entsprechen den Einstellungen `--cpus` und `--memory`, die im Rahmen des Befehls `docker run` verwendet werden.
