---
title: 'Tutorial: Extrahieren strukturierter Daten mit durch maschinelles Lernen erworbener Entität – LUIS'
titleSuffix: Azure Cognitive Services
description: Extrahieren Sie strukturierte Daten aus einer Äußerung mithilfe der durch maschinelles Lernen erworbenen Entität. Fügen Sie Unterkomponenten mit Deskriptoren und Einschränkungen hinzu, um die Genauigkeit der Extraktion zu erhöhen.
services: cognitive-services
author: diberry
manager: nitinme
ms.custom: seodec18
ms.service: cognitive-services
ms.subservice: language-understanding
ms.topic: tutorial
ms.date: 11/07/2019
ms.author: diberry
ms.openlocfilehash: 36b75f33b4fc9062d09fbc670a509594142f09bd
ms.sourcegitcommit: ac56ef07d86328c40fed5b5792a6a02698926c2d
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 11/08/2019
ms.locfileid: "73828256"
---
# <a name="tutorial-extract-structured-data-with-machine-learned-entities-in-language-understanding-luis"></a>Tutorial: Extrahieren strukturierter Daten mithilfe der durch maschinelles Lernen erworbenen Entitäten in Language Understanding (LUIS)

In diesem Tutorial extrahieren Sie strukturierte Daten aus einer Äußerung mithilfe der durch maschinelles Lernen erworbenen Entität. 

Die durch maschinelles Lernen erworbene Entität unterstützt das [Modellaufschlüsselungskonzept](luis-concept-model.md#v3-authoring-model-decomposition), indem sie Unterkomponentenentitäten mit ihren Deskriptoren und Einschränkungen bereitstellt. 

[!INCLUDE [Uses preview portal](includes/uses-portal-preview.md)]

**In diesem Tutorial lernen Sie Folgendes:**

> [!div class="checklist"]
> * Importieren der Beispiel-App
> * Hinzufügen einer durch maschinelles Lernen erworbenen Entität 
> * Hinzufügen von Unterkomponenten
> * Hinzufügen des Deskriptors der Unterkomponente
> * Hinzufügen der Einschränkung der Unterkomponente
> * Trainieren der App
> * Testen der App
> * App veröffentlichen
> * Abrufen der Entitätsvorhersage vom Endpunkt

[!INCLUDE [LUIS Free account](includes/quickstart-tutorial-use-free-starter-key.md)]


## <a name="why-use-a-machine-learned-entity"></a>Gründe für die Verwendung einer durch maschinelles Lernen erworbenen Entität

In diesem Tutorial wird eine durch maschinelles Lernen erworbene Entität hinzugefügt, um Daten aus einer Äußerung zu extrahieren. 

Der Zweck einer Entität ist es, die zu extrahierenden Daten zu definieren. Dies umfasst die Angabe eines Namens, eines Typs (falls möglich), einer beliebigen Auflösung der Daten bei Mehrdeutigkeit und des genauen Textes, aus dem die Daten bestehen. 

Um die Entität zu definieren, müssen Sie die Entität erstellen und dann den Text beschriften, der die Entität in der Beispieläußerung darstellt. Diese beschrifteten Beispiele vermitteln LUIS, worum es sich bei der Entität handelt und wo sie sich in einer Äußerung befindet. 

## <a name="entity-decomposability-is-important"></a>Die Möglichkeit zur Aufschlüsselung von Entitäten ist wichtig.

Die Möglichkeit zur Aufschlüsselung von Entitäten ist sowohl für die Absichtsvorhersage als auch für die Datenextraktion wichtig. 

Beginnen Sie mit einer durch maschinelles Lernen erworbenen Entität, die den Anfang und die Entität der höchsten Ebene für die Datenextraktion darstellt. Dann schlüsseln Sie die Entität in die für die Clientanwendung erforderlichen Teile auf. 

Obwohl Sie vielleicht nicht wissen, wie detailliert Ihre Entität sein soll, wenn Sie Ihre App starten, ist es eine bewährte Methode, mit einer durch maschinelles Lernen erworbenen Entität zu beginnen und dann mit der Aufschlüsselung von Teilkomponenten fortzufahren, wenn die Entwicklung Ihrer App fortschreitet.

In der Praxis werden Sie eine durch maschinelles Lernen erworbene Entität erstellen, um eine Bestellung für eine Pizza-App darzustellen. Die Bestellung sollte alle Teile enthalten, die zur Erfüllung der Bestellung erforderlich sind. Zu Beginn enthält die Entität alle auf die Bestellung bezogenen Texte und entnimmt insbesondere Größe und Menge. 

Eine Äußerung für `deliver one large cheese pizza` sollte die gesamte Äußerung als Bestellung und dann auch `1` und `large` extrahieren. 

Es gibt weitere Aufschlüsselungen, die Sie vornehmen können, z. B. Beläge oder Kruste. Nach diesem Tutorial sollten Sie sich beim Hinzufügen dieser Unterkomponenten zu Ihrer bestehenden `Order`-Entität sicher fühlen.

## <a name="import-example-json-to-begin-app"></a>Importieren von JSON-Beispielcode zum Starten einer App

1.  Laden Sie die [App-JSON-Datei](https://github.com/Azure-Samples/cognitive-services-language-understanding/blob/master/documentation-samples/tutorials/machine-learned-entity/pizza-intents-only.json) herunter, und speichern Sie sie.

1. Wählen Sie im [LUIS-Vorschauportal](https://preview.luis.ai) auf der Seite **Meine Apps** die Option **Importieren** und dann **Als JSON importieren** aus. Suchen Sie die gespeicherte JSON-Datei aus dem vorherigen Schritt. Sie müssen den Namen der App nicht ändern. Wählen Sie **Fertig**aus.

1. Wählen Sie die Version auf der Registerkarte **Versionen** aus dem Abschnitt **Verwalten** und dann **Klonen** aus, um die Version zu klonen. Benennen Sie sie anschließend `mach-learn`. Wählen Sie dann **Fertig** aus, um den Klonvorgang abzuschließen. Da der Versionsname als Teil der URL-Route verwendet wird, darf er keine Zeichen enthalten, die in einer URL ungültig sind.

    > [!TIP] 
    > Das Klonen ist eine bewährte Methode, bevor Sie Ihre App ändern. Wenn Sie mit einer Version fertig sind, exportieren Sie eine Version (als JSON- oder LU-Datei), und checken Sie diese in Ihre Quellcodeverwaltung ein.

1. Wählen Sie **Erstellen** und dann **Absichten** aus, um die wichtigsten Bausteine einer LUIS-Anwendung, die Absichten, anzuzeigen.

    ![Wechseln Sie von der Seite „Versionen“ zur Seite „Absichten“.](media/tutorial-machine-learned-entity/new-version-imported-app.png)

## <a name="label-text-as-entities-in-example-utterances"></a>Bezeichnungstext als Entitäten in Beispieläußerungen

Um Details einer Pizzabestellung zu extrahieren, erstellen Sie eine durch maschinelles Lernen erworbene `Order`-Entität auf höchster Ebene.

1. Wählen Sie auf der Seite **Absichten** die Absicht **OrderPizza** (Pizza bestellen) aus. 

1. Wählen Sie in der Liste der Beispieläußerungen die folgende Äußerung aus. 

    |Beispieläußerung zu einer Bestellung|
    |--|
    |`pickup a cheddar cheese pizza large with extra anchovies`|

    Beginnen Sie mit der Auswahl kurz vor dem linken Text von `pickup` (#1), dann gehen Sie kurz über den rechten Text hinaus, `anchovies` (#2 – damit endet der Bezeichnungsprozess). Ein Popupmenü wird angezeigt. Geben Sie im Popupfeld den Namen der Entität als `Order` (#3) ein. Wählen Sie dann `Order - Create new entity` aus der Liste aus (#4).

    ![Bezeichnen von Anfang und Ende des Textes für die vollständige Bestellung](media/tutorial-machine-learned-entity/mark-complete-order.png)

    > [!NOTE]
    > Eine Entität ist nicht immer die gesamte Äußerung. In diesem speziellen Fall gibt `pickup` an, wie die Bestellung empfangen werden soll, sodass sie Teil der bezeichneten Entität für die Bestellung sein sollte. 

1. Wählen Sie im Feld **Entitätstyp auswählen** die Option **Struktur hinzufügen** und dann **Weiter** aus. Die Struktur ist erforderlich, um Unterkomponenten wie Größe und Menge zu berücksichtigen.

    ![Hinzufügen einer Struktur zur Entität](media/tutorial-machine-learned-entity/add-structure-to-entity.png)

1. Fügen Sie im Feld zum **Erstellen einer durch maschinelles Lernen erworbenen Entität** im Feld **Struktur** die Option `Size` hinzu, und drücken Sie dann die EINGABETASTE. 
1. Wählen Sie zum Hinzufügen eines **Deskriptors** das `+` im Bereich **Deskriptoren für Größe** und dann **Neue Ausdrucksliste erstellen** aus.

1. Geben Sie im Feld **Neuen Ausdruckslistendeskriptor erstellen** den Namen `SizeDescriptor` und dann die Werte von `small`, `medium` und `large` ein. Wenn das Feld **Vorschläge** ausgefüllt ist, wählen Sie `extra large` und `xl` aus. Wählen Sie **Fertig** aus, um die neue Ausdrucksliste zu erstellen. 

    Dieser Ausruckslistendeskriptor hilft der Unterkomponente `Size`, Wörter zu finden, die sich auf die Größe beziehen, indem er ein Beispielwort bereitstellt. Diese Liste muss nicht jedes auf die Größe bezogene Wort enthalten, sondern sollte Wörter einbeziehen, die voraussichtlich die Größe angeben. 

    ![Erstellen eines Deskriptors für die Unterkomponente „Größe“](media/tutorial-machine-learned-entity/size-entity-size-descriptor-phrase-list.png)

1. Wählen Sie im Fenster **Durch maschinelles Lernen erworbene Entität** die Option **Erstellen** aus, um die Erstellung der Unterkomponente `Size` abzuschließen.  

    Die `Order`-Entität mit einer `Size`-Komponente wird erstellt, aber nur die `Order`-Entität wurde auf die Äußerung angewendet. Sie müssen den `Size`-Entitätstext in der Beispieläußerung bezeichnen. 

1. Bezeichnen Sie in derselben Beispieläußerung die Unterkomponente **Größe** von `large`, indem Sie das Wort und dann die Entität **Größe** aus der Dropdownliste auswählen. 

    ![Bezeichnen Sie die Entität „Größe“ für Text in der Äußerung.](media/tutorial-machine-learned-entity/mark-and-create-size-entity.png)

    Die Linie ist unter dem Text durchgezogen, da sowohl die Bezeichnung als auch die Vorhersage übereinstimmen, weil Sie den Text explizit bezeichnet haben.

1. Bezeichnen Sie die `Order`-Entität in den restlichen Äußerungen zusammen mit der Entität „Größe“. Die eckigen Klammern im Text zeigen die bezeichnete `Order`-Entität und die darin enthaltene `Size`-Entität.

    |Beispieläußerungen für die Bestellung|
    |--|
    |`can i get [a pepperoni pizza and a can of coke] please`|
    |`can i get [a [small] pizza with onions peppers and olives]`|
    |`[delivery for a [small] pepperoni pizza]`|
    |`i need [2 [large] cheese pizzas 6 [large] pepperoni pizzas and 1 [large] supreme pizza]`|

    ![Erstellen Sie Entitäten und Unterkomponenten in allen verbleibenden Beispieläußerungen.](media/tutorial-machine-learned-entity/entity-subentity-labeled-not-trained.png)

    > [!CAUTION]
    > Wie gehen Sie mit impliziten Daten wie dem Buchstaben `a` um, der eine einzelne Pizza auszeichnet? Oder das Fehlen von `pickup` und `delivery`, um anzugeben, wo die Pizza erwartet wird? Oder das Fehlen einer Größe, um Ihre Standardgröße bzw. „Klein“ oder „Groß“ anzugeben? Betrachten Sie den Umgang mit behandelten impliziten Daten als Teil Ihrer Geschäftsregeln in der Clientanwendung. 

1. Wählen Sie zum Trainieren der App **Trainieren** aus. Das Training wendet die Änderungen, z. B. die neuen Entitäten und die bezeichneten Äußerungen, auf das aktive Modell an.

1. Nach dem Training fügen Sie eine neue Beispieläußerung hinzu, um zu verstehen, wie gut LUIS die durch maschinelles Lernen erworbene Entität versteht. 

    |Beispieläußerung zu einer Bestellung|
    |--|
    |`pickup XL meat lovers pizza`|

    Die oberste Entität, `Order`, ist bezeichnet und die Unterkomponente `Size` ist ebenfalls mit gestrichelten Linien gekennzeichnet. Dies ist eine erfolgreiche Vorhersage. 

    ![Neues Beispieläußerung mit Vorhersage durch Entität](media/tutorial-machine-learned-entity/new-example-utterance-predicted-with-entity.png)

    Der gepunktete Link zeigt die Vorhersage an. 

1. Wählen Sie beim Ändern der Vorhersage in eine bezeichnete Entität die Zeile aus, und wählen Sie dann **Entitätsvorhersagen bestätigen** aus.

    ![Akzeptieren Sie die Vorhersage, indem Sie „Entitätsvorhersage bestätigen“ auswählen.](media/tutorial-machine-learned-entity/confirm-entity-prediction-for-new-example-utterance.png)

    An diesem Punkt funktioniert die durch maschinelles Lernen erworbene Entität, da sie die Entität innerhalb einer neuen Beispieläußerung finden kann. Wenn die Entität nicht ordnungsgemäß vorhergesagt wird, bezeichnen Sie die Entität und die Unterkomponenten, während Sie Beispieläußerungen hinzufügen. Wenn die Entität richtig vorhergesagt wird, stellen Sie sicher, dass Sie die Vorhersagen bestätigen. 

## <a name="add-prebuilt-number-to-app-to-help-extract-data"></a>Hinzufügen einer vordefinierten Nummer zur App, um beim Extrahieren der Daten zu helfen

Die Bestellinformationen sollten auch enthalten, wie viele Artikel in der Bestellung enthalten sind, z. B. die Anzahl der Pizzas. Um diese Daten zu extrahieren, muss eine neue durch maschinelles Lernen erworbene Unterkomponente zu `Order` hinzugefügt werden, und diese Komponente erfordert eine Einschränkung einer vordefinierten Nummer. Durch die Einschränkung der Entität auf eine vordefinierte Nummer findet und extrahiert die Entität Zahlen, unabhängig davon, ob es sich bei dem Text um eine Ziffer (`2`) oder Text (`two`) handelt.

Beginnen Sie mit dem Hinzufügen der vordefinierten Nummer zur App. 

1. Wählen Sie **Entitäten** aus dem linken Menü und dann **+ Vordefinierte Entität hinzufügen** aus. 

1. Suchen Sie im Feld **Vordefinierte Entitäten hinzufügen** nach **Nummer** und wählen Sie dies aus. Anschließend wählen Sie **Fertig** aus. 

    ![Hinzufügen einer vordefinierten Entität](media/tutorial-machine-learned-entity/add-prebuilt-entity-as-constraint-to-quantity-subcomponent.png)

    Die vordefinierte Entität wird der App hinzugefügt, ist aber noch keine Einschränkung. 

## <a name="create-subcomponent-entity-with-constraint-to-help-extract-data"></a>Erstellen einer Unterkomponentenentität mit Einschränkung, um beim Extrahieren der Daten zu helfen

Die `Order`-Entität sollte eine `Quantity`-Unterkomponente aufweisen, um die Anzahl der Artikel in der Bestellung zu ermitteln. Die Menge sollte auf eine Anzahl beschränkt werden, damit die extrahierten Daten für die Clientanwendung sofort nutzbar sind. 

Eine Einschränkung wird als Textübereinstimmung angewendet, entweder mit genauer Übereinstimmung (z. B. eine Listenentität) oder durch reguläre Ausdrücke (z. B. eine Entität mit regulärem Ausdruck oder eine vordefinierte Entität). 

1. Wählen Sie **Entitäten** und dann die `Order`-Einheit aus. 
1. Wählen Sie **+ Komponente hinzufügen** aus, geben Sie dann den Namen `Quantity` ein, und drücken Sie die EINGABETASTE, um der App die neue Entität hinzuzufügen.
1. Wählen Sie nach der Erfolgsmeldung die Unterkomponente `Quantity` und dann den Einschränkungsstift aus.
1. Wählen Sie in der Dropdownliste die vordefinierte Nummer aus. 

    ![Erstellen Sie eine Mengenentität mit vordefinierter Nummer als Einschränkung.](media/tutorial-machine-learned-entity/create-constraint-from-prebuilt-number.png)

    Die Entität mit der Einschränkung ist erstellt, aber noch nicht auf die Beispieläußerungen angewendet.

    > [!NOTE]
    > Eine Unterkomponente kann innerhalb einer Unterkomponente auf bis zu fünf Ebenen geschachtelt werden. Obwohl dies in diesem Artikel nicht dargestellt wird, ist es über das Portal und die API verfügbar.  

## <a name="label-example-utterance-with-subcomponent-for-quantity-to-teach-luis-about-the-entity"></a>Bezeichnen einer Beispieläußerung mit einer Unterkomponente für die Menge, um LUIS Informationen über die Entität zu vermitteln

1. Wählen Sie im linken Navigationsbereich **Absichten** und dann die Absicht **OrderPizza** aus. Die drei Nummern in den folgenden Äußerungen sind bezeichnet, liegen aber optisch unter der `Order`-Entitätslinie. Diese untere Ebene bedeutet, dass die Entitäten gefunden, aber nicht getrennt von der `Order`-Entität betrachtet werden.

    ![Die vordefinierte Nummer wird gefunden, aber noch nicht als Teil der Entität „Bestellung“ betrachtet.](media/tutorial-machine-learned-entity/prebuilt-number-not-part-of-order-entity.png)

1. Bezeichnen Sie die Nummern mit der `Quantity`-Einheit, indem Sie die `2` in der Beispieläußerung und dann `Quantity` aus der Liste auswählen. Bezeichnen Sie die `6` und die `1` in derselben Beispieläußerung.

    ![Bezeichnen Sie den Text mit der Entität „Menge“.](media/tutorial-machine-learned-entity/mark-example-utterance-with-quantity-entity.png)  

## <a name="train-the-app-to-apply-the-entity-changes-to-the-app"></a>Trainieren der App zum Anwenden der Entitätsänderungen auf die App

Wählen Sie **Trainieren** aus, um die App mit diesen neuen Äußerungen zu trainieren.

![Trainieren Sie die App, und überprüfen Sie dann die Beispieläußerungen.](media/tutorial-machine-learned-entity/trained-example-utterances.png)

An dieser Stelle weist die Bestellung einige Details auf, die extrahiert werden können (Größe, Menge und gesamter Text der Bestellung). Es gibt eine weitere Verfeinerung der `Order`-Entität, z. B. Pizzabeläge, Krustentyp und Nebenbestellungen. Jede dieser Komponenten sollte als Unterkomponente der `Order`-Entität erstellt werden. 

## <a name="test-the-app-to-validate-the-changes"></a>Testen der App zum Überprüfen der Änderungen

Testen Sie die App über den interaktiven **Testbereich**. Mit diesem Prozess können Sie eine neue Äußerung eingeben und dann die Vorhersageergebnisse anzeigen, um zu überprüfen, wie gut die aktive und trainierte App funktioniert. Die Absichtsvorhersage sollte ziemlich zuverlässig sein (über 70 %) und die Entitätsextraktion sollte mindestens die `Order`-Entität aufnehmen. Die Details der Entität „Bestellung“ fehlen möglicherweise, da fünf Äußerungen nicht ausreichen, um jeden Fall zu bearbeiten.

1. Klicken Sie auf der oberen Navigationsleiste auf **Test** (Testen).
1. Geben Sie die Äußerung `deliver a medium veggie pizza` ein, und wählen Sie die EINGABETASTE aus. Das aktive Modell hat die richtige Absicht mit einer Zuverlässigkeit von über 70 % vorhergesagt. 

    ![Geben Sie eine neue Äußerung ein, um die Absicht zu testen.](media/tutorial-machine-learned-entity/interactive-test-panel-with-first-utterance.png)

1. Wählen Sie **Überprüfen** aus, um die Entitätsvorhersagen anzuzeigen.

    ![Zeigen Sie die Entitätsvorhersagen im interaktiven Testbereich an.](media/tutorial-machine-learned-entity/interactive-test-panel-with-first-utterance-and-entity-predictions.png)

    Die Größe wurde richtig identifiziert. Denken Sie daran, dass die Beispieläußerungen in der `OrderPizza`-Absicht kein Beispiel von `medium` als Größe aufweisen, sondern einen Deskriptor einer `SizeDescriptor`-Ausdrucksliste verwenden, die „mittelgroß“ enthält.

    Die Menge ist nicht richtig vorhergesagt. Um dies zu beheben, können Sie weitere Beispieläußerungen hinzufügen, die dieses Wort verwenden, um die Menge anzugeben und dieses Wort als `Quantity`-Entität zu bezeichnen. 

## <a name="publish-the-app-to-access-it-from-the-http-endpoint"></a>Veröffentlichen der App für den Zugriff über den HTTP-Endpunkt

[!INCLUDE [LUIS How to Publish steps](includes/howto-publish.md)]

## <a name="get-intent-and-entity-prediction--from-http-endpoint"></a>Abrufen von Absicht und Entitätsvorhersage vom HTTP-Endpunkt

1. [!INCLUDE [LUIS How to get endpoint first step](includes/howto-get-endpoint.md)]

1. Wechseln Sie zum Ende der URL in der Adresse, und geben Sie dieselbe Abfrage ein, die Sie im interaktiven Testbereich eingegeben haben. 

    `deliver a medium veggie pizza`

    Der letzte Parameter der Abfragezeichenfolge lautet `query` (für die Abfrage (**query**) der Äußerung). 

    ```json
    {
        "query": "deliver a medium veggie pizza",
        "prediction": {
            "topIntent": "OrderPizza",
            "intents": {
                "OrderPizza": {
                    "score": 0.7812769
                },
                "None": {
                    "score": 0.0314020254
                },
                "Confirm": {
                    "score": 0.009299271
                },
                "Greeting": {
                    "score": 0.007551549
                }
            },
            "entities": {
                "Order": [
                    {
                        "Size": [
                            "medium"
                        ],
                        "$instance": {
                            "Size": [
                                {
                                    "type": "Size",
                                    "text": "medium",
                                    "startIndex": 10,
                                    "length": 6,
                                    "score": 0.9955588,
                                    "modelTypeId": 1,
                                    "modelType": "Entity Extractor",
                                    "recognitionSources": [
                                        "model"
                                    ]
                                }
                            ]
                        }
                    }
                ],
                "$instance": {
                    "Order": [
                        {
                            "type": "Order",
                            "text": "a medium veggie pizza",
                            "startIndex": 8,
                            "length": 21,
                            "score": 0.7983857,
                            "modelTypeId": 1,
                            "modelType": "Entity Extractor",
                            "recognitionSources": [
                                "model"
                            ]
                        }
                    ]
                }
            }
        }
    }    
    ```
    

[!INCLUDE [LUIS How to clean up resources](includes/quickstart-tutorial-cleanup-resources.md)]

## <a name="related-information"></a>Verwandte Informationen

* [Tutorial: Absichten](luis-quickstart-intents-only.md)
* [Konzept: Entitäten](luis-concept-entity-types.md) – Informationen zum Konzept
* [Konzept: Features](luis-concept-feature.md) – Informationen zum Konzept
* [Informationen zum Trainieren](luis-how-to-train.md)
* [Informationen zum Veröffentlichen](luis-how-to-publish-app.md)
* [Informationen zum Testen im LUIS-Portal](luis-interactive-test.md)

## <a name="next-steps"></a>Nächste Schritte

In diesem Tutorial verwendet die App eine durch maschinelles Lernen erworbene Entität, um die Absicht der Äußerung eines Benutzers zu finden und Details aus dieser Äußerung zu extrahieren. Die Verwendung der durch maschinelles Lernen erworbenen Entität ermöglicht es Ihnen, die Details der Entität aufzuschlüsseln.  

> [!div class="nextstepaction"]
> [Hinzufügen einer vorgefertigten keyPhrase-Entität](luis-quickstart-intent-and-key-phrase.md)
