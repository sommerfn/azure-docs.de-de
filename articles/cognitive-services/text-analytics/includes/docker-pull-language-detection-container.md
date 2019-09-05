---
title: Docker-Pullvorgang für den Sprachenerkennungscontainer
titleSuffix: Azure Cognitive Services
description: Befehl „docker pull“ für den Sprachenerkennungscontainer
services: cognitive-services
author: IEvangelist
manager: nitinme
ms.service: cognitive-services
ms.topic: include
ms.date: 08/20/2019
ms.author: dapine
ms.openlocfilehash: da6f04f1042d1a02178f345c5fe67387ae0a7d0f
ms.sourcegitcommit: bba811bd615077dc0610c7435e4513b184fbed19
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 08/27/2019
ms.locfileid: "70051310"
---
## <a name="pull-the-language-detection-container"></a>Pullen des Sprachenerkennungscontainers

In der Microsoft Container Registry stehen Containerimages für die Textanalyse zur Verfügung.

| Container | Containerregistrierung/Repository/Imagename |
|-----------|------------|
| Spracherkennung | `mcr.microsoft.com/azure-cognitive-services/language` |

Verwenden Sie den Befehl [`docker pull`](https://docs.docker.com/engine/reference/commandline/pull/), um ein Containerimage aus Microsoft Container Registry herunterzuladen.

Eine vollständige Beschreibung der verfügbaren Tags für die Textanalysecontainer finden Sie im Docker Hub im Container für die [Sprachenerkennung](https://go.microsoft.com/fwlink/?linkid=2018759).


### <a name="docker-pull-for-the-language-detection-container"></a>Docker-Pullvorgang für den Sprachenerkennungscontainer

```
docker pull mcr.microsoft.com/azure-cognitive-services/language:latest
```
