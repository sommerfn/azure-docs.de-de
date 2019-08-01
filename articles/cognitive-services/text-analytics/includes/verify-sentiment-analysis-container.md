---
title: Überprüfen der Stimmungsanalyse-Containerinstanz
titleSuffix: Azure Cognitive Services
description: Erfahren Sie, wie Sie die Stimmungsanalyse-Containerinstanz überprüfen.
services: cognitive-services
author: IEvangelist
manager: nitinme
ms.service: cognitive-services
ms.topic: include
ms.date: 06/26/2019
ms.author: dapine
ms.openlocfilehash: f69d573e9e70a505018e94cca354f363097cc1b8
ms.sourcegitcommit: 6b41522dae07961f141b0a6a5d46fd1a0c43e6b2
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 07/15/2019
ms.locfileid: "68229338"
---
## <a name="verify-the-sentiment-analysis-container-instance"></a>Überprüfen der Stimmungsanalyse-Containerinstanz

1. Wählen Sie die Registerkarte **Übersicht** aus, und kopieren Sie die IP-Adresse.
1. Öffnen Sie eine neue Registerkarte im Browser, und geben Sie die IP-Adresse ein. Geben Sie zum Beispiel `http://<IP-address>:5000 (http://55.55.55.55:5000`) ein. Daraufhin wird die Homepage des Containers angezeigt. Daran erkennen Sie, dass der Container ausgeführt wird.

    ![Anzeigen der Homepage des Containers, um sicherzustellen, dass er ausgeführt wird](../media/how-tos/container-instance/swagger-docs-on-container.png).

1. Klicken Sie auf den Link **Service API Description** (Dienst-API-Beschreibung), um die Swagger-Seite des Containers aufzurufen.

1. Wählen Sie eine der **POST**-APIs und dann **Ausprobieren** aus.  Die Parameter werden einschließlich der folgende Beispieleingabe angezeigt:

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

    Das im Container enthaltene Modell erzeugt einen Bewertungsbereich von 0 bis 1, wobei 0 negativ und 1 positiv ist.

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

Das Dokument `id` der in der Antwort enthaltenen JSON-Daten kann nun mit dem ursprünglichen Dokument `id` in Korrelation gebracht werden. Eine Bewertung von über `.98` wird angezeigt, was einer stark positiven Stimmung entspricht.