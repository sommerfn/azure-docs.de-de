---
title: Parameter in Azure Resource Manager-Vorlagen
description: Beschreibt, wie Parameter in einer Azure Resource Manager-Vorlage definiert werden.
author: tfitzmac
ms.service: azure-resource-manager
ms.topic: conceptual
ms.date: 09/05/2019
ms.author: tomfitz
ms.openlocfilehash: 4947b00d6fad5007751cd97d43ad6aca8d775330
ms.sourcegitcommit: 49c4b9c797c09c92632d7cedfec0ac1cf783631b
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 09/05/2019
ms.locfileid: "70383270"
---
# <a name="parameters-in-azure-resource-manager-templates"></a>Parameter in Azure Resource Manager-Vorlagen

Dieser Artikel beschreibt, wie Sie Parameter in Ihrer Azure Resource Manager-Vorlage definieren und verwenden. Durch Bereitstellen verschiedener Werte für Parameter können Sie eine Vorlage für verschiedene Umgebungen wiederverwenden.

Resource Manager löst Parameterwerte vor Beginn der Bereitstellungsvorgänge auf. Jedes Vorkommen des Parameters in der Vorlage wird von Resource Manager durch den aufgelösten Wert ersetzt.

## <a name="define-parameter"></a>Definieren eines Parameters

Das folgende Beispiel zeigt eine einfache Parameterdefinition. Hiermit wird ein Parameter mit dem Namen **storageSKU** definiert. Der Parameter ist ein Zeichenfolgenwert, und er akzeptiert nur Werte, die für die beabsichtigte Verwendung gültig sind. Wenn während der Bereitstellung kein Wert angegeben wird, verwendet der Parameter einen Standardwert.

```json
"parameters": {
  "storageSKU": {
    "type": "string",
    "allowedValues": [
      "Standard_LRS",
      "Standard_ZRS",
      "Standard_GRS",
      "Standard_RAGRS",
      "Premium_LRS"
    ],
    "defaultValue": "Standard_LRS",
    "metadata": {
      "description": "The type of replication to use for the storage account."
    }
  }
}
```

## <a name="use-parameter"></a>Verwenden eines Parameters

In der Vorlage verweisen Sie mithilfe der [parameters](resource-group-template-functions-deployment.md#parameters)-Funktion auf den Wert für den Parameter. Im folgenden Beispiel wird der Parameterwert zum Festlegen der SKU für das Speicherkonto verwendet.

```json
"resources": [
  {
    "type": "Microsoft.Storage/storageAccounts",
    "sku": {
      "name": "[parameters('storageSKU')]"
    },
    ...
  }
]
```

## <a name="template-functions"></a>Funktionen von Azure-Ressourcen-Manager-Vorlagen

Wenn Sie den Standardwert für einen Parameter angeben, können Sie die meisten Vorlagenfunktionen verwenden. Sie können einen anderen Parameterwert verwenden, um einen Standardwert zu erstellen. Die folgende Vorlage veranschaulicht die Verwendung von Funktionen im Standardwert. Wenn für die Website kein Name angegeben ist, wird ein eindeutiger Zeichenfolgenwert erstellt und an **site** angehängt. Wenn für den Hostplan kein Name angegeben ist, wird der Wert für die Website übernommen und **-plan** angefügt.

```json
"parameters": {
  "siteName": {
    "type": "string",
    "defaultValue": "[concat('site', uniqueString(resourceGroup().id))]",
    "metadata": {
      "description": "The site name. To use the default value, do not specify a new value."
    }
  },
  "hostingPlanName": {
    "type": "string",
    "defaultValue": "[concat(parameters('siteName'),'-plan')]",
    "metadata": {
      "description": "The host name. To use the default value, do not specify a new value."
    }
  }
}
```

Im Parameterabschnitt kann weder die [reference](resource-group-template-functions-resource.md#reference)-Funktion noch eine der [list](resource-group-template-functions-resource.md#list)-Funktionen verwendet werden. Diese Funktionen rufen den Laufzeitstatus einer Ressource ab und können nicht vor der Bereitstellung ausgeführt werden, wenn Parameter aufgelöst werden.

## <a name="objects-as-parameters"></a>Objekte als Parameter

Es kann einfacher sein, in Beziehung stehende Werte zu organisieren, indem sie als Objekt übergeben werden. Diese Vorgehensweise verringert auch die Anzahl der Parameter in der Vorlage.

Das folgende Beispiel zeigt einen Parameter, bei dem es sich um ein Objekt handelt. Der Standardwert gibt die erwarteten Eigenschaften für das Objekt an.

```json
"parameters": {
  "VNetSettings": {
    "type": "object",
    "defaultValue": {
      "name": "VNet1",
      "location": "eastus",
      "addressPrefixes": [
        {
          "name": "firstPrefix",
          "addressPrefix": "10.0.0.0/22"
        }
      ],
      "subnets": [
        {
          "name": "firstSubnet",
          "addressPrefix": "10.0.0.0/24"
        },
        {
          "name": "secondSubnet",
          "addressPrefix": "10.0.1.0/24"
        }
      ]
    }
  }
},
```

Sie verweisen auf die Eigenschaften des Objekts, indem Sie den Punktoperator verwenden.

```json
"resources": [
  {
    "apiVersion": "2015-06-15",
    "type": "Microsoft.Network/virtualNetworks",
    "name": "[parameters('VNetSettings').name]",
    "location": "[parameters('VNetSettings').location]",
    "properties": {
      "addressSpace":{
        "addressPrefixes": [
          "[parameters('VNetSettings').addressPrefixes[0].addressPrefix]"
        ]
      },
      "subnets":[
        {
          "name":"[parameters('VNetSettings').subnets[0].name]",
          "properties": {
            "addressPrefix": "[parameters('VNetSettings').subnets[0].addressPrefix]"
          }
        },
        {
          "name":"[parameters('VNetSettings').subnets[1].name]",
          "properties": {
            "addressPrefix": "[parameters('VNetSettings').subnets[1].addressPrefix]"
          }
        }
      ]
    }
  }
]
```

## <a name="example-templates"></a>Beispielvorlagen

In den folgenden Beispielen werden Szenarien für die Verwendung von Parametern veranschaulicht.

|Vorlage  |BESCHREIBUNG  |
|---------|---------|
|[Parameter mit Funktionen für Standardwerte](https://github.com/Azure/azure-docs-json-samples/blob/master/azure-resource-manager/parameterswithfunctions.json) | Zeigt, wie Vorlagenfunktionen verwendet werden, wenn Sie Standardwerte für Parameter definieren. Die Vorlage stellt keine Ressourcen bereit. Sie erstellt Parameterwerte und gibt diese Werte zurück. |
|[Parameterobjekt](https://github.com/Azure/azure-docs-json-samples/blob/master/azure-resource-manager/parameterobject.json) | Veranschaulicht die Verwendung eines Objekts für einen Parameter. Die Vorlage stellt keine Ressourcen bereit. Sie erstellt Parameterwerte und gibt diese Werte zurück. |


## <a name="next-steps"></a>Nächste Schritte

* Weitere Informationen zu den verfügbaren Eigenschaften für Parameter finden Sie unter [Verstehen der Struktur und Syntax von Azure Resource Manager-Vorlagen](resource-group-authoring-templates.md).
* Weitere Informationen zum Übergeben von Parameterwerten als Datei finden Sie unter [Erstellen einer Resource Manager-Parameterdatei](resource-manager-parameter-files.md).
* Empfehlungen zum Erstellen von Parametern finden Sie unter [Bewährte Methoden: Parameter](template-best-practices.md#parameters).
