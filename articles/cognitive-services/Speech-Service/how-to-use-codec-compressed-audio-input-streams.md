---
title: 'Streamen von per Codec komprimierten Audiodaten mit dem Speech SDK: Speech-Dienste'
titleSuffix: Azure Cognitive Services
description: Erfahren Sie, wie Sie mit dem Speech SDK komprimierte Audiodaten an Azure Speech Services streamen. Verfügbar für C++, C# und Java für Linux.
services: cognitive-services
author: amitkumarshukla
manager: nitinme
ms.service: cognitive-services
ms.subservice: speech-service
ms.topic: conceptual
ms.date: 05/02/2019
ms.author: amishu
ms.openlocfilehash: 8717dad27786450e4275c9fd91523c0e9379f375
ms.sourcegitcommit: 41ca82b5f95d2e07b0c7f9025b912daf0ab21909
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 06/13/2019
ms.locfileid: "67073059"
---
# <a name="using-codec-compressed-audio-input-with-the-speech-sdk"></a>Verwenden von per Codec komprimierter Audioeingabe mit dem Speech SDK

Die Speech SDK-API für **komprimierte Audioeingabestreams** bietet eine Möglichkeit zum Streamen von komprimierten Audiodaten an Speech Services mit PullStream oder PushStream.

> [!IMPORTANT]
> Das Streamen komprimierter Audiodaten wird nur für C++, C# und Java unter Linux (Ubuntu 16.04, Ubuntu 18.04, Debian 9) unterstützt.
> Das Speech SDK Version 1.4.0 oder höher ist erforderlich.

Informationen zu WAV/PCM finden Sie in der Hauptdokumentation zu Speech.  Neben WAV/PCM werden folgende per Codec komprimierten Eingabeformate unterstützt:

- MP3
- OPUS/OGG

## <a name="prerequisites-to-using-codec-compressed-audio-input"></a>Voraussetzungen für die Verwendung von per Codec komprimierter Audioeingabe

Installieren Sie die folgenden zusätzlichen Abhängigkeiten, um komprimierte Audioeingaben mit dem Speech SDK für Linux verwenden zu können:

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
- [Erkennen von Sprache in C#](quickstart-csharp-dotnet-windows.md)
