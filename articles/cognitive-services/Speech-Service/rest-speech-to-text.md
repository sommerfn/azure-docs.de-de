---
title: Spracherkennungs-API-Referenz (REST) – Sprachdienste
titleSuffix: Azure Cognitive Services
description: Erfahren Sie, wie Sie die Spracherkennungs-REST-API verwenden. In diesem Artikel erfahren Sie mehr über Autorisierungs- und Abfrageoptionen sowie darüber, wie Sie eine Anforderung strukturieren und eine Antwort erhalten.
services: cognitive-services
author: erhopf
manager: nitinme
ms.service: cognitive-services
ms.subservice: speech-service
ms.topic: conceptual
ms.date: 03/13/2019
ms.author: erhopf
ms.custom: seodec18
ms.openlocfilehash: 4d5bceff004d1cc6ddac4046a2ddcd0b5f0b5e73
ms.sourcegitcommit: 41ca82b5f95d2e07b0c7f9025b912daf0ab21909
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 06/13/2019
ms.locfileid: "67072517"
---
# <a name="speech-to-text-rest-api"></a>Spracherkennungs-REST-API

Als Alternative zum [Speech SDK](speech-sdk.md) ermöglichen die Sprachdienste das Konvertieren von Sprache in Text mithilfe einer REST-API. Jeder zugängliche Endpunkt ist einer Region zugeordnet. Ihre Anwendung benötigt einen Abonnementschlüssel für den Endpunkt, den Sie verwenden möchten.

Vor der Verwendung der Spracherkennungs-REST-API müssen Sie Folgendes verstanden haben:
* Anforderungen, die die REST-API verwenden, dürfen nur 10 Sekunden aufgezeichnetes Audiomaterial enthalten.
* Die Spracherkennung-REST-API gibt nur Endergebnisse zurück. Teilergebnisse werden nicht bereitgestellt.

Wenn das Senden von längerem Audio eine Anforderung für Ihre Anwendung ist, verwenden Sie das [Speech SDK](speech-sdk.md) oder die [Batchtranskription](batch-transcription.md).

[!INCLUDE [](../../../includes/cognitive-services-speech-service-rest-auth.md)]

## <a name="regions-and-endpoints"></a>Regionen und Endpunkte

Diese Regionen werden für die Spracherkennungstranskription über die REST-API unterstützt. Achten Sie darauf, dass Sie den Endpunkt für Ihre Abonnementregion auswählen.

[!INCLUDE [](../../../includes/cognitive-services-speech-service-endpoints-speech-to-text.md)]

## <a name="query-parameters"></a>Abfrageparameter

Diese Parameter können in der Abfragezeichenfolge der REST-Anforderung enthalten sein.

| Parameter | BESCHREIBUNG | Erforderlich/optional |
|-----------|-------------|---------------------|
| `language` | Identifiziert die gesprochene Sprache, die erkannt wird. Siehe [Unterstützte Sprachen](language-support.md#speech-to-text). | Erforderlich |
| `format` | Gibt das Ergebnisformat an. Zulässige Werte sind `simple` und `detailed`. Einfache Ergebnisse enthalten `RecognitionStatus`, `DisplayText`, `Offset` und `Duration`. Detaillierte Antworten enthalten mehrere Ergebnisse mit Zuverlässigkeitswerten und vier unterschiedliche Darstellungen. Die Standardeinstellung ist `simple`. | Optional |
| `profanity` | Gibt den Umgang mit Obszönitäten in Erkennungsergebnissen an. Zulässige Werte sind `masked` (Obszönitäten werden durch Sternchen ersetzt), `removed` (Obszönitäten werden aus dem Ergebnis entfernt) und `raw` (Obszönitäten sind im Ergebnis enthalten). Die Standardeinstellung ist `masked`. | Optional |

## <a name="request-headers"></a>Anforderungsheader

Diese Tabelle führt die erforderlichen und optionalen Header für Spracherkennungsanforderungen auf.

|Header| BESCHREIBUNG | Erforderlich/optional |
|------|-------------|---------------------|
| `Ocp-Apim-Subscription-Key` | Ihr Abonnementschlüssel des Spracherkennungsdiensts. | Entweder dieser Header oder `Authorization` ist erforderlich. |
| `Authorization` | Ein Autorisierungstoken, dem das Wort `Bearer` vorangestellt ist. Weitere Informationen finden Sie unter [Authentifizierung](#authentication). | Entweder dieser Header oder `Ocp-Apim-Subscription-Key` ist erforderlich. |
| `Content-type` | Beschreibt das Format und den Codec der bereitgestellten Audiodaten. Zulässige Werte sind `audio/wav; codecs=audio/pcm; samplerate=16000` und `audio/ogg; codecs=opus`. | Erforderlich |
| `Transfer-Encoding` | Gibt an, dass segmentierte Audiodaten anstatt einer einzelnen Datei gesendet werden. Verwenden Sie diesen Header nur, wenn Sie Audiodaten segmentieren. | Optional |
| `Expect` | Wenn Sie segmentierte Übertragung verwenden, senden Sie `Expect: 100-continue`. Der Spracherkennungsdienst bestätigt die ursprüngliche Anforderung und wartet auf weitere Daten.| Erforderlich, wenn segmentierte Audiodaten gesendet werden. |
| `Accept` | Wenn angegeben, muss der Wert `application/json` entsprechen. Der Spracherkennungsdienst übermittelt stellt Ergebnisse im JSON-Format bereit. Einige Webanforderungsframeworks stellen einen inkompatiblen Standardwert bereit, wenn Sie keinen Wert angeben. Deswegen wird empfohlen, `Accept` immer zu verwenden. | Optional, wird jedoch empfohlen. |

## <a name="audio-formats"></a>Audioformate

Audiodaten werden im Text der HTTP-`POST`-Anforderung gesendet. Sie müssen in einem der in der folgenden Tabelle aufgeführten Formate vorliegen:

| Format | Codec | Bitrate | Samplingrate |
|--------|-------|---------|-------------|
| WAV | PCM | 16 Bit | 16 kHz, mono |
| OGG | OPUS | 16 Bit | 16 kHz, mono |

>[!NOTE]
>Die oben genannten Formate werden durch die REST-API und WebSocket im Spracherkennungsdienst unterstützt. Das [Speech-SDK](speech-sdk.md) unterstützt gegenwärtig nur das WAV-Format mit dem PCM-Codec.

## <a name="sample-request"></a>Beispiel für eine Anforderung

Hier sehen Sie eine übliche HTTP-Anforderung. Das folgende Beispiel enthält den Hostnamen und die erforderlichen Header. Beachten Sie, dass der Dienst auch Audiodaten erwartet, die in diesem Beispiel nicht enthalten sind. Wie bereits erwähnt, wird die Segmentierung empfohlen, ist aber nicht erforderlich.

```HTTP
POST speech/recognition/conversation/cognitiveservices/v1?language=en-US&format=detailed HTTP/1.1
Accept: application/json;text/xml
Content-Type: audio/wav; codecs=audio/pcm; samplerate=16000
Ocp-Apim-Subscription-Key: YOUR_SUBSCRIPTION_KEY
Host: westus.stt.speech.microsoft.com
Transfer-Encoding: chunked
Expect: 100-continue
```

## <a name="http-status-codes"></a>HTTP-Statuscodes

Der HTTP-Statuscode jeder Antwort zeigt den Erfolg oder allgemeine Fehler an.

| HTTP-Statuscode | BESCHREIBUNG | Mögliche Ursache |
|------------------|-------------|-----------------|
| 100 | Weiter | Die ursprüngliche Anforderung wurde akzeptiert. Mit dem Senden der restlichen Daten fortfahren. (Wird mit segmentierter Übertragung verwendet.) |
| 200 | OK | Die Anforderung war erfolgreich. Der Antworttext ist ein JSON-Objekt. |
| 400 | Ungültige Anforderung | Der Sprachcode wurde nicht bereitgestellt oder ist keine unterstützte Sprache, ungültige Audiodatei. |
| 401 | Nicht autorisiert | Der Abonnementschlüssel oder das Autorisierungstoken ist in der angegebenen Region ungültig oder ungültiger Endpunkt. |
| 403 | Verboten | Fehlender Abonnementschlüssel oder fehlendes Autorisierungstoken. |

## <a name="chunked-transfer"></a>Segmentierte Übertragung

Die segmentierte Übertragung (`Transfer-Encoding: chunked`) kann die Erkennungslatenz verringern, da sie dem Spracherkennungsdienst ermöglicht, mit der Verarbeitung der Audiodatei bei der Übertragung zu beginnen. Der REST-API bietet keine Teil- oder Zwischenergebnisse. Diese Option dient ausschließlich zur Verbesserung der Reaktionsfähigkeit.

Dieses Codebeispiel zeigt, wie Sie Audio in Blöcken senden. Nur der erste Block sollte den Header der Audiodatei enthalten. `request` ist ein HTTPWebRequest-Objekt, das mit dem entsprechenden REST-Endpunkt verbunden ist. `audioFile` ist der Pfad zu einer Audiodatei auf dem Datenträger.

```csharp

    HttpWebRequest request = null;
    request = (HttpWebRequest)HttpWebRequest.Create(requestUri);
    request.SendChunked = true;
    request.Accept = @"application/json;text/xml";
    request.Method = "POST";
    request.ProtocolVersion = HttpVersion.Version11;
    request.Host = host;
    request.ContentType = @"audio/wav; codecs=audio/pcm; samplerate=16000";
    request.Headers["Ocp-Apim-Subscription-Key"] = args[1];
    request.AllowWriteStreamBuffering = false;

using (fs = new FileStream(audioFile, FileMode.Open, FileAccess.Read))
{
    /*
    * Open a request stream and write 1024 byte chunks in the stream one at a time.
    */
    byte[] buffer = null;
    int bytesRead = 0;
    using (Stream requestStream = request.GetRequestStream())
    {
        /*
        * Read 1024 raw bytes from the input audio file.
        */
        buffer = new Byte[checked((uint)Math.Min(1024, (int)fs.Length))];
        while ((bytesRead = fs.Read(buffer, 0, buffer.Length)) != 0)
        {
            requestStream.Write(buffer, 0, bytesRead);
        }

        // Flush
        requestStream.Flush();
    }
}
```

## <a name="response-parameters"></a>Antwortparameter

Ergebnisse werden im JSON-Format bereitgestellt. Das `simple`-Format schließt diese Felder auf oberster Ebene ein.

| Parameter | BESCHREIBUNG  |
|-----------|--------------|
|`RecognitionStatus`|Status, z.B. `Success` für erfolgreiche Erkennung. Siehe nächste Tabelle.|
|`DisplayText`|Der erkannte Text nach Großschreibung, Interpunktion, inverser Textnormalisierung (Umwandlung von gesprochenem Text in kürzere Formen, z.B. 200 für „zweihundert“ oder „Dr. Smith“ für „doctor smith“) und Obszönitätenmaskierung. Nur bei Erfolg vorhanden.|
|`Offset`|Die Zeit (in Einheiten von 100 Nanosekunden), zu der die erkannte Sprache im Audiostream beginnt.|
|`Duration`|Die Dauer (in Einheiten von 100 Nanosekunden) der erkannten Sprache im Audiostream.|

Das `RecognitionStatus`-Feld kann diese Werte enthalten:

| Status | BESCHREIBUNG |
|--------|-------------|
| `Success` | Die Erkennung war erfolgreich, und das `DisplayText`-Feld ist vorhanden. |
| `NoMatch` | Im Audiodatenstrom wurde Sprache erkannt, aber es wurde keine Übereinstimmung mit Wörtern aus der Zielsprache festgestellt. Normalerweise bedeutet dies, dass die Erkennungssprache eine andere Sprache ist als die, die der Benutzer spricht. |
| `InitialSilenceTimeout` | Der Anfang des Audiodatenstroms enthielt nur Stille, und beim Warten auf Sprache wurde das Timeout des Diensts aktiviert. |
| `BabbleTimeout` | Der Anfang des Audiodatenstroms enthielt nur Rauschen, und beim Warten auf Sprache wurde das Timeout des Diensts aktiviert. |
| `Error` | Der Erkennungsdienst hat einen internen Fehler erkannt und konnte nicht fortgesetzt werden. Versuchen Sie es noch mal, wenn möglich. |

> [!NOTE]
> Wenn die Audiodaten nur aus Obszönitäten bestehen und der `profanity`-Abfrageparameter auf `remove` festgelegt ist, gibt der Dienst kein Sprachergebnis zurück.

Das Format `detailed` enthält die gleichen Daten wie das Format `simple` sowie `NBest`, eine Liste alternativer Interpretationen desselben Erkennungsergebnisses. Diese Ergebnisse werden abnehmend nach Wahrscheinlichkeit geordnet. Der erste Eintrag ist identisch mit den Haupterkennungsergebnis.  Bei Verwendung des Formats `detailed` wird `DisplayText` als `Display` für jedes Ergebnis in der `NBest`-Liste angegeben.

Jedes Objekt in der `NBest`-Liste enthält:

| Parameter | BESCHREIBUNG |
|-----------|-------------|
| `Confidence` | Die Zuverlässigkeitsbewertung des Eintrags von 0,0 (keine Zuverlässigkeit) bis 1,0 (volle Zuverlässigkeit) |
| `Lexical` | Die lexikalische Form des erkannten Texts: die tatsächlich erkannten Wörter. |
| `ITN` | Die inverse Textnormalisierung („kanonische Form“) des erkannten Texts mit Telefonnummern, Zahlen, Abkürzungen („doctor smith“ in „dr. smith“) und anderen angewendeten Transformationen. |
| `MaskedITN` | Die ITN-Form mit angewendeter Obszönitätenmaskierung, wenn angefordert. |
| `Display` | Die Anzeigeform des erkannten Texts mit hinzugefügten Satzzeichen und Großschreibung. Dieser Parameter ist derselbe wie `DisplayText`, wenn das Format `simple` entspricht. |

## <a name="sample-responses"></a>Beispielantworten

Hier sehen Sie eine typische Antwort für die Erkennung `simple`.

```json
{
  "RecognitionStatus": "Success",
  "DisplayText": "Remind me to buy 5 pencils.",
  "Offset": "1236645672289",
  "Duration": "1236645672289"
}
```

Hier sehen Sie eine typische Antwort für die Erkennung `detailed`.

```json
{
  "RecognitionStatus": "Success",
  "Offset": "1236645672289",
  "Duration": "1236645672289",
  "NBest": [
      {
        "Confidence" : "0.87",
        "Lexical" : "remind me to buy five pencils",
        "ITN" : "remind me to buy 5 pencils",
        "MaskedITN" : "remind me to buy 5 pencils",
        "Display" : "Remind me to buy 5 pencils.",
      },
      {
        "Confidence" : "0.54",
        "Lexical" : "rewind me to buy five pencils",
        "ITN" : "rewind me to buy 5 pencils",
        "MaskedITN" : "rewind me to buy 5 pencils",
        "Display" : "Rewind me to buy 5 pencils.",
      }
  ]
}
```

## <a name="next-steps"></a>Nächste Schritte

- [Abrufen Ihres Testabonnements für Speech](https://azure.microsoft.com/try/cognitive-services/)
- [Tutorial: Erstellen eines benutzerdefinierten Akustikmodells](how-to-customize-acoustic-models.md)
- [Tutorial: Erstellen eines benutzerdefinierten Sprachmodells](how-to-customize-language-model.md)
