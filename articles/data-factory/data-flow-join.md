---
title: Join-Transformation in einem Azure Data Factory-Zuordnungsdatenfluss
description: Hier erfahren Sie, wie Sie Daten aus zwei Datenquellen mithilfe der Join-Transformation in einem Azure Data Factory-Zuordnungsdatenfluss miteinander kombinieren.
author: kromerm
ms.author: makromer
ms.reviewer: daperlov
ms.service: data-factory
ms.topic: conceptual
ms.date: 10/17/2019
ms.openlocfilehash: 4680804017a9b08248bb41ff999c6ba6371e99c8
ms.sourcegitcommit: 609d4bdb0467fd0af40e14a86eb40b9d03669ea1
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 11/06/2019
ms.locfileid: "73675909"
---
# <a name="join-transformation-in-mapping-data-flow"></a>Join-Transformation in einem Zuordnungsdatenfluss

Mithilfe der Join-Transformation können Sie Daten aus zwei Quellen oder Streams in einem Zuordnungsdatenfluss miteinander kombinieren. Der Ausgabestream enthält alle Spalten aus beiden Quellen, die auf der Grundlage einer Verknüpfungsbedingung abgeglichen wurden. 

## <a name="join-types"></a>Join-Typen

Von Zuordnungsdatenflüssen werden aktuell fünf verschiedene Jointypen unterstützt:

### <a name="inner-join"></a>Innerer Join

Bei einem inneren Join werden nur Zeilen ausgegeben, die übereinstimmende Werte in beiden Tabellen enthalten.

### <a name="left-outer"></a>Linker äußerer Join

Bei einem linken äußeren Join werden alle Zeilen aus dem linken Stream sowie übereinstimmende Datensätze aus dem rechten Stream zurückgegeben. Ist für eine Zeile aus dem linken Stream keine Übereinstimmung vorhanden, werden die Ausgabespalten aus dem rechten Stream auf NULL festgelegt. Die Ausgabe umfasst die von einem inneren Join zurückgegebenen Zeilen sowie die nicht übereinstimmenden Zeilen aus dem linken Stream.

### <a name="right-outer"></a>Rechter äußerer Join

Bei einem rechten äußeren Join werden alle Zeilen aus dem rechten Stream sowie übereinstimmende Datensätze aus dem linken Stream zurückgegeben. Ist für eine Zeile aus dem rechten Stream keine Übereinstimmung vorhanden, werden die Ausgabespalten aus dem linken Stream auf NULL festgelegt. Die Ausgabe umfasst die von einem inneren Join zurückgegebenen Zeilen sowie die nicht übereinstimmenden Zeilen aus dem rechten Stream.

### <a name="full-outer"></a>Vollständiger äußerer Join

Bei einem vollständigen äußeren Join werden alle Spalten und Zeilen beider Seiten zurückgegeben. Spalten ohne Übereinstimmung erhalten jeweils einen NULL-Wert.

### <a name="cross-join"></a>Cross Join

Bei einem Kreuzprodukt wird das Kreuzprodukt der beiden Streams auf der Grundlage einer Bedingung ausgegeben. Geben Sie bei Verwendung einer Ungleichheitsbedingung einen benutzerdefinierten Ausdruck als Kreuzproduktbedingung an. Der Ausgabestream umfasst alle Zeilen, die der Verknüpfungsbedingung entsprechen. Wenn Sie ein kartesisches Produkt erstellen möchten, das jede Zeilenkombination ausgibt, geben Sie `true()` als Verknüpfungsbedingung an.

## <a name="configuration"></a>Konfiguration

1. Wählen Sie in der Dropdownliste **Rechter Stream** den Datenstrom für die Verknüpfung aus.
1. Wählen Sie unter **Jointyp** den gewünschten Jointyp aus.
1. Wählen Sie die Schlüsselspalten aus, auf denen der Abgleich für Ihre Verknüpfungsbedingung basieren soll. Standardmäßig sucht der Datenfluss nach Übereinstimmung mit einer Spalte in jedem Datenstrom. Wenn der Vergleich auf einem berechneten Wert basieren soll, zeigen Sie mit dem Mauszeiger auf die Dropdownliste für die Spalte, und wählen Sie **Berechnete Spalte** aus.

![Join-Transformation](media/data-flow/join.png "Join")

## <a name="optimizing-join-performance"></a>Optimieren der Leistung beim Verknüpfen

Im Gegensatz zu „Merge Join“ in Tools wie SSIS ist die Join-Transformation kein obligatorischer Vorgang vom Typ „Merge Join“. Die Join-Schlüssel müssen nicht sortiert werden. Der Join-Vorgang erfolgt basierend auf dem optimalen Join-Vorgang in Spark (Broadcast- oder Map-Side-Join).

![Optimieren der Join-Transformation](media/data-flow/joinoptimize.png "Join-Optimierung")

Wenn mindestens einer der Datenströme in den Arbeitsspeicher des Workerknotens passt, können Sie die Leistung weiter optimieren, indem Sie auf der Registerkarte „Optimieren“ die Option **Broadcast** aktivieren. Darüber hinaus können Sie Ihre Daten im Join-Vorgang neu partitionieren, damit sie besser in den workerspezifischen Arbeitsspeicher passen.

## <a name="self-join"></a>Selbstverknüpfung

Wenn Sie einen Datenstrom mit sich selbst verknüpfen möchten, müssen Sie einen vorhandenen Stream per Auswahltransformation mit einem Alias versehen. Erstellen Sie einen neuen Branch, indem Sie neben einer Transformation auf das Pluszeichen klicken und **Neuer Branch** auswählen. Fügen Sie eine Auswahltransformation hinzu, um den ursprünglichen Stream mit einem Alias zu versehen. Fügen Sie eine Join-Transformation hinzu. Wählen Sie dabei den ursprünglichen Stream als **Linker Stream** und die Auswahltransformation als **Rechter Stream** aus.

![Selbstverknüpfung](media/data-flow/selfjoin.png "Selbstverknüpfung")

## <a name="testing-join-conditions"></a>Testen der Join-Bedingungen

Verwenden Sie einen kleinen Satz bekannter Daten, wenn Sie die Join-Transformationen mit Datenvorschau im Debugmodus testen. Bei der Stichprobenentnahme aus einem umfangreichen Dataset können Sie nicht vorhersagen, welche Zeilen und Schlüssel zu Testzwecken gelesen werden. Das Ergebnis ist nicht deterministisch. Das bedeutet, dass für Ihre Verknüpfungsbedingungen möglicherweise keine Übereinstimmungen zurückgegeben werden.

## <a name="data-flow-script"></a>Datenflussskript

### <a name="syntax"></a>Syntax

```
<leftStream>, <rightStream>
    join(
        <conditionalExpression>,
        joinType: { 'inner'> | 'outer' | 'left_outer' | 'right_outer' | 'cross' }
        broadcast: { 'none' | 'left' | 'right' | 'both' }
    ) ~> <joinTransformationName>
```

### <a name="inner-join-example"></a>Beispiel für einen inneren Join

Das folgende Beispiel ist eine Join-Transformation namens `JoinMatchedData` mit dem linken Stream `TripData` und dem rechten Stream `TripFare`.  Die Verknüpfungsbedingung ist der Ausdruck `hack_license == { hack_license} && TripData@medallion == TripFare@medallion && vendor_id == { vendor_id} && pickup_datetime == { pickup_datetime}`, der „true“ zurückgibt, wenn die Spalten `hack_license`, `medallion`, `vendor_id` und `pickup_datetime` in den beiden Streams übereinstimmen. Der Jointyp (`joinType`) lautet `'inner'`. Da Broadcasting nur im linken Stream aktiviert wird, hat `broadcast` den Wert `'left'`.

Auf der Data Factory-Benutzeroberfläche sieht diese Transformation wie folgt aus:

![Join-Beispiel](media/data-flow/join-script1.png "Join-Beispiel")

Das Datenflussskript für diese Transformation befindet sich im folgenden Codeausschnitt:

```
TripData, TripFare
    join(
        hack_license == { hack_license}
        && TripData@medallion == TripFare@medallion
        && vendor_id == { vendor_id}
        && pickup_datetime == { pickup_datetime},
        joinType:'inner',
        broadcast: 'left'
    )~> JoinMatchedData
```

### <a name="cross-join-example"></a>Beispiel für ein Kreuzprodukt

Das folgende Beispiel ist eine Join-Transformation namens `CartesianProduct` mit dem linken Stream `TripData` und dem rechten Stream `TripFare`. Diese Transformation akzeptiert zwei Streams und gibt ein kartesisches Produkt ihrer Zeilen zurück. Die Verknüpfungsbedingung ist `true()`, da sie ein vollständiges kartesisches Produkt ausgibt. Der Jointyp (`joinType`) lautet `cross`. Da Broadcasting nur im linken Stream aktiviert wird, hat `broadcast` den Wert `'left'`.

Auf der Data Factory-Benutzeroberfläche sieht diese Transformation wie folgt aus:

![Join-Beispiel](media/data-flow/join-script2.png "Join-Beispiel")

Das Datenflussskript für diese Transformation befindet sich im folgenden Codeausschnitt:

```
TripData, TripFare
    join(
        true(),
        joinType:'cross',
        broadcast: 'left'
    )~> CartesianProduct
```

## <a name="next-steps"></a>Nächste Schritte

Erstellen Sie nach dem Verknüpfen von Daten eine [abgeleitete Spalte](data-flow-derived-column.md), und [senken](data-flow-sink.md) Sie Ihre Daten in einen Zieldatenspeicher.
