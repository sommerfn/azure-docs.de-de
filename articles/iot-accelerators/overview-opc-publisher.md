---
title: Was ist OPC Publisher? – Azure | Microsoft-Dokumentation
description: Übersicht über OPC Publisher
author: dominicbetts
ms.author: dobett
ms.date: 06/10/2019
ms.topic: overview
ms.service: iot-industrialiot
services: iot-industrialiot
manager: philmea
ms.openlocfilehash: 99b3f6b4495ae5981322c90fce251b9b0a18bb5d
ms.sourcegitcommit: f56b267b11f23ac8f6284bb662b38c7a8336e99b
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 06/28/2019
ms.locfileid: "67451491"
---
# <a name="what-is-opc-publisher"></a>Was ist OPC Publisher?

OPC Publisher ist eine Referenzimplementierung, die Folgendes veranschaulicht:

- Herstellen einer Verbindung mit vorhandenen OPC UA-Servern.
- Veröffentlichen von mit JSON codierten Telemetriedaten von OPC UA-Servern im Pub/Sub-Format von OPC UA mithilfe einer JSON-Nutzlast im Azure IoT Hub.

Sie können ein beliebiges Transportprotokoll verwenden, das vom Azure IoT Hub Client SDK unterstützt wird: HTTPS, AMQP und MQTT.

Die Referenzimplementierung umfasst Folgendes:

- Einen OPC UA-*Client*, um eine Verbindung mit vorhandenen OPC UA-Servern in Ihrem Netzwerk herzustellen.
- Einen OPC UA-*Server* an Port 62222, mit dem Sie veröffentlichte Inhalte verwalten können und der direkte IoT Hub-Methoden für dieselben Aufgaben bietet.

Sie können die [Referenzimplementierung für OPC Publisher](https://github.com/Azure/iot-edge-opc-publisher) von GitHub herunterladen.

Die Anwendung wird mithilfe von .NET Core-Technologie implementiert und kann auf allen Plattformen ausgeführt werden, die .NET Core unterstützt.

OPC Publisher implementiert eine Wiederholungslogik, um Verbindungen mit Endpunkten herzustellen, die nicht auf eine bestimmte Anzahl von Keep-Alive-Anforderungen reagieren. Beispielsweise wenn ein OPC UA-Server aufgrund eines Stromausfalls nicht mehr reagiert.

Für jedes eindeutige Veröffentlichungsintervall, das einen OPC UA-Server betrifft, erstellt die Anwendung ein separates Abonnement, in dem alle Knoten mit diesem Veröffentlichungsintervall aktualisiert werden.

OPC Publisher unterstützt die Batchverarbeitung von Daten, die an IoT Hub gesendet werden, um die Netzwerklast zu verringern. Dieser Batchauftrag sendet ein Paket nur dann an IoT Hub, wenn die konfigurierte Paketgröße erreicht ist.

Diese Anwendung verwendet den OPC Foundation OPC UA-Referenzstapel als NuGet-Pakete. Unter [https://opcfoundation.org/license/redistributables/1.3/](https://opcfoundation.org/license/redistributables/1.3/) finden Sie die Lizenzierungsbedingungen.

### <a name="next-steps"></a>Nächste Schritte

Nachdem Sie erfahren haben, was OPC Publisher ist, wird als nächster Schritt das [Konfigurieren von OPC Publisher](howto-opc-publisher-configure.md) vorgeschlagen.
