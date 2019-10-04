---
title: 'Schnellstart: Durchführen einer Neuigkeitensuche mit Python und der REST-API der Bing-News-Suche'
titleSuffix: Azure Cognitive Services
description: Verwenden Sie diese Schnellstartanleitung zum Senden einer Anforderung an die REST-API der Bing-News-Suche mit Python, um eine JSON-Antwort zu erhalten.
services: cognitive-services
author: aahill
manager: nitinme
ms.service: cognitive-services
ms.subservice: bing-news-search
ms.topic: quickstart
ms.date: 6/18/2019
ms.author: aahi
ms.custom: seodec2018
ms.openlocfilehash: 2fc67cbfedccd69ff9063b0575ae51197eceb09d
ms.sourcegitcommit: 198c3a585dd2d6f6809a1a25b9a732c0ad4a704f
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 07/23/2019
ms.locfileid: "68423698"
---
# <a name="quickstart-perform-a-news-search-using-python-and-the-bing-news-search-rest-api"></a>Schnellstart: Durchführen einer Neuigkeitensuche mit Python und der REST-API der Bing-News-Suche

Verwenden Sie diese Schnellstartanleitung, um die Bing-News-Suche-API zum ersten Mal aufzurufen und eine JSON-Antwort zu erhalten. Diese einfache JavaScript-Anwendung sendet eine Suchabfrage an die API und verarbeitet die Ergebnisse. Diese Anwendung ist zwar in Python geschrieben, an sich ist die API aber ein RESTful-Webdienst, der mit den meisten Programmiersprachen kompatibel ist.

Sie können dieses Codebeispiel als Jupyter Notebook in [MyBinder](https://mybinder.org) ausführen, indem Sie auf das Binder-Badge klicken: 

[![Binder](https://mybinder.org/badge.svg)](https://mybinder.org/v2/gh/Microsoft/cognitive-services-notebooks/master?filepath=BingNewsSearchAPI.ipynb)

Den Quellcode des Beispiels finden Sie auch auf [GitHub](https://github.com/Azure-Samples/cognitive-services-REST-api-samples/blob/master/python/Search/BingNewsSearchv7.py).

## <a name="prerequisites"></a>Voraussetzungen

[!INCLUDE [cognitive-services-bing-news-search-signup-requirements](../../../includes/cognitive-services-bing-news-search-signup-requirements.md)]

Siehe auch [Cognitive Services-Preise – Bing-Suche-API](https://azure.microsoft.com/pricing/details/cognitive-services/search-api/)

## <a name="create-and-initialize-the-application"></a>Erstellen und Initialisieren der Anwendung

1. Erstellen Sie in Ihrer bevorzugten IDE bzw. in einem Editor eine neue Python-Datei, und importieren Sie das Anforderungsmodul. Erstellen Sie Variablen für Ihren Abonnementschlüssel, den Endpunkt und einen Suchbegriff. Ihren Endpunkt finden Sie auf dem Azure-Dashboard.

```python
import requests

subscription_key = "your subscription key"
search_term = "Microsoft"
search_url = "https://api.cognitive.microsoft.com/bing/v7.0/news/search"
```

### <a name="create-parameters-for-the-request"></a>Erstellen von Parametern für die Anforderung

1. Fügen Sie Ihren Abonnementschlüssel zu einem neuen Wörterbuch hinzu. Verwenden Sie dabei `"Ocp-Apim-Subscription-Key"` als Schlüssel. Gehen Sie bei den Suchparametern genauso vor.

    ```python
    headers = {"Ocp-Apim-Subscription-Key" : subscription_key}
    params  = {"q": search_term, "textDecorations": True, "textFormat": "HTML"}
    ```

## <a name="send-a-request-and-get-a-response"></a>Senden einer Anforderung und Erhalten einer Antwort

1. Verwenden Sie die Anforderungsbibliothek, um mithilfe Ihres Abonnementschlüssels die API für die visuelle Bing-Suche und die im letzten Schritt erstellten Wörterbuchobjekte aufzurufen.

    ```python
    response = requests.get(search_url, headers=headers, params=params)
    response.raise_for_status()
    search_results = response.json()
    ```

2. `search_results` enthält die Antwort der API als JSON-Objekt. Greifen Sie auf die in der Antwort enthaltenen Beschreibungen der Artikel zu.
    
    ```python
    descriptions = [article["description"] for article in search_results["value"]]
    ```

## <a name="displaying-the-results"></a>Anzeigen der Ergebnisse

Diese Beschreibungen lassen sich dann als Tabelle darstellen, wobei das Suchschlüsselwort **fett** hervorgehoben ist.

```python
from IPython.display import HTML
rows = "\n".join(["<tr><td>{0}</td></tr>".format(desc)
                  for desc in descriptions])
HTML("<table>"+rows+"</table>")
```

## <a name="next-steps"></a>Nächste Schritte

> [!div class="nextstepaction"]
> [Erstellen einer Single-Page-Web-App](tutorial-bing-news-search-single-page-app.md)
