---
title: Sicherheitsattribute für Azure VPN Gateway
description: Eine Checkliste der Sicherheitsattribute für die Auswertung von Azure VPN Gateway
services: sql-database
author: msmbaldwin
manager: barbkess
ms.service: load-balancer
ms.topic: conceptual
ms.date: 05/06/2019
ms.author: mbaldwin
ms.openlocfilehash: 0e58e7b3f77d9bb673e300aa60ad07ca9dba5153
ms.sourcegitcommit: 41ca82b5f95d2e07b0c7f9025b912daf0ab21909
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 06/13/2019
ms.locfileid: "67082167"
---
# <a name="security-attributes-for-azure-vpn-gateway"></a>Sicherheitsattribute für Azure VPN Gateway

In diesem Artikel werden die in Azure VPN Gateway integrierten allgemeinen Sicherheitsattribute beschrieben.

[!INCLUDE [Security Attributes Header](../../includes/security-attributes-header.md)]


## <a name="preventative"></a>Prävention

| Sicherheitsattribut | Ja/Nein | Notizen |
|---|---|--|
| Verschlüsselung ruhender Daten (z. B. serverseitige Verschlüsselung, serverseitige Verschlüsselung mit vom Kunden verwalteten Schlüsseln und andere Verschlüsselungsfunktionen) | – | VPN Gateway überträgt Kundendaten, speichert diese aber NICHT. |
| Verschlüsselung während der Übertragung (z. B. ExpressRoute-Verschlüsselung, Verschlüsselung im VNET und VNET-zu-VNET-Verschlüsselung)| Ja | VPN Gateway verschlüsselt Kundenpakete zwischen Azure VPN Gateways und lokalen VPN-Geräte (S2S) oder VPN-Clients (P2S) des Kunden. VPN Gateways unterstützen auch die VNet-zu-VNet-Verschlüsselung. |
| Verarbeitung von Verschlüsselungsschlüsseln (CMK, BYOK usw.)| Nein | Kundenspezifische vorinstallierte Schlüssel werden im Ruhezustand verschlüsselt; aber noch nicht in die CMK integriert. |
| Verschlüsselung auf Spaltenebene (Azure Data Services)| – | |
| Verschlüsselte API-Aufrufe| Ja | Über [Azure Resource Manager](../azure-resource-manager/index.yml) und HTTPS.  |

## <a name="network-segmentation"></a>Netzwerksegmentierung

| Sicherheitsattribut | Ja/Nein | Notizen |
|---|---|--|
| Unterstützung des Dienstendpunkts| – | |
| Unterstützung der VNet-Einschleusung| – | . |
| Netzwerkisolation und Firewallunterstützung| Ja | VPN Gateways sind dedizierte VM-Instanzen für das virtuelle Kundennetzwerk.  |
| Unterstützung der Tunnelerzwingung| Ja |  |

## <a name="detection"></a>Erkennung

| Sicherheitsattribut | Ja/Nein | Notizen|
|---|---|--|
| Unterstützung der Azure-Überwachung (Log Analytics, Application Insights usw.)| Ja | Weitere Informationen finden Sie unter [Azure Monitor-Diagnoseprotokolle/Warnung](vpn-gateway-howto-setup-alerts-virtual-network-gateway-log.md) & [Azure Monitor-Metriken/Warnung](vpn-gateway-howto-setup-alerts-virtual-network-gateway-metric.md).  |

## <a name="identity-and-access-management"></a>Identitäts- und Zugriffsverwaltung

| Sicherheitsattribut | Ja/Nein | Notizen|
|---|---|--|
| Authentication| Ja | [Azure Active Directory](../active-directory/fundamentals/active-directory-whatis.md) für die Verwaltung des Diensts und Konfiguration des Azure VPN Gateways. |
| Autorisierung| Ja | Unterstützung der Autorisierung über [RBAC](../role-based-access-control/overview.md). |


## <a name="audit-trail"></a>Überwachungspfad

| Sicherheitsattribut | Ja/Nein | Notizen|
|---|---|--|
| Protokollierung und Überwachung auf Steuerungs- und Verwaltungsebene| Ja | Azure Resource Manager-Aktivitätsprotokoll. |
| Protokollierung und Überwachung auf Datenebene | Ja | [Azure Monitor-Diagnoseprotokolle](../azure-resource-manager/resource-group-audit.md) für die Protokollierung und Überwachung von VPN-Verbindungen. |

## <a name="configuration-management"></a>Konfigurationsverwaltung

| Sicherheitsattribut | Ja/Nein | Notizen|
|---|---|--|
| Unterstützung der Konfigurationsverwaltung (Versionsverwaltung der Konfiguration usw.)| Ja | Für Verwaltungsvorgänge kann der Status einer Azure VPN Gateway-Konfiguration als eine Azure Resource Manager-Vorlage exportiert und im Lauf der Zeit versioniert werden. | 