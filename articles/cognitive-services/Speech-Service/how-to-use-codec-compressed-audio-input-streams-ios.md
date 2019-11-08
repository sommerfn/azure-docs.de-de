---
title: Streamen von per Codec komprimierten Audiodaten mit dem Speech SDK unter iOS – Speech Service
titleSuffix: Azure Cognitive Services
description: Erfahren Sie, wie Sie mit dem Speech SDK unter iOS komprimierte Audiodaten an Azure Speech Services streamen.
services: cognitive-services
author: chlandsi
manager: nitinme
ms.service: cognitive-services
ms.subservice: speech-service
ms.topic: conceptual
ms.date: 09/20/2019
ms.author: chlandsi
ms.openlocfilehash: 45b45c6c9afd43b711fc548f470ce0f0acd04a0a
ms.sourcegitcommit: c22327552d62f88aeaa321189f9b9a631525027c
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 11/04/2019
ms.locfileid: "73464284"
---
# <a name="using-codec-compressed-audio-input-with-the-speech-sdk-on-ios"></a>Verwenden von per Codec komprimierter Audioeingabe mit dem Speech SDK unter iOS

Die Speech SDK-API für **komprimierte Audioeingabestreams** bietet eine Möglichkeit zum Streamen von komprimierten Audiodaten an Speech Services mit Pull- oder Pushstream.

> [!IMPORTANT]
> Speech SDK Version 1.7.0 oder höher ist erforderlich, um komprimiertes Audio unter iOS zu streamen. Es wird auch für [C++, C# und Java unter Linux (Ubuntu 16.04, Ubuntu 18.04, Debian 9)](how-to-use-codec-compressed-audio-input-streams.md) und [Java in Android](how-to-use-codec-compressed-audio-input-streams-android.md) unterstützt.

Informationen zu WAV/PCM finden Sie in der Hauptdokumentation zu Speech.  Neben WAV/PCM werden folgende per Codec komprimierten Eingabeformate unterstützt:

- MP3
- OPUS/OGG
- FLAC
- ALAW im WAV-Container
- MULAW im WAV-Container

## <a name="prerequisites"></a>Voraussetzungen

Die Verarbeitung komprimierter Audiodaten wird mit [GStreamer](https://gstreamer.freedesktop.org) implementiert.
Aus Lizenzierungsgründen können diese Funktionen nicht mit dem SDK ausgeliefert werden, aber eine Wrapperbibliothek mit diesen Funktionen muss von Anwendungsentwicklern erstellt und mit den Anwendungen über das SDK ausgeliefert werden.
Um diese Wrapperbibliothek zu erstellen, laden Sie zunächst das [GStreamer SDK](https://gstreamer.freedesktop.org/data/pkg/ios/1.16.0/gstreamer-1.0-devel-1.16.0-ios-universal.pkg) herunter und installieren Sie es.
Laden Sie dann das Xcode-Projekt für die [Wrapperbibliothek](https://github.com/Azure-Samples/cognitive-services-speech-sdk/tree/master/samples/objective-c/ios/compressed-streams/GStreamerWrapper) herunter.
Öffnen Sie das Projekt in Xcode, und erstellen Sie es für das Ziel **Allgemeines iOS-Gerät**. Die Erstellung für ein bestimmtes Ziel wird nicht funktionieren.
Der Buildschritt generiert ein dynamisches Frameworkpaket mit einer dynamischen Bibliothek für alle erforderlichen Architekturen mit dem Namen `GStreamerWrapper.framework`.
Dieses Framework muss in allen Anwendungen enthalten sein, die komprimierte Audiostreams mit dem Speech Services SDK verwenden.

Wenden Sie dazu die folgenden Einstellungen in Ihrem Xcode-Projekt an:

1. Kopieren Sie sowohl das soeben erstellte `GStreamerWrapper.framework` als auch das Framework des Cognitive Services Speech SDK, das Sie [hier](https://aka.ms/csspeech/iosbinary) herunterladen können, in das Verzeichnis, das Ihr Beispielprojekt enthält.
1. Passen Sie die Pfade an die Frameworks in den *Projekteinstellungen* an.
    1. Fügen Sie auf der Registerkarte **Allgemein** unter der Überschrift **Eingebettete Binärdateien** die SDK-Bibliothek als Framework hinzu: **Eingebettete Binärdateien hinzufügen** > **Weitere hinzufügen...** , und navigieren Sie zu dem von Ihnen gewählten Verzeichnis, und wählen Sie dann beide Frameworks aus.
    1. Navigieren Sie zur Registerkarte **Buildeinstellungen**, und aktivieren Sie **Alle** Einstellungen.
1. Fügen Sie das Verzeichnis `$(SRCROOT)/..` den *Frameworksuchpfaden* unter der Überschrift **Suchpfade** hinzu.

## <a name="example-code-using-codec-compressed-audio-input"></a>Beispielcode für die Verwendung von per Codec komprimierter Audioeingabe

Erstellen Sie zum Streamen von komprimierten Audioformaten an Speech Services `SPXPullAudioInputStream` oder `SPXPushAudioInputStream`.
Der folgende Codeausschnitt zeigt, wie Sie ein `SPXAudioConfiguration` aus einer Instanz eines `SPXPushAudioInputStream` erstellen, indem Sie MP3 als Komprimierungsformat des Datenstroms angeben.

[!code-objectivec[Set up the input stream](~/samples-cognitive-services-speech-sdk/samples/objective-c/ios/compressed-streams/CompressedStreamsSample/CompressedStreamsSample/ViewController.m?range=66-77&highlight=2-11)]

Der nächste Codeausschnitt zeigt, wie komprimierte Audiodaten aus einer Datei gelesen und in das `SPXPushAudioInputStream` eingefügt werden können.

[!code-objectivec[Push compressed audio data into the stream](~/samples-cognitive-services-speech-sdk/samples/objective-c/ios/compressed-streams/CompressedStreamsSample/CompressedStreamsSample/ViewController.m?range=105-151&highlight=19-44)]

## <a name="next-steps"></a>Nächste Schritte

- [Abrufen Ihres Testabonnements für Speech](https://azure.microsoft.com/try/cognitive-services/)
* [Erkennen von Sprache in Java](~/articles/cognitive-services/Speech-Service/quickstarts/speech-to-text-from-microphone.md?pivots=programming-language-java)
