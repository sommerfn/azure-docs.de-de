---
title: Allgemeine Sicherheitsattribute für Azure Resource Manager
description: Eine Prüfliste allgemeiner Sicherheitsattribute für die Bewertung von Azure Resource Manager
services: azure-resource-manager
author: msmbaldwin
manager: barbkess
ms.service: azure-resource-manager
ms.topic: conceptual
ms.date: 04/25/2019
ms.author: mbaldwin
ms.openlocfilehash: a771d4c2ae22b7bf149c13c80fe5286ef52a4545
ms.sourcegitcommit: d4dfbc34a1f03488e1b7bc5e711a11b72c717ada
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 06/13/2019
ms.locfileid: "66002255"
---
# <a name="security-attributes-for-azure-resource-manager"></a>Sicherheitsattribute für Azure Resource Manager

In diesem Artikel werden die in Azure Resource Manager integrierten Sicherheitsattribute beschrieben.

[!INCLUDE [Security Attributes Header](../../includes/security-attributes-header.md)]

## <a name="preventative"></a>Prävention

| Sicherheitsattribut | Ja/Nein | Notizen |
|---|---|--|
| Verschlüsselung ruhender Daten:<ul><li>Serverseitige Verschlüsselung</li><li>Serverseitige Verschlüsselung mit vom Kunden verwalteten Schlüsseln</li><li>Weitere Verschlüsselungsfunktionen (z. B. clientseitig, immer verschlüsselt usw.)</ul>| Ja |  |
| Verschlüsselung während der Übertragung:<ul><li>ExpressRoute-Verschlüsselung</li><li>In VNet-Verschlüsselung</li><li>VNet-VNet-Verschlüsselung</ul>| Ja | HTTPS/TLS. |
| Verarbeitung von Verschlüsselungsschlüsseln (CMK, BYOK usw.)| – | Azure Resource Manager speichert keine Kundeninhalte, sondern nur Steuerdaten. |
| Verschlüsselung auf Spaltenebene (Azure Data Services)| Ja | |
| Verschlüsselte API-Aufrufe| Ja | |

## <a name="network-segmentation"></a>Netzwerksegmentierung

| Sicherheitsattribut | Ja/Nein | Notizen |
|---|---|--|
| Unterstützung des Dienstendpunkts| Nein | |
| Unterstützung der VNET-Einschleusung| Ja | |
| Unterstützung von Netzwerkisolation und Firewall| Nein |  |
| Unterstützung der Tunnelerzwingung| Nein |  |

## <a name="detection"></a>Erkennung

| Sicherheitsattribut | Ja/Nein | Notizen|
|---|---|--|
| Unterstützung der Azure-Überwachung (Log Analytics, Application Insights usw.)| Nein | |

## <a name="identity-and-access-management"></a>Identitäts- und Zugriffsverwaltung

| Sicherheitsattribut | Ja/Nein | Notizen|
|---|---|--|
| Authentication| Ja | Basierend auf [Azure Active Directory](/azure/active-directory).|
| Autorisierung| Ja | |


## <a name="audit-trail"></a>Überwachungspfad

| Sicherheitsattribut | Ja/Nein | Notizen|
|---|---|--|
| Protokollierung und Überwachung auf Steuerungs- und Verwaltungsebene| Ja | Aktivitätsprotokolle machen alle Schreibvorgänge (PUT, POST, DELETE) sichtbar, die auf Ihre Ressourcen angewendet werden. Siehe [Anzeigen von Aktivitätsprotokollen zu Überwachungsaktionen für Ressourcen](resource-group-audit.md). |
| Protokollierung und Überwachung auf Datenebene| – | |

## <a name="configuration-management"></a>Konfigurationsverwaltung

| Sicherheitsattribut | Ja/Nein | Notizen|
|---|---|--|
| Unterstützung der Konfigurationsverwaltung (Versionsverwaltung der Konfiguration usw.)| Ja |  |
