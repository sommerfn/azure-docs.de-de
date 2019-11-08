---
title: Vorbereiten von Daten für Custom Voice – Speech Service
titleSuffix: Azure Cognitive Services
description: Erstellen Sie mit Speech Service eine benutzerdefinierte Stimme für Ihre Marke. Sie stellen Studioaufnahmen und die zugehörigen Skripts bereit, und der Dienst generiert ein einzigartiges Stimmmodell, das auf die aufgezeichnete Stimme abgestimmt ist. Verwenden Sie diese Stimme dann, um Sprache in Ihren Produkten, Tools und Anwendungen zu synthetisieren.
services: cognitive-services
author: erhopf
manager: nitinme
ms.service: cognitive-services
ms.subservice: speech-service
ms.topic: conceptual
ms.date: 11/04/2019
ms.author: erhopf
ms.openlocfilehash: a954118cd0697213674bb9981f0d94100488fb38
ms.sourcegitcommit: c22327552d62f88aeaa321189f9b9a631525027c
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 11/04/2019
ms.locfileid: "73464509"
---
# <a name="prepare-data-to-create-a-custom-voice"></a>Vorbereiten von Daten zum Erstellen einer benutzerdefinierten Stimme

Wenn Sie bereit sind, eine benutzerdefinierte Stimme für die Sprachsynthese zu erstellen, besteht der erste Schritt darin, Audioaufnahmen und zugehörige Skripts für das Training des Stimmmodells zusammenzustellen. Der Dienst erstellt anhand dieser Daten eine einzigartige Stimme, die der Stimme in den Aufnahmen entspricht. Nachdem Sie die Stimme trainiert haben, können Sie mit dem Synthetisieren von Sprache in Ihren Anwendungen beginnen.

Zu Beginn sind für einen Proof of Concept nur wenige Daten erforderlich. Je mehr Daten Sie bereitstellen, desto natürlicher klingt Ihre benutzerdefinierte Stimme jedoch. Bevor Sie Ihr eigenes Stimmmodell für die Sprachsynthese trainieren können, benötigen Sie Audioaufnahmen und die zugehörigen Texttranskriptionen. Auf dieser Seite werden die Datentypen, ihre Verwendung und ihre Verwaltung beschrieben.

## <a name="data-types"></a>Datentypen

Ein Dataset für das Stimmtraining enthält Audioaufnahmen und eine Textdatei mit den zugehörigen Transkriptionen. Jede Audiodatei sollte genau eine Äußerung enthalten (einen einzelnen Satz oder Turn eines Dialogsystems) und kürzer als 15 Sekunden sein.

Es kann vorkommen, dass Sie noch kein passendes Dataset haben und das Training für die benutzerdefinierte Stimme mit den verfügbaren Audiodateien testen möchten – ob kurz oder lang, mit oder ohne Transkripte. Wir bieten Tools (Betaversion), mit denen Sie Ihre Audiodateien mit der [Batch-Transkriptions-API](batch-transcription.md) in Äußerungen segmentieren und Transkripte vorbereiten können.

In der folgenden Tabelle sind die Datentypen und ihre Verwendung zum Erstellen eines benutzerdefinierten Stimmmodells für die Sprachsynthese aufgeführt.

| Datentyp | BESCHREIBUNG | Einsatzgebiete | Zusätzlicher Dienst erforderlich | Menge zum Trainieren eines Modells | Gebietsschema(s) |
| --------- | ----------- | ----------- | --------------------------- | ----------------------------- | --------- |
| **Einzelne Äußerungen und entsprechendes Transkript** | Eine Sammlung (.zip) von Audiodateien (.wav) als einzelne Äußerungen. Jede Audiodatei sollte maximal 15 Sekunden lang sein und über ein zugehöriges formatiertes Transkript (.txt) verfügen. | Professionelle Aufnahmen mit entsprechenden Transkripten | Bereit zum Training. | Keine zwingende Anforderung für „en-US“ und „zh-CN“. Mehr als 2.000 unterschiedliche Äußerungen für andere Gebietsschemas. | [Alle Custom Voice-Gebietsschemas](language-support.md#customization) |
| **Lange Audiodatei und Transkript (Betaversion)** | Eine Sammlung (.zip) von langen, nicht segmentierten Audiodateien (länger als 20 Sekunden) mit einem zugehörigen Transkript (.txt), das alle gesprochenen Wörter enthält. | Sie haben Audiodateien und entsprechende Transkripte, die aber nicht in Äußerungen segmentiert sind. | Segmentierung (mithilfe der Batch-Transkription).<br>Transformation des Audioformats (sofern erforderlich). | Keine zwingende Anforderung  | [Alle Custom Voice-Gebietsschemas](language-support.md#customization) |
| **Nur Audio (Betaversion)** | Eine Sammlung (.zip) von Audiodateien ohne Transkript. | Sie haben nur Audiodateien ohne Transkripte. | Segmentierung und Transkriptgenerierung (mithilfe der Batch-Transkription).<br>Transformation des Audioformats (sofern erforderlich).| Keine zwingende Anforderung | [Alle Custom Voice-Gebietsschemas](language-support.md#customization) |

Dateien sollten nach Typ in einem Dataset gruppiert und als ZIP-Datei hochgeladen werden. Jedes Dataset darf nur einen einzelnen Datentyp enthalten.

> [!NOTE]
> Benutzer des kostenlosen Abonnements (F0) können maximal 10 ZIP-Dateien pro Abonnement als Datasets importieren. Bei Benutzern des Standardabonnements (S0) beträgt die Höchstzahl 500.

## <a name="individual-utterances--matching-transcript"></a>Einzelne Äußerungen und entsprechendes Transkript

Zum Vorbereiten der Aufnahmen von einzelnen Äußerungen und des zugehörigen Transkripts stehen zwei Methoden zur Auswahl. Entweder erstellen Sie zunächst das Skript, das dann von einem Sprecher eingesprochen wird, oder Sie nutzen eine öffentlich verfügbare Audiodatei und transkribieren diese in Text. Wenn Sie sich für Letzteres entscheiden, müssen Füllwörter wie „ähm“ sowie gestotterte, undeutlich gesprochene und falsch ausgesprochene Wörter entfernt werden.

Die Erstellung eines guten Voicefonts setzt voraus, dass die Aufnahmen in einem ruhigen Raum mit einem qualitativ hochwertigen Mikrofon erstellt werden. Eine gleichmäßige Lautstärke, Geschwindigkeit und Tonhöhe sowie eine ausdrucksstarke Prosodie sind entscheidend.

> [!TIP]
> Wenn Sie eine Stimme für eine Produktionsumgebung erstellen möchten, werden ein professioneller Sprecher und ein professionelles Tonstudio empfohlen. Weitere Informationen finden Sie unter [Aufzeichnen von Sprachbeispielen für eine benutzerdefinierte Stimme](record-custom-voice-samples.md).

### <a name="audio-files"></a>Audiodateien

Jede Audiodatei sollte genau eine Äußerung enthalten (einen einzelnen Satz oder Turn eines Dialogsystems) und kürzer als 15 Sekunden sein. Alle Dateien müssen in derselben gesprochenen Sprache vorliegen. Mehrsprachige benutzerdefinierte Stimmen für die Sprachsynthese werden nicht unterstützt, mit Ausnahme des zweisprachigen Chinesisch-Englisch. Jede Audiodatei muss über einen eindeutigen numerischen Dateinamen mit der Erweiterung WAV verfügen.

Beachten Sie beim Vorbereiten der Audiodateien die folgenden Richtlinien.

| Eigenschaft | Wert |
| -------- | ----- |
| Dateiformat | RIFF (.wav), in einer ZIP-Datei gruppiert |
| Samplingrate | Mindestens 16.000 Hz |
| Beispielformat | PCM, 16 Bit |
| Dateiname | Numerisch, mit der Erweiterung WAV. Doppelte Dateinamen sind nicht zulässig. |
| Länge der Audiodatei | Kürzer als 15 Sekunden |
| Archivierungsformat | .zip |
| Maximale Archivgröße | 2048 MB |

> [!NOTE]
> WAV-Dateien mit einer Samplingrate unter 16.000 Hz werden zurückgewiesen. Wenn eine ZIP-Datei WAV-Dateien mit unterschiedlichen Samplingraten enthält, werden nur die Dateien importiert, deren Rate mindestens 16.000 Hz beträgt. Aktuell werden im Portal ZIP-Archiv-Importe von bis zu 200 MB unterstützt. Es besteht allerdings die Möglichkeit, mehrere Archive hochzuladen.

### <a name="transcripts"></a>Transkripte

Die Transkriptionsdatei ist eine reine Textdatei. Beachten Sie beim Vorbereiten Ihrer Transkriptionen die folgenden Richtlinien.

| Eigenschaft | Wert |
| -------- | ----- |
| Dateiformat | Nur-Text (.txt) |
| Codierungsformat | ANSI/ASCII, UTF-8, UTF-8-BOM, UTF-16-LE oder UTF-16-BE. Für zh-CN werden ANSI/ASCII- und UTF-8-Codierungen nicht unterstützt. |
| Anzahl von Äußerungen pro Zeile | **Eine**: Jede Zeile der Transkriptionsdatei muss den Namen einer der Audiodateien enthalten, gefolgt von der jeweiligen Transkription. Der Dateiname und die Transkription sollten durch ein Tabulatorzeichen (\t) getrennt werden. |
| Maximale Dateigröße | 2048 MB |

Das folgende Beispiel zeigt, wie die Transkripte nach Äußerungen unterteilt in einer TXT-Datei organisiert werden:

```
0000000001[tab] This is the waistline, and it's falling.
0000000002[tab] We have trouble scoring.
0000000003[tab] It was Janet Maslin.
```
Entscheidend ist, dass die Transkripte zu 100 % mit den zugehörigen Audioaufnahmen übereinstimmen. Fehler in den Transkripten führen während des Trainings zu Qualitätsverlusten.

> [!TIP]
> Beim Erstellen von Sprachsynthesestimmen für eine Produktionsumgebung sollten Sie bei der Auswahl der Äußerungen (oder beim Schreiben der Skripts) sowohl auf eine ausreichende Anzahl phonetischer Eigenschaften als auch auf die phonetische Effizienz achten. Erzielen Sie nicht die gewünschten Ergebnisse? [Wenden Sie sich an das Custom Voice-Team](mailto:speechsupport@microsoft.com), um mehr über die Beratung durch uns zu erfahren.

## <a name="long-audio--transcript-beta"></a>Lange Audiodatei und Transkript (Betaversion)

Es kann vorkommen, dass Sie nicht über segmentierte Audioaufnahmen verfügen. Wir stellen über das Custom Voice-Portal einen Dienst (Betaversion) bereit, mit dem Sie lange Audiodateien segmentieren und Transkriptionen erstellen können. Für die Nutzung dieses Diensts fallen Gebühren an, die über Ihr Abonnement für die Spracherkennung abgerechnet werden.

> [!NOTE]
> Der Dienst für die Segmentierung langer Audiodateien nutzt die Funktion zur Batch-Transkription der Spracherkennung, die nur für Benutzer mit einem Standardabonnement (S0) verfügbar ist. Bei der Verarbeitung der Segmentierung werden Ihre Audiodateien und die Transkripte auch an den Custom Speech Service gesendet, um das Erkennungsmodell zu optimieren und so die Genauigkeit Ihrer Daten zu verbessern. Während dieses Prozesses werden keine Daten beibehalten. Nach Abschluss der Segmentierung werden nur die segmentierten Äußerungen und die entsprechenden Transkripte zum Herunterladen und Trainieren gespeichert.

### <a name="audio-files"></a>Audiodateien

Beachten Sie beim Vorbereiten der Audiodateien für die Segmentierung die folgenden Richtlinien.

| Eigenschaft | Wert |
| -------- | ----- |
| Dateiformat | RIFF-Dateien (.wav) mit einer Samplingrate von mindestens 16 kHz und 16 Bit in PCM oder MP3-Dateien mit einer Bitrate von mindestens 256 Kbit/s, die in einer ZIP-Datei gruppiert sind |
| Dateiname | ASCII- und Unicode-Zeichen werden unterstützt. Doppelte Namen sind nicht zulässig. |
| Länge der Audiodatei | Länger als 20 Sekunden |
| Archivierungsformat | .zip |
| Maximale Archivgröße | 2048 MB |

Alle Audiodateien müssen in einer ZIP-Datei gruppiert werden. Das gemeinsame Speichern von WAV- und MP3-Dateien in einer einzelnen Audio-ZIP-Datei ist zulässig. Sie können z. B. eine ZIP-Datei hochladen, die eine 45 Sekunden lange Audiodatei mit dem Namen „kingstory.wav“ und eine weitere 200 Sekunden lange Audiodatei mit dem Namen „queenstory.mp3“ enthält. Alle MP3-Dateien werden nach der Verarbeitung in das WAV-Format transformiert.

### <a name="transcripts"></a>Transkripte

Transkripte müssen entsprechend den Spezifikationen in dieser Tabelle vorbereitet werden. Jede Audiodatei muss über ein entsprechendes Transkript verfügen.

| Eigenschaft | Wert |
| -------- | ----- |
| Dateiformat | Nur-Text (.txt), in einer ZIP-Datei gruppiert |
| Dateiname | Namen der entsprechenden Audiodatei verwenden |
| Codierungsformat | Nur UTF-8-BOM |
| Anzahl von Äußerungen pro Zeile | Keine Begrenzung |
| Maximale Dateigröße | 2048 MB |

Alle Transkriptdateien dieses Datentyps müssen in einer ZIP-Datei gruppiert werden. Angenommen, Sie haben eine ZIP-Datei hochgeladen, die eine 45 Sekunden lange Audiodatei mit dem Namen „kingstory.wav“ und eine weitere 200 Sekunden lange Audiodatei mit dem Namen „queenstory.mp3“ enthält. In diesem Fall müssen Sie eine weitere ZIP-Datei hochladen, die zwei Transkripte enthält – eine mit dem Namen „kingstory.txt“ und eine mit dem Namen „queenstory.txt“. In jeder Nur-Text-Datei stellen Sie die richtige und vollständige Transkription für die entsprechende Audioaufnahme bereit.

Nachdem das Dataset erfolgreich hochgeladen wurde, helfen wir Ihnen dabei, die Audiodatei basierend auf dem bereitgestellten Transkript in Äußerungen zu segmentieren. Sie können die segmentierten Äußerungen und die entsprechenden Transkripte überprüfen, indem Sie das Dataset herunterladen. Den segmentierten Äußerungen werden automatisch eindeutige IDs zugewiesen. Sie müssen unbedingt sicherstellen, dass die von Ihnen bereitgestellten Transkripte zu 100 % genau sind. Fehler in den Transkripten können die Genauigkeit der Audiosegmentierung beeinträchtigen und später beim Training zu weiteren Qualitätsverlusten führen.

## <a name="audio-only-beta"></a>Nur Audio (Betaversion)

Wenn Sie keine Transkriptionen für Ihre Audioaufnahmen haben, verwenden Sie die Option **Nur Audio** zum Hochladen Ihrer Daten. Unser System unterstützt Sie beim Segmentieren und Transkribieren Ihrer Audiodateien. Bedenken Sie, dass für die Nutzung dieses Diensts Gebühren anfallen, die über Ihr Abonnement für die Spracherkennung abgerechnet werden.

Beachten Sie beim Vorbereiten der Audiodateien die folgenden Richtlinien.

> [!NOTE]
> Der Dienst für die Segmentierung langer Audiodateien nutzt die Funktion zur Batch-Transkription der Spracherkennung, die nur für Benutzer mit einem Standardabonnement (S0) verfügbar ist.

| Eigenschaft | Wert |
| -------- | ----- |
| Dateiformat | RIFF-Dateien (.wav) mit einer Samplingrate von mindestens 16 kHz und 16 Bit in PCM oder MP3-Dateien mit einer Bitrate von mindestens 256 Kbit/s, die in einer ZIP-Datei gruppiert sind |
| Dateiname | ASCII- und Unicode-Zeichen werden unterstützt. Doppelte Namen sind nicht zulässig. |
| Länge der Audiodatei | Länger als 20 Sekunden |
| Archivierungsformat | .zip |
| Maximale Archivgröße | 2048 MB |

Alle Audiodateien müssen in einer ZIP-Datei gruppiert werden. Nachdem das Dataset erfolgreich hochgeladen wurde, helfen wir Ihnen dabei, die Audiodatei mit unserem Dienst für die Batch-Sprachtranskription in Äußerungen zu segmentieren. Den segmentierten Äußerungen werden automatisch eindeutige IDs zugewiesen. Zugehörige Transkripte werden mittels Spracherkennung generiert. Alle MP3-Dateien werden nach der Verarbeitung in das WAV-Format transformiert. Sie können die segmentierten Äußerungen und die entsprechenden Transkripte überprüfen, indem Sie das Dataset herunterladen.

## <a name="next-steps"></a>Nächste Schritte

- [Erstellen einer benutzerdefinierten Stimme](how-to-custom-voice-create-voice.md)
- [Aufzeichnen Aufnehmen Ihrer Sprachbeispiele](record-custom-voice-samples.md)
