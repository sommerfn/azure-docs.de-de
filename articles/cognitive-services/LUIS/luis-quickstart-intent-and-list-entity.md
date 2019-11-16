---
title: 'Tutorial: Genaue Textübereinstimmung: LUIS'
titleSuffix: Azure Cognitive Services
description: Abrufen von Daten, die mit einer vordefinierten Liste von Elementen übereinstimmen. Jedes Element auf der Liste kann Synonyme aufweisen, die ebenfalls exakt übereinstimmen
services: cognitive-services
author: diberry
manager: nitinme
ms.custom: seodec18
ms.service: cognitive-services
ms.subservice: language-understanding
ms.topic: tutorial
ms.date: 11/11/2019
ms.author: diberry
ms.openlocfilehash: a8021885bf51ab6d44bc8576b9fdd69f1bdd270a
ms.sourcegitcommit: 44c2a964fb8521f9961928f6f7457ae3ed362694
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 11/12/2019
ms.locfileid: "73953705"
---
# <a name="tutorial-get-exact-text-matched-data-from-an-utterance"></a>Tutorial: Abrufen von Daten mit genauer Textübereinstimmung aus einer Äußerung

In diesem Tutorial lernen Sie, wie Entitätsdaten abgerufen werden, die mit einer vordefinierten Liste von Elementen übereinstimmen. 

[!INCLUDE [Waiting for LUIS portal refresh](./includes/wait-v3-upgrade.md)]

**In diesem Tutorial lernen Sie Folgendes:**

<!-- green checkmark -->
> [!div class="checklist"]
> * Erstellen einer App
> * Hinzufügen einer Absicht
> * Hinzufügen einer Entität vom Typ „Liste“ 
> * Trainieren 
> * Veröffentlichen
> * Abrufen von Absichten und Entitäten vom Endpunkt

[!INCLUDE [LUIS Free account](../../../includes/cognitive-services-luis-free-key-short.md)]

## <a name="what-is-a-list-entity"></a>Was ist eine Listenentität?

Eine Listenentität ist eine genaue Textübereinstimmung mit den Wörtern in der Äußerung. 

Jedes Element auf der Liste kann eine Liste mit Synonymen enthalten. Für eine Personalverwaltungs-App kann eine Abteilung im Unternehmen anhand einiger Schlüsselinformationen identifiziert werden, etwa ihres offiziellen Namens, gängiger Akronyme und Codes der Buchhaltung. 

Die Personalverwaltungs-App muss die Abteilung bestimmen, zu der ein Mitarbeiter wechselt. 

Eine Listenentität ist eine gute Wahl für diese Art von Daten, wenn Folgendes gilt:

* Bei den Datenwerten handelt es sich um einen bekannten Satz.
* Für den Satz werden die maximalen LUIS-[Grenzen](luis-boundaries.md) dieses Entitätstyps nicht überschritten.
* Der Text in der Äußerung ist eine exakte Übereinstimmung mit einem Synonym oder dem kanonischen Namen. LUIS verwendet die Liste über genaue Textübereinstimmungen hinaus nicht. Wortstammerkennung, Pluralformen und andere Varianten lassen sich nur mithilfe einer Listenentität nicht auflösen. Um Varianten zu behandeln, ziehen Sie die Verwendung eines [Musters](reference-pattern-syntax.md#syntax-to-mark-optional-text-in-a-template-utterance) mit der optionalen Textsyntax in Erwägung. 

## <a name="create-a-new-app"></a>Erstellen einer neuen App

[!INCLUDE [Follow these steps to create a new LUIS app](../../../includes/cognitive-services-luis-create-new-app-steps.md)]

## <a name="create-an-intent-to-transfer-employees-to-a-different-department"></a>Erstellen Sie eine Absicht, um Mitarbeiter in eine andere Abteilung zu überstellen

1. [!INCLUDE [Start in Build section](../../../includes/cognitive-services-luis-tutorial-build-section.md)]

2. Wählen Sie **Create new intent** (Neue Absicht erstellen) aus. 

3. Geben Sie im Popupdialogfeld die Zeichenfolge `TransferEmployeeToDepartment` ein, und klicken Sie anschließend auf **Fertig**. 

    ![Screenshot: Dialogfenster „Create new intent“ (Neue Absicht erstellen)](./media/luis-quickstart-intent-and-list-entity/hr-create-new-intent-ddl.png)

4. Fügen Sie der Absicht Beispieläußerungen hinzu.

    |Beispiele für Äußerungen|
    |--|
    |„move John W. Smith to the accounting department“ (John W. Smith in die Buchhaltungsabteilung versetzen)|
    |Übertragen Sie Jill Jones an F&E|
    |Abteilung 1234 hat ein neues Mitglied namens Bill Bradstreet|
    |Setzen Sie John Jackson in der technischen Abteilung ein |
    |Versetzen Sie Debra Doughtery in den Inlandsvertrieb|
    |Versetzen Sie Jill Jones in die IT|
    |Bringen Sie Alice Anderson in DevOps unter|
    |Carl Chamerlin wechselt zu Finanzen|
    |Steve Standish in 1234|
    |Tanner Thompson in 3456|

    [![Screenshot: Absicht mit Beispieläußerungen](media/luis-quickstart-intent-and-list-entity/intent-transfer-employee-to-department.png "Screenshot: Absicht mit Beispieläußerungen")](media/luis-quickstart-intent-and-list-entity/intent-transfer-employee-to-department.png#lightbox)

    [!INCLUDE [Do not use too few utterances](../../../includes/cognitive-services-luis-too-few-example-utterances.md)]  

## <a name="department-list-entity"></a>Entität „Abteilungsliste“

Jetzt, da für die Absicht **TransferEmployeeToDepartment** Beispieläußerungen vorhanden sind, muss LUIS verstehen, was eine Abteilung ist. 

Der primäre, _kanonische_ Name für jedes Element ist der Abteilungsname. Dies sind ein paar Beispiele für Synonyme der einzelnen kanonischen Namen: 

|Kanonischer Name|Synonyme|
|--|--|
|Buchhaltung|Buchh.<br>Bchltng<br>3456|
|Entwicklung und Betrieb|DevOps<br>4949|
|Entwicklung|Entw<br>Dev<br>4567|
|Finanzen|Fin<br>2020|
|Informationstechnologie|IT<br>2323|
|Vertrieb Inland|I-Vertrieb<br>I-Vertr.<br>1414|
|Forschung und Entwicklung|F&E<br>1234|

1. Klicken Sie im linken Bereich auf **Entitäten**.

1. Wählen Sie **Neue Entität erstellen** aus.

1. Geben Sie im Popupdialogfeld `Department` als Entitätsname und **List** (Liste) als Entitätstyp ein. Wählen Sie **Fertig**aus.  

    [![Screenshot: Popupdialogfeld zur Erstellung einer neuen Entität](media/luis-quickstart-intent-and-list-entity/create-new-list-entity-named-department.png "Screenshot: Popupdialogfeld zur Erstellung einer neuen Entität")](media/luis-quickstart-intent-and-list-entity/create-new-list-entity-named-department.png#lightbox)

1. Geben Sie auf der Seite mit der Abteilungsentität `Accounting` als neuen Wert ein.

1. Fügen Sie als Synonyme die Synonyme aus der vorhergehenden Tabelle ein.

1. Fahren Sie mit dem Hinzufügen aller kanonischen Namen und ihrer Synonyme fort. 

## <a name="add-example-utterances-to-the-none-intent"></a>Fügen Sie der Absicht „None“ Beispieläußerungen hinzu 

[!INCLUDE [Follow these steps to add the None intent to the app](../../../includes/cognitive-services-luis-create-the-none-intent.md)]

## <a name="train-the-app-so-the-changes-to-the-intent-can-be-tested"></a>Trainieren der App, um die Absichtsänderungen testen zu können 

[!INCLUDE [LUIS How to Train steps](../../../includes/cognitive-services-luis-tutorial-how-to-train.md)]

## <a name="publish-the-app-so-the-trained-model-is-queryable-from-the-endpoint"></a>Veröffentlichen der App, damit das trainierte Modell über den Endpunkt abgefragt werden kann

[!INCLUDE [LUIS How to Publish steps](../../../includes/cognitive-services-luis-tutorial-how-to-publish.md)]

## <a name="get-intent-and-entity-prediction-from-endpoint"></a>Abrufen von Absicht und Entitätsvorhersage vom Endpunkt

1. [!INCLUDE [LUIS How to get endpoint first step](../../../includes/cognitive-services-luis-tutorial-how-to-get-endpoint.md)] 

1. Geben Sie in der Adressleiste am Ende der URL `shift Joe Smith to IT` ein. Der letzte Parameter der Abfragezeichenfolge lautet `q` (für die Abfrage („**q**uery“) der Äußerung). Diese Äußerung entspricht keiner der bezeichneten Äußerungen. Sie ist daher ein guter Test, und es sollte die Absicht `TransferEmployeeToDepartment` mit Extrahierung von `Department` zurückgegeben werden.

   ```json
    {
      "query": "shift Joe Smith to IT",
      "topScoringIntent": {
        "intent": "TransferEmployeeToDepartment",
        "score": 0.9775754
      },
      "intents": [
        {
          "intent": "TransferEmployeeToDepartment",
          "score": 0.9775754
        },
        {
          "intent": "None",
          "score": 0.0154493852
        }
      ],
      "entities": [
        {
          "entity": "it",
          "type": "Department",
          "startIndex": 19,
          "endIndex": 20,
          "resolution": {
            "values": [
              "Information Technology"
            ]
          }
        }
      ]
    }
   ```

## <a name="clean-up-resources"></a>Bereinigen von Ressourcen

[!INCLUDE [LUIS How to clean up resources](../../../includes/cognitive-services-luis-tutorial-how-to-clean-up-resources.md)]

## <a name="related-information"></a>Verwandte Informationen

* Informationen zum Konzept der [Listenentität](luis-concept-entity-types.md#list-entity)
* [Gewusst wie: trainieren](luis-how-to-train.md)
* [Informationen zum Veröffentlichen](luis-how-to-publish-app.md)
* [Informationen zum Testen im LUIS-Portal](luis-interactive-test.md)


## <a name="next-steps"></a>Nächste Schritte
In diesem Tutorial wurden eine neue Absicht erstellt, Beispieläußerungen hinzugefügt und dann eine Listenentität erstellt, um exakte Textübereinstimmungen aus Äußerungen zu extrahieren. Nach dem Trainieren und Veröffentlichen der App hat eine Abfrage des Endpunkts die Absicht identifiziert und die extrahierten Daten zurückgegeben.

Fahren Sie mit dieser App fort, mit [Hinzufügen einer Entität vom Typ „Composite“](luis-tutorial-composite-entity.md).

> [!div class="nextstepaction"]
> [Hinzufügen einer vordefinierten Entität mit einer Rolle zur App](tutorial-entity-roles.md)

