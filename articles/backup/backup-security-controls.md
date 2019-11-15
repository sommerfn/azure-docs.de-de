---
title: Sicherheitskontrollen für Azure Backup
description: Erfahren Sie mehr über die Sicherheitskontrollen, die im Azure Backup-Dienst verwendet werden. Diese Kontrollen helfen dem Dienst, Sicherheitsrisiken zu verhindern, zu erkennen und darauf zu reagieren.
ms.reviewer: mbaldwin
author: dcurwin
manager: carmonm
ms.service: backup
ms.topic: conceptual
ms.date: 09/23/2019
ms.author: dacurwin
ms.openlocfilehash: 9d7bb2e9ea2d2cf9cce6c4f6d3b4f3346e7ce363
ms.sourcegitcommit: 827248fa609243839aac3ff01ff40200c8c46966
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 11/07/2019
ms.locfileid: "73747148"
---
# <a name="security-controls-for-azure-backup"></a>Sicherheitskontrollen für Azure Backup

In diesem Artikel werden die in Azure Backup integrierten Sicherheitskontrollen beschrieben.

[!INCLUDE [Security controls Header](../../includes/security-controls-header.md)]

## <a name="network"></a>Netzwerk

| Sicherheitskontrolle | Ja/Nein | Notizen | Dokumentation
|---|---|--|
| Unterstützung des Dienstendpunkts| Nein |  |  |
| Unterstützung der VNet-Einschleusung| Nein |  |  |
| Unterstützung von Netzwerkisolation und Firewall| Ja | Die Tunnelerzwingung wird für VM-Sicherungen unterstützt. Die Tunnelerzwingung wird für Workloads in virtuellen Computern nicht unterstützt. |  |
| Unterstützung der Tunnelerzwingung| Nein |  |  |

## <a name="monitoring--logging"></a>Überwachung und Protokollierung

| Sicherheitskontrolle | Ja/Nein | Notizen| | Dokumentation
|---|---|--|
| Unterstützung der Azure-Überwachung (Log Analytics, Application Insights usw.)| Ja | Log Analytics wird über Ressourcenprotokolle unterstützt. Weitere Informationen finden Sie unter [Überwachen von Azure Backup-geschützten Workloads mit Log Analytics](https://azure.microsoft.com/blog/monitor-all-azure-backup-protected-workloads-using-log-analytics/). |  |
| Protokollierung und Überwachung auf Steuerungs- und Verwaltungsebene| Ja | Alle vom Kunden über das Azure-Portal ausgelösten Aktionen werden in Aktivitätsprotokollen protokolliert. |  |
| Protokollierung und Überwachung auf Datenebene| Nein | Die Azure Backup-Datenebene kann nicht direkt aufgerufen werden.  |  |

## <a name="identity"></a>Identity

| Sicherheitskontrolle | Ja/Nein | Notizen| | Dokumentation
|---|---|--|
| Authentication| Ja | Die Authentifizierung erfolgt über Azure Active Directory. |  |
| Authorization| Ja | Vom Kunden erstellte und integrierte RBAC-Rollen werden verwendet. Weitere Informationen finden Sie unter [Verwenden der rollenbasierten Zugriffssteuerung zum Verwalten von Azure Backup-Wiederherstellungspunkten](/azure/backup/backup-rbac-rs-vault). |  |

## <a name="data-protection"></a>Datenschutz

| Sicherheitskontrolle | Ja/Nein | Notizen | | Dokumentation
|---|---|--|
| Serverseitige Verschlüsselung ruhender Daten: Von Microsoft verwaltete Schlüssel | Ja | Verwenden der Speicherdienstverschlüsselung für Speicherkonten |  |
| Serverseitige Verschlüsselung ruhender Daten: vom Kunden verwaltete Schlüssel (BYOK) | Nein |  |  |
| Verschlüsselung auf Spaltenebene (Azure Data Services)| Nein |  |  |
| Verschlüsselung während der Übertragung (z. B. ExpressRoute-Verschlüsselung, VNET-Verschlüsselung und VNET-VNET-Verschlüsselung)| Nein | Mithilfe von HTTPS |  |
| Verschlüsselte API-Aufrufe| Ja |  |  |

## <a name="configuration-management"></a>Konfigurationsverwaltung

| Sicherheitskontrolle | Ja/Nein | Notizen| | Dokumentation
|---|---|--|
| Unterstützung der Konfigurationsverwaltung (Versionsverwaltung der Konfiguration usw.)| Ja|  |  |

## <a name="next-steps"></a>Nächste Schritte

- Erfahren Sie mehr über die [integrierten Sicherheitskontrollen in Azure-Diensten](../security/fundamentals/security-controls.md).
