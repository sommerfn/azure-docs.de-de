---
title: 'Begriffslisten: LUIS'
titleSuffix: Azure Cognitive Services
description: Verwenden Sie Language Understanding (LUIS), um App-Features hinzuzufügen, die die Erkennung oder Vorhersage von Absichten und Entitäten verbessern können, die Kategorien und Muster
services: cognitive-services
author: diberry
manager: nitinme
ms.custom: seodec18
ms.service: cognitive-services
ms.subservice: language-understanding
ms.topic: conceptual
ms.date: 11/03/2019
ms.author: diberry
ms.openlocfilehash: 0e3e4226eaaa0505eea96d8b3aca820f2327349e
ms.sourcegitcommit: c22327552d62f88aeaa321189f9b9a631525027c
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 11/04/2019
ms.locfileid: "73467618"
---
# <a name="use-phrase-lists-to-boost-signal-of-word-list"></a>Verwenden von Begriffslisten, um das Signal von Wortlisten zu verstärken

Sie können Ihrer LUIS-App Features hinzufügen, um ihre Genauigkeit zu verbessern. Features unterstützen LUIS mit Hinweisen, dass bestimmte Wörter und Ausdrücke Teil eines App-Domänenvokabulars sind. 

Eine [Ausdrucksliste](luis-concept-feature.md) enthält eine Gruppe von Werten (Wörter oder Ausdrücke), die derselben Klasse angehören und auf ähnliche Weise behandelt werden sollen (z.B. Städte- oder Produktnamen). Erkenntnisse, die LUIS zu einem davon sammelt, werden automatisch auch auf die anderen angewandt. Diese Liste ist nicht dasselbe wie eine [Listenentität](reference-entity-list.md) (genaue Textübereinstimmungen) der übereinstimmenden Wörter.

Eine Ausdrucksliste ist Teil des Vokabulars der App-Domäne als zweiter Hinweis zu diesen Wörtern für LUIS.

Lesen Sie [Featurekonzepte](luis-concept-feature.md), um zu verstehen, wann und warum eine Ausdrucksliste verwendet werden soll. 

[!INCLUDE [Waiting for LUIS portal refresh](./includes/wait-v3-upgrade.md)]

## <a name="add-phrase-list"></a>Hinzufügen einer Begriffsliste

LUIS ermöglicht bis zu 10 Begriffslisten pro App. 

1. Öffnen Sie die App, indem Sie auf der Seite **Meine Apps** auf ihren Namen, dann auf **Erstellen** und im linken Bereich Ihrer App auf **Begriffslisten** klicken. 

1. Klicken Sie auf der Seite **Begriffslisten** auf **Neue Begriffsliste erstellen**. 
 
1. Geben Sie im Dialogfeld **Begriffsliste hinzufügen** als Namen für die Begriffsliste `Cities` ein. Geben Sie im Feld **Wert** die Werte der Begriffsliste ein. Sie können jeweils einen Wert oder eine Gruppe durch Kommas getrennter Werte eingeben und dann die **EINGABETASTE** drücken.

    ![Hinzufügen der Begriffsliste „Cities“](./media/luis-add-features/add-phrase-list-cities.png)

1. LUIS kann vorschlagen, verwandte Werte Ihrer Begriffsliste hinzuzufügen. Klicken Sie auf **Empfehlen**, um eine Gruppe von vorgeschlagenen Werte abzurufen, die semantisch mit den hinzugefügten Werten verwandt sind. Sie können auf die einzelnen vorgeschlagenen Werte klicken, oder auf **Alle hinzufügen**, um alle hinzuzufügen.

    ![Begriffsliste – vorgeschlagene Werte – Alle hinzufügen](./media/luis-add-features/related-values.png)

1. Klicken Sie auf **Diese Werte sind austauschbar**, wenn die hinzugefügten Begriffslistenwerte Alternativen sind, die austauschbar verwendet werden können.

    ![Begriffsliste – vorgeschlagene Werte – Aktivieren des Kontrollkästchens „Austauschbar“](./media/luis-add-features/interchangeable.png)

1. Klicken Sie auf **Fertig**. Die Begriffsliste „Cities“ wird der Seite **Begriffslisten** hinzugefügt.

<a name="edit-phrase-list"></a>
<a name="delete-phrase-list"></a>
<a name="deactivate-phrase-list"></a>

> [!Note]
> Sie können eine Begriffsliste aus der kontextbezogenen Symbolleiste auf der Seite **Begriffslisten** löschen oder deaktivieren.

## <a name="next-steps"></a>Nächste Schritte

Nach dem Hinzufügen, Bearbeiten, Löschen oder Deaktivieren einer Begriffsliste [trainieren und testen Sie die App](luis-interactive-test.md) erneut, um festzustellen, ob die Leistung besser wurde.
