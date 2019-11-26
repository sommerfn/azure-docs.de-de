---
title: Azure Application Insights – Automatisches Sammeln von Abhängigkeiten | Microsoft-Dokumentation
description: Mit Application Insights können Abhängigkeiten automatisch gesammelt und visualisiert werden.
ms.service: azure-monitor
ms.subservice: application-insights
ms.topic: reference
author: mrbullwinkle
ms.author: mbullwin
ms.date: 10/16/2018
ms.openlocfilehash: 1d3259c32de7f937d4e5ac8e21c8cab60b9cc635
ms.sourcegitcommit: 8e271271cd8c1434b4254862ef96f52a5a9567fb
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 10/23/2019
ms.locfileid: "72819177"
---
# <a name="application-insights-nuget-packages"></a>Application Insights-NuGet-Pakete

Nachfolgend finden Sie eine Liste der stabilen NuGet-Releasepakete für Application Insights.

## <a name="common-packages-for-aspnet"></a>Gängige Pakete für ASP.NET

| Paketname | Stabile Version | BESCHREIBUNG | Download |
|-------------------------------|-----------------------|------------|----|
| Microsoft.ApplicationInsights | 2.8.0 | Stellt die Kernfunktionalität zum Übertragen aller Application Insights-Telemetrietypen bereit und ist ein abhängiges Paket für alle weitere Application Insights-Pakete. | [Paket herunterladen](https://www.nuget.org/packages/Microsoft.ApplicationInsights/) |
|Microsoft.ApplicationInsights.Agent.Intercept | 2.4.0 | Ermöglicht das Abfangen von Methodenaufrufen. | [Paket herunterladen](https://www.nuget.org/packages/Microsoft.ApplicationInsights.Agent.Intercept/) |
| Microsoft.ApplicationInsights.DependencyCollector | 2.8.0 | Application Insights-Abhängigkeitserfassung für .NET-Anwendungen. Dies ist ein abhängiges Paket für plattformspezifische Application Insights-Pakete, das eine automatische Erfassung der Abhängigkeitstelemetrie ermöglicht. | [Paket herunterladen](https://www.nuget.org/packages/Microsoft.ApplicationInsights.DependencyCollector/) |
| Microsoft.ApplicationInsights.PerfCounterCollector | 2.8.0 | Mithilfe der Erfassung von Application Insights-Leistungsindikatoren können Sie über Leistungsindikatoren gesammelte Daten an Application Insights senden. | [Paket herunterladen](https://www.nuget.org/packages/Microsoft.ApplicationInsights.PerfCounterCollector/) |
| Microsoft.ApplicationInsights.Web | 2.8.0 | Application Insights für .NET-Webanwendungen | [Paket herunterladen](https://www.nuget.org/packages/Microsoft.ApplicationInsights.Web/) |
| Microsoft.ApplicationInsights.WindowsServer | 2.8.0 | Das Windows Server-NuGet-Paket für Application Insights bietet eine automatische Sammlung von Telemetriedaten für .NET-Anwendungen, um Anwendungserkenntnisse zu gewinnen. Dieses Paket kann als abhängiges Paket für plattformspezifische Application Insights-Pakete oder als eigenständiges Paket für .NET-Anwendungen verwendet werden, die nicht durch plattformspezifische Pakete abgedeckt werden (z.B. für .NET-Workerrollen). | [Paket herunterladen](https://www.nuget.org/packages/Microsoft.ApplicationInsights.WindowsServer/)  
| Microsoft.ApplicationInsights.WindowsServer.TelemetryChannel | 2.8.0 | Bietet einen Telemetriekanal zum Windows Server SDK für Application Insights, der Telemetriedaten in Offlineszenarien beibehält. | [Paket herunterladen](https://www.nuget.org/packages/Microsoft.ApplicationInsights.WindowsServer.TelemetryChannel/) |

## <a name="common-packages-for-aspnet-core"></a>Gängige Pakete für ASP.NET Core

| Paketname | Stabile Version | BESCHREIBUNG | Download |
|-------------------------------|-----------------------|------------|----|
| Microsoft.ApplicationInsights.AspNetCore | 2.5.0 | Application Insights für ASP.NET Core-Webanwendungen. | [Paket herunterladen](https://www.nuget.org/packages/Microsoft.ApplicationInsights.AspNetCore/) |
| Microsoft.ApplicationInsights | 2.8.0 | Dieses Paket stellt die Kernfunktionalität zum Übertragen aller Application Insights-Telemetrietypen bereit und ist ein abhängiges Paket für alle weitere Application Insights-Pakete. | [Paket herunterladen](https://www.nuget.org/packages/Microsoft.ApplicationInsights/) |
| Microsoft.ApplicationInsights.DependencyCollector | 2.8.0 | Application Insights-Abhängigkeitserfassung für .NET-Anwendungen. Dies ist ein abhängiges Paket für plattformspezifische Application Insights-Pakete, das eine automatische Erfassung der Abhängigkeitstelemetrie ermöglicht. | [Paket herunterladen](https://www.nuget.org/packages/Microsoft.ApplicationInsights.DependencyCollector/) |
| Microsoft.ApplicationInsights.PerfCounterCollector | 2.8.0 | Mithilfe der Erfassung von Application Insights-Leistungsindikatoren können Sie über Leistungsindikatoren gesammelte Daten an Application Insights senden. | [Paket herunterladen](https://www.nuget.org/packages/Microsoft.ApplicationInsights.PerfCounterCollector/) |
| Microsoft.ApplicationInsights.WindowsServer | 2.8.0 | Das Windows Server-NuGet-Paket für Application Insights bietet eine automatische Sammlung von Telemetriedaten für .NET-Anwendungen, um Anwendungserkenntnisse zu gewinnen. Dieses Paket kann als abhängiges Paket für plattformspezifische Application Insights-Pakete oder als eigenständiges Paket für .NET-Anwendungen verwendet werden, die nicht durch plattformspezifische Pakete abgedeckt werden (z.B. für .NET-Workerrollen). | [Paket herunterladen](https://www.nuget.org/packages/Microsoft.ApplicationInsights.WindowsServer/)  |
| Microsoft.ApplicationInsights.WindowsServer.TelemetryChannel | 2.8.0 | Bietet einen Telemetriekanal zum Windows Server SDK für Application Insights, der Telemetriedaten in Offlineszenarien beibehält. | [Paket herunterladen](https://www.nuget.org/packages/Microsoft.ApplicationInsights.WindowsServer.TelemetryChannel/) |

## <a name="common-packages-for-python-using-opencensus"></a>Gängige Pakete für Python mit OpenCensus
| Paketname | Stabile Version | BESCHREIBUNG | Download |
|-------------------------------|-----------------------|------------|----|
| opencensus-ext-azure | 1.0.0 | Application Insights für Python-Anwendungen unter Azure Monitor über OpenCensus. | [Paket herunterladen](https://pypi.org/project/opencensus-ext-azure/) |
| opencensus-ext-django | 0.7.2 | Dieses Paket ermöglicht die Integration in die Python-Datenbank [django](https://pypi.org/project/django/). | [Paket herunterladen](https://pypi.org/project/opencensus-ext-django/) |
| opencensus-ext-flask | 0.7.3 | Dieses Paket ermöglicht die Integration in die Python-Datenbank [flask](https://pypi.org/project/flask/). | [Paket herunterladen](https://pypi.org/project/opencensus-ext-flask/) |
| opencensus-ext-httplib | 0.7.2 | Dieses Paket ermöglicht die Integration in die Python-Datenbank [http.client](https://docs.python.org/3/library/http.client.html) für Python3 und [httplib](https://docs.python.org/2/library/httplib.html) für Python2. | [Paket herunterladen](https://pypi.org/project/opencensus-ext-httplib/) |
| opencensus-ext-logging | 0.1.0 | Mit diesem Paket werden Protokolldatensätze um Ablaufverfolgungsdaten erweitert. | [Paket herunterladen](https://pypi.org/project/opencensus-ext-logging/) |
| opencensus-ext-mysql | 0.1.2 | Dieses Paket ermöglicht die Integration in die Python-Datenbank [mysql-connector](https://pypi.org/project/mysql-connector/). | [Paket herunterladen](https://pypi.org/project/opencensus-ext-mysql/) |
| opencensus-ext-postgresql | 0.1.2 | Dieses Paket ermöglicht die Integration in die Python-Datenbank [psycopg2](https://pypi.org/project/psycopg2/). | [Paket herunterladen](https://pypi.org/project/opencensus-ext-postgresql/) |
| opencensus-ext-pymongo | 0.7.1 | Dieses Paket ermöglicht die Integration in die Python-Datenbank [pymongo](https://pypi.org/project/pymongo/). | [Paket herunterladen](https://pypi.org/project/opencensus-ext-pymongo/) |
| opencensus-ext-pymysql | 0.1.2 | Dieses Paket ermöglicht die Integration in die Python-Datenbank [PyMySQL](https://pypi.org/project/PyMySQL/). | [Paket herunterladen](https://pypi.org/project/opencensus-ext-pymysql/) |
| opencensus-ext-pyramid | 0.7.1 | Dieses Paket ermöglicht die Integration in die Python-Datenbank [pyramid](https://pypi.org/project/pyramid/). | [Paket herunterladen](https://pypi.org/project/opencensus-ext-pyramid/) |
| opencensus-ext-requests | 0.7.2 | Dieses Paket ermöglicht die Integration in die Python-Datenbank [requests](https://pypi.org/project/requests/). | [Paket herunterladen](https://pypi.org/project/opencensus-ext-requests/) |
| opencensus-ext-sqlalchemy | 0.1.2 | Dieses Paket ermöglicht die Integration in die Python-Datenbank [SQLAlchemy](https://pypi.org/project/SQLAlchemy/). | [Paket herunterladen](https://pypi.org/project/opencensus-ext-sqlalchemy/) |

## <a name="listenerscollectorsappenders"></a>Listener/Sammler/Appender

| Paketname | Stabile Version | BESCHREIBUNG | Download |
|-------------------------------|-----------------------|------------|----|
| Microsoft.ApplicationInsights.DiagnosticSourceListener | 2.7.2 |  Ermöglicht das Weiterleiten von Ereignissen aus DiagnosticSource an Application Insights. | [Paket herunterladen](https://www.nuget.org/packages/Microsoft.ApplicationInsights.DiagnosticSourceListener/) |
| Microsoft.ApplicationInsights.EventSourceListener | 2.7.2 | Der EventSourceListener für Application Insights ermöglicht das Senden von Daten aus EventSource-Ereignissen an Application Insights. | [Paket herunterladen](https://www.nuget.org/packages/Microsoft.ApplicationInsights.EventSourceListener/) |
| Microsoft.ApplicationInsights.EtwCollector | 2.7.2 | Der EtwCollector von Application Insights ermöglicht das Senden von Daten aus der Ereignisablaufverfolgung für Windows (ETW) an Application Insights. | [Paket herunterladen](https://www.nuget.org/packages/Microsoft.ApplicationInsights.EtwCollector/) |
| Microsoft.ApplicationInsights.TraceListener | 2.7.2 | Ein benutzerdefinierter TraceListener ermöglicht das Senden von Ablaufverfolgungsmeldungen an Application Insights. | [Paket herunterladen](https://www.nuget.org/packages/Microsoft.ApplicationInsights.TraceListener/) |
| Microsoft.ApplicationInsights.Log4NetAppender | 2.7.2 | Ein benutzerdefinierter Appender ermöglicht das Senden von Log4Net-Protokollmeldungen an Application Insights. | [Paket herunterladen](https://www.nuget.org/packages/Microsoft.ApplicationInsights.Log4NetAppender/)
| Microsoft.ApplicationInsights.NLogTarget | 2.7.2 |  Ein benutzerdefiniertes Ziel ermöglicht das Senden von NLog-Protokollmeldungen an Application Insights. | [Paket herunterladen](https://www.nuget.org/packages/Microsoft.ApplicationInsights.NLogTarget/)
| Microsoft.ApplicationInsights.SnapshotCollector | 1.3.1 | Hiermit werden Ausnahmen in Ihrer Anwendung überwacht und automatisch Momentaufnahmen zur Offlineanalyse erstellt. | [Paket herunterladen](https://www.nuget.org/packages/Microsoft.ApplicationInsights.SnapshotCollector/)

## <a name="service-fabric"></a>Service Fabric

| Paketname | Stabile Version | BESCHREIBUNG | Download |
|-------------------------------|-----------------------|------------|----|
| Microsoft.ApplicationInsights.ServiceFabric | 2.2.0 | Dieses Paket ermöglicht eine automatische Ergänzung der Telemetrie mit dem Service Fabric-Kontext, in der die Anwendung ausgeführt wird. Verwenden Sie dieses NuGet-Paket nicht für native Service Fabric-Anwendungen. | [Paket herunterladen](https://www.nuget.org/packages/Microsoft.ApplicationInsights.ServiceFabric/) |
| Microsoft.ApplicationInsights.ServiceFabric.Native | 2.2.0 | Application Insights-Modul für Service Fabric-Anwendungen. Verwenden Sie dieses NuGet-Paket nur für native Service Fabric-Anwendungen. Verwenden Sie für Anwendungen, die in Containern ausgeführt werden, das Microsoft.ApplicationInsights.ServiceFabric-Paket. | [Paket herunterladen](https://www.nuget.org/packages/Microsoft.ApplicationInsights.ServiceFabric.Native/) |  

## <a name="status-monitor"></a>Statusmonitor

| Paketname | Stabile Version | BESCHREIBUNG | Download |
|-------------------------------|-----------------------|------------|----|
| Microsoft.ApplicationInsights.Agent_x64 | 2.2.1 |  Ermöglicht eine Lauftzeitdatensammlung für x64-Anwendungen. | [Paket herunterladen](https://www.nuget.org/packages/Microsoft.ApplicationInsights.Agent_x64/) |
| Microsoft.ApplicationInsights.Agent_x86 | 2.2.1 |  Ermöglicht eine Lauftzeitdatensammlung für x86-Anwendungen. | [Paket herunterladen](https://www.nuget.org/packages/Microsoft.ApplicationInsights.Agent_x86/) |

Diese Pakete sind Bestandteil der Kernfunktionalität der Laufzeitüberwachung im [Statusmonitor](../../azure-monitor/app/monitor-performance-live-website-now.md). Sie müssen diese Pakete nicht direkt herunterladen, verwenden Sie einfach den Installer für den Statusmonitor. Wenn Sie mehr über die Funktionsweise dieser Pakete erfahren möchten, stellen diese [Blogbeiträge](https://apmtips.com/blog/2016/11/18/how-application-insights-status-monitor-not-monitors-dependencies/) von einem unserer Entwickler einen guten Einstiegspunkt dar.

## <a name="additional-packages"></a>Zusätzliche Pakete

| Paketname | Stabile Version | BESCHREIBUNG | Download |
|-------------------------------|-----------------------|------------|----|
| Microsoft.ApplicationInsights.AzureWebSites | 2.6.5 | Diese Erweiterung ermöglicht eine Application Insights-Überwachung für Azure App Service. SDK-Version 2.6.1. Anleitung: Fügen Sie APPINSIGHTS_INSTRUMENTATIONKEY-Anwendungseinstellungen mit Ihrem „ikey“ hinzu, und starten Sie die Web-App neu, damit die Einstellungen wirksam werden.| [Paket herunterladen](https://www.nuget.org/packages/Microsoft.ApplicationInsights.AzureWebSites/) |
| Microsoft.ApplicationInsights.Injector | 2.6.7 | Dieses Paket enthält Dateien, die für eine Application Insights-Einschleusung ohne Code benötigt werden. | [Paket herunterladen](https://www.nuget.org/packages/Microsoft.ApplicationInsights.Injector/) |

## <a name="next-steps"></a>Nächste Schritte

- Überwachen von [ASP.NET Core](../../azure-monitor/app/asp-net-core.md)
- Profilerstellung für [ASP.NET Core-Azure Linux-Web-Apps](../../azure-monitor/app/profiler-aspnetcore-linux.md)
- Debuggen von ASP.NET-[Momentaufnahmen](../../azure-monitor/app/snapshot-debugger.md)