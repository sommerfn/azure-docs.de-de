---
author: tamram
ms.service: storage
ms.topic: include
ms.date: 10/26/2018
ms.author: tamram
ms.openlocfilehash: 5102866cdda51ef545fd95b32946cb17c6e40a3c
ms.sourcegitcommit: d773b5743cb54b8cbcfa5c5e4d21d5b45a58b081
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 10/08/2019
ms.locfileid: "72038173"
---
Die [Microsoft Azure Configuration Manager-Bibliothek für .NET](https://www.nuget.org/packages/Microsoft.Azure.ConfigurationManager/) stellt eine Klasse für die Analyse einer Verbindungszeichenfolge aus einer Konfigurationsdatei bereit. Die Klasse [CloudConfigurationManager](https://msdn.microsoft.com/library/azure/mt634650.aspx) analysiert Konfigurationseinstellungen. Sie analysiert Einstellungen für Clientanwendungen, die auf dem Desktop, auf einem mobilen Gerät, auf einem virtuellen Azure-Computer oder in einem Azure-Clouddienst ausgeführt werden.

Fügen Sie die folgenden `using`-Anweisungen hinzu, um auf das Paket `CloudConfigurationManager` zu verweisen:

```csharp
using Microsoft.Azure; //Namespace for CloudConfigurationManager
using Microsoft.Azure.Storage;
```

Das folgende Beispiel zeigt, wie Sie eine Verbindungszeichenfolge aus einer Konfigurationsdatei abrufen:

```csharp
// Parse the connection string and return a reference to the storage account.
CloudStorageAccount storageAccount = CloudStorageAccount.Parse(
    CloudConfigurationManager.GetSetting("StorageConnectionString"));
```

Die Verwendung von Azure Configuration Manager ist optional. Sie können auch eine API wie die [Klasse „ConfigurationManager“](/dotnet/api/system.configuration.configurationmanager) von .NET Framework verwenden.
