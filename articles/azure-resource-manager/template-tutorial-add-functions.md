---
title: 'Tutorial: Hinzufügen von Vorlagenfunktionen zur Azure Resource Manager-Vorlage'
description: Fügen Sie Ihrer Azure Resource Manager-Vorlage Vorlagenfunktionen hinzu, um Werte zu erstellen.
services: azure-resource-manager
author: mumian
manager: carmonmills
ms.service: azure-resource-manager
ms.date: 10/04/2019
ms.topic: tutorial
ms.author: jgao
ms.openlocfilehash: ca6e7f326ba67eac33e4dee5fa844d7676ab864b
ms.sourcegitcommit: 4d177e6d273bba8af03a00e8bb9fe51a447196d0
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 10/04/2019
ms.locfileid: "71963657"
---
# <a name="tutorial-add-template-functions-to-your-resource-manager-template"></a>Tutorial: Hinzufügen von Vorlagenfunktionen zu Ihrer Resource Manager-Vorlage

In diesem Tutorial erfahren Sie, wie Sie Ihrer Vorlage [Vorlagenfunktionen](resource-group-template-functions.md) hinzufügen. Sie verwenden Funktionen, um dynamisch Werte zu erstellen. Zusätzlich zu diesen vom System bereitgestellten Vorlagenfunktionen können Sie auch [benutzerdefinierte Funktionen](./template-user-defined-functions.md) (User-Defined Functions, UDFs) erstellen. Dieses Tutorial dauert ungefähr **7 Minuten**.

## <a name="prerequisites"></a>Voraussetzungen

Wir empfehlen, das [Tutorial zu Parametern](template-tutorial-add-parameters.md) durchzuführen. Dies ist jedoch keine Voraussetzung.

Sie benötigen Visual Studio Code mit der Resource Manager-Tools-Erweiterung und entweder Azure PowerShell oder die Azure-Befehlszeilenschnittstelle (Azure CLI). Weitere Informationen finden Sie unter [Vorlagentools](template-tutorial-create-first-template.md#get-tools).

## <a name="review-your-template"></a>Überprüfen der Vorlage

Am Ende des vorherigen Tutorials enthielt Ihre Vorlage den folgenden JSON-Code:

[!code-json[](~/resourcemanager-templates/get-started-with-templates/add-sku/azuredeploy.json)]

Der Standort des Speicherkontos ist auf **USA, Osten** hartcodiert. Möglicherweise müssen Sie das Speicherkonto jedoch in anderen Regionen bereitstellen. Auch hier erweist sich die mangelnde Flexibilität Ihrer Vorlage als Problem. Sie könnten einen Parameter für den Standort hinzufügen, es wäre aber von Vorteil, wenn sein Standardwert nicht nur ein hartcodierter Wert wäre.

## <a name="use-function"></a>Verwenden einer Funktion

Wenn Sie das vorherige Tutorial dieser Reihe durchgeführt haben, haben Sie bereits eine Funktion verwendet. Beim Hinzufügen von **[parameters('storageName')]** haben Sie die Funktion [parameters](resource-group-template-functions-deployment.md#parameters) verwendet. Die Klammern geben an, dass es sich bei der Syntax innerhalb der Klammern um einen [Vorlagenausdruck](template-expressions.md) handelt. Resource Manager löst die Syntax auf, anstatt sie als Literalwert zu behandeln.

Funktionen rufen während der Bereitstellung dynamisch Werte ab und erhöhen dadurch die Flexibilität Ihrer Vorlage. In diesem Tutorial verwenden Sie eine Funktion, um den Standort der Ressourcengruppe abzurufen, die Sie für die Bereitstellung verwenden.

Das folgende Beispiel zeigt die Änderungen, die Sie vornehmen, um einen Parameter namens **location** hinzuzufügen.  Der Standardwert des Parameters ruft die Funktion [resourceGroup](resource-group-template-functions-resource.md#resourcegroup) auf. Diese Funktion gibt ein Objekt mit Informationen zu der für die Bereitstellung verwendeten Ressourcengruppe zurück. Eine der Eigenschaften im Objekt ist eine Standorteigenschaft. Wenn Sie den Standardwert verwenden, wird für das Speicherkonto der gleiche Standort verwendet wie für die Ressourcengruppe. Die Ressourcen in einer Ressourcengruppe müssen nicht den gleichen Standort haben. Sie können bei Bedarf auch einen anderen Standort angeben.

Kopieren Sie die gesamte Datei, und ersetzen Sie Ihre Vorlage durch den Inhalt der Datei.

[!code-json[](~/resourcemanager-templates/get-started-with-templates/add-location/azuredeploy.json?range=1-44&highlight=24-27,34)]

## <a name="deploy-template"></a>Bereitstellen der Vorlage

In den vorherigen Tutorials haben Sie ein Speicherkonto in der Region „USA, Osten“ erstellt, Ihre Ressourcengruppe wurde aber in „USA, Mitte“ erstellt. In diesem Tutorial wird Ihr Speicherkonto in der gleichen Region erstellt wie die Ressourcengruppe. Verwenden Sie den Standardwert für den Standort, damit Sie diesen Parameterwert nicht angeben müssen. Da Sie ein Speicherkonto an einem anderen Standort erstellen, müssen Sie einen neuen Namen für das Speicherkonto angeben. Verwenden Sie beispielsweise anstelle von **store1** das Präfix **store2**.

Falls Sie die Ressourcengruppe noch nicht erstellt haben, folgen Sie den Anweisungen unter [Erstellen einer Ressourcengruppe](template-tutorial-create-first-template.md#create-resource-group). Dieses Beispiel setzt voraus, dass Sie die Variable **templateFile** wie im [ersten Tutorial](template-tutorial-create-first-template.md#deploy-template) beschrieben auf den Pfad zur Vorlagendatei festgelegt haben.

# <a name="powershelltabazure-powershell"></a>[PowerShell](#tab/azure-powershell)

```azurepowershell
New-AzResourceGroupDeployment `
  -Name addlocationparameter `
  -ResourceGroupName myResourceGroup `
  -TemplateFile $templateFile `
  -storageName "{new-unique-name}"
```

# <a name="azure-clitabazure-cli"></a>[Azure-Befehlszeilenschnittstelle](#tab/azure-cli)

```azurecli
az group deployment create \
  --name addlocationparameter \
  --resource-group myResourceGroup \
  --template-file $templateFile \
  --parameters storageName={new-unique-name}
```

---

## <a name="verify-the-deployment"></a>Überprüfen der Bereitstellung

Sie können die Bereitstellung überprüfen, indem Sie sich die Ressourcengruppe im Azure-Portal ansehen.

1. Melden Sie sich beim [Azure-Portal](https://portal.azure.com) an.
1. Wählen Sie im linken Menü **Ressourcengruppen** aus.
1. Wählen Sie die Ressourcengruppe aus, in der Sie die Vorlage bereitgestellt haben.
1. Sie sehen, dass eine Speicherkontoressource bereitgestellt wurde, die den gleichen Standort aufweist wie die Ressourcengruppe.

## <a name="clean-up-resources"></a>Bereinigen von Ressourcen

Wenn Sie mit dem nächsten Tutorial fortfahren, müssen Sie die Ressourcengruppe nicht löschen.

Falls Sie keine weiteren Tutorials durchführen möchten, können Sie die bereitgestellten Ressourcen bereinigen, indem Sie die Ressourcengruppe löschen.

1. Wählen Sie im Azure-Portal im linken Menü die Option **Ressourcengruppe** aus.
2. Geben Sie den Namen der Ressourcengruppe in das Feld **Nach Name filtern** ein.
3. Klicken Sie auf den Namen der Ressourcengruppe.
4. Wählen Sie **Ressourcengruppe löschen** aus dem Menü ganz oben aus.

## <a name="next-steps"></a>Nächste Schritte

In diesem Tutorial haben Sie den Standardwert für einen Parameter mithilfe einer Funktion definiert. In den nächsten Tutorials dieser Reihe werden Sie weitere Funktionen verwenden. Am Ende der Reihe fügen Sie jedem Abschnitt der Vorlage Funktionen hinzu.

> [!div class="nextstepaction"]
> [Hinzufügen von Variablen](template-tutorial-add-variables.md)
