---
ms.openlocfilehash: fe9879f9574fe1496ebdf20ac76fe5b759d4ea6a
ms.sourcegitcommit: bba811bd615077dc0610c7435e4513b184fbed19
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 08/27/2019
ms.locfileid: "70051243"
---
### <a name="container-repositories-and-images"></a>Containerrepositorys und -images

Die folgenden Tabellen stellen eine umfassende Liste der verfügbaren Containerimages dar, die von Azure Cognitive Services geboten werden.

#### <a name="public-container-registry-mcrmicrosoftcom"></a>Öffentlich (Containerregistrierung: `mcr.microsoft.com`)

Die Microsoft-Containerregistrierung hostet alle Container für die allgemeine Verfügbarkeit (General Availability, GA) für Cognitive Services.

| Dienst | Container | Containerregistrierung/Repository/Imagename |
|--|--|--|
| [LUIS](../../LUIS/luis-container-howto.md) | LUIS | `mcr.microsoft.com/azure-cognitive-services/luis` |
| [Textanalyse](../../text-analytics/how-tos/text-analytics-how-to-install-containers.md) | Schlüsselwortextraktion | `mcr.microsoft.com/azure-cognitive-services/keyphrase` |
| [Textanalyse](../../text-analytics/how-tos/text-analytics-how-to-install-containers.md) | Spracherkennung | `mcr.microsoft.com/azure-cognitive-services/language` |
| [Textanalyse](../../text-analytics/how-tos/text-analytics-how-to-install-containers.md) | Standpunktanalyse | `mcr.microsoft.com/azure-cognitive-services/sentiment` |

#### <a name="public-preview-container-registry-containerpreviewazurecrio"></a>Öffentliche Vorschau (Containerregistrierung: `containerpreview.azurecr.io`)

Die Container Preview-Registrierung hostet alle „Öffentliche Vorschau“-Container für Cognitive Services, die noch nicht auf die allgemeine Verfügbarkeit (General Availability, GA) umgestellt wurden. Diese Container erfordern eine formelle Zugriffsanforderung, um sie zu nutzen.

| Dienst | Container | Containerregistrierung/Repository/Imagename |
|--|--|--|
| [Anomalieerkennung](../../anomaly-detector/anomaly-detector-container-howto.md) | Anomalieerkennung | `containerpreview.azurecr.io/microsoft/cognitive-services-anomaly-detector` |
| [Maschinelles Sehen](../../Computer-vision/computer-vision-how-to-install-containers.md) | Texterkennung | `containerpreview.azurecr.io/microsoft/cognitive-services-recognize-text` |
| [Gesichtserkennung](../../face/face-how-to-install-containers.md) | Gesicht | `containerpreview.azurecr.io/microsoft/cognitive-services-face` |
| [Formularerkennung](https://go.microsoft.com/fwlink/?linkid=2083826&clcid=0x409) | Formularerkennung | `containerpreview.azurecr.io/microsoft/cognitive-services-form-recognizer` |
| [Spracherkennungsdienst-API](../../speech-service/speech-container-howto.md) | Spracherkennung | `containerpreview.azurecr.io/microsoft/cognitive-services-speech-to-text` |
| [Spracherkennungsdienst-API](../../speech-service/speech-container-howto.md) | Text-zu-Sprache | `containerpreview.azurecr.io/microsoft/cognitive-services-text-to-speech` |
