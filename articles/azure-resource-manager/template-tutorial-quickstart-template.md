---
title: 'Tutorial: Verwenden von Azure-Schnellstartvorlagen | Microsoft-Dokumentation'
description: Erfahren Sie, wie Sie Azure-Schnellstartvorlagen verwenden, um die Vorlagenentwicklung abzuschließen.
services: azure-resource-manager
author: mumian
manager: carmonmills
ms.service: azure-resource-manager
ms.date: 10/04/2019
ms.topic: tutorial
ms.author: jgao
ms.openlocfilehash: a29d86d105579dda7c12b885e2977406f7b598a4
ms.sourcegitcommit: be344deef6b37661e2c496f75a6cf14f805d7381
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 10/07/2019
ms.locfileid: "72001484"
---
# <a name="tutorial-use-azure-quickstart-templates"></a>Tutorial: Verwenden von Azure-Schnellstartvorlagen

[Azure-Schnellstartvorlagen](https://azure.microsoft.com/resources/templates/) ist ein Repository von Vorlagen, die von der Community entwickelt wurden. Sie können die Beispielvorlagen in der Vorlagenentwicklung nutzen. In diesem Tutorial finden Sie die Definition einer Websiteressource, die Sie Ihrer eigenen Vorlage hinzufügen. Dieser Schritt dauert ungefähr **12 Minuten**.

## <a name="prerequisites"></a>Voraussetzungen

Wir empfehlen, das [Tutorial zu exportieren Vorlagen](template-tutorial-export-template.md) durchzuführen. Dies ist jedoch keine Voraussetzung.

Sie benötigen Visual Studio Code mit der Resource Manager-Tools-Erweiterung und entweder Azure PowerShell oder die Azure-Befehlszeilenschnittstelle (Azure CLI). Weitere Informationen finden Sie unter [Vorlagentools](template-tutorial-create-first-template.md#get-tools).

## <a name="review-your-template"></a>Überprüfen der Vorlage

Am Ende des vorherigen Tutorials enthielt Ihre Vorlage den folgenden JSON-Code:

[!code-json[](~/resourcemanager-templates/get-started-with-templates/export-template/azuredeploy.json)]

Diese Vorlage funktioniert für die Bereitstellung von Speicherkonten und App Service-Plänen, Sie sollten ihr jedoch eine Website hinzufügen. Sie können vorgefertigte Vorlagen verwenden, um schnell den JSON-Code zu ermitteln, der für die Bereitstellung einer Ressource erforderlich ist.

## <a name="find-a-template"></a>Eine Vorlage suchen

1. Öffnen Sie [Azure-Schnellstartvorlagen](https://azure.microsoft.com/resources/templates/).
1. Geben Sie **Linux-Web-App bereitstellen** in die **Suche** ein.
1. Wählen Sie die Vorlage mit dem Titel **Deploy a basic Linux web app** aus. Hier ist der [direkte Link](https://azure.microsoft.com/resources/templates/101-webapp-basic-linux/), falls Sie sie nicht finden können.
1. Wählen Sie **Auf GitHub suchen** aus.
1. Wählen Sie **azuredeploy.json** aus.
1. Überprüfen Sie die Vorlage. Suchen Sie insbesondere nach der Ressource `Microsoft.Web/sites`.

    ![Website: Resource Manager-Schnellstartvorlage](./media/template-tutorial-quickstart-template/resource-manager-template-quickstart-template-web-site.png)

## <a name="revise-the-existing-template"></a>Überarbeiten der vorhandenen Vorlage

Zusammenführen der Schnellstartvorlage mit der vorhandenen Vorlage:

[!code-json[](~/resourcemanager-templates/get-started-with-templates/quickstart-template/azuredeploy.json?range=1-108&highlight=32-45,49,85-100)]

Der WebApp-Name muss in Azure eindeutig sein. Zum Vermeiden von doppelten Namen wurde die Variable **webAppPortalName** von **"webAppPortalName": "[concat(parameters('webAppName'), '-webapp')]"** zu **"webAppPortalName": "[concat(parameters('webAppName'), uniqueString(resourceGroup().id))]"** aktualisiert.

Fügen Sie am Ende der `Microsoft.Web/serverfarms`-Definition ein Komma hinzu, um die Ressourcendefinition von der `Microsoft.Web/sites`-Definition zu trennen.

In dieser neuen Ressource sind einige wichtige Features zu beachten.

Sie werden feststellen, dass Sie ein Element mit dem Namen **dependsOn** enthält, das auf den App Service-Plan festgelegt ist. Diese Einstellung ist erforderlich, da der App Service-Plan vorhanden sein muss, bevor die Web-App erstellt wird. Das **dependsOn**-Element teilt Resource Manager mit, wie die Ressourcen für die Bereitstellung zu sortieren sind.

Die **serverFarmId**-Eigenschaft verwendet die Funktion [resourceId](resource-group-template-functions-resource.md#resourceid). Diese Funktion ruft den eindeutigen Bezeichner für eine Ressource ab. In diesem Fall wird der eindeutige Bezeichner für den App Service-Plan abgerufen. Die Web-App ist einem bestimmten App Service-Plan zugeordnet.

## <a name="deploy-the-template"></a>Bereitstellen der Vorlage

Zum Bereitstellen einer Vorlage verwenden Sie entweder die Azure CLI oder Azure PowerShell.

Falls Sie die Ressourcengruppe noch nicht erstellt haben, folgen Sie den Anweisungen unter [Erstellen einer Ressourcengruppe](template-tutorial-create-first-template.md#create-resource-group). Dieses Beispiel setzt voraus, dass Sie die Variable **templateFile** wie im [ersten Tutorial](template-tutorial-create-first-template.md#deploy-template) beschrieben auf den Pfad zur Vorlagendatei festgelegt haben.

# <a name="powershelltabazure-powershell"></a>[PowerShell](#tab/azure-powershell)

```azurepowershell
New-AzResourceGroupDeployment `
  -Name addwebapp `
  -ResourceGroupName myResourceGroup `
  -TemplateFile $templateFile `
  -storagePrefix "store" `
  -storageSKU Standard_LRS `
  -webAppName demoapp
```

# <a name="azure-clitabazure-cli"></a>[Azure-Befehlszeilenschnittstelle](#tab/azure-cli)

```azurecli
az group deployment create \
  --name addwebapp \
  --resource-group myResourceGroup \
  --template-file $templateFile \
  --parameters storagePrefix=store storageSKU=Standard_LRS webAppName=demoapp
```

---

## <a name="clean-up-resources"></a>Bereinigen von Ressourcen

Wenn Sie mit dem nächsten Tutorial fortfahren, müssen Sie die Ressourcengruppe nicht löschen.

Falls Sie keine weiteren Tutorials durchführen möchten, können Sie die bereitgestellten Ressourcen bereinigen, indem Sie die Ressourcengruppe löschen.

1. Wählen Sie im Azure-Portal im linken Menü die Option **Ressourcengruppe** aus.
2. Geben Sie den Namen der Ressourcengruppe in das Feld **Nach Name filtern** ein.
3. Klicken Sie auf den Namen der Ressourcengruppe.
4. Wählen Sie **Ressourcengruppe löschen** aus dem Menü ganz oben aus.

## <a name="next-steps"></a>Nächste Schritte

Sie haben gelernt, wie Sie eine Schnellstartvorlage für Ihre Vorlagenentwicklung verwenden. Im nächsten Tutorial fügen Sie den Ressourcen Tags hinzu.

> [!div class="nextstepaction"]
> [Hinzufügen von Tags](template-tutorial-add-tags.md)
