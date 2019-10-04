---
author: ggailey777
ms.author: glenga
ms.date: 7/24/2019
ms.topic: include
ms.service: azure-functions
ms.openlocfilehash: 1928a8238cd73087e3c199675574dd1395f4d76d
ms.sourcegitcommit: aa042d4341054f437f3190da7c8a718729eb675e
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 08/09/2019
ms.locfileid: "68881320"
---
#### <a name="built-in-log-streaming"></a>Integriertes Protokollstreaming

Verwenden Sie die Option `logstream`, um den Empfang von Streamingprotokollen einer bestimmten in Azure ausgeführten Funktions-App zu starten (siehe folgendes Beispiel):

```bash
func azure functionapp logstream <FunctionAppName>
```

#### <a name="live-metrics-stream"></a>Live Metrics Stream

Sie können auch den [Live Metrics Stream](../articles/azure-monitor/app/live-stream.md) für Ihre Funktions-App in einem neuen Browserfenster anzeigen, indem Sie die Option `--browser` wie im folgenden Beispiel einschließen:

```bash
func azure functionapp logstream <FunctionAppName> --browser
```
