---
title: Abfrage eines Spracherkennungscontainer-Endpunkts
services: cognitive-services
author: IEvangelist
manager: nitinme
ms.service: cognitive-services
ms.subservice: speech-service
ms.topic: include
ms.date: 11/04/2019
ms.author: dapine
ms.openlocfilehash: a55cf5ea6aa696d0cf0cdd619dc22839d748d83c
ms.sourcegitcommit: c22327552d62f88aeaa321189f9b9a631525027c
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 11/04/2019
ms.locfileid: "73491161"
---
### <a name="speech-to-text-or-custom-speech-to-text"></a>Spracherkennung oder benutzerdefinierte Spracherkennung

Der Container bietet websocketbasierte Abfrageendpunkt-APIs, auf die über das [Speech SDK](../index.md) zugegriffen wird. Standardmäßig verwendet das Speech SDK Online-Speech-Dienste. Um den Container verwenden zu können, müssen Sie die Initialisierungsmethode ändern.

> [!TIP]
> Wenn Sie das Sprach-SDK mit Containern verwenden, müssen Sie nicht die Azure Speech-Ressource [Abonnementschlüssel oder Authentifizierungsbearertoken](../rest-speech-to-text.md#authentication) bereitstellen.

Weitere Informationen finden Sie in den folgenden Beispielen.

# <a name="ctabcsharp"></a>[C#](#tab/csharp)

Ändern Sie den folgenden Azure-Cloudinitialisierungsaufruf

```csharp
var config = SpeechConfig.FromSubscription("YourSubscriptionKey", "YourServiceRegion");
```

in den folgenden Aufruf mit dem [Containerendpunkt](https://docs.microsoft.com/dotnet/api/microsoft.cognitiveservices.speech.speechconfig.fromendpoint?view=azure-dotnet):

```csharp
var config = SpeechConfig.FromEndpoint(
    new Uri("ws://localhost:5000/speech/recognition/dictation/cognitiveservices/v1"));
```
# <a name="pythontabpython"></a>[Python](#tab/python)

Ändern Sie den folgenden Azure-Cloudinitialisierungsaufruf

```python
speech_config = speechsdk.SpeechConfig(
    subscription=speech_key, region=service_region)
```

in den folgenden Aufruf mit dem [Containerendpunkt](https://docs.microsoft.com/python/api/azure-cognitiveservices-speech/azure.cognitiveservices.speech.speechconfig?view=azure-python):

```python
speech_config = speechsdk.SpeechConfig(
    endpoint="ws://localhost:5000/speech/recognition/dictation/cognitiveservices/v1")
```

***
