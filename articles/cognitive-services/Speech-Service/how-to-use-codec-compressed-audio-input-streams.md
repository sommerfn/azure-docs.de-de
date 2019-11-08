---
title: Streamen von per Codec komprimierten Audiodaten mit dem Speech SDK – Speech Service
titleSuffix: Azure Cognitive Services
description: Erfahren Sie, wie Sie mit dem Speech SDK komprimierte Audiodaten an Azure Speech Services streamen. Verfügbar für C++, C# und Java für Linux, Java in Android und Objective-C in iOS.
services: cognitive-services
author: amitkumarshukla
manager: nitinme
ms.service: cognitive-services
ms.subservice: speech-service
ms.topic: conceptual
ms.date: 09/20/2019
ms.author: amishu
ms.openlocfilehash: 668964c597b8d748220cbeec68e0ba68300cb406
ms.sourcegitcommit: c22327552d62f88aeaa321189f9b9a631525027c
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 11/04/2019
ms.locfileid: "73464349"
---
# <a name="using-codec-compressed-audio-input-with-the-speech-sdk"></a>Verwenden von per Codec komprimierter Audioeingabe mit dem Speech SDK

Die Speech SDK-API für **komprimierte Audioeingabestreams** bietet eine Möglichkeit zum Streamen von komprimierten Audiodaten an Speech Services mit PullStream oder PushStream.

> [!IMPORTANT]
> Das Streamen komprimierter Eingabeaudiodaten wird derzeit nur für C++, C# und Java unter Linux (Ubuntu 16.04, Ubuntu 18.04, Debian 9) unterstützt. Es wird auch für [Java in Android](how-to-use-codec-compressed-audio-input-streams-android.md) und [Objective-C in iOS](how-to-use-codec-compressed-audio-input-streams-ios.md) unterstützt.
> Das Speech SDK ab Version 1.7.0 ist erforderlich.

Informationen zu WAV/PCM finden Sie in der Hauptdokumentation zu Speech.  Neben WAV/PCM werden folgende per Codec komprimierten Eingabeformate unterstützt:

- MP3
- OPUS/OGG
- FLAC
- ALAW im WAV-Container
- MULAW im WAV-Container

## <a name="prerequisites"></a>Voraussetzungen

Die Verarbeitung komprimierter Audiodaten wird mit [GStreamer](https://gstreamer.freedesktop.org) implementiert. Aus Lizenzierungsgründen werden die GStreamer-Binärdateien nicht kompiliert und mit dem Speech SDK verknüpft. Daher muss der Anwendungsentwickler Folgendes unter 18.04, 16.04 und Debian 9 installieren, um komprimierte Audioeingaben zu verwenden.

```sh
sudo apt install libgstreamer1.0-0 gstreamer1.0-plugins-base gstreamer1.0-plugins-good gstreamer1.0-plugins-bad gstreamer1.0-plugins-ugly
```

## <a name="example-code-using-codec-compressed-audio-input"></a>Beispielcode für die Verwendung von per Codec komprimierter Audioeingabe

Erstellen Sie zum Streamen von komprimierten Audioformaten an Speech Services `PullAudioInputStream` oder `PushAudioInputStream`. Erstellen Sie dann eine `AudioConfig` aus einer Instanz Ihrer stream-Klasse, und geben Sie dabei das Komprimierungsformat des Streams an.

Angenommen, Sie verfügen über die Eingabestreamklasse `myPushStream` und verwenden OPUS/OGG. Ihr Code kann folgendermaßen aussehen:

```csharp
using Microsoft.CognitiveServices.Speech;
using Microsoft.CognitiveServices.Speech.Audio;

var speechConfig = SpeechConfig.FromSubscription("YourSubscriptionKey", "YourServiceRegion");

// Create an audio config specifying the compressed audio format and the instance of your input stream class.
var audioFormat = AudioStreamFormat.GetCompressedFormat(AudioStreamContainerFormat.OGG_OPUS);
var audioConfig = AudioConfig.FromStreamInput(myPushStream, audioFormat);

var recognizer = new SpeechRecognizer(speechConfig, audioConfig);

var result = await recognizer.RecognizeOnceAsync();

var text = result.GetText();
```

## <a name="next-steps"></a>Nächste Schritte

- [Abrufen Ihres Testabonnements für Speech](https://azure.microsoft.com/try/cognitive-services/)
* [Erkennen von Sprache in Java](~/articles/cognitive-services/Speech-Service/quickstarts/speech-to-text-from-microphone.md?pivots=programming-language-java)
