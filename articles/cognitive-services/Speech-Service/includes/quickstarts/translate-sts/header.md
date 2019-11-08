---
title: 'Schnellstart: Übersetzen von Sprache in Sprache – Speech-Dienst'
titleSuffix: Azure Cognitive Services
description: TBD
services: cognitive-services
author: yulin-li
manager: nitinme
ms.service: cognitive-services
ms.subservice: speech-service
ms.topic: include
ms.date: 09/20/2019
ms.author: yulili
ms.openlocfilehash: 77b060a5caf9865a1296cd6644cf0c51e3253a0e
ms.sourcegitcommit: c22327552d62f88aeaa321189f9b9a631525027c
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 11/04/2019
ms.locfileid: "73504843"
---
In dieser Schnellstartanleitung wird gesprochene Sprache mithilfe des [Speech SDK](~/articles/cognitive-services/speech-service/speech-sdk.md) interaktiv in eine andere Sprache übersetzt. Nach der Erfüllung einiger Voraussetzungen sind für die Übersetzung von Sprache in Sprache lediglich sechs Schritte erforderlich:
> [!div class="checklist"]
> * Erstellen eines Objekts vom Typ ````SpeechTranslationConfig```` auf der Grundlage Ihres Abonnementschlüssels und Ihrer Region
> * Aktualisieren des Objekts ````SpeechTranslationConfig````, um die Quell- und Zielsprache anzugeben
> * Aktualisieren des Objekts ````SpeechTranslationConfig````, um den Namen der Stimme für die Sprachausgabe anzugeben
> * Erstellen eines Objekts vom Typ ````TranslationRecognizer```` unter Verwendung des obigen Objekts ````SpeechTranslationConfig````
> * Starten des Erkennungsprozesses für eine einzelne Äußerung unter Verwendung des Objekts ````TranslationRecognizer````
> * Überprüfen des zurückgegebenen Objekts ````TranslationRecognitionResult````
