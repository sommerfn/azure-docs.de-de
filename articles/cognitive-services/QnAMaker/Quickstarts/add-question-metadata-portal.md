---
title: 'Schnellstart: Hinzufügen von Fragen und Antworten im QnA Maker-Portal'
titleSuffix: Azure Cognitive Services
description: ''
services: cognitive-services
author: diberry
manager: nitinme
ms.service: cognitive-services
ms.subservice: qna-maker
ms.topic: quickstart
ms.date: 10/01/2019
ms.author: diberry
ms.openlocfilehash: 92735d8982fb1364d5ebfe0494f5ee51f4302469
ms.sourcegitcommit: 018e3b40e212915ed7a77258ac2a8e3a660aaef8
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 11/07/2019
ms.locfileid: "73794005"
---
# <a name="quickstart-add-questions-and-answer-with-qna-maker-portal"></a>Schnellstart: Hinzufügen von Fragen und Antworten im QnA Maker-Portal

Wenn Sie eine Wissensdatenbank erstellt haben, fügen Sie Frage-Antwort-Sätze mit Metadaten hinzu, damit Ihre Benutzer die richtige Antwort auf ihre Frage finden können.

Die richtige Antwort ist eine einzige Antwort, aber es gibt unter Umständen viele Möglichkeiten, wie ein Kunde die Frage stellen kann, die zu dieser einzelnen Antwort führt.

Die Fragen in der folgenden Tabelle hängen beispielsweise alle mit Azure-Diensteinschränkungen zusammen, wobei sich aber jede auf einen anderen Azure-Dienst bezieht. 

<a name="qna-table"></a>


|Set|Fragen|Antwort|Metadaten|
|--|--|--|--|
|1|`How large a knowledge base can I create?`<br><br>`What is the max size of a knowledge base?`<br><br>`How many GB of data can a knowledge base hold?` |`The size of the knowledge base depends on the SKU of Azure search you choose when creating the QnA Maker service. Read [here](https://docs.microsoft.com/azure/cognitive-services/qnamaker/tutorials/choosing-capacity-qnamaker-deployment) for more details.`|`service=qna_maker`<br>`link_in_answer=true`|
|2|`How many knowledge bases can I have for my QnA Maker service?`<br><br>`I selected a Azure Cognitive Search tier that holds 15 knowledge bases, but I can only create 14 - what is going on?`<br><br>`What is the connection between the number of knowledge bases in my QnA Maker service and the Azure Cognitive Search service size?` |`Each knowledge base uses 1 index, and all the knowledge bases share a test index. You can have N-1 knowledge bases where N is the number of indexes your Azure Cognitive Search tier supports.`|`service=search`<br>`link_in_answer=false`|

Sobald Metadaten zu einem Frage-Antwort-Satz hinzugefügt wurden, kann die Clientanwendung folgende Aktionen ausführen:

* Antworten anfordern, die nur bestimmten Metadaten entsprechen
* Alle Antworten empfangen, die Antworten jedoch in Abhängigkeit von den Metadaten für die einzelnen Antworten nachverarbeiten

Wenn Sie kein Azure-Abonnement besitzen, können Sie ein [kostenloses Konto](https://azure.microsoft.com/free/?WT.mc_id=A261C142F) erstellen, bevor Sie beginnen. 

## <a name="prerequisites"></a>Voraussetzungen

* Ein QnA Maker-Dienst
* Eine in diesem QnA Maker-Dienst erstellte Wissensdatenbank

Beides wurde in der [ersten Schnellstartanleitung](../how-to/create-knowledge-base.md) erstellt.

## <a name="sign-in-to-the-qna-maker-portal"></a>Anmelden beim QnA Maker-Portal

1. Melden Sie sich beim [QnA Maker-Portal](https://www.qnamaker.ai) an.

1. Wählen Sie Ihre vorhandene Wissensdatenbank aus. Wenn Sie über keine Wissensdatenbank verfügen, kehren Sie zur [vorherigen Schnellstartanleitung](../how-to/create-knowledge-base.md) zurück, und schließen Sie die Schritte zum Erstellen Ihrer Wissensdatenbank ab.

## <a name="add-additional-alternatively-phrased-questions"></a>Hinzufügen zusätzlicher Fragen mit alternativen Formulierungen 

Die aktuelle Wissensdatenbank aus der [vorherigen Schnellstartanleitung](../how-to/create-knowledge-base.md) enthält die QnA Maker-Frage-Antwort-Sätze zur Problembehandlung. Diese Sätze wurden erstellt, als die URL während des Erstellungvorgangs zur Wissensdatenbank hinzugefügt wurde. 

Beim Importieren dieser URL wurde nur eine Frage mit einer Antwort erstellt. 

Fügen Sie in diesem Verfahren weitere Fragen hinzu.

1. Verwenden Sie auf der Seite **Edit** (Bearbeiten) das Suchfeld über den Frage-Antwort-Sätzen, um die Frage `How large a knowledge base can I create?` zu suchen.

1. Wählen Sie in der Spalte **Question** (Frage) die Option **+ Add alternative phrasing** (Alternative Formulierung hinzufügen) aus, und fügen Sie anschließend die neuen Formulierungen hinzu, die in der folgenden Tabelle angegeben sind.

    |Alternative Formulierung|
    |--|
    |`What is the max size of a knowledge base?`|
    |`How many GB of data can a knowledge base hold?`| 

1. Wählen Sie **Save and train** (Speichern und trainieren) aus, um die Wissensdatenbank erneut zu trainieren. 

1. Wählen Sie **Test** (Testen) aus, und geben Sie eine Frage ein, die einem der neuen alternativen Ausdrücke ähnelt, aber nicht genau denselben Wortlaut hat:

    `What GB size can a knowledge base be?`

    Die richtige Antwort wird im Markdownformat zurückgegeben: `The size of the knowledge base depends on the SKU of Azure search you choose when creating the QnA Maker service. Read [here](https://docs.microsoft.com/azure/cognitive-services/qnamaker/tutorials/choosing-capacity-qnamaker-deployment) for more details.`

    Wenn Sie unter der zurückgegebenen Antwort **Inspect** (Überprüfen) auswählen, sehen Sie, dass mehrere Antworten zur Frage passen, aber nicht mit der gleichen hohen Zuverlässigkeit. 

    Fügen Sie nicht alle möglichen Kombinationen alternativer Formulierungen hinzu. Aktivieren Sie das [aktive Lernen](../how-to/improve-knowledge-base.md) für QnA Maker. Dadurch werden alternative Formulierungen gesucht, die Ihre Wissensdatenbank am besten unterstützen, um die Anforderungen Ihrer Benutzer zu erfüllen.

1. Wählen Sie erneut **Test** (Testen) aus, um das Testfenster zu schließen.

## <a name="add-metadata-to-filter-the-answers"></a>Hinzufügen von Metadaten zum Filtern der Antworten

Durch das Hinzufügen von Metadaten zu einem Frage-Antwort-Satz kann Ihre Clientanwendung gefilterte Antworten anfordern. Dieser Filter wird angewendet, bevor [die erste und zweite Rangfolge](../concepts/knowledge-base.md#ranker-process) angewendet wird.

1. Fügen Sie den zweiten Frage-Antwort-Satz ohne Metadaten aus der [ersten Tabelle in dieser Schnellstartanleitung](#qna-table) hinzu, und fahren Sie dann mit den folgenden Schritten fort: 

1. Wählen Sie **View options** (Optionen anzeigen) und anschließend **Show metadata** (Metadaten anzeigen) aus. 

1. Wählen Sie für den Frage-Antwort-Satz, den Sie gerade hinzugefügt haben, die Option **Add metadata tags** (Metadatentags hinzufügen) aus, und fügen Sie dann den Namen für `service` sowie die Werte `search` und `service:search` hinzu.

1. Fügen Sie weitere Metadatentags mit dem Namen `link_in_answer` und den Werten `false` und `link_in_answer:false` hinzu.

1. Suchen Sie in der Tabelle `How large a knowledge base can I create?` nach der ersten Antwort. 
1. Fügen Sie für die gleichen zwei Metadatentags Metadatenpaare hinzu:

    `link_in_answer` : `true`<br>
    `server`: `qna_maker`

    Sie haben jetzt zwei Fragen mit denselben Metadatentags mit unterschiedlichen Werten. 

1. Wählen Sie **Save and train** (Speichern und trainieren) aus, um die Wissensdatenbank erneut zu trainieren. 

1. Wählen Sie **Publish** (Veröffentlichen) aus, um zur Veröffentlichungsseite zu wechseln. 
1. Wählen Sie die Schaltfläche **Publish** (Veröffentlichen) aus, um die aktuelle Wissensdatenbank auf einem abfragbaren Endpunkt zu veröffentlichen. 
1. Nachdem die Wissensdatenbank veröffentlicht wurde, wählen Sie die Registerkarte **Curl** aus, um einen cURL-Beispielbefehl anzuzeigen, mit dem eine Antwort aus der Wissensdatenbank generiert wird.
1. Kopieren Sie den Befehl, und fügen Sie ihn in den Editor oder eine andere Umgebung ein, in der Sie ihn bearbeiten können. Bearbeiten Sie Ihren eigenen Ressourcennamen, die Wissensdatenbank-ID und den Endpunktschlüssel:

    |Replace|
    |--|
    |`your-resource-name`|
    |`your-knowledge-base-id`|
    |`your-endpoint-key`|

    ```curl
    curl -X POST https://your-resource-name.azurewebsites.net/qnamaker/knowledgebases/your-knowledge-base-id/generateAnswer -H "Authorization: EndpointKey your-endpoint-key" -H "Content-type: application/json" -d "{'top':30, 'question':'size','strictFilters': [{'name':'service','value':'qna_maker'}]}"
    ```

    Beachten Sie, dass es sich bei der Frage lediglich um ein einzelnes Wort (`size`) handelt, das jeden Frage-Antwort-Satz zurückgeben kann. Das Array `strictFilters` gibt die Antwort aus und reduziert sie ausschließlich auf die `qna_maker`-Antworten. 

    [!INCLUDE [Tip for debug property to JSON request](../includes/tip-debug-json.md)]

1. Die Ausgabe enthält nur die Antwort, die den Filterkriterien entspricht. 

    Die folgende cURL-Antwort wurde aus Gründen der Lesbarkeit formatiert:

    ```JSON
    {
        "answers": [
            {
                "questions": [
                    "How large a knowledge base can I create?",
                    "What is the max size of a knowledge base?",
                    "How many GB of data can a knowledge base hold?"
                ],
                "answer": "The size of the knowledge base depends on the SKU of Azure search you choose when creating the QnA Maker service. Read [here](https://docs.microsoft.com/azure/cognitive-services/qnamaker/tutorials/choosing-capacity-qnamaker-deployment)for more details.",
                "score": 68.76,
                "id": 3,
                "source": "https://docs.microsoft.com/azure/cognitive-services/qnamaker/troubleshooting",
                "metadata": [
                    {
                        "name": "link_in_answer",
                        "value": "true"
                    },
                    {
                        "name": "service",
                        "value": "qna_maker"
                    }
                ],
                "context": {
                    "isContextOnly": false,
                    "prompts": []
                }
            }
        ],
        "debugInfo": null
    }
    ```

    Wenn ein Frage-Antwort-Satz nicht mit dem Suchbegriff, dafür aber mit dem Filter übereinstimmt, wird er nicht ausgegeben. Stattdessen wird die allgemeine Antwort `No good match found in KB.` zurückgegeben.

    Stellen Sie sicher, dass der Metadatenname und die Wertpaare innerhalb der erforderlichen Grenzwerte liegen. 

## <a name="clean-up-resources"></a>Bereinigen von Ressourcen

Wenn Sie ein Cognitive Services-Abonnement bereinigen und entfernen möchten, können Sie die Ressource oder die Ressourcengruppe löschen. Wenn Sie die Ressourcengruppe löschen, werden auch alle anderen Ressourcen gelöscht, die ihr zugeordnet sind.

* [Portal](../../cognitive-services-apis-create-account.md#clean-up-resources)
* [Azure-Befehlszeilenschnittstelle](../../cognitive-services-apis-create-account-cli.md#clean-up-resources)

## <a name="next-steps"></a>Nächste Schritte

> [!div class="nextstepaction"]
> [Abrufen einer Antwort mit Postman](get-answer-from-kb-using-postman.md)