---
title: Containerunterstützung
titleSuffix: Azure Cognitive Services
description: Erfahren Sie, wie Sie die Stimmungsanalyse-Containerinstanz überprüfen.
services: cognitive-services
author: IEvangelist
manager: nitinme
ms.service: cognitive-services
ms.topic: include
ms.date: 06/26/2019
ms.author: dapine
ms.openlocfilehash: f68d9c7098f2b1ca782e2522c632c2e267b35336
ms.sourcegitcommit: f56b267b11f23ac8f6284bb662b38c7a8336e99b
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 06/28/2019
ms.locfileid: "67455110"
---
## <a name="verify-the-sentiment-analysis-container-instance"></a>Überprüfen der Stimmungsanalyse-Containerinstanz

1. Wählen Sie die Registerkarte **Übersicht** aus, und kopieren Sie die IP-Adresse.
1. Öffnen Sie eine neue Browserregisterkarte, und verwenden Sie die IP-Adresse, z.B. `http://<IP-address>:5000 (http://55.55.55.55:5000`). Die Homepage des Containers wird angezeigt, und Sie erkennen daran, dass der Container ausgeführt wird.

    ![Anzeigen der Homepage des Containers, um sicherzustellen, dass er ausgeführt wird](../media/how-tos/container-instance/swagger-docs-on-container.png)

1. Wählen Sie den Link **Dienst-API-Beschreibung** aus, um zur Swagger-Seite des Containers zu navigieren.

1. Wählen Sie eine der **POST**-APIs und dann **Ausprobieren** aus.  Die Parameter werden einschließlich der Beispieleingabe angezeigt:

    ```json
    {
      "documents": [
        {
          "language": "en",
          "id": "1",
          "text": "Hello world. This is some input text that I love."
        },
        {
          "language": "fr",
          "id": "2",
          "text": "Bonjour tout le monde"
        },
        {
          "language": "es",
          "id": "3",
          "text": "La carretera estaba atascada. Había mucho tráfico el día de ayer."
        }
      ]
    }
    ```

1. Ersetzen Sie die Eingabe durch folgenden JSON-Code:

    ```json
    {
      "documents": [
        {
          "language": "en",
          "id": "7",
          "text": "I was fortunate to attend the KubeCon Conference in Barcelona, it is one of the best conferences I have ever attended. Great people, great sessions and I thoroughly enjoyed it!"
        }
      ]
    }
    ```

1. Legen Sie für **showStats** „true“ fest.

1. Wählen Sie **Ausführen** aus, um die Stimmung des Texts zu bestimmen.

    Das im Container verpackte Modell erzeugt einen Bewertungsbereich von 0 bis 1, wobei 0 negativ und 1 positiv ist.

    Die zurückgegebene JSON-Antwort enthält die Stimmung für die aktualisierte Texteingabe:

    ```json
    {
      "documents": [
      {
        "id": "7",
        "score": 0.9826303720474243,
        "statistics": {
          "charactersCount": 176,
            "transactionsCount": 1
          }
        }
      ],
      "errors": [],
      "statistics": {
        "documentsCount": 1,
        "validDocumentsCount": 1,
        "erroneousDocumentsCount": 0,
        "transactionsCount": 1
      }
    }
    ```

Wir können nun die Dokument-`id` der JSON-Daten der Antwortnutzlasten mit der ursprünglichen Anforderungsnutzlastdokument-`id` korrelieren und sehen, dass es eine Bewertung von über `.98` gab, die auf eine sehr positive Stimmung hinweist.