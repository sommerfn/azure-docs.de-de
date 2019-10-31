---
title: Untergeordnete Ressourcen – Azure Resource Manager-Vorlagen
description: Beschreibt, wie Sie den Namen und Typ für untergeordnete Ressourcen in einer Azure Resource Manager-Vorlage festlegen.
author: tfitzmac
ms.service: azure-resource-manager
ms.topic: conceptual
ms.date: 08/26/2019
ms.author: tomfitz
ms.openlocfilehash: 3a90b2155b11d4c12bc1f571af3f15fdbceb12b9
ms.sourcegitcommit: 6eecb9a71f8d69851bc962e2751971fccf29557f
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 10/17/2019
ms.locfileid: "72532287"
---
# <a name="set-name-and-type-for-child-resources"></a>Festlegen von Name und Typ für untergeordnete Ressourcen

Untergeordnete Ressourcen sind Ressourcen, die nur im Kontext einer anderen Ressource verfügbar sind. Beispielsweise kann eine [Erweiterung des virtuellen Computers](/azure/templates/microsoft.compute/2019-03-01/virtualmachines/extensions) nicht ohne einen [virtuellen Computer](/azure/templates/microsoft.compute/2019-03-01/virtualmachines) vorhanden sein. Die Erweiterungsressource ist ein untergeordnetes Element des virtuellen Computers.

In einer Resource Manager-Vorlage können Sie die untergeordnete Ressource entweder innerhalb oder außerhalb der übergeordneten Ressource angeben. Im folgenden Beispiel ist die untergeordnete Ressource in der Eigenschaft „resources“ der übergeordneten Ressource enthalten.

```json
"resources": [
  {
    <parent-resource>
    "resources": [
      <child-resource>
    ]
  }
]
```

Im nächsten Beispiel ist die untergeordnete Ressource außerhalb der übergeordneten Ressource angegeben. So können Sie vorgehen, wenn die übergeordnete Ressource nicht in derselben Vorlage bereitgestellt wird oder wenn Sie mithilfe von [copy](resource-group-create-multiple.md) mehrere untergeordnete Ressourcen erstellen möchten.

```json
"resources": [
  {
    <parent-resource>
  },
  {
    <child-resource>
  }
]
```

Welche Werte Sie für den Ressourcennamen und Typ angeben, richtet sich danach, ob die untergeordnete Ressource innerhalb oder außerhalb der übergeordneten Ressource definiert ist.

## <a name="within-parent-resource"></a>Innerhalb der übergeordneten Ressource

Bei Definition innerhalb des übergeordneten Ressourcentyps formatieren Sie die Werte für Typ und Name als ein einzelnes Wort ohne Schrägstriche.

```json
"type": "{child-resource-type}",
"name": "{child-resource-name}",
```

Das folgende Beispiel zeigt ein virtuelles Netzwerk mit einem Subnetz. Beachten Sie, dass das Subnetz im Ressourcenarray für das virtuelle Netzwerk enthalten ist. Der Name ist auf **Subnet1** und der Typ auf **subnets** festgelegt. Die untergeordnete Ressource ist als von der übergeordneten Ressource abhängig gekennzeichnet, da die übergeordnete Ressource vorhanden sein muss, bevor die untergeordnete Ressource bereitgestellt werden kann.

```json
"resources": [
  {
    "apiVersion": "2018-10-01",
    "type": "Microsoft.Network/virtualNetworks",
    "name": "VNet1",
    "location": "[parameters('location')]",
    "properties": {
      "addressSpace": {
        "addressPrefixes": [
          "10.0.0.0/16"
        ]
      }
    },
    "resources": [
      {
        "apiVersion": "2018-10-01",
        "type": "subnets",
        "location": "[parameters('location')]",
        "name": "Subnet1",
        "dependsOn": [
          "VNet1"
        ],
        "properties": {
          "addressPrefix": "10.0.0.0/24"
        }
      }
    ]
  }
]
```

Der vollständige Ressourcentyp lautet immer noch **Microsoft.Network/virtualNetworks/subnets**. **Microsoft.Network/virtualNetworks/** wird nicht angegeben, da dieser Teil vom übergeordneten Ressourcentyp übernommen wird.

Der Name der untergeordneten Ressource ist auf **Subnet1** festgelegt, der vollständige Name enthält jedoch auch den Namen der übergeordneten Ressource. **VNet1** wird nicht angegeben, da dieser Teil von der übergeordneten Ressource übernommen wird.

## <a name="outside-parent-resource"></a>Außerhalb der übergeordneten Ressource

Bei Definition außerhalb der übergeordneten Ressource formatieren Sie den Typ und Namen mit Schrägstrichen, um den übergeordneten Typ und Namen einzuschließen.

```json
"type": "{resource-provider-namespace}/{parent-resource-type}/{child-resource-type}",
"name": "{parent-resource-name}/{child-resource-name}",
```

Das folgende Beispiel zeigt ein virtuelles Netzwerk und ein Subnetz, die beide auf Stammebene definiert sind. Beachten Sie, dass das Subnetz nicht im Ressourcenarray für das virtuelle Netzwerk enthalten ist. Der Name ist auf **VNet1/Subnet1** und der Typ auf **Microsoft.Network/virtualNetworks/subnets** festgelegt. Die untergeordnete Ressource ist als von der übergeordneten Ressource abhängig gekennzeichnet, da die übergeordnete Ressource vorhanden sein muss, bevor die untergeordnete Ressource bereitgestellt werden kann.

```json
"resources": [
  {
    "apiVersion": "2018-10-01",
    "type": "Microsoft.Network/virtualNetworks",
    "name": "VNet1",
    "location": "[parameters('location')]",
    "properties": {
      "addressSpace": {
        "addressPrefixes": [
          "10.0.0.0/16"
        ]
      }
    }
  },
  {
    "apiVersion": "2018-10-01",
    "type": "Microsoft.Network/virtualNetworks/subnets",
    "location": "[parameters('location')]",
    "name": "VNet1/Subnet1",
    "dependsOn": [
      "VNet1"
    ],
    "properties": {
      "addressPrefix": "10.0.0.0/24"
    }
  }
]
```

## <a name="next-steps"></a>Nächste Schritte

* Weitere Informationen zum Erstellen von Azure-Ressourcen-Manager-Vorlagen finden Sie unter [Erstellen von Vorlagen](resource-group-authoring-templates.md). 

* Weitere Informationen zum Format des Ressourcennamens beim Verweisen auf die Ressource finden Sie unter der [Referenzfunktion](resource-group-template-functions-resource.md#reference).
