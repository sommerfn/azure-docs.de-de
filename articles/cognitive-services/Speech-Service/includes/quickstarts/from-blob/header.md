---
title: 'Schnellstart: Erkennen von im Blobspeicher gespeicherter Sprache – Spracherkennungsdienst'
titleSuffix: Azure Cognitive Services
description: TBD
services: cognitive-services
author: erhopf
manager: nitinme
ms.service: cognitive-services
ms.subservice: speech-service
ms.topic: include
ms.date: 10/28/2019
ms.author: erhopf
zone_pivot_groups: programming-languages-set-two
ms.openlocfilehash: 195c4c94f7ab00a8740c9dd14aad0d0fd0daa9b7
ms.sourcegitcommit: c22327552d62f88aeaa321189f9b9a631525027c
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 11/04/2019
ms.locfileid: "73506099"
---
In dieser Schnellstartanleitung verwenden Sie die [REST-API zur Batchtranskription](../../../batch-transcription.md), um die in einem [SAS-Blob](https://aka.ms/ignite2019/speech/placeholder) gespeicherte Sprache zu erkennen. Nach der Erfüllung einiger Voraussetzungen sind für die Spracherkennung über eine REST-API lediglich wenige Schritte erforderlich:
> [!div class="checklist"]
> * Senden Sie die JSON-Anforderung an den Spracherkennungsdienst, um die Sprache zu transkribieren.
> * Überprüfen des Status der Transkription.
> * Laden Sie die Transkriptionsergebnisse herunter, wenn Sie fertig sind.