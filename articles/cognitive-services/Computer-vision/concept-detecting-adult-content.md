---
title: Nicht jugendfreie und anzügliche Inhalte – maschinelles Sehen
titleSuffix: Azure Cognitive Services
description: Konzepte zur Erkennung nicht jugendfreier und anzüglicher Inhalte in Bildern mithilfe der Maschinelles Sehen-API.
services: cognitive-services
author: PatrickFarley
manager: nitinme
ms.service: cognitive-services
ms.subservice: computer-vision
ms.topic: conceptual
ms.date: 02/08/2019
ms.author: pafarley
ms.custom: seodec18
ms.openlocfilehash: ca5f35ab47822d74de556671c38886942d23d9ff
ms.sourcegitcommit: 124c3112b94c951535e0be20a751150b79289594
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 08/10/2019
ms.locfileid: "68946280"
---
# <a name="detect-adult-and-racy-content"></a>Erkennen von nicht jugendfreien und freizügigen Inhalten

Maschinelles Sehen kann nicht jugendfreie Inhalte in Bildern erkennen, damit Entwickler die Anzeige solcher Bilder in ihrer Software einschränken können. Inhaltsflags werden mit einem Wert von 0 bis 1 angewendet, damit Entwickler die Ergebnisse ihren eigenen Vorgaben entsprechend interpretieren können. 

> [!NOTE]
> Dieses Feature wird auch vom Dienst [Azure Content Moderator](https://docs.microsoft.com/azure/cognitive-services/content-moderator/overview) angeboten. Diese Alternative kann als Lösung für strengere Szenarien zur Inhaltsmoderation eingesetzt werden, wie etwa für die Textmoderation oder Workflows für die Überprüfung durch Personen.

## <a name="content-flag-definitions"></a>Definitionen für Inhaltsflags

Als **Adult** (erwachsen) gekennzeichnete Bilder sind gemäß Definition pornografischer Natur und stellen oft Nacktheit und sexuelle Handlungen dar. 

Als **Racy** (anzüglich) gekennzeichnete Bilder sind gemäß Definition sexuell suggestiv und enthalten häufig in sexueller Hinsicht weniger explizite Inhalte als Bilder, die als **Adult (erwachsen)** gekennzeichnet sind. 

## <a name="identify-adult-and-racy-content"></a>Identifizieren von nicht jugendfreien und freizügigen Inhalten

Die [Analyse](https://westus.dev.cognitive.microsoft.com/docs/services/5adf991815e1060e6355ad44/operations/56f91f2e778daf14a499e1fa)-API.

Die Methode zum Analysieren von Bildern gibt zwei boolesche Eigenschaften, `isAdultContent` und `isRacyContent`, in der JSON-Antwort der Methode zurück, um nicht jugendfreie bzw. anzügliche Inhalte zu identifizieren. Die Methode gibt auch zwei Eigenschaften zurück, `adultScore` und `racyScore`, die jeweils die Zuverlässigkeitsbewertungen für die Identifizierung von nicht jugendfreien und anzüglichen Inhalten darstellen.

## <a name="next-steps"></a>Nächste Schritte

Erfahren Sie mehr über die Konzepte zum [Erkennen domänenspezifischer Inhalte](concept-detecting-domain-content.md) und [Erkennen von Gesichtern](concept-detecting-faces.md).
