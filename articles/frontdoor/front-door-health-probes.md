---
title: 'Azure Front Door Service: Überwachung der Back-End-Integrität | Microsoft-Dokumentation'
description: In diesem Artikel wird erläutert, wie Azure Front Door Service die Integrität Ihrer Back-Ends überwacht.
services: frontdoor
documentationcenter: ''
author: sharad4u
ms.service: frontdoor
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 09/10/2018
ms.author: sharadag
ms.openlocfilehash: 289b05a2c50a2b4af50eb2114515a49bb653cf1a
ms.sourcegitcommit: d060947aae93728169b035fd54beef044dbe9480
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 08/02/2019
ms.locfileid: "68742387"
---
# <a name="health-probes"></a>Integritätstests

Um die Integrität der einzelnen Back-Ends zu ermitteln, sendet jede Front Door-Umgebung in regelmäßigen Abständen eine synthetische HTTP-/HTTPS-Anforderung an jedes konfigurierte Back-End. Anschließend ermittelt Front Door anhand der Antworten aus diesen Tests die „besten“ Back-Ends für die Weiterleitung echter Clientanforderungen. Beachten Sie Folgendes: Weil Front Door global über viele Edge-Umgebungen verfügt, kann das Volumen von Integritätstestanforderungen an Ihre Back-Ends so hoch sein, da mehr als eine Anforderung pro Sekunde von der konfigurierten Häufigkeit der Integritätstests abhängt. 



## <a name="supported-protocols"></a>Unterstützte Protokolle

Front Door unterstützt das Senden von Tests über die Protokolle HTTP und HTTPS. Diese Tests werden über dieselben TCP-Ports gesendet, die für das Routen von Clientanforderungen konfiguriert wurden. Sie können nicht außer Kraft gesetzt werden.

## <a name="health-probe-responses"></a>Integritätstestantworten

| Antworten  | BESCHREIBUNG | 
| ------------- | ------------- |
| Ermitteln der Integrität  |  Der Statuscode „200 OK“ gibt an, dass das Back-End fehlerfrei ist. Alle anderen Antworten werden als Fehler betrachtet. Wenn für einen Test aus irgendeinem Grund (einschließlich Netzwerkausfall) keine gültige HTTP-Antwort eingeht, gilt der Test als fehlerhaft.|
| Messen der Latenzzeit  | Latenz ist die Gesamtbetrachtungszeit, gemessen ab dem Zeitpunkt unmittelbar vor dem Senden der Testanforderung bis zu dem Moment, in dem wir das letzte Byte der Antwort erhalten. Wir verwenden für jede Anforderung eine neue TCP-Verbindung, damit diese Messung nicht zugunsten von Back-Ends mit vorhandenen betriebsbereiten Verbindungen ausfällt.  |

## <a name="how-front-door-determines-backend-health"></a>Vorgehensweise beim Ermitteln der Back-End-Integrität mit Front Door

Azure Front Door Service verwendet in allen Algorithmen denselben aus drei Schritten bestehenden Prozess, um die Integrität zu ermitteln:

1. Deaktivierte Back-Ends werden ausgeschlossen.

2. Back-Ends mit fehlerhaften Integritätstests werden ausgeschlossen:
    * Diese Auswahl erfolgt anhand der letzten _n_ Integritätstestantworten. Wenn mindestens _x_ fehlerfrei sind, wird das Back-End als fehlerfrei betrachtet.

    * _n_ wird durch Ändern der SampleSize-Eigenschaft in den Einstellungen für den Lastenausgleich konfiguriert.

    * _x_ wird durch Ändern der SuccessfulSamplesRequired-Eigenschaft in den Einstellungen für den Lastenausgleich konfiguriert.

3. Aus der Menge der fehlerfreien Back-Ends im Back-End-Pool misst und verwaltet Front Door zusätzlich die Latenz (Roundtripzeit) für jedes Back-End.


## <a name="complete-health-probe-failure"></a>Vollständiger Integritätstestfehler

Wenn Integritätstests für alle Back-Ends in einem Back-End-Pool fehlerhaft sind, betrachtet Front Door alle Back-Ends als fehlerfrei und routet den Datenverkehr in einer Roundrobinverteilung über alle Back-Ends.

Sobald ein Back-End zu einem fehlerfreien Integritätsstatus zurückkehrt, wird der normale Lastenausgleichsalgorithmus von Front Door fortgesetzt.

## <a name="next-steps"></a>Nächste Schritte

- Erfahren Sie mehr über das [Erstellen einer Azure Front Door Service-Konfiguration](quickstart-create-front-door.md).
- Informieren Sie sich über die [Funktionsweise von Azure Front Door Service](front-door-routing-architecture.md).
