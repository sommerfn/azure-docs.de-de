---
title: 'Schnellstart: Erkennen von Sprache aus einer Audiodatei, C# (.NET) – Speech-Dienst'
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
ms.openlocfilehash: fdb747212914769b8551d9cd12f1fbc8a01245dc
ms.sourcegitcommit: c22327552d62f88aeaa321189f9b9a631525027c
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 11/04/2019
ms.locfileid: "73506347"
---
## <a name="prerequisites"></a>Voraussetzungen

Führen Sie unbedingt die folgenden Schritte aus, bevor Sie beginnen:

> [!div class="checklist"]
> * [Erstellen einer Azure Speech-Ressource](../../../../get-started.md)
> * [Einrichten Ihrer Entwicklungsumgebung](../../../../quickstarts/setup-platform.md?tabs=dotnet)
> * [Erstellen eines leeren Beispielprojekts](../../../../quickstarts/create-project.md?tabs=dotnet)

## <a name="open-your-project-in-visual-studio"></a>Öffnen Sie Ihr Projekt in Visual Studio.

Im ersten Schritt müssen Sie sicherstellen, dass das Projekt in Visual Studio geöffnet ist.

1. Starten Sie Visual Studio 2019.
2. Laden Sie das Projekt, und öffnen Sie `Program.cs`.

## <a name="start-with-some-boilerplate-code"></a>Beginnen mit Codebausteinen

Fügen Sie Code hinzu, der als Gerüst für das Projekt fungiert. Beachten Sie, dass Sie eine asynchrone Methode mit dem Namen `RecognizeSpeechAsync()` erstellt haben.

````C#

using System;
using System.Threading.Tasks;
using Microsoft.CognitiveServices.Speech;

namespace helloworld
{
    class Program
    {
        public static async Task RecognizeSpeechAsync()
        {
        }

        static void Main()
        {
            RecognizeSpeechAsync().Wait();
        }
    }
}

````

## <a name="create-a-speech-configuration"></a>Erstellen einer Speech-Konfiguration

Bevor Sie ein `SpeechRecognizer`-Objekt initialisieren können, müssen Sie eine Konfiguration erstellen, die den Abonnementschlüssel und die Abonnementregion verwendet. Fügen Sie diesen Code in die Methode `RecognizeSpeechAsync()` ein.

> [!NOTE]
> In diesem Beispiel wird die Methode `FromSubscription()` zum Erstellen von `SpeechConfig` verwendet. Eine vollständige Liste der verfügbaren Methoden finden Sie unter [SpeechConfig-Klasse](https://docs.microsoft.com/dotnet/api/microsoft.cognitiveservices.speech.speechconfig?view=azure-dotnet).

````C#
var config = SpeechConfig.FromSubscription("YourSubscriptionKey", "YourServiceRegion");
````

## <a name="create-an-audio-configuration"></a>Erstellen einer Audiokonfiguration

Nun müssen Sie ein ````AudioConfig````-Objekt erstellen, das auf Ihre Audiodatei verweist. Dieses Objekt wird in einer using-Anweisung erstellt, um die ordnungsgemäße Freigabe nicht verwalteter Ressourcen sicherzustellen. Fügen Sie diesen Code in die Methode `RecognizeSpeechAsync()` direkt unterhalb der Speech-Konfiguration ein.

````C#
using (var audioInput = AudioConfig.FromWavFileInput(@"whatstheweatherlike.wav"))
{
}
````

## <a name="initialize-a-speechrecognizer"></a>Initialisieren eines SpeechRecognizer-Elements

Erstellen Sie nun das Objekt `SpeechRecognizer` mithilfe der zuvor erstellten Objekte `SpeechConfig` und `AudioConfig`. Dieses Objekt wird auch in einer using-Anweisung erstellt, um die ordnungsgemäße Freigabe nicht verwalteter Ressourcen sicherzustellen. Fügen Sie diesen Code in der Methode `RecognizeSpeechAsync()` in die using-Anweisung ein, die das Objekt ````AudioConfig```` umschließt.

````C#
using (var recognizer = new SpeechRecognizer(config, audioInput))
{
}
````

## <a name="recognize-a-phrase"></a>Erkennen eines Ausdrucks

Sie rufen die Methode `RecognizeOnceAsync()` über das Objekt `SpeechRecognizer` auf. Diese Methode teilt dem Speech-Dienst mit, dass Sie einen einzelnen Ausdruck zur Erkennung senden, und dass die Spracherkennung beendet werden soll, sobald der Ausdruck ermittelt wurde.

Fügen Sie in der using-Anweisung den folgenden Code hinzu:
````C#
Console.WriteLine("Recognizing first result...");
var result = await recognizer.RecognizeOnceAsync();
````

## <a name="display-the-recognition-results-or-errors"></a>Anzeigen der Erkennungsergebnisse (oder Fehler)

Wird das Erkennungsergebnis vom Speech-Dienst zurückgegeben, möchten Sie es verwenden. Zur Vereinfachung wird das Ergebnis in der Konsole ausgegeben.

Fügen Sie in der using-Anweisung unterhalb von `RecognizeOnceAsync()` den folgenden Code ein:
````C#
if (result.Reason == ResultReason.RecognizedSpeech)
{
    Console.WriteLine($"We recognized: {result.Text}");
}
else if (result.Reason == ResultReason.NoMatch)
{
    Console.WriteLine($"NOMATCH: Speech could not be recognized.");
}
else if (result.Reason == ResultReason.Canceled)
{
    var cancellation = CancellationDetails.FromResult(result);
    Console.WriteLine($"CANCELED: Reason={cancellation.Reason}");

    if (cancellation.Reason == CancellationReason.Error)
    {
        Console.WriteLine($"CANCELED: ErrorCode={cancellation.ErrorCode}");
        Console.WriteLine($"CANCELED: ErrorDetails={cancellation.ErrorDetails}");
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
        public static async Task RecognizeSpeechAsync()
        {
            var config = SpeechConfig.FromSubscription("YourSubscriptionKey", "YourServiceRegion");

            using (var audioInput = AudioConfig.FromWavFileInput(@"whatstheweatherlike.wav"))
            {
                using (var recognizer = new SpeechRecognizer(config, audioInput))
                {
                    Console.WriteLine("Recognizing first result...");
                    var result = await recognizer.RecognizeOnceAsync();

                    if (result.Reason == ResultReason.RecognizedSpeech)
                    {
                        Console.WriteLine($"We recognized: {result.Text}");
                    }
                    else if (result.Reason == ResultReason.NoMatch)
                    {
                        Console.WriteLine($"NOMATCH: Speech could not be recognized.");
                    }
                    else if (result.Reason == ResultReason.Canceled)
                    {
                        var cancellation = CancellationDetails.FromResult(result);
                        Console.WriteLine($"CANCELED: Reason={cancellation.Reason}");

                        if (cancellation.Reason == CancellationReason.Error)
                        {
                            Console.WriteLine($"CANCELED: ErrorCode={cancellation.ErrorCode}");
                            Console.WriteLine($"CANCELED: ErrorDetails={cancellation.ErrorDetails}");
                            Console.WriteLine($"CANCELED: Did you update the subscription info?");
                        }
                    }
                }
            }
        }

        static void Main()
        {
            RecognizeSpeechAsync().Wait();
        }
    }
}
````

## <a name="build-and-run-your-app"></a>Erstellen und Ausführen der App

Sie können nun Ihre App erstellen und die Spracherkennung mit dem Speech-Dienst testen.

1. **Kompilieren des Codes:** Wählen Sie auf der Menüleiste von Visual Studio **Build** > **Projektmappe erstellen** aus.
2. **Starten der App:** Wählen Sie auf der Menüleiste **Debuggen** > **Debuggen starten** aus, oder drücken Sie **F5**.
3. **Starten der Erkennung:** Ihre Audiodatei wird an den Speech-Dienst gesendet, in Text transkribiert und in der Konsole gerendert.

   ```text
   Recognizing first result...
   We recognized: What's the weather like?
   ```

## <a name="next-steps"></a>Nächste Schritte

[!INCLUDE [footer](./footer.md)]
