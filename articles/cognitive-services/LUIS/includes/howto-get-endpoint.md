---
title: include file
description: include file
services: cognitive-services
author: diberry
manager: nitinme
ms.custom: include file
ms.service: cognitive-services
ms.subservice: luis
ms.topic: include
ms.date: 11/05/2019
ms.author: diberry
ms.openlocfilehash: 39bfa5b6800c65112850faa8842b915ceedb9312
ms.sourcegitcommit: 359930a9387dd3d15d39abd97ad2b8cb69b8c18b
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 11/06/2019
ms.locfileid: "73648065"
---
Kopieren Sie im Abschnitt **Verwalten** (Menü oben rechts) auf der Seite **Azure-Ressourcen** (Menü auf der linken Seite) die **Beispielabfrage**-URL, und fügen Sie sie in eine neue Browserregisterkarte ein. 

Die Endpunkt-URL sieht wie folgt aus, wobei Ihre eigene App-ID und Ihr eigener Endpunktschlüssel APP-ID und KEY-ID ersetzen:

```console
https://westus.api.cognitive.microsoft.com/luis/prediction/v3.0/apps/APP-ID/slots/production/predict?subscription-key=KEY-ID&verbose=true&show-all-intents=true&log=true&query=YOUR_QUERY_HERE
```