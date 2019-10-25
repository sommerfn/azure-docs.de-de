---
title: Azure Data Factory-Mappingdatenfluss – Ausdrucks-Generator
description: Der Ausdrucks-Generator für Azure Data Factory-Mappingdatenflüsse
author: kromerm
ms.author: makromer
ms.service: data-factory
ms.topic: conceptual
ms.date: 09/30/2019
ms.openlocfilehash: 872c7ce6a0c39ab19165a5f16ea3e4f6ef8bd6a5
ms.sourcegitcommit: bb65043d5e49b8af94bba0e96c36796987f5a2be
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 10/16/2019
ms.locfileid: "72388050"
---
# <a name="mapping-data-flow-expression-builder"></a>Mappingdatenfluss: Ausdrucks-Generator



In Azure Data Factory-Mappingdatenflüssen stehen Ausdrucksfelder zur Verfügung, in die Sie Ausdrücke für die Datentransformation eingeben können. In diesen Feldern können Sie Spalten, Felder, Variablen, Parameter und Funktionen Ihres Datenflusses verwenden. Zum Erstellen des Ausdrucks verwenden Sie den Ausdrucks-Generator, der gestartet wird, wenn Sie in der Transformation auf das Ausdruckstextfeld klicken. Manchmal werden auch Optionen für „Berechnete Spalte“ angezeigt, wenn Sie Spalten für die Transformation auswählen. Wenn Sie darauf klicken, wird der Ausdrucks-Generator ebenfalls gestartet.

![Ausdrucks-Generator](media/data-flow/xpb1.png "Ausdrucks-Generator")

Der Ausdrucks-Generator verwendet standardmäßig die Text-Editor-Option. Die Funktion „AutoVervollständigen“ liest aus dem gesamten Azure Data Factory-Datenfluss-Objektmodell mit Syntaxprüfung und -hervorhebungen.

![Ausdrucks-Generator: AutoVervollständigen](media/data-flow/expb1.png "Ausdrucks-Generator: AutoVervollständigen")

## <a name="build-schemas-in-output-schema-pane"></a>Erstellen von Schemas im Ausgabeschemabereich

![Hinzufügen einer komplexen Spalte](media/data-flow/complexcolumn.png "Hinzufügen von Spalten")

Im Ausgabeschemabereich auf der linken Seite werden die Spalten angezeigt, die Sie ändern und dem Schema hinzufügen. Hier können Sie auf interaktive Weise einfache und komplexe Datenstrukturen erstellen. Mit „Spalte hinzufügen“ können Sie zusätzliche Felder hinzufügen und mit „Unterspalte hinzufügen“ Hierarchien erstellen.

![Hinzufügen einer Unterspalte](media/data-flow/addsubcolumn.png "Hinzufügen einer Unterspalte")

## <a name="data-preview-in-debug-mode"></a>Datenvorschau im Debugmodus

![Ausdrucks-Generator](media/data-flow/exp4b.png "Ausdrucksdatenvorschau")

Wenn Sie mit Datenflussausdrücken arbeiten, können Sie über die Entwurfsoberfläche für Azure Data Factory-Datenflüsse den Debugmodus aktivieren, um eine Livevorschau der Datenergebnisse aus dem von Ihnen erstellten Ausdruck zu erhalten. Für Ihre Ausdrücke ist Livedebugging in Echtzeit aktiviert.

![Debugmodus](media/data-flow/debugbutton.png "Schaltfläche „Debuggen“")

Klicken Sie auf die Schaltfläche „Aktualisieren“, um die Ergebnisse des Ausdrucks anhand eines Livebeispiels Ihrer Quelle in Echtzeit zu aktualisieren.

![Ausdrucks-Generator](media/data-flow/exp5.png "Ausdrucksdatenvorschau")

## <a name="comments"></a>Kommentare

Fügen Sie Ihren Ausdrücken unter Verwendung der einzeiligen und mehrzeiligen Kommentarsyntax Kommentare hinzu:

![Kommentare](media/data-flow/comments.png "Kommentare")

## <a name="regular-expressions"></a>Reguläre Ausdrücke

Die Ausdruckssprache von Azure Data Factory-Datenfluss ([die vollständige Referenzdokumentation finden Sie hier)](https://aka.ms/dataflowexpressions) ermöglicht das Verwenden von Funktionen, die eine Syntax für reguläre Ausdrücke enthalten. Wenn Sie Funktionen mit regulären Ausdrücken verwenden, versucht der Ausdrucks-Generator, den umgekehrten Schrägstrich (\\) als Escapezeichensequenz zu interpretieren. Wenn Sie in Ihrem regulären Ausdruck umgekehrte Schrägstriche verwenden, schließen Sie entweder den gesamten regulären Ausdruck (RegEx) in Ticks (\`) ein, oder verwenden Sie einen doppelten umgekehrten Schrägstrich.

Beispiel für die Verwendung von Ticks

```
regex_replace('100 and 200', `(\d+)`, 'digits')
```

Beispiel für die Verwendung eines doppelten umgekehrten Schrägstrichs

```
regex_replace('100 and 200', '(\\d+)', 'digits')
```

## <a name="addressing-array-indexes"></a>Angeben von Arrayindizes

Verwenden Sie bei Ausdrucksfunktionen, die Arrays zurückgeben, eckige Klammern [], um bestimmte Indizes innerhalb dieses Rückgabearrayobjekts anzugeben. Das Array ist einzelbasiert.

![Ausdrucks-Generator: Array](media/data-flow/expb2.png "Ausdrucksdatenvorschau")

## <a name="handling-names-with-special-characters"></a>Verarbeiten von Namen mit Sonderzeichen

Wenn Sie Spaltennamen besitzen, die Sonderzeichen oder Leerzeichen enthalten, setzen Sie die Namen in geschweiften Klammern.
* ```{[dbo].this_is my complex name$$$}```

## <a name="next-steps"></a>Nächste Schritte

[Mit dem Erstellen von Datentransformationsausdrücken beginnen](data-flow-expression-functions.md)
