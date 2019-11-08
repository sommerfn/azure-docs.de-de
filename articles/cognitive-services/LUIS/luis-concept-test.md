---
title: Testen Ihrer LUIS-App
titleSuffix: Azure Cognitive Services
description: Beim Testen stellen Sie LUIS Beispieläußerungen bereit und erhalten eine Antwort von Absichten und Entitäten, die LUIS erkannt hat.
author: diberry
manager: nitinme
services: cognitive-services
ms.custom: seodec18
ms.service: cognitive-services
ms.subservice: language-understanding
ms.topic: conceptual
ms.date: 10/10/2019
ms.author: diberry
ms.openlocfilehash: 25b360f90a0920aad2ea5e68cda31a68be5d37a9
ms.sourcegitcommit: c22327552d62f88aeaa321189f9b9a631525027c
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 11/04/2019
ms.locfileid: "73486679"
---
# <a name="testing-example-utterances-in-luis"></a>Testen von Beispieläußerungen in LUIS

Beim Testen stellen Sie LUIS Beispieläußerungen bereit und erhalten eine Antwort von Absichten und Entitäten, die LUIS erkannt hat. 

Sie können LUIS interaktiv mit jeweils einer einzelnen Äußerung testen, oder Sie können einen Batch von Äußerungen angeben. Beim Testen können Sie die Vorhersageantwort des aktuell aktiven Modells mit der Vorhersageantwort des veröffentlichten Modells vergleichen. 

<a name="A-test-score"></a>
<a name="Score-all-intents"></a>
<a name="E-(exponent)-notation"></a>

## <a name="what-is-a-score-in-testing"></a>Was ist beim Testen eine Bewertung?
Weitere Informationen zu Vorhersagebewertungen finden Sie unter [Vorhersageergebnis](luis-concept-prediction-score.md).

## <a name="interactive-testing"></a>Interaktives Testen
Interaktive Tests werden im **Testbereich** des LUIS-Portals durchgeführt. Sie können eine Äußerung eingeben, um zu ermitteln, wie Absichten und Entitäten identifiziert und bewertet werden. Wenn LUIS die Absichten und Entitäten für eine Äußerung im Testbereich nicht wie erwartet vorhersagt, kopieren Sie sie als neue Äußerung auf die Seite **Absicht**. Bezeichnen Sie dann die Teile dieser Äußerung nach Entitäten, und trainieren Sie LUIS. 

## <a name="batch-testing"></a>Testen in Batches
Lesen Sie unter [Testen in Batches](luis-concept-batch-test.md) nach, wenn Sie mehrere Äußerungen gleichzeitig testen möchten.

## <a name="endpoint-testing"></a>Testen an Endpunkten
Sie können den Test mithilfe des [Endpunkts](luis-glossary.md#endpoint) durchführen. Dabei können Sie maximal zwei Versionen der App einsetzen. Die Haupt- oder Liveversion Ihrer App wird dabei als **Produktionsendpunkt** festgelegt, und eine zweite Version wird auf dem **Stagingendpunkt** hinzugefügt. Mit diesem Ansatz erhalten Sie drei Versionen einer Äußerung: das aktuelle Modell im Testbereich der [LUIS](luis-reference-regions.md)-Website und die beiden Versionen auf zwei unterschiedlichen Endpunkten. 

Alle Endpunkttests werden auf Ihr Nutzungskontingent angerechnet. 

## <a name="do-not-log-tests"></a>Kein Protokollieren von Tests
Wenn Sie Tests an einem Endpunkt durchführen und nicht möchten, dass die Äußerung protokolliert wird, müssen Sie die Konfiguration `logging=false` für Abfragezeichenfolgen verwenden.

## <a name="where-to-find-utterances"></a>Speicherort von Äußerungen
LUIS speichert alle protokollierten Äußerungen im Abfrageprotokoll, das im LUIS-Portal auf der Seite mit der **Apps**-Liste sowie über die [Erstellungs-APIs](https://go.microsoft.com/fwlink/?linkid=2092087) für LUIS zum Download bereitsteht. 

Alle Äußerungen, bei denen LUIS unsicher ist, werden auf der Seite **[Überprüfen von Endpunktäußerungen](luis-how-to-review-endpoint-utterances.md)** der [LUIS](luis-reference-regions.md)-Website aufgeführt. 

## <a name="remember-to-train"></a>Trainieren
Denken Sie daran, LUIS zu [trainieren](luis-how-to-train.md), nachdem Sie Änderungen am Modell vorgenommen haben. Änderungen an der LUIS-App stehen erst für Tests zur Verfügung, nachdem die App trainiert wurde. 

## <a name="best-practices"></a>Bewährte Methoden
Informationen zu [Best Practices](luis-concept-best-practices.md).

## <a name="next-steps"></a>Nächste Schritte

* Erfahren Sie mehr über das [Testen](luis-interactive-test.md) Ihrer Äußerungen.
