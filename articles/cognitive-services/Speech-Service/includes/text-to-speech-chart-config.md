---
title: Installieren von Speech-Containern
titleSuffix: Azure Cognitive Services
description: Beschreibt die Konfigurationsoptionen für ein Helm-Diagramm zur Sprachsynthese.
services: cognitive-services
author: IEvangelist
manager: nitinme
ms.service: cognitive-services
ms.subservice: speech-service
ms.topic: include
ms.date: 06/26/2019
ms.author: dapine
ms.openlocfilehash: e6c7dcd3015b0b8ab5b3c719ebd2397bc814b81a
ms.sourcegitcommit: c105ccb7cfae6ee87f50f099a1c035623a2e239b
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 07/10/2019
ms.locfileid: "67717210"
---
### <a name="text-to-speech-sub-chart-chartstexttospeech"></a>Sprachsynthese (Unterdiagramm: charts/textToSpeech)

Zum Überschreiben des übergeordneten Diagramms fügen Sie das Präfix `textToSpeech.` bei einem beliebigen Parameter hinzu, um das Diagramm genauer zu definieren. Dadurch wird der entsprechende Parameter überschrieben. Beispielsweise überschreibt `textToSpeech.numberOfConcurrentRequest` den Parameter `numberOfConcurrentRequest`.

|Parameter|BESCHREIBUNG|Standard|
| -- | -- | -- |
| `enabled` | Gibt an, ob der **Sprachsynthese**-Dienst aktiviert ist. | `false` |
| `numberOfConcurrentRequest` | Die Anzahl gleichzeitiger Anforderungen für den **Sprachsynthese**-Dienst. In diesem Diagramm werden automatisch CPU- und Speicherressourcen basierend auf diesem Wert berechnet. | `2` |
| `optimizeForTurboMode`| Gibt an, ob der Dienst für die Texteingabe über Textdateien optimiert werden muss. Wenn `true`, werden in diesem Diagramm dem Dienst mehr CPU-Ressourcen zugeordnet. | `false` |
| `image.registry`| Die Docker-Imageregistrierung für **Sprachsynthese**. | `containerpreview.azurecr.io` |
| `image.repository` | Das Docker-Imagerepository für **Sprachsynthese**. | `microsoft/cognitive-services-text-to-speech` |
| `image.tag` | Das Docker-Imagetag für **Sprachsynthese**. | `latest` |
| `image.pullSecrets` | Die Imagegeheimnisse zum Abrufen des Docker-Images für **Sprachsynthese**. | |
| `image.pullByHash`| Gibt an, ob das Docker-Image mithilfe von Hashing abgerufen wird. Wenn `true`, ist `image.hash` erforderlich. | `false` |
| `image.hash`| Das Docker-Imagehash für **Sprachsynthese**. Nur verwendet, wenn `image.pullByHash: true`.  | |
| `image.args.eula` (erforderlich) | Gibt an, dass Sie die Lizenz akzeptiert haben. Der einzige gültige Wert ist `accept`. | |
| `image.args.billing` (erforderlich) | Den URI des Abrechnungsendpunkts finden Sie im Azure-Portal auf der Speech-Übersichtsseite. | |
| `image.args.apikey` (erforderlich) | Wird zum Nachverfolgen von Abrechnungsinformationen verwendet. ||
| `service.type` | Die Kubernetes-Diensttyp des **Sprachsynthese**-Diensts. Weitere Informationen finden Sie unter den [Kubernetes-Anweisungen für Diensttypen ](https://kubernetes.io/docs/concepts/services-networking/service/) und beim Support des Cloudanbieters. | `LoadBalancer` |
| `service.port`|  Der Port des **Sprachsynthese**-Diensts. | `80` |
| `service.autoScaler.enabled` | Gibt an, ob die [horizontale automatische Podskalierung](https://kubernetes.io/docs/tasks/run-application/horizontal-pod-autoscale/) aktiviert ist. Wenn `true`, wird `text-to-speech-autoscaler` im Kubernetes-Cluster bereitgestellt. | `true` |
| `service.podDisruption.enabled` | Gibt an, ob das [Budget für die Unterbrechung von Pods](https://kubernetes.io/docs/concepts/workloads/pods/disruptions/) aktiviert ist. Wenn `true`, wird `text-to-speech-poddisruptionbudget` im Kubernetes-Cluster bereitgestellt. | `true` |