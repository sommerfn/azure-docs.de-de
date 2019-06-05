---
title: Tutorial zur Verwendung von Featureflags in einer .NET Core-App | Microsoft-Dokumentation
description: In diesem Tutorial erfahren Sie, wie Sie Featureflags in .NET Core-Apps implementieren.
services: azure-app-configuration
documentationcenter: ''
author: yegu-ms
manager: maiye
editor: ''
ms.assetid: ''
ms.service: azure-app-configuration
ms.workload: tbd
ms.devlang: csharp
ms.topic: tutorial
ms.date: 04/19/2019
ms.author: yegu
ms.custom: mvc
ms.openlocfilehash: b0e48a0db63eded9e9c4921d33b03af39656ce0d
ms.sourcegitcommit: 8c49df11910a8ed8259f377217a9ffcd892ae0ae
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 05/29/2019
ms.locfileid: "66299263"
---
# <a name="tutorial-use-feature-flags-in-a-net-core-app"></a>Tutorial: Verwenden von Featureflags in einer .NET Core-App

Die .NET Core-Featureverwaltungsbibliotheken bieten idiomatische Unterstützung für die Implementierung von Featureflags in einer .NET- oder ASP.NET Core-Anwendung. Dank ihnen können Sie Ihrem Code Featureflags auf deklarativere Weise hinzufügen, um nicht sämtliche entsprechenden Anweisungen vom Typ `if` manuell schreiben zu müssen. Sie verwalten Featureflag-Lebenszyklen im Hintergrund (Aktualisieren und Zwischenspeichern von Flagzuständen, Garantieren, dass ein Flagzustand während eines Anforderungsaufrufs unveränderlich ist, und Ähnliches). Darüber hinaus bietet die ASP.NET Core-Bibliothek sofort einsetzbare Integrationen – einschließlich MVC-Controlleraktionen, Ansichten, Routen und Middleware.

In der Schnellstartanleitung [Add feature flags to an ASP.NET Core app](./quickstart-feature-flag-aspnet-core.md) (Hinzufügen von Featureflags zu einer ASP.NET Core-App) werden mehrere Methoden gezeigt, mit denen Sie Featureflags in einer ASP.NET Core-Anwendung hinzufügen können. Diese werden im vorliegenden Tutorial näher erläutert. Eine umfassende Referenz finden Sie in der [Featureverwaltungsdokumentation für ASP.NET Core](https://go.microsoft.com/fwlink/?linkid=2091410).

In diesem Lernprogramm lernen Sie Folgendes:

> [!div class="checklist"]
> * Hinzufügen von Featureflags in wichtigen Teilen Ihrer Anwendung, um die Verfügbarkeit von Features zu steuern
> * Integrieren in App Configuration, wenn Sie damit Featureflags verwalten

## <a name="setup"></a>Einrichtung

Der .NET Core-Feature-Manager `IFeatureManager` ruft Featureflags aus dem nativen Konfigurationssystem des Frameworks ab. Dadurch können Sie die Featureflags Ihrer Anwendung mit einer beliebigen, von .NET Core unterstützten Konfigurationsquelle konfigurieren – unter anderem mit der lokalen Datei *appsettings.json* oder mit Umgebungsvariablen. Der Feature-Manager basiert auf der .NET Core-Abhängigkeitsinjektion. Die Featureverwaltungsdienste können unter Verwendung von Standardkonventionen registriert werden.

```csharp
using Microsoft.FeatureManagement;

public class Startup
{
    public void ConfigureServices(IServiceCollection services)
    {
        services.AddFeatureManagement();
    }
}
```

Der Feature-Manager ruft Featureflags standardmäßig aus dem Abschnitt „FeatureManagement“ der .NET Core-Konfigurationsdaten ab. Im folgenden Beispiel wird er angewiesen, sie stattdessen aus einem Abschnitt namens „MyFeatureFlags“ zu lesen:

```csharp
using Microsoft.FeatureManagement;

public class Startup
{
    public void ConfigureServices(IServiceCollection services)
    {
        services.AddFeatureManagement(options =>
        {
                options.UseConfiguration(Configuration.GetSection("MyFeatureFlags"));
        });
    }
}
```

Wenn Sie in Ihren Featureflags Filter verwenden, müssen Sie eine zusätzliche Bibliothek einschließen und registrieren. Das folgende Beispiel zeigt, wie Sie einen integrierten Featurefilter namens **PercentageFilter** verwenden:

```csharp
using Microsoft.FeatureManagement;
using Microsoft.FeatureManagement.FeatureFilters;

public class Startup
{
    public void ConfigureServices(IServiceCollection services)
    {
        services.AddFeatureManagement()
                .AddFeatureFilter<PercentageFilter>();
    }
}
```

Aus Effektivitätsgründen sollten sich Featureflags außerhalb der Anwendung befinden und separat verwaltet werden. Dadurch können Sie Flagzustände jederzeit ändern, und die Änderungen werden in der Anwendung sofort wirksam. Mit App Configuration können Sie alle Ihre Featureflags über eine dedizierte Portalbenutzeroberfläche an einem zentralen Ort organisieren und steuern, und App Configuration übermittelt die Flags über seine .NET Core-Clientbibliotheken direkt an Ihre Anwendung. Die Verbindung zwischen Ihrer ASP.NET Core-Anwendung und App Configuration lässt sich am einfachsten über den Konfigurationsanbieter `Microsoft.Extensions.Configuration.AzureAppConfiguration` herstellen. Sie können dieses NuGet-Paket in Ihrem Code verwenden, indem Sie der Datei *Program.cs* Folgendes hinzufügen:

```csharp
using Microsoft.Extensions.Configuration.AzureAppConfiguration;

public static IWebHostBuilder CreateWebHostBuilder(string[] args) =>
    WebHost.CreateDefaultBuilder(args)
           .ConfigureAppConfiguration((hostingContext, config) => {
               var settings = config.Build();
               config.AddAzureAppConfiguration(options => {
                   options.Connect(settings["ConnectionStrings:AppConfig"])
                          .UseFeatureFlags();
                });
           })
           .UseStartup<Startup>();
```

Die Werte von Featureflags ändern sich üblicherweise im Laufe der Zeit. Der Feature-Manager aktualisiert die Werte von Featureflags standardmäßig alle 30 Sekunden. Sie können im obigen Aufruf `options.UseFeatureFlags()` ein anderes Abrufintervall verwenden.

```csharp
config.AddAzureAppConfiguration(options => {
    options.Connect(settings["ConnectionStrings:AppConfig"])
           .UseFeatureFlags(featureFlagOptions => {
                featureFlagOptions.PollInterval = TimeSpan.FromSeconds(5);
           });
});
```

## <a name="feature-flag-declaration"></a>Featureflagdeklaration

Jedes Featureflag besteht aus zwei Teilen: einem Namen und einer Filterliste, anhand der ausgewertet wird, ob ein Feature *aktiviert* ist (der Wert also `True` lautet). Ein Filter definiert einen Anwendungsfall, für den ein Feature aktiviert werden soll. Verfügt ein Featureflag über mehrere Filter, wird die Filterliste in der angegebenen Reihenfolge durchlaufen, bis einer der Filter angibt, dass das Feature aktiviert werden soll. Daraufhin wird das Featureflag als *aktiviert* betrachtet, und alle weiteren Filterergebnisse werden übersprungen. Falls durch keinen Filter angegeben wird, dass das Feature aktiviert werden soll, ist das Featureflag *deaktiviert*.

Der Feature-Manager unterstützt *appsettings.json* als Konfigurationsquelle für Featureflags. Das folgende Beispiel zeigt die Einrichtung von Featureflags in einer JSON-Datei:

```JSON
"FeatureManagement": {
    "FeatureX": true, // Feature flag set to on
    "FeatureY": false, // Feature flag set to off
    "FeatureC": {
        "EnabledFor": [
            {
                "Name": "Percentage",
                "Parameters": {
                    "Value": 50
                }
            }
        ]
    }
}
```

Der Abschnitt `FeatureManagement` dieses JSON-Dokuments wird konventionsgemäß für Featureflageinstellungen verwendet. Das obige Beispiel enthält drei Featureflags, deren Filter in der Eigenschaft *EnabledFor* definiert sind:

* **FeatureX** ist *aktiviert*.
* **FeatureY** ist *deaktiviert*.
* **FeatureC** gibt einen Filter namens *Percentage* mit einer Eigenschaft vom Typ *Parameters* an. *Percentage* ist ein Beispiel für einen konfigurierbaren Filter und gibt eine 50-prozentige Wahrscheinlichkeit dafür an, dass das Flag **FeatureC** *aktiviert* ist.

## <a name="referencing"></a>Verwenden von Verweisen

Featureflags sollten als Variablen vom Typ `enum` definiert werden, damit im Code problemlos auf sie verwiesen werden kann. (Dies ist jedoch nicht zwingend erforderlich.)

```csharp
public enum MyFeatureFlags
{
    FeatureA,
    FeatureB,
    FeatureC
}
```

## <a name="feature-flag-check"></a>Überprüfen von Featureflags

Bei der Featureverwaltung wird grundsätzlich zunächst geprüft, ob ein Featureflag *aktiviert* ist. Ist dies der Fall, werden die eingeschlossenen Aktionen ausgeführt.

```csharp
IFeatureManager featureManager;
...
if (featureManager.IsEnabled(nameof(MyFeatureFlags.FeatureA)))
{
    // Run the following code
}
```

## <a name="dependency-injection"></a>Abhängigkeitsinjektion

In ASP.NET Core MVC kann auf den Feature-Manager `IFeatureManager` mittels Abhängigkeitsinjektion zugegriffen werden.

```csharp
public class HomeController : Controller
{
    private readonly IFeatureManager _featureManager;

    public HomeController(IFeatureManager featureManager)
    {
        _featureManager = featureManager;
    }
}
```

## <a name="controller-action"></a>Controlleraktion

In MVC-Controllern kann mithilfe eines Attributs vom Typ `Feature` gesteuert werden, ob eine gesamte Controllerklasse oder eine spezifische Aktion aktiviert wird. Für den folgenden Controller `HomeController` muss *FeatureA* *aktiviert* sein, damit eine der enthaltenen Aktionen ausgeführt werden kann:

```csharp
[Feature(MyFeatureFlags.FeatureA)]
public class HomeController : Controller
{
    ...
}
```

Für die folgende Aktion `Index` muss *FeatureA* *aktiviert* sein, damit sie ausgeführt werden kann:

```csharp
[Feature(MyFeatureFlags.FeatureA)]
public IActionResult Index()
{
    return View();
}
```

Ist ein MVC-Controller oder eine Aktion blockiert, weil das steuernde Featureflag *deaktiviert* ist, wird ein registrierter Handler (`IDisabledFeatureHandler`) aufgerufen. Der Standardhandler `IDisabledFeatureHandler` gibt den Statuscode 404 ohne Antworttext an den Client zurück.

## <a name="view"></a>Sicht

In MVC-Ansichten kann ein Tag vom Typ `<feature>` verwendet werden, um das Rendern von Inhalten vom Aktivierungsstatus eines Featureflags abhängig zu machen.

```html
<feature name="FeatureA">
    <p>This can only be seen if 'FeatureA' is enabled.</p>
</feature>
```

## <a name="mvc-filter"></a>MVC-Filter

MVC-Filter können so eingerichtet werden, dass sie abhängig vom Zustand eines Featureflags aktiviert werden. Im folgenden Beispiel wird ein MVC-Filter namens `SomeMvcFilter` hinzugefügt. Dieser Filter wird innerhalb der MVC-Pipeline nur ausgelöst, wenn *FeatureA* aktiviert ist.

```csharp
using Microsoft.FeatureManagement.FeatureFilters;

IConfiguration Configuration { get; set;}

public void ConfigureServices(IServiceCollection services)
{
    services.AddMvc(options => {
        options.Filters.AddForFeature<SomeMvcFilter>(nameof(MyFeatureFlags.FeatureA));
    });
}
```

## <a name="route"></a>Weiterleiten

Routen können dynamisch auf der Grundlage von Featureflags verfügbar gemacht werden. Im folgenden Beispiel wird eine Route hinzugefügt, die `Beta` als den Standardcontroller festlegt – aber nur, wenn *FeatureA* aktiviert ist:

```csharp
app.UseMvc(routes => {
    routes.MapRouteForFeature(nameof(MyFeatureFlags.FeatureA), "betaDefault", "{controller=Beta}/{action=Index}/{id?}");
});
```

## <a name="middleware"></a>Middleware

Featureflags ermöglichen das bedingte Hinzufügen von Anwendungsbranches und Middleware. Im folgenden Beispiel wird nur dann eine Middlewarekomponente in die Anforderungspipeline eingefügt, wenn *FeatureA* aktiviert ist.

```csharp
app.UseMiddlewareForFeature<ThirdPartyMiddleware>(nameof(MyFeatureFlags.FeatureA));
```

Dies baut auf der allgemeineren Verzweigung der gesamten Anwendung auf der Grundlage eines Featureflags auf.

```csharp
app.UseForFeature(featureName, appBuilder => {
    appBuilder.UseMiddleware<T>();
});
```

## <a name="next-steps"></a>Nächste Schritte

In diesem Tutorial haben Sie gelernt, wie Sie Featureflags in Ihrer ASP.NET Core-Anwendung unter Verwendung der Bibliotheken vom Typ `Microsoft.FeatureManagement` implementieren. Die folgenden Ressourcen enthalten weitere Informationen zur Unterstützung der Featureverwaltung in ASP.NET Core und App Configuration:

* [Beispielcode für ASP.NET Core-Featureflag](/azure/azure-app-configuration/quickstart-feature-flag-aspnet-core)
* [Dokumentation für „Microsoft.FeatureManagement“](https://docs.microsoft.com/dotnet/api/microsoft.featuremanagement)
* [Verwalten von Featureflags](./manage-feature-flags.md)
