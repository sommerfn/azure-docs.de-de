---
title: 'Sprachkonzepte: QnA Maker'
titleSuffix: Azure Cognitive Services
description: QnA Maker unterstützt Wissensdatenbank-Inhalte in mehreren Sprachen. Allerdings sollte für jeden QnA Maker-Dienst eine einzelne Sprache festgelegt werden. Die erste Wissensdatenbank, die für einen bestimmten QnA Maker-Dienst erstellt wurde, bestimmt die Sprache dieses Diensts.
services: cognitive-services
author: diberry
manager: nitinme
ms.service: cognitive-services
ms.subservice: qna-maker
ms.topic: conceptual
ms.date: 09/24/2019
ms.author: diberry
ms.openlocfilehash: a818d84bf3df712190ccc6c1721c26933a9fbcdb
ms.sourcegitcommit: 018e3b40e212915ed7a77258ac2a8e3a660aaef8
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 11/07/2019
ms.locfileid: "73793938"
---
# <a name="language-support-of-knowledge-base-content-for-qna-maker"></a>Sprachunterstützung für Inhalte in QnA Maker-Wissensdatenbanken

Die Sprache für den Dienst ist ausgewählt, wenn Sie die erste Wissensdatenbank in der Ressource erstellen. Alle weiteren Wissensdatenbanken in der Ressource müssen die gleiche Sprache aufweisen. 

Die Sprache bestimmt die Relevanz der Ergebnisse, die QnA Maker als Antwort auf Benutzerabfragen bereitstellt.

## <a name="one-language-for-all-knowledge-bases-in-resource"></a>Eine Sprache für alle Wissensdatenbanken in der Ressource

Mit QnA Maker können Sie die Sprache für Ihren QnA-Dienst beim Erstellen der ersten Wissensdatenbank auswählen. Alle Wissensdatenbanken in einer QnA Maker-Ressource müssen in der gleichen Sprache vorliegen. Diese Sprache kann nicht geändert werden.

Das Erstellen von Wissensdatenbanken in verschiedenen Sprachen in einer Ressource wirkt sich nachteilig auf die Relevanz der Ergebnisse aus, die QnA Maker als Antwort auf Benutzeranfragen bereitstellt.

Sehen Sie sich die Liste der [unterstützten Sprachen](../overview/language-support.md#languages-supported) an und wie Sprachen sich auf [Zuordnung und Relevanz](#query-matching-and-relevance) auswirken. 

## <a name="select-language-when-creating-first-knowledge-base"></a>Auswählen der Sprache beim Erstellen der ersten Wissensdatenbank

Die Wahl der Sprache ist Teil der Schritte zur Erstellung der ersten Wissensdatenbank in einer Ressource. 

![Screenshot des QnA Maker-Portals zum Auswählen der Sprache für die erste Wissensdatenbank](../media/language-support/select-language-when-creating-knowledge-base.png)

## <a name="query-matching-and-relevance"></a>Abfrageabgleich und Relevanz
QnA Maker nutzt [Sprachanalysefunktionen von Azure Cognitive Search](https://docs.microsoft.com/rest/api/searchservice/language-support), um Ergebnisse bereitzustellen. 

Während die Azure Cognitive Search-Funktionen für unterstützte Sprachen ebenbürtig sind, verfügt QnA Maker über ein zusätzliches Rangfolgemodul, das oberhalb der Azure-Suchergebnisse ansetzt. In diesem Rangfolgemodul verwenden wir einige besondere semantische und wortbasierte Funktionen in den folgenden Sprachen. 

|Sprachen mit zusätzlichem Rangfolgemodul|
|--|
|Chinesisch|
|Tschechisch|
|Niederländisch|
|Deutsch|
|Französisch|
|Deutsch|
|Ungarisch|
|Italienisch|
|Japanisch|
|Koreanisch|
|Polnisch|
|Portugiesisch|
|Spanisch|
|Schwedisch|

Diese zusätzliche Rangbewertung ist zählt zu den internen Mechanismen des Rangfolgemoduls von QnA Maker.

## <a name="verify-language"></a>Überprüfen der Sprache

Sie können die Sprache Ihrer QnA Maker-Ressource auf der Seite „Diensteinstellungen“ in QnA Maker überprüfen.

![Screenshot der Seite „Diensteinstellungen“ im QnA Maker Portal](../media/language-support/language-knowledge-base.png) 


## <a name="next-steps"></a>Nächste Schritte

> [!div class="nextstepaction"]
> [Erstellen eines QnA-Bots mit Azure Bot Service](../Tutorials/create-qna-bot.md)
