---
title: Sicherheitskontrollen
description: Eine Prüfliste mit Sicherheitskontrollen zur Evaluierung von Azure SQL-Datenbank
services: sql-database
author: msmbaldwin
manager: rkalrin
ms.service: load-balancer
ms.topic: conceptual
ms.date: 09/04/2019
ms.author: mbaldwin
ms.openlocfilehash: 58070cab1221b9d9585784d82cf1a48afcedb8af
ms.sourcegitcommit: ac56ef07d86328c40fed5b5792a6a02698926c2d
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 11/08/2019
ms.locfileid: "73802840"
---
# <a name="security-controls-for-azure-sql-database"></a>Sicherheitskontrollen für Azure SQL-Datenbank

In diesem Artikel werden die in Azure SQL-Datenbank integrierten Sicherheitskontrollen beschrieben.

[!INCLUDE [Security controls Header](../../includes/security-controls-header.md)]

SQL-Datenbank umfasst Konzepte für [Einzeldatenbanken](sql-database-single-index.yml) und für [verwaltete Instanzen](sql-database-managed-instance.md). Die folgenden Einträge gelten für beide Angebote, sofern nichts anderes angegeben ist.

## <a name="network"></a>Netzwerk

| Sicherheitskontrolle | Ja/Nein | Notizen |
|---|---|--|
| Unterstützung des Dienstendpunkts| Ja | Gilt nur für eine [Einzeldatenbank](sql-database-single-index.yml). |
| Unterstützung für das Einfügen in Azure Virtual Network| Ja | Gilt nur für eine [verwaltete Instanz](sql-database-managed-instance.md). |
| Netzwerkisolation und Firewallunterstützung| Ja | Firewall sowohl auf Datenbankebene als auch auf Serverebene. Netzwerkisolation wird ausschließlich für die [verwaltete Instanz](sql-database-managed-instance.md) angeboten. |
| Unterstützung der Tunnelerzwingung| Ja | [Verwaltete Instanz](sql-database-managed-instance.md) über [Azure ExpressRoute](../expressroute/index.yml)-VPN. |

## <a name="monitoring--logging"></a>Überwachung und Protokollierung

| Sicherheitskontrolle | Ja/Nein | Notizen|
|---|---|--|
| Unterstützung der Azure-Überwachung, z. B. Protokollanalyse oder Application Insights| Ja | SecureSphere, die SIEM-Lösung von Imperva, wird ebenfalls unterstützt, und zwar mittels [Azure Event Hubs](../event-hubs/index.yml)-Integration über [SQL-Überwachung](sql-database-auditing.md). |
| Protokollierung und Überwachung auf Steuerungs- und Verwaltungsebene| Ja | Ja, für einige Ereignisse |
| Protokollierung und Überwachung auf Datenebene | Ja | Über [SQL-Überwachung](sql-database-auditing.md) |

## <a name="identity"></a>Identity

| Sicherheitskontrolle | Ja/Nein | Notizen|
|---|---|--|
| Authentication| Ja | Azure Active Directory (Azure AD) |
| Authorization| Ja | Keine |

## <a name="data-protection"></a>Datenschutz

| Sicherheitskontrolle | Ja/Nein | Notizen |
|---|---|--|
| Serverseitige Verschlüsselung ruhender Daten: Von Microsoft verwaltete Schlüssel | Ja | Wird als „Verschlüsselung während der Verwendung“ bezeichnet, wie im Artikel [Always Encrypted](sql-database-always-encrypted.md) beschrieben. Dienstseitige Verschlüsselung verwendet die [transparente Datenverschlüsselung](transparent-data-encryption-azure-sql.md).|
| Verschlüsselung während der Übertragung:<ul><li>Azure ExpressRoute-Verschlüsselung</li><li>Verschlüsselung in einem virtuellen Netzwerk</li><li>Verschlüsselung zwischen virtuellen Netzwerken</ul>| Ja | Mithilfe von HTTPS |
| Behandeln von Verschlüsselungsschlüsseln, wie z. B. CMK oder BYOK| Ja | Es wird sowohl die Verarbeitung von vom Dienst verwalteten Schlüsseln als auch die Verarbeitung von vom Kunden verwalteten Schlüsseln angeboten. Letztere Option wird über [Azure Key Vault](../key-vault/index.yml) angeboten. |
| Verschlüsselung auf Spaltenebene, die von Azure Data Services bereitgestellt wird| Ja | Durch [Always Encrypted](sql-database-always-encrypted.md). |
| Verschlüsselte API-Aufrufe| Ja | Mithilfe von HTTPS/SSL. |

## <a name="configuration-management"></a>Konfigurationsverwaltung

| Sicherheitskontrolle | Ja/Nein | Notizen|
|---|---|--|
| Unterstützung der Konfigurationsverwaltung, z. B. Versionsverwaltung der Konfiguration| Nein  | Keine |

## <a name="additional-security-controls-for-sql-database"></a>Zusätzliche Sicherheitskontrollen für die SQL-Datenbank

| Sicherheitskontrolle | Ja/Nein | Notizen|
|---|---|--|
| Prävention: Sicherheitsrisikobewertung | Ja | Weitere Informationen finden Sie unter [Mit dem Dienst zur SQL-Sicherheitsrisikobewertung können Sie Datenbankschwachstellen erkennen](sql-vulnerability-assessment.md). |
| Prävention: Datenermittlung und -klassifizierung  | Ja | Weitere Informationen finden Sie unter [Azure SQL-Datenbank und SQL Data Warehouse: Datenermittlung und -klassifizierung](sql-database-data-discovery-and-classification.md). |
| Erkennung: Bedrohungserkennung | Ja | Weitere Informationen finden Sie unter [Advanced Threat Protection für Azure SQL-Datenbank](sql-database-threat-detection-overview.md). |

## <a name="next-steps"></a>Nächste Schritte

- Erfahren Sie mehr über die [integrierten Sicherheitskontrollen in Azure-Diensten](../security/fundamentals/security-controls.md).
