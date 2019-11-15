---
title: Qualifikation „Textübersetzung“ der kognitiven Suche (Vorschau)
titleSuffix: Azure Cognitive Search
description: Wertet Text aus und gibt für jeden Datensatz Text zurück, der in einer KI-Anreicherungspipeline in Azure Cognitive Search in die angegebene Zielsprache übersetzt wurde. Diese Qualifikation ist zurzeit als öffentliche Vorschauversion verfügbar.
manager: nitinme
author: careyjmac
ms.author: chalton
ms.service: cognitive-search
ms.topic: conceptual
ms.date: 11/04/2019
ms.openlocfilehash: 7c42c9033fac057c12426726a96ae6079f3080da
ms.sourcegitcommit: bc7725874a1502aa4c069fc1804f1f249f4fa5f7
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 11/07/2019
ms.locfileid: "73715410"
---
#   <a name="text-translation-cognitive-skill"></a>Die Qualifikation „Textübersetzung“ der kognitiven Suche

> [!IMPORTANT] 
> Diese Qualifikation ist zurzeit als öffentliche Vorschauversion verfügbar. Die Vorschaufunktion wird ohne Vereinbarung zum Servicelevel bereitgestellt und ist nicht für Produktionsworkloads vorgesehen. Weitere Informationen finden Sie unter [Zusätzliche Nutzungsbestimmungen für Microsoft Azure-Vorschauen](https://azure.microsoft.com/support/legal/preview-supplemental-terms/). Previewfunktionen werden von der [REST-API-Version 2019-05-06-Preview](search-api-preview.md) bereitgestellt. Die Portalunterstützung ist momentan eingeschränkt, und das .NET SDK wird nicht unterstützt.

Die Qualifikation **Textübersetzung** wertet Text aus und gibt für jeden Datensatz Text zurück, der in die angegebene Zielsprache übersetzt wurde. Diese Qualifikation verwendet die in Cognitive Services verfügbare [Textübersetzungs-API Version 3.0](https://docs.microsoft.com/azure/cognitive-services/translator/reference/v3-0-translate).

Diese Funktion ist nützlich, wenn Sie davon ausgehen, dass nicht alle Ihre Dokumente in einer einzigen Sprache vorliegen. In diesem Fall können Sie den Text vor der Indizierung für die Suche in eine einzelne Sprache normalisieren, indem Sie ihn übersetzen.  Sie ist auch für Lokalisierungsanwendungsfälle nützlich, in denen Sie möglicherweise Kopien desselben Texts in mehreren Sprachen wünschen.

Die [Textübersetzungs-API 3.0](https://docs.microsoft.com/azure/cognitive-services/translator/reference/v3-0-reference) ist ein nicht regionaler Cognitive Service, was bedeutet, dass Ihre Daten nicht garantiert in der Region bleiben, in der sich Ihre Azure Cognitive Search-Instanz oder Ihre angefügte Cognitive Services-Ressource befindet.

> [!NOTE]
> Wenn Sie den Umfang erweitern, indem Sie die Verarbeitungsfrequenz erhöhen oder weitere Dokumente oder KI-Algorithmen hinzufügen, müssen Sie [eine kostenpflichtige Cognitive Services-Ressource anfügen](cognitive-search-attach-cognitive-services.md). Gebühren fallen beim Aufrufen von APIs in Cognitive Services sowie für die Bildextraktion im Rahmen der Dokumententschlüsselungsphase in Azure Cognitive Search an. Für die Textextraktion aus Dokumenten fallen keine Gebühren an.
>
> Die Ausführung integrierter Qualifikationen wird nach dem bestehenden [nutzungsbasierten Preis für Cognitive Services](https://azure.microsoft.com/pricing/details/cognitive-services/) berechnet. Die Preise für die Bildextraktion sind in der [Preisübersicht für Azure Cognitive Search](https://go.microsoft.com/fwlink/?linkid=2042400) angegeben.

## <a name="odatatype"></a>@odata.type  
Microsoft.Skills.Text.TranslationSkill

## <a name="data-limits"></a>Datengrenzwerte
Die maximale Größe eines Datensatzes ist 50.000 Zeichen, gemessen durch [`String.Length`](https://docs.microsoft.com/dotnet/api/system.string.length). Wenn Sie Ihre Daten teilen müssen, bevor Sie sie an die Übersetzungsqualifikation senden, denken Sie daran, die [Qualifikation „Text teilen“](cognitive-search-skill-textsplit.md) zu verwenden.

## <a name="skill-parameters"></a>Skillparameter

Bei den Parametern wird zwischen Groß- und Kleinschreibung unterschieden.

| Eingaben                | BESCHREIBUNG |
|---------------------|-------------|
| defaultToLanguageCode | (Erforderlich) Der Sprachcode, in den Dokumente übersetzt werden für Dokumente, in denen die Zielprache nicht explizit angegeben ist. <br/> Siehe die [vollständige Liste der unterstützten Sprachen](https://docs.microsoft.com/azure/cognitive-services/translator/language-support). |
| defaultFromLanguageCode | (Optional) Der Sprachcode, aus dem Dokumente übersetzt werden für Dokumente, in denen die Ausgangssprache nicht explizit angegeben ist.  Wenn defaultFromLanguageCode nicht angegeben wird, wird die von der Textübersetzungs-API bereitgestellte automatische Spracherkennung verwendet, um die Ausgangssprache zu bestimmen. <br/> Siehe die [vollständige Liste der unterstützten Sprachen](https://docs.microsoft.com/azure/cognitive-services/translator/language-support). |
| suggestedFrom | (Optional) Der Sprachcode, aus dem Dokumente übersetzt werden, wenn weder die fromLanguageCode-Eingabe noch der defaultFromLanguageCode-Parameter bereitgestellt wird, und die automatische Spracherkennung nicht erfolgreich ist.  Wenn die suggestedFrom-Sprache nicht festgelegt wird, wird Englisch (en) als suggestedFrom-Sprache verwendet. <br/> Siehe die [vollständige Liste der unterstützten Sprachen](https://docs.microsoft.com/azure/cognitive-services/translator/language-support). |

## <a name="skill-inputs"></a>Skilleingaben

| Eingabename     | BESCHREIBUNG |
|--------------------|-------------|
| text | Der zu übersetzende Text.|
| toLanguageCode    | Eine Zeichenfolge, die die Sprache angibt, in die der Text übersetzt werden sollte. Wenn diese Eingabe nicht angegeben wird, wird der Text mit defaultToLanguageCode übersetzt. <br/>Siehe die [vollständige Liste der unterstützten Sprachen](https://docs.microsoft.com/azure/cognitive-services/translator/language-support).|
| fromLanguageCode  | Eine Zeichenfolge, die die aktuelle Sprache des Texts angibt. Wenn dieser Parameter nicht angegeben wird, wird defaultFromLanguageCode (oder die automatische Spracherkennung, wenn defaultFromLanguageCode nicht angegeben wird) verwendet, um den Text zu übersetzen. <br/>Siehe die [vollständige Liste der unterstützten Sprachen](https://docs.microsoft.com/azure/cognitive-services/translator/language-support).|

## <a name="skill-outputs"></a>Skillausgaben

| Ausgabename    | BESCHREIBUNG |
|--------------------|-------------|
| translatedText | Das Zeichenfolgenergebnis der Textübersetzung von translatedFromLanguageCode in translatedToLanguageCode.|
| translatedToLanguageCode  | Eine Zeichenfolge, die den Sprachcode angibt, in den der Text übersetzt wurde. Nützlich, wenn Sie in mehrere Sprachen übersetzen und verfolgen möchten, welcher Text in welcher Sprache vorliegt.|
| translatedFromLanguageCode    | Eine Zeichenfolge, die den Sprachcode angibt, aus dem der Text übersetzt wurde. Nützlich, wenn Sie sich für die Option zur automatischen Spracherkennung entschieden haben, da diese Ausgabe das Ergebnis dieser Erkennung liefert.|

##  <a name="sample-definition"></a>Beispieldefinition

```json
 {
    "@odata.type": "#Microsoft.Skills.Text.TranslationSkill",
    "defaultToLanguageCode": "fr",
    "suggestedFrom": "en",
    "context": "/document",
    "inputs": [
      {
        "name": "text",
        "source": "/document/text"
      }
    ],
    "outputs": [
      {
        "name": "translatedText",
        "targetName": "translatedText"
      },
      {
        "name": "translatedFromLanguageCode",
        "targetName": "translatedFromLanguageCode"
      },
      {
        "name": "translatedToLanguageCode",
        "targetName": "translatedToLanguageCode"
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
      "data":
        {
          "text": "We hold these truths to be self-evident, that all men are created equal."
        }
    },
    {
      "recordId": "2",
      "data":
        {
          "text": "Estamos muy felices de estar con ustedes."
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
      "data":
        {
          "translatedText": "Nous tenons ces vérités pour évidentes, que tous les hommes sont créés égaux.",
          "translatedFromLanguageCode": "en",
          "translatedToLanguageCode": "fr"
        }
    },
    {
      "recordId": "2",
      "data":
        {
          "translatedText": "Nous sommes très heureux d'être avec vous.",
          "translatedFromLanguageCode": "es",
          "translatedToLanguageCode": "fr"
        }
    }
  ]
}
```


## <a name="errors-and-warnings"></a>Fehler und Warnungen
Wenn Sie einen nicht unterstützten Sprachcode für die Ausgangs- oder Zielsprache angeben, wird ein Fehler generiert, und der Text wird nicht übersetzt.
Wenn Ihr Text leer ist, wird eine Warnung angezeigt.
Wenn Ihr Text mehr als 50.000 Zeichen umfasst, werden nur die ersten 50.000 Zeichen übersetzt und eine Warnung ausgegeben.

## <a name="see-also"></a>Weitere Informationen

+ [Integrierte Qualifikationen](cognitive-search-predefined-skills.md)
+ [Definieren eines Skillsets](cognitive-search-defining-skillset.md)
