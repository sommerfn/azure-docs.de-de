---
title: Analysieren von JSON- und AVRO-Daten in Azure Stream Analytics
description: Dieser Artikel beschreibt, wie Sie mit komplexen Datentypen wie Arrays, JSON- und CSV-formatierten Daten arbeiten.
services: stream-analytics
ms.service: stream-analytics
author: mamccrea
ms.author: mamccrea
ms.topic: conceptual
ms.date: 06/21/2019
ms.openlocfilehash: daf5b97e4ac586f89e5964ee16ee73c86f59b01d
ms.sourcegitcommit: 08138eab740c12bf68c787062b101a4333292075
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 06/22/2019
ms.locfileid: "67329357"
---
# <a name="parse-json-and-avro-data-in-azure-stream-analytics"></a>Analysieren von JSON- und AVRO-Daten in Azure Stream Analytics

Azure Stream Analytics unterstützt die Verarbeitung von Ereignissen in den Datenformaten CSV, JSON und AVRO. Sowohl JSON- als auch AVRO-Daten können strukturiert sein und komplexe Typen enthalten, z. B. geschachtelte Objekte (Datensätze) und Arrays. 




## <a name="record-data-types"></a>Datensatz-Datentypen
Mit Datensatz-Datentypen werden JSON- und Avro-Arrays dargestellt, wenn entsprechende Formate in Eingabedatenströmen verwendet werden. Diese Beispiele zeigen einen Beispielsensor, der Eingabeereignisse im JSON-Format liest. Hier ist das Beispiel eines einzelnen Ereignisses:

```json
{
    "DeviceId" : "12345",
    "Location" :
    {
        "Lat": 47,
        "Long": 122
    },
    "SensorReadings" :
    {
        "Temperature" : 80,
        "Humidity" : 70,
        "CustomSensor01" : 5,
        "CustomSensor02" : 99,
        "SensorMetadata" : 
        {
        "Manufacturer":"ABC",
        "Version":"1.2.45"
        }
    }
}
```


### <a name="access-nested-fields-in-known-schema"></a>Zugreifen auf geschachtelte Felder in einem bekannten Schema
Verwenden Sie die Punktnotation (.), um ganz einfach über Ihre Abfrage auf geschachtelte Felder zuzugreifen. Beispielsweise wählt diese Abfrage in den obigen JSON-Daten die Breiten- und Längengradkoordinaten unter der Location-Eigenschaft aus. Die Punktnotation kann verwendet werden, um wie im Folgenden dargestellt zwischen mehreren Ebenen zu navigieren.

```SQL
SELECT
    DeviceID,
    Location.Lat,
    Location.Long,
    SensorReadings.SensorMetadata.Version
FROM input
```

### <a name="select-all-properties"></a>Auswählen aller Eigenschaften
Sie können alle Eigenschaften eines geschachtelten Datensatzes mit dem Platzhalter „*“ auswählen. Betrachten Sie das folgende Beispiel:

```SQL
SELECT input.Location.*
FROM input
```

Es wird folgendes Ergebnis ausgegeben:

```json
{
    "Lat" : 47,
    "Long" : 122
}
```


### <a name="access-nested-fields-when-property-name-is-a-variable"></a>Zugreifen auf geschachtelte Felder, wenn der Eigenschaftenname eine Variable ist
Verwenden Sie die Funktion [GetRecordPropertyValue](https://docs.microsoft.com/stream-analytics-query/getmetadatapropertyvalue), wenn der Eigenschaftenname eine Variable ist. 

Stellen Sie sich z. B. einmal vor, dass ein Beispieldatenstrom mit Referenzdaten verknüpft werden muss, die Schwellenwerte für jeden Gerätesensor enthalten. Nachfolgend finden Sie einen Ausschnitt aus solchen Referenzdaten.

```json
{
    "DeviceId" : "12345",
    "SensorName" : "Temperature",
    "Value" : 75
}
```

```SQL
SELECT
    input.DeviceID,
    thresholds.SensorName
FROM input      -- stream input
JOIN thresholds -- reference data input
ON
    input.DeviceId = thresholds.DeviceId
WHERE
    GetRecordPropertyValue(input.SensorReadings, thresholds.SensorName) > thresholds.Value
    -- the where statement selects the property value coming from the reference data
```

### <a name="convert-record-fields-into-separate-events"></a>Konvertieren von Eintragsfeldern in separate Ereignisse
Um Datensatzfelder in separate Ereignisse zu konvertieren, verwenden Sie den [APPLY](https://docs.microsoft.com/stream-analytics-query/apply-azure-stream-analytics)-Operator zusammen mit der [GetRecordProperties](https://docs.microsoft.com/stream-analytics-query/getrecordproperties-azure-stream-analytics)-Funktion. Wenn das obige Beispiel mehrere Einträge für SensorReading umfassen würde, könnte die folgende Abfrage verwendet werden, um diese Datensätze in verschiedene Ereignisse zu extrahieren:

```SQL
SELECT
    event.DeviceID,
    sensorReading.PropertyName,
    sensorReading.PropertyValue
FROM input as event
CROSS APPLY GetRecordProperties(event.SensorReadings) AS sensorReading
```



## <a name="array-data-types"></a>Arraydatentypen

Arraydatentypen sind eine geordnete Sammlung von Werten. Im Folgenden werden einige typische Vorgänge mit Arraywerten beschrieben. Diese Beispiele setzen voraus, dass die Eingabeereignisse eine Eigenschaft namens „arrayField“ besitzen, ein Arraydatentyp.

In diesen Beispielen werden die Funktionen [GetArrayElement](https://docs.microsoft.com/stream-analytics-query/getarrayelement-azure-stream-analytics), [GetArrayElements](https://docs.microsoft.com/stream-analytics-query/getarrayelements-azure-stream-analytics), [GetArrayLength](https://docs.microsoft.com/stream-analytics-query/getarraylength-azure-stream-analytics) und der [APPLY](https://docs.microsoft.com/stream-analytics-query/apply-azure-stream-analytics)-Operator verwendet.

### <a name="working-with-a-specific-array-element"></a>Arbeiten mit einem spezifischen Arrayelement
Wählen Sie das Arrayelement am angegebenen Index aus (Auswählen des ersten Arrayelements):

```SQL
SELECT
    GetArrayElement(arrayField, 0) AS firstElement
FROM input
```

### <a name="select-array-length"></a>Auswählen der Arraylänge

```SQL
SELECT
    GetArrayLength(arrayField) AS arrayLength
FROM input
```

### <a name="convert-array-elements-into-separate-events"></a>Konvertieren von Arrayelementen in separate Ereignisse
Wählen Sie alle Arrayelemente als einzelne Ereignisse aus. Der [APPLY](https://docs.microsoft.com/stream-analytics-query/apply-azure-stream-analytics)-Operator extrahiert zusammen mit der integrierten [GetArrayElements](https://docs.microsoft.com/stream-analytics-query/getarrayelements-azure-stream-analytics)-Funktion alle Elemente des Arrays als einzelne Ereignisse:

```SQL
SELECT
    arrayElement.ArrayIndex,
    arrayElement.ArrayValue
FROM input as event
CROSS APPLY GetArrayElements(event.arrayField) AS arrayElement
```


## <a name="see-also"></a>Siehe auch
[Datentypen in Azure Stream Analytics](https://docs.microsoft.com/stream-analytics-query/data-types-azure-stream-analytics)
