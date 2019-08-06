---
author: ggailey777
ms.service: azure-functions
ms.topic: include
ms.date: 07/05/2019
ms.author: glenga
ms.openlocfilehash: 4db460a5dcefb49de3ad2b594d3957cbcf7445c3
ms.sourcegitcommit: fe6b91c5f287078e4b4c7356e0fa597e78361abe
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 07/29/2019
ms.locfileid: "68592802"
---
In einem C#-Klassenbibliotheksprojekt werden die Bindungen als Bindungsattribute der Funktionsmethode definiert. Die Datei *function.json* wird basierend auf diesen Attributen automatisch generiert.

Öffnen Sie die Projektdatei *HttpTrigger.cs*, und fügen Sie die folgende `using`-Anweisung hinzu:

```cs
using Microsoft.Azure.WebJobs.Extensions.Storage;
```

Fügen Sie der `Run`-Methodendefinition den folgenden Parameter hinzu:

```cs
[Queue("outqueue"),StorageAccount("AzureWebJobsStorage")] ICollector<string> msg
```

Der Parameter `msg` ist ein `ICollector<T>`-Typ und stellt eine Sammlung von Nachrichten dar, die in eine Ausgabebindung geschrieben werden, wenn die Funktion abgeschlossen wird. In diesem Fall ist die Ausgabe eine Speicherwarteschlange mit dem Namen `outqueue`. Die Verbindungszeichenfolge für das Storage-Konto wird durch `StorageAccountAttribute` festgelegt. Dieses Attribut gibt die Einstellung an, die die Verbindungszeichenfolge des Storage-Kontos enthält, und kann auf Klassen-, Methoden- oder Parameterebene angewandt werden. In diesem Fall können Sie `StorageAccountAttribute` weglassen, da Sie bereits das Standardspeicherkonto verwenden.

Die Run-Methodendefinition sollte nun wie folgt aussehen:  

```cs
[FunctionName("HttpTrigger")]
public static async Task<IActionResult> Run(
    [HttpTrigger(AuthorizationLevel.Function, "get", "post", Route = null)] HttpRequest req, 
    [Queue("outqueue"),StorageAccount("AzureWebJobsStorage")] ICollector<string> msg, ILogger log)
```
