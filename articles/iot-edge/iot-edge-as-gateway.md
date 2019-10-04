---
title: Gateways für nachgeschaltete Geräte – Azure IoT Edge | Microsoft-Dokumentation
description: Verwenden Sie Azure IoT Edge zum Erstellen eines transparenten oder nicht transparenten Gateway-Geräts oder eines Proxygatewaygeräts, das Daten von mehreren nachgeschalteten Geräten in die Cloud sendet oder lokal verarbeitet.
author: kgremban
manager: philmea
ms.author: kgremban
ms.date: 02/25/2019
ms.topic: conceptual
ms.service: iot-edge
services: iot-edge
ms.custom: seodec18
ms.openlocfilehash: c3a49c4333838652f7063d6a89cfd8cceace1cf8
ms.sourcegitcommit: 41ca82b5f95d2e07b0c7f9025b912daf0ab21909
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 06/13/2019
ms.locfileid: "67054197"
---
# <a name="how-an-iot-edge-device-can-be-used-as-a-gateway"></a>Verwendung eines IoT Edge-Geräts als Gateway

Gateways in IoT Edge-Lösungen bieten Gerätekonnektivität und Edgeanalysen für IoT-Geräte, die sonst nicht über diese Funktionen verfügen würden. Mit Azure IoT Edge lassen sich alle Anforderungen an ein IoT-Gateway im Bezug auf Konnektivität, Identität und Edgeanalysen erfüllen. Gatewaymuster in diesem Artikel beziehen sich nur auf Merkmale der Konnektivität und Identität nachgeschalteter Geräte, nicht auf die Verarbeitung von Gerätedaten auf dem Gateway.

## <a name="patterns"></a>Muster

Es gibt drei Muster für die Verwendung eines IoT Edge-Geräts als Gateway: transparent, Protokollübersetzung und Identitätsübersetzung:
* **Transparent**: Geräte, die theoretisch eine Verbindung mit dem IoT Hub herstellen könnten, können stattdessen eine Verbindung mit einem Gatewaygerät herstellen. Die nachgeschalteten Geräte verfügen über ihre eigenen IoT Hub-Identitäten und verwenden MQTT-, AMQP- oder HTTP-Protokolle. Das Gateway übergibt einfach die Kommunikation zwischen den Geräten und IoT Hub. Die Geräte erkennen nicht, dass sie über ein Gateway mit der Cloud kommunizieren, und auch ein Benutzer, der mit den Geräten in IoT Hub interagiert, weiß nichts vom zwischengeschalteten Gatewaygerät. Daher ist das Gateway transparent. Einzelheiten zum Verwenden eines IoT Edge-Geräts als transparentes Gateway finden Sie unter [Erstellen eines transparenten Gateways](how-to-create-transparent-gateway.md).
* **Protokollübersetzung** (auch „transparentes Gatewaymuster“): Geräte, die MQTT, AMQP oder HTTP nicht unterstützen, können Daten über ein Gatewaygerät selbst an IoT Hub senden. Das Gateway versteht das Protokoll, das von den nachgeschalteten Geräten verwendet wird, und ist das einzige Gerät, das eine Identität in IoT Hub hat. Alle Informationen scheinen von einem Gerät – dem Gateway – zu stammen. Nachgeschaltete Geräte müssen zusätzliche identifizierende Informationen in ihre Nachrichten einbetten, wenn Cloudanwendungen die Daten pro Gerät analysieren möchten. Außerdem sind IoT Hub-Grundtypen wie Zwillinge und Methoden nur für das Gatewaygerät und nicht für nachgeschaltete Geräte verfügbar.
* **Identitätsübersetzung**: Geräte, die keine Verbindung mit IoT Hub herstellen können, können stattdessen eine Verbindung mit einem Gatewaygerät herstellen. Das Gateway stellt die IoT Hub-Identitäts- und -Protokollübersetzungen im Auftrag der nachgeschalteten Geräte bereit. Das Gateway ist intelligent genug, um das von den nachgeschalteten Geräten verwendete Protokoll zu verstehen, ihnen Identität bereitzustellen und IoT Hub-Grundtypen zu übersetzen. Nachgeschaltete Geräte werden in IoT Hub als erstrangige Geräte mit Zwillingen und Methoden angezeigt. Ein Benutzer kann mit den Geräten in IoT Hub interagieren, weiß jedoch nichts vom zwischengeschalteten Gatewaygerät.

![Diagramm – Transparente, Protokoll- und Identitätsgatewaymuster](./media/iot-edge-as-gateway/edge-as-gateway.png)

## <a name="use-cases"></a>Anwendungsfälle
Alle Gatewaymuster bieten folgende Vorteile:
* **Edgeanalysen**: Verwenden Sie lokal KI-Dienste zum Verarbeiten von Daten, die von nachgeschalteten Geräten stammen, ohne vollständige Telemetriedaten in die Cloud zu senden. Suchen und reagieren Sie lokal auf Informationen, senden Sie nur eine Teilmenge von Daten an IoT Hub. 
* **Isolierung von nachgeschalteten Geräten**: Das Gatewaygerät kann alle nachgeschalteten Geräte vor dem Zugriff über das Internet schützen. Es kann sich zwischen einem OT-Netzwerk ohne Konnektivität und einem IT-Netzwerk mit Internetzugriff befinden. 
* **Multiplexing der Verbindung**: Alle Geräte, die über ein IoT Edge-Gateway mit IoT Hub verbunden sind, verwenden dieselbe zugrunde liegende Verbindung.
* **Glättung des Datenverkehrs**: Das IoT Edge-Gerät implementiert automatisch exponentielles Backoff, wenn IoT Hub den Datenverkehr drosselt, wobei die Nachrichten lokal beibehalten werden. Durch diesen Vorteil wird Ihre Lösung unempfindlich gegenüber Spitzen im Datenverkehr.
* **Offlineunterstützung**: Das Gatewaygerät speichert Nachrichten und Zwillingsupdates, die nicht an IoT Hub übermittelt werden können.

Ein Gateway, das Protokolle übersetzt, kann auch Edgeanalysen, Geräteisolierung sowie eine Glättung des Datenverkehrs ausführen und bietet Offlineunterstützung für vorhandene Geräte und neue Geräte mit eingeschränkten Ressourcen. Zahlreiche vorhandene Geräte erzeugen Daten, aus denen geschäftliche Erkenntnissen gewonnen werden können; sie wurden jedoch nicht speziell für Cloudkonnektivität konzipiert. Nicht transparente Gateways ermöglichen das Entsperren und Verwenden dieser Daten in einer IoT-Lösung.

Ein Gateway, das Identitätsübersetzungen vornimmt, bietet die Vorteile der Protokollübersetzung und ermöglicht außerdem die vollständige Verwaltbarkeit von nachgeschalteten Geräten über die Cloud. Alle Geräte in Ihrer IoT-Lösung werden unabhängig vom jeweiligen Protokoll in IoT Hub angezeigt.

## <a name="cheat-sheet"></a>Cheat Sheet
Hier finden Sie ein schnelles Cheat Sheet, das IoT Hub-Grundtypen bei Verwendung von transparenten, nicht transparenten (Protokoll) und Proxy-Gateways vergleicht.

| &nbsp; | Transparentes Gateway | Protokollübersetzung | Identitätsübersetzung |
|--------|-------------|--------|--------|
| In der IoT Hub-Identitätsregistrierung gespeicherte Identitäten | Identitäten aller verbundenen Geräte | Nur die Identität des Gatewaygeräts | Identitäten aller verbundenen Geräte |
| Gerätezwilling | Jedes verbundene Gerät hat einen eigenen Gerätezwilling | Nur das Gateway hat einen Geräte- und Modulzwilling | Jedes verbundene Gerät hat einen eigenen Gerätezwilling |
| Direkte Methoden und C2D-Nachrichten | Die Cloud kann jedes verbundene Gerät einzeln adressieren | Die Cloud kann nur das Gatewaygerät adressieren | Die Cloud kann jedes verbundene Gerät einzeln adressieren |
| [IoT Hub-Drosselungen und -Kontingente](../iot-hub/iot-hub-devguide-quotas-throttling.md) | Gelten für jedes Gerät | Gelten für das Gatewaygerät | Gelten für jedes Gerät |

Bei Verwendung eines nicht transparenten Gatewaymusters (Protokollübersetzung) verwenden alle Geräte, die eine Verbindung über dieses Gateway herstellen, dieselbe C2D-Warteschlange, die höchstens 50 Nachrichten enthalten kann. Daraus folgt, dass das nicht transparente Gatewaymuster nur verwendet werden sollte, wenn wenige Geräte eine Verbindung über die einzelnen Bereichsgateways herstellen und der Cloud-zu-Gerät-Datenverkehr gering ist.

## <a name="next-steps"></a>Nächste Schritte

Erfahren Sie, wie Sie ein transparentes Gateway einrichten: 

* [Konfigurieren eines IoT Edge-Geräts als transparentes Gateway](how-to-create-transparent-gateway.md)
* [Authentifizieren eines nachgeschalteten Geräts bei Azure IoT Hub](how-to-authenticate-downstream-device.md)
* [Verbinden eines nachgeschalteten Geräts mit einem Azure IoT Edge-Gateway](how-to-connect-downstream-device.md)