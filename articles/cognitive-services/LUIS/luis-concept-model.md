---
title: Entwerfen mit Modellen – LUIS
titleSuffix: Azure Cognitive Services
description: Language Understanding bietet verschiedene Typen von Modellen. Einige Modelle können auf mehr als eine Weise eingesetzt werden.
services: cognitive-services
author: diberry
manager: nitinme
ms.custom: seodec18
ms.service: cognitive-services
ms.subservice: language-understanding
ms.topic: conceptual
ms.date: 10/25/2019
ms.author: diberry
ms.openlocfilehash: 4cbca96ed95167615f3ff2876e27e546d08d92f5
ms.sourcegitcommit: c22327552d62f88aeaa321189f9b9a631525027c
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 11/04/2019
ms.locfileid: "73506761"
---
# <a name="design-with-intent-and-entity-models"></a>Entwerfen mit Absichts- und Entitätsmodellen 

Language Understanding bietet verschiedene Typen von Modellen. Einige Modelle können auf mehr als eine Weise eingesetzt werden. 

## <a name="v3-authoring-uses-machine-teaching"></a>V3-Erstellung verwendet Machine Teaching

LUIS ermöglicht es, Computer auf komfortable Weise Konzepte zu lehren. Der Computer kann dann Modelle erstellen (funktionale Annäherungen von Konzepten wie Klassifizierern und Extraktoren), die verwendet werden können, um das Rückgrat intelligenter Anwendungen zu bilden. Zwar stützt sich LUIS auf maschinelles Lernen, das Verstehen von maschinellem Lernen ist aber für die Verwendung nicht erforderlich. Stattdessen vermitteln Machine Teachers LUIS Konzepte, indem sie positive und negative Beispiele des Konzepts vorstellen und mithilfe anderer verwandter Konzepte erläutern, wie ein Konzept modelliert werden sollte. Diese Lehrer können das Modell von LUIS außerdem interaktiv verbessern, indem sie Vorhersagefehler ermitteln und beheben. 

## <a name="v3-authoring-model-decomposition"></a>Aufschlüsselung des V3-Erstellungsmodells

LUIS unterstützt die _Modellaufschlüsselung_ mithilfe der V3-Erstellungs-APIs, bei der das Modell in kleinere Teile aufgegliedert wird. Auf diese Weise können Sie ihre Modelle mit der Zuversicht aufbauen, die sich aus der Kenntnis von Konstruktion und Vorhersagemechanismen der einzelnen Teile speist.

Die Aufschlüsselung von Modellen setzt sich aus folgenden Teilen zusammen:

* [Absichten](#intents-classify-utterances)
    * Von Features bereitgestellte [Deskriptoren](#descriptors-are-features)
* [Durch maschinelles Lernen erworbene Entitäten](#machine-learned-entities)
    * [Unterkomponenten](#entity-subcomponents-help-extract-data) (ebenfalls durch maschinelles Lernen gewonnene Entitäten)
        * Von Features bereitgestellte [Deskriptoren](#descriptors-are-features) 
        * Von nicht durch maschinelles Lernen erworbenen Entitäten bereitgestellte [Einschränkungen](#constraints-are-text-rules) (reguläre Ausdrücke und Listen).

## <a name="v2-authoring-models"></a>V2-Erstellungsmodelle

LUIS unterstützt zusammengesetzte Entitäten mit den V2-Erstellungs-APIs. Damit steht eine ähnliche Modellaufschlüsselung zur Verfügung, die sich jedoch von der Aufschlüsselung von V3-Modellen unterscheidet. In der empfohlenen Modellarchitektur wird die Modellaufschlüsselung in die V3-Erstellungs-APIs verlagert. 

## <a name="intents-classify-utterances"></a>Klassifizieren von Äußerungen durch Absichten

Beispieläußerungen werden durch eine Absicht klassifiziert, um LUIS Informationen über die Absicht zu vermitteln. Beispieläußerungen innerhalb einer Absicht werden als positive Beispiele der Äußerung verwendet. Diese gleichen Äußerungen werden in allen anderen Absichten als negative Beispiele verwendet.

Stellen Sie sich eine App vor, die die Absicht eines Benutzers bestimmen muss, ein Buch zu bestellen, und eine App, die die Versandadresse für den Kunden benötigt. Diese App weist zwei Absichten auf: `OrderBook` und `ShippingLocation`.

Die folgende Äußerung ist ein **positives Beispiel** für die Absicht `OrderBook` und ein **negatives Beispiel** für die Absichten `ShippingLocation` und `None`: 

`Buy the top-rated book on bot architecture.`

Das Ergebnis gut entworfener Absichten mit ihren Beispieläußerungen ist eine hohe Absichtsvorhersage. 

## <a name="entities-extract-data"></a>Extrahieren von Daten durch Entitäten

Eine Entität stellt eine Dateneinheit dar, die aus der Äußerung extrahiert werden soll. 

### <a name="machine-learned-entities"></a>Durch maschinelles Lernen erworbene Entitäten

Eine durch maschinelles Lernen erworbene Entität ist eine Entität der obersten Ebene, die Unterkomponenten enthält, die ihrerseits durch maschinelles Lernen erworbene Entitäten sind. 

**Verwenden Sie eine durch maschinelles Lernen erworbene Entität**:

* wenn die Unterkomponenten von der Clientanwendung benötigt werden
* um den Algorithmus für maschinelles Lernen beim Aufschlüsseln von Entitäten zu unterstützen

Jede Unterkomponente kann über Folgendes verfügen:

* Unterkomponenten
* Einschränkungen (Entität für reguläre Ausdrücke oder Listenentität)
* Deskriptoren (Features wie etwa eine Ausdrucksliste) 

Ein Beispiel für eine durch maschinelles Lernen erworbene Entität ist eine Bestellung eines Flugtickets. Konzeptionell handelt es sich dabei um eine einzelne Transaktion mit vielen kleineren Dateneinheiten, wie etwa Datum, Uhrzeit, Menge der Plätze, Art der Plätze (wie Erste Klasse oder Economy), Startort, Zielort und Auswahl des Bordmenüs.


### <a name="entity-subcomponents-help-extract-data"></a>Unterkomponenten von Entitäten helfen bei der Extraktion von Daten

Eine Unterkomponente ist eine durch maschinelles Lernen erworbene untergeordnete Entität innerhalb einer durch maschinelles Lernen erworbenen übergeordneten Entität. 

**Verwenden Sie die Unterkomponente für diesen Zweck**:

* Aufschlüsseln der Teile der durch maschinelles Lernen erworbenen Entität (übergeordnete Entität).

Das Folgende stellt eine durch maschinelles Lernen erworbene Entität mit all diesen separaten Datenstücken dar:

* TravelOrder (Reiseauftrag; durch maschinelles Lernen erworbene Entität)
    * DateTime (Datum und Uhrzeit; vordefinierte datetimeV2)
    * Location (Standort; durch maschinelles Lernen erworbene Entität)
        * Origin (Startort; durch Kontext wie etwa `from` gefundene Rolle)
        * Destination (Zielort; durch Kontext wie etwa `to` gefundene Rolle)
    * Seating (Platzierung; durch maschinelles Lernen erworbene Entität)
        * Quantity (Menge; vordefinierte Zahl)
        * Quality (Qualität; durch maschinelles Lernen erworbene Entität mit Deskriptor von Ausdrucksliste)
    * Meals (Bordmenü; durch maschinelles Lernen erworbene Entität mit Einschränkung von Listenentität als Speisenoptionen)

Einige dieser Daten, wie etwa der Startort und der Zielort, sollten aus dem Kontext der Äußerung gelernt werden, etwa mit Formulierungen wie `from` und `to`. Andere Teile der Daten können mit genauen Übereinstimmungen von Zeichenfolgen (`Vegan`) oder vordefinierten Entitäten (geographyV2 von `Seattle` und `Cairo`) extrahiert werden. 

Sie entwerfen die Weise, in der die Daten abgeglichen und extrahiert werden, durch Ihre Wahl der Modelle und deren Konfiguration.

### <a name="constraints-are-text-rules"></a>Einschränkungen sind Textregeln.

Eine Einschränkung ist eine Regel zum Textvergleich, die von einer nicht durch maschinelles Lernen erworbenen Entität bereitgestellt wird, etwa der Entität für reguläre Ausdrücke oder einer Listenentität. Die Einschränkung wird zur Vorhersagezeit angewendet, um die Vorhersage einzuschränken und die von der Clientanwendung benötigte Entitätsauflösung bereitzustellen. Sie definieren diese Regeln beim Erstellen der Unterkomponente. 

**Verwenden Sie eine Einschränkung**:
* wenn Sie den exakten Text kennen, der extrahiert werden soll.

Zu den Einschränkungen gehören:

* [RegEx](reference-entity-regular-expression.md)-Entitäten
* [Listenentitäten](reference-entity-list.md) 
* [vordefinierte](luis-reference-prebuilt-entities.md) Entitäten

Um das Beispiel des Flugtickets fortzusetzen, die Flughafencodes können sich in einer Listenentität für genaue Textübereinstimmungen befinden. 

Bei einer Flughafenliste besteht der Eintrag für Seattle aus dem Namen der Stadt, `Seattle` und die Synonyme für Seattle schließen den Flughafencode für Seattle zusammen mit Städten und Orten der Umgebung ein:

|Synonyme in der `Seattle`-Listenentität|
|--|
|`Sea`|
|`seatac`|
|`Bellevue`|

Wenn Sie nur aus drei Buchstaben bestehende Codes für Flughäfen erkennen möchten, verwenden Sie als Einschränkung einen regulären Ausdruck. 

`/^[A-Z]{3}$/`

## <a name="intents-versus-entities"></a>Absichten im Vergleich mit Entitäten

Eine Absicht ist das gewünschte Ergebnis der _gesamten_ Äußerung, während Entitäten Datenstücke darstellen, die aus der Äußerung extrahiert wurden. Normalerweise sind Absichten an Aktionen gebunden, die von der Clientanwendung ausgeführt werden sollen, und Entitäten sind die Informationen, die für das Durchführen dieser Aktion erforderlich sind. Aus Programmiererperspektive würde eine Absicht einen Methodenaufruf auslösen, und die Entitäten würden als Parameter dieses Methodenaufrufs fungieren.

Diese Äußerung _muss_ eine Absicht und _kann_ Entitäten aufweisen:

`Buy a airline ticket from Seattle to Cairo`

Diese Äußerung verfolgt ein einzelnes Ziel:

* den Kauf eines Flugtickets

Diese Äußerung _kann_ verschiedene Entitäten aufweisen:

* Die Orte Seattle (Startort) und Kairo (Zielort)
* Die Menge eines einzelnen Tickets

## <a name="descriptors-are-features"></a>Deskriptoren sind Features.

Ein Deskriptor ist ein Feature, das zur Trainingszeit auf ein Modell angewendet wird, einschließlich Ausdruckslisten und Entitäten. 

**Verwenden Sie einen Deskriptor, wenn Sie dies möchten**:

* die Bedeutsamkeit von Wörtern und Ausdrücken steigern, die vom Deskriptor bezeichnet werden
* LUIS neue Texte oder Ausdrücke als Empfehlung für den Deskriptor vorschlagen lassen
* einen Fehler in den Trainingsdaten beheben

## <a name="next-steps"></a>Nächste Schritte

* Grundlagen von [Absichten](luis-concept-intent.md) und [Entitäten](luis-concept-entity-types.md). 