---
title: 'Schnellstart: Erstellen, Trainieren und Veröffentlichen einer Wissensdatenbank: QnA Maker'
titleSuffix: Azure Cognitive Services
description: Sie können eine QnA Maker-Wissensdatenbank (KB) aus Ihren eigenen Inhalten erstellen, z.B. aus FAQs oder Produkthandbüchern. Die QnA Maker-Wissensdatenbank in diesem Beispiel wird auf der Grundlage einer einfachen FAQ-Webseite erstellt, um Fragen zur BitLocker-Schlüsselwiederherstellung zu beantworten.
author: diberry
manager: nitinme
services: cognitive-services
ms.service: cognitive-services
ms.subservice: qna-maker
ms.topic: quickstart
ms.date: 10/04/2019
ms.author: diberry
ms.openlocfilehash: aab64822730531acdcf5f3d91ed8bf028ce7cfd4
ms.sourcegitcommit: c2e7595a2966e84dc10afb9a22b74400c4b500ed
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 10/05/2019
ms.locfileid: "71971917"
---
# <a name="quickstart-create-train-and-publish-your-qna-maker-knowledge-base"></a>Schnellstart: Erstellen, Trainieren und Veröffentlichen der QnA Maker-Wissensdatenbank

Sie können eine QnA Maker-Wissensdatenbank (KB) aus Ihren eigenen Inhalten erstellen, z.B. aus FAQs oder Produkthandbüchern. Dieser Artikel enthält ein Beispiel zum Erstellen einer QnA Maker-Wissensdatenbank auf der Grundlage einer einfachen FAQ-Webseite erstellt, um Fragen zur BitLocker-Schlüsselwiederherstellung zu beantworten.

Fügen Sie eine Persönlichkeit für Geplauder ein, um Ihre Informationen für Ihre Benutzer ansprechender zu machen.

[!INCLUDE [Custom subdomains notice](../../../../includes/cognitive-services-custom-subdomains-note.md)]

## <a name="prerequisite"></a>Voraussetzung

> [!div class="checklist"]
> * Wenn Sie kein Azure-Abonnement besitzen, können Sie ein [kostenloses Konto](https://azure.microsoft.com/free/?WT.mc_id=A261C142F) erstellen, bevor Sie beginnen.

## <a name="create-a-new-qna-maker-knowledge-base"></a>Erstellen einer neuen QnA Maker-Wissensdatenbank

1. Melden Sie sich mit Ihren Azure-Anmeldeinformationen beim Portal [QnAMaker.ai](https://QnAMaker.ai) an.

1. Wählen Sie im QnA Maker-Portal die Option **Wissensdatenbank erstellen** aus.

1. Wählen Sie auf der Seite **Erstellen** die Option **QnA-Dienst erstellen** aus. Sie werden an das [Azure-Portal](https://ms.portal.azure.com/#create/Microsoft.CognitiveServicesQnAMaker) weitergeleitet, um einen QnA Maker-Dienst in Ihrem Abonnement einzurichten. 

1. Wählen Sie im QnA Maker-Portal den QnA Maker-Dienst aus den Dropdownlisten aus. Wenn Sie einen neuen QnA Maker-Dienst erstellt haben, achten Sie darauf, dass Sie die Seite aktualisieren.

   ![Screenshot: Auswählen einer Wissensdatenbank im QnA Maker-Dienst](../media/qnamaker-quickstart-kb/qnaservice-selection.png)

1. Nennen Sie Ihre Wissensdatenbank **My Sample QnA KB**.

1. Fügen Sie ein Word-Beispieldokument als URL hinzu: 

    `https://docs.microsoft.com/en-us/azure/cognitive-services/qnamaker/troubleshooting`

1. Wählen Sie `+ Add URL` aus.

1. Fügen Sie Ihrer Wissensdatenbank **_professionelles_ Geplauder** hinzu. 

1. Wählen Sie **Wissensdatenbank erstellen** aus.

    Der Extraktionsvorgang nimmt einige Minuten in Anspruch, um das Dokument zu lesen sowie Fragen und Antworten zu identifizieren.

    Nachdem die Wissensdatenbank von QnA Maker erfolgreich erstellt wurde, wird die Seite **Wissensdatenbank** geöffnet. Auf dieser Seite können Sie den Inhalt der Wissensdatenbank bearbeiten.

## <a name="add-a-new-question-and-answer-set"></a>Hinzufügen eines neuen Frage-Antwort-Satzes

1. Wählen Sie im QnA Maker-Portal auf der Seite **Bearbeiten** die Option **Add QnA pair** (Frage- und Antwortpaar hinzufügen) aus.
1. Fügen Sie die folgende Frage hinzu: 

    `How many Azure services are used by a knowledge base?`

1. Fügen Sie die mit _Markdown_ formatierte Antwort hinzu:

    ` * Azure QnA Maker service\n* Azure Search\n* Azure web app\n* Azure app plan`

    ![ Fügen Sie die Frage als Text und die mit Markdown formatierte Antwort hinzu.](../media/qnamaker-create-publish-knowledge-base/add-question-and-answer.png)

    Das Markdownsymbol `*` wird für Aufzählungszeichen verwendet. `\n` wird für eine neue Zeile verwendet.  

    Auf der Seite **Bearbeiten** wird der Markdowncode angezeigt. Wenn Sie später das Panel **Testen** verwenden, wird der Markdowncode richtig angezeigt. 

## <a name="save-and-train"></a>Speichern und trainieren

Wählen Sie in der oberen rechten Ecke **Speichern und trainieren** aus, um Ihre Änderungen zu speichern und das QnA Maker-Modell zu trainieren. Änderungen werden nur beibehalten, wenn sie gespeichert werden.

## <a name="test-the-knowledge-base"></a>Testen der Wissensdatenbank

1. Wählen Sie rechts oben im QnA Maker-Portal die Option **Testen** aus, um zu testen, ob die von Ihnen vorgenommenen Änderungen übernommen wurden. 
1. Geben Sie eine Beispielbenutzerabfrage in das Textfeld ein. 

    `How many Azure services are used by a knowledge base?`  

    ![ Geben Sie eine Beispielbenutzerabfrage in das Textfeld ein. ](../media/qnamaker-create-publish-knowledge-base/test-panel-in-qna-maker.png)

1. Wählen Sie **Untersuchen** aus, um die Antwort ausführlicher zu untersuchen. Das Testfenster wird verwendet, um die Änderungen an der Wissensdatenbank zu testen, bevor die Wissensdatenbank veröffentlicht wird.

1. Wählen Sie erneut **Testen** aus, um das Panel **Testen** zu schließen.

## <a name="publish-the-knowledge-base"></a>Veröffentlichen der Knowledge Base

Wenn Sie eine Wissensdatenbank veröffentlichen, werden die Inhalte Ihrer Wissensdatenbank aus dem Index `test` in den Index `prod` in Azure Search verschoben.

![Screenshot: Verschieben der Inhalte Ihrer Wissensdatenbank](../media/qnamaker-how-to-publish-kb/publish-prod-test.png)

1. Wählen Sie im QnA Maker-Portal **Veröffentlichen** aus. Wählen Sie dann zum Bestätigen **Veröffentlichen** auf der Seite aus.

    Die QnA Maker-Dienst wurde nun erfolgreich veröffentlicht. Sie können den Endpunkt in Ihrer Anwendung oder im Botcode verwenden.

    ![Screenshot: Erfolgreiche Veröffentlichung](../media/qnamaker-create-publish-knowledge-base/publish-knowledge-base-to-endpoint.png)

## <a name="create-a-bot"></a>Erstellen eines Bots

Nach der Veröffentlichung können Sie auf der Seite **Veröffentlichen** einen Bot erstellen: 

* Sie können schnell mehrere Bots erstellen, die alle auf die gleiche Wissensdatenbank verweisen, und dabei verschiedene Regionen oder Tarife für die einzelnen Bots verwenden. 
* Wenn Sie nur einen einzelnen Bot für die Wissensdatenbank erstellen möchten, verwenden Sie den Link zum **Anzeigen Ihrer Bots im Azure-Portal**, um eine Liste mit Ihren derzeitigen Bots anzuzeigen. 

Wenn Sie Änderungen an der Wissensdatenbank vornehmen und sie erneut veröffentlichen, müssen Sie keine weiteren Schritte für den Bot ausführen. Er ist bereits für die Verwendung mit der Wissensdatenbank konfiguriert und funktioniert auch nach späteren Änderungen. Nach jeder Veröffentlichung einer Wissensdatenbank werden alle mit ihr verbundenen Bots automatisch aktualisiert.

1. Wählen Sie im QnA Maker-Portal auf der Seite **Veröffentlichen** die Option **Bot erstellen** aus. Diese Schaltfläche wird erst angezeigt, wenn die Wissensdatenbank veröffentlicht wurde.

    ![Screenshot zum Erstellen eines Bots](../media/qnamaker-create-publish-knowledge-base/create-bot-from-published-knowledge-base-page.png)

1. Für das Azure-Portal wird eine neue Browserregisterkarte mit der Erstellungsseite von Azure Bot Service geöffnet. Konfigurieren Sie Azure Bot Service. 
    
    * Behalten Sie beim Erstellen des Bots die folgenden Einstellungen im Azure-Portal bei. Sie wurden automatisch für Ihre vorhandene Wissensdatenbank aufgefüllt: 
        * QnA-Authentifizierungsschlüssel
        * App Service-Plan und Standort
    * Der Bot und QnA Maker können den gleichen Web-App-Serviceplan, aber nicht die gleiche Web-App verwenden. Der **App-Name** für den Bot muss sich daher von dem App-Namen für den QnA Maker-Dienst unterscheiden. 

1. Öffnen Sie nach der Erstellung des Bots die **Bot Service**-Ressource. 
1. Wählen Sie unter **Botverwaltung** die Option **Testen im Webchat** aus.
1. Geben Sie bei der Chataufforderung **Nachricht eingeben** Folgendes ein:

    `Azure services?`

    Der Chatbot antwortet mit einer Antwort aus Ihrer Wissensdatenbank. 

    ![Geben Sie eine Benutzerabfrage in den Testwebchat ein.](../media/qnamaker-create-publish-knowledge-base/test-web-chat.png)

## <a name="clean-up-resources"></a>Bereinigen von Ressourcen

Bereinigen Sie die QnA Maker- und Bot Framework-Ressourcen im Azure-Portal. 

## <a name="next-steps"></a>Nächste Schritte

Weitere Informationen finden Sie unter:

* [Markdownformat in Antworten](../concepts/data-sources-supported.md)
* [Testen Ihres Markdowncodes](../concepts/data-sources-supported.md#testing-your-markdown)
* [Datenquellen](../Concepts/data-sources-supported.md) für QnA Maker-Inhalt 
* [Tutorial: Erstellen eines QnA-Bots mit Azure Bot Service v4](../tutorials/create-qna-bot.md)

> [!div class="nextstepaction"]
> [Hinzufügen von Fragen mit Metadaten](add-question-metadata-portal.md)

