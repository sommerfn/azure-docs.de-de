---
title: 'Schnellstart: Übersetzen von Sprache in Text – Spracherkennungsdienst'
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
ms.openlocfilehash: c45e75038d1731c933ccf8bf26f9de573e409292
ms.sourcegitcommit: c22327552d62f88aeaa321189f9b9a631525027c
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 11/04/2019
ms.locfileid: "73504739"
---
In dieser Schnellstartanleitung wird gesprochene Sprache mithilfe des [Sprach-SDK](~/articles/cognitive-services/speech-service/speech-sdk.md) interaktiv von einer Sprache in Text einer anderen Sprache übersetzt. Nach der Erfüllung einiger Voraussetzungen sind für die Übersetzung von Sprache in Text lediglich fünf Schritte erforderlich:
> [!div class="checklist"]
> * Erstellen Sie ein Objekt vom Typ ````SpeechConfig```` auf der Grundlage Ihres Abonnementschlüssels und Ihrer Region.
> * Aktualisieren Sie das ````SpeechConfig````-Objekt, um die Quell- und Zielsprachen anzugeben.
> * Erstellen Sie ein Objekt vom Typ ````TranslationRecognizer```` unter Verwendung des obigen ````SpeechConfig````-Objekts.
> * Starten Sie den Erkennungsprozess für eine einzelne Äußerung unter Verwendung des Objekts ````TranslationRecognizer````.
> * Überprüfen Sie das zurückgegebene ````TranslationRecognitionResult````-Objekt.
