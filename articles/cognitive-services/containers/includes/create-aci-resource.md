---
title: Containerunterstützung
titleSuffix: Azure Cognitive Services
description: Erfahren Sie, wie Sie eine ACI-Ressource (Azure Container Instances) erstellen können.
services: cognitive-services
author: IEvangelist
manager: nitinme
ms.service: cognitive-services
ms.topic: include
ms.date: 06/26/2019
ms.author: dapine
ms.openlocfilehash: b24a78873c3220b969fc548d2553e465e80773cd
ms.sourcegitcommit: f56b267b11f23ac8f6284bb662b38c7a8336e99b
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 06/28/2019
ms.locfileid: "67455076"
---
## <a name="create-an-azure-container-instance-aci-resource"></a>Erstellen einer ACI-Ressource (Azure Container Instances)

1. Wechseln Sie zur Seite [Erstellen](https://ms.portal.azure.com/#create/Microsoft.ContainerInstances) für Container Instances.

2. Geben Sie auf der Registerkarte **Grundeinstellungen** die folgenden Informationen ein:

    |Einstellung|Wert|
    |--|--|
    |Abonnement|Wählen Sie Ihr Abonnement aus.|
    |Ressourcengruppe|Wählen Sie die verfügbare Ressource aus, oder erstellen Sie eine neue wie z.B. `cognitive-services`.|
    |Containername|Geben Sie einen Namen ein (beispielsweise `cognitive-container-instance`). Dieser Name muss aus Kleinbuchstaben bestehen.|
    |Location|Wählen Sie eine Region für die Bereitstellung aus.|
    |Imagetyp|`Public`|
    |Imagename|Geben Sie den Speicherort des Cognitive Services-Containers an. Dies kann derselbe Speicherort sein, den Sie im Befehl `docker pull` verwendet haben, _beispielsweise_: <br>`mcr.microsoft.com/azure-cognitive-services/sentiment`|
    |Betriebssystemtyp|`Linux`|
    |Size|Ändern Sie die Größe entsprechend den vorgeschlagenen Empfehlungen für Ihren spezifischen Cognitive Services-Container:<br>2 Kerne<br>4 GB

3. Geben Sie auf der Registerkarte **Netzwerk** die folgenden Informationen ein:

    |Einstellung|Wert|
    |--|--|
    |Ports|Legen Sie den TCP-Port auf `5000` fest. Macht den Container an Port 5000 verfügbar.|

4. Geben Sie auf der Registerkarte **Erweitert** die folgenden Details ein, um die [erforderlichen Abrechnungseinstellungen](https://docs.microsoft.com/azure/cognitive-services/text-analytics/how-tos/text-analytics-how-to-install-containers#billing-arguments) des Containers an die ACI-Ressource zu übergeben:

    |Seite „Erweitert“: Schlüssel|Seite „Erweitert“: Wert|
    |--|--|
    |`apikey`|Wird von der Seite **Schlüssel** der Textanalyseressource kopiert. Es handelt sich um eine aus 32 alphanumerischen Zeichen bestehende Zeichenfolge ohne Leerzeichen oder Bindestriche, `xxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxx`.|
    |`billing`|Wird von der Seite **Übersicht** der Textanalyseressource kopiert. Beispiel: `https://northeurope.api.cognitive.microsoft.com/text/analytics/v2.0`|
    |`eula`|`accept`|

1. Klicken Sie auf **Überprüfen und erstellen**.
1. Nachdem die Validierung abgeschlossen ist, klicken Sie auf **Erstellen**, um den Erstellungsprozess abzuschließen.
1. Nach ihrer erfolgreicher Bereitstellung ist die Ressource einsatzbereit.