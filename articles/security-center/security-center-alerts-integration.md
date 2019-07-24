---
title: Security Center-Integration mit Azure-Sicherheitsprodukten | Microsoft-Dokumentation
description: In diesem Thema werden Azure-Sicherheitsprodukte mit Azure Security Center-Integration vorgestellt.
services: security-center
documentationcenter: na
author: monhaber
manager: rkarlin
editor: ''
ms.assetid: ad4b0373-08ee-46ca-a573-638ed93a647c
ms.service: security-center
ms.devlang: na
ms.topic: conceptual
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 7/02/2019
ms.author: monhaber
ms.openlocfilehash: 805c770f1a7e9bb4e0619b27ac937a2451421dc6
ms.sourcegitcommit: 1e347ed89854dca2a6180106228bfafadc07c6e5
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 07/05/2019
ms.locfileid: "67571332"
---
# <a name="security-center-integration-with-azure-security-products-in-asc"></a>Security Center-Integration mit Azure-Sicherheitsprodukten in ASC

Security Center bietet Kunden zusätzliche Microsoft-Lizenzen, um ihre Ergebnisse in Security Center integrieren und in konsolidierter Form betrachten zu können.

* [Azure WAF](#azure-waf)
* [Azure DDoS](#azure-ddos)

## Azure WAF <a name="azure-waf"></a>

Azure Application Gateway verfügt über eine Web Application Firewall (WAF), die den zentralisierten Schutz Ihrer Webanwendungen vor allgemeinen Exploits und Sicherheitsrisiken ermöglicht.

Webanwendungen sind zunehmend Ziele böswilliger Angriffe, die allgemein bekannte Sicherheitslücken ausnutzen. Die Application Gateway-WAF basiert auf Version 3.0 oder 2.2.9 des Kernregelsatzes (Core Rule Set, CRS) aus dem Open Web Application Security Project (OWASP). Die WAF wird automatisch aktualisiert und bietet ganz ohne zusätzliche Konfiguration Schutz vor neuen Sicherheitsrisiken. Durch die WAF generierte Warnungen werden an Security Center gestreamt. Weitere Informationen zu den Warnungen, die durch die WAF generiert werden, finden Sie in [diesem Artikel](https://docs.microsoft.com/azure/application-gateway/application-gateway-crs-rulegroups-rules?tabs=owasp3#crs911).

## Azure DDoS <a name="azure-ddos"></a>

Verteilte Denial-of-Service-Angriffe (Distributed Denial of Service, DDoS) sind bekanntlich einfach durchführbar. Dadurch haben sie sich zu einem wichtigen Sicherheitsthema für Kunden entwickelt, die ihre Anwendungen zur Cloud migrieren. 

Ein DDoS-Angriff hat das Ziel, die Ressourcen einer Anwendung zu verbrauchen, damit sie für berechtigte Benutzer nicht mehr verfügbar ist. DDoS-Angriffe können jeden beliebigen Endpunkt ins Visier nehmen, der über das Internet erreichbar sind.

Azure DDoS Protection trägt in Kombination mit bewährten Anwendungsentwurfsmethoden zum Schutz vor DDoS-Angriffen bei. Azure DDoS Protection bietet verschiedene Dienstebenen. Weitere Informationen finden Sie in der [Übersicht über Azure DDoS Protection Standard](https://docs.microsoft.com/azure/virtual-network/ddos-protection-overview).

Die folgenden Arten von Angriffen können mit DDoS Protection Standard abgewehrt werden:

> [!div class="mx-tableFixed"]

|Warnung|Beschreibung|
|---|---|
|**Volumetric attack detected** (Volumetrischen Angriff erkannt)|Ziel dieses Angriffs ist die Überflutung der Netzwerkebene mit einer beträchtlichen Menge scheinbar legitimen Datenverkehrs. Dazu zählen UDP-Überflutungen, Verstärkungsüberflutungen und andere Überflutungen mit gefälschten Paketen. DDoS Protection Standard wehrt diese bis zu mehreren Gigabytes großen Angriffe ab, indem sie mithilfe des weltweiten Netzwerks von Azure automatisch absorbiert und bereinigt werden.|
|**Protocol attack detected** (Protokollangriff erkannt)|Diese Angriffe machen den Zugriff auf ein Ziel unmöglich, indem sie eine Schwachstelle in den Schichten 3 und 4 des Protokollstapels ausnutzen. Dazu gehören SYN-Flutangriffe, Reflektionsangriffe und andere Protokollangriffe. DDoS Protection Standard wehrt diese Angriffe ab und unterscheidet dabei zwischen schädlichem und berechtigtem Datenverkehr. Nach Interaktion mit dem Client wird der schädliche Datenverkehr gesperrt.|
|**Resource (application) layer attack detected** (Angriff in der Ressourcenschicht (Anwendungsschicht) erkannt)|Das Ziel dieser Art von Angriffen sind Webanwendungspakete, um die Datenübertragung zwischen Hosts zu unterbrechen. Zu diesen Angriffen zählen Verletzungen des HTTP-Protokolls, die Einschleusung von SQL-Befehlen, XSS-Angriffe (Cross-Site Scripting) und andere Angriffe auf Schicht 7. Verwenden Sie die Web Application Firewall von Azure Application Gateway in Kombination mit DDoS Protection Standard, um sich vor diesen Angriffen zu schützen. Im Azure Marketplace finden Sie auch WAF-Angebote von Drittanbietern.|
