---
title: include file
description: include file
services: application-insights
author: mrbullwinkle
ms.service: application-insights
ms.topic: include
ms.date: 08/06/2019
ms.author: mbullwin
ms.custom: include file
ms.openlocfilehash: 14e2bd4af2616e9dd33fe8267de132ab6c0f1cfe
ms.sourcegitcommit: 6d2a147a7e729f05d65ea4735b880c005f62530f
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 08/22/2019
ms.locfileid: "69982601"
---
Es gibt einige Grenzwerte hinsichtlich der Anzahl von Metriken und Ereignissen pro Anwendung (d. h. pro Instrumentationsschlüssel). Die Beschränkungen hängen von dem von Ihnen ausgewählten [Tarif](https://azure.microsoft.com/pricing/details/application-insights/) ab.

| Resource | Standardlimit | Hinweis
| --- | --- | --- |
| Gesamtdaten pro Tag | 100 GB | Die Datenmenge kann durch Festlegen einer Obergrenze reduziert werden. Wird eine höhere Datenmenge benötigt, können Sie den Grenzwert im Portal auf bis zu 1.000 GB erhöhen. Bei Kapazitäten über 1.000 GB senden Sie eine E-Mail an AIDataCap@microsoft.com.
| Drosselung | 32.000 Ereignisse/s | Das Limit wird eine Minute lang gemessen.
| Beibehaltung von Daten | [30 – 730 Tage](https://docs.microsoft.com/azure/azure-monitor/app/pricing#change-the-data-retention-period)  | Hierbei handelt es sich um eine Ressource für [Suche](../articles/azure-monitor/app/diagnostic-search.md), [Analyse](../articles/azure-monitor/app/analytics.md) und [Metrik-Explorer](../articles/azure-monitor/app/metrics-explorer.md).
| [Webtests in mehreren Schritten](../articles/azure-monitor/app/availability-multistep.md) mit detaillierter Ergebnisaufbewahrung | 90 Tage | Diese Ressource liefert detaillierte Ergebnisse der einzelnen Schritte.
| Maximale Größe von Telemetrieelementen | 64 KB |
| Maximale Anzahl von Telemetrieelementen pro Batch | 64 K |
| Eingenschaft und Länge der Namen von Metriken | 150 | Siehe [Typschemas](https://github.com/microsoft/ApplicationInsights-Home/tree/master/EndpointSpecs/Schemas/Bond).
| Zeichenfolgenlänge des Eigenschaftswerts | 8\.192  | Siehe [Typschemas](https://github.com/microsoft/ApplicationInsights-Home/tree/master/EndpointSpecs/Schemas/Bond).
| Länge von Ablaufverfolgungs- und Ausnahmebenachrichtigungen | 32.768  | Siehe [Typschemas](https://github.com/microsoft/ApplicationInsights-Home/tree/master/EndpointSpecs/Schemas/Bond).
| [Verfügbarkeitstests](../articles/azure-monitor/app/monitor-web-app-availability.md) Anzahl pro App | 100 |
| Vermerkdauer von [Profiler](../articles/azure-monitor/app/profiler.md)-Daten | 5 Tage |
| Pro Tag gesendete [Profiler](../articles/azure-monitor/app/profiler.md)-Daten | 10 GB |

Weitere Informationen zu [Preisen und Kontingenten für Application Insights ](../articles/azure-monitor/app/pricing.md)finden Sie hier.