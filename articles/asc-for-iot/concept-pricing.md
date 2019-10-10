---
title: Grundlegendes zu den Kosten für Azure Security Center für IoT | Microsoft-Dokumentation
description: Informationen zu den für Azure Security Center für IoT anfallenden Kosten und dazu, wie sie gesteuert werden.
services: asc-for-iot
ms.service: asc-for-iot
documentationcenter: na
author: mlottner
manager: rkarlin
editor: ''
ms.assetid: ef839708-4574-4a40-bc45-07005f8e9daf
ms.subservice: asc-for-iot
ms.devlang: na
ms.topic: conceptual
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 09/27/2019
ms.author: mlottner
ms.openlocfilehash: dc9dcbfd00b5205fa5c66e334b30c76d549d8a42
ms.sourcegitcommit: 7f6d986a60eff2c170172bd8bcb834302bb41f71
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 09/27/2019
ms.locfileid: "71348523"
---
# <a name="pricing-and-associated-costs"></a>Preise und anfallende Kosten

In diesem Artikel wird das Preismodell für Azure Security Center für IoT erläutert. Außerdem werden alle anfallenden Kosten zusammenfasst, und es wird erläutert, wie sie verwaltet werden.

## <a name="pricing"></a>Preise

Das Preismodell für Azure Security Center für IoT besteht aus zwei Teilen und wird in Rechnung gestellt, sobald eine IoT Hub-Instanz in Azure Security Center für IoT [aktiviert](quickstart-onboard-iot-hub.md) wurde:

- Kosten nach Gerät: integrierte Sicherheitsfunktionen basierend auf der Analyse von IoT Hub-Protokollen

- Kosten nach Meldung: erweiterte Sicherheitsfunktionen basierend auf Sicherheitsmeldungen von IoT Edge oder Blattgeräten


Weitere Informationen finden Sie unter [Security Center – Preise](https://azure.microsoft.com/pricing/details/security-center/).

## <a name="associated-costs"></a>Anfallende Kosten

Für Azure Security Center für IoT fallen Kosten an, die nicht Teil der direkten Preise sind:


- Kosten für Log Analytics-Speicher

Sie können die anfallenden Kosten reduzieren, indem Sie bestimmte Funktionen der Lösung deaktivieren. Sie bewirken das, indem Sie Ihre Einstellungen ändern.

So ändern Sie Ihre Einstellungen

1. Öffnen Sie IoT Hub.

2. Klicken Sie unter **Sicherheit** auf **Übersicht**.

3. Klicken Sie auf **Einstellungen**.

Die folgende Tabelle enthält eine Zusammenfassung der anfallenden Kosten und Auswirkungen der einzelnen Optionen.

|     | Verwendung | Comment |
| --- | --- | --- |
| **Log Analytics-Speicher** |  |
| Geräteempfehlungen und -warnungen| Vom Dienst generierte Sicherheitsempfehlungen und -warnungen | Nicht optional |
| Sicherheitsrohdaten| Sicherheitsrohdaten von IoT-Geräten, erfasst von Sicherheits-Agents | Deaktivieren von _Store raw device security events_ (Sicherheitsrohereignisse für Geräte speichern) |
|

>[!Important]
> Die Deaktivierung hat schwerwiegende Auswirkungen auf die Verfügbarkeit von Azure Security Center für IoT-Sicherheitsfunktionen. 
  
| Deaktivierung | Auswirkungen |
| --- | --- |
| _Twin metadata collection (Sammlung von Gerätezwillingsmetadaten)_ | [Benutzerdefinierte Benachrichtigungen](quickstart-create-custom-alerts.md) sind deaktiviert |
| | Empfehlungen zum IoT Edge-Manifest sind deaktiviert |
| | Empfehlungen und Warnungen basierend auf Geräteidentität sind deaktiviert |
| _Store raw device security events (Sicherheitsrohereignisse für Geräte speichern)_ | Details zu grundlegenden Empfehlungen zum Gerätebetriebssystem sind nicht verfügbar |
| | Details zu Untersuchungen von [Warnungen](concept-security-alerts.md) und [Empfehlungen](concept-recommendations.md) sind nicht verfügbar |
|


## <a name="see-also"></a>Weitere Informationen

- [Zugreifen auf Sicherheitsdaten](how-to-security-data-access.md)
- [Untersuchen eines Geräts](how-to-investigate-device.md)
- Verstehen und Erkunden von [Sicherheitsempfehlungen](concept-recommendations.md)
- Verstehen und Erkunden von [Sicherheitswarnungen](concept-security-alerts.md)
