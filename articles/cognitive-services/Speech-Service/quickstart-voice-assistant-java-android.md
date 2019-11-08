---
title: 'Schnellstart: Benutzerdefinierter Sprach-Assistent, Java (Android) – Speech-Dienst'
titleSuffix: Azure Cognitive Services
description: Es wird beschrieben, wie Sie in Java unter Android mit dem Speech SDK eine Sprach-Assistent-Anwendung erstellen.
services: cognitive-services
author: trrwilson
manager: nitinme
ms.service: cognitive-services
ms.subservice: speech-service
ms.topic: quickstart
ms.date: 07/05/2019
ms.author: travisw
ms.openlocfilehash: 1de9283e1ad609a1f81d7e8a6d8171cc8052e3ae
ms.sourcegitcommit: c22327552d62f88aeaa321189f9b9a631525027c
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 11/04/2019
ms.locfileid: "73506211"
---
# <a name="quickstart-create-a-voice-assistant-in-java-on-android-by-using-the-speech-sdk"></a>Schnellstart: Erstellen eines Sprach-Assistenten in Java unter Android mit dem Speech SDK

Schnellstarts sind auch für [Spracherkennung](~/articles/cognitive-services/Speech-Service/quickstarts/speech-to-text-from-microphone.md?pivots=programming-language-java&tabs=android) und [Sprachsynthese](~/articles/cognitive-services/Speech-Service/quickstarts/text-to-speech.md?pivots=programming-language-java&tabs=android) verfügbar.

In diesem Artikel erstellen Sie einen Sprach-Assistenten in Java unter Android mit dem [Speech SDK](speech-sdk.md). Diese Anwendung verbindet sich mit einem Bot, den Sie bereits mit dem [Kanal „Direct Line Speech“](https://docs.microsoft.com/azure/bot-service/bot-service-channel-connect-directlinespeech) erstellt und konfiguriert haben. Sie sendet anschließend eine Sprachanforderung an den Bot und präsentiert eine sprachaktivierte Antwortaktivität.

Die Anwendung basiert auf dem Maven-Paket für das Speech SDK sowie auf Android Studio 3.3. Das Speech SDK ist zurzeit kompatibel mit Android-Geräten mit 32-/64-Bit-ARM-Prozessoren und Prozessoren, die mit Intel x86/x64 kompatibel sind.

> [!NOTE]
> Weitere Informationen zum SDK für sprachaktivierte Geräte und zum Roobo-Gerät finden Sie unter [SDK für sprachaktivierte Geräte](speech-devices-sdk.md).

## <a name="prerequisites"></a>Voraussetzungen

* Azure-Abonnementschlüssel für Spracherkennungsdienste. [Beziehen Sie einen kostenlos](get-started.md), oder erstellen Sie ihn im [Azure-Portal](https://portal.azure.com).
* Ein zuvor erstellter, mit dem [Direct Line Speech-Kanal](https://docs.microsoft.com/azure/bot-service/bot-service-channel-connect-directlinespeech) konfigurierter Bot
* [Android Studio 3.3](https://developer.android.com/studio/) oder höher

    > [!NOTE]
    > Beachten Sie [die Liste mit den unterstützten Regionen für Sprach-Assistenten](regions.md#voice-assistants), und stellen Sie sicher, dass Ihre Ressourcen in einer dieser Regionen bereitgestellt werden.

## <a name="create-and-configure-a-project"></a>Erstellen und Konfigurieren eines Projekts

[!INCLUDE [](../../../includes/cognitive-services-speech-service-quickstart-java-android-create-proj.md)]

## <a name="create-user-interface"></a>Erstellen der Benutzeroberfläche

In diesem Abschnitt erstellen Sie eine einfache Benutzeroberfläche für die Anwendung. Öffnen Sie zunächst die Hauptaktivität: `activity_main.xml`. Die einfache Vorlage umfasst eine Titelleiste mit den Namen der Anwendung und ein `TextView`-Element mit der Meldung „Hello World!“.

Ersetzen Sie den Inhalt von `activity_main.xml` durch folgenden Code:

   ```xml
   <?xml version="1.0" encoding="utf-8"?>
   <LinearLayout xmlns:android="http://schemas.android.com/apk/res/android"
    xmlns:tools="http://schemas.android.com/tools"
    android:layout_width="match_parent"
    android:layout_height="match_parent"
    android:orientation="vertical"
    tools:context=".MainActivity">

    <Button
        android:id="@+id/button"
        android:layout_width="wrap_content"
        android:layout_height="wrap_content"
        android:layout_gravity="center"
        android:onClick="onBotButtonClicked"
        android:text="Talk to your bot" />

    <TextView
        android:layout_width="match_parent"
        android:layout_height="wrap_content"
        android:text="Recognition Data"
        android:textSize="18dp"
        android:textStyle="bold" />

    <TextView
        android:id="@+id/recoText"
        android:layout_width="match_parent"
        android:layout_height="wrap_content"
        android:text="  \n(Recognition goes here)\n" />

    <TextView
        android:layout_width="match_parent"
        android:layout_height="wrap_content"
        android:text="Activity Data"
        android:textSize="18dp"
        android:textStyle="bold" />

    <TextView
        android:id="@+id/activityText"
        android:layout_width="match_parent"
        android:layout_height="match_parent"
        android:scrollbars="vertical"
        android:text="  \n(Activities go here)\n" />

   </LinearLayout>
   ```

Dieser XML-Code definiert eine einfache Benutzeroberfläche für die Interaktion mit Ihrem Bot.

* Das `button`-Element löst eine Interaktion aus und ruft beim Klicken darauf die `onBotButtonClicked`-Methode auf.
* Das `recoText`-Element zeigt die Ergebnisse der Spracherkennung, wenn Sie sich mit Ihrem Bot unterhalten.
* Das `activityText`-Element zeigt die JSON-Nutzlast der neuesten Bot Framework-Aktivität in Ihren Bot.

Der Text und die grafische Darstellung Ihrer Benutzeroberfläche sollten jetzt etwa wie folgt aussehen:

![](media/sdk/qs-java-android-assistant-designer-ui.png)

## <a name="add-sample-code"></a>Hinzufügen von Beispielcode

1. Öffnen Sie `MainActivity.java`, und ersetzen Sie den Inhalt durch den folgenden Code:

   ```java
    package samples.speech.cognitiveservices.microsoft.com;

    import android.media.AudioFormat;
    import android.media.AudioManager;
    import android.media.AudioTrack;
    import android.support.v4.app.ActivityCompat;
    import android.support.v7.app.AppCompatActivity;
    import android.os.Bundle;
    import android.text.method.ScrollingMovementMethod;
    import android.view.View;
    import android.widget.TextView;

    import com.microsoft.cognitiveservices.speech.audio.AudioConfig;
    import com.microsoft.cognitiveservices.speech.audio.PullAudioOutputStream;
    import com.microsoft.cognitiveservices.speech.dialog.DialogServiceConfig;
    import com.microsoft.cognitiveservices.speech.dialog.DialogServiceConnector;

    import org.json.JSONException;
    import org.json.JSONObject;

    import static android.Manifest.permission.*;

    public class MainActivity extends AppCompatActivity {
        // Replace below with your own speech subscription key
        private static String speechSubscriptionKey = "YourSpeechSubscriptionKey";
        // Replace below with your own speech service region
        private static String serviceRegion = "YourSpeechServiceRegion";

        private DialogServiceConnector connector;

        @Override
        protected void onCreate(Bundle savedInstanceState) {
            super.onCreate(savedInstanceState);
            setContentView(R.layout.activity_main);

            TextView recoText = (TextView) this.findViewById(R.id.recoText);
            TextView activityText = (TextView) this.findViewById(R.id.activityText);
            recoText.setMovementMethod(new ScrollingMovementMethod());
            activityText.setMovementMethod(new ScrollingMovementMethod());

            // Note: we need to request permissions for audio input and network access
            int requestCode = 5; // unique code for the permission request
            ActivityCompat.requestPermissions(MainActivity.this, new String[]{RECORD_AUDIO, INTERNET}, requestCode);
        }

        public void onBotButtonClicked(View v) {
            // Recreate the DialogServiceConnector on each button press, ensuring that the existing one is closed
            if (connector != null) {
                connector.close();
                connector = null;
            }

            // Create the DialogServiceConnector from speech subscription information
            BotFrameworkConfig config = BotFrameworkConfig.fromSubscription(speechSubscriptionKey, serviceRegion);
            connector = new DialogServiceConnector(config, AudioConfig.fromDefaultMicrophoneInput());

            // Optional step: preemptively connect to reduce first interaction latency
            connector.connectAsync();

            // Register the DialogServiceConnector's event listeners
            registerEventListeners();

            // Begin sending audio to your bot
            connector.listenOnceAsync();
        }

        private void registerEventListeners() {
            TextView recoText = (TextView) this.findViewById(R.id.recoText); // 'recoText' is the ID of your text view
            TextView activityText = (TextView) this.findViewById(R.id.activityText); // 'activityText' is the ID of your text view

            // Recognizing will provide the intermediate recognized text while an audio stream is being processed
            connector.recognizing.addEventListener((o, recoArgs) -> {
                recoText.setText("  Recognizing: " + recoArgs.getResult().getText());
            });

            // Recognized will provide the final recognized text once audio capture is completed
            connector.recognized.addEventListener((o, recoArgs) -> {
                recoText.setText("  Recognized: " + recoArgs.getResult().getText());
            });

            // SessionStarted will notify when audio begins flowing to the service for a turn
            connector.sessionStarted.addEventListener((o, sessionArgs) -> {
                recoText.setText("Listening...");
            });

            // SessionStopped will notify when a turn is complete and it's safe to begin listening again
            connector.sessionStopped.addEventListener((o, sessionArgs) -> {
            });

            // Canceled will be signaled when a turn is aborted or experiences an error condition
            connector.canceled.addEventListener((o, canceledArgs) -> {
                recoText.setText("Canceled (" + canceledArgs.getReason().toString() + ") error details: {}" + canceledArgs.getErrorDetails());
                connector.disconnectAsync();
            });

            // ActivityReceived is the main way your bot will communicate with the client and uses bot framework activities.
            connector.activityReceived.addEventListener((o, activityArgs) -> {
                try {
                    // Here we use JSONObject only to "pretty print" the condensed Activity JSON
                    String rawActivity = activityArgs.getActivity().serialize();
                    String formattedActivity = new JSONObject(rawActivity).toString(2);
                    activityText.setText(formattedActivity);
                } catch (JSONException e) {
                    activityText.setText("Couldn't format activity text: " + e.getMessage());
                }

                if (activityArgs.hasAudio()) {
                    // Text-to-speech audio associated with the activity is 16 kHz 16-bit mono PCM data
                    final int sampleRate = 16000;
                    int bufferSize = AudioTrack.getMinBufferSize(sampleRate, AudioFormat.CHANNEL_OUT_MONO, AudioFormat.ENCODING_PCM_16BIT);

                    AudioTrack track = new AudioTrack(
                            AudioManager.STREAM_MUSIC,
                            sampleRate,
                            AudioFormat.CHANNEL_OUT_MONO,
                            AudioFormat.ENCODING_PCM_16BIT,
                            bufferSize,
                            AudioTrack.MODE_STREAM);

                    track.play();

                    PullAudioOutputStream stream = activityArgs.getAudio();

                    // Audio is streamed as it becomes available. Play it as it arrives.
                    byte[] buffer = new byte[bufferSize];
                    long bytesRead = 0;

                    do {
                        bytesRead = stream.read(buffer);
                        track.write(buffer, 0, (int) bytesRead);
                    } while (bytesRead == bufferSize);

                    track.release();
                }
            });
        }
    }
   ```

   * Die `onCreate`-Methode enthält Code, der Mikrofon- und Internetberechtigungen anfordert.

   * Die Methode `onBotButtonClicked` ist (wie bereits erwähnt) der Klickhandler für die Schaltfläche. Ein Klick auf die Schaltfläche löst eine einzelne Interaktion („Turn“) mit Ihrem Bot aus.

   * Die `registerEventListeners`-Methode veranschaulicht die Ereignisse, die vom `DialogServiceConnector` verwendet werden, und die grundlegende Verarbeitung eingehender Aktivitäten.

1. Ersetzen Sie in der gleichen Datei die Konfigurationszeichenfolgen entsprechend Ihren Ressourcen:

    * Ersetzen Sie `YourChannelSecret` durch das Geheimnis des Direct Line Speech-Kanals Ihres Bots.

    * Ersetzen Sie `YourSpeechSubscriptionKey` durch Ihren Abonnementschlüssel.

    * Ersetzen Sie `YourServiceRegion` durch die [Region](regions.md), die Ihrem Abonnement zugeordnet ist. Nur eine Teilmenge der Regionen mit Unterstützung der Spracherkennungsdienste wird derzeit mit Direct Line Speech unterstützt. Weitere Informationen finden Sie unter [Regionen](regions.md#voice-assistants).

## <a name="build-and-run-the-app"></a>Erstellen und Ausführen der App

1. Verbinden Sie Ihr Android-Gerät mit Ihrem Entwicklungs-PC. Stellen Sie sicher, dass Sie den [Entwicklungsmodus und USB-Debuggen](https://developer.android.com/studio/debug/dev-options) für das Gerät aktiviert haben.

1. Um die Anwendung zu erstellen, drücken Sie STRG+F9, oder wählen Sie **Erstellen** > **Projekt erstellen** in der Menüleiste aus.

1. Um die Anwendung zu starten, drücken Sie UMSCHALT+F10, oder wählen Sie **Ausführen** >  **'App' ausführen** aus.

1. Wählen Sie im daraufhin angezeigten Fenster mit Bereitstellungszielen Ihr Android-Gerät aus.

   ![Screenshot des Fensters „Bereitstellungsziel auswählen“](media/sdk/qs-java-android-12-deploy.png)

Sobald die Anwendung und ihre Aktivität gestartet wurden, klicken Sie auf die Schaltfläche, um mit Ihrem Bot zu sprechen. Während des Gesprächs wird transkribierter Text angezeigt. Die letzte Aktivität, die Sie von Ihrem Bot erhalten haben, wird angezeigt, sobald sie empfangen wird. Wenn Ihr Bot so konfiguriert ist, dass er gesprochene Antworten liefert, wird die Spracherkennung automatisch wiedergegeben.

![Screenshot der Android-Anwendung](media/sdk/qs-java-android-assistant-completed-turn.png)

## <a name="next-steps"></a>Nächste Schritte

> [!div class="nextstepaction"]
> [Erstellen und Bereitstellen eines Basisbots](https://docs.microsoft.com/azure/bot-service/bot-builder-tutorial-basic-deploy?view=azure-bot-service-4.0)

## <a name="see-also"></a>Weitere Informationen
- [Informationen zu Sprach-Assistenten](voice-assistants.md)
- [Beziehen eines kostenlosen Abonnementschlüssels für die Spracherkennungsdienste](get-started.md)
- [Benutzerdefinierte Schlüsselwörter](speech-devices-sdk-create-kws.md)
- [Verbinden von Direct Line Speech mit Ihrem Bot](https://docs.microsoft.com/azure/bot-service/bot-service-channel-connect-directlinespeech)
- [Java-Beispiele auf GitHub](https://aka.ms/csspeech/samples)
