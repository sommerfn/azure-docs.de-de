---
title: Telemetriekanäle in Azure Application Insights | Microsoft-Dokumentation
description: Wie man Telemetriekanäle in Azure Application Insights SDKs für .NET und.NET Core anpasst.
services: application-insights
documentationcenter: .net
author: cijothomas
manager: carmonm
ms.assetid: 015ab744-d514-42c0-8553-8410eef00368
ms.service: application-insights
ms.workload: tbd
ms.tgt_pltfrm: ibiza
ms.topic: conceptual
ms.date: 05/14/2019
ms.reviewer: mbullwin
ms.author: cithomas
ms.openlocfilehash: af00641123354831c7bf174a743ded2886343579
ms.sourcegitcommit: d3b1f89edceb9bff1870f562bc2c2fd52636fc21
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 07/04/2019
ms.locfileid: "67561349"
---
# <a name="telemetry-channels-in-application-insights"></a>Telemetriekanäle in Application Insights

Telemetriekanäle sind ein integraler Bestandteil der [Azure Application Insights SDKs](../../azure-monitor/app/app-insights-overview.md). Sie verwalten die Pufferung und Übertragung der Telemetrie an den Application Insights-Dienst. Die.NET- und.NET-Core-Versionen der SDKs verfügen über zwei integrierte Telemetriekanäle: `InMemoryChannel` und `ServerTelemetryChannel` . Dieser Artikel beschreibt jeden Kanal im Detail, einschließlich der Anpassung des Kanalverhaltens.

## <a name="what-are-telemetry-channels"></a>Was sind Telemetrie-Kanäle?

Telemetriekanäle sind dafür verantwortlich, Telemetrieelemente zu puffern und an den Application Insights-Dienst zu senden, wo sie zur Abfrage und Analyse gespeichert werden. Ein Telemetriekanal ist jede Klasse, die das [`Microsoft.ApplicationInsights.ITelemetryChannel`](https://docs.microsoft.com/dotnet/api/microsoft.applicationinsights.channel.itelemetrychannel?view=azure-dotnet) Interface implementiert.

Das `Send(ITelemetry item)` Verfahren eines Telemetriekanals wird aufgerufen, nachdem alle Telemetrie-Initialisierer und Telemetrieprozessoren aufgerufen wurden. So erreichen alle Gegenstände, die von einem Telemetrieprozessor fallen gelassen werden, den Kanal nicht. `Send()` sendet die Elemente normalerweise nicht sofort an das Backend. Typischerweise puffert es sie im Speicher und sendet sie in Stapeln, für eine effiziente Übertragung.

[Live Metrics Stream](live-stream.md) verfügt auch über einen benutzerdefinierten Kanal, der das Live-Streaming der Telemetrie unterstützt. Dieser Kanal ist unabhängig vom normalen Telemetriekanal, und dieses Dokument gilt nicht für ihn.

## <a name="built-in-telemetry-channels"></a>Eingebaute Telemetriekanäle

Die Application Insights.NET und.NET Core SDKs werden mit zwei integrierten Kanälen ausgeliefert:

* `InMemoryChannel`: Ein einfacher Kanal, der Elemente im Speicher zwischenspeichert, bis sie gesendet werden. Die Elemente werden im Speicher gepuffert und einmal alle 30 Sekunden oder immer dann, wenn 500 Elemente gepuffert werden, geleert. Dieser Kanal bietet minimale Zuverlässigkeitsgarantien, da er nach einem Ausfall nicht erneut versucht, Telemetrie zu senden. Dieser Kanal speichert auch keine Elemente auf der Festplatte, so dass nicht gesendete Elemente beim Herunterfahren der Anwendung dauerhaft verloren gehen (ordnungsgemäß oder nicht). Dieser Kanal implementiert ein `Flush()`-Verfahren, mit dem alle Telemetrieelemente im Speicher synchron erzwungen werden können. Dieser Kanal ist gut geeignet für kurz laufende Anwendungen, bei denen eine synchrone Leerung ideal ist.

    Dieser Kanal ist Teil des größeren Microsoft.ApplicationInsights NuGet-Pakets und ist der Standardkanal, den das SDK verwendet, wenn nichts anderes konfiguriert ist.

* `ServerTelemetryChannel`: Ein fortschrittlicherer Kanal mit Wiederholungsrichtlinien und der Möglichkeit, Daten auf einer lokalen Festplatte zu speichern. Dieser Kanal versucht erneut, Telemetrie zu senden, wenn transiente Fehler auftreten. Dieser Kanal verwendet auch lokalen Datenträgerspeicher, um Elemente während Netzwerkausfällen oder bei großen Telemetriedatenmengen auf dem Datenträger aufzubewahren. Aufgrund dieser Wiederholungsmechanismen und des lokalen Festplattenspeichers gilt dieser Kanal als zuverlässiger und wird für alle Produktionsszenarien empfohlen. Dieser Kanal ist der Standard für [ASP.NET](https://docs.microsoft.com/azure/azure-monitor/app/asp-net)- und [ASP.NET-Core](https://docs.microsoft.com/azure/azure-monitor/app/asp-net-core)-Anwendungen, die gemäß der offiziellen Dokumentation konfiguriert sind. Dieser Kanal ist für Serverszenarien mit lang laufenden Prozessen optimiert. Die von diesem Kanal implementierte [`Flush()`](#which-channel-should-i-use)-Methode ist nicht synchron.

    Dieser Kanal wird als Microsoft.ApplicationInsights.WindowsServer.TelemetryChannel NuGet-Paket ausgeliefert und wird automatisch erworben, wenn Sie entweder das Microsoft.ApplicationInsights.Web oder Microsoft.ApplicationInsights.AspNetCore NuGet-Paket verwenden.

## <a name="configure-a-telemetry-channel"></a>Konfigurieren eines Telemetriekanals

Sie konfigurieren einen Telemetriekanal, indem Sie ihn auf die aktive Telemetriekonfiguration einstellen. Für ASP.NET-Anwendungen beinhaltet die Konfiguration das Setzen der Telemetriekanalinstanz auf `TelemetryConfiguration.Active`, oder durch Ändern von `ApplicationInsights.config`. Für ASP.NET Core-Anwendungen beinhaltet die Konfiguration das Hinzufügen des Kanals zum Dependency Injection Container.

Die folgenden Abschnitte zeigen Beispiele für die Konfiguration der `StorageFolder` Einstellung des Kanals in verschiedenen Anwendungsarten. `StorageFolder` ist nur eine der konfigurierbaren Einstellungen. Die vollständige Liste der Konfigurationseinstellungen finden Sie im Abschnitt [Einstellungen](telemetry-channels.md#configurable-settings-in-channels) weiter unten in diesem Artikel.

### <a name="configuration-by-using-applicationinsightsconfig-for-aspnet-applications"></a>Konfiguration mit ApplicationInsights.config für ASP.NET-Anwendungen

Der folgende Abschnitt von [ApplicationInsights.config](configuration-with-applicationinsights-config.md) zeigt den `ServerTelemetryChannel`Kanal, der mit `StorageFolder` einem benutzerdefinierten Speicherort konfiguriert wurde:

```xml
    <TelemetrySinks>
        <Add Name="default">
            <TelemetryProcessors>
                <!-- Telemetry processors omitted for brevity  -->
            </TelemetryProcessors>
            <TelemetryChannel Type="Microsoft.ApplicationInsights.WindowsServer.TelemetryChannel.ServerTelemetryChannel, Microsoft.AI.ServerTelemetryChannel">
                <StorageFolder>d:\temp\applicationinsights</StorageFolder>
            </TelemetryChannel>
        </Add>
    </TelemetrySinks>
```

### <a name="configuration-in-code-for-aspnet-applications"></a>Konfiguration im Code für ASP.NET-Anwendungen

Der folgende Code richtet eine 'ServerTelemetryChannel'-Instanz mit `StorageFolder` ein, die auf einen benutzerdefinierten Speicherort gesetzt wird. Fügen Sie diesen Code am Anfang der Anwendung hinzu, typischerweise in der `Application_Start()`-Methode in Global.aspx.cs.

```csharp
using Microsoft.ApplicationInsights.Extensibility;
using Microsoft.ApplicationInsights.WindowsServer.TelemetryChannel;
protected void Application_Start()
{
    var serverTelemetryChannel = new ServerTelemetryChannel();
    serverTelemetryChannel.StorageFolder = @"d:\temp\applicationinsights";
    serverTelemetryChannel.Initialize(TelemetryConfiguration.Active);
    TelemetryConfiguration.Active.TelemetryChannel = serverTelemetryChannel;
}
```

### <a name="configuration-in-code-for-aspnet-core-applications"></a>Konfiguration im Code für ASP.NET Core-Anwendungen

Ändern Sie die `ConfigureServices`-Methode der `Startup.cs`-Klasse wie hier gezeigt:

```csharp
using Microsoft.ApplicationInsights.Channel;
using Microsoft.ApplicationInsights.WindowsServer.TelemetryChannel;

public void ConfigureServices(IServiceCollection services)
{
    // This sets up ServerTelemetryChannel with StorageFolder set to a custom location.
    services.AddSingleton(typeof(ITelemetryChannel), new ServerTelemetryChannel() {StorageFolder = @"d:\temp\applicationinsights" });

    services.AddApplicationInsightsTelemetry();
}

```

> [!IMPORTANT]
> Die Konfiguration des Kanals mit Hilfe von `TelemetryConfiguration.Active` ASP.NET Core-Anwendungen wird nicht empfohlen.

### <a name="configuration-in-code-for-netnet-core-console-applications"></a>Konfiguration im Code für .NET/.NET Core Konsolenanwendungen

Bei Konsolenanwendungen ist der Code für .NET und.NET Core identisch:

```csharp
var serverTelemetryChannel = new ServerTelemetryChannel();
serverTelemetryChannel.StorageFolder = @"d:\temp\applicationinsights";
serverTelemetryChannel.Initialize(TelemetryConfiguration.Active);
TelemetryConfiguration.Active.TelemetryChannel = serverTelemetryChannel;
```

## <a name="operational-details-of-servertelemetrychannel"></a>Funktionsdetails zu ServerTelemetryChannel

`ServerTelemetryChannel` speichert eingehende Artikel in einem In-Memory-Puffer. Die Elemente werden serialisiert, komprimiert und einmal alle 30 Sekunden oder wenn 500 Elemente gepuffert wurden, in einer `Transmission`-Instanz gespeichert. Eine einzelne `Transmission`-Instanz enthält bis zu 500 Elemente und stellt einen Stapel Telemetrie dar, der über einen einzigen HTTPS-Aufruf an den Application Insights-Dienst gesendet wird.

Standardmäßig können maximal 10 `Transmission`-Instanzen parallel gesendet werden. Wenn die Telemetrie schneller eintrifft, oder wenn das Netzwerk oder das Application Insights Backend langsam ist, werden `Transmission`-Instanzen im Speicher gespeichert. Die Standardkapazität dieses In-Memory-`Transmission`-Puffers beträgt 5 MB. Wenn die In-Memory-Kapazität überschritten ist, werden `Transmission`-Instanzen auf der lokalen Festplatte bis zu einem Maximum von 50 MB gespeichert. `Transmission`-Instanzen werden auf der lokalen Festplatte gespeichert, auch wenn es Netzwerkprobleme gibt. Nur die Elemente, die auf einer lokalen Festplatte gespeichert sind, überleben einen Anwendungsabsturz. Sie werden gesendet, wenn die Anwendung erneut beginnt.

## <a name="configurable-settings-in-channels"></a>Konfigurierbare Einstellungen in den Kanälen

Für die vollständige Liste der konfigurierbaren Einstellungen für jeden Kanal siehe:

* [InMemoryChannel](https://github.com/microsoft/ApplicationInsights-dotnet/blob/develop/src/Microsoft.ApplicationInsights/Channel/InMemoryChannel.cs)

* [ServerTelemetryChannel](https://github.com/microsoft/ApplicationInsights-dotnet/blob/develop/src/ServerTelemetryChannel/ServerTelemetryChannel.cs)

Hier sind die am häufigsten verwendeten Einstellungen für `ServerTelemetryChannel` :

1. `MaxTransmissionBufferCapacity`: Die maximale Menge an Speicher, in Bytes, die der Kanal verwendet, um Übertragungen im Speicher zu puffern. Wenn diese Kapazität erreicht ist, werden neue Elemente direkt auf der lokalen Festplatte gespeichert. Der Standardwert ist 5 MB. Das Setzen eines höheren Wertes führt zu weniger Festplattennutzung, aber denken Sie daran, dass Elemente im Speicher verloren gehen, wenn die Anwendung abstürzt.

1. `MaxTransmissionSenderCapacity`: Die maximale Anzahl von `Transmission`-Instanzen, die gleichzeitig an Application Insights gesendet werden. Der Standardwert ist 10. Diese Einstellung kann auf eine höhere Anzahl konfiguriert werden, was empfohlen wird, wenn ein großes Volumen an Telemetrie erzeugt wird. Hohe Lautstärken treten typischerweise während der Lastprüfung oder bei ausgeschalteter Probenahme auf.

1. `StorageFolder`: Der Ordner, der vom Kanal verwendet wird, um Elemente bei Bedarf auf der Festplatte zu speichern. Unter Windows wird entweder %LOCALAPPDATA% oder %TEMP% verwendet, wenn kein anderer Pfad explizit angegeben wird. In anderen Umgebungen als Windows müssen Sie einen gültigen Standort angeben, da sonst die Telemetrie nicht auf der lokalen Festplatte gespeichert wird.

## <a name="which-channel-should-i-use"></a>Welchen Kanal sollte ich verwenden?

`ServerTelemetryChannel` wird für die meisten Produktionsszenarien mit lang laufenden Anwendungen empfohlen. Die `Flush()`-Methode, die von `ServerTelemetryChannel` implementierte wird, ist nicht synchron und garantiert auch nicht das Senden aller ausstehenden Elemente vom Speicher oder von der Festplatte. Wenn Sie diesen Kanal in Szenarien verwenden, in denen die Anwendung kurz vor dem Herunterfahren steht, empfehlen wir Ihnen, nach dem Aufruf von `Flush()` eine Verzögerung einzuleiten. Die genaue Höhe der Verzögerung, die Sie benötigen könnten, ist nicht vorhersehbar. Es hängt von Faktoren ab, wie z.B. wie viele Elemente oder `Transmission`-Instanzen sich im Speicher befinden, wie viele auf der Festplatte sind, wie viele an das Backend übertragen werden und ob sich der Kanal in der Mitte von exponentiellen Backoff-Szenarien befindet.

Wenn Sie ein synchrone Leerung durchführen müssen, empfehlen wir Ihnen `InMemoryChannel` zu nutzen.

## <a name="frequently-asked-questions"></a>Häufig gestellte Fragen

### <a name="does-the-application-insights-channel-guarantee-telemetry-delivery-if-not-what-are-the-scenarios-in-which-telemetry-can-be-lost"></a>Garantiert der Kanal Application Insights die Bereitstellung von Telemetrie? Wenn nicht, in welchen Szenarien kann die Telemetrie verloren gehen?

Die kurze Antwort ist, dass keiner der eingebauten Kanäle eine transaktionsartige Garantie für die Telemetrieübermittlung an das Backend bietet. `ServerTelemetryChannel` ist im Vergleich zu `InMemoryChannel` für verlässliche Lieferungen fortschrittlicher, führt aber auch nur den bestmöglichen Versuch durch, Telemetrie zu senden. Die Telemetrie kann in mehreren Situationen, einschließlich dieser häufigen Szenarien, immer noch verloren gehen:

1. Elemente im Speicher gehen verloren, wenn die Anwendung abstürzt.

1. Die Telemetrie geht bei längeren Netzwerkproblemen verloren. Die Telemetrie wird während eines Netzwerkausfalls oder bei Problemen mit dem Application Insights Backend auf der lokalen Festplatte gespeichert. Allerdings werden Elemente verworfen, die älter als 24 Stunden sind.

1. Die Standard-Plattenspeicherorte für die Speicherung von Telemetrie unter Windows sind %LOCALAPPDATA% oder %TEMP%. Diese Speicherorte befinden sich normalerweise lokal auf dem Computer. Wenn die Anwendung physisch von einem Standort zum anderen migriert, geht die am ursprünglichen Standort gespeicherte Telemetrie verloren.

1. In Web Apps unter Windows ist der Standard-Laufwerksspeicherort D:\local\LocalAppData. Dieser Ort bleibt nicht erhalten. Es wird bei Neustarts von Apps, Scale-Outs und anderen solchen Vorgängen ausgelöscht, was zum Verlust der dort gespeicherten Telemetrie führt. Sie können die Standardeinstellung überschreiben und den Speicherort an einem persistenten Ort wie D:\home angeben. Solche persistenten Standorte werden jedoch durch Remote-Speicher bedient und können daher langsam sein.

### <a name="does-servertelemetrychannel-work-on-systems-other-than-windows"></a>Funktioniert ServerTelemetryChannel auch auf anderen Systemen als Windows?

Obwohl der Name des Pakets und des Namensraums "WindowsServer" beinhaltet, wird dieser Kanal auf anderen Systemen als Windows unterstützt, mit der folgenden Ausnahme. Auf anderen Systemen als Windows erstellt der Kanal standardmäßig keinen lokalen Speicherordner. Sie müssen einen lokalen Speicherordner erstellen und den Kanal so konfigurieren, dass er ihn verwendet. Nachdem die lokale Speicherung konfiguriert wurde, funktioniert der Kanal auf allen Systemen gleich.

### <a name="does-the-sdk-create-temporary-local-storage-is-the-data-encrypted-at-storage"></a>Erstellt das SDK einen temporären lokalen Speicher? Sind die Daten beim Speichern verschlüsselt?

Das SDK speichert Telemetrieelemente im lokalen Speicher bei Netzwerkproblemen oder während der Drosselung. Diese Daten werden nicht lokal verschlüsselt.

Für Windows-Systeme erstellt das SDK automatisch einen temporären lokalen Ordner im Verzeichnis %TEMP% oder %LOCALAPPDATA% und beschränkt den Zugriff auf Administratoren und den aktuellen Benutzer.

Für andere Systeme als Windows wird vom SDK kein lokaler Speicher automatisch erstellt, so dass standardmäßig keine Daten lokal gespeichert werden. Sie können selbst ein Speicherverzeichnis erstellen und den Kanal so konfigurieren, dass er es verwendet. In diesem Fall sind Sie dafür verantwortlich, dass das Verzeichnis gesichert ist.
Lesen Sie weitere Informationen zum [Datenschutz](data-retention-privacy.md#does-the-sdk-create-temporary-local-storage).

## <a name="open-source-sdk"></a>Open Source SDK
Wie jedes SDK für Application Insights sind Kanäle Open Source. Lesen und tragen Sie zum Code bei oder melden Sie Probleme im [offiziellen GitHub Repo](https://github.com/Microsoft/ApplicationInsights-dotnet).

## <a name="next-steps"></a>Nächste Schritte

* [Stichproben](../../azure-monitor/app/sampling.md)
* [SDK-Problembehandlung](../../azure-monitor/app/asp-net-troubleshoot-no-data.md)
