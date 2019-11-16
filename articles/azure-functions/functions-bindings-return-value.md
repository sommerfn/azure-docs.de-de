---
title: Verwendung des Rückgabewerts einer Azure-Funktion
description: Informationen zum Verwalten von Rückgabewerten für Azure Functions
services: functions
documentationcenter: na
author: craigshoemaker
manager: gwallace
ms.service: azure-functions
ms.topic: reference
ms.date: 01/14/2019
ms.author: cshoe
ms.openlocfilehash: 8dd5a4d9d869c879ed402c5450690f0a691e1d2c
ms.sourcegitcommit: a107430549622028fcd7730db84f61b0064bf52f
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 11/14/2019
ms.locfileid: "74074401"
---
# <a name="using-the-azure-function-return-value"></a>Verwenden des Rückgabewerts einer Azure-Funktion

In diesem Artikel wird erläutert, wie Rückgabewerte innerhalb einer Funktion funktionieren.

In Sprachen mit Rückgabewert können Sie eine [Ausgabebindung](./functions-triggers-bindings.md#binding-direction) einer Funktion an den Rückgabewert binden:

* Wenden Sie in einer C#-Klassenbibliothek das Attribut der Ausgabebindung auf den Rückgabewert der Methode an.
* In anderen Sprachen legen Sie die Eigenschaft `name` in *function.json* auf `$return` fest.

Wenn mehrere Ausgabebindungen vorhanden sind, verwenden Sie den Rückgabewert für nur eine davon.

In C# und C#-Skripts können Daten alternativ mithilfe von `out`-Parametern und [Collector-Objekten](functions-reference-csharp.md#writing-multiple-output-values) an eine Ausgabebindung gesendet werden.

# <a name="ctabcsharp"></a>[C#](#tab/csharp)

Hier sehen Sie C#-Code, der den Rückgabewert für eine Ausgabebindung gefolgt von einem asynchronen Beispiel verwendet:

```cs
[FunctionName("QueueTrigger")]
[return: Blob("output-container/{id}")]
public static string Run([QueueTrigger("inputqueue")]WorkItem input, ILogger log)
{
    string json = string.Format("{{ \"id\": \"{0}\" }}", input.Id);
    log.LogInformation($"C# script processed queue message. Item={json}");
    return json;
}
```

```cs
[FunctionName("QueueTrigger")]
[return: Blob("output-container/{id}")]
public static Task<string> Run([QueueTrigger("inputqueue")]WorkItem input, ILogger log)
{
    string json = string.Format("{{ \"id\": \"{0}\" }}", input.Id);
    log.LogInformation($"C# script processed queue message. Item={json}");
    return Task.FromResult(json);
}
```

# <a name="c-scripttabcsharp-script"></a>[C#-Skript](#tab/csharp-script)

Hier sehen Sie die Bindungsdaten in der Datei *function.json*:

```json
{
    "name": "$return",
    "type": "blob",
    "direction": "out",
    "path": "output-container/{id}"
}
```

Hier sehen Sie den C#-Skriptcode gefolgt von einem asynchronen Beispiel:

```cs
public static string Run(WorkItem input, ILogger log)
{
    string json = string.Format("{{ \"id\": \"{0}\" }}", input.Id);
    log.LogInformation($"C# script processed queue message. Item={json}");
    return json;
}
```

```cs
public static Task<string> Run(WorkItem input, ILogger log)
{
    string json = string.Format("{{ \"id\": \"{0}\" }}", input.Id);
    log.LogInformation($"C# script processed queue message. Item={json}");
    return Task.FromResult(json);
}
```

# <a name="ftabfsharp"></a>[F#](#tab/fsharp)

Hier sehen Sie die Bindungsdaten in der Datei *function.json*:

```json
{
    "name": "$return",
    "type": "blob",
    "direction": "out",
    "path": "output-container/{id}"
}
```

Der F#-Code lautet wie folgt:

```fsharp
let Run(input: WorkItem, log: ILogger) =
    let json = String.Format("{{ \"id\": \"{0}\" }}", input.Id)   
    log.LogInformation(sprintf "F# script processed queue message '%s'" json)
    json
```

# <a name="javascripttabjavascript"></a>[JavaScript](#tab/javascript)

Hier sehen Sie die Bindungsdaten in der Datei *function.json*:

```json
{
    "name": "$return",
    "type": "blob",
    "direction": "out",
    "path": "output-container/{id}"
}
```

In JavaScript befindet sich der Rückgabewert im zweiten Parameter für `context.done`:

```javascript
module.exports = function (context, input) {
    var json = JSON.stringify(input);
    context.log('Node.js script processed queue message', json);
    context.done(null, json);
}
```

# <a name="pythontabpython"></a>[Python](#tab/python)

Hier sehen Sie die Bindungsdaten in der Datei *function.json*:

```json
{
    "name": "$return",
    "type": "blob",
    "direction": "out",
    "path": "output-container/{id}"
}
```
Dies ist der Python-Code:

```python
def main(input: azure.functions.InputStream) -> str:
    return json.dumps({
        'name': input.name,
        'length': input.length,
        'content': input.read().decode('utf-8')
    })
```

---

## <a name="next-steps"></a>Nächste Schritte

> [!div class="nextstepaction"]
> [Fehlerbehandlung bei der Azure Functions-Bindung](./functions-bindings-errors.md)
