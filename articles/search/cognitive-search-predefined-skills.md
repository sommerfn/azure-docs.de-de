---
title: Integrierte Datenextraktion, natürliche Sprache, Bildverarbeitung
titleSuffix: Azure Cognitive Search
description: Durch kognitive Skills für Datenextraktion, natürliche Sprache und Bildverarbeitung erhält der Rohdateninhalt in einer Pipeline der kognitiven Azure-Suche Semantik und Struktur.
manager: nitinme
author: luiscabrer
ms.author: luisca
ms.service: cognitive-search
ms.topic: conceptual
ms.date: 11/04/2019
ms.openlocfilehash: 435635018dc25ed2af0aec3d542c0388af8ab885
ms.sourcegitcommit: b050c7e5133badd131e46cab144dd5860ae8a98e
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 10/23/2019
ms.locfileid: "72792098"
---
# <a name="built-in-cognitive-skills-for-content-enrichment-azure-cognitive-search"></a>Integrierte kognitive Skills für die Inhaltsanreicherung (kognitive Azure-Suche)

In diesem Artikel werden die kognitiven Skills beschrieben, die mit der kognitiven Azure-Suche bereitgestellt werden. Ein *kognitiver Skill* ist ein Vorgang, der Inhalte in irgendeiner Weise transformiert. Oft handelt es sich um eine Komponente, die Daten extrahiert oder die Struktur ableitet und Eingabedaten somit verständlicher macht. Die Ausgabe erfolgt fast immer textbasiert. Ein *Skillset* ist eine Sammlung von Skills, die die Anreicherungspipeline definieren. 

> [!NOTE]
> Wenn Sie den Umfang erweitern, indem Sie die Verarbeitungsfrequenz erhöhen oder weitere Dokumente oder KI-Algorithmen hinzufügen, müssen Sie [eine kostenpflichtige Cognitive Services-Ressource anfügen](cognitive-search-attach-cognitive-services.md). Gebühren fallen beim Aufrufen von APIs in Cognitive Services sowie für die Bildextraktion im Rahmen der Dokumentaufschlüsselungsphase in der kognitiven Azure-Suche an. Für die Textextraktion aus Dokumenten fallen keine Gebühren an.
>
> Die Ausführung integrierter Qualifikationen wird nach dem bestehenden [nutzungsbasierten Preis für Cognitive Services](https://azure.microsoft.com/pricing/details/cognitive-services/) berechnet. Die Preise für die Bildextraktion sind in der [Preisübersicht für die kognitive Azure-Suche](https://go.microsoft.com/fwlink/?linkid=2042400) angegeben.


## <a name="built-in-skills"></a>Integrierte Skills

Mehrere Skills sind flexibel in dem, was sie verwenden oder generieren. Im Allgemeinen basieren die meisten Skills auf vortrainierten Modellen, d.h. Sie können das Modell nicht mit Ihren eigenen Trainingsdaten trainieren. In der folgenden Tabelle werden die von Microsoft bereitgestellten Skills aufgelistet und beschrieben. 

| Skill | BESCHREIBUNG |
|-------|-------------|
| [Microsoft.Skills.Text.KeyPhraseSkill](cognitive-search-skill-keyphrases.md) | Dieser Skill verwendet ein vortrainiertes Modell, um wichtige Phrasen zu erkennen, die auf der Platzierung von Begriffen, sprachlichen Regeln, der Nähe zu anderen Begriffen und der Ungewöhnlichkeit des Begriffs innerhalb der Quelldaten basieren. |
| [Microsoft.Skills.Text.LanguageDetectionSkill](cognitive-search-skill-language-detection.md)  | Dieser Skill verwendet ein vortrainiertes Modell, um die verwendete Sprache zu erkennen (eine Sprachen-ID pro Dokument). Wenn mehrere Sprachen in den gleichen Textsegmenten verwendet werden, wird die LCID der vorwiegend verwendeten Sprache ausgegeben.|
| [Microsoft.Skills.Text.MergeSkill](cognitive-search-skill-textmerger.md) | Konsolidiert Text aus einer Sammlung von Feldern in einem einzigen Feld.  |
| [Microsoft.Skills.Text.EntityRecognitionSkill](cognitive-search-skill-entity-recognition.md) | Dieser Skill verwendet ein vortrainiertes Modell, um Entitäten für eine feste Gruppe von Kategorien zu erstellen: Personen, Ort, Organisation, E-Mails, URLs, datetime-Felder. |
| [Microsoft.Skills.Text.SentimentSkill](cognitive-search-skill-sentiment.md)  | Dieser Skill verwendet ein vortrainiertes Modell, um eine positive oder negative Stimmung anhand von Datensätzen zu bewerten. Der Wert liegt zwischen 0 und 1. Neutrale Bewertungen treten sowohl im Fall „0“ auf, wenn die Stimmung nicht erkannt werden kann, als auch für Text, der als neutral angesehen wird.  |
| [Microsoft.Skills.Text.SplitSkill](cognitive-search-skill-textsplit.md) | Teilt Text in Seiten auf, damit Sie Inhalt inkrementell anreichern oder erweitern können. |
| [Microsoft.Skills.Text.TranslationSkill](cognitive-search-skill-text-translation.md) | Dieser Skill verwendet ein vortrainiertes Modell, um den Eingabetext zur Normalisierung oder Lokalisierung in verschiedene Sprachen zu übersetzen. |
| [Microsoft.Skills.Vision.ImageAnalysisSkill](cognitive-search-skill-image-analysis.md) | Dieser Skill verwendet einen Bilderkennungsalgorithmus, um den Inhalt eines Bildes zu identifizieren und eine Textbeschreibung zu erzeugen. |
| [Microsoft.Skills.Vision.OcrSkill](cognitive-search-skill-ocr.md) | Optische Zeichenerkennung |
| [Microsoft.Skills.Util.ConditionalSkill](cognitive-search-skill-conditional.md) | Ermöglicht Filterung, Zuweisung eines Standardwerts und Zusammenführung von Daten auf der Grundlage einer Bedingung.|
| [Microsoft.Skills.Util.ShaperSkill](cognitive-search-skill-shaper.md) | Ordnet die Ausgabe einem komplexen Typ zu (ein mehrteiliger Datentyp, der für einen vollständigen Namen, eine mehrzeilige Adresse oder eine Kombination aus Nachname und persönlicher ID verwendet werden kann). |
| [Microsoft.Skills.Custom.WebApiSkill](cognitive-search-custom-skill-web-api.md) | Ermöglicht die Erweiterbarkeit einer KI-Anreicherungspipeline, indem ein HTTP-Aufruf in einer benutzerdefinierten Web-API ausgeführt wird. |


Eine Anleitung zum Erstellen einer [benutzerdefinierten Qualifikation](cognitive-search-custom-skill-web-api.md) finden Sie unter [Hinzufügen eines benutzerdefinierten Skills zu einer Pipeline der kognitiven Suche](cognitive-search-custom-skill-interface.md) sowie unter [Beispiel: Erstellen eines benutzerdefinierten Skills für die KI-Anreicherung](cognitive-search-create-custom-skill-example.md)

## <a name="see-also"></a>Weitere Informationen

+ [Definieren eines Skillsets](cognitive-search-defining-skillset.md)
+ [Schnittstellendefinition für benutzerdefinierte Skills](cognitive-search-custom-skill-interface.md)
+ [Tutorial: Angereicherte Indizierung mit künstlicher Intelligenz (KI)](cognitive-search-tutorial-blob.md)
