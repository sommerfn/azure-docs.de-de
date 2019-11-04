---
title: 'Tutorial: Verwenden der dynamischen Konfiguration von Azure App Configuration in einer ASP.NET Core-App | Microsoft-Dokumentation'
description: In diesem Tutorial wird beschrieben, wie Sie die Konfigurationsdaten für ASP.NET Core-Apps dynamisch aktualisieren.
services: azure-app-configuration
documentationcenter: ''
author: lisaguthrie
manager: maiye
editor: ''
ms.assetid: ''
ms.service: azure-app-configuration
ms.workload: tbd
ms.devlang: csharp
ms.topic: tutorial
ms.date: 02/24/2019
ms.author: lcozzens
ms.custom: mvc
ms.openlocfilehash: 7fc7bd6fa0067857bde64d43be5799bd50712490
ms.sourcegitcommit: c22327552d62f88aeaa321189f9b9a631525027c
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 11/04/2019
ms.locfileid: "73469685"
---
# <a name="tutorial-use-dynamic-configuration-in-an-aspnet-core-app"></a>Tutorial: Verwenden der dynamischen Konfiguration in einer ASP.NET Core-App

ASP.NET Core verfügt über ein austauschbares Konfigurationssystem, die Konfigurationsdaten aus vielen verschiedenen Quellen lesen kann. Änderungen können nebenbei verarbeitet werden, ohne dass eine Anwendung neu gestartet werden muss. ASP.NET Core unterstützt die Bindung von Konfigurationseinstellungen an stark typisierte .NET-Klassen. Sie werden mithilfe der verschiedenen `IOptions<T>`-Muster in Ihren Code eingefügt. Eines dieser Muster (`IOptionsSnapshot<T>`) lädt die Anwendungskonfiguration automatisch neu, wenn die zugrunde liegenden Daten geändert werden. Sie können `IOptionsSnapshot<T>` in Controller in Ihrer Anwendung einfügen, um auf die aktuellste Konfiguration zuzugreifen, die in Azure App Configuration gespeichert ist.

Sie können auch die App Configuration-ASP.NET Core-Clientbibliothek einrichten, um eine Reihe von Konfigurationseinstellungen dynamisch per Middleware zu aktualisieren. Solange die Web-App weiterhin Anforderungen empfängt, werden die Konfigurationseinstellungen über den Konfigurationsspeicher aktualisiert.

Um die Einstellungen auf dem aktuellen Stand zu halten und zu viele Aufrufe des Konfigurationsspeichers zu vermeiden, wird ein Cache für jede Einstellung verwendet. Bis der zwischengespeicherte Wert einer Einstellung abgelaufen ist, wird der Wert vom Aktualisierungsvorgang nicht aktualisiert. Dies gilt auch, wenn sich der Wert im Konfigurationsspeicher geändert hat. Die Standardablaufzeit jeder Anforderung beträgt 30 Sekunden, aber dies kann bei Bedarf außer Kraft gesetzt werden.

In diesem Tutorial wird veranschaulicht, wie Sie dynamische Konfigurationsupdates in Ihrem Code implementieren können. Dies baut auf der Web-App auf, die in den Schnellstartanleitungen vorgestellt wurde. Führen Sie zuerst den Schnellstart [Erstellen einer ASP.NET Core-App mit Azure App Configuration](./quickstart-aspnet-core-app.md) durch, bevor Sie fortfahren.

Für die Ausführung der Schritte dieses Tutorials können Sie einen beliebigen Code-Editor verwenden. [Visual Studio Code](https://code.visualstudio.com/) ist eine hervorragende Option, die auf Windows-, macOS- und Linux-Plattformen verfügbar ist.

In diesem Tutorial lernen Sie Folgendes:

> [!div class="checklist"]
> * Einrichten Ihrer Anwendung für die Aktualisierung der Konfiguration als Reaktion auf Änderungen in einem App-Konfigurationsspeicher
> * Einfügen der aktuellen Konfiguration in die Controller Ihrer Anwendung

## <a name="prerequisites"></a>Voraussetzungen

Installieren Sie für dieses Tutorial das [.NET Core SDK](https://dotnet.microsoft.com/download).

[!INCLUDE [quickstarts-free-trial-note](../../includes/quickstarts-free-trial-note.md)]

Durchlaufen Sie zuerst die Schnellstartanleitung zum [Erstellen einer ASP.NET Core-App mit Azure App Configuration](./quickstart-aspnet-core-app.md), bevor Sie fortfahren.

## <a name="reload-data-from-app-configuration"></a>Erneutes Laden von Daten aus App Configuration

1. Fügen Sie einen Verweis auf das NuGet-Paket `Microsoft.Azure.AppConfiguration.AspNetCore` hinzu, indem Sie den folgenden Befehl ausführen:

    ```CLI
        dotnet add package Microsoft.Azure.AppConfiguration.AspNetCore --version 2.0.0-preview-010060003-1250
    ```

1. Öffnen Sie die Datei *Program.cs*, und aktualisieren Sie die `CreateWebHostBuilder`-Methode, um die `config.AddAzureAppConfiguration()`-Methode hinzuzufügen.

    ```csharp
    public static IWebHostBuilder CreateWebHostBuilder(string[] args) =>
        WebHost.CreateDefaultBuilder(args)
            .ConfigureAppConfiguration((hostingContext, config) =>
            {
                var settings = config.Build();

                config.AddAzureAppConfiguration(options =>
                {
                    options.Connect(settings["ConnectionStrings:AppConfig"])
                           .ConfigureRefresh(refresh =>
                           {
                               refresh.Register("TestApp:Settings:BackgroundColor")
                                      .Register("TestApp:Settings:FontColor")
                                      .Register("TestApp:Settings:Message");
                           });
                });
            })
            .UseStartup<Startup>();
    ```

    Die `ConfigureRefresh`-Methode wird genutzt, um die Einstellungen anzugeben, die zum Aktualisieren der Konfigurationsdaten mit dem App-Konfigurationsspeicher verwendet werden, wenn ein Aktualisierungsvorgang ausgelöst wird. Um wirklich einen Aktualisierungsvorgang auszulösen, muss eine Aktualisierungsmiddleware für die Anwendung konfiguriert werden. Hiermit können die Konfigurationsdaten dann aktualisiert werden, wenn es zu einer Änderung kommt.

2. Fügen Sie die Datei *Settings.cs* hinzu, mit der eine neue `Settings`-Klasse definiert und implementiert wird.

    ```csharp
    namespace TestAppConfig
    {
        public class Settings
        {
            public string BackgroundColor { get; set; }
            public long FontSize { get; set; }
            public string FontColor { get; set; }
            public string Message { get; set; }
        }
    }
    ```

3. Öffnen Sie die Datei *Startup.cs*, und aktualisieren Sie die `ConfigureServices`-Methode, um Konfigurationsdaten an die `Settings`-Klasse zu binden.

    ```csharp
    public void ConfigureServices(IServiceCollection services)
    {
        services.Configure<Settings>(Configuration.GetSection("TestApp:Settings"));

        services.Configure<CookiePolicyOptions>(options =>
        {
            options.CheckConsentNeeded = context => true;
            options.MinimumSameSitePolicy = SameSiteMode.None;
        });

        services.AddMvc().SetCompatibilityVersion(CompatibilityVersion.Version_2_1);
    }
    ```

4. Aktualisieren Sie die `Configure`-Methode so, dass Middleware hinzugefügt wird, mit der die für die Aktualisierung registrierten Konfigurationseinstellungen aktualisiert werden können, während die ASP.NET Core-Web-App weiterhin Anforderungen empfängt.

    ```csharp
    public void Configure(IApplicationBuilder app, IHostingEnvironment env)
    {
        app.UseAzureAppConfiguration();
        app.UseMvc();
    }
    ```
    
    Die Middleware nutzt die in der `AddAzureAppConfiguration`-Methode in `Program.cs` angegebene Aktualisierungskonfiguration, um eine Aktualisierung für jede Anforderung auszulösen, die von der ASP.NET Core-Web-App empfangen wird. Für jede Anforderung wird ein Aktualisierungsvorgang ausgelöst, und die Clientbibliothek überprüft, ob der zwischengespeicherte Wert für die registrierten Konfigurationseinstellungen abgelaufen ist. Für abgelaufene zwischengespeicherten Werte werden die Werte für die Einstellungen mit dem App-Konfigurationsspeicher aktualisiert, und die verbleibenden Werte bleiben unverändert.
    
    > [!NOTE]
    > Die Standardablaufzeit für eine Konfigurationseinstellung beträgt 30 Sekunden, aber dieser Wert kann außer Kraft gesetzt werden. Rufen Sie hierzu die `SetCacheExpiration`-Methode im Initialisierer für die Optionen auf, die als Argument an die `ConfigureRefresh`-Methode übergeben wird.

## <a name="use-the-latest-configuration-data"></a>Verwenden der aktuellsten Konfigurationsdaten

1. Öffnen Sie *HomeController.cs* im Verzeichnis „Controllers“, und fügen Sie einen Verweis auf das Paket `Microsoft.Extensions.Options` hinzu.

    ```csharp
    using Microsoft.Extensions.Options;
    ```

2. Aktualisieren Sie die `HomeController`-Klasse, um `Settings` per Abhängigkeitsinjektion zu erhalten, und verwenden Sie die darin enthaltenen Werte.

    ```csharp
    public class HomeController : Controller
    {
        private readonly Settings _settings;
        public HomeController(IOptionsSnapshot<Settings> settings)
        {
            _settings = settings.Value;
        }

        public IActionResult Index()
        {
            ViewData["BackgroundColor"] = _settings.BackgroundColor;
            ViewData["FontSize"] = _settings.FontSize;
            ViewData["FontColor"] = _settings.FontColor;
            ViewData["Message"] = _settings.Message;

            return View();
        }
    }
    ```

3. Öffnen Sie im Verzeichnis „Views > Home“ die Datei *Index.cshtml*, und ersetzen Sie den Inhalt durch das folgende Skript:

    ```html
    <!DOCTYPE html>
    <html lang="en">
    <style>
        body {
            background-color: @ViewData["BackgroundColor"]
        }
        h1 {
            color: @ViewData["FontColor"];
            font-size: @ViewData["FontSize"];
        }
    </style>
    <head>
        <title>Index View</title>
    </head>
    <body>
        <h1>@ViewData["Message"]</h1>
    </body>
    </html>
    ```

## <a name="build-and-run-the-app-locally"></a>Lokales Erstellen und Ausführen der App

1. Führen Sie den folgenden Befehl in der Befehlsshell aus, um die App mithilfe der .NET Core-CLI zu erstellen:

        dotnet build

2. Führen Sie nach der erfolgreichen Erstellung den folgenden Befehl aus, um die Web-App lokal auszuführen:

        dotnet run

3. Öffnen Sie ein Browserfenster, und navigieren Sie zu `http://localhost:5000`. Dies ist die Standard-URL für die lokal gehostete Web-App.

    ![Schnellstartanleitung: Lokales Starten der App](./media/quickstarts/aspnet-core-app-launch-local-before.png)

4. Melden Sie sich beim [Azure-Portal](https://portal.azure.com) an. Wählen Sie **Alle Ressourcen** aus, und wählen Sie dann die Instanz des App-Konfigurationsspeichers aus, die Sie in der Schnellstartanleitung erstellt haben.

5. Wählen Sie den **Konfigurations-Explorer** aus, und aktualisieren Sie die Werte der folgenden Schlüssel:

    | Schlüssel | Wert |
    |---|---|
    | TestApp:Settings:BackgroundColor | green |
    | TestApp:Settings:FontColor | lightGray |
    | TestApp:Settings:Message | Daten aus Azure App Configuration – jetzt mit Liveupdates! |

6. Aktualisieren Sie die Browserseite, um die neuen Konfigurationseinstellungen anzuzeigen. Unter Umständen ist mehr als eine Aktualisierung der Browserseite erforderlich, damit die Änderungen widergespiegelt werden.

    ![Schnellstart: Lokale Aktualisierung der App](./media/quickstarts/aspnet-core-app-launch-local-after.png)
    
    > [!NOTE]
    > Da die Konfigurationseinstellungen mit einer Standardablaufzeit von 30 Sekunden zwischengespeichert werden, werden alle Änderungen, die an den Einstellungen im App-Konfigurationsspeicher vorgenommen werden, nur nach Ablauf des Caches in der Web-App widergespiegelt.

## <a name="clean-up-resources"></a>Bereinigen von Ressourcen

[!INCLUDE [azure-app-configuration-cleanup](../../includes/azure-app-configuration-cleanup.md)]

## <a name="next-steps"></a>Nächste Schritte

In diesem Tutorial haben Sie eine verwaltete Azure-Dienstidentität hinzugefügt, um den Zugriff auf App Configuration zu optimieren und die Verwaltung der Anmeldeinformationen für Ihre App zu verbessern. Fahren Sie mit den Azure CLI-Beispielen fort, um mehr über die Verwendung von App Configuration zu erfahren.

> [!div class="nextstepaction"]
> [CLI-Beispiele](./cli-samples.md)
