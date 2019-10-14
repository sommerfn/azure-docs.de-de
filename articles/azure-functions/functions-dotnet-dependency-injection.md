---
title: Verwenden der Abhängigkeitsinjektion in Azure Functions (.NET)
description: Verwenden von Abhängigkeitsinjektionen für die Registrierung und Verwendung von Diensten in .NET-Funktionen
services: functions
documentationcenter: na
author: craigshoemaker
manager: gwallace
keywords: Azure-Funktionen, Funktionen, serverlose Architektur
ms.service: azure-functions
ms.devlang: dotnet
ms.topic: reference
ms.date: 09/05/2019
ms.author: cshoe
ms.reviewer: jehollan
ms.openlocfilehash: de8782edcc8b9c64621f1ca67d4bb810c926afaf
ms.sourcegitcommit: c2e7595a2966e84dc10afb9a22b74400c4b500ed
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 10/05/2019
ms.locfileid: "71973383"
---
# <a name="use-dependency-injection-in-net-azure-functions"></a>Verwenden der Abhängigkeitsinjektion in Azure Functions (.NET)

Azure Functions unterstützt das Softwareentwurfsmuster „Abhängigkeitsinjektion“ (Dependency Injection, DI). Damit kann eine [Umkehrung der Steuerung (Inversion of Control, IoC)](https://docs.microsoft.com/dotnet/standard/modern-web-apps-azure-architecture/architectural-principles#dependency-inversion) zwischen Klassen und ihren Abhängigkeiten erreicht werden.

- Die Abhängigkeitsinjektion in Azure Functions basiert auf den .NET Core-Features für die Abhängigkeitsinjektion. Vertrautheit mit der [.NET Core- Abhängigkeitsinjektion](https://docs.microsoft.com/aspnet/core/fundamentals/dependency-injection) wird empfohlen. Es bestehen jedoch Unterschiede darin, wie Sie Abhängigkeiten überschreiben und wie Konfigurationswerte mit Azure Functions im Verbrauchstarif gelesen werden.

- Die Unterstützung der Abhängigkeitsinjektion beginnt mit Azure Functions 2.x.

## <a name="prerequisites"></a>Voraussetzungen

Bevor Sie die Abhängigkeitsinjektion verwenden können, müssen Sie die folgenden NuGet-Pakete installieren:

- [Microsoft.Azure.Functions.Extensions](https://www.nuget.org/packages/Microsoft.Azure.Functions.Extensions/)

- [Microsoft.NET.Sdk.Functions-Paket](https://www.nuget.org/packages/Microsoft.NET.Sdk.Functions/), Version 1.0.28 oder höher

## <a name="register-services"></a>Registrieren von Diensten

Um Dienste zu registrieren, erstellen Sie eine Methode zum Konfigurieren und Hinzufügen von Komponenten zu einer `IFunctionsHostBuilder`-Instanz.  Der Azure Functions-Host erstellt eine Instanz von `IFunctionsHostBuilder` und übergibt diese direkt an Ihre Methode.

Fügen Sie zum Registrieren der Methode das `FunctionsStartup`-Assembly-Attribut hinzu, mit dem der Typname angegeben wird, der beim Starten verwendet wird.

```csharp
using System;
using Microsoft.Azure.Functions.Extensions.DependencyInjection;
using Microsoft.Extensions.DependencyInjection;
using Microsoft.Extensions.Http;
using Microsoft.Extensions.Logging;

[assembly: FunctionsStartup(typeof(MyNamespace.Startup))]

namespace MyNamespace
{
    public class Startup : FunctionsStartup
    {
        public override void Configure(IFunctionsHostBuilder builder)
        {
            builder.Services.AddHttpClient();

            builder.Services.AddSingleton((s) => {
                return new MyService();
            });

            builder.Services.AddSingleton<ILoggerProvider, MyLoggerProvider>();
        }
    }
}
```

### <a name="caveats"></a>Einschränkungen

Eine Reihe von Registrierungsschritten wird vor und nach dem Verarbeiten der Startup-Klasse ausgeführt. Berücksichtigen Sie daher die folgenden Punkte:

- *Die Startup-Klasse ist nur für Setup und Registrierung vorgesehen.* Vermeiden Sie die Verwendung von Diensten, die beim Start während des Startvorgangs registriert werden. Versuchen Sie beispielsweise nicht, eine Nachricht in einer Protokollierung zu protokollieren, die während des Starts registriert wird. Dieser Zeitpunkt des Registrierungsprozesses ist zu früh, damit ihre Dienste zur Verwendung verfügbar sind. Nachdem die `Configure`-Methode ausgeführt wurde, registriert die Azure Functions-Laufzeit weiterhin zusätzliche Abhängigkeiten, die sich den Betrieb ihrer Dienste auswirken können.

- *Der Container für die Abhängigkeitsinjektion enthält nur explizit registrierte Typen*. Die einzigen Dienste, die als injizierbare Typen verfügbar sind, sind diejenigen, die in der `Configure`-Methode eingerichtet werden. Folglich sind Azure Functions-spezifische Typen wie `BindingContext` und `ExecutionContext` während des Setups oder als injizierbare Typen nicht verfügbar.

## <a name="use-injected-dependencies"></a>Verwenden von eingefügten Abhängigkeiten

Die Konstruktorinjektion wird verwendet, um Ihre Abhängigkeiten in einer Funktion verfügbar zu machen. Die Verwendung der Konstruktorinjektion erfordert den Verzicht auf statische Klassen.

Im folgenden Beispiel wird veranschaulicht, wie die Abhängigkeiten `IMyService` und `HttpClient` in eine per HTTP ausgelöste Funktion eingefügt werden. In diesem Beispiel wird das Paket [Microsoft.Extensions.Http](https://www.nuget.org/packages/Microsoft.Extensions.Http/) verwendet, das beim Start `HttpClient` registrieren muss.

```csharp
using System;
using System.IO;
using System.Threading.Tasks;
using Microsoft.AspNetCore.Mvc;
using Microsoft.Azure.WebJobs;
using Microsoft.Azure.WebJobs.Extensions.Http;
using Microsoft.AspNetCore.Http;
using Microsoft.Extensions.Logging;

namespace MyNamespace
{
    public class HttpTrigger
    {
        private readonly IMyService _service;
        private readonly HttpClient _client;

        public HttpTrigger(IMyService service, IHttpClientFactory httpClientFactory)
        {
            _service = service;
            _client = httpClientFactory.CreateClient();
        }

        [FunctionName("GetPosts")]
        public async Task<IActionResult> Get(
            [HttpTrigger(AuthorizationLevel.Function, "get", Route = "posts")] HttpRequest req,
            ILogger log)
        {
            log.LogInformation("C# HTTP trigger function processed a request.");
            var res = await _client.GetAsync("https://microsoft.com");
            await _service.AddResponse(res);

            return new OkResult();
        }
    }
}
```

## <a name="service-lifetimes"></a>Dienstlebensdauer

Azure Functions-Apps bieten dieselbe Dienstlebensdauer wie die [ASP.NET-Abhängigkeitsinjektion](https://docs.microsoft.com/aspnet/core/fundamentals/dependency-injection#service-lifetimes). Bei einer Funktions-App weisen die verschiedenen Dienstlebensdauern die folgenden Verhalten auf:

- **Vorübergehend**: Bei jeder Anforderung des Diensts werden vorübergehende Dienste erstellt.
- **Bereichsbezogen**: Die bereichsbezogene Lebensdauer eines Diensts entspricht der Ausführungslebensdauer einer Funktion. Bereichsbezogene Dienste werden einmal pro Ausführung erstellt. In späteren Anforderungen für diesen Dienst während der Ausführung wird die vorhandene Dienstinstanz wiederverwendet.
- **Singleton**: Die Lebensdauer eines Singletondiensts entspricht der Lebensdauer des Hosts und wird für Funktionsausführungen dieser Instanz wiederverwendet. Dienste mit Singleton-Lebensdauer werden für Verbindungen und Clients empfohlen, z. B. Instanzen von `SqlConnection` oder `HttpClient`.

Sie können auf GitHub ein [Beispiel für verschiedene Dienstlebensdauern](https://aka.ms/functions/di-sample) anzeigen bzw. herunterladen.

## <a name="logging-services"></a>Protokollierungsdienste

Wenn Sie einen eigenen Protokollierungsanbieter benötigen, registrieren Sie einen benutzerdefinierten Typ als `ILoggerProvider`-Instanz. Application Insights wird von Azure Functions automatisch hinzugefügt.

> [!WARNING]
> - Fügen Sie `AddApplicationInsightsTelemetry()` nicht der Dienstsammlung hinzu, da sonst Dienste registriert werden, für die Konflikte mit den von der Umgebung bereitgestellten Diensten auftreten können.
> - Registrieren Sie keine eigene Instanz von `TelemetryConfiguration` oder `TelemetryClient`, wenn Sie die integrierten Application Insights-Funktionen verwenden.

## <a name="function-app-provided-services"></a>Dienste, die von Funktions-App bereitgestellt werden

Der Funktionshost registriert viele Dienste. Es ist sicher, die folgenden Dienste als Abhängigkeit in Ihrer Anwendung zu verwenden:

|Service Type|Gültigkeitsdauer|BESCHREIBUNG|
|--|--|--|
|`Microsoft.Extensions.Configuration.IConfiguration`|Singleton|Laufzeitkonfiguration|
|`Microsoft.Azure.WebJobs.Host.Executors.IHostIdProvider`|Singleton|Verantwortlich für die Bereitstellung der Hostinstanz-ID|

[Erstellen Sie ein Issue, und schlagen Sie es auf GitHub vor](https://github.com/azure/azure-functions-host), wenn Sie eine Abhängigkeit von anderen Diensten erstellen möchten.

### <a name="overriding-host-services"></a>Überschreiben von Hostdiensten

Das Überschreiben der vom Host bereitgestellten Dienste wird derzeit nicht unterstützt.  [Erstellen Sie ein Issue, und schlagen Sie es auf GitHub vor](https://github.com/azure/azure-functions-host), falls andere Dienste vorhanden sind, die Sie überschreiben möchten.

## <a name="working-with-options-and-settings"></a>Arbeiten mit Optionen und Einstellungen

Werte, die in [App-Einstellungen](./functions-how-to-use-azure-function-app-settings.md#settings) definiert sind, sind in einer `IConfiguration`-Instanz verfügbar, die es Ihnen ermöglicht, App-Einstellungswerte in der Startup-Klasse zu lesen.

Sie können Werte aus der `IConfiguration`-Instanz in einen benutzerdefinierten Typ extrahieren. Wenn Sie App-Einstellungswerte in einen benutzerdefinierten Typ kopieren, können Sie Ihre Dienste mühelos testen, indem Sie diese Werte injizierbar gestalten. Die in die Konfigurationsinstanz eingelesenen Einstellungen müssen einfache Schlüssel/Wert-Paare sein.

Beachten Sie die folgende Klasse, die eine Eigenschaft mit dem Namen „consistent“ mit einer App-Einstellung enthält.

```csharp
public class MyOptions
{
    public string MyCustomSetting { get; set; }
}
```

Innerhalb der `Startup.Configure`-Methode können Sie Werte aus der `IConfiguration`-Instanz mit dem folgenden Code in Ihren benutzerdefinierten Typ extrahieren:

```csharp
builder.Services.AddOptions<MyOptions>()
                .Configure<IConfiguration>((settings, configuration) =>
                                           {
                                                configuration.Bind(settings);
                                           });
```

Durch den Aufruf von `Bind` werden Werte mit übereinstimmenden Eigenschaftsnamen aus der Konfiguration in die benutzerdefinierte Instanz kopiert. Die Optionsinstanz ist nun im IoC-Container zum Injizieren in eine Funktion verfügbar.

Das Optionsobjekt wird als Instanz der generischen `IOptions`-Schnittstelle in die Funktion injiziert. Verwenden Sie die `Value`-Eigenschaft, um auf die in Ihrer Konfiguration gefundenen Werte zuzugreifen.

```csharp
using System;
using Microsoft.Extensions.Options;

public class HttpTrigger
{
    private readonly MyOptions _settings;

    public HttpTrigger(IOptions<MyOptions> options)
    {
        _service = service;
        _settings = options.Value;
    }
}
```

Weitere Informationen zum Arbeiten mit Optionen finden Sie unter [Optionsmuster in ASP.NET Core](https://docs.microsoft.com/aspnet/core/fundamentals/configuration/options).

> [!WARNING]
> Vermeiden Sie es, Werte aus Dateien wie *local.settings.json* oder *appsettings.{Umgebung}.json* im Verbrauchsplan zu lesen. Werte, die aus diesen Dateien im Zusammenhang mit Triggerverbindungen gelesen werden, sind bei der Skalierung der App nicht verfügbar, da die Hostinginfrastruktur keinen Zugriff auf die Konfigurationsinformationen hat.

## <a name="next-steps"></a>Nächste Schritte

Weitere Informationen finden Sie in den folgenden Ressourcen:

- [Überwachen Ihrer Funktions-App](functions-monitoring.md)
- [Bewährte Methoden für Funktionen](functions-best-practices.md)
