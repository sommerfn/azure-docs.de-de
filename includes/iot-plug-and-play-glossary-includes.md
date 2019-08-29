---
author: ChrisGMsft
ms.service: iot-pnp
ms.topic: include
ms.date: 06/28/2019
ms.author: chrisgre
ms.openlocfilehash: 7e8174855800bc6beea8750c32a6dd32588ccd9e
ms.sourcegitcommit: b3bad696c2b776d018d9f06b6e27bffaa3c0d9c3
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 08/21/2019
ms.locfileid: "69879202"
---
## <a name="iot-plug-and-play-device"></a>IoT Plug & Play-Gerät

Ein IoT Plug & Play-Gerät ist in der Regel ein kleines, eigenständiges Computergerät, das Daten sammeln oder andere Geräte steuern kann. Auf einem solchen Gerät werden Software- oder Firmwarekomponenten ausgeführt, die diejenigen Funktionen implementieren, die in einem [Gerätefunktionsmodell](#device-capability-model) deklariert sind.  Ein IoT Plug & Play-Gerät kann beispielsweise ein Gerät zur Überwachung von Umweltdaten oder ein Regler für ein intelligentes Bewässerungssystem in der Landwirtschaft sein. Zum Steuern, Überwachen oder Empfangen von Daten von IoT Plug & Play-Geräten können in der Cloud gehostete IoT-Lösungen geschrieben werden. IoT Plug & Play-Geräte finden Sie im [Azure Certified for IoT-Gerätekatalog](https://catalog.azureiotsolutions.com/). Jedes IoT Plug & Play-Gerät im Katalog wurde überprüft und verfügt über ein [Gerätefunktionsmodell](#device-capability-model).

## <a name="digital-twin"></a>Digitaler Zwilling

Digitale Zwillinge sind Modelle von IoT Plug & Play-Geräten.  Digitale Zwillinge werden mithilfe der [Digital Twin Definition Language](https://aka.ms/DTDL) modelliert.  Sie können die Azure-IoT-API verwenden, um mit digitalen Zwillingen zu interagieren. 

## <a name="digital-twin-definition-language"></a>Digital Twin Definition Language

Eine Sprache zum Beschreiben von Modellen und Schnittstellen für [IoT Plug & Play-Geräte](#iot-plug-and-play-device).  Verwenden Sie die [Digital Twin Definition Language](https://aka.ms/DTDL), um die Funktionen eines [digitalen Zwillings](#digital-twin) zu beschreiben und es der IoT-Plattform und den IoT-Lösungen zu ermöglichen, die Semantik der Entität zu nutzen.

## <a name="device-capability-model"></a>Gerätefunktionsmodell

Ein Gerätefunktionsmodell beschreibt ein Gerät und definiert die Schnittstellen, die von diesem Gerät implementiert werden. Sie können ein Gerätefunktionsmodell für ein physisches Gerät, ein Produkt oder eine SKU erstellen.

## <a name="interface"></a>Schnittstelle

Eine Schnittstelle beschreibt verwandte Funktionen, die durch ein Gerät oder einen digitalen Zwilling implementiert werden. Schnittstellen können in verschiedenen Funktionsmodellen wiederverwendet werden.

## <a name="property"></a>Eigenschaft

Eigenschaften sind Datenfelder, die in einer [Schnittstelle](#interface) definiert sind und einen bestimmten Zustand eines digitalen Zwillings repräsentieren. Sie können Eigenschaften als schreibgeschützt oder beschreibbar deklarieren. Schreibgeschützte Eigenschaften werden von Code festgelegt, der im Kontext des IoT Plug & Play-Geräts selbst ausgeführt wird (Beispiel: eine Seriennummer).  Beschreibbare Eigenschaften werden von externen Entitäten festgelegt (Beispiel: Alarmschwellenwerte).

## <a name="telemetry"></a>Telemetrie

In einer [Schnittstelle](#interface) definierte Telemetriefelder stellen Messungen dar. Bei diesen Messungen handelt es sich in der Regel um Werte wie beispielsweise Sensormessungen.

## <a name="command"></a>Get-Help

In einer [Schnittstelle](#interface) definierte Befehle stellen Methoden dar, die auf dem digitalen Zwilling ausgeführt werden können. (Beispiel: Ein reset-Befehl zum Zurücksetzen eines Geräts.)
