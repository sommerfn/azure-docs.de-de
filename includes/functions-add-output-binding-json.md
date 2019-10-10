---
author: ggailey777
ms.service: azure-functions
ms.topic: include
ms.date: 09/23/2019
ms.author: glenga
ms.openlocfilehash: 64a1062a8b73768a334277eafb663a7d2d5dd59a
ms.sourcegitcommit: 15e3bfbde9d0d7ad00b5d186867ec933c60cebe6
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 10/03/2019
ms.locfileid: "71838955"
---
Bindungsattribute werden direkt in der Datei „function.json“ definiert. Abhängig vom Bindungstyp sind möglicherweise zusätzliche Eigenschaften erforderlich. Die [Warteschlangenausgabekonfiguration](../articles/azure-functions/functions-bindings-storage-queue.md#output---configuration) beschreibt die Felder, die für eine Azure Storage-Warteschlangenbindung erforderlich sind. Mit der Erweiterung können Bindungen einfach in der Datei „function.json“ hinzugefügt werden. 

Klicken Sie zum Erstellen einer Bindung mit der rechten Maustaste (CTRL+Klicken unter macOS) auf die Datei `function.json` im Ordner „HttpTrigger“, und wählen Sie **Bindung hinzufügen** aus. Befolgen Sie die Anweisungen, um die folgenden Bindungseigenschaften für die neue Bindung zu definieren:

| Prompt | Wert | BESCHREIBUNG |
| -------- | ----- | ----------- |
| **Select binding direction** (Wählen Sie die Bindungsrichtung aus) | `out` | Die Bindung ist eine Ausgabebindung. |
| **Select binding with direction** (Wählen Sie die Bindung mit Richtung aus) | `Azure Queue Storage` | Die Bindung ist eine Azure Storage-Warteschlangenbindung. |
| **Der Name, der zum Identifizieren dieser Bindung in Ihrem Code verwendet wird** | `msg` | Name, der den Bindungsparameter identifiziert, auf den in Ihrem Code verwiesen wird. |
| **Die Warteschlange, an die die Nachricht gesendet wird** | `outqueue` | Der Name der Warteschlange, in den die Bindung schreibt. Wenn der *queueName* nicht vorhanden ist, erstellt die Bindung ihn bei der ersten Verwendung. |
| **Select setting from "local.setting.json"** (Wählen Sie eine Einstellung aus „local.setting.json“ aus) | `AzureWebJobsStorage` | Der Name einer Anwendungseinstellung, die die Verbindungszeichenfolge für das Storage-Konto enthält. Die Einstellung `AzureWebJobsStorage` enthält die Verbindungszeichenfolge für das Speicherkonto, das Sie mit der Funktions-App erstellt haben. |

Eine Bindung wird dem Array `bindings` in der Datei „function.json“ hinzugefügt, die nun wie das folgende Beispiel aussehen sollte:

```json
{
   ...

  "bindings": [
    {
      "authLevel": "function",
      "type": "httpTrigger",
      "direction": "in",
      "name": "req",
      "methods": [
        "get",
        "post"
      ]
    },
    {
      "type": "http",
      "direction": "out",
      "name": "$return"
    },
    {
      "type": "queue",
      "direction": "out",
      "name": "msg",
      "queueName": "outqueue",
      "connection": "AzureWebJobsStorage"
    }
  ]
}
```