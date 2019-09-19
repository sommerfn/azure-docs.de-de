---
title: Sicherheitskontrollen für Azure Cosmos DB
description: Eine Prüfliste mit Sicherheitskontrollen zur Evaluierung von Azure Cosmos DB
services: cosmos-db
documentationcenter: ''
author: msmbaldwin
manager: rkarlin
ms.service: cosmos-db
ms.topic: conceptual
ms.date: 09/04/2019
ms.author: mbaldwin
ms.openlocfilehash: 20cdfc61a4cdfe5263e48d049aab14cad2458b06
ms.sourcegitcommit: 7c5a2a3068e5330b77f3c6738d6de1e03d3c3b7d
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 09/11/2019
ms.locfileid: "70886317"
---
# <a name="security-controls-for-azure-cosmos-db"></a>Sicherheitskontrollen für Azure Cosmos DB

In diesem Artikel werden die in Azure Cosmos DB integrierten Sicherheitskontrollen beschrieben.

[!INCLUDE [Security controls Header](../../includes/security-controls-header.md)]

## <a name="network"></a>Netzwerk

| Sicherheitskontrolle | Ja/Nein | Notizen |
|---|---|--|
| Unterstützung des Dienstendpunkts| Ja |  |
| Unterstützung der VNet-Einschleusung| Ja | Bei einem VNET-Dienstendpunkt können Sie ein Azure Cosmos DB-Konto so konfigurieren, dass der Zugriff nur aus einem bestimmten Subnetz eines virtuellen Netzwerks (VNET) zugelassen wird. Sie können den VNET-Zugriff auch mit Firewallregeln kombinieren.  Siehe [Zugreifen auf Azure Cosmos DB über virtuelle Netzwerke](VNet-service-endpoint.md). |
| Netzwerkisolation und Firewallunterstützung| Ja | Durch die Firewallunterstützung können Sie Ihr Azure Cosmos-Konto so konfigurieren, dass der Zugriff nur über eine genehmigte Gruppe von IP-Adressen, einen IP-Adressbereich und/oder Clouddienste zugelassen wird. Siehe [Konfigurieren der IP-Firewall in Azure Cosmos DB](how-to-configure-firewall.md).|
| Unterstützung der Tunnelerzwingung| Ja | Kann auf Clientseite in dem VNET konfiguriert werden, in dem sich die virtuellen Computer befinden.   |

## <a name="monitoring--logging"></a>Überwachung und Protokollierung

| Sicherheitskontrolle | Ja/Nein | Notizen|
|---|---|--|
| Unterstützung der Azure-Überwachung (Log Analytics, Application Insights usw.)| Ja | Alle an Azure Cosmos DB gesendeten Anforderungen werden protokolliert. Die [Azure-Überwachung](../azure-monitor/overview.md), Azure-Metriken und die Azure-Überwachungsprotokollierung werden unterstützt.  Sie können Informationen protokollieren, die Datenebenenanforderungen, Abfragelaufzeitstatistiken, Abfragetext oder MongoDB-Anforderungen entsprechen. Sie können auch Benachrichtigungen einrichten. |
| Protokollierung und Überwachung auf Steuerungs- und Verwaltungsebene| Ja | Azure-Aktivitätsprotokoll für Vorgänge auf Kontoebene, z. B. Firewalls, VNETs, Schlüsselzugriff und IAM. |
| Protokollierung und Überwachung auf Datenebene | Ja | Diagnoseüberwachungsprotokolle für Vorgänge auf Containerebene, z. B. das Erstellen von Containern, Bereitstellen von Durchsatz, Indizieren von Richtlinien sowie für CRUD-Vorgänge für Dokumente. |

## <a name="identity"></a>Identity

| Sicherheitskontrolle | Ja/Nein | Notizen|
|---|---|--|
| Authentication| Ja | Ja, auf Ebene des Datenbankkontos und der Datenebene. Cosmos DB verwendet Ressourcentoken und Schlüsselzugriff. |
| Authorization| Ja | Für das Azure Cosmos-Konto mit (primärem und sekundärem) Hauptschlüssel und Ressourcentoken unterstützt. Sie können Hauptschlüssel verwenden, um Lese-/Schreibzugriff oder schreibgeschützten Zugriff auf Daten zu erhalten. Ressourcentoken ermöglichen den zeitlich begrenzten Zugriff auf Ressourcen wie Dokumente und Container. |

## <a name="data-protection"></a>Datenschutz

| Sicherheitskontrolle | Ja/Nein | Notizen |
|---|---|--|
| Serverseitige Verschlüsselung ruhender Daten: Von Microsoft verwaltete Schlüssel | Ja | Alle Cosmos-Datenbanken und -Sicherungen werden standardmäßig verschlüsselt. Weitere Informationen finden Sie unter [Datenverschlüsselung in Azure Cosmos DB](database-encryption-at-rest.md). Die serverseitige Verschlüsselung mit vom Kunden verwalteten Schlüsseln wird nicht unterstützt. |
| Serverseitige Verschlüsselung ruhender Daten: vom Kunden verwaltete Schlüssel (BYOK) | Nein |  |
| Verschlüsselung auf Spaltenebene (Azure Data Services)| Ja | Nur in der Tabellen-API (Premium). Diese Funktion wird nicht von allen APIs unterstützt. Siehe [Einführung in Azure Cosmos DB: Tabellen-API](table-introduction.md). |
| Verschlüsselung während der Übertragung (z. B. ExpressRoute-Verschlüsselung, Verschlüsselung im VNET und VNET-zu-VNET-Verschlüsselung)| Ja | Alle Azure Cosmos DB-Daten werden während der Übertragung verschlüsselt. |
| Verschlüsselte API-Aufrufe| Ja | Alle Verbindungen mit Azure Cosmos DB unterstützen HTTPS. Azure Cosmos DB unterstützt auch TLS 1.2-Verbindungen, dies ist jedoch noch keine zwingende Voraussetzung. Wenn Kunden Low-Level-TLS auf ihrer Seite deaktivieren, können sie sicherstellen, dass Verbindungen mit Cosmos DB hergestellt werden.  |

## <a name="configuration-management"></a>Konfigurationsverwaltung

| Sicherheitskontrolle | Ja/Nein | Notizen|
|---|---|--|
| Unterstützung der Konfigurationsverwaltung (Versionsverwaltung der Konfiguration usw.)| Nein  | | 

## <a name="additional-security-controls-for-cosmos-db"></a>Zusätzliche Sicherheitskontrollen für Cosmos DB

| Sicherheitskontrolle | Ja/Nein | Notizen|
|---|---|--|
| Ressourcenfreigabe zwischen verschiedenen Ursprüngen (CORS) | Ja | Siehe [Konfigurieren der Ressourcenfreigabe zwischen verschiedenen Ursprüngen (CORS)](how-to-configure-cross-origin-resource-sharing.md). |

## <a name="next-steps"></a>Nächste Schritte

- Erfahren Sie mehr über die [integrierten Sicherheitskontrollen in Azure-Diensten](../security/fundamentals/security-controls.md).