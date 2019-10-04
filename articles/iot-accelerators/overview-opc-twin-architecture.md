---
title: OPC Twin-Architektur – Azure | Microsoft-Dokumentation
description: OPC Twin-Architektur
author: dominicbetts
ms.author: dobett
ms.date: 11/26/2018
ms.topic: overview
ms.service: industrial-iot
services: iot-industrialiot
manager: philmea
ms.openlocfilehash: 6ce9394f3d454bda5ead51f2c77a47db137a5136
ms.sourcegitcommit: f10ae7078e477531af5b61a7fe64ab0e389830e8
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 07/05/2019
ms.locfileid: "67606177"
---
# <a name="opc-twin-architecture"></a>OPC Twin-Architektur

In den folgenden Diagrammen wird die OPC Twin-Architektur veranschaulicht.

## <a name="discover-and-activate"></a>Ermitteln und Aktivieren

1. Der Bediener aktiviert eine Netzwerküberprüfung im Modul oder führt per Ermittlungs-URL eine einmalige Ermittlung durch. Die ermittelten Endpunkte und die Anwendungsinformationen werden per Telemetriefunktion zur Verarbeitung an den Onboarding-Agent gesendet.  Der Onboarding-Agent für OPC UA-Geräte verarbeitet OPC UA-Serverermittlungsereignisse, die vom OPC Twin-IoT Edge-Modul im Ermittlungs- oder Überprüfungsmodus gesendet werden. Die Ermittlungsergebnisse führen zur Anwendungsregistrierung und zu Updates in der OPC UA-Geräteregistrierung.

   ![Funktionsweise von OPC Twin](media/overview-opc-twin-architecture/opc-twin1.png)

1. Der Bediener untersucht das Zertifikat des ermittelten Endpunkts und aktiviert den registrierten Endpunktzwilling für den Zugriff. 

   ![Funktionsweise von OPC Twin](media/overview-opc-twin-architecture/opc-twin2.png)

## <a name="browse-and-monitor"></a>Durchsuchen und Überwachen

1. Nach der Aktivierung kann der Bediener die Twin-Dienst-REST-API verwenden, um das Serverinformationsmodell zu durchsuchen und zu untersuchen, Objektvariablen zu lesen bzw. zu schreiben und Methoden aufzurufen.  Der Benutzer verwendet eine vereinfachte OPC UA-API, die vollständig in HTTP und JSON ausgedrückt wird.

   ![Funktionsweise von OPC Twin](media/overview-opc-twin-architecture/opc-twin3.png)

1. Die REST-Schnittstelle des Twin-Diensts kann auch zum Erstellen von überwachten Elementen und Abonnements in OPC Publisher verwendet werden. Mit OPC Publisher können Telemetriedaten von OPC UA-Serversystemen an IoT Hub gesendet werden. Weitere Informationen zu OPC Publisher finden Sie unter [Was ist OPC Publisher?](overview-opc-publisher.md).

   ![Funktionsweise von OPC Twin](media/overview-opc-twin-architecture/opc-twin4.png)
