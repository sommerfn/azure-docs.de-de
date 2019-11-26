---
title: Was sind Speech-Dienste?
titleSuffix: Azure Cognitive Services
description: Die Speech-Dienste dienen zur Vereinheitlichung von Spracherkennung, Sprachsynthese und Sprachübersetzung in einem einzelnen Azure-Abonnement. Über das Speech SDK, das Speech Devices SDK oder REST-APIs können Sie Ihren Anwendungen, Tools und Geräten Sprachfunktionen hinzufügen.
services: cognitive-services
author: erhopf
manager: nitinme
ms.service: cognitive-services
ms.subservice: speech-service
ms.topic: overview
ms.date: 11/05/2019
ms.author: erhopf
ms.openlocfilehash: b1c32042fcb70ff16bb9d014a9f3546f4435e83f
ms.sourcegitcommit: a107430549622028fcd7730db84f61b0064bf52f
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 11/14/2019
ms.locfileid: "74075772"
---
# <a name="what-are-the-speech-services"></a>Was sind Speech-Dienste?

Die Speech-Dienste dienen zur Vereinheitlichung von Spracherkennung, Sprachsynthese und Sprachübersetzung in einem einzelnen Azure-Abonnement. Über das [Speech SDK](speech-sdk-reference.md), das [Speech Devices SDK](https://aka.ms/sdsdk-quickstart) oder [REST-APIs](rest-apis.md) können Sie Ihren Anwendungen, Tools und Geräten komfortabel mit Sprachfunktionen ausstatten.

> [!IMPORTANT]
> Speech-Dienste haben die Bing-Spracheingabe-API sowie Sprachübersetzung und Custom Speech abgelöst. Eine Migrationsanleitung finden Sie unter _Anleitungen > Migration_.

Die Azure Speech-Dienste umfassen folgende Features. Verwenden Sie die Links in der Tabelle, um weitere Informationen zu gängigen Anwendungsfällen für das jeweilige Feature zu erhalten oder sich die API-Referenz anzusehen.

| Dienst | Feature | BESCHREIBUNG | SDK | REST |
| ------- | ------- | ----------- | --- | ---- |
| [Spracherkennung](speech-to-text.md) | Spracherkennung | Die Spracherkennung wandelt Audiodatenströme in Echtzeit in Text um, der von Ihren Anwendungen, Tools oder Geräten genutzt oder angezeigt werden kann. In Kombination mit [Language Understanding (LUIS)](https://docs.microsoft.com/azure/cognitive-services/luis/) können Sie Benutzerabsichten aus transkribierter Sprache ableiten und auf Sprachbefehle reagieren. | [Ja](https://docs.microsoft.com/azure/cognitive-services/speech-service/speech-sdk-reference) | [Ja](https://docs.microsoft.com/azure/cognitive-services/speech-service/rest-apis) |
|         | [Batch-Transkription](batch-transcription.md) | Die Batch-Transkription ermöglicht eine asynchrone Spracherkennung/Transkription großer Datenmengen. Dieser REST-basierte Dienst verwendet den gleichen Endpunkt wie die Anpassung und Modellverwaltung. | Nein | [Ja](https://westus.cris.ai/swagger/ui/index) |
|         | [Unterhaltungstranskription](conversation-transcription-service.md) | Dieser Dienst ermöglicht Spracherkennung, Sprecheridentifikation und Diarisierung in Echtzeit. Er eignet sich optimal für das Transkribieren persönlicher Besprechungen, wobei zwischen Sprechern unterschieden werden kann. | Ja | Nein |
|         | [Erstellen von benutzerdefinierten Sprachmodellen](#customize-your-speech-experience) | Wenn Sie die Spracherkennung für die Erkennung und Transkription in einer individuellen Umgebung verwenden, können Sie benutzerdefinierte Akustik-, Sprach- und Aussprachemodelle erstellen, um Umgebungsgeräusche zu kompensieren oder branchenspezifisches Vokabular zu berücksichtigen. | Nein | [Ja](https://westus.cris.ai/swagger/ui/index) |
| [Text-zu-Sprache](text-to-speech.md) | Text-zu-Sprache | Die Sprachsynthese konvertiert Eingabetext mithilfe von [Speech Synthesis Markup Language (SSML)](text-to-speech.md#speech-synthesis-markup-language-ssml) in menschenähnliche synthetische Sprache. Sie haben die Wahl zwischen Standardstimmen und neuronalen Stimmen. (Weitere Informationen finden Sie unter [Sprach- und Regionsunterstützung für die API für den Spracherkennungsdienst](language-support.md).) | [Ja](https://docs.microsoft.com/azure/cognitive-services/speech-service/speech-sdk-reference) | [Ja](https://docs.microsoft.com/azure/cognitive-services/speech-service/rest-apis) |
|         | [Erstellen benutzerdefinierter Stimmen](#customize-your-speech-experience) | Erstellen Sie spezielle benutzerdefinierte Voicefonts für Ihre Marke oder Ihr Produkt. | Nein | [Ja](https://docs.microsoft.com/azure/cognitive-services/speech-service/rest-apis) |
| [Sprachübersetzung](speech-translation.md) | Sprachübersetzung | Die Sprachübersetzung ermöglicht Echtzeitübersetzungen in mehreren Sprachen für Ihre Anwendungen, Tools und Geräte. Verwenden Sie diesen Dienst für die Übersetzung von Sprache in Sprache und Sprache in Text. | [Ja](https://docs.microsoft.com/azure/cognitive-services/speech-service/speech-sdk-reference) | Nein |
| [Sprachassistenten](voice-assistants.md) | Sprachassistenten | Sprachassistenten, die Azure Speech-Dienste verwenden, ermöglichen es Entwicklern, natürliche, menschenähnliche Konversationsschnittstellen für ihre Anwendungen und Umgebungen zu erstellen. Der Dienst für Sprachassistenten ermöglicht eine schnelle und zuverlässige Interaktion zwischen einem Gerät und einer Assistentenimplementierung über den Direct Line Speech-Kanal des Bot-Frameworks oder über benutzerdefinierte Befehle (Vorschauversion) für die Aufgabenausführung. | [Ja](voice-assistants.md) | Nein |

## <a name="news-and-updates"></a>Neuigkeiten und Aktualisierungen

Hier finden Sie Neuigkeiten im Zusammenhang mit den Azure Speech-Diensten.

- September 2019
  - Speech SDK 1.7.0 veröffentlicht. Eine vollständige Liste der Aktualisierungen, Verbesserungen und bekannten Probleme finden Sie in den [Versionshinweisen](releasenotes.md).
- August 2019
  - **Neues Tutorial:** [Tutorial: Voice-enable your bot using the Speech SDK](tutorial-voice-enable-your-bot-speech-sdk.md) (Ermöglichen der Verwendung von Sprache in Ihrem Bot mit dem Speech SDK, C#)
  - Für die Stimme `en-US-JessaNeural` wurde eine neue Sprechweise ([`chat`](speech-synthesis-markup.md#adjust-speaking-styles)) hinzugefügt.
- Juni 2019
  - Speech SDK 1.6.0 veröffentlicht. Eine vollständige Liste der Aktualisierungen, Verbesserungen und bekannten Probleme finden Sie in den [Versionshinweisen](releasenotes.md).
- Mai 2019: Die Dokumentation für [Unterhaltungstranskription](conversation-transcription-service.md), [Callcentertranskription](call-center-transcription.md) und [Sprachassistenten](voice-assistants.md) ist jetzt verfügbar.
- Mai 2019
  - Speech SDK 1.5.1 veröffentlicht. Eine vollständige Liste der Aktualisierungen, Verbesserungen und bekannten Probleme finden Sie in den [Versionshinweisen](releasenotes.md).
  - Speech SDK 1.5.0 veröffentlicht. Eine vollständige Liste der Aktualisierungen, Verbesserungen und bekannten Probleme finden Sie in den [Versionshinweisen](releasenotes.md).

## <a name="try-speech-services"></a>Ausprobieren der Speech-Dienste

Wir stellen Schnellstartanleitungen für die gängigsten Programmiersprachen bereit. Diese sind jeweils so konzipiert, dass Sie in weniger als zehn Minuten über ausführbaren Code verfügen. Die folgende Tabelle enthält die beliebtesten Schnellstartanleitungen für die einzelnen Features. Über die Navigationsleiste auf der linken Seite gelangen Sie auf Wunsch zu weiteren Programmiersprachen und Plattformen.

| Spracherkennung (SDK) | Sprachsynthese (SDK) | Übersetzung (SDK) |
| -------------------- | -------------------- | ----------------- |
| [Erkennen von Sprache aus einer Audiodatei](quickstarts/speech-to-text-from-file.md) | [Synthetisieren von Sprache in eine Audiodatei](quickstarts/text-to-speech-audio-file.md) | [Übersetzen von gesprochener Sprache in Text](quickstarts/translate-speech-to-text.md) |
| [Erkennen von Sprache mit einem Mikrofon](quickstarts/speech-to-text-from-microphone.md) | [Synthetisieren von Sprache über einen Lautsprecher](quickstarts/text-to-speech.md) | [Übersetzen von Sprache in mehrere Zielsprachen](quickstarts/translate-speech-to-text-multiple-languages.md) |
| [Erkennen von Sprache aus einem Blobspeicher](quickstarts/from-blob.md) | [Asynchrone Synthese für lange Audioinhalte](quickstarts/text-to-speech/async-synthesis-long-form-audio.md) | [Übersetzen von Sprache in Sprache](quickstarts/translate-speech-to-speech.md) |

> [!NOTE]
> Spracherkennung und Sprachsynthese verfügen ebenfalls über REST-Endpunkte und die zugehörigen Schnellstartanleitungen.

Nachdem Sie Gelegenheit hatten, die Speech-Dienste zu verwenden, erfahren Sie in unserem Tutorial, wie Sie mithilfe des Speech SDK und LUIS Absichten in gesprochener Sprache erkennen.

- [Tutorial: Erkennen von Absichten anhand von gesprochener Sprache mit dem Speech SDK für C#](how-to-recognize-intents-from-speech-csharp.md)
- [Tutorial: Voice-enable your bot using the Speech SDK](tutorial-voice-enable-your-bot-speech-sdk.md) (Ermöglichen der Verwendung von Sprache in Ihrem Bot mit dem Speech SDK, C#)
- [Tutorial: Erstellen einer Flask-App zum Übersetzen von Text, Analysieren der Stimmung und Synthetisieren von Sprache, REST](https://docs.microsoft.com/azure/cognitive-services/translator/tutorial-build-flask-app-translation-synthesis?toc=%2fazure%2fcognitive-services%2fspeech-service%2ftoc.json&bc=%2fazure%2fcognitive-services%2fspeech-service%2fbreadcrumb%2ftoc.json&toc=%2Fen-us%2Fazure%2Fcognitive-services%2Fspeech-service%2Ftoc.json&bc=%2Fen-us%2Fazure%2Fbread%2Ftoc.json)

## <a name="get-sample-code"></a>Abrufen des Beispielcodes

Auf GitHub steht Beispielcode für jeden der Azure Speech-Dienste zur Verfügung. In den Beispielen werden gängige Szenarien wie etwa das Lesen von Audiodaten aus einer Datei oder einem Stream, die kontinuierliche und einmalige Erkennung oder die Verwendung benutzerdefinierter Modelle behandelt. Über die folgenden Links gelangen Sie zu den SDK- und REST-Beispielen:

- [Beispiele für Spracherkennung, Sprachsynthese und Sprachübersetzung (SDK)](https://github.com/Azure-Samples/cognitive-services-speech-sdk)
- [Batch transcription samples (REST) (Batchtranskriptionsbeispiele (REST))](https://github.com/Azure-Samples/cognitive-services-speech-sdk/tree/master/samples/batch)
- [Beispiele für Text-zu-Sprache (REST)](https://github.com/Azure-Samples/Cognitive-Speech-TTS)
- [Beispiele für Sprachassistenten (SDK)](https://aka.ms/csspeech/samples)

## <a name="customize-your-speech-experience"></a>Anpassen der Speech-Funktionen

Die Azure Speech-Dienste funktionieren bestens mit integrierten Modellen, können auf Wunsch aber auch speziell für Ihr Produkt oder Ihre Umgebung angepasst und optimiert werden. Die Anpassungsoptionen reichen von der Feinabstimmung des Akustikmodells bis hin zu individuellen Voicefonts für Ihre Marke. Nachdem Sie ein benutzerdefiniertes Modell erstellt haben, können Sie es mit allen Azure Speech-Diensten verwenden.

| Spracherkennungsdienst | Plattform | BESCHREIBUNG |
| -------------- | -------- | ----------- |
| Spracherkennung | [Custom Speech Service](https://aka.ms/customspeech) | Passen Sie Spracherkennungsmodelle an Ihre Anforderungen und verfügbaren Daten an. Überwinden Sie Grenzen der Spracherkennung wie z. B. Sprachstil, Vokabular und Hintergrundgeräusche. |
| Sprachsynthese | [Custom Voice](https://aka.ms/customvoice) | Erstellen Sie mit Ihren verfügbaren Sprachdaten eine erkennbare, einzigartige Stimme für Ihre Sprachsynthese-Apps. Sie können die Sprachausgaben weiter optimieren, indem Sie eine Reihe von Sprachparametern anpassen. |

## <a name="reference-docs"></a>Referenz

- [Speech SDK](speech-sdk-reference.md)
- [Speech-Geräte-SDK](speech-devices-sdk.md)
- [REST-API: Spracherkennung](rest-speech-to-text.md)
- [REST-API: Sprachsynthese](rest-text-to-speech.md)
- [REST-API: Batchtranskription und Anpassung](https://westus.cris.ai/swagger/ui/index)

## <a name="next-steps"></a>Nächste Schritte

> [!div class="nextstepaction"]
> [Kostenloses Testen von Speech Services](get-started.md)
