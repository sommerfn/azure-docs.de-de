---
title: Containeranforderungen und -empfehlungen
titleSuffix: Azure Cognitive Services
services: cognitive-services
author: IEvangelist
manager: nitinme
ms.service: cognitive-services
ms.topic: include
ms.date: 09/18/2019
ms.author: dapine
ms.openlocfilehash: 47c889a428ec2916dc8cad73e30ef5ff630372a7
ms.sourcegitcommit: 2ed6e731ffc614f1691f1578ed26a67de46ed9c2
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 09/19/2019
ms.locfileid: "71129811"
---
> [!NOTE]
> Die Anforderungen und Empfehlungen basieren auf Benchmarks mit einer einzelnen Anforderung pro Sekunde, wobei ein 8 MB großes Bild eines gescannten Geschäftsbriefs mit 29 Zeilen und insgesamt 803 Zeichen verwendet wird.

#### <a name="readtabread"></a>[Lesen](#tab/read)

In der folgenden Tabelle werden die minimale und empfohlene Zuordnung von Ressourcen für jeden Container für das Lesen beschrieben.

| Container | Minimum | Empfohlen |TPS<br>(Minimum, Maximum)|
|-----------|---------|-------------|--|
| Lesen | Ein Kern, 8 GB Arbeitsspeicher, 0,24 TPS | Acht Kerne, 16 GB Arbeitsspeicher, 1,17 TPS | 0,24, 1,17 |

#### <a name="recognize-texttabrecognize-text"></a>[Texterkennung](#tab/recognize-text)

In der folgenden Tabelle werden die minimale und empfohlene Zuordnung von Ressourcen für jeden Container für die Texterkennung beschrieben.

| Container | Minimum | Empfohlen |TPS<br>(Minimum, Maximum)|
|-----------|---------|-------------|--|
| Texterkennung | Ein Kern, 8 GB Arbeitsspeicher, 0,12 TPS | Zwei Kerne, 16 GB Arbeitsspeicher, 0,60 TPS | 0,12, 0,60 |

***

* Jeder Kern muss eine Geschwindigkeit von mindestens 2,6 GHz aufweisen.
* TPS: Transaktionen pro Sekunde

Kern und Arbeitsspeicher entsprechen den Einstellungen `--cpus` und `--memory`, die im Rahmen des Befehls `docker run` verwendet werden.
