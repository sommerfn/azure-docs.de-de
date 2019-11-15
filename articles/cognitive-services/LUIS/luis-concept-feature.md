---
title: Features – LUIS
titleSuffix: Azure Cognitive Services
description: Fügen Sie einem Sprachmodell Features hinzu, um Hinweise zur Erkennung von Eingaben, die Sie bezeichnen oder klassifizieren möchten, bereitzustellen.
services: cognitive-services
author: diberry
manager: nitinme
ms.custom: seodec18
ms.service: cognitive-services
ms.subservice: language-understanding
ms.topic: conceptual
ms.date: 11/03/2019
ms.author: diberry
ms.openlocfilehash: 0cab6eb38459a632f1e7bd1a21e6a7251d33f683
ms.sourcegitcommit: 359930a9387dd3d15d39abd97ad2b8cb69b8c18b
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 11/06/2019
ms.locfileid: "73647415"
---
# <a name="machine-learned-features"></a>Durch maschinelles Lernen erworbene Features 

Beim maschinellen Lernen ist ein *Feature* ein eindeutiges Merkmal oder Attribut der Daten, die Ihr System untersucht oder durch die es lernt. In Language Understanding (LUIS) beschreibt und erläutert ein Feature, was an Ihren Absichten und Entitäten wichtig ist.

Im [LUIS-Vorschauportal](https://preview.luis.ai) handelt es sich bei Features um _Deskriptoren_, da sie zum _Beschreiben_ der Absicht oder Entität verwendet werden.  

## <a name="features-_descriptors_-in-language-understanding"></a>Features (_Deskriptoren_) in Language Understanding

Features, die auch als Deskriptoren bezeichnet werden, beschreiben Hinweise, um Language Understanding das Identifizieren der Beispieläußerungen zu erleichtern. Folgende Features sind enthalten: 

* Ausdruckslisten als Features für Absichten oder Entitäten
* Entitäten als Features für Absichten oder Entitäten

Features sollten zum Zweck der Modellaufschlüsselung als notwendiger Teil Ihres Schemas angesehen werden. 

## <a name="what-is-a-phrase-list"></a>Was ist eine Ausdrucksliste

Eine Ausdrucksliste ist eine Liste von Wörtern, Ausdrücken, Zahlen oder anderen Zeichen, die bei der Erkennung des Konzepts helfen, das Sie zu erkennen versuchen. In der Liste wird Groß-/Kleinschreibung unterschieden. 

## <a name="when-to-use-a-phrase-list"></a>Wann Sie Ausdruckslisten verwenden

Bei einer Ausdrucksliste berücksichtigt LUIS den Kontext und verallgemeinert, um Elemente zu erkennen, die einander ähnlich sind, ohne eine genaue textliche Übereinstimmung darzustellen. Wenn Sie Ihre LUIS-App in der Lage sein soll, zu generalisieren und neue Elemente zu identifizieren, verwenden Sie eine Ausdrucksliste. 

Wenn Sie neue Instanzen erkennen möchten, z. B. einen Besprechungsplan, bei dem auch die Namen neuer Kontakte ermittelt werden, oder eine Inventur-App, bei der neue Produkte ermittelt werden sollen, beginnen Sie mit einer durch maschinelles Lernen erworbenen Entität. Erstellen Sie dann eine Ausdrucksliste, die LUIS hilft, Wörter mit ähnlicher Bedeutung zu finden. Diese Ausdrucksliste führt LUIS beim Erkennen von Beispielen, indem sie dem Wert dieser Wörter zusätzliche Wichtigkeit verleiht. 

Ausdruckslisten ähneln einem domänenspezifischen Vokabular, das zur Verbesserung des Verständnisses von Absichten und Entitäten beiträgt. 

## <a name="considerations-when-using-a-phrase-list"></a>Wichtige Aspekte bei Verwendung einer Ausdrucksliste

Eine Ausdrucksliste wird standardmäßig auf alle Modelle in der App angewendet. Dies funktioniert bei Ausdruckslisten, die alle Absichten und Entitäten übergreifen können. Zum Zweck der Aufschlüsselung sollten Sie eine Ausdrucksliste nur auf die Modelle anwenden, für die sie relevant ist. 

Wenn Sie eine Ausdrucksliste erstellen (standardmäßig global erstellt) und sie dann später als Deskriptor (Feature) auf ein bestimmtes Modell anwenden, wird sie aus den anderen Modellen entfernt. Diese Entfernung verleiht der Ausdruckliste für das Modell, auf das sie angewendet wird, Wichtigkeit, und hilft, die Genauigkeit, die sie im Modell bietet, zu verbessern. 

Das `enabledForAllModels`-Flag steuert diesen Modellbereich in der API. 

<a name="how-to-use-phrase-lists"></a>

### <a name="how-to-use-a-phrase-list"></a>Verwenden von Ausdruckslisten

[Erstellen Sie eine Ausdrucksliste](luis-how-to-add-features.md), wenn Ihre Absicht oder Entität wichtige Wörter oder Ausdrücke enthält, wie etwa:

* Branchenbegriffe
* Jargon
* Abkürzungen
* Unternehmensspezifische Sprache
* Ausdrücke in einer anderen Sprache, die in Ihrer App jedoch häufig verwendet werden
* Schlüsselwörter und -ausdrücke in Ihren Beispieläußerungen

Fügen Sie **nicht** jedes denkbare Wort oder jeden denkbaren Ausdruck hinzu. Fügen Sie jeweils einige Wörter oder Ausdrücke hinzu, trainieren Sie erneut, und veröffentlichen Sie. Wenn die Liste Ihrer App mit der Zeit anwächst, können einige Ausdrücke möglicherweise verschiedene Formen (Synonyme) aufweisen. Gliedern Sie diese in eine weitere Liste aus. 

<a name="phrase-lists-help-identify-simple-exchangeable-entities"></a>

## <a name="when-to-use-an-entity-as-a-feature"></a>Wann eine Entität als Feature verwendet werden soll 

Eine Entität kann auf der Absichts- oder der Entitätsebene als Feature hinzugefügt werden. 

### <a name="entity-as-a-feature-to-an-intent"></a>Entität als Feature einer Absicht

Fügen Sie eine Entität einer Absicht als Deskriptor (Feature) hinzu, wenn die Erkennung dieser Entität für die Absicht wichtig ist.

Wenn die Absicht beispielsweise das Buchen eines Fluges und die Entität die Ticketinformationen sind (etwa die Anzahl der Sitze, Abflugort und Ankunftsort), dann würde das Auffinden der Entität mit den Ticketinformationen der Vorhersage der Flugbuchungsabsicht größeres Gewicht verleihen. 

### <a name="entity-as-a-feature-to-another-entity"></a>Entität als Feature einer anderen Entität

Eine Entität (A) sollte einer anderen Entität (B) als Feature hinzugefügt werden, wenn die Erkennung dieser Entität (A) für (B) wichtig ist.

Wenn beispielsweise die Straßenanschriftsentität (A) erkannt wird, dann verleiht das Auffinden der Straßenanschrift (A) der Vorhersage für die Versandadressentität (B) zusätzliches Gewicht. 

## <a name="best-practices"></a>Bewährte Methoden
Informationen zu [Best Practices](luis-concept-best-practices.md).

## <a name="next-steps"></a>Nächste Schritte

Weitere Informationen zum Hinzufügen von Features zu LUIS-Apps finden Sie unter [Hinzufügen von Features](luis-how-to-add-features.md).