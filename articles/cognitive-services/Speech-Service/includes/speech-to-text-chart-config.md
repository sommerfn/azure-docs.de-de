---
title: Installieren von Speech-Containern
titleSuffix: Azure Cognitive Services
description: Beschreibt die Konfigurationsoptionen für ein Helm-Diagramm zur Spracherkennung.
services: cognitive-services
author: IEvangelist
manager: nitinme
ms.service: cognitive-services
ms.subservice: speech-service
ms.topic: include
ms.date: 08/22/2019
ms.author: dapine
ms.openlocfilehash: 3f390affe7badb401277aa86d1867c763aa0ae3b
ms.sourcegitcommit: 47b00a15ef112c8b513046c668a33e20fd3b3119
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 08/22/2019
ms.locfileid: "69971324"
---
### <a name="speech-to-text-sub-chart-chartsspeechtotext"></a>Spracherkennung (Unterdiagramm: charts/speechToText)

Zum Überschreiben des übergeordneten Diagramms fügen Sie das Präfix `speechToText.` bei einem beliebigen Parameter hinzu, um das Diagramm genauer zu definieren. Dadurch wird der entsprechende Parameter überschrieben. Beispielsweise überschreibt `speechToText.numberOfConcurrentRequest` den Parameter `numberOfConcurrentRequest`.

|Parameter|BESCHREIBUNG|Standard|
| -- | -- | -- |
| `enabled` | Gibt an, ob der **Spracherkennungs**-Dienst aktiviert ist. | `false` |
| `numberOfConcurrentRequest` | Die Anzahl gleichzeitiger Anforderungen für den **Spracherkennungs**-Dienst. In diesem Diagramm werden automatisch CPU- und Speicherressourcen basierend auf diesem Wert berechnet. | `2` |
| `optimizeForAudioFile`| Gibt an, ob der Dienst für die Audioeingabe über Audiodateien optimiert werden muss. Wenn `true`, werden in diesem Diagramm dem Dienst mehr CPU-Ressourcen zugeordnet. | `false` |
| `image.registry`| Die Docker-Imageregistrierung für **Spracherkennung**. | `containerpreview.azurecr.io` |
| `image.repository` | Das Docker-Imagerepository für **Spracherkennung**. | `microsoft/cognitive-services-speech-to-text` |
| `image.tag` | Das Docker-Imagetag für **Spracherkennung**. | `latest` |
| `image.pullSecrets` | Die Imagegeheimnisse zum Abrufen des Docker-Images für **Spracherkennung**. | |
| `image.pullByHash`| Gibt an, ob das Docker-Image mithilfe von Hashing abgerufen wird. Wenn `true`, ist `image.hash` erforderlich. | `false` |
| `image.hash`| Das Docker-Imagehash für **Spracherkennung**. Nur verwendet, wenn `image.pullByHash: true`.  | |
| `image.args.eula` (erforderlich) | Gibt an, dass Sie die Lizenz akzeptiert haben. Der einzige gültige Wert ist `accept`. | |
| `image.args.billing` (erforderlich) | Den URI des Abrechnungsendpunkts finden Sie im Azure-Portal auf der Speech-Übersichtsseite. | |
| `image.args.apikey` (erforderlich) | Wird zum Nachverfolgen von Abrechnungsinformationen verwendet. ||
| `service.type` | Die Kubernetes-Diensttyp des **Spracherkennungs**-Diensts. Weitere Informationen finden Sie unter den [Kubernetes-Anweisungen für Diensttypen ](https://kubernetes.io/docs/concepts/services-networking/service/) und beim Support des Cloudanbieters. | `LoadBalancer` |
| `service.port`|  Der Port des **Spracherkennungs**-Diensts. | `80` |
| `service.annotations` | Die **Spracherkennungs**-Anmerkungen für die Dienstmetadaten. Anmerkungen sind Schlüssel-Wert-Paare. <br>`annotations:`<br>&nbsp;&nbsp;`some/annotation1: value1`<br>&nbsp;&nbsp;`some/annotation2: value2` | |
| `service.autoScaler.enabled` | Gibt an, ob die [horizontale automatische Podskalierung](https://kubernetes.io/docs/tasks/run-application/horizontal-pod-autoscale/) aktiviert ist. Wenn `true`, wird `speech-to-text-autoscaler` im Kubernetes-Cluster bereitgestellt. | `true` |
| `service.podDisruption.enabled` | Gibt an, ob das [Budget für die Unterbrechung von Pods](https://kubernetes.io/docs/concepts/workloads/pods/disruptions/) aktiviert ist. Wenn `true`, wird `speech-to-text-poddisruptionbudget` im Kubernetes-Cluster bereitgestellt. | `true` |