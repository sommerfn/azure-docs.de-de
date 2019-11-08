---
title: Entitätstypen – LUIS
titleSuffix: Azure Cognitive Services
description: 'Entitäten extrahieren Daten aus der Äußerung. Entitätstypen ermöglichen eine vorhersagbare Extraktion von Daten. Es gibt zwei Arten von Entitäten: maschinell erlernte und nicht maschinell erlernte. Es ist wichtig zu wissen, mit welcher Art von Entität Sie in Äußerungen arbeiten.'
services: cognitive-services
author: diberry
manager: nitinme
ms.custom: seodec18
ms.service: cognitive-services
ms.subservice: language-understanding
ms.topic: conceptual
ms.date: 10/25/2019
ms.author: diberry
ms.openlocfilehash: 9dc26e50e1c0f43e816e422f0fee91a246ea04a9
ms.sourcegitcommit: c22327552d62f88aeaa321189f9b9a631525027c
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 11/04/2019
ms.locfileid: "73487600"
---
# <a name="entities-and-their-purpose-in-luis"></a>Entitäten und ihr Zweck in LUIS

Der primäre Zweck von Entitäten besteht darin, der Clientanwendung eine vorhersagbare Extraktion von Daten zu ermöglichen. Ein _optionaler_ sekundärer Zweck besteht darin, die Vorhersage der Absicht mit Deskriptoren zu verbessern. 

Es gibt zwei Arten von Entitäten: 

* durch maschinelles Lernen erworben – im Kontext
* nicht durch maschinelles Lernen erworben – für genaue Textübereinstimmungen

Beginnen Sie immer mit einer durch maschinelles Lernen erworbenen Entität, da diese die größte Auswahl an Datenextraktionen bietet.

## <a name="entity-compared-to-intent"></a>Entität im Vergleich zu Absichten

Die Entität stellt ein Datenkonzept innerhalb der Äußerung dar, die Sie extrahieren möchten. 

Gehen Sie von den folgenden drei Äußerungen aus:

|Äußerung|Daten extrahiert|Erklärung|
|--|--|--|
|`Help`|-|Nichts zu extrahieren.|
|`Send Bob a present`|Bob, vorhanden|Bob ist definitiv wichtig, um die Aufgabe abzuschließen. Die vorhandenen Informationen sind möglicherweise ausreichend, oder der Bot muss ggf. die aktuelle Frage mit einer Anschlussfrage klären.|
|`Send Bob a box of chocolates.`|Die beiden Daten („Bob“ und „box of chocolates“) sind wichtig, um die Anforderung des Benutzers abzuschließen.|

Eine Äußerung kann viele Entitäten oder auch überhaupt keine enthalten. Eine Clientanwendung benötigt _eventuell_ die Entität, um ihre Aufgabe auszuführen. 

Im Vergleich dazu ist die Vorhersage der Absicht für eine Äußerung _erforderlich_ und stellt die gesamte Äußerung dar. Für LUIS müssen die Beispieläußerungen in einer Absicht enthalten sind. Wenn die primäre Absicht der Äußerung für die Clientanwendung nicht wichtig ist, fügen Sie alle Äußerungen der Absicht „None“ hinzu. 

Wenn Sie zu einem späteren Zeitpunkt im App-Lebenszyklus feststellen, dass Sie die Äußerungen unterteilen möchten, können Sie dies problemlos erledigen. Dadurch können Sie die Äußerungen bei der Erstellung organisieren, oder Sie können die vorhergesagte Absicht in der Clientanwendung verwenden. 

Es ist nicht erforderlich, die vorhergesagte Absicht in der Clientanwendung zu verwenden, sie wird jedoch als Teil der Antwort vom Vorhersageendpunkt zurückgegeben.

## <a name="entities-represent-data"></a>Darstellen von Daten durch Entitäten

Entitäten sind Daten, die Sie aus der Äußerung pullen möchten. Dabei kann es sich um einen Namen, ein Datum, einen Produktnamen oder eine Wortgruppe handeln. 

|Äußerung|Entität|Data|
|--|--|--|
|Kaufe 3 Tickets nach New York|Vordefinierte Anzahl<br>Location.Destination|3<br>New York|
|Kaufe ein Ticket von New York nach London am 5. März|Location.Origin<br>Location.Destination<br>Vordefinierte datetimeV2|New York<br>London<br>5\. März 2018|

## <a name="entities-are-optional-but-highly-recommended"></a>Entitäten sind optional, jedoch dringend empfohlen.

Im Gegensatz zu Absichten sind Entitäten optional. Sie müssen keine Entitäten für sämtliche Konzepte Ihrer App erstellen, sondern lediglich für die, die für Aktionen der Clientanwendung erforderlich sind. 

Wenn Ihre Äußerungen keine Details enthalten, die Ihr Bot für das Fortfahren benötigt, müssen Sie diese auch nicht hinzufügen. Sie können sie bei fortschreitender Entwicklung Ihrer App später hinzufügen. 

Gehen Sie wie folgt vor, wenn Sie nicht sicher sind, wie Sie die Informationen verwenden können: Fügen Sie einige allgemeine vordefinierte Entitäten, z.B. [datetimeV2](luis-reference-prebuilt-datetimev2.md), [ordinal](luis-reference-prebuilt-ordinal.md), [email](luis-reference-prebuilt-email.md) und [phonenumber](luis-reference-prebuilt-phonenumber.md) hinzu.

## <a name="design-entities-for-decomposition"></a>Entwerfen von Entitäten zur Analyse

Gehen Sie beim Entwerfen Ihrer Entität von einer durch maschinelles Lernen erworbenen Entität aus. So können Sie Ihre Entität im Lauf der Zeit ganz einfach erweitern und ändern. Fügen Sie **Unterkomponenten** (untergeordnete Entitäten) mit **Einschränkungen** und **Deskriptoren** hinzu, um das Entwerfen der Entitäten abzuschließen. 

Der Entwurf für die Analyse bietet LUIS eine genauere Aufschlüsselung der Entitäten für Ihre Clientanwendung. Dadurch kann sich die Clientanwendung auf Geschäftsregeln konzentrieren und die Datenauflösung LUIS überlassen.

### <a name="machine-learned-entities-are-primary-data-collections"></a>Durch maschinelles Lernen erworbene Entitäten sind primäre Datensammlungen

Durch maschinelles Lernen erworbene Entitäten sind die Dateneinheit auf oberster Ebene. Unterkomponenten sind untergeordnete Entitäten von durch maschinelles Lernen erworbenen Entitäten. 

**Einschränkungen** sind Entitäten mit genauer Textübereinstimmung, die Regeln zum Identifizieren und Extrahieren von Daten anwenden. **Deskriptoren** sind Features, mit denen die Relevanz von Wörtern oder Ausdrücken für die Vorhersage erhöht wird.

<a name="composite-entity"></a>
<a name="list-entity"></a>
<a name="patternany-entity"></a>
<a name="prebuilt-entity"></a>
<a name="regular-expression-entity"></a>
<a name="simple-entity"></a>

## <a name="types-of-entities"></a>Typen von Entitäten

Wählen Sie die Entität basierend darauf aus, wie die Daten extrahiert und nach der Extraktion dargestellt werden sollen.

|Entitätstyp|Zweck|
|--|--|
|[**Durch maschinelles Lernen erworben**](#composite-entity)|Übergeordnete Gruppierung von Entitäten, unabhängig vom Entitätstyp. Durch maschinelles Lernen erworbene Entitäten lernen über den Kontext der Äußerung. Aus diesem Grund ist die Variation der Platzierung in Beispieläußerungen wichtig. |
|[**Liste**](#list-entity)|Liste mit Elementen und den zugehörigen Synonymen, die per **genauer Textübereinstimmung** extrahiert werden.|
|[**Pattern.any**](#patternany-entity)|Entität, bei der das Entitätsende schwierig zu ermitteln ist. |
|[**Vordefiniert**](#prebuilt-entity)|Bereits trainierte oder extrahierte spezielle Daten URLs oder E-Mail-Adressen. Einige dieser vordefinierten Entitäten werden im Open-Source-Projekt [Recognizers-Text](https://github.com/Microsoft/Recognizers-Text) definiert. Wenn Ihre Kultur oder Entität derzeit nicht unterstützt wird, können Sie sich am Projekt beteiligen.|
|[**Regulärer Ausdruck**](#regular-expression-entity)|Verwendet einen regulären Ausdruck für **genaue Textübereinstimmungen**.|

### <a name="entity-role-defines-context"></a>Die Entitätsrolle definiert den Kontext.

Die Rolle einer Entität ist der benannte Alias, der auf dem Kontext innerhalb der Äußerung beruht. Ein Beispiel ist eine Äußerung zum Buchen eines Flugs mit zwei Orten: Abflugs- und Ankunftsort.

`Book a flight from Seattle to Cairo`

Die zwei Beispiele einer `location`-Entität müssen extrahiert werden. Die Clientanwendung muss den jeweiligen Ort kennen, um den Ticketkauf abschließen zu können. Die `location`-Entität benötigt die beiden Rollen von `origin` und `destination`, und beide müssen in den Beispieläußerungen gekennzeichnet werden. 

Wenn LUIS zwar `location` findet, aber die Rolle nicht ermitteln kann, wird die Entität für den Ort trotzdem zurückgegeben. Die Clientanwendung muss eine Anschlussfrage stellen, um zu ermitteln, welche Art von Ort der Benutzer gemeint hat. 

Mehrere Entitäten können in einer Äußerung existieren und können ohne Verwendung von Rollen extrahiert werden. Wenn der Kontext des Satzes den Entitätswert angibt, sollte eine Rolle verwendet werden.

Wenn die Äußerung eine Liste von Orten enthält, `I want to travel to Seattle, Cairo, and London.`, ist dies eine Liste, in der jedes Element keine zusätzliche Bedeutung hat. 

## <a name="if-you-need-more-than-the-maximum-number-of-entities"></a>Wenn mehr als die maximale Anzahl von Entitäten erforderlich ist 

Wenn Sie mehr als das Limit benötigen, wenden Sie sich an den Support. Sammeln Sie dazu ausführliche Informationen über Ihr System, navigieren Sie zur [LUIS](luis-reference-regions.md#luis-website)-Website, und wählen Sie dann **Support** aus. Wenn Ihr Azure-Abonnement Supportdienste umfasst, wenden Sie sich an den [technischen Support von Azure](https://azure.microsoft.com/support/options/). 

## <a name="entity-prediction-status"></a>Status der Entitätsvorhersage

Im LUIS-Portal wird angezeigt, wenn die Entität in einer Beispieläußerung eine andere Entitätsvorhersage aufweist als die von Ihnen ausgewählte Entität. Der Unterschied beim Score kommt vom aktuellen trainierten Modell. 

## <a name="next-steps"></a>Nächste Schritte

Erfahren Sie mehr über die Konzepte für gute [Äußerungen](luis-concept-utterance.md). 

Weitere Informationen zum Hinzufügen von Entitäten zu LUIS-Apps finden Sie unter [Hinzufügen von Entitäten](luis-how-to-add-entities.md).
