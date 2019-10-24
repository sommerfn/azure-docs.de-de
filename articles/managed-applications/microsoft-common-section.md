---
title: Benutzeroberflächenelement „Section“ in Azure | Microsoft-Dokumentation
description: Hier wird das Benutzeroberflächenelement „Microsoft.Common.Section“ für das Azure-Portal beschrieben. Es wird zum Gruppieren von Elementen im Portal für die Bereitstellung verwalteter Anwendungen verwendet.
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
ms.openlocfilehash: a48c89785e0a448609026aab53364f6cf704e948
ms.sourcegitcommit: 1d0b37e2e32aad35cc012ba36200389e65b75c21
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 10/15/2019
ms.locfileid: "72331650"
---
# <a name="microsoftcommonsection-ui-element"></a>Benutzerobeflächenelement „Microsoft.Common.Section“
Ein Steuerelement, das Elemente unter einer Überschrift zusammenfasst.

## <a name="ui-sample"></a>Benutzeroberflächenbeispiel
![Microsoft.Common.Section](./media/managed-application-elements/microsoft.common.section.png)

## <a name="schema"></a>Schema
```json
{
  "name": "section1",
  "type": "Microsoft.Common.Section",
  "label": "Example section",
  "elements": [
    {
      "name": "text1",
      "type": "Microsoft.Common.TextBox",
      "label": "Example text box 1"
    },
    {
      "name": "text2",
      "type": "Microsoft.Common.TextBox",
      "label": "Example text box 2"
    }
  ],
  "visible": true
}
```

## <a name="remarks"></a>Anmerkungen
- `elements` muss mindestens ein Element und darf alle Elementtypen außer `Microsoft.Common.Section` enthalten.
- Dieses Element unterstützt die `toolTip`-Eigenschaft nicht.

## <a name="sample-output"></a>Beispielausgabe
Verwenden Sie zum Zugreifen auf die Ausgabewerte von Elementen in `elements` die Funktion [basics()](create-uidefinition-functions.md#basics) oder [steps()](create-uidefinition-functions.md#steps) und die Punktnotation:

```json
steps('configuration').section1.text1
```

Elemente des Typs `Microsoft.Common.Section` haben selbst keine Ausgabewerte.

## <a name="next-steps"></a>Nächste Schritte
* Eine Einführung zum Erstellen von Benutzeroberflächendefinitionen finden Sie unter [Erste Schritte mit „CreateUiDefinition“](create-uidefinition-overview.md).
* Eine Beschreibung der allgemeinen Eigenschaften in Benutzeroberflächenelementen finden Sie unter [CreateUiDefinition-Elemente](create-uidefinition-elements.md).
