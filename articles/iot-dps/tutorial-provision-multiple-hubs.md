---
title: Bereitstellen von Geräten für IoT Hubs mit Lastausgleich mithilfe des Azure IoT Hub Device Provisioning-Diensts | Microsoft-Dokumentation
description: Automatische Device Provisioning Service-Gerätebereitstellung für IoT Hubs mit Lastausgleich im Azure-Portal
author: sethmanheim
ms.author: sethm
ms.date: 09/05/2017
ms.topic: tutorial
ms.service: iot-dps
services: iot-dps
manager: timlt
ms.custom: mvc
ms.openlocfilehash: 8ed1b128fc2663cfc09c40f001bd0b14663a728e
ms.sourcegitcommit: c22327552d62f88aeaa321189f9b9a631525027c
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 11/04/2019
ms.locfileid: "73486014"
---
# <a name="provision-devices-across-load-balanced-iot-hubs"></a>Bereitstellen von Geräten für IoT Hubs mit Lastausgleich

In diesem Tutorial wird gezeigt, wie Sie mit Device Provisioning Service Geräte für mehrere IoT Hubs mit Lastausgleich bereitstellen. In diesem Tutorial lernen Sie Folgendes:

> [!div class="checklist"]
> * Bereitstellen eines zweiten Geräts für eine zweite IoT Hub-Instanz mit dem Azure-Portal 
> * Hinzufügen eines Registrierungslisteneintrags für das zweite Gerät
> * Festlegen der Device Provisioning Service-Zuordnungsrichtlinie zur **gleichmäßigen Verteilung**
> * Verknüpfen der neuen IoT Hub-Instanz mit Device Provisioning Service

Wenn Sie kein Azure-Abonnement besitzen, können Sie ein [kostenloses Konto](https://azure.microsoft.com/free/) erstellen, bevor Sie beginnen.

## <a name="prerequisites"></a>Voraussetzungen

Dieses Tutorial baut auf dem vorherigen Tutorial [Bereitstellen eines Geräts für einen Hub](tutorial-provision-device-to-hub.md) auf.

## <a name="use-the-azure-portal-to-provision-a-second-device-to-a-second-iot-hub"></a>Bereitstellen eines zweiten Geräts für eine zweite IoT Hub-Instanz mit dem Azure-Portal

Um ein zweites Gerät für eine andere IoT Hub-Instanz bereitzustellen, führen Sie die Schritte im Tutorial [Bereitstellen eines Geräts für einen Hub](tutorial-provision-device-to-hub.md) durch.

## <a name="add-an-enrollment-list-entry-to-the-second-device"></a>Hinzufügen eines Registrierungslisteneintrags für das zweite Gerät

Die Registrierungsliste weist Device Provisioning Service an, welche Nachweismethode (die Methode zur Bestätigung einer Geräteidentität) beim Gerät verwendet wird. Der nächste Schritt besteht darin, einen Registrierungslisteneintrag für das zweite Gerät hinzuzufügen. 

1. Klicken Sie auf der Seite für Ihre Device Provisioning Service-Instanz auf **Registrierungen verwalten**. Die Seite **Registrierungslisteneintrag hinzufügen** wird angezeigt. 
2. Klicken Sie oben auf der Seite auf **Hinzufügen**.
2. Füllen Sie die Felder aus, und klicken Sie dann auf **Speichern**.

## <a name="set-the-device-provisioning-service-allocation-policy"></a>Festlegen der Device Provisioning Service-Zuordnungsrichtlinie

Die Zuordnungsrichtlinie ist eine Device Provisioning Service-Einstellung, die festlegt, wie Geräte einer IoT Hub-Instanz zugewiesen werden. Es gibt drei unterstützte Zuordnungsrichtlinien: 

1. **Niedrigste Latenz**: Geräte werden basierend auf dem Hub mit der geringsten Latenz auf dem Gerät für eine IoT Hub-Instanz bereitgestellt.
2. **Gleichmäßig gewichtete Verteilung** (Standard): Bei verknüpften IoT Hubs ist die Wahrscheinlichkeit gleich hoch, dass Geräte für sie bereitgestellt werden. Dies ist die Standardeinstellung. Wenn Sie nur für eine IoT Hub-Instanz Geräte bereitstellen, können Sie diese Einstellung beibehalten. 
3. **Statische Konfiguration per Registrierungsliste**: Die Angabe der gewünschten IoT Hub-Instanz in der Registrierungsliste hat gegenüber der Zuordnungsrichtlinie auf Ebene des Device Provisioning-Diensts Vorrang.

Führen Sie zum Festlegen der Zuordnungsrichtlinie folgende Schritte durch:

1. Klicken Sie zum Festlegen der Zuordnungsrichtlinie auf der Seite „Device Provisioning-Dienst“ auf **Zuordnungsrichtlinie verwalten**.
2. Legen Sie für die Zuordnungsrichtlinie **Gleichmäßig gewichtete Verteilung** fest.
3. Klicken Sie auf **Speichern**.

## <a name="link-the-new-iot-hub-to-the-device-provisioning-service"></a>Verknüpfen der neuen IoT Hub-Instanz mit Device Provisioning Service

Verknüpfen Sie Device Provisioning Service und die IoT Hub-Instanz, damit Device Provisioning Service Geräte in diesem Hub registrieren kann.

1. Klicken Sie auf der Seite **Alle Ressourcen** auf die Device Provisioning Service-Instanz, die Sie zuvor erstellt haben.
2. Klicken Sie auf der Seite „Device Provisioning-Dienst“ auf **Verknüpfte IoT Hubs**.
3. Klicken Sie auf **Hinzufügen**.
4. Aktivieren Sie auf der Seite **Verknüpfung zu IoT Hub hinzufügen** die Optionsfelder, um anzugeben, ob sich der verknüpfte IoT Hub im aktuellen Abonnement oder in einem anderen Abonnement befindet. Wählen Sie dann im Feld **IoT Hub** den Namen von IoT Hub.
5. Klicken Sie auf **Speichern**.

## <a name="next-steps"></a>Nächste Schritte

In diesem Tutorial haben Sie Folgendes gelernt:

> [!div class="checklist"]
> * Bereitstellen eines zweiten Geräts für eine zweite IoT Hub-Instanz mit dem Azure-Portal 
> * Hinzufügen eines Registrierungslisteneintrags für das zweite Gerät
> * Festlegen der Device Provisioning Service-Zuordnungsrichtlinie zur **gleichmäßigen Verteilung**
> * Verknüpfen der neuen IoT Hub-Instanz mit Device Provisioning Service

<!-- Advance to the next tutorial to learn how to 
 Replace this .md
> [!div class="nextstepaction"]
> [Bind an existing custom SSL certificate to Azure Web Apps]()
-->
