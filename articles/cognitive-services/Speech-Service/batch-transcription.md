---
title: Verwenden von Batch-Transkriptionen – Speech Services
titlesuffix: Azure Cognitive Services
description: Batch-Transkriptionen eignen sich besonders, wenn Sie eine große Menge von Audiodaten in einen Speicher wie z.B. Azure-Blobs transkribieren möchten. Mithilfe der spezifischen Rest-API können Sie per SAS-URI (Shared Access Signature) auf Audiodateien verweisen und Transkriptionen asynchron empfangen.
services: cognitive-services
author: PanosPeriorellis
manager: nitinme
ms.service: cognitive-services
ms.subservice: speech-service
ms.topic: conceptual
ms.date: 07/05/2019
ms.author: panosper
ms.openlocfilehash: b71400c3ae3c1cc6737d9194b4d94bf0b9c7efa9
ms.sourcegitcommit: f10ae7078e477531af5b61a7fe64ab0e389830e8
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 07/05/2019
ms.locfileid: "67606740"
---
# <a name="why-use-batch-transcription"></a>Gründe für die Verwendung von Batch-Transkriptionen

Batch-Transkriptionen eignen sich besonders, wenn Sie eine große Menge von Audiodaten in einen Speicher wie z.B. Azure-Blobs transkribieren möchten. Mithilfe der spezifischen Rest-API können Sie per SAS-URI (Shared Access Signature) auf Audiodateien verweisen und Transkriptionen asynchron empfangen.

## <a name="prerequisites"></a>Voraussetzungen

### <a name="subscription-key"></a>Abonnementschlüssel

Wie bei allen Features des Speech-Diensts erstellen Sie mithilfe der Anleitung unter [Erste Schritte](get-started.md) einen Abonnementschlüssel im [Azure-Portal](https://portal.azure.com). Wenn Sie das Abrufen von Transkriptionen aus unseren Basismodellen planen, müssen Sie außer der Schlüsselerstellung nichts weiter tun.

>[!NOTE]
> Für die Verwendung von Batch-Transkriptionen ist ein Standardabonnement (S0) für Speech Services erforderlich. Kostenlose Abonnementschlüssel (F0) funktionieren nicht. Weitere Informationen finden Sie unter [Preise und Einschränkungen](https://azure.microsoft.com/pricing/details/cognitive-services/speech-services/).

### <a name="custom-models"></a>Benutzerdefinierte Modelle

Wenn Sie die Anpassung von Audio- oder Sprachmodellen planen, befolgen Sie die Schritte unter [Anpassen von Akustikmodellen](how-to-customize-acoustic-models.md) und [Erstellen eines benutzerdefinierten Sprachmodells](how-to-customize-language-model.md). Um die erstellten Modelle in der Batch-Transkription zu verwenden, benötigen Sie ihre Modell-IDs. Bei dieser ID handelt es sich nicht um die Endpunkt-ID, die in der Ansicht „Endpunktdetails“ angegeben ist, sondern um die Modell-ID. Diese können Sie abrufen, indem Sie die Details der Modelle auswählen.

## <a name="the-batch-transcription-api"></a>Die Batch-Transkriptions-API

Die Batch-Transkriptions-API bietet asynchrone Transkription von Sprache in Text sowie zusätzliche Funktionen. Es handelt sich um eine REST-API, die Methoden für Folgendes verfügbar macht:

1. Erstellen von Anforderungen für Batch-Verarbeitungen
1. Abfragestatus
1. Herunterladen von Transkriptionen

> [!NOTE]
> Die Batch-Transkriptions-API eignet sich ideal für Callcenter, bei denen sich üblicherweise Tausende von Stunden mit Audioaufzeichnungen ansammeln. Sie vereinfacht die Transkription großer Mengen von Audioaufnahmen.

### <a name="supported-formats"></a>Unterstützte Formate

Die Batch-Transkriptions-API unterstützt die folgenden Formate:

| Format | Codec | Bitrate | Samplingrate |
|--------|-------|---------|-------------|
| WAV | PCM | 16 Bit | 8 oder 16 kHz, Mono, Stereo |
| MP3 | PCM | 16 Bit | 8 oder 16 kHz, Mono, Stereo |
| OGG | OPUS | 16 Bit | 8 oder 16 kHz, Mono, Stereo |

Bei Audiostreams in Stereo teilt die Batch-Transkriptions-API den linken und rechten Kanal während des Transkriptionsvorgangs. Die beiden JSON-Dateien mit dem Ergebnis werden jeweils von einem einzigen Kanal erstellt. Die Zeitstempel pro Äußerung ermöglicht es dem Entwickler, eine geordnete endgültige Transkription zu erstellen. Diese Beispielanforderung umfasst Eigenschaften für die Filterung anstößiger Ausdrücke, Satzzeichen und Zeitstempel auf Wortebene.

### <a name="configuration"></a>Konfiguration

Die Konfigurationsparameter werden als JSON angegeben:

```json
{
  "recordingsUrl": "<URL to the Azure blob to transcribe>",
  "models": [{"Id":"<optional acoustic model ID>"},{"Id":"<optional language model ID>"}],
  "locale": "<locale to us, for example en-US>",
  "name": "<user defined name of the transcription batch>",
  "description": "<optional description of the transcription>",
  "properties": {
    "ProfanityFilterMode": "Masked",
    "PunctuationMode": "DictatedAndAutomatic",
    "AddWordLevelTimestamps" : "True",
    "AddSentiment" : "True"
  }
}
```

> [!NOTE]
> Die Batch-Transkriptions-API verwendet einen REST-Dienst zum Anfordern von Transkriptionen, deren Status und zugehörigen Ergebnissen. Sie können die API in jeder Sprache verwenden. Im nächsten Abschnitt wird die Verwendung der API beschrieben.

### <a name="configuration-properties"></a>Konfigurationseigenschaften

Verwenden Sie diese optionalen Eigenschaften zum Konfigurieren der Transkription:

| Parameter | BESCHREIBUNG |
|-----------|-------------|
| `ProfanityFilterMode` | Gibt den Umgang mit Obszönitäten in Erkennungsergebnissen an. Zulässige Werte sind `none` (deaktiviert den Obszönitätenfilter), `masked` (Obszönitäten werden durch Sternchen ersetzt), `removed` (Obszönitäten werden aus dem Ergebnis entfernt) und `tags` (fügt „Obszönität“-Tags ein). Die Standardeinstellung ist `masked`. |
| `PunctuationMode` | Gibt den Umgang mit Satzzeichen in Erkennungsergebnissen an. Gültige Werte sind `none` (deaktiviert Satzzeichen), `dictated` (explizite Satzzeichen), `automatic` (der Decoder verwaltet die Satzzeichen), `dictatedandautomatic` (vorgeschriebene Satzzeichen) oder „automatic“. |
 | `AddWordLevelTimestamps` | Gibt an, ob der Ausgabe Zeitstempel auf Wortebene hinzugefügt werden sollen. Gültige Werte sind `true` zum Aktivieren und `false` zum Deaktivieren von Zeitstempeln auf Wortebene. |
 | `AddSentiment` | Gibt an, dass die Stimmung der Äußerung hinzugefügt werden soll. Gültige Werte sind `true` zum Aktivieren der Stimmung pro Äußerung und `false` (Standardwert) zum Deaktivieren. |
 | `AddDiarization` | Gibt an, dass die Diarisierungsanalyse bei der Eingabe durchgeführt werden sollte. Es wird erwartet, dass diese Eingabe ein Monokanal mit zwei Stimmen ist. Gültige Werte sind `true` zum Aktivieren der Diarisierung und `false` (der Standardwert) zu deren Deaktivierung. Außerdem muss `AddWordLevelTimestamps` auf „true“ festgelegt werden.|

### <a name="storage"></a>Storage

Die Batchtranskription unterstützt [Azure Blob-Speicher](https://docs.microsoft.com/azure/storage/blobs/storage-blobs-overview) zum Lesen von Audio und zum Schreiben von Transkriptionen in den Speicher.

## <a name="webhooks"></a>webhooks

Die Abfrage des Transkriptionsstatus ist möglicherweise nicht die leistungsstärkste Option oder bietet nicht die bestmögliche Benutzererfahrung. Zur Abfrage des Status können Sie Rückrufe registrieren, die den Client benachrichtigen, wenn zeitintensive Transkriptionsaufgaben abgeschlossen sind.

Weitere Informationen finden Sie unter [Webhooks](webhooks.md).

## <a name="speaker-separation-diarization"></a>Sprechertrennung (Diarisierung)

Diarisierung ist der Prozess, bei dem Sprecher in einem Audioelement getrennt werden. Unsere Batch-Pipeline unterstützt die Diarisierung und kann zwei Sprecher in Monokanalaufnahmen erkennen.

Wenn Sie anfordern möchten, dass Ihre Audiotranskriptionsanforderung zur Diarisierung verarbeitet wird, müssen Sie den relevanten Parameter einfach wie unten gezeigt in der HTTP-Anforderung hinzufügen.

 ```json
{
  "recordingsUrl": "<URL to the Azure blob to transcribe>",
  "models": [{"Id":"<optional acoustic model ID>"},{"Id":"<optional language model ID>"}],
  "locale": "<locale to us, for example en-US>",
  "name": "<user defined name of the transcription batch>",
  "description": "<optional description of the transcription>",
  "properties": {
    "AddWordLevelTimestamps" : "True",
    "AddDiarization" : "True"
  }
}
```

Zeitstempel auf Wortebene müssten außerdem in der oben angegebenen Anforderung als Parameter ‚aktiviert‘ werden.

Das entsprechende Audio enthält die durch eine Zahl identifizierten Sprecher (weil wir derzeit nur zwei Stimmen unterstützen, werden die Sprecher als ‚Sprecher 1‘ und ‚Sprecher 2‘ identifiziert), gefolgt von der Transkriptionsausgabe.

Beachten Sie außerdem, dass die Diarisierung in Stereoaufnahmen nicht verfügbar ist. Darüber hinaus enthalten alle JSON-Ausgaben das „Speaker“-Tag. Wenn keine Diarisierung verwendet wird, steht in der JSON-Ausgabe: ‚Speaker: Null‘.

> [!NOTE]
> Die Diarisierung ist in allen Regionen und für alle Gebietsschemas verfügbar!

## <a name="sentiment"></a>Stimmung

Die Stimmung ist ein neues Feature in der Batch-Transkriptions-API und ein wichtiges Feature in der Callcenterdomäne. Mit den `AddSentiment`-Parametern für ihre Anforderungen können Kunden:

1.  Einblicke in die Kundenzufriedenheit gewinnen
2.  Einblicke in die Leistung der Mitarbeiter erhalten (das Team, das Anrufe entgegennimmt)
3.  den exakten Zeitpunkt ermitteln, an dem sich ein Gespräch in eine negative Richtung entwickelt hat
4.  ermitteln, was zu einer positiven Entwicklung bei negativen Gesprächen geführt hat
5.  herausfinden, was Kunden an einem Produkt oder Dienst gefallen oder missfallen hat

Die Stimmung wird pro Audiosegment bewertet. Ein Audiosegment ist der Zeitraum vom Start der Äußerung (Offset) bis zur Erkennung von Stille am Ende des Byte-Streams. Der gesamte Text in diesem Segment wird zum Berechnen der Stimmung bewertet. Wir berechnen KEINE aggregierten Werte für den gesamten Anruf oder die gesamte Sprechzeit jedes Kanals. Diese Aggregationen bleiben dem Domänenbesitzer überlassen.

Die Stimmung wird auf die lexikalische Form angewendet.

Nachfolgend sehen Sie ein Beispiel für eine JSON-Ausgabe:

```json
{
  "AudioFileResults": [
    {
      "AudioFileName": "Channel.0.wav",
      "AudioFileUrl": null,
      "SegmentResults": [
        {
          "RecognitionStatus": "Success",
          "ChannelNumber": null,
          "Offset": 400000,
          "Duration": 13300000,
          "NBest": [
            {
              "Confidence": 0.976174,
              "Lexical": "what's the weather like",
              "ITN": "what's the weather like",
              "MaskedITN": "what's the weather like",
              "Display": "What's the weather like?",
              "Words": null,
              "Sentiment": {
                "Negative": 0.206194,
                "Neutral": 0.793785,
                "Positive": 0.0
              }
            }
          ]
        }
      ]
    }
  ]
}
```
Dieses Feature verwendet ein Stimmungsmodell, das sich derzeit in der Betaphase befindet.

## <a name="sample-code"></a>Beispielcode

Vollständige Beispiele stehen im [GitHub-Beispielrepository](https://aka.ms/csspeech/samples) innerhalb des Unterverzeichnisses `samples/batch` zur Verfügung.

Sie müssen den Beispielcode mit Ihren Abonnementinformationen, der Dienstregion, dem SAS-URI mit Verweis auf die zu transkribierende Audiodatei und die Modell-IDs anpassen, falls Sie ein benutzerdefiniertes Audio- oder Sprachmodell verwenden möchten.

[!code-csharp[Configuration variables for batch transcription](~/samples-cognitive-services-speech-sdk/samples/batch/csharp/program.cs#batchdefinition)]

Der Beispielcode richtet den Client ein und sendet die Transkriptionsanforderung. Anschließend fragt er die Statusinformationen ab und druckt Details über den Fortschritt der Transkription.

[!code-csharp[Code to check batch transcription status](~/samples-cognitive-services-speech-sdk/samples/batch/csharp/program.cs#batchstatus)]

Vollständige Informationen zu den vorherigen Aufrufen finden Sie in unserer [Swagger-Dokumentation](https://westus.cris.ai/swagger/ui/index) (in englischer Sprache). Das vollständige hier gezeigte Beispiel finden Sie auf [GitHub](https://aka.ms/csspeech/samples) im Unterverzeichnis `samples/batch`.

Beachten Sie das asynchrone Setup für das Senden von Audiodaten und das Empfangen des Transkriptionsstatus. Der von Ihnen erstellte Client ist ein .NET-HTTP-Client. Es gibt eine `PostTranscriptions`-Methode für das Senden der Audiodateidetails und eine `GetTranscriptions`-Methode zum Empfangen der Ergebnisse. `PostTranscriptions` gibt ein Handle zurück, und `GetTranscriptions` verwendet dieses Handle, um ein Handle zum Abrufen des Transkriptionsstatus zu erstellen.

Im aktuellen Beispielcode ist kein benutzerdefiniertes Modell angegeben. Der Dienst verwendet die Basismodelle zum Transkribieren der Datei bzw. Dateien. Zum Angeben der Modelle können Sie dieselbe Methode wie bei den Modell-IDs für das Akustikmodell und das Sprachmodell übergeben.

> [!NOTE]
> Für Basistranskriptionen müssen Sie die ID der Basismodelle nicht deklarieren. Wenn Sie nur eine Sprachmodell-ID (und keine Akustikmodell-ID) angeben, wird automatisch ein entsprechendes Akustikmodell ausgewählt. Wenn Sie nur eine Akustikmodell-ID angeben, wird automatisch ein entsprechendes Sprachmodell ausgewählt.

## <a name="download-the-sample"></a>Herunterladen des Beispiels

Sie finden das Beispiel im [GitHub-Beispielrepository](https://aka.ms/csspeech/samples) im Verzeichnis `samples/batch`.

> [!NOTE]
> Aufträge der Batch-Transkription werden auf bestmögliche Weise geplant. Es gibt keine geschätzte Zeit dafür, wann ein Auftrag in den Ausführungszustand wechselt. Sobald er ausgeführt wird, wird die aktuelle Transkription schneller als in Audioechtzeit verarbeitet.

## <a name="next-steps"></a>Nächste Schritte

* [Abrufen Ihres Testabonnements für Speech](https://azure.microsoft.com/try/cognitive-services/)
