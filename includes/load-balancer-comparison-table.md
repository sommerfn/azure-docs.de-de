---
title: include file
description: include file
services: load balancer
author: KumudD
ms.service: load-balancer
ms.topic: include
ms.date: 02/08/2018
ms.author: kumud
ms.custom: include file
ms.openlocfilehash: 8b08e0ced0c7094890a80c37452c7f1b001fe511
ms.sourcegitcommit: cf36df8406d94c7b7b78a3aabc8c0b163226e1bc
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 11/09/2019
ms.locfileid: "73888694"
---
| | Standard-SKU | Basic-SKU |
| --- | --- | --- |
| Größe des Back-End-Pools | Unterstützt bis zu 1.000 Instanzen | Unterstützt bis zu 100 Instanzen |
| Back-End-Pool-Endpunkte | Jeder virtuelle Computer in einem einzelnen virtuellen Netzwerk, einschließlich Kombinationen aus virtuellen Computern, Verfügbarkeitsgruppen und VM-Skalierungsgruppen | Virtuelle Computer in einer einzelnen Verfügbarkeitsgruppe oder VM-Skalierungsgruppe |
| [Integritätstests](../articles/load-balancer/load-balancer-custom-probe-overview.md#types) | TCP, HTTP, HTTPS | TCP, HTTP |
| [Verhalten bei Ausfall während Integritätstest](../articles/load-balancer/load-balancer-custom-probe-overview.md#probedown) | TCP-Verbindungen bleiben bei Ausfall eines Instanztests __und__ bei Ausfall aller Tests bestehen. | TCP-Verbindungen bleiben bei Ausfall eines Instanztests bestehen. Bei Ausfall aller Tests werden alle TCP-Verbindungen getrennt. |
| Verfügbarkeitszonen | Zonenredundante und zonale Front-Ends für eingehenden und ausgehenden Datenverkehr. Zuordnungen für ausgehende Datenflüsse bleiben bei Zonenfehlern erhalten. Zonenübergreifender Lastenausgleich. | Nicht verfügbar |
| Diagnose | Azure Monitor. Mehrdimensionale Metriken einschließlich Byte- und Paketleistungsindikatoren. Integritätsteststatus. Verbindungsversuche (TCP SYN). Integrität ausgehender Verbindungen (erfolgreiche und nicht erfolgreiche SNAT-Datenströme) Messungen für die aktive Datenebene. | Azure Log Analytics nur für öffentlichen Lastenausgleich. SNAT-Überlastungswarnung. Integritätsanzahl für Back-End-Pool. |
| HA-Ports | Interner Lastenausgleich | Nicht verfügbar |
| Standardmäßig sicher | Öffentliche IP-Adresse, Endpunkte für öffentlichen Lastenausgleich und Endpunkte für internen Lastenausgleich sind für eingehende Datenflüsse geschlossen, es sei denn, sie werden von einer Netzwerksicherheitsgruppe zugelassen. | Standardmäßig geöffnet. Netzwerksicherheitsgruppe optional. |
| [Ausgehende Verbindungen](../articles/load-balancer/load-balancer-outbound-connections.md) | Sie können die poolbasierte ausgehende Netzwerkadressenübersetzung (NAT) explizit mit [Regeln für ausgehenden Datenverkehr](../articles/load-balancer/load-balancer-outbound-rules-overview.md) definieren. Sie können mehrere Front-Ends mit Deaktivierung per Lastenausgleichsregel verwenden. Es _muss_ explizit ein Szenario für ausgehenden Datenverkehr erstellt werden, damit der virtuelle Computer, die Verfügbarkeitsgruppe oder die VM-Skalierungsgruppe ausgehende Verbindungen verwenden kann. VNET-Dienstendpunkte sind erreichbar, ohne ausgehende Verbindungen zu definieren, und zählen nicht zu verarbeiteten Daten. Alle öffentlichen IP-Adressen (einschließlich Azure-PaaS-Dienste, die nicht als VNET-Dienstendpunkte verfügbar sind) müssen über ausgehende Verbindungen erreicht werden und zählen zu verarbeiteten Daten. Wenn nur ein interner Lastenausgleich einen virtuellen Computer, eine Verfügbarkeitsgruppe oder eine VM-Skalierungsgruppe versorgt, sind keine ausgehenden Verbindungen über Standard-SNAT verfügbar. Verwenden Sie stattdessen [Ausgangsregeln](../articles/load-balancer/load-balancer-outbound-rules-overview.md). Die SNAT-Programmierung für ausgehenden Datenverkehr hängt vom Transportprotokoll der Lastenausgleichsregel für eingehenden Datenverkehr ab. | Einzelnes Front-End, ausgewählt nach dem Zufallsprinzip, wenn mehrere Front-Ends vorhanden sind. Wenn nur ein interner Load Balancer einen virtuellen Computer, eine Verfügbarkeitsgruppe oder eine VM-Skalierungsgruppe versorgt, wird Standard-SNAT verwendet. |
| [Regeln für ausgehenden Datenverkehr](../articles/load-balancer/load-balancer-outbound-rules-overview.md) | Deklarative NAT-Konfiguration für ausgehenden Datenverkehr mit öffentlichen IP-Adressen, Präfixen für öffentliche IP-Adressen oder beidem. Konfigurierbares Leerlauftimeout für ausgehenden Datenverkehr (4–120 Minuten). Benutzerdefinierte SNAT-Portzuordnung. | Nicht verfügbar |
| [TCP-Zurücksetzung bei Leerlauf](../articles/load-balancer/load-balancer-tcp-reset.md) | Aktivieren von TCP-Zurücksetzung (TCP RST) bei Leerlauftimeout bei einer beliebigen Regel | Nicht verfügbar |
| [Mehrere Front-Ends](../articles/load-balancer/load-balancer-multivip-overview.md) | Eingehend und [ausgehend](../articles/load-balancer/load-balancer-outbound-connections.md) | Nur eingehend |
| Verwaltungsvorgänge | Die meisten Vorgänge < 30 Sekunden | Meist 60 bis 90+ Sekunden |
| SLA | 99,99 % für Datenpfad mit zwei fehlerfreien virtuellen Computern | Nicht zutreffend | 
| Preise | Berechnung anhand der Anzahl der Regeln, der Daten, die eingehend und ausgehend verarbeitet werden, die der Ressource zugeordnet sind. | Keine Berechnung |
|  |  |  |
