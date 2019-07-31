---
title: Erstellen einer Cognitive Services-Ressource zur Textanalyse
titleSuffix: Azure Cognitive Services
description: Erfahren Sie, wie Sie eine Cognitive Services-Ressource zur Textanalyse erstellen.
services: cognitive-services
author: IEvangelist
manager: nitinme
ms.service: cognitive-services
ms.topic: include
ms.date: 06/26/2019
ms.author: dapine
ms.openlocfilehash: 7e8b4480911f00afa8524ef4b81d697bb8ee5bcc
ms.sourcegitcommit: de47a27defce58b10ef998e8991a2294175d2098
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 07/15/2019
ms.locfileid: "67877445"
---
## <a name="create-a-cognitive-services-text-analytics-resource"></a>Erstellen einer Cognitive Services-Ressource zur Textanalyse

1. Melden Sie sich beim [Azure-Portal](https://portal.azure.com) an.
1. Klicken Sie auf **Ressource erstellen**, und navigieren Sie dann zu **KI + Machine Learning** > **Textanalyse**.
   Alternativ können Sie zu [Textanalyse erstellen](https://ms.portal.azure.com/#create/Microsoft.CognitiveServicesTextAnalytics) navigieren.
1. Geben Sie alle erforderlichen Einstellungen ein:

    |Einstellung|Wert|
    |--|--|
    |NAME|Geben Sie einen Namen ein (2 bis 64 Zeichen).|
    |Subscription|Wählen Sie das entsprechende Abonnement aus.|
    |Location|Wählen Sie einen nahe gelegenen Speicherort aus|
    |Tarif| Geben Sie **S** für den Tarif „Standard“ ein.|
    |Resource group|Wählen Sie eine verfügbare Ressourcengruppe aus|

1. Klicken Sie auf **Erstellen**, und warten Sie darauf, dass die Ressource erstellt wurde. Ihr Browser leitet Sie automatisch zur neu erstellten Ressourcenseite weiter.
1. Rufen Sie den konfigurierten `endpoint` und einen API-Schlüssel ab:

    |Registerkarte „Ressource“ im Portal|Einstellung|Wert|
    |--|--|--|
    |**Übersicht**|Endpunkt|Kopieren Sie den Endpunkt. Dieser ähnelt dem Folgendem: `https://northeurope.api.cognitive.microsoft.com/text/analytics/v2.0`|
    |**Schlüssel**|API-Schlüssel|Kopieren Sie einen der beiden Schlüssel. Es handelt sich um eine aus 32 alphanumerischen Zeichen bestehende Zeichenfolge ohne Leerzeichen oder Bindestriche: <`xxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxx`>.|
