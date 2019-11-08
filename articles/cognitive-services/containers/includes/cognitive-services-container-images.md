---
title: Containerrepositorys und -images
services: cognitive-services
author: IEvangelist
manager: nitinme
description: Zwei Tabellen, die die Containerregistrierungen, Repositorys und Imagenamen für alle Cognitive Service-Angebote darstellen.
ms.service: cognitive-services
ms.topic: include
ms.date: 10/09/2019
ms.author: dapine
ms.openlocfilehash: c1593cb3dad7ee1370a66747fa3fe47e93c19957
ms.sourcegitcommit: c22327552d62f88aeaa321189f9b9a631525027c
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 11/04/2019
ms.locfileid: "73499156"
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
| [Maschinelles Sehen](../../Computer-vision/computer-vision-how-to-install-containers.md) | Lesen | `containerpreview.azurecr.io/microsoft/cognitive-services-read` |
| [Gesichtserkennung](../../face/face-how-to-install-containers.md) | Gesicht | `containerpreview.azurecr.io/microsoft/cognitive-services-face` |
| [Formularerkennung](https://go.microsoft.com/fwlink/?linkid=2083826&clcid=0x409) | Formularerkennung | `containerpreview.azurecr.io/microsoft/cognitive-services-form-recognizer` |
<<<<<<< KOPFTEIL | [Spracherkennungsdienst-API](../../speech-service/speech-container-howto.md?tab=stt) | Spracherkennung | `containerpreview.azurecr.io/microsoft/cognitive-services-speech-to-text` | | [Spracherkennungsdienst-API](../../speech-service/speech-container-howto.md?tab=cstt) | benutzerdefinierte Spracherkennung | `containerpreview.azurecr.io/microsoft/cognitive-services-custom-speech-to-text` | | [Spracherkennungsdienst-API](../../speech-service/speech-container-howto.md?tab=tts) | Text-zu-Sprache | `containerpreview.azurecr.io/microsoft/cognitive-services-text-to-speech` | | [Spracherkennungsdienst-API](../../speech-service/speech-container-howto.md?tab=ctts) | Text-zu-Sprache benutzerdefiniert | `containerpreview.azurecr.io/microsoft/cognitive-services-custom-text-to-speech` | ======= | [Spracherkennungsdienst-API](../../speech-service/speech-container-howto.md) | Spracherkennung | `containerpreview.azurecr.io/microsoft/cognitive-services-speech-to-text` | | [Spracherkennungsdienst-API](../../speech-service/speech-container-howto.md) | Text-zu-Sprache | `containerpreview.azurecr.io/microsoft/cognitive-services-text-to-speech` | | [Textübersetzung](../../translator/how-to-install-containers.md) | Textübersetzung | `containerpreview.azurecr.io/microsoft/cognitive-services-translator-text` |
>>>>>>> refs/remotes/MicrosoftDocs/master
