---
title: Grundlegendes zum Azure Security Center für das IoT-Sicherheitsmodul für IoT Edge | Microsoft-Dokumentation
description: Grundlegendes zur Architektur und den Funktionen des Azure Security Center für IoT-Sicherheitsmoduls für IoT Edge.
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
ms.date: 07/23/2019
ms.author: mlottner
ms.openlocfilehash: 148c68234a937efde554ef00a6014cdc1a350f34
ms.sourcegitcommit: 9fba13cdfce9d03d202ada4a764e574a51691dcd
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 09/26/2019
ms.locfileid: "71315888"
---
# <a name="azure-iot-edge-security-module"></a>Azure IoT Edge-Sicherheitsmodul

[Azure IoT Edge](https://docs.microsoft.com/azure/iot-edge/) umfasst leistungsstarke Funktionen zum Verwalten und Ausführen von Geschäftsworkflows am Edge.
Seine Schlüsselrolle in IoT-Umgebungen macht IoT Edge zu einem besonders attraktiven Ziel für böswillige Akteure.

Das Azure Security Center für IoT-Sicherheitsmodul bietet eine umfassende Sicherheitslösung für Ihre IoT Edge-Geräte.
Das Azure Security Center für IoT-Modul sammelt, aggregiert und analysiert Sicherheitsrohdaten von Ihrem Betriebs- und Containersystem und gibt verwertbare Sicherheitsempfehlungen und -warnungen aus.

Ähnlich wie die Azure Security Center für IoT-Sicherheits-Agents für IoT-Geräte ist das Azure Security Center für IoT Edge-Modul durch seinen Modulzwilling hochgradig anpassbar.
Weitere Informationen finden Sie unter [Konfigurieren Ihres Agents](how-to-agent-configuration.md).

Das Azure Security Center für IoT-Sicherheitsmodul für IoT Edge umfasst die folgenden Funktionen:

- Sammeln von Sicherheitsrohereignissen vom zugrunde liegenden Betriebssystem (Linux) und den IoT Edge-Containersystemen
  
  Weitere Informationen zu verfügbaren Sicherheitsdatensammlern finden Sie unter [Konfiguration für den Azure Security Center für IoT-Agent](how-to-agent-configuration.md).

- Analyse von IoT Edge-Bereitstellungsmanifesten

- Aggregieren von Sicherheitsrohereignissen in Meldungen, die über [IoT Edge-Hub](https://docs.microsoft.com/azure/iot-edge/iot-edge-runtime#iot-edge-hub) gesendet werden.

- Entfernen der Konfiguration über den Sicherheitsmodulzwilling

  Weitere Informationen finden Sie unter [Konfigurieren eines Azure Security Center für IoT-Agents](how-to-agent-configuration.md).

Das Azure Security Center für IoT-Sicherheitsmodul für IoT Edge wird im privilegierten Modus unter IoT Edge ausgeführt.
Der privilegierte Modus ist erforderlich, damit das Modul das Betriebssystem und andere IoT Edge-Module überwachen kann.

## <a name="module-supported-platforms"></a>Vom Modul unterstützte Plattformen

Das Azure Security Center für IoT-Sicherheitsmodul für IoT Edge ist derzeit nur für Linux verfügbar. 

## <a name="next-steps"></a>Nächste Schritte

In diesem Artikel haben Sie Informationen zur Architektur und zu den Funktionen des Azure Security Center für IoT-Sicherheitsmoduls für IoT Edge erhalten.

Wenn Sie die ersten Schritte mit der Azure Security Center für IoT-Bereitstellung fortsetzen möchten, lesen Sie die folgenden Artikel:

- Bereitstellen des [Sicherheitsmoduls für IoT Edge](how-to-deploy-edge.md)
- Erfahren Sie, wie Sie [Ihr Sicherheitsmodul konfigurieren](how-to-agent-configuration.md).
- Überprüfen der [Dienstvoraussetzungen](service-prerequisites.md) für Azure Security Center für IoT
- Informationen zum [Aktivieren des Diensts „Azure Security Center für IoT“ in Ihrer IoT Hub-Instanz](quickstart-onboard-iot-hub.md)
- Weitere Informationen zum Dienst finden Sie in [Azure Security Center für IoT – Häufig gestellte Fragen (FAQ)](resources-frequently-asked-questions.md)