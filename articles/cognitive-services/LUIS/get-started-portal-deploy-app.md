---
title: 'Schnellstart: Bereitstellen einer App im LUIS-Portal'
titleSuffix: Azure Cognitive Services
description: In dieser Schnellstartanleitung erfahren Sie, wie Sie eine App bereitstellen, indem Sie eine Vorhersageendpunktressource erstellen, die Ressource zuweisen, ein Training durchführen und die App veröffentlichen.
services: cognitive-services
author: diberry
manager: nitinme
ms.service: cognitive-services
ms.subservice: language-understanding
ms.topic: quickstart
ms.date: 11/04/2019
ms.author: diberry
ms.openlocfilehash: 279ec4e8a6d9a9d473cc511b4ec690391cdbd634
ms.sourcegitcommit: 609d4bdb0467fd0af40e14a86eb40b9d03669ea1
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 11/06/2019
ms.locfileid: "73669418"
---
# <a name="quickstart-deploy-an-app-in-the-luis-portal"></a>Schnellstart: Bereitstellen einer App im LUIS-Portal

[!INCLUDE [Uses preview portal](./includes/uses-portal-preview.md)]

Wenn Ihre LUIS-App bereit ist, Äußerungsvorhersagen an eine Clientanwendung (beispielsweise einen Chatbot) zurückzugeben, müssen Sie die App für den Vorhersageendpunkt bereitstellen.

In diesem Schnellstart erfahren Sie, wie eine Anwendung bereitgestellt wird. Sie erstellen eine Vorhersageendpunktressource, weisen die Ressource der App zu, trainieren die App und veröffentlichen sie.

## <a name="prerequisites"></a>Voraussetzungen

* Erwerben Sie ein [Azure-Abonnement](https://azure.microsoft.com/free).
* Führen Sie den vorherigen [Schnellstart: Erstellen einer neuen App im LUIS-Portal](get-started-portal-build-app.md) durch, oder [laden Sie die App herunter, und importieren Sie die App](https://github.com/Azure-Samples/cognitive-services-language-understanding/blob/master/documentation-samples/quickstarts/in-portal/build-portal-app.json).

## <a name="create-the-endpoint-resource"></a>Erstellen der Endpunktressource

Sie erstellen die Vorhersageendpunktressource im Azure-Portal. Diese Ressource darf nur für Endpunktvorhersageabfragen verwendet werden. Verwenden Sie diese Ressource nicht für das Erstellen von Änderungen an der App.

1. Melden Sie sich beim [Azure-Portal](https://ms.portal.azure.com/#create/Microsoft.CognitiveServicesLUISAllInOne) an, und erstellen Sie eine Ressource.

1. Konfigurieren Sie das Abonnement mit den folgenden Einstellungen:

   |Einstellung|Wert|Zweck|
   |--|--|--|
   |NAME|`my-luis-resource`|Der Name der Azure-Ressource. Sie benötigen diesen Namen, wenn Sie der App im LUIS-Portal die Ressource zuweisen.|
   |Subscription|Ihr Abonnement|Wählen Sie eines der mit Ihrem Konto verknüpften Abonnements aus.|
   |Resource group|`my-resource-group`|Erstellen Sie eine neue Ressourcengruppe für alle Ihre Cognitive Services-Ressourcen. Wenn Sie mit den Ressourcen fertig sind, können Sie die Ressourcengruppe löschen, um Ihr Abonnement zu bereinigen. |
   |Authoring location (Erstellungsstandort)|**USA, Westen**|Die Azure-Region für die Erstellung.|
   |Authoring pricing tier (Erstellungstarif)|**F0**|Der Standardtarif für die Erstellung.|
   |Runtime location (Laufzeitstandort)|**USA, Westen**|Die Azure-Region für Vorhersageendpunktabfragen.|
   |Runtime pricing tier (Laufzeittarif)|**S0**|Dieser Tarif ist für Websites mit hohem Datenverkehrsaufkommen geeignet.|
   | | | |


   ![Azure-API-Auswahl](./media/luis-how-to-azure-subscription/create-resource-in-azure.png)

1. Wählen Sie **Erstellen** aus, um die Azure-Ressource zu erstellen.

   Im nächsten Abschnitt wird erläutert, wie Sie diese neue Ressource mit einer LUIS-App im LUIS-Portal verbinden.

## <a name="assign-the-resource-key-to-the-luis-app-in-the-luis-portal"></a>Zuweisen des Ressourcenschlüssels für die LUIS-App im LUIS-Portal

Jedes Mal, wenn Sie eine neue Ressource für LUIS erstellen, müssen Sie der LUIS-App die Ressource zuweisen. Nachdem Sie die Ressource zugewiesen haben, müssen Sie diesen Schritt nur dann erneut ausführen, wenn Sie eine neue Ressource erstellen. Sie können beispielsweise eine neue Ressource erstellen, um die Regionen Ihrer App zu erweitern oder eine größere Anzahl von Vorhersageabfragen zu unterstützen.

1. Melden Sie sich beim [LUIS-Portal](https://www.luis.ai) an, und wählen Sie in der Liste der Apps die App **myEnglishApp** aus.

1. Wählen Sie im Menü oben rechts die Option **Verwalten** aus, und wählen Sie dann **Azure-Ressourcen** aus.

1. Um LUIS hinzuzufügen, wählen Sie **Add prediction resource** (Vorhersageressource hinzufügen) aus.

    ![Hinzufügen der LUIS-Vorhersageressource durch Auswählen von „Vorhersageressource hinzufügen“](./media/get-started-portal-deploy-app/azure-resources-add-prediction-resource.png)

1. Wählen Sie Ihren Mandanten, Ihr Abonnement und den Namen Ihrer Ressource aus. Wählen Sie **Ressource zuweisen** aus.

   ![Zuweisen einer Ressource zu Ihrer App](./media/get-started-portal-deploy-app/assign-resource.png)

1. Führen Sie die gleichen Schritte aus, um Ihrer App den Erstellungsschlüssel hinzuzufügen.

1. Suchen Sie die neue Zeile in der Tabelle für die neue Vorhersageressource, und kopieren Sie die Endpunkt-URL. Sie ist ordnungsgemäß aufgebaut, um eine `HTTP GET`-Anforderung an den LUIS-API-Endpunkt zum Abrufen einer Vorhersage zu senden.

## <a name="train-the-app"></a>Trainieren der App

[!INCLUDE [LUIS How to Train steps](includes/howto-train.md)]

## <a name="publish-the-app-to-the-prediction-endpoint"></a>Veröffentlichen der App für den Vorhersageendpunkt

[!INCLUDE [LUIS How to Train steps](includes/howto-publish.md)]

## <a name="prediction-endpoint-request"></a>Anforderung für Vorhersageendpunkt

Die Äußerung des Benutzers wird am Ende der URL bei `query=` an die GET-Anforderung angefügt. Geben Sie nach `query=` die gleiche Benutzeräußerung ein, die am Ende des vorherigen Schnellstarts verwendet wurde:

```Is there a form named hrf-234098```

Stellen Sie sicher, dass die Abfragezeichenfolge folgende Paare enthält:

* `show-all-intents=true`
* `verbose=true`

Der Browser zeigt die folgende Antwort an:

```JSON
{
    "query": "Is there a form named hrf-234098",
    "prediction": {
        "topIntent": "FindForm",
        "intents": {
            "FindForm": {
                "score": 0.9768753
            },
            "None": {
                "score": 0.0216071177
            }
        },
        "entities": {
            "Human Resources Form Number": [
                "hrf-234098"
            ],
            "$instance": {
                "Human Resources Form Number": [
                    {
                        "type": "Human Resources Form Number",
                        "text": "hrf-234098",
                        "startIndex": 22,
                        "length": 10,
                        "modelTypeId": 8,
                        "modelType": "Regex Entity Extractor",
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

Um im Testbereich die gleiche Informationsmenge anzuzeigen, müssen Sie die App veröffentlichen. Wählen Sie nach dem Veröffentlichen der App im Testbereich **Compare with published** (Mit veröffentlichtem Element vergleichen) aus. Verwenden Sie im Testbereich für die Veröffentlichung die Option **JSON-Ansicht anzeigen**, um dieselbe JSON wie im vorherigen Schritt anzuzeigen. Auf diese Weise können Sie Änderungen an der aktuell bearbeiteten App mit der App vergleichen, die für den Endpunkt veröffentlicht wird.

[![Aktuell bearbeitete Version mit der veröffentlichten Version der App vergleichen](./media/get-started-portal-deploy-app/compare-test-pane.png)](./media/get-started-portal-deploy-app/compare-test-pane.png#lightbox)

## <a name="clean-up-resources"></a>Bereinigen von Ressourcen

Wählen Sie nach Abschluss dieses Schnellstarts im oberen Navigationsmenü die Option **Meine Apps** aus. Aktivieren Sie in der Liste das Kontrollkästchen der App, und wählen Sie auf der Kontextsymbolleiste oberhalb der Liste **Löschen** aus.

## <a name="next-steps"></a>Nächste Schritte

> [!div class="nextstepaction"]
> [Identifizieren häufiger Absichten und Entitäten](luis-tutorial-prebuilt-intents-entities.md)
