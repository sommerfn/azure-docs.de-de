---
title: Docker-Pullvorgang für den Sprachenerkennungscontainer
titleSuffix: Azure Cognitive Services
description: Befehl „docker pull“ für den Sprachenerkennungscontainer
services: cognitive-services
author: IEvangelist
manager: nitinme
ms.service: cognitive-services
ms.topic: include
ms.date: 09/12/2019
ms.author: dapine
ms.openlocfilehash: 23669a5440799ff071b3a30e3e41ab5d49f212df
ms.sourcegitcommit: fbea2708aab06c19524583f7fbdf35e73274f657
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 09/13/2019
ms.locfileid: "70966785"
---
#### <a name="docker-pull-for-the-language-detection-container"></a>Docker-Pullvorgang für den Sprachenerkennungscontainer

Verwenden Sie den Befehl [`docker pull`](https://docs.docker.com/engine/reference/commandline/pull/), um ein Containerimage aus Microsoft Container Registry herunterzuladen.

Eine vollständige Beschreibung der verfügbaren Tags für die Textanalysecontainer finden Sie im Docker Hub im Container für die [Sprachenerkennung](https://go.microsoft.com/fwlink/?linkid=2018759).

```
docker pull mcr.microsoft.com/azure-cognitive-services/language:latest
```
