---
title: Benutzeroberflächenelement „TagsByResource“ von Azure
description: Hier wird das Benutzeroberflächenelement „Microsoft.Common.TagsByResource“ für das Azure-Portal beschrieben. Verwenden Sie dies, um während der Bereitstellung Tags auf eine Ressource anzuwenden.
author: tfitzmac
ms.service: managed-applications
ms.topic: reference
ms.date: 11/11/2019
ms.author: tomfitz
ms.openlocfilehash: 5711759666cd68dc93e5bfb67e51ec1cc0a48bd1
ms.sourcegitcommit: a10074461cf112a00fec7e14ba700435173cd3ef
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 11/12/2019
ms.locfileid: "73933116"
---
# <a name="microsoftcommontagsbyresource-ui-element"></a>Benutzeroberflächenelement „Microsoft.Common.TagsByResource“

Ein Steuerelement, um den Ressourcen in einer Bereitstellung [Tags](../azure-resource-manager/resource-group-using-tags.md) zuzuordnen.

## <a name="ui-sample"></a>Benutzeroberflächenbeispiel

![Microsoft.Common.DropDown](./media/managed-application-elements/microsoft.common.tagsbyresource.png)

## <a name="schema"></a>Schema

```json
{
  "name": "element1",
  "type": "Microsoft.Common.TagsByResource",
  "resources": [
    "Microsoft.Storage/storageAccounts",
    "Microsoft.Compute/virtualMachines"
  ]
}
```

## <a name="sample-output"></a>Beispielausgabe

```json
{
  "Microsoft.Storage/storageAccounts": {
    "Dept": "Finance",
    "Environment": "Production"
  },
  "Microsoft.Compute/virtualMachines": {
    "Dept": "Finance"
  }
}
```

## <a name="remarks"></a>Anmerkungen

- Mindestens ein Element im `resources`-Array muss angegeben sein.
- Jedes Element in `resources` muss ein vollqualifizierter Ressourcentyp sein. Diese Elemente werden in der Dropdownliste **Ressourcen** angezeigt und können vom Benutzer mit Tags versehen werden.
- Die Ausgabe des Steuerelements wird für die einfache Zuweisung von Tagwerten in einer Azure Resource Manager-Vorlage formatiert. Um die Ausgabe des Steuerelements in einer Vorlage zu empfangen, fügen Sie einen Parameter in Ihre Vorlage ein, wie im folgenden Beispiel gezeigt:

  ```json
  "parameters": {
    "tagsByResource": { "type": "object", "defaultValue": {} }
  }
  ```

  Weisen Sie für jede markierbare Ressource die Tags-Eigenschaft dem Parameterwert für diesen Ressourcentyp zu:

  ```json
  {
    "name": "saName1",
    "type": "Microsoft.Storage/storageAccounts",
    "tags": "[ if(contains(parameters('tagsByResource'), 'Microsoft.Storage/storageAccounts'), parameters('tagsByResource')['Microsoft.Storage/storageAccounts'], json('{}')) ]",
    ...
  ```

- Verwenden Sie die [if](../azure-resource-manager/resource-group-template-functions-logical.md#if)-Funktion, wenn Sie auf den „tagsByResource“-Parameter zugreifen. Dies ermöglicht Ihnen, ein leeres Objekt zuzuweisen, wenn dem angegebenen Ressourcentyp keine Tags zugewiesen sind.

## <a name="next-steps"></a>Nächste Schritte

- Eine Einführung zum Erstellen von Benutzeroberflächendefinitionen finden Sie unter [Erste Schritte mit „CreateUiDefinition“](create-uidefinition-overview.md).
- Eine Beschreibung der allgemeinen Eigenschaften in Benutzeroberflächenelementen finden Sie unter [CreateUiDefinition-Elemente](create-uidefinition-elements.md).
