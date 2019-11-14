---
title: Asynchrone Unterhaltungstranskription (Vorschau) – Spracherkennungsdienst
titleSuffix: Azure Cognitive Services
description: Hier erfahren Sie, wie Sie die asynchrone Unterhaltungstranskription mit dem Spracherkennungsdienst verwenden. Nur für Java verfügbar.
services: cognitive-services
author: markamos
manager: nitinme
ms.service: cognitive-services
ms.subservice: speech-service
ms.topic: conceptual
ms.date: 11/04/2019
ms.author: amishu
ms.openlocfilehash: 9a2a00a8c76c7b5c02cde623bf93f536b27cf074
ms.sourcegitcommit: 6c2c97445f5d44c5b5974a5beb51a8733b0c2be7
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 11/05/2019
ms.locfileid: "73621950"
---
# <a name="asynchronous-conversation-transcription-preview"></a>Asynchrone Unterhaltungstranskription (Vorschau)

In diesem Artikel wird die asynchrone Unterhaltungstranskription mithilfe der **RemoteConversationTranscriptionClient**-API veranschaulicht. Wenn Sie die Unterhaltungstranskription für die asynchrone Transkription konfiguriert haben und über eine `conversationId` verfügen, können Sie die mit dieser `conversationId` verbundene Transkription mit der **RemoteConversationTranscriptionClient**-API abrufen.

## <a name="asynchronous-vs-real-time--asynchronous"></a>Asynchron im Vergleich zu Echtzeit + Asynchron

Bei der asynchronen Transkription streamen Sie die Audiodaten der Unterhaltung, benötigen aber keine Transkription, die in Echtzeit zurückgegeben wird. Verwenden Sie stattdessen nach dem Senden der Audiodaten die `conversationId` von `Conversation`, um den Status der asynchronen Transkription abzufragen. Wenn die asynchrone Transkription bereit ist, erhalten Sie ein `RemoteConversationTranscriptionResult`.

Bei „Echtzeit plus asynchron“ erhalten Sie die Transkription in Echtzeit, aber auch die Transkription durch Abfragen mit dem `conversationId` (ähnlich dem asynchronen Szenario).

Es sind zwei Schritte erforderlich, um eine asynchrone Transkription durchzuführen. Der erste Schritt ist das Hochladen der Audiodaten, wobei Sie entweder nur „Asynchron“ oder „Echtzeit plus asynchron“ auswählen. Der zweite Schritt besteht darin, die Ergebnisse der Transkription abzurufen.

## <a name="upload-the-audio"></a>Hochladen der Audiodaten

Damit die asynchrone Transkription durchgeführt werden kann, müssen Sie die Audiodaten zunächst mit dem Microsoft Cognitive Speech Client SDK (ab Version 1.8.0) an den Dienst für Unterhaltungstranskription senden.

Dieser Beispielcode zeigt, wie Sie eine Unterhaltungstranskription für den ausschließlich asynchronen Modus erstellen. Zum Streamen von Audiodaten an den Transkriptor müssen Sie Audiostreamingcode hinzufügen, der von [Transkribieren von Unterhaltungen in Echtzeit mit dem Speech SDK](how-to-use-conversation-transcription-service.md) abgeleitet wurde. Informationen zu den unterstützten Plattformen und Sprachen-APIs finden Sie im Abschnitt **Einschränkungen** dieses Themas.

```java
// Create the speech config object
// Substitute real information for "YourSubscriptionKey" and "Region"
SpeechConfig speechConfig = SpeechConfig.fromSubscription("YourSubscriptionKey", "Region");
speechConfig.setProperty("ConversationTranscriptionInRoomAndOnline", "true");

// Set the property for asynchronous transcription
speechConfig.setServiceProperty("transcriptionMode", "Async", ServicePropertyChannel.UriQueryParameter);

// Set the property for real-time plus asynchronous transcription
//speechConfig.setServiceProperty("transcriptionMode", "RealTimeAndAsync", ServicePropertyChannel.UriQueryParameter);

// pick a conversation Id that is a GUID.
String conversationId = UUID.randomUUID().toString();

// Create a Conversation
Conversation conversation = new Conversation(speechConfig, conversationId);

// Create an audio stream from a wav file or from the default microphone if you want to stream live audio from the supported devices
// Replace with your own audio file name and Helper class which implements AudioConfig using PullAudioInputStreamCallback
PullAudioInputStreamCallback wavfilePullStreamCallback = Helper.OpenWavFile("16Khz16Bits8channelsOfRecordedPCMAudio.wav");
// Create an audio stream format assuming the file used above is 16Khz, 16 bits and 8 channel pcm wav file
AudioStreamFormat audioStreamFormat = AudioStreamFormat.getWaveFormatPCM((long)16000, (short)16,(short)8);
// Create an input stream
AudioInputStream audioStream = AudioInputStream.createPullStream(wavfilePullStreamCallback, audioStreamFormat);

// Create a conversation transcriber
ConversationTranscriber transcriber = new ConversationTranscriber(AudioConfig.fromStreamInput(audioStream));

// join a conversation
transcriber.joinConversationAsync(conversation);

// Add the event listener for the realtime events
transcriber.transcribed.addEventListener((o, e) -> {
    System.out.println("Conversation transcriber Recognized:" + e.toString());
});

transcriber.canceled.addEventListener((o, e) -> {
    System.out.println("Conversation transcriber canceled:" + e.toString());
    try {
        transcriber.stopTranscribingAsync().get();
    } catch (InterruptedException ex) {
        ex.printStackTrace();
    } catch (ExecutionException ex) {
        ex.printStackTrace();
    }
});

transcriber.sessionStopped.addEventListener((o, e) -> {
    System.out.println("Conversation transcriber stopped:" + e.toString());

    try {
        transcriber.stopTranscribingAsync().get();
    } catch (InterruptedException ex) {
        ex.printStackTrace();
    } catch (ExecutionException ex) {
        ex.printStackTrace();
    }
});

// start the transcription.
Future<?> future = transcriber.startTranscribingAsync();
...
```

Wenn Sie „Echtzeit _plus_ asynchron“ wünschen, kommentieren Sie die entsprechenden Codezeilen wie folgt aus, oder heben Sie die Auskommentierung auf:

```java
// Set the property for asynchronous transcription
//speechConfig.setServiceProperty("transcriptionMode", "Async", ServicePropertyChannel.UriQueryParameter);

// Set the property for real-time plus asynchronous transcription
speechConfig.setServiceProperty("transcriptionMode", "RealTimeAndAsync", ServicePropertyChannel.UriQueryParameter);
```

## <a name="get-transcription-results"></a>Abrufen von Transkriptionsergebnissen

Dieser Schritt ruft die asynchronen Transkriptionsergebnisse ab, geht aber davon aus, dass jede von Ihnen benötigte Echtzeitverarbeitung an anderer Stelle durchgeführt wird. Weitere Informationen finden Sie unter [Transkribieren von Unterhaltungen in Echtzeit mit dem Speech SDK](how-to-use-conversation-transcription-service.md).

Für den hier gezeigten Code benötigen Sie **remote-conversation version 1.8.0**. Diese Komponente wird nur für Java (ab Version 1.8.0) unter Windows, Linux und Android (erst ab der API-Ebene 26) unterstützt.

### <a name="obtaining-the-client-sdk"></a>Abrufen des Client-SDK

Sie können **remote-conversation** abrufen, indem Sie die Datei „pom.xml“ wie folgt bearbeiten:

1. Erstellen Sie vor dem schließenden Tag `</project>` am Ende der Datei ein `repositories`-Element mit einem Verweis auf das Maven-Repository für das Speech SDK:

   ```xml
   <repositories>
     <repository>
       <id>maven-cognitiveservices-speech</id>
       <name>Microsoft Cognitive Services Speech Maven Repository</name>
       <url>https://csspeechstorage.blob.core.windows.net/maven/</url>
     </repository>
   </repositories>
   ```

2. Fügen Sie auch ein Element vom Typ `dependencies` mit „remoteconversation-client-sdk 1.8.0“ als Abhängigkeit hinzu:

   ```xml
   <dependencies>
     <dependency>
       <groupId>com.microsoft.cognitiveservices.speech.remoteconversation</groupId>
       <artifactId>remote-conversation</artifactId>
       <version>1.8.0</version>
     </dependency>
   </dependencies>
   ```

3. Speichern der Änderungen

### <a name="sample-transcription-code"></a>Beispieltranskriptionscode

Nachdem Sie über die Unterhaltungs-ID (`conversationId`) verfügen, erstellen Sie einen Remoteunterhaltungstranskriptions-Client (**RemoteConversationTranscriptionClient**) in der Clientanwendung, um den Status der asynchronen Transkription abzufragen. Verwenden Sie die Methode **getTranscriptionOperation** in **RemoteConversationTranscriptionClient**, um ein Objekt vom Typ [PollerFlux](https://github.com/Azure/azure-sdk-for-java/blob/master/sdk/core/azure-core/src/main/java/com/azure/core/util/polling/PollerFlux.java) zu erhalten. Das PollerFlux-Objekt enthält Informationen zum Status des Remotevorgangs (**RemoteConversationTranscriptionOperation**) sowie zum Endergebnis (**RemoteConversationTranscriptionResult**). Rufen Sie nach Abschluss des Vorgangs **RemoteConversationTranscriptionResult** ab, indem Sie **getFinalResult** für eine Instanz von [SyncPoller](https://github.com/Azure/azure-sdk-for-java/blob/master/sdk/core/azure-core/src/main/java/com/azure/core/util/polling/SyncPoller.java) aufrufen. In diesem Code geben wir einfach den Ergebnisinhalt über die Systemausgabe aus.

```java
// Create the speech config object
SpeechConfig speechConfig = SpeechConfig.fromSubscription("YourSubscriptionKey", "Region");

// Create a remote Conversation Transcription client
RemoteConversationTranscriptionClient client = new RemoteConversationTranscriptionClient(speechConfig);

// Get the PollerFlux for the remote operation
PollerFlux<RemoteConversationTranscriptionOperation, RemoteConversationTranscriptionResult> remoteTranscriptionOperation = client.getTranscriptionOperation(conversationId);

// Subscribe to PollerFlux to get the remote operation status
remoteTranscriptionOperation.subscribe(
        pollResponse -> {
            System.out.println("Poll response status : " + pollResponse.getStatus());
            System.out.println("Poll response status : " + pollResponse.getValue().getServiceStatus());
        }
);

// Obtain the blocking operation using getSyncPoller
SyncPoller<RemoteConversationTranscriptionOperation, RemoteConversationTranscriptionResult> blockingOperation =  remoteTranscriptionOperation.getSyncPoller();

// Wait for the operation to finish
blockingOperation.waitForCompletion();

// Get the final result response
RemoteConversationTranscriptionResult resultResponse = blockingOperation.getFinalResult();

// Print the result
if(resultResponse != null) {
    if(resultResponse.getConversationTranscriptionResults() != null) {
        for (int i = 0; i < resultResponse.getConversationTranscriptionResults().size(); i++) {
            ConversationTranscriptionResult result = resultResponse.getConversationTranscriptionResults().get(i);
            System.out.println(result.getProperties().getProperty(PropertyId.SpeechServiceResponse_JsonResult.name()));
            System.out.println(result.getProperties().getProperty(PropertyId.SpeechServiceResponse_JsonResult));
            System.out.println(result.getOffset());
            System.out.println(result.getDuration());
            System.out.println(result.getUserId());
            System.out.println(result.getReason());
            System.out.println(result.getResultId());
            System.out.println(result.getText());
            System.out.println(result.toString());
        }
    }
}

System.out.println("Operation finished");
```

## <a name="next-steps"></a>Nächste Schritte

> [!div class="nextstepaction"]
> [Erkunden unserer Beispiele auf GitHub](https://aka.ms/csspeech/samples)
