---
title: 'Schnellstart: Erkennen von Sprache aus einer Audiodatei – Speech-Dienst'
titleSuffix: Azure Cognitive Services
description: TBD
services: cognitive-services
author: erhopf
manager: nitinme
ms.service: cognitive-services
ms.subservice: speech-service
ms.topic: quickstart
ms.date: 10/28/2019
ms.author: erhopf
ms.openlocfilehash: bbd3880ea679dc5fc86c0fc1ece101ca3fca74d9
ms.sourcegitcommit: c22327552d62f88aeaa321189f9b9a631525027c
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 11/04/2019
ms.locfileid: "73506283"
---
In dieser Schnellstartanleitung wird das [Speech SDK](~/articles/cognitive-services/speech-service/speech-sdk.md) verwendet, um Sprache aus einer Audiodatei zu erkennen. Nach der Erfüllung einiger Voraussetzungen sind für die Erkennung von Sprache aus einer Datei lediglich fünf Schritte erforderlich:
> [!div class="checklist"]
> * Erstellen eines Objekts vom Typ ````SpeechConfig```` auf der Grundlage Ihres Abonnementschlüssels und Ihrer Region
> * Erstellen eines Objekts vom Typ ````AudioConfig```` zum Angeben des Namens der WAV-Datei
> * Erstellen eines Objekts vom Typ ````SpeechRecognizer```` unter Verwendung der obigen Objekte ````SpeechConfig```` und ````AudioConfig````
> * Starten des Erkennungsprozesses für eine einzelne Äußerung unter Verwendung des Objekts ````SpeechRecognizer````
> * Überprüfen des zurückgegebenen Objekts ````SpeechRecognitionResult````
