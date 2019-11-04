---
title: 'Tutorial: Musterrollen: LUIS'
titleSuffix: Azure Cognitive Services
description: Mithilfe von Mustern können Daten aus einer wohlgeformten Vorlagenäußerung extrahiert werden. Die Vorlagenäußerung verwendet eine einfache Entität und Rollen zum Extrahieren aufeinander bezogener Daten, wie etwa einen Ursprungsort und einen Zielort.
ms.custom: seodec18
services: cognitive-services
author: diberry
manager: nitinme
ms.service: cognitive-services
ms.subservice: language-understanding
ms.topic: tutorial
ms.date: 10/14/2019
ms.author: diberry
ms.openlocfilehash: 13a1589a6cc6ed48f159f361ff69a5a3ba8f0f80
ms.sourcegitcommit: c22327552d62f88aeaa321189f9b9a631525027c
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 11/04/2019
ms.locfileid: "73499448"
---
# <a name="tutorial-extract-contextually-related-patterns-using-roles"></a>Tutorial: Extrahieren kontextbezogener Muster mithilfe von Rollen

In diesem Tutorial verwenden Sie ein Muster, um Daten aus einer wohlgeformten Vorlagenäußerung zu extrahieren. Die Vorlagenäußerung verwendet eine [einfache Entität](luis-concept-entity-types.md#simple-entity) und [Rollen](luis-concept-roles.md) zum Extrahieren aufeinander bezogener Daten, wie etwa einen Ursprungsort und einen Zielort.  Bei der Verwendung von Mustern sind weniger Beispieläußerungen für die Absicht erforderlich.

[!INCLUDE [Waiting for LUIS portal refresh](./includes/wait-v3-upgrade.md)]

**In diesem Tutorial lernen Sie Folgendes:**

> [!div class="checklist"]
> * Importieren der Beispiel-App
> * Erstellen von neuen Entitäten
> * Erstellen einer neuen Absicht
> * Trainieren
> * Veröffentlichen
> * Abrufen von Absichten und Entitäten vom Endpunkt
> * Erstellen von Mustern mit Rollen
> * Erstellen von Ausdruckslisten von Städten
> * Abrufen von Absichten und Entitäten vom Endpunkt

[!INCLUDE [LUIS Free account](../../../includes/cognitive-services-luis-free-key-short.md)]

## <a name="using-roles-in-patterns"></a>Verwendung von Rollen in Mustern

Der Zweck der Rollen besteht darin, kontextabhängige Entitäten in einer Äußerung zu extrahieren. In der Äußerung `Move new employee Robert Williams from Sacramento and San Francisco` sind die Werte für den Ursprungs- und Zielort miteinander verknüpft, und sie verwenden eine gemeinsame Sprache zur Bezeichnung der einzelnen Standorte. 


Der Name des neuen Mitarbeiters, Billy Patterson, ist noch nicht Teil der Listenentität **Employee**. Der Name des neuen Mitarbeiters wird zuerst extrahiert, um den Namen für die Erstellung der Unternehmensanmeldeinformationen an ein externes System zu senden. Nachdem die Unternehmensanmeldeinformationen erstellt wurden, werden die Mitarbeiteranmeldeinformationen der Listenentität **Employee** hinzugefügt.

Der neue Mitarbeiter und seine Familie müssen von dem aktuellen Ort in den Ort verlegt werden, in dem das fiktive Unternehmen ansässig ist. Da ein neuer Mitarbeiter von einem beliebigen Ort herkommen kann, müssen die Standorte ermittelt werden. Eine festgelegte Liste wie eine Listenentität würde nicht funktionieren, da nur die Orte in der Liste extrahiert werden würden.

Die den Ursprungs- und Zielorten zugeordneten Rollennamen müssen für alle Entitäten eindeutig sein. Eine einfache Möglichkeit, sicherzustellen, dass die Rollen eindeutig sind, besteht darin, diese mittels einer Benennungsstrategie der enthaltenen Entität zuzuordnen. Die Entität **NewEmployeeRelocation** ist eine einfache Entität mit zwei Rollen: **NewEmployeeReloOrigin** und **NewEmployeeReloDestination**. Relo ist die Kurzform für „Relocation“ (Verlegung).

Da die Beispieläußerung `Move new employee Robert Williams from Sacramento and San Francisco` nur über per Machine Learning trainierte Entitäten verfügt, ist es wichtig, genügend Beispieläußerungen für die Absicht bereitzustellen, damit die Entitäten erkannt werden.  

**Muster ermöglichen Ihnen, weniger Beispieläußerungen bereitzustellen. Doch wenn die Entitäten nicht erkannt werden, stimmt das Muster nicht überein.**

Wenn Sie Schwierigkeiten mit der Erkennung einfacher Entitäten haben, da es um einen Namen wie den eines Orts geht, sollten Sie eventuell eine Liste von Ausdrücken mit ähnlichen Werten hinzufügen. Dadurch wird die Erkennung des Ortsnamens verbessert, da der LUIS-App ein zusätzlicher Hinweis für den jeweiligen Wort- oder Ausdruckstyp zur Verfügung gestellt wird. Ausdruckslisten sind für Muster nur im Hinblick auf die Entitätserkennung nützlich, die für die Musterübereinstimmung erforderlich ist. 

## <a name="import-example-app"></a>Importieren der Beispiel-App
Fahren Sie mit der im letzten Tutorial erstellten App mit dem Namen **HumanResources** fort. 

Führen Sie die folgenden Schritte aus:

1.  Laden Sie die [App-JSON-Datei](https://github.com/Azure-Samples/cognitive-services-language-understanding/blob/master/documentation-samples/tutorials/custom-domain-patterns-HumanResources-v2.json) herunter, und speichern Sie sie.

2. Importieren Sie den JSON-Code in eine neue App.

3. Klonen Sie die Version von der Registerkarte **Versionen** aus dem Abschnitt **Verwalten**, und geben Sie ihr den Namen `roles`. Durch Klonen können Sie ohne Auswirkungen auf die ursprüngliche Version mit verschiedenen Features von LUIS experimentieren. Da der Versionsname als Teil der URL-Route verwendet wird, darf er keine Zeichen enthalten, die in einer URL ungültig sind.

## <a name="create-new-entities"></a>Erstellen von neuen Entitäten

1. [!INCLUDE [Start in Build section](../../../includes/cognitive-services-luis-tutorial-build-section.md)]

2. Wählen Sie im linken Navigationsmenü die Option **Entitäten** aus. 

3. Wählen Sie **Neue Entität erstellen** aus.

4. Geben Sie im Popupfenster `NewEmployee` als **Simple**-Entität ein.

5. Wählen Sie **Neue Entität erstellen** aus.

6. Geben Sie im Popupfenster `NewEmployeeRelocation` als **Simple**-Entität ein.

7. Wählen Sie aus der Liste der Entitäten **NewEmployeeRelocation** aus. 

8. Geben Sie als erste Rolle `NewEmployeeReloOrigin` ein, und drücken Sie die EINGABETASTE.

9. Geben Sie als zweite Rolle `NewEmployeeReloDestination` ein, und drücken Sie die EINGABETASTE.

## <a name="create-new-intent"></a>Erstellen einer neuen Absicht
Die Bezeichnung der Entitäten in den folgenden Schritten ist möglicherweise einfacher, wenn Sie vor der Durchführung der Schritte die vordefinierte keyPhrase-Entität entfernen und sie dann wieder hinzufügen, nachdem Sie die Schritte in diesem Abschnitt abgeschlossen haben. 

1. Wählen Sie im linken Navigationsbereich **Absichten** aus.

2. Wählen Sie **Create new intent** (Neue Absicht erstellen) aus. 

3. Geben Sie `NewEmployeeRelocationProcess` als Namen für die Absicht in das Popupdialogfeld ein.

4. Geben Sie die folgenden Beispieläußerungen ein, die die neuen Entitäten bezeichnen. Die Entitäts- und Rollenwerte werden in Fettschrift angezeigt. Denken Sie daran, zu der **Tokenansicht** zu wechseln, wenn Sie es einfacher finden, den Text zu bezeichnen. 

    Die Angabe der Entitätsrolle erfolgt nicht bei der Bezeichnung in der Absicht. Dies geschieht später bei der Erstellung des Musters. 

    |Äußerung|NewEmployee|NewEmployeeRelocation|
    |--|--|--|
    |Move **Bob Jones** from **Seattle** to **Los Colinas**|Bob Jones|Seattle, Los Colinas|
    |Move **Dave C. Cooper** from **Redmond** to **New York City**|Dave C. Cooper|Redmond, New York City|
    |Move **Jim Paul Smith** from **Toronto** to **West Vancouver**|Jim Paul Smith|Toronto, West Vancouver|
    |Move **J. Benson** from **Boston** to **Staines-upon-Thames**|J. Benson|Boston, Staines-upon-Thames|
    |Move **Travis "Trav" Hinton** from **Castelo Branco** to **Orlando**|Travis "Trav" Hinton|Castelo Branco, Orlando|
    |Move **Trevor Nottington III** from **Aranda de Duero** to **Boise**|Trevor Nottington III|Aranda de Duero, Boise|
    |Move **Dr. Greg Williams** from **Orlando** to **Ellicott City**|Dr. Greg Williams|Orlando, Ellicott City|
    |Move **Robert "Bobby" Gregson** from **Kansas City** to **San Juan Capistrano**|Robert "Bobby" Gregson|Kansas City, San Juan Capistrano|
    |Move **Patti Owens** from **Bellevue** to **Rockford**|Leni Schneider|Bellevue, Rockford|
    |Move **Janet Bartlet** from **Tuscan** to **Santa Fe**|Janet Bartlet|Tuscan, Santa Fe|

    Die Mitarbeiternamen weisen eine Vielzahl von Präfixen, Wortzahlen, Syntaxen und Suffixen auf. Dies ist wichtig, damit die LUIS-App die Varianten eines Namens eines neuen Mitarbeiters auflösen kann. Die Ortsnamen weisen ebenfalls unterschiedliche Wortzahlen und Syntaxen auf. Diese Vielfalt ist wichtig, um der LUIS-App beizubringen, wie diese Entitäten in der Äußerung eines Benutzers vorkommen können. 
    
    Wenn beide Entitäten die gleiche Wortzahl und keine anderen Varianten aufweisen würden, würden Sie der LUIS-App damit beibringen, dass diese Entität nur diese Wortzahl und keine andere Varianten aufweist. Die LUIS-App wäre so nicht in der Lage, ein größeres Spektrum von Varianten richtig vorherzusagen, da diese nicht vermittelt wurden. 

    Wenn Sie die keyPhrase-Entität entfernt haben, fügen Sie sie nun wieder der App hinzu.

## <a name="train"></a>Trainieren

[!INCLUDE [LUIS How to Train steps](../../../includes/cognitive-services-luis-tutorial-how-to-train.md)]

## <a name="publish"></a>Veröffentlichen

[!INCLUDE [LUIS How to Publish steps](../../../includes/cognitive-services-luis-tutorial-how-to-publish.md)]

## <a name="get-intent-and-entities-from-endpoint"></a>Abrufen von Absicht und Entitäten von einem Endpunkt

1. [!INCLUDE [LUIS How to get endpoint first step](../../../includes/cognitive-services-luis-tutorial-how-to-get-endpoint.md)] 

2. Geben Sie in der Adressleiste am Ende der URL `Move Wayne Berry from Miami to Mount Vernon` ein. Der letzte Parameter der Abfragezeichenfolge lautet `q` (für die Abfrage (**query**) der Äußerung). 

    ```json
    {
      "query": "Move Wayne Berry from Newark to Columbus",
      "topScoringIntent": {
        "intent": "NewEmployeeRelocationProcess",
        "score": 0.514479756
      },
      "intents": [
        {
          "intent": "NewEmployeeRelocationProcess",
          "score": 0.514479756
        },
        {
          "intent": "Utilities.Confirm",
          "score": 0.017118983
        },
        {
          "intent": "MoveEmployee",
          "score": 0.009982505
        },
        {
          "intent": "GetJobInformation",
          "score": 0.008637771
        },
        {
          "intent": "ApplyForJob",
          "score": 0.007115978
        },
        {
          "intent": "Utilities.StartOver",
          "score": 0.006120186
        },
        {
          "intent": "Utilities.Cancel",
          "score": 0.00452428637
        },
        {
          "intent": "None",
          "score": 0.00400899537
        },
        {
          "intent": "OrgChart-Reports",
          "score": 0.00240071164
        },
        {
          "intent": "Utilities.Help",
          "score": 0.001770991
        },
        {
          "intent": "EmployeeFeedback",
          "score": 0.001697356
        },
        {
          "intent": "OrgChart-Manager",
          "score": 0.00168116146
        },
        {
          "intent": "Utilities.Stop",
          "score": 0.00163952739
        },
        {
          "intent": "FindForm",
          "score": 0.00112958835
        }
      ],
      "entities": [
        {
          "entity": "wayne berry",
          "type": "NewEmployee",
          "startIndex": 5,
          "endIndex": 15,
          "score": 0.629158735
        },
        {
          "entity": "newark",
          "type": "NewEmployeeRelocation",
          "startIndex": 22,
          "endIndex": 27,
          "score": 0.638941
        }
      ]
    }  
    ```

Das Ergebnis der Absichtsvorhersage beträgt nur etwa 50 %. Wenn für Ihre Clientanwendung eine höhere Zahl erforderlich ist, müssen Sie dies korrigieren. Die beiden Entitäten wurden nicht vorhergesagt.

Einer der Orte wurde extrahiert, der andere jedoch nicht. 

Muster sind für das Vorhersageergebnis zwar hilfreich, allerdings müssen die Entitäten korrekt vorhergesagt werden, damit das Muster mit der Äußerung übereinstimmt. 

## <a name="pattern-with-roles"></a>Muster mit Rollen

1. Wählen Sie in der oberen Navigationsleiste **Erstellen** aus.

2. Wählen Sie im linken Navigationsbereich **Muster** aus.

3. Wählen Sie **NewEmployeeRelocationProcess** aus der Dropdownliste **Eine Absicht auswählen** aus. 

4. Geben Sie das folgende Muster ein: `move {NewEmployee} from {NewEmployeeRelocation:NewEmployeeReloOrigin} to {NewEmployeeRelocation:NewEmployeeReloDestination}[.]`

    Wenn Sie den Endpunkt trainieren, veröffentlichen und abfragen, werden Sie möglicherweise enttäuscht sein, wenn die Entitäten nicht gefunden werden, und daraus den Schluss ziehen, dass das Muster nicht übereinstimmt und die Vorhersage nicht verbessert wurde. Dies ist darauf zurückzuführen, dass nicht genügend Beispieläußerungen mit bezeichneten Entitäten vorhanden sind. Um dieses Problem zu beheben, fügen Sie statt weiteren Beispielen eine Liste von Ausdrücken hinzu.

## <a name="cities-phrase-list"></a>Städte-Ausdrucksliste
Ortsnamen sind wie Namen von Personen schwierig, da sie eine beliebige Kombination von Wörtern und Interpunktion aufweisen können. Die Orte in der Region und auf der Welt sind bekannt, sodass LUIS für den Einstieg in das Training eine Ausdrucksliste mit Orten benötigt. 

1. Wählen Sie im Menü auf der linken Seite im Abschnitt **App-Leistung verbessern** die Option **Liste von Ausdrücken** aus. 

2. Versehen Sie die Liste mit dem Namen `Cities`, und fügen Sie die folgenden `values` für die Liste hinzu:

    |Werte der Ausdrucksliste|
    |--|
    |Seattle|
    |San Diego|
    |New York City|
    |Los Angeles|
    |Portland|
    |Philadelphia|
    |Miami|
    |Dallas|

    Fügen Sie nicht jeden Ort auf der Welt oder sogar jeden Ort in der Region hinzu. Die LUIS-App muss generalisieren können, welcher Ort aus der Liste stammt. Achten Sie darauf, dass die Option **Diese Werte sind austauschbar** aktiviert bleibt. Diese Einstellung bedeutet, dass die Wörter in der Liste als Synonyme behandelt werden. 

3. Trainieren und veröffentlichen Sie die App.

## <a name="get-intent-and-entities-from-endpoint"></a>Abrufen von Absicht und Entitäten von einem Endpunkt

1. [!INCLUDE [Start in Build section](../../../includes/cognitive-services-luis-tutorial-build-section.md)]

2. Geben Sie in der Adressleiste am Ende der URL `Move wayne berry from miami to mount vernon` ein. Der letzte Parameter der Abfragezeichenfolge lautet `q` (für die Abfrage (**query**) der Äußerung). 

    ```json
    {
      "query": "Move Wayne Berry from Miami to Mount Vernon",
      "topScoringIntent": {
        "intent": "NewEmployeeRelocationProcess",
        "score": 0.9999999
      },
      "intents": [
        {
          "intent": "NewEmployeeRelocationProcess",
          "score": 0.9999999
        },
        {
          "intent": "Utilities.Confirm",
          "score": 1.49678385E-06
        },
        {
          "intent": "MoveEmployee",
          "score": 8.240291E-07
        },
        {
          "intent": "GetJobInformation",
          "score": 6.3131273E-07
        },
        {
          "intent": "None",
          "score": 4.25E-09
        },
        {
          "intent": "OrgChart-Manager",
          "score": 2.8E-09
        },
        {
          "intent": "OrgChart-Reports",
          "score": 2.8E-09
        },
        {
          "intent": "EmployeeFeedback",
          "score": 1.64E-09
        },
        {
          "intent": "Utilities.StartOver",
          "score": 1.64E-09
        },
        {
          "intent": "Utilities.Help",
          "score": 1.48181822E-09
        },
        {
          "intent": "Utilities.Stop",
          "score": 1.48181822E-09
        },
        {
          "intent": "Utilities.Cancel",
          "score": 1.35E-09
        },
        {
          "intent": "FindForm",
          "score": 1.23846156E-09
        },
        {
          "intent": "ApplyForJob",
          "score": 5.692308E-10
        }
      ],
      "entities": [
        {
          "entity": "wayne berry",
          "type": "builtin.keyPhrase",
          "startIndex": 5,
          "endIndex": 15
        },
        {
          "entity": "miami",
          "type": "builtin.keyPhrase",
          "startIndex": 22,
          "endIndex": 26
        },
        {
          "entity": "wayne berry",
          "type": "NewEmployee",
          "startIndex": 5,
          "endIndex": 15,
          "score": 0.9410646,
          "role": ""
        },
        {
          "entity": "miami",
          "type": "NewEmployeeRelocation",
          "startIndex": 22,
          "endIndex": 26,
          "score": 0.9853915,
          "role": "NewEmployeeReloOrigin"
        },
        {
          "entity": "mount vernon",
          "type": "NewEmployeeRelocation",
          "startIndex": 31,
          "endIndex": 42,
          "score": 0.986044347,
          "role": "NewEmployeeReloDestination"
        }
      ],
      "sentimentAnalysis": {
        "label": "neutral",
        "score": 0.5
      }
   }
    ```

Das Absichtsergebnis fällt nun sehr viel höher aus, und die Rollennamen sind Teil der Entitätsantwort.

## <a name="clean-up-resources"></a>Bereinigen von Ressourcen

[!INCLUDE [LUIS How to clean up resources](../../../includes/cognitive-services-luis-tutorial-how-to-clean-up-resources.md)]

## <a name="next-steps"></a>Nächste Schritte

In diesem Tutorial wurden eine Entität mit Rollen und eine Absicht mit Beispieläußerungen hinzugefügt. Die erste Endpunktvorhersage mit ordnungsgemäßer Verwendung der Entität führte zu einer richtigen Vorhersage der Absicht, jedoch mit einer niedrigen Zuverlässigkeitsbewertung. Nur eine der beiden Entitäten wurde erkannt. Als Nächstes fügte das Tutorial ein Muster, das die Entitätsrollen verwendete, und eine Ausdrucksliste hinzu, um den Wert der Städtenamen in den Äußerungen zu verstärken. Die zweite Endpunktvorhersage gab eine hohe Zuverlässigkeitsbewertung zurück, und beide Entitätsrollen wurden gefunden. 

> [!div class="nextstepaction"]
> [Best Practices für LUIS-Apps kennenlernen](luis-concept-best-practices.md)
