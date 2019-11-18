---
title: Architektur der IoT-vernetzten Logistik | Microsoft-Dokumentation
description: Architektur der Anwendungsvorlage für vernetzte Logistik für Azure IoT Central
author: KishorIoT
ms.author: nandab
ms.service: iot-central
ms.subservice: iot-central-retail
ms.topic: overview
ms.date: 10/20/2019
ms.openlocfilehash: 797488632f04af9a170f05590154e50310570b28
ms.sourcegitcommit: cf36df8406d94c7b7b78a3aabc8c0b163226e1bc
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 11/09/2019
ms.locfileid: "73890720"
---
# <a name="architecture-of-iot-central-connected-logistics-application-template"></a>Architektur der IoT Central-Anwendungsvorlage für vernetzte Logistik

[!INCLUDE [iot-central-pnp-original](../../../includes/iot-central-pnp-original-note.md)]

Partner und Kunden können die App-Vorlage nutzen und die nachstehenden Anweisungen befolgen, um **End-to-End-Lösungen für vernetzte Logistik** zu entwickeln.

> [!div class="mx-imgBorder"]
> ![Vernetzte Logistik: Dashboard](./media/concept-connected-logistics-architecture/connected-logistics-architecture.png)

1. Ein Satz aus IoT-Tags zum Senden von Telemetriedaten an ein Gatewaygerät
2. Gatewaygeräte senden Telemetrie und aggregierte Erkenntnisse an IoT Central
3. Daten werden zur Bearbeitung an den gewünschten Azure-Dienst geroutet
4. Mithilfe von Azure-Diensten wie ASA oder Azure Functions können Datenströme neu formatiert und an die gewünschten Speicherkonten gesendet werden 
5. Verschiedene Geschäftsworkflows können durch Endbenutzer-Geschäftsanwendungen unterstützt werden

## <a name="details"></a>Details
In den folgenden Abschnitten werden die einzelnen Komponenten der Architektur für die Telemetrieerfassung aus IoT-Tags und Gateways erläutert.

## <a name="iot-tags"></a>IoT-Tags
IoT-Tags bieten Sensorfunktionen zur Übermittlung von physikalischen, umweltspezifischen und umgebungsbezogenen Informationen wie Temperatur, Luftfeuchtigkeit, Erschütterung, Neigung und Licht. IoT-Tags sind in der Regel über Zigbee (802.15.4) mit einem Gatewaygerät verbunden. Tags sind kostengünstige Sensoren und können daher nach Beendigung eines typischen Logistikprojekts entsorgt werden, um Probleme bei der Retourenlogistik zu vermeiden.

## <a name="gateway"></a>Gateway
Gateways können mit ihren Umgebungssensorfunktionen ebenfalls als IoT-Tags fungieren. Ein Gateway bietet über Mobilfunk- oder WLAN-Kanäle Upstreamkonnektivität mit der Azure IoT-Cloud (MQTT).  Für die Kommunikation mit nachgeschalteten IoT-Tags werden Bluetooth-, NFC- und 802.15.4 WSN-Modi (Wireless Sensor Network) verwendet. Gateways bieten sichere End-to-End-Cloudkonnektivität und unterstützen die Kopplung von IoT-Tags, das Aggregieren von Sensordaten, die Aufbewahrung von Daten sowie die Möglichkeit, Alarmschwellenwerte zu konfigurieren.

## <a name="device-management-with-iot-central"></a>Geräteverwaltung mit IoT Central 
Azure IoT Central ist eine Plattform für die Lösungsentwicklung, die die Konnektivität, Konfiguration und Verwaltung von IoT-Geräten vereinfacht. Die Plattform verringert Aufwand und Kosten von Entwicklungen für IoT-Geräteverwaltung, -betrieb und verwandte Entwicklungen erheblich. Kunden und Partner können eine End-to-End-Enterprise-Lösung aufbauen, um eine digitale Feedbackschleife in der Logistik zu realisieren.

## <a name="business-insights--actions-via-data-egress"></a>Geschäftliche Erkenntnisse und Aktionen über ausgehende Daten 
Die IoT Central-Plattform bietet umfassende Erweiterungsmöglichkeiten in Form von CDE (Continuous Data Export) und APIs. Geschäftserkenntnisse, die auf der Verarbeitung von Telemetriedaten oder auf unverarbeiteter Telemetrie basieren, werden normalerweise in eine bevorzugte Branchenanwendung exportiert. Dies kann über Webhook, Service Bus, Event Hub oder Blob-Speicher erreicht werden, um Machine Learning-Modelle zu erstellen, zu trainieren, bereitzustellen und weitere Erkenntnisse zu gewinnen.

## <a name="next-steps"></a>Nächste Schritte
* Informieren Sie sich über die Bereitstellung der [Lösungsvorlage für vernetzte Logistik](./tutorial-iot-central-connected-logistics-pnp.md).
* Weitere Informationen zu [IoT Central-Einzelhandelsvorlagen](./overview-iot-central-retail-pnp.md)
* Lesen Sie die [Übersicht über IoT Central](../preview/overview-iot-central.md).
