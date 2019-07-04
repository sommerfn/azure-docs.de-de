---
title: include file
description: include file
services: cognitive-services
author: diberry
manager: cjgronlund
ms.service: cognitive-services
ms.subservice: luis
ms.topic: include
ms.custom: include file
ms.date: 12/21/2018
ms.author: diberry
ms.openlocfilehash: 355fe134939b26c51d6e03368f782845628a6b96
ms.sourcegitcommit: 3e98da33c41a7bbd724f644ce7dedee169eb5028
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 06/18/2019
ms.locfileid: "67178399"
---
Die Clientanwendung muss wissen, ob eine Äußerung für die Anwendung unsinnig oder ungeeignet ist. Die Absicht **None** (Keine) wird jeder Anwendung bei ihrer Erstellung hinzugefügt, um zu bestimmen, ob eine Äußerung nicht von der Clientanwendung beantwortet werden kann.

Wenn LUIS für eine Äußerung die Absicht **None** (Keine) zurückgibt, kann Ihre Clientanwendung fragen, ob der Benutzer die Unterhaltung beenden oder weitere Anweisungen zum Fortsetzen der Unterhaltung angeben möchte. 

> [!CAUTION] 
> Belassen Sie die Absicht **None** (Keine) nicht leer. 

1. Klicken Sie im linken Bereich auf **Intents** (Absichten).

2. Wählen Sie die Absicht **None** (Keine) aus. Fügen Sie drei Äußerungen hinzu, die der Benutzer unter Umständen eingibt, die für Ihre Personalwesen-App jedoch nicht relevant sind:

    | Beispiele für Äußerungen|
    |--|
    |Bellende Hunde sind nervig|
    |Bestell mir eine Pizza|
    |Pinguine im Ozean|
