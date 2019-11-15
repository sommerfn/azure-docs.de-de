---
title: Audio Content Creation – Speech Service
titleSuffix: Azure Cognitive Services
description: Audio Content Creation ist ein Onlinetool, mit dem Sie die Sprachsynthese von Microsoft an Ihre Apps und Produkte anpassen und optimieren können.
services: cognitive-services
author: erhopf
manager: nitinme
ms.service: cognitive-services
ms.subservice: speech-service
ms.topic: conceptual
ms.date: 11/04/2019
ms.author: erhopf
ms.openlocfilehash: 6ec98d1f7956ef1460909f62384a931400b596c4
ms.sourcegitcommit: f4d8f4e48c49bd3bc15ee7e5a77bee3164a5ae1b
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 11/04/2019
ms.locfileid: "73579627"
---
# <a name="audio-content-creation"></a>Audio Content Creation

[Audio Content Creation](https://aka.ms/audiocontentcreation) ist ein Onlinetool, mit dem Sie die Sprachsynthese von Microsoft an Ihre Apps und Produkte anpassen und optimieren können. Sie können mit diesem Tool öffentliche und benutzerdefinierte Stimmen optimieren, um noch natürlichere Sprachausdrücke zu erzielen und Ihre Ausgabe in der Cloud zu verwalten.

Das Tool Audio Content Creation basiert auf der [Speech Synthesis Markup Language (SSML)](speech-synthesis-markup.md). Um die Anpassung und Optimierung zu vereinfachen, ermöglicht Audio Content Creation das visuelle Untersuchen von Sprachsyntheseausgaben in Echtzeit.

## <a name="how-does-it-work"></a>Wie funktioniert dies?

Dieses Diagramm zeigt die Schritte zum Optimieren und Exportieren von Sprachausgaben. Verwenden Sie die unten angegebenen Links, um mehr über die einzelnen Schritte zu erfahren.

![](media/audio-content-creation/audio-content-creation-diagram.jpg)

1. Der erste Schritt besteht darin, [ein Azure-Konto zu erstellen, eine Speech-Ressource zu registrieren und einen Abonnementschlüssel zu erhalten](#create-a-speech-resource). Wenn Sie über einen Abonnementschlüssel verfügen, können Sie damit den Speech-Dienst aufrufen und auf [Audio Content Creation](https://aka.ms/audiocontentcreation) zugreifen.
2. [Erstellen Sie eine Datei zur Audiooptimierung](#create-an-audio-tuning-file) als Nur-Text-Datei oder mit SSML.
3. Wählen Sie die Stimme und die Sprache aus, die Sie optimieren möchten. Audio Content Creation umfasst alle [Stimmen der Microsoft-Sprachsynthese](language-support.md#text-to-speech). Sie können eine Standardstimme, eine neuronale Stimme oder Ihre eigene benutzerdefinierte Stimme verwenden.
   >[!NOTE]
   > Der Zugriff auf benutzerdefinierte neuronale Stimmen ist nur eingeschränkt möglich. Hiermit können Sie High-Definition-Stimmen erstellen, die wie natürliche Stimmen klingen. Weitere Einzelheiten finden Sie unter [Zulassung](https://aka.ms/ignite2019/speech/ethics).

4. Überprüfen Sie das Standardergebnis. Verwenden Sie dann das Optimierungstool, um Aussprache, Tonhöhe, Geschwindigkeit, Intonation, Sprachstil usw. anzupassen. Eine umfassende Liste der Optionen finden Sie unter [Markupsprache für Sprachsynthese](speech-synthesis-markup.md).
5. Speichern und [exportieren Sie die optimierten Audiodaten](#export-tuned-audio). Nachdem Sie die Optimierungsspur im System gespeichert haben, können Sie weiterhin mit der Ausgabe fortfahren. Wenn Sie mit der Ausgabe zufrieden sind, können Sie mit der Exportfunktion eine Aufgabe zur Audioerstellung erstellen. Sie können den Status der Exportaufgabe beobachten und die Ausgabe für die Verwendung in Ihren Apps und Produkten herunterladen.
6. Im letzten Schritt verwenden Sie die benutzerdefinierte und optimierte Stimme in Ihren Apps und Produkten.

## <a name="create-a-speech-resource"></a>Erstellen einer Speech-Ressource

Führen Sie die folgenden Schritte aus, um eine Speech-Ressource zu erstellen und mit Speech Studio zu verbinden.

1. Befolgen Sie diese Anweisungen, um sich [für ein Azure-Konto zu registrieren](https://docs.microsoft.com/azure/cognitive-services/speech-service/get-started#new-azure-account) und [eine Speech-Ressource zu erstellen](https://docs.microsoft.com/azure/cognitive-services/speech-service/get-started#create-a-speech-resource-in-azure). Vergewissern Sie sich, dass Ihr Tarif **S0** ist. Wenn Sie eine der neuronalen Stimmen verwenden, müssen Sie die Ressource in einer [unterstützten Region](regions.md#standard-and-neural-voices) erstellen.
2. Melden Sie sich bei [Audio Content Creation](https://aka.ms/audiocontentcreation) an.
3. Wählen Sie ein vorhandenes Projekt aus, oder klicken Sie auf **Neu erstellen**.
4. Sie können Ihr Abonnement jederzeit ändern, indem Sie im oberen Navigationsbereich die Option **Einstellungen** auswählen.

## <a name="create-an-audio-tuning-file"></a>Erstellen einer Audiooptimierungsdatei

Es gibt zwei Möglichkeiten, Ihre Inhalte in das Audio Content Creation-Tool zu übernehmen.

**Option 1:**

1. Nachdem Sie sich bei [Audio Content Creation](https://aka.ms/audiocontentcreation) angemeldet haben, klicken Sie auf **Audio Tuning** (Audiooptimierung), um eine neue Audiooptimierungsdatei zu erstellen.
2. Wenn das Bearbeitungsfenster angezeigt wird, können Sie bis zu 10.000 Zeichen eingeben.
3. Vergessen Sie nicht zu speichern.

**Option 2:**

1. Nachdem Sie sich bei [Audio Content Creation](https://aka.ms/audiocontentcreation) angemeldet haben, klicken Sie auf **Hochladen**, um Textdateien zu importieren. Sowohl Nur-Text als auch SSML werden unterstützt.
2. Stellen Sie beim Hochladen Ihrer Textdateien sicher, dass der Inhalt diese Anforderungen erfüllt.

   | Eigenschaft | Wert/Hinweise |
   |----------|---------------|
   | Dateiformat | Nur-Text (.txt)<br/> SSML-Text (.txt)<br/> ZIP-Dateien werden nicht unterstützt. |
   | Codierungsformat | UTF-8 |
   | Dateiname | Jede Datei muss einen eindeutigen Namen haben. Duplikate werden nicht unterstützt. |
   | Textlänge | Textdateien dürfen maximal 10.000 Zeichen lang sein. |
   | SSML-Einschränkungen | Jede SSML-Datei darf nur ein einziges SSML-Element enthalten. |

### <a name="plain-text-example"></a>Beispiel für Nur-Text

```txt
Welcome to use Audio Content Creation to customize audio output for your products.
```

### <a name="ssml-text-example"></a>Beispiel für SSML-Text

```xml
<speak xmlns="http://www.w3.org/2001/10/synthesis" xmlns:mstts="http://www.w3.org/2001/mstts" version="1.0" xml:lang="en-US">
    <voice name="Microsoft Server Speech Text to Speech Voice (en-US, JessaNeural)">
    Welcome to use Audio Content Creation <break time="10ms" />to customize audio output for your products.
    </voice>
</speak>
```

## <a name="export-tuned-audio"></a>Exportieren der optimierten Audiodatei

Nachdem Sie Ihre Audioausgabe überprüft haben und mit der Optimierung und den Anpassungen zufrieden sind, können Sie die Audiodatei exportieren.

1. Klicken Sie im Tool [Audio Content Creation](https://aka.ms/audiocontentcreation) auf **Exportieren**, um eine Aufgabe zur Audioerstellung zu erstellen.
2. Wählen Sie das Ausgabeformat für die Audiooptimierung aus. Eine Liste der unterstützten Formate und Abtastraten finden Sie unten.
3. Sie können den Status der Aufgabe auf der Registerkarte der **Exportaufgabe** anzeigen. Wenn die Aufgabe zu einem Fehler führt, finden Sie auf der Seite mit den ausführlichen Informationen einen vollständigen Bericht.
4. Wenn die Aufgabe abgeschlossen wurde, steht Ihre Audiodatei auf der Registerkarte **Audio Library** (Audiobibliothek) zum Download zur Verfügung.
5. Klicken Sie auf **Download**. Sie können nun Ihre benutzerdefinierten und optimierten Audiodaten in Ihren Apps oder Produkten verwenden.

### <a name="supported-audio-formats"></a>Unterstützte Audioformate

| Format | 16-kHz-Abtastrate | 24-kHz-Abtastrate |
|--------|--------------------|--------------------|
| WAV | riff-16khz-16bit-mono-pcm | riff-24khz-16bit-mono-pcm |
| MP3 | audio-16khz-128kbitrate-mono-mp3 | audio-24khz-160kbitrate-mono-mp3 |

## <a name="see-also"></a>Weitere Informationen

* [API für lange Audioinhalte](https://aka.ms/long-audio-api)

## <a name="next-steps"></a>Nächste Schritte

> [!div class="nextstepaction"]
> [Speech Studio](https://speech.microsoft.com)
