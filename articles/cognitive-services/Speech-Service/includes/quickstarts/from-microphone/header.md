---
title: 'Schnellstart: Erkennen von Spracheingaben per Mikrofon – Speech-Dienst'
titleSuffix: Azure Cognitive Services
description: TBD
services: cognitive-services
author: erhopf
manager: nitinme
ms.service: cognitive-services
ms.subservice: speech-service
ms.topic: quickstart
ms.date: 11/07/2019
ms.author: erhopf
ms.openlocfilehash: f3f28cdd8c3c77a5cde2ead86c49b41d54bab5d7
ms.sourcegitcommit: 39da2d9675c3a2ac54ddc164da4568cf341ddecf
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 11/12/2019
ms.locfileid: "73961174"
---
In dieser Schnellstartanleitung verwenden Sie das [Speech SDK](~/articles/cognitive-services/speech-service/speech-sdk.md) zur interaktiven Erkennung von Sprache aus Audiodaten, die über ein Mikrofon erfasst wurden. Nach der Erfüllung einiger Voraussetzungen sind für die Spracherkennung über ein Mikrofon lediglich vier Schritte erforderlich:

> [!div class="checklist"]
> * Erstellen eines Objekts vom Typ `SpeechConfig` auf der Grundlage Ihres Abonnementschlüssels und Ihrer Region
> * Erstellen eines Objekts vom Typ `SpeechRecognizer` unter Verwendung des obigen Objekts `SpeechConfig`
> * Starten des Erkennungsprozesses für eine einzelne Äußerung unter Verwendung des Objekts `SpeechRecognizer`
> * Überprüfen des zurückgegebenen Objekts `SpeechRecognitionResult`
