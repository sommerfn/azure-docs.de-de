---
title: Überwachen der Nutzung und Leistung bei Windows-Desktop-Apps
description: Analysieren Sie die Nutzung und Leistung Ihrer Windows-Desktop-App mit Application Insights.
ms.service: azure-monitor
ms.subservice: application-insights
ms.topic: conceptual
author: mrbullwinkle
ms.author: mbullwin
ms.date: 10/29/2019
ms.openlocfilehash: a9dfc32a0f33db5639d5f74667a90a248dc358a1
ms.sourcegitcommit: 87efc325493b1cae546e4cc4b89d9a5e3df94d31
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 10/29/2019
ms.locfileid: "73052455"
---
# <a name="monitoring-usage-and-performance-in-classic-windows-desktop-apps"></a>Überwachen der Nutzung und Leistung von klassischen Windows-Desktop-Apps

Lokal, in Azure oder in anderen Clouds gehostete Anwendungen können Application Insights nutzen. Die einzige Einschränkung besteht darin, dass die [Kommunikation](../../azure-monitor/app/ip-addresses.md) mit dem Application Insights-Dienst zugelassen werden muss. Für die Überwachung von UWP-Anwendungen (universelle Windows-Plattform) wird [Visual Studio App Center](../../azure-monitor/learn/mobile-center-quickstart.md) empfohlen.

## <a name="to-send-telemetry-to-application-insights-from-a-classic-windows-application"></a>So senden Sie Telemetriedaten aus einer klassischen Windows-Anwendung an Application Insights
1. Erstellen Sie im [Azure-Portal](https://portal.azure.com) [eine Application Insights-Ressource](../../azure-monitor/app/create-new-resource.md ). Wählen Sie als Anwendungstyp "ASP.NET-App" aus.
2. Erstellen Sie eine Kopie des Instrumentierungsschlüssels. Diesen finden Sie in der Dropdownliste „Essentials“ der neuen Ressource, die Sie gerade erstellt haben. 
3. Bearbeiten Sie die NuGet-Pakete Ihres App-Projekts in Visual Studio, und fügen Sie ihnen „Microsoft.ApplicationInsights.WindowsServer“ hinzu. (Oder wählen Sie „Microsoft.ApplicationInsights“ aus, wenn Sie nur die reine API ohne die Standardmodule zur Telemetrieerfassung verwenden möchten.)
4. Legen Sie den Instrumentierungsschlüssel im Code fest:
   
    `TelemetryConfiguration.Active.InstrumentationKey = "`*Ihr Schlüssel*`";`
   
    Oder legen Sie ihn in der Datei „ApplicationInsights.config“ fest (wenn Sie eines der Standardtelemetriepakete installiert haben):
   
    `<InstrumentationKey>`*Ihr Schlüssel*`</InstrumentationKey>` 
   
    Wenn Sie die Datei „ApplicationInsights.config“ verwenden, stellen Sie sicher, dass ihre Eigenschaften im Projektmappen-Explorer auf **Buildvorgang = Inhalt, In Ausgabeverzeichnis kopieren = Kopieren**festgelegt sind.
5. [Verwenden Sie die API](../../azure-monitor/app/api-custom-events-metrics.md) , um Telemetriedaten zu senden.
6. Führen Sie die App aus, und sehen Sie sich die Telemetriedaten in der Ressource an, die Sie im Azure-Portal erstellt haben.

## <a name="telemetry"></a>Beispielcode
```csharp
using Microsoft.ApplicationInsights;

    public partial class Form1 : Form
    {
        private TelemetryClient tc = new TelemetryClient();
        ...
        private void Form1_Load(object sender, EventArgs e)
        {
            // Alternative to setting ikey in config file:
            tc.InstrumentationKey = "key copied from portal";

            // Set session data:
            tc.Context.Session.Id = Guid.NewGuid().ToString();
            tc.Context.Device.OperatingSystem = Environment.OSVersion.ToString();

            // Log a page view:
            tc.TrackPageView("Form1");
            ...
        }

        protected override void OnClosing(System.ComponentModel.CancelEventArgs e)
        {
            e.Cancel = true;

            if (tc != null)
            {
                tc.Flush(); // only for desktop apps

                // Allow time for flushing:
                System.Threading.Thread.Sleep(1000);
            }
            base.OnClosing(e);
        }

```

## <a name="override-storage-of-computer-name"></a>Außerkraftsetzung des gespeicherten Computernamens

Standardmäßig erfasst und speichert dieses SDK den Computernamen des Systems, das Telemetriedaten ausgibt. Zum Überschreiben der Erfassung müssen Sie einen Telemetrie-Initialisierer verwenden:

**Schreiben Sie einen benutzerdefinierten Telemetrie-Initialisierer wie nachfolgend gezeigt.**

```csharp
using Microsoft.ApplicationInsights.Channel;
using Microsoft.ApplicationInsights.Extensibility;

namespace CustomInitializer.Telemetry
{
    public class MyTelemetryInitializer : ITelemetryInitializer
    {
        public void Initialize(ITelemetry telemetry)
        {
            if (string.IsNullOrEmpty(telemetry.Context.Cloud.RoleName))
            {
                //set custom role name here, you can pass an empty string if needed.
                  telemetry.Context.Cloud.RoleInstance = "Custom RoleInstance";
            }
        }
    }
}
```
Instanziieren Sie den Initialisierer in der `Program.cs` `Main()`-Methode unten, indem Sie den Instrumentierungsschlüssel festlegen:

```csharp
 using Microsoft.ApplicationInsights.Extensibility;
 using CustomInitializer.Telemetry;

   static void Main()
        {
            TelemetryConfiguration.Active.InstrumentationKey = "{Instrumentation-key-here}";
            TelemetryConfiguration.Active.TelemetryInitializers.Add(new MyTelemetryInitializer());
        }
```

## <a name="next-steps"></a>Nächste Schritte
* [Erstellen eines Dashboards](../../azure-monitor/app/overview-dashboard.md)
* [Diagnosesuche](../../azure-monitor/app/diagnostic-search.md)
* [Untersuchen von Metriken](../../azure-monitor/app/metrics-explorer.md)
* [Schreiben von Analytics-Abfragen](../../azure-monitor/app/analytics.md)

