---
title: Webhooks – Speech Services
titlesuffix: Azure Cognitive Services
description: Webhooks sind HTTP-Callbacks, die sich ideal zur Optimierung Ihrer Lösung bei zeitintensiven Prozessen wie Importen, Anpassungen, Genauigkeitsprüfungen oder Transkriptionen von zeitintensiven Dateien eignen.
services: cognitive-services
author: PanosPeriorellis
manager: nitinme
ms.service: cognitive-services
ms.subservice: speech-service
ms.topic: conceptual
ms.date: 07/05/2019
ms.author: panosper
ms.openlocfilehash: a100049ddfc9d4859e303546c1b10e814cf96ebb
ms.sourcegitcommit: f10ae7078e477531af5b61a7fe64ab0e389830e8
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 07/05/2019
ms.locfileid: "67606219"
---
# <a name="webhooks-for-speech-services"></a>Webhooks für Speech Services

Webhooks sind wie HTTP-Callbacks, die es Ihrer Anwendung ermöglichen, Daten von den Speech Services zu akzeptieren, sobald sie verfügbar sind. Mithilfe von Webhooks können Sie die Nutzung unserer REST-APIs optimieren, da Sie nicht ständig nach einer Antwort abfragen müssen. In den nächsten Abschnitten erfahren Sie, wie Sie Webhooks mit den Speech Services verwenden können.

## <a name="supported-operations"></a>Unterstützte Vorgänge

Die Speech Services unterstützen Webhooks für alle zeitintensiven Vorgänge. Jeder der unten aufgeführten Vorgänge kann nach Abschluss einen HTTP-Callback auslösen.

* DataImportCompletion
* ModelAdaptationCompletion
* AccuracyTestCompletion
* TranscriptionCompletion
* EndpointDeploymentCompletion
* EndpointDataCollectionCompletion

Als Nächstes erstellen wir einen Webhook.

## <a name="create-a-webhook"></a>Erstellen eines Webhooks

Wir erstellen einen Webhook für eine Offlinetranskription. Das Szenario: Ein Benutzer besitzt eine zeitintensive Audiodatei, die er asynchron mit der Batch-Transkriptions-API transkribieren möchte.

Webhooks können mit einer POST-Anforderung an https://\<Region\>.cris.ai/api/speechtotext/v2.1/transcriptions/hooks erstellt werden.

Die Konfigurationsparameter für die Anforderung werden als JSON bereitgestellt:

```json
{
  "configuration": {
    "url": "https://your.callback.url/goes/here",
    "secret": "<my_secret>"
  },
  "events": [
    "TranscriptionCompletion"
  ],
  "active": true,
  "name": "TranscriptionCompletionWebHook",
  "description": "This is a Webhook created to trigger an HTTP POST request when my audio file transcription is completed.",
  "properties": {
      "Active" : "True"
  }

}
```
Alle POST-Anforderungen an die Batch-Transkriptions-API erfordern einen `name`. Die Parameter `description` und `properties` sind optional.

Die Eigenschaft `Active` wird verwendet, um den Wechsel zurück in Ihre URL zu aktivieren oder zu deaktivieren, ohne die Webhookregistrierung löschen und neu erstellen zu müssen. Wenn Sie nach Abschluss des Prozesses nur einen Callback ausführen müssen, dann löschen Sie den Webhook, und legen Sie die Eigenschaft `Active` auf „false“ fest.

Der Ereignistyp `TranscriptionCompletion` wird im Ereignisarray bereitgestellt. Er führt einen Callback zum Endpunkt aus, wenn eine Transkription in einen Endzustand wechselt (`Succeeded` oder `Failed`). Beim Callback zur registrierten URL enthält die Anforderung einen `X-MicrosoftSpeechServices-Event`-Header, der einen der registrierten Ereignistypen enthält. Es gibt eine Anforderung pro registriertem Ereignistyp.

Es gibt einen Ereignistyp, den Sie nicht abonnieren können. Es handelt sich um den Ereignistyp `Ping`. Eine Anforderung mit diesem Typ wird an die URL gesendet, wenn die Erstellung eines Webhooks bei Verwendung der Ping-URL abgeschlossen ist (siehe unten).  

In der Konfiguration ist die Eigenschaft `url` erforderlich. An diese URL werden POST-Anforderungen gesendet. Das `secret` wird verwendet, um einen SHA256-Hash der Nutzlast zu erstellen, mit dem Geheimnis als HMAC-Schlüssel. Der Hash wird beim Aufruf der registrierten URL als `X-MicrosoftSpeechServices-Signature`-Header festgelegt. Dieser Header ist Base64-codiert.

In diesem Beispiel wird veranschaulicht, wie eine Nutzlast mit C# überprüft werden kann:

```csharp

private const string EventTypeHeaderName = "X-MicrosoftSpeechServices-Event";
private const string SignatureHeaderName = "X-MicrosoftSpeechServices-Signature";

[HttpPost]
public async Task<IActionResult> PostAsync([FromHeader(Name = EventTypeHeaderName)]WebHookEventType eventTypeHeader, [FromHeader(Name = SignatureHeaderName)]string signature)
{
    string body = string.Empty;
    using (var streamReader = new StreamReader(this.Request.Body))
    {
        body = await streamReader.ReadToEndAsync().ConfigureAwait(false);
        var secretBytes = Encoding.UTF8.GetBytes("my_secret");
        using (var hmacsha256 = new HMACSHA256(secretBytes))
        {
            var contentBytes = Encoding.UTF8.GetBytes(body);
            var contentHash = hmacsha256.ComputeHash(contentBytes);
            var storedHash = Convert.FromBase64String(signature);
            var validated = contentHash.SequenceEqual(storedHash);
        }
    }

    switch (eventTypeHeader)
    {
        case WebHookEventType.Ping:
            // Do your ping event related stuff here (or ignore this event)
            break;
        case WebHookEventType.TranscriptionCompletion:
            // Do your subscription related stuff here.
            break;
        default:
            break;
    }

    return this.Ok();
}

```
In diesem Codeausschnitt wird das `secret` decodiert und überprüft. Sie werden auch feststellen, dass der Webhookereignistyp gewechselt wurde. Derzeit gibt es ein Ereignis pro abgeschlossener Transkription. Der Code führt fünfmal für jedes Ereignis (mit einer Verzögerung von einer Sekunde) einen erneuten Versuch durch, bevor er aufgibt.

### <a name="other-webhook-operations"></a>Andere Webhookvorgänge

So rufen Sie alle registrierten Webhooks ab GET https://westus.cris.ai/api/speechtotext/v2.1/transcriptions/hooks

So rufen Sie einen bestimmten Webhook ab GET https://westus.cris.ai/api/speechtotext/v2.1/transcriptions/hooks/:id

So entfernen Sie einen bestimmten Webhook DELETE https://westus.cris.ai/api/speechtotext/v2.1/transcriptions/hooks/:id

> [!Note]
> Im obigen Beispiel ist die Region „westus“. Diese sollte durch die Region ersetzt werden, in der Sie Ihre Speech Services-Ressource im Azure-Portal erstellt haben.

POST https://westus.cris.ai/api/speechtotext/v2.1/transcriptions/hooks/:id/ping Body: empty

Sendet eine POST-Anforderung an die registrierte URL. Die Anforderung enthält einen `X-MicrosoftSpeechServices-Event`-Header mit einem Ping als Wert. Wenn der Webhook mit einem Geheimnis registriert wurde, enthält er einen `X-MicrosoftSpeechServices-Signature`-Header mit einem SHA256-Hash der Nutzlast mit dem Geheimnis als HMAC-Schlüssel. Der Hash ist Base64-codiert.

POST https://westus.cris.ai/api/speechtotext/v2.1/transcriptions/hooks/:id/test Body: empty

Sendet eine POST-Anforderung an die registrierte URL, wenn eine Entität für den abonnierten Ereignistyp (Transkription) im System vorhanden ist und sich im entsprechenden Zustand befindet. Die Nutzlast wird aus der letzten Entität generiert, die den Webhook aufgerufen hätte. Wenn keine Entität vorhanden ist, antwortet der POST mit 204. Wenn eine Testanforderung gestellt werden kann, lautet die Antwort 200. Der Anforderungstext weist dieselbe Form auf wie in der GET-Anforderung für eine bestimmte Entität, die der Webhook abonniert hat (z. B. Transkription). Die Anforderung verfügt wie zuvor beschrieben über die Header `X-MicrosoftSpeechServices-Event` und `X-MicrosoftSpeechServices-Signature`.

### <a name="run-a-test"></a>Ausführen eines Tests

Ein Schnelltest kann über die Website https://bin.webhookrelay.com durchgeführt werden. Von dort aus können Sie Callback-URLs erhalten, die als Parameter an den HTTP POST übergeben werden, um einen Webhook zu erstellen, der zuvor im Dokument beschrieben wurde.

Klicken Sie auf „Bucket erstellen“, und befolgen Sie die Anweisungen auf dem Bildschirm, um einen Hook zu erhalten. Verwenden Sie die auf dieser Seite bereitgestellten Informationen, um den Hook beim Speech-Dienst zu registrieren. Die Nutzlast einer weitergeleiteten Nachricht – als Reaktion auf den Abschluss einer Transkription – sieht wie folgt aus:

```json
{
    "results": [],
    "recordingsUrls": [
        "my recording URL"
    ],
    "models": [
        {
            "modelKind": "AcousticAndLanguage",
            "datasets": [],
            "id": "a09c8c8b-1090-443c-895c-3b1cf442dec4",
            "createdDateTime": "2019-03-26T12:48:46Z",
            "lastActionDateTime": "2019-03-26T14:04:47Z",
            "status": "Succeeded",
            "locale": "en-US",
            "name": "v4.13 Unified",
            "description": "Unified",
            "properties": {
                "Purpose": "OnlineTranscription,BatchTranscription,LanguageAdaptation",
                "ModelClass": "unified-v4"
            }
        }
    ],
    "statusMessage": "None.",
    "id": "d41615e1-a60e-444b-b063-129649810b3a",
    "createdDateTime": "2019-04-16T09:35:51Z",
    "lastActionDateTime": "2019-04-16T09:38:09Z",
    "status": "Succeeded",
    "locale": "en-US",
    "name": "Simple transcription",
    "description": "Simple transcription description",
    "properties": {
        "PunctuationMode": "DictatedAndAutomatic",
        "ProfanityFilterMode": "Masked",
        "AddWordLevelTimestamps": "True",
        "AddSentiment": "True",
        "Duration": "00:00:02"
    }
}
```
Die Nachricht enthält die Aufzeichnung-URL und die zum Transkribieren der Aufzeichnung verwendeten Modelle.

## <a name="next-steps"></a>Nächste Schritte

* [Abrufen Ihres Testabonnements für Speech](https://azure.microsoft.com/try/cognitive-services/)
