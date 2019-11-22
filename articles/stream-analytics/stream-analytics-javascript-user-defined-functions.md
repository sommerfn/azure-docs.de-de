---
title: Azure Stream Analytics – benutzerdefinierte JavaScript-Funktionen
description: In diesem Tutorial führen Sie erweiterte Abfragemechanismen mit benutzerdefinierten JavaScript-Funktionen aus.
services: stream-analytics
author: rodrigoamicrosoft
ms.author: rodrigoa
ms.service: stream-analytics
ms.topic: tutorial
ms.reviewer: mamccrea
ms.custom: mvc
ms.date: 04/01/2018
ms.openlocfilehash: 9ddf8a2a11cb863a0016726074c5279bfde96959
ms.sourcegitcommit: 92d42c04e0585a353668067910b1a6afaf07c709
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 10/28/2019
ms.locfileid: "72990226"
---
# <a name="tutorial-azure-stream-analytics-javascript-user-defined-functions"></a>Tutorial: Azure Stream Analytics – benutzerdefinierte JavaScript-Funktionen
 
Azure Stream Analytics unterstützt benutzerdefinierte Funktionen, die in JavaScript geschrieben sind. Die umfassenden **String**-, **RegExp**-, **Math**-, **Array**- und **Date**-Methoden von JavaScript vereinfachen die Erstellung von komplexen Datentransformationen mit Stream Analytics-Aufträgen.

In diesem Tutorial lernen Sie Folgendes:

> [!div class="checklist"]
> * Definieren einer benutzerdefinierten JavaScript-Funktion
> * Hinzufügen der Funktion zum Portal
> * Definieren einer Abfrage, die die Funktion ausführt

Wenn Sie kein Azure-Abonnement besitzen, können Sie ein [kostenloses Konto](https://azure.microsoft.com/free/?WT.mc_id=A261C142F) erstellen, bevor Sie beginnen.

## <a name="javascript-user-defined-functions"></a>JavaScript, benutzerdefinierte Funktionen
Benutzerdefinierte JavaScript-Funktionen unterstützen zustandslose skalare Computefunktionen, für die keine externe Verbindung benötigt wird. Der Rückgabewert einer Funktion kann nur ein skalarer (einzelner) Wert sein. Nachdem Sie einem Auftrag eine benutzerdefinierte JavaScript-Funktion hinzugefügt haben, können Sie die Funktion wie eine integrierte Skalarfunktion an einer beliebigen Stelle in der Abfrage verwenden.

Hier finden Sie einige Szenarien, in denen benutzerdefinierte JavaScript-Funktionen hilfreich sein könnten:
* Analysieren und Bearbeiten von Zeichenfolgen mit Funktionen mit regulären Ausdrücken, z.B. **Regexp_Replace()** und **Regexp_Extract()**
* Decodieren und Codieren von Daten, z.B. Konvertierung von binär in hexadezimal
* Durchführen mathematischer Berechnungen mit **Math**-Funktionen aus JavaScript
* Durchführen von Arrayvorgängen, z.B. Sortieren, Verknüpfen, Suchen und Füllen

Hier ist aufgeführt, was mit benutzerdefinierten JavaScript-Funktionen in Stream Analytics nicht möglich ist:
* Aufrufen von externen REST-Endpunkten, z.B. für die Durchführung umgekehrter IP-Suchen oder zum Abrufen von Referenzdaten von einer externen Quelle
* Durchführen einer benutzerdefinierten Serialisierung oder Deserialisierung von Ereignisformaten für Ein-/Ausgaben
* Erstellen benutzerdefinierter Aggregate

Auch wenn die Verwendung von Funktionen wie **Date.GetDate()** oder **Math.random()** in der Funktionsdefinition nicht blockiert ist, ist es ratsam, auf sie zu verzichten. Bei diesen Funktionen wird **nicht** bei jedem Aufruf das gleiche Ergebnis zurückgegeben, und im Azure Stream Analytics-Dienst wird kein Journal mit den Funktionsaufrufen und den zurückgegebenen Ergebnissen geführt. Wenn eine Funktion unterschiedliche Ergebnisse für dieselben Ereignisse zurückgibt, ist daher beim Starten eines Auftrags durch Sie oder den Stream Analytics-Dienst die Wiederholbarkeit nicht sichergestellt.

## <a name="add-a-javascript-user-defined-function-in-the-azure-portal"></a>Hinzufügen benutzerdefinierter JavaScript-Funktionen im Azure-Portal
Führen Sie die unten angegebenen Schritte aus, um eine einfache benutzerdefinierte JavaScript-Funktion unter einem vorhandenen Stream Analytics-Auftrag zu erstellen:

> [!NOTE]
> Diese Schritte eignen sich für Stream Analytics-Aufträge, die für die Ausführung in der Cloud konfiguriert sind. Wenn Ihr Stream Analytics-Auftrag für die Ausführung in Azure IoT Edge konfiguriert ist, verwenden Sie stattdessen Visual Studio, und [schreiben Sie die benutzerdefinierte Funktion mit C#](stream-analytics-edge-csharp-udf.md).

1.  Suchen Sie den Stream Analytics-Auftrag im Azure-Portal.

2. Wählen Sie unter der Überschrift **Auftragstopologie** die Option **Funktionen** aus. Eine leere Liste von Funktionen wird angezeigt.

3.  Wählen Sie zum Erstellen einer neuen benutzerdefinierten Funktion **+ Hinzufügen** aus.

4.  Wählen Sie auf dem Blatt **Neue Funktion** als **Funktionstyp** die Option **JavaScript** aus. Eine Standardfunktionsvorlage wird im Editor angezeigt.

5.  Geben Sie **hex2Int** als **UDF-Alias** ein, und ändern Sie die Funktionsimplementierung wie folgt:

    ```javascript
    // Convert Hex value to integer.
    function hex2Int(hexValue) {
        return parseInt(hexValue, 16);
    }
    ```

6.  Wählen Sie **Speichern** aus. Ihre Funktion wird in der Liste der Funktionen angezeigt.
7.  Wählen Sie die neue **hex2Int**-Funktion aus, und überprüfen Sie die Funktionsdefinition. Bei allen Funktionen wird dem Funktionsalias ein **UDF**-Präfix hinzugefügt. Sie müssen das Präfix beim Aufruf der Funktion in der Stream Analytics-Abfrage *einschließen*. Rufen Sie in diesem Fall **UDF.hex2Int** aus.

## <a name="testing-javascript-udfs"></a>Testen von JavaScript-UDFs 
Sie können Ihre JavaScript-UDF-Logik in einem beliebigen Browser testen und debuggen. Das Debuggen und Testen der Logik dieser benutzerdefinierten Funktionen wird im Stream Analytics-Portal derzeit nicht unterstützt. Sobald die Funktion wie gewünscht funktioniert, können Sie sie wie oben beschrieben dem Stream Analytics-Auftrag hinzufügen und dann direkt über Ihre Abfrage aufrufen.

## <a name="call-a-javascript-user-defined-function-in-a-query"></a>Aufrufen benutzerdefinierter JavaScript-Funktionen in einer Abfrage

1. Wählen Sie im Abfrage-Editor unter der Überschrift **Auftragstopologie** die Option **Abfrage** aus.
2.  Bearbeiten Sie die Abfrage, und rufen Sie anschließend die benutzerdefinierte Funktion wie folgt auf:

    ```SQL
    SELECT
        time,
        UDF.hex2Int(offset) AS IntOffset
    INTO
        output
    FROM
        InputStream
    ```

3.  Klicken Sie zum Hochladen der Beispieldatendatei mit der rechten Maustaste auf die Auftragseingabe.
4.  Wählen Sie zum Testen der Abfrage **Testen** aus.


## <a name="supported-javascript-objects"></a>Unterstützte JavaScript-Objekte
Benutzerdefinierte JavaScript-Funktionen in Azure Stream Analytics unterstützen die integrierten JavaScript-Standardobjekte. Eine Liste dieser Objekte finden Sie unter [Globale Objekte](https://developer.mozilla.org/docs/Web/JavaScript/Reference/Global_Objects).

### <a name="stream-analytics-and-javascript-type-conversion"></a>Stream Analytics- und JavaScript-Typkonvertierung

Es gibt Unterschiede zwischen den Typen, die in der Stream Analytics-Abfragesprache und in JavaScript unterstützt werden. In der folgenden Tabelle sind die Konvertierungszuordnungen für diese beiden Sprachen aufgeführt:

Stream Analytics | JavaScript
--- | ---
bigint | Number (in JavaScript können nur ganze Zahlen bis genau 2^53 dargestellt werden)
Datetime | Date (JavaScript unterstützt nur Millisekunden)
double | Number
nvarchar(MAX) | Zeichenfolge
Datensatz | Object
Array | Array
NULL | Null


Konvertierungen von JavaScript zu Stream Analytics:


JavaScript | Stream Analytics
--- | ---
Number | bigint (wenn die Zahl gerundet ist und zwischen long.MinValue und long.MaxValue liegt, andernfalls double)
Date | Datetime
Zeichenfolge | nvarchar(MAX)
Object | Datensatz
Array | Array
Null, Undefined | NULL
Beliebiger anderer Typ (z.B. eine Funktion oder ein Fehler) | Nicht unterstützt (führt zu einem Laufzeitfehler)

Die JavaScript-Sprache unterscheidet Groß-/Kleinschreibung, weshalb die Groß-/Kleinschreibung in den Objektfeldern im JavaScript-Code mit der Groß-/Kleinschreibung in den Feldern in den eingehenden Daten übereinstimmen muss. Beachten Sie, dass Aufträge mit dem Kompatibilitätsgrad 1.0 Felder aus der SQL SELECT-Anweisung in Kleinbuchstaben konvertieren. Ab Kompatibilitätsgrad 1.1 haben Felder aus der SELECT-Anweisung die gleiche Schreibung wie in der SQL-Abfrage angegeben.

## <a name="troubleshooting"></a>Problembehandlung
JavaScript-Laufzeitfehler werden als schwerwiegend angesehen und im Aktivitätsprotokoll aufgezeichnet. Navigieren Sie im Azure-Portal zu Ihrem Auftrag, und wählen Sie **Aktivitätsprotokoll** aus, um das Protokoll abzurufen.

## <a name="other-javascript-user-defined-function-patterns"></a>Weitere benutzerdefinierte JavaScript-Funktionsmuster

### <a name="write-nested-json-to-output"></a>Schreiben von geschachteltem JSON-Code in die Ausgabe
Wenn ein nachfolgender Verarbeitungsschritt die Ausgabe des Stream Analytics-Auftrags als Eingabe verwendet und das JSON-Format benötigt, können Sie eine JSON-Zeichenfolge in die Ausgabe schreiben. Im nächsten Beispiel wird die Funktion **JSON.stringify()** aufgerufen, um alle Name-Wert-Paare der Eingabe zu packen und als einzelnen Zeichenfolgenwert in die Ausgabe zu schreiben.

**Definition von benutzerdefinierten JavaScript-Funktionen:**

```javascript
function main(x) {
return JSON.stringify(x);
}
```

**Beispielabfrage:**
```SQL
SELECT
    DataString,
    DataValue,
    HexValue,
    UDF.json_stringify(input) As InputEvent
INTO
    output
FROM
    input PARTITION BY PARTITIONID
```

## <a name="clean-up-resources"></a>Bereinigen von Ressourcen

Löschen Sie die Ressourcengruppe, den Streamingauftrag und alle dazugehörigen Ressourcen, wenn Sie sie nicht mehr benötigen. Durch das Löschen des Auftrags verhindern Sie, dass Kosten für die vom Auftrag verbrauchten Streamingeinheiten anfallen. Wenn Sie den Auftrag später erneut verwenden möchten, können Sie ihn beenden und bei Bedarf neu starten. Wenn Sie diesen Auftrag nicht mehr verwenden möchten, löschen Sie alle Ressourcen, die im Rahmen dieser Schnellstartanleitung erstellt wurden:

1. Klicken Sie im Azure-Portal im Menü auf der linken Seite auf **Ressourcengruppen**, und klicken Sie auf den Namen der erstellten Ressource.  
2. Klicken Sie auf der Seite mit Ihrer Ressourcengruppe auf **Löschen**, geben Sie im Textfeld den Namen der zu löschenden Ressource ein, und klicken Sie dann auf **Löschen**.

## <a name="get-help"></a>Hier erhalten Sie Hilfe
Weitere Hilfe finden Sie in unserem [Azure Stream Analytics-Forum](https://social.msdn.microsoft.com/Forums/azure/home?forum=AzureStreamAnalytics).

## <a name="next-steps"></a>Nächste Schritte

In diesem Tutorial haben Sie einen Stream Analytics-Auftrag erstellt, der eine einfache benutzerdefinierte JavaScript-Funktion ausführt. Weitere Informationen zu Stream Analytics finden Sie in den Artikeln mit Echtzeitszenarien:

> [!div class="nextstepaction"]
> [Twitter-Standpunktanalyse in Echtzeit in Azure Stream Analytics](stream-analytics-twitter-sentiment-analysis-trends.md)
