---
title: 'Schnellstart: Abrufen von Antworten aus einer Wissensdatenbank mit Postman – QnA Maker'
titleSuffix: Azure Cognitive Services
description: In diesem Schnellstart wird Schritt für Schritt erläutert, wie Sie mit Postman eine Antwort aus einer Wissensdatenbank abrufen.
services: cognitive-services
author: diberry
manager: nitinme
ms.service: cognitive-services
ms.subservice: qna-maker
ms.topic: quickstart
ms.date: 07/16/2019
ms.author: diberry
ms.openlocfilehash: 7c419ea7cb5c9ae1d1cc419798961ec67f250160
ms.sourcegitcommit: 7c4de3e22b8e9d71c579f31cbfcea9f22d43721a
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 07/26/2019
ms.locfileid: "68563008"
---
# <a name="quickstart-get-an-answer-from-knowledge-base-using-postman"></a>Schnellstart: Abrufen einer Antwort aus einer Wissensdatenbank mit Postman

In diesem Schnellstart auf Postman-Basis wird Schritt für Schritt erläutert, wie Sie eine Antwort aus einer Wissensdatenbank abrufen.

## <a name="prerequisites"></a>Voraussetzungen

* Neueste Version von [**Postman**](https://www.getpostman.com/).
* Sie benötigen einen [QnA Maker-Dienst](../How-To/set-up-qnamaker-service-azure.md) und eine [Wissensdatenbank mit Fragen und Antworten](../Tutorials/create-publish-query-in-portal.md). 

## <a name="publish-to-get-endpoint"></a>Veröffentlichen zum Abrufen des Endpunkts

Wenn Sie bereit sind, eine Antwort auf eine Frage aus Ihrer Wissensdatenbank zu generieren, [veröffentlichen](../Quickstarts/create-publish-knowledge-base.md#publish-the-knowledge-base) Sie Ihre Wissensdatenbank.

## <a name="use-production-endpoint-with-postman"></a>Verwenden des Produktionsendpunkts mit Postman

Wenn Ihre Wissensdatenbank veröffentlicht wird, werden auf der Seite **Veröffentlichen** die HTTP-Anforderungseinstellungen zum Generieren einer Antwort angezeigt. Die Standardansicht zeigt die erforderlichen Einstellungen zum Generieren einer Antwort von [Postman](https://www.getpostman.com) an.

Die gelben Zahlen in der folgenden Abbildung kennzeichnen, welche Name/Wert-Paare in den folgenden Schritten verwendet werden sollen.

[![Veröffentlichen der Ergebnisse](../media/qnamaker-quickstart-get-answer-with-postman/publish-settings.png)](../media/qnamaker-quickstart-get-answer-with-postman/publish-settings.png#lightbox)

Um eine Antwort mit Postman zu generieren, führen Sie die folgenden Schritte aus:

1. Öffnen Sie Postman. Wenn Sie aufgefordert werden, einen Baustein auszuwählen, wählen Sie den Baustein **Basic Request** aus. Legen Sie für den **Request name (Anforderungsnamen)** `Generate QnA Maker answer`und für die **Collection (Sammlung)** `Generate QnA Maker answers` fest. Wenn Sie nicht in einer Sammlung speichern möchten, wählen Sie die Schaltfläche **Abbrechen** aus.
1. Wählen Sie im Arbeitsbereich die HTTP-Methode von **POST** aus.

    [![In Postman die POST-Methode festlegen](../media/qnamaker-quickstart-get-answer-with-postman/postman-select-post-method.png)](../media/qnamaker-quickstart-get-answer-with-postman/postman-select-post-method.png#lightbox)

1. Verketten Sie für die URL den Host-Wert (Nr. 2 in der Abbildung) und den POST-Wert (Nr. 1 in der Abbildung), um die vollständige URL zu erstellen. Eine vollständige Beispiel-URL sieht so aus: 

    `https://qnamaker-f0.azurewebsites.net/qnamaker/knowledgebases/e1115f8c-d01b-4698-a2ed-85b0dbf3348c/generateAnswer`

    [![Die vollständige URL in Postman festlegen](../media/qnamaker-quickstart-get-answer-with-postman/set-postman-method-and-url.png)](../media/qnamaker-quickstart-get-answer-with-postman/set-postman-method-and-url.png#lightbox)

1. Wählen Sie die Registerkarte **Headers (Kopfzeilen)** unter der URL und dann **Bulk Edit (Massenbearbeitung)** aus. 

1. Kopieren Sie die Header (Nr. 3 und Nr. 4 aus der Abbildung) in den Textbereich.

    [![Festlegen der Kopfzeilen in Postman](../media/qnamaker-quickstart-get-answer-with-postman/set-postman-headers.png)](../media/qnamaker-quickstart-get-answer-with-postman/set-postman-headers.png#lightbox)

1. Wählen Sie die Registerkarte **Body (Hauptteil)** aus.
1. Wählen Sie das Format **raw (unformatiert)** aus, und geben Sie den JSON-Code (Nr. 5 aus der Abbildung) ein, der die Frage darstellt.

    `{"question":"How do I programmatically update my Knowledge Base?"}`

    [![Festlegen des JSON-Werts für den Hauptteil in Postman](../media/qnamaker-quickstart-get-answer-with-postman/set-postman-body-json-value.png)](../media/qnamaker-quickstart-get-answer-with-postman/set-postman-body-json-value.png#lightbox)

1. Wählen Sie die Schaltfläche **Send (Senden)** aus.
1. Die Rückgabe enthält neben der Antwort andere Informationen, die für die Clientanwendung wichtig sein können. 

    [![Festlegen des JSON-Werts für den Hauptteil in Postman](../media/qnamaker-quickstart-get-answer-with-postman/receive-postman-response.png)](../media/qnamaker-quickstart-get-answer-with-postman/receive-postman-response.png#lightbox)

## <a name="use-staging-endpoint"></a>Verwenden des Stagingendpunkts

Wenn Sie eine Antwort vom Stagingendpunkt erhalten möchten, fügen Sie die `isTest`-Texteigenschaft an die URL an.

## <a name="next-steps"></a>Nächste Schritte

Die Veröffentlichungsseite enthält auch Informationen zum [Generieren einer Antwort](get-answer-from-kb-using-curl.md) mit cURL. 

> [!div class="nextstepaction"]
> [Verwenden von Metadaten beim Generieren einer Antwort](../How-to/metadata-generateanswer-usage.md)
