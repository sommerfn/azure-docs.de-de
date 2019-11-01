---
title: Profilerstellung für Live-Azure App Service-Apps mit Application Insights | Microsoft-Dokumentation
description: Profilerstellung für Live-Apps in Azure App Service mit Application Insights Profiler
ms.service: azure-monitor
ms.subservice: application-insights
ms.topic: conceptual
author: cweining
ms.author: cweining
ms.date: 08/06/2018
ms.reviewer: mbullwin
ms.openlocfilehash: d463732fc8e8f488851a57fe520f138b101eb6cf
ms.sourcegitcommit: 5acd8f33a5adce3f5ded20dff2a7a48a07be8672
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 10/24/2019
ms.locfileid: "72899942"
---
# <a name="profile-live-azure-app-service-apps-with-application-insights"></a>Profilerstellung für Live-Azure App Service-Apps mit Application Insights

Sie können Profiler für ASP.NET- und ASP.NET Core-Apps verwenden, die unter Azure App Service mit dem Dienstebene „Basic“ oder höher ausgeführt werden. Profiler unter Linux lässt sich derzeit nur mit [dieser Methode](profiler-aspnetcore-linux.md) aktivieren.

## <a id="installation"></a> Aktivieren von Profiler für Ihre App
Befolgen Sie die unten angegebene Anleitung, um Profiler für eine App zu aktivieren. Wenn Sie eine andere Art von Azure-Dienst ausführen, helfen Ihnen diese Anleitungen zum Aktivieren von Profiler auf anderen unterstützten Plattformen weiter:
* [Cloud Services](../../azure-monitor/app/profiler-cloudservice.md?toc=/azure/azure-monitor/toc.json)
* [Service Fabric-Anwendungen](../../azure-monitor/app/profiler-servicefabric.md?toc=/azure/azure-monitor/toc.json)
* [Virtuelle Computer](../../azure-monitor/app/profiler-vm.md?toc=/azure/azure-monitor/toc.json)

Application Insights Profiler ist als Teil der App Services-Runtime bereits vorinstalliert. In den folgenden Schritten wird erläutert, wie Sie das Tool für Ihren App Service aktivieren. Führen Sie diese Schritte aus, auch wenn Sie das App Insights SDK zum Zeitpunkt der Erstellung in Ihrer Anwendung eingefügt haben.

1. Aktivieren Sie die Einstellung „Always On“ für Ihren App Service. Sie können die Einstellung auf der Konfigurationsseite Ihrer App Service-Instanz unter „Allgemeine Einstellungen“ aktualisieren.
1. Wechseln Sie zum Bereich **App Services** im Azure-Portal.
1. Navigieren Sie zum Bereich **Einstellungen > Application Insights**.

   ![Aktivieren von App Insights im App Services-Portal](./media/profiler/AppInsights-AppServices.png)

1. Befolgen Sie die Anweisungen im Bereich zum Erstellen einer neuen Ressource, oder wählen Sie eine vorhandene App Insights-Ressource aus, um Ihre App zu überwachen. Vergewissern Sie sich außerdem, dass der Profiler auf **Ein** festgelegt ist. Wenn sich Ihre Application Insights-Ressource in einem anderen Abonnement als Ihr App Service befindet, können Sie Application Insights auf dieser Seite nicht konfigurieren. Sie können die Konfiguration jedoch manuell vornehmen, indem Sie die gewünschten App-Einstellungen manuell erstellen. [Der nächste Abschnitt enthält Anweisungen zum manuellen Aktivieren von Profiler.](#enable-profiler-manually-or-with-azure-resource-manager) 

   ![Hinzufügen der App Insights-Websiteerweiterung][Enablement UI]

1. Der Profiler wurde mithilfe einer App Services-App-Einstellung aktiviert.

    ![App-Einstellung für Profiler][profiler-app-setting]

## <a name="enable-profiler-manually-or-with-azure-resource-manager"></a>Manuelles Aktivieren von Profiler oder Aktivieren über Azure Resource Manager
Application Insights Profiler kann aktiviert werden, indem Sie App-Einstellungen für Ihren Azure App Service erstellen. Auf der oben abgebildeten Seite mit den Optionen werden diese App-Einstellungen automatisch erstellt. Sie können die Erstellung dieser Einstellungen jedoch auch mithilfe einer Vorlage oder auf andere Weise automatisieren. Diese Einstellungen funktionieren auch dann, wenn sich die Application Insights-Ressource in einem anderen Abonnement als Ihre Azure App Service-Instanz befindet.
Im Folgenden finden Sie die Einstellungen, die zum Aktivieren von Profiler erforderlich sind:

|App-Einstellung    | Wert    |
|---------------|----------|
|APPINSIGHTS_INSTRUMENTATIONKEY         | iKey für Ihre Application Insights-Ressource    |
|APPINSIGHTS_PROFILERFEATURE_VERSION | 1.0.0 |
|DiagnosticServices_EXTENSION_VERSION | ~3 |


Sie können diese Werte mithilfe von [Azure Resource Manager-Vorlagen](../../azure-monitor/app/azure-web-apps.md#app-service-application-settings-with-azure-resource-manager), [Azure PowerShell](https://docs.microsoft.com/powershell/module/az.websites/set-azwebapp) oder der [Azure-Befehlszeilenschnittstelle](https://docs.microsoft.com/cli/azure/webapp/config/appsettings?view=azure-cli-latest) festlegen.

### <a name="enabling-profiler-for-other-clouds-manually"></a>Manuelles Aktivieren von Profiler für andere Clouds

Wenn Sie Profiler für andere Clouds aktivieren möchten, können Sie die folgenden App-Einstellungen verwenden.

|App-Einstellung    | US Government-Werte| China-Cloud |   
|---------------|---------------------|-------------|
|ApplicationInsightsProfilerEndpoint         | https://agent.serviceprofiler.azure.us    | https://profiler.applicationinsights.azure.cn |
|ApplicationInsightsProfilerEndpoint | https://dc.applicationinsights.us | https://dc.applicationinsights.azure.cn |

## <a name="disable-profiler"></a>Deaktivieren von Profiler

Wenn Sie Profiler für eine einzelne Instanz einer App beenden oder neu starten möchten, wechseln Sie unter **Webaufträge** zur App-Ressource. Wenn Sie Profiler löschen möchten, wechseln Sie zu **Erweiterungen**.

![Deaktivieren von Profiler für einen Webauftrag][disable-profiler-webjob]

Es wird empfohlen, Profiler für alle Apps zu aktivieren, um Leistungsprobleme möglichst schnell zu ermitteln.

Profiler-Dateien können gelöscht werden, wenn Änderungen mithilfe von WebDeploy in der Webanwendung bereitgestellt werden. Sie können die Löschung verhindern, indem Sie festlegen, dass der Ordner „App_Data“ vom Löschen bei der Bereitstellung ausgeschlossen wird. 


## <a name="next-steps"></a>Nächste Schritte

* [Verwenden von Azure Application Insights in Visual Studio](https://docs.microsoft.com/azure/application-insights/app-insights-visual-studio)

[Enablement UI]: ./media/profiler/Enablement_UI.png
[profiler-app-setting]:./media/profiler/profiler-app-setting.png
[disable-profiler-webjob]: ./media/profiler/disable-profiler-webjob.png
