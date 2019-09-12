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
ms.openlocfilehash: 3f94481e6a8550479788d92c744327e1dc3b58c4
ms.sourcegitcommit: 23389df08a9f4cab1f3bb0f474c0e5ba31923f12
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 09/10/2019
ms.locfileid: "66376917"
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
| Themen pro Ereignisdomäne | 1\.000 während der Public Preview-Phase |
| Ereignisabonnements pro Thema innerhalb einer Domäne | 50 während der Public Preview-Phase |
| Domänenbereich-Ereignisabonnements | 50 während der Public Preview-Phase |
| Veröffentlichungsrate für eine Ereignisdomäne (Eingang) | 5\.000 Ereignisse pro Sekunde während der Public Preview-Phase |
| Veröffentlichungsanforderungen | 250 pro Sekunde |