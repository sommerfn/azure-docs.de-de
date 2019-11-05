---
title: Verwenden der Azure-Protokollanalyse zur Untersuchung von Protokollen der Web Application Firewall für Application Gateway
description: Diese Artikel zeigt, wie Sie die Azure-Protokollanalyse zur Untersuchung von Protokollen der Web Application Firewall für Application Gateway verwenden.
services: web-application-firewall
author: vhorne
ms.service: web-application-firewall
ms.topic: article
ms.date: 09/06/2019
ms.author: victorh
ms.openlocfilehash: e1bc3b58f425b374e4cae1da6e9800579e503f0d
ms.sourcegitcommit: c22327552d62f88aeaa321189f9b9a631525027c
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 11/04/2019
ms.locfileid: "73511439"
---
# <a name="use-log-analytics-to-examine-application-gateway-web-application-firewall-logs"></a>Verwenden der Protokollanalyse zur Untersuchung von Protokollen der Web Application Firewall für Application Gateway

Sobald Ihre Application Gateway-WAF betriebsbereit ist, können Sie Protokolle aktivieren, um zu überprüfen, was bei jeder Anforderung passiert. Firewallprotokolle geben Aufschluss darüber, was die WAF auswertet, abgleicht und blockiert. Mit der Protokollanalyse können Sie die Daten in den Firewallprotokollen untersuchen, um noch mehr Erkenntnisse zu erhalten. Weitere Informationen zum Erstellen eines Log Analytics-Arbeitsbereichs finden Sie unter [Erstellen eines Log Analytics-Arbeitsbereichs im Azure-Portal](../../azure-monitor/learn/quick-create-workspace.md). Weitere Informationen zu Protokollabfragen finden Sie unter [Übersicht über Protokollabfragen in Azure Monitor](../../azure-monitor/log-query/log-query-overview.md).

## <a name="import-waf-logs"></a>Importieren von WAF-Protokollen

Informationen zum Importieren von Firewallprotokollen in Log Analytics finden Sie unter [Back-End-Integrität, Diagnoseprotokolle und Metriken für Application Gateway](../../application-gateway/application-gateway-diagnostics.md#diagnostic-logging). Wenn sich die Firewallprotokolle in Ihrem Log Analytics-Arbeitsbereich befinden, können Sie Daten anzeigen, Abfragen schreiben, Visualisierungen erstellen und sie zu Ihrem Portaldashboard hinzufügen.

## <a name="explore-data-with-examples"></a>Erkunden von Daten anhand von Beispielen

Um die Rohdaten im Firewallprotokoll anzuzeigen, können Sie die folgende Abfrage ausführen:

```
AzureDiagnostics 
| where ResourceProvider == "MICROSOFT.NETWORK" and Category == "ApplicationGatewayFirewallLog"
```

Diese sieht etwa wie die folgende Abfrage aus:

![Log Analytics-Abfrage](../media/log-analytics/log-query.png)

Von hier aus können Sie einen Drilldown für die Daten durchführen und Grafiken oder Visualisierungen erstellen. Die folgenden Abfragen können Sie als Ausgangspunkt verwenden:

### <a name="matchedblocked-requests-by-ip"></a>Von IP abgeglichene/blockierte Anforderungen

```
AzureDiagnostics
| where ResourceProvider == "MICROSOFT.NETWORK" and Category == "ApplicationGatewayFirewallLog"
| summarize count() by clientIp_s, bin(TimeGenerated, 1m)
| render timechart
```

### <a name="matchedblocked-requests-by-uri"></a>Von URI abgeglichene/blockierte Anforderungen

```
AzureDiagnostics
| where ResourceProvider == "MICROSOFT.NETWORK" and Category == "ApplicationGatewayFirewallLog"
| summarize count() by requestUri_s, bin(TimeGenerated, 1m)
| render timechart
```

### <a name="top-matched-rules"></a>Ersten übereinstimmende Regeln

```
AzureDiagnostics
| where ResourceProvider == "MICROSOFT.NETWORK" and Category == "ApplicationGatewayFirewallLog"
| summarize count() by ruleId_s, bin(TimeGenerated, 1m)
| where count_ > 10
| render timechart
```

### <a name="top-five-matched-rule-groups"></a>Die ersten fünf übereinstimmenden Regelgruppen

```
AzureDiagnostics
| where ResourceProvider == "MICROSOFT.NETWORK" and Category == "ApplicationGatewayFirewallLog"
| summarize Count=count() by details_file_s, action_s
| top 5 by Count desc
| render piechart
```

## <a name="add-to-your-dashboard"></a>Hinzufügen zum Dashboard

Nachdem Sie eine Abfrage erstellt haben, können Sie sie zu Ihrem Dashboard hinzufügen.  Wählen Sie **An Dashboard anheften** in der oberen rechten Ecke des Log Analytics-Arbeitsbereichs aus. Wenn die vorherigen vier Abfragen an ein Beispieldashboard angeheftet sind, sind dies die Daten, die Sie auf einen Blick sehen können:

![Dashboard](../media/log-analytics/dashboard.png)

## <a name="next-steps"></a>Nächste Schritte

[Back-End-Integrität, Diagnoseprotokolle und Metriken für Application Gateway](../../application-gateway/application-gateway-diagnostics.md)