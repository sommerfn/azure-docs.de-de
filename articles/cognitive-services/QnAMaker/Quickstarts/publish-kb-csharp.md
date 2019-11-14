---
title: 'Schnellstart: Veröffentlichen einer Wissensdatenbank, REST, C# – QnA Maker'
titleSuffix: Azure Cognitive Services
description: Diese auf C# REST basierende Schnellstartanleitung veröffentlicht Ihre Wissensdatenbank und erstellt einen Endpunkt, der in Ihrer Anwendung oder Ihrem Chatbot aufgerufen werden kann.
services: cognitive-services
author: diberry
manager: nitinme
ms.custom: seodec18
ms.service: cognitive-services
ms.subservice: qna-maker
ms.topic: quickstart
ms.date: 10/01/2019
ms.author: diberry
ms.openlocfilehash: 192f0ced2b718deca123f3f72dcf549f6a48f59f
ms.sourcegitcommit: 018e3b40e212915ed7a77258ac2a8e3a660aaef8
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 11/07/2019
ms.locfileid: "73794113"
---
# <a name="quickstart-publish-a-knowledge-base-in-qna-maker-using-c"></a>Schnellstart: Veröffentlichen einer Wissensdatenbank in QnA Maker mit C#

In diesem REST-basierten Schnellstart wird das programmgesteuerte Veröffentlichen Ihrer Wissensdatenbank (Knowledge Base, KB) Schritt für Schritt beschrieben. Bei der Veröffentlichung wird die aktuelle Version der Wissensdatenbank per Pushvorgang an einen dedizierten Azure Cognitive Search-Index übertragen und ein Endpunkt erstellt, der in Ihrer Anwendung oder Ihrem Chatbot aufgerufen werden kann.

In dieser Schnellstartanleitung werden QnA Maker-APIs aufgerufen:
* [Veröffentlichen](https://docs.microsoft.com/rest/api/cognitiveservices/qnamaker/knowledgebase/publish): Für diese API sind keine Informationen im Text der Anforderung erforderlich.

## <a name="prerequisites"></a>Voraussetzungen

* Aktuelle [**Visual Studio Community Edition**](https://www.visualstudio.com/downloads/)
* Sie benötigen einen [QnA Maker-Dienst](../How-To/set-up-qnamaker-service-azure.md). Wählen Sie für Ihre Ressource im Azure-Portal die Option **Schnellstart** aus, um den Schlüssel und den Endpunkt (der den Ressourcennamen enthält) abzurufen.
* ID der QnA Maker-Wissensdatenbank, die in der URL des Abfragezeichenfolgenparameters „kbid“ enthalten ist, wie nachfolgend gezeigt:

    ![ID der QnA Maker-Wissensdatenbank](../media/qnamaker-quickstart-kb/qna-maker-id.png)

    Falls Sie noch keine Wissensdatenbank besitzen, können Sie für diese Schnellstartanleitung eine Beispieldatenbank erstellen: [Erstellen einer neuen Wissensdatenbank](create-new-kb-csharp.md).

> [!NOTE] 
> Die vollständigen Projektmappendateien sind über das [GitHub-Repository **Azure-Samples/cognitive-services-qnamaker-csharp**](https://github.com/Azure-Samples/cognitive-services-qnamaker-csharp/tree/master/documentation-samples/quickstarts/publish-knowledge-base) verfügbar.

## <a name="create-knowledge-base-project"></a>Erstellen eines Wissensdatenbank-Projekts

1. Öffnen Sie Visual Studio 2019 Community Edition.
1. Erstellen Sie ein neues Projekt vom Typ **Konsolen-App (.NET Core)** , und nennen Sie es `QnaMakerQuickstart`. Übernehmen Sie für die übrigen Einstellungen die Standardwerte.

## <a name="add-required-dependencies"></a>Hinzufügen von erforderlichen Abhängigkeiten

Ersetzen Sie am Anfang von „Program.cs“ die einzelne using-Anweisung durch die folgenden Zeilen, um dem Projekt die erforderlichen Abhängigkeiten hinzuzufügen:

[!code-csharp[Add the required dependencies](~/samples-qnamaker-csharp/documentation-samples/quickstarts/publish-knowledge-base/QnAMakerPublishQuickstart/Program.cs?range=1-2 "Add the required dependencies")]

## <a name="add-required-constants"></a>Hinzufügen von erforderlichen Konstanten

Fügen Sie am Anfang der **Program**-Klasse die erforderlichen Konstanten für den Zugriff auf QnA Maker hinzu.

[!code-csharp[Add the required constants](~/samples-qnamaker-csharp/documentation-samples/quickstarts/publish-knowledge-base/QnAMakerPublishQuickstart/Program.cs?range=8-34 "Add the required constants")]

## <a name="add-the-main-method-to-publish-the-knowledge-base"></a>Hinzufügen der Main-Methode zum Veröffentlichen der Wissensdatenbank

Fügen Sie nach den erforderlichen Konstanten den folgenden Code hinzu, der eine HTTPS-Anforderung an die QnA Maker-API sendet, um eine Wissensdatenbank zu veröffentlichen, und die Antwort empfängt:

[!code-csharp[Add HTTP Post request and response](~/samples-qnamaker-csharp/documentation-samples/quickstarts/publish-knowledge-base/QnAMakerPublishQuickstart/Program.cs?range=36-56 "Add HTTP Post request and response")]

Der API-Aufruf gibt den Status 204 für eine erfolgreiche Veröffentlichung ohne Inhalt im Text der Antwort zurück. 
 
## <a name="build-and-run-the-program"></a>Erstellen und Ausführen des Programms

Erstellen Sie das Programm, und führen Sie es aus. Die Anforderung wird automatisch an die QnA Maker-API gesendet, um die Wissensdatenbank zu veröffentlichen. Anschließend wird die Antwort im Konsolenfenster angezeigt.

Nachdem Ihre Wissensdatenbank veröffentlicht wurde, können Sie sie vom Endpunkt mit einer Clientanwendung oder einem Chatbot abfragen. 

[!INCLUDE [Clean up files and knowledge base](../../../../includes/cognitive-services-qnamaker-quickstart-cleanup-resources.md)] 

## <a name="next-steps"></a>Nächste Schritte

Nachdem die Wissensdatenbank veröffentlicht wurde, benötigen Sie die [Endpunkt-URL, um eine Antwort zu generieren](../Tutorials/create-publish-answer.md#generating-an-answer). 

> [!div class="nextstepaction"]
> [REST-API-Referenz für QnA Maker (V4)](https://go.microsoft.com/fwlink/?linkid=2092179)
