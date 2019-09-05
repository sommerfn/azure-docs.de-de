---
title: Sprache-in-Text – Speech-Dienst
titleSuffix: Azure Cognitive Services
description: Die Spracherkennung im Speech-Dienst, auch als Sprache-in-Text bezeichnet, ermöglicht die Echtzeittranskription von Audiostreams in Text, den Ihre Anwendungen, Tools oder Geräte nutzen, anzeigen und als Befehlseingabe verarbeiten können. Dieser Dienst nutzt dieselbe Erkennungstechnologie, die Microsoft auch bei Cortana und Office-Produkten einsetzt, und funktioniert nahtlos mit Übersetzungen und Sprachsynthese.
services: cognitive-services
author: erhopf
manager: nitinme
ms.service: cognitive-services
ms.subservice: speech-service
ms.topic: conceptual
ms.date: 07/05/2019
ms.author: erhopf
ms.openlocfilehash: fa80ebea93ae897ba01ff54bdb797ed4194665cd
ms.sourcegitcommit: 82499878a3d2a33a02a751d6e6e3800adbfa8c13
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 08/28/2019
ms.locfileid: "70068860"
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

Zusätzlich zum Standardbasismodell, das von den Spracherkennungsdiensten verwendet wird, können Sie die Modelle mit verfügbaren Daten an Ihre Bedürfnisse anpassen, um Hindernisse bei der Spracherkennung wie Sprachstil, Wortschatz und Hintergrundgeräusche zu überwinden. Weitere Informationen finden Sie unter [Benutzerdefinierte Spracherkennung](how-to-custom-speech.md).

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
