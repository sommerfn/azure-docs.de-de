---
author: ggailey777
ms.service: azure-functions
ms.topic: include
ms.date: 05/27/2019
ms.author: glenga
ms.openlocfilehash: 8110d0a9d574c6691322df2162ca877b031cbc59
ms.sourcegitcommit: f56b267b11f23ac8f6284bb662b38c7a8336e99b
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 06/28/2019
ms.locfileid: "67442275"
---
Der einfachste Weg, Bindungserweiterungen zu installieren, ist die Aktivierung von [Erweiterungspaketen](../articles/azure-functions/functions-bindings-register.md#extension-bundles). Wenn die Pakete aktiviert sind, wird ein vordefinierter Satz von Erweiterungspaketen automatisch installiert.

Um Erweiterungspakete zu aktivieren, öffnen Sie die Datei *host.json* und aktualisieren Sie ihren Inhalt entsprechend dem folgenden Code:

```json
{
    "version": "2.0",
    "extensionBundle": {
        "id": "Microsoft.Azure.Functions.ExtensionBundle",
        "version": "[1.*, 2.0.0)"
    }
}
```