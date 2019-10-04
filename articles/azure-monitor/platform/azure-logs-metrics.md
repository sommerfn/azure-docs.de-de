---
title: Protokolle und Metriken in Azure | Microsoft-Dokumentation
description: Übersicht über Diagnoseprotokolle in Azure, die regelmäßig umfassende Daten zum Betrieb einer Azure-Ressource liefern.
author: bwren
services: azure-monitor
ms.service: azure-monitor
ms.topic: conceptual
ms.date: 07/29/2019
ms.author: bwren
ms.subservice: logs
ms.openlocfilehash: 3fe220506e074f881a16c1805d25fb4b39927488
ms.sourcegitcommit: 55f7fc8fe5f6d874d5e886cb014e2070f49f3b94
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 09/25/2019
ms.locfileid: "71262977"
---
# <a name="logs-and-metrics-in-azure"></a>Protokolle und Metriken in Azure
Es gibt verschiedene [Protokolle](data-platform-logs.md) und [Metriken](data-platform-metrics.md).

Überwachungsanwendungen und -dienste in Azure können in solche unterteilt werden, die auf der [Azure-Datenplattform](data-platform.md) gespeichert sind. 

Protokolle und Metriken können in zwei Kategorien unterteilt werden.

- Plattform: Protokolle und Metriken, die automatisch generiert werden und bei denen nur eine einfache Konfiguration erforderlich ist. 



| Ebene | Plattformprotokolle | Plattformmetriken | Benutzerdefinierte Protokolle | Benutzerdefinierte Metriken |
|:---|:---|:---|:---|:---|
| Anwendung  | | | | |
| Gastbetriebssystem     | Heartbeat |  | Diagnoseerweiterung<br>Log Analytics-Agent | Diagnoseerweiterung |
| Resource     | [Ressourcenprotokolle](resource-logs-overview.md)<br>(für jeden Dienst spezifisch) | [Ressourcenmetriken](metrics-supported.md)<br>(für jeden Dienst spezifisch) | | [Benutzerdefinierte Metriken](metrics-custom-overview.md) |
| Subscription | [Aktivitätsprotokoll](activity-logs-overview.md) | | | |
| Tenant       | 

## <a name="next-steps"></a>Nächste Schritte

* [Streamen von Diagnoseprotokollen für Ressourcen an **Event Hubs**](resource-logs-stream-event-hubs.md)
* [Ändern der Diagnoseeinstellungen für Ressourcen mithilfe der Azure Monitor-REST-API Service Diagnostic Settings](https://docs.microsoft.com/rest/api/monitor/) (Diagnoseeinstellungen für Dienste))
* [Analysieren von Protokollen aus Azure Storage mit Azure Monitor](collect-azure-metrics-logs.md)
