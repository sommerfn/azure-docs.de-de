---
title: Einführung in IoT Plug & Play (Vorschauversion) | Microsoft-Dokumentation
description: Hier finden Sie Informationen zu IoT Plug & Play (Vorschauversion). IoT Plug & Play beruht auf einer offenen Modelliersprache, mit der IoT-Geräte ihre Funktionen deklarieren können. IoT-Geräte geben diese als Gerätefunktionsmodell bezeichnete Deklaration an, wenn sie eine Verbindung mit Cloudlösungen wie Azure IoT Central oder Partneranwendungen herstellen. Die Cloudlösung kann das Gerät dann automatisch verstehen und mit der Interaktion beginnen – ohne dass Sie Code schreiben müssen.
author: ChrisGMsft
ms.author: chrisgre
ms.date: 06/21/2019
ms.topic: overview
ms.custom: mvc
ms.service: iot-pnp
services: iot-pnp
manager: philmea
ms.openlocfilehash: fd1479a224255f74f4d47cfd90576a5afd25010f
ms.sourcegitcommit: 4c3d6c2657ae714f4a042f2c078cf1b0ad20b3a4
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 10/25/2019
ms.locfileid: "72935393"
---
# <a name="what-is-iot-plug-and-play-preview"></a>Was ist IoT Plug & Play (Vorschauversion)?

Mit IoT Plug & Play (Vorschauversion) können Lösungsentwickler Geräte in ihre Lösungen integrieren, ohne eingebetteten Code schreiben zu müssen. Das Kernstück von IoT Plug & Play ist ein _Gerätefunktionsmodell_-Schema, das Gerätefunktionen beschreibt. Bei diesem Schema handelt es sich um ein JSON-Dokument, das als eine Gruppe von Schnittstellen mit den folgenden Definitionen strukturiert ist:

- _Eigenschaften_, die den schreibgeschützten Zustand und Lese-/Schreibzustand eines Geräts oder einer anderen Entität darstellen. Beispielsweise kann eine Geräteseriennummer eine schreibgeschützte Eigenschaft sein und die Zieltemperatur eines Thermostats eine Lese-/Schreibeigenschaft.
- _Telemetriedaten_, d. h. die von einem Gerät ausgegebenen Daten, ob es sich bei den Daten um einen regulären Datenstrom von Sensormesswerten, einen gelegentlichen Fehler oder eine Informationsmeldung handelt.
- _Befehle_, die eine Funktion oder einen Vorgang beschreiben, die bzw. der auf einem Gerät ausgeführt werden kann. Durch einen Befehl kann beispielsweise ein Gateway neu gestartet oder ein Foto mit einer Remotekamera aufgenommen werden.

Sie können Schnittstellen in verschiedenen Gerätefunktionsmodellen wieder verwenden, um die Zusammenarbeit zu vereinfachen und die Entwicklung zu beschleunigen.

Damit IoT Plug & Play nahtlos mit [Azure Digital Twins](../digital-twins/about-digital-twins.md) funktioniert, wird das IoT Plug & Play-Schema mithilfe der [Digital Twin Definition Language (DTDL)](https://github.com/Azure/IoTPlugandPlay/tree/master/DTDL) definiert. IoT Plug & Play und die DTDL sind für die Community freigegeben, und Microsoft begrüßt die Zusammenarbeit mit Kunden, Partnern und der Branche. Beide Technologien basieren auf offenen W3C-Standards (World Wide Web Consortium) wie JSON-LD und RDF, die eine einfachere Nutzung in verschiedenen Diensten und Tools ermöglichen. Zudem fallen für die Verwendung von IoT Plug & Play und der DTDL keine zusätzlichen Kosten an. Für [Azure IoT Hub](../iot-hub/about-iot-hub.md), [Azure IoT Central](../iot-central/core/overview-iot-central.md) und andere Azure-Dienste gelten weiterhin die Standardtarife.

Auf IoT Hub oder IoT Central beruhende Lösungen können von IoT Plug & Play profitieren.

In diesem Artikel wird Folgendes beschrieben:

- Die typischen Rollen bei einem Projekt mit IoT Plug & Play
- Verwenden von IoT Plug & Play-Geräten in Ihrer Anwendung
- Entwickeln einer IoT-Geräteanwendung, die IoT Plug & Play unterstützt
- Zertifizieren eines IoT Plug & Play-Geräts und Veröffentlichen des Geräts im [Certified for IoT-Gerätekatalog](https://catalog.azureiotsolutions.com/)

## <a name="user-roles"></a>Benutzerrollen

IoT Plug & Play ist für zwei Arten von Entwicklern nützlich:

- Ein _Lösungsentwickler_ ist für das Entwickeln einer IoT-Lösung mit Azure IoT und anderen Azure-Ressourcen sowie für das Ermitteln der zu integrierenden IoT-Geräte verantwortlich.
- Ein _Geräteentwickler_ erstellt den Code, der auf einem mit der Lösung verbundenen Gerät ausgeführt wird.

## <a name="use-iot-plug-and-play-devices"></a>Verwenden von IoT Plug & Play-Geräten

Als Lösungsentwickler können Sie eine in der Cloud gehostete IoT-Lösung entwickeln, die IoT Plug & Play-Geräte nutzt. Dazu können Sie einen der folgenden Azure-Dienste verwenden:

- [IoT Central](../iot-central/core/overview-iot-central.md) ist eine vollständig verwaltete IoT-SaaS-Lösung (Software-as-a-Service) zur einfachen Erstellung von Produkten, die die physische mit der digitalen Welt verbinden.
- [IoT Hub](../iot-hub/about-iot-hub.md) ist ein verwalteter Clouddienst, der als Nachrichtenhub für die sichere, bidirektionale Kommunikation zwischen Ihrer IoT-Anwendung und Ihren Geräten fungiert.

IoT Plug & Play-Geräte finden Sie im Azure Certified for IoT-Gerätekatalog. Jedes IoT Plug & Play-Gerät im Katalog wurde überprüft und verfügt über ein Gerätefunktionsmodell. Sie können das Gerätefunktionsmodell anzeigen, um die Funktionalität des Geräts zu verstehen, oder das Gerät mithilfe des Modells in Azure IoT Central simulieren.

Wenn Sie eine Verbindung mit einem IoT Plug & Play-Gerät herstellen, können Sie sein Gerätefunktionsmodell, die im Modell enthaltenen Schnittstellen sowie die in diesen Schnittstellen definierten Telemetriedaten, Eigenschaften und Befehle anzeigen.

## <a name="develop-an-iot-device-application"></a>Entwickeln einer IoT-Geräteanwendung

Als Geräteentwickler können Sie ein IoT-Hardwareprodukt entwickeln, das IoT Plug & Play unterstützt. Der Prozess umfasst zwei Hauptschritte:

1. Definieren Sie das Gerätefunktionsmodell und die Schnittstellen. Dazu erstellen Sie mehrere JSON-Dateien, die die Funktionen Ihres Geräts mithilfe der [DTDL](https://github.com/Azure/IoTPlugandPlay/tree/master/DTDL)deklarieren. Ein Gerätefunktionsmodell beschreibt eine komplette Entität (z. B. ein physisches Produkt) und definiert die Schnittstellen, die von dieser Entität implementiert werden. Schnittstellen sind freigegebene Verträge, die die von einem Gerät unterstützten Telemetriedaten, Eigenschaften und Befehle eindeutig identifizieren. Schnittstellen können in verschiedenen Gerätefunktionsmodellen wieder verwendet werden.

1. Erstellen Sie die Gerätesoftware oder -firmware, die die im Gerätefunktionsmodell deklarierten Funktionen und Schnittstellen implementiert. Das Azure IoT SDK enthält APIs zum Implementieren von Gerätefunktionsmodellen.

Das Erweiterungspaket [Azure IoT Tools für VS Code](https://marketplace.visualstudio.com/items?itemName=vsciot-vscode.azure-iot-tools) bietet viele hilfreiche Funktionen. Als Geräteentwickler können Sie eine Erweiterung beispielsweise verwenden, um ein C-Gerüstprojekt aus einem Funktionsmodell zu generieren. Sie können Gerätefunktionsmodelle jedoch mit jeder integrierten Entwicklungsumgebung (Integrated Development Environment, IDE) erstellen und implementieren.

## <a name="certify-an-iot-plug-and-play-device"></a>Zertifizieren eines IoT Plug & Play-Geräts

Als Geräteentwickler können Sie IoT-Hardwareprodukte zur Zertifizierung übermitteln. Sie können ein zertifiziertes Gerät im Certified for IoT-Gerätekatalog veröffentlichen. Der Zertifizierungsprozess umfasst folgende Schritte:

- Mitglied des [Microsoft Partner Network](https://partner.microsoft.com) werden
- Onboarding beim Azure Certified for IoT-Portal durchführen
- IoT Plug & Play-Gerätefunktionsmodell und Marketinginformationen übermitteln, um einen neuen Gerätedatensatz zu erstellen
- Automatisierte Validierungstests für das Gerät erfolgreich abschließen
- Gerät im Certified for IoT-Gerätekatalog veröffentlichen

## <a name="regional-availability"></a>Regionale Verfügbarkeit

Während der Public Preview-Phase ist IoT Plug & Play in den Regionen „Europa, Norden“, „USA, Mitte“ und „Japan, Osten“ verfügbar. Achten Sie darauf, dass Sie Ihren Hub in einer dieser Regionen erstellen.

## <a name="message-quotas-in-iot-hub"></a>Nachrichtenkontingente in IoT Hub
Während der Public Preview-Phase senden IoT Plug & Play-Geräte separate Nachrichten pro Schnittstelle, wodurch sich die Anzahl von Nachrichten erhöhen kann, die auf das [Nachrichtenkontingent](../iot-hub/iot-hub-devguide-quotas-throttling.md) angerechnet werden.

## <a name="next-steps"></a>Nächste Schritte

Nachdem Sie sich nun einen Überblick über IoT Plug & Play verschafft haben, empfehlen wir Ihnen als Nächstes einen der folgenden Schnellstarts:

- [Verwenden eines Gerätefunktionsmodells zum Erstellen eines IoT Plug & Play-Geräts](./quickstart-create-pnp-device.md)
- [Verbinden eines Geräts mit IoT Hub](./quickstart-connect-pnp-device.md)
- [Herstellen einer Verbindung mit einem Gerät in Ihrer Lösung](./quickstart-connect-pnp-device-solution.md)
