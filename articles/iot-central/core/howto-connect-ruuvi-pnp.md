---
title: Verbinden eines RuuviTag-Geräts in Azure IoT Central | Microsoft-Dokumentation
description: Erfahren Sie, wie Sie einen RuuviTag-Umgebungssensor mit Ihrer IoT Central-Anwendung verbinden.
services: iot-central
ms.service: iot-central
ms.topic: conceptual
ms.custom:
- iot-storeAnalytics-conditionMonitor
- iot-p0-scenario
ms.author: avneets
author: avneet723
ms.date: 10/19/2019
ms.openlocfilehash: f1d152c921d38931f8c67396fc5769cfd2dfcf58
ms.sourcegitcommit: c22327552d62f88aeaa321189f9b9a631525027c
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 11/04/2019
ms.locfileid: "73468237"
---
# <a name="connect-a-ruuvitag-sensor-to-your-azure-iot-central-application"></a>Verbinden eines RuuviTag-Sensors mit Ihrer Azure IoT Central-Anwendung

In diesem Artikel wird beschrieben, wie Sie als Lösungsentwickler einen RuuviTag-Sensor mit Ihrer Microsoft Azure IoT Central-Anwendung verbinden können.

Was ist RuuviTag?

RuuviTag ist eine moderne und führende Open-Source-Plattform für Sensorbeacons für die Anforderungen von Geschäftskunden, Entwicklern, Herstellern und Studenten, die sogar in Ihrem persönlichen Umfeld verwendet werden kann. Das Gerät ist so eingerichtet, dass es direkt nach dem Auspacken einsatzbereit ist und am gewünschten Ort bereitgestellt werden kann. Dabei handelt es sich um einen Bluetooth LE-Beacon mit integriertem Umgebungssensor und Beschleunigungsmesser. 

RuuviTag kommuniziert über BLE (Bluetooth Low Energy) und erfordert daher ein Gatewaygerät für die Kommunikation mit Azure IoT Central. Sie benötigen ein Gatewaygerät wie „Rigado Cascade 500“, das für die Verbindung von RuuviTag mit IoT Central konfiguriert sein muss. 

Befolgen Sie [diese Anweisungen](./howto-connect-rigado-cascade-500-pnp.md), um ein Rigado Cascade 500-Gatewaygerät einzurichten.

## <a name="prerequisites"></a>Voraussetzungen
Zum Verbinden von RuuviTag-Sensoren benötigen Sie folgende Ressourcen:

* Einen RuuviTag-Sensor. Weitere Informationen finden Sie unter [RuuviTag](https://ruuvi.com/). 
* Ein Rigado Cascade 500-Gerät oder ein anderes BLE-Gateway. Weitere Informationen finden Sie unter [Rigado](https://www.rigado.com/).
* Eine Azure IoT Central-Anwendung, die mit einer der Vorschauanwendungsvorlagen erstellt wurde. Weitere Informationen finden Sie unter [Erstellen einer neuen Anwendung](https://docs.microsoft.com/azure/iot-central/core/quick-deploy-iot-central-pnp?toc=/azure/iot-central-pnp/toc.json&bc=/azure/iot-central-pnp/breadcrumb/toc.json).

## <a name="add-a-ruuvitag-device-template"></a>Hinzufügen einer RuuviTag-Gerätevorlage

Für das Onboarding eines RuuviTag-Sensors in Ihrer Azure IoT Central-Anwendungsinstanz müssen Sie eine entsprechende Gerätevorlage in der Anwendung konfigurieren.

So fügen Sie eine RuuviTag-Gerätevorlage hinzu 

1. Navigieren Sie im linken Bereich zur Registerkarte ***Gerätevorlagen***, und wählen Sie **+ Neue** aus: ![Erstellen einer neuen Gerätevorlage](./media/howto-connect-ruuvi/devicetemplate-new.png) Sie haben auf der Seite die Optionen ***Benutzerdefinierte Vorlage erstellen*** und ***Vorkonfigurierte Gerätevorlage verwenden***.
1. Wählen Sie in der Liste der vorkonfigurierten Gerätevorlagen die RuuviTag-Gerätevorlage aus, wie unten dargestellt: ![Auswählen der RuuviTag-Gerätevorlage](./media/howto-connect-ruuvi/devicetemplate-preconfigured.png)
1. Klicken Sie auf ***Weiter: Anpassen***, um mit dem nächsten Schritt fortzufahren. 
1. Wählen Sie auf dem nächsten Bildschirm ***Erstellen*** aus, um das Onboarding der C500-Gerätevorlage in Ihrer IoT Central-Anwendung durchzuführen.

## <a name="connect-a-ruuvitag-sensor"></a>Verbinden eines RuuviTag-Sensors

Wie bereits erwähnt, müssen Sie ein Gatewaygerät einrichten, um das RuuviTag-Gerät mit Ihrer IoT Central-Anwendung zu verbinden. In den folgenden Schritten wird davon ausgegangen, dass Sie ein Rigado Cascade 500-Gatewaygerät eingerichtet haben.  

1. Schalten Sie Ihr Rigado Cascade 500-Gerät ein, und verbinden Sie es mit Ihrem Netzwerk (über Ethernet oder drahtlos).
1. Nehmen Sie die Abdeckung des RuuviTag-Geräts ab, und ziehen Sie die Plastiklasche heraus, um die Akkusicherung aufzuheben. 
1. Platzieren Sie das RuuviTag-Gerät in unmittelbarer Nähe Ihres Rigado Cascade 500-Gateways, das zuvor mit Ihrer IoT Central-Anwendung konfiguriert wurde. 
1. Nach wenigen Sekunden sollte Ihr RuuviTag-Gerät in der Liste der Geräte in IoT Central angezeigt werden.  
![RuuviTag-Geräteliste](./media/howto-connect-ruuvi/ruuvi-devicelist.png) Sie können dieses RuuviTag-Gerät nun in Ihrer IoT Central-Anwendung verwenden.  

## <a name="create-a-simulated-ruuvitag"></a>Erstellen eines simulierten RuuviTag-Geräts
Wenn Sie nicht über ein physisches RuuviTag-Gerät verfügen, können Sie einen simulierten RuuviTag-Sensor erstellen und für Tests in Ihrer Azure IoT Central-Anwendung verwenden.

So erstellen Sie ein simuliertes RuuviTag-Gerät

1. Wählen Sie **Geräte > RuuviTag** aus. 
1. Wählen Sie **+ Neu** aus. 
1. Geben Sie eine eindeutige **Geräte-ID** und einen benutzerfreundlichen **Gerätenamen** an.  
1. Aktivieren Sie die Einstellung **Simuliert**.
1. Klicken Sie auf **Erstellen**.  

## <a name="next-steps"></a>Nächste Schritte

Nachdem Sie nun erfahren haben, wie Sie ein RuuviTag-Gerät mit Ihrer Azure IoT Central-Anwendung verbinden können, wird im nächsten Schritt empfohlen, sich über das [Anpassen Ihrer IoT Central-Anwendung](../retail/tutorial-in-store-analytics-customize-dashboard-pnp.md) zu informieren, um eine vollständige Lösung entwickeln zu können. 