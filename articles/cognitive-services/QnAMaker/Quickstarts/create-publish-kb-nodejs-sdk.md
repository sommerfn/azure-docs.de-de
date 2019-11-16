---
title: 'Schnellstart: QnA Maker-Clientbibliothek für Node.js'
titleSuffix: Azure Cognitive Services
description: Erste Schritte mit der QnA Maker-Clientbibliothek für Node.js. Führen Sie die nachfolgenden Schritte zum Installieren des Pakets aus, und testen Sie den Beispielcode für grundlegende Aufgaben.  Mit QnA Maker können Sie einen Frage-und-Antwort-Dienst auf der Grundlage Ihrer teilweise strukturierten Inhalte bereitstellen. Bei solchen Inhalten kann es sich beispielsweise um FAQ-Dokumente, URLs und Produkthandbücher handeln.
services: cognitive-services
author: diberry
manager: nitinme
ms.service: cognitive-services
ms.subservice: qna-maker
ms.topic: quickstart
ms.date: 09/26/2019
ms.author: diberry
ms.openlocfilehash: 7650dce9a3ef494815b8d7a326eb07e1e25f2da2
ms.sourcegitcommit: 5a8c65d7420daee9667660d560be9d77fa93e9c9
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 11/15/2019
ms.locfileid: "74123093"
---
# <a name="quickstart-qna-maker-client-library-for-nodejs"></a>Schnellstart: QnA Maker-Clientbibliothek für Node.js

Erste Schritte mit der QnA Maker-Clientbibliothek für Node.js. Führen Sie die nachfolgenden Schritte zum Installieren des Pakets aus, und testen Sie den Beispielcode für grundlegende Aufgaben.  Mit QnA Maker können Sie einen Frage-und-Antwort-Dienst auf der Grundlage Ihrer teilweise strukturierten Inhalte bereitstellen. Bei solchen Inhalten kann es sich beispielsweise um FAQ-Dokumente, URLs und Produkthandbücher handeln. 

Verwenden Sie die QnA Maker-Clientbibliothek für Node.js für folgende Zwecke:

* Erstellen einer Wissensdatenbank 
* Verwalten einer Wissensdatenbank
* Veröffentlichen einer Wissensdatenbank

[Referenzdokumentation](https://docs.microsoft.com/javascript/api/@azure/cognitiveservices-textanalytics) | [Quellcode der Bibliothek](https://github.com/Azure/azure-sdk-for-js/tree/master/sdk/cognitiveservices/Knowledge.QnAMaker) | [Paket (npm)](https://www.npmjs.com/package/@azure/cognitiveservices-qnamaker) | [Node.js-Beispiele](https://github.com/Azure-Samples/cognitive-services-qnamaker-nodejs/blob/master/documentation-samples/quickstarts/knowledgebase_quickstart/knowledgebase_quickstart.js)

[!INCLUDE [Custom subdomains notice](../../../../includes/cognitive-services-custom-subdomains-note.md)]

## <a name="prerequisites"></a>Voraussetzungen

* Azure-Abonnement – [Erstellen eines kostenlosen Kontos](https://azure.microsoft.com/free/)
* Die aktuelle Version von [Node.js](https://nodejs.org).

## <a name="setting-up"></a>Einrichten

### <a name="create-a-qna-maker-azure-resource"></a>Erstellen einer QnA Maker-Azure-Ressource

Azure Cognitive Services werden von Azure-Ressourcen dargestellt, die Sie abonnieren. Erstellen Sie eine Ressource für QnA Maker mithilfe des [Azure-Portals](https://docs.microsoft.com/azure/cognitive-services/cognitive-services-apis-create-account) oder der [Azure CLI](https://docs.microsoft.com/azure/cognitive-services/cognitive-services-apis-create-account-cli) auf Ihrem lokalen Computer. 

Führen Sie nach dem Abrufen eines Schlüssels aus Ihrer Ressource die [Erstellung von Umgebungsvariablen](https://docs.microsoft.com/azure/cognitive-services/cognitive-services-apis-create-account#configure-an-environment-variable-for-authentication) mit den Namen `QNAMAKER_SUBSCRIPTION_KEY` und `QNAMAKER_HOST` für die Ressource durch. Verwenden Sie die Schlüssel- und Endpunktwerte auf den Seiten **Schlüssel** und **Übersicht** der Ressource im Azure-Portal.

### <a name="create-a-new-nodejs-application"></a>Erstellen einer neuen Node.js-Anwendung

Erstellen Sie in einem Konsolenfenster (etwa cmd, PowerShell oder Bash) ein neues Verzeichnis für Ihre App, und rufen Sie es auf. 

```console
mkdir myapp && cd myapp
```

Führen Sie den Befehl `npm init -y` aus, um eine Knotenanwendung mit der Datei `package.json` zu erstellen. 

```console
npm init -y
```

### <a name="install-the-client-library"></a>Installieren der Clientbibliothek

Installieren Sie die npm-Pakete `ms-rest-azure` und `azure-cognitiveservices-qnamaker`:

```console
npm install azure-cognitiveservices--qnamaker ms-rest-azure --save
```

Die Datei `package.json` Ihrer App wird mit den Abhängigkeiten aktualisiert.


## <a name="object-model"></a>Objektmodell

Der QnA Maker-Client ist ein [QnAMakerClient](https://docs.microsoft.com/javascript/api/@azure/cognitiveservices-qnamaker/qnamakerclient?view=azure-node-latest)-Objekt, das per „ServiceClientCredentials“-Element, in dem Ihr Schlüssel enthalten ist, bei Azure authentifiziert wird.

Nachdem der Client erstellt wurde, verwenden Sie die [Knowledge Base](https://docs.microsoft.com/javascript/api/@azure/cognitiveservices-qnamaker/qnamakerclient?view=azure-node-latest#knowledgebase)-Eigenschaft, um Ihre Knowledge Base zu erstellen, zu verwalten und zu veröffentlichen. 

Verwalten Sie Ihre Knowledge Base durch Senden eines JSON-Objekts. Bei sofortigen Vorgängen gibt eine Methode in der Regel ein JSON-Objekt zurück, das den Status angibt. Bei zeitintensiven Vorgängen ist die Antwort die Vorgangs-ID. Rufen Sie die [client.Operations.getDetails](https://docs.microsoft.com/javascript/api/@azure/cognitiveservices-qnamaker/operations?view=azure-node-latest#getdetails-string--servicecallback-operation--)-Methode mit der Vorgangs-ID auf, um den [Status der Anforderung](https://docs.microsoft.com/javascript/api/@azure/cognitiveservices-qnamaker/operationstatetype?view=azure-node-latest) zu bestimmen. 

 
## <a name="code-examples"></a>Codebeispiele

Mit den Codeausschnitten wird veranschaulicht, wie folgende Vorgänge mit der QnA Maker-Clientbibliothek für Node.js durchgeführt werden:

* [Erstellen einer Wissensdatenbank](#create-a-knowledge-base)
* [Aktualisieren einer Wissensdatenbank](#update-a-knowledge-base)
* [Veröffentlichen einer Wissensdatenbank](#publish-a-knowledge-base)
* [Löschen einer Wissensdatenbank](#delete-a-knowledge-base)
* [Abrufen des Status eines Vorgangs](#get-status-of-an-operation)

## <a name="add-the-dependencies"></a>Hinzufügen der Abhängigkeiten

Erstellen Sie eine Datei mit dem Namen `index.js`. Fügen Sie der Datei die QnA Maker-Bibliothek und die Azure-REST-Bibliothek hinzu.

[!code-javascript[Require statements](~/samples-qnamaker-nodejs/documentation-samples/quickstarts/knowledgebase_quickstart/knowledgebase_quickstart.js?name=dependencies)]

Erstellen Sie Variablen für den Azure-Endpunkt und -Schlüssel Ihrer Ressource. Wenn Sie die Umgebungsvariable nach dem Start der Anwendung erstellt haben, müssen der Editor, die IDE oder die Shell, in dem bzw. der sie ausgeführt wird, geschlossen und erneut geöffnet werden, damit der Zugriff auf die Variable möglich ist.

|Umgebungsvariable|Node.js-Variable|Beispiel|
|--|--|--|
|`QNAMAKER_SUBSCRIPTION_KEY`|`subscription_key`|32-Zeichen-GUID|
|`QNAMAKER_HOST`|`endpoint`|`https://your-resource-name.api.cognitive.microsoft.com`: Ersetzen Sie die Unterdomäne `your-resource-name` durch den Wert des Ressourcennamens.|
||||

[!code-javascript[Azure resource variables](~/samples-qnamaker-nodejs/documentation-samples/quickstarts/knowledgebase_quickstart/knowledgebase_quickstart.js?name=resourcekeys)]

## <a name="authenticate-the-client"></a>Authentifizieren des Clients

Erstellen Sie als Nächstes ein ServiceClientCredentials-Objekt mit Ihrem Schlüssel, und verwenden Sie es mit Ihrem Endpunkt, um ein [QnAMakerClient](https://docs.microsoft.com/javascript/api/%40azure/cognitiveservices-qnamaker/qnamakerclient?view=azure-node-latest#qnamakerclient-serviceclientcredentials--string--msrest-serviceclientoptions-)-Objekt zu erstellen. Verwenden Sie das Clientobjekt, um ein Objekt vom Typ [Knowledgebase](https://docs.microsoft.com/javascript/api/@azure/cognitiveservices-qnamaker/knowledgebase?view=azure-node-latest) abzurufen.


[!code-javascript[Authorization to resource key](~/samples-qnamaker-nodejs/documentation-samples/quickstarts/knowledgebase_quickstart/knowledgebase_quickstart.js?name=authorization)]

## <a name="create-a-knowledge-base"></a>Erstellen einer Wissensdatenbank

Eine Knowledge Base speichert Frage- und Antwortpaare für das [CreateKbDTO](https://docs.microsoft.com/javascript/api/@azure/cognitiveservices-qnamaker/createkbdto?view=azure-node-latest)-Objekt aus drei Quellen:

* Für **redaktionellen Inhalt** verwenden Sie das [QnADTO](https://docs.microsoft.com/javascript/api/@azure/cognitiveservices-qnamaker/qnadto?view=azure-node-latest)-Objekt.
* Für **Dateien** verwenden Sie das [FileDTO](https://docs.microsoft.com/javascript/api/@azure/cognitiveservices-qnamaker/filedto?view=azure-node-latest)-Objekt. 
* Für **URLs** verwenden Sie eine Liste mit Zeichenfolgen.

Rufen Sie die [create](https://docs.microsoft.com/javascript/api/@azure/cognitiveservices-qnamaker/knowledgebase?view=azure-node-latest#create-createkbdto--servicecallback-operation--)-Methode auf, und übergeben Sie dann die zurückgegebene Vorgangs-ID an die [Operations.getDetails](#get-status-of-an-operation)-Methode, um den Status abzurufen. 

[!code-javascript[Create a knowledge base](~/samples-qnamaker-nodejs/documentation-samples/quickstarts/knowledgebase_quickstart/knowledgebase_quickstart.js?name=createkb&highlight=15)]

Nehmen Sie zum erfolgreichen Erstellen einer Wissensdatenbank unbedingt die Funktion [`wait_for_operation`](#get-status-of-an-operation) mit auf, auf die im Code oben verwiesen wird. 

## <a name="update-a-knowledge-base"></a>Aktualisieren einer Wissensdatenbank

Sie können eine Wissensdatenbank aktualisieren, indem Sie die Wissensdatenbank-ID und ein [UpdateKbOperationDTO](https://docs.microsoft.com/javascript/api/@azure/cognitiveservices-qnamaker/updatekboperationdto?view=azure-node-latest)-Element, das [add](https://docs.microsoft.com/javascript/api/@azure/cognitiveservices-qnamaker/updatekboperationdto?view=azure-node-latest#add)-, [update](https://docs.microsoft.com/javascript/api/@azure/cognitiveservices-qnamaker/updatekboperationdto?view=azure-node-latest#update)- und [delete](https://docs.microsoft.com/javascript/api/@azure/cognitiveservices-qnamaker/updatekboperationdto?view=azure-node-latest#deleteproperty)-DTO-Objekte enthält, als Eingabe an die [update](https://docs.microsoft.com/javascript/api/@azure/cognitiveservices-qnamaker/knowledgebase?view=azure-node-latest#update-string--updatekboperationdto--msrest-requestoptionsbase-)-Methode übergeben. Verwenden Sie die [Operation.getDetail](#get-status-of-an-operation) -Methode, um zu bestimmen, ob das Update erfolgreich war.

[!code-javascript[Update a knowledge base](~/samples-qnamaker-nodejs/documentation-samples/quickstarts/knowledgebase_quickstart/knowledgebase_quickstart.js?name=updatekb&highlight=19)]

Nehmen Sie zum erfolgreichen Aktualisieren einer Wissensdatenbank unbedingt die Funktion [`wait_for_operation`](#get-status-of-an-operation) mit auf, auf die im Code oben verwiesen wird. 

## <a name="publish-a-knowledge-base"></a>Veröffentlichen einer Wissensdatenbank

Veröffentlichen Sie die Wissensdatenbank mit der [publish](https://docs.microsoft.com/javascript/api/@azure/cognitiveservices-qnamaker/knowledgebase?view=azure-node-latest#publish-string--msrest-requestoptionsbase-)-Methode. Diese übernimmt das aktuell gespeicherte und trainierte Modell, auf das von der Knowledge Base-ID verwiesen wird, und veröffentlicht dieses an einem Endpunkt. 

[!code-javascript[Publish a knowledge base](~/samples-qnamaker-nodejs/documentation-samples/quickstarts/knowledgebase_quickstart/knowledgebase_quickstart.js?name=publishkb&highlight=2)]

## <a name="delete-a-knowledge-base"></a>Löschen einer Wissensdatenbank

Löschen Sie die Wissensdatenbank mit der [delete](https://docs.microsoft.com/javascript/api/@azure/cognitiveservices-qnamaker/knowledgebase?view=azure-node-latest#deletemethod-string--msrest-requestoptionsbase-)-Methode mit einem Parameter der Wissensdatenbank-ID. 

[!code-javascript[Delete a knowledge base](~/samples-qnamaker-nodejs/documentation-samples/quickstarts/knowledgebase_quickstart/knowledgebase_quickstart.js?name=deletekbs&highlight=2)]

## <a name="get-status-of-an-operation"></a>Abrufen des Status eines Vorgangs

Einige Methoden, wie „create“ und „update“, können so viel Zeit in Anspruch nehmen, dass ein [Vorgang](https://docs.microsoft.com/javascript/api/@azure/cognitiveservices-qnamaker/operations?view=azure-node-latest) zurückgegeben wird, anstatt auf den Abschluss des Prozesses zu warten. Verwenden Sie die [Vorgangs-ID](https://docs.microsoft.com/javascript/api/@azure/cognitiveservices-qnamaker/operation?view=azure-node-latest#operationid) des Vorgangs, um den Status der ursprünglichen Methode mittels Abrufen (mit Wiederholungslogik) zu bestimmen. 

Der _setTimeout_-Aufruf im folgenden Codeblock wird verwendet, um asynchronen Code zu simulieren. Ersetzen Sie dies durch Wiederholungslogik. 

[!code-javascript[Monitor an operation](~/samples-qnamaker-nodejs/documentation-samples/quickstarts/knowledgebase_quickstart/knowledgebase_quickstart.js?name=monitorOperation&highlight=2,17)]

## <a name="run-the-application"></a>Ausführen der Anwendung

Führen Sie die Anwendung mit dem Befehl `node index.js` über das Anwendungsverzeichnis aus.


Alle Codeausschnitte in diesem Artikel sind [verfügbar](https://github.com/Azure-Samples/cognitive-services-qnamaker-nodejs/blob/master/documentation-samples/quickstarts/knowledgebase_quickstart/knowledgebase_quickstart.js) und können als einzelne Datei ausgeführt werden.

```console
node index.js
```

## <a name="clean-up-resources"></a>Bereinigen von Ressourcen

Wenn Sie ein Cognitive Services-Abonnement bereinigen und entfernen möchten, können Sie die Ressource oder die Ressourcengruppe löschen. Wenn Sie die Ressourcengruppe löschen, werden auch alle anderen Ressourcen gelöscht, die ihr zugeordnet sind.

* [Portal](../../cognitive-services-apis-create-account.md#clean-up-resources)
* [Azure-Befehlszeilenschnittstelle](../../cognitive-services-apis-create-account-cli.md#clean-up-resources)

## <a name="next-steps"></a>Nächste Schritte

> [!div class="nextstepaction"]
>[Tutorial: ](../tutorials/create-publish-query-in-portal.md)Erstellen und Beantworten einer Knowledge Base

* [Was ist die QnA Maker-API?](../Overview/overview.md)
* [Bearbeiten einer Knowledge Base](../how-to/edit-knowledge-base.md)
* [Abrufen von Nutzungsanalysen](../how-to/get-analytics-knowledge-base.md)