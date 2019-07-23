---
title: Registrieren von Bindungserweiterungen von Azure Functions
description: Lernen Sie, wie Sie eine Azure Functions-Bindungserweiterung abhängig von Ihrer Umgebung registrieren.
services: functions
documentationcenter: na
author: craigshoemaker
manager: gwallace
ms.service: azure-functions
ms.devlang: multiple
ms.topic: reference
ms.date: 07/08/2019
ms.author: cshoe
ms.openlocfilehash: 5969c3e0d270b45347f8132b2d655ba2e56cb2c0
ms.sourcegitcommit: c0419208061b2b5579f6e16f78d9d45513bb7bbc
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 07/08/2019
ms.locfileid: "67625891"
---
# <a name="register-azure-functions-binding-extensions"></a>Registrieren von Bindungserweiterungen von Azure Functions

In Azure Functions Version 2.x sind [Bindungen](./functions-triggers-bindings.md) als separate Pakete aus der Functions-Runtime verfügbar. Während .NET-Funktionen über NuGet-Pakete auf Bindungen zugreifen, ermöglichen Erweiterungsbundles anderen Funktionen den Zugriff auf alle Bindungen über eine Konfigurationseinstellung.

Beachten Sie die folgenden Punkte im Zusammenhang mit Bindungserweiterungen:

- Bindungserweiterungen werden in Functions 1.x nicht explizit registriert, mit Ausnahme der [Erstellung einer C#-Klassenbibliothek mithilfe von Visual Studio](#local-csharp).

- HTTP und Zeitgebertrigger werden standardmäßig unterstützt und erfordern keine Erweiterung.

In der folgenden Tabelle ist angegeben, wann und wie Sie Bindungen registrieren.

| Entwicklungsumgebung |Registrierung<br/> in Functions 1.x  |Registrierung<br/> in Functions 2.x  |
|-------------------------|------------------------------------|------------------------------------|
|Azure-Portal|Automatisch|Automatisch|
|Nicht zu .NET gehörende Sprachen oder lokale Azure Core Tools-Entwicklung|Automatisch|[Verwenden von Azure Functions Core Tools und Erweiterungsbundles](#extension-bundles)|
|C#-Klassenbibliothek mit Visual Studio 2019|[Verwendung von NuGet-Tools](#vs)|[Verwendung von NuGet-Tools](#vs)|
|C#-Klassenbibliothek mit Visual Studio Code|–|[Verwendung der .NET Core-CLI](#vs-code)|

## <a name="extension-bundles"></a>Erweiterungsbündel für die lokale Entwicklung

Erweiterungsbündel stellen eine Technologie für die lokale Entwicklung für Runtimeversion 2.x dar, mit der Sie Ihrem Funktions-App-Projekt einen kompatiblen Satz mit Functions-Bindungserweiterungen hinzufügen können. Diese Erweiterungspakete werden bei der Bereitstellung in Azure dann in das Bereitstellungspaket eingefügt. Bei den Bündeln werden alle Bindungen, die von Microsoft veröffentlicht werden, über eine Einstellung in der Datei *host.json* verfügbar gemacht. Da in einem Bündel definierte Erweiterungspakete miteinander kompatibel sind, ist dies für die Vermeidung von Konflikten zwischen Paketen hilfreich. Stellen Sie beim lokalen Entwickeln sicher, dass Sie die aktuelle Version der [Azure Functions Core Tools](functions-run-local.md#v2) verwenden.

Nutzen Sie Erweiterungsbündel für die gesamte lokale Entwicklung mit Azure Functions Core Tools oder Visual Studio Code.

Wenn Sie keine Erweiterungsbündel verwenden, müssen Sie das .NET Core 2.x SDK auf Ihrem lokalen Computer installieren, bevor Sie Bindungserweiterungen installieren. Mit Bündeln wird diese Anforderung für die lokale Entwicklung beseitigt. 

Um die Erweiterungsbündel zu verwenden, aktualisieren Sie die Datei *host.json* so, dass sie den folgenden Eintrag für `extensionBundle` enthält:

```json
{
    "version": "2.0",
    "extensionBundle": {
        "id": "Microsoft.Azure.Functions.ExtensionBundle",
        "version": "[1.*, 2.0.0)"
    }
}
```

In `extensionBundle` sind die folgenden Eigenschaften verfügbar:

| Eigenschaft | BESCHREIBUNG |
| -------- | ----------- |
| **`id`** | Der Namespace für Microsoft Azure Functions-Erweiterungsbündel. |
| **`version`** | Die Version des zu installierenden Pakets. Die Functions Runtime wählt immer die zulässige Höchstversion aus, die durch den Versionsbereich oder das Intervall definiert ist. Mit dem obigen Versionswert sind alle Bündelversionen von 1.0.0 bis kleiner als 2.0.0 zulässig. Weitere Informationen finden Sie im Abschnitt zur [Intervallnotation zum Angeben von Versionsbereichen](https://docs.microsoft.com/nuget/reference/package-versioning#version-ranges-and-wildcards). |

Bündelversionen werden erhöht, wenn sich Pakete im Bündel ändern. Zu größeren Versionsänderungen kommt es, wenn Pakete im Bündel um einen Hauptversionswert inkrementiert werden. Dies trifft in der Regel mit einer Änderung der Hauptversion der Functions-Runtime zusammen.  

Der aktuelle Erweiterungssatz, der vom Standardbündel installiert wird, ist in der [Datei „extensions.json“](https://github.com/Azure/azure-functions-extension-bundles/blob/master/src/Microsoft.Azure.Functions.ExtensionBundle/extensions.json) aufgelistet.

<a name="local-csharp"></a>

## <a name="vs"></a>C\#-Klassenbibliothek mit Visual Studio

In **Visual Studio** können Sie Pakete mithilfe des Befehls [Install-Package](https://docs.microsoft.com/nuget/tools/ps-ref-install-package) aus der Paket-Manager-Konsole installieren, wie im folgenden Beispiel gezeigt wird:

```powershell
Install-Package Microsoft.Azure.WebJobs.Extensions.ServiceBus -Version <TARGET_VERSION>
```

Der Name des Pakets, das für eine bestimmte Bindung verwendet wird, ist im Referenzartikel für diese Bindung angegeben. Ein Beispiel finden Sie im [Referenzartikel zur Service Bus-Bindung im Abschnitt „Pakete“](functions-bindings-service-bus.md#packages---functions-1x).

Ersetzen Sie `<TARGET_VERSION>` im Beispiel durch eine bestimmte Version des Pakets, z.B. `3.0.0-beta5`. Gültige Versionen sind auf den Seiten der einzelnen Pakete auf [NuGet.org](https://nuget.org) aufgeführt. Die Hauptversionen, die Functions Runtime 1.x oder 2.x entsprechen, sind im Referenzartikel für die Bindung angegeben.

Wenn Sie `Install-Package` verwenden, um auf eine Bindung zu verweisen, müssen Sie keine [Erweiterungsbündel](#extension-bundles) verwenden. Diese Vorgehensweise ist spezifisch für Klassenbibliotheken, die in Visual Studio erstellt werden.

## <a name="vs-code"></a> C#-Klassenbibliothek mit Visual Studio Code

> [!NOTE]
> Wir empfehlen Ihnen die Verwendung von [Erweiterungsbündeln](#extension-bundles), damit von Functions automatisch ein kompatibler Satz mit Bindungserweiterungspaketen installiert wird.

Installieren Sie in **Visual Studio Code** Pakete für eine C#-Klassenbibliothek über die Eingabeaufforderung. Verwenden Sie hierfür den Befehl [dotnet add package](https://docs.microsoft.com/dotnet/core/tools/dotnet-add-package) in der .NET Core-CLI. Im folgenden Beispiel wird veranschaulicht, wie Sie eine Bindung hinzufügen:

```terminal
dotnet add package Microsoft.Azure.WebJobs.Extensions.<BINDING_TYPE_NAME> --version <TARGET_VERSION>
```

Die .NET Core-CLI kann nur für die Entwicklung in Azure Functions 2.x verwendet werden.

Ersetzen Sie `<BINDING_TYPE_NAME>` durch den Namen des Pakets, das im Referenzartikel für die gewünschte Bindung bereitgestellt wird. Den gewünschten Bindungsreferenzartikel finden Sie in der [Liste der unterstützten Bindungen](./functions-triggers-bindings.md#supported-bindings).

Ersetzen Sie `<TARGET_VERSION>` im Beispiel durch eine bestimmte Version des Pakets, z.B. `3.0.0-beta5`. Gültige Versionen sind auf den Seiten der einzelnen Pakete auf [NuGet.org](https://nuget.org) aufgeführt. Die Hauptversionen, die Functions Runtime 1.x oder 2.x entsprechen, sind im Referenzartikel für die Bindung angegeben.

## <a name="next-steps"></a>Nächste Schritte
> [!div class="nextstepaction"]
> [Trigger- und Bindungsbeispiel für Azure Functions](./functions-bindings-example.md)
