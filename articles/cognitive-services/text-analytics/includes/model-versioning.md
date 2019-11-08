---
title: Versionsverwaltung der Modelle
titleSuffix: Azure Cognitive Services
description: Angeben von Modellversionen in den V3-Endpunkten
services: cognitive-services
author: aahill
manager: nitinme
ms.service: cognitive-services
ms.topic: include
ms.date: 10/25/2019
ms.author: aahi
ms.openlocfilehash: 5a06e26e5f1640024e343c714db3df134422115c
ms.sourcegitcommit: c22327552d62f88aeaa321189f9b9a631525027c
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 11/04/2019
ms.locfileid: "73488643"
---
Mit Version 3 der Textanalyse-API können Sie auswählen, welches Textanalysemodell für Ihre Daten verwendet wird. Verwenden Sie den optionalen `model-version`-Parameter, um eine Version des Modells in Ihren Anforderungen auszuwählen. Wenn dieser Parameter nicht angegeben wird, legt die API standardmäßig `latest` fest, die neueste stabile Modellversion.

Verfügbare Modellversionen:
* `2019-10-01` (`latest`)

Jede Antwort von V3-Endpunkten enthält ein `model-version`-Feld, das die verwendete Modellversion angibt.

```json
{
    “documents”: […]
    “errors”: []
    “model-version”: “2019-10-01”
}
```
