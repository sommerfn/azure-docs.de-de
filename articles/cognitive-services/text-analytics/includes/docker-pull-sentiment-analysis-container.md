---
title: Docker-Pullvorgang für den Standpunktanalysecontainer
titleSuffix: Azure Cognitive Services
description: Befehl „docker pull“ für den Standpunktanalysecontainer
services: cognitive-services
author: IEvangelist
manager: nitinme
ms.service: cognitive-services
ms.topic: include
ms.date: 08/20/2019
ms.author: dapine
ms.openlocfilehash: 0954ef88f6917159156fbb73103b7b7af3283c00
ms.sourcegitcommit: bba811bd615077dc0610c7435e4513b184fbed19
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 08/27/2019
ms.locfileid: "70051276"
---
## <a name="pull-the-sentiment-analysis-container"></a>Pullen des Standpunktanalysecontainers

In der Microsoft Container Registry stehen Containerimages für die Textanalyse zur Verfügung.

| Container | Containerregistrierung/Repository/Imagename |
|-----------|------------|
| Standpunktanalyse | `mcr.microsoft.com/azure-cognitive-services/sentiment` |

Verwenden Sie den Befehl [`docker pull`](https://docs.docker.com/engine/reference/commandline/pull/), um ein Containerimage aus Microsoft Container Registry herunterzuladen.

Eine vollständige Beschreibung der verfügbaren Tags für die Textanalysecontainer finden Sie im Docker Hub im Container für die [Standpunktanalyse](https://go.microsoft.com/fwlink/?linkid=2018654).

### <a name="docker-pull-for-the-sentiment-analysis-container"></a>Docker-Pullvorgang für den Standpunktanalysecontainer

```
docker pull mcr.microsoft.com/azure-cognitive-services/sentiment:latest
```
