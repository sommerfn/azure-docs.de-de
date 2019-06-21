---
title: Anwendungseinstellungen
titleSuffix: Azure Cognitive Services
description: Machen Sie sich mit den Anwendungseinstellungen für Language Understanding-Apps vertraut.
services: cognitive-services
author: diberry
manager: nitinme
ms.custom: seodec18
ms.service: cognitive-services
ms.subservice: language-understanding
ms.topic: article
ms.date: 05/29/2019
ms.author: diberry
ms.openlocfilehash: a8a1f6fe9372d013d310c557161ceb813b8a478b
ms.sourcegitcommit: 3e98da33c41a7bbd724f644ce7dedee169eb5028
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 06/17/2019
ms.locfileid: "67163636"
---
# <a name="application-settings"></a>Anwendungseinstellungen

Diese Anwendungseinstellungen werden in der [exportierten](https://westus.dev.cognitive.microsoft.com/docs/services/5890b47c39e2bb17b84a55ff/operations/5890b47c39e2bb052c5b9c40) App gespeichert und mit den REST-APIs [aktualisiert](https://westus.dev.cognitive.microsoft.com/docs/services/5890b47c39e2bb17b84a55ff/operations/versions-update-application-version-settings). Wenn Sie die Einstellungen Ihrer App-Version ändern, wird der Trainingsstatus Ihrer App auf „Untrainiert“ zurückgesetzt.

|Einstellung|Standardwert|Notizen|
|--|--|--|
|NormalizePunctuation|True|Entfernt die Interpunktion.|
|NormalizeDiacritics|True|Entfernt diakritische Zeichen.|

## <a name="diacritics-normalization"></a>Normalisierung von diakritischen Zeichen 

Aktivieren Sie die Äußerungsnormalisierung für diakritische Zeichen für Ihre LUIS-JSON-App-Datei im Parameter `settings`.

```JSON
"settings": [
    {"name": "NormalizeDiacritics", "value": "true"}
] 
```

Anhand der folgenden Äußerungen wird veranschaulicht, wie sich die Normalisierung von diakritischen Zeichen auf Äußerungen auswirkt:

|Einstellung „false“ für diakritische Zeichen|Einstellung „true“ für diakritische Zeichen|
|--|--|
|`quiero tomar una piña colada`|`quiero tomar una pina colada`|
|||

### <a name="language-support-for-diacritics"></a>Sprachunterstützung für diakritische Zeichen

#### <a name="brazilian-portuguese-pt-br-diacritics"></a>Diakritische Zeichen: Portugiesisch (Brasilien) `pt-br`

|Einstellung „false“ für diakritische Zeichen|Einstellung „false“ für diakritische Zeichen|
|-|-|
|`á`|`a`|
|`â`|`a`|
|`ã`|`a`|
|`à`|`a`|
|`ç`|`c`|
|`é`|`e`|
|`ê`|`e`|
|`í`|`i`|
|`ó`|`o`|
|`ô`|`o`|
|`õ`|`o`|
|`ú`|`u`| 
|||

#### <a name="dutch-nl-nl-diacritics"></a>Diakritische Zeichen: Niederländisch `nl-nl`

|Einstellung „false“ für diakritische Zeichen|Einstellung „false“ für diakritische Zeichen|
|-|-|
|`á`|`a`|
|`à`|`a`|
|`é`|`e`|
|`ë`|`e`|
|`è`|`e`|
|`ï`|`i`|
|`í`|`i`|
|`ó`|`o`|
|`ö`|`o`|
|`ú`|`u`| 
|`ü`|`u`|
|||

#### <a name="french-fr--diacritics"></a>Diakritische Zeichen: Französisch `fr-`

Gilt sowohl für den französischen als auch für den kanadischen Unterkulturbereich.

|Einstellung „false“ für diakritische Zeichen|Einstellung „false“ für diakritische Zeichen|
|--|--|
|`é`|`e`|
|`à`|`a`|
|`è`|`e`|
|`ù`|`u`|
|`â`|`a`| 
|`ê`|`e`| 
|`î`|`i`| 
|`ô`|`o`| 
|`û`|`u`| 
|`ç`|`c`| 
|`ë`|`e`| 
|`ï`|`i`| 
|`ü`|`u`| 
|`ÿ`|`y`| 

#### <a name="german-de-de-diacritics"></a>Diakritische Zeichen: Deutsch `de-de`

|Einstellung „false“ für diakritische Zeichen|Einstellung „false“ für diakritische Zeichen|
|--|--|
|`ä`|`a`|
|`ö`|`o`| 
|`ü`|`u`| 

#### <a name="italian-it-it-diacritics"></a>Diakritische Zeichen: Italienisch `it-it`

|Einstellung „false“ für diakritische Zeichen|Einstellung „false“ für diakritische Zeichen|
|--|--|
|`à`|`a`|
|`è`|`e`|
|`é`|`e`|
|`ì`|`i`| 
|`í`|`i`| 
|`î`|`i`| 
|`ò`|`o`| 
|`ó`|`o`| 
|`ù`|`u`|
|`ú`|`u`|

#### <a name="spanish-es--diacritics"></a>Diakritische Zeichen: Spanisch `es-`

Gilt sowohl für Spanisch als auch für Mexikanisch (Kanada).

|Einstellung „false“ für diakritische Zeichen|Einstellung „false“ für diakritische Zeichen|
|-|-|
|`á`|`a`|
|`é`|`e`|
|`í`|`i`| 
|`ó`|`o`| 
|`ú`|`u`|
|`ü`|`u`|
|`ñ`|`u`|


## <a name="punctuation-normalization"></a>Normalisierung der Interpunktion

Aktivieren Sie die Äußerungsnormalisierung für die Interpunktion für Ihre LUIS-JSON-App-Datei im Parameter `settings`.

```JSON
"settings": [
    {"name": "NormalizePunctuation", "value": "true"}
] 
```

Anhand der folgenden Äußerungen wird veranschaulicht, wie sich diakritische Zeichen auf Äußerungen auswirken:

|Einstellung „false“ für diakritische Zeichen|Einstellung „true“ für diakritische Zeichen|
|--|--|
|`Hmm..... I will take the cappuccino`|`Hmm I will take the cappuccino`|
|||

### <a name="punctuation-removed"></a>Entfernte Interpunktion

Die folgende Interpunktion wird entfernt, wenn `NormalizePunctuation` auf „true“ festgelegt ist.

|Interpunktion|
|--|
|`-`| 
|`.`| 
|`'`|
|`"`|
|`\`|
|`/`|
|`?`|
|`!`|
|`_`|
|`,`|
|`;`|
|`:`|
|`(`|
|`)`|
|`[`|
|`]`|
|`{`|
|`}`|
|`+`|
|`¡`|
