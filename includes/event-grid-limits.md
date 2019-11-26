---
title: include file
description: include file
services: event-grid
author: tfitzmac
ms.service: event-grid
ms.topic: include
ms.date: 05/22/2019
ms.author: tomfitz
ms.custom: include file
ms.openlocfilehash: 890095718833c90a6764d0799d2081b393b1d0bd
ms.sourcegitcommit: 7efb2a638153c22c93a5053c3c6db8b15d072949
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 10/24/2019
ms.locfileid: "72887778"
---
Die folgenden Grenzwerte gelten für Systemthemen und benutzerdefinierte Themen von Azure Event Grid, *nicht* für Ereignisdomänen.

| Resource | Begrenzung |
| --- | --- |
| Benutzerdefinierte Themen pro Azure-Abonnement | 100 |
| Ereignisabonnements pro Thema | 500 |
| Veröffentlichungsrate für ein benutzerdefiniertes Thema (eingehend) | 5\.000 Ereignisse pro Sekunde pro Thema |
| Veröffentlichungsanforderungen | 250 pro Sekunde |
| Ereignisgröße | Unterstützung für 64 KB in der allgemeinen Verfügbarkeit (GA). Die Unterstützung für 1 MB befindet sich derzeit in der Vorschauphase. |

Die folgenden Grenzwerte gelten ausschließlich für Ereignisdomänen.

| Resource | Begrenzung |
| --- | --- |
| Themen pro Ereignisdomäne | 100.000 |
| Ereignisabonnements pro Thema innerhalb einer Domäne | 500 |
| Domänenbereich-Ereignisabonnements | 50 |
| Veröffentlichungsrate für eine Ereignisdomäne (Eingang) | 5\.000 Ereignisse pro Sekunde |
| Veröffentlichungsanforderungen | 250 pro Sekunde |
| Ereignisdomänen pro Azure-Abonnement | 100 |