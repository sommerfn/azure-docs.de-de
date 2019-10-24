---
title: 'API-Referenz: Gesichtserkennungs-API'
titleSuffix: Azure Cognitive Services
description: Die API-Referenz bietet Informationen zu den APIs „Person“, „LargePersonGroup/PersonGroup“, „LargeFaceList/FaceList“ und „Face Algorithms“.
services: cognitive-services
author: SteveMSFT
manager: nitinme
ms.service: cognitive-services
ms.subservice: face-api
ms.topic: reference
ms.date: 03/01/2018
ms.author: sbowles
ms.openlocfilehash: 940a5e25125acc2631fd3629bd610bb0e927e650
ms.sourcegitcommit: bb65043d5e49b8af94bba0e96c36796987f5a2be
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 10/16/2019
ms.locfileid: "72390103"
---
# <a name="face-api-reference-list"></a>Referenzliste zur Gesichtserkennungs-API

Die Gesichtserkennungs-API ist eine cloudbasierte API, die Algorithmen zur Gesichtserkennung und -wiedererkennung bereitstellt. Die Gesichtserkennungs-APIs decken folgende Kategorien ab:

- Face Algorithm APIs: Bieten grundlegende Funktionen wie [Detection](https://westus.dev.cognitive.microsoft.com/docs/services/563879b61984550e40cbbe8d/operations/563879b61984550f30395236) (Erkennung), [Find Similar](https://westus.dev.cognitive.microsoft.com/docs/services/563879b61984550e40cbbe8d/operations/563879b61984550f30395237) (Suchen ähnlicher Elemente), [Verification](https://westus.dev.cognitive.microsoft.com/docs/services/563879b61984550e40cbbe8d/operations/563879b61984550f3039523a) (Überprüfung), [Identification](https://westus.dev.cognitive.microsoft.com/docs/services/563879b61984550e40cbbe8d/operations/563879b61984550f30395239) (Identifikation) und [Group](https://westus.dev.cognitive.microsoft.com/docs/services/563879b61984550e40cbbe8d/operations/563879b61984550f30395238) (Gruppe).
- [FaceList-APIs](https://westus.dev.cognitive.microsoft.com/docs/services/563879b61984550e40cbbe8d/operations/563879b61984550f3039524b): Werden zum Verwalten eines FaceList-Elements für [Find Similar](https://westus.dev.cognitive.microsoft.com/docs/services/563879b61984550e40cbbe8d/operations/563879b61984550f30395237) (Suchen ähnlicher Elemente) verwendet.
- [LargePersonGroup Person-APIs](https://westus.dev.cognitive.microsoft.com/docs/services/563879b61984550e40cbbe8d/operations/599adcba3a7b9412a4d53f40): Werden zur Verwaltung von LargePersonGroup Person Faces-Elementen für [Identification](https://westus.dev.cognitive.microsoft.com/docs/services/563879b61984550e40cbbe8d/operations/563879b61984550f30395239) (Identifikation) verwendet.
- [LargePersonGroup-APIs](https://westus.dev.cognitive.microsoft.com/docs/services/563879b61984550e40cbbe8d/operations/599acdee6ac60f11b48b5a9d): Werden zur Verwaltung eines LargePersonGroup-Datasets für [Identification](https://westus.dev.cognitive.microsoft.com/docs/services/563879b61984550e40cbbe8d/operations/563879b61984550f30395239) (Identifikation) verwendet.
- [LargeFaceList-APIs](https://westus.dev.cognitive.microsoft.com/docs/services/563879b61984550e40cbbe8d/operations/5a157b68d2de3616c086f2cc): Werden zum Verwalten eines LargeFaceList-Elements für [Find Similar](https://westus.dev.cognitive.microsoft.com/docs/services/563879b61984550e40cbbe8d/operations/563879b61984550f30395237) (Suchen ähnlicher Elemente) verwendet.
- [PersonGroup Person-APIs](https://westus.dev.cognitive.microsoft.com/docs/services/563879b61984550e40cbbe8d/operations/563879b61984550f3039523c): Werden zur Verwaltung von PersonGroup Person Faces-Elementen für [Identification](https://westus.dev.cognitive.microsoft.com/docs/services/563879b61984550e40cbbe8d/operations/563879b61984550f30395239) (Identifikation) verwendet.
- [PersonGroup-APIs](https://westus.dev.cognitive.microsoft.com/docs/services/563879b61984550e40cbbe8d/operations/563879b61984550f30395244): Werden zur Verwaltung eines PersonGroup-Datasets für [Identification](https://westus.dev.cognitive.microsoft.com/docs/services/563879b61984550e40cbbe8d/operations/563879b61984550f30395239) (Identifikation) verwendet.
- [Momentaufnahme-APIs](https://westus.dev.cognitive.microsoft.com/docs/services/563879b61984550e40cbbe8d/operations/snapshot-take): Wird verwendet, um eine Momentaufnahme für die Datenmigration zwischen Abonnements zu verwalten.
