---
title: Sicherheitskontrollen für Azure VPN Gateway
description: Eine Prüfliste mit Sicherheitskontrollen zur Auswertung von Azure VPN Gateway
services: sql-database
author: msmbaldwin
manager: rkarlin
ms.service: load-balancer
ms.topic: conceptual
ms.date: 09/06/2019
ms.author: mbaldwin
ms.openlocfilehash: 1babb892063da6d460ea2bc4c567da954731956f
ms.sourcegitcommit: 7c5a2a3068e5330b77f3c6738d6de1e03d3c3b7d
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 09/11/2019
ms.locfileid: "70886294"
---
# <a name="security-controls-for-azure-vpn-gateway"></a>Sicherheitskontrollen für Azure VPN Gateway

In diesem Artikel sind die in Azure VPN Gateway integrierten Sicherheitskontrollen beschrieben.

[!INCLUDE [Security controls Header](../../includes/security-controls-header.md)]

## <a name="network"></a>Netzwerk

| Sicherheitskontrolle | Ja/Nein | Notizen |
|---|---|--|
| Unterstützung des Dienstendpunkts| – | |
| Unterstützung der VNet-Einschleusung| – | |
| Netzwerkisolation und Firewallunterstützung| Ja | VPN Gateways sind dedizierte VM-Instanzen für das virtuelle Kundennetzwerk.  |
| Unterstützung der Tunnelerzwingung| Ja |  |

## <a name="monitoring--logging"></a>Überwachung und Protokollierung

| Sicherheitskontrolle | Ja/Nein | Notizen|
|---|---|--|
| Unterstützung der Azure-Überwachung (Log Analytics, Application Insights usw.)| Ja | Weitere Informationen finden Sie unter [Azure Monitor-Diagnoseprotokolle/Warnung](vpn-gateway-howto-setup-alerts-virtual-network-gateway-log.md) & [Azure Monitor-Metriken/Warnung](vpn-gateway-howto-setup-alerts-virtual-network-gateway-metric.md).  |
| Protokollierung und Überwachung auf Steuerungs- und Verwaltungsebene| Ja | Azure Resource Manager-Aktivitätsprotokoll. |
| Protokollierung und Überwachung auf Datenebene | Ja | [Azure Monitor-Diagnoseprotokolle](../azure-resource-manager/resource-group-audit.md) für die Protokollierung und Überwachung von VPN-Verbindungen. |

## <a name="identity"></a>Identity

| Sicherheitskontrolle | Ja/Nein | Notizen|
|---|---|--|
| Authentication| Ja | [Azure Active Directory](../active-directory/fundamentals/active-directory-whatis.md) für die Verwaltung des Diensts und Konfiguration des Azure VPN Gateways. |
| Authorization| Ja | Unterstützung der Autorisierung über [RBAC](../role-based-access-control/overview.md). |

## <a name="data-protection"></a>Datenschutz

| Sicherheitskontrolle | Ja/Nein | Notizen |
|---|---|--|
| Serverseitige Verschlüsselung ruhender Daten: Von Microsoft verwaltete Schlüssel | – | VPN Gateway überträgt Kundendaten, speichert diese aber NICHT. |
| Verschlüsselung während der Übertragung (z. B. ExpressRoute-Verschlüsselung, Verschlüsselung im VNET und VNET-zu-VNET-Verschlüsselung)| Ja | VPN Gateway verschlüsselt Kundenpakete zwischen Azure VPN Gateways und lokalen VPN-Geräte (S2S) oder VPN-Clients (P2S) des Kunden. VPN Gateways unterstützen auch die VNet-zu-VNet-Verschlüsselung. |
| Serverseitige Verschlüsselung ruhender Daten: vom Kunden verwaltete Schlüssel (BYOK) | Nein | Kundenspezifische vorinstallierte Schlüssel werden im Ruhezustand verschlüsselt; aber noch nicht in die CMK integriert. |
| Verschlüsselung auf Spaltenebene (Azure Data Services)| – | |
| Verschlüsselte API-Aufrufe| Ja | Über [Azure Resource Manager](../azure-resource-manager/index.yml) und HTTPS.  |

## <a name="configuration-management"></a>Konfigurationsverwaltung

| Sicherheitskontrolle | Ja/Nein | Notizen|
|---|---|--|
| Unterstützung der Konfigurationsverwaltung (Versionsverwaltung der Konfiguration usw.)| Ja | Für Verwaltungsvorgänge kann der Status einer Azure VPN Gateway-Konfiguration als eine Azure Resource Manager-Vorlage exportiert und im Lauf der Zeit versioniert werden. | 

## <a name="next-steps"></a>Nächste Schritte

- Erfahren Sie mehr über die [integrierten Sicherheitskontrollen in Azure-Diensten](../security/fundamentals/security-controls.md).