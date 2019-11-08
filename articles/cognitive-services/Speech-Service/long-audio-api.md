---
title: API für lange Audioinhalte (Vorschau) – Spracherkennungsdienst
titleSuffix: Azure Cognitive Services
description: ''
services: cognitive-services
author: erhopf
manager: nitinme
ms.service: cognitive-services
ms.subservice: speech-service
ms.topic: conceptual
ms.date: 10/26/2019
ms.author: erhopf
ms.openlocfilehash: 391cddbbd1b69fb7cb5422adbaea2f3378e273bf
ms.sourcegitcommit: f4d8f4e48c49bd3bc15ee7e5a77bee3164a5ae1b
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 11/04/2019
ms.locfileid: "73580157"
---
# <a name="long-audio-api-preview"></a>API für lange Audioinhalte (Vorschau)

Die API für lange Audioinhalte ist für die asynchrone Synthese von langen Sprachsynthesen (z. B. Hörbücher) konzipiert. Diese API gibt keine synthetisierten Audiodaten in Echtzeit zurück, sondern erwartet, dass Sie die Antworten abrufen und die Ausgaben so verwenden, wie sie vom Dienst bereitgestellt werden. Im Gegensatz zur Text-to-Speech-API, die vom Speech SDK verwendet wird, kann die API für lange Audioinhalte synthetisierte Audiodaten erzeugen, die länger als 10 Minuten sind, wodurch sie ideal für Herausgeber und Plattformen für Audioinhalte geeignet ist.

Weitere Vorteile der API für lange Audioinhalte:

* Die vom Dienst zurückgegebene synthetisierte Sprache verwendet neuronale Stimmen, was eine High-Fidelity-Audioausgabe sicherstellt.
* Da Echtzeitantworten nicht unterstützt werden, ist es nicht erforderlich, einen Sprachendpunkt bereitzustellen.

## <a name="workflow"></a>Workflow

Wenn Sie die API für lange Audioinhalte verwenden, senden Sie typischerweise eine Textdatei oder zu synthetisierende Dateien, fragen den Status ab und können die Audioausgabe herunterladen, wenn der Status „Erfolgreich“ ist.

Dieses Diagramm bietet eine allgemeine Übersicht über den Workflow.

![Diagramm zum Workflow der API für lange Audioinhalte](media/long-audio-api/long-audio-api-workflow.png)

## <a name="prepare-content-for-synthesis"></a>Vorbereiten der Inhalte für die Synthese

Stellen Sie bei der Vorbereitung Ihrer Textdatei Folgendes sicher:

* Es handelt sich entweder um Nur-Text (.txt) oder SSML-Text (.txt).
  * Bei Nur-Text wird jeder Absatz durch Drücken der **EINGABETASTE** getrennt – [Beispiel für die Eingabe von Nur-Text](https://github.com/Azure-Samples/Cognitive-Speech-TTS/blob/master/CustomVoice-API-Samples/Java/en-US.txt) anzeigen
  * Bei SSML-Text wird jede SSML-Komponente als Absatz betrachtet. SSML-Komponenten sollen durch verschiedene Absätze getrennt werden – [Beispiel für die Eingabe von SSML-Text](https://github.com/Azure-Samples/Cognitive-Speech-TTS/blob/master/CustomVoice-API-Samples/Java/SSMLTextInputSample.txt) anzeigen. Informationen zum Sprachcode finden Sie unter [Markupsprache für Sprachsynthese](speech-synthesis-markup.md) (Speech Synthesis Markup Language, SSML).
* Die Verschlüsselung ist [UTF-8 mit Bytereihenfolge-Marke (BOM)](https://www.w3.org/International/questions/qa-utf8-bom.en#bom).
* Sie enthält mehr als 10.000 Zeichen oder mehr als 50 Absätze.
* Es handelt sich um eine einzelne Datei, nicht um ein ZIP-Archiv.

## <a name="audio-output-formats"></a>Audioausgabeformate

Die folgenden Audioausgabeformate werden von der API für lange Audioinhalte unterstützt:

> [!NOTE]
> Das Standardaudioformat ist riff-16khz-16bit-mono-pcm.

* riff-8khz-16bit-mono-pcm
* riff-16khz-16bit-mono-pcm
* riff-24khz-16bit-mono-pcm
* riff-48khz-16bit-mono-pcm
* audio-16khz-32kbitrate-mono-mp3
* audio-16khz-64kbitrate-mono-mp3
* audio-16khz-128kbitrate-mono-mp3
* audio-24khz-48kbitrate-mono-mp3
* audio-24khz-96kbitrate-mono-mp3
* audio-24khz-160kbitrate-mono-mp3

## <a name="quickstarts"></a>Schnellstarts

Wir bieten Schnellstarts, die Ihnen helfen sollen, die API für lange Audioinhalte erfolgreich zu verwenden. In dieser Tabelle werden Schnellstarts für die API für lange Audioinhalte nach Sprache aufgelistet.

* [Schnellstart: Python](https://aka.ms/long-audio-python)

## <a name="sample-code"></a>Beispielcode
Beispielcode für die API für lange Audioinhalte finden Sie auf GitHub.

* [Beispielcode: Python](https://github.com/Azure-Samples/Cognitive-Speech-TTS/tree/master/CustomVoice-API-Samples/Python)
* [Beispielcode: C#](https://github.com/Azure-Samples/Cognitive-Speech-TTS/tree/master/CustomVoice-API-Samples/CSharp)
* [Beispielcode: Java](https://github.com/Azure-Samples/Cognitive-Speech-TTS/blob/master/CustomVoice-API-Samples/Java/)
