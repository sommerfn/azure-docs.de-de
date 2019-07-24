---
title: Sicherheitsattribute für Azure Load Balancer
description: Eine Prüfliste der Sicherheitsattribute für die Auswertung von Load Balancer
services: load-balancer
author: msmbaldwin
manager: barbkess
ms.service: load-balancer
ms.topic: conceptual
ms.date: 06/10/2019
ms.author: mbaldwin
ms.openlocfilehash: 1892b918ed35221bc73d5070d5a73ecc359aa4c2
ms.sourcegitcommit: 66237bcd9b08359a6cce8d671f846b0c93ee6a82
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 07/11/2019
ms.locfileid: "67799859"
---
# <a name="security-attributes-for-azure-load-balancer"></a>Sicherheitsattribute für Azure Load Balancer

In diesem Artikel werden die in Azure Load Balancer integrierten allgemeinen Sicherheitsattribute beschrieben.

[!INCLUDE [Security Attributes Header](../../includes/security-attributes-header.md)]

## <a name="preventative"></a>Prävention

| Sicherheitsattribut | Ja/Nein | Notizen |
|---|---|--|
| Verschlüsselung ruhender Daten (z. B. serverseitige Verschlüsselung, serverseitige Verschlüsselung mit vom Kunden verwalteten Schlüsseln und andere Verschlüsselungsfunktionen) | – | |
| Verschlüsselung während der Übertragung (z. B. ExpressRoute-Verschlüsselung, Verschlüsselung im VNET und VNET-zu-VNET-Verschlüsselung)| – | |
| Verarbeitung von Verschlüsselungsschlüsseln (CMK, BYOK usw.)| – | |
| Verschlüsselung auf Spaltenebene (Azure Data Services)| – | |
| Verschlüsselte API-Aufrufe| Ja | Über [Azure Resource Manager](../azure-resource-manager/index.yml) |

## <a name="network-segmentation"></a>Netzwerksegmentierung

| Sicherheitsattribut | Ja/Nein | Notizen |
|---|---|--|
| Unterstützung des Dienstendpunkts| – | |
| Unterstützung der VNet-Einschleusung| – | . |
| Netzwerkisolation und Firewallunterstützung| – |  |
| Unterstützung der Tunnelerzwingung| – | |

## <a name="detection"></a>Erkennung

| Sicherheitsattribut | Ja/Nein | Notizen|
|---|---|--|
| Unterstützung der Azure-Überwachung (Log Analytics, Application Insights usw.)| Ja | Siehe [Azure Monitor-Protokolle für öffentlichen Load Balancer vom Typ „Basic“](load-balancer-monitor-log.md) |

## <a name="identity-and-access-management"></a>Identitäts- und Zugriffsverwaltung

| Sicherheitsattribut | Ja/Nein | Notizen|
|---|---|--|
| Authentication| – |  |
| Authorization| – |  |


## <a name="audit-trail"></a>Überwachungspfad

| Sicherheitsattribut | Ja/Nein | Notizen|
|---|---|--|
| Protokollierung und Überwachung auf Steuerungs- und Verwaltungsebene| Ja | Siehe [Azure Monitor-Protokolle für öffentlichen Load Balancer vom Typ „Basic“](load-balancer-monitor-log.md) |
| Protokollierung und Überwachung auf Datenebene | – |  |

## <a name="configuration-management"></a>Konfigurationsverwaltung

| Sicherheitsattribut | Ja/Nein | Notizen|
|---|---|--|
| Unterstützung der Konfigurationsverwaltung (Versionsverwaltung der Konfiguration usw.)| – |  | 
