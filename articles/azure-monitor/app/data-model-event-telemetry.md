---
title: 'Azure Application Insights-Telemetriedatenmodell: Ereignistelemetrie | Microsoft-Dokumentation'
description: Application Insights-Datenmodell für Ereignistelemetrie
ms.service: azure-monitor
ms.subservice: application-insights
ms.topic: conceptual
author: mrbullwinkle
ms.author: mbullwin
ms.date: 04/25/2017
ms.reviewer: sergkanz
ms.openlocfilehash: 620983fb2174fe7f32a61503b0b87e8e7ce0c330
ms.sourcegitcommit: 1bd2207c69a0c45076848a094292735faa012d22
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 10/21/2019
ms.locfileid: "72678144"
---
# <a name="event-telemetry-application-insights-data-model"></a>Ereignistelemetrie: Application Insights-Datenmodell

Sie können Ereignistelemetrieelemente erstellen (in [Application Insights](../../azure-monitor/app/app-insights-overview.md)), um ein Ereignis darzustellen, das in Ihrer Anwendung aufgetreten ist. In der Regel handelt es sich um eine Benutzerinteraktion wie ein Klicken auf eine Schaltfläche oder das Zahlen eines Warenkorbs. Es kann sich auch um ein Ereignis im Lebenszyklus einer Anwendung handeln, wie z.B. eine Initialisierung oder Konfigurationsaktualisierung. 

Semantisch können Ereignisse nun mit Anforderungen in Korrelation stehen. Bei ordnungsgemäßer Verwendung ist Ereignistelemetrie jedoch wichtiger als Anforderungen oder Ablaufverfolgungen. Ereignisse stellen Geschäftstelemetrie dar, für die eine getrennte, weniger aggressive [Stichprobennahme](../../azure-monitor/app/api-filtering-sampling.md) erfolgen muss.

## <a name="name"></a>NAME

Ereignisname. Um eine ordnungsgemäße Gruppierung und nützliche Metriken zu ermöglichen, schränken Sie Ihre Anwendung so ein, dass sie eine geringe Anzahl von separaten Ereignisnamen generiert. Verwenden Sie z. B. nicht für jede generierte Instanz eines Ereignisses einen separaten Namen.

Max. Länge: 512 Zeichen

## <a name="custom-properties"></a>Benutzerdefinierte Eigenschaften

[!INCLUDE [application-insights-data-model-properties](../../../includes/application-insights-data-model-properties.md)]

## <a name="custom-measurements"></a>Benutzerdefinierte Messungen

[!INCLUDE [application-insights-data-model-measurements](../../../includes/application-insights-data-model-measurements.md)]

## <a name="next-steps"></a>Nächste Schritte

- Lesen Sie die Informationen zu den Application Insights-Typen und zum Datenmodell unter [Datenmodell](data-model.md).
- [Schreiben benutzerdefinierter Ereignistelemetriedaten](../../azure-monitor/app/api-custom-events-metrics.md#trackevent)
- Lesen Sie die Informationen zu den von Application Insights unterstützten [Plattformen](../../azure-monitor/app/platforms.md).
