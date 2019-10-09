---
title: 'Schnellstart: Bereitstellen einer App im LUIS-Portal'
titleSuffix: Azure Cognitive Services
description: Erfahren Sie, wie Sie Ihre LUIS-App im Vorhersageendpunkt bereitstellen, sobald die App bereit ist, Äußerungsvorhersagen an eine Clientanwendung wie einen Chatbot zurückzugeben. Diese Schnellstartanleitung führt Sie schrittweise durch die Bereitstellung einer Anwendung, indem Sie eine Vorhersageendpunktressource erstellen, der App die Ressource zuweisen, die App trainieren und die App veröffentlichen.
services: cognitive-services
author: diberry
manager: nitinme
ms.service: cognitive-services
ms.subservice: language-understanding
ms.topic: quickstart
ms.date: 09/27/2019
ms.author: diberry
ms.openlocfilehash: f640921e6f48559db3f1414551d6ed974df15e4f
ms.sourcegitcommit: 6fe40d080bd1561286093b488609590ba355c261
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 10/01/2019
ms.locfileid: "71703219"
---
# <a name="quickstart-deploy-an-app-in-the-luis-portal"></a>Schnellstart: Bereitstellen einer App im LUIS-Portal

Wenn Ihre LUIS-App bereit ist, Äußerungsvorhersagen an eine Clientanwendung (beispielsweise einen Chatbot) zurückzugeben, müssen Sie die App für den Vorhersageendpunkt bereitstellen.

In diesem Schnellstart erfahren Sie, wie eine Anwendung bereitgestellt wird. Sie erstellen eine Vorhersageendpunktressource, weisen die Ressource der App zu, trainieren die App und veröffentlichen sie.

## <a name="prerequisites"></a>Voraussetzungen

* Erwerben Sie ein [Azure-Abonnement](https://azure.microsoft.com/free).
* Führen Sie den vorherigen [Schnellstart: Erstellen einer neuen App im LUIS-Portal](get-started-portal-build-app.md) durch, oder [laden Sie die App herunter, und importieren Sie die App](https://github.com/Azure-Samples/cognitive-services-language-understanding/blob/master/documentation-samples/quickstarts/in-portal/build-portal-app.json).

## <a name="create-the-endpoint-resource"></a>Erstellen der Endpunktressource

Sie erstellen die Vorhersageendpunktressource im Azure-Portal. Diese Ressource darf nur für Endpunktvorhersageabfragen verwendet werden. Verwenden Sie diese Ressource nicht für das Erstellen von Änderungen an der App.

1. Melden Sie sich beim [Azure-Portal](https://ms.portal.azure.com/) an.

1. Wählen Sie das grüne **+** -Anmeldesymbol im oberen linken Bereich aus. Suchen Sie im Marketplace nach `Cognitive Services`, und wählen Sie sie aus.

1. Konfigurieren Sie das Abonnement mit den folgenden Einstellungen:

   |Einstellung|Wert|Zweck|
   |--|--|--|
   |NAME|`my-cognitive-service-resource`|Der Name der Azure-Ressource. Sie benötigen diesen Namen, wenn Sie der App im LUIS-Portal die Ressource zuweisen.|
   |Subscription|Ihr Abonnement|Wählen Sie eines der mit Ihrem Konto verknüpften Abonnements aus.|
   |Location|**USA, Westen**|Die Azure-Region für diese Ressource.|
   |Tarif|**S0**|Der Standardtarif für diese Ressource.|
   |Resource group|`my-cognitive-service-resource-group`|Erstellen Sie eine neue Ressourcengruppe für alle Ihre Cognitive Services-Ressourcen. Wenn Sie mit den Ressourcen fertig sind, können Sie die Ressourcengruppe löschen, um Ihr Abonnement zu bereinigen. |
   | | | |

   ![Azure-API-Auswahl](./media/get-started-portal-deploy-app/create-cognitive-services-resource.png)

1. Wählen Sie **Erstellen** aus, um die Azure-Ressource zu erstellen.

   Im nächsten Abschnitt wird erläutert, wie Sie diese neue Ressource mit einer LUIS-App im LUIS-Portal verbinden.

## <a name="assign-the-resource-key-to-the-luis-app-in-the-luis-portal"></a>Zuweisen des Ressourcenschlüssels für die LUIS-App im LUIS-Portal

Jedes Mal, wenn Sie eine neue Ressource für LUIS erstellen, müssen Sie der LUIS-App die Ressource zuweisen. Nachdem Sie die Ressource zugewiesen haben, müssen Sie diesen Schritt nur dann erneut ausführen, wenn Sie eine neue Ressource erstellen. Sie können beispielsweise eine neue Ressource erstellen, um die Regionen Ihrer App zu erweitern oder eine größere Anzahl von Vorhersageabfragen zu unterstützen.

1. Melden Sie sich beim [LUIS-Portal](https://www.luis.ai) an, und wählen Sie in der Liste der Apps die App **myEnglishApp** aus.

1. Wählen Sie im Menü oben rechts die Option **Verwalten** aus, und wählen Sie dann **Azure-Ressourcen** aus.

1. Um LUIS hinzuzufügen, wählen Sie **Add prediction resource** (Vorhersageressource hinzufügen) aus.

    <!-- TBD: get screenshot-->

1. Wählen Sie Ihren Mandanten, Ihr Abonnement und den Namen Ihrer Ressource aus. Wählen Sie **Ressource zuweisen** aus.

   ![Zuweisen einer Ressource zu Ihrer App](./media/get-started-portal-deploy-app/assign-resource.png)

1. Suchen Sie die neue Zeile in der Tabelle, und kopieren Sie die Endpunkt-URL. Sie ist ordnungsgemäß aufgebaut, um eine `HTTP GET`-Anforderung an den LUIS-API-Endpunkt zum Abrufen einer Vorhersage zu senden.

## <a name="train-and-publish-the-app"></a>Trainieren und Veröffentlichen der App

Trainieren Sie die App, wenn Sie bereit sind, sie zu testen. Veröffentlichen Sie die App, sobald Sie die aktuell trainierte Version über die Laufzeit des Vorhersageendpunkts für Clientanwendungen verfügbar machen möchten.

1. Wenn die App noch nicht trainiert wurde, wählen Sie im Menü oben rechts die Option **Trainieren** aus.

1. Wählen Sie im Menü oben die Option **Veröffentlichen** aus. Übernehmen Sie die Standardumgebungseinstellungen, und wählen Sie **Veröffentlichen** aus.

1. Wenn oben im Browserfenster die grüne Erfolgsbenachrichtigungsleiste angezeigt wird, wählen Sie **Auf die Liste der Endpunkte verweisen** aus.

   ![Benachrichtigungsleiste im Browser für eine erfolgreich veröffentlichte App](./media/get-started-portal-deploy-app/successfully-published-notification.png)

1. Auf der Seite **Schlüssel- und Endpunkteinstellungen** befindet sich im unteren Bereich die Liste mit den zugewiesenen Ressourcen und entsprechenden Endpunkt-URLs.

1. Wählen Sie die Endpunkt-URL aus, die Ihrem neuen Ressourcennamen zugeordnet ist. Diese Aktion öffnet einen Webbrowser mit einer ordnungsgemäß aufgebauten URL, um eine `GET`-Anforderung an die Laufzeit des Vorhersageendpunkts zu senden.

## <a name="prediction-endpoint-request"></a>Anforderung für Vorhersageendpunkt

<!-- V3FIX -->

Die Zeichenfolge `q=` am Ende der URL ist die Kurzform für **Abfrage** (query). Dort wird die Äußerung des Benutzers an die GET-Anforderung angefügt. Geben Sie nach `q=` die gleiche Benutzeräußerung ein, die am Ende des vorherigen Schnellstarts verwendet wurde:

```Is there a form named hrf-234098```

Im Browser wird die Antwort angezeigt, bei der es sich um die gleiche JSON handelt, die Ihre Clientanwendung erhält:

```JSON
{
"query": "Is there a form named hrf-234098",
"topScoringIntent": {
    "intent": "FindForm",
    "score": 0.9768753
},
"intents": [
    {
    "intent": "FindForm",
    "score": 0.9768753
    },
    {
    "intent": "None",
    "score": 0.0216071066
    }
],
"entities": [
    {
    "entity": "hrf-234098",
    "type": "Human Resources Form Number",
    "startIndex": 22,
    "endIndex": 31
    }
    ]
}
```

Diese Antwort bietet Ihnen mehr Informationen als der Standardtestbereich im vorherigen Tutorial. Um im Testbereich die gleiche Informationsmenge anzuzeigen, müssen Sie die App veröffentlichen. Wählen Sie nach dem Veröffentlichen der App im Testbereich **Compare with published** (Mit veröffentlichtem Element vergleichen) aus. Verwenden Sie im Testbereich für die Veröffentlichung die Option **JSON-Ansicht anzeigen**, um dieselbe JSON wie im vorherigen Schritt anzuzeigen. Auf diese Weise können Sie die aktuelle App, die Sie bearbeiten, mit der App vergleichen, die am Endpunkt veröffentlicht wird.

[![Aktuell bearbeitete Version mit der veröffentlichten Version der App vergleichen](./media/get-started-portal-deploy-app/compare-test-pane.png)](./media/get-started-portal-deploy-app/compare-test-pane.png#lightbox)




## <a name="clean-up-resources"></a>Bereinigen von Ressourcen

Wählen Sie nach Abschluss dieses Schnellstarts im oberen Navigationsmenü die Option **Meine Apps** aus. Aktivieren Sie in der Liste das Kontrollkästchen der App, und wählen Sie auf der Kontextsymbolleiste oberhalb der Liste **Löschen** aus.

[![App aus der Liste „Meine Apps“ löschen](./media/get-started-portal-build-app/delete-app.png)](./media/get-started-portal-build-app/delete-app.png#lightbox)

## <a name="next-steps"></a>Nächste Schritte

> [!div class="nextstepaction"]
> [Identifizieren häufiger Absichten und Entitäten](luis-tutorial-prebuilt-intents-entities.md)
