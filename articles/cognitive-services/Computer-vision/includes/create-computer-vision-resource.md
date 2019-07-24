---
title: Containerunterstützung
titleSuffix: Azure Cognitive Services
services: cognitive-services
author: IEvangelist
manager: nitinme
ms.service: cognitive-services
ms.topic: include
ms.date: 7/5/2019
ms.author: dapine
ms.openlocfilehash: cbf11c13bfb5c90739ea67fab92df08796a88e50
ms.sourcegitcommit: c105ccb7cfae6ee87f50f099a1c035623a2e239b
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 07/10/2019
ms.locfileid: "67717284"
---
## <a name="create-an-computer-vision-resource"></a>Erstellen einer Maschinelles Sehen-Ressource

1. Melden Sie sich beim [Azure-Portal](https://portal.azure.com) an.
1. Klicken Sie auf [**Maschinelles Sehen**-Ressource erstellen](https://ms.portal.azure.com/#create/Microsoft.CognitiveServicesComputerVision).
1. Geben Sie alle erforderlichen Einstellungen ein:

    |Einstellung|Wert|
    |--|--|
    |NAME|Gewünschter Name (2 bis 64 Zeichen)|
    |Abonnement|Wählen Sie ein passendes Abonnement aus|
    |Location|Wählen Sie einen nahe gelegenen und verfügbaren Speicherort aus|
    |Preisstufe|`F0` – der Mindesttarif|
    |Ressourcengruppe|Wählen Sie eine verfügbare Ressourcengruppe aus|

1. Klicken Sie auf **Erstellen**, und warten Sie, bis die Ressource erstellt wird. Nachdem sie erstellt wurde, navigieren Sie zur Ressourcenseite.
1. Erfassen Sie den konfigurierten `endpoint` und einen API-Schlüssel:

    |Registerkarte „Ressource“ im Portal|Einstellung|Wert|
    |--|--|--|
    |**Übersicht**|Endpunkt|Kopieren Sie den Endpunkt. Er sieht etwa so aus: `https://computer-vision.cognitiveservices.azure.com/`.|
    |**Schlüssel**|API-Schlüssel|Kopieren Sie einen der beiden Schlüssel. Es handelt sich um eine aus 32 alphanumerischen Zeichen bestehende Zeichenfolge ohne Leerzeichen oder Bindestriche, `xxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxx`.|
