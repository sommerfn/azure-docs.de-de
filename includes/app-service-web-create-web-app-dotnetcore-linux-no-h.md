---
title: include file
description: Includedatei
services: app-service
author: cephalin
ms.service: app-service
ms.topic: include
ms.date: 08/06/2019
ms.author: cephalin
ms.custom: include file
ms.openlocfilehash: 2dc35727a82459750b4414b6e3eeddf28b31b718
ms.sourcegitcommit: 3073581d81253558f89ef560ffdf71db7e0b592b
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 08/06/2019
ms.locfileid: "68824727"
---
Erstellen Sie eine [Web-App](../articles/app-service/containers/app-service-linux-intro.md) im App Service-Plan `myAppServicePlan`. 

In Cloud Shell können Sie den Befehl [`az webapp create`](/cli/azure/webapp?view=azure-cli-latest) verwenden. Ersetzen Sie im folgenden Beispiel `<app-name>` durch einen global eindeutigen App-Namen (gültige Zeichen sind `a-z`, `0-9` und `-`). Die Runtime ist auf `DOTNETCORE|2.2` festgelegt. Führen Sie [`az webapp list-runtimes --linux`](/cli/azure/webapp?view=azure-cli-latest) aus, um alle unterstützten Laufzeiten anzuzeigen. 

```azurecli-interactive
# Bash
az webapp create --resource-group myResourceGroup --plan myAppServicePlan --name <app-name> --runtime "DOTNETCORE|2.2" --deployment-local-git
# PowerShell
az --% webapp create --resource-group myResourceGroup --plan myAppServicePlan --name <app-name> --runtime "DOTNETCORE|2.2" --deployment-local-git
```

Nach Erstellung der Web-App zeigt die Azure CLI eine Ausgabe wie im folgenden Beispiel an:

```json
Local git is configured with url of 'https://<username>@<app-name>.scm.azurewebsites.net/<app-name>.git'
{
  "availabilityState": "Normal",
  "clientAffinityEnabled": true,
  "clientCertEnabled": false,
  "cloningInfo": null,
  "containerSize": 0,
  "dailyMemoryTimeQuota": 0,
  "defaultHostName": "<app-name>.azurewebsites.net",
  "deploymentLocalGitUrl": "https://<username>@<app-name>.scm.azurewebsites.net/<app-name>.git",
  "enabled": true,
  < JSON data removed for brevity. >
}
```

Sie haben eine leere Web-App in einem Linux-Container mit aktivierter Git-Bereitstellung erstellt.

> [!NOTE]
> Die URL des Git-Remotespeicherorts wird in der `deploymentLocalGitUrl`-Eigenschaft im Format `https://<username>@<app-name>.scm.azurewebsites.net/<app-name>.git` angezeigt. Speichern Sie diese URL, da Sie sie später noch benötigen.
>
