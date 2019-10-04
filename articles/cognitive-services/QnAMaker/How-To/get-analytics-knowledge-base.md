---
title: Analysen für die Wissensdatenbank – QnA Maker
titleSuffix: Azure Cognitive Services
description: QnA Maker speichert alle Chatprotokolle und andere Telemetriedaten, wenn Sie App Insights bei der Erstellung Ihres QnA Maker-Diensts aktiviert haben. Führen Sie die Beispielabfragen aus, um Ihre Chatprotokolle aus App Insights abzurufen.
services: cognitive-services
author: diberry
manager: nitinme
displayName: chat history, history, chat logs, logs
ms.service: cognitive-services
ms.subservice: qna-maker
ms.topic: conceptual
ms.date: 09/12/2019
ms.author: diberry
ms.openlocfilehash: 5c55084a57e46931049841f5011941b2115e9e69
ms.sourcegitcommit: dd69b3cda2d722b7aecce5b9bd3eb9b7fbf9dc0a
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 09/12/2019
ms.locfileid: "70961529"
---
# <a name="get-analytics-on-your-knowledge-base"></a>Abrufen von Analysen zu Ihrer Wissensdatenbank

QnA Maker speichert alle Chatprotokolle und andere Telemetriedaten, wenn Sie App Insights bei der [Erstellung Ihres QnA Maker-Diensts](./set-up-qnamaker-service-azure.md) aktiviert haben. Führen Sie die Beispielabfragen aus, um Ihre Chatprotokolle aus App Insights abzurufen.

1. Wechseln Sie zu Ihrer App Insights-Ressource.

    ![Auswählen Ihrer Application Insights-Ressource](../media/qnamaker-how-to-analytics-kb/resources-created.png)

2. Wählen Sie **Protokoll (Analytics)** aus. Daraufhin wird ein neues Fenster geöffnet, in dem Sie QnA Maker-Telemetriedaten abfragen können.

3. Fügen Sie die folgende Abfrage ein, und führen Sie sie aus.

    ```kusto
    requests
    | where url endswith "generateAnswer"
    | project timestamp, id, name, resultCode, duration, performanceBucket
    | parse kind = regex name with *"(?i)knowledgebases/"KbId"/generateAnswer"
    | join kind= inner (
    traces | extend id = operation_ParentId
    ) on id
    | extend question = tostring(customDimensions['Question'])
    | extend answer = tostring(customDimensions['Answer'])
    | extend score = tostring(customDimensions['Score'])
    | project timestamp, resultCode, duration, id, question, answer, score, performanceBucket,KbId 
    ```

    Wählen Sie **Ausführen** aus, um die Abfrage auszuführen.

    [![Ausführen einer Abfrage zum Ermitteln von Fragen, Antworten und Bewertungen von Benutzern](../media/qnamaker-how-to-analytics-kb/run-query.png)](../media/qnamaker-how-to-analytics-kb/run-query.png#lightbox)

## <a name="run-queries-for-other-analytics-on-your-qna-maker-knowledge-base"></a>Ausführen von Abfragen für andere Analysen zu Ihrer QnA Maker-Wissensdatenbank

### <a name="total-90-day-traffic"></a>Gesamter Datenverkehr von 90 Tagen

```kusto
    //Total Traffic
    requests
    | where url endswith "generateAnswer" and name startswith "POST"
    | parse kind = regex name with *"(?i)knowledgebases/"KbId"/generateAnswer" 
    | summarize ChatCount=count() by bin(timestamp, 1d), KbId
```

### <a name="total-question-traffic-in-a-given-time-period"></a>Gesamter Fragendatenverkehr in einem bestimmten Zeitraum

```kusto
    //Total Question Traffic in a given time period
    let startDate = todatetime('2018-02-18');
    let endDate = todatetime('2018-03-12');
    requests
    | where timestamp <= endDate and timestamp >=startDate
    | where url endswith "generateAnswer" and name startswith "POST" 
    | parse kind = regex name with *"(?i)knowledgebases/"KbId"/generateAnswer" 
    | summarize ChatCount=count() by KbId
```

### <a name="user-traffic"></a>Benutzerdatenverkehr

```kusto
    //User Traffic
    requests
    | where url endswith "generateAnswer"
    | project timestamp, id, name, resultCode, duration
    | parse kind = regex name with *"(?i)knowledgebases/"KbId"/generateAnswer"
    | join kind= inner (
    traces | extend id = operation_ParentId 
    ) on id
    | extend UserId = tostring(customDimensions['UserId'])
    | summarize ChatCount=count() by bin(timestamp, 1d), UserId, KbId
```

### <a name="latency-distribution-of-questions"></a>Latenzverteilung von Fragen

```kusto
    //Latency distribution of questions
    requests
    | where url endswith "generateAnswer" and name startswith "POST"
    | parse kind = regex name with *"(?i)knowledgebases/"KbId"/generateAnswer"
    | project timestamp, id, name, resultCode, performanceBucket, KbId
    | summarize count() by performanceBucket, KbId
```

## <a name="next-steps"></a>Nächste Schritte

> [!div class="nextstepaction"]
> [Auswählen der Kapazität](../tutorials/choosing-capacity-qnamaker-deployment.md)
