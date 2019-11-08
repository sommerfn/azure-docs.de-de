---
title: Abrufen des V3-Vorhersageendpunkts
titleSuffix: Azure Cognitive Services
services: cognitive-services
author: diberry
manager: nitinme
ms.service: cognitive-services
ms.topic: include
ms.date: 10/14/2019
ms.author: diberry
ms.openlocfilehash: bc6e43faca47e360daa8214e6b9f6e9df4a2f130
ms.sourcegitcommit: c22327552d62f88aeaa321189f9b9a631525027c
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 11/04/2019
ms.locfileid: "73495258"
---
1. Wählen Sie im LUIS-Portal im Abschnitt „Verwalten“ (Menü rechts oben) auf der Seite „Schlüssel und Endpunkte“ (linkes Menü) die Endpunkt-URL im unteren Seitenbereich aus.

    Hierdurch wird eine Browserregisterkarte mit der Endpunkt-URL in der Adressleiste geöffnet.

    Die URL enthält Ihre App-ID, den Schlüssel und den Slotnamen. Die URL für den V3-Vorhersageendpunkt sieht wie folgt aus:

    `https://REGION.api.cognitive.microsoft.com/luis/prediction/v3.0/apps/APP-ID/slots/SLOT-NAME/predict?subscription-key=YOUR-KEY&<optional-name-value-pairs>&query=UTTERANCE-TEXT.`

