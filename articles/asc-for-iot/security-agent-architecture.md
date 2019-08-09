---
title: Grundlegendes zur Sicherheits-Agent-Architektur für Azure Security Center für IoT | Microsoft-Dokumentation
description: Grundlegendes zur Sicherheits-Agent-Architektur für die im Azure Security Center für IoT-Dienst verwendeten Agents.
services: asc-for-iot
ms.service: asc-for-iot
documentationcenter: na
author: mlottner
manager: rkarlin
editor: ''
ms.assetid: e78523ae-d70a-456a-818d-f8b1b025d7cb
ms.subservice: asc-for-iot
ms.devlang: na
ms.topic: conceptual
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 07/26/2019
ms.author: mlottner
ms.openlocfilehash: 998aeab197931a75579fc39b28e3a248b85fc57b
ms.sourcegitcommit: fe6b91c5f287078e4b4c7356e0fa597e78361abe
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 07/29/2019
ms.locfileid: "68596906"
---
# <a name="security-agent-reference-architecture"></a>Sicherheits-Agent-Referenzarchitektur

Azure Security Center für IoT bietet eine Referenzarchitektur für Sicherheits-Agents, die Sicherheitsdaten über IoT Hub protokollieren, verarbeiten, aggregieren und senden.

Sicherheits-Agents sind für den Einsatz in einer eingeschränkten IoT-Umgebung konzipiert und können in Bezug auf die Werte, die sie im Vergleich zu den von ihnen verbrauchten Ressourcen bereitstellen, sehr flexibel angepasst werden.

Sicherheits-Agents unterstützen die folgenden Features:

- Sammeln von Rohdaten zu Sicherheitsereignissen aus dem zugrunde liegenden Betriebssystem (Linux, Windows). Weitere Informationen zu verfügbaren Sicherheitsdatensammlern finden Sie unter [Konfiguration für den Azure Security Center für IoT-Agent](how-to-agent-configuration.md).

- Aggregieren von Rohdaten zu Sicherheitsereignissen in Nachrichten, die über IoT Hub gesendet werden.

- Authentifizieren mit vorhandener Geräteidentität oder einer dedizierten Modulidentität. Weitere Informationen finden Sie unter [Sicherheits-Agent-Authentifizierungsmethoden](concept-security-agent-authentication-methods.md).

- Remotekonfigurieren durch Verwendung des Modulzwillings **azureiotsecurity**. Weitere Informationen finden Sie unter [Konfigurieren eines Azure Security Center für IoT-Agents](how-to-agent-configuration.md).

Azure Security Center für IoT-Sicherheits-Agents werden als Open-Source-Projekte entwickelt und sind über GitHub verfügbar: 

- [C-basierter Azure Security Center für IoT-Agent](https://github.com/Azure/Azure-IoT-Security-Agent-C) 
- [C#-basierter Azure Security Center für IoT-Agent](https://github.com/Azure/Azure-IoT-Security-Agent-CS)

## <a name="agent-supported-platforms"></a>Vom Agent unterstützte Plattformen

Azure Security Center für IoT bietet verschiedene Installer-Agents für 32-Bit- und 64-Bit-Windows sowie für 32-Bit- und 64-Bit-Linux. Stellen Sie anhand der folgenden Tabelle sicher, dass Sie den richtigen Agent-Installer für jedes Ihrer Geräte verwenden:

| Architecture | Linux | Windows |    Details|
|----------|----------------------------------------------|-------------|-------------------------------------------|
| 32 Bit  | C  | C#  ||
| 64 Bit  | C# oder C           | C#      | Wir empfehlen, den C-Agent für Geräte mit eingeschränkteren oder minimalen Geräteressourcen zu verwenden.|
|

## <a name="next-steps"></a>Nächste Schritte

In diesem Artikel haben Sie die Sicherheits-Agent-Architektur für Azure Security Center für IoT und die verfügbaren Installer kennen gelernt.

Wenn Sie die ersten Schritte mit der Azure Security Center für IoT-Bereitstellung fortsetzen möchten, lesen Sie die folgenden Artikel:

- Grundlegendes zu den [Authentifizierungsmethoden des Sicherheits-Agents](concept-security-agent-authentication-methods.md)
- Wählen Sie einen [Sicherheits-Agent](how-to-deploy-agent.md) aus, und stellen Sie ihn bereit.
- Überprüfen der [Dienstvoraussetzungen](service-prerequisites.md) für Azure Security Center für IoT
- Informationen zum [Aktivieren des Diensts „Azure Security Center für IoT“ in Ihrer IoT Hub-Instanz](quickstart-onboard-iot-hub.md)
- Weitere Informationen zum Dienst in [Azure Security Center für IoT – Häufig gestellte Fragen (FAQ)](resources-frequently-asked-questions.md)
