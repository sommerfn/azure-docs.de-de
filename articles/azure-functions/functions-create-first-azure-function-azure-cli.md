---
title: Erstellen Sie Ihre erste Funktion mit der Azure-CLI
description: Erfahren Sie, wie Sie Ihre erste Azure-Funktion für die serverlose Ausführung mit der Azure CLI und Azure Functions Core Tools erstellen.
author: ggailey777
ms.author: glenga
ms.assetid: 674a01a7-fd34-4775-8b69-893182742ae0
ms.date: 11/13/2018
ms.topic: quickstart
ms.service: azure-functions
ms.custom: mvc
manager: gwallace
ms.openlocfilehash: 1d0c6edf250e49fe966388253617181ed2b3dff1
ms.sourcegitcommit: 1d0b37e2e32aad35cc012ba36200389e65b75c21
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 10/15/2019
ms.locfileid: "72329651"
---
# <a name="quickstart-create-your-first-function-from-the-command-line-using-azure-cli"></a>Schnellstart: Erstellen Ihrer ersten Funktion über die Befehlszeile mithilfe der Azure-Befehlszeilenschnittstelle

In diesem Schnellstartthema erfahren Sie Schritt für Schritt, wie Sie Ihre erste Funktion über Befehlszeile oder Terminal erstellen. Sie erstellen über die Azure-Befehlszeilenschnittstelle eine Funktionen-App. Hierbei handelt es sich um die [serverlose](https://azure.microsoft.com/solutions/serverless/) Infrastruktur, die Ihre Funktion hostet. Das Funktionscodeprojekt wird mithilfe der [Azure Functions Core Tools](functions-run-local.md), mit denen auch das Funktions-App-Projekt in Azure bereitgestellt wird, aus einer Vorlage generiert.

Die folgenden Schritte können auf einem Mac oder auf einem Computer unter Windows oder Linux ausgeführt werden.

## <a name="prerequisites"></a>Voraussetzungen

Vor dem Ausführen dieses Beispiels benötigen Sie Folgendes:

+ Installieren von [Azure Functions Core Tools](./functions-run-local.md#v2), Version 2.6.666 oder höher

+ Installieren Sie die [Azure CLI](/cli/azure/install-azure-cli). Für diesen Artikel ist die Azure CLI-Version 2.0 oder höher erforderlich. Führen Sie `az --version` aus, um herauszufinden, welche Version Sie haben. Sie können ebenso [Azure Cloud Shell](https://shell.azure.com/bash) verwenden.

+ Ein aktives Azure-Abonnement.

[!INCLUDE [quickstarts-free-trial-note](../../includes/quickstarts-free-trial-note.md)]

[!INCLUDE [functions-create-function-app-cli](../../includes/functions-create-function-app-cli.md)]

## <a name="enable-extension-bundles"></a>Aktivieren von Erweiterungsbundles

[!INCLUDE [functions-extension-bundles](../../includes/functions-extension-bundles.md)]

[!INCLUDE [functions-create-function-core-tools](../../includes/functions-create-function-core-tools.md)]

[!INCLUDE [functions-run-function-test-local](../../includes/functions-run-function-test-local.md)]

[!INCLUDE [functions-create-resource-group](../../includes/functions-create-resource-group.md)]

[!INCLUDE [functions-create-storage-account](../../includes/functions-create-storage-account.md)]

## <a name="create-a-function-app"></a>Erstellen einer Funktionen-App

Sie müssen über eine Funktionen-App verfügen, die die Ausführung Ihrer Funktionen in Azure hostet. Die Funktionen-App bietet eine Umgebung für die serverlose Ausführung des Funktionscodes. Mit ihr können Sie Funktionen zu logischen Einheiten gruppieren. Dies erleichtert die Verwaltung, Bereitstellung und Freigabe von Ressourcen. Erstellen Sie eine Funktionen-App mithilfe des Befehls [az functionapp create](/cli/azure/functionapp#az-functionapp-create). 

Ersetzen Sie im folgenden Befehl den Platzhalter `<APP_NAME>` durch einen eindeutigen Namen der Funktionen-App und den Platzhalter `<STORAGE_NAME>` durch den Speicherkontonamen. Da `<APP_NAME>` als DNS-Standarddomäne für die Funktionen-App verwendet wird, muss der Name für alle Apps in Azure eindeutig sein. Darüber hinaus sollten Sie die `<language>`-Runtime für Ihre Funktions-App auf `dotnet` (C#) oder `node` (JavaScript) festlegen.

```azurecli-interactive
az functionapp create --resource-group myResourceGroup --consumption-plan-location westeurope \
--name <APP_NAME> --storage-account  <STORAGE_NAME> --runtime <language>
```

Wenn Sie den Parameter _consumption-plan-location_ festlegen, wird die Funktions-App im Rahmen eines Hostingplans vom Typ „Verbrauch“ gehostet. Bei diesem serverlosen Plan werden Ressourcen dynamisch nach Bedarf für Ihre Funktionen hinzugefügt, und Sie bezahlen nur, wenn Funktionen ausgeführt werden. Weitere Informationen finden Sie unter [Auswählen des richtigen Serviceplans](functions-scale.md).

Nach Erstellung der Funktionen-App zeigt die Azure-CLI Informationen wie im folgenden Beispiel an:

```json
{
  "availabilityState": "Normal",
  "clientAffinityEnabled": true,
  "clientCertEnabled": false,
  "containerSize": 1536,
  "dailyMemoryTimeQuota": 0,
  "defaultHostName": "quickstart.azurewebsites.net",
  "enabled": true,
  "enabledHostNames": [
    "quickstart.azurewebsites.net",
    "quickstart.scm.azurewebsites.net"
  ],
   ....
    // Remaining output has been truncated for readability.
}
```

[!INCLUDE [functions-publish-project](../../includes/functions-publish-project.md)]

[!INCLUDE [functions-test-function-code](../../includes/functions-test-function-code.md)]

[!INCLUDE [functions-cleanup-resources](../../includes/functions-cleanup-resources.md)]

[!INCLUDE [functions-quickstart-next-steps-cli](../../includes/functions-quickstart-next-steps-cli.md)]

