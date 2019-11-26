---
title: Erstellen interaktiver Berichte mit Azure Monitor-Arbeitsmappen aus mehreren Azure-Datenquellen | Microsoft-Dokumentation
description: Vereinfachen Sie die komplexe Berichterstellung mit vordefinierten und benutzerdefiniert parametrisierten Azure Monitor-Arbeitsmappen, die aus mehreren Datenquellen erstellt werden.
services: azure-monitor
documentationcenter: ''
author: mrbullwinkle
manager: carmonm
ms.service: azure-monitor
ms.workload: tbd
ms.tgt_pltfrm: ibiza
ms.topic: conceptual
ms.date: 10/23/2019
ms.author: mbullwin
ms.openlocfilehash: dd5068da31c3aa863fc56022834a28b60ee15004
ms.sourcegitcommit: 0b1a4101d575e28af0f0d161852b57d82c9b2a7e
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 10/30/2019
ms.locfileid: "73164524"
---
# <a name="azure-monitor-workbooks-data-sources"></a>Datenquellen für Azure Monitor-Arbeitsmappen

Arbeitsmappen sind mit einer großen Anzahl von Datenquellen kompatibel. In diesem Artikel werden die Datenquellen erläutert, die derzeit für Azure Monitor-Arbeitsmappen verfügbar sind.

## <a name="logs"></a>Protokolle

Arbeitsmappen ermöglichen das Abfragen von Protokollen aus folgenden Quellen:

* Azure Monitor-Protokolle (Application Insights-Ressourcen und Log Analytics-Arbeitsbereiche)
* Ressourcenbezogene Daten (Aktivitätsprotokolle)

Arbeitsmappenautoren können KQL-Abfragen verwenden, mit denen die zugrunde liegenden Ressourcendaten transformiert werden, um ein Resultset auszuwählen, das als Text, Diagramme oder Raster visuell dargestellt werden kann.

![Screenshot der Berichtsschnittstelle für Protokolle in der Arbeitsmappe](./media/workbooks-overview/logs.png)

Arbeitsmappenautoren können problemlos Abfragen für mehrere Ressourcen durchführen und so eine wirklich einheitliche und umfassende Berichtsoberfläche erstellen.

## <a name="metrics"></a>Metriken

Azure-Ressourcen geben [Metriken](data-platform-metrics.md) aus, auf die über Arbeitsmappen zugegriffen werden kann. Der Zugriff auf Metriken kann in Arbeitsmappen über ein spezielles Steuerelement erfolgen, das Ihnen die Angabe der Zielressourcen, der gewünschten Metriken und deren Aggregation ermöglicht. Diese Daten können dann in Diagrammen oder Rastern dargestellt werden.

![Screenshot des Metrikdiagramms für die CPU-Auslastung in der Arbeitsmappe](./media/workbooks-overview/metrics-graph.png)

![Screenshot der Schnittstelle für Metriken in der Arbeitsmappe](./media/workbooks-overview/metrics.png)

## <a name="azure-resource-graph"></a>Azure Resource Graph 

Arbeitsmappen unterstützen das Abfragen von Ressourcen und deren Metadaten mithilfe von Azure Resource Graph (ARG). Diese Funktion wird hauptsächlich zum Erstellen benutzerdefinierter Abfragebereiche für Berichte verwendet. Der Ressourcenbereich wird über eine KQL-Teilmenge ausgedrückt, die von ARG unterstützt wird. Dies ist für gängige Anwendungsfälle oft ausreichend.

Damit ein Abfragesteuerelement diese Datenquelle verwendet, wählen Sie über die Dropdownliste „Abfragetyp“ die Option „Azure Resource Graph“ aus, und wählen Sie dann die Zielabonnements aus. Verwenden Sie das Abfragesteuerelement, um die KQL-Teilmenge von ARG hinzuzufügen, mit der eine interessante Ressourcenteilmenge ausgewählt wird.


![Screenshot der KQL-Abfrage von Azure Resource Graph](./media/workbooks-overview/azure-resource-graph.png)

## <a name="alerts-preview"></a>Warnungen (Vorschau)

Arbeitsmappen ermöglichen es Benutzern, die aktiven Warnungen in Bezug auf Ressourcen visuell darzustellen. Diese Funktion ermöglicht das Erstellen von Berichten, bei denen Benachrichtigungsdaten (Warnungen) und Diagnoseinformationen (Metriken, Protokolle) in einem Bericht zusammengeführt werden. Diese Informationen können auch miteinander verknüpft werden, um umfassende Berichte zu erstellen, in denen Erkenntnisse aus diesen Datenquellen kombiniert sind.

Damit ein Abfragesteuerelement diese Datenquelle verwendet, wählen Sie über die Dropdownliste „Abfragetyp“ die Option „Warnungen“ aus, und wählen Sie dann die als Ziel zu verwendenden Abonnements, Ressourcengruppen und Ressourcen aus. Verwenden Sie die Dropdownlisten für Warnungsfilter, um eine interessante Teilmenge von Warnungen für Ihre Analyseanforderungen auszuwählen.

![Screenshot der Warnungsabfrage](./media/workbooks-overview/alerts.png)

## <a name="workload-health-preview"></a>Workloadintegrität (Vorschau)

Azure Monitor bietet Funktionen, mit denen die Verfügbarkeit und Leistung von Windows- oder Linux-Gastbetriebssystemen proaktiv überwacht werden. Azure Monitor modelliert wichtige Komponenten und deren Beziehungen, Kriterien für die Messung der Integrität dieser Komponenten und welche Komponenten eine Warnung ausgeben, sobald ein fehlerhafter Zustand erkannt wird. Arbeitsmappen ermöglichen es Benutzern, diese Informationen zum Erstellen umfassender interaktiver Berichte zu verwenden.

Damit ein Abfragesteuerelement diese Datenquelle verwendet, wählen Sie über die Dropdownliste **Abfragetyp** die Option „Workloadintegrität“ aus, und wählen Sie dann das als Ziel zu verwendende Abonnement, die Ressourcengruppe oder die VM-Ressourcen aus. Verwenden Sie die Dropdownlisten für Integritätsfilter, um eine interessante Teilmenge von Integritätsvorfällen für Ihre Analyseanforderungen auszuwählen.

![Screenshot der Warnungsabfrage](./media/workbooks-overview/workload-health.png)

## <a name="azure-resource-health"></a>Azure Resource Health 

Arbeitsmappen unterstützen das Abrufen von Azure Resource Health und das Kombinieren mit anderen Datenquellen, um umfassende, interaktive Integritätsberichte zu erstellen.

Damit ein Abfragesteuerelement diese Datenquelle verwendet, wählen Sie über die Dropdownliste **Abfragetyp** die Option „Azure Health“ aus, und wählen Sie dann die Zielressourcen aus. Verwenden Sie die Dropdownlisten für Integritätsfilter, um eine interessante Teilmenge von Ressourcenproblemen für Ihre Analyseanforderungen auszuwählen.

![Screenshot der Warnungsabfrage](./media/workbooks-overview/resource-health.png)

## <a name="azure-data-explorer-preview"></a>Azure Data Explorer (Vorschau)

Arbeitsmappen unterstützen jetzt das Abfragen von [Azure Data Explorer](https://docs.microsoft.com/azure/data-explorer/)-Clustern mit der leistungsstarken [Kusto](https://docs.microsoft.com/azure/kusto/query/index)-Abfragesprache.   

![Screenshot des Kusto-Abfragefensters](./media/workbooks-overview/data-explorer.png)

## <a name="next-steps"></a>Nächste Schritte

* [Erfahren Sie mehr](workbooks-visualizations.md) über die vielen umfassenden Visualisierungsoptionen für Arbeitsmappen.
* [Steuern](workbooks-access-control.md) Sie den Zugriff auf Ihre Arbeitsmappenressourcen, und geben Sie diese frei.
