---
title: Erstellen einer Azure Resource Manager-Parameterdatei
description: Erstellen einer Parameterdatei zum Übergeben von Werten während der Bereitstellung einer Azure Resource Manager-Vorlage
author: tfitzmac
ms.service: azure-resource-manager
ms.topic: conceptual
ms.date: 08/21/2019
ms.author: tomfitz
ms.openlocfilehash: 4305213d272172cb89bfdd207b6c8106af3f4939
ms.sourcegitcommit: 909ca340773b7b6db87d3fb60d1978136d2a96b0
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 09/13/2019
ms.locfileid: "70983929"
---
# <a name="create-resource-manager-parameter-file"></a>Erstellen einer Resource Manager-Parameterdatei

Anstatt Parameter als Inlinewerte in Ihrem Skript zu übergeben, ist es wohl einfacher, eine JSON-Datei zu verwenden, die die Parameterwerte enthält. In diesem Artikel wird veranschaulicht, wie Sie die Parameterdatei erstellen.

## <a name="parameter-file"></a>Parameterdatei

Die Parameterdatei hat das folgende Format:

```json
{
    "$schema": "https://schema.management.azure.com/schemas/2015-01-01/deploymentParameters.json#",
    "contentVersion": "1.0.0.0",
    "parameters": {
        "<first-parameter-name>": {
            "value": "<first-value>"
        },
        "<second-parameter-name>": {
            "value": "<second-value>"
        }
    }
}
```

Beachten Sie, dass die Parameterwerte in der Parameterdatei als Klartext gespeichert werden. Diese Vorgehensweise funktioniert bei Werten, die nicht sensibler Art sind, z. B. beim Angeben der SKU für eine Ressource. Sie ist nicht für sensible Werte, z. B. Kennwörter, geeignet. Falls Sie einen sensiblen Wert als Parameter übergeben müssen, sollten Sie den Wert in einem Schlüsseltresor speichern und in Ihrer Parameterdatei auf den Schlüsseltresor verweisen. Der sensible Wert wird während der Bereitstellung dann auf sichere Weise abgerufen.

Die folgende Parameterdatei enthält einen Klartextwert und einen Wert, der in einem Schlüsseltresor gespeichert ist.

```json
{
    "$schema": "https://schema.management.azure.com/schemas/2015-01-01/deploymentParameters.json#",
    "contentVersion": "1.0.0.0",
    "parameters": {
        "<first-parameter-name>": {
            "value": "<first-value>"
        },
        "<second-parameter-name>": {
            "reference": {
                "keyVault": {
                    "id": "<resource-id-key-vault>"
                },
                "secretName": "<secret-name>"
            }
        }
    }
}
```

Weitere Informationen zur Verwendung von Werten aus einem Schlüsseltresor finden Sie unter [Verwenden von Azure Key Vault zum Übergeben eines sicheren Parameterwerts während der Bereitstellung](resource-manager-keyvault-parameter.md).

## <a name="define-parameter-values"></a>Definieren von Parameterwerten

Öffnen Sie die Vorlage, die Sie bereitstellen, um zu ermitteln, wie Sie die Parameterwerte definieren. Sehen Sie sich den Parameterabschnitt der Vorlage an. Im folgenden Beispiel sind die Parameter aus einer Vorlage dargestellt.

```json
"parameters": {
    "storagePrefix": {
      "type": "string",
      "maxLength": 11
    },
    "storageAccountType": {
      "type": "string",
      "defaultValue": "Standard_LRS",
      "allowedValues": [
        "Standard_LRS",
        "Standard_GRS",
        "Standard_ZRS",
        "Premium_LRS"
      ]
    }
}
```

Das erste Detail ist der Name der einzelnen Parameter. Die Werte in Ihrer Parameterdatei müssen mit den Namen übereinstimmen.

```json
{
    "$schema": "https://schema.management.azure.com/schemas/2015-01-01/deploymentParameters.json#",
    "contentVersion": "1.0.0.0",
    "parameters": {
        "storagePrefix": {
        },
        "storageAccountType": {
        }
    }
}
```

Achten Sie auf den Typ des Parameters. Die Werte in Ihrer Parameterdatei müssen die gleichen Typen aufweisen. Für diese Vorlage können Sie beide Parameter als Zeichenfolgen angeben.

```json
{
    "$schema": "https://schema.management.azure.com/schemas/2015-01-01/deploymentParameters.json#",
    "contentVersion": "1.0.0.0",
    "parameters": {
        "storagePrefix": {
            "value": ""
        },
        "storageAccountType": {
            "value": ""
        }
    }
}
```

Suchen Sie als Nächstes nach einem Standardwert. Wenn ein Parameter über einen Standardwert verfügt, können Sie einen Wert angeben, aber dies ist nicht zwingend erforderlich.

```json
{
    "$schema": "https://schema.management.azure.com/schemas/2015-01-01/deploymentParameters.json#",
    "contentVersion": "1.0.0.0",
    "parameters": {
        "storagePrefix": {
            "value": "" // This value must be provided.
        },
        "storageAccountType": {
            "value": "" // This value is optional. Template will use default value if not provided.
        }
    }
}
```

Sehen Sie sich abschließend die zulässigen Werte und die Einschränkungen an, z. B. die maximale Länge. Hieraus können Sie den Bereich der Werte ablesen, den Sie für den Parameter angeben können.

```json
{
    "$schema": "https://schema.management.azure.com/schemas/2015-01-01/deploymentParameters.json#",
    "contentVersion": "1.0.0.0",
    "parameters": {
        "storagePrefix": {
            "value": "storage"
        },
        "storageAccountType": {
            "value": "Standard_ZRS"
        }
    }
}
```

## <a name="parameter-type-formats"></a>Parametertypformate

Im folgenden Beispiel sind die Formate der unterschiedlichen Parametertypen dargestellt.

```json
{
    "$schema": "https://schema.management.azure.com/schemas/2015-01-01/deploymentParameters.json#",
    "contentVersion": "1.0.0.0",
    "parameters": {
        "exampleString": {
            "value": "test string"
        },
        "exampleInt": {
            "value": 4
        },
        "exampleBool": {
            "value": true
        },
        "exampleArray": {
            "value": [
                "value 1",
                "value 2"
            ]
        },
        "exampleObject": {
            "value": {
                "property1": "value1",
                "property2": "value2"
            }
        }
   }
}
```

## <a name="file-name"></a>Dateiname

Die allgemeine Konvention für das Benennen der Parameterdatei ist das Hinzufügen von **.parameters** zum Vorlagennamen. Wenn Ihre Vorlage beispielsweise den Namen **azuredeploy.json** hat, ergibt sich für Ihre Parameterdatei der Name **azuredeploy.parameters.json**. Anhand dieser Namenskonvention können Sie die Verbindung zwischen der Vorlage und den Parametern erkennen.

Erstellen Sie mehr als eine Parameterdatei, wenn Sie die Bereitstellung in unterschiedlichen Umgebungen durchführen möchten. Fügen Sie beim Benennen der Parameterdatei Informationen zu ihrem Verwendungszweck hinzu. Verwenden Sie beispielsweise **azuredeploy.parameters-dev.json** und **azuredeploy.parameters-prod.json**.


## <a name="parameter-precedence"></a>Parameterrangfolge

Sie können Inlineparameter und eine lokale Parameterdatei im selben Bereitstellungsvorgang verwenden. Sie können beispielsweise einige Werte in der lokalen Parameterdatei angeben und weitere Werte während der Bereitstellung inline hinzufügen. Wenn Sie Werte für einen Parameter sowohl in der lokalen Parameterdatei als auch inline bereitstellen, haben die Inlinewerte Vorrang.

Bei Verwendung einer externen Parameterdatei können Sie jedoch keine anderen Werte (weder inline noch aus einer lokalen Datei) übergeben. Alle Inlineparameter werden ignoriert. Stellen Sie alle Parameterwerte in der externen Datei bereit.

## <a name="parameter-name-conflicts"></a>Parameternamenskonflikte

Wenn Ihre Vorlage einen Parameter enthält, der den gleichen Namen besitzt wie einer der Parameter des PowerShell-Befehls, zeigt PowerShell den Parameter der Vorlage mit dem Postfix **FromTemplate** an. Beispiel: Ein Parameter namens **ResourceGroupName** in Ihrer Vorlage verursacht einen Konflikt mit dem Parameter **ResourceGroupName** im Cmdlet [New-AzResourceGroupDeployment](/powershell/module/az.resources/new-azresourcegroupdeployment). Sie werden zur Eingabe eines Werts für **ResourceGroupNameFromTemplate** aufgefordert. Sie können diese Verwirrung vermeiden, indem Sie Parameternamen verwenden, die nicht für Bereitstellungsbefehle verwendet werden.

## <a name="next-steps"></a>Nächste Schritte

- Um zu verstehen, wie Parameter in der Vorlage definiert werden, lesen Sie [Parameter in Azure Resource Manager-Vorlagen](template-parameters.md).
- Weitere Informationen zur Verwendung von Werten aus einem Schlüsseltresor finden Sie unter [Verwenden von Azure Key Vault zum Übergeben eines sicheren Parameterwerts während der Bereitstellung](resource-manager-keyvault-parameter.md).
- Weitere Informationen zu Parametern finden Sie unter [Parameter in Azure Resource Manager-Vorlagen](template-parameters.md).
