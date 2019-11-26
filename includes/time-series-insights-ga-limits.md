---
title: include file
description: include file
services: digital-twins
ms.service: digital-twins
ms.topic: include
ms.date: 10/16/2019
author: deepakpalled
ms.author: dpalled
manager: cshankar
ms.custom: include file
ms.openlocfilehash: e8c040bc186932680ce7eb4418a787dc061d02ce
ms.sourcegitcommit: 92d42c04e0585a353668067910b1a6afaf07c709
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 10/28/2019
ms.locfileid: "72991145"
---
Im Folgenden werden die wichtigsten Grenzwerte für die allgemeine Verfügbarkeit zusammengefasst.

### <a name="sku-ingress-rates-and-capacities"></a>SKU: Eingangsraten und Kapazitäten

Eingangsraten und Kapazitäten der S1 und S2 SKU bieten Flexibilität bei der Konfiguration einer neuen Time Series Insights-Umgebung.

| Kapazität der SKU „S1“ | Eingangsrate | Maximale Speicherkapazität
| --- | --- | --- |
| 1 | 1 GB (1 Mio. Ereignisse) | 30 GB (30 Mio. Ereignisse) pro Monat |
| 10 | 10 GB (10 Mio. Ereignisse) | 300 GB (300 Mio. Ereignisse) pro Monat |

| Kapazität der SKU „S2“ | Eingangsrate | Maximale Speicherkapazität
| --- | --- | --- |
| 1 | 10 GB (10 Mio. Ereignisse) | 300 GB (300 Mio. Ereignisse) pro Monat |
| 10 | 100 GB (100 Mio. Ereignisse) | 3 TB (3 Mrd. Ereignisse) pro Monat |

> [!NOTE]
> Kapazitäten werden linear skaliert, sodass eine S1-SKU mit der Kapazität „2“ eine Erfassungsrate von 2 GB (2 Mio. Ereignisse) pro Tag und 60 GB (60 Mio. Ereignisse) pro Monat unterstützt.

S2 SKU-Umgebungen unterstützen wesentlich mehr Ereignisse pro Monat und haben eine deutlich höhere Eingangskapazität.

| SKU  | Ereignisanzahl pro Monat  | Ereignisgröße pro Monat  | Ereignisanzahl pro Minute | Ereignisgröße pro Minute  |
|---------|---------|---------|---------|---------|
| S1     |   30 Millionen     |  30 GB     |  720    |  720 KB   |
 |S2     |   300 Millionen    |   300 GB   | 7\.200   | 7\.200 KB  |

### <a name="property-limits"></a>Eigenschaftsgrenzwerte

Eigenschaftsgrenzwerte der allgemeinen Verfügbarkeit (GA) sind von der ausgewählten SKU-Umgebung abhängig. Die bereitgestellten Ereigniseigenschaften verfügen über entsprechende JSON-, CSV- und Diagrammspalten, die im [Time Series Insights-Explorer](https://docs.microsoft.com/azure/time-series-insights/time-series-quickstart) angezeigt werden können.

| SKU | Maximale Eigenschaften |
| --- | --- |
| S1 | 600 Eigenschaften (Spalten) |
| S2 | 800 Eigenschaften (Spalten) |

### <a name="event-sources"></a>Ereignisquellen

Maximal zwei Ereignisquellen werden pro Instanz unterstützt. 

* Informieren Sie sich über das [Hinzufügen einer Event Hub-Quelle](https://docs.microsoft.com/azure/time-series-insights/time-series-insights-how-to-add-an-event-source-eventhub).
* Konfigurieren [einer IoT-Hub-Quelle](https://docs.microsoft.com/azure/time-series-insights/time-series-insights-how-to-add-an-event-source-iothub).

### <a name="api-limits"></a>API-Grenzwerte

REST-API-Grenzwerte für Time Series Insights GA (allgemeine Verfügbarkeit) sind in der [Referenzdokumentation zur REST-API](https://docs.microsoft.com/rest/api/time-series-insights/ga-query-api#limits) angegeben.