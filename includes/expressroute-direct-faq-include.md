---
title: include file
description: include file
services: expressroute
author: jaredr80
ms.service: expressroute
ms.topic: include
ms.date: 10/29/2019
ms.author: jaredro
ms.custom: include file
ms.openlocfilehash: 1f6c65d6d8da275d6c52ac2802092dbb5e4e507d
ms.sourcegitcommit: 0b1a4101d575e28af0f0d161852b57d82c9b2a7e
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 10/30/2019
ms.locfileid: "73182214"
---
### <a name="what-is-expressroute-direct"></a>Was ist ExpressRoute Direct?

ExpressRoute Direct bietet Kunden die Möglichkeit, sich direkt mit dem globalen Netzwerk von Microsoft zu verbinden, und zwar an strategisch über die ganze Welt verteilten Peeringstandorten. ExpressRoute Direct bietet duale Konnektivität mit 100 oder 10 GBit/s, die eine Aktiv/Aktiv-Konnektivität nach Bedarf unterstützt. 

### <a name="how-do-customers-connect-to-expressroute-direct"></a>Wie stellen Kunden eine Verbindung mit ExpressRoute Direct her? 

Kunden müssen mit ihren lokalen Carriern und Co-Location-Anbietern zusammenarbeiten, um eine Verbindung mit ExpressRoute-Routern herzustellen, um die Vorteile von ExpressRoute Direct nutzen zu können.

### <a name="what-locations-currently-support-expressroute-direct"></a>Welche Standorte unterstützen derzeit ExpressRoute Direct? 

Überprüfen Sie die Verfügbarkeit auf der [Seite mit Standorten](../articles/expressroute/expressroute-locations-providers.md). 

### <a name="what-is-the-sla-for-expressroute-direct"></a>Welche SLA gilt für ExpressRoute Direct?

ExpressRoute Direct verwendet die gleiche [Unternehmensstufe wie ExpressRoute](https://azure.microsoft.com/support/legal/sla/expressroute/v1_3/). 

### <a name="what-scenarios-should-customers-consider-with-expressroute-direct"></a>Welche Szenarien sollten Kunden mit ExpressRoute Direct in Betracht ziehen?  

ExpressRoute Direct bietet Kunden direkte Portpaare mit 100 oder 10 GBit/s in den globalen Microsoft-Backbone. Zu den Szenarien, die Kunden den größten Nutzen bieten, gehören: massive Datenerfassung, physische Isolierung für regulierte Märkte und dedizierte Kapazität für Burstszenarien wie Rendering. 

### <a name="what-is-the-billing-model-for-expressroute-direct"></a>Welches Abrechnungsmodell gilt für ExpressRoute Direct? 

ExpressRoute Direct wird für das Portpaar zu einem festen Betrag in Rechnung gestellt. Standardverbindungen sind ohne zusätzliche Stunden enthalten, und Premium wird mit einem leichten Aufpreis berechnet. Ausgehende Datenübertragungen werden pro Verbindung basierend auf der Zone des Peeringstandorts abgerechnet.

### <a name="when-does-billing-start-for-the-expressroute-direct-port-pairs"></a>Wann beginnt die Abrechnung für die ExpressRoute Direct-Portpaare?

ExpressRoute Direct-Portpaare werden 45 Tage nach Beginn der Erstellung der ExpressRoute Direct-Ressource berechnet, oder wenn 1 oder beide Links aktiviert werden, was auch immer zuerst eintritt. Die 45-Tage-Karenzzeit wird Kunden gewährt, damit diese den Querverbindungsprozess mit dem Housinganbieter abschließen können.
