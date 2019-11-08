---
title: Aktivität „Variable festlegen“ in Azure Data Factory
description: Erfahren Sie, wie Sie die Aktivität „Variable festlegen“ zum Festlegen des Werts einer vorhandenen Variablen verwenden, die in einer Data Factory-Pipeline definiert ist.
services: data-factory
documentationcenter: ''
ms.service: data-factory
ms.workload: data-services
ms.tgt_pltfrm: na
ms.topic: conceptual
ms.date: 10/10/2018
author: djpmsft
ms.author: daperlov
manager: jroth
ms.reviewer: maghan
ms.openlocfilehash: 2589e56d3170b235f31d07f902eb75c749e8573f
ms.sourcegitcommit: 609d4bdb0467fd0af40e14a86eb40b9d03669ea1
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 11/06/2019
ms.locfileid: "73679301"
---
# <a name="set-variable-activity-in-azure-data-factory"></a>Aktivität „Variable festlegen“ in Azure Data Factory

Verwenden Sie die Aktivität „Variable festlegen“, um den Wert einer vorhandenen Variablen vom Typ „String“, „Bool“ oder „Array“ festzulegen, die in einer Data Factory-Pipeline definiert ist.

## <a name="type-properties"></a>Typeigenschaften

Eigenschaft | BESCHREIBUNG | Erforderlich
-------- | ----------- | --------
name | Der Name der Aktivität in der Pipeline. | Ja
description | Text, der beschreibt, welche Aktion die Aktivität ausführt. | no
type | Der Aktivitätstyp ist SetVariable. | Ja
value | Ein Zeichenfolgenliteral- oder Ausdrucksobjektwert, der zum Festlegen der angegebenen Variable verwendet wird. | Ja
variableName | Der Name der Variablen, die von dieser Aktivität festgelegt wird. | Ja


## <a name="next-steps"></a>Nächste Schritte
Machen Sie sich mit einer verwandten Ablaufsteuerungsaktivität vertraut, die von Data Factory unterstützt wird: 

- [Aktivität „Variable anfügen“](control-flow-append-variable-activity.md)
