---
title: Grenzwerte – LUIS
titleSuffix: Azure Cognitive Services
description: Dieser Artikel behandelt die bekannten Einschränkungen von Azure Cognitive Services Language Understanding (LUIS). Für LUIS gelten verschiedene Einschränkungsbereiche. Die Modelleinschränkung steuert Absichten, Entitäten und Features in LUIS. Kontingentgrenzen basierend auf dem Schlüsseltyp. Tastenkombinationen steuern die LUIS-Website.
services: cognitive-services
author: diberry
manager: nitinme
ms.service: cognitive-services
ms.subservice: language-understanding
ms.topic: conceptual
ms.date: 11/04/2019
ms.author: diberry
ms.custom: seodec18
ms.openlocfilehash: 5a65ee27d5175887b7bf0d9146afa025e665657c
ms.sourcegitcommit: c22327552d62f88aeaa321189f9b9a631525027c
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 11/04/2019
ms.locfileid: "73488408"
---
# <a name="boundaries-for-your-luis-model-and-keys"></a>Grenzen für das LUIS-Modell und die Schlüssel
Für LUIS gelten verschiedene Einschränkungsbereiche. Der erste umfasst die [Modelleinschränkung](#model-boundaries), mit der Absichten, Entitäten und Features in LUIS gesteuert werden. Der zweite Bereich sind [Kontingentlimits](#key-limits), die auf dem Schlüsseltyp basieren. Ein dritter Bereich umfasst die [Tastenkombinationen](#keyboard-controls) zum Steuern der LUIS-Website. Ein vierter Bereich bezieht sich auf die [Zuordnung der Regionen weltweit](luis-reference-regions.md) zwischen der LUIS-Erstellungswebsite und den LUIS-[Endpunkt](luis-glossary.md#endpoint)-APIs. 


## <a name="model-boundaries"></a>Modelleinschränkungen

Wenn Ihre App die LUIS-Modellgrenzwerte und -grenzen überschreitet, sollten Sie eine [LUIS-Dispatch](luis-concept-enterprise.md#dispatch-tool-and-model)-App oder einen [LUIS-Container](luis-container-howto.md) verwenden. 

|Bereich|Begrenzung|
|--|:--|
| [App-Name][luis-get-started-create-app] | \* Standardzeichenlimit |
| ANWENDUNGEN| 500 Anwendungen pro Azure-Erstellungsressource |
| [Batchtests][batch-testing]| 10 Datasets, 1.000 Äußerungen pro Dataset|
| Explizite Listen | 50 pro Anwendung|
| Externe Entitäten | Keine Einschränkungen |
| [Absichten][intents]|500 pro Anwendung: 499 benutzerdefinierte Absichten und die erforderliche Absicht _None_.<br>[Dispatchbasierte](https://aka.ms/dispatch-tool) Anwendungen verfügen entsprechend über 500 Dispatchquellen.|
| [List-Entitäten](./luis-concept-entity-types.md) | Übergeordnet: 50, untergeordnet: 20.000 Elemente. Kanonischer Name mit * Standardzeichenlimit. Für Synonymwerte gelten keine Längenbeschränkungen. |
| [Durch maschinelles Lernen erworbene Entitäten und Rollen:](./luis-concept-entity-types.md)<br> Zusammengesetzt,<br>Einfach,<br>Entitätsrolle|Maximal 100 übergeordnete Entitäten oder 330 Entitäten, je nachdem, welches Limit der Benutzer zuerst erreicht. Eine Rolle zählt für diese Grenze als eine Entität. Ein Beispiel für eine zusammengesetzte Entität mit einer einfachen Entität, die über zwei Rollen verfügt: 1 zusammengesetzte + 1 einfache + 2 Rollen = 4 der 330 Entitäten.|
|Modell als Feature| Die maximale Anzahl von Modellen, die als Deskriptor (Feature) für ein bestimmtes Modell verwendet werden können, beträgt 10 Modelle. Die maximale Anzahl von Ausdruckslisten, die als Deskriptor (Feature) für ein bestimmtes Modell verwendet werden können, beträgt 10 Ausdruckslisten.|
| [Vorschau – Entitäten vom Typ „dynamische Liste“](https://aka.ms/luis-api-v3-doc#dynamic-lists-passed-in-at-prediction-time)|2 Listen mit ca. 1.000 pro Anforderung für den Endpunkt der Abfragevorhersage|
| [Muster](luis-concept-patterns.md)|500 Muster pro Anwendung.<br>Muster dürfen maximal 400 Zeichen lang sein.<br>3 Pattern.any-Entitäten pro Muster<br>Maximal 2 geschachtelte optionale Texte im Muster|
| [Pattern.any-Entitäten](./luis-concept-entity-types.md)|100 pro Anwendung, 3 Pattern.any-Entitäten pro Muster |
| [Begriffsliste][phrase-list]|500 Ausdruckslisten Nicht austauschbare Ausdrucksliste mit maximal 5.000 Ausdrücken. Austauschbare Ausdrucksliste mit maximal 50.000 Ausdrücken. Die maximale Anzahl von Ausdrücken pro Anwendung beträgt 500.000 Ausdrücke.|
| [Vordefinierte Entitäten](./luis-prebuilt-entities.md) | Keine Begrenzung|
| [RegEx-Entitäten](./luis-concept-entity-types.md)|20 Entitäten<br>max. 500 Zeichen pro RegEx-Entitätsmuster|
| [Rollen](luis-concept-roles.md)|300 Rollen pro Anwendung. 10 Rollen pro Entität|
| [Äußerung][utterances] | 500 Zeichen|
| [Äußerungen][utterances] | 15.000 pro Anwendung – es gibt keine Beschränkung für die Anzahl von Äußerungen pro Absicht|
| [Versionen](luis-concept-version.md)| 100 Versionen pro Anwendung |
| [Versionsname][luis-how-to-manage-versions] | 10 Zeichen, ausschließlich alphanumerische Zeichen und Punkt (.) |

\* Das Standardzeichenlimit beträgt 50 Zeichen. 

<a name="intent-and-entity-naming"></a>

## <a name="name-uniqueness"></a>Eindeutigkeit des Namens

Verwenden Sie die folgenden Regeln zur Eindeutigkeit bei der Benennung.

Folgendes muss innerhalb einer LUIS-App eindeutig sein:

* Versionsname
* Absicht
* Entität
* roles

Folgendes muss innerhalb des Gültigkeitsbereichs eindeutig sein:

* Ausdrucksliste 

## <a name="object-naming"></a>Benennung der Objekte

Verwenden Sie die folgenden Zeichen in den folgenden Namen.

|Object|Zeichen ausschließen|
|--|--|
|Absicht, Entität und Rollennamen|`:`<br>`$`|
|Versionsname|`\`<br> `/`<br> `:`<br> `?`<br> `&`<br> `=`<br> `*`<br> `+`<br> `(`<br> `)`<br> `%`<br> `@`<br> `$`<br> `~`<br> `!`<br> `#`|

## <a name="key-usage"></a>Schlüsselverwendung

Language Understanding verfügt über separate Schlüssel, einen Typ für die Erstellung und einen Typ für die Abfrage des Vorhersageendpunkts. Weitere Informationen zu den Unterschieden zwischen Schlüsseltypen finden Sie unter [Erstellungsschlüssel und Endpunktschlüssel für Vorhersageabfragen in LUIS](luis-concept-keys.md).

<a name="key-limits"></a>

## <a name="resource-key-limits"></a>Grenzen von Ressourcenschlüsseln

Für Ressourcenschlüssel gelten verschiedene Grenzwerte für die Verwendung beim Erstellen und auf Endpunkten. Der LUIS-Endpunktschlüssel für Vorhersageabfragen gilt ausschließlich für Endpunktabfragen. 

* 500 Anwendungen pro Azure-Erstellungsressource 

|Schlüssel|Erstellen|Endpunkt|Zweck|
|--|--|--|--|
|Starter|1 Mio./Monat, 5/Sekunde|1\.000/Monat, 5/Sekunde|Erstellen Ihrer LUIS-App|
|F0: Free-Tarif |1 Mio./Monat, 5/Sekunde|10.000/Monat, 5/Sekunde|Abfragen Ihres LUIS-Endpunkts|
|S0: Basic-Tarif|-|50/Sekunde|Abfragen Ihres LUIS-Endpunkts|
|S0: Standard-Tarif|-|50/Sekunde|Abfragen Ihres LUIS-Endpunkts|
|[Integration der Standpunktanalyse](luis-how-to-publish-app.md#enable-sentiment-analysis)|-|-|Das Hinzufügen von Informationen zur Stimmung, einschließlich Datenextraktion für Schlüsselausdrücke wird bereitgestellt, ohne eine weitere Azure-Ressource zu benötigen. |
|[Speech-Integration](../speech-service/how-to-recognize-intents-from-speech-csharp.md)|-|1\.000 Endpunktanforderungen pro Einheitenkosten|Konvertieren gesprochener Äußerungen in Textäußerungen und Zurückgeben der LUIS-Ergebnisse|

[Weitere Informationen zu Preisen][pricing]

## <a name="keyboard-controls"></a>Tastenkombinationen

|Tastatureingabe | BESCHREIBUNG | 
|--|--|
|STRG+E|Wechselt in der Liste der Äußerungen zwischen Token und Entitäten|

## <a name="website-sign-in-time-period"></a>Anmeldezeitraum für die Website

Ihr Anmeldezugriff gilt für **60 Minuten**. Nach diesem Zeitraum erhalten Sie diesen Fehler. Sie müssen sich erneut anmelden.

[luis-get-started-create-app]: https://docs.microsoft.com/azure/cognitive-services/luis/luis-get-started-create-app
[batch-testing]: https://docs.microsoft.com/azure/cognitive-services/luis/luis-concept-test#batch-testing
[intents]: https://docs.microsoft.com/azure/cognitive-services/luis/luis-concept-intent
[phrase-list]: https://docs.microsoft.com/azure/cognitive-services/luis/luis-concept-feature
[utterances]: https://docs.microsoft.com/azure/cognitive-services/luis/luis-concept-utterance
[luis-how-to-manage-versions]: https://docs.microsoft.com/azure/cognitive-services/luis/luis-how-to-manage-versions
[pricing]: https://azure.microsoft.com/pricing/details/cognitive-services/language-understanding-intelligent-services/
<!-- TBD: fix this link -->
[speech-to-intent-pricing]: https://azure.microsoft.com/pricing/details/cognitive-services/language-understanding-intelligent-services/
