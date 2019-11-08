---
title: Dokumentationslinks zur KI-Anreicherung
titleSuffix: Azure Cognitive Search
description: Eine kommentierte Liste von Artikeln, Tutorials, Beispielen und Blogbeiträgen zu Workloads der KI-Anreicherung in Azure Cognitive Search.
manager: nitinme
author: HeidiSteen
ms.author: heidist
ms.service: cognitive-search
ms.topic: quickstart
ms.date: 11/04/2019
ms.openlocfilehash: cf0332909e053a0875d41a00d15f196b193ba15e
ms.sourcegitcommit: c22327552d62f88aeaa321189f9b9a631525027c
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 11/04/2019
ms.locfileid: "73466716"
---
# <a name="documentation-resources-for-ai-enrichment-in-azure-cognitive-search"></a>Dokumentationressourcen für die KI-Anreicherung in Azure Cognitive Search

Die KI-Anreicherung ist eine Funktion im Azure Cognitive Search-Index, die latente Informationen in Nicht-Textquellen und undifferenziertem Text findet und sie in durchsuchbare Volltextinhalte in Azure Cognitive Search umwandelt.

Die folgenden Artikel sind die vollständige Dokumentation zur KI-Anreicherung.

## <a name="getting-started"></a>Erste Schritte
+ [Einführung in die KI in Azure Cognitive Search](cognitive-search-concept-intro.md)
+ [Schnellstart: Erstellen eines kognitiven Skillsets im Azure-Portal](cognitive-search-quickstart-blob.md)
+ [Tutorial: Angereicherte Indizierung mit KI](cognitive-search-tutorial-blob.md)
+ [Beispiel: Erstellen eines benutzerdefinierten Skills für die KI-Anreicherung](cognitive-search-create-custom-skill-example.md)

## <a name="how-to-guidance"></a>Leitfäden zur Vorgehensweise
+ [Definieren eines Skillsets](cognitive-search-defining-skillset.md)
+ [Verweisen auf Anmerkungen in einem Skillset](cognitive-search-concept-annotations-syntax.md)
+ [Zuordnen von Feldern zu einem Index](cognitive-search-output-field-mapping.md)
+ [Verarbeiten und Extrahieren von Informationen aus Bildern](cognitive-search-concept-image-scenarios.md)
+ [Neuerstellen eines Azure Cognitive Search-Indexes](search-howto-reindex.md)
+ [Definieren einer benutzerdefinierten Skillsschnittstelle](cognitive-search-custom-skill-interface.md)
+ [Tipps zur Problembehandlung](cognitive-search-concept-troubleshooting.md)

## <a name="reference"></a>Verweis

+ [Integrierte Qualifikationen](cognitive-search-predefined-skills.md)
  + [Microsoft.Skills.Text.KeyPhraseExtractionSkill](cognitive-search-skill-keyphrases.md)
  + [Microsoft.Skills.Text.LanguageDetectionSkill](cognitive-search-skill-language-detection.md)
  + [Microsoft.Skills.Text.EntityRecognitionSkill](cognitive-search-skill-entity-recognition.md)
  + [Microsoft.Skills.Text.MergeSkill](cognitive-search-skill-textmerger.md)
  + [Microsoft.Skills.Text.SplitSkill](cognitive-search-skill-textsplit.md)
  + [Microsoft.Skills.Text.SentimentSkill](cognitive-search-skill-sentiment.md)
  + [Microsoft.Skills.Text.TranslationSkill](cognitive-search-skill-text-translation.md)
  + [Microsoft.Skills.Vision.ImageAnalysisSkill](cognitive-search-skill-image-analysis.md)
  + [Microsoft.Skills.Vision.OcrSkill](cognitive-search-skill-ocr.md)
  + [Microsoft.Skills.Util.ConditionalSkill](cognitive-search-skill-conditional.md)
  + [Microsoft.Skills.Util.DocumentExtractionSkill](cognitive-search-skill-document-extraction.md)
  + [Microsoft.Skills.Util.ShaperSkill](cognitive-search-skill-shaper.md)

+ Benutzerdefinierte Qualifikationen
  + [Microsoft.Skills.Custom.WebApiSkill](cognitive-search-custom-skill-web-api.md)

+ [Veraltete Skills](cognitive-search-skill-deprecated.md)
  + [Microsoft.Skills.Text.NamedEntityRecognitionSkill](cognitive-search-skill-named-entity-recognition.md)

+ [REST-API](https://docs.microsoft.com/rest/api/searchservice/)
  + [Erstellen einer Qualifikationsgruppe (API-Version 2019-05-06)](https://docs.microsoft.com/rest/api/searchservice/create-skillset)
  + [Erstellen eines Indexers (API-Version 2019-05-06)](https://docs.microsoft.com/rest/api/searchservice/create-indexer)

## <a name="see-also"></a>Weitere Informationen

+ [Azure Cognitive Search-REST-API](https://docs.microsoft.com/rest/api/searchservice/)
+ [Indexer in Azure Cognitive Search](search-indexer-overview.md)
+ [Was ist Azure Cognitive Search?](search-what-is-azure-search.md)
