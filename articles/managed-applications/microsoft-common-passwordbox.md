---
title: Benutzeroberflächenelement PasswordBox in Azure | Microsoft-Dokumentation
description: Hier wird das Benutzeroberflächenelement Microsoft.Common.PasswordBox für das Azure-Portal beschrieben. Es ermöglicht Benutzern die Angabe eines Geheimniswerts beim Bereitstellen von verwalteten Anwendungen.
services: managed-applications
documentationcenter: na
author: tfitzmac
manager: timlt
editor: tysonn
ms.service: managed-applications
ms.devlang: na
ms.topic: reference
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 06/27/2018
ms.author: tomfitz
ms.openlocfilehash: 083b0be20d4e9ad7769a9c673139a361fb7b70d4
ms.sourcegitcommit: ae8b23ab3488a2bbbf4c7ad49e285352f2d67a68
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 11/13/2019
ms.locfileid: "74009167"
---
# <a name="microsoftcommonpasswordbox-ui-element"></a>Benutzeroberflächenelement „Microsoft.Common.PasswordBox“
Ein Steuerelement, das zum Angeben und Bestätigen eines Kennworts verwendet werden kann.

## <a name="ui-sample"></a>Benutzeroberflächenbeispiel
![Microsoft.Common.PasswordBox](./media/managed-application-elements/microsoft.common.passwordbox.png)

## <a name="schema"></a>Schema
```json
{
  "name": "element1",
  "type": "Microsoft.Common.PasswordBox",
  "label": {
    "password": "Password",
    "confirmPassword": "Confirm password"
  },
  "toolTip": "",
  "constraints": {
    "required": true,
    "regex": "^[a-zA-Z0-9]{8,}$",
    "validationMessage": "Password must be at least 8 characters long, contain only numbers and letters"
  },
  "options": {
    "hideConfirmation": false
  },
  "visible": true
}
```

## <a name="remarks"></a>Anmerkungen
- Dieses Element unterstützt die `defaultValue`-Eigenschaft nicht.
- Implementierungsdetails zu `constraints` finden Sie unter [Microsoft.Common.TextBox](microsoft-common-textbox.md).
- Wenn `options.hideConfirmation` auf **true** festgelegt ist, wird das zweite Textfeld zum Bestätigen des Benutzerkennworts ausgeblendet. Der Standardwert ist **false**.

## <a name="sample-output"></a>Beispielausgabe
```json
"p4ssw0rd"
```

## <a name="next-steps"></a>Nächste Schritte
* Eine Einführung zum Erstellen von Benutzeroberflächendefinitionen finden Sie unter [Erste Schritte mit „CreateUiDefinition“](create-uidefinition-overview.md).
* Eine Beschreibung der allgemeinen Eigenschaften in Benutzeroberflächenelementen finden Sie unter [CreateUiDefinition-Elemente](create-uidefinition-elements.md).
