---
title: Containerunterstützung
titleSuffix: Azure Cognitive Services
description: Erfahren Sie, wie Sie eine Cognitive Services-Ressource zur Textanalyse erstellen.
services: cognitive-services
author: IEvangelist
manager: nitinme
ms.service: cognitive-services
ms.topic: include
ms.date: 06/26/2019
ms.author: dapine
ms.openlocfilehash: 022ffcd806d4d4f89f8a8cf256a541518ea12602
ms.sourcegitcommit: f56b267b11f23ac8f6284bb662b38c7a8336e99b
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 06/28/2019
ms.locfileid: "67455108"
---
## <a name="create-a-cognitive-services-text-analytics-resource"></a>Erstellen einer Cognitive Services-Ressource zur Textanalyse

1. Melden Sie sich beim [Azure-Portal](https://portal.azure.com) an.
1. Wählen Sie **+Ressource erstellen** aus, navigieren Sie zu **KI und Machine Learning > Textanalyse**, oder klicken Sie auf [Erstellen **Textanalyse**](https://ms.portal.azure.com/#create/Microsoft.CognitiveServicesTextAnalytics).
1. Geben Sie alle erforderlichen Einstellungen ein:

    |Einstellung|Wert|
    |--|--|
    |NAME|Gewünschter Name (2-64 Zeichen)|
    |Abonnement|Wählen Sie ein passendes Abonnement aus|
    |Location|Wählen Sie einen nahe gelegenen Speicherort aus|
    |Preisstufe|`S` – der Tarif „Standard“|
    |Ressourcengruppe|Wählen Sie eine verfügbare Ressourcengruppe aus|

1. Klicken Sie auf **Erstellen**, und warten Sie, bis die Ressource erstellt wird. Sobald sie erstellt ist, leitet Ihr Browser Sie automatisch zur neu erstellten Ressourcenseite weiter.
1. Sammeln Sie den konfigurierten `endpoint` und einen API-Schlüssel:

    |Registerkarte „Ressource“ im Portal|Einstellung|Wert|
    |--|--|--|
    |**Übersicht**|Endpunkt|Kopieren Sie den Endpunkt. Er sieht etwa so aus: `https://northeurope.api.cognitive.microsoft.com/text/analytics/v2.0`.|
    |**Schlüssel**|API-Schlüssel|Kopieren Sie einen der beiden Schlüssel. Es handelt sich um eine aus 32 alphanumerischen Zeichen bestehende Zeichenfolge ohne Leerzeichen oder Bindestriche, `xxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxx`.|