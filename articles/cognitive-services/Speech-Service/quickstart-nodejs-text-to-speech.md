---
title: 'Schnellstart: Konvertieren von Text in Sprache, Node.js – Speech-Dienst'
titleSuffix: Azure Cognitive Services
description: In dieser Schnellstartanleitung erfahren Sie, wie Sie mithilfe von Node.js und der Text-to-Speech-REST-API Text in Sprache konvertieren. Der Beispieltext in diesem Leitfaden ist als SSML (Speech Synthesis Markup Language) strukturiert. Auf diese Weise können Sie die Stimme und Sprache für die Sprachantwort auswählen.
services: cognitive-services
author: erhopf
manager: nitinme
ms.service: cognitive-services
ms.subservice: speech-service
ms.topic: quickstart
ms.date: 07/05/2019
ms.author: erhopf
ms.openlocfilehash: 5a218db0527a5e1d5642cb485b75df894a275764
ms.sourcegitcommit: f10ae7078e477531af5b61a7fe64ab0e389830e8
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 07/05/2019
ms.locfileid: "67605003"
---
# <a name="quickstart-convert-text-to-speech-using-nodejs"></a>Schnellstart: Konvertieren von Text in Sprache mit Node.js

In dieser Schnellstartanleitung erfahren Sie, wie Sie mithilfe von Node.js und der Text-to-Speech-REST-API Text in Sprache konvertieren. Der in diesem Leitfaden enthaltene Anforderungstext ist als [Speech Synthesis Markup Language (SSML)](speech-synthesis-markup.md) strukturiert, wodurch Sie die Stimme und die Sprache für die Sprachantwort auswählen können.

Für diese Schnellstartanleitung wird ein [Azure Cognitive Services-Konto](https://docs.microsoft.com/azure/cognitive-services/cognitive-services-apis-create-account) mit einer Ressource für den Speech-Dienst benötigt. Wenn Sie über kein Konto verfügen, können Sie über die [kostenlose Testversion](get-started.md) einen Abonnementschlüssel abrufen.

## <a name="prerequisites"></a>Voraussetzungen

Für diese Schnellstartanleitung ist Folgendes erforderlich:

* [Node 8.12.x oder höher](https://nodejs.org/en/)
* [Visual Studio](https://visualstudio.microsoft.com/downloads/), [Visual Studio Code](https://code.visualstudio.com/download) oder ein anderer Editor
* Ein Azure-Abonnementschlüssel für die Speech-Dienste. [Hier erhalten Sie einen kostenlosen Schlüssel.](get-started.md)

## <a name="create-a-project-and-require-dependencies"></a>Erstellen eines Projekts und der erforderlichen Abhängigkeiten

Erstellen Sie in Ihrer bevorzugten IDE oder Ihrem bevorzugten Editor ein neues Node.js-Projekt. Kopieren Sie anschließend den folgenden Codeausschnitt in Ihr Projekt in eine Datei namens `tts.js`.

```javascript
// Requires request and request-promise for HTTP requests
// e.g. npm install request request-promise
const rp = require('request-promise');
// Requires fs to write synthesized speech to a file
const fs = require('fs');
// Requires readline-sync to read command line inputs
const readline = require('readline-sync');
// Requires xmlbuilder to build the SSML body
const xmlbuilder = require('xmlbuilder');
```

> [!NOTE]
> Wenn Sie diese Module bisher nicht verwendet haben, müssen Sie sie vor der Ausführung Ihres Programms installieren. Führen Sie zum Installieren dieser Pakete `npm install request request-promise xmlbuilder readline-sync` aus.

## <a name="get-an-access-token"></a>Abrufen eines Zugriffstokens

Alle Anforderungen an die Text-to-Speech-REST-API erfordern ein Zugriffstoken für die Authentifizierung. Um ein Zugriffstoken anzufordern, ist ein Austausch erforderlich. In dieser Funktion wird Ihr Abonnementschlüssel für den Speech-Dienst unter Verwendung des `issueToken`-Endpunkts gegen ein Zugriffstoken ausgetauscht.

In diesem Beispiel wird vorausgesetzt, dass Ihr Abonnement für den Speech-Dienst in der Region „USA, Westen“ enthalten ist. Wenn Sie eine andere Region verwenden, aktualisieren Sie den Wert für `uri`. Eine vollständige Liste finden Sie unter [Regionen](https://docs.microsoft.com/azure/cognitive-services/speech-service/regions#rest-apis).

Kopieren Sie diesen Code in Ihr Projekt:

```javascript
// Gets an access token.
function getAccessToken(subscriptionKey) {
    let options = {
        method: 'POST',
        uri: 'https://westus.api.cognitive.microsoft.com/sts/v1.0/issueToken',
        headers: {
            'Ocp-Apim-Subscription-Key': subscriptionKey
        }
    }
    return rp(options);
}
```

> [!NOTE]
> Weitere Informationen zur Authentifizierung finden Sie unter [Authentifizieren mit einem Authentifizierungstoken](https://docs.microsoft.com/azure/cognitive-services/authentication#authenticate-with-an-authentication-token).

Im nächsten Abschnitt erstellen wir die Funktion zum Aufrufen der Text-to-Speech-API und speichern die Sprachsyntheseantwort.

## <a name="make-a-request-and-save-the-response"></a>Senden der Anforderung und Speichern der Antwort

Hier erstellen Sie die Anforderung an die Text-to-Speech-API und speichern die Sprachantwort. In diesem Beispiel wird davon ausgegangen, dass Sie den Endpunkt „USA, Westen“ verwenden. Wenn Ihre Ressource in einer anderen Region registriert ist, stellen Sie sicher, dass Sie die `uri` aktualisieren. Weitere Informationen finden Sie unter [Sprachdienstregionen](https://docs.microsoft.com/azure/cognitive-services/speech-service/regions#text-to-speech).

Als Nächstes müssen Sie erforderliche Header für die Anforderung hinzufügen. Stellen Sie sicher, dass Sie `User-Agent` mit dem Namen Ihrer Ressourcengruppe aktualisieren (befindet sich im Azure-Portal), und `X-Microsoft-OutputFormat` auf Ihre bevorzugte Audioausgabe festlegen. Eine vollständige Liste der Ausgabeformate finden Sie unter [Audioausgaben](https://docs.microsoft.com/azure/cognitive-services/speech-service/rest-apis).

Erstellen Sie dann den Anforderungstext mithilfe der Speech Synthesis Markup Language (SSML). Dieses Beispiel definiert die Struktur und verwendet die zuvor von Ihnen erstellte `text`-Eingabe.

>[!NOTE]
> In diesem Beispiel wird der Voicefont `JessaRUS` verwendet. Eine vollständige Liste der von Microsoft bereitgestellten Stimmen/Sprachen finden Sie unter [Sprachunterstützung](language-support.md).
> Wenn Sie daran interessiert sind, eine eigene Stimme mit Wiedererkennungswert für Ihre Marke zu erstellen, finden Sie unter [Erstellen von benutzerdefinierten Voicefonts](how-to-customize-voice-font.md) weitere Informationen.

Abschließend stellen Sie eine Anforderung an den Dienst. Wenn die Anforderung erfolgreich war und der Statuscode 200 zurückgegeben wird, wird die Sprachausgabe als `TTSOutput.wav` geschrieben.

```javascript
// Make sure to update User-Agent with the name of your resource.
// You can also change the voice and output formats. See:
// https://docs.microsoft.com/azure/cognitive-services/speech-service/language-support#text-to-speech
function textToSpeech(accessToken, text) {
    // Create the SSML request.
    let xml_body = xmlbuilder.create('speak')
        .att('version', '1.0')
        .att('xml:lang', 'en-us')
        .ele('voice')
        .att('xml:lang', 'en-us')
        .att('name', 'Microsoft Server Speech Text to Speech Voice (en-US, Guy24KRUS)')
        .txt(text)
        .end();
    // Convert the XML into a string to send in the TTS request.
    let body = xml_body.toString();

    let options = {
        method: 'POST',
        baseUrl: 'https://westus.tts.speech.microsoft.com/',
        url: 'cognitiveservices/v1',
        headers: {
            'Authorization': 'Bearer ' + accessToken,
            'cache-control': 'no-cache',
            'User-Agent': 'YOUR_RESOURCE_NAME',
            'X-Microsoft-OutputFormat': 'riff-24khz-16bit-mono-pcm',
            'Content-Type': 'application/ssml+xml'
        },
        body: body
    }

    let request = rp(options)
        .on('response', (response) => {
            if (response.statusCode === 200) {
                request.pipe(fs.createWriteStream('TTSOutput.wav'));
                console.log('\nYour file is ready.\n')
            }
        });
    return request;
}
```

## <a name="put-it-all-together"></a>Korrektes Zusammenfügen

Sie sind fast fertig. Der letzte Schritt ist das Erstellen einer asynchronen Funktion. Diese Funktion liest Ihren Abonnementschlüssel aus einer Umgebungsvariablen, fordert zur Eingabe von Text auf, ruft ein Token ab, wartet, bis die Anforderung abgeschlossen wurde, konvertiert dann den Text in Sprache und speichert das Audio als WAV-Datei.

Wenn Sie mit Umgebungsvariablen nicht vertraut sind oder es vorziehen, mit Ihrem Abonnementschlüssel zu testen, der als Zeichenfolge hartcodiert ist, ersetzen Sie `process.env.SPEECH_SERVICE_KEY` durch Ihren Abonnementschlüssel als Zeichenfolge.

```javascript
// Use async and await to get the token before attempting
// to convert text to speech.
async function main() {
    // Reads subscription key from env variable.
    // You can replace this with a string containing your subscription key. If
    // you prefer not to read from an env variable.
    // e.g. const subscriptionKey = "your_key_here";
    const subscriptionKey = process.env.SPEECH_SERVICE_KEY;
    if (!subscriptionKey) {
        throw new Error('Environment variable for your subscription key is not set.')
    };
    // Prompts the user to input text.
    const text = readline.question('What would you like to convert to speech? ');

    try {
        const accessToken = await getAccessToken(subscriptionKey);
        await textToSpeech(accessToken, text);
    } catch (err) {
        console.log(`Something went wrong: ${err}`);
    }
}

main()
```

## <a name="run-the-sample-app"></a>Ausführen der Beispiel-App

Fertig! Sie können Ihre Beispiel-App jetzt ausführen. Navigieren Sie über die Befehlszeile (oder Terminalsitzung) zu Ihrem Projektverzeichnis, und führen Sie Folgendes aus:

```console
node tts.js
```

Wenn Sie dazu aufgefordert werden, geben Sie den Text ein, den Sie in Sprache konvertieren möchten. Bei erfolgreicher Ausführung wird die Sprachdatei in Ihrem Projektordner gespeichert. Geben Sie die Sprachdatei mit Ihrem bevorzugten Medienplayer wieder.

## <a name="clean-up-resources"></a>Bereinigen von Ressourcen

Entfernen Sie unbedingt alle vertraulichen Informationen wie etwa Abonnementschlüssel aus dem Quellcode Ihrer Beispiel-App.

## <a name="next-steps"></a>Nächste Schritte

> [!div class="nextstepaction"]
> [Node.js-Beispiele auf GitHub](https://github.com/Azure-Samples/Cognitive-Speech-TTS/tree/master/Samples-Http/NodeJS)

## <a name="see-also"></a>Weitere Informationen

* [Referenz zur Text-to-Speech-API](https://docs.microsoft.com/azure/cognitive-services/speech-service/rest-apis)
* [Erstellen benutzerdefinierter Voicefonts](how-to-customize-voice-font.md)
* [Aufzeichnen von Sprachbeispielen zum Erstellen einer benutzerdefinierten Stimme](record-custom-voice-samples.md)
