---
title: 'Tutorial: Kontextbezogene Daten mit Rollen: LUIS'
titleSuffix: Azure Cognitive Services
description: Es wird beschrieben, wie Sie verwandte Daten anhand des Kontexts finden. Beispielsweise sind der Ausgangsort und der Zielort für eine physische Verlegung aus einem Gebäude und Büro in ein anderes Gebäude und Büro aufeinander bezogen.
services: cognitive-services
author: diberry
manager: nitinme
ms.custom: seodec18
ms.service: cognitive-services
ms.subservice: language-understanding
ms.topic: tutorial
ms.date: 10/14/2019
ms.author: diberry
ms.openlocfilehash: 3e85067439fe412822ac34a065753e9a13c7a506
ms.sourcegitcommit: c22327552d62f88aeaa321189f9b9a631525027c
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 11/04/2019
ms.locfileid: "73488729"
---
# <a name="tutorial-extract-contextually-related-data-from-an-utterance"></a>Tutorial: Extrahieren von Daten mit ähnlichem Kontext aus einer Äußerung

In diesem Tutorial wird das kontextbasierte Finden aufeinander bezogener Teildaten behandelt. Ein Beispiel wären etwa Ursprungs- und Zielorte bei der Versetzung in eine andere Stadt. Beide Teilinformationen können erforderlich sein und stehen miteinander in Verbindung.  

Eine Rolle kann mit allen vordefinierten oder benutzerdefinierten Entitätstypen und sowohl in Beispieläußerungen als auch in Mustern verwendet werden. 

[!INCLUDE [Waiting for LUIS portal refresh](./includes/wait-v3-upgrade.md)]

**In diesem Tutorial lernen Sie Folgendes:**

> [!div class="checklist"]
> * Erstellen einer neuen App
> * Hinzufügen einer Absicht 
> * Abrufen von Ursprungs- und Zielinformationen mithilfe von Rollen
> * Trainieren
> * Veröffentlichen
> * Abrufen von Absichten und Entitätsrollen vom Endpunkt

[!INCLUDE [LUIS Free account](../../../includes/cognitive-services-luis-free-key-short.md)]

## <a name="related-data"></a>Verwandte Daten

Diese App erkennt, ob ein Mitarbeiter vom Ursprungsort an den Zielort versetzt werden soll. Es wird eine vordefinierte GeographyV2-Entität verwendet, um die Städtenamen zu identifizieren, und es werden Rollen genutzt, um die Standorttypen (Ursprung und Ziel) in der Äußerung zu ermitteln.

Eine Rolle sollte verwendet werden, wenn für die zu extrahierenden Entitätsdaten Folgendes gilt:

* Sie sind im Kontext der Äußerung miteinander verknüpft.
* Es werden bestimmte Wörter verwendet, um die einzelnen Rollen anzugeben. Beispiele für diese Wörter sind: von/nach, Verlassen/Ziel.
* Beide Rolle kommen häufig in derselben Äußerung vor, sodass LUIS von dieser häufigen kontextbezogenen Nutzung lernen kann.
* Müssen gruppiert und von einer Client-App als eine Informationseinheit verarbeitet werden.

## <a name="create-a-new-app"></a>Erstellen einer neuen App

[!INCLUDE [Follow these steps to create a new LUIS app](../../../includes/cognitive-services-luis-create-new-app-steps.md)]

## <a name="create-an-intent-to-move-employees-between-cities"></a>Erstellen einer Absicht zur Versetzung von Mitarbeitern an einen anderen Ort

1. [!INCLUDE [Start in Build section](../../../includes/cognitive-services-luis-tutorial-build-section.md)]

1. Wählen Sie **Create new intent** (Neue Absicht erstellen) aus. 

1. Geben Sie im Popupdialogfeld die Zeichenfolge `MoveEmployeeToCity` ein, und klicken Sie anschließend auf **Fertig**. 

    ![Screenshot: Dialogfenster „Create new intent“ (Neue Absicht erstellen)](./media/tutorial-entity-roles/create-new-intent-move-employee-to-city.png)

1. Fügen Sie der Absicht Beispieläußerungen hinzu.

    |Beispiele für Äußerungen|
    |--|
    |„move John W. Smith leaving Seattle headed to Orlando“ (John W. Smith von Seattle nach Orlando versetzen)|
    |„transfer Jill Jones from Seattle to Cairo“ (Jill Jones von Seattle nach Kairo transferieren)|
    |„Place John Jackson away from Tampa, coming to Atlanta“ (John Jackson von Tampa abziehen und nach Atlanta holen) |
    |„move Debra Doughtery to Tulsa from Chicago“ (Debra Doughtery von Chicago nach Tulsa versetzen)|
    |„mv Jill Jones leaving Cairo headed to Tampa“ (Jill Jones von Kairo nach Tampa holen)|
    |„Shift Alice Anderson to Oakland from Redmond“ (Alice Anderson von Redmond nach Oakland versetzen)|
    |„Carl Chamerlin from San Francisco to Redmond“ (Carl Chamerlin von San Francisco nach Redmond)|
    |„Transfer Steve Standish from San Diego toward Bellevue“ (Steve Standish von San Diego abziehen und nach Bellevue versetzen) |
    |„lift Tanner Thompson from Kansas city and shift to Chicago“ (Tanner Thompson von Kansas City nach Chicago versetzen)|

    [![Screenshot: LUIS mit neuen Äußerungen in der MoveEmployee-Absicht](./media/tutorial-entity-roles/hr-enter-utterances.png)](./media/tutorial-entity-roles/hr-enter-utterances.png#lightbox)

## <a name="add-prebuilt-entity-geographyv2"></a>Hinzufügen der vordefinierten geographyV2-Entität

Mit der vordefinierten Entität „geographyV2“ werden Standortinformationen extrahiert, z. B. Städtenamen. Da die Äußerungen zwei Städtenamen enthalten, die im Kontext miteinander in Beziehung stehen, sollten Sie Rollen zum Extrahieren dieses Kontexts verwenden.

1. Wählen Sie im linken Navigationsmenü die Option **Entitäten**.

1. Wählen Sie **Vordefinierte Entität hinzufügen** und dann in der Suchleiste die Option `geo`, um die vordefinierten Entitäten zu filtern. 

    ![Hinzufügen einer vordefinierten geographyV2-Entität zur App](media/tutorial-entity-roles/add-geographyV2-prebuilt-entity.png)
1. Aktivieren Sie das Kontrollkästchen, und wählen Sie **Fertig**.
1. Wählen Sie in der Liste **Entitäten** die Option **geographyV2**, um die neue Entität zu öffnen. 
1. Fügen Sie die beiden Rollen `Origin` und `Destination` hinzu. 

    ![Hinzufügen von Rollen zur vordefinierten Entität](media/tutorial-entity-roles/add-roles-to-prebuilt-entity.png)
1. Wählen Sie im Navigationsbereich auf der linken Seite die Option **Absichten** und dann die Absicht **MoveEmployeeToCity** aus. Beachten Sie, dass die Städtenamen mit der vordefinierten Entität **geographyV2** bezeichnet sind.
1. Wählen Sie in der ersten Äußerung der Liste den Standort des Ursprungs aus. Ein Dropdownmenü wird angezeigt. Wählen Sie in der Liste den Eintrag **geographyV2** aus, und wählen Sie dann im angezeigten Menü die Option **Ursprung**.
1. Verwenden Sie die Methode aus dem vorherigen Schritt, um alle Rollen der Standorte in allen Äußerungen zu kennzeichnen. 


## <a name="add-example-utterances-to-the-none-intent"></a>Hinzufügen von Beispieläußerungen zur Absicht „None“ 

[!INCLUDE [Follow these steps to add the None intent to the app](../../../includes/cognitive-services-luis-create-the-none-intent.md)]

## <a name="train-the-app-so-the-changes-to-the-intent-can-be-tested"></a>Trainieren der App, um die Absichtsänderungen testen zu können 

[!INCLUDE [LUIS How to Train steps](../../../includes/cognitive-services-luis-tutorial-how-to-train.md)]

## <a name="publish-the-app-so-the-trained-model-is-queryable-from-the-endpoint"></a>Veröffentlichen der App, damit das trainierte Modell über den Endpunkt abgefragt werden kann

[!INCLUDE [LUIS How to Publish steps](../../../includes/cognitive-services-luis-tutorial-how-to-publish.md)]

## <a name="get-intent-and-entity-prediction-from-endpoint"></a>Abrufen von Absicht und Entitätsvorhersage vom Endpunkt

1. [!INCLUDE [LUIS How to get endpoint first step](../../../includes/cognitive-services-luis-tutorial-how-to-get-endpoint.md)]


1. Geben Sie in der Adressleiste am Ende der URL `Please move Carl Chamerlin from Tampa to Portland` ein. Der letzte Parameter der Abfragezeichenfolge lautet `q` (für die Abfrage (**query**) der Äußerung). Diese Äußerung entspricht keiner der bezeichneten Äußerungen. Sie ist daher ein guter Test, und es sollte die `MoveEmployee`-Absicht mit extrahierter Entität zurückgegeben werden.

    ```json
    {
      "query": "Please move Carl Chamerlin from Tampa to Portland",
      "topScoringIntent": {
        "intent": "MoveEmployeeToCity",
        "score": 0.9706451
      },
      "intents": [
        {
          "intent": "MoveEmployeeToCity",
          "score": 0.9706451
        },
        {
          "intent": "None",
          "score": 0.0307451729
        }
      ],
      "entities": [
        {
          "entity": "tampa",
          "type": "builtin.geographyV2.city",
          "startIndex": 32,
          "endIndex": 36,
          "role": "Origin"
        },
        {
          "entity": "portland",
          "type": "builtin.geographyV2.city",
          "startIndex": 41,
          "endIndex": 48,
          "role": "Destination"
        }
      ]
    }
    ```
    
    Die richtige Absicht wird vorhergesagt, und das Entitätenarray weist sowohl die Ursprungs- als auch die Zielrolle in der entsprechenden Eigenschaft vom Typ **entities** auf.
    
## <a name="clean-up-resources"></a>Bereinigen von Ressourcen

[!INCLUDE [LUIS How to clean up resources](../../../includes/cognitive-services-luis-tutorial-how-to-clean-up-resources.md)]

## <a name="related-information"></a>Verwandte Informationen

* [Entitäten: Konzepte](luis-concept-entity-types.md)
* [Rollen: Konzepte](luis-concept-roles.md)
* [Liste mit vordefinierten Entitäten](luis-reference-prebuilt-entities.md)
* [Informationen zum Trainieren](luis-how-to-train.md)
* [Informationen zum Veröffentlichen](luis-how-to-publish-app.md)
* [Informationen zum Testen im LUIS-Portal](luis-interactive-test.md)
* [Rollen](luis-concept-roles.md)

## <a name="next-steps"></a>Nächste Schritte

In diesem Tutorial wurden eine neue Absicht erstellt und Beispieläußerungen für die im Kontext gelernten Daten von Ursprungs- und Zielort hinzugefügt. Nachdem die App trainiert und veröffentlicht ist, kann eine Clientanwendung diese Informationen für einen Verlegungsauftrag mit den relevanten Informationen verwenden.

> [!div class="nextstepaction"] 
> [Hinzufügen einer zusammengesetzten Entität](luis-tutorial-composite-entity.md) 
