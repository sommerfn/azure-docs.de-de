---
title: 'Abrufen der Absicht, Python: LUIS'
titleSuffix: Azure Cognitive Services
description: In dieser Schnellstartanleitung übergeben Sie Äußerungen an einen LUIS-Endpunkt, um Absichten und Entitäten zurückzugeben.
services: cognitive-services
author: diberry
manager: nitinme
ms.custom: seodec18
ms.service: cognitive-services
ms.subservice: language-understanding
ms.topic: quickstart
ms.date: 07/16/2019
ms.author: diberry
ms.openlocfilehash: defcb6713516952e56e6cca2d63a99ee61f5b868
ms.sourcegitcommit: 7c4de3e22b8e9d71c579f31cbfcea9f22d43721a
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 07/26/2019
ms.locfileid: "68563780"
---
# <a name="quickstart-get-intent-using-python"></a>Schnellstart: Abrufen der Absicht mit Python
In dieser Schnellstartanleitung übergeben Sie Äußerungen an einen LUIS-Endpunkt, um Absichten und Entitäten zurückzugeben.

[!INCLUDE [Quickstart introduction for endpoint](../../../includes/cognitive-services-luis-qs-endpoint-intro-para.md)]

## <a name="prerequisites"></a>Voraussetzungen

* [Python 3.6](https://www.python.org/downloads/) oder höher
* [Visual Studio Code](https://code.visualstudio.com/)

[!INCLUDE [Use authoring key for endpoint](../../../includes/cognitive-services-luis-qs-endpoint-luis-repo-note.md)]

## <a name="get-luis-key"></a>Abrufen des LUIS-Schlüssels

[!INCLUDE [Use authoring key for endpoint](../../../includes/cognitive-services-luis-qs-endpoint-get-key-para.md)]

## <a name="get-intent-with-browser"></a>Abrufen der Absicht im Browser

[!INCLUDE [Use authoring key for endpoint](../../../includes/cognitive-services-luis-qs-endpoint-browser-para.md)]

## <a name="get-intent--programmatically"></a>Programmgesteuertes Abrufen der Absicht

Sie können mit Python auf die gleichen Ergebnisse zugreifen, die im vorherigen Schritt im Browserfenster angezeigt wurden.

1. Kopieren Sie einen der folgenden Codeausschnitte in eine Datei namens `quickstart-call-endpoint.py`:

   [!code-python[Console app code that calls a LUIS endpoint for Python 2.7](~/samples-luis/documentation-samples/quickstarts/analyze-text/python/2.x/quickstart-call-endpoint-2-7.py)]

   [!code-python[Console app code that calls a LUIS endpoint for Python 3.6](~/samples-luis/documentation-samples/quickstarts/analyze-text/python/3.x/quickstart-call-endpoint-3-6.py)]

2. Ersetzen Sie den Wert des Felds `Ocp-Apim-Subscription-Key` durch Ihren LUIS-Endpunktschlüssel.

3. Installieren Sie Abhängigkeiten mit `pip install requests`.

4. Führen Sie das Skript mit `python ./quickstart-call-endpoint.py` aus. Es wird das gleiche JSON-Ergebnis wie zuvor im Browserfenster angezeigt.

## <a name="luis-keys"></a>LUIS-Schlüssel

[!INCLUDE [Use authoring key for endpoint](../../../includes/cognitive-services-luis-qs-endpoint-key-usage-para.md)]

## <a name="clean-up-resources"></a>Bereinigen von Ressourcen
Löschen Sie die Python-Datei. 

## <a name="next-steps"></a>Nächste Schritte

> [!div class="nextstepaction"]
> [Hinzufügen von Äußerungen](luis-get-started-python-add-utterance.md)
