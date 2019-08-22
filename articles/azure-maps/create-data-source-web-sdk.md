---
title: Erstellen einer Datenquelle in Azure Maps | Microsoft-Dokumentation
description: Erfahren Sie, wie Sie eine Datenquelle erstellen und mit dem Azure Maps Web SDK verwenden.
author: rbrundritt
ms.author: richbrun
ms.date: 08/08/2019
ms.topic: conceptual
ms.service: azure-maps
services: azure-maps
manager: ''
ms.custom: codepen
ms.openlocfilehash: c225aa5f821925247d27890e9ee75f3eac4d9eb9
ms.sourcegitcommit: 62bd5acd62418518d5991b73a16dca61d7430634
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 08/13/2019
ms.locfileid: "68976606"
---
# <a name="create-a-data-source"></a>Erstellen einer Datenquelle

Das Azure Maps Web SDK speichert Daten in Datenquellen, die die Daten für Abfragen und das Rendern optimieren. Derzeit gibt es zwei Arten von Datenquellen:

**GeoJSON-Datenquelle**

Mithilfe der `DataSource`-Klasse kann eine GeoJSON-basierte Datenquelle Daten lokal laden und speichern. GeoJSON-Daten können manuell oder mithilfe der Hilfsklassen im [Atlas. Data](https://docs.microsoft.com/javascript/api/azure-maps-control/atlas.data)-Namespace erstellt werden. Die `DataSource`-Klasse stellt Funktionen zum Importieren von lokalen oder Remote-GeoJSON-Dateien bereit. Remote-GeoJSON-Dateien müssen auf einem CORs-fähigen Endpunkt gehostet werden. Die `DataSource`-Klasse bietet Funktionen für das Clustering von Punktdaten. 

**Vektorkachelquelle**

Eine Vektorkachelquelle beschreibt den Zugriff auf eine Vektorkachelebene und kann mithilfe der [VectorTileSource](https://docs.microsoft.com/javascript/api/azure-maps-control/atlas.source.vectortilesource)-Klasse erstellt werden. Azure Maps entspricht der dem offenen Standard [Mapbox Vector Tile Specification](https://github.com/mapbox/vector-tile-spec). Vektorkachelebenen sind ähnlich wie Kachelebenen. Anstatt dass jede Kachel ein Rasterbild ist, bilden alle zusammen jedoch eine komprimierte Datei (PBF-Format), die Vektorkartendaten und eine oder mehrere Ebenen enthält, die auf dem Client basierend auf dem Stil jeder Ebene gerendert und gestaltet werden können. Die Daten in einer Vektorkachel enthalten geografische Merkmale in Form von Punkten, Linien und Polygonen. Vektorkachelebenen haben gegenüber Rasterkachelebenen mehrere Vorteile.

 - Eine Dateigröße einer Vektorkachel ist in der Regel wesentlich kleiner als eine vergleichbare Rasterkachel. Dadurch wird weniger Bandbreite verbraucht, was eine geringere Latenz und eine schnellere Karte bedeutet. Dies sorgt für eine höhere Benutzerfreundlichkeit.
 - Da Vektorkacheln auf dem Client gerendert werden, können sie sich an die Auflösung des Geräts anpassen, auf dem sie angezeigt werden. Dadurch werden die gerenderten Karten wesentlich klarer und mit eindeutigen Bezeichnungen dargestellt. 
 - Um den Stil der Daten in den Vektorkarten zu ändern, müssen Sie die Daten nicht erneut herunterladen, da der neue Stil auf den Client angewendet werden kann. Im Gegensatz dazu erfordert das Ändern des Stils einer Rasterkachelebene in der Regel das Laden von Kacheln vom Server, auf die der neue Stil angewendet wird.
 - Da die Daten in Vektorform bereitgestellt werden, ist zur Aufbereitung der Daten weniger serverseitige Verarbeitung erforderlich, sodass neuere Daten schneller zur Verfügung gestellt werden können.

Alle Ebenen, die eine Vektorquelle verwenden, müssen einen `sourceLayer`-Wert angeben. 

Nach der Erstellung können Datenquellen über die `map.sources`-Eigenschaft, die ein [SourceManager](https://docs.microsoft.com/javascript/api/azure-maps-control/atlas.sourcemanager) ist, der Karte hinzugefügt werden. Der folgende Code zeigt, wie Sie eine `DataSource` erstellen und der Karte hinzufügen.

```javascript
//Create a data source and add it to the map.
var dataSource = new atlas.source.DataSource();
map.sources.add(dataSource);
```

## <a name="connecting-a-data-source-to-a-layer"></a>Verbinden einer Datenquelle mit einer Ebene

Daten werden mithilfe von Renderebenen auf der Karte gerendert. Auf eine einzelne Datenquelle kann von einer oder mehreren Renderebene verwiesen werden. Die folgenden Renderebenen erfordern eine Datenquelle:

- [Blasenebene](map-add-bubble-layer.md) – Punktdaten werden als skalierte Kreise mit Pixelradius gerendert.
- [Symbolebene](map-add-pin.md) – Punktdaten werden als Symbole und Text gerendert.
- [Wärmebildebene](map-add-heat-map-layer.md) – Punktdaten werden als Dichtewärmebild gerendert.
- [Linienebene](map-add-shape.md) – kann verwendet werden, um eine Linie und/oder die Kontur von Polygonen zu rendern. 
- [Polygonebene](map-add-shape.md) – füllt den Bereich eines Polygons mit einer Volltonfarbe oder einem Bildmuster aus.

Der folgende Code zeigt, wie Sie eine Datenquelle erstellen, sie zur Karte hinzufügen und mit einer Blasenebene verbinden und dann GeoJSON-Point-Daten von einem Remotespeicherort importieren. 

```javascript
//Create a data source and add it to the map.
var datasource = new atlas.source.DataSource();
map.sources.add(datasource);

//Create a layer that defines how to render points in the data source and add it to the map.
map.layers.add(new atlas.layer.BubbleLayer(datasource));

//Load the earthquake data.
datasource.importDataFromUrl('https://earthquake.usgs.gov/earthquakes/feed/v1.0/summary/significant_month.geojson');
```

Es gibt zusätzliche Renderebenen, die sich nicht mit diesen Datenquellen verbinden, sondern die von ihnen gerenderten Daten direkt laden. 

- [Bildebene](map-add-image-layer.md) – überlagert ein einzelnes Bild oberhalb der Karte und bindet seine Ecken an einen Satz angegebener Koordinaten.
- [Kachelebene](map-add-tile-layer.md) – überlagert eine Rasterkachelebene oberhalb der Karte.

## <a name="one-data-source-with-multiple-layers"></a>Eine Datenquelle mit mehreren Ebenen

Es können mehrere Ebenen mit einer Datenquelle verbunden sein. Das mag merkwürdig klingen, ist jedoch in vielen verschiedenen Szenarien sehr nützlich. Nehmen wir als Beispiel ein Szenario, in dem Benutzern das Zeichnen von Polygonen ermöglicht werden soll. Wenn wir einem Benutzer ermöglichen, ein Polygon zu zeichnen, sollten wir den Füllbereich des Polygons rendern, sobald der Benutzer Punkte zur Karte hinzufügt. Durch Hinzufügen einer formatierten Linie für den Umriss des Polygons lassen sich die Kanten des Polygons während des Zeichnens besser erkennen. Wenn wir dann noch oberhalb jeder Position im Polygon einen Ziehpunkt hinzufügen, z.B. eine Stecknadel oder eine andere Markierung, vereinfachen wir damit die Bearbeitung jeder einzelnen Position. Die folgende Abbildung veranschaulicht dieses Szenario.

![Karte mit mehreren Ebenen, die Daten aus einer einzigen Datenquelle rendern](media/create-data-source-web-sdk/multiple-layers-one-datasource.png)

Um dies zu erreichen, müssen Sie auf den meisten Kartenplattformen ein Polygonobjekt, ein Linienobjekt und für jede Position auf dem Polygon eine Stecknadel erstellen. Wenn das Polygon verändert wird, müssen Sie die Linien und Pins manuell aktualisieren. Dies kann sehr schnell zu einem komplexen Vorgang werden.

Mit Azure Maps benötigen Sie nur ein einzelnes Polygon in einer Datenquelle, wie im Code unten gezeigt.

```javascript
//Create a data source and add it to the map.
var dataSource = new atlas.source.DataSource();
map.sources.add(dataSource);

//Create a polygon and add it to the data source.
dataSource.add(new atlas.data.Polygon([[[/* Coordinates for polygon */]]]));

//Create a polygon layer to render the filled in area of the polygon.
var polygonLayer = new atlas.layer.PolygonLayer(dataSource, 'myPolygonLayer', {
     fillColor: 'rgba(255,165,0,0.2)'
});

//Create a line layer for greater control of rendering the outline of the polygon.
var lineLayer = new atlas.layer.LineLayer(dataSource, 'myLineLayer', {
     color: 'orange',
     width: 2
});

//Create a bubble layer to render the vertices of the polygon as scaled circles.
var bubbleLayer = new atlas.layer.BubbleLayer(dataSource, 'myBubbleLayer', {
     color: 'orange',
     radius: 5,
     outlineColor: 'white',
     outlineWidth: 2
});

//Add all layers to the map.
map.layers.add([polygonLayer, lineLayer, bubbleLayer]);
```

## <a name="next-steps"></a>Nächste Schritte

Erfahren Sie mehr zu den in diesem Artikel verwendeten Klassen und Methoden:

> [!div class="nextstepaction"]
> [DataSource](https://docs.microsoft.com/javascript/api/azure-maps-control/atlas.source.datasource?view=azure-maps-typescript-latest)

> [!div class="nextstepaction"]
> [DataSourceOptions](https://docs.microsoft.com/javascript/api/azure-maps-control/atlas.datasourceoptions?view=azure-maps-typescript-latest)

> [!div class="nextstepaction"]
> [VectorTileSource](https://docs.microsoft.com/javascript/api/azure-maps-control/atlas.source.vectortilesource?view=azure-maps-typescript-latest)

> [!div class="nextstepaction"]
> [VectorTileSourceOptions](https://docs.microsoft.com/javascript/api/azure-maps-control/atlas.vectortilesourceoptions?view=azure-maps-typescript-latest)

In den folgenden Artikeln finden Sie weitere Codebeispiele, die Sie Ihren Karten hinzufügen können:

> [!div class="nextstepaction"]
> [Hinzufügen eines Popups](map-add-popup.md)

> [!div class="nextstepaction"]
> [Verwenden von datengesteuerten Formatvorlagenausdrücken](data-driven-style-expressions-web-sdk.md)

> [!div class="nextstepaction"]
> [Hinzufügen einer Symbolebene](map-add-pin.md)

> [!div class="nextstepaction"]
> [Hinzufügen einer Blasenebene](map-add-bubble-layer.md)

> [!div class="nextstepaction"]
> [Hinzufügen einer Linienebene](map-add-line-layer.md)

> [!div class="nextstepaction"]
> [Hinzufügen einer Polygonebene](map-add-shape.md)

> [!div class="nextstepaction"]
> [Hinzufügen eines Wärmebilds](map-add-heat-map-layer.md)

> [!div class="nextstepaction"]
> [Codebeispiele](https://docs.microsoft.com/samples/browse/?products=azure-maps)