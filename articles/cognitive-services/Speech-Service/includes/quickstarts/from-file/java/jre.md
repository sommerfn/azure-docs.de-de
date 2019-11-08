---
author: erhopf
ms.service: cognitive-services
ms.topic: include
ms.date: 08/06/2019
ms.author: erhopf
ms.openlocfilehash: 0ba76ed2bd7d6259307e2e8046c4695786ff3045
ms.sourcegitcommit: c22327552d62f88aeaa321189f9b9a631525027c
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 11/04/2019
ms.locfileid: "73506411"
---
## <a name="prerequisites"></a>Voraussetzungen

> [!div class="checklist"]
> * [Erstellen einer Azure Speech-Ressource](../../../../get-started.md)
> * [Einrichten Ihrer Entwicklungsumgebung](../../../../quickstarts/setup-platform.md?tabs=jre)
> * [Erstellen eines leeren Beispielprojekts](../../../../quickstarts/create-project.md?tabs=jre)

## <a name="add-sample-code"></a>Hinzufügen von Beispielcode

1. Um Ihrem Java-Projekt eine neue leere Klasse hinzuzufügen, wählen Sie **Datei** > **Neu** > **Klasse** aus.

1. Geben Sie im Fenster **Neue Java-Klasse** **speechsdk.quickstart** in das Feld **Paket** und **Main** in das Feld **Name** ein.

   ![Screenshot des Fensters „Neue Java-Klasse“](~/articles/cognitive-services/Speech-Service/media/sdk/qs-java-jre-06-create-main-java.png)

1. Ersetzen Sie den Code in `Main.java` mit dem folgenden Ausschnitt:

   ```Java

   package speechsdk.quickstart;

   import java.util.concurrent.Future;
   import com.microsoft.cognitiveservices.speech.*;

   /**
    * Quickstart: recognize speech using the Speech SDK for Java.
    */
   public class Main {

       /**
        * @param args Arguments are ignored in this sample.
        */
       public static void main(String[] args) {
           try {
               // Replace below with your own subscription key
               String speechSubscriptionKey = "YourSubscriptionKey";
               // Replace below with your own service region (e.g., "westus").
               String serviceRegion = "YourServiceRegion";
               // Replace below with your own filename.
               String audioFileName = "whatstheweatherlike.wav";

               int exitCode = 1;
               SpeechConfig config = SpeechConfig.fromSubscription(speechSubscriptionKey, serviceRegion);
               assert(config != null);

               AudioConfig audioInput = AudioConfig.fromWavFileInput(audioFileName);
               assert(audioInput != null);

               SpeechRecognizer reco = new SpeechRecognizer(config, audioInput);
               assert(reco != null);

               System.out.println("Recognizing first result...");

               Future<SpeechRecognitionResult> task = reco.recognizeOnceAsync();
               assert(task != null);

               SpeechRecognitionResult result = task.get();
               assert(result != null);

               if (result.getReason() == ResultReason.RecognizedSpeech) {
                   System.out.println("We recognized: " + result.getText());
                   exitCode = 0;
               }
               else if (result.getReason() == ResultReason.NoMatch) {
                   System.out.println("NOMATCH: Speech could not be recognized.");
               }
               else if (result.getReason() == ResultReason.Canceled) {
                   CancellationDetails cancellation = CancellationDetails.fromResult(result);
                   System.out.println("CANCELED: Reason=" + cancellation.getReason());

                   if (cancellation.getReason() == CancellationReason.Error) {
                       System.out.println("CANCELED: ErrorCode=" + cancellation.getErrorCode());
                       System.out.println("CANCELED: ErrorDetails=" + cancellation.getErrorDetails());
                       System.out.println("CANCELED: Did you update the subscription info?");
                   }
               }

               reco.close();

               System.exit(exitCode);
           } catch (Exception ex) {
               System.out.println("Unexpected exception: " + ex.getMessage());

               assert(false);
               System.exit(1);
           }
       }
   }
   ```

1. Ersetzen Sie die Zeichenfolge `YourSubscriptionKey` durch Ihren Abonnementschlüssel.

1. Ersetzen Sie die Zeichenfolge `YourServiceRegion` durch die [Region](~/articles/cognitive-services/Speech-Service/regions.md), die mit Ihrem Abonnement verknüpft ist (z. B. `westus` für das kostenlose Testabonnement).

1. Ersetzen Sie die Zeichenfolge `whatstheweatherlike.wav` durch ihren eigenen Dateinamen.

1. Speichern Sie die Änderungen am Projekt.

## <a name="build-and-run-the-app"></a>Erstellen und Ausführen der App

Drücken Sie F11, oder wählen Sie **Ausführen** > **Debuggen** aus.
Die ersten 15 Sekunden der Spracheingabe von Ihrer Audiodatei werden erkannt und im Konsolenfenster protokolliert.

   ```text
   Recognizing first result...
   We recognized: What's the weather like?
   ```

## <a name="next-steps"></a>Nächste Schritte

[!INCLUDE [footer](./footer.md)]