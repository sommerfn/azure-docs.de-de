---
title: 'Schnellstart: Asynchrone Synthese für lange Audioinhalte (Vorschau) – Spracherkennungsdienst'
titleSuffix: Azure Cognitive Services
description: Verwenden Sie die API für lange Audioinhalte, um Text asynchron in Sprache zu konvertieren und die Audioausgabe von einem vom Dienst bereitgestellten URI abzurufen. Diese REST-API eignet sich ideal für Inhaltsanbieter, die Textdateien mit mehr als 10.000 Zeichen oder 50 Absätzen in synthetisierte Sprache konvertieren müssen.
services: cognitive-services
author: erhopf
manager: nitinme
ms.service: cognitive-services
ms.subservice: speech-service
ms.topic: conceptual
ms.date: 11/04/2019
ms.author: erhopf
ms.openlocfilehash: 2c1e6ef84a472ccec5116b12e18ad80c92b68960
ms.sourcegitcommit: 609d4bdb0467fd0af40e14a86eb40b9d03669ea1
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 11/06/2019
ms.locfileid: "73681880"
---
# <a name="quickstart-asynchronous-synthesis-for-long-form-audio-in-python-preview"></a>Schnellstart: Asynchrone Synthese für lange Audioinhalte in Python (Vorschau)

In dieser Schnellstartanleitung verwenden Sie die API für lange Audioinhalte, um Text asynchron in Sprache zu konvertieren und die Audioausgabe von einem vom Dienst bereitgestellten URI abzurufen. Diese REST-API eignet sich ideal für Inhaltsanbieter, die Textdateien mit mehr als 10.000 Zeichen oder 50 Absätzen in synthetisierte Sprache konvertieren müssen. Weitere Informationen finden Sie unter [API für lange Audioinhalte](../../long-audio-api.md).

## <a name="prerequisites"></a>Voraussetzungen

Für diese Schnellstartanleitung ist Folgendes erforderlich:

* Python 2.7.x oder 3.x
* [Visual Studio](https://visualstudio.microsoft.com/downloads/), [Visual Studio Code](https://code.visualstudio.com/download) oder ein anderer Editor
* Ein Azure-Abonnement und ein Abonnementschlüssel für den Spracherkennungsdienst. [Erstellen Sie ein Azure-Konto](https://docs.microsoft.com/azure/cognitive-services/speech-service/get-started#new-azure-account) und [eine Speech-Ressource](https://docs.microsoft.com/azure/cognitive-services/speech-service/get-started#create-a-speech-resource-in-azure), um den Schlüssel zu erhalten. Stellen Sie beim Erstellen der Speech-Ressource sicher, dass als Tarif **S0** und als Standort eine [unterstützte Region](../../regions.md#standard-and-neural-voices) festgelegt ist.

## <a name="create-a-project-and-import-required-modules"></a>Erstellen eines Projekts und Importieren der erforderlichen Module

Erstellen Sie in Ihrer bevorzugten IDE oder Ihrem bevorzugten Editor ein neues Python-Projekt. Kopieren Sie anschließend den folgenden Codeausschnitt in eine Datei namens `voice_synthesis_client.py`.

```python
import argparse
import json
import ntpath
import urllib3
import requests
import time
from json import dumps, loads, JSONEncoder, JSONDecoder
import pickle

urllib3.disable_warnings(urllib3.exceptions.InsecureRequestWarning)
```

> [!NOTE]
> Wenn Sie diese Module bisher nicht verwendet haben, müssen Sie sie vor der Ausführung Ihres Programms installieren. Führen Sie zum Installieren dieser Pakete `pip install requests urllib3` aus.

Diese Module werden verwendet, um Argumente zu analysieren, die HTTP-Anforderung zu erstellen und die REST-API für lange Audioinhalte (Sprachsynthese) aufzurufen.

## <a name="get-a-list-of-supported-voices"></a>Abrufen einer Liste der unterstützten Stimmen

Dieser Code ruft eine Liste der verfügbaren Stimmen ab, mit denen Sie Text in Sprache konvertieren können. Fügen Sie diesen Code `voice_synthesis_client.py` hinzu:

```python
parser = argparse.ArgumentParser(description='Cris client tool to submit voice synthesis requests.')
parser.add_argument('--voices', action="store_true", default=False, help='print voice list')
parser.add_argument('-key', action="store", dest="key", required=True, help='the cris subscription key, like ff1eb62d06d34767bda0207acb1da7d7 ')
parser.add_argument('-region', action="store", dest="region", required=True, help='the region information, could be centralindia, canadacentral or uksouth')
args = parser.parse_args()
baseAddress = 'https://%s.cris.ai/api/texttospeech/v3.0-beta1/' % args.region

def getVoices():
    response=requests.get(baseAddress+"voicesynthesis/voices", headers={"Ocp-Apim-Subscription-Key":args.key}, verify=False)
    voices = json.loads(response.text)
    return voices

if args.voices:
    voices = getVoices()
    print("There are %d voices available:" % len(voices))
    for voice in voices:
        print ("Name: %s, Description: %s, Id: %s, Locale: %s, Gender: %s, PublicVoice: %s, Created: %s" % (voice['name'], voice['description'], voice['id'], voice['locale'], voice['gender'], voice['isPublicVoice'], voice['created']))
```

### <a name="test-your-code"></a>Testen Ihres Codes

Lassen Sie uns testen, was Sie bisher geleistet haben. Führen Sie diesen Befehl aus, und ersetzen Sie `<your_key>` durch Ihren Speech-Abonnementschlüssel und `<region>` durch die Region, in der Ihre Speech-Ressource erstellt wurde (Beispiel: `eastus` oder `westus`). Diese Informationen sind auf der Registerkarte **Übersicht** für Ihre Ressource im [Azure-Portal](https://aka.ms/azureportal) verfügbar.

```console
python voice_synthesis_client.py --voices -key <your_key> -region <Region>
```

Sie sollten eine Ausgabe erhalten, die wie folgt aussieht:

```console
There are xx voices available:

Name: Microsoft Server Speech Text to Speech Voice (en-US, xxx), Description: xxx , Id: xxx, Locale: en-US, Gender: Male, PublicVoice: xxx, Created: 2019-07-22T09:38:14Z
Name: Microsoft Server Speech Text to Speech Voice (zh-CN, xxx), Description: xxx , Id: xxx, Locale: zh-CN, Gender: Female, PublicVoice: xxx, Created: 2019-08-26T04:55:39Z
```

## <a name="convert-text-to-speech"></a>Konvertieren von Text in Sprache

Der nächste Schritt ist die Vorbereitung einer Eingabetextdatei. Es kann entweder Nur-Text oder SSML sein, muss aber mehr als 10.000 Zeichen oder 50 Absätze umfassen. Eine vollständige Liste der Anforderungen finden Sie unter [API für lange Audioinhalte](../../long-audio-api.md).

Nachdem Sie die Textdatei vorbereitet haben. Der nächste Schritt ist das Hinzufügen von Code für die Sprachsynthese zu Ihrem Projekt. Fügen Sie diesen Code `voice_synthesis_client.py` hinzu:

> [!NOTE]
> Standardmäßig ist die Audioausgabe auf riff-16khz-16bit-16bit-mono-pcm eingestellt. Weitere Informationen zu unterstützten Audioausgaben finden Sie unter [API für lange Audioinhalte](../../long-audio-api.md#audio-output-formats).

```python
parser.add_argument('--submit', action="store_true", default=False, help='submit a synthesis request')
parser.add_argument('--concatenateResult', action="store_true", default=False, help='If concatenate result in a single wave file')
parser.add_argument('-file', action="store", dest="file", help='the input text script file path')
parser.add_argument('-voiceId', action="store", nargs='+', dest="voiceId", help='the id of the voice which used to synthesis')
parser.add_argument('-locale', action="store", dest="locale", help='the locale information like zh-CN/en-US')
parser.add_argument('-format', action="store", dest="format", default='riff-16khz-16bit-mono-pcm', help='the output audio format')

def submitSynthesis():
    modelList = args.voiceId
    data={'name': 'simple test', 'description': 'desc...', 'models': json.dumps(modelList), 'locale': args.locale, 'outputformat': args.format}
    if args.concatenateResult:
        properties={'ConcatenateResult': 'true'}
        data['properties'] = json.dumps(properties)
    if args.file is not None:
        scriptfilename=ntpath.basename(args.file)
        files = {'script': (scriptfilename, open(args.file, 'rb'), 'text/plain')}
    response = requests.post(baseAddress+"voicesynthesis", data, headers={"Ocp-Apim-Subscription-Key":args.key}, files=files, verify=False)
    if response.status_code == 202:
        location = response.headers['Operation-Location']
        id = location.split("/")[-1]
        print("Submit synthesis request successful")
        return id
    else:
        print("Submit synthesis request failed")
        print("response.status_code: %d" % response.status_code)
        print("response.text: %s" % response.text)
        return 0

def getSubmittedSynthesis(id):
    response=requests.get(baseAddress+"voicesynthesis/"+id, headers={"Ocp-Apim-Subscription-Key":args.key}, verify=False)
    synthesis = json.loads(response.text)
    return synthesis

if args.submit:
    id = submitSynthesis()
    if (id == 0):
        exit(1)

    while(1):
        print("\r\nChecking status")
        synthesis=getSubmittedSynthesis(id)
        if synthesis['status'] == "Succeeded":
            r = requests.get(synthesis['resultsUrl'])
            filename=id + ".zip"
            with open(filename, 'wb') as f:  
                f.write(r.content)
                print("Succeeded... Result file downloaded : " + filename)
            break
        elif synthesis['status'] == "Failed":
            print("Failed...")
            break
        elif synthesis['status'] == "Running":
            print("Running...")
        elif synthesis['status'] == "NotStarted":
            print("NotStarted...")
        time.sleep(10)
```

### <a name="test-your-code"></a>Testen Ihres Codes

Lassen Sie uns versuchen, eine Anforderung zur Textsynthese zu stellen, bei der Ihre Eingabedatei als Quelle verwendet wird. Sie müssen einige Punkte in der folgenden Anforderung aktualisieren:

* Ersetzen Sie `<your_key>` durch Ihren Abonnementschlüssel für den Spracherkennungsdienst. Diese Informationen sind auf der Registerkarte **Übersicht** für Ihre Ressource im [Azure-Portal](https://aka.ms/azureportal) verfügbar.
* Ersetzen Sie `<region>` durch die Region, in der Ihre Speech-Ressource erstellt wurde (Beispiel: `eastus` oder `westus`). Diese Informationen sind auf der Registerkarte **Übersicht** für Ihre Ressource im [Azure-Portal](https://aka.ms/azureportal) verfügbar.
* Ersetzen Sie `<input>` durch den Pfad zu der Textdatei, die Sie von Text in Sprache konvertieren möchten.
* Ersetzen Sie `<locale>` durch das gewünschte Ausgabegebietsschema. Weitere Informationen finden Sie unter [Sprachunterstützung](../../language-support.md#neural-voices).
* Ersetzen Sie `<voice_guid>` durch die gewünschte Stimme für die Audioausgabe. Verwenden Sie eine der Stimmen, die von [Abrufen einer Liste der unterstützten Stimmen](#get-a-list-of-supported-voices) zurückgegeben wurden, oder verwenden Sie die Liste der neuronalen Stimmen, die in [Sprachunterstützung](../../language-support.md#neural-voices) enthalten ist.

Konvertieren Sie mit dem folgenden Befehl den Text in Sprache:

```console
python voice_synthesis_client.py --submit -key <your_key> -region <Region> -file <input> -locale <locale> -voiceId <voice_guid>
```

> [!NOTE]
> „concatenateResult“ ist ein optionaler Parameter. Wenn dieser Parameter nicht angegeben wird, erfolgt die Ausgabe in mehreren WAV-Dateien (eine für jede Zeile).

Sie sollten eine Ausgabe erhalten, die wie folgt aussieht:

```console
Submit synthesis request successful

Checking status
NotStarted...

Checking status
Running...

Checking status
Running...

Checking status
Succeeded... Result file downloaded : xxxx.zip
```

Das bereitgestellte Ergebnis enthält den Eingabetext und die vom Dienst erzeugten Audioausgabedateien. Diese werden als ZIP-Datei heruntergeladen.

## <a name="remove-previous-requests"></a>Entfernen vorheriger Anforderungen

Für jedes Abonnement gilt ein Grenzwert von 2.000 Anforderungen. Daher wird es zeitweise erforderlich sein, bereits übermittelte Anforderungen zu entfernen, bevor Sie neue stellen können. Wenn Sie vorhandene Anforderungen nicht entfernen, erhalten Sie eine Fehlermeldung, wenn Sie 2.000 überschreiten.

Fügen Sie diesen Code `voice_synthesis_client.py` hinzu:

```python
parser.add_argument('--syntheses', action="store_true", default=False, help='print synthesis list')
parser.add_argument('--delete', action="store_true", default=False, help='delete a synthesis request')
parser.add_argument('-synthesisId', action="store", nargs='+', dest="synthesisId", help='the id of the voice synthesis which need to be deleted')

def getSubmittedSyntheses():
    response=requests.get(baseAddress+"voicesynthesis", headers={"Ocp-Apim-Subscription-Key":args.key}, verify=False)
    syntheses = json.loads(response.text)
    return syntheses

def deleteSynthesis(ids):
    for id in ids:
        print("delete voice synthesis %s " % id)
        response = requests.delete(baseAddress+"voicesynthesis/"+id, headers={"Ocp-Apim-Subscription-Key":args.key}, verify=False)
        if (response.status_code == 204):
            print("delete successful")
        else:
            print("delete failed, response.status_code: %d, response.text: %s " % (response.status_code, response.text))

if args.syntheses:
    synthese = getSubmittedSyntheses()
    print("There are %d synthesis requests submitted:" % len(synthese))
    for synthesis in synthese:
        print ("ID : %s , Name : %s, Status : %s " % (synthesis['id'], synthesis['name'], synthesis['status']))

if args.delete:
    deleteSynthesis(args.synthesisId)
```

### <a name="test-your-code"></a>Testen Ihres Codes

Führen Sie diesen Befehl aus, und ersetzen Sie `<your_key>` durch Ihren Speech-Abonnementschlüssel und `<region>` durch die Region, in der Ihre Speech-Ressource erstellt wurde (Beispiel: `eastus` oder `westus`). Diese Informationen sind auf der Registerkarte **Übersicht** für Ihre Ressource im [Azure-Portal](https://aka.ms/azureportal) verfügbar.

```console
python voice_synthesis_client.py – syntheses -key <your_key> -region <Region>
```

Dadurch wird eine Liste der von Ihnen angeforderten Synthesen angezeigt. Sie sollten eine Ausgabe erhalten, die wie folgt aussieht:

```console
There are <number> synthesis requests submitted:
ID : xxx , Name : xxx, Status : Succeeded
ID : xxx , Name : xxx, Status : Running
ID : xxx , Name : xxx : Succeeded
```

Lassen Sie uns jetzt einige dieser Werte verwenden, um zuvor übermittelte Anforderungen zu entfernen bzw. zu löschen. Führen Sie diesen Befehl aus, und ersetzen Sie `<your_key>` durch Ihren Speech-Abonnementschlüssel und `<region>` durch die Region, in der Ihre Speech-Ressource erstellt wurde (Beispiel: `eastus` oder `westus`). Diese Informationen sind auf der Registerkarte **Übersicht** für Ihre Ressource im [Azure-Portal](https://aka.ms/azureportal) verfügbar. Die `<synthesis_id>` sollte einer der Werte sein, die in der vorherigen Anforderung zurückgegeben wurden.

> [!NOTE]
> Anforderungen mit dem Status „Wird ausgeführt“/„Wartend“ können nicht entfernt oder gelöscht werden.

```console
python voice_synthesis_client.py – delete -key <your_key> -region <Region> -synthesisId <synthesis_id>
```

Sie sollten eine Ausgabe erhalten, die wie folgt aussieht:

```console
delete voice synthesis xxx
delete successful
```

## <a name="get-the-full-client"></a>Abrufen des vollständigen Clients

Das vollständige `voice_synthesis_client.py` kann von [GitHub](https://github.com/Azure-Samples/Cognitive-Speech-TTS/blob/master/CustomVoice-API-Samples/Python/voiceclient.py) heruntergeladen werden.

## <a name="next-steps"></a>Nächste Schritte

> [!div class="nextstepaction"]
> [Weitere Informationen zur API für lange Audioinhalte](../../long-audio-api.md)
