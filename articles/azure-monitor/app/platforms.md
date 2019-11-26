---
title: 'Application Insights: Programmiersprachen, Plattformen und Integrationsmöglichkeiten | Microsoft-Dokumentation'
description: Verfügbare Programmiersprachen, Plattformen und Integrationsmöglichkeiten für Application Insights
ms.service: azure-monitor
ms.subservice: application-insights
ms.topic: conceptual
author: mrbullwinkle
ms.author: mbullwin
ms.date: 07/18/2019
ms.reviewer: olegan
ms.openlocfilehash: 01cebc3a86808d549f1a7dc3adfd2883bc289076
ms.sourcegitcommit: f7f70c9bd6c2253860e346245d6e2d8a85e8a91b
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 10/30/2019
ms.locfileid: "73061446"
---
# <a name="supported-languages"></a>Unterstützte Sprachen

* [C#|VB (.NET)](../../azure-monitor/app/asp-net.md)
* [Java](../../azure-monitor/app/java-get-started.md)
* [JavaScript](../../azure-monitor/app/javascript.md)
* [Node.JS](../../azure-monitor/app/nodejs.md)
* [Python (Vorschauversion)](../../azure-monitor/app/opencensus-python.md)

## <a name="supported-platforms-and-frameworks"></a>Unterstützte Plattformen und Frameworks

### <a name="instrumentation-for-already-deployed-applications-codeless-agent-based"></a>Instrumentierung für bereits bereitgestellte Anwendungen (ohne Code, Agent-basiert)
* [Virtuelle Azure-Computer und Azure-VM-Skalierungsgruppen](../../azure-monitor/app/azure-vm-vmss-apps.md)
* [Azure App Service](../../azure-monitor/app/azure-web-apps.md)
* [ASP.NET – Für Apps, die bereits aktiv sind.](../../azure-monitor/app/monitor-performance-live-website-now.md)
* [Azure Cloud Services](../../azure-monitor/app/cloudservices.md), einschließlich Web- und Workerrollen
* [Azure-Funktionen](https://docs.microsoft.com/azure/azure-functions/functions-monitoring)
### <a name="instrumentation-through-code-sdks"></a>Instrumentierung durch Code (SDKs)
* [ASP.NET](../../azure-monitor/app/asp-net.md)
* [ASP.NET Core](../../azure-monitor/app/asp-net-core.md)
* [Android](../../azure-monitor/learn/mobile-center-quickstart.md) (App Center)
* [iOS](../../azure-monitor/learn/mobile-center-quickstart.md) (App Center)
* [Java EE](../../azure-monitor/app/java-get-started.md)
* [Node.JS](https://www.npmjs.com/package/applicationinsights)
* [Python (Vorschauversion)](../../azure-monitor/app/opencensus-python.md)
* [Universelle Windows-App](../../azure-monitor/learn/mobile-center-quickstart.md) (App Center)
* [Windows-Desktopanwendungen, -Dienste und -Workerrollen](../../azure-monitor/app/windows-desktop.md)

## <a name="logging-frameworks"></a>Protokollierungsframeworks
* [ILogger](https://docs.microsoft.com/azure/azure-monitor/app/ilogger)
* [Log4Net, NLog oder System.Diagnostics.Trace](../../azure-monitor/app/asp-net-trace-logs.md)
* [Java, Log4J oder Logback](../../azure-monitor/app/java-trace-logs.md)
* [LogStash-Plug-in](https://github.com/Azure/azure-diagnostics-tools/tree/master/Logstash/logstash-output-applicationinsights)
* [Azure Monitor](https://blogs.technet.microsoft.com/msoms/2016/09/26/application-insights-connector-in-oms/)

## <a name="export-and-data-analysis"></a>Export und Datenanalyse
* [Power BI](https://blogs.msdn.com/b/powerbi/archive/2015/11/04/explore-your-application-insights-data-with-power-bi.aspx)
* [Stream Analytics](../../azure-monitor/app/export-power-bi.md)

## <a name="unsupported-sdks"></a>Nicht unterstützte SDKs
Wir sind uns bewusst, dass es noch einige andere von der Community unterstützte SDKs gibt. Für Azure Monitor wird aber nur Support geleistet, wenn die auf dieser Seite aufgeführten unterstützten SDKs verwendet werden. Wir prüfen ständig neue Möglichkeiten zur Unterstützung anderer Sprachen. Verfolgen Sie die Seite mit den [GitHub-Ankündigungen](https://github.com/microsoft/ApplicationInsights-Announcements/issues), um die aktuellen SDK-News zu erhalten. 
