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
ms.date: 05/28/2019
ms.author: cshoe
ms.reviewer: jehollan
ms.openlocfilehash: 1ebb2fd77830074648a580dddad98e05e10c9c75
ms.sourcegitcommit: fa45c2bcd1b32bc8dd54a5dc8bc206d2fe23d5fb
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 07/12/2019
ms.locfileid: "67850024"
---
# <a name="use-dependency-injection-in-net-azure-functions"></a>Verwenden der Abhängigkeitsinjektion in Azure Functions (.NET)

Azure Functions unterstützt das Softwareentwurfsmuster „Abhängigkeitsinjektion“ (Dependency Injection, DI). Damit kann eine [Umkehrung der Steuerung (Inversion of Control, IoC)](https://docs.microsoft.com/dotnet/standard/modern-web-apps-azure-architecture/architectural-principles#dependency-inversion) zwischen Klassen und ihren Abhängigkeiten erreicht werden.

Azure Functions basiert auf den Abhängigkeitsinjektionsfunktionen von ASP.NET Core. Wir empfehlen Ihnen, sich mit den Diensten, Lebensdauern und Entwurfsmustern der [ASP.NET Core-Abhängigkeitsinjektion](https://docs.microsoft.com/aspnet/core/fundamentals/dependency-injection) vertraut zu machen, bevor Sie DI-Features in einer Azure Functions-App nutzen.

Die Unterstützung der Abhängigkeitsinjektion beginnt mit Azure Functions 2.x.

## <a name="prerequisites"></a>Voraussetzungen

Bevor Sie die Abhängigkeitsinjektion verwenden können, müssen Sie die folgenden NuGet-Pakete installieren:

- [Microsoft.Azure.Functions.Extensions](https://www.nuget.org/packages/Microsoft.Azure.Functions.Extensions/)

- [Microsoft.NET.Sdk.Functions-Paket](https://www.nuget.org/packages/Microsoft.NET.Sdk.Functions/), Version 1.0.28 oder höher

- Optional: [Microsoft.Extensions.Http](https://www.nuget.org/packages/Microsoft.Extensions.Http/) – nur für die Registrierung von HttpClient beim Starten erforderlich

## <a name="register-services"></a>Registrieren von Diensten

Sie können eine Methode zum Konfigurieren und Hinzufügen von Komponenten zu einer `IFunctionsHostBuilder`-Instanz erstellen.  Der Azure Functions-Host erstellt eine Instanz von `IFunctionsHostBuilder` und übergibt diese direkt an Ihre Methode.

Fügen Sie zum Registrieren der Methode das `FunctionsStartup`-Assembly-Attribut hinzu, mit dem der Typname angegeben wird, der beim Starten verwendet wird. Auch Code verweist auf eine Vorabversion von [Microsoft.Azure.Cosmos](https://www.nuget.org/packages/Microsoft.Azure.Cosmos/) auf Nuget.

```csharp
using System;
using Microsoft.Azure.Functions.Extensions.DependencyInjection;
using Microsoft.Extensions.DependencyInjection;
using Microsoft.Extensions.Http;
using Microsoft.Extensions.Logging;
using Microsoft.Azure.Cosmos;

[assembly: FunctionsStartup(typeof(MyNamespace.Startup))]

namespace MyNamespace
{
    public class Startup : FunctionsStartup
    {
        public override void Configure(IFunctionsHostBuilder builder)
        {
            builder.Services.AddHttpClient();
            builder.Services.AddSingleton((s) => {
                return new CosmosClient(Environment.GetEnvironmentVariable("COSMOSDB_CONNECTIONSTRING"));
            });
            builder.Services.AddSingleton<ILoggerProvider, MyLoggerProvider>();
        }
    }
}
```

## <a name="use-injected-dependencies"></a>Verwenden von eingefügten Abhängigkeiten

Für ASP.NET Core wird die Konstruktorinjektion verwendet, um Ihre Abhängigkeiten für Ihre Funktion verfügbar zu machen. Im folgenden Beispiel wird veranschaulicht, wie die Abhängigkeiten `IMyService` und `HttpClient` in eine per HTTP ausgelöste Funktion eingefügt werden.

```csharp
using System;
using System.IO;
using System.Threading.Tasks;
using Microsoft.AspNetCore.Mvc;
using Microsoft.Azure.WebJobs;
using Microsoft.Azure.WebJobs.Extensions.Http;
using Microsoft.AspNetCore.Http;
using Microsoft.Extensions.Logging;
using Newtonsoft.Json;

namespace MyNamespace
{
    public class HttpTrigger
    {
        private readonly IMyService _service;
        private readonly HttpClient _client;

        public HttpTrigger(IMyService service, IHttpClientFactory httpClientFactory)
        {
            _service = service;
            _client = httpClientFactory.CreateClient();;
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

Die Verwendung der Konstruktorinjektion bedeutet, dass Sie nicht die statischen Funktionen nutzen sollten, wenn Sie die Abhängigkeitsinjektion einsetzen möchten.

## <a name="service-lifetimes"></a>Dienstlebensdauer

Azure Functions-Apps verfügen über dieselbe Dienstlebensdauer wie die [ASP.NET-Abhängigkeitsinjektion](https://docs.microsoft.com/aspnet/core/fundamentals/dependency-injection#service-lifetimes): Sie sind kurzlebig, bereichsbezogen und „Singleton“.

In einer Funktions-App entspricht eine bereichsbezogene Lebensdauer der Ausführungslebensdauer einer Funktion. Bereichsbezogene Dienste werden einmal pro Ausführung erstellt. In späteren Anforderungen für diesen Dienst während der Ausführung wird die vorhandene Dienstinstanz wiederverwendet. Die Lebensdauer eines Singletondiensts entspricht der Lebensdauer des Hosts und wird für Funktionsausführungen dieser Instanz wiederverwendet.

Dienste mit Singleton-Lebensdauer werden für Verbindungen und Clients empfohlen, z. B. Instanzen von `SqlConnection`, `CloudBlobClient` oder `HttpClient`.

Sie können auf GitHub ein [Beispiel für verschiedene Dienstlebensdauern](https://aka.ms/functions/di-sample) anzeigen bzw. herunterladen.

## <a name="logging-services"></a>Protokollierungsdienste

Wenn Sie Ihren eigenen Protokollanbieter benötigen, ist die empfohlene Vorgehensweise die Registrierung einer `ILoggerProvider`-Instanz. Application Insights wird von Azure Functions automatisch hinzugefügt.

> [!WARNING]
> Fügen Sie `AddApplicationInsightsTelemetry()` nicht der Dienstsammlung hinzu, da sonst Dienste registriert werden, für die Konflikte mit den von der Umgebung bereitgestellten Diensten auftreten können.

## <a name="function-app-provided-services"></a>Dienste, die von Funktions-App bereitgestellt werden

Der Funktionshost registriert viele Dienste. Es ist sicher, die folgenden Dienste als Abhängigkeit in Ihrer Anwendung zu verwenden:

|Service Type|Gültigkeitsdauer|BESCHREIBUNG|
|--|--|--|
|`Microsoft.Extensions.Configuration.IConfiguration`|Singleton|Laufzeitkonfiguration|
|`Microsoft.Azure.WebJobs.Host.Executors.IHostIdProvider`|Singleton|Verantwortlich für die Bereitstellung der Hostinstanz-ID|

[Erstellen Sie ein Issue, und schlagen Sie es auf GitHub vor](https://github.com/azure/azure-functions-host), wenn Sie eine Abhängigkeit von anderen Diensten erstellen möchten.

### <a name="overriding-host-services"></a>Überschreiben von Hostdiensten

Das Überschreiben der vom Host bereitgestellten Dienste wird derzeit nicht unterstützt.  [Erstellen Sie ein Issue, und schlagen Sie es auf GitHub vor](https://github.com/azure/azure-functions-host), falls andere Dienste vorhanden sind, die Sie überschreiben möchten.

## <a name="next-steps"></a>Nächste Schritte

Weitere Informationen finden Sie in den folgenden Ressourcen:

- [Überwachen Ihrer Funktions-App](functions-monitoring.md)
- [Bewährte Methoden für Funktionen](functions-best-practices.md)
