---
title: include file
description: include file
services: azure-monitor
author: rboucher
tags: azure-service-management
ms.topic: include
ms.date: 02/07/2019
ms.author: robb
ms.custom: include file
ms.openlocfilehash: 050d3314345e64e3d69a07367a0e9acc318fa106
ms.sourcegitcommit: d4dfbc34a1f03488e1b7bc5e711a11b72c717ada
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 06/13/2019
ms.locfileid: "66271545"
---
| Resource | Standardlimit | Maximales Limit |
| --- | --- | --- |
| Einstellungen für automatische Skalierung |100 pro Region und Abonnement. | Wie Standard. |
| Metrikwarnungen (klassisch) |100 aktive Warnungsregeln pro Abonnement. | Wenden Sie sich an den Support. |
| Metrikwarnungen |1000 aktive Warnungsregeln pro Abonnement (in öffentlichen Clouds) und 100 aktive Warnungsregeln pro Abonnement in Azure China und Azure Government. | Wenden Sie sich an den Support. |
| Aktivitätsprotokollwarnungen | 100 aktive Warnungsregeln pro Abonnement. | Wie Standard. |
| Protokollwarnungen | 512 | Wenden Sie sich an den Support. |
| Aktionsgruppen |2\.000 Aktionsgruppen pro Abonnement. | Wenden Sie sich an den Support. |

**Aktionsgruppenspezifische Einschränkungen**

| Resource | Standardlimit | Maximales Limit |
| --- | --- | --- |
| Azure-App-Push | 10 Azure-App-Aktionen pro Aktionsgruppe. | Wenden Sie sich an den Support. |
| E-Mail | 1\.000 E-Mail-Aktionen in einer Aktionsgruppe. Weitere Informationen finden Sie in den [Informationen zu Ratenbegrenzungen](../articles/azure-monitor/platform/alerts-rate-limiting.md). | Wenden Sie sich an den Support. |
| ITSM | 10 ITSM-Aktionen in einer Aktionsgruppe. | Wenden Sie sich an den Support. | 
| Logik-App | 10 Logik-App-Aktionen in einer Aktionsgruppe. | Wenden Sie sich an den Support. |
| Runbook | 10 Runbook-Aktionen in einer Aktionsgruppe. | Wenden Sie sich an den Support. |
| sms | 10 SMS-Aktionen in einer Aktionsgruppe. Weitere Informationen finden Sie in den [Informationen zu Ratenbegrenzungen](../articles/azure-monitor/platform/alerts-rate-limiting.md). | Wenden Sie sich an den Support. |
| Sprache | 10 Sprachaktionen in einer Aktionsgruppe. Weitere Informationen finden Sie in den [Informationen zu Ratenbegrenzungen](../articles/azure-monitor/platform/alerts-rate-limiting.md). | Wenden Sie sich an den Support. |
| Webhook | 10 Webhookaktionen in einer Aktionsgruppe.  Maximale Anzahl von Webhookaufrufen ist 1500 pro Minute pro Abonnement. Andere Grenzwerte finden Sie unter [Aktionsspezifische Informationen](../articles/azure-monitor/platform/action-groups.md#action-specific-information).  | Wenden Sie sich an den Support. |
