---
title: Docker-Beispiele für Schlüsselbegriffserkennungs-Container
titleSuffix: Azure Cognitive Services
description: Docker-Beispiele für Schlüsselbegriffserkennungs-Container
services: cognitive-services
author: IEvangelist
manager: nitinme
ms.service: cognitive-services
ms.topic: include
ms.date: 08/21/2019
ms.author: dapine
ms.openlocfilehash: a26dd24cbac10d2296a51be85ab4b3f4bdb03cf2
ms.sourcegitcommit: bba811bd615077dc0610c7435e4513b184fbed19
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 08/27/2019
ms.locfileid: "70051108"
---
## <a name="key-phrase-extraction-container-docker-examples"></a>Docker-Beispiele für Schlüsselbegriffserkennungs-Container

Im Anschluss finden Sie Docker-Beispiele für den Schlüsselbegriffserkennungs-Container.

### <a name="basic-example"></a>Einfaches Beispiel 

  ```
  docker run --rm -it -p 5000:5000 --memory 4g --cpus 1 \
mcr.microsoft.com/azure-cognitive-services/keyphrase \
Eula=accept \
Billing={ENDPOINT_URI} \
ApiKey={API_KEY} 
  ```

### <a name="logging-example"></a>Beispiel für die Protokollierung 

  ```
  docker run --rm -it -p 5000:5000 --memory 4g --cpus 1 \
mcr.microsoft.com/azure-cognitive-services/keyphrase \
Eula=accept \
Billing={ENDPOINT_URI} \
ApiKey={API_KEY} \
Logging:Console:LogLevel:Default=Information
  ```
