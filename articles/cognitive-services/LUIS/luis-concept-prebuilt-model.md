---
title: Vordefinierte Modelle – LUIS
titleSuffix: Azure Cognitive Services
description: Vordefinierte Modelle stellen Domänen, Absichten, Entitäten und Äußerungen bereit. Sie können Ihre App mit einer vordefinierten Domäne starten oder ihr später eine geeignete Domäne hinzufügen.
services: cognitive-services
author: diberry
manager: nitinme
ms.custom: seodec18
ms.service: cognitive-services
ms.subservice: language-understanding
ms.topic: conceptual
ms.date: 07/29/2019
ms.author: diberry
ms.openlocfilehash: b88801ded3dea7c7514ff117361feba3e95444ed
ms.sourcegitcommit: b4665f444dcafccd74415fb6cc3d3b65746a1a31
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 10/11/2019
ms.locfileid: "72264388"
---
# <a name="prebuilt-domain-intent-and-entity-models"></a>Vordefinierte Modelle für Domäne, Absicht und Entität

Vordefinierte Modelle stellen Domänen, Absichten, Entitäten und Äußerungen bereit. Sie können Ihre App mit einer vordefinierten Domäne starten oder ihr später eine geeignete Domäne hinzufügen. 

## <a name="types-of-prebuilt-models"></a>Typen von vordefinierten Modellen

LUIS stellt drei Typen von vordefinierten Modellen bereit. Jedes Modell kann Ihrer App jederzeit hinzugefügt werden. 

|Modelltyp|Umfang|
|--|--|
|Domain|Absichten, Äußerungen, Entitäten|
|Absichten|Absichten, Äußerungen|
|Entitäten|Nur Entitäten| 

## <a name="prebuilt-domains"></a>Vordefinierte Domänen

Language Understanding Intelligent Service (LUIS) bietet *vordefinierte Domänen*. Dabei handelt es sich um vordefinierte Sätze von [Absichten](luis-how-to-add-intents.md) und [Entitäten](luis-concept-entity-types.md), die für Domänen oder allgemeine Kategorien von Clientanwendungen zusammenarbeiten. 

Die vordefinierten Domänen wurden trainiert und können Ihrer LUIS-App direkt hinzugefügt werden. Die Absichten und Entitäten in einer vordefinierten Domäne sind nach dem Hinzufügen zu einer App vollständig anpassbar. 

Wenn Sie eine gesamte vordefinierte Domäne anpassen, löschen Sie die Absichten und Entitäten, die Ihre Anwendung nicht verwenden muss. Sie können auch der Gruppe einige Absichten oder Entitäten hinzufügen, die die vordefinierte Domäne bereits enthält. Wenn Sie beispielsweise die vordefinierte Domäne **Ereignisse** für eine Sportveranstaltungs-App verwenden, können Sie Entitäten für Sportteams hinzufügen. Wenn Sie in LUIS [Äußerungen bereitstellen](luis-how-to-add-example-utterances.md), schließen Sie Begriffe ein, die für Ihre App spezifisch sind. LUIS lernt, diese zu erkennen, und passt die Absichten und Entitäten der vordefinierten Domäne an die Anforderungen Ihrer App an. 

> [!TIP]
> Die Absichten und Entitäten in einer vordefinierten Domäne funktionieren am besten zusammen. Es ist besser, nach Möglichkeit Absichten und Entitäten aus derselben Domäne zu kombinieren.
> In der vordefinierten Domäne „Utilities“ finden Sie Absichten, die Sie für die Verwendung in beliebigen Domänen anpassen können. Beispielsweise können Sie Ihrer App `Utilities.Repeat` hinzufügen und für die Aktionen trainieren, die Benutzer in Ihrer Anwendung wiederholen möchten. 

### <a name="changing-the-behavior-of-a-prebuilt-domain-intent"></a>Ändern des Verhaltens der Absicht einer vordefinierten Domäne

Möglicherweise enthält eine vordefinierte Domäne eine Absicht, die einer Absicht, die Sie in Ihrer LUIS-App haben möchten, ähnelt, Sie möchten das Verhalten jedoch ändern. Beispielsweise enthält die vordefinierte Domäne **Places** die Absicht `MakeReservation` zum Durchführen einer Restaurantreservierung, aber Ihre App soll mit dieser Absicht Hotelreservierungen vornehmen. In diesem Fall können Sie das Verhalten dieser Absicht ändern, indem Sie LUIS Äußerungen zu Hotelreservierungen bereitstellen und diese mithilfe der Absicht `MakeReservation` bezeichnen. LUIS kann dann neu trainiert werden, sodass die Absicht `MakeReservation` in einer Anforderung zum Buchen eines Hotelzimmers erkannt wird.

Eine vollständige Auflistung der vordefinierten Domänen finden Sie in der [Referenz zu vordefinierten Domänen](./luis-reference-prebuilt-domains.md).

## <a name="prebuilt-intents"></a>Vordefinierte Absichten

LUIS bietet vordefinierte Absichten mit Äußerungen. Absichten können auch ohne die gesamte Domäne hinzugefügt werden. Beim Hinzufügen einer Absicht werden die Absicht selbst und ihre Äußerungen hinzugefügt. Sowohl der Name der Absicht als auch die Liste der Äußerungen können geändert werden.  

## <a name="prebuilt-entities"></a>Vordefinierte Entitäten

LUIS umfasst eine Reihe von vordefinierten Entitäten zur Erkennung allgemeiner Informationstypen, z.B. Datumsangaben, Zeitangaben, Zahlen, Maße und Währungen. Die Unterstützung von vordefinierten Entität variiert je nach Kultur Ihrer LUIS-App. Eine vollständige Liste vordefinierter Entitäten, die von LUIS unterstützt werden, einschließlich der Unterstützung durch die Kultur, finden Sie unter [Referenz zu vordefinierten Entitäten](./luis-reference-prebuilt-entities.md).

Wenn Ihre Anwendung eine vordefinierte Entität enthält, werden ihre Vorhersagen in der veröffentlichten Anwendung eingeschlossen. Das Verhalten vordefinierter Entitäten ist vortrainiert und kann **nicht** angepasst werden. 

> [!NOTE]
> Die Entität **builtin.datetime** ist veraltet. Sie wurde durch [**builtin.datetimeV2**](luis-reference-prebuilt-datetimev2.md) ersetzt. Diese stellt die Erkennung von Datums- und Zeitbereichen sowie verbesserte Erkennung mehrdeutiger Datums- und Zeitangaben bereit.

## <a name="next-steps"></a>Nächste Schritte

Erfahren Sie, wie Sie Ihrer App [vordefinierte Entitäten hinzufügen](luis-prebuilt-entities.md).
