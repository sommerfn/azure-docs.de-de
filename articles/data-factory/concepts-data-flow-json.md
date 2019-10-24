---
title: Verwenden von JSON in Mapping Data Flow in Azure Data Factory
description: Azure Data Factory Mapping Data Flow verfügt über integrierte Funktionen für die Verarbeitung von JSON-Dokumenten mit Hierarchien.
author: kromerm
ms.author: makromer
ms.review: djpmsft
ms.service: data-factory
ms.topic: conceptual
ms.date: 08/30/2019
ms.openlocfilehash: fe412e9e682fb55f1664c546e6b6c5a347527adb
ms.sourcegitcommit: e0e6663a2d6672a9d916d64d14d63633934d2952
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 10/21/2019
ms.locfileid: "72387359"
---
# <a name="mapping-data-flow-json-handling"></a>JSON-Verarbeitung mit Mapping Data Flow

## <a name="creating-json-structures-in-derived-column"></a>Erstellen von JSON-Strukturen in abgeleiteten Spalten

Sie können Ihrem Datenfluss eine komplexe Spalte über den Ausdrucks-Editor für abgeleitete Spalten hinzufügen. Fügen Sie in der Transformation für abgeleitete Spalten eine neue Spalte hinzu, und öffnen Sie den Ausdrucks-Generator, indem Sie auf das blaue Feld klicken. Um eine Spalte komplex zu gestalten, können Sie die JSON-Struktur manuell eingeben oder die Benutzeroberfläche verwenden, um Unterspalten interaktiv hinzuzufügen.

### <a name="using-the-expression-builder-ux"></a>Verwenden der Benutzeroberfläche des Ausdrucks-Generators

Zeigen Sie im Seitenbereich des Ausgabeschemas mit dem Mauszeiger auf eine Spalte, und klicken Sie auf das Pluszeichen. Wählen Sie **Unterspalte hinzufügen** aus, um die Spalte als komplexen Typ zu gestalten.

![Unterspalte hinzufügen](media/data-flow/addsubcolumn.png "Unterspalte hinzufügen")

Sie können weitere Spalten und Unterspalten auf die gleiche Weise hinzufügen. Im Ausdrucks-Editor auf der rechten Seite kann für jedes nicht komplexe Feld ein Ausdruck hinzugefügt werden.

![Komplexe Spalte](media/data-flow/complexcolumn.png "Komplexe Spalte")

### <a name="entering-the-json-structure-manually"></a>Manuelles Eingeben der JSON-Struktur

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

Wenn Sie ein JSON-Dataset als Quelle in Ihrem Datenfluss verwenden, können Sie fünf weitere Einstellungen festlegen. Diese Einstellungen finden Sie auf der Registerkarte **Quelloptionen**  im Accordion-Element **JSON-Einstellungen**.  

![JSON-Einstellungen](media/data-flow/json-settings.png "JSON-Einstellungen")

### <a name="default"></a>Standard

JSON-Daten werden standardmäßig im folgenden Format gelesen.

```
{ "json": "record 1" }
{ "json": "record 2" }
{ "json": "record 3" }
```

### <a name="single-document"></a>Einzelnes Dokument

Wenn **Einzelnes Dokument** ausgewählt ist, liest Mapping Data Flow ein einzelnes JSON-Dokument aus jeder Datei. 

``` json
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

Wenn **Spaltennamen ohne Anführungszeichen** ausgewählt ist, liest Mapping Data Flow JSON-Spalten, die nicht in Anführungszeichen eingeschlossen sind. 

```
{ json: "record 1" }
{ json: "record 2" }
{ json: "record 3" }
```

### <a name="has-comments"></a>Mit Kommentaren

Wählen Sie **Mit Kommentaren** aus, wenn die JSON-Daten Kommentare im C- oder C++-Stil enthalten.

``` json
{ "json": /** comment **/ "record 1" }
{ "json": "record 2" }
{ /** comment **/ "json": "record 3" }
```

### <a name="single-quoted"></a>Einfache Anführungszeichen

Wählen Sie **Einfache Anführungszeichen** aus, wenn für JSON-Felder und -Werte einfache Anführungszeichen anstelle von doppelten Anführungszeichen verwendet werden.

```
{ 'json': 'record 1' }
{ 'json': 'record 2' }
{ 'json': 'record 3' }
```

### <a name="backslash-escaped"></a>Umgekehrter Schrägstrich mit Escapezeichen

Wählen Sie **Einfache Anführungszeichen** aus, wenn umgekehrte Schrägstriche für Escapezeichen in den JSON-Daten verwendet werden.

```
{ "json": "record 1" }
{ "json": "\} \" \' \\ \n \\n record 2" }
{ "json": "record 3" }
```

## <a name="higher-order-functions"></a>Funktionen höherer Ordnung

Eine Funktion höherer Ordnung ist eine Funktion, die eine oder mehrere Funktionen als Argument annimmt. Es folgt eine Liste der Funktionen höherer Ordnung, die in Mapping Data Flow unterstützt werden und Arrayvorgänge ermöglichen.

### <a name="filter"></a>filter
Filtert Elemente aus dem Array heraus, die dem bereitgestellten Prädikat nicht entsprechen. Die filter-Funktion erwartet einen Verweis auf ein Element in der Prädikatfunktion als #item.

#### <a name="examples"></a>Beispiele
```
filter([1, 2, 3, 4], #item > 2) => [3, 4]
filter(['a', 'b', 'c', 'd'], #item == 'a' || #item == 'b') => ['a', 'b']
```

### <a name="map"></a>map
Ordnet anhand des bereitgestellten Ausdrucks jedes Element des Arrays einem neuen Element zu. Die map-Funktion erwartet einen Verweis auf ein Element in der Ausdrucksfunktion als #item.

#### <a name="examples"></a>Beispiele
```
map([1, 2, 3, 4], #item + 2) => [3, 4, 5, 6]
map(['a', 'b', 'c', 'd'], #item + '_processed') => ['a_processed', 'b_processed', 'c_processed', 'd_processed']
```

### <a name="reduce"></a>reduce
Akkumuliert Elemente in einem Array. Die reduce-Funktion erwartet einen Verweis auf einen Akkumulator und ein Element in der ersten Ausdrucksfunktion als #acc und #item. Außerdem wird erwartet, dass der resultierende Wert als #result in der zweiten Ausdrucksfunktion verwendet wird.

#### <a name="examples"></a>Beispiele
```
reduce([1, 2, 3, 4], 0, #acc + #item, #result) => 10
reduce(['1', '2', '3', '4'], '0', #acc + #item, #result) => '01234'
reduce([1, 2, 3, 4], 0, #acc + #item, #result + 15) => 25
```

### <a name="sort"></a>sort
Sortiert das Array mithilfe der bereitgestellten Prädikatfunktion. Die sort-Funktion erwartet einen Verweis auf zwei aufeinander folgende Elemente in der Ausdrucksfunktion als #item1 und #item2.

#### <a name="examples"></a>Beispiele
```
sort([4, 8, 2, 3], compare(#item1, #item2)) => [2, 3, 4, 8]
sort(['a3', 'b2', 'c1'],
        iif(right(#item1, 1) >= right(#item2, 1), 1, -1)) => ['c1', 'b2', 'a3']
sort(['a3', 'b2', 'c1'],
        iif(#item1 >= #item2, 1, -1)) => ['a3', 'b2', 'c1']
```

### <a name="contains"></a>contains
Gibt TRUE zurück, wenn ein beliebiges Element im angegebenen Array als TRUE im bereitgestellten Prädikat ausgewertet wird. Die contains-Funktion erwartet einen Verweis auf ein Element in der Prädikatfunktion als #item.

#### <a name="examples"></a>Beispiele
```
contains([1, 2, 3, 4], #item == 3) => true
contains([1, 2, 3, 4], #item > 5) => false
```

## <a name="next-steps"></a>Nächste Schritte

* [Verwenden der Transformation für abgeleitete Spalten zum Erstellen hierarchischer Strukturen](data-flow-derived-column.md)
