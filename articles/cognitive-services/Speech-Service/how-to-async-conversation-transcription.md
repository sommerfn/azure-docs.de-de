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
ms.openlocfilehash: 433ea2778d99de39991565b2529e2f8eab4e13df
ms.sourcegitcommit: c22327552d62f88aeaa321189f9b9a631525027c
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 11/04/2019
ms.locfileid: "73506493"
---
# <a name="asynchronous-conversation-transcription-preview"></a>Asynchrone Unterhaltungstranskription (Vorschau)

In diesem Artikel wird die asynchrone Unterhaltungstranskription mithilfe der **RemoteConversationTranscriptionClient**-API veranschaulicht. Wenn Sie die Unterhaltungstranskription für die asynchrone Transkription konfiguriert haben und über eine `conversationId` verfügen, können Sie die mit dieser `conversationId` verbundene Transkription mit der **RemoteConversationTranscriptionClient**-API abrufen.

## <a name="asynchronous-vs-real-time--asynchronous"></a>Asynchron im Vergleich zu Echtzeit + Asynchron

Bei der asynchronen Transkription streamen Sie die Audiodaten der Unterhaltung, benötigen aber keine Transkription, die in Echtzeit zurückgegeben wird. Verwenden Sie stattdessen nach dem Senden der Audiodaten die `conversationId` von `ConversationTranscriber`, um den Status der asynchronen Transkription abzufragen. Wenn die asynchrone Transkription bereit ist, erhalten Sie ein `RemoteConversationTranscriptionResult`.

Bei „Echtzeit plus asynchron“ erhalten Sie die Transkription in Echtzeit, aber auch die Transkription durch Abfragen mit dem `conversationId` (ähnlich dem asynchronen Szenario).

Es sind zwei Schritte erforderlich, um eine asynchrone Transkription durchzuführen. Der erste Schritt ist das Hochladen der Audiodaten, wobei Sie entweder nur „Asynchron“ oder „Echtzeit plus asynchron“ auswählen. Der zweite Schritt besteht darin, die Ergebnisse der Transkription abzurufen.

## <a name="upload-the-audio"></a>Hochladen der Audiodaten

Bevor die asynchrone Transkription durchgeführt werden kann, müssen Sie die Audiodaten mit dem Microsoft Cognitive Speech Client SDK (Version 1.8.0 oder höher) an die Unterhaltungstranskription senden.

Dieser Beispielcode zeigt, wie Sie eine Unterhaltungstranskription für den ausschließlich asynchronen Modus erstellen. Zum Streamen von Audiodaten an den Transkriptor müssen Sie Audiostreamingcode hinzufügen, der von [Transkribieren von Unterhaltungen in Echtzeit mit dem Speech SDK](how-to-use-conversation-transcription-service.md) abgeleitet wurde. Informationen zu den unterstützten Plattformen und Sprachen-APIs finden Sie im Abschnitt **Einschränkungen** dieses Themas.

```java
// Create the speech config object
// Substitute real information for "YourSubscriptionKey" and "Region"
SpeechConfig speechConfig = SpeechConfig.fromSubscription("YourSubscriptionKey", "Region");
speech_config.setProperty("ConversationTranscriptionInRoomAndOnline", "true");

// Set the property for asynchronous transcription
speechConfig.setServiceProperty("transcriptionMode", "Async", ServicePropertyChannel.UriQueryParameter);

// Set the property for real-time plus asynchronous transcription
//speechConfig.setServiceProperty("transcriptionMode", "RealTimeAndAsync", ServicePropertyChannel.UriQueryParameter);

// Do rest of the things as explained in how to use Conversation Transcription

// pick a conversation Id that is a GUID.
Conversation conversation = new Conversation(speechConfig, conversationId);

// Create a conversation transcriber
ConversationTranscriber transcriber = new ConversationTranscriber(AudioConfig.fromDefaultMicrophoneInput());

// join a conversation
transcriber.joinConversationAsync(conversation).get();

// stream audio as shown in “Transcribe conversations in real time”
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

Für den hier gezeigten Code benötigen Sie **remoteconversation-client-sdk Version 1.0.0**, die nur für Java (Version 1.8 oder höher) unter Windows, Linux und Android (API-Ebene 26 oder höher) unterstützt wird.

### <a name="obtaining-the-client-sdk"></a>Abrufen des Client-SDK

Sie können **remoteconversation-client-sdk** abrufen, indem Sie Ihre pom.xml-Datei wie folgt bearbeiten.

- Erstellen Sie vor dem schließenden Tag `</project>` am Ende der Datei ein `repositories`-Element mit einem Verweis auf das Maven-Repository für das Speech SDK:

  ```xml
  <repositories>
    <repository>
      <id>maven-cognitiveservices-speech</id>
      <name>Microsoft Cognitive Services Speech Maven Repository</name>
      <url>https://csspeechstorage.blob.core.windows.net/maven/</url>
    </repository>
  </repositories>
  ```

- Fügen Sie auch ein `dependencies`-Element mit dem „remoteconversation-client-sdk 1.0.0“ als Abhängigkeit hinzu:

  ```xml
  <dependencies>
    <dependency>
      <groupId>com.microsoft.cognitiveservices.speech.remoteconversation</groupId>
      <artifactId>remoteconversation-client-sdk</artifactId>
      <version>1.0.0</version>
    </dependency>
  </dependencies>
  ```

- Speichern der Änderungen

### <a name="sample-transcription-code"></a>Beispieltranskriptionscode

Nachdem Sie über die `conversationId` verfügen, erstellen Sie ein Remotevorgangsobjekt **RemoteConversationTranscriptionOperation** auf dem Client, um den Status der asynchronen Transkription abzufragen. **RemoteConversationTranscriptionOperation** wird von [Poller](https://github.com/Azure/azure-sdk-for-java/blob/master/sdk/core/azure-core/src/main/java/com/azure/core/util/polling/Poller.java) erweitert. Sobald der Poller fertig ist, erhalten Sie **RemoteConversationTranscriptionResult**, indem Sie den Poller abonnieren und das Objekt abfragen. In diesem Code geben wir einfach den Ergebnisinhalt über die Systemausgabe aus.

```java
// Create the speech config object
SpeechConfig speechConfig = SpeechConfig.fromSubscription("YourSubscriptionKey", "Region");

// Create a remote Conversation Transcription client
RemoteConversationTranscriptionClient client = new RemoteConversationTranscriptionClient(speechConfig);

// Create a remote Conversation Transcription operation
RemoteConversationTranscriptionOperation operation = new RemoteConversationTranscriptionOperation(conversationId, client);

// Operation identifier now be the value of `conversationId`
System.out.println("Operation Identifier:" + operation.getId());

// Get the observer (which is a Flux) and subscribe to it for the response
operation.getObserver()
                .subscribe(
                        pollResponse -> {
                            System.out.println("Poll response status : " + pollResponse.getStatus());
                            if(pollResponse.getValue().getConversationTranscriptionResults() != null) {
                                for (int i = 0; i < pollResponse.getValue().getConversationTranscriptionResults().size(); i++) {
                                    ConversationTranscriptionResult result = pollResponse.getValue().getConversationTranscriptionResults().get(i);
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
                );
// Block on the operation till it is finished
operation.block();

System.out.println("Operation finished");
```

## <a name="next-steps"></a>Nächste Schritte

> [!div class="nextstepaction"]
> [Erkunden unserer Beispiele auf GitHub](https://aka.ms/csspeech/samples)
