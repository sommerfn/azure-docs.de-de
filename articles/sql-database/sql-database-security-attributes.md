---
title: Sicherheitsattribute für Azure SQL-Datenbank
description: Eine Prüfliste der Sicherheitsattribute für die Auswertung von Azure SQL-Datenbank
services: sql-database
author: msmbaldwin
manager: barbkess
ms.service: load-balancer
ms.topic: conceptual
ms.date: 05/06/2019
ms.author: mbaldwin
ms.openlocfilehash: 71e87a3295a9cd73dca2f97b3fa04a5d5ff76f84
ms.sourcegitcommit: 66237bcd9b08359a6cce8d671f846b0c93ee6a82
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 07/11/2019
ms.locfileid: "67798690"
---
# <a name="security-attributes-for-azure-sql-database"></a>Sicherheitsattribute für Azure SQL-Datenbank

In diesem Artikel werden die allgemeinen in Azure SQL-Datenbank integrierten Sicherheitsattribute beschrieben.

[!INCLUDE [Security Attributes Header](../../includes/security-attributes-header.md)]

SQL-Datenbank umfasst Konzepte für [Einzeldatenbanken](sql-database-single-index.yml) und für [verwaltete Instanzen](sql-database-managed-instance.md). Die folgenden Einträge gelten für beide Angebote, sofern nichts anderes angegeben ist.

## <a name="preventative"></a>Prävention

| Sicherheitsattribut | Ja/Nein | Notizen |
|---|---|--|
| Verschlüsselung ruhender Daten:<ul><li>Serverseitige Verschlüsselung</li><li>Serverseitige Verschlüsselung mit vom Kunden verwalteten Schlüsseln</li><li>Weitere Verschlüsselungsfunktionen, z. B. clientseitig oder Always Encrypted</ul>| Ja | Wird als „Verschlüsselung während der Verwendung“ bezeichnet, wie im Artikel [Always Encrypted](sql-database-always-encrypted.md) beschrieben. Dienstseitige Verschlüsselung verwendet die [transparente Datenverschlüsselung](transparent-data-encryption-azure-sql.md).|
| Verschlüsselung während der Übertragung:<ul><li>Azure ExpressRoute-Verschlüsselung</li><li>Verschlüsselung in einem virtuellen Netzwerk</li><li>Verschlüsselung zwischen virtuellen Netzwerken</ul>| Ja | Mithilfe von HTTPS |
| Behandeln von Verschlüsselungsschlüsseln, wie z. B. CMK oder BYOK| Ja | Es wird sowohl die Verarbeitung von vom Dienst verwalteten Schlüsseln als auch die Verarbeitung von vom Kunden verwalteten Schlüsseln angeboten. Letztere Option wird über [Azure Key Vault](../key-vault/index.yml) angeboten. |
| Verschlüsselung auf Spaltenebene, die von Azure Data Services bereitgestellt wird| Ja | Durch [Always Encrypted](sql-database-always-encrypted.md). |
| Verschlüsselte API-Aufrufe| Ja | Mithilfe von HTTPS/SSL. |

## <a name="network-segmentation"></a>Netzwerksegmentierung

| Sicherheitsattribut | Ja/Nein | Notizen |
|---|---|--|
| Unterstützung des Dienstendpunkts| Ja | Gilt nur für eine [Einzeldatenbank](sql-database-single-index.yml). |
| Unterstützung für das Einfügen in Azure Virtual Network| Ja | Gilt nur für eine [verwaltete Instanz](sql-database-managed-instance.md). |
| Netzwerkisolation und Firewallunterstützung| Ja | Firewall sowohl auf Datenbankebene als auch auf Serverebene. Netzwerkisolation wird ausschließlich für die [verwaltete Instanz](sql-database-managed-instance.md) angeboten. |
| Unterstützung der Tunnelerzwingung| Ja | [Verwaltete Instanz](sql-database-managed-instance.md) über [Azure ExpressRoute](../expressroute/index.yml)-VPN. |

## <a name="detection"></a>Erkennung

| Sicherheitsattribut | Ja/Nein | Notizen|
|---|---|--|
| Unterstützung der Azure-Überwachung, z. B. Protokollanalyse oder Application Insights| Ja | SecureSphere, die SIEM-Lösung von Imperva, wird ebenfalls unterstützt, und zwar mittels [Azure Event Hubs](../event-hubs/index.yml)-Integration über [SQL-Überwachung](sql-database-auditing.md). |

## <a name="identity-and-access-management"></a>Identitäts- und Zugriffsverwaltung

| Sicherheitsattribut | Ja/Nein | Notizen|
|---|---|--|
| Authentication| Ja | Azure Active Directory (Azure AD) |
| Authorization| Ja | Keine |

## <a name="audit-trail"></a>Überwachungspfad

| Sicherheitsattribut | Ja/Nein | Notizen|
|---|---|--|
| Protokollierung und Überwachung auf Steuerungs- und Verwaltungsebene| Ja | Ja, für einige Ereignisse |
| Protokollierung und Überwachung auf Datenebene | Ja | Über [SQL-Überwachung](sql-database-auditing.md) |

## <a name="configuration-management"></a>Konfigurationsverwaltung

| Sicherheitsattribut | Ja/Nein | Notizen|
|---|---|--|
| Unterstützung der Konfigurationsverwaltung, z. B. Versionsverwaltung der Konfiguration| Nein  | Keine |

## <a name="additional-security-attributes-for-sql-database"></a>Zusätzliche Sicherheitsattribute für SQL-Datenbank

| Sicherheitsattribut | Ja/Nein | Notizen|
|---|---|--|
| Prävention: Sicherheitsrisikobewertung | Ja | Weitere Informationen finden Sie unter [Mit dem Dienst zur SQL-Sicherheitsrisikobewertung können Sie Datenbankschwachstellen erkennen](sql-vulnerability-assessment.md). |
| Prävention: Datenermittlung und -klassifizierung  | Ja | Weitere Informationen finden Sie unter [Azure SQL-Datenbank und SQL Data Warehouse: Datenermittlung und -klassifizierung](sql-database-data-discovery-and-classification.md). |
| Erkennung: Bedrohungserkennung | Ja | Weitere Informationen finden Sie unter [Advanced Threat Protection für Azure SQL-Datenbank](sql-database-threat-detection-overview.md). |
