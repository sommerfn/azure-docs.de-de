---
title: include file
description: include file
services: functions
author: ggailey777
ms.service: azure-functions
ms.topic: include
ms.date: 04/24/2019
ms.author: glenga
ms.custom: include file
ms.openlocfilehash: e2d63ab38bad341400538c5079fee22737cf0b8e
ms.sourcegitcommit: 39d95a11d5937364ca0b01d8ba099752c4128827
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 08/16/2019
ms.locfileid: "69562951"
---
## <a name="deploy-the-function-app-project-to-azure"></a>Bereitstellen des Funktions-App-Projekts in Azure

Nach dem Erstellen der Funktions-App in Azure können Sie den Core Tools-Befehl [`func azure functionapp publish`](../articles/azure-functions/functions-run-local.md#project-file-deployment) verwenden, um Ihren Projektcode in Azure bereitzustellen. Ersetzen Sie in diesen Beispielen `<APP_NAME>` durch den Namen Ihrer App aus dem vorherigen Schritt.

### <a name="c--javascript"></a>C\#/JavaScript

```command
func azure functionapp publish <APP_NAME>
```

### <a name="python"></a>Python

```command
func azure functionapp publish <APP_NAME> --build remote
```

### <a name="typescript"></a>TypeScript

```command
npm run build:production 
func azure functionapp publish <APP_NAME>
```

Es wird in etwa die folgende Ausgabe angezeigt, die zur Verbesserung der Lesbarkeit gekürzt wurde:

```output
Getting site publishing info...
...

Preparing archive...
Uploading content...
Upload completed successfully.
Deployment completed successfully.
Syncing triggers...
Functions in myfunctionapp:
    HttpTrigger - [httpTrigger]
        Invoke url: https://myfunctionapp.azurewebsites.net/api/httptrigger?code=cCr8sAxfBiow548FBDLS1....
```

Kopieren Sie den Wert `Invoke url` für `HttpTrigger`, den Sie nun zum Testen Ihrer Funktion in Azure verwenden können. Die URL enthält einen `code`-Abfragezeichenfolgenwert, der Ihr Funktionsschlüssel ist. Dieser Schlüssel erschwert es anderen Benutzern, Ihren HTTP-Triggerendpunkt in Azure aufzurufen.