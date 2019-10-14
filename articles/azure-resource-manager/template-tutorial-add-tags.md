---
title: 'Tutorial: Hinzufügen von Tags zu Ressourcen in einer Azure Resource Manager-Vorlage'
description: Fügen Sie den Ressourcen, die Sie in Ihrer Azure Resource Manager-Vorlage bereitstellen, Tags hinzu. Mithilfe von Tags können Sie Ressourcen logisch organisieren.
services: azure-resource-manager
author: mumian
manager: carmonmills
ms.service: azure-resource-manager
ms.date: 10/04/2019
ms.topic: tutorial
ms.author: jgao
ms.openlocfilehash: 8b6ff50f7254a51bcdf37ecb0afd8f0041a2c5da
ms.sourcegitcommit: 42748f80351b336b7a5b6335786096da49febf6a
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 10/09/2019
ms.locfileid: "72177567"
---
# <a name="tutorial-add-tags-in-your-resource-manager-template"></a>Tutorial: Hinzufügen von Tags in Ihrer Resource Manager-Vorlage

In diesem Tutorial erfahren Sie, wie Sie Ressourcen in Ihrer Vorlage Tags hinzufügen. [Tags](resource-group-using-tags.md) helfen Ihnen dabei, Ihre Ressourcen logisch zu organisieren. Die Tagwerte werden in Kostenberichten angezeigt. Dieses Tutorial dauert ungefähr **8 Minuten**.

## <a name="prerequisites"></a>Voraussetzungen

Wir empfehlen, das [Tutorial zu Schnellstartvorlagen](template-tutorial-quickstart-template.md) durchzuführen. Dies ist jedoch keine Voraussetzung.

Sie benötigen Visual Studio Code mit der Resource Manager-Tools-Erweiterung und entweder Azure PowerShell oder die Azure-Befehlszeilenschnittstelle (Azure CLI). Weitere Informationen finden Sie unter [Vorlagentools](template-tutorial-create-first-template.md#get-tools).

## <a name="review-your-template"></a>Überprüfen der Vorlage

Ihre bisherige Vorlage hat ein Speicherkonto, einen App Service-Plan und eine Web-App bereitgestellt.

[!code-json[](~/resourcemanager-templates/get-started-with-templates/quickstart-template/azuredeploy.json)]

Nach der Bereitstellung dieser Ressourcen müssen Sie möglicherweise die Kosten nachverfolgen und nach Ressourcen suchen, die zu einer Kategorie gehören. Um dies zu ermöglichen, können Sie Tags hinzufügen.

## <a name="add-tags"></a>Hinzufügen von Tags

Sie taggen Ressourcen, um Werte hinzuzufügen, die Aufschluss über ihre Verwendung geben. Beispielsweise können Sie Tags hinzufügen, die die Umgebung und das Projekt auflisten. Sie können Tags hinzufügen, die eine Kostenstelle oder das für die Ressource zuständige Team identifizieren. Fügen Sie beliebige Werte hinzu, die für Ihre Organisation sinnvoll sind.

Im folgenden Beispiel sind die Änderungen hervorgehoben, die Sie an der Vorlage vornehmen. Kopieren Sie die gesamte Datei, und ersetzen Sie Ihre Vorlage durch den Inhalt der Datei.

[!code-json[](~/resourcemanager-templates/get-started-with-templates/add-tags/azuredeploy.json?range=1-118&highlight=46-52,64,78,100)]

## <a name="deploy-the-template"></a>Bereitstellen der Vorlage

Jetzt können Sie die Vorlage bereitstellen und sich die Ergebnisse ansehen.

Falls Sie die Ressourcengruppe noch nicht erstellt haben, folgen Sie den Anweisungen unter [Erstellen einer Ressourcengruppe](template-tutorial-create-first-template.md#create-resource-group). Dieses Beispiel setzt voraus, dass Sie die Variable **templateFile** wie im [ersten Tutorial](template-tutorial-create-first-template.md#deploy-template) beschrieben auf den Pfad zur Vorlagendatei festgelegt haben.

# <a name="powershelltabazure-powershell"></a>[PowerShell](#tab/azure-powershell)

```azurepowershell
New-AzResourceGroupDeployment `
  -Name addtags `
  -ResourceGroupName myResourceGroup `
  -TemplateFile $templateFile `
  -storagePrefix "store" `
  -storageSKU Standard_LRS `
  -webAppName demoapp
```

# <a name="azure-clitabazure-cli"></a>[Azure-Befehlszeilenschnittstelle](#tab/azure-cli)

```azurecli
az group deployment create \
  --name addtags \
  --resource-group myResourceGroup \
  --template-file $templateFile \
  --parameters storagePrefix=store storageSKU=Standard_LRS webAppName=demoapp
```

---

## <a name="verify-the-deployment"></a>Überprüfen der Bereitstellung

Sie können die Bereitstellung überprüfen, indem Sie sich die Ressourcengruppe im Azure-Portal ansehen.

1. Melden Sie sich beim [Azure-Portal](https://portal.azure.com) an.
1. Wählen Sie im linken Menü **Ressourcengruppen** aus.
1. Wählen Sie die Ressourcengruppe aus, in der Sie die Vorlage bereitgestellt haben.
1. Wählen Sie eine der Ressourcen aus, z. B. die Speicherkontoressource. Wie Sie sehen, enthält die Ressource jetzt Tags.

   ![Anzeigen von Tags](./media/template-tutorial-add-tags/show-tags.png)

## <a name="clean-up-resources"></a>Bereinigen von Ressourcen

Wenn Sie mit dem nächsten Tutorial fortfahren, müssen Sie die Ressourcengruppe nicht löschen.

Falls Sie keine weiteren Tutorials durchführen möchten, können Sie die bereitgestellten Ressourcen bereinigen, indem Sie die Ressourcengruppe löschen.

1. Wählen Sie im Azure-Portal im linken Menü die Option **Ressourcengruppe** aus.
2. Geben Sie den Namen der Ressourcengruppe in das Feld **Nach Name filtern** ein.
3. Klicken Sie auf den Namen der Ressourcengruppe.
4. Wählen Sie **Ressourcengruppe löschen** aus dem Menü ganz oben aus.

## <a name="next-steps"></a>Nächste Schritte

In diesem Tutorial haben Sie den Ressourcen Tags hinzugefügt. Im nächsten Tutorial erfahren Sie, wie Sie die Übergabe von Werten an die Vorlage mithilfe von Parameterdateien vereinfachen.

> [!div class="nextstepaction"]
> [Verwenden einer Parameterdatei](template-tutorial-use-parameter-file.md)
