---
title: 'Schnellstart: Konvertieren von Text in Sprache, Python – Speech-Dienst'
titleSuffix: Azure Cognitive Services
description: In diesem Schnellstart erfahren Sie, wie Sie mithilfe von Python und der Text-to-Speech-REST-API Text in Sprache konvertieren. Der Beispieltext in diesem Leitfaden ist als SSML (Speech Synthesis Markup Language) strukturiert. Auf diese Weise können Sie die Stimme und Sprache für die Sprachantwort auswählen.
services: cognitive-services
author: erhopf
manager: nitinme
ms.service: cognitive-services
ms.subservice: speech-service
ms.topic: quickstart
ms.date: 07/05/2019
ms.author: erhopf
ms.openlocfilehash: a66835d605b9005b8f94eb79a3c266f735f0a3b6
ms.sourcegitcommit: c22327552d62f88aeaa321189f9b9a631525027c
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 11/04/2019
ms.locfileid: "73467216"
---
# <a name="quickstart-convert-text-to-speech-using-python"></a>Schnellstart: Konvertieren von Text in Sprache mit Python

In diesem Schnellstart erfahren Sie, wie Sie mithilfe von Python und der Text-to-Speech-REST-API Text in Sprache konvertieren. Der in diesem Leitfaden enthaltene Anforderungstext ist als [Speech Synthesis Markup Language (SSML)](speech-synthesis-markup.md) strukturiert, wodurch Sie die Stimme und die Sprache für die Sprachantwort auswählen können.

Für diese Schnellstartanleitung wird ein [Azure Cognitive Services-Konto](https://docs.microsoft.com/azure/cognitive-services/cognitive-services-apis-create-account) mit einer Ressource für den Speech-Dienst benötigt. Wenn Sie über kein Konto verfügen, können Sie über die [kostenlose Testversion](get-started.md) einen Abonnementschlüssel abrufen.

## <a name="prerequisites"></a>Voraussetzungen

Für diese Schnellstartanleitung ist Folgendes erforderlich:

* Python 2.7.x oder 3.x
* [Visual Studio](https://visualstudio.microsoft.com/downloads/), [Visual Studio Code](https://code.visualstudio.com/download) oder ein anderer Editor
* Ein Azure-Abonnementschlüssel für die Speech-Dienste

## <a name="create-a-project-and-import-required-modules"></a>Erstellen eines Projekts und Importieren der erforderlichen Module

Erstellen Sie in Ihrer bevorzugten IDE oder Ihrem bevorzugten Editor ein neues Python-Projekt. Kopieren Sie anschließend den folgenden Codeausschnitt in Ihr Projekt in eine Datei namens `tts.py`.

```python
import os
import requests
import time
from xml.etree import ElementTree
```

> [!NOTE]
> Wenn Sie diese Module bisher nicht verwendet haben, müssen Sie sie vor der Ausführung Ihres Programms installieren. Führen Sie zum Installieren dieser Pakete `pip install requests` aus.

Diese Module werden verwendet, um die Sprachausgabe in eine Datei mit einem Zeitstempel zu schreiben, die HTTP-Anforderung zu erstellen und die Text-to-Speech-API aufzurufen.

## <a name="set-the-subscription-key-and-create-a-prompt-for-tts"></a>Festlegen des Abonnementschlüssels und Erstellen einer Eingabeaufforderung für TTS

In den nächsten Abschnitten werden Sie Methoden zur Handhabung der Autorisierung erstellen, die Text-to-Speech-API aufrufen und die Antwort überprüfen. Fangen wir damit an, Code hinzuzufügen, der sicherstellt, dass dieses Beispiel mit Python 2.7.x und 3.x funktioniert.

```python
try:
    input = raw_input
except NameError:
    pass
```

Als Nächstes erstellen wir eine Klasse. Hier werden wir unsere Methoden für den Tokenaustausch und den Aufruf der Text-to-Speech-API einfügen.

```python
class TextToSpeech(object):
    def __init__(self, subscription_key):
        self.subscription_key = subscription_key
        self.tts = input("What would you like to convert to speech: ")
        self.timestr = time.strftime("%Y%m%d-%H%M")
        self.access_token = None
```

Der `subscription_key` ist Ihr individueller Schlüssel aus dem Azure-Portal. `tts` fordert den Benutzer auf, Text einzugeben, der in Sprache konvertiert wird. Diese Eingabe ist ein Zeichenfolgenliteral, sodass Zeichen nicht mit Escapezeichen versehen werden müssen. Abschließend wird über `timestr` die aktuelle Zeit abgerufen, die wir zum Benennen Ihrer Datei verwenden werden.

## <a name="get-an-access-token"></a>Abrufen eines Zugriffstokens

Alle Anforderungen an die Text-to-Speech-REST-API erfordern ein Zugriffstoken für die Authentifizierung. Um ein Zugriffstoken anzufordern, ist ein Austausch erforderlich. In diesem Beispiel wird Ihr Abonnementschlüssel für den Speech-Dienst unter Verwendung des Endpunkts `issueToken` gegen ein Zugriffstoken ausgetauscht.

In diesem Beispiel wird vorausgesetzt, dass Ihr Abonnement für den Speech-Dienst in der Region „USA, Westen“ enthalten ist. Wenn Sie eine andere Region verwenden, aktualisieren Sie den Wert für `fetch_token_url`. Eine vollständige Liste finden Sie unter [Regionen](https://docs.microsoft.com/azure/cognitive-services/speech-service/regions#rest-apis).

Kopieren Sie diesen Code in die `TextToSpeech`-Klasse:

```python
def get_token(self):
    fetch_token_url = "https://westus.api.cognitive.microsoft.com/sts/v1.0/issueToken"
    headers = {
        'Ocp-Apim-Subscription-Key': self.subscription_key
    }
    response = requests.post(fetch_token_url, headers=headers)
    self.access_token = str(response.text)
```

> [!NOTE]
> Weitere Informationen zur Authentifizierung finden Sie unter [Authentifizieren mit einem Authentifizierungstoken](https://docs.microsoft.com/azure/cognitive-services/authentication#authenticate-with-an-authentication-token).

## <a name="make-a-request-and-save-the-response"></a>Senden der Anforderung und Speichern der Antwort

An dieser Stelle werden Sie die Anfrage erstellen und die Sprachantwort speichern. Zunächst müssen Sie `base_url` und `path` festlegen. In diesem Beispiel wird davon ausgegangen, dass Sie den Endpunkt „USA, Westen“ verwenden. Wenn Ihre Ressource in einer anderen Region registriert ist, stellen Sie sicher, dass Sie die `base_url` aktualisieren. Weitere Informationen finden Sie unter [Sprachdienstregionen](https://docs.microsoft.com/azure/cognitive-services/speech-service/regions#text-to-speech).

Als Nächstes müssen Sie erforderliche Header für die Anforderung hinzufügen. Stellen Sie sicher, dass Sie `User-Agent` mit dem Namen Ihrer Ressourcengruppe aktualisieren (befindet sich im Azure-Portal), und `X-Microsoft-OutputFormat` auf Ihre bevorzugte Audioausgabe festlegen. Eine vollständige Liste der Ausgabeformate finden Sie unter [Audioausgaben](https://docs.microsoft.com/azure/cognitive-services/speech-service/rest-apis).

Erstellen Sie dann den Anforderungstext mithilfe der Speech Synthesis Markup Language (SSML). Dieses Beispiel definiert die Struktur und verwendet die zuvor von Ihnen erstellte `tts`-Eingabe.

>[!NOTE]
> In diesem Beispiel wird der Voicefont `Guy24KRUS` verwendet. Eine vollständige Liste der von Microsoft bereitgestellten Stimmen/Sprachen finden Sie unter [Sprachunterstützung](language-support.md).
> Wenn Sie daran interessiert sind, eine eigene Stimme mit Wiedererkennungswert für Ihre Marke zu erstellen, finden Sie unter [Erstellen von benutzerdefinierten Voicefonts](how-to-customize-voice-font.md) weitere Informationen.

Abschließend stellen Sie eine Anforderung an den Dienst. Wenn die Anforderung erfolgreich war und der Statuscode 200 zurückgegeben wird, wird die Sprachausgabe in eine mit Zeitstempel versehene Datei geschrieben.

Kopieren Sie diesen Code in die `TextToSpeech`-Klasse:

```python
def save_audio(self):
    base_url = 'https://westus.tts.speech.microsoft.com/'
    path = 'cognitiveservices/v1'
    constructed_url = base_url + path
    headers = {
        'Authorization': 'Bearer ' + self.access_token,
        'Content-Type': 'application/ssml+xml',
        'X-Microsoft-OutputFormat': 'riff-24khz-16bit-mono-pcm',
        'User-Agent': 'YOUR_RESOURCE_NAME'
    }
    xml_body = ElementTree.Element('speak', version='1.0')
    xml_body.set('{http://www.w3.org/XML/1998/namespace}lang', 'en-us')
    voice = ElementTree.SubElement(xml_body, 'voice')
    voice.set('{http://www.w3.org/XML/1998/namespace}lang', 'en-US')
    voice.set(
        'name', 'Microsoft Server Speech Text to Speech Voice (en-US, Guy24KRUS)')
    voice.text = self.tts
    body = ElementTree.tostring(xml_body)

    response = requests.post(constructed_url, headers=headers, data=body)
    if response.status_code == 200:
        with open('sample-' + self.timestr + '.wav', 'wb') as audio:
            audio.write(response.content)
            print("\nStatus code: " + str(response.status_code) +
                  "\nYour TTS is ready for playback.\n")
    else:
        print("\nStatus code: " + str(response.status_code) +
              "\nSomething went wrong. Check your subscription key and headers.\n")
```

## <a name="put-it-all-together"></a>Korrektes Zusammenfügen

Sie sind fast fertig. Der letzte Schritt besteht im Instanziieren Ihrer Klasse und dem Aufrufen Ihrer Funktionen.

```python
if __name__ == "__main__":
    subscription_key = "YOUR_KEY_HERE"
    app = TextToSpeech(subscription_key)
    app.get_token()
    app.save_audio()
```

## <a name="run-the-sample-app"></a>Ausführen der Beispiel-App

Fertig! Sie können Ihre Beispiel-App jetzt ausführen. Navigieren Sie über die Befehlszeile (oder Terminalsitzung) zu Ihrem Projektverzeichnis, und führen Sie Folgendes aus:

```console
python tts.py
```

Wenn Sie dazu aufgefordert werden, geben Sie den Text ein, den Sie in Sprache konvertieren möchten. Bei erfolgreicher Ausführung wird die Sprachdatei in Ihrem Projektordner gespeichert. Geben Sie die Sprachdatei mit Ihrem bevorzugten Medienplayer wieder.

## <a name="clean-up-resources"></a>Bereinigen von Ressourcen

Entfernen Sie unbedingt alle vertraulichen Informationen wie etwa Abonnementschlüssel aus dem Quellcode Ihrer Beispiel-App.

## <a name="next-steps"></a>Nächste Schritte

> [!div class="nextstepaction"]
> [Python-Beispiele auf GitHub](https://github.com/Azure-Samples/Cognitive-Speech-TTS/tree/master/Samples-Http/Python)

## <a name="see-also"></a>Weitere Informationen

* [Referenz zur Text-to-Speech-API](https://docs.microsoft.com/azure/cognitive-services/speech-service/rest-apis)
* [Konvertieren von Text in Sprache mit Python und dem Speech SDK](quickstarts/speech-to-text-from-microphone.md)
* [Erstellen benutzerdefinierter Voicefonts](how-to-customize-voice-font.md)
* [Aufzeichnen von Sprachbeispielen zum Erstellen einer benutzerdefinierten Stimme](record-custom-voice-samples.md)
