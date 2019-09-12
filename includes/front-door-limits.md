---
title: include file
description: include file
services: frontdoor
author: sharad4u
ms.service: frontdoor
ms.topic: include
ms.date: 05/09/2019
ms.author: sharadag
ms.custom: include file
ms.openlocfilehash: deca0034996f6c8ddcac71cd4f191c1a0659b655
ms.sourcegitcommit: 23389df08a9f4cab1f3bb0f474c0e5ba31923f12
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 09/10/2019
ms.locfileid: "67333376"
---
| Resource | Standard/maximales Limit |
| --- | --- |
| Azure Front Door Service-Ressourcen pro Abonnement | 100 |
| Front-End-Hosts (einschließlich benutzerdefinierter Domänen) pro Ressource | 100 |
| Routingregeln pro Ressource | 100 |
| Back-End-Pools pro Ressource | 50 |
| Back-Ends pro Back-End-Pool | 100 |
| Abzustimmende Pfadmuster für eine Routingregel | 25 |
| Benutzerdefinierte Web Application Firewall-Regeln pro Richtlinie | 10 |
| Web Application Firewall-Richtlinien pro Ressource | 100 |
| Web Application Firewall – Übereinstimmungsbedingungen pro benutzerdefinierter Regel | 10 |
| Web Application Firewall – IP-Adressbereiche pro Übereinstimmungsbedingung | 600 |
| Web Application Firewall – Zeichenfolgen-Übereinstimmungswerte pro Übereinstimmungsbedingung | 10 |
| Web Application Firewall – Länge des Zeichenfolgen-Übereinstimmungswerts | 256 |
| Web Application Firewall – Länge des POST-Textparameternamens | 256 |
| Web Application Firewall – Länge des HTTP-Headernamens | 256 |
| Web Application Firewall – Länge des Cookienamens | 256 |
| Web Application Firewall – Größe des überprüften HTTP-Anforderungstexts | 128 KB |
| Web Application Firewall – Länge des benutzerdefinierten Antworttexts | 2 KB |

### <a name="timeout-values"></a>Timeoutwerte
#### <a name="client-to-front-door"></a>Client zu Front Door
- Front Door verfügt über ein Timeout für eine TCP-Verbindung im Leerlauf von 61 Sekunden.

#### <a name="front-door-to-application-back-end"></a>Front Door zum Anwendungs-Back-End
- Wenn es sich bei der Antwort um eine segmentierte Antwort handelt, wird der Status 200 zurückgegeben, falls/wenn der erste Block empfangen wird.
- Nach der Weiterleitung der HTTP-Anforderung an das Back-End wartet Front Door 30 Sekunden lang auf das erste Paket vom Back-End. Anschließend wird ein 503-Fehler an den Client zurückgegeben.
- Nach dem Empfang des ersten Pakets vom Back-End wartet Front Door 30 Sekunden in einem Leerlauftimeout. Anschließend wird ein 503-Fehler an den Client zurückgegeben.
- Das Timeout für die Front Door-TCP-Sitzung mit dem Back-End beträgt 30 Minuten.

### <a name="upload-and-download-data-limit"></a>Datenlimit beim Hoch- und Herunterladen

|  | Mit Codierung für segmentierte Übertragung (Chunked Transfer Encoding, CTE) | Ohne HTTP-Segmentierung |
| ---- | ------- | ------- |
| **Download** | Es gibt keine Beschränkung für die Downloadgröße. | Es gibt keine Beschränkung für die Downloadgröße. |
| **Upload** |  Es gibt keine Beschränkung, so lange jeder CTE-Upload kleiner als 2 GB ist. | Die Größe darf 2 GB nicht überschreiten. |

### <a name="other-limits"></a>Andere Limits
- Maximale URL-Größe – 8.192 Byte – Gibt die maximale Länge der Basis-URL-an (Schema + Hostname + Port + Pfad+ Abfragezeichenfolge der URL) – Maximale Größe der Abfragezeichenfolge – 4.096 Byte – Gibt die maximale Länge der Abfragezeichenfolge in Bytes an.