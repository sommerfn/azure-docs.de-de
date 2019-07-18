---
title: Erstellen von Subclips für ein Video beim Codieren mit der REST-API für Azure Media Services
description: In diesem Artikel wird beschrieben, wie Sie beim Codieren mit Azure Media Services mithilfe von REST Subclips für ein Video erstellen
services: media-services
documentationcenter: ''
author: Juliako
manager: femila
editor: ''
ms.service: media-services
ms.workload: media
ms.tgt_pltfrm: na
ms.devlang: ne
ms.topic: article
ms.date: 06/10/2019
ms.author: juliako
ms.openlocfilehash: df8c8a4040b4aae4379b4bfe0e9a16337588dd1b
ms.sourcegitcommit: 82efacfaffbb051ab6dc73d9fe78c74f96f549c2
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 06/20/2019
ms.locfileid: "67304852"
---
# <a name="subclip-a-video-when-encoding-with-media-services---rest"></a>Erstellen von Subclips für ein Video beim Codieren mit Media Services: REST

Sie können mithilfe eines [Auftrags](https://docs.microsoft.com/rest/api/media/jobs) Videos beim Codieren zuschneiden oder Subclips für diese erstellen. Dies funktioniert mit jeder [Transformation](https://docs.microsoft.com/rest/api/media/transforms), die entweder mit den [BuiltInStandardEncoderPreset](https://docs.microsoft.com/rest/api/media/transforms/createorupdate#builtinstandardencoderpreset)-Voreinstellungen oder mit den [StandardEncoderPreset](https://docs.microsoft.com/rest/api/media/transforms/createorupdate#standardencoderpreset)-Voreinstellungen erstellt wird. 

Mithilfe des in diesem Artikel verwendeten REST-Beispiels wird ein Auftrag erstellt, der ein Video zuschneidet, sobald er einen Codierungsauftrag übermittelt. 

## <a name="prerequisites"></a>Voraussetzungen

Für die in diesem Thema beschriebenen Schritte ist Folgendes erforderlich:

- [Erstellen eines Azure Media Services-Kontos](create-account-cli-how-to.md)
- [Konfigurieren Sie Postman für Azure Media Services-REST-API-Aufrufe](media-rest-apis-with-postman.md).
    
    Befolgen Sie dabei unbedingt den letzten Schritt im Thema [Abrufen von Azure AD-Token](media-rest-apis-with-postman.md#get-azure-ad-token). 
- Erstellen Sie eine Transformation und ein Ausgabeobjekt. Informationen zum Erstellen einer Transformation und von Ausgabeobjekten finden Sie im Tutorial [Codieren einer Remotedatei anhand einer URL und Streamen des Videos über REST](stream-files-tutorial-with-rest.md).
- Lesen Sie sich den Artikel [Encoding concept (Codierungskonzept)](encoding-concept.md) durch.

## <a name="create-a-subclipping-job"></a>Erstellen eines Auftrags zum Erstellen von Subclips

1. Klicken Sie in der heruntergeladenen Postman Collection auf **Transforms and Jobs** (Transformationen und Aufträge) -> **Create Job with Sub Clipping** (Auftrag mit Subclips erstellen).
    
    Die **PUT**-Anforderung sieht wie folgt aus:
    
    ```
    https://management.azure.com/subscriptions/:subscriptionId/resourceGroups/:resourceGroupName/providers/Microsoft.Media/mediaServices/:accountName/transforms/:transformName/jobs/:jobName?api-version={{api-version}}
    ```
1. Aktualisieren Sie den Wert der Umgebungsvariablen „transformName“ durch den Namen Ihrer Transformation. 
1. Klicken Sie auf die Registerkarte **Body** (Text), und aktualisieren Sie das Element „myOutputAsset“ durch den Namen Ihres Ausgabeobjekts.

    ```
    {
      "properties": {
        "description": "A Job with transform cb9599fb-03b3-40eb-a2ff-7ea909f53735 and single clip.",
       
        "input": {
          "@odata.type": "#Microsoft.Media.JobInputHttp",
          "baseUri": "https://nimbuscdn-nimbuspm.streaming.mediaservices.windows.net/2b533311-b215-4409-80af-529c3e853622/",
          "files": [
                "Ignite-short.mp4"
            ],
          "start": {
            "@odata.type": "#Microsoft.Media.AbsoluteClipTime",
            "time": "PT10S"
          },
          "end": {
            "@odata.type": "#Microsoft.Media.AbsoluteClipTime",
            "time": "PT40S"
          }
        },
      
        "outputs": [
          {
            "@odata.type": "#Microsoft.Media.JobOutputAsset",
            "assetName": "myOutputAsset"
          }
        ],
        "priority": "Normal"
      }
    }
    ```
1. Klicken Sie auf **Senden**.

    Dann wird Ihnen die **Antwort** mit den Informationen zu dem Auftrag einschließlich dessen Status angezeigt, der erstellt und übermittelt wurde. 

## <a name="next-steps"></a>Nächste Schritte

[Codieren mit einer benutzerdefinierten Transformation](custom-preset-rest-howto.md) 
