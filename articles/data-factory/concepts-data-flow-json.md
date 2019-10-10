---
title: JSON-Konzepte für Azure Data Factory Mapping Data Flow
description: Data Factory Mapping Data Flow verfügt über integrierte Funktionen für die Verarbeitung von JSON-Dokumenten mit Hierarchien.
author: kromerm
ms.author: makromer
ms.service: data-factory
ms.topic: conceptual
ms.date: 08/30/2019
ms.openlocfilehash: 37db3e153e8dfcbc1120fcb1f6d2f77187edc78e
ms.sourcegitcommit: 11265f4ff9f8e727a0cbf2af20a8057f5923ccda
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 10/08/2019
ms.locfileid: "72029666"
---
# <a name="mapping-data-flow-json-handling"></a>JSON-Verarbeitung mit Mapping Data Flow



## <a name="creating-json-structures-in-expression-editor"></a>Erstellen von JSON-Strukturen im Ausdrucks-Editor
### <a name="derived-column-transformation"></a>Transformation für abgeleitete Spalten
Das Hinzufügen einer komplexen Spalte zum Datenfluss kann über den Ausdrucks-Editor für abgeleitete Spalten einfacher vorgenommen werden. Nachdem Sie eine neue Spalte hinzugefügt und den Editor geöffnet haben, stehen zwei Möglichkeiten zur Verfügung: Sie können die JSON-Struktur manuell eingeben oder die Benutzeroberfläche verwenden, um Unterspalten interaktiv hinzuzufügen.

#### <a name="interactive-ui-json-design"></a>Interaktiver JSON-Entwurf über die Benutzeroberfläche
Im Seitenbereich des Ausgabeschemas können über das `+`-Menü neue Unterspalten hinzugefügt werden: ![Unterspalte hinzufügen](media/data-flow/addsubcolumn.png "Unterspalte hinzufügen")

Von dort aus können neue Spalten und Unterspalten auf die gleiche Weise hinzugefügt werden. Im Ausdrucks-Editor auf der rechten Seite kann für jedes nicht komplexe Feld ein Ausdruck hinzugefügt werden.

![Komplexe Spalte](media/data-flow/complexcolumn.png "Komplexe Spalte")

#### <a name="manual-json-design"></a>Manueller JSON-Entwurf
Zum manuellen Hinzufügen einer JSON-Struktur fügen Sie eine neue Spalte hinzu, und geben Sie den Ausdruck im Editor ein. Der Ausdruck weist das folgende allgemeine Format auf:
```
@(
    field1=0,
    field2=@(
        field1=0
    )
)
```
Wenn dieser Ausdruck für eine Spalte mit dem Namen „complexColumn“ eingegeben wird, wird er als der folgende JSON-Code in die Senke geschrieben:
```
{
    "complexColumn": {
        "field1": 0,
        "field2": {
            "field1": 0
        }
    }
}
```

#### <a name="sample-manual-script-for-complete-hierarchical-definition"></a>Beispiel für ein manuelles Skript für eine vollständige hierarchische Definition
```
@(
    title=Title,
    firstName=FirstName,
    middleName=MiddleName,
    lastName=LastName,
    suffix=Suffix,
    contactDetails=@(
        email=EmailAddress,
        phone=Phone
    ),
    address=@(
        line1=AddressLine1,
        line2=AddressLine2,
        city=City,
        state=StateProvince,
        country=CountryRegion,
        postCode=PostalCode
    ),
    ids=[
        toString(CustomerID), toString(AddressID), rowguid
    ]
)
```

## <a name="source-format-options"></a>Quellformatoptionen
### <a name="default"></a>Standard
```
{ "json": "record 1" }
{ "json": "record 2" }
{ "json": "record 3" }
```

### <a name="single-document"></a>Einzelnes Dokument
* Option 1
```
[
    {
        "json": "record 1"
    },
    {
        "json": "record 2"
    },
    {
        "json": "record 3"
    }
]
```

* Option 2
```
File1.json
{
    "json": "record 1"
}
File2.json
{
    "json": "record 2"
}
File3.json
{
    "json": "record 3"
}
```

### <a name="unquoted-column-names"></a>Spaltennamen ohne Anführungszeichen
```
{ json: "record 1" }
{ json: "record 2" }
{ json: "record 3" }
```

### <a name="has-comments"></a>Mit Kommentaren
```
{ "json": /** comment **/ "record 1" }
{ "json": "record 2" }
{ /** comment **/ "json": "record 3" }
```

### <a name="single-quoted"></a>Einfache Anführungszeichen
```
{ 'json': 'record 1' }
{ 'json': 'record 2' }
{ 'json': 'record 3' }
```

### <a name="backslash-escaped"></a>Umgekehrter Schrägstrich mit Escapezeichen
```
{ "json": "record 1" }
{ "json": "\} \" \' \\ \n \\n record 2" }
{ "json": "record 3" }
```

## <a name="higher-order-functions"></a>Funktionen höherer Ordnung
## <a name="filter"></a>filter
Filtert Elemente aus dem Array heraus, die dem bereitgestellten Prädikat nicht entsprechen. Die filter-Funktion erwartet einen Verweis auf ein Element in der Prädikatfunktion als #item.

### <a name="examples"></a>Beispiele
```
filter([1, 2, 3, 4], #item > 2) => [3, 4]
filter(['a', 'b', 'c', 'd'], #item == 'a' || #item == 'b') => ['a', 'b']
```

## <a name="map"></a>map
Ordnet anhand des bereitgestellten Ausdrucks jedes Element des Arrays einem neuen Element zu. Die map-Funktion erwartet einen Verweis auf ein Element in der Ausdrucksfunktion als #item.

### <a name="examples"></a>Beispiele
```
map([1, 2, 3, 4], #item + 2) => [3, 4, 5, 6]
map(['a', 'b', 'c', 'd'], #item + '_processed') => ['a_processed', 'b_processed', 'c_processed', 'd_processed']
```

## <a name="reduce"></a>reduce
Akkumuliert Elemente in einem Array. Die reduce-Funktion erwartet einen Verweis auf einen Akkumulator und ein Element in der ersten Ausdrucksfunktion als #acc und #item. Außerdem wird erwartet, dass der resultierende Wert als #result in der zweiten Ausdrucksfunktion verwendet wird.

### <a name="examples"></a>Beispiele
```
reduce([1, 2, 3, 4], 0, #acc + #item, #result) => 10
reduce(['1', '2', '3', '4'], '0', #acc + #item, #result) => '01234'
reduce([1, 2, 3, 4], 0, #acc + #item, #result + 15) => 25
```

## <a name="sort"></a>sort
Sortiert das Array mithilfe der bereitgestellten Prädikatfunktion. Die sort-Funktion erwartet einen Verweis auf zwei aufeinander folgende Elemente in der Ausdrucksfunktion als #item1 und #item2.

### <a name="examples"></a>Beispiele
```
sort([4, 8, 2, 3], compare(#item1, #item2)) => [2, 3, 4, 8]
sort(['a3', 'b2', 'c1'],
        iif(right(#item1, 1) >= right(#item2, 1), 1, -1)) => ['c1', 'b2', 'a3']
sort(['a3', 'b2', 'c1'],
        iif(#item1 >= #item2, 1, -1)) => ['a3', 'b2', 'c1']
```

## <a name="contains"></a>contains
Gibt TRUE zurück, wenn ein beliebiges Element im angegebenen Array als TRUE im bereitgestellten Prädikat ausgewertet wird. Die contains-Funktion erwartet einen Verweis auf ein Element in der Prädikatfunktion als #item.

### <a name="examples"></a>Beispiele
```
contains([1, 2, 3, 4], #item == 3) => true
contains([1, 2, 3, 4], #item > 5) => false
```

## <a name="next-steps"></a>Nächste Schritte

* [Verwenden der Transformation für abgeleitete Spalten zum Erstellen hierarchischer Strukturen](data-flow-derived-column.md)
