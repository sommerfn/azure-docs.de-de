---
title: Sicherheitskontrollen für Azure Service Bus Messaging
description: Eine Prüfliste mit Sicherheitskontrollen zur Auswertung von Azure Service Bus Messaging
services: service-bus-messaging
ms.service: service-bus-messaging
author: axisc
ms.topic: conceptual
ms.date: 09/04/2019
ms.author: aschhab
ms.openlocfilehash: ac931ac7cf6c8609019b120388e1b6ca836f72b8
ms.sourcegitcommit: 7c5a2a3068e5330b77f3c6738d6de1e03d3c3b7d
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 09/11/2019
ms.locfileid: "70886244"
---
# <a name="security-controls-for-azure-service-bus-messaging"></a>Sicherheitskontrollen für Azure Service Bus Messaging

In diesem Artikel werden die in Azure Service Bus Messaging integrierten Sicherheitskontrollen beschrieben.

[!INCLUDE [Security controls Header](../../includes/security-controls-header.md)]

## <a name="network"></a>Netzwerk

| Sicherheitskontrolle | Ja/Nein | Notizen |
|---|---|--|
| Unterstützung des Dienstendpunkts| Ja (nur Premium-Tarif) | VNET-Dienstendpunkte werden nur für den [Premium-Tarif von Service Bus](service-bus-premium-messaging.md) unterstützt. |
| Unterstützung der VNet-Einschleusung| Nein | |
| Unterstützung von Netzwerkisolation und Firewall| Ja (nur Premium-Tarif) |  |
| Unterstützung der Tunnelerzwingung| Nein |  |

## <a name="monitoring--logging"></a>Überwachung und Protokollierung

| Sicherheitskontrolle | Ja/Nein | Notizen|
|---|---|--|
| Unterstützung der Azure-Überwachung (Log Analytics, Application Insights usw.)| Ja | Unterstützt über [Azure Monitor und -Warnungen](service-bus-metrics-azure-monitor.md). |
| Protokollierung und Überwachung auf Steuerungs- und Verwaltungsebene| Ja | Vorgangsprotokolle sind verfügbar; siehe [Service Bus-Diagnoseprotokolle](service-bus-diagnostic-logs.md).  |
| Protokollierung und Überwachung auf Datenebene| Nein |  |

## <a name="identity"></a>Identity

| Sicherheitskontrolle | Ja/Nein | Notizen|
|---|---|--|
| Authentication| Ja | Verwaltet durch [verwaltete Dienstidentität von Azure Active Directory](service-bus-managed-service-identity.md); siehe [Service Bus-Authentifizierung und -Autorisierung](service-bus-authentication-and-authorization.md).|
| Authorization| Ja | Unterstützt Autorisierung über [rollenbasierte Zugriffssteuerung (RBAC)](authenticate-application.md) und SAS-Token. Siehe [Service Bus-Authentifizierung und -Autorisierung](service-bus-authentication-and-authorization.md). |

## <a name="data-protection"></a>Datenschutz

| Sicherheitskontrolle | Ja/Nein | Notizen |
|---|---|--|
| Serverseitige Verschlüsselung ruhender Daten: Von Microsoft verwaltete Schlüssel |  Ja, standardmäßig für serverseitige Verschlüsselung ruhender Daten. | Von Kunden verwaltete Schlüssel und BYOK werden noch nicht unterstützt. Clientseitige Verschlüsselung liegt in der Verantwortung des Clients. |
| Serverseitige Verschlüsselung ruhender Daten: vom Kunden verwaltete Schlüssel (BYOK) | Nein |   |
| Verschlüsselung auf Spaltenebene (Azure Data Services)| – | |
| Verschlüsselung während der Übertragung (z. B. ExpressRoute-Verschlüsselung, VNET-Verschlüsselung und VNET-VNET-Verschlüsselung)| Ja | Unterstützt HTTPS/TLS-Standardverfahren. |
| Verschlüsselte API-Aufrufe| Ja | API-Aufrufe erfolgen über [Azure Resource Manager](../azure-resource-manager/index.yml) und HTTPS. |

## <a name="configuration-management"></a>Konfigurationsverwaltung

| Sicherheitskontrolle | Ja/Nein | Notizen|
|---|---|--|
| Unterstützung der Konfigurationsverwaltung (Versionsverwaltung der Konfiguration usw.)| Ja | Unterstützt Versionsverwaltung durch den Ressourcenanbieter über [Azure Resource Manager-API](/rest/api/resources/).|

## <a name="next-steps"></a>Nächste Schritte

- Erfahren Sie mehr über die [integrierten Sicherheitskontrollen in Azure-Diensten](../security/fundamentals/security-controls.md).