---
title: Roundtrip-Latenzstatistiken von Azure-Netzwerken | Microsoft-Dokumentation
description: Erfahren Sie mehr über Statistiken zur Roundtrip-Latenz zwischen Azure-Regionen.
services: networking
documentationcenter: na
author: nayak-mahesh
ms.service: virtual-network
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 11/04/2019
ms.author: mnayak
ms.openlocfilehash: 500676983233f943fdc9638d75758645dee65564
ms.sourcegitcommit: f4d8f4e48c49bd3bc15ee7e5a77bee3164a5ae1b
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 11/04/2019
ms.locfileid: "73586871"
---
# <a name="azure-network-round-trip-latency-statistics"></a>Roundtrip-Latenzstatistiken von Azure-Netzwerken

Azure überwacht kontinuierlich die Latenz (Geschwindigkeit) der Kernbereiche des Netzwerks mithilfe interner Überwachungstools und Messungen, die von [ThousandEyes](https://thousandeyes.com) erfasst werden, einem synthetischen Überwachungsdienst eines Drittanbieters.

## <a name="how-are-the-measurements-collected"></a>Wie werden die Messungen erfasst?

Die Latenzmessungen werden von ThousandEyes-Agents erfasst, die weltweit in Azure-Cloudregionen gehostet werden und fortlaufend in Intervallen von 1 Minute untereinander Netzwerktests senden. Die monatlichen Latenzstatistiken werden aus dem Mittelwert der erfassten Stichproben für den Monat abgeleitet.

## <a name="october-2019-latency-figures"></a>Latenzzahlen vom Oktober 2019

Für den Zeitraum von 31 Tagen, der am 31. Oktober 2019 endet, lauten die monatlichen minimalen und maximalen Roundtrip-Latenzzeiten in aggregierten Regionen wie folgt:

- **5 ms** bis **72 ms** für Roundtrips innerhalb der Regionen in **Nordamerika**.
- **3 ms** bis **28 ms** für Roundtrips innerhalb der Regionen in **Europa**.
- **4 ms** bis **134 ms** für Roundtrips innerhalb der Regionen in **Asien**.

Die folgenden Latenzmessungen bei regionsübergreifender Datenübertragung werden von [ThousandEyes](https://thousandeyes.com) unterstützt. Die Maßeinheit in der folgenden Tabelle ist Millisekunden (ms).

![Latenzstatistiken zu regionsübergreifender Datenübertragung in Azure](media/azure-network-latency/azure-inter-region-latency.png)


## <a name="next-steps"></a>Nächste Schritte
- Weitere Informationen zu [Azure-Regionen](https://azure.microsoft.com/global-infrastructure/regions/).