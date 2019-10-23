---
title: Regionszuordnungen für Azure Monitor für Container
description: Dieser Artikel beschreibt die zwischen Azure Monitor für Container, dem Log Analytics-Arbeitsbereich und benutzerdefinierten Metriken unterstützten Regionszuordnungen.
ms.service: azure-monitor
ms.subservice: ''
ms.topic: conceptual
author: mgoedtel
ms.author: magoedte
ms.date: 06/26/2019
ms.openlocfilehash: f22c62dddf2e38fa2c9471ce98b49a8aa32390b3
ms.sourcegitcommit: ae461c90cada1231f496bf442ee0c4dcdb6396bc
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 10/17/2019
ms.locfileid: "72554018"
---
# <a name="region-mappings-supported-by-azure-monitor-for-containers"></a>Von Azure Monitor für Container unterstützte Regionszuordnungen

 Wenn Sie Azure Monitor für Container aktivieren, werden nur bestimmte Regionen unterstützt, um einen Log Analytics-Arbeitsbereich und einen AKS-Cluster zu verbinden und benutzerdefinierte Metriken zu sammeln, die an Azure Monitor übermittelt werden.

## <a name="log-analytics-workspace-supported-mappings"></a>Log Analytics-Arbeitsbereich unterstützt Zuordnungen

Die AKS-Clusterressourcen oder der Log Analytics-Arbeitsbereich können sich in anderen Regionen befinden, und die folgende Tabelle zeigt unsere Zuordnungen.

|**AKS-Clusterregion** | **Log Analytics-Arbeitsbereichsregion** |
|-----------------------|------------------------------------|
|**Afrika** | |
|Südafrika, Nord |Europa, Westen |
|Südafrika, West |Europa, Westen |
|**Australien** | |
|Australien, Osten |Australien, Osten |
|Australien, Mitte |Australien, Mitte |
|Australien, Mitte 2 |Australien, Mitte |
|Australien, Osten |Australien, Osten |
|**Asien-Pazifik** | |
|Asien, Osten |Asien, Osten |
|SoutheastAsia |SoutheastAsia |
|**Brasilien** | |
|Brasilien, Süden | USA, Süden-Mitte |
|**Kanada** ||
|CanadaCentral |CanadaCentral |
|CanadaEast |CanadaCentral |
|**Europa** | |
|Frankreich, Mitte |Frankreich, Mitte |
|Frankreich, Süden |Frankreich, Mitte |
|Europa, Norden |Europa, Norden |
|UKSouth |UKSouth |
|UK, Westen |UKSouth |
|Europa, Westen |Europa, Westen |
|**Indien** | |
|CentralIndia |CentralIndia |
|SouthIndia |CentralIndia |
|WestIndia |CentralIndia |
|**Japan** | |
|JapanEast |JapanEast |
|Japan, Westen |JapanEast |
|**Korea** | |
|KoreaCentral |KoreaCentral |
|KoreaSouth |KoreaCentral |
|**USA** | |
|USA, Mitte |USA, Mitte|
|EastUS |EastUS |
|EastUS2 |EastUS2 |
|USA, Westen |USA, Westen |
|WestUS2 |WestUS2 |
|USA, Westen-Mitte<sup>1</sup>|EastUS<sup>1</sup>|
|US Government, Virginia |US Government, Virginia |

<sup>1</sup> Aufgrund von Kapazitätseinschränkungen ist die Region nicht verfügbar, wenn neue Ressourcen erstellt werden. Dies beinhaltet einen Log Analytics-Arbeitsbereich. Allerdings sollten bereits in der Region vorhandene verknüpfte Ressourcen weiterhin funktionieren.

## <a name="custom-metrics-supported-regions"></a>Regionen, die benutzerdefinierte Metriken unterstützen

Das Sammeln von Metriken von Knoten und Pods von AKS-Clustern (Azure Kubernetes Services) wird für die Veröffentlichung als benutzerdefinierte Metriken nur in den folgenden [Azure-Regionen](../platform/metrics-custom-overview.md#supported-regions) unterstützt.

## <a name="next-steps"></a>Nächste Schritte

Um in die Überwachung Ihrer AKS-Cluster einzusteigen, lesen Sie [Aktivieren von Azure Monitor für Container](container-insights-onboard.md), um ein Verständnis für die Anforderungen und verfügbaren Methoden zum Aktivieren von Überwachung zu entwickeln.  
