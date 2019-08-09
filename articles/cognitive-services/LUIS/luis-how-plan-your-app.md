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
ms.date: 07/29/2019
ms.author: diberry
ms.openlocfilehash: dc648b30dc1236080be06044f510557ae0ce9476
ms.sourcegitcommit: 3877b77e7daae26a5b367a5097b19934eb136350
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 07/30/2019
ms.locfileid: "68638313"
---
# <a name="plan-your-luis-app-with-subject-domain-intents-and-entities"></a>Planen Ihrer LUIS-App mit Thema, Absichten und Entitäten

Bestimmen Sie zur Planung Ihrer App Ihren Themenbereich. Dies schließt mögliche Absichten und Entitäten mit ein, die für Ihre Anwendung relevant sind.  

## <a name="identify-your-domain"></a>Identifizieren Ihrer Domäne

Eine LUIS-App baut auf einem domänenspezifischen Thema auf.  Beispielsweise könnten Sie eine Reise-App erstellen, die Buchungen von Tickets, Flügen, Hotels und Mietwagen ausführt. Eine andere App könnte Inhalte im Zusammenhang mit Sportaktivitäten, dem Nachverfolgen von Fitnessübungen und dem Festlegen von Zielen bereitstellen. Durch die Identifizierung der Domäne können Sie Wörter oder Ausdrücke finden, die für Ihre Domäne wichtig sind.

> [!TIP]
> LUIS bietet [vordefinierte Domänen](luis-how-to-use-prebuilt-domains.md) für viele häufige Szenarien.
> Überprüfen Sie, ob Sie eine vordefinierte Domäne als Ausgangspunkt für Ihre App verwenden können.

## <a name="identify-your-intents"></a>Identifizieren Ihrer Absichten

Überlegen Sie sich, welche [Absichten](luis-concept-intent.md) wichtig für die Aufgabe Ihrer Anwendung sind. Beispiel: eine Reise-App mit Funktionen zum Buchen eines Flugs und Überprüfen des Wetters am Ziel des Benutzers. Sie können die Absichten „BookFlight“ und „GetWeather“ für diese Aktionen definieren. In einer komplexeren App mit umfassenderen Funktionen gibt es weitere Absichten, und Sie sollten diese sorgfältig definieren, damit sie nicht zu spezifisch sind. Beispielsweise sind „BookFlight“ und „BookHotel“ möglicherweise separate Absichten, aber „BookInternationalFlight“ und „BookDomesticFlight“ könnten zu ähnlich sein.

> [!NOTE]
> Als bewährte Methode sollten Sie nur so viele Absichten verwenden, wie zum Ausführen der Funktionen Ihrer App erforderlich sind. Wenn Sie zu viele Absichten definieren, wird es für LUIS schwieriger, Äußerungen richtig zu klassifizieren. Wenn Sie zu wenige definieren, sind sie möglicherweise so allgemein gehalten, dass sie sich überschneiden.

## <a name="create-example-utterances-for-each-intent"></a>Erstellen von Beispieläußerungen für jede Absicht

Nachdem Sie die Absichten festgelegt haben, können Sie pro Absicht jeweils 15 bis 30 Beispieläußerungen erstellen. Erstellen Sie zunächst nicht weniger als die hier angegebene Anzahl von Äußerungen für die einzelnen Absichten, aber auch nicht zu viele. Jede Äußerung sollte sich von der vorherigen Äußerung unterscheiden. Eine gute Vielfalt in den Äußerungen umfasst die Gesamtzahl der Wörter, die Wortwahl, die Verbform und die Interpunktion. 

Weitere Informationen finden Sie unter [Grundlegendes zu geeigneten Äußerungen für Ihre LUIS-App](luis-concept-utterance.md).

## <a name="identify-your-entities"></a>Identifizieren Ihrer Entitäten

Identifizieren Sie in den Beispieläußerungen die Entitäten, die extrahiert werden sollen. Um einen Flug zu buchen, benötigen Sie Informationen wie Ziel, Datum, Fluggesellschaft, Ticketkategorie und Reiseklasse. Erstellen Sie Entitäten für diese Datentypen, und markieren Sie dann die [Entitäten](luis-concept-entity-types.md) in den Beispieläußerungen, da sie für die Umsetzung einer Absicht wichtig sind. 

Wenn Sie festlegen, welche Entitäten in Ihrer App verwendet werden sollen, bedenken Sie, dass es verschiedene Typen von Entitäten zum Aufzeichnen der Beziehungen zwischen Objekttypen gibt. Unter [Entitäten in LUIS](luis-concept-entity-types.md) finden Sie weitere Details zu den verschiedenen Typen.

## <a name="next-steps"></a>Nächste Schritte

Nachdem Ihre App trainiert und veröffentlicht wurde und Endpunktäußerungen erhält, sollten Sie planen, Vorhersageverbesserungen mit [aktivem Lernen](luis-how-to-review-endpoint-utterances.md), [Ausdruckslisten](luis-concept-feature.md) und [Mustern](luis-concept-patterns.md) zu implementieren. 


* Eine kurze exemplarische Vorgehensweise zum Erstellen einer LUIS-App finden Sie unter [Erstellen Ihrer ersten LUIS-App (Language Understanding Intelligent Service)](luis-get-started-create-app.md).
