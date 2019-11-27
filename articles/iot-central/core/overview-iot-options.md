---
title: Microsoft Azure IoT-Optionen | Microsoft-Dokumentation
description: 'Entscheiden Sie, wie Ihre Azure IoT-Lösung implementiert werden soll: per Azure IoT Central, per IoT Solution Accelerators oder per IoT Hub.'
author: dominicbetts
ms.author: dobett
ms.date: 06/09/2019
ms.topic: overview
ms.service: iot-central
services: iot-central
ms.custom: mvc
manager: timlt
ms.openlocfilehash: 2ec6e1d00d331a7bb8dd7182dd4b0c91dd3d25a7
ms.sourcegitcommit: b1a8f3ab79c605684336c6e9a45ef2334200844b
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 11/13/2019
ms.locfileid: "74049092"
---
# <a name="compare-azure-iot-central-and-azure-iot-options"></a>Vergleich der Optionen von Azure IoT Central und Azure IoT

In Microsoft Azure IoT Central and Azure IoT stehen verschiedene Optionen für die Erstellung einer IoT-Lösung zur Verfügung. Diese Optionen sind für verschiedene Kundenanforderungen geeignet:

* [Azure IoT Central](overview-iot-central.md) ist eine IoT-Anwendungsplattform mit einem modellbasierten Ansatz, der Sie bei der Entwicklung von IoT-Lösungen für Unternehmen unterstützt, ohne dass Sie dazu mit der Entwicklung von Cloudlösungen vertraut sein müssen.

* [Azure IoT Solution Accelerators](https://docs.microsoft.com/azure/iot-accelerators/) sind eine für Unternehmen konzipierte Sammlung mit [Solution Accelerators](../../iot-accelerators/iot-accelerators-what-are-solution-accelerators.md) auf Basis von Azure-PaaS (Platform-as-a-Service), mit denen Sie die Entwicklung von benutzerdefinierten IoT-Lösungen beschleunigen können.

## <a name="azure-iot-hub"></a>Azure IoT Hub

Azure IoT Hub ist die Azure-PaaS-Kernanwendung, die sowohl von Azure IoT Central als auch von Azure IoT Solution Accelerators genutzt wird. IoT Hub unterstützt die zuverlässige und sichere bidirektionale Kommunikation zwischen Millionen von IoT-Geräten und einer Cloudlösung. Mit IoT Hub können Sie beispielsweise folgende Anforderungen der IoT-Implementierung erfüllen:

* Gerätekonnektivität und -verwaltung mit hohem Volumen
* Telemetriedatenerfassung mit hohem Volumen
* Steuerung von Geräten mit Befehlen
* Erzwingung der Gerätesicherheit

## <a name="compare-azure-iot-central-and-azure-iot-solution-accelerators"></a>Vergleich von Azure IoT Central und Azure IoT Solution Accelerators

Die Auswahl Ihres Azure IoT-Produkts ist ein wichtiger Teil der Planung Ihrer IoT-Lösung. IoT Hub ist ein individueller Azure-Dienst, von dem nicht selbst eine End-to-End-IoT-Lösung bereitgestellt wird. IoT Hub kann als Ausgangspunkt für eine beliebige IoT-Lösung verwendet werden. Darüber hinaus kann IoT Hub ohne Azure IoT Solution Accelerators oder Azure IoT Central verwendet werden. Sowohl von IoT Solution Accelerators als auch von Azure IoT Central wird eine Kombination aus IoT Hub und anderen Azure-Diensten genutzt.

In der folgenden Tabelle sind die wichtigsten Unterschiede zwischen IoT Solution Accelerators und Azure IoT Central zusammengefasst, damit Sie die richtige Wahl für Ihre Anforderungen treffen können:

|     | Azure IoT Central | Azure IoT Solution Accelerators |
| --- | ----------- | --------- |
| Primäre Nutzung                      | Verkürzen des Zeitraums bis zur Markteinführung für einfach aufgebaute IoT-Lösungen, für die keine eingehende Dienstanpassung erforderlich ist.                                                    | Beschleunigen der Entwicklung einer benutzerdefinierten IoT-Lösung, für die eine maximale Flexibilität erforderlich ist.                                                                                                                             |
| Zugriff auf zugrunde liegende PaaS-Dienste | Da es sich um eine vollständig verwaltete Lösung handelt, werden die zugrunde liegenden Dienste nicht verfügbar gemacht.                                                                                            | Sie haben Zugriff auf die zugrunde liegenden Azure-Dienste, um diese zu verwalten oder je nach Bedarf zu ersetzen.                                                                                                                    |
| Flexibilität                        | Mittel. Sie können die integrierte browserbasierte Benutzeroberfläche verwenden, um das Lösungsmodell und die Aspekte der Benutzeroberfläche anzupassen. Die Infrastruktur kann nicht angepasst werden, da die unterschiedlichen Komponenten nicht verfügbar gemacht werden. | Hoch. Der Code für die Microservices ist Open-Source-Code, den Sie beliebig ändern können. Außerdem können Sie die Bereitstellungsinfrastruktur anpassen.                                               |
| Kenntnisstand                        | Niedrig. Sie benötigen Modellierungskenntnisse, um die Lösung anzupassen. Es sind keine Codierungskenntnisse erforderlich.                                                                          | Mittelhoch. Sie benötigen Java- oder .NET-Kenntnisse, um das Back-End der Lösung anzupassen. Sie benötigen JavaScript-Kenntnisse, um die Visualisierung anzupassen.                                                                       |
| Erste Schritte             | Über Anwendungs- und Gerätevorlagen werden vorgefertigte Modelle bereitgestellt. Die Bereitstellung dauert nur wenige Minuten.                                                                                                  | Vorkonfigurierte Lösungen ermöglichen die Implementierung von häufigen IoT-Szenarien. Die Bereitstellung dauert nur wenige Minuten.                                                                                                                            |
| Preise                            | Einfache vorhersagbare Preisstruktur.                                                                                                                           | Sie können die Dienste optimieren, um die Kosten zu kontrollieren.                                                                                                                                                            |

Welches Produkt Sie für die Erstellung Ihrer IoT-Lösung verwenden, hängt von folgenden Faktoren ab:

* Den Anforderungen Ihres Unternehmens
* Der Art der zu erstellenden Lösung
* Der Fähigkeit Ihrer Organisation, die Lösung zu erstellen und langfristig zu warten

## <a name="next-steps"></a>Nächste Schritte

Je nach gewähltem Produkt und Ansatz sind die folgenden nächsten Schritte zu empfehlen:

* **Azure IoT Central**: [Was ist Azure IoT Central?](overview-iot-central.md)
* **IoT Solution Accelerators**: [Was sind Azure IoT Solution Accelerators?](../../iot-accelerators/iot-accelerators-what-are-solution-accelerators.md)
* **IoT Hub**: [Was ist Azure IoT Hub?](https://docs.microsoft.com/azure/iot-hub/iot-hub-what-is-iot-hub)