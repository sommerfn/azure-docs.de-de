---
title: 'Schnellstart: Aufrufen Ihres Endpunkts für die benutzerdefinierte Bing-Suche mit dem Python SDK | Microsoft-Dokumentation'
titleSuffix: Azure Cognitive Services
description: Verwenden Sie diese Schnellstartanleitung, um unter Verwendung des Python SDK Suchergebnisse von Ihrer Instanz der benutzerdefinierten Bing-Suche anzufordern.
services: cognitive-services
author: aahill
manager: nitinme
ms.service: cognitive-services
ms.subservice: bing-custom-search
ms.topic: quickstart
ms.date: 07/26/2019
ms.author: aahi
ms.openlocfilehash: 7a9d0d6619878299768e1f17b4da3280ea1ec1fc
ms.sourcegitcommit: 7c4de3e22b8e9d71c579f31cbfcea9f22d43721a
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 07/26/2019
ms.locfileid: "68564611"
---
# <a name="quickstart-call-your-bing-custom-search-endpoint-using-the-python-sdk"></a>Schnellstart: Aufrufen Ihres Endpunkts für die benutzerdefinierte Bing-Suche mit dem Python SDK 

Verwenden Sie diese Schnellstartanleitung, um unter Verwendung des Python SDK Suchergebnisse von Ihrer Instanz der benutzerdefinierten Bing-Suche anzufordern. Die benutzerdefinierte Bing-Suche verfügt zwar über eine REST-API, die mit den meisten Programmiersprachen kompatibel ist, aber das SDK für die benutzerdefinierte Bing-Suche ist eine einfache Möglichkeit, den Dienst in Ihre Anwendungen zu integrieren. Der Quellcode für dieses Beispiel steht mit zusätzlichen Fehlerbehandlungen und Anmerkungen auf [GitHub](https://github.com/Azure-Samples/cognitive-services-python-sdk-samples/blob/master/samples/search/custom_search_samples.py) bereit.

## <a name="prerequisites"></a>Voraussetzungen

- Eine Instanz der benutzerdefinierten Bing-Suche. Weitere Informationen finden Sie unter [Schnellstart: Erstellen Ihrer ersten Instanz der benutzerdefinierten Bing-Suche](quick-start.md).
- Python [2.x oder 3.x](https://www.python.org/) 
- Das [SDK für die benutzerdefinierte Bing-Suche für Python](https://pypi.org/project/azure-cognitiveservices-search-customsearch/) 

## <a name="install-the-python-sdk"></a>Installieren des Python SDK

Installieren Sie das SDK für die benutzerdefinierte Bing-Suche mithilfe des folgenden Befehls:

```Console
python -m pip install azure-cognitiveservices-search-customsearch
```


## <a name="create-a-new-application"></a>Erstellen einer neuen Anwendung

Erstellen Sie in Ihrem bevorzugten Editor oder in Ihrer bevorzugten IDE eine neue Python-Datei, und fügen Sie die folgenden Importe hinzu:

```python
from azure.cognitiveservices.search.customsearch import CustomSearchClient
from msrest.authentication import CognitiveServicesCredentials
```

## <a name="create-a-search-client-and-send-a-request"></a>Erstellen eines Clients für die Suche und Senden einer Anforderung

1. Erstellen Sie eine Variable für Ihren Abonnementschlüssel.

    ```python
    subscription_key = 'your-subscription-key'
    ```

2. Erstellen Sie eine Instanz von `CustomSearchClient`. Verwenden Sie dabei ein Objekt vom Typ `CognitiveServicesCredentials` mit dem Abonnementschlüssel. 

    ```python
    client = CustomSearchClient(CognitiveServicesCredentials(subscription_key))
    ```

3. Senden Sie eine Suchanforderung mit `client.custom_instance.search()`. Fügen Sie Ihren Suchbegriff an den Parameter `query` an, und legen Sie `custom_config` auf Ihre benutzerdefinierte Konfigurations-ID fest, um Ihre Suchinstanz zu verwenden. Die ID finden Sie im [Portal für die benutzerdefinierte Bing-Suche](https://www.customsearch.ai/) auf der Registerkarte **Produktion**.

    ```python
    web_data = client.custom_instance.search(query="xbox", custom_config="your-configuration-id")
    ```

## <a name="view-the-search-results"></a>Anzeigen der Suchergebnisse

Sofern bei der Suche Webseiten gefunden wurden, rufen Sie das erste Ergebnis ab, und geben Sie Name, URL und die Gesamtanzahl gefundener Webseiten aus.

```python
if web_data.web_pages.value:
    first_web_result = web_data.web_pages.value[0]
    print("Web Pages result count: {}".format(len(web_data.web_pages.value)))
    print("First Web Page name: {}".format(first_web_result.name))
    print("First Web Page url: {}".format(first_web_result.url))
else:
    print("Didn't see any web data..")
```

## <a name="next-steps"></a>Nächste Schritte

> [!div class="nextstepaction"]
> [Erstellen einer Web-App für die benutzerdefinierte Suche](./tutorials/custom-search-web-page.md)
