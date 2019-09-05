---
title: Docker-Pullvorgang für den Schlüsselbegriffserkennungs-Container
titleSuffix: Azure Cognitive Services
description: Docker-Pullbefehl für den Schlüsselbegriffserkennungs-Container
services: cognitive-services
author: IEvangelist
manager: nitinme
ms.service: cognitive-services
ms.topic: include
ms.date: 08/20/2019
ms.author: dapine
ms.openlocfilehash: e71db5455a3d6726cd6567d568a13fcdcf9cb520
ms.sourcegitcommit: bba811bd615077dc0610c7435e4513b184fbed19
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 08/27/2019
ms.locfileid: "70051375"
---
## <a name="pull-the-key-phrase-extraction-container"></a>Pullen des Schlüsselbegriffserkennungs-Containers

In der Microsoft Container Registry stehen Containerimages für die Textanalyse zur Verfügung.

| Container | Container Registry/Repository/Imagename |
|-----------|------------|
| Schlüsselwortextraktion | `mcr.microsoft.com/azure-cognitive-services/keyphrase` |

Verwenden Sie den Befehl [`docker pull`](https://docs.docker.com/engine/reference/commandline/pull/), um ein Containerimage aus Microsoft Container Registry herunterzuladen.

Eine vollständige Beschreibung der verfügbaren Tags für die Textanalysecontainer finden Sie in Docker Hub im [Container für die Schlüsselbegriffserkennung](https://go.microsoft.com/fwlink/?linkid=2018757).

### <a name="docker-pull-for-the-key-phrase-extraction-container"></a>Docker-Pullvorgang für den Schlüsselbegriffserkennungs-Container

```
docker pull mcr.microsoft.com/azure-cognitive-services/keyphrase:latest
```
