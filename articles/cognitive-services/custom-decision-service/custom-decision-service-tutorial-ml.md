---
title: 'Tutorial: Featurebereitstellung und Featurespezifikationen – Custom Decision Service'
titlesuffix: Azure Cognitive Services
description: Ein Tutorial zur Featurebereitstellung und zu den Featurespezifikationen für maschinelles Lernen in Custom Decision Service.
services: cognitive-services
author: slivkins
manager: nitinme
ms.service: cognitive-services
ms.subservice: custom-decision-service
ms.topic: tutorial
ms.date: 05/08/2018
ms.author: slivkins
ROBOTS: NOINDEX
ms.openlocfilehash: 409dcf7dc224eaf2a3f51325010507b9182fadf2
ms.sourcegitcommit: ad9120a73d5072aac478f33b4dad47bf63aa1aaa
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 08/01/2019
ms.locfileid: "68707041"
---
# <a name="tutorial-featurization-and-feature-specification"></a>Tutorial: Featurebereitstellung und Featurespezifikationen

Dieses Tutorial behandelt die erweiterten Machine Learning-Funktionen in Custom Decision Service. Das Tutorial besteht aus zwei Teilen: [Featurebereitstellung](#featurization-concepts-and-implementation) und [Featurespezifikation](#feature-specification-format-and-apis). „Featurebereitstellung“ bezieht sich auf die Darstellung Ihrer Daten als „Features“ für Machine Learning. „Featurespezifikation“ umfasst das JSON-Format und die Hilfs-APIs zum Angeben von Features.

Standardmäßig ist Machine Learning im Custom Decision Service für den Kunden transparent. Features werden automatisch aus Ihrem Inhalt extrahiert, und es wird ein Standardverstärkungslernalgorithmus verwendet. Die Featureextraktion verwendet verschiedene weitere Azure Cognitive Services: [Entitätsverknüpfung](../entitylinking/home.md), [Textanalyse](../text-analytics/overview.md), [Emotionen](../emotion/home.md) und [Maschinelles Sehen](../computer-vision/home.md). Sie können dieses Tutorial überspringen, wenn nur die Standardfunktionen verwendet werden.

## <a name="featurization-concepts-and-implementation"></a>Featurebereitstellung: Konzepte und Implementierung

Custom Decision Service trifft Entscheidungen nacheinander. Jede Entscheidung umfasst die Auswahl zwischen verschiedenen Alternativen, die auch als „Aktionen“ bezeichnet werden. Je nach Anwendung kann die Entscheidung eine einzelne Aktion oder eine (kurze) Rangliste von Aktionen auswählen.

Beispielsweise die Personalisierung der Artikelauswahl auf der Startseite einer Website. Hier entsprechen die Aktionen den Artikeln, und jede Entscheidung besteht darin, welche Artikel einem bestimmten Benutzer angezeigt werden sollen.

Jede Aktion wird durch einen Vektor von Eigenschaften dargestellt, die als *Features* bezeichnet werden. Sie können zusätzlich zu den automatisch extrahierten Features neue Features angeben. Sie können Custom Decision Service auch anweisen, einige Features zu protokollieren, aber für Machine Learning zu ignorieren.

### <a name="native-vs-internal-features"></a>Native im Vergleich zu internen Features

Sie können Features in dem für Ihre Anwendung natürlichsten Format angeben, sei es eine Zahl, eine Zeichenfolge oder ein Array. Diese Features werden als „native Features“ bezeichnet. Custom Decision Service übersetzt jedes native Feature in mindestens ein numerisches Feature, das als „internes Feature“ bezeichnet wird.

Die Übersetzung in interne Features erfolgt folgendermaßen:

- Numerische Features bleiben gleich.
- Ein numerisches Array wird in mehrere numerische Features übersetzt, eines für jedes Element des Arrays.
- Ein Feature mit einem Zeichenfolgenwert `"Name":"Value"` wird standardmäßig in ein Feature mit dem Namen `"NameValue"` und dem Wert 1 übersetzt.
- Optional kann eine Zeichenfolge als [Bag-of-Words](https://en.wikipedia.org/wiki/Bag-of-words_model) dargestellt werden. Dann wird für jedes Wort in der Zeichenfolge ein internes Feature erstellt, dessen Wert die Anzahl der Vorkommen dieses Worts ist.
- Interne Features mit dem Wert NULL werden ausgelassen.

### <a name="shared-vs-action-dependent-features"></a>Gemeinsam verwendete im Vergleich zu aktionsabhängigen Features

Einige Features beziehen sich auf die gesamte Entscheidung und sind für alle Aktionen gleich. Diese werden als *gemeinsam verwendete Features* bezeichnet. Einige andere Features sind spezifisch für eine bestimmte Aktion. Diese werden als *aktionsabhängige Features* (Action-Dependent Features, ADFs) bezeichnet.

Im ausgeführten Beispiel könnten gemeinsame verwendete Features den Benutzer und/oder den Zustand der Welt beschreiben. Features wie Geolocation, Alter und Geschlecht des Benutzers, und welche wichtigen Ereignisse aktuell stattfinden. Aktionsabhängige Features könnten die Eigenschaften eines bestimmten Artikels beschreiben, z.B. die in diesem Artikel behandelten Themen.

### <a name="interacting-features"></a>Interagierende Features

Features interagieren häufig: Die Wirkung des einen hängt von anderen ab. Feature X gibt z.B. an, ob der Benutzer an Sport interessiert ist. Merkmal Y gibt an, ob es in einem bestimmten Artikel um Sport geht. In diesem Fall ist die Wirkung von Feature Y stark von Feature X abhängig.

Um die Interaktion zwischen den Features X und Y zu berücksichtigen, erstellen Sie ein *quadratisches* Feature, dessen Wert X\*Y ist. (Dies wird auch als „Kreuzen“ von X und Y bezeichnet.) Sie können auswählen, welche Featurepaare gekreuzt werden sollen.

> [!TIP]
> Ein gemeinsam verwendetes Feature sollte mit aktionsabhängigen Features gekreuzt werden, um deren Rang zu beeinflussen. Ein aktionsabhängiges Feature sollte mit gemeinsam verwendeten Features gekreuzt werden, um zur Personalisierung beizutragen.

Mit anderen Worten: Ein gemeinsam verwendetes Feature, das nicht mit einem ADF gekreuzt wird, beeinflusst jede Aktion auf die gleiche Weise. Ein ADF, das nicht mit einem gemeinsam verwendeten Feature gekreuzt wird, beeinflusst ebenfalls jede Entscheidung. Diese Art von Features kann die Varianz der Ergebnisabschätzungen verringern.

### <a name="implementation-via-namespaces"></a>Implementierung über Namespaces

Sie können gekreuzte Features (wie auch andere Featurebereitstellungskonzepte) über die „VW-Befehlszeile“ im Portal implementieren. Die Syntax basiert auf der [Vowpal Wabbit](http://hunch.net/~vw/)-Befehlszeile.

Im Mittelpunkt der Implementierung steht das Konzept des *Namespaces*: eine benannte Teilmenge von Features. Jedes Feature gehört zu genau einem Namespace. Der Namespace kann explizit angegeben werden, wenn der Featurewert Custom Decision Service zur Verfügung gestellt wird. Dies ist die einzige Möglichkeit, auf ein Feature in der VW-Befehlszeile zu verweisen.

Ein Namespace ist „gemeinsam verwendet“ oder „aktionsabhängig“: Entweder besteht er nur aus gemeinsam verwendeten Features, oder er besteht nur aus aktionsabhängigen Features der gleichen Aktion.

> [!TIP]
> Es ist empfehlenswert, Features in explizit angegebenen Namespaces zusammenzufassen. Gruppieren Sie verwandte Features im gleichen Namespace.

Wenn der Namespace nicht angegeben ist, wird das Feature automatisch dem Standardnamespace zugeordnet.

> [!IMPORTANT]
> Features und Namespaces müssen nicht aktionsübergreifend konsistent sein. Ein Namespace kann für verschiedene Aktionen unterschiedliche Features aufweisen. Außerdem kann ein bestimmter Namespace für einige Aktionen definiert werden, nicht jedoch für andere.

Mehrere interne Features, die aus dem gleichen nativen Feature mit einem Zeichenfolgenwert stammen, werden im gleichen Namespace gruppiert. Zwei native Features, die sich in unterschiedlichen Namespaces befinden, werden als verschieden behandelt, auch wenn sie den gleichen Featurenamen aufweisen.

> [!IMPORTANT]
> Während lange, beschreibende Namespace-IDs häufig sind, unterscheidet die VW-Befehlszeile nicht zwischen Namespaces, deren ID mit dem gleichen Buchstaben beginnt. Im Folgenden sind Namespace-IDs einzelne Buchstaben wie `x` oder `y`.

Die Implementierungsdetails sind wie folgt:

- Um die Namespaces `x` und `y` zu kreuzen, geben Sie `-q xy` oder `--quadratic xy` an. Anschließend wird jedes Feature in `x` mit jedem Feature in `y` gekreuzt. Verwenden Sie `-q x:`, um `x` mit jedem Namespace zu kreuzen, und `-q ::`, um alle Paare von Namespaces zu kreuzen.

- Um alle Features im Namespace `x` zu ignorieren, geben Sie `--ignore x`an.

Diese Befehle werden auf jede Aktion einzeln angewendet, wenn die Namespaces definiert werden.

### <a name="estimated-average-as-a-feature"></a>Geschätzter Durchschnitt als Feature

Was wäre als Gedankenexperiment das durchschnittliche Ergebnis für eine bestimmte Aktion, wenn sie für alle Entscheidungen ausgewählt würde? Ein solches durchschnittliches Ergebnis könnte als Maß für die „Gesamtqualität“ dieser Aktion herangezogen werden. Es ist nicht genau bekannt, wann in manchen Entscheidungen andere Aktionen ausgewählt wurden. Dies lässt sich jedoch mithilfe von Verstärkungslerntechniken schätzen. Die Qualität dieser Schätzung verbessert sich in der Regel mit der Zeit.

Sie können dieses „geschätzte durchschnittliche Ergebnis“ als Feature für eine bestimmte Aktion verwenden. Custom Decision Service würde diese Schätzung dann automatisch aktualisieren, sobald neue Daten verfügbar sind. Dieses Feature wird als das *marginale Feature* dieser Aktion bezeichnet. Marginale Features können für Machine Learning und für die Überwachung verwendet werden.

Wenn Sie marginale Features hinzufügen möchten, geben Sie `--marginal <namespace>` in die VW-Befehlszeile ein. Definieren Sie `<namespace>` in JSON wie folgt:

```json
{<namespace>: {"mf_name":1 "action_id":1}
```

Fügen Sie diesen Namespace zusammen mit anderen aktionsabhängigen Features einer bestimmten Aktion ein. Geben Sie diese Definition für jede Entscheidung an, indem Sie den gleichen `mf_name` und die gleiche `action_id` für alle Entscheidungen verwenden.

Das marginale Feature wird für jede Aktion mit `<namespace>` hinzugefügt. Die `action_id` kann jeder Featurename sein, der die Aktion eindeutig identifiziert. Der Featurename wird auf `mf_name` festgelegt. Insbesondere werden marginale Features mit unterschiedlichen `mf_name` als unterschiedliche Features behandelt: Für jeden `mf_name` wird eine andere Gewichtung gelernt.

Die Standardverwendung besteht darin, dass `mf_name` für alle Aktionen gleich ist. Dann wird eine Gewichtung für alle marginalen Features gelernt.

Sie können auch mehrere marginale Features für die gleiche Aktion mit gleichen Werten, aber unterschiedlichen Featurenamen angeben.

```json
{<namespace>: {"mf_name1":1 "action_id":1 "mf_name2":1 "action_id":1}}
```

### <a name="1-hot-encoding"></a>1-Hot-Codierung

Sie können einige Features als Bitvektoren darstellen, wobei jedes Bit einem Bereich von möglichen Werten entspricht. Dieses Bit wird auf 1 festgelegt, wenn und nur wenn das Feature in diesem Bereich liegt. Es gibt also ein „heißes“ Bit, das auf 1 festgelegt ist, und die anderen Bits werden auf 0 festgelegt. Diese Darstellung wird allgemein als *1-Hot-Codierung* bezeichnet.

Die 1-Hot-Codierung ist typisch für kategorische Features wie „geografische Region“, die keine aussagekräftige numerische Darstellung besitzen. Sie wird auch für numerische Features empfohlen, deren Einfluss auf das Ergebnis wahrscheinlich nicht linear ist. Zum Beispiel könnte ein bestimmter Artikel für eine bestimmte Altersgruppe relevant sein und für jeden, der älter oder jünger ist, irrelevant.

Jedes Feature mit einem Zeichenfolgenwert ist standardmäßig 1-hot-codiert: Für jeden möglichen Wert wird ein eigenes internes Feature erstellt. Eine automatische 1-Hot-Codierung für numerische Features und/oder angepasste Bereiche wird zurzeit nicht bereitgestellt.

> [!TIP]
> Die Machine Learning-Algorithmen behandeln alle möglichen Werte eines bestimmten internen Features einheitlich: über eine gemeinsame „Gewichtung“. Die 1-Hot-Codierung erlaubt für jeden Wertebereich eine eigene „Gewichtung“. Eine Verkleinerung der Bereiche führt zu einem besseren Ergebnis, sobald genügend Daten gesammelt wurden, kann aber die Datenmenge erhöhen, die für eine Annäherung an bessere Ergebnisse erforderlich ist.

## <a name="feature-specification-format-and-apis"></a>Featurespezifikation: Format und APIs

Sie können Features über mehrere Hilfs-APIs angeben. Alle APIs verwenden ein gemeinsames JSON-Format. Im Folgenden werden die APIs und das Format auf konzeptioneller Ebene dargestellt. Die Spezifikation wird durch ein Swagger-Schema ergänzt.

Die grundlegende JSON-Vorlage für die Featurespezifikation sieht folgendermaßen aus:

```json
{
"<name>":<value>, "<name>":<value>, ... ,
"namespace1": {"<name>":<value>, ... },
"namespace2": {"<name>":<value>, ... },
...
}
```

Hier stehen `<name>` und `<value>` für den Featurenamen bzw. den Featurewert. `<value>` kann eine Zeichenfolge, eine ganze Zahl, ein Floatwert, ein boolescher Wert oder ein Array sein. Ein Feature, das nicht in einen Namespace eingebunden ist, wird automatisch dem Standardnamespace zugeordnet.

Um eine Zeichenfolge als Bag-of-Words darzustellen, verwenden Sie eine spezielle Syntax `"_text":"string"` anstelle von `"<name>":<value>`. Effektiv wird für jedes Wort in der Zeichenfolge ein separates internes Feature erstellt. Sein Wert ist die Anzahl der Vorkommen dieses Worts.

Wenn `<name>` mit „_“ beginnt (und nicht `"_text"` ist), wird das Feature ignoriert.

> [!TIP]
> Manchmal werden Features aus mehreren JSON-Quellen zusammengeführt. Der Einfachheit halber können Sie diese wie folgt darstellen:
>
> ```json
> {
> "source1":<features>,
> "source2":<features>,
> ...
> }
> ```

Hier bezieht sich `<features>` auf die zuvor definierte grundlegende Featurespezifikation. Auch tiefere Ebenen der „Schachtelung“ sind zulässig. Custom Decision Service ermittelt automatisch die „tiefsten“ JSON-Objekte, die als `<features>` interpretiert werden können.

#### <a name="feature-set-api"></a>Funktionsumfangs-API

Die Funktionsumfangs-API gibt eine Liste von Features im zuvor beschriebenen JSON-Format zurück. Sie können verschiedene Endpunkte der Funktionsumfangs-API verwenden. Jeder Endpunkt wird durch eine Funktionsumfangs-ID und eine URL identifiziert. Die Zuordnung zwischen Funktionsumfangs-IDs und URLs wird im Portal festgelegt.

Rufen Sie die Funktionsumfangs-API auf, indem Sie die entsprechende Funktionsumfangs-ID an der entsprechenden Stelle in JSON einfügen. Für aktionsabhängige Features wird der Aufruf automatisch über die Aktions-ID parametriert. Sie können mehrere Funktionsumfangs-IDs für dieselbe Aktion angeben.

#### <a name="action-set-api-json-version"></a>Funktionsumfangs-API (JSON-Version)

Die Funktionsumfangs-API weist eine Version auf, in der Aktionen und Features in JSON angegeben werden. Features können explizit und/oder über Funktionsumfangs-APIs angegeben werden. Gemeinsam verwendete Features können ein Mal für alle Aktionen angegeben werden.

#### <a name="ranking-api-http-post-call"></a>Rangfolge-API (HTTP POST-Aufruf)

Die Rangfolge-API weist eine Version auf, die einen HTTP POST-Aufruf verwendet. Der Textkörper dieses Aufrufs gibt Aktionen und Features über eine flexible JSON-Syntax an.

Aktionen können explizit und/oder über Aktionssatz-APIs angegeben werden. Immer wenn eine Aktionssatz-ID gefunden wird, wird ein Aufruf des entsprechenden Endpunkts der Aktionssatz-API ausgeführt.

Features können für die Aktionssatz-API explizit und/oder über Funktionsumfangs-APIs angegeben werden. Gemeinsam verwendete Features können ein Mal für alle Aktionen angegeben werden.
