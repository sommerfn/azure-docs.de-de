---
title: Einrichten von Azure-Funktionen für benutzerdefinierte Azure-Anbieter
description: In diesem Tutorial wird beschrieben, wie Sie eine Azure-Funktion erstellen und für benutzerdefinierte Azure-Anbieter einrichten.
author: jjbfour
ms.service: managed-applications
ms.topic: tutorial
ms.date: 06/19/2019
ms.author: jobreen
ms.openlocfilehash: d7e4de43659db88bfd9aad40cc3b9f1753189bba
ms.sourcegitcommit: 66237bcd9b08359a6cce8d671f846b0c93ee6a82
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 07/11/2019
ms.locfileid: "67799119"
---
# <a name="setup-azure-functions-for-azure-custom-providers"></a>Einrichten von Azure-Funktionen für benutzerdefinierte Azure-Anbieter

Benutzerdefinierte Anbieter ermöglichen Ihnen die Anpassung von Workflows in Azure. Ein benutzerdefinierter Anbieter ist ein Vertrag zwischen Azure und einem Endpunkt (`endpoint`). In diesem Tutorial wird der Prozess zum Einrichten einer Azure-Funktion für den Endpunkt (`endpoint`) eines benutzerdefinierten Anbieters beschrieben.

Dieses Tutorial ist in die folgenden Schritte unterteilt:

- Erstellen der Azure-Funktion
- Installieren von Azure-Tabellenbindungen
- Aktualisieren von RESTful-HTTP-Methoden
- Hinzufügen von Azure Resource Manager-NuGet-Paketen

Dieses Tutorial baut auf den folgenden Tutorials auf:

- [Erstellen Ihrer ersten Funktion im Azure-Portal](../azure-functions/functions-create-first-azure-function.md)

## <a name="creating-the-azure-function"></a>Erstellen der Azure-Funktion

> [!NOTE]
> In diesem Tutorial erstellen wir einen einfachen Dienstendpunkt, indem wir eine Azure-Funktion verwenden. Für einen benutzerdefinierten Anbieter kann aber ein beliebiger Endpunkt (`endpoint`) verwendet werden, der öffentlich zugänglich ist. Azure Logic Apps, Azure API Management und Azure-Web-Apps sind einige hervorragende Alternativen.

Als Einstieg in dieses Tutorial sollten Sie das Tutorial [Erstellen Ihrer ersten Funktion im Azure-Portal](../azure-functions/functions-create-first-azure-function.md) durcharbeiten. In diesem Tutorial wird eine .NET Core-Webhookfunktion erstellt, die im Azure-Portal geändert werden kann.

## <a name="install-azure-table-bindings"></a>Installieren von Azure-Tabellenbindungen

In diesem Abschnitt werden die Schritte zum schnellen Installieren der Azure-Tabellenspeicherbindungen beschrieben.

1. Navigieren Sie zur Registerkarte `Integrate` für „HttpTrigger“.
2. Klicken Sie auf `+ New Input`.
3. Wählen Sie `Azure Table Storage` aus.
4. Installieren Sie `Microsoft.Azure.WebJobs.Extensions.Storage`, falls dieses Element noch nicht vorhanden ist.
5. Aktualisieren Sie `Table parameter name` auf „tableStorage“ und `Table name` auf „myCustomResources“.
6. Speichern Sie den aktualisierten Eingabeparameter.

![Übersicht zu benutzerdefinierten Anbietern](./media/create-custom-providers/azure-functions-table-bindings.png)

## <a name="update-restful-http-methods"></a>Aktualisieren von RESTful-HTTP-Methoden

In diesem Abschnitt werden die Schritte beschrieben, mit denen die Azure-Funktion schnell so eingerichtet werden kann, dass sie die RESTful-Anforderungsmethoden des benutzerdefinierten Anbieters enthält.

1. Navigieren Sie zur Registerkarte `Integrate` für „HttpTrigger“.
2. Aktualisieren Sie `Selected HTTP methods` in: GET, POST, DELETE und PUT.

![Übersicht zu benutzerdefinierten Anbietern](./media/create-custom-providers/azure-functions-http-methods.png)

## <a name="modifying-the-csproj"></a>Ändern der CSPROJ-Datei

> [!NOTE]
> Wenn die CSPROJ-Datei im Verzeichnis fehlt, können Sie sie manuell hinzufügen, oder sie wird angezeigt, nachdem die Erweiterung `Microsoft.Azure.WebJobs.Extensions.Storage` für die Funktion installiert wurde.

Als Nächstes aktualisieren wir die CSPROJ-Datei, damit sie hilfreiche NuGet-Bibliotheken enthält, die die Analyse eingehender Anforderungen von benutzerdefinierten Anbietern vereinfachen. Führen Sie die Schritte unter [Manuelles Installieren oder Aktualisieren von Azure Functions-Bindungserweiterungen aus dem Portal](../azure-functions/install-update-binding-extensions-manual.md) aus, und aktualisieren Sie die CSPROJ-Datei so, dass sie die folgenden Paketverweise enthält:

```xml
<PackageReference Include="Microsoft.Azure.WebJobs.Extensions.Storage" Version="3.0.4" />
<PackageReference Include="Microsoft.Azure.Management.ResourceManager.Fluent" Version="1.22.2" />
<PackageReference Include="Microsoft.Azure.WebJobs.Script.ExtensionsMetadataGenerator" Version="1.1.*" />
```

CSPROJ-Beispieldatei:

```xml
<Project Sdk="Microsoft.NET.Sdk">
  <PropertyGroup>
    <TargetFramework>netstandard2.0</TargetFramework>
    <WarningsAsErrors />
  </PropertyGroup>
  <ItemGroup>
    <PackageReference Include="Microsoft.Azure.WebJobs.Extensions.Storage" Version="3.0.4" />
    <PackageReference Include="Microsoft.Azure.Management.ResourceManager.Fluent" Version="1.22.2" />
    <PackageReference Include="Microsoft.Azure.WebJobs.Script.ExtensionsMetadataGenerator" Version="1.1.*" />
  </ItemGroup>
</Project>
```

## <a name="next-steps"></a>Nächste Schritte

In diesem Artikel richten wir eine Azure-Funktion so ein, dass sie wie ein Endpunkt (`endpoint`) eines benutzerdefinierten Azure-Anbieters funktioniert. Fahren Sie mit dem nächsten Artikel fort, um zu erfahren, wie Sie einen RESTful-Endpunkt (`endpoint`) eines benutzerdefinierten Anbieters erstellen.

- [Tutorial: Erstellen eines RESTful-Endpunkts für benutzerdefinierte Anbieter](./tutorial-custom-providers-function-authoring.md)
