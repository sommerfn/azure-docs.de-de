---
title: Sicherheitskontrollen für Azure Service Bus Messaging
description: Eine Prüfliste mit Sicherheitskontrollen zur Auswertung von Azure Service Bus Messaging
services: service-bus-messaging
ms.service: service-bus-messaging
author: spelluru
ms.topic: conceptual
ms.date: 09/23/2019
ms.author: spelluru
ms.openlocfilehash: d2b92759384a9a0b63d784a8cb1afb3d18d55aeb
ms.sourcegitcommit: 3fa4384af35c64f6674f40e0d4128e1274083487
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 09/24/2019
ms.locfileid: "71219312"
---
# <a name="security-controls-for-azure-service-bus-messaging"></a>Sicherheitskontrollen für Azure Service Bus Messaging

In diesem Artikel werden die in Azure Service Bus Messaging integrierten Sicherheitskontrollen beschrieben.

[!INCLUDE [Security controls Header](../../includes/security-controls-header.md)]

## <a name="network"></a>Netzwerk

| Sicherheitskontrolle | Ja/Nein | Notizen | Dokumentation |
|---|---|--|--|
| Unterstützung des Dienstendpunkts| Ja (nur Premium-Tarif) | VNET-Dienstendpunkte werden nur für den [Premium-Tarif von Service Bus](service-bus-premium-messaging.md) unterstützt. |  |
| Unterstützung der VNet-Einschleusung| Nein | |  |
| Unterstützung von Netzwerkisolation und Firewall| Ja (nur Premium-Tarif) |  |  |
| Unterstützung der Tunnelerzwingung| Nein |  |  |

## <a name="monitoring--logging"></a>Überwachung und Protokollierung

| Sicherheitskontrolle | Ja/Nein | Notizen| Dokumentation |
|---|---|--|--|
| Unterstützung der Azure-Überwachung (Log Analytics, Application Insights usw.)| Ja | Unterstützt über [Azure Monitor und -Warnungen](service-bus-metrics-azure-monitor.md). |  |
| Protokollierung und Überwachung auf Steuerungs- und Verwaltungsebene| Ja | Vorgangsprotokolle sind verfügbar.  | [Service Bus-Diagnoseprotokolle](service-bus-diagnostic-logs.md) |
| Protokollierung und Überwachung auf Datenebene| Nein |  |

## <a name="identity"></a>Identity

| Sicherheitskontrolle | Ja/Nein | Notizen| Dokumentation |
|---|---|--|--|
| Authentication| Ja | Verwaltet über die [verwaltete Dienstidentität von Azure Active Directory](service-bus-managed-service-identity.md).| [Service Bus-Authentifizierung und -Autorisierung](service-bus-authentication-and-authorization.md) |
| Authorization| Ja | Unterstützt die Autorisierung über rollenbasierte Zugriffssteuerung ([RBAC](authenticate-application.md)) und SAS-Token. | [Service Bus-Authentifizierung und -Autorisierung](service-bus-authentication-and-authorization.md) |

## <a name="data-protection"></a>Datenschutz

| Sicherheitskontrolle | Ja/Nein | Notizen | Dokumentation |
|---|---|--|--|
| Serverseitige Verschlüsselung ruhender Daten: Von Microsoft verwaltete Schlüssel |  Ja, standardmäßig für serverseitige Verschlüsselung ruhender Daten. | Von Kunden verwaltete Schlüssel und BYOK werden noch nicht unterstützt. Clientseitige Verschlüsselung liegt in der Verantwortung des Clients. |
| Serverseitige Verschlüsselung ruhender Daten: vom Kunden verwaltete Schlüssel (BYOK) | Nein |   |   |
| Verschlüsselung auf Spaltenebene (Azure Data Services)| – | |   |
| Verschlüsselung während der Übertragung (z. B. ExpressRoute-Verschlüsselung, VNET-Verschlüsselung und VNET-VNET-Verschlüsselung)| Ja | Unterstützt HTTPS/TLS-Standardverfahren. |   |
| Verschlüsselte API-Aufrufe| Ja | API-Aufrufe erfolgen über [Azure Resource Manager](../azure-resource-manager/index.yml) und HTTPS. |   |

## <a name="configuration-management"></a>Konfigurationsverwaltung

| Sicherheitskontrolle | Ja/Nein | Notizen| Dokumentation |
|---|---|--|--|
| Unterstützung der Konfigurationsverwaltung (Versionsverwaltung der Konfiguration usw.)| Ja | Unterstützt Versionsverwaltung durch den Ressourcenanbieter über [Azure Resource Manager-API](/rest/api/resources/).|   |

## <a name="next-steps"></a>Nächste Schritte

- Erfahren Sie mehr über die [integrierten Sicherheitskontrollen in Azure-Diensten](../security/fundamentals/security-controls.md).