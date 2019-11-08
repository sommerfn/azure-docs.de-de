---
title: 'Schnellstart: Synthetisieren von Sprache in eine Audiodatei, C# (.NET) – Spracherkennungsdienst'
titleSuffix: Azure Cognitive Services
description: TBD
services: cognitive-services
author: erhopf
manager: nitinme
ms.service: cognitive-services
ms.subservice: speech-service
ms.topic: quickstart
ms.date: 10/28/2019
ms.author: erhopf
ms.openlocfilehash: 24f44d02170866645711cb302453a009d2935fbf
ms.sourcegitcommit: c22327552d62f88aeaa321189f9b9a631525027c
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 11/04/2019
ms.locfileid: "73505099"
---
## <a name="prerequisites"></a>Voraussetzungen

Führen Sie die folgenden Schritte aus, bevor Sie beginnen:

> [!div class="checklist"]
> * [Erstellen einer Azure Speech-Ressource](../../../../get-started.md)
> * [Einrichten Ihrer Entwicklungsumgebung](../../../../quickstarts/setup-platform.md?tabs=dotnet)
> * [Erstellen eines leeren Beispielprojekts](../../../../quickstarts/create-project.md?tabs=dotnet)

## <a name="open-your-project-in-visual-studio"></a>Öffnen Sie Ihr Projekt in Visual Studio.

Im ersten Schritt müssen Sie sicherstellen, dass das Projekt in Visual Studio geöffnet ist.

1. Starten Sie Visual Studio 2019.
2. Laden Sie das Projekt, und öffnen Sie `Program.cs`.

## <a name="start-with-some-boilerplate-code"></a>Beginnen mit Codebausteinen

Fügen Sie Code hinzu, der als Gerüst für das Projekt fungiert. Beachten Sie, dass Sie eine asynchrone Methode mit dem Namen `SynthesisToAudioFileAsync()` erstellt haben.

````C#

using System;
using System.Threading.Tasks;
using Microsoft.CognitiveServices.Speech;

namespace helloworld
{
    class Program
    {
        public static async Task SynthesisToAudioFileAsync()
        {
        }

        static void Main()
        {
            SynthesisToAudioFileAsync().Wait();
        }
    }
}

````

## <a name="create-a-speech-configuration"></a>Erstellen einer Sprachkonfiguration

Bevor Sie ein `SpeechSynthesizer`-Objekt initialisieren können, müssen Sie eine Konfiguration erstellen, die den Abonnementschlüssel und die Abonnementregion verwendet. Fügen Sie diesen Code in die Methode `SynthesisToAudioFileAsync()` ein.

````C#
var config = SpeechConfig.FromSubscription("YourSubscriptionKey", "YourServiceRegion");
````

## <a name="create-an-audio-configuration"></a>Erstellen einer Audiokonfiguration

Nun müssen Sie ein ````AudioConfig````-Objekt erstellen, das auf Ihre Audiodatei verweist. Dieses Objekt wird in einer using-Anweisung erstellt, um die ordnungsgemäße Freigabe nicht verwalteter Ressourcen sicherzustellen. Fügen Sie diesen Code in die Methode `SynthesisToAudioFileAsync()` direkt unterhalb der Sprachkonfiguration ein.

````C#
var fileName = "helloworld.wav";
using (var fileOutput = AudioConfig.FromWavFileOutput(fileName))
{
}
````

## <a name="initialize-a-speechsynthesizer"></a>Initialisieren eines SpeechSynthesizer-Objekts

Erstellen Sie nun das Objekt `SpeechSynthesizer` mithilfe der zuvor erstellten Objekte `SpeechConfig` und `AudioConfig`. Dieses Objekt wird auch in einer using-Anweisung erstellt, um die ordnungsgemäße Freigabe nicht verwalteter Ressourcen sicherzustellen. Fügen Sie diesen Code in der Methode `SynthesisToAudioFileAsync()` in die using-Anweisung ein, die das Objekt ````AudioConfig```` umschließt.

````C#
using (var synthesizer = new SpeechSynthesizer(config, fileOutput))
{
}
````

## <a name="synthesize-text-using-speaktextasync"></a>Synthetisieren von Text mithilfe von SpeakTextAsync

Sie rufen die Methode `SpeakTextAsync()` über das Objekt `SpeechSynthesizer` auf. Diese Methode sendet Ihren Text an den Spracherkennungsdienst, der ihn in Audiodaten konvertiert. Der `SpeechSynthesizer` verwendet die Standardstimme, wenn `config.VoiceName` nicht explizit angegeben wird.

Fügen Sie in der using-Anweisung den folgenden Code hinzu:
````C#
var text = "Hello world!";
var result = await synthesizer.SpeakTextAsync(text);
````

## <a name="check-for-errors"></a>Überprüfen auf Fehler

Wenn das Syntheseergebnis vom Spracherkennungsdienst zurückgegeben wird, sollten Sie überprüfen, ob der Text erfolgreich synthetisiert wurde.

Fügen Sie in der using-Anweisung unterhalb von `SpeakTextAsync()` den folgenden Code ein:
````C#
if (result.Reason == ResultReason.SynthesizingAudioCompleted)
{
    Console.WriteLine($"Speech synthesized to [{fileName}] for text [{text}]");
}
else if (result.Reason == ResultReason.Canceled)
{
    var cancellation = SpeechSynthesisCancellationDetails.FromResult(result);
    Console.WriteLine($"CANCELED: Reason={cancellation.Reason}");

    if (cancellation.Reason == CancellationReason.Error)
    {
        Console.WriteLine($"CANCELED: ErrorCode={cancellation.ErrorCode}");
        Console.WriteLine($"CANCELED: ErrorDetails=[{cancellation.ErrorDetails}]");
        Console.WriteLine($"CANCELED: Did you update the subscription info?");
    }
}
````

## <a name="check-your-code"></a>Überprüfen des Codes

Ihr Code sollte nun wie folgt aussehen:

````C#
//
// Copyright (c) Microsoft. All rights reserved.
// Licensed under the MIT license. See LICENSE.md file in the project root for full license information.
//

using System;
using System.Threading.Tasks;
using Microsoft.CognitiveServices.Speech;

namespace helloworld
{
    class Program
    {
        public static async Task SynthesisToAudioFileAsync()
        {
            var config = SpeechConfig.FromSubscription("YourSubscriptionKey", "YourServiceRegion");

            var fileName = "helloworld.wav";
            using (var fileOutput = AudioConfig.FromWavFileOutput(fileName))
            {
                using (var synthesizer = new SpeechSynthesizer(config, fileOutput))
                {
                    var text = "Hello world!";
                    var result = await synthesizer.SpeakTextAsync(text);

                    if (result.Reason == ResultReason.SynthesizingAudioCompleted)
                    {
                        Console.WriteLine($"Speech synthesized to [{fileName}] for text [{text}]");
                    }
                    else if (result.Reason == ResultReason.Canceled)
                    {
                        var cancellation = SpeechSynthesisCancellationDetails.FromResult(result);
                        Console.WriteLine($"CANCELED: Reason={cancellation.Reason}");

                        if (cancellation.Reason == CancellationReason.Error)
                        {
                            Console.WriteLine($"CANCELED: ErrorCode={cancellation.ErrorCode}");
                            Console.WriteLine($"CANCELED: ErrorDetails=[{cancellation.ErrorDetails}]");
                            Console.WriteLine($"CANCELED: Did you update the subscription info?");
                        }
                    }
                }
            }
        }

        static void Main()
        {
            SynthesisToAudioFileAsync().Wait();
        }
    }
}
````

## <a name="build-and-run-your-app"></a>Erstellen und Ausführen der App

Sie können nun Ihre App erstellen und die Sprachsynthese mit dem Spracherkennungsdienst testen.

1. **Kompilieren des Codes:** Wählen Sie auf der Menüleiste von Visual Studio **Build** > **Projektmappe erstellen** aus.
2. **Starten der App:** Wählen Sie auf der Menüleiste **Debuggen** > **Debuggen starten** aus, oder drücken Sie **F5**.
3. **Starten der Synthese**: Ihr Text wird in Sprache konvertiert und in den angegebenen Audiodaten gespeichert.

   ```text
   Speech synthesized to [helloworld.wav] for text [Hello world!]
   ```

## <a name="next-steps"></a>Nächste Schritte

[!INCLUDE [footer](./footer.md)]

## <a name="see-also"></a>Weitere Informationen

- [Create a Custom Voice](~/articles/cognitive-services/Speech-Service/how-to-custom-voice-create-voice.md) (Erstellen einer benutzerdefinierten Stimme)
- [Aufzeichnen benutzerdefinierter Stimmbeispiele](~/articles/cognitive-services/Speech-Service/record-custom-voice-samples.md)
