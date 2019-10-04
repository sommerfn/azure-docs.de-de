---
title: Sicherheitskontrollen für Azure Load Balancer
description: Eine Prüfliste der Sicherheitskontrollen für die Auswertung von Load Balancer
services: load-balancer
author: msmbaldwin
manager: barbkess
ms.service: load-balancer
ms.topic: conceptual
ms.date: 09/04/2019
ms.author: mbaldwin
ms.openlocfilehash: e0be6635a0916183e1dfe776bef4c547578383dc
ms.sourcegitcommit: 7c5a2a3068e5330b77f3c6738d6de1e03d3c3b7d
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 09/11/2019
ms.locfileid: "70886292"
---
# <a name="security-controls-for-azure-load-balancer"></a>Sicherheitskontrollen für Azure Load Balancer

In diesem Artikel werden die in Azure Load Balancer integrierten Sicherheitskontrollen beschrieben.

[!INCLUDE [Security controls Header](../../includes/security-controls-header.md)]

## <a name="network"></a>Netzwerk

| Sicherheitskontrolle | Ja/Nein | Notizen |
|---|---|--|
| Unterstützung des Dienstendpunkts| – | |
| Unterstützung der VNet-Einschleusung| – | |
| Netzwerkisolation und Firewallunterstützung| – |  |
| Unterstützung der Tunnelerzwingung| – | |

## <a name="monitoring--logging"></a>Überwachung und Protokollierung

| Sicherheitskontrolle | Ja/Nein | Notizen|
|---|---|--|
| Unterstützung der Azure-Überwachung (Log Analytics, Application Insights usw.)| Ja | Siehe [Azure Monitor-Protokolle für öffentlichen Load Balancer vom Typ „Basic“](load-balancer-monitor-log.md) |
| Protokollierung und Überwachung auf Steuerungs- und Verwaltungsebene| Ja | Siehe [Azure Monitor-Protokolle für öffentlichen Load Balancer vom Typ „Basic“](load-balancer-monitor-log.md) |
| Protokollierung und Überwachung auf Datenebene | – |  |

## <a name="identity"></a>Identity

| Sicherheitskontrolle | Ja/Nein | Notizen|
|---|---|--|
| Authentication| – |  |
| Authorization| – |  |

## <a name="data-protection"></a>Datenschutz

| Sicherheitskontrolle | Ja/Nein | Notizen |
|---|---|--|
| Serverseitige Verschlüsselung ruhender Daten: Von Microsoft verwaltete Schlüssel | – | |
| Verschlüsselung während der Übertragung (z. B. ExpressRoute-Verschlüsselung, Verschlüsselung im VNET und VNET-zu-VNET-Verschlüsselung)| – | |
| Serverseitige Verschlüsselung ruhender Daten: vom Kunden verwaltete Schlüssel (BYOK) | – | |
| Verschlüsselung auf Spaltenebene (Azure Data Services)| – | |
| Verschlüsselte API-Aufrufe| Ja | Über [Azure Resource Manager](../azure-resource-manager/index.yml) |

## <a name="configuration-management"></a>Konfigurationsverwaltung

| Sicherheitskontrolle | Ja/Nein | Notizen|
|---|---|--|
| Unterstützung der Konfigurationsverwaltung (Versionsverwaltung der Konfiguration usw.)| – |  | 

## <a name="next-steps"></a>Nächste Schritte

- Erfahren Sie mehr über die [integrierten Sicherheitskontrollen in Azure-Diensten](../security/fundamentals/security-controls.md).