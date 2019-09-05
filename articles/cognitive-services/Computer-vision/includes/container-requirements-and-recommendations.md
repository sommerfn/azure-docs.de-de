---
title: Containeranforderungen und -empfehlungen
titleSuffix: Azure Cognitive Services
services: cognitive-services
author: IEvangelist
manager: nitinme
ms.service: cognitive-services
ms.topic: include
ms.date: 8/26/2019
ms.author: dapine
ms.openlocfilehash: 5da088bf0356dd54d98ec5393fd2db8068f9c666
ms.sourcegitcommit: 94ee81a728f1d55d71827ea356ed9847943f7397
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 08/26/2019
ms.locfileid: "70034474"
---
In der folgenden Tabelle werden die Mindestanforderungen und empfohlenen Werte für CPU-Kerne und Arbeitsspeicher beschrieben, die jedem Container für die Texterkennung zugeordnet werden.

| Container | Minimum | Empfohlen |TPS<br>(Minimum, Maximum)|
|-----------|---------|-------------|--|
|Texterkennung|Ein Kern, 8 GB Arbeitsspeicher, 0,5 TPS|Zwei Kerne, 8 GB Arbeitsspeicher, 1 TPS|0,5, 1|

* Jeder Kern muss eine Geschwindigkeit von mindestens 2,6 GHz aufweisen.
* TPS: Transaktionen pro Sekunde

Kern und Arbeitsspeicher entsprechen den Einstellungen `--cpus` und `--memory`, die im Rahmen des Befehls `docker run` verwendet werden.