---
title: Spracherkennung mit Azure Speech-Diensten
titleSuffix: Azure Cognitive Services
description: Die Spracherkennung der Azure Speech-Dienste, auch als Sprache-zu-Text bezeichnet, ermöglicht die Echtzeittranskription von Audiostreams in Text, den Ihre Anwendungen, Tools oder Geräte nutzen, anzeigen und als Befehlseingabe verarbeiten können. Dieser Dienst nutzt dieselbe Erkennungstechnologie, die Microsoft auch bei Cortana und Office-Produkten einsetzt, und funktioniert nahtlos mit Übersetzungen und Sprachsynthese.
services: cognitive-services
author: erhopf
manager: nitinme
ms.service: cognitive-services
ms.subservice: speech-service
ms.topic: conceptual
ms.date: 07/05/2019
ms.author: erhopf
ms.openlocfilehash: cb9362c4d58deb5472c8d5adab39cdd1cc4e2600
ms.sourcegitcommit: e9c866e9dad4588f3a361ca6e2888aeef208fc35
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 07/19/2019
ms.locfileid: "68333571"
---
# <a name="what-is-speech-to-text"></a>Was ist die Spracherkennung?

Die Spracherkennung der Azure Speech-Dienste, auch als Sprache-zu-Text bezeichnet, ermöglicht die Echtzeittranskription von Audiostreams in Text, den Ihre Anwendungen, Tools oder Geräte nutzen, anzeigen und als Befehlseingabe verarbeiten können. Dieser Dienst nutzt dieselbe Erkennungstechnologie, die Microsoft auch bei Cortana und Office-Produkten einsetzt, und funktioniert nahtlos mit Übersetzungen und Sprachsynthese.  Eine vollständige Liste der Spracherkennungssprachen finden Sie unter [Unterstützte Sprachen](https://docs.microsoft.com/azure/cognitive-services/speech-service/language-support#speech-to-text).

Der Spracherkennungsdienst verwendet standardmäßig das sogenannte Universal Language Model. Dieses Modell wurde mit Microsoft-Daten trainiert, und es wird in der Cloud bereitgestellt. Es eignet sich besonders für Gesprächs- oder Diktatszenarios. Wenn Sie die Spracherkennung für die Erkennung und Transkription in einer individuellen Umgebung verwenden, können Sie benutzerdefinierte Akustik-, Sprach- und Aussprachemodelle erstellen, um Umgebungsgeräusche zu kompensieren oder branchenspezifisches Vokabular zu berücksichtigen.

Mit dem Speech SDK und den REST-APIs können Sie unkompliziert Tonaufnahmen eines Mikrofons erfassen, aus einem Stream lesen und auf Audiodateien in einem Speicher zugreifen. Das Speech SDK unterstützt für die Spracherkennung 16-Bit-WAV/PCM-Einkanalaudiodateien mit einer Frequenz von 16 kHz/8 kHz. Zusätzliche Audioformate werden mit dem [Spracherkennungs-REST-Endpunkt](https://docs.microsoft.com/azure/cognitive-services/speech-service/rest-apis) oder dem [Batchtranskriptionsdienst](https://docs.microsoft.com/azure/cognitive-services/speech-service/batch-transcription#supported-formats) unterstützt.

## <a name="core-features"></a>Wichtige Funktionen

In der folgenden Tabelle sind die Funktionen des Speech SDK und der REST-APIs aufgelistet:

| Anwendungsfall | SDK | REST |
|----------|-----|------|
| Transkribieren einer kurzen Äußerung (< 15 Sekunden). Unterstützt nur das endgültige Transkriptionsergebnis. | Ja | Ja |
| Kontinuierliche Transkription einer langen Äußerungen und das Streamen von Audio (> 15 Sekunden). Unterstützt zeitweilige und endgültige Transkriptionsergebnisse. | Ja | Nein |
| Ableiten von Absichten von Erkennungsergebnissen mit dem [LUIS](https://docs.microsoft.com/azure/cognitive-services/luis/what-is-luis). | Ja | Nein\* |
| Asynchrone Batchtranskription langer Audiodateien. | Nein | Ja\** |
| Erstellen und Verwalten von Sprachmodellen. | Nein | Ja\** |
| Erstellen und Verwalten benutzerdefinierter Modellimplementierungen. | Nein | Ja\** |
| Erstellen von Genauigkeitsprüfungen zum Messen der Genauigkeit eines einfachen Modells im Vergleich zu einem benutzerdefinierten Modell. | Nein | Ja\** |
| Verwalten von Abonnements. | Nein | Ja\** |

\* *Sie können die LUIS-Absichten und Entitäten mithilfe eines separaten LUIS-Abonnements ableiten. Über dieses Abonnement kann das SDK den LUIS aufrufen und Ergebnisse für Absichten und Entitäten ausgeben. Mithilfe der REST-API können Sie LUIS selbst aufrufen, um Absichten und Entitäten mithilfe Ihres LUIS-Abonnements abzuleiten.*

\** *Diese Dienste sind über den Endpunkt cris.ai verfügbar. Weitere Informationen finden Sie in der [Swagger-Referenz](https://westus.cris.ai/swagger/ui/index).*

## <a name="get-started-with-speech-to-text"></a>Erste Schritte mit der Spracherkennung

Wir stellen Schnellstartanleitungen für die gängigsten Programmiersprachen bereit. Diese sind jeweils so konzipiert, dass Sie in weniger als zehn Minuten über ausführbaren Code verfügen. In [dieser Tabelle](https://aka.ms/csspeech#5-minute-quickstarts) sind alle Schnellstarts für das Speech-SDK nach Plattform und Sprache aufgelistet.  Einen API-Verweis finden Sie auch [hier](https://aka.ms/csspeech#reference).

Wenn Sie den Spracherkennungs-REST-Dienst verwenden möchten, finden Sie weitere Informationen unter [REST-APIs](https://docs.microsoft.com/azure/cognitive-services/speech-service/rest-apis).

## <a name="tutorials-and-sample-code"></a>Tutorials und Beispielcode

Nachdem Sie Gelegenheit hatten, die Speech-Dienste zu verwenden, erfahren Sie in unserem Tutorial, wie Sie mithilfe des Speech SDK und LUIS Absichten in gesprochener Sprache erkennen.

* [Tutorial: Erkennen von Absichten anhand von gesprochener Sprache mit dem Speech SDK für C#](how-to-recognize-intents-from-speech-csharp.md)

Beispielcode für das Speech SDK finden Sie auf GitHub. In den Beispielen werden gängige Szenarios wie etwa das Lesen von Audiodaten aus einer Datei oder einem Stream, die kontinuierliche und einmalige Erkennung oder die Verwendung benutzerdefinierter Modelle behandelt.

* [Speech-to-text samples (SDK) (Spracherkennungsbeispiele (SDK))](https://github.com/Azure-Samples/cognitive-services-speech-sdk)
* [Batch transcription samples (REST) (Batchtranskriptionsbeispiele (REST))](https://github.com/Azure-Samples/cognitive-services-speech-sdk/tree/master/samples/batch)

## <a name="customization"></a>Anpassung

Die Speech-Dienste verwenden das Universal Language Model. Sie können aber auch benutzerdefinierte Akustik-, Sprach- oder Aussprachemodelle erstellen, die genau zu Ihrem Szenario passen. In der folgenden Tabelle sind mögliche Anpassungsoptionen aufgelistet:

| Modell | BESCHREIBUNG |
|-------|-------------|
| [Akustikmodell](how-to-customize-acoustic-models.md) | Wenn Sie ein benutzerdefiniertes Akustikmodell erstellen, ist dies besonders für Anwendungen, Tools oder Geräte hilfreich, die in speziellen Umgebungen (etwa in einem Auto oder in einer Produktionshalle) verwendet werden, in denen jeweils besondere Aufzeichnungsverhältnisse herrschen. Zu Beispielen zählen Sprechen mit Akzent, bestimmte Hintergrundgeräusche oder die Verwendung eines bestimmten Mikrofons für Aufzeichnungen. |
| [Sprachmodell](how-to-customize-language-model.md) | Erstellen Sie ein benutzerdefiniertes Sprachmodell, um die Transkription spezifischen Vokabulars und spezifischer Grammatik (beispielsweise aus der Medizin- oder IT-Branche) zu verbessern. |
| [Aussprachemodell](how-to-customize-pronunciation.md) | Durch ein benutzerdefiniertes Aussprachemodell können Sie die phonetische Form und Darstellung eines Worts oder Begriffs definieren. Dies ist für die Verarbeitung angepasster Benennungen wie Produktnamen oder Akronymen hilfreich. Erforderlich ist zu Beginn lediglich eine Aussprachedatei – eine einfache TXT-Datei. |

> [!NOTE]
> Die Anpassungsoptionen unterscheiden sich je nach Sprache/Gebietsschema. Weitere Informationen erhalten Sie unter [Unterstützte Sprachen](supported-languages.md).

## <a name="migration-guides"></a>Migrationsleitfäden

> [!WARNING]
> Die Bing-Spracheingabe wird am 15. Oktober 2019 eingestellt.

Wenn Ihre Anwendungen, Tools oder Produkte die Bing-Spracheingabe-APIs oder Custom Speech verwenden, finden Sie in den nachfolgend aufgelisteten Leitfäden Informationen zur Migration zu den Speech-Diensten.

* [Migrieren von der Bing-Spracheingabe zum Spracherkennungsdienst](https://docs.microsoft.com/azure/cognitive-services/speech-service/how-to-migrate-from-bing-speech)
* [Migrieren vom Custom Speech Service zum Speech-Dienst](https://docs.microsoft.com/azure/cognitive-services/speech-service/how-to-migrate-from-custom-speech-service)

## <a name="reference-docs"></a>Referenz

* [Speech SDK](https://aka.ms/csspeech)
* [Speech-Geräte-SDK](speech-devices-sdk.md)
* [REST-API: Spracherkennung](rest-speech-to-text.md)
* [REST-API: Sprachsynthese](rest-text-to-speech.md)
* [REST-API: Batchtranskription und Anpassung](https://westus.cris.ai/swagger/ui/index)

## <a name="next-steps"></a>Nächste Schritte

* [Beziehen eines kostenlosen Abonnementschlüssels für die Spracherkennungsdienste](get-started.md)
* [Abrufen des Speech SDK](speech-sdk.md)
