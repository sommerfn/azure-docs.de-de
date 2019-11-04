---
title: 'Tutorial: Entität vom Typ „Pattern.any“: LUIS'
titleSuffix: Azure Cognitive Services
description: Verwenden Sie die Entität „pattern.any“, um Daten aus Äußerungen zu extrahieren, in denen die Äußerungen wohlgeformt sind und das Ende der Daten leicht mit den verbleibenden Wörtern der Äußerung zu verwechseln ist.
services: cognitive-services
ms.custom: seodec18
author: diberry
manager: nitinme
ms.service: cognitive-services
ms.subservice: language-understanding
ms.topic: tutorial
ms.date: 10/14/2019
ms.author: diberry
ms.openlocfilehash: 43ad6558b685d3959562be720f7494b3d2a7334e
ms.sourcegitcommit: c22327552d62f88aeaa321189f9b9a631525027c
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 11/04/2019
ms.locfileid: "73499423"
---
# <a name="tutorial-extract-free-form-data-with-patternany-entity"></a>Tutorial: Extrahieren von Freiformdaten mit der Entität „pattern.any“

In diesem Tutorial verwenden Sie die Entität „pattern.any“, um Daten aus Äußerungen zu extrahieren, in denen die Äußerungen wohlgeformt sind und das Ende der Daten leicht mit den verbleibenden Wörtern der Äußerung zu verwechseln ist. 

[!INCLUDE [Waiting for LUIS portal refresh](./includes/wait-v3-upgrade.md)]

**In diesem Tutorial lernen Sie Folgendes:**

> [!div class="checklist"]
> * Importieren der Beispiel-App
> * Hinzufügen von Beispieläußerungen zu einer vorhandenen Entität
> * Erstellen einer Pattern.any-Entität
> * Erstellen eines Musters
> * Trainieren
> * Testen des neuen Musters

[!INCLUDE [LUIS Free account](../../../includes/cognitive-services-luis-free-key-short.md)]

## <a name="using-patternany-entity"></a>Verwenden der Entität „pattern.any“

Mit der Entität „pattern.any“ können Sie Freiformdaten finden, bei denen die Formulierung der Entität erschwert, das Ende der Entität vom Rest der Äußerung zu unterscheiden. 

Diese Human Resources-App hilft Mitarbeitern, Unternehmensformulare zu finden. 

|Äußerung|
|--|
|Wo ist **HRF-123456**?|
|Wer hat **HRF-123234** erstellt?|
|Ist **HRF-456098** in Französisch veröffentlicht?|

Jedes Formular verfügt jedoch sowohl über einen formatierten Name, der in der obigen Tabelle verwendet wird, als auch einen Anzeigenamen, z.B. `Request relocation from employee new to the company 2018 version 5`. 

Äußerungen mit dem Anzeigenamen sehen folgendermaßen aus:

|Äußerung|
|--|
|Wo ist **Request relocation from employee new to the company 2018 version 5**?|
|Wer hat **Request relocation from employee new to the company 2018 version 5** erstellt?|
|Wurde **Request relocation from employee new to the company 2018 version 5** in Französisch veröffentlicht?|

Die unterschiedlichen Längen enthalten Wörter, die es LUIS erschweren, das Ende von Entitäten zu erkennen. Mit einer Pattern.any-Entität in einem Muster können Sie den Anfang und das Ende des Formularnamens angeben, damit LUIS diesen ordnungsgemäß extrahiert.

|Beispiel für eine Vorlagenäußerung|
|--|
|Wo ist {FormName}[?]|
|Wer hat {FormName} erstellt[?]|
|Wurde {FormName} in Französisch veröffentlicht[?]|

## <a name="import-example-app"></a>Importieren der Beispiel-App

1. Laden Sie die [App-JSON-Datei](https://github.com/Azure-Samples/cognitive-services-language-understanding/blob/master/documentation-samples/tutorials/custom-domain-pattern-roles-HumanResources.json) herunter, und speichern Sie sie.

1. Importieren Sie im [LUIS-Portal](https://www.luis.ai) auf der Seite **Meine Apps** den JSON-Code in eine neue Anwendung.

1. Klonen Sie die Version von der Registerkarte **Versionen** aus dem Abschnitt **Verwalten**, und geben Sie ihr den Namen `patt-any`. Durch Klonen können Sie ohne Auswirkungen auf die ursprüngliche Version mit verschiedenen Features von LUIS experimentieren. Da der Versionsname als Teil der URL-Route verwendet wird, darf er keine Zeichen enthalten, die in einer URL ungültig sind.

## <a name="add-example-utterances"></a>Hinzufügen von Beispieläußerungen 

1. Wählen Sie **Erstellen** im oberen Navigationsbereich und dann **Absichten** im linken Navigationsbereich aus.

1. Wählen Sie **FindForm** in der Absichtenliste aus.

1. Fügen Sie einige Beispieläußerungen hinzu:

    |Beispieläußerung|
    |--|
    |Wo ist das Formular **What to do when a fire breaks out in the Lab**, und wer muss es unterzeichnen, nachdem ich es gelesen habe?|
    |Wo ist **Request relocation from employee new to the company** auf dem Server?|
    |Wer hat **Health and wellness requests on the main campus** erstellt, und was ist die aktuelle Version?|
    |Ich suche das Formular mit dem Namen **Office move request including physical assets**. |

    Ohne eine Pattern.any-Entität wäre es für LUIS schwierig, zu verstehen, wo der Titel des Formulars endet, weil viele Variationen des Formularnamens vorliegen.

## <a name="create-a-patternany-entity"></a>Erstellen einer Pattern.any-Entität
Die Pattern.any-Entität extrahiert Entitäten unterschiedlicher Länge. Sie funktioniert nur in einem Muster, da das Muster Anfang und Ende der Entität kennzeichnet.  

1. Wählen Sie im linken Navigationsbereich **Entitäten** aus.

1. Wählen Sie **Neue Entität erstellen** aus, geben Sie den Namen `FormName` ein, und wählen Sie **Pattern.any** als Typ aus. Wählen Sie **Fertig**aus. 

    Sie können die Entität in den Beispieläußerungen einer Absicht nicht beschriften, da Pattern.any nur in einem Muster gültig ist. 

    Wenn Sie möchten, dass die extrahierten Daten weitere Entitäten einschließen, z.B. „number“ oder datetimeV2, müssen Sie eine zusammengesetzte Entität erstellen, die Pattern.any als auch „number“ und datetimeV2 enthält.

## <a name="add-a-pattern-that-uses-the-patternany"></a>Hinzufügen eines Muster, das Pattern.any verwendet

1. Wählen Sie im linken Navigationsbereich **Muster** aus.

1. Wählen Sie die Absicht **FindForm** aus.

1. Geben Sie die folgenden Vorlagenäußerungen ein, die die neue Entität verwenden:

    |Vorlagenäußerungen|
    |--|
    |Wo ist das Formular ["]{FormName}["], und wer muss es unterzeichnen, nachdem ich es gelesen habe[?]|
    |Wo ist ["]{FormName}["] auf dem Server[?]|
    |Wer hat ["]{FormName}["] erstellt, und was ist die aktuelle Version[?]|
    |Ich suche das Formular mit dem Namen ["]{FormName}["][.]|

    Wenn Sie Variationen des Formulars wie z.B. einfache Anführungszeichen statt doppelter Anführungszeichen oder einen Punkt anstelle eines Fragezeichens berücksichtigen möchten, erstellen Sie für jede Variation ein neues Muster.

## <a name="train-the-luis-app"></a>Trainieren der LUIS-App

[!INCLUDE [LUIS How to Train steps](../../../includes/cognitive-services-luis-tutorial-how-to-train.md)]

## <a name="test-the-new-pattern-for-free-form-data-extraction"></a>Testen des neuen Musters für das Extrahieren von Freiformdaten
1. Wählen Sie auf der oberen Leiste **Test** aus, um den Testbereich zu öffnen. 

1. Geben Sie die folgende Äußerung ein: 

    `Where is the form Understand your responsibilities as a member of the community and who needs to sign it after I read it?`

1. Wählen Sie unter dem Ergebnis **Überprüfen** aus, um die Testergebnisse für die Entität und die Absicht anzuzeigen.

    Zuerst wird die Entität `FormName` gefunden, und danach das Muster, das die Absicht angibt. Wenn bei einem Testergebnis die Entitäten nicht erkannt wurden und damit das Muster nicht gefunden wurde, müssen Sie der Absicht (nicht dem Muster) weitere Beispieläußerungen hinzufügen.

1. Schließen Sie den Testbereich durch Auswählen der Schaltfläche **Test** auf der oberen Navigationsleiste.

## <a name="using-an-explicit-list"></a>Verwenden einer expliziten Liste

Wenn Ihr Muster eine Pattern.any-Entität enthält und Entitäten falsch extrahiert, verwenden Sie eine [explizite Liste](luis-concept-patterns.md#explicit-lists), um dieses Problem zu beheben.


## <a name="clean-up-resources"></a>Bereinigen von Ressourcen

[!INCLUDE [LUIS How to clean up resources](../../../includes/cognitive-services-luis-tutorial-how-to-clean-up-resources.md)]

## <a name="next-steps"></a>Nächste Schritte

In diesem Tutorial wurden einer vorhandenen Absicht Beispieläußerungen hinzugefügt und dann eine neue Pattern.any für den Formularnamen erstellt. Anschließend erstellte das Tutorial ein Muster für die vorhandene Absicht mit den neuen Beispieläußerungen und der Entität. Durch interaktives Testen wurde gezeigt, dass das Muster und seine Absicht vorhergesagt wurden, weil die Entität gefunden wurde. 

> [!div class="nextstepaction"]
> [Verwenden von Rollen mit einem Muster](luis-tutorial-pattern-roles.md)
