---
title: Was ist OPC Publisher? – Azure | Microsoft-Dokumentation
description: Dieser Artikel enthält eine Übersicht über die Features von Azure OPC Publisher. Sie ermöglicht Ihnen das Veröffentlichen von codierten JSON-Telemetriedaten mithilfe einer JSON-Nutzlast auf Azure IoT Hub.
author: dominicbetts
ms.author: dobett
ms.date: 06/10/2019
ms.topic: overview
ms.service: industrial-iot
services: iot-industrialiot
manager: philmea
ms.openlocfilehash: 2310a81d3cfaeff203134af9968bc2d5caea3e9c
ms.sourcegitcommit: ac56ef07d86328c40fed5b5792a6a02698926c2d
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 11/08/2019
ms.locfileid: "73824777"
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
