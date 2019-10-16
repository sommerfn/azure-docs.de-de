---
title: Hinzufügen einer Polygonebene für die Extrusion in Azure Maps | Microsoft-Dokumentation
description: Erfahren Sie, wie Sie dem Azure Maps Web SDK eine Polygonebene für die Extrusion hinzufügen.
author: walsehgal
ms.author: v-musehg
ms.date: 10/08/2019
ms.topic: conceptual
ms.service: azure-maps
services: azure-maps
manager: ''
ms.custom: codepen
ms.openlocfilehash: fdb5e2b78d9e5817c5a5d139cdf0b34744ed011f
ms.sourcegitcommit: aef6040b1321881a7eb21348b4fd5cd6a5a1e8d8
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 10/09/2019
ms.locfileid: "72170864"
---
# <a name="add-an-extrusion-polygon-layer-to-the-map"></a>Hinzufügen einer Polygonebene für die Extrusion zur Karte

In diesem Artikel erfahren Sie, wie Sie mithilfe der Polygonextrusionsebene Bereiche von `Polygon`- und `MultiPolygon`-Featuregeometrien als extrudierte Formen auf der Karte darstellen. Das Azure Maps-Web-SDK unterstützt auch die Erstellung von Kreisgeometrien gemäß der Definition im [erweiterten GeoJSON-Schema](extend-geojson.md#circle). Diese Kreise werden beim Rendern auf der Karte in Polygone umgewandelt. Alle Featuregeometrien können auch ganz einfach aktualisiert werden, wenn in der Klasse [atlas.Shape](https://docs.microsoft.com/javascript/api/azure-maps-control/atlas.shape?view=azure-iot-typescript-latest) umschlossen sind.


## <a name="use-a-polygon-extrusion-layer"></a>Verwenden einer Polygonextrusionsebene

Wenn eine [Polygonextrusionsebene](https://docs.microsoft.com/javascript/api/azure-maps-control/atlas.layer.polygonextrusionlayer?view=azure-maps-typescript-latest) mit einer Datenquelle verbunden ist und in die Karte geladen wird, werden die Bereiche der `Polygon`- und `MultiPolygon`-Features als extrudierte Formen gerendert. Die Eigenschaften `height` und `base` der Polygonextrusionsebene definieren den Basisabstand vom Boden und die Höhe der extrudierten Form in **Metern**. Der folgende Code zeigt, wie Sie ein Polygon erstellen, dieses einer Datenquelle hinzufügen und dann mit einer Klasse für die Polygonextrusionsebene rendern.

> [!Note]
> Der in der Polygonextrusionsebene definierte Wert `base` sollte kleiner oder gleich dem Wert von `height` sein.

<br/>

<iframe height="500" style="width: 100%;" scrolling="no" title="Extrudiertes Polygon" src="https://codepen.io/azuremaps/embed/wvvBpvE?height=265&theme-id=0&default-tab=js,result&editable=true" frameborder="no" allowtransparency="true" allowfullscreen="true">
Weitere Informationen finden Sie unter dem Pen <a href='https://codepen.io/azuremaps/pen/wvvBpvE'>Extruded polygon</a> (Extrudiertes Polygon) von Azure Maps (<a href='https://codepen.io/azuremaps'>@azuremaps</a>) auf <a href='https://codepen.io'>CodePen</a>.</iframe>


## <a name="add-data-driven-multipolygons"></a>Hinzufügen von datengesteuerten Multipolygonen

Eine Choroplethenkarte kann mithilfe der Polygonextrusionsebene gerendert werden, indem die Eigenschaften `height` und `fillColor` proportional zur Messung der statistischen Variable in den Featuregeometrien `Polygon` und `MultiPolygon` festgelegt werden. Das folgende Codebeispiel zeigt eine extrudierte Choroplethenkarte der USA basierend auf der Messung der Bevölkerungsdichte nach Bundesstaat.

<br/>

<iframe height="500" style="width: 100%;" scrolling="no" title="Extrudierte Choroplethenkarte" src="https://codepen.io/azuremaps/embed/eYYYNox?height=265&theme-id=0&default-tab=result&editable=true" frameborder="no" allowtransparency="true" allowfullscreen="true">
Weitere Informationen finden Sie unter dem Pen <a href='https://codepen.io/azuremaps/pen/eYYYNox'>Extruded choropleth map</a> (Extrudierte Choroplethenkarte) von Azure Maps (<a href='https://codepen.io/azuremaps'>@azuremaps</a>) auf <a href='https://codepen.io'>CodePen</a>.
</iframe>

## <a name="add-a-circle-to-the-map"></a>Hinzufügen eines Kreises zur Karte

Azure Maps verwendet eine erweiterte Version des GeoJSON-Schemas, das eine Definition für Kreise bereitstellt, wie [hier](https://docs.microsoft.com/azure/azure-maps/extend-geojson#circle) beschrieben. Ein extrudierter Kreis kann auf der Karte gerendert werden, indem ein `point`-Feature erstellt wird, das eine `subType`-Eigenschaft dem Wert `Circle` und eine `Radius`-Eigenschaft mit einer Zahl, die den Radius in **Metern** darstellt, aufweist. Beispiel:

```Javascript
{
    "type": "Feature",
    "geometry": {
        "type": "Point",
        "coordinates": [-105.203135, 39.664087]
    },
    "properties": {
        "subType": "Circle",
        "radius": 1000
    }
} 
```

Das Azure Maps Web SDK wandelt diese `Point`-Features im Hintergrund in `Polygon`-Features um, die auf der Karte auf der Polygonextrusionsebene gerendert werden können, wie im folgenden Codebeispiel gezeigt.

<br/>

<iframe height="500" style="width: 100%;" scrolling="no" title="Drohnenluftraum-Polygon" src="https://codepen.io/azuremaps/embed/zYYYrxo?height=265&theme-id=0&default-tab=js,result&editable=true" frameborder="no" allowtransparency="true" allowfullscreen="true">
Weitere Informationen finden Sie unter dem Pen <a href='https://codepen.io/azuremaps/pen/zYYYrxo'>Drone airspace polygon</a> (Drohnenluftraum-Polygon) von Azure Maps (<a href='https://codepen.io/azuremaps'>@azuremaps</a>) auf <a href='https://codepen.io'>CodePen</a>.
</iframe>


## <a name="next-steps"></a>Nächste Schritte

Erfahren Sie mehr zu den in diesem Artikel verwendeten Klassen und Methoden:

> [!div class="nextstepaction"]
> [Polygon](https://docs.microsoft.com/javascript/api/azure-maps-control/atlas.data.polygon?view=azure-iot-typescript-latest)

> [!div class="nextstepaction"]
> [Polygonextrusionsebene](https://docs.microsoft.com/javascript/api/azure-maps-control/atlas.layer.polygonextrusionlayer?view=azure-maps-typescript-latest)

Zusätzliche Ressourcen:

> [!div class="nextstepaction"]
> [Azure Maps-Erweiterung der Maps GeoJSON-Spezifikation](extend-geojson.md#circle)
