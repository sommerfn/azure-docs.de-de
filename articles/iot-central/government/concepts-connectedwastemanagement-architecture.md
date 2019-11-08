---
title: Referenzarchitektur für die mit Azure IoT Central erstellte Lösung für vernetzte Abfallwirtschaft | Microsoft-Dokumentation
description: Lernen Sie Konzepte der mit Azure IoT Central erstellten Lösung für vernetzte Abfallwirtschaft kennen.
author: miriambrus
ms.author: miriamb
ms.date: 10/23/2019
ms.topic: tutorial
ms.service: iot-central
services: iot-central
ms.openlocfilehash: 2905a7ab7999c66bde0a705ff129fa049617df10
ms.sourcegitcommit: c22327552d62f88aeaa321189f9b9a631525027c
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 11/04/2019
ms.locfileid: "73498870"
---
# <a name="connected-waste-monitoring-reference-architecture"></a>Überwachung für vernetzte Abfallwirtschaft: Referenzarchitektur 

[!INCLUDE [iot-central-pnp-original](../../../includes/iot-central-pnp-original-note.md)]

Eine Lösung für vernetzte Abfallwirtschaft kann mit der **App-Vorlage „Azure IoT Central“** als Ausgangspunkt für IoT-Anwendungen entwickelt werden. Dieser Artikel bietet einen Leitfaden mit einer allgemeinen Referenzarchitektur zum Entwickeln einer Komplettlösung. 

![Architektur für vernetzte Abfallwirtschaft](./media/concepts-connectedwastemanagement-architecture/concepts-connectedwastemanagement-architecture1.png)


Konzepte:

1. Geräte und Konnektivität  
1. IoT Central 
2. Erweiterbarkeit und Integrationen
3. Geschäftsanwendungen

Werfen wir einen Blick auf Schlüsselkomponenten, die in der Regel bei einer Lösung zur Überwachung des Wasserverbrauchs eine Rolle spielen.

## <a name="devices-and-connectivity"></a>Geräte und Konnektivität 
Geräte, die im öffentlichen Raum zum Einsatz kommen, wie z. B. Mülltonnen, können im Allgemeinen mithilfe von Low-Power Wide Area Networks (LPWAN) über einen externen Netzbetreiber vernetzt werden. Für diese Gerätetypen können Sie die [Azure IoT Central-Geräte-Bridge](https://docs.microsoft.com/azure/iot-central/core/howto-build-iotc-device-bridge) nutzen, um Daten vom Gerät an Ihre IoT-Anwendung in Azure IoT Central zu senden. Alternativ bieten sich Gerätegateways an, die IP-fähig sind und direkt mit IoT Central verbunden werden können.

## <a name="iot-central"></a>IoT Central 
Azure IoT Central ist eine IoT-App-Plattform, mit der Sie Ihre IoT-Lösung schnell in Betrieb nehmen können. Sie können Ihre Lösung mit Branding versehen, anpassen und in Dienste von Drittanbietern integrieren.
Nachdem Sie Ihre Geräte mit intelligentem Wasserverbrauch mit IoT Central verbunden haben, stehen Ihnen für die Geräte Befehls-, Steuerungs-, Überwachungs- und Alarmfunktionen, eine Benutzeroberfläche mit integrierter rollenbasierter Zugriffssteuerung (RBAC), konfigurierbare Dashboards zum Gewinnen von Erkenntnissen und Erweiterungsoptionen zur Verfügung. 

## <a name="extensibility-and-integrations"></a>Erweiterbarkeit und Integrationen 
Sie können Ihre IoT-Anwendung in IoT Central erweitern und optional:
* Ihre IoT-Daten für erweiterte Analysen transformieren und integrieren, z. B. für das Training von Machine Learning-Modellen durch kontinuierlichen Datenexport aus der IoT Central-Anwendung. 
* Workflows in anderen Systemen automatisieren, indem in der IoT Central-Anwendung Aktionen über Microsoft Flow oder Webhooks ausgelöst werden
* über IoT Central-APIs programmgesteuert auf Ihre IoT-Anwendung in IoT Central zugreifen

## <a name="business-applications"></a>Geschäftsanwendungen 
Die IoT-Daten können verwendet werden, um eine Vielzahl von Geschäftsanwendungen innerhalb eines Abfallwirtschaftsbetriebs zu betreiben. Um zu erfahren, wie Sie Ihre mit IoT Central vernetzte Abfallwirtschaftsanwendung mit dem Außendienst verbinden können, sehen Sie sich das Tutorial über die [Integration in Dynamics 365 for Field Services](./how-to-configure-connected-field-services.md) an. 

## <a name="next-steps"></a>Nächste Schritte
* Erfahren Sie, wie Sie eine Azure IoT Central-Anwendung für [vernetzte Abfallwirtschaft](./tutorial-connected-waste-management.md) erstellen.
* Erfahren Sie mehr über [IoT Central-Vorlagen für Behörden](./overview-iot-central-government.md).
* Weitere Informationen zu IoT Central finden Sie in der [Übersicht über IoT Central](https://docs.microsoft.com/azure/iot-central/core/overview-iot-central).

