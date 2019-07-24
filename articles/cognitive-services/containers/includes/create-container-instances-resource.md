---
title: Containerunterstützung
titleSuffix: Azure Cognitive Services
description: Erfahren Sie, wie Sie eine Azure Container Instances-Ressource erstellen können.
services: cognitive-services
author: IEvangelist
manager: nitinme
ms.service: cognitive-services
ms.topic: include
ms.date: 7/3/2019
ms.author: dapine
ms.openlocfilehash: 38addf4651373ba0f4df411325218a255c835508
ms.sourcegitcommit: c105ccb7cfae6ee87f50f099a1c035623a2e239b
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 07/10/2019
ms.locfileid: "67717197"
---
## <a name="create-an-azure-container-instance-resource"></a>Erstellen einer Azure Container Instances-Ressource

1. Wechseln Sie zur Seite [Erstellen](https://ms.portal.azure.com/#create/Microsoft.ContainerInstances) für Container Instances.

2. Geben Sie auf der Registerkarte **Grundeinstellungen** die folgenden Informationen ein:

    |Einstellung|Wert|
    |--|--|
    |Abonnement|Wählen Sie Ihr Abonnement aus.|
    |Ressourcengruppe|Wählen Sie die verfügbare Ressource aus, oder erstellen Sie eine neue wie z.B. `cognitive-services`.|
    |Containername|Geben Sie einen Namen ein (beispielsweise `cognitive-container-instance`). Der Name muss aus Kleinbuchstaben bestehen.|
    |Location|Wählen Sie eine Region für die Bereitstellung aus.|
    |Imagetyp|`Public`|
    |Imagename|Geben Sie den Speicherort des Cognitive Services-Containers an. Der Speicherort kann mit dem im Befehl `docker pull` verwendeten Speicherort identisch sein. Die verfügbaren Imagenamen und entsprechenden Repositorys finden Sie unter [Containerrepositorys und -images](../../cognitive-services-container-support.md#container-repositories-and-images).|
    |Betriebssystemtyp|`Linux`|
    |Size|Ändern Sie die Größe entsprechend den vorgeschlagenen Empfehlungen für Ihren spezifischen Cognitive Services-Container:<br>2 CPU-Kerne<br>4 GB

3. Geben Sie auf der Registerkarte **Netzwerk** die folgenden Informationen ein:

    |Einstellung|Wert|
    |--|--|
    |Ports|Legen Sie den TCP-Port auf `5000` fest. Macht den Container an Port 5000 verfügbar.|

4. Geben Sie auf der Registerkarte **Erweitert** die erforderlichen **Umgebungsvariablen** für die [Abrechnungseinstellungen](https://docs.microsoft.com/azure/cognitive-services/text-analytics/how-tos/text-analytics-how-to-install-containers#billing-arguments) des Containers der ACI-Ressource ein:

    | Schlüssel | Wert |
    |--|--|
    |`apikey`|Wird von der Seite **Schlüssel** der Textanalyseressource kopiert. Es handelt sich um eine aus 32 alphanumerischen Zeichen bestehende Zeichenfolge ohne Leerzeichen oder Bindestriche, `xxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxx`.|
    |`billing`|Wird von der Seite **Übersicht** der Textanalyseressource kopiert. Beispiel: `https://northeurope.api.cognitive.microsoft.com/text/analytics/v2.0`|
    |`eula`|`accept`|

1. Klicken Sie auf **Überprüfen und erstellen**.
1. Nachdem die Überprüfung abgeschlossen ist, klicken Sie auf **Erstellen**, um den Erstellungsprozess abzuschließen.
1. Nach ihrer erfolgreicher Bereitstellung ist die Ressource bereit.