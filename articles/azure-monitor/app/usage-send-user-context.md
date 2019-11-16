---
title: Senden von Benutzerkontext-IDs zur Aktivierung der Nutzung in Azure Application Insights | Microsoft-Dokumentation
description: Verfolgen Sie nach, welche Vorgänge Benutzer in Ihrem Dienst ausführen, indem Sie ihnen in Application Insights eine eindeutige, persistente ID-Zeichenfolge zuweisen.
ms.service: azure-monitor
ms.subservice: application-insights
ms.topic: conceptual
author: NumberByColors
ms.author: daviste
ms.date: 01/03/2019
ms.reviewer: abgreg;mbullwin
ms.openlocfilehash: b105e8ba79e99be02420ca1821f6a080b0862d6f
ms.sourcegitcommit: cf36df8406d94c7b7b78a3aabc8c0b163226e1bc
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 11/09/2019
ms.locfileid: "73887412"
---
# <a name="send-user-context-ids-to-enable-usage-experiences-in-azure-application-insights"></a>Senden von Benutzerkontext-IDs zur Nutzung in Azure Application Insights

## <a name="tracking-users"></a>Nachverfolgen von Benutzern

Mithilfe von Application Insights können Sie Ihre Benutzer durch eine Reihe von Produktnutzungstools überwachen und nachverfolgen:

- [Benutzer, Sitzungen, Ereignisse](https://docs.microsoft.com/azure/application-insights/app-insights-usage-segmentation)
- [Trichter](https://docs.microsoft.com/azure/application-insights/usage-funnels)
- [Aufbewahrungskohorten](https://docs.microsoft.com/azure/application-insights/app-insights-usage-retention)
- [Arbeitsmappen](https://docs.microsoft.com/azure/application-insights/app-insights-usage-workbooks)

Um nachzuverfolgen, welche Vorgänge ein Benutzer im Laufe der Zeit durchführt, erfordert Application Insights eine ID für jeden Benutzer bzw. jede Sitzung. Schließen Sie die folgenden IDs in jedem benutzerdefinierten Ereignis bzw. in jeder Seitenansicht ein.

- Benutzer, Verkaufstrichter, Vermerkdauer und Kohorten: Fügen Sie die Benutzer-ID ein.
- Sitzungen: Fügen Sie die Sitzungs-ID ein.

> [!NOTE]
> In diesem Artikel werden die manuellen Schritte zum Nachverfolgen der Benutzeraktivität mit Application Insights erläutert. Bei vielen Webanwendungen **sind diese Schritte möglicherweise nicht erforderlich**, da die serverseitigen Standard-SDKs zusammen mit dem [clientseitigen/browserseitigen JavaScript SDK](../../azure-monitor/app/website-monitoring.md ) oftmals ausreichend sind, um die Benutzeraktivität automatisch nachzuverfolgen. Wenn Sie zusätzlich zum serverseitigen SDK die [clientseitige Überwachung](../../azure-monitor/app/website-monitoring.md ) noch nicht konfiguriert haben, erledigen Sie dies zunächst, und testen Sie anschließend, ob die Analysetools für Benutzerverhalten wie erwartet ausgeführt werden.

## <a name="choosing-user-ids"></a>Auswählen von Benutzer-IDs

Benutzer-IDs sollten benutzersitzungsübergreifend beibehalten werden, um das Benutzerverhalten im Laufe der Zeit nachzuverfolgen. Es gibt verschiedene Methoden zur Beibehaltung der ID.

- Behalten Sie sie als Definition eines Benutzers bei, der bereits in Ihrem Dienst vorhanden ist.
- Wenn der Dienst Zugriff auf einen Browser hat, kann ein Cookie mit einer ID an den Browser übergeben werden. Die ID wird solange beibehalten wie das Cookie im Browser des Benutzers.
- Bei Bedarf können Sie für jede Sitzung eine neue ID verwenden, die Ergebnisse bezüglich der Benutzer sind jedoch begrenzt. Beispielsweise können Sie nicht feststellen, inwiefern sich das Verhalten eines Benutzers im Laufe der Zeit ändert.

Bei der ID muss es sich um eine GUID oder eine andere komplexe Zeichenfolge handeln, die den Benutzer eindeutig identifiziert. Verwenden Sie beispielsweise eine lange Zufallszahl.

Eine ID, die personenbezogene Informationen über den Benutzer enthält, ist kein geeigneter Wert, der als Benutzer-ID an Application Insights gesendet werden kann. Sie können diese ID zwar als [authentifizierte Benutzer-ID](https://docs.microsoft.com/azure/application-insights/app-insights-api-custom-events-metrics#authenticated-users) senden, sie erfüllt jedoch nicht die Anforderung an Benutzer-ID für Verwendungsszenarien.

## <a name="aspnet-apps-setting-the-user-context-in-an-itelemetryinitializer"></a>ASP.NET-Apps: Festlegen des Benutzerkontexts in ITelemetryInitializer

Erstellen Sie einen Telemetrieinitialisierer. Eine ausführliche Beschreibung finden Sie [hier](https://docs.microsoft.com/azure/application-insights/app-insights-api-filtering-sampling#addmodify-properties-itelemetryinitializer). Übergeben Sie die Sitzungs-ID über die Anforderungstelemetrie, und legen Sie „Context.User.Id“ und „Context.Session.Id“ fest.

In diesem Beispiel wird die Benutzer-ID auf einen Bezeichner festgelegt, der nach der Sitzung abläuft. Wenn möglich, verwenden Sie eine Benutzer-ID, die sitzungsübergreifend beibehalten wird.

### <a name="telemetry-initializer"></a>Telemetrieinitialisierer

```csharp
using System;
using System.Web;
using Microsoft.ApplicationInsights.Channel;
using Microsoft.ApplicationInsights.Extensibility;

namespace MvcWebRole.Telemetry
{
  /*
   * Custom TelemetryInitializer that sets the user ID.
   *
   */
  public class MyTelemetryInitializer : ITelemetryInitializer
  {
    public void Initialize(ITelemetry telemetry)
    {
        var ctx = HttpContext.Current;

        // If telemetry initializer is called as part of request execution and not from some async thread
        if (ctx != null)
        {
            var requestTelemetry = ctx.GetRequestTelemetry();
 
            // Set the user and session ids from requestTelemetry.Context.User.Id, which is populated in Application_PostAcquireRequestState in Global.asax.cs.
            if (requestTelemetry != null && !string.IsNullOrEmpty(requestTelemetry.Context.User.Id) &&
                (string.IsNullOrEmpty(telemetry.Context.User.Id) || string.IsNullOrEmpty(telemetry.Context.Session.Id)))
            {
                // Set the user id on the Application Insights telemetry item.
                telemetry.Context.User.Id = requestTelemetry.Context.User.Id;
 
                // Set the session id on the Application Insights telemetry item.
                telemetry.Context.Session.Id = requestTelemetry.Context.User.Id;
            }
        }
    }
  }
}
```

### <a name="globalasaxcs"></a>Global.asax.cs

```csharp
using System.Web;
using System.Web.Http;
using System.Web.Mvc;
using System.Web.Optimization;
using System.Web.Routing;

namespace MvcWebRole.Telemetry
{
    public class MvcApplication : HttpApplication
    {
        protected void Application_Start()
        {
            AreaRegistration.RegisterAllAreas();
            GlobalConfiguration.Configure(WebApiConfig.Register);
            FilterConfig.RegisterGlobalFilters(GlobalFilters.Filters);
            RouteConfig.RegisterRoutes(RouteTable.Routes);
            BundleConfig.RegisterBundles(BundleTable.Bundles);
        }
 
        protected void Application_PostAcquireRequestState()
        {
            var requestTelemetry = Context.GetRequestTelemetry();
 
            if (HttpContext.Current.Session != null && requestTelemetry != null && string.IsNullOrEmpty(requestTelemetry.Context.User.Id))
            {
                requestTelemetry.Context.User.Id = Session.SessionID;
            }
        }
    }
}
```

## <a name="next-steps"></a>Nächste Schritte

- Um mit der Nutzung zu beginnen, senden Sie [benutzerdefinierte Ereignisse](https://docs.microsoft.com/azure/application-insights/app-insights-api-custom-events-metrics#trackevent) oder [Seitenansichten](https://docs.microsoft.com/azure/application-insights/app-insights-api-custom-events-metrics#page-views).
- Wenn Sie bereits benutzerdefinierte Ereignisse oder Seitenansichten senden, finden Sie mithilfe der Nutzungstools heraus, wie Benutzer den Dienst verwenden.
    - [Nutzungsübersicht](usage-overview.md)
    - [Benutzer-, Sitzungs- und Ereignisanalyse in Azure Application Insights](usage-segmentation.md)
    - [Trichter](usage-funnels.md)
    - [Vermerkdauer](usage-retention.md)
    - [Arbeitsmappen](../../azure-monitor/app/usage-workbooks.md)
