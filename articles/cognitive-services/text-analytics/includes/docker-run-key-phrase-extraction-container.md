---
title: Beispiel zum Ausführen eines Containers mit dem „docker run“-Befehl
titleSuffix: Azure Cognitive Services
description: Docker-Ausführungsbefehl für den Schlüsselbegriffserkennungs-Container
services: cognitive-services
author: IEvangelist
manager: nitinme
ms.service: cognitive-services
ms.topic: include
ms.date: 09/12/2019
ms.author: dapine
ms.openlocfilehash: b8c3bdfc05e855139b595cd0ba7bd723cdeaee45
ms.sourcegitcommit: fbea2708aab06c19524583f7fbdf35e73274f657
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 09/13/2019
ms.locfileid: "70966746"
---
Zum Ausführen des Containers für die *Schlüsselbegriffserkennung* führen Sie den folgenden `docker run`-Befehl aus.

```bash
docker run --rm -it -p 5000:5000 --memory 4g --cpus 1 \
mcr.microsoft.com/azure-cognitive-services/keyphrase \
Eula=accept \
Billing={ENDPOINT_URI} \
ApiKey={API_KEY}
```

Dieser Befehl:

* Führt einen *Schlüsselbegriffserkennungs*-Container auf der Grundlage des Containerimages aus.
* Weist einen einzelnen CPU-Kern und 4 GB Arbeitsspeicher zu
* Verfügbarmachen des TCP-Ports 5000 und Zuweisen einer Pseudo-TTY-Verbindung für den Container
* Entfernt den Container automatisch, nachdem er beendet wurde. Das Containerimage ist auf dem Hostcomputer weiterhin verfügbar.