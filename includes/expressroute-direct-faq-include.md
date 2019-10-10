---
title: include file
description: include file
services: expressroute
author: jaredr80
ms.service: expressroute
ms.topic: include
ms.date: 10/07/2019
ms.author: jaredro
ms.custom: include file
ms.openlocfilehash: 9c53aac47e965a6eba681008d6b6476142190ef8
ms.sourcegitcommit: be344deef6b37661e2c496f75a6cf14f805d7381
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 10/07/2019
ms.locfileid: "72023082"
---
### <a name="what-is-expressroute-direct"></a>Was ist ExpressRoute Direct?

ExpressRoute Direct bietet Kunden die Möglichkeit, sich direkt mit dem globalen Netzwerk von Microsoft zu verbinden, und zwar an strategisch über die ganze Welt verteilten Peeringstandorten. ExpressRoute Direct bietet duale Konnektivität mit 100 oder 10 GBit/s, die eine Aktiv/Aktiv-Konnektivität nach Bedarf unterstützt. 

### <a name="how-do-customers-connect-to-expressroute-direct"></a>Wie stellen Kunden eine Verbindung mit ExpressRoute Direct her? 

Kunden müssen mit ihren lokalen Carriern und Co-Location-Anbietern zusammenarbeiten, um eine Verbindung mit ExpressRoute-Routern herzustellen, um die Vorteile von ExpressRoute Direct nutzen zu können.

### <a name="what-locations-currently-support-expressroute-direct"></a>Welche Standorte unterstützen derzeit ExpressRoute Direct? 

Die verfügbaren Ports sind dynamisch und werden in PowerShell verfügbar sein, um die Kapazität anzuzeigen. Die folgenden Standorte sind geplant. Sie können sich *je nach Verfügbarkeit ändern*:

* Amsterdam
* Amsterdam2
* Auckland 
* Chicago
* Dallas
* Dublin
* Hongkong (SAR)
* London
* London2
* Melbourne
* New York City
* Perth
* San Antonio
* Seattle
* Seoul
* Silicon Valley
* Singapur2 
* Sydney
* Taipeh
* Tokio
* Toronto
* Washington DC
* Washington DC2

### <a name="what-is-the-sla-for-expressroute-direct"></a>Welche SLA gilt für ExpressRoute Direct?

ExpressRoute Direct verwendet die gleiche [Unternehmensstufe wie ExpressRoute](https://azure.microsoft.com/support/legal/sla/expressroute/v1_3/). 

### <a name="what-scenarios-should-customers-consider-with-expressroute-direct"></a>Welche Szenarien sollten Kunden mit ExpressRoute Direct in Betracht ziehen?  

ExpressRoute Direct bietet Kunden direkte Portpaare mit 100 oder 10 GBit/s in den globalen Microsoft-Backbone. Zu den Szenarien, die Kunden den größten Nutzen bieten, gehören: massive Datenerfassung, physische Isolierung für regulierte Märkte und dedizierte Kapazität für Burstszenarien wie Rendering. 

### <a name="what-is-the-billing-model-for-expressroute-direct"></a>Welches Abrechnungsmodell gilt für ExpressRoute Direct? 

ExpressRoute Direct wird für das Portpaar zu einem festen Betrag in Rechnung gestellt. Standardverbindungen sind ohne zusätzliche Stunden enthalten, und Premium wird mit einem leichten Aufpreis berechnet. Ausgehende Datenübertragungen werden pro Verbindung basierend auf der Zone des Peeringstandorts abgerechnet.

### <a name="when-does-billing-start-for-the-expressroute-direct-port-pairs"></a>Wann beginnt die Abrechnung für die ExpressRoute Direct-Portpaare?

ExpressRoute Direct-Portpaare werden 45 Tage nach Beginn der Erstellung der ExpressRoute Direct-Ressource berechnet, oder wenn 1 oder beide Links aktiviert werden, was auch immer zuerst eintritt. Die 45-Tage-Karenzzeit wird Kunden gewährt, damit diese den Querverbindungsprozess mit dem Housinganbieter abschließen können.
