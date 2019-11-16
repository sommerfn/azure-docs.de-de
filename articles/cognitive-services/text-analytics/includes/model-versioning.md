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
ms.openlocfilehash: 29850cb9cb40eae0829b5d8c2b58b5f9518f18d5
ms.sourcegitcommit: ae8b23ab3488a2bbbf4c7ad49e285352f2d67a68
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 11/13/2019
ms.locfileid: "74021094"
---
Mit Version 3 der Textanalyse-API können Sie auswählen, welches Textanalysemodell für Ihre Daten verwendet wird. Verwenden Sie den optionalen `model-version`-Parameter, um eine Version des Modells in Ihren Anforderungen auszuwählen. Wenn dieser Parameter nicht angegeben wird, legt die API standardmäßig `latest` fest, die neueste stabile Modellversion.

Verfügbare Modellversionen:
* `2019-10-01` (`latest`)

Jede Antwort von V3-Endpunkten enthält ein `model-version`-Feld, das die verwendete Modellversion angibt.

```json
{
    "documents": […]
    "errors": []
    "model-version": "2019-10-01"
}
```
