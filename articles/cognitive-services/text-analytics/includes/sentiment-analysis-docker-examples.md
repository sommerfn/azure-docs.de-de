---
title: Docker-Beispiele für Standpunktanalyse-Container
titleSuffix: Azure Cognitive Services
description: Docker-Beispiele für Standpunktanalyse-Container
services: cognitive-services
author: IEvangelist
manager: nitinme
ms.service: cognitive-services
ms.topic: include
ms.date: 09/19/2019
ms.author: dapine
ms.openlocfilehash: c384d9d8af932db343aad106b6e89ea87821d900
ms.sourcegitcommit: 2ed6e731ffc614f1691f1578ed26a67de46ed9c2
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 09/19/2019
ms.locfileid: "71148413"
---
### <a name="sentiment-analysis-container-docker-examples"></a>Docker-Beispiele für Standpunktanalyse-Container

Im Anschluss finden Sie Docker-Beispiele für den Standpunktanalyse-Container.

#### <a name="basic-example"></a>Einfaches Beispiel 

  ```
  docker run --rm -it -p 5000:5000 --memory 4g --cpus 1 \
mcr.microsoft.com/azure-cognitive-services/sentiment \
Eula=accept \
Billing={ENDPOINT_URI} \
ApiKey={API_KEY} 
  ```

#### <a name="logging-example"></a>Beispiel für die Protokollierung 

  ```
  docker run --rm -it -p 5000:5000 --memory 4g --cpus 1 \
mcr.microsoft.com/azure-cognitive-services/sentiment \
Eula=accept \
Billing={ENDPOINT_URI} \
ApiKey={API_KEY} \
Logging:Console:LogLevel:Default=Information
  ```
