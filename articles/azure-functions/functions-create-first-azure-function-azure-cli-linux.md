---
title: Erstellen Ihrer ersten Funktion unter Linux in Azure
description: Hier erfahren Sie, wie Sie unter Verwendung von Befehlszeilentools, Azure Functions Core Tools und der Azure-Befehlszeilenschnittstelle Ihre erste Funktion erstellen, die unter Linux in Azure gehostet wird.
ms.date: 03/12/2019
ms.topic: quickstart
ms.custom: mvc, fasttrack-edit
ms.openlocfilehash: 19abfee69db53c560dfa2696d85f8c1c3d770c09
ms.sourcegitcommit: d6b68b907e5158b451239e4c09bb55eccb5fef89
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 11/20/2019
ms.locfileid: "74230812"
---
# <a name="quickstart-create-your-first-function-hosted-on-linux-using-command-line-tools"></a>Schnellstart: Erstellen Ihrer ersten unter Linux gehosteten Funktion unter Verwendung von Befehlszeilentools

Mit Azure Functions können Sie Code in einer [serverlosen](https://azure.com/serverless) Linux-Umgebung ausführen, ohne vorher einen virtuellen Computer erstellen oder eine Webanwendung veröffentlichen zu müssen. Für das Hosten unter Linux ist [die Functions 2.x-Runtime](functions-versions.md) erforderlich. Serverlose Funktionen werden im [Verbrauchstarif](functions-scale.md#consumption-plan) ausgeführt.

In diesem Schnellstartartikel erfahren Sie Schritt für Schritt, wie Sie mithilfe der Azure-Befehlszeilenschnittstelle Ihre erste unter Linux ausgeführte Funktions-App erstellen. Der Funktionscode wird lokal erstellt und dann über [Azure Functions Core Tools](functions-run-local.md) in Azure bereitgestellt.

Die folgenden Schritte werden für Mac-, Windows- oder Linux-Computer unterstützt. In diesem Artikel wird gezeigt, wie Sie Funktionen in JavaScript oder C# erstellen. Informationen zum Erstellen von Python-Funktionen finden Sie unter [Erstellen einer durch HTTP ausgelösten Funktion in Azure](functions-create-first-function-python.md).

## <a name="prerequisites"></a>Voraussetzungen

Vor dem Ausführen dieses Beispiels benötigen Sie Folgendes:

+ Installieren von [Azure Functions Core Tools](./functions-run-local.md#v2), Version 2.6.666 oder höher.

+ Installieren Sie die [Azure CLI]( /cli/azure/install-azure-cli). Für diesen Artikel ist die Azure CLI-Version 2.0 oder höher erforderlich. Führen Sie `az --version` aus, um herauszufinden, welche Version Sie haben. Sie können ebenso [Azure Cloud Shell](https://shell.azure.com/bash) verwenden.

+ Ein aktives Azure-Abonnement.

[!INCLUDE [quickstarts-free-trial-note](../../includes/quickstarts-free-trial-note.md)]

[!INCLUDE [functions-create-function-app-cli](../../includes/functions-create-function-app-cli.md)]

## <a name="enable-extension-bundles"></a>Aktivieren von Erweiterungsbundles

[!INCLUDE [functions-extension-bundles](../../includes/functions-extension-bundles.md)]

[!INCLUDE [functions-create-function-core-tools](../../includes/functions-create-function-core-tools.md)]

[!INCLUDE [functions-run-function-test-local](../../includes/functions-run-function-test-local.md)]

[!INCLUDE [functions-create-resource-group](../../includes/functions-create-resource-group.md)]

[!INCLUDE [functions-create-storage-account](../../includes/functions-create-storage-account.md)]

## <a name="create-a-linux-function-app-in-azure"></a>Erstellen einer Linux-Funktions-App in Azure

Sie müssen über eine Funktions-App verfügen, die die Ausführung Ihrer Funktionen unter Linux hostet. Die Funktions-App bietet eine serverlose Umgebung für die Ausführung Ihres Funktionscodes. Mit ihr können Sie Funktionen zu logischen Einheiten gruppieren. Dies erleichtert die Verwaltung, Bereitstellung und Freigabe von Ressourcen. Erstellen Sie mithilfe des Befehls [az functionapp create](/cli/azure/functionapp#az-functionapp-create) eine unter Linux ausführbare Funktions-App.

Ersetzen Sie im folgenden Befehl den Platzhalter `<app_name>` durch einen eindeutigen Namen für die Funktions-App und `<storage_name>` durch den Speicherkontonamen. `<app_name>` ist gleichzeitig die DNS-Standarddomäne für die Funktions-App. Dieser Name muss für alle Apps in Azure eindeutig sein. Zudem sollten Sie die `<language>`-Runtime für Ihre Funktions-App auf `dotnet` (C#), `node` (JavaScript/TypeScript) oder `python` festlegen.

```azurecli-interactive
az functionapp create --resource-group myResourceGroup --consumption-plan-location westus --os-type Linux \
--name <app_name> --storage-account  <storage_name> --runtime <language>
```

Nach Erstellung der Funktions-App wird die folgende Meldung angezeigt:

```output
Your serverless Linux function app 'myfunctionapp' has been successfully created.
To active this function app, publish your app content using Azure Functions Core Tools or the Azure portal.
```

Nun können Sie Ihr Projekt in der neuen Funktions-App in Azure veröffentlichen.

[!INCLUDE [functions-publish-project](../../includes/functions-publish-project.md)]

[!INCLUDE [functions-test-function-code](../../includes/functions-test-function-code.md)]

[!INCLUDE [functions-cleanup-resources](../../includes/functions-cleanup-resources.md)]

[!INCLUDE [functions-quickstart-next-steps-cli](../../includes/functions-quickstart-next-steps-cli.md)]