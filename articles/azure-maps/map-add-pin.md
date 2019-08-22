---
title: Hinzufügen einer Symbolebene zu Azure Maps | Microsoft-Dokumentation
description: Hinzufügen eines Symbolen zum Azure Maps Web SDK.
author: rbrundritt
ms.author: richbrun
ms.date: 07/29/2019
ms.topic: conceptual
ms.service: azure-maps
services: azure-maps
manager: ''
ms.custom: codepen
ms.openlocfilehash: 10f6a7ef92bfd6558ed93e7fb40df9e48e1b92f5
ms.sourcegitcommit: 62bd5acd62418518d5991b73a16dca61d7430634
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 08/13/2019
ms.locfileid: "68976176"
---
# <a name="add-a-symbol-layer-to-a-map"></a>Hinzufügen einer Symbolebene zu einer Karte

Ein Symbol kann mit einer Datenquelle verbunden und dazu verwendet werden, ein Symbol und/oder einen Text an einem bestimmten Punkt darzustellen. Symbolebenen werden mit WebGL gerendert und können verwendet werden, um große Sammlungen von Punkten auf der Karte darzustellen. Diese Ebene kann bei guter Leistung wesentlich mehr Punktdaten auf der Karte darstellen, als mit HTML-Markern möglich wäre. Die Symbolebene unterstützt jedoch keine traditionellen CSS- und HTML-Elemente für die Gestaltung.  

> [!TIP]
> Symbolebenen rendern in der Standardeinstellung die Koordinaten aller Geometrien in einer Datenquelle. Legen Sie die Eigenschaft `filter` der Ebene auf `['==', ['geometry-type'], 'Point']` oder `['any', ['==', ['geometry-type'], 'Point'], ['==', ['geometry-type'], 'MultiPoint']]` fest, um die Ebene dahin gehend zu beschränken, dass nur Punktgeometriefunktionen gerendert werden, wenn auch MultiPoint-Funktionen berücksichtigt werden sollen.

## <a name="add-a-symbol-layer"></a>Hinzufügen einer Symbolebene

Um der Karte eine Symbolebene hinzuzufügen und Daten zu rendern, muss zunächst eine Datenquelle erstellt und die Karte hinzugefügt werden. Anschließend kann eine Symbolebene erstellt und in der Datenquelle übergeben werden, um die Daten abzurufen. Abschließend müssen Daten in die Datenquelle eingefügt werden, damit diese gerendert werden können. Der folgende Code zeigt den Code, der der Karte hinzugefügt werden soll, nachdem sie geladen wurde, um einen einzelnen Punkt auf der Karte mit einer Symbolebene darzustellen. 

```javascript
//Create a data source and add it to the map.
var dataSource = new atlas.source.DataSource();
map.sources.add(dataSource);

//Create a symbol layer to render icons and/or text at points on the map.
var layer = new atlas.layer.SymbolLayer(dataSource);

//Add the layer to the map.
map.layers.add(layer);

//Create a point and add it to the data source.
dataSource.add(new atlas.data.Point([0, 0]));
```

Es gibt vier verschiedene Arten von Punktdaten, die der Karte hinzugefügt werden können:

- GeoJSON-Point-Geometrie: Dieses Objekt enthält lediglich eine Koordinate eines Punkts. Mit der `atlas.data.Point`-Hilfsklasse können diese Objekte ganz einfach erstellt werden.
- GeoJSON-MultiPoint-Geometrie: Dieses Objekt enthält lediglich die Koordinate mehrerer Punkte. Mit der `atlas.data.MultiPoint`-Hilfsklasse können diese Objekte ganz einfach erstellt werden.
- GeoJSON-Feature: Dieses Objekt besteht aus einer beliebigen GeoJSON-Geometrie und einer Reihe von Eigenschaften, die Metadaten der Geometrie enthalten. Mit der `atlas.data.Feature`-Hilfsklasse können diese Objekte ganz einfach erstellt werden.
- Die `atlas.Shape`-Klasse ist ähnlich dem GeoJSON-Feature, das Sie ebenfalls aus einer beliebigen GeoJSON-Geometrie und einer Reihe von Eigenschaften besteht, die Metadaten der Geometrie enthalten. Wenn ein GeoJSON-Objekt zu einer Datenquelle hinzugefügt wird, kann es leicht in einer Ebene gerendert werden. Wenn jedoch die Koordinateneigenschaft dieses GeoJSON-Objekts aktualisiert wird, ändern sich die Datenquelle und die Karte nicht, da es im JSON-Objekt keinen Mechanismus gibt, um ein Update auszulösen. Die Shape-Klasse bietet Funktionen zum Aktualisieren der darin enthaltenen Daten. Wenn dann eine Änderung vorgenommen wird, werden die Datenquelle und die Karte automatisch informiert und aktualisiert. 

Das folgende Codebeispiel erstellt eine GeoJSON Point-Geometrie und übergibt sie an die `atlas.Shape`-Klasse, um einfacher aktualisieren zu können. Der Mittelpunkt der Karte wird zunächst zum Rendern eines Symbols verwendet. Der Karte wird ein Klickereignis hinzugefügt, sodass beim Auslösen die Koordinaten, an denen mit der Maus geklickt wurde, mit der `setCoordinates`-Funktion der Form verwendet werden, die die Position des Symbols auf der Karte aktualisiert.

<br/>

<iframe height='500' scrolling='no' title='Ändern der Stecknadel eines Standorts' src='//codepen.io/azuremaps/embed/ZqJjRP/?height=500&theme-id=0&default-tab=js,result&embed-version=2&editable=true' frameborder='no' allowtransparency='true' allowfullscreen='true' style='width: 100%;'>Weitere Informationen finden Sie unter <a href='https://codepen.io/azuremaps/pen/ZqJjRP/'>Ändern der Stecknadel eines Standorts</a> in Azure Maps (<a href='https://codepen.io/azuremaps'>@azuremaps</a>) auf <a href='https://codepen.io'>CodePen</a>.
</iframe>

> [!TIP]
> Standardmäßig optimieren Symbolebenen aus Leistungsgründen das Rendering von Symbolen, indem sie überlappende Symbole ausblenden. Beim Zoomen werden die ausgeblendeten Symbole angezeigt. Um dieses Feature zu deaktivieren und alle Symbole jederzeit anzuzeigen, legen Sie die Eigenschaft `allowOverlap` der `iconOptions`-Optionen auf `true` fest.

## <a name="add-a-custom-icon-to-a-symbol-layer"></a>Hinzufügen eines benutzerdefinierten Zeichens zu einer Symbolebene

Symbolebenen werden mit WebGL gerendert. Daher müssen alle Ressourcen wie etwa Zeichenbilder in den WebGL-Kontext geladen werden. Dieses Beispiel zeigt, wie ein benutzerdefiniertes Symbol zu den Kartenressourcen hinzugefügt und anschließend zum Rendern von Punktdaten mit einem benutzerdefinierten Symbol auf der Karte verwendet wird. Die `textField`-Eigenschaft der Symbolebene erfordert, dass ein Ausdruck angegeben wird. In diesem Fall soll die Temperatureigenschaft gerendert werden. Da es sich aber um eine Zahl handelt, muss sie in eine Zeichenfolge konvertiert werden. Außerdem soll „°F“ angefügt werden. Dies ist mit folgendem Ausdruck möglich: `['concat', ['to-string', ['get', 'temperature']], '°F']`. 

<br/>

<iframe height='500' scrolling='no' title='Benutzerdefiniertes Symbolbildzeichen' src='//codepen.io/azuremaps/embed/WYWRWZ/?height=500&theme-id=0&default-tab=js,result&embed-version=2&editable=true' frameborder='no' allowtransparency='true' allowfullscreen='true' style='width: 100%;'>Weitere Informationen finden Sie unter dem Pen <a href='https://codepen.io/azuremaps/pen/WYWRWZ/'>Custom Symbol Image Icon</a> (Benutzerdefiniertes Symbolbildzeichen) von Azure Maps (<a href='https://codepen.io/azuremaps'>@azuremaps</a>) auf <a href='https://codepen.io'>CodePen</a>.
</iframe>

> [!TIP]
> Das Azure Maps Web SDK bietet verschiedene anpassbare Bildvorlagen, die Sie mit der Symbolebene verwenden können. Weitere Informationen finden Sie im Dokument [Verwenden von Bildvorlagen](how-to-use-image-templates-web-sdk.md).

## <a name="customize-a-symbol-layer"></a>Anpassen einer Symbolebene 

Die Symbolebene bietet zahlreiche Optionen für die Formatierung. Im Folgenden finden Sie ein Tool, mit dem Sie diese verschiedenen Formatoptionen testen können.

<br/>

<iframe height='700' scrolling='no' title='Symbolebenenoptionen' src='//codepen.io/azuremaps/embed/PxVXje/?height=700&theme-id=0&default-tab=result' frameborder='no' allowtransparency='true' allowfullscreen='true' style='width: 100%;'>Weitere Informationen finden Sie unter dem Pen <a href='https://codepen.io/azuremaps/pen/PxVXje/'>Symbol Layer Options</a> (Symbolebenenoptionen) von Azure Maps (<a href='https://codepen.io/azuremaps'>@azuremaps</a>) auf <a href='https://codepen.io'>CodePen</a>.
</iframe>

> [!TIP]
> Wenn Sie nur Text mit einer Symbolebene rendern möchten, können Sie das Symbol ausblenden, indem Sie die `image`-Eigenschaft der Symboloptionen auf `'none'` setzen.

## <a name="next-steps"></a>Nächste Schritte

Erfahren Sie mehr zu den in diesem Artikel verwendeten Klassen und Methoden:

> [!div class="nextstepaction"]
> [SymbolLayer](https://docs.microsoft.com/javascript/api/azure-maps-control/atlas.layer.symbollayer?view=azure-iot-typescript-latest)

> [!div class="nextstepaction"]
> [SymbolLayerOptions](https://docs.microsoft.com/javascript/api/azure-maps-control/atlas.symbollayeroptions?view=azure-iot-typescript-latest)

> [!div class="nextstepaction"]
> [IconOptions](https://docs.microsoft.com/javascript/api/azure-maps-control/atlas.iconoptions?view=azure-iot-typescript-latest)

> [!div class="nextstepaction"]
> [TextOptions](https://docs.microsoft.com/javascript/api/azure-maps-control/atlas.textoptions?view=azure-iot-typescript-latest)

In den folgenden Artikeln finden Sie weitere Codebeispiele, die Sie Ihren Karten hinzufügen können:

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

> [!div class="nextstepaction"]
> [Hinzufügen einer Polygonebene](map-add-shape.md)

> [!div class="nextstepaction"]
> [Hinzufügen einer Blasenebene](map-add-bubble-layer.md)

> [!div class="nextstepaction"]
> [Hinzufügen von HTML-Markern](map-add-bubble-layer.md)
