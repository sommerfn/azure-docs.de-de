---
title: 'Schnellstart: Auflisten von Stimmen für Text-to-Speech, Python – Speech-Dienst'
titleSuffix: Azure Cognitive Services
description: In dieser Schnellstartanleitung erfahren Sie, wie Sie mit Python die vollständige Liste der Standard- und neuronalen Stimmen für eine Region bzw. einen Endpunkt erhalten. Die Liste wird als JSON zurückgegeben, und die Verfügbarkeit der Stimmen variiert je nach Region.
services: cognitive-services
author: erhopf
manager: nitinme
ms.service: cognitive-services
ms.subservice: speech-service
ms.topic: quickstart
ms.date: 07/05/2019
ms.author: erhopf
ms.openlocfilehash: ac96c3ce3924b8b2fe834e2b350e95ce23c52e1f
ms.sourcegitcommit: 7c4de3e22b8e9d71c579f31cbfcea9f22d43721a
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 07/26/2019
ms.locfileid: "68559348"
---
# <a name="quickstart-get-the-list-of-text-to-speech-voices-using-python"></a>Schnellstart: Abrufen der Liste der Stimmen für Text-to-Speech mit Python

In dieser Schnellstartanleitung erfahren Sie, wie Sie mit Python die vollständige Liste der Standard- und neuronalen Stimmen für eine Region bzw. einen Endpunkt erhalten. Die Liste wird als JSON zurückgegeben, und die Verfügbarkeit der Stimmen variiert je nach Region. Eine Liste der unterstützten Regionen finden Sie unter [Regionen](regions.md).

Für diese Schnellstartanleitung wird ein [Azure Cognitive Services-Konto](https://docs.microsoft.com/azure/cognitive-services/cognitive-services-apis-create-account) mit einer Ressource für den Speech-Dienst benötigt. Wenn Sie über kein Konto verfügen, können Sie über die [kostenlose Testversion](get-started.md) einen Abonnementschlüssel abrufen.

## <a name="prerequisites"></a>Voraussetzungen

Für diese Schnellstartanleitung ist Folgendes erforderlich:

* Python 2.7.x oder 3.x
* [Visual Studio](https://visualstudio.microsoft.com/downloads/), [Visual Studio Code](https://code.visualstudio.com/download) oder ein anderer Editor
* Ein Azure-Abonnementschlüssel für die Speech-Dienste

## <a name="create-a-project-and-import-required-modules"></a>Erstellen eines Projekts und Importieren der erforderlichen Module

Erstellen Sie in Ihrer bevorzugten IDE oder Ihrem bevorzugten Editor ein neues Python-Projekt. Kopieren Sie anschließend den folgenden Codeausschnitt in Ihr Projekt in eine Datei namens `get-voices.py`.

```python
import requests
```

> [!NOTE]
> Wenn Sie diese Module bisher nicht verwendet haben, müssen Sie sie vor der Ausführung Ihres Programms installieren. Führen Sie zum Installieren dieser Pakete `pip install requests` aus.

Für HTTP-Anforderungen an den Text-to-Speech-Dienst wird Requests verwendet.

## <a name="set-the-subscription-key-and-create-a-prompt-for-tts"></a>Festlegen des Abonnementschlüssels und Erstellen einer Eingabeaufforderung für TTS

In den nächsten Abschnitten werden Sie Methoden zur Handhabung der Autorisierung erstellen, die Text-to-Speech-API aufrufen und die Antwort überprüfen. Erstellen Sie als Erstes eine Klasse. Hier werden wir unsere Methoden für den Tokenaustausch und den Aufruf der Text-to-Speech-API einfügen.

```python
class GetVoices(object):
    def __init__(self, subscription_key):
        self.subscription_key = subscription_key
        self.access_token = None
```

Der `subscription_key` ist Ihr individueller Schlüssel aus dem Azure-Portal.

## <a name="get-an-access-token"></a>Abrufen eines Zugriffstokens

Dieser Endpunkt erfordert ein Zugriffstoken für die Authentifizierung. Um ein Zugriffstoken anzufordern, ist ein Austausch erforderlich. In diesem Beispiel wird Ihr Abonnementschlüssel für den Speech-Dienst unter Verwendung des Endpunkts `issueToken` gegen ein Zugriffstoken ausgetauscht.

In diesem Beispiel wird vorausgesetzt, dass Ihr Abonnement für den Speech-Dienst in der Region „USA, Westen“ enthalten ist. Wenn Sie eine andere Region verwenden, aktualisieren Sie den Wert für `fetch_token_url`. Eine vollständige Liste finden Sie unter [Regionen](https://docs.microsoft.com/azure/cognitive-services/speech-service/regions#rest-apis).

Kopieren Sie diesen Code in die `GetVoices`-Klasse:

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

An dieser Stelle werden Sie die Anforderung erstellen und die Liste der zurückgegebenen Stimmen speichern. Zunächst müssen Sie `base_url` und `path` festlegen. In diesem Beispiel wird davon ausgegangen, dass Sie den Endpunkt „USA, Westen“ verwenden. Wenn Ihre Ressource in einer anderen Region registriert ist, stellen Sie sicher, dass Sie die `base_url` aktualisieren. Weitere Informationen finden Sie unter [Speech-Dienstregionen](https://docs.microsoft.com/azure/cognitive-services/speech-service/regions#text-to-speech).

Als Nächstes fügen Sie erforderliche Header für die Anforderung hinzu. Abschließend stellen Sie eine Anforderung an den Dienst. Wenn die Anforderung erfolgreich war und der Statuscode 200 zurückgegeben wird, wird die Antwort in eine Datei geschrieben.

Kopieren Sie diesen Code in die `GetVoices`-Klasse:

```python
def get_voices(self):
    base_url = 'https://westus.tts.speech.microsoft.com'
    path = '/cognitiveservices/voices/list'
    get_voices_url = base_url + path
    headers = {
        'Authorization': 'Bearer ' + self.access_token
    }
    response = requests.get(get_voices_url, headers=headers)
    if response.status_code == 200:
        with open('voices.json', 'wb') as voices:
            voices.write(response.content)
            print("\nStatus code: " + str(response.status_code) +
                  "\nvoices.json is ready to view.\n")
    else:
        print("\nStatus code: " + str(
            response.status_code) + "\nSomething went wrong. Check your subscription key and headers.\n")
```

## <a name="put-it-all-together"></a>Korrektes Zusammenfügen

Sie sind fast fertig. Der letzte Schritt besteht im Instanziieren Ihrer Klasse und dem Aufrufen Ihrer Funktionen.

```python
if __name__ == "__main__":
    subscription_key = "YOUR_KEY_HERE"
    app = GetVoices(subscription_key)
    app.get_token()
    app.get_voices()
```

## <a name="run-the-sample-app"></a>Ausführen der Beispiel-App

Sie können das Beispiel jetzt starten. Navigieren Sie über die Befehlszeile (oder Terminalsitzung) zu Ihrem Projektverzeichnis, und führen Sie Folgendes aus:

```console
python get-voices.py
```

## <a name="clean-up-resources"></a>Bereinigen von Ressourcen

Entfernen Sie unbedingt alle vertraulichen Informationen wie etwa Abonnementschlüssel aus dem Quellcode Ihrer Beispiel-App.

## <a name="next-steps"></a>Nächste Schritte

> [!div class="nextstepaction"]
> [Python-Beispiele auf GitHub](https://github.com/Azure-Samples/Cognitive-Speech-TTS/tree/master/Samples-Http/Python)

## <a name="see-also"></a>Weitere Informationen

* [Referenz zur Text-to-Speech-API](https://docs.microsoft.com/azure/cognitive-services/speech-service/rest-apis)
* [Erstellen benutzerdefinierter Voicefonts](how-to-customize-voice-font.md)
* [Aufzeichnen von Sprachbeispielen zum Erstellen einer benutzerdefinierten Stimme](record-custom-voice-samples.md)
