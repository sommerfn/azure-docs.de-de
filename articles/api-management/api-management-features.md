---
title: Featurebasierter Vergleich der Azure API Management-Tarife | Microsoft-Dokumentation
description: In diesem Artikel werden die API Management-Tarife anhand der angebotenen Features verglichen.
services: api-management
documentationcenter: ''
author: vladvino
manager: erikre
editor: ''
ms.service: api-management
ms.workload: mobile
ms.tgt_pltfrm: na
ms.topic: article
ms.date: 11/26/2018
ms.author: apimpm
ms.openlocfilehash: bc5eab6bb5044e474c386d3e5771d8517e6ba9cf
ms.sourcegitcommit: 82499878a3d2a33a02a751d6e6e3800adbfa8c13
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 08/28/2019
ms.locfileid: "70072535"
---
# <a name="feature-based-comparison-of-the-azure-api-management-tiers"></a>Featurebasierter Vergleich der Azure API Management-Tarife

>[!IMPORTANT]
> Beachten Sie, dass der Tarif „Developer“ nicht für Anwendungsfälle und Auswertungen in Produktionsumgebungen vorgesehen ist. Es wird keine SLA angeboten. 

Jeder API Management-[Tarif](https://aka.ms/apimpricing) bietet einen bestimmten Satz von Features und [Kapazität pro Einheit](api-management-capacity.md). In der folgenden Tabelle werden die wichtigsten Features, die in den einzelnen Tarifen verfügbar sind, zusammengefasst. Einige Features funktionieren je nach Tarif möglicherweise anders oder weisen unterschiedliche Funktionen auf. In solchen Fällen werden die Unterschiede in den Dokumentationsartikel zu diesen einzelnen Features hervorgehoben.

| Feature                                                                                      | Nutzung | Developer      | Basic          | Standard       | Premium        |
| -------------------------------------------------------------------------------------------- | ----------------------------- | -------------- | -------------- | -------------- | -------------- |
| Azure AD-Integration<sup>1</sup>                                                             | Nein                            | Ja            | Nein             | Ja            | Ja            |
| Unterstützung von virtuellen Netzwerken (VNETs)                                                               | Nein                            | Ja            | Nein             | Nein             | Ja            |
| Bereitstellung in mehreren Regionen                                                                      | Nein                            | Nein             | Nein             | Nein             | Ja            |
| Mehrere benutzerdefinierte Domänennamen                                                                 | Nein                            | Nein             | Nein             | Nein             | Ja            |
| Entwicklerportal<sup>2</sup>                                                                 | Nein                            | Ja            | Ja            | Ja            | Ja            |
| Integrierter Cache                                                                               | Nein                            | Ja            | Ja            | Ja            | Ja            |
| Integrierte Analysen                                                                           | Nein                            | Ja            | Ja            | Ja            | Ja            |
| [SSL-Einstellungen](api-management-howto-manage-protocols-ciphers.md)                             | Ja                            | Ja            | Ja            | Ja            | Ja            |
| [Externer Cache](https://aka.ms/apimbyoc)                                                    | Ja                           | Ja            | Ja            | Ja            | Ja            |
| [Clientzertifikatsauthentifizierung](api-management-howto-mutual-certificates-for-clients.md) | Ja                | Ja            | Ja            | Ja            | Ja            |
| [Sichern und Wiederherstellen](api-management-howto-disaster-recovery-backup-restore.md)               | Nein                            | Ja            | Ja            | Ja            | Ja            |
| [Verwaltung über Git](api-management-configuration-repository-git.md)                        | Nein                            | Ja            | Ja            | Ja            | Ja            |
| Direkte Verwaltungs-API                                                                        | Nein                            | Ja            | Ja            | Ja            | Ja            |
| Azure Monitor-Protokolle und -Metriken                                                               | Nein                | Ja            | Ja            | Ja            | Ja            |
| Statische IP                                                               | Nein                | Ja            | Ja            | Ja            | Ja            |

<sup>1</sup> Ermöglicht die Verwendung von Azure AD (und Azure AD B2C) als Identitätsanbieter für die Benutzeranmeldung im Entwicklerportal.<br/>
<sup>2</sup> Einschließlich zugehöriger Funktionen, z.B. Benutzer, Gruppen, Probleme, Anwendungs- und E-Mail-Vorlagen und Benachrichtigungen.<br/>
