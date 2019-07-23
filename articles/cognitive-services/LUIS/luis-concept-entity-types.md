---
title: Entitätstypen
titleSuffix: Language Understanding - Azure Cognitive Services
description: 'Entitäten extrahieren Daten aus der Äußerung. Entitätstypen ermöglichen eine vorhersagbare Extraktion von Daten. Es gibt zwei Arten von Entitäten: maschinell erlernte und nicht maschinell erlernte. Es ist wichtig zu wissen, mit welcher Art von Entität Sie in Äußerungen arbeiten.'
services: cognitive-services
author: diberry
manager: nitinme
ms.custom: seodec18
ms.service: cognitive-services
ms.subservice: language-understanding
ms.topic: conceptual
ms.date: 06/12/2019
ms.author: diberry
ms.openlocfilehash: 628a96c4e912341226d67a7ed8f241194e7b7825
ms.sourcegitcommit: 41ca82b5f95d2e07b0c7f9025b912daf0ab21909
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 06/13/2019
ms.locfileid: "67080037"
---
# <a name="entity-types-and-their-purposes-in-luis"></a>Entitätstypen und ihr Zweck in LUIS

Entitäten extrahieren Daten aus der Äußerung. Entitätstypen ermöglichen eine vorhersagbare Extraktion von Daten. Es gibt zwei Arten von Entitäten: maschinell erlernte und nicht maschinell erlernte. Es ist wichtig zu wissen, mit welcher Art von Entität Sie in Äußerungen arbeiten. 

## <a name="entity-compared-to-intent"></a>Entität im Vergleich zu Absichten

Die Entität stellt ein Wort oder einen Ausdruck innerhalb der Äußerung dar, die extrahiert werden soll. Eine Äußerung kann viele Entitäten oder auch überhaupt keine enthalten. Eine Clientanwendung kann es erforderlich machen, dass die Entität ihre Aufgabe erfüllt, oder sie kann sie als Richtlinie für mehrere Auswahlmöglichkeiten verwenden, die dem Benutzer präsentiert werden. 

Eine Entität:

* Stellt eine Klasse dar, einschließlich einer Sammlung ähnlicher Objekte (Orte, Dinge, Personen, Ereignisse oder Konzepte). 
* Beschreibt Informationen, die für die Absicht relevant sind


Eine App zur News-Suche kann beispielsweise Entitäten wie „Thema“, „Quelle“, „Schlüsselwort“ und „Veröffentlichungsdatum“ enthalten, die besonders wichtig für die Suche nach Nachrichten sind. In einer Reisebuchungs-App sind „Ort“, „Datum“, „Airline“, „Reiseklasse“ und „Tickets“ Schlüsselinformationen für die Flugbuchung (relevant für die Absicht „Flug buchen“).

Im Vergleich dazu stellt die Absicht die Vorhersage der gesamten Äußerung dar. 

## <a name="entities-help-with-data-extraction-only"></a>Entitäten dienen nur als Hilfe bei der Datenextraktion

Sie beschriften bzw. kennzeichnen Entitäten nur für die Entitätsextraktion. Die Vorhersage von Absichten wird hierdurch nicht unterstützt.

## <a name="entities-represent-data"></a>Darstellen von Daten durch Entitäten

Entitäten sind Daten, die Sie aus der Äußerung pullen möchten. Dabei kann es sich um einen Namen, ein Datum, einen Produktnamen oder eine Wortgruppe handeln. 

|Äußerung|Entität|Daten|
|--|--|--|
|Kaufe 3 Tickets nach New York|Vordefinierte Anzahl<br>Location.Destination|3<br>New York|
|Kaufe ein Ticket von New York nach London am 5. März|Location.Origin<br>Location.Destination<br>Vordefinierte datetimeV2|New York<br>London<br>5\. März 2018|

## <a name="entities-are-optional-but-highly-recommended"></a>Entitäten sind optional, jedoch dringend empfohlen.

Im Gegensatz zu Absichten sind Entitäten optional. Sie müssen keine Entitäten für sämtliche Konzepte Ihrer App erstellen, sondern lediglich für die, die für Aktionen der Clientanwendung erforderlich sind. 

Wenn Ihre Äußerungen keine Details enthalten, die Ihr Bot für das Fortfahren benötigt, müssen Sie diese auch nicht hinzufügen. Sie können sie bei fortschreitender Entwicklung Ihrer App später hinzufügen. 

Gehen Sie wie folgt vor, wenn Sie nicht sicher sind, wie Sie die Informationen verwenden können: Fügen Sie einige allgemeine vordefinierte Entitäten, z.B. [datetimeV2](luis-reference-prebuilt-datetimev2.md), [ordinal](luis-reference-prebuilt-ordinal.md), [email](luis-reference-prebuilt-email.md) und [phonenumber](luis-reference-prebuilt-phonenumber.md) hinzu.

## <a name="label-for-word-meaning"></a>Bezeichnungen für Wortbedeutungen

Wenn die Wortauswahl oder die Anordnung der Wörter identisch ist, sie aber nicht dieselbe Bedeutung haben, sollten Sie sie nicht mit der Entität bezeichnen. 

In den folgenden Äußerungen ist das Wort `fair` ein Homograph. Die Schreibweise ist identisch, aber die Bedeutung ist eine andere:

|Äußerung|
|--|
|Welche Arten von Jahrmärkten finden in diesem Sommer in der Region Seattle statt?|
|Ist die aktuelle Bewertung für die Seattle-Rezension fair?|

Wenn Sie mit einer Veranstaltungsentität alle Veranstaltungsdaten suchen möchten, bezeichnen Sie das Wort `fair` in der ersten Äußerung, aber nicht in der zweiten.

## <a name="entities-are-shared-across-intents"></a>Wiederverwenden von Entitäten für mehrere Absichten

Entitäten werden für mehrere Absichten verwendet. Sie gehören nicht zu einer bestimmten Absicht. Absichten und Entitäten können semantisch zugeordnet werden, aber es handelt sich nicht um eine exklusive Beziehung.

In der Äußerung „Buche mir ein Ticket nach Paris“ ist „Paris“ eine Entität, die auf den Ort verweist. Indem die in der Äußerung des Benutzers erwähnten Entitäten erkannt werden, unterstützt LUIS Ihre Clientanwendung beim Auswählen der jeweiligen Aktionen, die zum Erfüllen der Benutzeranforderung durchgeführt werden müssen.

## <a name="mark-entities-in-none-intent"></a>Kennzeichnen von Entitäten der Absicht „Keine“

Alle Absichten, auch **Keine**, sollten nach Möglichkeit über gekennzeichnete Entitäten verfügen. Auf diese Weise kann LUIS mehr darüber erfahren, wo sich die Entitäten in den Äußerungen befinden und welche Wörter sich um die Entitäten herum befinden. 

## <a name="entity-status-for-predictions"></a>Vorhersagen zum Entitätsstatus

Sie werden im LUIS-Portal darüber informiert, wenn die Entität in einer Beispieläußerung entweder von der gekennzeichneten Entität abweicht oder einer anderen Entität zu stark ähnelt und daher nicht eindeutig ist. Dies wird in der Beispieläußerung durch eine rote Unterstreichung angezeigt. 

Weitere Informationen finden Sie unter [Vorhersagen zum Entitätsstatus](luis-how-to-add-example-utterances.md#entity-status-predictions). 

## <a name="types-of-entities"></a>Typen von Entitäten

LUIS verfügt über viele Arten von Entitäten. Wählen Sie die Entität basierend darauf aus, wie die Daten extrahiert und nach der Extraktion dargestellt werden sollen.

Entitäten können mit maschinellem Lernen extrahiert werden. So kann LUIS weiterhin lernen, wie die Entität in der Äußerung angezeigt wird. Entitäten können auch ohne maschinelles Lernen extrahiert werden, indem entweder ein Abgleich auf den genauen Text oder anhand eines regulären Ausdrucks durchgeführt wird. Entitäten in Mustern können mit einer gemischten Implementierung extrahiert werden. 

Nachdem die Entität extrahiert wurde, können die Entitätsdaten als einzelne Informationseinheit dargestellt oder mit anderen Entitäten kombiniert werden, damit sie eine Informationseinheit bilden, die von der Clientanwendung verwendet werden kann.

|Durch maschinelles Lernen erworben|Kennzeichnung möglich|Tutorial|Beispiel<br>response|Entitätstyp|Zweck|
|--|--|--|--|--|--|
|✔|✔|[✔](luis-tutorial-composite-entity.md)|[✔](luis-concept-data-extraction.md#composite-entity-data)|[**Zusammengesetzt**](#composite-entity)|Gruppierung von Entitäten, unabhängig vom Entitätstyp.|
|||[✔](luis-quickstart-intent-and-list-entity.md)|[✔](luis-concept-data-extraction.md#list-entity-data)|[**Liste**](#list-entity)|Liste mit Elementen und den zugehörigen Synonymen, die per genauer Textübereinstimmung extrahiert werden.|
|Gemischt||[✔](luis-tutorial-pattern.md)|[✔](luis-concept-data-extraction.md#patternany-entity-data)|[**Pattern.any**](#patternany-entity)|Entität, bei der das Entitätsende schwierig zu ermitteln ist.|
|||[✔](luis-tutorial-prebuilt-intents-entities.md)|[✔](luis-concept-data-extraction.md#prebuilt-entity-data)|[**Vordefiniert**](#prebuilt-entity)|Bereits trainiert, um verschiedene Arten von Daten zu extrahieren.|
|||[✔](luis-quickstart-intents-regex-entity.md)|[✔](luis-concept-data-extraction.md#regular-expression-entity-data)|[**Regulärer Ausdruck**](#regular-expression-entity)|Verwendet einen regulären Ausdruck zum Abgleichen von Text.|
|✔|✔|[✔](luis-quickstart-primary-and-secondary-data.md)|[✔](luis-concept-data-extraction.md#simple-entity-data)|[**Einfach**](#simple-entity)|Enthält ein einzelnes Konzept in einem Wort oder Ausdruck.|

Nur Entitäten, die durch maschinelles Lernen erworben wurden, müssen in den Beispieläußerungen gekennzeichnet werden. Durch maschinelles Lernen erworbene Entitäten funktionieren am besten, wenn sie mithilfe von [Endpunktabfragen](luis-concept-test.md#endpoint-testing) und durch das [Überprüfen der Endpunktäußerungen](luis-how-to-review-endoint-utt.md) getestet werden. 

Pattern.any-Entitäten müssen in [Pattern](luis-how-to-model-intent-pattern.md)-Vorlagenbeispielen gekennzeichnet werden, nicht in den Absichtsbenutzerbeispielen. 

Für gemischte Entitäten wird eine Kombination aus Methoden für die Entitätserkennung verwendet.

## <a name="machine-learned-entities-use-context"></a>Durch maschinelles Lernen erworbene Entitäten nutzen Kontext

Durch maschinelles Lernen erworbene Entitäten lernen über den Kontext der Äußerung. Aus diesem Grund ist die Variation der Platzierung in Beispieläußerungen wichtig. 

## <a name="non-machine-learned-entities-dont-use-context"></a>Entitäten, die nicht durch maschinelles Lernen erworben wurden, nutzen den Kontext nicht

Die folgenden, nicht durch maschinelles Lernen erworbenen Entitäten berücksichtigen beim Vergleichen von Entitäten den Kontext einer Äußerung nicht: 

* [Vordefinierte Entitäten](#prebuilt-entity)
* [RegEx-Entitäten](#regular-expression-entity)
* [List-Entitäten](#list-entity) 

Diese Entitäten erfordern keine Bezeichnungen oder ein Trainieren des Modells. Nach dem Hinzufügen oder Konfigurieren der Entität, werden die Entitäten extrahiert. Der Nachteil hierbei sind möglicherweise zu viele Übereinstimmungen, die bei einer Berücksichtigung des Kontexts nicht zurückgegeben worden wären. 

Dies tritt bei List-Entitäten in neuen Modellen häufig auf. Sie erstellen und testen Ihr Modell mit einer List-Entität, aber wenn Sie Ihr Modell veröffentlichen und Abfragen vom Endpunkt erhalten, stellen Sie fest, dass das Modell aufgrund des fehlenden Kontexts zu viele Übereinstimmungen findet. 

Wenn Sie Wörter oder Ausdrücke finden und den Kontext berücksichtigen möchten, haben Sie zwei Möglichkeiten. Die erste ist eine einfache Entität zusammen mit einer Begriffsliste. Die Begriffsliste wird nicht für den Abgleich verwendet, sondern hilft stattdessen beim Hervorheben relativ ähnlicher Wörter (Liste austauschbarer Begriffe). Wenn Sie eine genaue Übereinstimmung anstelle von Varianten einer Begriffsliste benötigen, verwenden Sie eine List-Entität mit einer Rolle, die unten beschrieben wird.

### <a name="context-with-non-machine-learned-entities"></a>Kontext bei Entitäten, die nicht durch maschinelles Lernen erworben wurden

Wenn Sie bei Entitäten, die nicht durch maschinelles Lernen erworben wurden, den Kontext berücksichtigen möchten, sollten Sie [Rollen](luis-concept-roles.md) verwenden.

Wenn Sie über eine Entität, die nicht durch maschinelles Lernen erworben wurde, verfügen (z. B. [vordefinierte Entitäten](#prebuilt-entity), [RegEx](#regular-expression-entity)-Entitäten oder [List](#list-entity)-Entitäten), die zu mehr Übereinstimmung als gewünscht führen, sollten Sie eine Entität mit zwei Rollen erstellen. Eine Rolle erfasst, was Sie suchen, und die zweite Rolle erfasst, was Sie nicht suchen. Beide Versionen müssen in Beispieläußerungen bezeichnet werden.  

## <a name="composite-entity"></a>Entität vom Typ „Composite“

Eine zusammengesetzte Entität besteht aus anderen Entitäten, z. B. vom Typ Vordefiniert, Einfach, Regulärer Ausdruck oder Liste. Die einzelnen Entitäten bilden zusammen die gesamte Entität. 

Diese Entität ist gut geeignet, wenn für die Daten Folgendes gilt:

* Sind aufeinander bezogen. 
* Sie sind im Kontext der Äußerung miteinander verknüpft.
* Verwenden eine Vielzahl von Entitätstypen.
* Müssen gruppiert und von der Clientanwendung als eine Informationseinheit verarbeitet werden.
* Verfügen über viele verschiedene Benutzeräußerungen, für die maschinelles Lernen erforderlich ist.

![Entität vom Typ „Composite“](./media/luis-concept-entities/composite-entity.png)

[Tutorial](luis-tutorial-composite-entity.md)<br>
[JSON-Beispielantwort für Entität](luis-concept-data-extraction.md#composite-entity-data)<br>

## <a name="list-entity"></a>Entität vom Typ „List“

Listenentitäten stellen einen festen, abgeschlossenen Satz verwandter Wörter zusammen mit ihren Synonymen dar. LUIS ermittelt keine zusätzlichen Werte für Listenentitäten. Suchen Sie mithilfe des Features **Empfehlen** nach Vorschlägen für neue Wörter basierend auf der aktuellen Liste. Wenn mehr als eine Listenentität mit demselben Wert vorhanden ist, wird in der Endpunktabfrage jede Entität zurückgegeben. 

Diese Entität ist gut geeignet, wenn für die Textdaten Folgendes gilt:

* Es handelt sich um einen bekannten Satz.
* Sie werden nicht oft geändert. Wenn Sie die Liste häufig ändern oder möchten, dass die Liste selbstständig erweitert wird, stellt eine einfache Entität, die um eine Begriffsliste erweitert wurde, eine bessere Wahl dar. 
* Für den Satz werden die maximalen LUIS-[Grenzen](luis-boundaries.md) dieses Entitätstyps nicht überschritten.
* Der Text in der Äußerung ist eine exakte Übereinstimmung mit einem Synonym oder dem kanonischen Namen. LUIS verwendet die Liste über genaue Textübereinstimmungen hinaus nicht. Fuzzyübereinstimmungen, Nicht-Berücksichtigung der Groß- und Kleinschreibung, Wortstammerkennung, Pluralformen und andere Varianten lassen sich mit einer List-Entität nicht auflösen. Um Varianten zu behandeln, sollten Sie die Verwendung eines [Musters](luis-concept-patterns.md#syntax-to-mark-optional-text-in-a-template-utterance) mit der optionalen Textsyntax in Erwägung ziehen.

![Entität vom Typ „List“](./media/luis-concept-entities/list-entity.png)

[Tutorial](luis-quickstart-intent-and-list-entity.md)<br>
[JSON-Beispielantwort für Entität](luis-concept-data-extraction.md#list-entity-data)

## <a name="patternany-entity"></a>Entität „Pattern.any“

Pattern.any ist ein Platzhalter variabler Länge, der nur in der Vorlagenäußerung eines Musters verwendet wird, um zu kennzeichnen, wo die Entität beginnt und endet.  

Diese Entität ist gut geeignet, wenn Folgendes gilt:

* Die Endung der Entität kann leicht mit dem verbleibenden Text der Äußerung verwechselt werden. 
[Tutorial](luis-tutorial-pattern.md)<br>
[JSON-Beispielantwort für Entität](luis-concept-data-extraction.md#patternany-entity-data)

**Beispiel**  
Bei einer Clientanwendung, mit der anhand des Titels nach Büchern gesucht wird, wird mit pattern.any der vollständige Titel extrahiert. Eine Vorlagenäußerung mit pattern.any für diese Buchsuche lautet `Was {BookTitle} written by an American this year[?]` (Wurde {Buchtitel} in diesem Jahr von einem Autor bzw. einer Autorin aus den USA geschrieben?). 

In der folgenden Tabelle enthält jede Zeile zwei Versionen der Äußerung. In der oberen Äußerung ist dargestellt, wie LUIS die Äußerung zuerst sieht. Hierbei ist unklar, wo der Buchtitel beginnt und endet. Die untere Äußerung gibt an, wie LUIS den Buchtitel erkennen kann, wenn ein Muster für die Extraktion vorhanden ist. 

|Äußerung|
|--|
|Wurde das Buch The Man Who Mistook His Wife for a Hat and Other Clinical Tales in diesem Jahr von einem Autor bzw. einer Autorin aus den USA geschrieben?<br><br>Wurde das Buch **The Man Who Mistook His Wife for a Hat and Other Clinical Tales** in diesem Jahr von einem Autor bzw. einer Autorin aus den USA geschrieben?|
|Wurde Half Asleep in Frog Pajamas in diesem Jahr von einem Autor bzw. einer Autorin aus den USA geschrieben?<br><br>Wurde **Half Asleep in Frog Pajamas** in diesem Jahr von einem Autor bzw. einer Autorin aus den USA geschrieben?|
|Wurde The Particular Sadness of Lemon Cake: A Novel in diesem Jahr von einem Autor bzw. einer Autorin aus den USA geschrieben?<br><br>Wurde **The Particular Sadness of Lemon Cake: A Novel** in diesem Jahr von einem Autor bzw. einer Autorin aus den USA geschrieben?|
|Wurde There's A Wocket In My Pocket! in diesem Jahr von einem Autor bzw. einer Autorin aus den USA geschrieben?<br><br>Wurde **There's A Wocket In My Pocket!** in diesem Jahr von einem Autor bzw. einer Autorin aus den USA geschrieben?|
||

## <a name="prebuilt-entity"></a>Vordefinierte Entität

Bei vordefinierten Entitäten handelt es sich um integrierte Typen, mit denen häufig verwendete Konzepte, z.B. E-Mail, URL und Telefonnummer, dargestellt werden. Vordefinierte Entitätsnamen sind reserviert. [Alle vordefinierten Entitäten](luis-prebuilt-entities.md), die der Anwendung hinzugefügt werden, werden in der Endpunktvorhersage-Abfrage zurückgegeben, wenn sie in der Äußerung enthalten sind. 

Diese Entität ist gut geeignet, wenn Folgendes gilt:

* Die Daten stimmen mit einem häufigen Anwendungsfall überein, der von vordefinierten Entitäten für Ihre Sprachkultur unterstützt wird. 

Vordefinierte Entitäten können jederzeit hinzugefügt und entfernt werden.

![Vordefinierte Entität „number“](./media/luis-concept-entities/number-entity.png)

[Tutorial](luis-tutorial-prebuilt-intents-entities.md)<br>
[JSON-Beispielantwort für Entität](luis-concept-data-extraction.md#prebuilt-entity-data)

Einige dieser vordefinierten Entitäten werden im Open-Source-Projekt [Recognizers-Text](https://github.com/Microsoft/Recognizers-Text) definiert. Wenn Ihre Kultur oder Entität derzeit nicht unterstützt wird, können Sie sich am Projekt beteiligen. 

### <a name="troubleshooting-prebuilt-entities"></a>Problembehandlung bei vordefinierten Entitäten

Wenn anstelle Ihrer benutzerdefinierten Entität eine vordefinierte Entität markiert ist, haben Sie im LUIS-Portal mehrere Optionen zum Beheben dieses Problems.

Die der App hinzugefügten vordefinierten Entitäten werden _immer_ zurückgegeben, auch wenn aus der Äußerung benutzerdefinierte Entitäten für den gleichen Text extrahiert werden. 

#### <a name="change-tagged-entity-in-example-utterance"></a>Ändern der markierten Entität in der Beispieläußerung

Wenn die vordefinierte Entität dem Text oder den Token der benutzerdefinierten Entität entspricht, wählen Sie den Text in der Beispieläußerung aus, und ändern Sie die markierte Äußerung. 

Wenn die vordefinierte Entität mit mehr Text oder Token markiert ist als die benutzerdefinierte Entität, haben Sie zwei Optionen, um dies zu korrigieren:

* [Entfernen der Beispieläußerung](#remove-example-utterance-to-fix-tagging)
* [Entfernen der vordefinierten Entität](#remove-prebuilt-entity-to-fix-tagging)

#### <a name="remove-example-utterance-to-fix-tagging"></a>Entfernen der Beispieläußerung zur Korrektur der Markierung 

Die erste Option ist das Entfernen der Beispieläußerung. 

1. Löschen Sie die Beispieläußerung.
1. Trainieren Sie die App erneut. 
1. Fügen Sie nur das Wort oder den Ausdruck, das bzw. den die vordefinierte Entität markierte Entität darstellt, als vollständige Beispieläußerung ein. Für das Wort oder den Ausdruck ist weiterhin die vordefinierte Entität markiert. 
1. Wählen Sie auf der Seite **Absicht** die Entität in der Beispieläußerung aus, ändern Sie sie in die benutzerdefinierte Entität, und trainieren Sie die App erneut. Dies sollte verhindern, dass in LUIS genau dieser Text als vordefinierte Entität in Beispieläußerungen markiert wird, in denen dieser Text verwendet wird. 
1. Fügen Sie die vollständige ursprüngliche Beispieläußerung wieder auf der Seite „Absicht“ hinzu. Die benutzerdefinierte Entität sollte weiterhin anstelle der vordefinierten Entität markiert werden. Wenn die benutzerdefinierte Entität nicht markiert wird, müssen Sie weitere Beispiele für diesen Text in Äußerungen hinzufügen.

#### <a name="remove-prebuilt-entity-to-fix-tagging"></a>Entfernen der vordefinierten Entität zur Korrektur der Markierung

1. Entfernen Sie die vordefinierte Entität aus der App. 
1. Markieren Sie auf der Seite **Absicht** die benutzerdefinierte Entität in der Beispieläußerung.
1. Trainieren Sie die App.
1. Fügen Sie die vordefinierte Entität wieder in die App ein, und trainieren Sie die App. Bei dieser Korrektur wird davon ausgegangen, dass die vordefinierte Entität kein Teil einer zusammengesetzten Entität ist.

## <a name="regular-expression-entity"></a>Entität vom Typ „RegEx“ 

Ein regulärer Ausdruck ist am besten für unformatierten Text von Äußerungen geeignet. Die Groß-/Kleinschreibung sowie die Kultur werden ignoriert.  Reguläre Ausdrücke werden nach Änderungen durch die Rechtschreibprüfung auf Zeichenebene (nicht auf Tokenebene) angewandt. Wenn der reguläre Ausdruck zu komplex ist (z.B. mit zu vielen Klammern), können Sie den Ausdruck dem Modell nicht hinzufügen. Es wird ein Teil der [.NET Regex](https://docs.microsoft.com/dotnet/standard/base-types/regular-expressions)-Bibliothek verwendet, aber nicht die gesamte Bibliothek. 

Diese Entität ist gut geeignet, wenn Folgendes gilt:

* Die Daten sind einheitlich formatiert, und auch bei den Abweichungen herrscht Einheitlichkeit.
* Für den regulären Ausdruck sind nicht mehr als zwei Schachtelungsebenen erforderlich. 

![Entität vom Typ „RegEx“](./media/luis-concept-entities/regex-entity.png)

[Tutorial](luis-quickstart-intents-regex-entity.md)<br>
[JSON-Beispielantwort für Entität](luis-concept-data-extraction.md#regular-expression-entity-data)<br>

Reguläre Ausdrücke finden möglicherweise mehr, als Sie erwarten. Ein Beispiel hierfür ist eine numerische Wortsuche, wie z. B. `one` und `two`. Ein Beispiel ist der folgende reguläre Ausdruck, der die Zahl `one` zusammen mit anderen Zahlen findet:

```javascript
(plus )?(zero|one|two|three|four|five|six|seven|eight|nine)(\s+(zero|one|two|three|four|five|six|seven|eight|nine))*
``` 

Dieser RegEx-Ausdruck findet auch alle Wörter, die auf diese Zahlen enden, wie z. B. `phone`. Um Probleme wie dieses zu beheben, stellen Sie sicher, dass bei Übereinstimmungen für reguläre Ausdrücke die Wortgrenzen berücksichtigt werden. Der reguläre Ausdruck mit Berücksichtigung von Wortgrenzen für dieses Beispiel wird im folgenden regulären Ausdruck verwendet:

```javascript
\b(plus )?(zero|one|two|three|four|five|six|seven|eight|nine)(\s+(zero|one|two|three|four|five|six|seven|eight|nine))*\b
```

## <a name="simple-entity"></a>Entität vom Typ „Simple“ 

Eine einfache Entität ist eine generische Entität, die ein einzelnes Konzept beschreibt und im Kontext des maschinellen Lernens erworben wurde. Da es sich bei einfachen Entitäten normalerweise um Namen handelt, z.B. Unternehmensnamen, Produktnamen oder andere Namen, sollten Sie wie folgt vorgehen: Fügen Sie eine [Liste mit Ausdrücken](luis-concept-feature.md) hinzu, wenn Sie eine einfache Entität verwenden, um das Signal für die verwendeten Namen zu verstärken. 

Diese Entität ist gut geeignet, wenn Folgendes gilt:

* Die Daten sind nicht einheitlich formatiert, aber weisen auf denselben Sachverhalt hin. 

![Entität vom Typ „Simple“](./media/luis-concept-entities/simple-entity.png)

[Tutorial](luis-quickstart-primary-and-secondary-data.md)<br/>
[Beispielantwort für Entität](luis-concept-data-extraction.md#simple-entity-data)<br/>

## <a name="entity-limits"></a>Grenzwerte für Entitäten

Informieren Sie sich über die [Grenzwerte](luis-boundaries.md#model-boundaries), um zu erfahren, wie viele Entitäten jedes Typs Sie einem Modell hinzufügen können.

## <a name="if-you-need-more-than-the-maximum-number-of-entities"></a>Wenn mehr als die maximale Anzahl von Entitäten erforderlich ist 

Möglicherweise müssen Sie gemischte Entitäten in Kombination mit Entitätsrollen verwenden.

Zusammengesetzte Entitäten stellen Teile eines Ganzen dar. Die zusammengesetzte Entität PlaneTicketOrder könnte z.B. die untergeordneten Entitäten Airline, Destination, DepartureCity, DepartureDate und PlaneTicketClass enthalten.

LUIS verfügt auch über den Listenentitätstyp, der nicht per maschinellem Lernen erworben wird, sondern es Ihrer LUIS-App ermöglicht, eine feste Liste mit Werten anzugeben. In der Referenz zu [LUIS-Grenzen](luis-boundaries.md) finden Sie Informationen zu den Grenzwerten des Listenentitätstyps. 

Wenn Sie diese Entitäten berücksichtigt haben und trotzdem mehr als den Grenzwert benötigen, wenden Sie sich an den Support. Sammeln Sie dazu ausführliche Informationen über Ihr System, navigieren Sie zur [LUIS](luis-reference-regions.md#luis-website)-Website, und wählen Sie dann **Support** aus. Wenn Ihr Azure-Abonnement Supportdienste umfasst, wenden Sie sich an den [technischen Support von Azure](https://azure.microsoft.com/support/options/). 

## <a name="next-steps"></a>Nächste Schritte

Erfahren Sie mehr über die Konzepte für gute [Äußerungen](luis-concept-utterance.md). 

Weitere Informationen zum Hinzufügen von Entitäten zu LUIS-Apps finden Sie unter [Hinzufügen von Entitäten](luis-how-to-add-entities.md).
