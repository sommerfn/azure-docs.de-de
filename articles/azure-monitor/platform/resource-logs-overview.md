---
title: Übersicht über Azure-Ressourcenprotokolle | Microsoft-Dokumentation
description: Erläuterung der unterstützten Dienste und Ereignisschemas für Azure-Ressourcenprotokolle.
ms.service: azure-monitor
ms.subservice: logs
ms.topic: reference
author: rboucher
ms.author: robb
ms.date: 10/22/2019
ms.openlocfilehash: b953f9b5e5fd8c853746caad3047986786bd1317
ms.sourcegitcommit: 92d42c04e0585a353668067910b1a6afaf07c709
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 10/28/2019
ms.locfileid: "72989624"
---
# <a name="azure-resource-logs-overview"></a>Übersicht über Azure-Ressourcenprotokolle
Azure-Ressourcenprotokolle sind [Plattformprotokolle](platform-logs-overview.md), die von Azure-Ressourcen ausgegeben werden und deren internen Betrieb beschreiben. Alle Ressourcenprotokolle nutzen ein gemeinsames Schema der obersten Ebene. Auf diese Weise kann jeder Dienst für seine eigenen Ereignisse flexibel eindeutige Eigenschaften ausgeben.

> [!NOTE]
> Ressourcenprotokolle wurden zuvor als Diagnoseprotokolle bezeichnet.

## <a name="collecting-resource-logs"></a>Erfassen von Ressourcenprotokollen
Ressourcenprotokolle werden von unterstützten Azure-Ressourcen automatisch generiert, jedoch nur dann erfasst, wenn Sie diese mithilfe einer [Diagnoseeinstellung](diagnostic-settings.md) konfigurieren. Erstellen Sie eine Diagnoseeinstellung für jede Azure-Ressource, um die Protokolle an die folgenden Ziele weiterzuleiten:

| Destination | Szenario |
|:---|:---|:---|
| [Log Analytics-Arbeitsbereich](resource-logs-collect-workspace.md) | Analysieren Sie die Protokolle mit anderen Überwachungsdaten, und nutzen Sie Azure Monitor-Features wie Protokollabfragen und Protokollwarnungen. |
| [Azure Storage](resource-logs-collect-storage.md) | Archivieren Sie die Protokolle zur Überwachung oder Sicherung. |
| [Event Hub](resource-logs-stream-event-hubs.md) | Streamen Sie die Protokolle an Protokollierungs- und Telemetriesysteme von Drittanbietern.  |

## <a name="compute-resources"></a>Computeressourcen
Ressourcenprotokolle unterscheiden sich von Protokollen auf Gastbetriebssystemebene auf Azure-Computeressourcen. Computeressourcen erfordern, dass ein Agent Protokolle und Metriken vom jeweiligen Gastbetriebssystem erfasst, einschließlich solcher Daten wie Ereignisprotokolle, Syslog und Leistungsindikatoren. Verwenden Sie die [Diagnoseerweiterung](agents-overview.md#azure-diagnostic-extension), um Protokolldaten von virtuellen Azure-Computern und dem [Log Analytics-Agent](agents-overview.md#log-analytics-agent) weiterzuleiten und so Protokolle und Metriken von virtuellen Computern in Azure, in anderen Clouds und lokal in einem Log Analytics Arbeitsbereich zu erfassen. Ausführliche Informationen finden Sie unter [Quellen für Überwachungsdaten für Azure Monitor](data-sources.md).

## <a name="resource-logs-schema"></a>Ressourcenprotokollschema
Weitere Informationen zum Ressourcenprotokollschema und zu den Kategorien finden Sie unter [Ressourcenprotokollschema](diagnostic-logs-schema.md). 

## <a name="next-steps"></a>Nächste Schritte

* [Erfahren Sie mehr über andere Protokolle der Azure-Plattform](platform-logs-overview.md), die Sie zum Überwachen von Azure verwenden können.
