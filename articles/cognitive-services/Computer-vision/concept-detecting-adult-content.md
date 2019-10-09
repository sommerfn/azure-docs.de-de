---
title: Nicht jugendfreie, anzügliche und gewaltverherrlichende Inhalte – maschinelles Sehen
titleSuffix: Azure Cognitive Services
description: Konzepte zur Erkennung nicht jugendfreier Inhalte in Bildern mithilfe der Maschinelles Sehen-API.
services: cognitive-services
author: PatrickFarley
manager: nitinme
ms.service: cognitive-services
ms.subservice: computer-vision
ms.topic: conceptual
ms.date: 10/01/2019
ms.author: pafarley
ms.custom: seodec18
ms.openlocfilehash: ee18916a59bb081d65494f46e7aba7c29c7177cc
ms.sourcegitcommit: a19f4b35a0123256e76f2789cd5083921ac73daf
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 10/02/2019
ms.locfileid: "71718511"
---
# <a name="detect-adult-content"></a>Erkennen nicht jugendfreier Inhalte

Maschinelles Sehen kann nicht jugendfreie Inhalte in Bildern erkennen, damit Entwickler die Anzeige solcher Bilder in ihrer Software einschränken können. Inhaltsflags werden mit einem Wert von 0 bis 1 angewendet, damit Entwickler die Ergebnisse ihren eigenen Vorgaben entsprechend interpretieren können.

> [!NOTE]
> Viele dieser Funktionen werden auch vom Dienst [Azure Content Moderator](https://docs.microsoft.com/azure/cognitive-services/content-moderator/overview) angeboten. Diese Alternative kann als Lösung für strengere Szenarien zur Inhaltsmoderation eingesetzt werden, wie etwa für die Textmoderation oder Workflows für die Überprüfung durch Personen.

## <a name="content-flag-definitions"></a>Definitionen für Inhaltsflags

Innerhalb der Klassifizierung „Nicht jugendfrei“ gibt es mehrere verschiedene Kategorien:

- Als **Adult** (erwachsen) gekennzeichnete Bilder sind explizit sexueller Natur und stellen oft Nacktheit und sexuelle Handlungen dar.
- Als **Racy** (anzüglich) gekennzeichnete Bilder sind gemäß Definition sexuell suggestiv und enthalten häufig in sexueller Hinsicht weniger explizite Inhalte als Bilder, die als **Adult (erwachsen)** gekennzeichnet sind.
- **Gewaltverherrlichende** Bilder sind als Bilder definiert, die blutrünstige Szenen darstellen.

## <a name="use-the-api"></a>Verwenden der API

Mit der API [Bild analysieren](https://westus.dev.cognitive.microsoft.com/docs/services/5adf991815e1060e6355ad44/operations/56f91f2e778daf14a499e1fa) können Sie nicht jugendfreie Inhalte erkennen. Wenn Sie den Wert von `Adult` zum **visualFeatures**-Abfrageparameter hinzufügen, gibt die API in ihrer JSON-Antwort drei boolesche Eigenschaften (`isAdultContent`, `isRacyContent` und `isGoryContent`) zurück. Die Methode gibt auch die entsprechenden Eigenschaften (`adultScore`, `racyScore` und `goreScore`) zurück, die Zuverlässigkeitsbewertungen zwischen 0 und 1 für jede Kategorie darstellen.

- [Schnellstart: Analysieren eines Bilds (.NET SDK)](./quickstarts-sdk/csharp-analyze-sdk.md)
- [Schnellstart: Analysieren eines Bilds (REST-API)](./quickstarts/csharp-analyze.md)
