---
title: QnA-Bot – Azure Bot Service – QnA Maker
titleSuffix: Azure Cognitive Services
description: Erstellen Sie einen QnA-Chatbot über die Seite „Veröffentlichen“ für eine vorhandene Wissensdatenbank. Dieser Bot verwendet das Bot Framework SDK v4. Sie müssen keinen Code zum Erstellen des Bots schreiben; der gesamte Code wird automatisch bereitgestellt.
services: cognitive-services
author: diberry
manager: nitinme
ms.custom: seodec18
ms.service: cognitive-services
ms.subservice: qna-maker
ms.topic: tutorial
ms.date: 06/24/2019
ms.author: diberry
ms.openlocfilehash: 1d475d988847bd4cc9e37b91a0c5d28678cb05f2
ms.sourcegitcommit: f56b267b11f23ac8f6284bb662b38c7a8336e99b
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 06/28/2019
ms.locfileid: "67446653"
---
# <a name="tutorial-create-a-qna-bot-with-azure-bot-service-v4"></a>Tutorial: Erstellen eines QnA-Bots mit Azure Bot Service v4

Erstellen Sie einen QnA-Chatbot über die Seite **Veröffentlichen** für eine vorhandene Wissensdatenbank. Dieser Bot verwendet das Bot Framework SDK v4. Sie müssen keinen Code zum Erstellen des Bots schreiben; der gesamte Code wird automatisch bereitgestellt.

**In diesem Tutorial lernen Sie Folgendes:**

<!-- green checkmark -->
> [!div class="checklist"]
> * Erstellen eines Azure Bot Service aus einer vorhandenen Wissensdatenbank
> * Chatten mit dem Bot, um die richtige Funktionsweise des Codes zu überprüfen 

## <a name="prerequisites"></a>Voraussetzungen

Für dieses Tutorial benötigen Sie eine veröffentlichte Wissensdatenbank. Wenn Sie noch keine haben, führen Sie die Schritte im Tutorial [Erstellen und Beantworten über die Wissensdatenbank](create-publish-query-in-portal.md) aus, um eine QnA Maker-Wissensdatenbank mit Fragen und Antworten zu erstellen.

<a name="create-a-knowledge-base-bot"></a>

## <a name="create-a-qna-bot"></a>Erstellen eines QnA-Bots

Erstellen Sie einen Bot als Clientanwendung für die Wissensdatenbank. 

1. Navigieren Sie im QnA Maker-Portal zur Seite **Veröffentlichen**, und veröffentlichen Sie Ihre Wissensdatenbank. Wählen Sie **Bot erstellen** aus. 

    ![Navigieren Sie im QnA Maker-Portal zur Seite „Veröffentlichen“, und veröffentlichen Sie Ihre Wissensdatenbank. Wählen Sie „Bot erstellen“ aus.](../media/qnamaker-tutorials-create-bot/create-bot-from-published-knowledge-base-page.png)

    Das Azure-Portal wird mit der Konfiguration zur Bot-Erstellung geöffnet.

1.  Geben Sie die Einstellungen zum Erstellen des Bots ein:

    |Einstellung|Wert|Zweck|
    |--|--|--|
    |Botname|`my-tutorial-kb-bot`|Dies ist der Name der Azure-Ressource für den Bot.|
    |Abonnement|Siehe „Zweck“.|Wählen Sie das Abonnement aus, mit dem Sie auch die QnA Maker-Ressourcen erstellt haben.|
    |Ressourcengruppe|`my-tutorial-rg`|Die Ressourcengruppe, die für alle Bot-bezogenen Azure-Ressourcen verwendet wird.|
    |Location|`west us`|Der Azure-Ressourcenspeicherort für den Bot.|
    |Tarif|`F0`|Der kostenlose Tarif für den Azure-Botdienst.|
    |App-Name|`my-tutorial-kb-bot-app`|Dies ist eine Web-App nur zur Unterstützung Ihres Bots. Dies sollte nicht derselbe App-Name wie derjenige sein, den Ihr QnA Maker-Dienst bereits verwendet. Die Freigabe der QnA Maker-Web-App bei einer anderen Ressource wird nicht unterstützt.|
    |SDK-Sprache|C#|Dies ist die zugrunde liegende Programmiersprache, die vom Bot-Framework-SDK verwendet wird. Die Optionen sind [C#](https://github.com/Microsoft/botbuilder-dotnet) oder [Node.js](https://github.com/Microsoft/botbuilder-js).|
    |QnA-Authentifizierungsschlüssel|**Nicht ändern**|Der Wert wird automatisch ausgefüllt.|
    |App Service-Plan/Standort|**Nicht ändern**|Bei diesem Tutorial ist der Standort unwichtig.|
    |Azure Storage|**Nicht ändern**|Unterhaltungsdaten werden in Azure Storage-Tabellen gespeichert.|
    |Application Insights|**Nicht ändern**|Die Protokollierung wird an Application Insights gesendet.|
    |Microsoft-App-ID|**Nicht ändern**|Active Directory-Benutzer und Kennwort sind erforderlich.|

    ![Erstellen Sie den Bot für die Wissensdatenbank mit diesen Einstellungen.](../media/qnamaker-tutorials-create-bot/create-bot-from-published-knowledge-base.png)

    Warten Sie einige Minuten, bis eine Erfolgsmeldung zum Boterstellungsvorgang angezeigt wird.

<a name="test-the-bot"></a>

## <a name="chat-with-the-bot"></a>Chatten mit dem Bot

1. Öffnen Sie im Azure-Portal die neue Bot-Ressource direkt aus dieser Benachrichtigung. 

    ![Öffnen Sie im Azure-Portal die neue Bot-Ressource direkt aus dieser Benachrichtigung.](../media/qnamaker-tutorials-create-bot/azure-portal-notifications.png)

1. Wählen Sie unter **Botverwaltung** die Option **Testen im Webchat** aus, und geben Sie Folgendes ein: `How large can my KB be?`. Der Bot antwortet mit: 


    `The size of the knowledge base depends on the SKU of Azure search you choose when creating the QnA Maker service. Read [here](https://docs.microsoft.com/azure/cognitive-services/qnamaker/tutorials/choosing-capacity-qnamaker-deployment)for more details.`


    ![„Testen Sie den neuen Bot für die Wissensdatenbank.“](../media/qnamaker-tutorial-create-publish-query-in-portal/test-bot-in-web-chat-in-azure-portal.png)

    Weitere Informationen zu Azure-Bots finden Sie unter [Beantworten von Fragen mit QnA Maker](https://docs.microsoft.com/azure/bot-service/bot-builder-howto-qna?view=azure-bot-service-4.0&tabs=cs).

## <a name="clean-up-resources"></a>Bereinigen von Ressourcen

Wenn Sie den Bot dieses Tutorials nicht mehr benötigen, entfernen Sie ihn im Azure-Portal. 

Wenn Sie eine neue Ressourcengruppe für die Ressourcen des Bots erstellt haben, löschen Sie die Ressourcengruppe. 

Wenn Sie keine neue Ressourcengruppe erstellt haben, müssen Sie die dem Bot zugeordneten Ressourcen suchen. Am einfachsten ist es, nach dem Namen des Bots und der Bot-App zu suchen. Zu den Bot-Ressourcen gehört Folgendes:

* Der App Service-Plan
* Der Suchdienst
* Der Cognitive Service
* Der App-Dienst
* Optional sind darüber hinaus der Application Insights-Dienst und -Speicher für die Application Insights-Daten enthalten.


## <a name="related-to-qna-maker-bots"></a>In Bezug auf QnA Maker-Bots

* QnA Maker-Hilfebot, der im QnA Maker-Portal verwendet wird, ist als [Bot-Beispel](https://github.com/Microsoft/BotBuilder-Samples/tree/master/experimental/csharp_dotnetcore/qnamaker-support-bot) verfügbar.
    ![QnA Maker-Hilfebotsymbol ist ein roter Roboter](../media/qnamaker-tutorials-create-bot/answer-bot-icon.PNG)
* [Bots für das Gesundheitswesen](https://docs.microsoft.com/HealthBot/qna_model_howto) verwenden QnA Maker als eines ihrer [Sprachmodelle](https://docs.microsoft.com/HealthBot/qna_model_howto).


[!INCLUDE [Bot Information](../../../../includes/cognitive-services-qnamaker-luis-bot-info.md)]

## <a name="next-steps"></a>Nächste Schritte

> [!div class="nextstepaction"]
> [Konzept: Wissensdatenbank](../concepts/knowledge-base.md)

## <a name="see-also"></a>Weitere Informationen

- [Verwalten Ihrer Knowledge Base](https://qnamaker.ai)
- [Aktivieren Ihres Bots in verschiedenen Kanälen](https://docs.microsoft.com/azure/bot-service/bot-service-manage-channels)
