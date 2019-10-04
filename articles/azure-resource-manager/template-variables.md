---
title: Variablen in Azure Resource Manager-Vorlagen
description: Beschreibt, wie Variablen in einer Azure Resource Manager-Vorlage definiert werden.
author: tfitzmac
ms.service: azure-resource-manager
ms.topic: conceptual
ms.date: 09/05/2019
ms.author: tomfitz
ms.openlocfilehash: 5ed6bb58f2f45de557f2127fdc8abd5cdf2ef965
ms.sourcegitcommit: 49c4b9c797c09c92632d7cedfec0ac1cf783631b
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 09/05/2019
ms.locfileid: "70383989"
---
# <a name="variables-in-azure-resource-manager-template"></a>Variablen in einer Azure Resource Manager-Vorlage

Dieser Artikel beschreibt, wie Sie Variablen in Ihrer Azure Resource Manager-Vorlage definieren und verwenden. Mit Variablen vereinfachen Sie Ihre Vorlage. Anstatt komplizierte Ausdrücke innerhalb der Vorlage zu wiederholen, definieren Sie eine Variable, die den komplizierten Ausdruck enthält. Anschließend verweisen Sie in der gesamten Vorlage auf diese Variable.

Resource Manager löst Variablen vor Beginn der Bereitstellungsvorgänge auf. Jedes Vorkommen der Variablen in der Vorlage wird von Resource Manager durch den aufgelösten Wert ersetzt.

## <a name="define-variable"></a>Definieren einer Variablen

Im folgenden Beispiel wird eine Variablendefinition gezeigt. Dabei wird ein Zeichenfolgenwert für einen Speicherkontonamen erstellt. Es werden mehrere Vorlagenfunktionen verwendet, um einen Parameterwert abzurufen und zu einer eindeutigen Zeichenfolge zu verketten.

```json
"variables": {
  "storageName": "[concat(toLower(parameters('storageNamePrefix')), uniqueString(resourceGroup().id))]"
},
```

Im Variablenabschnitt kann weder die [reference](resource-group-template-functions-resource.md#reference)-Funktion noch eine der [list](resource-group-template-functions-resource.md#list)-Funktionen verwendet werden. Diese Funktionen rufen den Laufzeitstatus einer Ressource ab und können nicht vor der Bereitstellung ausgeführt werden, wenn Variablen aufgelöst werden.

## <a name="use-variable"></a>Verwenden einer Variablen

In der Vorlage verweisen Sie mithilfe der [variables](resource-group-template-functions-deployment.md#variables)-Funktion auf den Wert für den Parameter. Im folgenden Beispiel wird gezeigt, wie die Variable für eine Ressourceneigenschaft verwendet wird.

```json
"resources": [
  {
    "type": "Microsoft.Storage/storageAccounts",
    "name": "[variables('storageName')]",
    ...
  }
]
```

## <a name="configuration-variables"></a>Konfigurationsvariablen

Sie können Variablen definieren, die zugehörige Werte zum Konfigurieren einer Umgebung enthalten. Die Variable wird als ein Objekt mit den Werten definiert. Das folgende Beispiel zeigt ein Objekt, das Werte für zwei Umgebungen enthält: **test** und **prod**.

```json
"variables": {
  "environmentSettings": {
    "test": {
      "instanceSize": "Small",
      "instanceCount": 1
    },
    "prod": {
      "instanceSize": "Large",
      "instanceCount": 4
    }
  }
},
```

Unter „Parameters“ erstellen Sie einen Wert, der angibt, welche Konfigurationswerte verwendet werden sollen.

```json
"parameters": {
  "environmentName": {
    "type": "string",
    "allowedValues": [
      "test",
      "prod"
    ]
  }
},
```

Verwenden Sie die Variable und den Parameter zusammen, um Einstellungen für die angegebene Umgebung abzurufen.

```json
"[variables('environmentSettings')[parameters('environmentName')].instanceSize]"
```

## <a name="example-templates"></a>Beispielvorlagen

In den folgenden Beispielen werden Szenarien für die Verwendung von Variablen veranschaulicht.

|Vorlage  |BESCHREIBUNG  |
|---------|---------|
| [Variablendefinitionen](https://github.com/Azure/azure-docs-json-samples/blob/master/azure-resource-manager/variables.json) | Zeigt die verschiedenen Typen von Variablen. Die Vorlage stellt keine Ressourcen bereit. Sie erstellt Variablenwerte und gibt diese Werte zurück. |
| [Konfigurationsvariable](https://github.com/Azure/azure-docs-json-samples/blob/master/azure-resource-manager/variablesconfigurations.json) | Zeigt die Verwendung einer Variablen, die Konfigurationswerte definiert. Die Vorlage stellt keine Ressourcen bereit. Sie erstellt Variablenwerte und gibt diese Werte zurück. |
| [Netzwerksicherheitsregeln](https://github.com/Azure/azure-docs-json-samples/blob/master/azure-resource-manager/multipleinstance/multiplesecurityrules.json) und [Parameterdatei](https://github.com/Azure/azure-docs-json-samples/blob/master/azure-resource-manager/multipleinstance/multiplesecurityrules.parameters.json) | Generiert ein Array im richtigen Format zum Zuweisen von Sicherheitsregeln zu einer Netzwerksicherheitsgruppe. |

## <a name="next-steps"></a>Nächste Schritte

* Weitere Informationen zu den verfügbaren Eigenschaften für Variablen finden Sie unter [Verstehen der Struktur und Syntax von Azure Resource Manager-Vorlagen](resource-group-authoring-templates.md).
* Empfehlungen zum Erstellen von Variablen finden Sie unter [Bewährte Methoden: Variablen](template-best-practices.md#variables).
