---
title: Grundlegendes zu Sicherheitsmodulzwillingen für Azure Security Center für IoT | Microsoft-Dokumentation
description: Lernen Sie das Konzept der Sicherheitsmodulzwillinge kennen, und erfahren Sie, wie sie in Azure Security Center für IoT verwendet werden.
services: asc-for-iot
ms.service: asc-for-iot
documentationcenter: na
author: mlottner
manager: rkarlin
editor: ''
ms.assetid: a5c25cba-59a4-488b-abbe-c37ff9b151f9
ms.subservice: asc-for-iot
ms.devlang: na
ms.topic: conceptual
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 07/24/2019
ms.author: mlottner
ms.openlocfilehash: ab3b6e740e644a1ed1495eb776045888be448047
ms.sourcegitcommit: fe6b91c5f287078e4b4c7356e0fa597e78361abe
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 07/29/2019
ms.locfileid: "68596499"
---
# <a name="security-module"></a>Sicherheitsmodul


In diesem Artikel wird erläutert, wie Azure Security Center für IoT Gerätezwillinge und -module verwendet. 

## <a name="device-twins"></a>Gerätezwillinge

Für in Azure erstellte IoT-Lösungen spielen Gerätezwillinge eine wichtige Rolle – sowohl bei der Geräteverwaltung als auch bei der Prozessautomatisierung.  

Azure Security Center für IoT ermöglicht die vollständige Integration in Ihre vorhandene Plattform für die IoT-Geräteverwaltung. Auf diese Weise können Sie Ihren Gerätesicherheitsstatus verwalten und die vorhandenen Funktionen für die Gerätesteuerung nutzen. Die Integration wird erreicht, indem der Mechanismus für IoT Hub-Zwillinge verwendet wird.  

Erfahren Sie mehr über das Konzept der [Gerätezwillinge](https://docs.microsoft.com/azure/iot-hub/iot-hub-devguide-device-twins) in Azure IoT Hub. 

## <a name="security-module-twins"></a>Sicherheitsmodulzwillinge

Azure Security Center für IoT verwaltet für jedes Gerät im Dienst einen Sicherheitsmodulzwilling.
Der Sicherheitsmodulzwilling enthält alle für die Gerätesicherheit relevanten Informationen für jedes einzelne Gerät in Ihrer Lösung.
Die Gerätesicherheitseigenschaften werden in einem dedizierten Sicherheitsmodulzwilling verwaltet, um eine sicherere Kommunikation zu gewährleisten sowie Updates und Wartungen zu ermöglichen, die weniger Ressourcen erfordern.  

Unter [Erstellen eines Sicherheitsmodulzwillings](quickstart-create-security-twin.md) und [Konfigurieren von Sicherheits-Agents](how-to-agent-configuration.md) erfahren Sie, wie Sie den Zwilling erstellen, anpassen und konfigurieren. Weitere Informationen zum Konzept der Modulzwillinge in IoT Hub finden Sie unter [Grundlegendes zu Modulzwillingen](https://docs.microsoft.com/azure/iot-hub/iot-hub-devguide-module-twins). 
 

## <a name="see-also"></a>Weitere Informationen
- [Übersicht über Azure Security Center für IoT](overview.md)
- [Bereitstellen von Sicherheits-Agents](how-to-deploy-agent.md)
- [Sicherheits-Agent-Authentifizierungsmethoden](concept-security-agent-authentication-methods.md)