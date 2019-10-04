---
title: Hinzufügen eines Popupfensters mit Azure Maps | Microsoft-Dokumentation
description: Hinzufügen eines Popups zum Azure Maps Web SDK.
author: jingjing-z
ms.author: jinzh
ms.date: 07/29/2019
ms.topic: conceptual
ms.service: azure-maps
services: azure-maps
manager: ''
ms.custom: codepen
ms.openlocfilehash: cde6c745034d0963bd372e36e6e5a046113c202b
ms.sourcegitcommit: 62bd5acd62418518d5991b73a16dca61d7430634
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 08/13/2019
ms.locfileid: "68976552"
---
# <a name="add-a-popup-to-the-map"></a>Hinzufügen eines Popupfensters zu der Karte

In diesem Artikel wird gezeigt, wie Sie ein Popupfenster einem Punkt auf einer Karte hinzufügen.

## <a name="understand-the-code"></a>Grundlegendes zum Code

Der folgende Code fügt der Karte über eine Symbolebene ein Punktfeature mit den Eigenschaften `name` und `description` hinzu. Es wird eine Instanz der [Popup-Klasse](https://docs.microsoft.com/javascript/api/azure-maps-control/atlas.popup?view=azure-iot-typescript-latest) hinzugefügt, aber nicht angezeigt. Mausereignisse werden der Symbolebene hinzugefügt, um das Öffnen und Schließen des Popups auszulösen, wenn die Maus über und aus dem Symbolmarker bewegt wird. Wenn Sie den Mauszeiger über das Markersymbol bewegen, wird die `position`-Eigenschaft des Popups mit der Position des Markers aktualisiert und die `content`-Option wird mit HTML aktualisiert, das die `name`- und `description`-Eigenschaften des zu bewegenden Punktfeatures umschließt. Das Popup wird dann mithilfe der `open`-Funktion auf der Karte angezeigt.

```javascript
//Define an HTML template for a custom popup content laypout.
var popupTemplate = '<div class="customInfobox"><div class="name">{name}</div>{description}</div>';

//Create a data source and add it to the map.
var dataSource = new atlas.source.DataSource();
map.sources.add(dataSource);

dataSource.add(new atlas.data.Feature(new atlas.data.Point([-122.1333, 47.63]), {
  name: 'Microsoft Building 41', 
  description: '15571 NE 31st St, Redmond, WA 98052'
}));

//Create a layer to render point data.
var symbolLayer = new atlas.layer.SymbolLayer(dataSource);

//Add the polygon and line the symbol layer to the map.
map.layers.add(symbolLayer);

//Create a popup but leave it closed so we can update it and display it later.
popup = new atlas.Popup({
  pixelOffset: [0, -18],
  closeButton: false
});

//Add a hover event to the symbol layer.
map.events.add('mouseover', symbolLayer, function (e) {
  //Make sure that the point exists.
  if (e.shapes && e.shapes.length > 0) {
    var content, coordinate;
    var properties = e.shapes[0].getProperties();
    content = popupTemplate.replace(/{name}/g, properties.name).replace(/{description}/g, properties.description);
    coordinate = e.shapes[0].getCoordinates();

    popup.setOptions({
      //Update the content of the popup.
      content: content,

      //Update the popup's position with the symbol's coordinate.
      position: coordinate

    });
    //Open the popup.
    popup.open(map);
  }
});

map.events.add('mouseleave', symbolLayer, function (){
  popup.close();
});
```

Nachfolgend finden Sie das vollständige Beispiel für ausführbaren Code der obigen Funktionalität.

<br/>

<iframe height='500' scrolling='no' title='Hinzufügen eines Popupfensters mit Azure Maps' src='//codepen.io/azuremaps/embed/MPRPvz/?height=500&theme-id=0&default-tab=js,result&embed-version=2&editable=true' frameborder='no' allowtransparency='true' allowfullscreen='true' style='width: 100%;'>Sehen Sie sich bei <a href='https://codepen.io'>CodePen</a> die Informationen unter <a href='https://codepen.io/azuremaps/pen/MPRPvz/'>Add a pop up using Azure Maps</a> (Hinzufügen eines Popupfensters mit Azure Maps) von Azure Maps (<a href='https://codepen.io/azuremaps'>@azuremaps</a>) an.
</iframe>

## <a name="reusing-a-popup-with-multiple-points"></a>Wiederverwenden eines Popups für mehrere Punkte

Wenn Sie viele Punkte verwenden und immer nur ein Popup anzeigen möchten, sollten Sie am besten ein Popup erstellen und wiederverwenden, anstatt für jedes Punktfeature ein Popup zu erstellen. Durch die Wiederverwendung des Popup wird die Anzahl der von der Anwendung erstellten DOM-Elemente erheblich reduziert, wodurch eine bessere Leistung erzielt werden kann. Im folgenden Beispiel werden drei Punktfeatures erstellt. Wenn Sie auf eines davon klicken, wird ein Popup mit dem Inhalt für dieses Punktfeature angezeigt.

<br/>

<iframe height='500' scrolling='no' title='Wiederverwenden eines Popups für mehrere Stecknadeln' src='//codepen.io/azuremaps/embed/rQbjvK/?height=500&theme-id=0&default-tab=js,result&embed-version=2&editable=true' frameborder='no' allowtransparency='true' allowfullscreen='true' style='width: 100%;'>Informationen hierzu finden Sie unter <a href='https://codepen.io/azuremaps/pen/rQbjvK/'>Reusing Popup with Multiple Pins</a> (Wiederverwenden eines Popups mit mehreren Stecknadeln) von Azure Maps (<a href='https://codepen.io/azuremaps'>@azuremaps</a>) auf <a href='https://codepen.io'>CodePen</a>.
</iframe>

## <a name="customizing-a-popup"></a>Anpassen eines Popups

Standardmäßig hat das Popup einen weißen Hintergrund, einen Zeigerpfeil unten und eine Schaltfläche „Schließen“ in der oberen rechten Ecke. Das folgende Beispiel ändert die Hintergrundfarbe mit der Option `fillColor` des Popups in Schwarz. Die Schaltfläche „Schließen“ wird entfernt, indem die Option `shoCloseButton` auf „false“ gesetzt wird. Der HTML-Inhalt des Popups verwendet aufgefüllte 10 Pixel von den Rändern des Popups und der Text ist weiß, sodass er gut auf dem schwarzen Hintergrund dargestellt wird.  

<br/>

<iframe height="500" style="width: 100%;" scrolling="no" title="Benutzerdefiniertes Popup" src="//codepen.io/azuremaps/embed/ymKgdg/?height=500&theme-id=0&default-tab=js,result" frameborder="no" allowtransparency="true" allowfullscreen="true">
Weitere Informationen finden Sie unter dem Pen <a href='https://codepen.io/azuremaps/pen/ymKgdg/'>Angepasste Popups</a> von Azure Maps (<a href='https://codepen.io/azuremaps'>@azuremaps</a>) auf <a href='https://codepen.io'>CodePen</a>.
</iframe>

## <a name="popup-events"></a>Popup-Ereignisse

Popups können geöffnet, geschlossen und gezogen werden. Die Popup-Klasse stellt Ereignisse für die Hilfeentwickler bereit, die auf diese Aktionen reagieren können. Das folgende Beispiel zeigt, welche Ereignisse ausgelöst werden, wenn Sie das Popup öffnen, schließen oder ziehen. 

<br/>

<iframe height="500" style="width: 100%;" scrolling="no" title="Popup-Ereignisse" src="//codepen.io/azuremaps/embed/BXrpvB/?height=500&theme-id=0&default-tab=js,result" frameborder="no" allowtransparency="true" allowfullscreen="true">
Weitere Informationen finden Sie unter dem Pen <a href='https://codepen.io/azuremaps/pen/BXrpvB/'>Popup-Ereignisse</a> von Azure Maps (<a href='https://codepen.io/azuremaps'>@azuremaps</a>) auf <a href='https://codepen.io'>CodePen</a>.
</iframe>

## <a name="next-steps"></a>Nächste Schritte

Erfahren Sie mehr zu den in diesem Artikel verwendeten Klassen und Methoden:

> [!div class="nextstepaction"]
> [Popup](https://docs.microsoft.com/javascript/api/azure-maps-control/atlas.popup?view=azure-iot-typescript-latest)

> [!div class="nextstepaction"]
> [PopupOptions](https://docs.microsoft.com/javascript/api/azure-maps-control/atlas.popupoptions?view=azure-iot-typescript-latest)

Die folgenden interessanten Artikel enthalten vollständige Codebeispiele:

> [!div class="nextstepaction"]
> [Hinzufügen einer Symbolebene](./map-add-pin.md)

> [!div class="nextstepaction"]
> [Hinzufügen eines HTML-Markers](./map-add-custom-html.md)

> [!div class="nextstepaction"]
> [Hinzufügen einer Linienebene](map-add-line-layer.md)

> [!div class="nextstepaction"]
> [Hinzufügen einer Polygonebene](map-add-shape.md)