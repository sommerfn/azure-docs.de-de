---
title: Sicherheitskontrollen für Azure ExpressRoute
description: Eine Prüfliste mit Sicherheitskontrollen zur Evaluierung von Azure ExpressRoute
services: expressroute
ms.service: expressroute
documentationcenter: ''
author: msmbaldwin
manager: barbkess
ms.topic: conceptual
ms.date: 06/05/2019
ms.author: mbaldwin
ms.openlocfilehash: 7ab73ba6dd4b78d3cd0be5f4c7f7a502e5c58e08
ms.sourcegitcommit: 7c5a2a3068e5330b77f3c6738d6de1e03d3c3b7d
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 09/11/2019
ms.locfileid: "70886272"
---
# <a name="security-controls-for-azure-expressroute"></a>Sicherheitskontrollen für Azure ExpressRoute

In diesem Artikel werden die in Azure ExpressRoute integrierten Sicherheitskontrollen beschrieben.

[!INCLUDE [Security controls Header](../../includes/security-controls-header.md)]

## <a name="network"></a>Netzwerk

| Sicherheitskontrolle | Ja/Nein | Notizen |
|---|---|--|
| Unterstützung des Dienstendpunkts| – |  |
| Unterstützung der VNet-Einschleusung| – | |
| Unterstützung von Netzwerkisolation und Firewall| Ja | Jeder Kunde ist in seiner eigenen Routingdomäne enthalten und wird zu seinem eigenen VNET getunnelt |
| Unterstützung der Tunnelerzwingung| – | Über Border Gateway Protocol (BGP). |

## <a name="monitoring--logging"></a>Überwachung und Protokollierung

| Sicherheitskontrolle | Ja/Nein | Notizen|
|---|---|--|
| Unterstützung der Azure-Überwachung (Log Analytics, Application Insights usw.)| Ja | Siehe [ExpressRoute-Überwachung, Metriken und Warnungen](expressroute-monitoring-metrics-alerts.md).|
| Protokollierung und Überwachung auf Steuerungs- und Verwaltungsebene| Ja |  |
| Protokollierung und Überwachung auf Datenebene| Nein |   |

## <a name="identity"></a>Identity

| Sicherheitskontrolle | Ja/Nein | Notizen|
|---|---|--|
| Authentication| Ja | Dienstkonto für Gateway für Microsoft (GWM), Controller; JIT-Zugriff (Just-in-Time) für Entwicklung und Betrieb. |
| Authorization|  Ja |Dienstkonto für Gateway für Microsoft (GWM), Controller; JIT-Zugriff (Just-in-Time) für Entwicklung und Betrieb. |

## <a name="data-protection"></a>Datenschutz

| Sicherheitskontrolle | Ja/Nein | Notizen |
|---|---|--|
| Serverseitige Verschlüsselung ruhender Daten: Von Microsoft verwaltete Schlüssel |  – | ExpressRoute speichert keine Kundendaten. |
| Serverseitige Verschlüsselung ruhender Daten: vom Kunden verwaltete Schlüssel (BYOK) | – |  |
| Verschlüsselung auf Spaltenebene (Azure Data Services)| – | |
| Verschlüsselung während der Übertragung (z. B. ExpressRoute-Verschlüsselung, VNET-Verschlüsselung und VNET-VNET-Verschlüsselung)| Nein | |
| Verschlüsselte API-Aufrufe| Ja | Über [Azure Resource Manager](../azure-resource-manager/index.yml) und HTTPS. |


## <a name="configuration-management"></a>Konfigurationsverwaltung

| Sicherheitskontrolle | Ja/Nein | Notizen|
|---|---|--|
| Unterstützung der Konfigurationsverwaltung (Versionsverwaltung der Konfiguration usw.)| Ja | Über Netzwerkressourcenanbieter (Network Resource Provider, NRP). |

## <a name="next-steps"></a>Nächste Schritte

- Erfahren Sie mehr über die [integrierten Sicherheitskontrollen in Azure-Diensten](../security/fundamentals/security-controls.md).