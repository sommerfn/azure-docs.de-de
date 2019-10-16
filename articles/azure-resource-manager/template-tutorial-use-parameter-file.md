---
title: 'Tutorial: Verwenden der Parameterdatei zum Bereitstellen einer Azure Resource Manager-Vorlage'
description: Verwenden Sie Parameterdateien, die die Werte enthalten, die Sie für die Bereitstellung Ihrer Azure Resource Manager-Vorlage verwenden möchten.
services: azure-resource-manager
author: mumian
manager: carmonmills
ms.service: azure-resource-manager
ms.date: 10/04/2019
ms.topic: tutorial
ms.author: jgao
ms.openlocfilehash: fa29ea3d2f6edbbb016ce5c0c74415a5e765e85a
ms.sourcegitcommit: 42748f80351b336b7a5b6335786096da49febf6a
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 10/09/2019
ms.locfileid: "72177551"
---
# <a name="tutorial-use-parameter-files-to-deploy-your-resource-manager-template"></a>Tutorial: Verwenden von Parameterdateien zum Bereitstellen Ihrer Resource Manager-Vorlage

In diesem Tutorial erfahren Sie, wie Sie [Parameterdateien](resource-manager-parameter-files.md) verwenden, um die Werte zu speichern, die Sie während der Bereitstellung übergeben. In den vorherigen Tutorials haben Sie Inline-Parameter mit dem Bereitstellungs Befehl verwendet. Dieser Ansatz funktionierte beim Testen Ihrer Vorlage, aber bei der Automatisierung von Bereitstellungen kann es einfacher sein, eine Gruppe von Werten für Ihre Umgebung zu übergeben. Parameterdateien vereinfachen das Packen von Parameterwerten für eine bestimmte Umgebung. In diesem Tutorial erstellen Sie Parameterdateien für Entwicklungs- und Produktionsumgebungen. Dieser Schritt dauert ungefähr **12 Minuten**.

## <a name="prerequisites"></a>Voraussetzungen

Wir empfehlen, das [Tutorial zu Tags](template-tutorial-add-tags.md) durchzuführen. Dies ist jedoch keine Voraussetzung.

Sie benötigen Visual Studio Code mit der Resource Manager-Tools-Erweiterung und entweder Azure PowerShell oder die Azure-Befehlszeilenschnittstelle (Azure CLI). Weitere Informationen finden Sie unter [Vorlagentools](template-tutorial-create-first-template.md#get-tools).

## <a name="review-your-template"></a>Überprüfen der Vorlage

Ihre Vorlage verfügt über zahlreiche Parameter, die Sie während der Bereitstellung angeben können. Am Ende des vorherigen Tutorials sah Ihre Vorlage wie folgt aus:

[!code-json[](~/resourcemanager-templates/get-started-with-templates/add-tags/azuredeploy.json)]

Diese Vorlage funktioniert gut, jetzt möchten Sie jedoch problemlos die Parameter verwalten, die Sie für die Vorlage übergeben.

## <a name="add-parameter-files"></a>Parameterdateien hinzufügen

Parameterdateien sind JSON-Dateien mit einer Struktur, die ihrer Vorlage ähnelt. In der Datei geben Sie die Parameterwerte an, die Sie während der Bereitstellung übergeben möchten.

Erstellen Sie in VS Code eine neue Datei mit folgendem Inhalt. Speichern Sie die Datei mit dem Namen **azuredeploy.parameters.dev.json**.

[!code-json[](~/resourcemanager-templates/get-started-with-templates/add-tags/azuredeploy.parameters.dev.json)]

Diese Datei ist Ihre Parameterdatei für die Entwicklungsumgebung. Beachten Sie, dass „Standard_LRS“ als Speicherkonto verwendet wird, Ressourcen mit dem Präfix **dev** benannt werden, und das **Environment**-Tag auf **Dev** festgelegt wird.

Erstellen Sie erneut eine neue Datei mit dem folgenden Inhalt. Speichern Sie die Datei mit dem Namen **azuredeploy.parameters.prod.json**.

[!code-json[](~/resourcemanager-templates/get-started-with-templates/add-tags/azuredeploy.parameters.prod.json)]

Diese Datei ist Ihre Parameterdatei für die Produktionsumgebung. Beachten Sie, dass „Standard_GRS“ als Speicherkonto verwendet wird, Ressourcen mit dem Präfix **contoso** benannt werden, und das **Environment**-Tag auf **Production** festgelegt wird. In einer realen Produktionsumgebung würden Sie auch einen App-Dienst mit einer anderen SKU als „Free“ verwenden, für dieses Tutorial verwenden wir diese SKU aber weiterhin.

## <a name="deploy-the-template"></a>Bereitstellen der Vorlage

Verwenden Sie entweder die Azure CLI oder Azure PowerShell, um die Vorlage bereitzustellen.

Als abschließenden Test Ihrer Vorlage erstellen wir zwei neue Ressourcengruppen. Eine für die Entwicklungsumgebung und eine für die Produktionsumgebung.

Zuerst führen wir die Bereitstellung in der Entwicklungsumgebung durch.

# <a name="powershelltabazure-powershell"></a>[PowerShell](#tab/azure-powershell)

```azurepowershell
$templateFile = "{provide-the-path-to-the-template-file}"
$parameterFile="{path-to-azuredeploy.parameters.dev.json}"
New-AzResourceGroup `
  -Name myResourceGroupDev `
  -Location "East US"
New-AzResourceGroupDeployment `
  -Name devenvironment `
  -ResourceGroupName myResourceGroupDev `
  -TemplateFile $templateFile `
  -TemplateParameterFile $parameterFile
```

# <a name="azure-clitabazure-cli"></a>[Azure-Befehlszeilenschnittstelle](#tab/azure-cli)

```azurecli
templateFile="{provide-the-path-to-the-template-file}"
az group create \
  --name myResourceGroupDev \
  --location "East US"
az group deployment create \
  --name devenvironment \
  --resource-group myResourceGroupDev \
  --template-file $templateFile \
  --parameters azuredeploy.parameters.dev.json
```

---

Jetzt stellen wir in der Produktionsumgebung bereit.

# <a name="powershelltabazure-powershell"></a>[PowerShell](#tab/azure-powershell)

```azurepowershell
$parameterFile="{path-to-azuredeploy.parameters.prod.json}"
New-AzResourceGroup `
  -Name myResourceGroupProd `
  -Location "West US"
New-AzResourceGroupDeployment `
  -Name prodenvironment `
  -ResourceGroupName myResourceGroupProd `
  -TemplateFile $templateFile `
  -TemplateParameterFile $parameterFile
```

# <a name="azure-clitabazure-cli"></a>[Azure-Befehlszeilenschnittstelle](#tab/azure-cli)

```azurecli
az group create \
  --name myResourceGroupProd \
  --location "West US"
az group deployment create \
  --name prodenvironment \
  --resource-group myResourceGroupProd \
  --template-file $templateFile \
  --parameters azuredeploy.parameters.prod.json
```

---

## <a name="verify-the-deployment"></a>Überprüfen der Bereitstellung

Sie können die Bereitstellung überprüfen, indem Sie sich die Ressourcengruppen im Azure-Portal ansehen.

1. Melden Sie sich beim [Azure-Portal](https://portal.azure.com) an.
1. Wählen Sie im linken Menü **Ressourcengruppen** aus.
1. Sie sehen die beiden neuen Ressourcengruppen, die Sie in diesem Tutorial bereitgestellt haben.
1. Wählen Sie eine davon aus, und zeigen Sie die bereitgestellten Ressourcen an. Beachten Sie, dass Sie mit den Werten übereinstimmen, die Sie in der Parameterdatei für diese Umgebung angegeben haben.

## <a name="clean-up-resources"></a>Bereinigen von Ressourcen

1. Wählen Sie im Azure-Portal im linken Menü die Option **Ressourcengruppe** aus.
2. Geben Sie den Namen der Ressourcengruppe in das Feld **Nach Name filtern** ein. Wenn Sie diese Reihe abgeschlossen haben, verfügen Sie über drei Ressourcengruppen zum Löschen: myResourceGroup, myResourceGroupDev und myResourceGroupProd.
3. Klicken Sie auf den Namen der Ressourcengruppe.
4. Wählen Sie **Ressourcengruppe löschen** aus dem Menü ganz oben aus.

## <a name="next-steps"></a>Nächste Schritte

Herzlichen Glückwunsch, Sie haben diese Einführung in die Bereitstellung von Vorlagen in Azure abgeschlossen. Informieren Sie uns im Abschnitt „Feedback“, wenn Sie Kommentare oder Vorschläge haben. Vielen Dank!

Sie sind bereit, sich mit fortgeschritteneren Konzepten für Vorlagen zu befassen. Das nächste Tutorial geht detaillierter auf die Verwendung der Referenzdokumentation von Vorlagen zum Definieren von bereitzustellenden Ressourcen ein.

> [!div class="nextstepaction"]
> [Nutzen der Vorlagenreferenz](resource-manager-tutorial-create-encrypted-storage-accounts.md)
