---
title: Sicherheitskontrollen für Azure Service Bus Relay
description: Eine Prüfliste mit Sicherheitskontrollen zur Auswertung von Azure Service Bus Relay
services: service-bus-relay
ms.service: service-bus-relay
author: spelluru
ms.topic: conceptual
ms.date: 09/23/2019
ms.author: spelluru
ms.openlocfilehash: 7f4e2a31673905a7e28d1dbb5520650aefc6f368
ms.sourcegitcommit: 3fa4384af35c64f6674f40e0d4128e1274083487
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 09/24/2019
ms.locfileid: "71219983"
---
# <a name="security-controls-for-azure-service-bus-relay"></a>Sicherheitskontrollen für Azure Service Bus Relay

In diesem Artikel werden die in Azure Service Bus Relay integrierten Sicherheitskontrollen beschrieben.

[!INCLUDE [Security controls Header](../../includes/security-controls-header.md)]

## <a name="network"></a>Netzwerk

| Sicherheitskontrolle | Ja/Nein | Notizen | Dokumentation |
|---|---|--|--|
| Unterstützung des Dienstendpunkts| Nein |  |   |
| Unterstützung von Netzwerkisolation und Firewall| Nein |  |   |
| Unterstützung der Tunnelerzwingung| – | Relay ist der TLS-Tunnel  |   |

## <a name="monitoring--logging"></a>Überwachung und Protokollierung

| Sicherheitskontrolle | Ja/Nein | Notizen| Dokumentation |
|---|---|--|--|
| Unterstützung der Azure-Überwachung (Log Analytics, Application Insights usw.)| Ja | |   |
| Protokollierung und Überwachung auf Steuerungs- und Verwaltungsebene| Ja | Über [Azure Resource Manager](../azure-resource-manager/index.yml). |   |
| Protokollierung und Überwachung auf Datenebene| Ja | Verbindungserfolg/-fehler und Fehler werden protokolliert.  |   |

## <a name="identity"></a>Identity

| Sicherheitskontrolle | Ja/Nein | Notizen| Dokumentation |
|---|---|--|--|
| Authentication| Ja | Über SAS. | [Azure Relay-Authentifizierung und -Autorisierung](relay-authentication-and-authorization.md) |
| Authorization|  Ja | Über SAS. | [Azure Relay-Authentifizierung und -Autorisierung](relay-authentication-and-authorization.md) |

## <a name="data-protection"></a>Datenschutz

| Sicherheitskontrolle | Ja/Nein | Notizen | Dokumentation |
|---|---|--|--|
| Serverseitige Verschlüsselung ruhender Daten: Von Microsoft verwaltete Schlüssel |  – | Relay ist ein Websocket, das Daten nicht beibehält. |   |
| Serverseitige Verschlüsselung ruhender Daten: vom Kunden verwaltete Schlüssel (BYOK) | Nein | Verwendet nur Microsoft-TLS-Zertifikate.  |   |
| Verschlüsselung auf Spaltenebene (Azure Data Services)| – | |   |
| Verschlüsselung während der Übertragung (z. B. ExpressRoute-Verschlüsselung, VNET-Verschlüsselung und VNET-VNET-Verschlüsselung)| Ja | Der Dienst erfordert TLS. |   |
| Verschlüsselte API-Aufrufe| Ja | HTTPS. |


## <a name="configuration-management"></a>Konfigurationsverwaltung

| Sicherheitskontrolle | Ja/Nein | Notizen| Dokumentation |
|---|---|--|--|
| Unterstützung der Konfigurationsverwaltung (Versionsverwaltung der Konfiguration usw.)| Ja | Über [Azure Resource Manager](../azure-resource-manager/index.yml).|   |

## <a name="next-steps"></a>Nächste Schritte

- Erfahren Sie mehr über die [integrierten Sicherheitskontrollen in Azure-Diensten](../security/fundamentals/security-controls.md).