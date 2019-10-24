---
title: Übersicht über den Azure Application Insights-Agent | Microsoft-Dokumentation
description: Hier finden Sie eine Übersicht über den Application Insights-Agent. Überwachen Sie die Websiteleistung ohne erneute Bereitstellung der Website. Funktioniert mit ASP.NET-Web-Apps, die lokal, auf virtuellen Computern oder in Azure gehostet werden.
services: application-insights
documentationcenter: .net
author: TimothyMothra
manager: alexklim
ms.assetid: 769a5ea4-a8c6-4c18-b46c-657e864e24de
ms.service: application-insights
ms.workload: tbd
ms.tgt_pltfrm: ibiza
ms.topic: conceptual
ms.date: 09/16/2019
ms.author: tilee
ms.openlocfilehash: 294b0d2d91650f33f0b92179a069a8c7cd845525
ms.sourcegitcommit: bb65043d5e49b8af94bba0e96c36796987f5a2be
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 10/16/2019
ms.locfileid: "72389837"
---
# <a name="deploy-azure-monitor-application-insights-agent-for-on-premises-servers"></a>Bereitstellen des Azure Monitor Application Insights-Agents für lokale Server

> [!IMPORTANT]
> Diese Anleitung wird für lokale Bereitstellungen von Application Insights-Agent und Bereitstellungen in einer anderen Cloud als Azure empfohlen. Die empfohlene Vorgehensweise für Bereitstellungen auf Azure-VMs und in VM-Skalierungsgruppen finden Sie [hier](https://docs.microsoft.com/azure/azure-monitor/app/azure-vm-vmss-apps).

Application Insights-Agent (früher Statusmonitor V2) ist ein im [PowerShell-Katalog](https://www.powershellgallery.com/packages/Az.ApplicationMonitor) veröffentlichtes PowerShell-Modul.
Es ersetzt den [Statusmonitor](https://docs.microsoft.com/azure/azure-monitor/app/monitor-performance-live-website-now).
Telemetriedaten werden an das Azure-Portal gesendet, wo Sie Ihre App [überwachen](https://docs.microsoft.com/azure/azure-monitor/app/app-insights-overview) können.

> [!NOTE]
> Das Modul unterstützt derzeit nur die codefreie Instrumentierung von mit IIS gehosteten .NET-Web-Apps. Verwenden Sie ein SDK zum Instrumentieren von ASP.NET Core-, Java- und Node.js-Anwendungen.

## <a name="powershell-gallery"></a>PowerShell-Katalog

Sie finden den Application Insights-Agent unter https://www.powershellgallery.com/packages/Az.ApplicationMonitor.

![PowerShell-Katalog](https://img.shields.io/powershellgallery/v/Az.ApplicationMonitor.svg?color=Blue&label=Current%20Version&logo=PowerShell&style=for-the-badge)


## <a name="instructions"></a>Anleitung
- Lesen Sie unsere [Anweisungen für den Einstieg](status-monitor-v2-get-started.md), um mithilfe präziser Codebeispiele starten zu können.
- Lesen Sie unsere [ausführlichen Anweisungen](status-monitor-v2-detailed-instructions.md), um detaillierte Informationen zu den ersten Schritten zu erhalten.

## <a name="powershell-api-reference"></a>PowerShell-API-Referenz
- [Disable-ApplicationInsightsMonitoring](status-monitor-v2-api-disable-monitoring.md)
- [Disable-InstrumentationEngine](status-monitor-v2-api-disable-instrumentation-engine.md)
- [Enable-ApplicationInsightsMonitoring](status-monitor-v2-api-enable-monitoring.md)
- [Enable-InstrumentationEngine](status-monitor-v2-api-enable-instrumentation-engine.md)
- [Get-ApplicationInsightsMonitoringConfig](status-monitor-v2-api-get-config.md)
- [Get-ApplicationInsightsMonitoringStatus](status-monitor-v2-api-get-status.md)
- [Set-ApplicationInsightsMonitoringConfig](status-monitor-v2-api-set-config.md)
- [Start-ApplicationInsightsMonitoringTrace](status-monitor-v2-api-start-trace.md)

## <a name="troubleshooting"></a>Problembehandlung
- [Problembehandlung](status-monitor-v2-troubleshoot.md)
- [Bekannte Probleme](status-monitor-v2-troubleshoot.md#known-issues)


## <a name="faq"></a>Häufig gestellte Fragen

- Unterstützt der Application Insights-Agent Proxy-Installationen?

  *Ja*. Es gibt mehrere Möglichkeiten, den Application Insights-Agent herunterzuladen. Wenn Ihr Computer über einen Internetzugang verfügt, können Sie ein Onboarding des PowerShell-Katalogs mithilfe der `-Proxy`-Parameter durchführen.
Sie können dieses Modul auch manuell herunterladen und es auf Ihrem Computer installieren oder direkt verwenden.
Jede dieser Optionen wird in den [ausführlichen Anweisungen](status-monitor-v2-detailed-instructions.md) beschrieben.

- Werden ASP.NET Core-Anwendungen von Version 2 des Statusmonitors unterstützt?

  *Nein*. Anweisungen zum Aktivieren der Überwachung von ASP.NET Core-Anwendungen finden Sie unter [Application Insights für ASP.NET Core-Anwendungen.](https://docs.microsoft.com/azure/azure-monitor/app/asp-net-core) Es ist nicht erforderlich, den Statusmonitor für eine ASP.NET Core-Anwendung zu installieren. Dies gilt auch, wenn die ASP.NET Core-Anwendung in IIS gehostet wird.

- Wie überprüfe ich, ob die Aktivierung erfolgreich war?

  - Mit dem Cmdlet [Get-ApplicationInsightsMonitoringStatus](status-monitor-v2-api-get-status.md) können Sie überprüfen, ob die Aktivierung erfolgreich war.
  - Es wird empfohlen, anhand von [Livemetriken](https://docs.microsoft.com/azure/azure-monitor/app/live-stream) schnell zu ermitteln, ob Ihre App Telemetriedaten sendet.

  - Sie können auch [Log Analytics](../log-query/get-started-portal.md) verwenden, um alle Cloudrollen aufzulisten, die derzeit Telemetriedaten senden.
      ```Kusto
      union * | summarize count() by cloud_RoleName, cloud_RoleInstance
      ```

## <a name="next-steps"></a>Nächste Schritte

Anzeigen der Telemetrie:

* [Untersuchen Sie Metriken](../../azure-monitor/app/metrics-explorer.md) zum Überwachen der Leistung und Nutzung.
* [Durchsuchen Sie Ereignisse und Protokolle](../../azure-monitor/app/diagnostic-search.md), um Probleme zu diagnostizieren.
* Verwenden Sie [Analytics](../../azure-monitor/app/analytics.md) für erweiterte Abfragen.
* [Erstellen Sie Dashboards](../../azure-monitor/app/overview-dashboard.md).

Hinzufügen weiterer Telemetrieelemente:

* [Erstellen Sie Webtests](monitor-web-app-availability.md), um sicherzustellen, dass Ihre Website live bleibt.
* [Fügen Sie Webclient-Telemetriedaten hinzu](../../azure-monitor/app/javascript.md), um Ausnahmen von Webseitencode anzuzeigen und Ablaufverfolgungsaufrufe zu aktivieren.
* [Fügen Sie Ihrem Code das Application Insights SDK hinzu](../../azure-monitor/app/asp-net.md), um Ablaufverfolgungs- und Protokollaufrufe einfügen zu können.

