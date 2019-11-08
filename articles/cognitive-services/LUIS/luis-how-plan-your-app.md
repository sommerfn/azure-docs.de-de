---
title: Planen Ihrer App – LUIS
titleSuffix: Azure Cognitive Services
description: Beschreiben Sie kurz relevante App-Absichten und -Entitäten, und erstellen Sie dann Ihre Anwendungspläne in Language Understanding Intelligent Service (LUIS).
services: cognitive-services
author: diberry
manager: nitinme
ms.custom: seodec18
ms.service: cognitive-services
ms.subservice: language-understanding
ms.topic: conceptual
ms.date: 10/25/2019
ms.author: diberry
ms.openlocfilehash: b5e5df111b81cb60b6d194be190421bdb5ce2683
ms.sourcegitcommit: c22327552d62f88aeaa321189f9b9a631525027c
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 11/04/2019
ms.locfileid: "73467699"
---
# <a name="plan-your-luis-app-schema-with-subject-domain-and-data-extraction"></a>Planen Ihres LUIS-App-Schemas mit Motivdomäne und Datenextraktion

Ein LUIS-App-Schema enthält Absichten und Entitäten, die für Ihre Motivdomäne relevant sind. Die Absichten klassifizieren Benutzeräußerungen, und die Entitäten extrahieren Daten aus den Benutzeräußerungen. 

## <a name="identify-your-domain"></a>Identifizieren Ihrer Domäne

Eine LUIS-App baut auf einem domänenspezifischen Thema auf.  Beispielsweise könnten Sie eine Reise-App erstellen, die Buchungen von Tickets, Flügen, Hotels und Mietwagen ausführt. Eine andere App könnte Inhalte im Zusammenhang mit Sportaktivitäten, dem Nachverfolgen von Fitnessübungen und dem Festlegen von Zielen bereitstellen. Durch die Identifizierung der Domäne können Sie Wörter oder Ausdrücke finden, die für Ihre Domäne wichtig sind.

> [!TIP]
> LUIS bietet [vordefinierte Domänen](luis-how-to-use-prebuilt-domains.md) für viele häufige Szenarien.
> Überprüfen Sie, ob Sie eine vordefinierte Domäne als Ausgangspunkt für Ihre App verwenden können.

## <a name="identify-your-intents"></a>Identifizieren Ihrer Absichten

Überlegen Sie sich, welche [Absichten](luis-concept-intent.md) wichtig für die Aufgabe Ihrer Anwendung sind. 

Beispiel: eine Reise-App mit Funktionen zum Buchen eines Flugs und Überprüfen des Wetters am Ziel des Benutzers. Sie können für diese Aktionen die Absichten `BookFlight` und `GetWeather` definieren. 

In einer komplexeren App mit umfassenderen Funktionen gibt es weitere Absichten, und Sie sollten diese sorgfältig definieren, damit sie nicht zu spezifisch sind. Beispielsweise müssen möglicherweise `BookFlight` und `BookHotel` separate Absichten sein, aber `BookInternationalFlight` und `BookDomesticFlight` können zu ähnlich sein.

> [!NOTE]
> Als bewährte Methode sollten Sie nur so viele Absichten verwenden, wie zum Ausführen der Funktionen Ihrer App erforderlich sind. Wenn Sie zu viele Absichten definieren, wird es für LUIS schwieriger, Äußerungen richtig zu klassifizieren. Wenn Sie zu wenige definieren, sind sie möglicherweise so allgemein gehalten, dass sie sich überschneiden.

Wenn Sie nicht die Gesamtabsicht identifizieren müssen, fügen Sie alle Beispielbenutzeräußerungen zur Absicht „None“ hinzu. Wenn sich bei Ihrer App die Notwendigkeit weiterer Absichten herausstellt, können Sie diese später erstellen. 

## <a name="create-example-utterances-for-each-intent"></a>Erstellen von Beispieläußerungen für jede Absicht

Nachdem Sie die Absichten festgelegt haben, können Sie pro Absicht jeweils 15 bis 30 Beispieläußerungen erstellen. Erstellen Sie zunächst nicht weniger als die hier angegebene Anzahl von Äußerungen für die einzelnen Absichten, aber auch nicht zu viele. Jede Äußerung sollte sich von der vorherigen Äußerung unterscheiden. Eine gute Vielfalt in den Äußerungen umfasst die Gesamtzahl der Wörter, die Wortwahl, die Verbform und die Interpunktion. 

Weitere Informationen finden Sie unter [Grundlegendes zu geeigneten Äußerungen für Ihre LUIS-App](luis-concept-utterance.md).

## <a name="identify-your-entities"></a>Identifizieren Ihrer Entitäten

Identifizieren Sie in den Beispieläußerungen die Entitäten, die extrahiert werden sollen. Um einen Flug zu buchen, benötigen Sie Informationen wie Ziel, Datum, Fluggesellschaft, Ticketkategorie und Reiseklasse. Erstellen Sie Entitäten für diese Datentypen, und markieren Sie dann die [Entitäten](luis-concept-entity-types.md) in den Beispieläußerungen, da sie für die Umsetzung einer Absicht wichtig sind. 

Wenn Sie festlegen, welche Entitäten in Ihrer App verwendet werden sollen, bedenken Sie, dass es verschiedene Typen von Entitäten zum Aufzeichnen der Beziehungen zwischen Objekttypen gibt. Unter [Entitäten in LUIS](luis-concept-entity-types.md) finden Sie weitere Details zu den verschiedenen Typen.

## <a name="next-steps"></a>Nächste Schritte

Erfahren Sie mehr über den typischen [Entwicklungszyklus](luis-concept-app-iteration.md).  