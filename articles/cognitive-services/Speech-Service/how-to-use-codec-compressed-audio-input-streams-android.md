---
title: Streamen von per Codec komprimierten Audiodaten mit dem Speech SDK unter Android – Speech Service
titleSuffix: Azure Cognitive Services
description: Erfahren Sie, wie Sie mit dem Speech SDK unter Android komprimierte Audiodaten an Azure Speech Services streamen.
services: cognitive-services
author: amitkumarshukla
manager: nitinme
ms.service: cognitive-services
ms.subservice: speech-service
ms.topic: conceptual
ms.date: 09/20/2019
ms.author: amishu
ms.openlocfilehash: 2e741e8a8df2cebff167a381cef41351ead4c6cf
ms.sourcegitcommit: c22327552d62f88aeaa321189f9b9a631525027c
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 11/04/2019
ms.locfileid: "73464372"
---
# <a name="using-codec-compressed-audio-input-with-the-speech-sdk-on-android"></a>Verwenden von per Codec komprimierter Audioeingabe mit dem Speech SDK unter Android

Die Speech SDK-API für **komprimierte Audioeingabestreams** bietet eine Möglichkeit zum Streamen von komprimierten Audiodaten an Speech Services mit PullStream oder PushStream.

> [!IMPORTANT]
> Das Streamen komprimierter Eingabeaudiodaten wird derzeit nur für [C++, C# und Java unter Linux (Ubuntu 16.04, Ubuntu 18.04, Debian 9)](how-to-use-codec-compressed-audio-input-streams.md) unterstützt. Es wird auch für Java in Android und [Objective-C in iOS](how-to-use-codec-compressed-audio-input-streams-ios.md) unterstützt.
> Das Speech SDK ab Version 1.7.0 ist erforderlich.

Informationen zu WAV/PCM finden Sie in der Hauptdokumentation zu Speech.  Neben WAV/PCM werden folgende per Codec komprimierten Eingabeformate unterstützt:

- MP3
- OPUS/OGG
- FLAC
- ALAW im WAV-Container
- MULAW im WAV-Container

## <a name="prerequisites-to-using-codec-compressed-audio-input-on-android"></a>Voraussetzungen für die Verwendung von per Codec komprimierter Audioeingabe unter Android

Per Codec komprimierte Audiodaten werden mit [GStreamer](https://gstreamer.freedesktop.org) implementiert. Aus Lizenzierungsgründen werden die GStreamer-Binärdateien nicht mit dem SDK kompiliert. Sie müssen die vorkonfigurierten Binärdateien für Android verwenden. Informationen zum Herunterladen der vorkonfigurierten Bibliotheken finden Sie unter [Installieren für die Android-Entwicklung](https://gstreamer.freedesktop.org/documentation/installing/for-android-development.html?gi-language=c). 

`libgstreamer_android.so` ist erforderlich. Stellen Sie sicher, dass Ihre GStreamer-Plug-Ins in `libgstreamer_android.so` verknüpft sind.

```make
GSTREAMER_PLUGINS := coreelements app audioconvert mpg123 audioresample audioparsers ogg opusparse opus wavparse alaw mulaw flac
```

Nachfolgend finden Sie ein Beispiel für die Datei `Android.mk` und `Application.mk`. Führen Sie diese Schritte aus, um das von „gstreamer“ freigegebene Objekt zu erstellen: `libgstreamer_android.so`.

```make
# Android.mk
LOCAL_PATH := $(call my-dir)

include $(CLEAR_VARS)

LOCAL_MODULE    := dummy
LOCAL_SHARED_LIBRARIES := gstreamer_android
LOCAL_LDLIBS := -llog
include $(BUILD_SHARED_LIBRARY)


ifndef GSTREAMER_ROOT_ANDROID
$(error GSTREAMER_ROOT_ANDROID is not defined!)
endif

ifndef APP_BUILD_SCRIPT
$(error APP_BUILD_SCRIPT is not defined!)
endif

ifndef TARGET_ARCH_ABI
$(error TARGET_ARCH_ABI is not defined!)
endif

ifeq ($(TARGET_ARCH_ABI),armeabi)
GSTREAMER_ROOT        := $(GSTREAMER_ROOT_ANDROID)/arm
else ifeq ($(TARGET_ARCH_ABI),armeabi-v7a)
GSTREAMER_ROOT        := $(GSTREAMER_ROOT_ANDROID)/armv7
else ifeq ($(TARGET_ARCH_ABI),arm64-v8a)
GSTREAMER_ROOT        := $(GSTREAMER_ROOT_ANDROID)/arm64
else ifeq ($(TARGET_ARCH_ABI),x86)
GSTREAMER_ROOT        := $(GSTREAMER_ROOT_ANDROID)/x86
else ifeq ($(TARGET_ARCH_ABI),x86_64)
GSTREAMER_ROOT        := $(GSTREAMER_ROOT_ANDROID)/x86_64
else
$(error Target arch ABI not supported: $(TARGET_ARCH_ABI))
endif

GSTREAMER_NDK_BUILD_PATH  := $(GSTREAMER_ROOT)/share/gst-android/ndk-build/
include $(GSTREAMER_NDK_BUILD_PATH)/plugins.mk
GSTREAMER_PLUGINS         :=  coreelements app audioconvert mpg123 audioresample audioparsers ogg opusparse opus wavparse alaw mulaw flac
GSTREAMER_EXTRA_LIBS      := -liconv
include $(GSTREAMER_NDK_BUILD_PATH)/gstreamer-1.0.mk
```

```make
# Application.mk
APP_STL = c++_shared
APP_PLATFORM = android-21
APP_BUILD_SCRIPT = Android.mk
```

Sie können `libgstreamer_android.so` mit dem folgenden Befehl unter Ubuntu 16.04 oder 18.04 erstellen. Die folgenden Befehlszeilen wurden nur für [Gstreamer (Android-Version 1.14.4)](https://gstreamer.freedesktop.org/data/pkg/android/1.14.4/gstreamer-1.0-android-universal-1.14.4.tar.bz2) mit [Android NDK b16b](https://dl.google.com/android/repository/android-ndk-r16b-linux-x86_64.zip) getestet.

```sh
# assuming wget and unzip already installed on the system
mkdir buildLibGstreamer
cd buildLibGstreamer
wget https://dl.google.com/android/repository/android-ndk-r16b-linux-x86_64.zip
unzip -q -o android-ndk-r16b-linux-x86_64.zip
export PATH=$PATH:$(pwd)/android-ndk-r16b
export NDK_PROJECT_PATH=$(pwd)/android-ndk-r16b
wget https://gstreamer.freedesktop.org/data/pkg/android/1.14.4/gstreamer-1.0-android-universal-1.14.4.tar.bz2
mkdir gstreamer_android
tar -xjf gstreamer-1.0-android-universal-1.14.4.tar.bz2 -C $(pwd)/gstreamer_android/
export GSTREAMER_ROOT_ANDROID=$(pwd)/gstreamer_android

mkdir gstreamer
# Copy the Application.mk and Android.mk from the documentation above and put it inside $(pwd)/gstreamer

# Enable only one of the following at one time to create the shared object for the targeted ABI
echo "building for armeabi-v7a. libgstreamer_android.so will be placed in $(pwd)/armeabi-v7a"
ndk-build -C $(pwd)/gstreamer "NDK_APPLICATION_MK=Application.mk" APP_ABI=armeabi-v7a NDK_LIBS_OUT=$(pwd)

#echo "building for arm64-v8a. libgstreamer_android.so will be placed in $(pwd)/arm64-v8a"
#ndk-build -C $(pwd)/gstreamer "NDK_APPLICATION_MK=Application.mk" APP_ABI=arm64-v8a NDK_LIBS_OUT=$(pwd)

#echo "building for x86_64. libgstreamer_android.so will be placed in $(pwd)/x86_64"
#ndk-build -C $(pwd)/gstreamer "NDK_APPLICATION_MK=Application.mk" APP_ABI=x86_64 NDK_LIBS_OUT=$(pwd)

#echo "building for x86. libgstreamer_android.so will be placed in $(pwd)/x86"
#ndk-build -C $(pwd)/gstreamer "NDK_APPLICATION_MK=Application.mk" APP_ABI=x86 NDK_LIBS_OUT=$(pwd)
```

Nachdem das freigegebene Objekt (libgstreamer_android.so) erstellt wurde, muss der Anwendungsentwickler das freigegebene Objekt in der Android-App positionieren, damit es vom Speech SDK geladen werden kann.

## <a name="example-code-using-codec-compressed-audio-input"></a>Beispielcode für die Verwendung von per Codec komprimierter Audioeingabe

Erstellen Sie zum Streamen von komprimierten Audioformaten an Speech Services `PullAudioInputStream` oder `PushAudioInputStream`. Erstellen Sie dann eine `AudioConfig` aus einer Instanz Ihrer stream-Klasse, und geben Sie dabei das Komprimierungsformat des Streams an.

Angenommen, Sie verfügen über die Eingabestreamklasse `myPullStream` und verwenden OPUS/OGG. Ihr Code kann folgendermaßen aussehen:

```java
import com.microsoft.cognitiveservices.speech.audio.AudioConfig;
import com.microsoft.cognitiveservices.speech.audio.AudioInputStream;
import com.microsoft.cognitiveservices.speech.audio.AudioStreamFormat;
import com.microsoft.cognitiveservices.speech.audio.PullAudioInputStream;
import com.microsoft.cognitiveservices.speech.internal.AudioStreamContainerFormat;

SpeechConfig speechConfig = SpeechConfig.fromSubscription("YourSubscriptionKey", "YourServiceRegion");

// Create an audio config specifying the compressed audio format and the instance of your input stream class.
AudioStreamFormat audioFormat = AudioStreamFormat.getCompressedFormat(AudioStreamContainerFormat.OGG_OPUS);
AudioConfig audioConfig = AudioConfig.fromStreamInput(myPullStream, audioFormat);

SpeechRecognizer recognizer = new SpeechRecognizer(speechConfig, audioConfig);

SpeechRecognitionResult result = recognizer.recognizeOnceAsync().get()

String text = result.getText();
```

## <a name="next-steps"></a>Nächste Schritte

- [Abrufen Ihres Testabonnements für Speech](https://azure.microsoft.com/try/cognitive-services/)
* [Erkennen von Sprache in Java](~/articles/cognitive-services/Speech-Service/quickstarts/speech-to-text-from-microphone.md?pivots=programming-language-java)
