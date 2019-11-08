---
title: Der Skill „Text teilen“
titleSuffix: Azure Cognitive Search
description: Teilen Sie Text in mehrere Blöcke oder Seiten anhand der Länge in einer KI-Anreicherungspipeline in der kognitiven Azure-Suche auf.
manager: nitinme
author: luiscabrer
ms.author: luisca
ms.service: cognitive-search
ms.topic: conceptual
ms.date: 11/04/2019
ms.openlocfilehash: 3f80169808b1e6420f04b786d2bb06bde9c96231
ms.sourcegitcommit: c22327552d62f88aeaa321189f9b9a631525027c
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 11/04/2019
ms.locfileid: "73479666"
---
# <a name="text-split-cognitive-skill"></a>Der Skill „Text teilen“

Der Skill **Text teilen** unterteilt den Text in Blöcke. Sie können festlegen, ob Sie den Text in Sätze oder in Seiten einer bestimmten Länge aufteilen möchten. Dieser Skill ist besonders nützlich, wenn Sie eine maximale Textlänge in nachfolgenden Skills einhalten müssen. 

> [!NOTE]
> Dieser Skill ist nicht an eine Cognitive Services-API gebunden, und Ihnen entstehen für seine Nutzung keine Kosten. Es wird jedoch empfohlen, dennoch eine [Cognitive Services-Ressource anzufügen](cognitive-search-attach-cognitive-services.md), um die Ressourcenoption **Free** außer Kraft zu setzen, durch die Sie auf eine geringe Anzahl von Anreicherungen pro Tag beschränkt werden.

## <a name="odatatype"></a>@odata.type  
Microsoft.Skills.Text.SplitSkill 

## <a name="skill-parameters"></a>Skillparameter

Bei den Parametern wird zwischen Groß- und Kleinschreibung unterschieden.

| Parametername     | BESCHREIBUNG |
|--------------------|-------------|
| textSplitMode      | Entweder „Seiten“ oder „Sätze“ | 
| maximumPageLength | Wenn „textSplitMode“ auf „Seiten“ gesetzt ist, bezieht sich dies auf die maximale Seitenlänge, gemessen durch `String.Length`. Der Mindestwert ist 100.  Wenn textSplitMode auf „pages“ festgelegt ist, versucht der Algorithmus, den Text in Blöcke aufzuteilen, deren Größe höchstens „maximumPageLenth“ beträgt. Dabei versucht der Algorithmus, Sätze an Satzgrenzen zu teilen, sodass die Größe der Blöcke etwas kleiner als „maximumPageLength“ sein kann. | 
| defaultLanguageCode   | (Optional) Einer der folgenden Sprachcodes: `da, de, en, es, fi, fr, it, ko, pt`. Die Standardsprache ist Englisch (en). Zu beachtende Aspekte:<ul><li>Wenn Sie ein Sprachcode-Ländercode-Format übergeben, wird nur der Sprachcodeteil des Formats verwendet.</li><li>Ist die Sprache nicht in der obigen Liste aufgeführt, wird der Text durch der Skill „Text teilen“ an Zeichengrenzen getrennt.</li><li>Die Angabe eines Sprachcodes ist sinnvoll, um bei Sprachen ohne Leerzeichen wie Chinesisch, Japanisch oder Koreanisch zu vermeiden, dass ein Wort in der Mitte getrennt wird.</li><li>Wenn Sie die Sprache nicht kennen (d. h., Sie müssen den Text für die Eingabe in [LanguageDetectionSkill](cognitive-search-skill-language-detection.md) teilen), sollte der Standardwert „English (en)“ ausreichen. </li></ul>  |


## <a name="skill-inputs"></a>Skilleingaben

| Parametername       | BESCHREIBUNG      |
|----------------------|------------------|
| text  | Der Text, der in Teilzeichenfolgen aufgeteilt werden soll. |
| languageCode  | (Optional) Der Sprachcode für das Dokument. Wenn Sie die Sprache nicht kennen (d. h., Sie müssen den Text für die Eingabe in [LanguageDetectionSkill](cognitive-search-skill-language-detection.md) teilen), können Sie diese Eingabe sicher entfernen.  |

## <a name="skill-outputs"></a>Skillausgaben 

| Parametername     | BESCHREIBUNG |
|--------------------|-------------|
| textItems | Ein Array von Teilzeichenfolgen, die extrahiert wurden. |


##  <a name="sample-definition"></a>Beispieldefinition

```json
{
    "@odata.type": "#Microsoft.Skills.Text.SplitSkill",
    "textSplitMode" : "pages", 
    "maximumPageLength": 1000,
    "defaultLanguageCode": "en",
    "inputs": [
        {
            "name": "text",
            "source": "/document/content"
        },
        {
            "name": "languageCode",
            "source": "/document/language"
        }
    ],
    "outputs": [
        {
            "name": "textItems",
            "targetName": "mypages"
        }
    ]
}
```

##  <a name="sample-input"></a>Beispieleingabe

```json
{
    "values": [
        {
            "recordId": "1",
            "data": {
                "text": "This is a the loan application for Joe Romero, a Microsoft employee who was born in Chile and who then moved to Australia…",
                "languageCode": "en"
            }
        },
        {
            "recordId": "2",
            "data": {
                "text": "This is the second document, which will be broken into several pages...",
                "languageCode": "en"
            }
        }
    ]
}
```

##  <a name="sample-output"></a>Beispielausgabe

```json
{
    "values": [
        {
            "recordId": "1",
            "data": {
                "textItems": [
                    "This is the loan…",
                    "On the second page we…"
                ]
            }
        },
        {
            "recordId": "2",
            "data": {
                "textItems": [
                    "This is the second document...",
                    "On the second page of the second doc…"
                ]
            }
        }
    ]
}
```

## <a name="error-cases"></a>Auftretende Fehler
Wird eine Sprache nicht unterstützt, wird eine Warnung generiert und der Text an den Zeichengrenzen geteilt.

## <a name="see-also"></a>Weitere Informationen

+ [Integrierte Qualifikationen](cognitive-search-predefined-skills.md)
+ [Definieren eines Skillsets](cognitive-search-defining-skillset.md)
