---
title: Erste Schritte mit der Verwendung der Azure Security Center für IoT-Sicherheits-Agents | Microsoft-Dokumentation
description: Informieren Sie sich über die ersten Schritte mit Azure Security Center für IoT-Sicherheits-Agents.
services: asc-for-iot
ms.service: asc-for-iot
documentationcenter: na
author: mlottner
manager: rkarlin
editor: ''
ms.assetid: 55c8d3b6-3126-4246-8d07-ef88fe5ea84f
ms.subservice: asc-for-iot
ms.devlang: na
ms.topic: conceptual
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 07/23/2019
ms.author: mlottner
ms.openlocfilehash: 6e790c125c59aea85f1ac34240c5a1d1969544ae
ms.sourcegitcommit: fe6b91c5f287078e4b4c7356e0fa597e78361abe
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 07/29/2019
ms.locfileid: "68600325"
---
# <a name="get-started-with-azure-security-center-for-iot-device-security-agents"></a>Erste Schritte mit Azure Security Center für IoT-Agents für Gerätesicherheit

Azure Security Center für IoT-Sicherheits-Agents bieten erweiterte Sicherheitsfunktionen, wie z.B. das Überwachen von Remoteverbindungen, aktiven Anwendungen, Anmeldeereignissen und bewährten Methoden für die Betriebssystemkonfiguration. Übernehmen Sie mit einem einzigen Dienst die Kontrolle über den Bedrohungsschutz und den Sicherheitsstatus Ihres Gerätefelds. 

Sie erhalten eine Referenzarchitektur für Linux- und Windows-Sicherheits-Agents, die sowohl in C# als auch in C verfügbar ist.

Die Azure Security Center für IoT-Sicherheits-Agents verarbeiten unformatierte Ereigniserfassungen aus dem Betriebssystem des Geräts, ermöglichen eine Ereignisaggregation zur Kostensenkung und erlauben die Konfiguration über einen Modulzwilling des Geräts. Sicherheitsmeldungen werden über Ihre IoT Hub-Instanz an die Analysedienste von Azure Security Center für IoT gesendet.

Verwenden Sie den folgenden Workflow, um Ihre Azure Security Center für IoT-Sicherheits-Agents bereitzustellen und zu testen: 

1. [Aktivieren des Diensts „Azure Security Center für IoT“ in Ihrer IoT Hub-Instanz](quickstart-onboard-iot-hub.md)
1. Wenn für Ihre IoT Hub-Instanz keine Geräte registriert sind, [registrieren Sie ein neues Gerät](https://docs.microsoft.com/azure/iot-accelerators/quickstart-device-simulation-deploy).
1. [Erstellen Sie ein azureiotsecurity-Sicherheitsmodul](quickstart-create-security-twin.md) für Ihre Geräte.
1. Um den Agent auf einem simulierten Azure-Gerät anstatt auf einem echten Gerät zu installieren, [starten Sie eine neue Azure-VM](https://docs.microsoft.com/azure/virtual-machines/linux/quick-create-portal) in einer verfügbaren Zone. 
1. [Stellen Sie einen Azure Security Center für IoT-Sicherheits-Agent bereit](how-to-deploy-linux-cs.md) – auf Ihrem IoT-Gerät oder einer neuen VM.
1. Folgen Sie den Anweisungen für [trigger_events](https://aka.ms/iot-security-github-trigger-events), um eine harmlose Simulation eines Angriffs auszuführen.
1. Überprüfen Sie die Benachrichtigungen von Azure Security Center für IoT als Reaktion auf den im vorherigen Schritt simulierten Angriff. Beginnen Sie mit der Überprüfung erst fünf Minuten, nachdem Sie das Skript ausgeführt haben.
1. Gehen Sie [Benachrichtigungen](concept-security-alerts.md), [Empfehlungen](concept-recommendations.md) und die [ausführlichen Informationen zu Log Analytics](how-to-security-data-access.md) mit IoT Hub durch. 


## <a name="next-steps"></a>Nächste Schritte

- [Quickstart: Configure your IoT solution](quickstart-configure-your-solution.md) (Schnellstart: Konfigurieren Ihrer IoT-Lösung)
- [Quickstart: Create an azureiotsecurity module twin](quickstart-create-security-twin.md) (Schnellstart: Erstellen eines azureiotsecurity-Modulzwillings)
- [Quickstart: Create custom alerts](quickstart-create-custom-alerts.md) (Schnellstart: Erstellen benutzerdefinierter Benachrichtigungen)
- [Select and deploy a security agent on your IoT device](how-to-deploy-agent.md) (Auswählen und Bereitstellen eines Sicherheits-Agents auf Ihrem IoT-Gerät)
