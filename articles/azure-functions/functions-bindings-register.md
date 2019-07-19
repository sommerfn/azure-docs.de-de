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
ms.date: 02/25/2019
ms.author: cshoe
ms.openlocfilehash: 88ffd6ec24ed19dd3b1e57277884c8759cdac1f9
ms.sourcegitcommit: 9b80d1e560b02f74d2237489fa1c6eb7eca5ee10
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 07/01/2019
ms.locfileid: "67480331"
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
|C#-Klassenbibliothek mit Visual Studio 2019|[Verwendung von NuGet-Tools](#c-class-library-with-visual-studio-2019)|[Verwendung von NuGet-Tools](#c-class-library-with-visual-studio-2019)|
|C#-Klassenbibliothek mit Visual Studio Code|–|[Verwendung der .NET Core-CLI](#c-class-library-with-visual-studio-code)|

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

## <a name="c-class-library-with-visual-studio-2019"></a>C\#-Klassenbibliothek mit Visual Studio 2019

In **Visual Studio 2019** können Sie Pakete mithilfe des Befehls [Install-Package](https://docs.microsoft.com/nuget/tools/ps-ref-install-package) aus der Paket-Manager-Konsole installieren, wie es im folgenden Beispiel gezeigt wird:

```powershell
Install-Package Microsoft.Azure.WebJobs.Extensions.ServiceBus -Version <TARGET_VERSION>
```

Der Name des Pakets, das für eine bestimmte Bindung verwendet wird, ist im Referenzartikel für diese Bindung angegeben. Ein Beispiel finden Sie im [Referenzartikel zur Service Bus-Bindung im Abschnitt „Pakete“](functions-bindings-service-bus.md#packages---functions-1x).

Ersetzen Sie `<TARGET_VERSION>` im Beispiel durch eine bestimmte Version des Pakets, z.B. `3.0.0-beta5`. Gültige Versionen sind auf den Seiten der einzelnen Pakete auf [NuGet.org](https://nuget.org) aufgeführt. Die Hauptversionen, die Functions Runtime 1.x oder 2.x entsprechen, sind im Referenzartikel für die Bindung angegeben.

## <a name="c-class-library-with-visual-studio-code"></a>C#-Klassenbibliothek mit Visual Studio Code

> [!NOTE]
> Wir empfehlen Ihnen die Verwendung von [Erweiterungsbündeln](#extension-bundles), damit von Functions automatisch ein kompatibler Satz mit Bindungserweiterungspaketen installiert wird.

Installieren Sie in **Visual Studio Code** Pakete für eine C#-Klassenbibliothek über die Eingabeaufforderung. Verwenden Sie hierfür den Befehl [dotnet add package](https://docs.microsoft.com/dotnet/core/tools/dotnet-add-package) in der .NET Core-CLI, wie im folgenden Beispiel gezeigt:

```terminal
dotnet add package Microsoft.Azure.WebJobs.Extensions.ServiceBus --version <TARGET_VERSION>
```

Die .NET Core-CLI kann nur für die Entwicklung in Azure Functions 2.x verwendet werden.

Der Name des Pakets, das für eine bestimmte Bindung verwendet werden muss, ist im Referenzartikel für diese Bindung angegeben. Ein Beispiel finden Sie im [Referenzartikel zur Service Bus-Bindung im Abschnitt „Pakete“](functions-bindings-service-bus.md#packages---functions-1x).

Ersetzen Sie `<TARGET_VERSION>` im Beispiel durch eine bestimmte Version des Pakets, z.B. `3.0.0-beta5`. Gültige Versionen sind auf den Seiten der einzelnen Pakete auf [NuGet.org](https://nuget.org) aufgeführt. Die Hauptversionen, die Functions Runtime 1.x oder 2.x entsprechen, sind im Referenzartikel für die Bindung angegeben.

## <a name="next-steps"></a>Nächste Schritte
> [!div class="nextstepaction"]
> [Trigger- und Bindungsbeispiel für Azure Functions](./functions-bindings-example.md)

