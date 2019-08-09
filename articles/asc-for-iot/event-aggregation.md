---
title: Grundlegendes zur Ereignisaggregation bei der Lösung „Azure Security Center für IoT“ | Microsoft-Dokumentation
description: Erfahren Sie, wie Ereignisse im Dienst „Azure Security Center für IoT“ aggregiert werden.
services: asc-for-iot
ms.service: asc-for-iot
documentationcenter: na
author: mlottner
manager: rkarlin
editor: ''
ms.assetid: d40dac0b-abd0-4ff5-82eb-0f511ee13725
ms.subservice: asc-for-iot
ms.devlang: na
ms.topic: conceptual
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 07/21/2019
ms.author: mlottner
ms.openlocfilehash: a8f751d0a40a8d8e1555549c200a9a7ca8ef0661
ms.sourcegitcommit: fe6b91c5f287078e4b4c7356e0fa597e78361abe
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 07/29/2019
ms.locfileid: "68600333"
---
# <a name="security-agent-event-aggregation"></a>Ereignisaggregation des Sicherheits-Agents

Azure Security Center für IoT-Sicherheits-Agents erfassen Daten und Systemereignisse von Ihrem lokalen Gerät und senden diese Daten zur Verarbeitung und Analyse an die Azure-Cloud. Der Sicherheits-Agent sammelt viele Arten von Geräteereignissen, einschließlich neuer Prozess- und neuer Verbindungsereignisse. Sowohl neue Prozess- als auch neue Verbindungsereignisse können innerhalb einer Sekunde auf einem Gerät regelmäßig auftreten und sind wichtig für eine stabile und umfassende Sicherheit. Allerdings können aufgrund der Menge an Nachrichten, die die Sicherheits-Agents deshalb senden müssen, Ihr IoT Hub-Kontingent und die Kostengrenzen möglicherweise schnell erreicht oder überschritten werden. Diese Ereignisse enthalten jedoch äußerst wertvolle Sicherheitsinformationen, die für den Schutz Ihres Geräts von entscheidender Bedeutung sind.

Um das zusätzliche Kontingent und die Kosten zu reduzieren, während Ihre Geräte geschützt bleiben, aggregieren Azure Security Center für IoT-Agents diese Ereignistypen.

Die Ereignisaggregation ist standardmäßig **aktiviert**. Sie kann jederzeit manuell **deaktiviert** werden, obwohl von dieser Vorgehensweise abgeraten wird.

Die Aggregation steht derzeit für die folgenden Ereignistypen zur Verfügung:
* ProcessCreate
* ConnectionCreate
* ProcessTerminate (nur Windows)

## <a name="how-does-event-aggregation-work"></a>Wie funktioniert die Ereignisaggregation?
Wenn die Ereignisaggregation **aktiviert** ist, aggregieren Azure Security Center für IoT-Agents Ereignisse für den Intervallzeitraum oder das Zeitfenster.
Nachdem der Intervallzeitraum abgelaufen ist, sendet der Agent die aggregierten Ereignisse zur weiteren Analyse an die Azure-Cloud.
Die aggregierten Ereignisse werden im Arbeitsspeicher gespeichert, bis sie an die Azure-Cloud gesendet werden.

Um den Speicherbedarf des Agents zu verringern, erhöht der Agent immer dann die Trefferanzahl dieses spezifischen Ereignisses, wenn er ein identisches Ereignis zu einem Ereignis erfasst, das im Arbeitsspeicher bereits enthalten ist. Wenn das Aggregationszeitfenster überschritten wird, sendet der Agent die Trefferanzahl für jeden spezifischen Ereignistyp, der aufgetreten ist. Die Ereignisaggregation ist einfach die Aggregation der Trefferanzahl für die einzelnen erfassten Ereignistypen.

Ereignisse werden nur dann als identisch betrachtet, wenn die folgenden Bedingungen erfüllt sind: 

* „ProcessCreate“-Ereignisse – Wenn **commandLine**, **executable**, **username** und **userid** identisch sind
* „ConnectionCreate“-Ereignisse – Wenn **commandLine**, **userId**, **direction**, **local address**, **remote address**, **protocol** und **destination port** identisch sind
* „ProcessTerminate“-Ereignisse – Wenn **executable** und **exit status** identisch sind

### <a name="working-with-aggregated-events"></a>Arbeiten mit aggregierten Ereignissen

Während der Aggregation werden nicht aggregierte Ereigniseigenschaften verworfen und in Log Analytics mit dem Wert „0“ angezeigt. 
* „ProcessCreate“-Ereignisse – **processId** und **parentProcessId** sind auf „0“ festgelegt.
* „ConnectionCreate“-Ereignisse – **processId** und **source port** sind auf „0“ festgelegt.

## <a name="event-aggregation-based-alerts"></a>Warnungen, die auf Ereignisaggregation basieren 
Nach der Analyse erstellt Azure Security Center für IoT Sicherheitswarnungen für verdächtige aggregierte Ereignisse. Aus aggregierten Ereignissen erstellte Warnungen werden für jedes aggregierte Ereignis nur einmal angezeigt.

Die Start- und Endzeit für die Aggregation sowie die Trefferanzahl für jedes Ereignis werden im Ereignisfeld **ExtraDetails** in Log Analytics zur Verwendung während Untersuchungen protokolliert. 


## <a name="event-aggregation-twin-configuration"></a>Konfiguration der Ereignisaggregation von Modulzwillingen
Nehmen Sie Änderungen an der Konfiguration der Ereignisaggregation von Azure Security Center für IoT im [Agentkonfigurationsobjekt](how-to-agent-configuration.md) der Modulzwillingsidentität des Moduls **azureiotsecurity** vor.

| Konfigurationsname | Mögliche Werte | Details | Anmerkungen |
|:-----------|:---------------|:--------|:--------|
| aggregationEnabledProcessCreate | boolean | Ereignisaggregation für „ProcessCreate“-Ereignisse aktivieren/deaktivieren |
| aggregationIntervalProcessCreate | ISO8601 TimeSpan-Zeichenfolge | Aggregationsintervall für „ProcessCreate“-Ereignisse |
| aggregationEnabledConnectionCreate | boolean| Ereignisaggregation für „ConnectionCreate“-Ereignisse aktivieren/deaktivieren |
| aggregationIntervalConnectionCreate | ISO8601 TimeSpan-Zeichenfolge | Aggregationsintervall für „ConnectionCreate“-Ereignisse |
| aggregationEnabledProcessTerminate | boolean | Ereignisaggregation für „ProcessTerminate“-Ereignisse aktivieren/deaktivieren | Nur Windows|
| aggregationIntervalProcessTerminate | ISO8601 TimeSpan-Zeichenfolge | Aggregationsintervall für „ProcessTerminate“-Ereignisse | Nur Windows|
|

## <a name="default-configurations-settings"></a>Standardkonfigurationseinstellungen

| Konfigurationsname | Standardwerte |
|:-----------|:---------------|
| aggregationEnabledProcessCreate | true |
| aggregationIntervalProcessCreate | „PT1H“|
| aggregationEnabledConnectionCreate | true |
| aggregationIntervalConnectionCreate | „PT1H“|
| aggregationEnabledProcessTerminate | true |
| aggregationIntervalProcessTerminate | „PT1H“|
|

## <a name="next-steps"></a>Nächste Schritte

In diesem Artikel haben Sie etwas über die Ereignisaggregation beim Azure Security Center für IoT-Sicherheits-Agent und die verfügbaren Optionen für die Ereigniskonfiguration erfahren.

Wenn Sie die ersten Schritte mit der Azure Security Center für IoT-Bereitstellung fortsetzen möchten, lesen Sie die folgenden Artikel:

- Machen Sie sich mit den [Methoden der Sicherheits-Agent-Authentifizierung](concept-security-agent-authentication-methods.md) vertraut.
- Wählen Sie einen [Sicherheits-Agent](how-to-deploy-agent.md) aus, und stellen Sie ihn bereit.
- Überprüfen der [Dienstvoraussetzungen](service-prerequisites.md) für Azure Security Center für IoT
- Informationen zum [Aktivieren des Diensts „Azure Security Center für IoT“ in Ihrer IoT Hub-Instanz](quickstart-onboard-iot-hub.md)
- Weitere Informationen zum Dienst finden Sie in [Azure Security Center für IoT – Häufig gestellte Fragen (FAQ)](resources-frequently-asked-questions.md)
