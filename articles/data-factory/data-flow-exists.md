---
title: Exists-Transformation in Azure Data Factory Mapping Data Flow | Microsoft-Dokumentation
description: Prüfen auf vorhandene Zeilen mithilfe der Exists-Transformation in Azure Data Factory Mapping Data Flow
author: kromerm
ms.author: makromer
ms.reviewer: daperlov
ms.service: data-factory
ms.topic: conceptual
ms.date: 10/16/2019
ms.openlocfilehash: bfc2a810d34f03fc0f10c486344c6dccec548305
ms.sourcegitcommit: 12de9c927bc63868168056c39ccaa16d44cdc646
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 10/17/2019
ms.locfileid: "72515133"
---
# <a name="mapping-data-flow-exists-transformation"></a>Mapping Data Flow: Exists-Transformation

Die Exists-Transformation ist eine Zeilenfilterungstransformation, mit der geprüft wird, ob Ihre Daten in einer anderen Quelle oder einem anderen Datenstrom vorhanden sind. Der Ausgabedatenstrom enthält alle Zeilen im linken Datenstrom, die im rechten Datenstrom entweder vorhanden oder nicht vorhanden sind. Die Exists-Transformation ist mit ```SQL WHERE EXISTS``` und ```SQL WHERE NOT EXISTS``` vergleichbar.

## <a name="configuration"></a>Konfiguration

Wählen Sie in der Dropdownliste **Rechter Datenstrom** den Datenstrom aus, den Sie auf Vorhandensein prüfen möchten.

Geben Sie in der Einstellung **Exist-Typ** an, ob das Vorhandensein oder das Nichtvorhandensein der Daten geprüft werden soll.

Wählen Sie die Schlüsselspalten aus, die Sie als Exists-Bedingungen vergleichen möchten. Standardmäßig sucht der Datenfluss nach Übereinstimmung mit einer Spalte in jedem Datenstrom. Für den Vergleich mittels eines Computewerts zeigen Sie mit dem Mauszeiger auf die Dropdownliste für die Spalte, und wählen Sie **Berechnete Spalte** aus.

![Exists-Einstellungen](media/data-flow/exists.png "Exists-Ausdruck 1")

### <a name="multiple-exists-conditions"></a>Mehrere Exists-Bedingungen

Wenn Sie mehrere Spalten aus jedem Datenstrom vergleichen möchten, fügen Sie eine neue Exists-Bedingung hinzu, indem Sie auf das Pluszeichen neben einer vorhandenen Zeile klicken. Jede weitere Bedingung wird durch eine „und“-Anweisung verknüpft. Der Vergleich von zwei Spalten entspricht dem folgenden Ausdruck:

`source1@column1 == source2@column1 && source1@column2 == source2@column2`

### <a name="custom-expression"></a>Benutzerdefinierter Ausdruck

Zum Erstellen eines Freiformausdrucks, der andere Operatoren als „und“ und „gleich“ enthält, wählen Sie das Feld **Benutzerdefinierter Ausdruck** aus. Geben Sie einen benutzerdefinierten Ausdruck über den Datenfluss-Ausdrucks-Generator ein, indem Sie auf das blaue Feld klicken.

![Benutzerdefinierte Exists-Einstellungen](media/data-flow/exists1.png "Benutzerdefinierter Exists-Ausdruck")

## <a name="data-flow-script"></a>Datenflussskript

### <a name="syntax"></a>Syntax

```
<lefttream>, <rightStream>
    exists(
        <conditionalExpression>,
        negate: true | <false>,
        broadcast: 'none' | 'left' | 'right' | 'both'
    ) ~> <existsTransformationName>
```

### <a name="example"></a>Beispiel

Das folgende Beispiel ist eine Exists-Transformation mit dem Namen `checkForChanges`, die den linken Datenstrom `NameNorm2` und den rechten Datenstrom `TypeConversions` verwendet.  Die Exists-Bedingung ist der Ausdruck `NameNorm2@EmpID == TypeConversions@EmpID && NameNorm2@Region == DimEmployees@Region`, der „true“ zurückgibt, wenn sowohl die Spalte `EMPID` als auch die Spalte `Region` in den beiden Datenströmen übereinstimmen. Da auf Vorhandensein geprüft wird, entspricht `negate` dem Wert „false“. Wir aktivieren auf der Registerkarte „Optimieren“ keine Übertragung, sodass `broadcast` den Wert `'none'` hat.

Auf der Data Factory-Benutzeroberfläche sieht diese Transformation wie folgt aus:

![Exists-Beispiel](media/data-flow/exists-script.png "Exists-Beispiel")

Das Datenflussskript für diese Transformation befindet sich im folgenden Codeausschnitt:

```
NameNorm2, TypeConversions
    exists(
        NameNorm2@EmpID == TypeConversions@EmpID && NameNorm2@Region == DimEmployees@Region,
        negate:false,
        broadcast: 'none'
    ) ~> checkForChanges
```

## <a name="next-steps"></a>Nächste Schritte

Ähnliche Transformationen sind [Lookup](data-flow-lookup.md) und [Join](data-flow-join.md).
