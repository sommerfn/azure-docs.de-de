---
title: Erweiterte GeoJSON-Geometrien in Azure Maps | Microsoft-Dokumentation
description: Erfahren Sie, wie Sie die GeoJSON-Geometrien in Azure Maps erweitern können
author: sataneja
ms.author: sataneja
ms.date: 05/17/2018
ms.topic: conceptual
ms.service: azure-maps
services: azure-maps
manager: ''
ms.openlocfilehash: 52325248d21a5d5112c9a7f9497c3e03fdf102a4
ms.sourcegitcommit: aa042d4341054f437f3190da7c8a718729eb675e
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 08/09/2019
ms.locfileid: "68881984"
---
# <a name="extended-geojson-geometries"></a>Erweiterte GeoJSON-Geometrien

Azure Maps stellt eine Liste leistungsstarker APIs zum Suchen in und entlang geografischer Merkmale bereit.
Diese APIs verwenden standardmäßig die [GeoJSON-Spezifikation][1] zur Darstellung der geografischen Features (z. B. Landesgrenzen, Routen usw.).  

Die [GeoJSON-Spezifikation][1] unterstützt nur die folgenden Geometrien:

* GeometryCollection
* LineString
* MultiLineString
* MultiPoint
* MultiPolygon
* Point
* Polygon

Einige Azure Maps-APIs (z. B. [Search Inside Geometry (Suche innerhalb der Geometrie)](https://docs.microsoft.com/rest/api/maps/search/postsearchinsidegeometry)) akzeptieren Geometrien wie „Circle“ (Kreis), die nicht Teil der [GeoJSON-Spezifikation][1] sind.

In diesem Artikel wird ausführlich beschrieben, wie Azure Maps die [GeoJSON-Spezifikation][1] erweitert, um bestimmte Geometrien darzustellen.

## <a name="circle"></a>Circle

Die `Circle`-Geometrie wird von der [GeoJSON-Spezifikation][1] nicht unterstützt. Wir verwenden ein `GeoJSON Point Feature`-Objekt, um einen Kreis darzustellen.

Eine `Circle`-Geometrie, die mithilfe eines `GeoJSON Feature`-Objekts dargestellt ist, __muss__ Folgendes enthalten:

- Zentrum

    Das Zentrum des Kreises wird mit einem `GeoJSON Point`-Objekt dargestellt.

- Radius

    Der `radius` des Kreises wird mithilfe der Eigenschaften von `GeoJSON Feature` dargestellt. Der Radiuswert wird in _Metern_ dargestellt und muss vom Typ `double` sein.

- SubType

    Die Geometrie des Kreises muss ebenso die `subType`-Eigenschaft enthalten. Diese Eigenschaft muss Teil der Eigenschaften von `GeoJSON Feature` sein, und der Wert sollte _Circle_ lauten.

#### <a name="example"></a>Beispiel

So stellen Sie einen Kreis dar, der sich auf dem Breitengrad 47.639754 und dem Längengrad -122.126986 befindet und dessen Radius gleich 100 Meter ist. Dazu verwenden Sie ein `GeoJSON Feature`-Objekt:

```json            
{
    "type": "Feature",
    "geometry": {
        "type": "Point",
        "coordinates": [-122.126986, 47.639754]
    },
    "properties": {
        "subType": "Circle",
        "radius": 100
    }
}          
```

## <a name="rectangle"></a>Rechteck

Die `Rectangle`-Geometrie wird von der [GeoJSON-Spezifikation][1] nicht unterstützt. Wir verwenden ein `GeoJSON Polygon Feature`-Objekt, um ein Rechteck darzustellen. Die Rechteckerweiterung wird hauptsächlich vom Zeichentoolmodul des Web SDK genutzt.

Eine `Rectangle`-Geometrie, die mithilfe eines `GeoJSON Polygon Feature`-Objekts dargestellt ist, __muss__ Folgendes enthalten:

- Ecken

    Die Ecken des Rechtecks werden mit den Koordinaten eines `GeoJSON Polygon`-Objekts dargestellt. Es sollten fünf Koordinaten vorhanden sein: eine für jede Ecke und eine fünfte Koordinate, die identisch mit der ersten ist, um den Polygonring zu schließen. Es wird davon ausgegangen, dass diese Koordinaten je nach Bedarf vom Entwickler ausgerichtet und gedreht werden.

- SubType

    Die Geometrie des Rechtecks muss auch die `subType`-Eigenschaft enthalten. Diese Eigenschaft muss Teil der Eigenschaften von `GeoJSON Feature` sein, und der Wert sollte _Rectangle_ lauten.

### <a name="example"></a>Beispiel

```json
{
    "type": "Feature",
    "geometry": {
        "type": "Polygon",
        "coordinates": [[[5,25],[14,25],[14,29],[5,29],[5,25]]]
    },
    "properties": {
        "subType": "Rectangle"
    }
}

```
## <a name="next-steps"></a>Nächste Schritte

Weitere Informationen zu GeoJSON-Daten in Azure Maps:

> [!div class="nextstepaction"]
> [GeoJSON-Format des Geofence](geofence-geojson.md)

Sehen Sie sich das Glossar mit den häufigen technischen Ausdrücken an, die Azure Maps und Location Intelligence-Anwendungen zugeordnet sind:

> [!div class="nextstepaction"]
> [Azure Maps-Glossar](glossary.md)

[1]: https://tools.ietf.org/html/rfc7946
