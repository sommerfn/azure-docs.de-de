---
title: Hinzufügen einer Polygonebene zu Azure Maps | Microsoft-Dokumentation
description: Hier erfahren Sie, wie Sie eine Polygonebene zum Azure Maps Web SDK hinzufügen.
author: jingjing-z
ms.author: jinzh
ms.date: 07/29/2019
ms.topic: conceptual
ms.service: azure-maps
services: azure-maps
manager: ''
ms.custom: codepen
ms.openlocfilehash: ca6c0f5e6fde5a31655ed17f4a016bf44216643f
ms.sourcegitcommit: 62bd5acd62418518d5991b73a16dca61d7430634
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 08/13/2019
ms.locfileid: "68976140"
---
# <a name="add-a-polygon-layer-to-the-map"></a>Hinzufügen einer Polygonebene zur Karte

In diesem Artikel erfahren Sie, wie Sie mithilfe einer Polygonebene die Bereiche der `Polygon`- und `MultiPolygon`-Featuregeometrien auf einer Karte rendern können. Das Azure Maps-Web-SDK unterstützt auch die Erstellung von Kreisgeometrien gemäß der Definition im [erweiterten GeoJSON-Schema](extend-geojson.md#circle). Diese Kreise werden beim Rendern auf der Karte in Polygone umgewandelt. Alle Featuregeometrien können auch ganz einfach aktualisiert werden, wenn in der Klasse [atlas.Shape](https://docs.microsoft.com/javascript/api/azure-maps-control/atlas.shape?view=azure-iot-typescript-latest) umschlossen sind.

## <a name="use-a-polygon-layer"></a>Verwenden einer Polygonebene 

Wenn eine Polygonebene mit einer Datenquelle verbunden ist und in die Karte geladen wird, werden die Bereiche der `Polygon`- und `MultiPolygon`-Features gerendert. Der folgende Code zeigt das Erstellen eines Polygons, das Hinzufügen zu einer Datenquelle und das Rendern mit einer Polygonebene mit der [PolygonLayer](https://docs.microsoft.com/javascript/api/azure-maps-control/atlas.layer.polygonlayer?view=azure-iot-typescript-latest)-Klasse.

```javascript
//Create a data source and add it to the map.
var dataSource = new atlas.source.DataSource();
map.sources.add(dataSource);

//Create a rectangular polygon.
dataSource.add(new atlas.data.Feature(
    new atlas.data.Polygon([[
        [-73.98235, 40.76799],
        [-73.95785, 40.80044],
        [-73.94928, 40.7968],
        [-73.97317, 40.76437],
        [-73.98235, 40.76799]
    ]])
));

//Create and add a polygon layer to render the polygon to the map.
map.layers.add(new atlas.layer.PolygonLayer(dataSource, null,{
    fillColor: 'red',
    opacaty: 0.5
}));
```

Nachfolgend finden Sie das vollständige ausführbare Codebeispiel für die oben erläuterte Funktionalität.

<br/>

<iframe height='500' scrolling='no' title='Hinzufügen eines Polygons zu einer Karte ' src='//codepen.io/azuremaps/embed/yKbOvZ/?height=500&theme-id=0&default-tab=js,result&embed-version=2&editable=true' frameborder='no' allowtransparency='true' allowfullscreen='true' style='width: 100%;'>Weitere Informationen finden Sie unter dem Pen <a href='https://codepen.io/azuremaps/pen/yKbOvZ/'>Add a polygon to a map</a> (Hinzufügen eines Polygons zu einer Karte) von Azure Maps (<a href='https://codepen.io/azuremaps'>@azuremaps</a>) auf <a href='https://codepen.io'>CodePen</a>.
</iframe>

## <a name="use-a-polygon-and-line-layer-together"></a>Verwenden von Polygon- und Linienebene

Eine Linienebene kann verwendet werden, um die Kontur von Polygonen zu rendern. Das folgende Codebeispiel rendert ebenso wie das vorherige Beispiel ein Polygon, fügt aber eine Linienebene als zweite Ebene hinzu, die mit der Datenquelle verbunden ist.  

<iframe height='500' scrolling='no' title='Polygon- und Linienebene zum Hinzufügen eines Polygons' src='//codepen.io/azuremaps/embed/aRyEPy/?height=500&theme-id=0&default-tab=js,result&embed-version=2&editable=true' frameborder='no' allowtransparency='true' allowfullscreen='true' style='width: 100%;'>Weitere Informationen finden Sie unter <a href='https://codepen.io/azuremaps/pen/aRyEPy/'>Polygon- und Linienebene zum Hinzufügen eines Polygons</a> in Azure Maps (<a href='https://codepen.io/azuremaps'>@azuremaps</a>) auf <a href='https://codepen.io'>CodePen</a>.
</iframe>

## <a name="fill-a-polygon-with-a-pattern"></a>Füllen eines Polygons mit einem Muster

Zusätzlich zum Füllen eines Polygons mit einer Farbe kann auch ein Bildmuster verwendet werden. Laden Sie ein Bildmuster in die Sprite-Ressourcen für Kartenbilder, und verweisen Sie dann mit der `fillPattern`-Eigenschaft der Polygonebene (PolygonLayer) auf dieses Bild.

<br/>

<iframe height="500" style="width: 100%;" scrolling="no" title="Füllmuster für Polygon" src="//codepen.io/azuremaps/embed/JzQpYX/?height=500&theme-id=0&default-tab=js,result" frameborder="no" allowtransparency="true" allowfullscreen="true">
Weitere Informationen finden Sie unter dem Pen <a href='https://codepen.io/azuremaps/pen/JzQpYX/'>Polygon fill pattern</a> (Füllmuster für Polygon) von Azure Maps (<a href='https://codepen.io/azuremaps'>@azuremaps</a>) auf <a href='https://codepen.io'>CodePen</a>.
</iframe>


> [!TIP]
> Das Azure Maps Web SDK bietet verschiedene anpassbare Bildvorlagen, die Sie als Füllmuster verwenden können. Weitere Informationen finden Sie im Dokument [Verwenden von Bildvorlagen](how-to-use-image-templates-web-sdk.md).

## <a name="customize-a-polygon-layer"></a>Anpassen einer Polygonebene

Die Polygonebene verfügt nur über ein paar Formatierungsoptionen. Hier ist ein Tool, mit dem Sie diese testen können.

<br/>

<iframe height='700' scrolling='no' title='LXvxpg' src='//codepen.io/azuremaps/embed/LXvxpg/?height=700&theme-id=0&default-tab=result' frameborder='no' allowtransparency='true' allowfullscreen='true' style='width: 100%;'>Weitere Informationen finden Sie unter dem Pen <a href='https://codepen.io/azuremaps/pen/LXvxpg/'>LXvxpg</a> von Azure Maps (<a href='https://codepen.io/azuremaps'>@azuremaps</a>) auf <a href='https://codepen.io'>CodePen</a>.
</iframe>

<a id="addACircle"></a>

## <a name="add-a-circle-to-the-map"></a>Hinzufügen eines Kreises zur Karte

Azure Maps verwendet eine erweiterte Version des GeoJSON-Schemas, das eine Definition für Kreise bereitstellt, wie [hier](extend-geojson.md#circle) beschrieben. Ein Kreis kann auf der Karte gerendert werden, indem ein `Point`-Feature erstellt wird, die eine `subType`-Eigenschaft mit einem Wert von `"Circle"` und eine `radius`-Eigenschaft mit einer Zahl aufweist, die den Radius in Metern darstellt. Beispiel:

```javascript
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

Das Azure Maps Web-SDK wandelt diese `Point`-Features im Hintergrund in `Polygon`-Features um, die auf der Karte mit Polygon- und Linienebenen gerendert werden können, wie im folgenden Codebeispiel gezeigt.

<br/>

<iframe height='500' scrolling='no' title='Hinzufügen eines Kreises zu einer Karte' src='//codepen.io/azuremaps/embed/PRmzJX/?height=500&theme-id=0&default-tab=js,result&embed-version=2&editable=true' frameborder='no' allowtransparency='true' allowfullscreen='true' style='width: 100%;'>Weitere Informationen finden Sie unter dem Pen <a href='https://codepen.io/azuremaps/pen/PRmzJX/'>Add a circle to a map</a> (Hinzufügen eines Kreises zu einer Karte) von Azure Maps (<a href='https://codepen.io/azuremaps'>@azuremaps</a>) auf <a href='https://codepen.io'>CodePen</a>.
</iframe>

## <a name="make-a-geometry-easy-to-update"></a>Vereinfachen der Aktualisierung einer Geometrie

Eine `Shape`-Klasse umschließt ein [Geometry](https://docs.microsoft.com/javascript/api/azure-maps-control/atlas.data.geometry?view=azure-iot-typescript-latest)- oder [Feature](https://docs.microsoft.com/javascript/api/azure-maps-control/atlas.data.feature?view=azure-iot-typescript-latest)-Objekt und vereinfacht seine Aktualisierung und Verwaltung. Eine Form kann erstellt werden, indem eine Geometrie und eine Reihe von Eigenschaften übergeben werden. Alternativ kann auch ein Feature übergeben werden, wie im folgenden Code zu sehen.

```javascript
//Creating a shape by passing in a geometry and a object containing properties.
var shape1 = new atlas.Shape(new atlas.data.Point[0,0], { myProperty: 1 });

//Creating a shape using a feature.
var shape2 = new atlas.Shape(new atlas.data.Feature(new atlas.data.Point[0,0], { myProperty: 1 });
```

Das folgende Codebeispiel zeigt, wie Sie ein geoJSON-Kreisobjekt mit einer Shape-Klasse umschließen und die Eigenschaft für den Radius ganz einfach mithilfe eines Schiebereglers aktualisieren können. Wenn sich der Wert für „radius“ in „Shape“ ändert, wird die Anzeige des Kreises auf der Karte automatisch aktualisiert.

<br/>

<iframe height='500' scrolling='no' title='Aktualisieren von Formeigenschaften' src='//codepen.io/azuremaps/embed/ZqMeQY/?height=500&theme-id=0&default-tab=js,result&embed-version=2&editable=true' frameborder='no' allowtransparency='true' allowfullscreen='true' style='width: 100%;'>Weitere Informationen finden Sie unter <a href='https://codepen.io/azuremaps/pen/ZqMeQY/'>Aktualisieren von Formeigenschaften</a> in Azure Maps (<a href='https://codepen.io/azuremaps'>@azuremaps</a>) auf <a href='https://codepen.io'>CodePen</a>.
</iframe>

## <a name="next-steps"></a>Nächste Schritte

Erfahren Sie mehr zu den in diesem Artikel verwendeten Klassen und Methoden:

> [!div class="nextstepaction"]
> [Polygon](https://docs.microsoft.com/javascript/api/azure-maps-control/atlas.data.polygon?view=azure-iot-typescript-latest)

> [!div class="nextstepaction"]
> [PolygonLayer](https://docs.microsoft.com/javascript/api/azure-maps-control/atlas.layer.polygonlayer?view=azure-iot-typescript-latest)

> [!div class="nextstepaction"]
> [PolygonLayerOptions](/javascript/api/azure-maps-control/atlas.polygonlayeroptions?view=azure-iot-typescript-latest)

Weitere Codebeispiele, die Sie zu Ihren Karten hinzufügen können, finden Sie in den folgenden Artikeln:

> [!div class="nextstepaction"]
> [Erstellen einer Datenquelle](create-data-source-web-sdk.md)

> [!div class="nextstepaction"]
> [Hinzufügen eines Popups](map-add-popup.md)

> [!div class="nextstepaction"]
> [Verwenden von datengesteuerten Formatvorlagenausdrücken](data-driven-style-expressions-web-sdk.md)

> [!div class="nextstepaction"]
> [Verwenden von Bildvorlagen](how-to-use-image-templates-web-sdk.md)

> [!div class="nextstepaction"]
> [Hinzufügen einer Linienebene](map-add-line-layer.md)

Zusätzliche Ressourcen:

> [!div class="nextstepaction"]
> [Azure Maps-Erweiterung der Maps GeoJSON-Spezifikation](extend-geojson.md#circle)