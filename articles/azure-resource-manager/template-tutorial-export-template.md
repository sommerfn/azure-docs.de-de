---
title: 'Tutorial: Exportieren von Azure Resource Manager-Vorlagen aus dem Azure-Portal'
description: Erfahren Sie, wie Sie eine exportierte Vorlage verwenden, um die Vorlagenentwicklung abzuschließen.
services: azure-resource-manager
author: mumian
manager: carmonmills
ms.service: azure-resource-manager
ms.date: 10/04/2019
ms.topic: tutorial
ms.author: jgao
ms.openlocfilehash: 6e4f246cac0ecc1ab5942e522595f59c3625db8f
ms.sourcegitcommit: 824e3d971490b0272e06f2b8b3fe98bbf7bfcb7f
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 10/10/2019
ms.locfileid: "72243204"
---
# <a name="tutorial-use-exported-template-from-the-azure-portal"></a>Tutorial: Verwenden einer exportieren Vorlage aus dem Azure-Portal

In dieser Tutorialreihe haben Sie eine Vorlage zum Bereitstellen eines Azure Storage-Kontos erstellt. In den nächsten beiden Tutorials fügen Sie einen *App Service-Plan* und eine *Website* hinzu. Anstatt Vorlagen von Grund auf neu zu erstellen, erfahren Sie, wie Sie Vorlagen aus dem Azure-Portal exportieren und Beispielvorlagen aus den [Azure-Schnellstartvorlagen](https://azure.microsoft.com/resources/templates/) verwenden. Sie passen diese Vorlagen für ihre Verwendung an. Dieses Tutorial geht es vor allem um das Exportieren von Vorlagen und das Anpassen des Ergebnisses für ihre Vorlage. Dieser Schritt dauert ungefähr **14 Minuten**.

## <a name="prerequisites"></a>Voraussetzungen

Wir empfehlen, das [Tutorial zu Ausgaben](template-tutorial-add-outputs.md) durchzuführen. Dies ist jedoch keine Voraussetzung.

Sie benötigen Visual Studio Code mit der Resource Manager-Tools-Erweiterung und entweder Azure PowerShell oder die Azure-Befehlszeilenschnittstelle (Azure CLI). Weitere Informationen finden Sie unter [Vorlagentools](template-tutorial-create-first-template.md#get-tools).

## <a name="review-your-template"></a>Überprüfen der Vorlage

Am Ende des vorherigen Tutorials enthielt Ihre Vorlage den folgenden JSON-Code:

[!code-json[](~/resourcemanager-templates/get-started-with-templates/add-outputs/azuredeploy.json)]

Diese Vorlage eignet sich gut für die Bereitstellung von Speicherkonten, Sie sollten sie jedoch mit weiteren Ressourcen ergänzen. Sie können eine Vorlage aus einer vorhandenen Ressource exportieren, um den JSON-Code für diese Ressource schnell zu erhalten.

## <a name="create-app-service-plan"></a>App Service-Plan erstellen

1. Melden Sie sich beim [Azure-Portal](https://portal.azure.com) an.
1. Wählen Sie **Ressource erstellen**.
1. Geben Sie in **Marketplace durchsuchen** den Suchbegriff **App Service-Plan** ein, und wählen Sie dann **App Service-Plan** aus.  Wählen Sie nicht **App Service-Plan (klassisch)** aus.
1. Klicken Sie auf **Erstellen**.
1. Geben Sie Folgendes ein:

    - **Abonnement**: Wählen Sie Ihr Azure-Abonnement aus.
    - **Ressourcengruppe**: Wählen Sie **Neu erstellen** aus, und geben Sie einen Namen an. Geben Sie einen anderen Ressourcengruppennamen an, als Sie bisher in dieser Tutorialreihe verwendet haben.
    - **Name**: Geben Sie einen Namen für den App Service-Plan ein.
    - **Betriebssystem**: Wählen Sie **Linux** aus.
    - **Region**: Wählen Sie eine Azure-Region aus. Beispiel: **USA, Mitte**.
    - **Tarif**: Ändern Sie die SKU (unter „Dev/Test“) in **Basic B1**, um Kosten zu sparen.

    ![Exportieren einer Resource Manager-Vorlage aus dem Portal](./media/template-tutorial-export-template/resource-manager-template-export.png)
1. Klicken Sie auf **Überprüfen und erstellen**.
1. Klicken Sie auf **Erstellen**. Es dauert einen Moment, bis die Ressource erstellt wird.

## <a name="export-the-template"></a>Exportieren der Vorlage

1. Wählen Sie **Zu Ressource wechseln** aus.

    ![Zu Ressource wechseln](./media/template-tutorial-export-template/resource-manager-template-export-go-to-resource.png)

1. Wählen Sie **Vorlage exportieren** aus.

    ![Exportieren einer Resource Manager-Vorlage](./media/template-tutorial-export-template/resource-manager-template-export-template.png)

   Die Funktion zum Exportieren von Vorlagen erfasst den aktuellen Zustand einer Ressource und generiert eine Vorlage für deren Bereitstellung. Das Exportieren einer Vorlage kann hilfreich sein, um schnell den JSON-Code zu erhalten, den Sie benötigen, um eine Ressource bereitzustellen.

1. Kopieren Sie die Definition **Microsoft.Web/serverfarms** und die Parameterdefinition in Ihre Vorlage.

    ![Exportieren einer Resource Manager-Vorlage aus dem Portal: exportierte Vorlage](./media/template-tutorial-export-template/resource-manager-template-exported-template.png)

> [!IMPORTANT]
> In der Regel ist die exportierte Vorlage ausführlicher als bei der Erstellung einer Vorlage. Beispielsweise verfügt das SKU-Objekt in der exportierten Vorlage über fünf Eigenschaften. Diese Vorlage funktioniert, aber Sie könnten einfach die **name**-Eigenschaft verwenden. Sie können mit der exportierten Vorlage beginnen und Sie dann wie gewünscht ändern, um Sie an Ihre Anforderungen anzupassen.

## <a name="revise-the-existing-template"></a>Überarbeiten der vorhandenen Vorlage

Mit der exportierten Vorlage erhalten Sie den größten Teil des benötigten JSON-Code, müssen ihn aber für Ihre Vorlage anpassen. Achten Sie besonders auf Unterschiede in den Parametern und Variablen zwischen Ihrer Vorlage und der exportierten Vorlage. Der Exportprozess kennt natürlich nicht die Parameter und Variablen, die Sie bereits in ihrer Vorlage definiert haben.

Im folgenden Beispiel sind Ihre Ergänzungen der Vorlage hervorgehoben. Sie enthält den exportierten Code sowie einige Änderungen. Zuerst wird der Name des Parameters entsprechend ihrer Namenskonvention geändert. Zweitens wird der location-Parameter für den Standort des App Service-Plans verwendet. Drittens wird der **Name** innerhalb des Objekts **properties**, entfernt, da dieser Wert mit der **name**-Eigenschaft auf Ressourcenebene redundant ist.

Kopieren Sie die gesamte Datei, und ersetzen Sie Ihre Vorlage durch den Inhalt der Datei.

[!code-json[](~/resourcemanager-templates/get-started-with-templates/export-template/azuredeploy.json?range=1-77&highlight=28-31,50-69)]

## <a name="deploy-the-template"></a>Bereitstellen der Vorlage

Zum Bereitstellen einer Vorlage verwenden Sie entweder die Azure CLI oder Azure PowerShell.

Falls Sie die Ressourcengruppe noch nicht erstellt haben, folgen Sie den Anweisungen unter [Erstellen einer Ressourcengruppe](template-tutorial-create-first-template.md#create-resource-group). Dieses Beispiel setzt voraus, dass Sie die Variable **templateFile** wie im [ersten Tutorial](template-tutorial-create-first-template.md#deploy-template) beschrieben auf den Pfad zur Vorlagendatei festgelegt haben.

# <a name="powershelltabazure-powershell"></a>[PowerShell](#tab/azure-powershell)

```azurepowershell
New-AzResourceGroupDeployment `
  -Name addappserviceplan `
  -ResourceGroupName myResourceGroup `
  -TemplateFile $templateFile `
  -storagePrefix "store" `
  -storageSKU Standard_LRS
```

# <a name="azure-clitabazure-cli"></a>[Azure-Befehlszeilenschnittstelle](#tab/azure-cli)

```azurecli
az group deployment create \
  --name addappserviceplan \
  --resource-group myResourceGroup \
  --template-file $templateFile \
  --parameters storagePrefix=store storageSKU=Standard_LRS
```

---

## <a name="verify-deployment"></a>Überprüfen der Bereitstellung

Sie können die Bereitstellung überprüfen, indem Sie sich die Ressourcengruppe im Azure-Portal ansehen.

1. Melden Sie sich beim [Azure-Portal](https://portal.azure.com) an.
1. Wählen Sie im linken Menü **Ressourcengruppen** aus.
1. Wählen Sie die Ressourcengruppe aus, in der Sie die Vorlage bereitgestellt haben.
1. Die Ressourcengruppe enthält ein Speicherkonto und einen App Service-Plan.

## <a name="clean-up-resources"></a>Bereinigen von Ressourcen

Wenn Sie mit dem nächsten Tutorial fortfahren, müssen Sie die Ressourcengruppe nicht löschen.

Falls Sie keine weiteren Tutorials durchführen möchten, können Sie die bereitgestellten Ressourcen bereinigen, indem Sie die Ressourcengruppe löschen.

1. Wählen Sie im Azure-Portal im linken Menü die Option **Ressourcengruppe** aus.
2. Geben Sie den Namen der Ressourcengruppe in das Feld **Nach Name filtern** ein.
3. Klicken Sie auf den Namen der Ressourcengruppe.
4. Wählen Sie **Ressourcengruppe löschen** aus dem Menü ganz oben aus.

## <a name="next-steps"></a>Nächste Schritte

Sie haben gelernt, wie eine Vorlage aus dem Azure-Portal exportiert und anschließend für Ihre Vorlagenentwicklung verwendet wird. Sie können auch die Azure-Schnellstartvorlagen verwenden, um die Vorlagenentwicklung zu vereinfachen.

> [!div class="nextstepaction"]
> [Verwenden von Azure-Schnellstartvorlagen](template-tutorial-quickstart-template.md)
