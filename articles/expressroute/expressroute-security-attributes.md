---
title: Häufig verwendete Sicherheitsattribute für Azure ExpressRoute
description: Eine Prüfliste allgemeiner Sicherheitsattribute für die Auswertung von Azure ExpressRoute
services: expressroute
ms.service: expressroute
documentationcenter: ''
author: msmbaldwin
manager: barbkess
ms.topic: conceptual
ms.date: 06/05/2019
ms.author: mbaldwin
ms.openlocfilehash: d6156715fb87831d465197fd8eec59d245221e48
ms.sourcegitcommit: 41ca82b5f95d2e07b0c7f9025b912daf0ab21909
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 06/13/2019
ms.locfileid: "67082211"
---
# <a name="common-security-attributes-for-azure-expressroute"></a>Häufig verwendete Sicherheitsattribute für Azure ExpressRoute

Sicherheit ist ein wesentlicher Bestandteil jedes Azure-Diensts. In diesem Artikel werden die in Azure ExpressRoute integrierten Sicherheitsattribute beschrieben.

[!INCLUDE [Security Attributes Header](../../includes/security-attributes-header.md)]

## <a name="preventative"></a>Prävention

| Sicherheitsattribut | Ja/Nein | Notizen |
|---|---|--|
| Verschlüsselung ruhender Daten:<ul><li>Serverseitige Verschlüsselung</li><li>Serverseitige Verschlüsselung mit vom Kunden verwalteten Schlüsseln</li><li>Weitere Verschlüsselungsfunktionen (z. B. clientseitig, immer verschlüsselt usw.)</ul>|  – | ExpressRoute speichert keine Kundendaten. |
| Verschlüsselung während der Übertragung:<ul><li>ExpressRoute-Verschlüsselung</li><li>In VNet-Verschlüsselung</li><li>VNet-VNet-Verschlüsselung</ul>| Nein | |
| Verarbeitung von Verschlüsselungsschlüsseln (CMK, BYOK usw.)| – |  |
| Verschlüsselung auf Spaltenebene (Azure Data Services)| – | |
| Verschlüsselte API-Aufrufe| Ja | Über [Azure Resource Manager](../azure-resource-manager/index.yml) und HTTPS. |

## <a name="network-segmentation"></a>Netzwerksegmentierung

| Sicherheitsattribut | Ja/Nein | Notizen |
|---|---|--|
| Unterstützung des Dienstendpunkts| – |  |
| Unterstützung der vNET-Einschleusung| – | |
| Unterstützung von Netzwerkisolation und Firewall| Ja | Jeder Kunde ist in seiner eigenen Routingdomäne enthalten und wird zu seinem eigenen VNET getunnelt |
| Unterstützung der Tunnelerzwingung| – | Über Border Gateway Protocol (BGP). |

## <a name="detection"></a>Erkennung

| Sicherheitsattribut | Ja/Nein | Notizen|
|---|---|--|
| Unterstützung der Azure-Überwachung (Log Analytics, Application Insights usw.)| Ja | Siehe [ExpressRoute-Überwachung, Metriken und Warnungen](expressroute-monitoring-metrics-alerts.md).|

## <a name="identity-and-access-management"></a>Identitäts- und Zugriffsverwaltung

| Sicherheitsattribut | Ja/Nein | Notizen|
|---|---|--|
| Authentication| Ja | Dienstkonto für Gateway für Microsoft (GWM), Controller; JIT-Zugriff (Just-in-Time) für Entwicklung und Betrieb. |
| Autorisierung|  Ja |Dienstkonto für Gateway für Microsoft (GWM), Controller; JIT-Zugriff (Just-in-Time) für Entwicklung und Betrieb. |


## <a name="audit-trail"></a>Überwachungspfad

| Sicherheitsattribut | Ja/Nein | Notizen| 
|---|---|--|
| Protokollierung und Überwachung auf Steuerungs- und Verwaltungsebene| Ja |  |
| Protokollierung und Überwachung auf Datenebene| Nein |   |

## <a name="configuration-management"></a>Konfigurationsverwaltung

| Sicherheitsattribut | Ja/Nein | Notizen|
|---|---|--|
| Unterstützung der Konfigurationsverwaltung (Versionsverwaltung der Konfiguration usw.)| Ja | Über Netzwerkressourcenanbieter (Network Resource Provider, NRP). |
