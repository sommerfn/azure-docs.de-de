---
title: Containerrepositorys und -images
services: cognitive-services
author: IEvangelist
manager: nitinme
description: Zwei Tabellen, die die Containerregistrierungen, Repositorys und Imagenamen für alle Cognitive Service-Angebote darstellen.
ms.service: cognitive-services
ms.topic: include
ms.date: 09/06/2019
ms.author: dapine
ms.openlocfilehash: 181f4acd86dfacb15592ded6f2df3287e3dc13bf
ms.sourcegitcommit: 2ed6e731ffc614f1691f1578ed26a67de46ed9c2
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 09/19/2019
ms.locfileid: "71130151"
---
### <a name="container-repositories-and-images"></a>Containerrepositorys und -images

Die folgenden Tabellen stellen eine umfassende Liste der verfügbaren Containerimages dar, die von Azure Cognitive Services geboten werden.

#### <a name="public-ungated-container-registry-mcrmicrosoftcom"></a>Öffentliche „nicht verwaltete“ Vorschau (Containerregistrierung: `mcr.microsoft.com`)

Die Microsoft-Containerregistrierung hostet alle öffentlich verfügbaren, „nicht verwalteten“ Container für Cognitive Services.

| Dienst | Container | Containerregistrierung/Repository/Imagename |
|--|--|--|
| [LUIS](../../LUIS/luis-container-howto.md) | LUIS | `mcr.microsoft.com/azure-cognitive-services/luis` |
| [Textanalyse](../../text-analytics/how-tos/text-analytics-how-to-install-containers.md) | Schlüsselwortextraktion | `mcr.microsoft.com/azure-cognitive-services/keyphrase` |
| [Textanalyse](../../text-analytics/how-tos/text-analytics-how-to-install-containers.md) | Spracherkennung | `mcr.microsoft.com/azure-cognitive-services/language` |
| [Textanalyse](../../text-analytics/how-tos/text-analytics-how-to-install-containers.md) | Standpunktanalyse | `mcr.microsoft.com/azure-cognitive-services/sentiment` |

#### <a name="public-gated-preview-container-registry-containerpreviewazurecrio"></a>Öffentliche "verwaltete" Vorschau (Containerregistrierung: `containerpreview.azurecr.io`)

Die Vorschauversion der Containerregistrierung hostet alle öffentlich verfügbaren, „verwalteten“ Container für Cognitive Services. Diese Container erfordern eine formelle Zugriffsanforderung, um sie zu nutzen.

| Dienst | Container | Containerregistrierung/Repository/Imagename |
|--|--|--|
| [Anomalieerkennung](../../anomaly-detector/anomaly-detector-container-howto.md) | Anomalieerkennung | `containerpreview.azurecr.io/microsoft/cognitive-services-anomaly-detector` |
| [Maschinelles Sehen](../../Computer-vision/computer-vision-how-to-install-containers.md) | Texterkennung | `containerpreview.azurecr.io/microsoft/cognitive-services-recognize-text` |
| [Maschinelles Sehen](../../Computer-vision/computer-vision-how-to-install-containers.md) | Lesen | `containerpreview.azurecr.io/microsoft/cognitive-services-read` |
| [Gesichtserkennung](../../face/face-how-to-install-containers.md) | Gesicht | `containerpreview.azurecr.io/microsoft/cognitive-services-face` |
| [Formularerkennung](https://go.microsoft.com/fwlink/?linkid=2083826&clcid=0x409) | Formularerkennung | `containerpreview.azurecr.io/microsoft/cognitive-services-form-recognizer` |
| [Spracherkennungsdienst-API](../../speech-service/speech-container-howto.md) | Spracherkennung | `containerpreview.azurecr.io/microsoft/cognitive-services-speech-to-text` |
| [Spracherkennungsdienst-API](../../speech-service/speech-container-howto.md) | Text-zu-Sprache | `containerpreview.azurecr.io/microsoft/cognitive-services-text-to-speech` |
