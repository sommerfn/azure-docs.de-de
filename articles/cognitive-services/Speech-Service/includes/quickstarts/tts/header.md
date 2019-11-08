---
title: 'Schnellstart: Synthetisieren von Sprache – Spracherkennungsdienst'
titleSuffix: Azure Cognitive Services
description: Hier erfahren Sie, wie Sie mit dem Sprach-SDK Sprache synthetisieren.
services: cognitive-services
author: yulin-li
manager: nitinme
ms.service: cognitive-services
ms.subservice: speech-service
ms.topic: include
ms.date: 09/20/2019
ms.author: yulili
ms.openlocfilehash: 263a8e98c4c029728272c020efe00e82be20f5e2
ms.sourcegitcommit: c22327552d62f88aeaa321189f9b9a631525027c
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 11/04/2019
ms.locfileid: "73505075"
---
In dieser Schnellstartanleitung wird Text unter Verwendung des [Sprach-SDK](~/articles/cognitive-services/speech-service/speech-sdk.md) in synthetisierte Sprache umgewandelt. Nachdem Sie einige Voraussetzungen erfüllt haben, erfordert das Rendern synthetisierter Sprache über die Standardlautsprecher nur vier Schritte:
> [!div class="checklist"]
> * Erstellen Sie ein Objekt vom Typ ````SpeechConfig```` auf der Grundlage Ihres Abonnementschlüssels und Ihrer Region.
> * Erstellen Sie ein Objekt vom Typ ````SpeechSynthesizer```` unter Verwendung des obigen ````SpeechConfig````-Objekts.
> * Verwenden Sie das ````SpeechSynthesizer````-Objekt, um den Text zu sprechen.
> * Überprüfen Sie das zurückgegebene ````SpeechSynthesisResult````-Objekt auf Fehler.
