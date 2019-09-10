---
title: 'Schnellstart: Textanalyse-Clientbibliothek für Python | Microsoft-Dokumentation'
titleSuffix: Azure Cognitive Services
description: In dieser Schnellstartanleitung werden die ersten Schritte mit der Textanalyse-API von Azure Cognitive Services erläutert.
services: cognitive-services
author: ctufts
manager: assafi
ms.service: cognitive-services
ms.subservice: text-analytics
ms.topic: quickstart
ms.date: 08/28/2019
ms.author: aahi
ms.openlocfilehash: 0543bc639e60c65d9ab5a6cc810ddf6b7d10087a
ms.sourcegitcommit: d200cd7f4de113291fbd57e573ada042a393e545
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 08/29/2019
ms.locfileid: "70142730"
---
# <a name="quickstart-text-analytics-client-library-for-python"></a>Schnellstart: Textanalyse-Clientbibliothek für Python
<a name="HOLTop"></a>

Hier finden Sie Informationen zu den ersten Schritten mit der Textanalyse-Clientbibliothek für Python. Führen Sie die nachfolgenden Schritte zum Installieren des Pakets aus, und testen Sie den Beispielcode für grundlegende Aufgaben. 

Die Textanalyse-Clientbibliothek für Python kann für Folgendes verwendet werden:

* Stimmungsanalyse
* Spracherkennung
* Entitätserkennung
* Schlüsselwortextraktion


[Referenzdokumentation](https://docs.microsoft.com/python/api/overview/azure/cognitiveservices/textanalytics?view=azure-python) | [Quellcode der Bibliothek](https://github.com/Azure/azure-sdk-for-python/tree/master/sdk/cognitiveservices/azure-cognitiveservices-language-textanalytics) | [Paket (PiPy)](https://pypi.org/project/azure-cognitiveservices-language-textanalytics/) | [Beispiele](https://github.com/Azure-Samples/cognitive-services-python-sdk-samples/)


## <a name="prerequisites"></a>Voraussetzungen

* Azure-Abonnement – [Erstellen eines kostenlosen Kontos](https://azure.microsoft.com/free/)
* [Python 3.x](https://www.python.org/)

## <a name="setting-up"></a>Einrichten

### <a name="create-a-text-analytics-azure-resource"></a>Erstellen einer Azure-Ressource für die Textanalyse

Azure Cognitive Services werden von Azure-Ressourcen dargestellt, die Sie abonnieren. Erstellen Sie mithilfe des [Azure-Portals](../../cognitive-services-apis-create-account.md) oder der [Azure CLI](../../cognitive-services-apis-create-account-cli.md) auf Ihrem lokalen Computer eine Ressource für die Textanalyse. Weitere Funktionen:

* Rufen Sie einen [Testschlüssel](https://azure.microsoft.com/try/cognitive-services/#decision) ab, mit dem Sie sieben Tage lang kostenlos testen können. Nach der Registrierung steht dieser auf der [Azure-Website](https://azure.microsoft.com/try/cognitive-services/my-apis/) zur Verfügung.  
* Zeigen Sie Ihre Ressource im [Azure-Portal](https://portal.azure.com/) an.

Nachdem Sie einen Schlüssel für Ihr Testabonnement bzw. Ihre Ressource erhalten haben, [erstellen Sie eine Umgebungsvariable](https://docs.microsoft.com/azure/cognitive-services/cognitive-services-apis-create-account#configure-an-environment-variable-for-authentication) für den Schlüssel (`TEXT_ANALYTICS_SUBSCRIPTION_KEY`).

### <a name="install-the-client-library"></a>Installieren der Clientbibliothek

Nach der Installation von Python, können Sie die Clientbibliothek mit Folgendem installieren:

```console
pip install --upgrade azure-cognitiveservices-language-textanalytics
```

### <a name="create-a-new-python-application"></a>Erstellen einer neuen Python-Anwendung

Erstellen Sie eine neue Python-Anwendung in Ihrem bevorzugten Editor oder Ihrer bevorzugten IDE. Importieren Sie dann die folgenden Bibliotheken:

```python
from azure.cognitiveservices.language.textanalytics import TextAnalyticsClient
from msrest.authentication import CognitiveServicesCredentials
```

Erstellen Sie Variablen für den Azure-Endpunkt und den Abonnementschlüssel Ihrer Ressource. Rufen Sie diese Werte aus den Umgebungsvariablen „TEXT_ANALYTICS_SUBSCRIPTION_KEY“ und „TEXT_ANALYTICS_ENDPOINT“ ab. Wenn Sie diese Umgebungsvariablen erstellt haben, nachdem Sie mit der Bearbeitung der Anwendung begonnen haben, muss der Editor, die IDE oder die Shell, den bzw. die Sie für den Zugriff auf die Variablen verwenden, geschlossen und erneut geöffnet werden.

[!INCLUDE [text-analytics-find-resource-information](../includes/find-azure-resource-info.md)]

```python
import os

key_var_name = 'TEXT_ANALYTICS_SUBSCRIPTION_KEY'
if not key_var_name in os.environ:
    raise Exception('Please set/export the environment variable: {}'.format(key_var_name))
subscription_key = os.environ[key_var_name]

endpoint_var_name = 'TEXT_ANALYTICS_ENDPOINT'
if not endpoint_var_name in os.environ:
    raise Exception('Please set/export the environment variable: {}'.format(endpoint_var_name))
endpoint = os.environ[endpoint_var_name]

credentials = CognitiveServicesCredentials(subscription_key)
```

## <a name="object-model"></a>Objektmodell

Der Textanalyse-Client ist ein [TextAnalyticsClient](https://docs.microsoft.com/python/api/azure-cognitiveservices-language-textanalytics/azure.cognitiveservices.language.textanalytics.textanalyticsclient?view=azure-python)-Objekt, das sich mit Ihrem Schlüssel bei Azure authentifiziert. Der Client verfügt über verschiedene Methoden zum Analysieren von Text als einzelne Zeichenfolge oder als Batch. 

Text wird als `documents`-Liste an die API gesendet. Hierbei handelt es sich um `dictionary`-Objekte, die je nach verwendeter Methode eine Kombination aus den Attributen `id`, `text` und `language` enthalten. Das `text`-Attribut speichert den zu analysierenden Text in der Ausgangssprache (`language`), und `id` kann ein beliebiger Wert sein. 

Das Antwortobjekt ist eine Liste mit den Analyseinformationen für jedes Dokument. 

## <a name="code-examples"></a>Codebeispiele

Mit den Codeausschnitten wird veranschaulicht, wie die folgenden Vorgänge mit der Textanalyse-Clientbibliothek für Python durchgeführt werden:

* [Authentifizieren des Clients](#authenticate-the-client)
* [Standpunktanalyse](#sentiment-analysis)
* [Sprachenerkennung](#language-detection)
* [Entitätserkennung](#entity-recognition)
* [Schlüsselbegriffserkennung](#key-phrase-extraction)

## <a name="authenticate-the-client"></a>Authentifizieren des Clients

Erstellen Sie ein [TextAnalyticsClient](https://docs.microsoft.com/python/api/azure-cognitiveservices-language-textanalytics/azure.cognitiveservices.language.textanalytics.textanalyticsclient?view=azure-python)-Objekt mit `credentials` und `text_analytics_url` als Parameter. Verwenden Sie die richtige Azure-Region für Ihr Textanalyse-Abonnement (z. B. `westcentralus`).

```python
text_analytics = TextAnalyticsClient(endpoint=text_analytics_url, credentials=credentials)
```

## <a name="sentiment-analysis"></a>Stimmungsanalyse

Rufen Sie die Funktion [sentiment()](https://docs.microsoft.com/python/api/azure-cognitiveservices-language-textanalytics/azure.cognitiveservices.language.textanalytics.textanalyticsclient?view=azure-python#sentiment-show-stats-none--documents-none--custom-headers-none--raw-false----operation-config-) auf, um das Ergebnis abzurufen. Durchlaufen Sie dann die Ergebnisse, und drucken Sie die ID und den Stimmungswert jedes Dokuments. Eine gegen 0 tendierende Punktzahl deutet auf eine negative Stimmung hin, eine gegen 1 tendierende Punktzahl auf eine positive.

```python
documents = [
    {
        "id": "1",
        "language": "en",
        "text": "I had the best day of my life."
    }
]
response = text_analytics.sentiment(documents=documents)
for document in response.documents:
    print("Document Id: ", document.id, ", Sentiment Score: ",
          "{:.2f}".format(document.score))
```

### <a name="output"></a>Output

```console
Document Id:  1 , Sentiment Score:  0.87
```

## <a name="language-detection"></a>Spracherkennung

Rufen Sie mit dem zuvor erstellten Client die Funktion [detect_language()](https://docs.microsoft.com/python/api/azure-cognitiveservices-language-textanalytics/azure.cognitiveservices.language.textanalytics.textanalyticsclient?view=azure-python#detect-language-show-stats-none--documents-none--custom-headers-none--raw-false----operation-config-) auf, um das Ergebnis abzurufen. Durchlaufen Sie dann die Ergebnisse, und drucken Sie die ID und die erste zurückgegebene Sprache.

```python
documents = [
    {
        'id': '1',
        'text': 'This is a document written in English.'
    }
]
response = text_analytics.detect_language(documents=documents)
for document in response.documents:
    print("Document Id: ", document.id, ", Language: ",
          document.detected_languages[0].name)
```

### <a name="output"></a>Output

```console
Document Id:  1 , Language:  English
```

## <a name="entity-recognition"></a>Entitätserkennung

Rufen Sie mit dem zuvor erstellten Client die Funktion [entities()](https://docs.microsoft.com/python/api/azure-cognitiveservices-language-textanalytics/azure.cognitiveservices.language.textanalytics.textanalyticsclient?view=azure-python#entities-show-stats-none--documents-none--custom-headers-none--raw-false----operation-config-) auf, um das Ergebnis abzurufen. Durchlaufen Sie dann die Ergebnisse, und geben Sie für jedes Dokument die ID und die enthaltenen Entitäten aus.

```python
documents = [
    {
        "id": "1",
        "language": "en",
        "text": "Microsoft was founded by Bill Gates and Paul Allen on April 4, 1975, to develop and sell BASIC interpreters for the Altair 8800."
    }
]
response = text_analytics.entities(documents=documents)

for document in response.documents:
    print("Document Id: ", document.id)
    print("\tKey Entities:")
    for entity in document.entities:
        print("\t\t", "NAME: ", entity.name, "\tType: ",
              entity.type, "\tSub-type: ", entity.sub_type)
        for match in entity.matches:
            print("\t\t\tOffset: ", match.offset, "\tLength: ", match.length, "\tScore: ",
                  "{:.2f}".format(match.entity_type_score))
```

### <a name="output"></a>Output

```console
Document Id:  1
    Key Entities:
         NAME:  Microsoft   Type:  Organization     Sub-type:  None
            Offset:  0  Length:  9  Score:  1.00
         NAME:  Bill Gates  Type:  Person   Sub-type:  None
            Offset:  25     Length:  10     Score:  1.00
         NAME:  Paul Allen  Type:  Person   Sub-type:  None
            Offset:  40     Length:  10     Score:  1.00
         NAME:  April 4     Type:  Other    Sub-type:  None
            Offset:  54     Length:  7  Score:  0.80
         NAME:  April 4, 1975   Type:  DateTime     Sub-type:  Date
            Offset:  54     Length:  13     Score:  0.80
         NAME:  BASIC   Type:  Other    Sub-type:  None
            Offset:  89     Length:  5  Score:  0.80
         NAME:  Altair 8800     Type:  Other    Sub-type:  None
            Offset:  116    Length:  11     Score:  0.80
```

## <a name="key-phrase-extraction"></a>Schlüsselwortextraktion

Rufen Sie mit dem zuvor erstellten Client die Funktion [key_phrases()](https://docs.microsoft.com/python/api/azure-cognitiveservices-language-textanalytics/azure.cognitiveservices.language.textanalytics.textanalyticsclient?view=azure-python#key-phrases-show-stats-none--documents-none--custom-headers-none--raw-false----operation-config-) auf, um das Ergebnis abzurufen. Durchlaufen Sie dann die Ergebnisse, und geben Sie für jedes Dokument die ID und die enthaltenen Schlüsselausdrücke aus.

```python
documents = [
    {
        "id": "1",
        "language": "en",
        "text": "My cat might need to see a veterinarian."
    }
]
response = text_analytics.key_phrases(documents=documents)

for document in response.documents:
    print("Document Id: ", document.id)
    print("\tKey Phrases:")
    for phrase in document.key_phrases:
        print("\t\t", phrase)
```


### <a name="output"></a>Output

```console
Document Id:  3
    Phrases:
         cat
         veterinarian
```

## <a name="clean-up-resources"></a>Bereinigen von Ressourcen

Wenn Sie ein Cognitive Services-Abonnement bereinigen und entfernen möchten, können Sie die Ressource oder die Ressourcengruppe löschen. Wenn Sie die Ressourcengruppe löschen, werden auch alle anderen Ressourcen gelöscht, die der Ressourcengruppe zugeordnet sind.

* [Portal](../../cognitive-services-apis-create-account.md#clean-up-resources)
* [Azure-Befehlszeilenschnittstelle](../../cognitive-services-apis-create-account-cli.md#clean-up-resources)

## <a name="next-steps"></a>Nächste Schritte

> [!div class="nextstepaction"]
> [Textanalyse mit Power BI](../tutorials/tutorial-power-bi-key-phrases.md)


* [Übersicht über die Textanalyse](../overview.md)
* [Standpunktanalyse](../how-tos/text-analytics-how-to-sentiment-analysis.md)
* [Entitätserkennung](../how-tos/text-analytics-how-to-entity-linking.md)
* [Sprache erkennen](../how-tos/text-analytics-how-to-keyword-extraction.md)
* [Spracherkennung](../how-tos/text-analytics-how-to-language-detection.md)
* Den Quellcode für dieses Beispiel finden Sie auf [GitHub](https://github.com/Azure-Samples/cognitive-services-python-sdk-samples/blob/master/samples/language/text_analytics_samples.py).
