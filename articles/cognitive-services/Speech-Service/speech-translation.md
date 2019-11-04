---
title: Sprachübersetzung mit dem Speech-Dienst
titleSuffix: Azure Cognitive Services
description: Mit dem Speech-Dienst können Sie Ihren Anwendungen, Tools und Geräten End-to-End- und Echtzeit-Sprachübersetzungen sowie mehrsprachige Übersetzungen hinzufügen. Die gleiche API kann für Speech-to-Speech- und für Speech-to-Text-Übersetzungen verwendet werden.
services: cognitive-services
author: erhopf
manager: nitinme
ms.service: cognitive-services
ms.subservice: speech-service
ms.topic: conceptual
ms.date: 07/05/2019
ms.author: erhopf
ms.openlocfilehash: 17ad32cba18915bf7f83163cd876686d42323750
ms.sourcegitcommit: c22327552d62f88aeaa321189f9b9a631525027c
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 11/04/2019
ms.locfileid: "73468763"
---
# <a name="what-is-speech-translation"></a>Was ist Sprachübersetzung?

Die Sprachübersetzung aus den Azure-Sprachdiensten ermöglicht eine mehrsprachige Sprache-zu-Sprache- und eine Sprache-zu-Text-Übersetzung von Audiostreams in Echtzeit. Mit dem Speech SDK haben Ihre Anwendungen, Tools und Geräte Zugriff auf Quelltranskriptionen und Übersetzungsausgaben für bereitgestelltes Audio. Die Zwischenergebnisse der Transkription und Übersetzung werden zurückgegeben, wenn Sprache erkannt wird, und die Endergebnisse können in synthetisierte Sprache konvertiert werden.

Der Übersetzungsengine von Microsoft liegen zwei unterschiedliche Ansätze zugrunde: statistische maschinelle Übersetzung (Statistical Machine Translation, SMT) und neuronale maschinelle Übersetzung (Neural Machine Translation, NMT). SMT verwendet erweiterte statistische Analysen, um die bestmöglichen Übersetzungen im Kontext einiger weniger Wörter zu schätzen. Mit NMT werden neuronale Netze verwendet, um genauere, natürlich klingende Übersetzungen bereitzustellen, indem der gesamte Kontext von Sätzen zur Übersetzung von Wörtern genutzt wird.

Heute verwendet Microsoft NMT für die Übersetzung in die gängigsten Sprachen. Alle [für die Speech-to-Speech-Übersetzung verfügbaren Sprachen](language-support.md#speech-translation) werden von NMT unterstützt. Bei der Speech-to-Speech-Übersetzung kann je nach Sprachkombination SMT oder NMT verwendet werden. Wenn die Zielsprache von NMT unterstützt wird, wird die gesamte Übersetzung über NMT abgewickelt. Wenn die Zielsprache nicht von NMT unterstützt wird, wird für die Übersetzung eine Kombination aus NMT und SMT mit Englisch als „Pivot“ zwischen den beiden Sprachen verwendet.

## <a name="core-features"></a>Wichtige Funktionen

In der folgenden Tabelle sind die Funktionen des Speech SDK und der REST-APIs aufgelistet:

| Anwendungsfall | SDK | REST |
|----------|-----|------|
| Sprache-zu-Text-Übersetzung mit Erkennungsergebnissen. | Ja | Nein |
| Sprache-zu-Sprache-Übersetzung. | Ja | Nein |
| Zwischenergebnisse der Spracherkennung und Übersetzung. | Ja | Nein |

## <a name="get-started-with-speech-translation"></a>Erste Schritte mit der Sprachübersetzung

Wir bieten Schnellstarts an, die so konzipiert sind, dass Sie in weniger als 10 Minuten Code ausführen können. In dieser Tabelle werden Schnellstarts Sprachübersetzungen nach Sprache aufgelistet.

| Schnellstart | Plattform | API-Referenz |
|------------|----------|---------------|
| [C#, .NET Core](~/articles/cognitive-services/Speech-Service/quickstarts/translate-speech-to-text.md?pivots=programming-language-csharp&tabs=dotnetcore) | Windows | [Browse](https://aka.ms/csspeech/csharpref) |
| [C#, .NET Framework](~/articles/cognitive-services/Speech-Service/quickstarts/translate-speech-to-text.md?pivots=programming-language-csharp&tabs=dotnet) | Windows | [Browse](https://aka.ms/csspeech/csharpref) |
| [C#, UWP](~/articles/cognitive-services/Speech-Service/quickstarts/translate-speech-to-text.md?pivots=programming-language-csharp&tabs=uwp) | Windows | [Browse](https://aka.ms/csspeech/csharpref) |
| [C++](~/articles/cognitive-services/Speech-Service/quickstarts/translate-speech-to-text.md?pivots=programming-language-cpp&tabs=windows) | Windows | [Browse](https://aka.ms/csspeech/cppref)|
| [Java](~/articles/cognitive-services/Speech-Service/quickstarts/translate-speech-to-text.md?pivots=programming-language-java&tabs=jre) | Windows, Linux, macOS | [Browse](https://aka.ms/csspeech/javaref) |

## <a name="sample-code"></a>Beispielcode

Beispielcode für das Speech SDK finden Sie auf GitHub. In den Beispielen werden gängige Szenarien wie etwa das Lesen von Audiodaten aus einer Datei oder einem Stream, die kontinuierliche und einmalige Erkennung/Übersetzung oder die Verwendung benutzerdefinierter Modelle behandelt.

* [Beispiele für Spracherkennung und Übersetzung (SDK)](https://github.com/Azure-Samples/cognitive-services-speech-sdk)

## <a name="migration-guides"></a>Migrationsleitfäden

Wenn Ihre Anwendungen, Tools oder Produkte die [Sprachübersetzungs-API](https://docs.microsoft.com/azure/cognitive-services/translator-speech/overview) verwenden, finden Sie in den nachfolgend aufgelisteten Leitfäden Informationen zur Migration zu den Sprachdiensten.

* [Migrieren von der Sprachübersetzungs-API zum Spracherkennungsdienst](how-to-migrate-from-translator-speech-api.md)

## <a name="reference-docs"></a>Referenz

* [Speech SDK](speech-sdk-reference.md)
* [Speech-Geräte-SDK](speech-devices-sdk.md)
* [REST-API: Spracherkennung](rest-speech-to-text.md)
* [REST-API: Sprachsynthese](rest-text-to-speech.md)
* [REST-API: Batchtranskription und Anpassung](https://westus.cris.ai/swagger/ui/index)

## <a name="next-steps"></a>Nächste Schritte

* [Beziehen eines kostenlosen Abonnementschlüssels für die Spracherkennungsdienste](get-started.md)
* [Abrufen des Speech SDK](speech-sdk.md)
