---
title: Übersicht über Erkenntnisse in Azure Monitor | Microsoft-Dokumentation
description: Mit Erkenntnissen wird eine angepasste Überwachungsoberfläche in Azure Monitor für bestimmte Anwendungen und Dienste bereitgestellt. Dieser Artikel enthält eine kurze Beschreibung der einzelnen Erkenntnisse, die derzeit verfügbar sind.
ms.service: azure-monitor
ms.subservice: ''
ms.topic: conceptual
author: bwren
ms.author: bwren
ms.date: 05/22/2019
ms.openlocfilehash: 3abf3d562b9ca4d6fc9907c065dbaf9a52d58c5f
ms.sourcegitcommit: ae461c90cada1231f496bf442ee0c4dcdb6396bc
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 10/17/2019
ms.locfileid: "72555345"
---
# <a name="overview-of-insights-in-azure-monitor"></a>Übersicht über Erkenntnisse in Azure Monitor
Mit Erkenntnissen wird eine angepasste Überwachungsoberfläche für bestimmte Anwendungen und Dienste bereitgestellt. Hierbei werden Daten auf der [Azure Monitor-Datenplattform](../platform/data-platform.md) gespeichert und weitere Azure Monitor-Funktionen für Analyse und Warnungen genutzt, doch können auch zusätzliche Daten gesammelt und eine einzigartige Benutzeroberfläche im Azure-Portal bereitgestellt werden. Greifen Sie auf Erkenntnisse im Azure-Portal über den Abschnitt **Erkenntnisse** im Azure Monitor-Menü zu.

Die folgenden Abschnitte enthalten eine kurze Beschreibung der Erkenntnisse, die derzeit in Azure Monitor verfügbar sind. Weitere Informationen zu den jeweiligen Erkenntnissen finden Sie in der ausführlichen Dokumentation.

## <a name="application-insights"></a>Application Insights
Application Insights ist ein erweiterbarer, für Webentwickler konzipierter Dienst zur Verwaltung der Anwendungsleistung (Application Performance Management, APM) auf mehreren Plattformen. Verwenden Sie ihn, um Ihre aktiven Webanwendung zu überwachen. Er lässt sich für Anwendungen auf einer Vielzahl von Plattformen einsetzen. Dazu zählen unter anderem .NET, Node.js und Java EE (lokal gehostet, als Hybridmodell oder in einer öffentlichen Cloud). Der Dienst lässt sich auch in Ihren DevOps-Prozess integrieren und verfügt über Verbindungspunkte mit einer Vielzahl von Entwicklungstools.

Informationen finden Sie unter [Was ist Application Insights?](../app/app-insights-overview.md)

![Application Insights](media/insights-overview/app-insights.png)

## <a name="azure-monitor-for-containers"></a>Azure Monitor für Container
Azure Monitor für Container überwacht die Leistung von Containerworkloads, die entweder in Azure Container Instances oder in Managed Kubernetes-Clustern bereitgestellt sind, die im Azure Kubernetes Service (AKS) gehostet werden. Die Überwachung Ihrer Container ist vor allem dann entscheidend, wenn Sie einen umfangreichen Produktionscluster mit mehreren Anwendungen ausführen.

Informationen finden Sie unter [Azure Monitor für Container – Übersicht](../insights/container-insights-overview.md).

![Azure Monitor für Container](media/insights-overview/container-insights.png)

## <a name="azure-monitor-for-resource-groups-preview"></a>Azure Monitor für Ressourcengruppen (Vorschauversion)
Azure Monitor für Ressourcengruppen hilft beim Selektieren und Diagnostizieren von Problemen Ihrer individuellen Ressourcen und bietet gleichzeitig einen Kontext für die Integrität und Leistung der Ressourcengruppe als Ganzes.

Informationen finden Sie unter [Überwachen von Ressourcengruppen mit Azure Monitor (Vorschauversion)](../insights/resource-group-insights.md).

![Azure Monitor für Ressourcengruppen](media/insights-overview/resource-group-insights.png)

## <a name="azure-monitor-for-vms-preview"></a>Azure Monitor für VMs (Vorschauversion)
Azure Monitor für VMs überwacht Ihre virtuellen Azure-Computer (VM) und VM-Skalierungsgruppen bedarfsorientiert. Der Dienst analysiert die Leistung und Integrität Ihrer Windows- und Linux-VMs und überwacht deren Prozesse und Abhängigkeiten von anderen Ressourcen und externen Prozessen.

Informationen finden Sie unter [Was ist Azure Monitor für VMs?](vminsights-overview.md)

![Azure Monitor für VMs](media/insights-overview/vm-insights.png)

## <a name="next-steps"></a>Nächste Schritte
* Informieren Sie sich über die [Azure Monitor-Datenplattform](../platform/data-platform.md), die von Erkenntnissen genutzt wird.
* Erfahren Sie mehr über die verschiedenen [Datenquellen, die von Azure Monitor verwendet werden](../platform/data-sources.md), und die verschiedenen Arten von Daten, die von den jeweiligen Erkenntnissen erfasst werden.
