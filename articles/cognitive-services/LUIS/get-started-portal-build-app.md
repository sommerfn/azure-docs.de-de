---
title: 'Schnellstart: Erstellen einer neuen App im LUIS-Portal'
titleSuffix: Azure Cognitive Services
description: In dieser Schnellstartanleitung erstellen Sie die grundlegenden Komponenten einer App sowie Absichten und Entitäten. Außerdem verwenden Sie zu Testzwecken eine Beispieläußerung im LUIS-Portal.
services: cognitive-services
author: diberry
manager: nitinme
ms.service: cognitive-services
ms.subservice: language-understanding
ms.topic: quickstart
ms.date: 11/04/2019
ms.author: diberry
ms.openlocfilehash: 087b3a61902c533648b5d6e1b4b763f88ee5d794
ms.sourcegitcommit: 609d4bdb0467fd0af40e14a86eb40b9d03669ea1
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 11/06/2019
ms.locfileid: "73669678"
---
# <a name="quickstart-create-a-new-app-in-the-luis-portal"></a>Schnellstart: Erstellen einer neuen App im LUIS-Portal

[!INCLUDE [Uses preview portal](./includes/uses-portal-preview.md)]

In diesem Schnellstart erstellen Sie eine neue App im [LUIS-Portal](https://www.luis.ai). Zuerst erstellen Sie die Grundbestandteile einer App, **Absichten** und **Entitäten**. Anschließend testen Sie die App, indem Sie im interaktiven Testbereich eine Beispielbenutzeräußerung eingeben, um die vorhergesagte Absicht zu erhalten.

Das Erstellen einer App ist kostenlos, und es ist kein Azure-Abonnement erforderlich. Wenn Sie so weit sind, Ihre App bereitzustellen, lesen Sie den [Schnellstart zum Bereitstellen einer App](get-started-portal-deploy-app.md). Dort erfahren Sie, wie Sie eine Azure Cognitive Service-Ressource erstellen und sie der App zuweisen.

[!INCLUDE [Sign in to LUIS](./includes/sign-in-process.md)]

## <a name="create-an-app"></a>Erstellen einer App

1. Wählen Sie auf der Kontextsymbolleiste die Option **+ Erstellen** aus.

   [![Erstellen einer neuen App im LUIS-Portal](./media/create-app-in-portal.png)](./media/create-app-in-portal.png#lightbox)

1. Konfigurieren Sie die App im Popupfenster mit den folgenden Einstellungen, und wählen Sie dann **Fertig** aus.

   |Einstellungsname| Wert | Zweck|
   |--|--|--|
   |NAME|`myEnglishApp`|Eindeutiger Name der LUIS-App<br>required|
   |Kultur|**Englisch**|Sprache der Äußerungen von Benutzern, **en-us**<br>required|
   |Beschreibung (optional)|`App made with LUIS Portal`|Beschreibung der App<br>optional|
   |Vorhersageressource (optional) |-  |Nicht auswählen. Sie erhalten von LUIS einen Startschlüssel für die kostenlose Erstellung sowie für 1.000 Vorhersageendpunktanforderungen. |

   ![Eingeben der Einstellungen für die neue App](./media/get-started-portal-build-app/create-new-app-settings.png)

## <a name="create-intents"></a>Erstellen von Absichten

Nachdem die LUIS-App erstellt wurde, müssen Sie Absichten erstellen. Absichten dienen zum Kategorisieren der Texteingaben von Benutzern. Beispielsweise kann eine Personalverwaltungs-App zwei Funktionen aufweisen. Personen dabei zu helfen:

 1. Stellen zu finden und sich auf sie zu bewerben
 1. Formulare für die Bewerbung auf Stellen zu finden

Die zwei verschiedenen _Absichten_ der App richten sich an den folgenden Absichten aus:

|Absicht|Beispieltext vom Benutzer<br>bezeichnet als _Äußerung_|
|--|--|
|ApplyForJob|`I want to apply for the new software engineering position in Cairo.`|
|FindForm|`Where is the job transfer form hrf-123456?`|

Führen Sie die folgenden Schritte aus, um Absichten zu erstellen:

1. Nachdem die App erstellt wurde, wird die Seite **Intents** (Absichten) des Abschnitts **Erstellen** angezeigt. Klicken Sie auf **Erstellen**.

   [![Auswählen von „Erstellen“, um eine neue Absicht zu erstellen](./media/get-started-portal-build-app/create-new-intent-button.png)](./media/get-started-portal-build-app/create-new-intent-button.png#lightbox)

1. Geben Sie den Namen der Absicht ein, `FindForm`, und wählen Sie dann **Fertig** aus.

## <a name="add-an-example-utterance"></a>Hinzufügen einer Beispieläußerung

Nach dem Erstellen von Absichten fügen Sie Beispieläußerungen hinzu. Beispieläußerungen sind Text, den ein Benutzer in einen Chatbot oder eine andere Clientanwendung eingibt. Sie bilden die Absicht des Texts des Benutzers auf eine LUIS-Absicht ab.

Für die `FindForm`-Absicht dieser Beispielanwendung gehört zu den Beispieläußerungen die Formularnummer. Die Clientanwendung benötigt die Formularnummer, um die Anforderung des Benutzers zu erfüllen, daher ist es wichtig, sie in die Äußerung einzuschließen.

[![Eingeben von Beispieläußerungen für die Absicht „FindForm“](./media/get-started-portal-build-app/add-example-utterance.png)](./media/get-started-portal-build-app/add-example-utterance.png#lightbox)

Fügen Sie der `FindForm`-Absicht die folgenden 15 Beispieläußerungen hinzu.

|#|Beispiele für Äußerungen|
|--|--|
|1|Looking for hrf-123456 (Nach hrf-123456 suchen)|
|2|Where is the human resources form hrf-234591? (Wo ist das Personalformular hrf-234591?)|
|3|hrf-345623, where is it (hrf-345623, wo)|
|4|Is it possible to send me hrf-345794 (Kann mir hrf-345794 zugesendet werden)|
|5|Do I need hrf-234695 to apply for an internal job? (Benötige ich hrf-234695 zur Bewerbung um eine interne Stelle?)|
|6|Does my manager need to know I'm applying for a job with hrf-234091 (Muss mein Vorgesetzter wissen, dass ich mich mit hrf-234091 um eine Stelle bewerbe)|
|7|Where do I send hrf-234918? Do I get an email response it was received? (Wohin muss ich hrf-234918 senden? Wird der Empfang per E-Mail bestätigt?)|
|8|hrf-234555|
|9|When was hrf-234987 updated? (Wann wurde hrf-234987 aktualisiert?)|
|10|Do I use form hrf-876345 to apply for engineering positions (Verwende ich das Formular hrf-876345 zur Bewerbung um Technikerstellen)|
|11|Was a new version of hrf-765234 submitted for my open req? (Wurde für meine offene Anfrage eine neue Version von hrf-765234 übermittelt?)|
|12|Do I use hrf-234234 for international jobs? (Muss ich für interne Stellen hrf-234234 verwenden?)|
|13|hrf-234598 spelling mistake (Rechtschreibfehler in hrf-234598)|
|14|will hrf-234567 be edited for new requirements (Wird hrf-234567 mit neuen Anforderungen überarbeitet)|
|15|hrf-123456, hrf-123123, hrf-234567|

Diese Beispieläußerungen unterscheiden sich entwurfsbedingt in folgenden Punkten:

* Länge der Äußerung
* Interpunktion
* Wortwahl
* Zeitform des Verbs (is, was, will be)
* Reihenfolge der Wörter



## <a name="create-a-regular-expression-entity"></a>Erstellen einer Entität vom Typ „Regulärer Ausdruck“

Um in der Vorhersageantwort zur Laufzeit die Formularnummer zurückzugeben, muss das Formular als Entität gekennzeichnet sein. Da der Text mit der Formularnummer sehr strukturiert ist, können Sie ihn mithilfe einer Entität vom Typ „Regulärer Ausdruck“ markieren. Gehen Sie wie folgt vor, um die Entität zu erstellen:

1. Wählen Sie im Menü links die Option **Entitäten** aus.

1. Wählen Sie auf der Seite **Entitäten** die Option **Erstellen** aus.

1. Geben Sie den Namen `Human Resources Form Number` ein, und wählen Sie den Entitätstyp **RegEx** und anschließend **Weiter** aus.

   ![Erstellen einer Entität vom Typ „Regulärer Ausdruck“](./media/get-started-portal-build-app/create-regular-expression-entity.png)

1. Geben Sie den folgenden regulären Ausdruck (**RegEx**) ein: `hrf-[0-9]{6}`. Dieser Eintrag entspricht den Literalzeichen `hrf-` und lässt genau sechs Ziffern zu. Wählen Sie anschließend **Erstellen** aus.

   ![Eingeben eines regulären Ausdrucks für eine Entität](./media/get-started-portal-build-app/create-regular-expression-entity-with-expression.png)


## <a name="add-example-utterances-to-the-none-intent"></a>Hinzufügen von Beispieläußerungen zur Absicht „None“

Die Absicht **None** ist die Fallbackabsicht und sollte nicht leer gelassen werden. Diese Absicht sollte eine Äußerung für jeweils 10 Beispieläußerungen enthalten, die Sie für die anderen Absichten der App hinzugefügt haben.

Die Beispieläußerungen der Absicht **None** dürfen nicht im Bereich der Clientanwendung enthalten sein.

1. Klicken Sie im linken Menü auf **Intents** (Absichten), und wählen Sie dann in der Liste der Absichten **None** (Keine) aus.

1. Fügen Sie der Absicht die folgenden Beispieläußerungen hinzu.

   |Beispieläußerungen für die Absicht „None“|
   |--|
   |Bellende Hunde sind nervig|
   |Bestell mir eine Pizza|
   |Pinguine im Ozean|

   Diese Beispieläußerungen sind für die aktuelle App nicht relevant. Falls Begriffe für Tiere, Lebensmittel oder den Ozean für Ihre App relevant sind, müssen für die Absicht **None** andere Beispieläußerungen verwendet werden.

## <a name="train-the-app"></a>Trainieren der App

[!INCLUDE [LUIS How to Train steps](includes/howto-train.md)]

## <a name="look-at-the-regular-expression-entity-in-the-example-utterances"></a>Überprüfen der Entität vom Typ „Regulärer Ausdruck“ in den Beispieläußerungen

1. Überprüfen Sie, ob die Entität in der Absicht **FindForm** gefunden wird, indem Sie im linken Menü **Intents** (Absichten) auswählen. Wählen Sie dann die Absicht **FindForm** aus.

   Die Entität ist an den Stellen markiert, an denen sie in den Beispieläußerungen vorkommt. Falls Sie anstelle des Entitätsnamens den ursprünglichen Text anzeigen möchten, schalten Sie über die Symbolleiste die **Entities View** (Entitätsansicht) um.

   [![Alle Beispieläußerungen mit markierten Entitäten](./media/get-started-portal-build-app/all-example-utterances-marked-with-entities.png)](./media/get-started-portal-build-app/all-example-utterances-marked-with-entities.png#lightbox)

## <a name="test-your-new-app-with-the-interactive-test-pane"></a>Testen der neuen App im interaktiven Testbereich

Verwenden Sie den interaktiven **Testbereich** im LUIS-Portal, um zu überprüfen, ob die Entität aus neuen Äußerungen, die die App noch nicht kennt, extrahiert wird.

1. Wählen Sie im Menü oben rechts die Option **Testen** aus.

1. Fügen Sie eine neue Äußerung hinzu, und drücken Sie dann die EINGABETASTE:

   ```Is there a form named hrf-234098```

   ![Testen einer neuen Äußerung im Testbereich](./media/get-started-portal-build-app/test-new-utterance.png)

   Die am stärksten vorhergesagte Absicht ist richtigerweise **FindForm** mit einer Zuverlässigkeit von mehr als 90 % (0,977). Die Entität **Personalwesen-Formularnummer** wird mit dem Wert hrf-234098 extrahiert.

## <a name="clean-up-resources"></a>Bereinigen von Ressourcen

Wenn Sie mit diesem Schnellstart fertig sind und nicht mit dem nächsten Schnellstart fortfahren möchten, wählen Sie im oberen Navigationsmenü **Meine Apps** aus. Aktivieren Sie dann in der Liste das Kontrollkästchen links neben der App, und wählen Sie auf der Kontextsymbolleiste über der Liste die Option **Löschen** aus.

## <a name="next-steps"></a>Nächste Schritte

> [!div class="nextstepaction"]
> [2. Bereitstellen einer App](get-started-portal-deploy-app.md)
