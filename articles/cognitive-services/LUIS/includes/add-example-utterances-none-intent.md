---
title: include file
description: include file
services: cognitive-services
author: diberry
manager: nitinme
ms.service: cognitive-services
ms.subservice: luis
ms.topic: include
ms.custom: include file
ms.date: 11/05/2019
ms.author: diberry
ms.openlocfilehash: bbe40083159d433c0b5746834e1c530b23b03851
ms.sourcegitcommit: 359930a9387dd3d15d39abd97ad2b8cb69b8c18b
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 11/06/2019
ms.locfileid: "73648089"
---
Die Clientanwendung muss wissen, ob eine Äußerung für die Anwendung unsinnig oder ungeeignet ist. Die Absicht **None** (Keine) wird jeder Anwendung bei ihrer Erstellung hinzugefügt, um zu bestimmen, ob eine Äußerung nicht von der Clientanwendung beantwortet werden kann.

Wenn LUIS für eine Äußerung die Absicht **None** (Keine) zurückgibt, kann Ihre Clientanwendung fragen, ob der Benutzer die Unterhaltung beenden oder weitere Anweisungen zum Fortsetzen der Unterhaltung angeben möchte. 

Wenn Sie die Absicht **None** leer lassen, wird eine Äußerung, die außerhalb der Motivdomäne vorhergesagt werden sollte, in einer der vorhandenen Motivdomänenabsichten vorhergesagt. Das Ergebnis ist, dass die Clientanwendung, z. B. ein Chatbot, auf der Grundlage einer falschen Vorhersage falsche Vorgänge ausführt. 

1. Klicken Sie im linken Bereich auf **Intents** (Absichten).

1. Wählen Sie die Absicht **None** (Keine) aus. Fügen Sie drei Äußerungen hinzu, die der Benutzer unter Umständen eingibt, aber die für Ihre App nicht relevant sind:

    |`None` Beispieläußerungen|
    |--|
    |`Barking dogs are annoying`|
    |`Order a pizza for me`|
    |`Penguins in the ocean`|
