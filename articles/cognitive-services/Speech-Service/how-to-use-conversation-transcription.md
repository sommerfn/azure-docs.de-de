---
title: Unterhaltungstranskription in Echtzeit (Vorschau) – Speech Service
titleSuffix: Azure Cognitive Services
description: Erfahren Sie, wie Sie die Unterhaltungstranskription in Echtzeit mit dem Speech SDK verwenden. Verfügbar für C++, C# und Java
services: cognitive-services
author: markamos
manager: nitinme
ms.service: cognitive-services
ms.subservice: speech-service
ms.topic: conceptual
ms.date: 11/04/2019
ms.author: weixu
ms.openlocfilehash: 4ee597f96d28b43b9c69d1515634aadde2f0ab7c
ms.sourcegitcommit: c62a68ed80289d0daada860b837c31625b0fa0f0
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 11/05/2019
ms.locfileid: "73608018"
---
# <a name="real-time-conversation-transcription-preview"></a>Unterhaltungstranskription in Echtzeit (Vorschau)

Mit der **ConversationTranscriber**-API des Speech SDK können Sie Besprechungen und andere Unterhaltungen transkribieren und dabei mehrere Teilnehmer hinzufügen, entfernen und identifizieren, indem Sie Audio mithilfe von `PullStream` oder `PushStream` an die Speech Services streamen. Bei diesem Thema wird vorausgesetzt, dass Sie mit der Verwendung der Spracherkennung mit dem Speech SDK (ab Version 1.8.0) vertraut sind. Weitere Informationen finden Sie unter [Was ist Speech Services?](overview.md).

## <a name="limitations"></a>Einschränkungen

- Die ConversationTranscriber-API wird für C++, C# und Java unter Windows, Linux und Android unterstützt.
- Sie ist derzeit in den Sprachen „en-US“ und „zh-CN“ in den folgenden Regionen verfügbar: „USA, Mitte“ (_centralus_) und „Asien, Osten“ (_eastasia_).
- Erforderlich ist ein Mikrofonarray aus 7 kreisförmig angeordneten Mikrofonen mit einem Wiedergabereferenzstream. Das Mikrofonarray sollte [unsere Spezifikation](https://aka.ms/sdsdk-microphone) erfüllen.
- Das [Speech-Geräte-SDK](speech-devices-sdk.md) stellt geeignete Geräte und eine Beispiel-App zur Veranschaulichung der Unterhaltungstranskription bereit.

## <a name="optional-sample-code-resources"></a>Optionale Ressourcen mit Beispielcode

Das Speech-Geräte-SDK stellt Beispielcode in Java für die Audioerfassung in Echtzeit mit acht Kanälen bereit.

- [Beispielcode für ROOBO-Geräte](https://github.com/Azure-Samples/Cognitive-Services-Speech-Devices-SDK/blob/master/Samples/Android/Speech%20Devices%20SDK%20Starter%20App/example/app/src/main/java/com/microsoft/cognitiveservices/speech/samples/sdsdkstarterapp/Conversation.java)
- [Beispielcode für Azure Kinect DK](https://github.com/Azure-Samples/Cognitive-Services-Speech-Devices-SDK/blob/master/Samples/Windows_Linux/SampleDemo/src/com/microsoft/cognitiveservices/speech/samples/Cts.java)

## <a name="prerequisites"></a>Voraussetzungen

Ein Speech Services-Abonnement. Falls Sie keines besitzen, können Sie ein [Speech-Testabonnement](https://azure.microsoft.com/try/cognitive-services/) erhalten.

## <a name="create-voice-signatures"></a>Erstellen von Stimmsignaturen

Erstellen Sie als Erstes Stimmsignaturen für die Teilnehmer der Unterhaltung, um die Sprecheridentifikation zu verbessern.

### <a name="audio-input-requirements"></a>Audioeingabeanforderungen

- Die WAV-Datei für die Audioeingabe zum Erstellen von Stimmsignaturen muss in 16-Bit-Abtastungen, 16-kHz-Abtastrate und im Monoformat (einzelner Kanal) vorliegen.
- Die empfohlene Länge der einzelnen Audiostichproben liegt zwischen 30 Sekunden und zwei Minuten.

### <a name="sample-code"></a>Beispielcode

Das folgende Beispiel zeigt zwei verschiedene Möglichkeiten zum Erstellen der Stimmsignatur [über die REST-API](https://aka.ms/cts/signaturegenservice) in C#. Beachten Sie, dass Sie „YourSubscriptionKey“ durch Ihre tatsächlichen Informationen, „speakerVoice.wav“ durch den Namen Ihrer WAV-Datei und `{region}` und „YourServiceRegion“ durch Ihre Region (_centralus_ oder _eastasia_) ersetzen müssen.

```csharp
class Program
{
    static async Task CreateVoiceSignatureByUsingFormData()
    {
        // Replace with your own region
        var region = "YourServiceRegion";
        // Change the name of the wave file to match yours
        byte[] fileBytes = File.ReadAllBytes(@"speakerVoice.wav");
        var form = new MultipartFormDataContent();
        var content = new ByteArrayContent(fileBytes);
        form.Add(content, "file", "file");
        var client = new HttpClient();
        // Add your subscription key to the header Ocp-Apim-Subscription-Key directly
        // Replace "YourSubscriptionKey" with your own subscription key
        client.DefaultRequestHeaders.Add("Ocp-Apim-Subscription-Key", "YourSubscriptionKey");
        // Edit with your desired region for `{region}`
        var response = await client.PostAsync($"https://signature.{region}.cts.speech.microsoft.com/api/v1/Signature/GenerateVoiceSignatureFromFormData", form);
        // A voice signature contains Version, Tag and Data key values from the Signature json structure from the Response body.
        // Voice signature format example: { "Version": <Numeric value>, "Tag": "string", "Data": "string" }
        var jsonData = await response.Content.ReadAsStringAsync();
    }

    static async Task CreateVoiceSignatureByUsingBody()
    {
        // Replace with your own region
        var region = "YourServiceRegion";
        // Change the name of the wave file to match yours
        byte[] fileBytes = File.ReadAllBytes(@"speakerVoice.wav");
        var content = new ByteArrayContent(fileBytes);

        var client = new HttpClient();
        // Add your subscription key to the header Ocp-Apim-Subscription-Key directly
        // Replace "YourSubscriptionKey" with your own subscription key
        client.DefaultRequestHeaders.Add("Ocp-Apim-Subscription-Key", "YourSubscriptionKey");
        // Edit with your desired region for `{region}`
        var response = await client.PostAsync($"https://signature.{region}.cts.speech.microsoft.com/api/v1/Signature/GenerateVoiceSignatureFromByteArray", content);
        // A voice signature contains Version, Tag and Data key values from the Signature json structure from the Response body.
        // Voice signature format example: { "Version": <Numeric value>, "Tag": "string", "Data": "string" }
        var jsonData = await response.Content.ReadAsStringAsync();
    }

    static void Main(string[] args)
    {
        CreateVoiceSignatureByUsingFormData().Wait();
        CreateVoiceSignatureByUsingBody().Wait();
    }
}
```

## <a name="transcribe-conversations"></a>Transkribieren von Konversationen

Der folgende Beispielcode veranschaulicht das Transkribieren von Konversationen in Echtzeit für drei Sprecher. Dabei wird davon ausgegangen, dass Sie bereits für alle Sprecher wie oben gezeigt Stimmsignaturen erstellt haben. Ersetzen Sie bei der Erstellung des SpeechConfig-Objekts „YourSubscriptionKey“ und „YourServiceRegion“ durch echte Informationen.

Wichtige Aspekte im Beispielcode:

- Erstellen eines `Conversation`-Objekts aus dem `SpeechConfig`-Objekt mithilfe eines Besprechungsbezeichners, der mithilfe von `Guid.NewGuid()` generiert wurde
- Erstellen eines `ConversationTranscriber`-Objekts und Verknüpfen der Konversation mit `JoinConversationAsync()`, um die Transkription zu starten
- Registrieren der relevanten Ereignisse
- Hinzufügen oder Entfernen von Teilnehmern an der Konversation mithilfe des Conversation-Objekts
- Streamen der Audioinhalte

Die Transkriptions- und Sprecherbezeichner werden in den registrierten Ereignissen zurückgegeben.

```csharp
using Microsoft.CognitiveServices.Speech;
using Microsoft.CognitiveServices.Speech.Audio;
using Microsoft.CognitiveServices.Speech.Transcription;

public class MyConversationTranscriber
{
    public static async Task ConversationWithPullAudioStreamAsync()
    {
        // Creates an instance of a speech config with specified subscription key and service region
        // Replace with your own subscription key and region
        var config = SpeechConfig.FromSubscription("YourSubscriptionKey", "YourServiceRegion");
        config.SetProperty("ConversationTranscriptionInRoomAndOnline", "true");
        var stopTranscription = new TaskCompletionSource<int>();

        // Create an audio stream from a wav file or from the default microphone if you want to stream live audio from the supported devices
        // Replace with your own audio file name and Helper class which implements AudioConfig using PullAudioInputStreamCallback
        using (var audioInput = Helper.OpenWavFile(@"8channelsOfRecordedPCMAudio.wav"))
        {
            var meetingId = Guid.NewGuid().ToString();
            using (var conversation = new Conversation(config, meetingId))
            {
                // Create a conversation transcriber using audio stream input
                using (var conversationTranscriber = new ConversationTranscriber    (audioInput))
                {
                    await conversationTranscriber.JoinConversationAsync(conversation);

                    // Subscribe to events
                    conversationTranscriber.Transcribing += (s, e) =>
                    {
                            Console.WriteLine($"TRANSCRIBING: Text={e.Result.Text}");
                    };

                    conversationTranscriber.Transcribed += (s, e) =>
                    {
                        if (e.Result.Reason == ResultReason.RecognizedSpeech)
                        {
                            Console.WriteLine($"TRANSCRIBED: Text={e.Result.Text}, UserID={e.Result.UserId}");
                        }
                        else if (e.Result.Reason == ResultReason.NoMatch)
                        {
                            Console.WriteLine($"NOMATCH: Speech could not be recognized.");
                        }
                    };

                    conversationTranscriber.Canceled += (s, e) =>
                    {
                        Console.WriteLine($"CANCELED: Reason={e.Reason}");

                        if (e.Reason == CancellationReason.Error)
                        {
                            Console.WriteLine($"CANCELED: ErrorCode={e.ErrorCode}");
                            Console.WriteLine($"CANCELED: ErrorDetails={e.ErrorDetails}");
                            Console.WriteLine($"CANCELED: Did you update the subscription info?");
                            stopTranscription.TrySetResult(0);
                        }
                    };

                    conversationTranscriber.SessionStarted += (s, e) =>
                    {
                        Console.WriteLine("\nSession started event.");
                    };

                    conversationTranscriber.SessionStopped += (s, e) =>
                    {
                        Console.WriteLine("\nSession stopped event.");
                        Console.WriteLine("\nStop recognition.");
                        stopTranscription.TrySetResult(0);
                    };

                    // Add participants to the conversation.
                    // Create voice signatures using REST API described in the earlier section in this document.
                    // Voice signature needs to be in the following format:
                    // { "Version": <Numeric value>, "Tag": "string", "Data": "string" }

                    var speakerA = Participant.From("Speaker_A", "en-us", signatureA);
                    var speakerB = Participant.From("Speaker_B", "en-us", signatureB);
                    var speakerC = Participant.From("SPeaker_C", "en-us", signatureC);
                    await conversation.AddParticipantAsync(speakerA);
                    await conversation.AddParticipantAsync(speakerB);
                    await conversation.AddParticipantAsync(speakerC);

                    // Starts transcribing of the conversation. Uses StopTranscribingAsync() to stop transcribing when all participants leave.
                    await conversationTranscriber.StartTranscribingAsync().ConfigureAwait(false);

                    // Waits for completion.
                    // Use Task.WaitAny to keep the task rooted.
                    Task.WaitAny(new[] { stopTranscription.Task });

                    // Stop transcribing the conversation.
                    await conversationTranscriber.StopTranscribingAsync().ConfigureAwait(false);
                 }
            }
       }
    }
}
```

## <a name="next-steps"></a>Nächste Schritte

> [!div class="nextstepaction"]
> [Asynchrone Unterhaltungstranskription](how-to-async-conversation-transcription.md)
