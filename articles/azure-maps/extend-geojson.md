---
title: Erweitern der GeoJSON-Geometrien in Azure Maps | Microsoft-Dokumentation
description: Erfahren Sie, wie Sie die GeoJSON-Geometrien in Azure Maps erweitern können
author: sataneja
ms.author: sataneja
ms.date: 05/17/2018
ms.topic: conceptual
ms.service: azure-maps
services: azure-maps
manager: ''
ms.openlocfilehash: be3c31951c4721a861f9239c5220419dec11b6bf
ms.sourcegitcommit: d4dfbc34a1f03488e1b7bc5e711a11b72c717ada
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 06/13/2019
ms.locfileid: "60799150"
---
# <a name="extending-geojson-geometries"></a>Erweitern der GeoJSON-Geometrien

Azure Maps stellt eine Liste leistungsstarker APIs zum Suchen in und entlang geografischer Merkmale bereit.
Diese APIs verwenden standardmäßig die [GeoJSON-Spezifikation][1] zur Darstellung der geografischen Features (z.B. Staatsgrenzen, Routen, usw.).  

Die [GeoJSON-Spezifikation][1] unterstützt nur die folgenden Geometrien:

* GeometryCollection
* LineString
* MultiLineString
* MultiPoint
* MultiPolygon
* Point
* Polygon

Einige Azure Maps-APIs (z. B. [Search Inside Geometry (Suche innerhalb der Geometrie)](https://docs.microsoft.com/rest/api/maps/search/postsearchinsidegeometry)) akzeptieren Geometrien wie „Circle“ (Kreis), die nicht Teil der [GeoJSON-Spezifikation][1] sind.

Dieser Artikel bietet eine ausführliche Erläuterung darüber, wie Azure Maps die [GeoJSON-Spezifikation][1] erweitert, um bestimmte Geometrien darzustellen.

### <a name="circle"></a>Circle

Die `Circle`-Geometrie wird von der [GeoJSON-Spezifikation][1] nicht unterstützt. Wir verwenden das `GeoJSON Feature`-Objekt, um einen Kreis darzustellen.

Eine `Circle`-Geometrie, die mithilfe eines `GeoJSON Feature`-Objekts dargestellt ist, __muss__ Folgendes enthalten:

1. Zentrum
   >Das Zentrum des Kreises wird mithilfe eines `GeoJSON Point`-Typs dargestellt.

2. Radius
   >Der `radius` des Kreises wird mithilfe der Eigenschaften von `GeoJSON Feature` dargestellt. Der Radiuswert wird in _Metern_ dargestellt und muss vom Typ `double` sein.

3. SubType
   >Die Geometrie des Kreises muss ebenso die `subType`-Eigenschaft enthalten. Diese Eigenschaft muss Teil der Eigenschaft von `GeoJSON Feature` sein, und der Wert sollte _Circle_ sein.


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

[1]: https://tools.ietf.org/html/rfc7946
