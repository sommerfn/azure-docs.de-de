---
title: 'Schnellstart: Benutzerdefinierter Sprach-Assistent (Vorschauversion), Java (Windows, Linux) – Speech-Dienst'
titleSuffix: Azure Cognitive Services
description: In dieser Schnellstartanleitung erfahren Sie, wie Sie das Cognitive Services Speech SDK in einer Java-Konsolenanwendung verwenden. Es wird beschrieben, wie Sie Ihre Clientanwendung mit einem zuvor erstellten Bot Framework-Bot verbinden, der für die Verwendung des Direct Line Speech-Kanals konfiguriert ist, und einen Sprach-Assistenten implementieren.
services: cognitive-services
author: bidishac
manager: nitinme
ms.service: cognitive-services
ms.subservice: speech-service
ms.topic: quickstart
ms.date: 11/05/2019
ms.author: bidishac
ms.openlocfilehash: 2fbe2e38ae1f41c2559b5ef5c6f66d19f3c41dae
ms.sourcegitcommit: c22327552d62f88aeaa321189f9b9a631525027c
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 11/04/2019
ms.locfileid: "73506203"
---
# <a name="quickstart-create-a-voice-assistant-with-the-speech-sdk-java"></a>Schnellstart: Erstellen eines Sprach-Assistenten mit dem Speech SDK – Java

Schnellstartanleitungen sind auch für [Spracherkennung](~/articles/cognitive-services/Speech-Service/quickstarts/speech-to-text-from-microphone.md?pivots=programming-language-java&tabs=jre), [Sprachsynthese](~/articles/cognitive-services/Speech-Service/quickstarts/text-to-speech.md?pivots=programming-language-java&tabs=jre) und [Sprachübersetzung](~/articles/cognitive-services/Speech-Service/quickstarts/translate-speech-to-text.md?pivots=programming-language-java&tabs=jre) verfügbar.

In diesem Artikel erstellen Sie eine Java-Konsolenanwendung unter Verwendung des [Azure Cognitive Services Speech SDK](speech-sdk.md). Die Anwendung stellt eine Verbindung mit einem zuvor erstellten Bot her, der für die Verwendung des Direct Line Speech-Kanals konfiguriert ist, sendet eine Sprachanforderung und gibt eine Sprachantwortaktivität zurück (sofern konfiguriert). Die Anwendung wird mit dem Maven-Paket für das Speech SDK sowie mit der Eclipse Java-IDE unter Windows, Ubuntu Linux oder macOS erstellt. Sie wird in einer 64-Bit Java 8-JRE (Runtime Environment) ausgeführt.

## <a name="prerequisites"></a>Voraussetzungen

Für diese Schnellstartanleitung ist Folgendes erforderlich:

* Betriebssystem: Windows (64 Bit), Ubuntu Linux 16.04/18.04 (64 Bit) oder macOS (ab Version 10.13)
* [Eclipse Java IDE](https://www.eclipse.org/downloads/)
* [Java 8](https://www.oracle.com/technetwork/java/javase/downloads/jre8-downloads-2133155.html) oder [JDK 8](https://www.oracle.com/technetwork/java/javase/downloads/index.html)
* Azure-Abonnementschlüssel für Spracherkennungsdienste. [Beziehen Sie einen kostenlosen Schlüssel](get-started.md), oder erstellen Sie einen im [Azure-Portal](https://portal.azure.com).
* Ein vorkonfigurierter Bot, der mit der Bot Framework-Version 4.2 oder einer höheren Version erstellt wurde. Der Bot muss den neuen Direct Line Speech-Kanal abonnieren, um Spracheingaben zu empfangen.

    > [!NOTE]
    > Beachten Sie [die Liste mit den unterstützten Regionen für Sprach-Assistenten](regions.md#voice-assistants), und stellen Sie sicher, dass Ihre Ressourcen in einer dieser Regionen bereitgestellt werden.

Vergewissern Sie sich bei Verwendung von Ubuntu 16.04/18.04, dass die folgenden Abhängigkeiten installiert sind, bevor Sie Eclipse starten:

```console
sudo apt-get update
sudo apt-get install build-essential libssl1.0.0 libasound2 wget
```

Vergewissern Sie sich bei Verwendung von Windows (64 Bit), dass Microsoft Visual C++ Redistributable für Ihre Plattform installiert ist:
* [Microsoft Visual C++ Redistributable für Visual Studio 2017 herunterladen](https://support.microsoft.com/help/2977003/the-latest-supported-visual-c-downloads)

## <a name="optional-get-started-fast"></a>Optional: Schneller Einstieg

In dieser Schnellstartanleitung erfahren Sie Schritt für Schritt, wie Sie eine einfache Clientanwendung erstellen, die Sie mit Ihrem sprachaktivierten Bot verbinden können. Wenn Sie sofort loslegen möchten, finden Sie den kompilierbereiten Quellcode, der in dieser Schnellstartanleitung verwendet wird, in den [Speech SDK-Beispielen](https://aka.ms/csspeech/samples) im Ordner `quickstart`.

## <a name="create-and-configure-project"></a>Erstellen und Konfigurieren des Projekts

[!INCLUDE [](../../../includes/cognitive-services-speech-service-quickstart-java-create-proj.md)]

Fügen Sie außerdem der Datei *pom.xml* die folgende Abhängigkeit hinzu, um die Protokollierung zu aktivieren:

   ```xml
    <dependency>
        <groupId>org.slf4j</groupId>
        <artifactId>slf4j-simple</artifactId>
        <version>1.7.5</version>
    </dependency>
   ```

## <a name="add-sample-code"></a>Hinzufügen von Beispielcode

1. Um Ihrem Java-Projekt eine neue leere Klasse hinzuzufügen, wählen Sie **Datei** > **Neu** > **Klasse** aus.

1. Geben Sie im Fenster **Neue Java-Klasse** die Zeichenfolge *speechsdk.quickstart* in das Feld **Paket** und die Zeichenfolge *Main* in das Feld **Name** ein.

   ![Screenshot des Fensters „Neue Java-Klasse“](media/sdk/qs-java-jre-06-create-main-java.png)

1. Öffnen Sie die neu erstellte Klasse `Main`, und ersetzen Sie den Inhalt der Datei `Main.java` durch den folgenden Startcode:

    ```java
    package speechsdk.quickstart;

    import com.microsoft.cognitiveservices.speech.audio.AudioConfig;
    import com.microsoft.cognitiveservices.speech.audio.PullAudioOutputStream;
    import com.microsoft.cognitiveservices.speech.dialog.DialogServiceConfig;
    import com.microsoft.cognitiveservices.speech.dialog.DialogServiceConnector;
    import org.slf4j.Logger;
    import org.slf4j.LoggerFactory;

    import javax.sound.sampled.AudioFormat;
    import javax.sound.sampled.AudioSystem;
    import javax.sound.sampled.DataLine;
    import javax.sound.sampled.SourceDataLine;
    import java.io.InputStream;

    public class Main {
        final Logger log = LoggerFactory.getLogger(Main.class);

        public static void main(String[] args) {
            // New code will go here
        }

        private void playAudioStream(PullAudioOutputStream audio) {
            ActivityAudioStream stream = new ActivityAudioStream(audio);
            final ActivityAudioStream.ActivityAudioFormat audioFormat = stream.getActivityAudioFormat();
            final AudioFormat format = new AudioFormat(
                    AudioFormat.Encoding.PCM_SIGNED,
                    audioFormat.getSamplesPerSecond(),
                    audioFormat.getBitsPerSample(),
                    audioFormat.getChannels(),
                    audioFormat.getFrameSize(),
                    audioFormat.getSamplesPerSecond(),
                    false);
            try {
                int bufferSize = format.getFrameSize();
                final byte[] data = new byte[bufferSize];

                SourceDataLine.Info info = new DataLine.Info(SourceDataLine.class, format);
                SourceDataLine line = (SourceDataLine) AudioSystem.getLine(info);
                line.open(format);

                if (line != null) {
                    line.start();
                    int nBytesRead = 0;
                    while (nBytesRead != -1) {
                        nBytesRead = stream.read(data);
                        if (nBytesRead != -1) {
                            line.write(data, 0, nBytesRead);
                        }
                    }
                    line.drain();
                    line.stop();
                    line.close();
                }
                stream.close();

            } catch (Exception e) {
                e.printStackTrace();
            }
        }

    }
    ```

1. In der Methode `main` müssen Sie zunächst `DialogServiceConfig` konfigurieren und damit eine `DialogServiceConnector`-Instanz erstellen. Diese Instanz stellt eine Verbindung mit dem Direct Line Speech-Kanal für die Interaktion mit Ihrem Bot her. Außerdem wird eine `AudioConfig`-Instanz verwendet, um die Quelle für die Audioeingabe anzugeben. In diesem Beispiel wird mit `AudioConfig.fromDefaultMicrophoneInput()` das Standardmikrofon verwendet.

    * Ersetzen Sie die Zeichenfolge `YourSubscriptionKey` durch Ihren Abonnementschlüssel (auf [dieser Website](get-started.md) verfügbar).
    * Ersetzen Sie die Zeichenfolge `YourServiceRegion` durch die mit Ihrem Abonnement verknüpfte [Region](regions.md).

    > [!NOTE]
    > Beachten Sie [die Liste mit den unterstützten Regionen für Sprach-Assistenten](regions.md#voice-assistants), und stellen Sie sicher, dass Ihre Ressourcen in einer dieser Regionen bereitgestellt werden.

    ```java
    final String subscriptionKey = "YourSubscriptionKey"; // Your subscription key
    final String region = "YourServiceRegion"; // Your speech subscription service region
    final BotFrameworkConfig botConfig = BotFrameworkConfig.fromSubscription(subscriptionKey, region);

    // Configure audio input from a microphone.
    final AudioConfig audioConfig = AudioConfig.fromDefaultMicrophoneInput();

    // Create a DialogServiceConnector instance.
    final DialogServiceConnector connector = new DialogServiceConnector(botConfig, audioConfig);
    ```

1. Der Connector`DialogServiceConnector` nutzt mehrere Ereignisse, um Botaktivitäten, Spracherkennungsergebnisse und andere Informationen zu kommunizieren. Fügen Sie daher als Nächstes die folgenden Ereignislistener hinzu:

    ```java
    // Recognizing will provide the intermediate recognized text while an audio stream is being processed.
    connector.recognizing.addEventListener((o, speechRecognitionResultEventArgs) -> {
        log.info("Recognizing speech event text: {}", speechRecognitionResultEventArgs.getResult().getText());
    });

    // Recognized will provide the final recognized text once audio capture is completed.
    connector.recognized.addEventListener((o, speechRecognitionResultEventArgs) -> {
        log.info("Recognized speech event reason text: {}", speechRecognitionResultEventArgs.getResult().getText());
    });

    // SessionStarted will notify when audio begins flowing to the service for a turn.
    connector.sessionStarted.addEventListener((o, sessionEventArgs) -> {
        log.info("Session Started event id: {} ", sessionEventArgs.getSessionId());
    });

    // SessionStopped will notify when a turn is complete and it's safe to begin listening again.
    connector.sessionStopped.addEventListener((o, sessionEventArgs) -> {
        log.info("Session stopped event id: {}", sessionEventArgs.getSessionId());
    });

    // Canceled will be signaled when a turn is aborted or experiences an error condition.
    connector.canceled.addEventListener((o, canceledEventArgs) -> {
        log.info("Canceled event details: {}", canceledEventArgs.getErrorDetails());
        connector.disconnectAsync();
    });

    // ActivityReceived is the main way your bot will communicate with the client and uses Bot Framework activities.
    connector.activityReceived.addEventListener((o, activityEventArgs) -> {
        final String act = activityEventArgs.getActivity().serialize();
            log.info("Received activity {} audio", activityEventArgs.hasAudio() ? "with" : "without");
            if (activityEventArgs.hasAudio()) {
                playAudioStream(activityEventArgs.getAudio());
            }
        });
    ```

1. Rufen Sie die Methode `connectAsync()` auf, um `DialogServiceConnector` mit Direct Line Speech zu verbinden. Um Ihren Bot zu testen, können Sie die Methode `listenOnceAsync` aufrufen und eine Audioeingabe von Ihrem Mikrofon senden. Außerdem können Sie die Methode `sendActivityAsync` verwenden, um eine benutzerdefinierte Aktivität als serialisierte Zeichenfolge zu senden. Diese benutzerdefinierten Aktivitäten können zusätzliche Daten bereitstellen, die Ihr Bot in der Konversation verwendet.

    ```java
    connector.connectAsync();
    // Start listening.
    System.out.println("Say something ...");
    connector.listenOnceAsync();

    // connector.sendActivityAsync(...)
    ```

1. Speichern Sie die Änderungen an der Datei `Main`.

1. Zur Unterstützung der Antwortwiedergabe fügen Sie eine zusätzliche Klasse hinzu, die das von der API „getAudio()“ zurückgegebene PullAudioOutputStream-Objekt in ein Java-InputStream-Objekt umwandelt, um die Verarbeitung zu vereinfachen. `ActivityAudioStream` ist eine spezielle Klasse für die Verarbeitung der Audioantwort aus dem Direct Line Speech-Kanal. Sie stellt Accessoren zum Abrufen von erforderlichen Audioformatinformationen für die Wiedergabe bereit. Wählen Sie dazu **Datei** > **Neu** > **Klasse** aus.

1. Geben Sie im Fenster **Neue Java-Klasse** die Zeichenfolge *speechsdk.quickstart* in das Feld **Paket** und *ActivityAudioStream* in das Feld **Name** ein.

1. Öffnen Sie die neu erstellte Klasse `ActivityAudioStream`, und ersetzen Sie sie durch den folgenden Code:

    ```java
    package com.speechsdk.quickstart;

    import com.microsoft.cognitiveservices.speech.audio.PullAudioOutputStream;

    import java.io.IOException;
    import java.io.InputStream;


    public final class ActivityAudioStream extends InputStream {
        /**
         * The number of samples played per second (16 kHz).
         */
        public static final long SAMPLE_RATE = 16000;
        /**
         * The number of bits in each sample of a sound that has this format (16 bits).
         */
        public static final int BITS_PER_SECOND = 16;
        /**
         * The number of audio channels in this format (1 for mono).
         */
        public static final int CHANNELS = 1;
        /**
         * The number of bytes in each frame of a sound that has this format (2).
         */
        public static final int FRAME_SIZE = 2;

        /**
         * Reads up to a specified maximum number of bytes of data from the audio
         * stream, putting them into the given byte array.
         *
         * @param b   the buffer into which the data is read
         * @param off the offset, from the beginning of array <code>b</code>, at which
         *            the data will be written
         * @param len the maximum number of bytes to read
         * @return the total number of bytes read into the buffer, or -1 if there
         * is no more data because the end of the stream has been reached
         */
        @Override
        public int read(byte[] b, int off, int len) {
            byte[] tempBuffer = new byte[len];
            int n = (int) this.pullStreamImpl.read(tempBuffer);
            for (int i = 0; i < n; i++) {
                if (off + i > b.length) {
                    throw new ArrayIndexOutOfBoundsException(b.length);
                }
                b[off + i] = tempBuffer[i];
            }
            if (n == 0) {
                return -1;
            }
            return n;
        }

        /**
         * Reads the next byte of data from the activity audio stream if available.
         *
         * @return the next byte of data, or -1 if the end of the stream is reached
         * @see #read(byte[], int, int)
         * @see #read(byte[])
         * @see #available
         * <p>
         */
        @Override
        public int read() {
            byte[] data = new byte[1];
            int temp = read(data);
            if (temp <= 0) {
                // we have a weird situation if read(byte[]) returns 0!
                return -1;
            }
            return data[0] & 0xFF;
        }

        /**
         * Reads up to a specified maximum number of bytes of data from the activity audio stream,
         * putting them into the given byte array.
         *
         * @param b the buffer into which the data is read
         * @return the total number of bytes read into the buffer, or -1 if there
         * is no more data because the end of the stream has been reached
         */
        @Override
        public int read(byte[] b) {
            int n = (int) pullStreamImpl.read(b);
            if (n == 0) {
                return -1;
            }
            return n;
        }

        /**
         * Skips over and discards a specified number of bytes from this
         * audio input stream.
         *
         * @param n the requested number of bytes to be skipped
         * @return the actual number of bytes skipped
         * @throws IOException if an input or output error occurs
         * @see #read
         * @see #available
         */
        @Override
        public long skip(long n) {
            if (n <= 0) {
                return 0;
            }
            if (n <= Integer.MAX_VALUE) {
                byte[] tempBuffer = new byte[(int) n];
                return read(tempBuffer);
            }
            long count = 0;
            for (long i = n; i > 0; i -= Integer.MAX_VALUE) {
                int size = (int) Math.min(Integer.MAX_VALUE, i);
                byte[] tempBuffer = new byte[size];
                count += read(tempBuffer);
            }
            return count;
        }

        /**
         * Closes this audio input stream and releases any system resources associated
         * with the stream.
         */
        @Override
        public void close() {
            this.pullStreamImpl.close();
        }

        /**
         * Fetch the audio format for the ActivityAudioStream. The ActivityAudioFormat defines the sample rate, bits per sample, and the # channels.
         *
         * @return instance of the ActivityAudioFormat associated with the stream
         */
        public ActivityAudioStream.ActivityAudioFormat getActivityAudioFormat() {
            return activityAudioFormat;
        }

        /**
         * Returns the maximum number of bytes that can be read (or skipped over) from this
         * audio input stream without blocking.
         *
         * @return the number of bytes that can be read from this audio input stream without blocking.
         * As this implementation does not buffer, this will be defaulted to 0
         */
        @Override
        public int available() {
            return 0;
        }

        public ActivityAudioStream(final PullAudioOutputStream stream) {
            pullStreamImpl = stream;
            this.activityAudioFormat = new ActivityAudioStream.ActivityAudioFormat(SAMPLE_RATE, BITS_PER_SECOND, CHANNELS, FRAME_SIZE, AudioEncoding.PCM_SIGNED);
        }

        private PullAudioOutputStream pullStreamImpl;

        private ActivityAudioFormat activityAudioFormat;

        /**
         * ActivityAudioFormat is an internal format which contains metadata regarding the type of arrangement of
         * audio bits in this activity audio stream.
         */
        static class ActivityAudioFormat {

            private long samplesPerSecond;
            private int bitsPerSample;
            private int channels;
            private int frameSize;
            private AudioEncoding encoding;

            public ActivityAudioFormat(long samplesPerSecond, int bitsPerSample, int channels, int frameSize, AudioEncoding encoding) {
                this.samplesPerSecond = samplesPerSecond;
                this.bitsPerSample = bitsPerSample;
                this.channels = channels;
                this.encoding = encoding;
                this.frameSize = frameSize;
            }

            /**
             * Fetch the number of samples played per second for the associated audio stream format.
             *
             * @return the number of samples played per second
             */
            public long getSamplesPerSecond() {
                return samplesPerSecond;
            }

            /**
             * Fetch the number of bits in each sample of a sound that has this audio stream format.
             *
             * @return the number of bits per sample
             */
            public int getBitsPerSample() {
                return bitsPerSample;
            }

            /**
             * Fetch the number of audio channels used by this audio stream format.
             *
             * @return the number of channels
             */
            public int getChannels() {
                return channels;
            }

            /**
             * Fetch the default number of bytes in a frame required by this audio stream format.
             *
             * @return the number of bytes
             */
            public int getFrameSize() {
                return frameSize;
            }

            /**
             * Fetch the audio encoding type associated with this audio stream format.
             *
             * @return the encoding associated
             */
            public AudioEncoding getEncoding() {
                return encoding;
            }
        }

        /**
         * Enum defining the types of audio encoding supported by this stream.
         */
        public enum AudioEncoding {
            PCM_SIGNED("PCM_SIGNED");

            String value;

            AudioEncoding(String value) {
                this.value = value;
            }
        }
    }

    ```

1. Speichern Sie die Änderungen an der Datei `ActivityAudioStream`.

## <a name="build-and-run-the-app"></a>Erstellen und Ausführen der App

Drücken Sie F11, oder wählen Sie **Ausführen** > **Debuggen** aus.
Die Konsole zeigt die Meldung „Sagen Sie etwas...“ an.
Sprechen Sie nun einen englischen Ausdruck oder Satz, den Ihr Bot versteht. Ihre Spracheingabe wird über den Direct Line Speech-Kanal an den Bot übermittelt und vom Bot erkannt und verarbeitet. Die Antwort wird als Aktivität zurückgegeben. Wenn Ihr Bot eine Sprachantwort zurückgibt, erfolgt die Audiowiedergabe unter Verwendung der Klasse `AudioPlayer`.

![Screenshot der Konsolenausgabe nach erfolgreicher Erkennung](media/sdk/qs-java-jre-08-console-output.png)

## <a name="next-steps"></a>Nächste Schritte

Weitere Beispiele, z.B. das Auslesen von Sprache aus einer Audiodatei, sind auf GitHub verfügbar.

> [!div class="nextstepaction"]
> [Erstellen und Bereitstellen eines Basisbots](https://docs.microsoft.com/azure/bot-service/bot-builder-tutorial-basic-deploy?view=azure-bot-service-4.0)

## <a name="see-also"></a>Weitere Informationen

- [Informationen zu Sprach-Assistenten](voice-assistants.md)
- [Beziehen eines kostenlosen Abonnementschlüssels für die Spracherkennungsdienste](get-started.md)
- [Benutzerdefinierte Schlüsselwörter](speech-devices-sdk-create-kws.md)
- [Verbinden von Direct Line Speech mit Ihrem Bot](https://docs.microsoft.com/azure/bot-service/bot-service-channel-connect-directlinespeech)
- [Java-Beispiele auf GitHub](https://aka.ms/csspeech/samples)
