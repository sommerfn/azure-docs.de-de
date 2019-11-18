---
title: Übertragen zusätzlicher Daten zwischen dem Gerät und dem Device Provisioning-Dienst von Azure
description: In diesem Dokument wird beschrieben, wie Sie zusätzliche Daten zwischen Gerät und DPS übertragen.
author: menchi
ms.author: menchi
ms.date: 10/29/2019
ms.topic: conceptual
ms.service: iot-dps
services: iot-dps
ms.openlocfilehash: 1ff06afd363745ae465a8f5b625c27a4a9e4a222
ms.sourcegitcommit: c62a68ed80289d0daada860b837c31625b0fa0f0
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 11/05/2019
ms.locfileid: "73608606"
---
# <a name="how-to-transfer-additional-data-between-device-and-dps"></a>Übertragen zusätzlicher Daten zwischen Gerät und DPS
Mitunter benötigt DPS weitere Daten von Geräten, um sie ordnungsgemäß im richtigen IoT Hub bereitzustellen, und diese Daten müssen vom Gerät bereitgestellt werden. Ebenso kann DPS Daten an das Gerät zurückgeben, um clientseitige Logik zu ermöglichen. 

## <a name="when-to-use-it"></a>Einsatzgebiete
Diese Funktion kann als Ergänzung der [benutzerdefinierten Zuordnung](https://docs.microsoft.com/azure/iot-dps/how-to-use-custom-allocation-policies) verwendet werden. Beispiel: Sie möchten Ihre Geräte anhand des Gerätemodells ohne menschliche Eingriffe zuordnen. In diesem Fall verwenden Sie die [benutzerdefinierte Zuordnung](https://docs.microsoft.com/azure/iot-dps/how-to-use-custom-allocation-policies). Sie können das Gerät in diesem Fall für die Meldung der Modellinformationen im Rahmen des [Registrierungsgeräteaufrufs](https://docs.microsoft.com/rest/api/iot-dps/runtimeregistration/registerdevice) konfigurieren. DPS übergibt die Informationen des Geräts an den benutzerdefinierten Zuweisungswebhook. Dann kann Ihre Funktion entscheiden, auf welchen IoT Hub dieses Gerät geht, wenn es Gerätemodellinformationen empfängt. Wenn der Webhook einige Daten an das Gerät zurückgeben möchte, werden die Daten auf ähnliche Weise als Zeichenfolge in der Antwort des Webhooks übergeben.  

## <a name="device-sends-data-to-dps"></a>Gerät sendet Daten an DPS
Wenn Ihr Gerät einen [Registrierungsgeräteaufruf](https://docs.microsoft.com/rest/api/iot-dps/runtimeregistration/registerdevice) an DPS sendet. Der Registrierungsaufruf kann erweitert werden, um andere Felder im Textkörper zu verwenden. Der Textkörper sieht wie folgt aus: 
   ```
   { 
       “registrationId”: “mydevice”, 
       “tpm”:                
       { 
           “endorsementKey”: “stuff”, 
           “storageRootKey”: “things” 
       }, 
       “interfaces”: “TODO: get how interfaces are reported by devices from PnP folks.”, 
       “data”: “your additional data goes here. It can be nested JSON.” 
    } 
   ```

## <a name="dps-returns-data-to-the-device"></a>DPS gibt Daten an das Gerät zurück
Wenn der Webhook für die benutzerdefinierte Zuweisungsrichtlinie Daten an das Gerät zurückgeben möchte, werden die Daten als Zeichenfolge in der Antwort des Webhooks übergeben. Die Änderung befindet sich unten im Abschnitt returnData. 
   ```
   { 
       "iotHubHostName": "sample-iot-hub-1.azure-devices.net", 
       "initialTwin": { 
           "tags": { 
               "tag1": true 
               }, 
               "properties": { 
                   "desired": { 
                       "tag2": true 
                    } 
                } 
            }, 
        "returnData": "whatever is returned by the webhook" 
    } 
   ```

## <a name="sdk-support"></a>SDK-Unterstützung
Diese Funktion ist in den [Client-SDKs](https://docs.microsoft.com/azure/iot-dps/) für C, C#, Java und Node.js verfügbar.  

## <a name="next-steps"></a>Nächste Schritte
* Entwickeln mithilfe des [Azure IoT SDK]( https://github.com/Azure/azure-iot-sdks) für Azure IoT Hub und des Device Provisioning-Diensts von Azure IoT Hub